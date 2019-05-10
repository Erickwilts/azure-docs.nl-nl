---
title: Geautomatiseerde ML-experimenten maken
titleSuffix: Azure Machine Learning service
description: Geautomatiseerde machine learning, kiest een algoritme voor u en genereert een model dat gereed is voor implementatie. Meer informatie over de opties die u kunt met geautomatiseerde machine learning-experimenten configureren.
author: nacharya1
ms.author: nilesha
ms.reviewer: sgilley
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 05/02/2019
ms.custom: seodec18
ms.openlocfilehash: 64ba7096f181371a378708e024f46bce17449e98
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/09/2019
ms.locfileid: "65510590"
---
# <a name="configure-automated-ml-experiments-in-python"></a>Geautomatiseerde ML experimenten in Python configureren

In deze handleiding informatie over het definiëren van verschillende configuratie-instellingen van uw geautomatiseerde voor machine learning gebruikt met de [Azure Machine Learning-SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py). Geautomatiseerde machine learning, kiest een algoritme en hyperparameters voor u en genereert een model dat gereed is voor implementatie. Er zijn diverse opties, kunt u geautomatiseerde machine learning-experimenten configureren.

Voor voorbeelden van een geautomatiseerde voor machine learning gebruikt, raadpleegt u [zelfstudie: Een model classificatie met geautomatiseerde machine learning te trainen](tutorial-auto-train-models.md) of [trainen van modellen met geautomatiseerde machine learning in de cloud](how-to-auto-train-remote.md).

Configuratie-opties zijn beschikbaar in geautomatiseerde machine learning:

* Selecteer het type experiment: Classificatie, regressie of Timeseries-prognoses
* De gegevensbron, indelingen en ophalen van gegevens
* Kies uw compute-doel: lokale of externe
* Geautomatiseerde machine learning-experiment-instellingen
* Een geautomatiseerde machine learning-experiment uitvoeren
* Model metrische gegevens verkennen
* Registreer en implementeer model

Als u liever een geen code-ervaring, kunt u ook [uw geautomatiseerde experimenten voor machine learning in Azure portal maken](how-to-create-portal-experiments.md).

## <a name="select-your-experiment-type"></a>Selecteer het type experiment

Voordat u uw experiment, moet u het type van machine learning probleem, u het oplossen van bepalen. Geautomatiseerde machine learning ondersteunt taaktypen classificatie, regressie en prognose.

Geautomatiseerde machine learning ondersteunt de volgende algoritmen tijdens de automatisering en het afstemmen van proces. Als een gebruiker is er niet nodig voor u het algoritme opgeven. DNN algoritmen zijn beschikbaar tijdens de training, stel geautomatiseerde ML DNN modellen niet samen.

Classificatie | Regressie | Time Series-prognoses
|-- |-- |--
[Logistic Regression](https://scikit-learn.org/stable/modules/linear_model.html#logistic-regression)| [Elastische Net](https://scikit-learn.org/stable/modules/linear_model.html#elastic-net)| [Elastische Net](https://scikit-learn.org/stable/modules/linear_model.html#elastic-net)
[Licht GBM](https://lightgbm.readthedocs.io/en/latest/index.html)|[Licht GBM](https://lightgbm.readthedocs.io/en/latest/index.html)|[Licht GBM](https://lightgbm.readthedocs.io/en/latest/index.html)
[Versterking van kleurovergang](https://scikit-learn.org/stable/modules/ensemble.html#classification)|[Versterking van kleurovergang](https://scikit-learn.org/stable/modules/ensemble.html#regression)|[Versterking van kleurovergang](https://scikit-learn.org/stable/modules/ensemble.html#regression)
[Beslissingsstructuur](https://scikit-learn.org/stable/modules/tree.html#decision-trees)|[Beslissingsstructuur](https://scikit-learn.org/stable/modules/tree.html#regression)|[Beslissingsstructuur](https://scikit-learn.org/stable/modules/tree.html#regression)
[K dichtstbijzijnde Neighbors](https://scikit-learn.org/stable/modules/neighbors.html#nearest-neighbors-regression)|[K dichtstbijzijnde Neighbors](https://scikit-learn.org/stable/modules/neighbors.html#nearest-neighbors-regression)|[K dichtstbijzijnde Neighbors](https://scikit-learn.org/stable/modules/neighbors.html#nearest-neighbors-regression)
[Lineaire SVC](https://scikit-learn.org/stable/modules/svm.html#classification)|[LARS Lasso](https://scikit-learn.org/stable/modules/linear_model.html#lars-lasso)|[LARS Lasso](https://scikit-learn.org/stable/modules/linear_model.html#lars-lasso)
[C-Support Vector classificatie (SVC)](https://scikit-learn.org/stable/modules/svm.html#classification)|[De toolkit leren met stochastische Gradiëntdaling (SGD)](https://scikit-learn.org/stable/modules/sgd.html#regression)|[De toolkit leren met stochastische Gradiëntdaling (SGD)](https://scikit-learn.org/stable/modules/sgd.html#regression)
[Willekeurige Forest](https://scikit-learn.org/stable/modules/ensemble.html#random-forests)|[Willekeurige Forest](https://scikit-learn.org/stable/modules/ensemble.html#random-forests)|[Willekeurige Forest](https://scikit-learn.org/stable/modules/ensemble.html#random-forests)
[Zeer willekeurige structuren](https://scikit-learn.org/stable/modules/ensemble.html#extremely-randomized-trees)|[Zeer willekeurige structuren](https://scikit-learn.org/stable/modules/ensemble.html#extremely-randomized-trees)|[Zeer willekeurige structuren](https://scikit-learn.org/stable/modules/ensemble.html#extremely-randomized-trees)
[Xgboost](https://xgboost.readthedocs.io/en/latest/parameter.html)|[Xgboost](https://xgboost.readthedocs.io/en/latest/parameter.html)| [Xgboost](https://xgboost.readthedocs.io/en/latest/parameter.html)
[DNN classificatie](https://www.tensorflow.org/api_docs/python/tf/estimator/DNNClassifier)|[DNN regressor zijn](https://www.tensorflow.org/api_docs/python/tf/estimator/DNNRegressor) | [DNN regressor zijn](https://www.tensorflow.org/api_docs/python/tf/estimator/DNNRegressor)|
[DNN lineaire classificatie](https://www.tensorflow.org/api_docs/python/tf/estimator/LinearClassifier)|[Lineaire regressor zijn](https://www.tensorflow.org/api_docs/python/tf/estimator/LinearRegressor)|[Lineaire regressor zijn](https://www.tensorflow.org/api_docs/python/tf/estimator/LinearRegressor)
[Naive Bayes](https://scikit-learn.org/stable/modules/naive_bayes.html#bernoulli-naive-bayes)|
[De toolkit leren met stochastische Gradiëntdaling (SGD)](https://scikit-learn.org/stable/modules/sgd.html#sgd)|


## <a name="data-source-and-format"></a>Gegevensbron en indeling
Geautomatiseerde machine learning biedt ondersteuning voor gegevens die zich bevinden op het lokale bureaublad of in de cloud zoals Azure Blob Storage. De gegevens kunnen worden gelezen in scikit-informatie over ondersteunde gegevensindelingen. U kunt de gegevens in lezen:
* Numpy matrices X (kenmerken)- en y (doelvariabele of ook wel bekend als label)
* Pandas dataframe

Voorbeelden:

*   Numpy matrices

    ```python
    digits = datasets.load_digits()
    X_digits = digits.data
    y_digits = digits.target
    ```

*   Pandas dataframe

    ```python
    import pandas as pd
    df = pd.read_csv("https://automldemods.blob.core.windows.net/datasets/PlayaEvents2016,_1.6MB,_3.4k-rows.cleaned.2.tsv", delimiter="\t", quotechar='"')
    # get integer labels
    df = df.drop(["Label"], axis=1)
    df_train, _, y_train, _ = train_test_split(df, y, test_size=0.1, random_state=42)
    ```

## <a name="fetch-data-for-running-experiment-on-remote-compute"></a>Ophalen van gegevens voor het experiment uitvoeren op externe compute

Als u van een externe compute gebruikmaakt om uit te voeren van uw experiment, het ophalen van gegevens moet worden verpakt in een afzonderlijke python-script `get_data()`. Met dit script wordt uitgevoerd op de externe compute waar de geautomatiseerde machine learning-experiment wordt uitgevoerd. `get_data` Elimineert de noodzaak voor het ophalen van de gegevens via de kabel voor elke iteratie. Zonder `get_data`, uw experiment mislukken wanneer u op de externe compute uitvoert.

Hier volgt een voorbeeld van `get_data`:

```python
%%writefile $project_folder/get_data.py
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import LabelEncoder
def get_data(): # Burning man 2016 data
    df = pd.read_csv("https://automldemods.blob.core.windows.net/datasets/PlayaEvents2016,_1.6MB,_3.4k-rows.cleaned.2.tsv", delimiter="\t", quotechar='"')
    # get integer labels
    le = LabelEncoder()
    le.fit(df["Label"].values)
    y = le.transform(df["Label"].values)
    df = df.drop(["Label"], axis=1)
    df_train, _, y_train, _ = train_test_split(df, y, test_size=0.1, random_state=42)
    return { "X" : df, "y" : y }
```

In uw `AutoMLConfig` -object, geeft u de `data_script` parameter en geef het pad naar de `get_data` scriptbestand vergelijkbaar met hieronder:

```python
automl_config = AutoMLConfig(****, data_script=project_folder + "/get_data.py", **** )
```

`get_data` script kan retourneren:

Sleutel | Type | Sluiten elkaar wederzijds uit met    | Description
---|---|---|---
X | Pandas Dataframe of Numpy matrix | data_train, label, kolommen |  Alle functies te trainen met
Y | Pandas Dataframe of Numpy matrix |   label   | Voeg een label te trainen met gegevens. Voor de classificatie moet een matrix van gehele getallen zijn.
X_valid | Pandas Dataframe of Numpy matrix   | data_train, label | _Optionele_ alle functies waarmee moet worden gevalideerd. Indien niet opgegeven, X wordt verdeeld tussen train en valideren
y_valid |   Pandas Dataframe of Numpy matrix | data_train, label | _Optionele_ de labelgegevens te valideren met. Indien niet opgegeven, y wordt verdeeld tussen train en valideren
sample_weight | Pandas Dataframe of Numpy matrix |   data_train, label, kolommen| _Optionele_ een gewicht voor elk voorbeeld. Gebruik deze sjabloon wanneer u wilt toewijzen van verschillende gewichten voor uw data-punten
sample_weight_valid | Pandas Dataframe of Numpy matrix | data_train, label, kolommen |    _Optionele_ een gewicht voor elk Validatievoorbeeld. Indien niet opgegeven, sample_weight wordt verdeeld tussen train en valideren
data_train |    Pandas Dataframe |  X, y, X_valid, y_valid |    Alle gegevens (functies en label) te trainen met
label | string  | X, y, X_valid, y_valid |  Welke kolom in data_train Hiermee geeft u het label
Kolommen | matrix van tekenreeksen  ||  _Optionele_ Whitelist van kolommen moet worden gebruikt voor functies
cv_splits_indices   | Matrix van gehele getallen ||  _Optionele_ lijst van indexen in het splitsen van de gegevens voor cross-validatie

### <a name="load-and-prepare-data-using-data-prep-sdk"></a>Laden en voorbereiden van gegevens met behulp van gegevensvoorbereiding SDK
Geautomatiseerde experimenten voor machine learning biedt ondersteuning voor het laden van gegevens en transformaties met behulp van de SDK voor gegevensvoorbereiding. Met de SDK biedt de mogelijkheid om

>* Laden uit veel bestandstypen met het parseren van de parameter Deductie (codering, scheidingsteken, headers)
>* Type converteren met behulp van Deductie tijdens het laden van bestand
>* Ondersteuning voor MS SQL Server en Azure Data Lake Storage-verbindingen
>* Met behulp van een expressie kolom toevoegen
>* Ontbrekende waarden worden toegerekend
>* Kolom afleiden per voorbeeld
>* Filteren
>* Aangepaste Python-transformaties

Voor meer informatie over de gegevens prep sdk verwijzen de [over het voorbereiden van gegevens voor het modelleren van artikel](how-to-load-data.md).
Hieronder volgt een voorbeeld van het laden van gegevens met behulp van data prep sdk.
```python
# The data referenced here was pulled from `sklearn.datasets.load_digits()`.
simple_example_data_root = 'https://dprepdata.blob.core.windows.net/automl-notebook-data/'
X = dprep.auto_read_file(simple_example_data_root + 'X.csv').skip(1)  # Remove the header row.
# You can use `auto_read_file` which intelligently figures out delimiters and datatypes of a file.

# Here we read a comma delimited file and convert all columns to integers.
y = dprep.read_csv(simple_example_data_root + 'y.csv').to_long(dprep.ColumnSelector(term='.*', use_regex = True))
```

## <a name="train-and-validation-data"></a>Train en validatie

Kunt u afzonderlijke train en validatie ingesteld via get_data() of rechtstreeks in de `AutoMLConfig` methode.

## <a name="cross-validation-split-options"></a>Opties voor cross-validatie splitsen

### <a name="k-folds-cross-validation"></a>K-vouwen Kruisvalidatie

Gebruik `n_cross_validations` instelling om op te geven van het aantal cross-bevestigingen. De set trainingsgegevens wordt willekeurig worden gesplitst in `n_cross_validations` vouwen van gelijke grootte. Tijdens elke cross validatie ronde, een van de vouwen dat wordt gebruikt voor de validatie van het model is getraind op de resterende vouwen. Dit proces wordt herhaald voor `n_cross_validations` totdat elke vouwen één keer wordt gebruikt als validatie afgerond. De gemiddelde scores voor alle `n_cross_validations` Rondt worden gerapporteerd, en de bijbehorende model zal opnieuw worden getraind op de gehele training gegevensset. 

### <a name="monte-carlo-cross-validation-repeated-random-sub-sampling"></a>Validatie van Monte Carlo Cross (herhaalde onderliggende steekproeven)

Gebruik `validation_size` om op te geven van het percentage van de training-gegevensset die moet worden gebruikt voor validatie en gebruik `n_cross_validations` om op te geven van het aantal cross-bevestigingen. Tijdens elke cross-validatie afgerond, een subset van de grootte van `validation_size` willekeurig worden geselecteerd voor de validatie van het model is getraind op de resterende gegevens. Ten slotte het gemiddelde voor alle beoordeelt `n_cross_validations` Rondt worden gerapporteerd, en de bijbehorende model zal opnieuw worden getraind op de gehele training gegevensset. Monte Carlo wordt niet ondersteund voor time series-prognoses.

### <a name="custom-validation-dataset"></a>Aangepaste validatie-gegevensset

Aangepaste validatie-gegevensset als willekeurige splitsen niet geaccepteerd wordt, meestal time series-gegevens of imbalanced gegevens gebruiken. U kunt uw eigen gegevensset validatie opgeven. Het model wordt geëvalueerd op basis van de gegevensset validatie is opgegeven in plaats van willekeurige gegevensset.

## <a name="compute-to-run-experiment"></a>COMPUTE experiment uitvoeren

Vervolgens kunt u bepalen waar u het model wordt getraind. Een geautomatiseerde machine learning-trainingsexperiment kunt uitvoeren op de volgende compute-opties:
*   Uw lokale machine, zoals een lokale desktop of laptop – algemeen wanneer u kleine gegevensset hebt en u bent nog steeds in de fase verkennen.
*   Een externe computer in de cloud, [Azure Machine Learning-beheerd Compute](concept-azure-machine-learning-architecture.md#managed-and-unmanaged-compute-targets) is een beheerde service die het mogelijk om te trainen op clusters virtuele machines van Azure machine learning-modellen.

Zie de [GitHub-site](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning) bijvoorbeeld-notebooks met lokale en externe compute-doelen.

*   Een Azure Databricks-cluster in uw Azure-abonnement. U vindt meer informatie hier: [Setup Azure Databricks-cluster voor geautomatiseerde ML](how-to-configure-environment.md#azure-databricks)

Zie de [GitHub-site](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/azure-databricks/automl) bijvoorbeeld-notebooks met Azure Databricks.

<a name='configure-experiment'></a>

## <a name="configure-your-experiment-settings"></a>Uw experiment-instellingen configureren

Er zijn diverse opties, kunt u uw geautomatiseerde machine learning-experiment configureren. Deze parameters zijn ingesteld door het instantiëren van een `AutoMLConfig` object. Zie de [AutoMLConfig klasse](https://docs.microsoft.com/python/api/azureml-train-automl/azureml.train.automl.automlconfig?view=azure-ml-py) voor een volledige lijst met parameters.  

Voorbeelden zijn:

1.  Classificatie-experiment met behulp van AUC gewogen als de primaire metrische gegevens met een maximale tijd van 12.000 seconden per herhaling, met het experiment te beëindigen nadat u hebt 50 iteraties en 2 cross validatie vouwen.

    ```python
    automl_classifier = AutoMLConfig(
        task='classification',
        primary_metric='AUC_weighted',
        max_time_sec=12000,
        iterations=50,
        blacklist_models='XGBoostClassifier',
        X=X,
        y=y,
        n_cross_validations=2)
    ```
2.  Hieronder volgt een voorbeeld van een set regressie-experiment na 100 herhalingen, elke herhaling duurt maximaal 600 seconden met 5 validatie kruislings vouwen.

    ```python
    automl_regressor = AutoMLConfig(
        task='regression',
        max_time_sec=600,
        iterations=100,
        whitelist_models='kNN regressor'
        primary_metric='r2_score',
        X=X,
        y=y,
        n_cross_validations=5)
    ```

De drie verschillende `task` parameterwaarden bepalen de lijst met algoritmen om toe te passen.  Gebruik de `whitelist` of `blacklist` parameters voor het verder aanpassen iteraties met de beschikbare algoritmen wilt opnemen of uitsluiten. De lijst met ondersteunde modellen kan worden gevonden op [SupportedAlgorithms klasse](https://docs.microsoft.com/python/api/azureml-train-automl/azureml.train.automl.constants.supportedalgorithms?view=azure-ml-py).

## <a name="primary-metric"></a>Primaire metrische gegevens
De primaire metric; zoals wordt weergegeven in de bovenstaande voorbeelden bepaalt de metrische gegevens moet worden gebruikt tijdens het trainen van het model voor optimalisatie. De primaire metrische gegevens die u kunt selecteren, wordt bepaald door het taaktype dat u kiest. Hieronder vindt u een lijst met beschikbare metrische gegevens.

|Classificatie | Regressie | Time Series-prognoses
|-- |-- |--
|accuracy| spearman_correlation | spearman_correlation
|AUC_weighted | normalized_root_mean_squared_error | normalized_root_mean_squared_error
|average_precision_score_weighted | r2_score | r2_score
|norm_macro_recall | normalized_mean_absolute_error | normalized_mean_absolute_error
|precision_score_weighted |

## <a name="data-preprocessing--featurization"></a>Gegevens voorverwerken & parametrisatie

In elke geautomatiseerde machine learning-experiment, uw gegevens is [automatisch wordt geschaald en genormaliseerd](concept-automated-ml.md#preprocess) om u te helpen algoritmen presteren.  U kunt echter ook aanvullende voorverwerking/parametrisatie, zoals ontbrekende waarden toerekening, codering en -transformaties inschakelen. [Meer informatie over welke parametrisatie opgenomen is](how-to-create-portal-experiments.md#preprocess). 

Geef zodat deze parametrisatie `"preprocess": True` voor de [ `AutoMLConfig` klasse](https://docs.microsoft.com/python/api/azureml-train-automl/azureml.train.automl.automlconfig?view=azure-ml-py).

## <a name="time-series-forecasting"></a>Time Series-prognoses
Voor time series prognoses taaktype hebt u aanvullende parameters te definiëren.
1. time_column_name - dit is een vereiste parameter waarin de naam van de kolom zijn gedefinieerd in uw trainingen met datum/tijd gegevensreeks. 
1. max_horizon - Hiermee definieert u de hoeveelheid tijd die u wilt om te voorspellen op basis van de periodiciteit van de trainingsgegevens. Zo hebt u trainingsgegevens met dagelijkse tijd korrels, definieert u hoe ver uit in de dagen die u wilt dat het model te trainen voor.
1. grain_column_names - Hiermee definieert u de naam van de kolommen die afzonderlijke time series-gegevens in uw trainingsgegevens bevatten. Als u bent verkoopprognoses van een bepaalde merk door store, zou u kolommen store en merk definiëren als uw kolommen tijdsinterval.

Zie het voorbeeld van deze instellingen hieronder wordt gebruikt, laptop-voorbeeld is beschikbaar [hier](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-orange-juice-sales/auto-ml-forecasting-orange-juice-sales.ipynb).

```python
# Setting Store and Brand as grains for training.
grain_column_names = ['Store', 'Brand']
nseries = data.groupby(grain_column_names).ngroups

# View the number of time series data with defined grains
print('Data contains {0} individual time-series.'.format(nseries))
```

```python
time_series_settings = {
    'time_column_name': time_column_name,
    'grain_column_names': grain_column_names,
    'drop_column_names': ['logQuantity'],
    'max_horizon': n_test_periods
}

automl_config = AutoMLConfig(task='forecasting',
                             debug_log='automl_oj_sales_errors.log',
                             primary_metric='normalized_root_mean_squared_error',
                             iterations=10,
                             X=X_train,
                             y=y_train,
                             n_cross_validations=5,
                             path=project_folder,
                             verbosity=logging.INFO,
                             **time_series_settings)
```

## <a name="run-experiment"></a>Experiment uit te voeren

Het experiment uitvoeren en het genereren van een model verzenden. Geeft de `AutoMLConfig` naar de `submit` methode voor het genereren van het model.

```python
run = experiment.submit(automl_config, show_output=True)
```

>[!NOTE]
>Afhankelijkheden zijn geïnstalleerd op een nieuwe virtuele machine.  Het kan maximaal 10 minuten duren voordat uitvoer wordt weergegeven.
>Instellen van `show_output` naar `True` resulteert in de uitvoer wordt weergegeven op de console.

## <a name="exit-criteria"></a>Afsluitcriteria 
Er enkele opties kunt u definiëren voor het voltooien van uw experiment.
1. Er zijn geen Criteria - als u niet dat een definieert afsluiten parameters die het experiment wordt voortgezet totdat er geen verdere voortgang is gemaakt op uw primaire metrische gegevens. 
1. Nummer van iteraties - definieert u het aantal iteraties voor het experiment om uit te voeren. U kunt optioneel iteration_timeout_minutes voor het definiëren van een tijdslimiet in minuten per elke herhaling toevoegen.
1. Afgesloten na een tijdsduur - experiment_timeout_minutes gebruiken in de instellingen die u kunt opgeven hoe lang in minuten moet een experiment blijven uitvoeren.
1. Sluit af nadat een score is bereikt - met behulp van experiment_exit_score die kunt u het experiment uitvoeren nadat een score op basis van uw primaire meetwaarde is bereikt.

## <a name="explore-model-metrics"></a>Model metrische gegevens verkennen
U kunt uw resultaten weergeven in een widget of een inline als u zich in een notitieblok. Zie [bijhouden en evalueren van modellen](how-to-track-experiments.md#view-run-details) voor meer informatie.


### <a name="classification-metrics"></a>Classificatie metrische gegevens
De volgende metrische gegevens worden opgeslagen in elke iteratie van een classificatietaak.

|Metric|Description|Berekening|Extra Parameters
--|--|--|--|
AUC_Macro| AUC is het gebied onder de ontvanger operationele Characteristic-Curve. Macro is het rekenkundige gemiddelde van de AUC voor elke categorie.  | [Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html) | gemiddelde = "macro"|
AUC_Micro| AUC is het gebied onder de ontvanger operationele Characteristic-Curve. Micro wordt wereldwijd berekend door het echt positieven en fout-positieven van elke klasse te combineren| [Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html) | gemiddelde = "micro"|
AUC_Weighted  | AUC is het gebied onder de ontvanger operationele Characteristic-Curve. Gewogen is het rekenkundige gemiddelde van de score van elke klasse, gewogen door het aantal waar elke klasse-instanties| [Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html)|gemiddelde = "gewogen"
accuracy|Nauwkeurigheid is het percentage van de voorspelde labels die precies overeenkomen met de waarde true labels. |[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.accuracy_score.html) |Geen|
average_precision_score_macro|Gemiddelde precisie bevat een overzicht van een curve precisie-/ oproepdiagram als het gewogen gemiddelde van Precision-systemen op elke drempelwaarde, met de toename in het intrekken van de vorige drempel gebruikt als het gewicht behaald. Macro is het rekenkundige gemiddelde van de gemiddelde precisie score van elke klasse|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html)|gemiddelde = "macro"|
average_precision_score_micro|Gemiddelde precisie bevat een overzicht van een curve precisie-/ oproepdiagram als het gewogen gemiddelde van Precision-systemen op elke drempelwaarde, met de toename in het intrekken van de vorige drempel gebruikt als het gewicht behaald. Micro wordt wereldwijd berekend door het echt positieven en fout-positieven op elke afsluitdatum zoekfunctionaliteit door te combineren|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html)|gemiddelde = "micro"|
average_precision_score_weighted|Gemiddelde precisie bevat een overzicht van een curve precisie-/ oproepdiagram als het gewogen gemiddelde van Precision-systemen op elke drempelwaarde, met de toename in het intrekken van de vorige drempel gebruikt als het gewicht behaald. Gewogen is het rekenkundige gemiddelde van de gemiddelde precisie score voor elke objectklasse, gewogen door het aantal waar elke klasse-instanties|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html)|gemiddelde = "gewogen"|
balanced_accuracy|Met gelijke taakverdeling nauwkeurigheid is het rekenkundige gemiddelde van terugroeping voor elke categorie.|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|gemiddelde = "macro"|
f1_score_macro|F1 score is het gemiddelde harmonische van precisie- en intrekken. Macro is het rekenkundige gemiddelde van F1 score voor elke objectklasse|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html)|gemiddelde = "macro"|
f1_score_micro|F1 score is het gemiddelde harmonische van precisie- en intrekken. Micro wordt wereldwijd berekend door het tellen van de totale echt positieven, false negatieven en fout-positieven|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html)|gemiddelde = "micro"|
f1_score_weighted|F1 score is het gemiddelde harmonische van precisie- en intrekken. Gewogen gemiddelde frequentie van de klasse van F1 score voor elke objectklasse|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html)|gemiddelde = "gewogen"|
log_loss|Dit is de verlies-functie die wordt gebruikt voor logistieke regressie (wordt) en -extensies van het zoals neurale netwerken, gedefinieerd als de negatieve log kans van de waarde true labels een probabilistic classificatie voorspellingen gegeven. Label voor een enkele voorbeeld met waar yt in {0,1} en de verwachte kans yp die yt = 1, het verlies van het logboek is - P aanmelden (yt&#124;yp) =-(yt log(yp) + (1 - yt) logboek (1 - yp))|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.log_loss.html)|Geen|
norm_macro_recall|Genormaliseerde Macro intrekken is Macro intrekken genormaliseerd zodat willekeurige prestaties een score van 0 krijgt en perfecte prestaties een score van 1 krijgt. Dit wordt bereikt door norm_macro_recall: (recall_score_macro - R) = /(1-R), waarbij R de verwachte waarde van recall_score_macro voor willekeurige voorspellingen (dat wil zeggen, R = 0,5 voor binaire classificatie) en R=(1/C) voor C-klasse classificatie problemen|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|gemiddelde = "macro" en vervolgens (recall_score_macro - R) /(1-R), waarbij R de verwachte waarde van recall_score_macro voor willekeurige voorspellingen (dat wil zeggen, R = 0,5 voor binaire classificatie) en R=(1/C) voor C-klasse classificatie problemen|
precision_score_macro|De precisie is het percentage van de elementen die worden aangeduid als een bepaalde klasse die daadwerkelijk in die klasse. Macro is het rekenkundige gemiddelde van precisie voor elke objectklasse|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|gemiddelde = "macro"|
precision_score_micro|De precisie is het percentage van de elementen die worden aangeduid als een bepaalde klasse die daadwerkelijk in die klasse. Micro wordt wereldwijd berekend door het tellen van de totale echt positieven en fout-positieven|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|gemiddelde = "micro"|
precision_score_weighted|De precisie is het percentage van de elementen die worden aangeduid als een bepaalde klasse die daadwerkelijk in die klasse. Gewogen is het rekenkundige gemiddelde van precisie voor elke objectklasse, gewogen door het aantal waar elke klasse-instanties|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|gemiddelde = "gewogen"|
recall_score_macro|Intrekken is het percentage van de elementen in een bepaalde klasse daadwerkelijk die goed zijn gelabeld. Macro is het rekenkundige gemiddelde van terugroeping voor elke objectklasse|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|gemiddelde = "macro"|
recall_score_micro|Intrekken is het percentage van de elementen in een bepaalde klasse daadwerkelijk die goed zijn gelabeld. Micro wordt wereldwijd berekend door het totaal aantal echt positieven, false negatieven tellen|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|gemiddelde = "micro"|
recall_score_weighted|Intrekken is het percentage van de elementen in een bepaalde klasse daadwerkelijk die goed zijn gelabeld. Gewogen is het rekenkundige gemiddelde van terugroeping voor elke objectklasse, gewogen door het aantal waar elke klasse-instanties|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|gemiddelde = "gewogen"|
weighted_accuracy|Gewogen nauwkeurigheid is nauwkeurigheid waarin het gewicht gegeven aan elk voorbeeld gelijk is aan het aandeel van true-exemplaren in dat de waarde true voorbeeldklasse|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.accuracy_score.html)|sample_weight is gelijk aan het aandeel van die klasse voor elk element in de doel-vector|

### <a name="regression-and-time-series-forecasting-metrics"></a>Regressie en -tijd reeks prognose van metrische gegevens
De volgende metrische gegevens worden opgeslagen in elke iteratie een regressie of prognoses taak.

|Metric|Description|Berekening|Extra Parameters
--|--|--|--|
explained_variance|Uitgelegd afwijking is de verhouding die een wiskundige model gebruikersaccounts voor de variant van een bepaalde verzameling. Het is dat het percentage variantie van de oorspronkelijke gegevens om de afwijking van de fouten te verlagen. Wanneer het gemiddelde van de fouten 0 is, is het gelijk aan de afwijking van uitgelegd.|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.explained_variance_score.html)|Geen|
r2_score|R2 is de coëfficiënt bepalen of de procent vermindering in gekwadrateerde fouten ten opzichte van een basislijn-model dat u het gemiddelde weergeeft. Wanneer het gemiddelde van de fouten 0 is, is het gelijk aan de afwijking van uitgelegd.|[Berekening](https://scikit-learn.org/0.16/modules/generated/sklearn.metrics.r2_score.html)|Geen|
spearman_correlation|Spearman correlatie is een nonparametric meting van de monotonicity van de relatie tussen twee gegevenssets. In tegenstelling tot de correlatie Pearson de correlatie Spearman wordt niet vanuit gegaan dat beide gegevenssets normaal gesproken worden gedistribueerd. Dit varieert net als andere coëfficiënten correlatie tussen-1 en + 1 met 0, wat geen correlatie impliceert. Correlaties op basis van -1 of + 1 houdt in dat een exacte monotone relatie. Positieve correlaties impliceren dat x toeneemt, dus y wordt. Negatieve correlaties impliceren dat x toeneemt, y afneemt.|[Berekening](https://docs.scipy.org/doc/scipy-0.16.1/reference/generated/scipy.stats.spearmanr.html)|Geen|
mean_absolute_error|Mean absolute fout is de verwachte waarde van de absolute waarde van het verschil tussen het doel en de voorspelling|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_absolute_error.html)|Geen|
normalized_mean_absolute_error|Genormaliseerde mean absolute fout is mean Absolute Error gedeeld door het bereik van de gegevens|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_absolute_error.html)|Delen door het bereik van de gegevens|
median_absolute_error|Mediaan absolute fout is de mediaan van alle absolute verschillen tussen het doel en de voorspelling. Dit verlies is robuuste naar uitbijters.|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.median_absolute_error.html)|Geen|
normalized_median_absolute_error|Genormaliseerde mediaan absolute fout is mediaan absolute fout gedeeld door het bereik van de gegevens|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.median_absolute_error.html)|Delen door het bereik van de gegevens|
root_mean_squared_error|Root mean squared fout is de vierkantswortel van het verwachte gekwadrateerde verschil tussen het doel en de voorspelling|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_error.html)|Geen|
normalized_root_mean_squared_error|Genormaliseerde hoofdmap mean squared fout root mean squared fout gedeeld door het bereik van de gegevens is|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_error.html)|Delen door het bereik van de gegevens|
root_mean_squared_log_error|Root mean squared log-fout is de vierkantswortel van de verwachte gekwadrateerde logaritmische fout|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_log_error.html)|Geen|
normalized_root_mean_squared_log_error|Genormaliseerde Root mean squared log-fout is root mean squared log fout gedeeld door het bereik van de gegevens|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_log_error.html)|Delen door het bereik van de gegevens|


## <a name="understand-automated-ml-models"></a>Informatie over geautomatiseerde ML-modellen

Een model gemaakt met behulp van geautomatiseerde ML bevat de volgende stappen uit:
+ Geautomatiseerde feature-engineering (als voorverwerken = True)
+ Schaal/normalisering en het algoritme met hypermeter waarden

We maken het transparant voor deze informatie ophalen uit de uitvoer fitted_model van geautomatiseerde ML.

```python
automl_config = AutoMLConfig(…)
automl_run = experiment.submit(automl_config …)
best_run, fitted_model = automl_run.get_output()
```

### <a name="automated-feature-engineering"></a>Geautomatiseerde feature-engineering

Zie de lijst met vooraf en [geautomatiseerde feature-engineering](concept-automated-ml.md#preprocess) in dat geval wanneer voorverwerken = True.  

Bekijk het volgende voorbeeld:
+ Er zijn 4 invoerfuncties: A (Numeric), B (Numeric), C (Numeric), D (DateTime)
+ Numerieke functie C is verwijderd omdat deze geen ID-kolom met alleen unieke waarden
+ Numerieke functies A en B ontbrekende waarden en daarom worden toegerekende door gemiddelde
+ Datum/tijd-functie D is boommodel in 11 verschillende social engineering-functies

Gebruik deze 2 API's op de eerste stap van gemonteerd model om meer inzicht in.  Zie [dit voorbeeldnotitieblok](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning/forecasting-energy-demand).

+ API 1: `get_engineered_feature_names()` retourneert een lijst van Social engineering onderdeelnamen.

  Gebruik: 
  ```python
  fitted_model.named_steps['timeseriestransformer']. get_engineered_feature_names ()
  ```

  ```
  Output: ['A', 'B', 'A_WASNULL', 'B_WASNULL', 'year', 'half', 'quarter', 'month', 'day', 'hour', 'am_pm', 'hour12', 'wday', 'qday', 'week']
  ```

  Deze lijst bevat alle Social engineering onderdeelnamen.

  >[!Note]
  >Gebruik 'timeseriestransformer' voor taak = 'prognoses', anders gebruik 'datatransformer' voor 'regressie' of 'categorie'-taak. 

+ API 2: `get_featurization_summary()` parametrisatie samenvatting voor alle functies van de invoer retourneert.

  Gebruik: 
  ```python
  fitted_model.named_steps['timeseriestransformer'].get_featurization_summary()
  ```

  >[!Note]
  >Gebruik 'timeseriestransformer' voor taak = 'prognoses', anders gebruik 'datatransformer' voor 'regressie' of 'categorie'-taak.

  Uitvoer:
  ```
  [{'RawFeatureName': 'A',
    'TypeDetected': 'Numeric',
    'Dropped': 'No',
    'EngineeredFeatureCount': 2,
    'Tranformations': ['MeanImputer', 'ImputationMarker']},
   {'RawFeatureName': 'B',
    'TypeDetected': 'Numeric',
    'Dropped': 'No',
    'EngineeredFeatureCount': 2,
    'Tranformations': ['MeanImputer', 'ImputationMarker']},
   {'RawFeatureName': 'C',
    'TypeDetected': 'Numeric',
    'Dropped': 'Yes',
    'EngineeredFeatureCount': 0,
    'Tranformations': []},
   {'RawFeatureName': 'D',
    'TypeDetected': 'DateTime',
    'Dropped': 'No',
    'EngineeredFeatureCount': 11,
    'Tranformations': ['DateTime','DateTime','DateTime','DateTime','DateTime','DateTime','DateTime','DateTime','DateTime','DateTime','DateTime']}]
  ```
  
   Waar:
   
   |Uitvoer|Definitie|
   |----|--------|
   |RawFeatureName|De naam van de functie/invoerkolom uit de gegevensset die is opgegeven.| 
   |TypeDetected|Gedetecteerde-gegevenstype van de functie voor invoer.|
   |Verwijderd|Hiermee wordt aangegeven als de functie voor invoer is verwijderd of gebruikt.|
   |EngineeringFeatureCount|Het aantal functies die worden gegenereerd door de geautomatiseerde functie engineering transformaties.|
   |Transformaties|Lijst van transformaties toegepast voor het invoeren van functies voor het genereren van Social engineering functies.|  

### <a name="scalingnormalization-and-algorithm-with-hypermeter-values"></a>Schaal/normalisering en algoritme met hypermeter waarden:

Voor meer informatie over de schaal/normalisering en algoritme/hyperparameter waarden voor een pijplijn, gebruikt u fitted_model.steps. [Meer informatie over schaling/normalisering](concept-automated-ml.md#preprocess). Hier volgt een voorbeeld van uitvoer:

```
[('RobustScaler', RobustScaler(copy=True, quantile_range=[10, 90], with_centering=True, with_scaling=True)), ('LogisticRegression', LogisticRegression(C=0.18420699693267145, class_weight='balanced', dual=False, fit_intercept=True, intercept_scaling=1, max_iter=100, multi_class='multinomial', n_jobs=1, penalty='l2', random_state=None, solver='newton-cg', tol=0.0001, verbose=0, warm_start=False))
```

Voor meer informatie, gebruikt u deze Help-functie die wordt weergegeven in [dit voorbeeldnotitieblok](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification/auto-ml-classification.ipynb).

```python
from pprint import pprint
def print_model(model, prefix=""):
    for step in model.steps:
        print(prefix + step[0])
        if hasattr(step[1], 'estimators') and hasattr(step[1], 'weights'):
            pprint({'estimators': list(e[0] for e in step[1].estimators), 'weights': step[1].weights})
            print()
            for estimator in step[1].estimators:
                print_model(estimator[1], estimator[0]+ ' - ')
        else:
            pprint(step[1].get_params())
            print()
                
print_model(fitted_model)
```

Hier volgt een voorbeeld van uitvoer:

+ De pijplijn met een specifieke algoritme (LogisticRegression met RobustScalar in dit geval):

  ```
  RobustScaler
  {'copy': True,
   'quantile_range': [10, 90],
   'with_centering': True,
   'with_scaling': True}
  
  LogisticRegression
  {'C': 0.18420699693267145,
   'class_weight': 'balanced',
   'dual': False,
   'fit_intercept': True,
   'intercept_scaling': 1,
   'max_iter': 100,
   'multi_class': 'multinomial',
   'n_jobs': 1,
   'penalty': 'l2',
   'random_state': None,
   'solver': 'newton-cg',
   'tol': 0.0001,
   'verbose': 0,
   'warm_start': False}
  ```
  
+ De pijplijn met behulp van ensembles benadering: In dit geval is het een ensembles van 2 verschillende pijplijnen

  ```
  prefittedsoftvotingclassifier
  {'estimators': ['1', '18'],
  'weights': [0.6666666666666667,
              0.3333333333333333]}

  1 - RobustScaler
  {'copy': True,
   'quantile_range': [25, 75],
   'with_centering': True,
   'with_scaling': False}
  
  1 - LightGBMClassifier
  {'boosting_type': 'gbdt',
   'class_weight': None,
   'colsample_bytree': 0.2977777777777778,
   'importance_type': 'split',
   'learning_rate': 0.1,
   'max_bin': 30,
   'max_depth': 5,
   'min_child_samples': 6,
   'min_child_weight': 5,
   'min_split_gain': 0.05263157894736842,
   'n_estimators': 200,
   'n_jobs': 1,
   'num_leaves': 176,
   'objective': None,
   'random_state': None,
   'reg_alpha': 0.2631578947368421,
   'reg_lambda': 0,
   'silent': True,
   'subsample': 0.8415789473684211,
   'subsample_for_bin': 200000,
   'subsample_freq': 0,
   'verbose': -10}
  
  18 - StandardScalerWrapper
  {'class_name': 'StandardScaler',
   'copy': True,
   'module_name': 'sklearn.preprocessing.data',
   'with_mean': True,
   'with_std': True}
  
  18 - LightGBMClassifier
  {'boosting_type': 'goss',
   'class_weight': None,
   'colsample_bytree': 0.2977777777777778,
   'importance_type': 'split',
   'learning_rate': 0.07894947368421053,
   'max_bin': 30,
   'max_depth': 6,
   'min_child_samples': 47,
   'min_child_weight': 0,
   'min_split_gain': 0.2631578947368421,
   'n_estimators': 400,
   'n_jobs': 1,
   'num_leaves': 14,
   'objective': None,
   'random_state': None,
   'reg_alpha': 0.5789473684210527,
   'reg_lambda': 0.7894736842105263,
   'silent': True,
   'subsample': 1,
   'subsample_for_bin': 200000,
   'subsample_freq': 0,
   'verbose': -10}
  ```
  
<a name="explain"></a>

## <a name="explain-the-model-interpretability"></a>Leg uit het model (interpretability)

Geautomatiseerde machine learning kunt u inzicht krijgt in de functie belang.  Tijdens de training krijgt u de algemene functie van belang voor het model.  U kunt ook op klasseniveau functie belang ophalen voor de classificatie-scenario's.  U moet een validatie-gegevensset (X_valid) om op te halen van de functie urgentie opgeven.

Er zijn twee manieren voor het genereren van functie belang.

*   Als een experiment voltooid is, kunt u `explain_model` methode bij elke iteratie.

    ```python
    from azureml.train.automl.automlexplainer import explain_model

    shap_values, expected_values, overall_summary, overall_imp, per_class_summary, per_class_imp = \
        explain_model(fitted_model, X_train, X_test)

    #Overall feature importance
    print(overall_imp)
    print(overall_summary)

    #Class-level feature importance
    print(per_class_imp)
    print(per_class_summary)
    ```

*   Als u wilt bekijken functie belang voor alle iteraties, `model_explainability` markering `True` in AutoMLConfig.

    ```python
    automl_config = AutoMLConfig(task = 'classification',
                                 debug_log = 'automl_errors.log',
                                 primary_metric = 'AUC_weighted',
                                 max_time_sec = 12000,
                                 iterations = 10,
                                 verbosity = logging.INFO,
                                 X = X_train,
                                 y = y_train,
                                 X_valid = X_test,
                                 y_valid = y_test,
                                 model_explainability=True,
                                 path=project_folder)
    ```

    Zodra u klaar bent, kunt u retrieve_model_explanation methode om op te halen van belang van de functie voor een specifieke versie.

    ```python
    from azureml.train.automl.automlexplainer import retrieve_model_explanation

    shap_values, expected_values, overall_summary, overall_imp, per_class_summary, per_class_imp = \
        retrieve_model_explanation(best_run)

    #Overall feature importance
    print(overall_imp)
    print(overall_summary)

    #Class-level feature importance
    print(per_class_imp)
    print(per_class_summary)
    ```

De grafiek voor het belang van functie in uw werkruimte in de Azure-portal, kunt u visualiseren. De grafiek wordt ook weergegeven wanneer u de Jupyter-widget in een notitieblok. Voor meer informatie over de grafieken verwijzen naar de [voorbeeld van Azure Machine Learning-service-laptops artikel.](samples-notebooks.md)

```Python
from azureml.widgets import RunDetails
RunDetails(local_run).show()
```
![functie belang graph](./media/how-to-configure-auto-train/feature-importance.png)

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [hoe en waar u kunt een model implementeren](how-to-deploy-and-where.md).

Meer informatie over [hoe u een regressiemodel met automatische machine learning te trainen](tutorial-auto-train-models.md) of [hoe u met behulp van de trein geautomatiseerde machine learning in een externe bron](how-to-auto-train-remote.md).
