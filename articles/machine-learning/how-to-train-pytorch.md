---
title: PyTorch-modellen voor diepe Learning trainen
titleSuffix: Azure Machine Learning
description: Meer informatie over het uitvoeren van uw PyTorch-trainings scripts op ENTER prise Scale met behulp van Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.author: minxia
author: mx-iao
ms.reviewer: peterlu
ms.date: 09/28/2020
ms.topic: conceptual
ms.custom: how-to
ms.openlocfilehash: 22e834ccc31e2d01646250c973080848173661de
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "91743774"
---
# <a name="train-pytorch-models-at-scale-with-azure-machine-learning"></a>PyTorch-modellen op schaal trainen met Azure Machine Learning

In dit artikel leert u hoe u uw [PyTorch](https://pytorch.org/) -trainings scripts kunt uitvoeren op ENTER prise Scale met behulp van Azure machine learning.

De voorbeeld scripts in dit artikel worden gebruikt voor het classificeren van kippen en Turkije-installatie kopieën voor het bouwen van een diep gaande Neural-netwerk (DNN) op basis van de [zelf studie](https://pytorch.org/tutorials/beginner/transfer_learning_tutorial.html)over de overdrachts training van PyTorch. 

Of u nu een diep Learning PyTorch-model traint of een bestaand model in de Cloud brengt, u kunt Azure Machine Learning gebruiken om open-source trainings taken te schalen met behulp van elastische Cloud Compute-resources. U kunt modellen voor productie kwaliteit bouwen, implementeren, versie en bewaken met Azure Machine Learning. 

## <a name="prerequisites"></a>Vereisten

Voer deze code uit in een van de volgende omgevingen:

- Azure Machine Learning Compute-instantie-geen down loads of installatie vereist

    - Voltooi de [zelf studie: installatie omgeving en werk ruimte](tutorial-1st-experiment-sdk-setup.md) om een toegewezen notebook server te maken vooraf geladen met de SDK en de voor beeld-opslag plaats.
    - Zoek in de map met uitgebreide trainingen op de notebook server een volledig en uitgebreid notitie blok door naar deze map te navigeren: **instructies-to-use-azureml > ml-frameworks > pytorch > Train-afstemming-Tune-Deploy-with-pytorch** . 
 
 - Uw eigen Jupyter Notebook-server
    - [Installeer de Azure machine learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py&preserve-view=true) (>= 1.15.0).
    - [Maak een configuratie bestand voor de werk ruimte](how-to-configure-environment.md#workspace).
    - [De voorbeeld script bestanden downloaden](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/ml-frameworks/pytorch/train-hyperparameter-tune-deploy-with-pytorch)`pytorch_train.py`
     
    U kunt ook een voltooide [Jupyter notebook versie](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/ml-frameworks/pytorch/train-hyperparameter-tune-deploy-with-pytorch/train-hyperparameter-tune-deploy-with-pytorch.ipynb) van deze hand leiding vinden op de pagina met voor beelden van github. Het notitie blok bevat uitgebreide secties die betrekking hebben op intelligent afstemming tuning, model implementatie en notebook widgets.

## <a name="set-up-the-experiment"></a>Het experiment instellen

In deze sectie wordt het trainings experiment opgesteld door het laden van de vereiste Python-pakketten, het initialiseren van een werk ruimte, het maken van het reken doel en het definiëren van de trainings omgeving.

### <a name="import-packages"></a>Pakketten importeren

Importeer eerst de benodigde python-bibliotheken.

```Python
import os
import shutil

from azureml.core.workspace import Workspace
from azureml.core import Experiment

from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException
```

### <a name="initialize-a-workspace"></a>Een werk ruimte initialiseren

De [Azure machine learning werk ruimte](concept-workspace.md) is de resource op het hoogste niveau voor de service. Het biedt u een centrale locatie voor het werken met alle artefacten die u maakt. In de python-SDK hebt u toegang tot de werkruimte artefacten door een [`workspace`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py&preserve-view=true) object te maken.

Maak een werkruimte object op basis van het `config.json` bestand dat in de [sectie vereisten](#prerequisites)is gemaakt.

```Python
ws = Workspace.from_config()
```

### <a name="get-the-data"></a>De gegevens ophalen

De gegevensset bestaat uit ongeveer 120 trainings afbeeldingen voor kalk oenen en kuikens, met 100-validatie kopieën voor elke klasse. We zullen de gegevensset downloaden en uitpakken als onderdeel van het trainings script `pytorch_train.py` . De installatie kopieën zijn een subset van de [Open Image V5-gegevensset](https://storage.googleapis.com/openimages/web/index.html).

### <a name="prepare-training-script"></a>Trainings script voorbereiden

In deze zelf studie wordt het trainings script, `pytorch_train.py` ,, al meegeleverd. In de praktijk kunt u een aangepast trainings script uitvoeren, zoals dat is, en dit met Azure Machine Learning.

Maak een map voor uw trainings script (s).

```python
project_folder = './pytorch-birds'
os.makedirs(project_folder, exist_ok=True)
shutil.copy('pytorch_train.py', project_folder)
```

### <a name="create-a-compute-target"></a>Een rekendoel maken

Maak een compute-doel voor uw PyTorch-taak om uit te voeren. In dit voor beeld maakt u een Azure Machine Learning Compute-cluster waarvoor GPU is ingeschakeld.

```Python
cluster_name = "gpu-cluster"

try:
    compute_target = ComputeTarget(workspace=ws, name=cluster_name)
    print('Found existing compute target')
except ComputeTargetException:
    print('Creating a new compute target...')
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_NC6', 
                                                           max_nodes=4)

    compute_target = ComputeTarget.create(ws, cluster_name, compute_config)

    compute_target.wait_for_completion(show_output=True, min_node_count=None, timeout_in_minutes=20)
```

[!INCLUDE [low-pri-note](../../includes/machine-learning-low-pri-vm.md)]

Zie voor meer informatie over Compute-doelen het artikel [Wat is een reken doel](concept-compute-target.md) .

### <a name="define-your-environment"></a>Uw omgeving definiëren

Als u de Azure ML- [omgeving](concept-environments.md) wilt definiëren waarin de afhankelijkheden van uw trainings script zijn opgenomen, kunt u een aangepaste omgeving definiëren of een Azure ml-gewerkte omgeving gebruiken.

#### <a name="use-a-curated-environment"></a>Een gecuratore omgeving gebruiken
Azure ML biedt vooraf ontwikkelde, geconstrueerde omgevingen als u uw eigen omgeving niet wilt definiëren. Azure ML heeft verschillende CPU-en GPU-gecuratore omgevingen voor PyTorch die overeenkomen met de verschillende versies van PyTorch. Zie [hier](resource-curated-environments.md)voor meer informatie.

Als u een gewerkte omgeving wilt gebruiken, kunt u in plaats daarvan de volgende opdracht uitvoeren:

```python
curated_env_name = 'AzureML-PyTorch-1.6-GPU'
pytorch_env = Environment.get(workspace=ws, name=curated_env_name)
```

Als u de pakketten wilt zien die zijn opgenomen in de gewerkte omgeving, kunt u de Conda-afhankelijkheden naar de schijf schrijven:
```python
pytorch_env.save_to_directory(path=curated_env_name)
```

Zorg ervoor dat de gecuratorde omgeving alle afhankelijkheden bevat die vereist zijn voor uw trainings script. Als dat niet het geval is, moet u de omgeving aanpassen zodat de ontbrekende afhankelijkheden worden toegevoegd. Als de omgeving is gewijzigd, moet u deze een nieuwe naam geven, omdat het voor voegsel ' AzureML ' is gereserveerd voor de gevoerde omgevingen. Als u het YAML-bestand met Conda-afhankelijkheden hebt gewijzigd, kunt u een nieuwe omgeving maken op basis van een nieuwe naam, bijvoorbeeld:
```python
pytorch_env = Environment.from_conda_specification(name='pytorch-1.6-gpu', file_path='./conda_dependencies.yml')
```

Als u in plaats daarvan het object met de gemodificeerde omgeving rechtstreeks hebt gewijzigd, kunt u die omgeving klonen met een nieuwe naam:
```python
pytorch_env = pytorch_env.clone(new_name='pytorch-1.6-gpu')
```

#### <a name="create-a-custom-environment"></a>Een aangepaste omgeving maken

U kunt ook uw eigen Azure ML-omgeving maken waarin de afhankelijkheden van uw trainings script worden ingekapseld.

Definieer eerst uw Conda-afhankelijkheden in een YAML-bestand. in dit voor beeld heeft het bestand de naam `conda_dependencies.yml` .

```yaml
channels:
- conda-forge
dependencies:
- python=3.6.2
- pip:
  - azureml-defaults
  - torch==1.6.0
  - torchvision==0.7.0
  - future==0.17.1
  - pillow
```

Een Azure ML-omgeving maken op basis van deze Conda Environment-specificatie. De omgeving wordt tijdens runtime ingepakt in een docker-container.

Als er geen basis installatie kopie is opgegeven, wordt door Azure ML standaard een CPU-installatie kopie gebruikt `azureml.core.environment.DEFAULT_CPU_IMAGE` als basis installatie kopie. Omdat in dit voor beeld training wordt uitgevoerd op een GPU-cluster, moet u een GPU-basis installatie kopie opgeven met de benodigde GPU-Stuur Programma's en-afhankelijkheden. Azure ML onderhoudt een set basis installatie kopieën die zijn gepubliceerd op micro soft Container Registry (MCR) die u kunt gebruiken. Zie de [Azure/AzureML-containers](https://github.com/Azure/AzureML-Containers) github opslag plaats voor meer informatie.

```python
from azureml.core import Environment

pytorch_env = Environment.from_conda_specification(name='pytorch-1.6-gpu', file_path='./conda_dependencies.yml')

# Specify a GPU base image
pytorch_env.docker.enabled = True
pytorch_env.docker.base_image = 'mcr.microsoft.com/azureml/openmpi3.1.2-cuda10.1-cudnn7-ubuntu18.04'
```

> [!TIP]
> U kunt desgewenst al uw afhankelijkheden rechtstreeks vastleggen in een aangepaste docker-installatie kopie of Dockerfile, en uw omgeving maken op basis hiervan. Zie voor meer informatie [trainen met aangepaste installatie kopie](how-to-train-with-custom-image.md).

Zie [software omgevingen maken en gebruiken in azure machine learning](how-to-use-environments.md)voor meer informatie over het maken van en het gebruik van omgevingen.

## <a name="configure-and-submit-your-training-run"></a>Uw trainings uitvoering configureren en verzenden

### <a name="create-a-scriptrunconfig"></a>Een ScriptRunConfig maken

Maak een [ScriptRunConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.scriptrunconfig?view=azure-ml-py&preserve-view=true)-object om de configuratiegegevens van uw trainingstaak op te geven, inclusief het trainingsscript, de omgeving die u wilt gebruiken en het rekendoel om uit te voeren. Eventuele argumenten voor uw trainings script worden door gegeven via de opdracht regel indien opgegeven in de `arguments` para meter. 

```python
from azureml.core import ScriptRunConfig

src = ScriptRunConfig(source_directory=project_folder,
                      script='pytorch_train.py',
                      arguments=['--num_epochs', 30, '--output_dir', './outputs'],
                      compute_target=compute_target,
                      environment=pytorch_env)
```

> [!WARNING]
> Azure Machine Learning trainings scripts worden uitgevoerd door de hele bronmap te kopiëren. Als u gevoelige gegevens hebt die u niet wilt uploaden, gebruikt u een [. ignore-bestand](how-to-save-write-experiment-files.md#storage-limits-of-experiment-snapshots) of neemt u het niet op in de bron directory. In plaats daarvan kunt u toegang krijgen tot uw gegevens met behulp van een Azure ML- [gegevensset](how-to-train-with-datasets.md).

Zie [trainings uitvoeringen configureren en verzenden](how-to-set-up-training-targets.md)voor meer informatie over het configureren van taken met ScriptRunConfig.

> [!WARNING]
> Als u eerder de PyTorch Estimator hebt gebruikt om uw PyTorch-trainings taken te configureren, moet u er rekening mee houden dat de schattingen worden afgeschaft in een toekomstige versie van de Azure ML SDK. Met Azure ML SDK >= 1.15.0 is ScriptRunConfig de aanbevolen manier om trainings taken te configureren, met inbegrip van die van DL-frameworks.

## <a name="submit-your-run"></a>Uw uitvoering verzenden

Het [object run](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py&preserve-view=true) biedt de interface voor de uitvoerings geschiedenis terwijl de taak wordt uitgevoerd en nadat deze is voltooid.

```Python
run = Experiment(ws, name='pytorch-birds').submit(src)
run.wait_for_completion(show_output=True)
```

### <a name="what-happens-during-run-execution"></a>Wat er gebeurt tijdens de uitvoering van het programma
Wanneer de uitvoering wordt uitgevoerd, worden de volgende fasen door lopen:

- **Voorbereiden**: een docker-installatie kopie wordt gemaakt volgens de gedefinieerde omgeving. De afbeelding wordt geüpload naar het container register van de werk ruimte en opgeslagen in de cache voor latere uitvoeringen. Logboeken worden ook gestreamd naar de uitvoerings geschiedenis en kunnen worden weer gegeven om de voortgang te bewaken. Als er in plaats daarvan een gecuratorde omgeving wordt opgegeven, wordt er een back-up van de in de cache opgeslagen installatie kopie gebruikt.

- **Schalen**: het cluster probeert omhoog te schalen als het batch AI-cluster meer knoop punten nodig heeft om de uitvoering uit te voeren dan momenteel beschikbaar zijn.

- **Uitvoeren**: alle scripts in de map script worden geüpload naar het Compute-doel, gegevens archieven worden gekoppeld of gekopieerd en de `script` wordt uitgevoerd. Uitvoer van stdout en de map **./logs** worden gestreamd naar de uitvoerings geschiedenis en kunnen worden gebruikt om de uitvoering te bewaken.

- **Na de verwerking**: de map **./outputs** van de uitvoering wordt gekopieerd naar de uitvoerings geschiedenis.

## <a name="register-or-download-a-model"></a>Een model registreren of downloaden

Zodra u het model hebt getraind, kunt u het registreren in uw werk ruimte. Met model registratie kunt u uw modellen in uw werk ruimte opslaan en versieren om het [model beheer en de implementatie](concept-model-management-and-deployment.md)te vereenvoudigen.

```Python
model = run.register_model(model_name='pytorch-birds', model_path='outputs/model.pt')
```

> [!TIP]
> De implementatie-instructie bevat een sectie over het registreren van modellen, maar u kunt direct door gaan naar het [maken van een reken doel](how-to-deploy-and-where.md#choose-a-compute-target) voor implementatie, omdat u al een geregistreerd model hebt.

U kunt ook een lokale kopie van het model downloaden met behulp van het object run. In het trainings script `pytorch_train.py` slaat een PyTorch-object opslaan het model op in een lokale map (lokaal naar het Compute-doel). U kunt het object run gebruiken om een kopie te downloaden.

```Python
# Create a model folder in the current directory
os.makedirs('./model', exist_ok=True)

# Download the model from run history
run.download_file(name='outputs/model.pt', output_file_path='./model/model.pt'), 
```

## <a name="distributed-training"></a>Gedistribueerde training

Azure Machine Learning biedt ook ondersteuning voor gedistribueerde PyTorch-taken met meerdere knoop punten zodat u de werk belasting van uw training kunt schalen. U kunt eenvoudig gedistribueerde PyTorch-taken uitvoeren en Azure ML beheert de indeling voor u.

Azure ML biedt ondersteuning voor het uitvoeren van gedistribueerde PyTorch-taken met de ingebouwde DistributedDataParallel module Horovod en PyTorch.

### <a name="horovod"></a>Horovod
[Horovod](https://github.com/uber/horovod) is een open source-, alle verlaagde structuur voor gedistribueerde training ontwikkeld door uber. Het biedt een eenvoudig pad voor het schrijven van gedistribueerde PyTorch code voor training.

Uw trainings code moet worden beinstrumented met Horovod voor gedistribueerde trainingen. Zie de [Horovod-documentatie](https://horovod.readthedocs.io/en/stable/pytorch.html)voor meer informatie over het gebruik van Horovod met PyTorch.

Bovendien moet u ervoor zorgen dat uw trainings omgeving het **horovod** -pakket bevat. Als u een PyTorch-gecuratorde omgeving gebruikt, is horovod al opgenomen als een van de afhankelijkheden. Als u uw eigen omgeving gebruikt, zorg er dan voor dat de horovod-afhankelijkheid is opgenomen, bijvoorbeeld:

```yaml
channels:
- conda-forge
dependencies:
- python=3.6.2
- pip:
  - azureml-defaults
  - torch==1.6.0
  - torchvision==0.7.0
  - horovod==0.19.5
```

Als u een gedistribueerde taak wilt uitvoeren met behulp van MPI/Horovod op Azure ML, moet u een [MpiConfiguration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfig.mpiconfiguration?view=azure-ml-py&preserve-view=true) opgeven voor de `distributed_job_config` para meter van de ScriptRunConfig-constructor. De onderstaande code configureert een gedistribueerde taak van 2 knoop punten die per knoop punt wordt uitgevoerd. Als u ook meerdere processen per knoop punt wilt uitvoeren (dat wil zeggen, als uw cluster-SKU meerdere Gpu's heeft), geeft u ook de `process_count_per_node` para meter op in MpiConfiguration (de standaard instelling is `1` ).

```python
from azureml.core import ScriptRunConfig
from azureml.core.runconfig import MpiConfiguration

src = ScriptRunConfig(source_directory=project_folder,
                      script='pytorch_horovod_mnist.py',
                      compute_target=compute_target,
                      environment=pytorch_env,
                      distributed_job_config=MpiConfiguration(node_count=2))
```

Zie [gedistribueerde PyTorch met Horovod](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/ml-frameworks/pytorch/distributed-pytorch-with-horovod)voor een volledige zelf studie over het uitvoeren van gedistribueerde PyTorch met Horovod in azure ml.

### <a name="distributeddataparallel"></a>DistributedDataParallel
Als u de ingebouwde [DistributedDataParallel](https://pytorch.org/tutorials/intermediate/ddp_tutorial.html) -module van PyTorch gebruikt die is gebouwd met behulp van het **Torch. distributed** -pakket in uw trainings code, kunt u de gedistribueerde taak ook starten via Azure ml.

Als u een gedistribueerde PyTorch-taak wilt uitvoeren met DistributedDataParallel, geeft u een [PyTorchConfiguration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfig.pytorchconfiguration?view=azure-ml-py&preserve-view=true) op voor de `distributed_job_config` para meter van de ScriptRunConfig-constructor. Als u de NCCL-back-end voor Torch. distributed wilt gebruiken, geeft u `communication_backend='Nccl'` in de PyTorchConfiguration op. Met de onderstaande code wordt een gedistribueerde taak van 2 knoop punten geconfigureerd. De NCCL-back-end is de aanbevolen back-end voor PyTorch Distributed GPU training.

Voor gedistribueerde PyTorch-taken die zijn geconfigureerd via PyTorchConfiguration, worden in azure ML de volgende omgevings variabelen ingesteld voor de knoop punten van het Compute-doel:

* `AZ_BATCHAI_PYTORCH_INIT_METHOD`: URL voor de initialisatie van het gedeelde bestands systeem van de proces groep
* `AZ_BATCHAI_TASK_INDEX`: globale positie van het werk proces

U kunt deze omgevings variabelen opgeven voor de bijbehorende argumenten van het trainings script via de `arguments` para meter ScriptRunConfig.

```python
from azureml.core import ScriptRunConfig
from azureml.core.runconfig import PyTorchConfiguration

args = ['--dist-backend', 'nccl',
        '--dist-url', '$AZ_BATCHAI_PYTORCH_INIT_METHOD',
        '--rank', '$AZ_BATCHAI_TASK_INDEX',
        '--world-size', 2]

src = ScriptRunConfig(source_directory=project_folder,
                      script='pytorch_mnist.py',
                      arguments=args,
                      compute_target=compute_target,
                      environment=pytorch_env,
                      distributed_job_config=PyTorchConfiguration(communication_backend='Nccl', node_count=2))
```

Als u in plaats daarvan de Gloo-back-end voor gedistribueerde trainingen wilt gebruiken, geeft u `communication_backend='Gloo'` in plaats daarvan op. De Gloo-back-end wordt aanbevolen voor gedistribueerde CPU-training.

Zie [gedistribueerde PyTorch met DistributedDataParallel](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/ml-frameworks/pytorch/distributed-pytorch-with-nccl-gloo)voor een volledige zelf studie over het uitvoeren van gedistribueerde PyTorch in azure ml.

## <a name="export-to-onnx"></a>Exporteren naar ONNX

Converteer uw getrainde PyTorch-model naar de ONNX-indeling om de deinterferentie te optimaliseren met de [ONNX-runtime](concept-onnx.md). Defactorion of model Score is de fase waarin het geïmplementeerde model wordt gebruikt voor de voor spelling, meestal op productie gegevens. Raadpleeg de [zelf studie](https://github.com/onnx/tutorials/blob/master/tutorials/PytorchOnnxExport.ipynb) voor een voor beeld.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u een diep gaande Neural-netwerk getraind en geregistreerd met behulp van PyTorch in Azure Machine Learning. Ga verder met ons model implementatie artikel voor meer informatie over het implementeren van een model.

* [Hoe en waar modellen moeten worden geïmplementeerd](how-to-deploy-and-where.md)
* [Metrische uitvoerings gegevens tijdens de training volgen](how-to-track-experiments.md)
* [Hyperparameters afstemmen](how-to-tune-hyperparameters.md)
* [Een getraind model implementeren](how-to-deploy-and-where.md)
* [Referentie architectuur voor gedistribueerde training van diep gaande lessen in azure](/azure/architecture/reference-architectures/ai/training-deep-learning)
