---
title: 'Zelfstudie: Een model trainen in Python met geautomatiseerde machine learning'
description: Zelfstudie over hoe u een machine learning-model traint in Python met Apache Spark en geautomatiseerde machine learning.
services: synapse-analytics
author: midesa
ms.service: synapse-analytics
ms.topic: tutorial
ms.subservice: machine-learning
ms.date: 06/30/2020
ms.author: midesa
ms.reviewer: jrasnick
ms.openlocfilehash: 2594e25bff3ca949b329f8b66f4427eb1f6950b0
ms.sourcegitcommit: aacbf77e4e40266e497b6073679642d97d110cda
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2021
ms.locfileid: "98118707"
---
# <a name="tutorial-train-a-model-in-python-with-automated-machine-learning"></a>Zelfstudie: Een model trainen in Python met geautomatiseerde machine learning

Azure Machine Learning is een cloudomgeving die u kunt gebruiken voor het trainen, implementeren, automatiseren, beheren en volgen van machine learning-modellen. 

In deze zelfstudie gebruikt u [geautomatiseerde machine learning](../../machine-learning/concept-automated-ml.md) in Azure Machine Learning om een regressiemodel te maken om de ritprijzen van taxi’s te voorspellen. Dit proces gebruikt trainingsgegevens en configuratie-instellingen en doorloopt automatisch combinaties van verschillende methoden voor het normaliseren/standaardiseren van functies, modellen en instellingen van hyperparameters om het beste model te bepalen.

In deze zelfstudie leert u het volgende:
- De gegevens downloaden met behulp van Apache Spark en Azure Open Datasets.
- Gegevens transformeren en opschonen met Apache Spark-gegevensframes.
- Een regressiemodel trainen met geautomatiseerde machine learning.
- De nauwkeurigheid van een model berekenen.

## <a name="before-you-begin"></a>Voordat u begint

- Maak een serverloze Apache Spark-pool door de stappen te volgen in de [quickstart Een serverloze Apache Spark-pool maken](../quickstart-create-apache-spark-pool-studio.md).
- Voltooi de [zelfstudie Azure Machine Learning-werkruimte instellen](../../machine-learning/tutorial-1st-experiment-sdk-setup.md) als u niet beschikt over een bestaande Azure Machine Learning-werkruimte. 

## <a name="understand-regression-models"></a>Regressiemodellen begrijpen

*Regressiemodellen* voorspellen numerieke uitvoerwaarden op basis van onafhankelijke voorspellingen. In regressie is het doel om te helpen de relatie tot stand te brengen tussen deze onafhankelijke voorspellingsvariabelen door te schatten hoe één variabele de andere beïnvloedt.  

### <a name="example-based-on-new-york-city-taxi-data"></a>Voorbeeld op basis van taxigegevens in New York City

In dit voorbeeld gebruikt u Spark om een analyse uit te voeren voor gegevens over fooien voor taxiritten in New York City (NYC). De gegevens zijn beschikbaar via [Azure Open Datasets](https://azure.microsoft.com/services/open-datasets/catalog/nyc-taxi-limousine-commission-yellow-taxi-trip-records/). Deze subset van de gegevensset bevat informatie over taxiritten, waaronder informatie over elke rit, de begin- en eindtijd en locaties en de kosten.

> [!IMPORTANT]
> Er kunnen extra kosten gelden voor het ophalen van deze gegevens uit de opslaglocatie. In de volgende stappen ontwikkelt u een model om de ritprijzen voor NYC-taxi’s te voorspellen. 

##  <a name="download-and-prepare-the-data"></a>De gegevens downloaden en voorbereiden

U doet dit als volgt:

1. Maak een notebook met behulp van de PySpark-kernel. Zie [Een notebook maken](../quickstart-apache-spark-notebook.md#create-a-notebook) voor instructies.
   
    > [!Note]
    > Vanwege de PySpark-kernel hoeft u niet expliciet contexten te maken. De Spark-context wordt automatisch voor u gemaakt wanneer u de eerste codecel uitvoert.
  
2. Omdat de onbewerkte gegevens de Parquet-indeling hebben, kunt u de Spark-context gebruiken om het bestand rechtstreeks in het geheugen te plaatsen als een gegevensframe. Maak een Spark-gegevensframe door de gegevens op te halen via de Open Datasets-API. Hier gebruiken we de `schema on read`-eigenschappen van Spark-gegevensframe om de gegevenstypen en het schema af te leiden. 
   
    ```python
    blob_account_name = "azureopendatastorage"
    blob_container_name = "nyctlc"
    blob_relative_path = "yellow"
    blob_sas_token = r""

    # Allow Spark to read from Blob remotely
    wasbs_path = 'wasbs://%s@%s.blob.core.windows.net/%s' % (blob_container_name, blob_account_name, blob_relative_path)
    spark.conf.set('fs.azure.sas.%s.%s.blob.core.windows.net' % (blob_container_name, blob_account_name),blob_sas_token)

    # Spark read parquet, note that it won't load any data yet by now
    df = spark.read.parquet(wasbs_path)

    ```

3. Afhankelijk van de grootte van de Spark-pool kan de hoeveelheid onbewerkte gegevens te groot zijn, of kan het bewerken ervan te veel tijd in beslag nemen. U kunt deze gegevens filteren naar een kleinere hoeveelheid met behulp van de filters ```start_date``` en ```end_date```. Hiermee wordt een filter toegepast op basis waarvan de gegevens van één maand worden geretourneerd. Als we beschikken over het gefilterde gegevensframe, voeren we ook de functie ```describe()``` uit op het nieuwe dataframe om samenvattingsstatistieken voor elk veld weer te geven. 

   Op basis van de samenvattingsstatistieken ziet u dat er sprake is van wat onregelmatigheden in de gegevens. De statistieken laten bijvoorbeeld zien dat de minimale reisafstand kleiner is dan 0. Deze onregelmatige gegevenspunten moeten worden uitgefilterd.
   
   ```python
   # Create an ingestion filter
   start_date = '2015-01-01 00:00:00'
   end_date = '2015-12-31 00:00:00'

   filtered_df = df.filter('tpepPickupDateTime > "' + start_date + '" and tpepPickupDateTime< "' + end_date + '"')

   filtered_df.describe().show()
   ```

4. Vervolgens genereert u functies uit de gegevensset door een set kolommen te selecteren en verschillende op tijd gebaseerde functies te maken op basis van het datum/tijd-veld voor vertrek. Filter de uitschieters uit die in een eerdere stap zijn geïdentificeerd, en verwijder vervolgens de laatste paar kolommen omdat die onnodig zijn voor de training.
   
   ```python
   from datetime import datetime
   from pyspark.sql.functions import *

   # To make development easier, faster and less expensive down sample for now
   sampled_taxi_df = filtered_df.sample(True, 0.001, seed=1234)

   taxi_df = sampled_taxi_df.select('vendorID', 'passengerCount', 'tripDistance',  'startLon', 'startLat', 'endLon' \
                                   , 'endLat', 'paymentType', 'fareAmount', 'tipAmount'\
                                   , column('puMonth').alias('month_num') \
                                   , date_format('tpepPickupDateTime', 'hh').alias('hour_of_day')\
                                   , date_format('tpepPickupDateTime', 'EEEE').alias('day_of_week')\
                                   , dayofmonth(col('tpepPickupDateTime')).alias('day_of_month')
                                   ,(unix_timestamp(col('tpepDropoffDateTime')) - unix_timestamp(col('tpepPickupDateTime'))).alias('trip_time'))\
                           .filter((sampled_taxi_df.passengerCount > 0) & (sampled_taxi_df.passengerCount < 8)\
                                   & (sampled_taxi_df.tipAmount >= 0)\
                                   & (sampled_taxi_df.fareAmount >= 1) & (sampled_taxi_df.fareAmount <= 250)\
                                   & (sampled_taxi_df.tipAmount < sampled_taxi_df.fareAmount)\
                                   & (sampled_taxi_df.tripDistance > 0) & (sampled_taxi_df.tripDistance <= 200)\
                                   & (sampled_taxi_df.rateCodeId <= 5)\
                                   & (sampled_taxi_df.paymentType.isin({"1", "2"})))
   taxi_df.show(10)
   ```
   
   Zoals u ziet, wordt er nu een nieuwe gegevensframe gemaakt met extra kolommen voor dag van de maand, uur van vertrek, weekdag, en totale duur van de rit. 

   ![Afbeelding van de gegevensframe voor de taxi.](./media/azure-machine-learning-spark-notebook/dataset.png#lightbox)

## <a name="generate-test-and-validation-datasets"></a>Gegevens genereren voor test en validatie

Zodra u beschikt over de definitieve gegevensset, kunt u de gegevens splitsen in trainings- en testsets, met behulp van de functie ```random_ split ``` in Spark. Met behulp van de opgegeven gewichten worden met deze functie de gegevens willekeurig gesplitst in de trainingsgegevensset voor modeltraining en de validatiegegevensset voor tests.

```python
# Random split dataset using Spark, convert Spark to Pandas
training_data, validation_data = taxi_df.randomSplit([0.8,0.2], 223)

```
Met deze stap zorgt u ervoor dat de gegevenspunten die worden gebruikt om het voltooide model te testen, niet ook zijn gebruikt om het model te trainen. 

## <a name="connect-to-an-azure-machine-learning-workspace"></a>Verbinding maken met een Azure Machine Learning-werkruimte
In Azure Machine Learning is een werkruimte een klasse die de gegevens over uw Azure-abonnement en -resources accepteert. Hier wordt ook een cloudresource gemaakt om de uitvoeringen van uw model te controleren en bij te houden. In deze stap maakt u een werkruimteobject op basis van de bestaande Azure Machine Learning-werkruimte.
   
```python
from azureml.core import Workspace

# Enter your workspace subscription, resource group, name, and region.
subscription_id = "<enter your subscription ID>" #you should be owner or contributor
resource_group = "<enter your resource group>" #you should be owner or contributor
workspace_name = "<enter your workspace name>" #your workspace name
workspace_region = "<enter workspace region>" #your region

ws = Workspace(workspace_name = workspace_name,
               subscription_id = subscription_id,
               resource_group = resource_group)

```

## <a name="convert-a-dataframe-to-an-azure-machine-learning-dataset"></a>Een gegevensframe converteren naar een Azure Machine Learning-gegevensset
Als u een extern experiment wilt verzenden, moet u de gegevensset converteren naar een Azure Machine Learning-```TabularDatset```. Een [TabularDataset](/python/api/azureml-core/azureml.data.tabulardataset?preserve-view=true&view=azure-ml-py) vertegenwoordigt gegevens in een tabellaire indeling door het opgegeven bestand te parseren.

Met de volgende code worden de bestaande werkruimte en de standaard-Azure Machine Learning-gegevensopslag opgehaald. Vervolgens worden de gegevensopslag en bestandslocaties doorgegeven aan de padparameter om een nieuwe ```TabularDataset``` te maken. 

```python
import pandas 
from azureml.core import Dataset

# Get the Azure Machine Learning Default Datastore
datastore = ws.get_default_datastore()
training_pd = training_data.toPandas().to_csv('training_pd.csv', index=False)

# Convert into Azure Machine Learning Tabular Dataset
datastore.upload_files(files = ['training_pd.csv'],
                       target_path = 'train-dataset/tabular/',
                       overwrite = True,
                       show_progress = True)
dataset_training = Dataset.Tabular.from_delimited_files(path = [(datastore, 'train-dataset/tabular/training_pd.csv')])
```
![Afbeelding van geüploade gegevensset.](./media/azure-machine-learning-spark-notebook/upload-dataset.png)

## <a name="submit-an-automated-experiment"></a>Een geautomatiseerd experiment verzenden

In de volgende secties wordt stapsgewijs uitgelegd hoe u een geautomatiseerd machine learning-experiment verzendt.

### <a name="define-training-settings"></a>Trainingsinstellingen definiëren
1. U moet de experimentparameter en modelinstellingen voor training definiëren om een experiment te verzenden. Zie [geautomatiseerde machine learning-experimenten configureren in Python](../../machine-learning/how-to-configure-auto-train.md) voor een volledige lijst met instellingen.

   ```python
   import logging

   automl_settings = {
       "iteration_timeout_minutes": 10,
       "experiment_timeout_minutes": 30,
       "enable_early_stopping": True,
       "primary_metric": 'r2_score',
       "featurization": 'auto',
       "verbosity": logging.INFO,
       "n_cross_validations": 2}
   ```

1. Geef de gedefinieerde trainingsinstellingen door als `kwargs`-parameter voor een `AutoMLConfig`-object. Omdat u Spark gebruikt, moet u ook de Spark-context doorgeven die automatisch toegankelijk is voor de variabele ```sc```. Daarnaast geeft u de trainingsgegevens en het modeltype op. In dit geval is dat regressie.

   ```python
   from azureml.train.automl import AutoMLConfig

   automl_config = AutoMLConfig(task='regression',
                                debug_log='automated_ml_errors.log',
                                training_data = dataset_training,
                                spark_context = sc,
                                model_explainability = False, 
                                label_column_name ="fareAmount",**automl_settings)
   ```

> [!NOTE]
>De stappen voor vooraf bewerken van geautomatiseerde machine learning worden onderdeel van het onderliggende model. Deze stappen omvatten functienormalisatie, het verwerken van ontbrekende gegevens en het converteren van tekst naar numerieke waarden. Wanneer u het model voor voorspellingen gebruikt, worden dezelfde voorverwerkingsstappen die tijdens de training zijn toegepast, automatisch toegepast op uw invoergegevens.

### <a name="train-the-automatic-regression-model"></a>Het automatische regressiemodel trainen 
Vervolgens maakt u een experimentobject in uw Azure Machine Learning-werkruimte. Een experiment fungeert als een container voor uw afzonderlijke uitvoeringen. 

```python
from azureml.core.experiment import Experiment

# Start an experiment in Azure Machine Learning
experiment = Experiment(ws, "aml-synapse-regression")
tags = {"Synapse": "regression"}
local_run = experiment.submit(automl_config, show_output=True, tags = tags)

# Use the get_details function to retrieve the detailed output for the run.
run_details = local_run.get_details()
```
Wanneer het experiment is voltooid, retourneert de uitvoer informatie over de voltooide iteraties. Voor elke iteratie ziet u het modeltype, de uitvoeringsduur en de nauwkeurigheid van de training. In het veld **BESTE** wordt de beste trainingsscore op basis van uw type metrische waarde bijgehouden.

![Schermopname van modeluitvoer.](./media/azure-machine-learning-spark-notebook/model-output.png)

> [!NOTE]
> Nadat u het geautomatiseerde machine learning-experiment hebt verzonden, worden verschillende iteraties en modeltypen uitgevoerd. Het uitvoeren duurt doorgaans 60-90 minuten. 

### <a name="retrieve-the-best-model"></a>Het beste model ophalen
Om het beste model uit uw iteraties te selecteren, gebruiken we de functie ```get_output``` om het model te retourneren dat het beste is uitgevoerd en aangepast. Met de volgende code wordt het model opgehaald dat het beste is uitgevoerd en aangepast voor alle geregistreerde metrische gegevens of voor een bepaalde iteratie.

```python
# Get best model
best_run, fitted_model = local_run.get_output()
```

### <a name="test-model-accuracy"></a>Modelnauwkeurigheid testen
1. Als u de nauwkeurigheid van het model wilt testen, gebruikt u het beste model om de ritprijzen van taxi’s te voorspellen in de testgegevensset. De functie ```predict``` maakt gebruik van het beste model en voorspelt de y-waarden (ritprijs) uit de validatiegegevensset. 

   ```python
   # Test best model accuracy
   validation_data_pd = validation_data.toPandas()
   y_test = validation_data_pd.pop("fareAmount").to_frame()
   y_predict = fitted_model.predict(validation_data_pd)
   ```

1. De wortel van de gemiddelde kwadratische fout is een veelgebruikte meting van de verschillen tussen voorbeeldwaarden die zijn voorspeld met een model, en de waargenomen waarden. U berekent de wortel van de gemiddelde kwadratische fout van de resultaten door het `y_test`-gegevensframe te vergelijken met de waarden die zijn voorspeld met het model. 

   Met de functie ```mean_squared_error``` wordt de gemiddelde gekwadrateerde fout berekend tussen twee matrices. Vervolgens neemt u de vierkantswortel uit het resultaat. Deze metrische waarde geeft aan in welke mate de voorspelde taxiprijzen ongeveer afwijken van de werkelijke waarden.

   ```python
   from sklearn.metrics import mean_squared_error
   from math import sqrt

   # Calculate Root Mean Square Error
   y_actual = y_test.values.flatten().tolist()
   rmse = sqrt(mean_squared_error(y_actual, y_predict))

   print("Root Mean Square Error:")
   print(rmse)
   ```

   ```Output
   Root Mean Square Error:
   2.309997102577151
   ```
   De wortel van de gemiddelde kwadratische fout is een goede meting van hoe nauwkeurig het model het antwoord voorspelt. In de resultaten ziet u dat het model redelijk goed is in het voorspellen van de ritprijzen van taxi’s op basis van de functies van de gegevensset, met een afwijking van maximaal $2,00.

1. Voer de volgende code uit om het gemiddelde absolute foutpercentage te berekenen. Deze metrische waarde toont nauwkeurigheid als een percentage van de fout. Dit gebeurt door een absoluut verschil tussen elke voorspelde en werkelijke waarde te berekenen, en alle verschillen vervolgens op te tellen. Vervolgens wordt deze som weergegeven als een percentage van het totaal van de werkelijke waarden.

   ```python
   # Calculate MAPE and Model Accuracy 
   sum_actuals = sum_errors = 0

   for actual_val, predict_val in zip(y_actual, y_predict):
       abs_error = actual_val - predict_val
       if abs_error < 0:
           abs_error = abs_error * -1

       sum_errors = sum_errors + abs_error
       sum_actuals = sum_actuals + actual_val

   mean_abs_percent_error = sum_errors / sum_actuals

   print("Model MAPE:")
   print(mean_abs_percent_error)
   print()
   print("Model Accuracy:")
   print(1 - mean_abs_percent_error)
   ```

   ```Output
   Model MAPE:
   0.03655071038487368

   Model Accuracy:
   0.9634492896151263
   ```
   In de twee metrische gegevens voor de nauwkeurigheid van de voorspellingen ziet u dat het model redelijk goed is in het voorspellen van de ritprijzen van taxi’s op basis van de functies van de gegevensset. 

1. Na het aanpassen van een lineair regressiemodel moet u nu bepalen hoe goed het model past bij de gegevens. Hiervoor tekent u de werkelijke ritprijs ten opzichte van de voorspelde uitvoer. Daarnaast berekent u ook de R-kwadraatsmeting om te begrijpen hoe dicht de gegevens zich bij de aangepaste regressieregel bevinden.

   ```python
   import matplotlib.pyplot as plt
   import numpy as np
   from sklearn.metrics import mean_squared_error, r2_score

   # Calculate the R2 score using the predicted and actual fare prices
   y_test_actual = y_test["fareAmount"]
   r2 = r2_score(y_test_actual, y_predict)

   # Plot the Actual vs Predicted Fare Amount Values
   plt.style.use('ggplot')
   plt.figure(figsize=(10, 7))
   plt.scatter(y_test_actual,y_predict)
   plt.plot([np.min(y_test_actual), np.max(y_test_actual)], [np.min(y_test_actual), np.max(y_test_actual)], color='lightblue')
   plt.xlabel("Actual Fare Amount")
   plt.ylabel("Predicted Fare Amount")
   plt.title("Actual vs Predicted Fare Amont R^2={}".format(r2))
   plt.show()

   ```
   ![Schermopname van regressietekening.](./media/azure-machine-learning-spark-notebook/fare-amount.png)

   Uit de resultaten blijkt dat de R-kwadraatsmeting geldt voor 95 procent van de variantie. Dit wordt ook gevalideerd door de werkelijke grafiek versus de waargenomen grafiek. Hoe meer variantie er is verantwoord met het regressiemodel, hoe dichter de gegevenspunten zich bij de aangepaste regressieregel bevinden.  

## <a name="register-model-to-azure-machine-learning"></a>Model registreren bij Azure Machine Learning
Nadat u het beste model hebt gevalideerd, kunt u dit registreren bij Azure Machine Learning. Vervolgens kunt u het geregistreerde model downloaden of implementeren, en alle bestanden ontvangen die u hebt geregistreerd.

```python
description = 'My automated ML model'
model_path='outputs/model.pkl'
model = best_run.register_model(model_name = 'NYCGreenTaxiModel', model_path = model_path, description = description)
print(model.name, model.version)
```
```Output
NYCGreenTaxiModel 1
```
## <a name="view-results-in-azure-machine-learning"></a>Resultaten bekijken in Azure Machine Learning
Ten slotte kunt u ook de resultaten van herhalingen bekijken door naar het experiment te gaan in uw Azure Machine Learning-werkruimte. Hier kunt u extra informatie vinden over de status van de uitvoering, de gebruikte modellen, en andere metrische modelgegevens. 

![Schermopname van Azure Machine Learning-werkruimte.](./media/azure-machine-learning-spark-notebook/azure-machine-learning-workspace.png)

## <a name="next-steps"></a>Volgende stappen
- [Azure Synapse Analytics](../index.yml)
- [Zelfstudie: Een machine learning-app bouwen met Apache Spark MLlib en Azure Synapse Analytics](./apache-spark-machine-learning-mllib-notebook.md)