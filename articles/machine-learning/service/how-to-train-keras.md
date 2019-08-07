---
title: Een diepe Learning Neural-netwerk trainen met Keras
titleSuffix: Azure Machine Learning service
description: Meer informatie over het trainen en registreren van een Keras diepe Neural-netwerk classificatie model dat wordt uitgevoerd op tensor flow met Azure Machine Learning service.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: maxluk
author: maxluk
ms.reviewer: peterlu
ms.date: 08/01/2019
ms.custom: seodec18
ms.openlocfilehash: bfe7f975539c76c1d369d111729820f4d0ada470
ms.sourcegitcommit: 4b5dcdcd80860764e291f18de081a41753946ec9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2019
ms.locfileid: "68775068"
---
# <a name="train-and-register-a-keras-classification-model-with-azure-machine-learning-service"></a>Een Keras-classificatie model trainen en registreren met Azure Machine Learning-service

In dit artikel leest u hoe u een Keras-classificatie model kunt trainen en registreren dat is gebaseerd op tensor flow met behulp van Azure Machine Learning-service. Er wordt gebruikgemaakt van de populaire [MNIST-gegevensset](http://yann.lecun.com/exdb/mnist/) om handgeschreven cijfers te classificeren met behulp van een diepe Neural Network (DNN) die is gebouwd met behulp van de [Keras python-bibliotheek](https://keras.io) op [tensor flow](https://www.tensorflow.org/overview).

Keras is een Neural-netwerk-API op hoog niveau die de ontwikkeling van andere populaire DNN-Frameworks kan vereenvoudigen. Met Azure Machine Learning-service kunt u trainings taken snel uitschalen met behulp van elastische Cloud Compute-resources. U kunt ook uw trainings uitvoeringen, versie modellen, implementatie modellen en nog veel meer volgen.

Of u nu een Keras-model ontwikkelt of een bestaand model in de Cloud brengt, Azure Machine Learning service kan u helpen bij het bouwen van modellen die gereed zijn voor productie.

Zie het [conceptuele artikel](concept-deep-learning-vs-machine-learning.md) voor informatie over de verschillen tussen machine learning en diep gaande lessen.

## <a name="prerequisites"></a>Vereisten

Voer deze code uit in een van de volgende omgevingen:

 - Azure Machine Learning-notebook-VM-geen down loads of installatie vereist

     - U moet de [Zelfstudie: Omgeving en werk ruimte](tutorial-1st-experiment-sdk-setup.md) instellen voor het maken van een toegewezen notebook server vooraf geladen met de SDK en de voor beeld-opslag plaats.
    - Zoek in de map met voor beelden op de notebook server een volledig en uitgebreid notitie blok door naar deze map te navigeren: **instructies-to-use-azureml > training-met-learning > Train-afstemming-Tune-Deploy-with-Keras** -map.

 - Uw eigen Jupyter Notebook-server

     - [De Azure Machine Learning SDK voor python installeren](setup-create-workspace.md#sdk)
    - [Een configuratie bestand voor een werk ruimte maken](setup-create-workspace.md#write-a-configuration-file)
    - [De voorbeeld script bestanden downloaden](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-keras) `mnist-keras.py` en`utils.py`

    U kunt ook een voltooide [Jupyter notebook versie](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-keras/train-hyperparameter-tune-deploy-with-keras.ipynb) van deze hand leiding vinden op de pagina met voor beelden van github. Het notitie blok bevat uitgebreide secties die betrekking hebben op intelligent afstemming tuning, model implementatie en notebook widgets.

## <a name="set-up-the-experiment"></a>Het experiment instellen

In deze sectie wordt het trainings experiment opgesteld door het laden van de vereiste Python-pakketten, het initialiseren van een werk ruimte, het maken van een experiment en het uploaden van de trainings gegevens en trainings scripts.

### <a name="import-packages"></a>Pakketten importeren

Importeer eerst de benodigde python-bibliotheken.

```Python
import os
import urllib
import shutil
import azureml

from azureml.core import Experiment
from azureml.core import Workspace, Run

from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException
```

### <a name="initialize-a-workspace"></a>Een werk ruimte initialiseren

De [werk ruimte](concept-workspace.md) van de Azure machine learning-service is de resource op het hoogste niveau voor de service. Het biedt u een centrale locatie voor het werken met alle artefacten die u maakt. In de python-SDK hebt u toegang tot de werkruimte artefacten door een [`workspace`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py) object te maken.

Maak een werkruimte object op basis `config.json` van het bestand dat in de [sectie vereisten](#prerequisites)is gemaakt.

```Python
ws = Workspace.from_config()
```

### <a name="create-an-experiment"></a>Een experiment maken

Maak een experiment en een map om uw trainings scripts te bewaren. In dit voor beeld maakt u een experiment met de naam ' Keras-mnist '.

```Python
script_folder = './keras-mnist'
os.makedirs(script_folder, exist_ok=True)

exp = Experiment(workspace=ws, name='keras-mnist')
```

### <a name="upload-dataset-and-scripts"></a>Gegevensset en scripts uploaden

Het gegevens [Archief](how-to-access-data.md) is een plek waar de informatie kan worden opgeslagen en geopend door het koppelen of kopiëren van de gegevens naar het reken doel. Elke werk ruimte biedt een standaard gegevens opslag. Upload de gegevens-en trainings scripts naar het gegevens archief zodat ze eenvoudig kunnen worden geopend tijdens de training.

1. Down load de MNIST-gegevensset lokaal.

    ```Python
    os.makedirs('./data/mnist', exist_ok=True)

    urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/train-images-idx3-ubyte.gz', filename = './data/mnist/train-images.gz')
    urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/train-labels-idx1-ubyte.gz', filename = './data/mnist/train-labels.gz')
    urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/t10k-images-idx3-ubyte.gz', filename = './data/mnist/test-images.gz')
    urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/t10k-labels-idx1-ubyte.gz', filename = './data/mnist/test-labels.gz')
    ```

1. Upload de MNIST-gegevensset naar de standaard gegevens opslag.

    ```Python
    ds = ws.get_default_datastore()
    ds.upload(src_dir='./data/mnist', target_path='mnist', overwrite=True, show_progress=True)
    ```

1. Upload het Keras-trainings script `keras_mnist.py`, en het helperbestand, `utils.py`.

    ```Python
    shutil.copy('./keras_mnist.py', script_folder)
    shutil.copy('./utils.py', script_folder)
    ```

## <a name="create-a-compute-target"></a>Een compute-doel maken

Maak een compute-doel voor uw tensor flow-taak om uit te voeren. In dit voor beeld maakt u een Azure Machine Learning Compute-cluster waarvoor GPU is ingeschakeld.

```Python
cluster_name = "gpucluster"

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

Zie voor meer informatie over Compute-doelen het artikel [Wat is een reken doel](concept-compute-target.md) .

## <a name="create-a-tensorflow-estimator-and-import-keras"></a>Een tensor flow-Estimator maken en Keras importeren

De [tensor flow Estimator](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py) biedt een eenvoudige manier om tensor flow-trainings taken op reken doel te starten. Aangezien Keras boven op tensor flow wordt uitgevoerd, kunt u de tensor flow Estimator gebruiken en de Keras-bibliotheek importeren met `pip_packages` behulp van het argument.

De tensor flow Estimator wordt geïmplementeerd via de algemene [`estimator`](https://docs.microsoft.com//python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py) klasse, die kan worden gebruikt ter ondersteuning van een Framework. Maak bovendien een woorden lijst `script_params` die de DNN afstemming-instellingen bevat. Voor meer informatie over trainings modellen die gebruikmaken van de algemene Estimator, raadpleegt [u modellen met Azure machine learning met behulp van Estimator](how-to-train-ml-models.md)

```Python
script_params = {
    '--data-folder': ds.path('mnist').as_mount(),
    '--batch-size': 50,
    '--first-layer-neurons': 300,
    '--second-layer-neurons': 100,
    '--learning-rate': 0.001
}

est = TensorFlow(source_directory=script_folder,
                 entry_script='keras_mnist.py',
                 script_params=script_params,
                 compute_target=compute_target,
                 pip_packages=['keras', 'matplotlib'],
                 use_gpu=True)
```

## <a name="submit-a-run"></a>Een run verzenden

Het [object run](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py) biedt de interface voor de uitvoerings geschiedenis terwijl de taak wordt uitgevoerd en nadat deze is voltooid.

```Python
run = exp.submit(est)
run.wait_for_completion(show_output=True)
```

Wanneer de uitvoering wordt uitgevoerd, worden de volgende fasen door lopen:

- **Voorbereiden**: Een docker-installatie kopie wordt gemaakt op basis van de tensor flow-Estimator. De afbeelding wordt geüpload naar het container register van de werk ruimte en opgeslagen in de cache voor latere uitvoeringen. Logboeken worden ook gestreamd naar de uitvoerings geschiedenis en kunnen worden weer gegeven om de voortgang te bewaken.

- **Schalen**: Het cluster probeert omhoog te schalen als het Batch AI-cluster meer knoop punten nodig heeft om de uitvoering uit te voeren dan momenteel beschikbaar zijn.

- **Uitvoeren**: Alle scripts in de map script worden geüpload naar het Compute-doel, gegevens archieven worden gekoppeld of gekopieerd en de entry_script wordt uitgevoerd. Uitvoer van stdout en de map./logs worden gestreamd naar de uitvoerings geschiedenis en kunnen worden gebruikt om de uitvoering te bewaken.

- **Nabewerken**: De map./outputs van de uitvoering wordt gekopieerd naar de uitvoerings geschiedenis.

## <a name="register-the-model"></a>Registreer het model

Nadat u het DNN-model hebt getraind, kunt u dit registreren in uw werk ruimte. Met model registratie kunt u uw modellen in uw werk ruimte opslaan en versieren om het [model beheer en de implementatie](concept-model-management-and-deployment.md)te vereenvoudigen.

```Python
model = run.register_model(model_name='keras-dnn-mnist', model_path='outputs/model')
```

U kunt ook een lokale kopie van het model downloaden. Dit kan handig zijn om een extra model validatie lokaal te kunnen uitvoeren. In het trainings script wordt `mnist-keras.py`het model door een tensor flow-beveiligings object naar een lokale map (lokaal naar het Compute-doel) bewaard. U kunt het object run gebruiken om een kopie te downloaden uit de gegevens opslag.

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

In dit artikel hebt u een Keras-model getraind en geregistreerd bij Azure Machine Learning service. Ga verder met ons model implementatie artikel voor meer informatie over het implementeren van een model.

> [!div class="nextstepaction"]
> [Hoe en waar modellen moeten worden geïmplementeerd](how-to-deploy-and-where.md)
