---
title: Toegangs sleutels voor opslag accounts wijzigen
titleSuffix: Azure Machine Learning service
description: Meer informatie over het wijzigen van de toegangs sleutels voor het Azure Storage-account dat wordt gebruikt door uw werk ruimte. Azure Machine Learning-service gebruikt een Azure Storage-account om gegevens en modellen op te slaan. Wanneer u de toegangs sleutel voor het opslag account opnieuw genereert, moet u de Azure Machine Learning-service bijwerken om de nieuwe sleutels te gebruiken.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 05/17/2019
ms.openlocfilehash: 0721542811709e9b938fea3f31bc2a0a28ecdc74
ms.sourcegitcommit: 4b647be06d677151eb9db7dccc2bd7a8379e5871
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2019
ms.locfileid: "68358771"
---
# <a name="regenerate-storage-account-access-keys"></a>Toegangs sleutels voor het opslag account opnieuw genereren

Meer informatie over het wijzigen van de toegangs sleutels voor Azure Storage accounts die worden gebruikt door de Azure Machine Learning-service. Azure Machine Learning kunt opslag accounts gebruiken om gegevens of getrainde modellen op te slaan.

Uit veiligheids overwegingen moet u de toegangs sleutels voor een Azure Storage-account mogelijk wijzigen. Wanneer u de toegangs sleutel opnieuw genereert, moet Azure Machine Learning worden bijgewerkt om de nieuwe sleutel te gebruiken. Azure Machine Learning maakt mogelijk gebruik van het opslag account voor zowel model opslag als een gegevens opslag.

## <a name="prerequisites"></a>Vereisten

* Een werkruimte van Azure Machine Learning-service. Zie het artikel [een werk ruimte maken](setup-create-workspace.md) voor meer informatie.

* De [Azure machine learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py).

* De [Azure machine learning cli-extensie](reference-azure-machine-learning-cli.md).

<a id="whattoupdate"></a> 

## <a name="what-needs-to-be-updated"></a>Wat moet worden bijgewerkt?

Opslag accounts kunnen worden gebruikt door de Azure Machine Learning service-werk ruimte (het opslaan van Logboeken, modellen, moment opnamen, enzovoort) en als een gegevens opslag. Het proces voor het bijwerken van de werk ruimte is een enkele Azure CLI-opdracht en kan worden uitgevoerd na het bijwerken van de opslag sleutel. Het proces voor het bijwerken van gegevens opslag is meer betrokken en hiervoor is het vereist dat er wordt gedetecteerd welke data stores momenteel gebruikmaken van het opslag account en vervolgens opnieuw moeten worden geregistreerd.

> [!IMPORTANT]
> Werk de werk ruimte bij met behulp van de Azure CLI en de gegevens opslag in Python, op hetzelfde moment. Het bijwerken van de ene of de andere is niet voldoende en kan fouten veroorzaken totdat beide zijn bijgewerkt.

Gebruik de volgende code om de opslag accounts te detecteren die worden gebruikt door uw gegevens opslag:

```python
import azureml.core
from azureml.core import Workspace, Datastore

ws = Workspace.from_config()

default_ds = ws.get_default_datastore()
print("Default datstore: " + default_ds.name + ", storage account name: " +
      default_ds.account_name + ", container name: " + ds.container_name)

datastores = ws.datastores
for name, ds in datastores.items():
    if ds.datastore_type == "AzureBlob" or ds.datastore_type == "AzureFile":
        print("datastore name: " + name + ", storage account name: " +
              ds.account_name + ", container name: " + ds.container_name)
```

Met deze code wordt gezocht naar geregistreerde gegevens opslag die Azure Storage gebruikt en wordt de volgende informatie weer gegeven:

* Naam gegevens opslag: De naam van het gegevens archief waarvoor het opslag account is geregistreerd.
* Naam van opslagaccount: De naam van het Azure Storage-account.
* Verpakking De container in het opslag account dat wordt gebruikt door deze registratie.

Als er een vermelding bestaat voor het opslag account dat u wilt gebruiken voor het opnieuw genereren van toegangs sleutels voor, slaat u de naam van het gegevens archief, de naam van het opslag account en de naam van de container op.

## <a name="update-the-access-key"></a>De toegangs sleutel bijwerken

Voer de volgende stappen uit om Azure Machine Learning-service bij te werken voor het gebruik van de nieuwe sleutel:

> [!IMPORTANT]
> Voer alle stappen uit en werk de werk ruimte bij met behulp van de CLI en gegevens opslag met python. Het bijwerken van de ene of de andere kan fouten veroorzaken totdat beide zijn bijgewerkt.

1. Genereer de sleutel opnieuw. Zie het artikel [een opslag account beheren](/azure/storage/common/storage-account-manage#access-keys) voor meer informatie over het opnieuw genereren van een toegangs sleutel. Sla de nieuwe sleutel op.

1. Als u de werk ruimte voor het gebruik van de nieuwe sleutel wilt bijwerken, gebruikt u de volgende stappen:

    1. Meld u aan bij het Azure-abonnement dat uw werk ruimte bevat met behulp van de volgende Azure CLI-opdracht:

        ```azurecli-interactive
        az login
        ```

    1. Als u de extensie Azure Machine Learning wilt installeren, gebruikt u de volgende opdracht:

        ```azurecli-interactive
        az extension add -n azure-cli-ml 
        ```

    1. Als u de werk ruimte voor het gebruik van de nieuwe sleutel wilt bijwerken, gebruikt u de volgende opdracht. Vervang `myworkspace` door de naam van uw Azure machine learning werkruimte en `myresourcegroup` Vervang door de naam van de Azure-resource groep die de werk ruimte bevat.

        ```azurecli-interactive
        az ml workspace sync-keys -w myworkspace -g myresourcegroup
        ```

        Met deze opdracht worden automatisch de nieuwe sleutels gesynchroniseerd voor het Azure-opslag account dat wordt gebruikt door de werk ruimte.

1. Als u de Data Store (s) die gebruikmaken van het opslag account opnieuw wilt registreren, gebruikt u de waarden in de sectie [Wat moet worden bijgewerkt](#whattoupdate) en de sleutel uit stap 1 met de volgende code:

    ```python
    ds = Datastore.register_azure_blob_container(workspace=ws, 
                                              datastore_name='your datastore name', 
                                              container_name='your container name',
                                              account_name='your storage account name', 
                                              account_key='new storage account key',
                                              overwrite=True)
    ```

    Sinds `overwrite=True` is opgegeven, overschrijft deze code de bestaande registratie en werkt deze bij om de nieuwe sleutel te gebruiken.

## <a name="next-steps"></a>Volgende stappen

Zie de verwijzing naar een klasse voor meer informatie over [`Datastore`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py) het registreren van gegevens opslag.
