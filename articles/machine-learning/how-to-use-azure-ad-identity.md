---
title: AAD-identiteit gebruiken met uw webservice
titleSuffix: Azure Machine Learning
description: Gebruik de AAD-identiteit met uw webservice in azure Kubernetes service om toegang te krijgen tot cloud resources tijdens de score.
services: machine-learning
ms.author: larryfr
author: BlackMist
ms.reviewer: aashishb
ms.service: machine-learning
ms.subservice: core
ms.date: 02/10/2020
ms.topic: conceptual
ms.custom: how-to
ms.openlocfilehash: 1789f83f048a2ab0fb75aa33635e58b0850b865b
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/04/2020
ms.locfileid: "93319133"
---
# <a name="use-azure-ad-identity-with-your-machine-learning-web-service-in-azure-kubernetes-service"></a>Azure AD-identiteit gebruiken met uw Machine Learning-webservice in Azure Kubernetes Service

In deze procedure leert u hoe u een Azure Active Directorys-id (AAD) toewijst aan uw geïmplementeerde machine learning model in de Azure Kubernetes-service. Met het [pod Identity](https://github.com/Azure/aad-pod-identity) project van Aad kunnen toepassingen veilig toegang krijgen tot Cloud bronnen met Aad met behulp van een [beheerde identiteit](../active-directory/managed-identities-azure-resources/overview.md) en Kubernetes primitieven. Zo kunt u uw webservices veilig toegang geven tot uw Azure-resources zonder dat u referenties hoeft in te sluiten of tokens rechtstreeks in uw script hoeft te beheren `score.py` . In dit artikel worden de stappen beschreven voor het maken en installeren van een Azure-identiteit in uw Azure Kubernetes service-cluster en het toewijzen van de identiteit aan uw geïmplementeerde webservice.

## <a name="prerequisites"></a>Vereisten

- De [Azure cli-extensie voor de machine learning-service](reference-azure-machine-learning-cli.md), de [Azure machine learning SDK voor Python](/python/api/overview/azure/ml/intro?preserve-view=true&view=azure-ml-py)of de [Azure machine learning Visual Studio code extension](tutorial-setup-vscode-extension.md).

- Toegang tot uw AKS-cluster met behulp van de `kubectl` opdracht. Zie [verbinding maken met het cluster](../aks/kubernetes-walkthrough.md#connect-to-the-cluster) voor meer informatie.

- Een Azure Machine Learning-webservice die is geïmplementeerd in uw AKS-cluster.

## <a name="create-and-install-an-azure-identity-in-your-aks-cluster"></a>Een Azure-identiteit maken en installeren in uw AKS-cluster

1. Gebruik de volgende opdracht om te bepalen of het AKS-cluster RBAC is ingeschakeld:

    ```azurecli-interactive
    az aks show --name <AKS cluster name> --resource-group <resource group name> --subscription <subscription id> --query enableRbac
    ```

    Met deze opdracht wordt een waarde geretourneerd `true` als RBAC is ingeschakeld. Deze waarde bepaalt de opdracht die in de volgende stap moet worden gebruikt.

1. Gebruik een van de volgende opdrachten om de [Aad pod-identiteit](https://github.com/Azure/aad-pod-identity#getting-started) in uw AKS-cluster te installeren:

    * Als op uw AKS-cluster **RBAC is ingeschakeld** , gebruikt u de volgende opdracht:
    
        ```azurecli-interactive
        kubectl apply -f https://raw.githubusercontent.com/Azure/aad-pod-identity/master/deploy/infra/deployment-rbac.yaml
        ```
    
    * Als op uw AKS-cluster **geen RBAC is ingeschakeld** , gebruikt u de volgende opdracht:
    
        ```azurecli-interactive
        kubectl apply -f https://raw.githubusercontent.com/Azure/aad-pod-identity/master/deploy/infra/deployment.yaml
        ```
    
        De uitvoer van de opdracht is vergelijkbaar met de volgende tekst:

        ```text
        customresourcedefinition.apiextensions.k8s.io/azureassignedidentities.aadpodidentity.k8s.io created
        customresourcedefinition.apiextensions.k8s.io/azureidentitybindings.aadpodidentity.k8s.io created
        customresourcedefinition.apiextensions.k8s.io/azureidentities.aadpodidentity.k8s.io created
        customresourcedefinition.apiextensions.k8s.io/azurepodidentityexceptions.aadpodidentity.k8s.io created
        daemonset.apps/nmi created
        deployment.apps/mic created
        ```

1. [Maak een Azure-identiteit](https://github.com/Azure/aad-pod-identity#2-create-an-azure-identity) volgens de stappen die worden weer gegeven op de pagina Aad pod Identity project.

1. [Installeer de Azure-identiteit](https://github.com/Azure/aad-pod-identity#3-install-the-azure-identity) volgens de stappen die worden weer gegeven op de pagina Aad pod Identity project.

1. [Installeer de Azure-identiteits binding](https://github.com/Azure/aad-pod-identity#5-install-the-azure-identity-binding) volgens de stappen die worden weer gegeven op de pagina Aad pod Identity project.

1. Als de Azure-identiteit die u in de vorige stap hebt gemaakt, zich niet in dezelfde resource groep bevindt als uw AKS-cluster, kunt u [machtigingen voor mic instellen](https://github.com/Azure/aad-pod-identity#6-set-permissions-for-mic) volgen volgens de stappen die worden weer gegeven op de pagina Aad pod Identity project.

## <a name="assign-azure-identity-to-machine-learning-web-service"></a>Azure-identiteit aan machine learning-webservice toewijzen

De volgende stappen maken gebruik van de Azure-identiteit die in de vorige sectie is gemaakt en wijst deze toe aan uw AKS-webservice via een **selector-label**.

Bepaal eerst de naam en naam ruimte van uw implementatie in uw AKS-cluster waaraan u de Azure-identiteit wilt toewijzen. U kunt deze informatie verkrijgen door de volgende opdracht uit te voeren. De naam ruimten moet uw Azure Machine Learning werkruimte zijn en uw implementatie naam moet de naam van het eind punt zijn zoals weer gegeven in de portal.

```azurecli-interactive
kubectl get deployment --selector=isazuremlapp=true --all-namespaces --show-labels
```

Voeg het label selector van Azure-identiteit toe aan uw implementatie door de implementatie specificatie te bewerken. De selectorwaarde moet de waarde zijn die u in stap 5 van [de Azure-identiteits binding installeren](https://github.com/Azure/aad-pod-identity#5-install-the-azure-identity-binding)hebt gedefinieerd.

```yaml
apiVersion: "aadpodidentity.k8s.io/v1"
kind: AzureIdentityBinding
metadata:
  name: demo1-azure-identity-binding
spec:
  AzureIdentity: <a-idname>
  Selector: <label value to match>
```

Bewerk de implementatie om het label van de Azure Identity Selector toe te voegen. Ga naar de volgende sectie onder `/spec/template/metadata/labels` . U ziet waarden zoals `isazuremlapp: “true”` . Voeg het label Aad-pod-Identity toe, zoals hieronder wordt weer gegeven.

```azurecli-interactive
    kubectl edit deployment/<name of deployment> -n azureml-<name of workspace>
```

```yaml
spec:
  template:
    metadata:
      labels:
       aadpodidbinding: "<value of Selector in AzureIdentityBinding>"
      ...
```

Voer de volgende opdracht uit om te controleren of het label correct is toegevoegd.

```azurecli-interactive
   kubectl get deployment <name of deployment> -n azureml-<name of workspace> --show-labels
```

Voer de volgende opdracht uit om alle pod-statussen weer te geven.

```azurecli-interactive
    kubectl get pods -n azureml-<name of workspace>
```

Zodra het meren onderdeel actief is, kunnen de webservices voor deze implementatie nu toegang krijgen tot Azure-resources via uw Azure-identiteit zonder dat u de referenties in uw code hoeft in te sluiten. 

## <a name="assign-the-appropriate-roles-to-your-azure-identity"></a>De juiste rollen toewijzen aan uw Azure-identiteit

[Wijs uw door Azure beheerde identiteit toe met de juiste rollen](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md) om toegang te krijgen tot andere Azure-resources. Zorg ervoor dat de rollen die u toewijst de juiste **gegevens acties** hebben. De rol van de [gegevens lezer](../role-based-access-control/built-in-roles.md#storage-blob-data-reader) van de opslag-BLOB heeft bijvoorbeeld lees machtigingen voor uw opslag-BLOB terwijl de rol van algemene [lezer](../role-based-access-control/built-in-roles.md#reader) niet mogelijk is.

## <a name="use-azure-identity-with-your-machine-learning-web-service"></a>Azure Identity gebruiken met uw machine learning-webservice

Implementeer een model op uw AKS-cluster. Het `score.py` script kan bewerkingen bevatten die verwijzen naar de Azure-resources waartoe uw Azure-identiteit toegang heeft. Zorg ervoor dat u de vereiste client bibliotheek afhankelijkheden hebt geïnstalleerd voor de bron waartoe u toegang probeert te krijgen. Hieronder vindt u enkele voor beelden van hoe u uw Azure-identiteit kunt gebruiken voor toegang tot verschillende Azure-resources vanuit uw service.

### <a name="access-key-vault-from-your-web-service"></a>Toegang tot Key Vault via uw webservice

Als u uw Azure Identity Lees toegang hebt gegeven tot een geheim in een **Key Vault** , `score.py` kunt u er toegang toe krijgen met de volgende code.

```python
from azure.identity import DefaultAzureCredential
from azure.keyvault.secrets import SecretClient

my_vault_name = "yourkeyvaultname"
my_vault_url = "https://{}.vault.azure.net/".format(my_vault_name) 
my_secret_name = "sample-secret"

# This will use your Azure Managed Identity
credential = DefaultAzureCredential()
secret_client = SecretClient(
    vault_url=my_vault_url,
    credential=credential)
secret = secret_client.get_secret(my_secret_name)
```

> [!IMPORTANT]
> In dit voor beeld wordt het DefaultAzureCredential gebruikt. Zie [een Key Vault toegangs beleid toewijzen met behulp van de Azure-cli](../key-vault/general/assign-access-policy-cli.md)als u uw identiteit toegang wilt verlenen met behulp van een specifiek toegangs beleid.

### <a name="access-blob-from-your-web-service"></a>Toegang tot BLOB vanuit uw webservice

Als u uw Azure Identity Lees toegang hebt tot gegevens in een **opslag-BLOB** , `score.py` kunt u deze openen met de volgende code.

```python
from azure.identity import DefaultAzureCredential
from azure.storage.blob import BlobServiceClient

my_storage_account_name = "yourstorageaccountname"
my_storage_account_url = "https://{}.blob.core.windows.net/".format(my_storage_account_name)

# This will use your Azure Managed Identity
credential = DefaultAzureCredential()
blob_service_client = BlobServiceClient(
    account_url=my_storage_account_url,
    credential=credential
)
blob_client = blob_service_client.get_blob_client(container="some-container", blob="some_text.txt")
blob_data = blob_client.download_blob()
blob_data.readall()
```

## <a name="next-steps"></a>Volgende stappen

* Zie de [opslag plaats](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/identity/azure-identity#azure-identity-client-library-for-python) op github voor meer informatie over het gebruik van de python Azure Identity client-bibliotheek.
* Zie voor een gedetailleerde hand leiding voor het implementeren van modellen voor Azure Kubernetes-Service clusters de [instructies](how-to-deploy-azure-kubernetes-service.md).