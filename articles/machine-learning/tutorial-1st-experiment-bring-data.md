---
title: 'Zelfstudie: Uw eigen gegevens gebruiken'
titleSuffix: Azure Machine Learning
description: In deel 4 van de aan de slag-serie van Azure ML ziet u hoe u uw eigen gegevens kunt gebruiken in een externe trainingsuitvoering.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: aminsaied
ms.author: amsaied
ms.reviewer: sgilley
ms.date: 09/15/2020
ms.custom: tracking-python
ms.openlocfilehash: 13d43eb788c750a2f24033a6138ebf00ac57fffe
ms.sourcegitcommit: 5dbea4631b46d9dde345f14a9b601d980df84897
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2020
ms.locfileid: "91372563"
---
# <a name="tutorial-use-your-own-data-part-4-of-4"></a>Zelfstudie: Uw eigen gegevens gebruiken (Deel 4 van 4)

Deze zelfstudie laat zien hoe u uw eigen gegevens kunt uploaden en gebruiken om machine learning-modellen in Azure Machine Learning te trainen.

Deze zelfstudie is *deel 4 van een vierdelige zelfstudiereeks* waarin u de grondbeginselen van Azure Machine Learning kunt zien en machine learning-taken op basis van taken uitvoert in Azure. Deze zelfstudie borduurt voort op het werk dat u hebt voltooid in [Deel 1: Instellen](tutorial-1st-experiment-sdk-setup-local.md), [Deel 2: 'Hello World!' uitvoeren](tutorial-1st-experiment-hello-world.md) en [Deel 3: Een model trainen](tutorial-1st-experiment-sdk-train.md).

In [Deel 3: Een model trainen](tutorial-1st-experiment-sdk-train.md) zijn gegevens gedownload met behulp van de ingebouwde `torchvision.datasets.CIFAR10`-methode in de PyTorch-API. In veel gevallen wilt u echter uw eigen gegevens gebruiken in een externe trainingsuitvoering. In dit artikel ziet u welke werkstroom u kunt gebruiken om met uw eigen gegevens te werken in Azure Machine Learning.

In deze zelfstudie gaat u:

> [!div class="checklist"]
> * Een trainingsscript configureren voor het gebruik van gegevens in een lokale map.
> * Het trainingsscript lokaal testen.
> * Gegevens uploaden naar Azure.
> * Een besturingsscript maken.
> * Inzicht krijgen in de nieuwe Azure Machine Learning-concepten (parameters doorgeven, gegevenssets, gegevensarchief).
> * Uw trainingsscript verzenden en uitvoeren.
> * De code-uitvoer weergeven in de cloud.

## <a name="prerequisites"></a>Vereisten

* Voltooiing van [Deel 3](tutorial-1st-experiment-sdk-train.md) van de serie.
* Introductiekennis van de Python-taal en machine learning-werkstromen.
* Lokale ontwikkelomgeving, zoals Visual Studio Code, Jupyter of PyCharm.
* Python (versie 3.5 tot 3.7).

## <a name="adjust-the-training-script"></a>Het trainingsscript aanpassen
U hebt nu uw trainingsscript (tutorial/src/train.py) aangezet in Azure Machine Learning en kunt de modelprestaties bewaken. We gaan de parameters van het trainingsscript bepalen door argumenten te introduceren. Door argumenten te gebruiken, kunt u eenvoudig verschillende hyperparameters vergelijken.

Het trainingsscript is nu ingesteld op het downloaden van de CIFAR10-gegevensset voor elke uitvoering. De volgende Python-code is zo aangepast dat de gegevens uit een map worden gelezen.

>[!NOTE] 
> Als u `argparse` gebruikt, worden parameters bepaald voor het script.

```python
# tutorial/src/train.py
import os
import argparse
import torch
import torch.optim as optim
import torchvision
import torchvision.transforms as transforms

from model import Net
from azureml.core import Run

run = Run.get_context()

if __name__ == "__main__":
    parser = argparse.ArgumentParser()
    parser.add_argument('--data_path', type=str, help='Path to the training data')
    parser.add_argument('--learning_rate', type=float, default=0.001, help='Learning rate for SGD')
    parser.add_argument('--momentum', type=float, default=0.9, help='Momentum for SGD')
    args = parser.parse_args()
    
    print("===== DATA =====")
    print("DATA PATH: " + args.data_path)
    print("LIST FILES IN DATA PATH...")
    print(os.listdir(args.data_path))
    print("================")
    
    # prepare DataLoader for CIFAR10 data
    transform = transforms.Compose([transforms.ToTensor(), transforms.Normalize((0.5, 0.5, 0.5), (0.5, 0.5, 0.5))])
    trainset = torchvision.datasets.CIFAR10(
        root=args.data_path,
        train=True,
        download=False,
        transform=transform,
    )
    trainloader = torch.utils.data.DataLoader(trainset, batch_size=4, shuffle=True, num_workers=2)

    # define convolutional network
    net = Net()

    # set up pytorch loss /  optimizer
    criterion = torch.nn.CrossEntropyLoss()
    optimizer = optim.SGD(
        net.parameters(),
        lr=args.learning_rate,
        momentum=args.momentum,
    )

    # train the network
    for epoch in range(2):

        running_loss = 0.0
        for i, data in enumerate(trainloader, 0):
            # unpack the data
            inputs, labels = data

            # zero the parameter gradients
            optimizer.zero_grad()

            # forward + backward + optimize
            outputs = net(inputs)
            loss = criterion(outputs, labels)
            loss.backward()
            optimizer.step()

            # print statistics
            running_loss += loss.item()
            if i % 2000 == 1999:
                loss = running_loss / 2000
                run.log('loss', loss) # log loss metric to AML
                print(f'epoch={epoch + 1}, batch={i + 1:5}: loss {loss:.2f}')
                running_loss = 0.0

    print('Finished Training')
```

### <a name="understanding-the-code-changes"></a>De codewijzigingen begrijpen

Voor de code in `train.py` is de bibliotheek `argparse` gebruikt om `data_path`, `learning_rate` en `momentum` in te stellen.

```python
# .... other code
parser = argparse.ArgumentParser()
parser.add_argument('--data_path', type=str, help='Path to the training data')
parser.add_argument('--learning_rate', type=float, default=0.001, help='Learning rate for SGD')
parser.add_argument('--momentum', type=float, default=0.9, help='Momentum for SGD')
args = parser.parse_args()
# ... other code
```

Daarnaast is het `train.py`-script aangepast om de optimizer bij te werken voor het gebruik van door de gebruiker gedefinieerde parameters:

```python
optimizer = optim.SGD(
    net.parameters(),
    lr=args.learning_rate,     # get learning rate from command-line argument
    momentum=args.momentum,    # get momentum from command-line argument
)
```

## <a name="test-the-script-locally"></a>Het script lokaal testen

Uw script accepteert nu _gegevenspad_ als een argument. U moet het als eerste lokaal testen. Voeg een map met de naam `data` toe aan de mapstructuur van uw zelfstudie. De mapstructuur moet er als volgt uitzien:

```txt
tutorial
└──.azureml
|  └──config.json
|  └──pytorch-env.yml
└──data
└──src
|  └──hello.py
|  └──model.py
|  └──train.py
└──01-create-workspace.py
└──02-create-compute.py
└──03-run-hello.py
└──04-run-pytorch.py
```

Als u `train.py` niet lokaal hebt uitgevoerd in de vorige zelfstudie, hebt u de map `data/` niet. In dit geval voert u de `torchvision.datasets.CIFAR10`-methode lokaal uit met `download=True` in uw `train.py`-script.

Om het gewijzigde trainingsscript lokaal uit te voeren, roept u het volgende aan:

```bash
python src/train.py --data_path ./data --learning_rate 0.003 --momentum 0.92
```

U moet voorkomen dat u de CIFAR10-gegevensset downloadt door een lokaal pad naar de gegevens op te geven. U kunt ook experimenteren met verschillende waarden voor de hyperparameters voor _leersnelheid_ en _momentum_ zonder ze in het trainingsscript te hoeven coderen.

## <a name="upload-the-data-to-azure"></a>De gegevens uploaden naar Azure

Als u dit script in Azure Machine Learning wilt uitvoeren, moet u uw trainingsgegevens beschikbaar maken in Azure. Uw Azure Machine Learning-werkruimte is voorzien van een _standaard_gegevensopslag. Dit is een Azure Blob Storage-account waarin u uw trainingsgegevens kunt opslaan.

>[!NOTE] 
> Met Azure Machine Learning kunt u verbinding maken met andere gegevensarchieven in de cloud die uw gegevens opslaan. Zie [Documentatie over gegevensarchieven](./concept-data.md) voor meer informatie.  

Maak een nieuw Python-besturingsscript met de naam `05-upload-data.py` in de `tutorial`-map:

```python
# tutorial/05-upload-data.py
from azureml.core import Workspace
ws = Workspace.from_config()
datastore = ws.get_default_datastore()
datastore.upload(src_dir='./data', target_path='datasets/cifar10', overwrite=True)
```

De waarde `target_path` geeft het pad op naar het gegevensarchief waarnaar de CIFAR10-gegevens worden geüpload.

>[!TIP] 
> Terwijl u Azure Machine Learning gebruikt om de gegevens te uploaden, kunt u [Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/) gebruiken om ad-hocbestanden te uploaden. Als u een ETL-hulpprogramma nodig hebt, kunt u [Azure Data Factory](https://docs.microsoft.com/azure/data-factory/introduction) gebruiken om uw gegevens in Azure op te nemen.

Voer het Python-bestand uit om de gegevens te uploaden. (De upload zou minder dan 60 seconden moeten duren.)

```bash
python 05-upload-data.py
```
U ziet de volgende standaarduitvoer:
```txt
Uploading ./data\cifar-10-batches-py\data_batch_2
Uploaded ./data\cifar-10-batches-py\data_batch_2, 4 files out of an estimated total of 9
.
.
Uploading ./data\cifar-10-batches-py\data_batch_5
Uploaded ./data\cifar-10-batches-py\data_batch_5, 9 files out of an estimated total of 9
Uploaded 9 files
```


## <a name="create-a-control-script"></a>Een besturingsscript maken

Net zoals u eerder hebt gedaan, maakt u een nieuw Python-besturingsscript met de naam `06-run-pytorch-data.py`:

```python
# tutorial/06-run-pytorch-data.py
from azureml.core import Workspace
from azureml.core import Experiment
from azureml.core import Environment
from azureml.core import ScriptRunConfig
from azureml.core import Dataset

if __name__ == "__main__":
    ws = Workspace.from_config()
    
    datastore = ws.get_default_datastore()
    dataset = Dataset.File.from_files(path=(datastore, 'datasets/cifar10'))

    experiment = Experiment(workspace=ws, name='day1-experiment-data')

    config = ScriptRunConfig(
        source_directory='./src',
        script='train.py',
        compute_target='cpu-cluster',
        arguments=[
            '--data_path', dataset.as_named_input('input').as_mount(),
            '--learning_rate', 0.003,
            '--momentum', 0.92],
        )
    
    # set up pytorch environment
    env = Environment.from_conda_specification(name='pytorch-env',file_path='.azureml/pytorch-env.yml')
    config.run_config.environment = env

    run = experiment.submit(config)
    aml_url = run.get_portal_url()
    print("Submitted to an Azure Machine Learning compute cluster. Click on the link below")
    print("")
    print(aml_url)
```

### <a name="understand-the-code-changes"></a>De codewijzigingen begrijpen

Het besturingsscript is vergelijkbaar met dat van [Deel 3 van deze serie](tutorial-1st-experiment-sdk-train.md) met de volgende nieuwe regels:

:::row:::
   :::column span="":::
      `dataset = Dataset.File.from_files( ... )`
   :::column-end:::
   :::column span="2":::
      Een [gegevensset](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py&preserve-view=true) wordt gebruikt om te verwijzen naar de gegevens die u hebt geüpload naar Azure Blob Storage. Gegevenssets zijn een abstracte laag bovenop uw gegevens die is ontworpen voor de verbetering van de betrouwbaarheid.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      `config = ScriptRunConfig(...)`
   :::column-end:::
   :::column span="2":::
      [ScriptRunConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.scriptrunconfig?view=azure-ml-py&preserve-view=true) is gewijzigd en bevat een lijst met argumenten die worden doorgegeven aan `train.py`. Het argument `dataset.as_named_input('input').as_mount()` betekent dat de opgegeven map wordt _gekoppeld_ aan het rekendoel.
   :::column-end:::
:::row-end:::

## <a name="submit-the-run-to-azure-machine-learning"></a>De uitvoering versturen naar Microsoft Azure Machine Learning

Verzend de uitvoering nu opnieuw om de nieuwe configuratie te gebruiken:

```bash
python 06-run-pytorch-data.py
```

Met deze code wordt een URL naar het experiment afgedrukt in Azure Machine Learning Studio. Als u naar deze link gaat, kunt u zien dat de code wordt uitgevoerd.

### <a name="inspect-the-log-file"></a>Het logboekbestand controleren

Ga in Studio naar de experimentuitvoering (door de vorige URL-uitvoer te selecteren), gevolgd door **Uitvoer en logboeken**. Selecteer het bestand `70_driver_log.txt`. U moet de volgende uitvoer zien:

```txt
Processing 'input'.
Processing dataset FileDataset
{
  "source": [
    "('workspaceblobstore', 'datasets/cifar10')"
  ],
  "definition": [
    "GetDatastoreFiles"
  ],
  "registration": {
    "id": "XXXXX",
    "name": null,
    "version": null,
    "workspace": "Workspace.create(name='XXXX', subscription_id='XXXX', resource_group='X')"
  }
}
Mounting input to /tmp/tmp9kituvp3.
Mounted input to /tmp/tmp9kituvp3 as folder.
Exit __enter__ of DatasetContextManager
Entering Run History Context Manager.
Current directory:  /mnt/batch/tasks/shared/LS_root/jobs/dsvm-aml/azureml/tutorial-session-3_1600171983_763c5381/mounts/workspaceblobstore/azureml/tutorial-session-3_1600171983_763c5381
Preparing to call script [ train.py ] with arguments: ['--data_path', '$input', '--learning_rate', '0.003', '--momentum', '0.92']
After variable expansion, calling script [ train.py ] with arguments: ['--data_path', '/tmp/tmp9kituvp3', '--learning_rate', '0.003', '--momentum', '0.92']

Script type = None
===== DATA =====
DATA PATH: /tmp/tmp9kituvp3
LIST FILES IN DATA PATH...
['cifar-10-batches-py', 'cifar-10-python.tar.gz']
```

Kennisgeving:

- Azure Machine Learning heeft Blob Storage automatisch gekoppeld aan het rekencluster.
- De ``dataset.as_named_input('input').as_mount()`` die wordt gebruikt in het besturingsscript, wordt omgezet naar het koppelpunt.

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [aml-delete-resource-group](../../includes/aml-delete-resource-group.md)]

U kunt de resourcegroep ook bewaren en slechts één werkruimte verwijderen. Bekijk de eigenschappen van de werkruimte en selecteer **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebben we gezien hoe we gegevens naar Azure moeten uploaden met `Datastore`. Het gegevensarchief wordt geleverd als cloudopslag voor uw werkruimte, waardoor u een permanente en flexibele plaats hebt om uw gegevens te bewaren.

U hebt gezien hoe u uw trainingsscript kunt aanpassen om een gegevenspad te accepteren via de opdrachtregel. Met `Dataset` hebt u een map gekoppeld aan de externe uitvoering. 

Nu u een model hebt, leert u het volgende:

* [Modellen implementeren met Azure Machine Learning](how-to-deploy-and-where.md).
