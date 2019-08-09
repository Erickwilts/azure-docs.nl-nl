---
title: Een diepe Learning Neural-netwerk trainen met Chainer
titleSuffix: Azure Machine Learning service
description: Meer informatie over het uitvoeren van uw PyTorch-trainings scripts op ondernemings schaal met behulp van de Estimator-klasse van Azure Machine Learning Chainer.  In het voorbeeld script classifis worden handgeschreven cijfer afbeeldingen gemaakt om een diep gaande Neural-netwerk te bouwen met behulp van de keten python-bibliotheek die op numpy wordt uitgevoerd.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: maxluk
author: maxluk
ms.reviewer: sdgilley
ms.date: 08/02/2019
ms.openlocfilehash: 91ac8b596373963aeba00c1932dfde5d76f27bd0
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2019
ms.locfileid: "68856029"
---
# <a name="train-and-register-chainer-models-at-scale-with-azure-machine-learning-service"></a>Keten modellen trainen en registreren op schaal met Azure Machine Learning-service

In dit artikel leert u hoe u uw [Chainer](https://chainer.org/) -trainings scripts kunt uitvoeren op ENTER prise Scale met behulp van de [Estimator](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.chainer?view=azure-ml-py) -klasse van Azure machine learning. In het voorbeeld script in dit artikel wordt gebruikgemaakt van de populaire [MNIST-gegevensset](http://yann.lecun.com/exdb/mnist/) om handgeschreven cijfers te classificeren met behulp van een diepe Neural Network (DNN) die is gebouwd met behulp van de python-bibliotheek van de keten die boven op [numpy](https://www.numpy.org/)wordt uitgevoerd.

Of u nu een diep gaande leer keten van het model traint of een bestaand model in de Cloud brengt, u kunt Azure Machine Learning gebruiken om open-source trainings taken uit te breiden met behulp van elastische Cloud Compute-resources. U kunt modellen voor productie kwaliteit bouwen, implementeren, versie en bewaken met Azure Machine Learning. 

Meer informatie over [uitgebreide kennis en machine learning](concept-deep-learning-vs-machine-learning.md).

Als u nog geen Azure-abonnement hebt, maakt u een gratis account voordat u begint. Probeer nog vandaag de [gratis of betaalde versie van de Azure Machine Learning Service](https://aka.ms/AMLFree).

## <a name="prerequisites"></a>Vereisten

Voer deze code uit in een van de volgende omgevingen:

- Azure Machine Learning-notebook-VM-geen down loads of installatie vereist

    - U moet de [Zelfstudie: Omgeving en werk ruimte](tutorial-1st-experiment-sdk-setup.md) instellen voor het maken van een toegewezen notebook server vooraf geladen met de SDK en de voor beeld-opslag plaats.
    - Zoek in de map met de voor beelden diepe Learning op de notebook server naar een voltooide notebook en bestanden in de map **How-to-use-azureml/training-with-diepe-Learning/Train-afstemming-Tune-Deploy-with-Chainer** .  Het notitie blok bevat uitgebreide secties die betrekking hebben op intelligent afstemming tuning, model implementatie en notebook widgets.

- Uw eigen Jupyter Notebook-server

    - [Installeer de Azure machine learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py).
    - [Maak een configuratie bestand voor de werk ruimte](how-to-configure-environment.md#workspace).
    - Down load het voorbeeld script bestand [chainer_mnist. py](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-chainer/chainer_mnist.py).
     - U kunt ook een voltooide [Jupyter notebook versie](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-chainer/train-hyperparameter-tune-deploy-with-chainer.ipynb) van deze hand leiding vinden op de pagina met github-voor beelden. Het notitie blok bevat uitgebreide secties die betrekking hebben op intelligent afstemming tuning, model implementatie en notebook widgets.

## <a name="set-up-the-experiment"></a>Het experiment instellen

In deze sectie wordt het trainings experiment opgesteld door het laden van de vereiste Python-pakketten, het initialiseren van een werk ruimte, het maken van een experiment en het uploaden van de trainings gegevens en trainings scripts.

### <a name="import-packages"></a>Pakketten importeren

Importeer eerst de python-bibliotheek van azureml. Core en geef het versie nummer weer.

```
# Check core SDK version number
import azureml.core

print("SDK version:", azureml.core.VERSION)
```

### <a name="initialize-a-workspace"></a>Een werk ruimte initialiseren

De [werk ruimte](concept-workspace.md) van de Azure machine learning-service is de resource op het hoogste niveau voor de service. Het biedt u een centrale locatie voor het werken met alle artefacten die u maakt. In de python-SDK hebt u toegang tot de werkruimte artefacten door een [`workspace`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py) object te maken.

Maak een werkruimte object door het `config.json` bestand te lezen dat is gemaakt in de [sectie vereisten](#prerequisites):

```Python
ws = Workspace.from_config()
```

### <a name="create-a-project-directory"></a>Een projectmap maken
Maak een map die alle benodigde code bevat van uw lokale computer waartoe u toegang moet hebben op de externe bron. Dit omvat het trainings script en eventuele extra bestanden waarop uw trainings script van toepassing is.

```
import os

project_folder = './chainer-mnist'
os.makedirs(project_folder, exist_ok=True)
```

### <a name="prepare-training-script"></a>Trainings script voorbereiden

In deze zelf studie is het trainings script **chainer_mnist. py** al voor u ingevuld. In de praktijk moet u een aangepast trainings script kunnen uitvoeren zoals dat is en dit kan worden uitgevoerd met Azure ML zonder dat u de code hoeft te wijzigen.

Als u de mogelijkheden voor bijhouden en metrische gegevens van Azure ML wilt gebruiken, voegt u een kleine hoeveelheid Azure ML-code toe in uw trainings script.  In het trainings script **chainer_mnist. py** ziet u hoe u bepaalde metrische gegevens registreert bij uw Azure ml `Run` -uitvoering met behulp van het-object in het script.

In het meegeleverde trainings script worden voorbeeld gegevens van de Chainer `datasets.mnist.get_mnist` -functie gebruikt.  Voor uw eigen gegevens moet u mogelijk stappen zoals [gegevensset uploaden en scripts](how-to-train-keras.md#upload-dataset-and-scripts) gebruiken om gegevens beschikbaar te maken tijdens de training.

Kopieer het trainings script **chainer_mnist. py** in de projectmap.

```
import shutil

shutil.copy('chainer_mnist.py', project_folder)
```

### <a name="create-a-deep-learning-experiment"></a>Een diep leer experiment maken

Een experiment maken. In dit voor beeld maakt u een experiment met de naam ' Chainer-mnist '.

```
from azureml.core import Experiment

experiment_name = 'chainer-mnist'
experiment = Experiment(ws, name=experiment_name)
```


## <a name="create-or-get-a-compute-target"></a>Een reken doel maken of ophalen

U hebt een [berekenings doel](concept-compute-target.md) nodig voor het trainen van uw model. In dit voor beeld gebruikt u Azure ML Managed Compute (AmlCompute) voor uw externe trainings Compute-resource.

Het **maken van AmlCompute duurt ongeveer 5 minuten**. Als de AmlCompute met die naam al in uw werk ruimte staat, slaat deze code het aanmaak proces over.  

```Python
from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException

# choose a name for your cluster
cluster_name = "gpu-cluster"

try:
    compute_target = ComputeTarget(workspace=ws, name=cluster_name)
    print('Found existing compute target.')
except ComputeTargetException:
    print('Creating a new compute target...')
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_NC6', 
                                                           max_nodes=4)

    # create the cluster
    compute_target = ComputeTarget.create(ws, cluster_name, compute_config)

    compute_target.wait_for_completion(show_output=True)

# use get_status() to get a detailed status for the current cluster. 
print(compute_target.get_status().serialize())
```

Zie voor meer informatie over Compute-doelen het artikel [Wat is een reken doel](concept-compute-target.md) .

## <a name="create-a-chainer-estimator"></a>Een Estimator maken

De [Chainer Estimator](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.chainer?view=azure-ml-py) biedt een eenvoudige manier om de trainings taken voor Chainer te starten op uw reken doel.

De Chainer Estimator wordt geïmplementeerd via de algemene [`estimator`](https://docs.microsoft.com//python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py) klasse, die kan worden gebruikt ter ondersteuning van een Framework. Voor meer informatie over trainings modellen die gebruikmaken van de algemene Estimator, raadpleegt [u modellen met Azure machine learning met behulp van Estimator](how-to-train-ml-models.md)

```Python
from azureml.train.dnn import Chainer

script_params = {
    '--epochs': 10,
    '--batchsize': 128,
    '--output_dir': './outputs'
}

estimator = Chainer(source_directory=project_folder, 
                    script_params=script_params,
                    compute_target=compute_target,
                    pip_packages=['numpy', 'pytest'],
                    entry_script='chainer_mnist.py',
                    use_gpu=True)
```

## <a name="submit-a-run"></a>Een run verzenden

Het [object run](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py) biedt de interface voor de uitvoerings geschiedenis terwijl de taak wordt uitgevoerd en nadat deze is voltooid.

```Python
run = exp.submit(est)
run.wait_for_completion(show_output=True)
```

Wanneer de uitvoering wordt uitgevoerd, worden de volgende fasen door lopen:

- **Voorbereiden**: Een docker-installatie kopie wordt gemaakt volgens de Chainer Estimator. De afbeelding wordt geüpload naar het container register van de werk ruimte en opgeslagen in de cache voor latere uitvoeringen. Logboeken worden ook gestreamd naar de uitvoerings geschiedenis en kunnen worden weer gegeven om de voortgang te bewaken.

- **Schalen**: Het cluster probeert omhoog te schalen als het Batch AI-cluster meer knoop punten nodig heeft om de uitvoering uit te voeren dan momenteel beschikbaar zijn.

- **Uitvoeren**: Alle scripts in de map script worden geüpload naar het Compute-doel, gegevens archieven worden gekoppeld of gekopieerd en de entry_script wordt uitgevoerd. Uitvoer van stdout en de map./logs worden gestreamd naar de uitvoerings geschiedenis en kunnen worden gebruikt om de uitvoering te bewaken.

- **Nabewerken**: De map./outputs van de uitvoering wordt gekopieerd naar de uitvoerings geschiedenis.

## <a name="save-and-register-the-model"></a>Het model opslaan en registreren

Zodra u het model hebt getraind, kunt u het opslaan en registreren in uw werk ruimte. Met model registratie kunt u uw modellen in uw werk ruimte opslaan en versieren om het [model beheer en de implementatie](concept-model-management-and-deployment.md)te vereenvoudigen.


Nadat de model training is voltooid, registreert u het model in uw werk ruimte met de volgende code.  

```Python
model = run.register_model(model_name='chainer-dnn-mnist', model_path='outputs/model.npz')
```

> [!TIP]
> Als er een fout bericht wordt weer gegeven dat het model niet wordt gevonden, geeft u het een minuut en probeert u het opnieuw.  Soms is er sprake van een lichte vertraging tussen het einde van de uitvoering van de training en de beschik baarheid van het model in de uitvoer Directory.

U kunt ook een lokale kopie van het model downloaden. Dit kan handig zijn om een extra model validatie lokaal te kunnen uitvoeren. In het trainings script `chainer_mnist.py`bewaart een beveiligings-object het model naar een lokale map (lokaal naar het Compute-doel). U kunt het object run gebruiken om een kopie te downloaden uit de gegevens opslag.

```Python
# Create a model folder in the current directory
os.makedirs('./model', exist_ok=True)

for f in run.get_file_names():
    if f.startswith('outputs/model'):
        output_file_path = os.path.join('./model', f.split('/')[-1])
        print('Downloading from {} to {} ...'.format(f, output_file_path))
        run.download_file(name=f, output_file_path=output_file_path)
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u een diep gaande training getraind en geregistreerd, Neural-netwerk met Chainer op Azure Machine Learning service. Ga verder met ons [model implementatie](how-to-deploy-and-where.md) artikel voor meer informatie over het implementeren van een model.

* [Afstemmen van hyperparameters](how-to-tune-hyperparameters.md)

* [Metrische gegevens over uitvoeren tijdens de training bijhouden](how-to-track-experiments.md)
