---
title: 'Zelfstudie: Aan de slag met machine learning - Python'
titleSuffix: Azure Machine Learning
description: In deze zelfstudie gaat u aan de slag met de Azure Machine Learning SDK voor Python die wordt uitgevoerd op uw persoonlijke ontwikkelsomgeving.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: aminsaied
ms.author: amsaied
ms.reviewer: sgilley
ms.date: 09/15/2020
ms.custom: devx-track-python
ms.openlocfilehash: c0fe3c3808709de732bec8ce0599d380094405e8
ms.sourcegitcommit: 5dbea4631b46d9dde345f14a9b601d980df84897
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2020
ms.locfileid: "91368478"
---
# <a name="tutorial-get-started-with-azure-machine-learning-in-your-development-environment-part-1-of-4"></a>Zelfstudie: Aan de slag met Azure Machine Learning in uw ontwikkelomgeving (deel 1 van 4)

In deze *vierdelige zelfstudiereeks* leert u de grondbeginselen van Azure Machine Learning en kunt u op taken gebaseerde Python machine learning-taken uitvoeren op het Azure-cloudplatform. Hierbij gaat het onder andere om de volgende taken:

1. Een werkruimte en uw lokale machine learning-ontwikkelomgeving instellen.
2. Code uitvoeren in de cloud met behulp van de Azure Machine Learning-SDK voor Python.
3. De Python-omgeving beheren die u gebruikt voor modeltraining.
4. Gegevens uploaden naar Azure en die gegevens gebruiken in de training.

In deel 1 van deze zelfstudiereeks doet u het volgende:

> [!div class="checklist"]
> * De Azure Machine Learning-SDK installeren.
> * De mapstructuur voor code instellen
> * Een Azure Machine Learning-werkruimte maken.
> * Uw lokale ontwikkelomgeving configureren.
> * Een rekencluster instellen.

>[!NOTE]
> Deze zelfstudiereeks is gericht op de Azure Machine Learning-concepten die zijn afgestemd op *op taken gebaseerde* machine learning-taken van Python die rekenintensief zijn en/of reproduceerbaarheid vereisen. Als uw machine learning-taken niet in dit profiel passen, gebruikt u de functionaliteit [Jupyter of RStudio op een Azure Machine Learning-rekeninstantie](tutorial-1st-experiment-sdk-setup.md) om naar Azure Machine Learning over te stappen.

## <a name="prerequisites"></a>Vereisten

- Een Azure-abonnement. Als u geen Azure-abonnement hebt, maakt u een gratis account voordat u begint. Probeer [Azure Machine Learning](https://aka.ms/AMLFree) uit.
- Vertrouwd met Python en [Machine Learning-concepten](concept-azure-machine-learning-architecture.md). Bijvoorbeeld omgevingen, training, scoren, enzovoort.
- Een lokale ontwikkelomgeving: een laptop met Python geïnstalleerd en uw favoriete IDE (bijvoorbeeld Visual Studio Code, PyCharm of Jupyter).

## <a name="install-the-azure-machine-learning-sdk"></a>De Azure Machine Learning-SDK installeren

In deze zelfstudie maken we gebruik van de Azure Machine Learning-SDK voor Python.

U kunt de hulpprogramma's die u het beste kent, gebruiken (bijvoorbeeld Conda en pip) om een omgeving in te stellen om in deze zelfstudie te gebruiken. Installeer de Azure Machine Learning-SDK voor Python via pip in de omgeving:

```bash
pip install azureml-sdk
```

## <a name="create-a-directory-structure-for-code"></a>Mapstructuur voor code maken
We raden aan dat u de volgende eenvoudige mapstructuur instelt voor deze zelfstudie:

```markdown
tutorial
└──.azureml
```

- `tutorial`: Map op het hoogste niveau van het project.
- `.azureml`: Verborgen submap om Azure Machine Learning-configuratiebestanden op te slaan.

## <a name="create-an-azure-machine-learning-workspace"></a>Een Azure Machine Learning-werkruimte maken

Een werkruimte is een resource op het hoogste niveau voor Azure Machine Learning en is een centrale locatie voor het volgende:

- Resources zoals Compute beheren.
- Assets opslaan zoals notebooks, omgevingen, gegevenssets, pijplijnen, modellen, eindpunten, enzovoort.
- Samenwerken met andere teamleden.

Voeg in de map op het hoogste niveau `tutorial` een nieuw Python-bestand toe met de naam `01-create-workspace.py` met behulp van de volgende code. Pas de parameters (naam, abonnements-id, resourcegroep en [locatie](https://azure.microsoft.com/global-infrastructure/services/?products=machine-learning-service) aan met uw voorkeuren.

U kunt de code uitvoeren in een interactieve sessie of als een Python-bestand.

>[!NOTE]
> Wanneer u een lokale ontwikkelomgeving (zoals een laptop) gebruikt, wordt u gevraagd om u te verifiëren bij uw werkruimte met behulp van een *apparaatcode* de eerste keer dat u de volgende code uitvoert. Volg de instructies op het scherm.

```python
# tutorial/01-create-workspace.py
from azureml.core import Workspace

ws = Workspace.create(name='<my_workspace_name>', # provide a name for your workspace
                      subscription_id='<azure-subscription-id>', # provide your subscription ID
                      resource_group='<myresourcegroup>', # provide a resource group name
                      create_resource_group=True,
                      location='<NAME_OF_REGION>') # For example: 'westeurope' or 'eastus2' or 'westus2' or 'southeastasia'.

# write out the workspace details to a configuration file: .azureml/config.json
ws.write_config(path='.azureml')
```

Voer deze code uit vanuit de map `tutorial`:

```bash
cd <path/to/tutorial>
python ./01-create-workspace.py
```

Nadat u het voorgaande codefragment hebt uitgevoerd, ziet uw mapstructuur er als volgt uit:

```markdown
tutorial
└──.azureml
|  └──config.json
└──01-create-workspace.py
```

Het bestand `.azureml/config.json` bevat de metagegevens die nodig zijn om verbinding te maken met uw Azure Machine Learning-werkruimte. Anders gezegd, het bevat uw abonnements-ID, resourcegroep en de naam van uw werkruimte. 

> [!NOTE]
> De inhoud van `config.json` is niet geheim. Deze details mogen gedeeld worden.
>
> Verificatie is wel nog vereist om te communiceren met uw Azure Machine Learning-werkruimte.

## <a name="create-an-azure-machine-learning-compute-cluster"></a>Een Azure Machine Learning-rekencluster maken

Maak een Python-script in de map `tutorial` op het hoogste niveau en geef deze de naam `02-create-compute.py`. Vul dit met de volgende code in om een Azure Machine Learning-rekencluster te maken dat automatisch wordt geschaald tussen nul en vier knooppunten:

```python
# tutorial/02-create-compute.py
from azureml.core import Workspace
from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException

ws = Workspace.from_config() # This automatically looks for a directory .azureml

# Choose a name for your CPU cluster
cpu_cluster_name = "cpu-cluster"

# Verify that the cluster does not exist already
try:
    cpu_cluster = ComputeTarget(workspace=ws, name=cpu_cluster_name)
    print('Found existing cluster, use it.')
except ComputeTargetException:
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_D2_V2',
                                                            max_nodes=4, 
                                                            idle_seconds_before_scaledown=2400)
    cpu_cluster = ComputeTarget.create(ws, cpu_cluster_name, compute_config)

cpu_cluster.wait_for_completion(show_output=True)
```

Het Python-bestand uitvoeren:

```bash
python ./02-create-compute.py
```


> [!NOTE]
> Wanneer de cluster is gemaakt, zijn er 0 knooppunten ingericht. Voor de worden cluster *geen* kosten in rekening gebracht totdat u een taak verzendt. Dit cluster wordt omlaag geschaald wanneer het gedurende 2,400 seconden inactief is geweest (40 minuten).

De mapstructuur ziet er nu als volgt uit:

```bash
tutorial
└──.azureml
|  └──config.json
└──01-create-workspace.py
└──02-create-compute.py
```

## <a name="next-steps"></a>Volgende stappen

In deze installatiezelfstudie hebt u het volgende gedaan:

- Een Azure Machine Learning-werkruimte gemaakt.
- De lokale ontwikkelomgeving instellen.
- Een Azure Machine Learning-rekencluster gemaakt.

In de volgende zelfstudie gaat u een script verzenden naar het Azure Machine Learning-rekencluster.

> [!div class="nextstepaction"]
> [Zelfstudie: Een Python-script voor 'Hallo wereld' uitvoeren Python-script in Azure](tutorial-1st-experiment-hello-world.md)
