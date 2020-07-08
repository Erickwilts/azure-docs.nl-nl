---
title: Versie beheer van gegevensset
titleSuffix: Azure Machine Learning
description: Meer informatie over het beste versie nummer van uw gegevens sets en hoe versie beheer werkt met machine learning pijp lijnen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.author: sihhu
author: MayMSFT
ms.reviewer: nibaccam
ms.date: 03/09/2020
ms.custom: tracking-python
ms.openlocfilehash: e0b2d7abb378a6717eb4444882ede54debdb5968
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2020
ms.locfileid: "84555632"
---
# <a name="version-and-track-datasets-in-experiments"></a>Versie gegevens sets in experimenten bijhouden
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

In dit artikel leert u hoe u Azure Machine Learning gegevens sets voor reproduceer baarheid kunt versie en bijhouden. Het versie beheer van de gegevensset is een manier om de status van uw gegevens te blad maken, zodat u een specifieke versie van de gegevensset kunt Toep assen voor toekomstige experimenten.

Typische scenario's voor versies:

* Wanneer nieuwe gegevens beschikbaar zijn voor retraining
* Wanneer u verschillende benaderingen voor gegevens voorbereiding of functie techniek toepast

## <a name="prerequisites"></a>Vereisten

Voor deze zelfstudie hebt u het volgende nodig:

- [Azure machine learning SDK voor python geïnstalleerd](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py). Deze SDK bevat het pakket met de [azureml-gegevens sets](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset?view=azure-ml-py) .
    
- Een [Azure machine learning-werk ruimte](concept-workspace.md). U kunt een bestaand item ophalen door de volgende code uit te voeren of [een nieuwe werk ruimte te maken](how-to-manage-workspace.md).

    ```Python
    import azureml.core
    from azureml.core import Workspace
    
    ws = Workspace.from_config()
    ```
- Een [Azure machine learning-gegevensset](how-to-create-register-datasets.md).

<a name="register"></a>

## <a name="register-and-retrieve-dataset-versions"></a>Versie van gegevensset registreren en ophalen

Door een gegevensset te registreren, kunt u deze versie, hergebruiken en delen in experimenten en met collega's. U kunt meerdere gegevens sets onder dezelfde naam registreren en een specifieke versie ophalen op naam en versie nummer.

### <a name="register-a-dataset-version"></a>Een gegevensset-versie registreren

Met de volgende code wordt een nieuwe versie van de `titanic_ds` gegevensset geregistreerd door de `create_new_version` para meter in te stellen op `True` . Als er geen bestaande `titanic_ds` gegevensset is geregistreerd bij de werk ruimte, maakt de code een nieuwe gegevensset met de naam `titanic_ds` en wordt de versie ingesteld op 1.

```Python
titanic_ds = titanic_ds.register(workspace = workspace,
                                 name = 'titanic_ds',
                                 description = 'titanic training data',
                                 create_new_version = True)
```

### <a name="retrieve-a-dataset-by-name"></a>Een gegevensset op naam ophalen

De methode [get_by_name ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#get-by-name-workspace--name--version--latest--) voor de `Dataset` klasse retourneert standaard de meest recente versie van de gegevensset die is geregistreerd bij de werk ruimte. 

Met de volgende code wordt versie 1 van de `titanic_ds` gegevensset opgehaald.

```Python
from azureml.core import Dataset
# Get a dataset by name and version number
titanic_ds = Dataset.get_by_name(workspace = workspace,
                                 name = 'titanic_ds', 
                                 version = 1)
```

<a name="best-practice"></a>

## <a name="versioning-best-practice"></a>Versie beheer best practice

Wanneer u een gegevensset-versie maakt, maakt u *geen* extra kopie van gegevens met de werk ruimte. Omdat gegevens sets verwijzingen naar de gegevens in uw opslag service zijn, hebt u één bron van waarheid, die wordt beheerd door uw opslag service.

>[!IMPORTANT]
> Als de gegevens waarnaar wordt verwezen door uw gegevensset worden overschreven of verwijderd, wordt de wijziging *niet* ongedaan gemaakt door het aanroepen van een specifieke versie van de gegevensset.

Wanneer u gegevens laadt vanuit een gegevensset, wordt de huidige gegevens inhoud waarnaar wordt verwezen door de gegevensset, altijd geladen. Als u er zeker van wilt zijn dat elke gegevensset-versie reproduceerbaar is, raden wij aan dat u geen gegevens inhoud wijzigt waarnaar wordt verwezen door de gegevensset-versie. Als er nieuwe gegevens binnenkomen in, slaat u nieuwe gegevens bestanden op in een afzonderlijke gegevensmap en maakt u vervolgens een nieuwe gegevensset-versie om gegevens uit de nieuwe map op te halen.

De volgende afbeeldings-en voorbeeld code tonen de aanbevolen manier om uw gegevens mappen te structureren en gegevensset-versies te maken die verwijzen naar deze mappen:

![Mapstructuur](./media/how-to-version-track-datasets/folder-image.png)

```Python
from azureml.core import Dataset

# get the default datastore of the workspace
datastore = workspace.get_default_datastore()

# create & register weather_ds version 1 pointing to all files in the folder of week 27
datastore_path1 = [(datastore, 'Weather/week 27')]
dataset1 = Dataset.File.from_files(path=datastore_path1)
dataset1.register(workspace = workspace,
                  name = 'weather_ds',
                  description = 'weather data in week 27',
                  create_new_version = True)

# create & register weather_ds version 2 pointing to all files in the folder of week 27 and 28
datastore_path2 = [(datastore, 'Weather/week 27'), (datastore, 'Weather/week 28')]
dataset2 = Dataset.File.from_files(path = datastore_path2)
dataset2.register(workspace = workspace,
                  name = 'weather_ds',
                  description = 'weather data in week 27, 28',
                  create_new_version = True)

```

<a name="pipeline"></a>

## <a name="version-a-pipeline-output-dataset"></a>Een gegevensset voor een pijplijn uitvoer

U kunt een gegevensset gebruiken als de invoer en uitvoer van elke Machine Learning pijplijn stap. Wanneer u pijp lijnen opnieuw uitvoert, wordt de uitvoer van elke pijplijn stap geregistreerd als een nieuwe gegevensset-versie.

Omdat Machine Learning pijp lijnen de uitvoer van elke Step Into een nieuwe map elke keer dat de pijp lijn opnieuw wordt uitgevoerd, kunnen de versie-uitvoer gegevens sets worden gereproduceerd. Meer informatie over [gegevens sets in pijp lijnen](how-to-create-your-first-pipeline.md#steps).

```Python
from azureml.core import Dataset
from azureml.pipeline.steps import PythonScriptStep
from azureml.pipeline.core import Pipeline, PipelineData
from azureml.core. runconfig import CondaDependencies, RunConfiguration

# get input dataset 
input_ds = Dataset.get_by_name(workspace, 'weather_ds')

# register pipeline output as dataset
output_ds = PipelineData('prepared_weather_ds', datastore=datastore).as_dataset()
output_ds = output_ds.register(name='prepared_weather_ds', create_new_version=True)

conda = CondaDependencies.create(
    pip_packages=['azureml-defaults', 'azureml-dataprep[fuse,pandas]'], 
    pin_sdk_version=False)

run_config = RunConfiguration()
run_config.environment.docker.enabled = True
run_config.environment.python.conda_dependencies = conda

# configure pipeline step to use dataset as the input and output
prep_step = PythonScriptStep(script_name="prepare.py",
                             inputs=[input_ds.as_named_input('weather_ds')],
                             outputs=[output_ds],
                             runconfig=run_config,
                             compute_target=compute_target,
                             source_directory=project_folder)
```

<a name="track"></a>

## <a name="track-datasets-in-experiments"></a>Gegevens sets bijhouden in experimenten

Voor elk Machine Learning experiment kunt u eenvoudig de gegevens sets traceren die worden gebruikt als invoer via het `Run` object experiment.

De volgende code gebruikt de [`get_details()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py#get-details--) methode om bij te houden welke invoer gegevens sets zijn gebruikt bij de uitvoering van het experiment:

```Python
# get input datasets
inputs = run.get_details()['inputDatasets']
input_dataset = inputs[0]['dataset']

# list the files referenced by input_dataset
input_dataset.to_path()
```

U kunt de van experimenten ook vinden met `input_datasets` behulp van https://ml.azure.com/ . 

De volgende afbeelding laat zien waar u de invoer gegevensset van een experiment op Azure Machine Learning Studio kunt vinden. Voor dit voor beeld gaat u naar het deel venster **experimenten** en opent u het tabblad **Eigenschappen** voor een specifieke uitvoering van uw experiment `keras-mnist` .

![Invoer gegevens sets](./media/how-to-version-track-datasets/input-datasets.png)

Gebruik de volgende code om modellen te registreren met gegevens sets:

```Python
model = run.register_model(model_name='keras-mlp-mnist',
                           model_path=model_path,
                           datasets =[('training data',train_dataset)])
```

Na de registratie ziet u de lijst met modellen die zijn geregistreerd bij de gegevensset met behulp van python of gaat u naar https://ml.azure.com/ .

De volgende weer gave is afkomstig uit het deel venster **gegevens sets** onder **assets**. Selecteer de gegevensset en selecteer vervolgens het tabblad **modellen** voor een lijst van de modellen die zijn geregistreerd bij de gegevensset. 

![Modellen van invoer gegevens sets](./media/how-to-version-track-datasets/dataset-models.png)

## <a name="next-steps"></a>Volgende stappen

* [Trainen met gegevenssets](how-to-train-with-datasets.md)
* [Meer voor beelden van gegevensset-notitie blokken](https://aka.ms/dataset-tutorial)
