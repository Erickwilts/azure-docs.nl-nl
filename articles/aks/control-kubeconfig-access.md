---
title: Beperk de toegang tot kubeconfig in Azure Kubernetes Service (AKS)
description: Meer informatie over het beheren van toegang tot het Kubernetes-configuratiebestand (kubeconfig) voor de clusterbeheerders en gebruikers van de cluster
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 05/31/2019
ms.author: mlearned
ms.openlocfilehash: cbc653b86ed83f9d6a7348d39f51dc7cd49c6892
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2019
ms.locfileid: "67615677"
---
# <a name="use-azure-role-based-access-controls-to-define-access-to-the-kubernetes-configuration-file-in-azure-kubernetes-service-aks"></a>Azure op rollen gebaseerd toegangsbeheer gebruiken om te definiëren de toegang tot het Kubernetes-configuratiebestand in Azure Kubernetes Service (AKS)

U kunt werken met Kubernetes-clusters met behulp van de `kubectl` hulpprogramma. De Opdrachtregelinterface van Azure biedt een eenvoudige manier om op te halen van de referenties voor toegang en informatie over verbinding maken met uw AKS-clusters met `kubectl`. Om te beperken wie die Kubernetes-configuratie krijgt (*kubeconfig*) informatie en om te beperken welke machtigingen ze vervolgens hebt, kunt u Azure op rollen gebaseerd toegangsbeheer (RBAC).

In dit artikel wordt beschreven hoe u RBAC-rollen toewijzen die beperken aan wie de configuratiegegevens voor een AKS-cluster kunt krijgen.

## <a name="before-you-begin"></a>Voordat u begint

In dit artikel wordt ervan uitgegaan dat u een bestaand AKS-cluster hebt. Als u een cluster AKS nodig hebt, raadpleegt u de Quick Start voor AKS [met de Azure CLI][aks-quickstart-cli] or [using the Azure portal][aks-quickstart-portal].

In dit artikel is ook vereist dat u de Azure CLI versie 2.0.65 worden uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Zie [Azure CLI installeren][azure-cli-install] als u de CLI wilt installeren of een upgrade wilt uitvoeren.

## <a name="available-cluster-roles-permissions"></a>Rolmachtigingen beschikbare cluster

Wanneer u communiceert met een AKS-cluster met de `kubectl` hulpprogramma, een configuratiebestand waarin de gegevens wordt gebruikt. Dit configuratiebestand worden meestal opgeslagen in *~/.kube/config*. Meerdere clusters kunnen worden gedefinieerd in dit *kubeconfig* bestand. U schakelt tussen clusters met behulp van de [kubectl config gebruik context][kubectl-config-use-context] opdracht.

De [az aks get-credentials][az-aks-get-credentials] opdracht kunt u de toegangsreferenties te verkrijgen voor een AKS-cluster en voegt u deze naar de *kubeconfig* bestand. U kunt Azure op rollen gebaseerd toegangsbeheer (RBAC) gebruiken voor het beheren van toegang tot deze referenties. Deze Azure RBAC-rollen kunnen u definiëren die kunnen worden opgehaald de *kubeconfig* bestands- en welke machtigingen ze vervolgens binnen het cluster hebben.

De twee ingebouwde rollen zijn:

* **Beheerdersrol voor Azure Kubernetes Service-Cluster**  
    * Toegang tot de *Microsoft.ContainerService/managedClusters/listClusterAdminCredential/action* API-aanroep. Deze API-aanroep [geeft een lijst van de cluster-beheerdersreferenties][api-cluster-admin].
    * Downloads *kubeconfig* voor de *clusterAdmin* rol.
* **Azure Kubernetes Service-Cluster-gebruikersrol**
    * Toegang tot de *Microsoft.ContainerService/managedClusters/listClusterUserCredential/action* API-aanroep. Deze API-aanroep [geeft een lijst van de referenties van de cluster-gebruiker][api-cluster-user].
    * Downloads *kubeconfig* voor *clusterUser* rol.

Deze RBAC-rollen kunnen worden toegepast op een Azure Active Directory (AD)-gebruiker of groep.

## <a name="assign-role-permissions-to-a-user-or-group"></a>Rolmachtigingen toewijzen aan een gebruiker of groep

Als u wilt toewijzen aan een van de beschikbare rollen, moet u de resource-ID van het AKS-cluster en de ID van de Azure AD-gebruikersaccount of de groep. De volgende voorbeeldopdrachten:

* De cluster resource ID met behulp van de [az aks show][az-aks-show] opdracht voor het cluster met de naam *myAKSCluster* in de *myResourceGroup* resourcegroep. Geef de naam van uw eigen cluster en de resource-groep, zoals die nodig zijn.
* Maakt gebruik van de [az account show][az-account-show] and [az ad user show][az-ad-user-show] opdrachten voor het ophalen van uw gebruikers-ID.
* Ten slotte wijst een rol met de [az roltoewijzing maken][az-role-assignment-create] opdracht.

In het volgende voorbeeld wordt de *beheerdersrol voor Azure Kubernetes Service-Cluster* aan het account van een afzonderlijke gebruiker:

```azurecli-interactive
# Get the resource ID of your AKS cluster
AKS_CLUSTER=$(az aks show --resource-group myResourceGroup --name myAKSCluster --query id -o tsv)

# Get the account credentials for the logged in user
ACCOUNT_UPN=$(az account show --query user.name -o tsv)
ACCOUNT_ID=$(az ad user show --upn-or-object-id $ACCOUNT_UPN --query objectId -o tsv)

# Assign the 'Cluster Admin' role to the user
az role assignment create \
    --assignee $ACCOUNT_ID \
    --scope $AKS_CLUSTER \
    --role "Azure Kubernetes Service Cluster Admin Role"
```

> [!TIP]
> Als u machtigingen toewijzen aan een Azure AD-groep wilt, werkt de `--assignee` parameter die wordt weergegeven in het vorige voorbeeld met de object-ID voor de *groep* in plaats van een *gebruiker*. Voor de object-ID voor een groep verkrijgen, gebruikt u de [az ad group show][az-ad-group-show] opdracht. Het volgende voorbeeld wordt de object-ID voor de Azure AD-groep met de naam *appdev*: `az ad group show --group appdev --query objectId -o tsv`

Kunt u de vorige toewijzing aan de *Cluster gebruikersrol* indien nodig.

De volgende voorbeelduitvoer ziet u dat de roltoewijzing is gemaakt:

```
{
  "canDelegate": null,
  "id": "/subscriptions/<guid>/resourcegroups/myResourceGroup/providers/Microsoft.ContainerService/managedClusters/myAKSCluster/providers/Microsoft.Authorization/roleAssignments/b2712174-5a41-4ecb-82c5-12b8ad43d4fb",
  "name": "b2712174-5a41-4ecb-82c5-12b8ad43d4fb",
  "principalId": "946016dd-9362-4183-b17d-4c416d1f8f61",
  "resourceGroup": "myResourceGroup",
  "roleDefinitionId": "/subscriptions/<guid>/providers/Microsoft.Authorization/roleDefinitions/0ab01a8-8aac-4efd-b8c2-3ee1fb270be8",
  "scope": "/subscriptions/<guid>/resourcegroups/myResourceGroup/providers/Microsoft.ContainerService/managedClusters/myAKSCluster",
  "type": "Microsoft.Authorization/roleAssignments"
}
```

## <a name="get-and-verify-the-configuration-information"></a>Krijgen te controleren of de configuratie-informatie

Met RBAC-rollen toegewezen, gebruiken de [az aks get-credentials][az-aks-get-credentials] -opdracht voor het *kubeconfig* definitie voor uw AKS-cluster. Het volgende voorbeeld wordt de *--admin* referenties, dat goed werkt als de gebruiker zijn verleend de *Cluster beheerdersrol*:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster --admin
```

Vervolgens kunt u de [kubectl config weergave][kubectl-config-view] opdracht uit om te controleren of de *context* voor het cluster ziet u dat de informatie over de configuratie van de beheerder is toegepast:

```
$ kubectl config view

apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: DATA+OMITTED
    server: https://myaksclust-myresourcegroup-19da35-4839be06.hcp.eastus.azmk8s.io:443
  name: myAKSCluster
contexts:
- context:
    cluster: myAKSCluster
    user: clusterAdmin_myResourceGroup_myAKSCluster
  name: myAKSCluster-admin
current-context: myAKSCluster-admin
kind: Config
preferences: {}
users:
- name: clusterAdmin_myResourceGroup_myAKSCluster
  user:
    client-certificate-data: REDACTED
    client-key-data: REDACTED
    token: e9f2f819a4496538b02cefff94e61d35
```

## <a name="remove-role-permissions"></a>Machtigingen van de rol verwijderen

Als u wilt verwijderen van roltoewijzingen, gebruikt u de [az-roltoewijzing verwijderen][az-role-assignment-delete] opdracht. Geef de account-ID en cluster resource-ID, die zijn verkregen in de voorgaande opdrachten. Als u de rol aan een groep in plaats van een gebruiker toegewezen, geeft u de juiste group-object-ID in plaats van object-ID voor de `--assignee` parameter:

```azurecli-interactive
az role assignment delete --assignee $ACCOUNT_ID --scope $AKS_CLUSTER
```

## <a name="next-steps"></a>Volgende stappen

Voor een betere beveiliging van toegang tot de AKS-clusters, [Azure Active Directory-verificatie integreren][aad-integration].

<!-- LINKS - external -->
[kubectl-config-use-context]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#config
[kubectl-config-view]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#config

<!-- LINKS - internal -->
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-get-credentials]: /cli/azure/aks#az-aks-get-credentials
[azure-rbac]: ../role-based-access-control/overview.md
[api-cluster-admin]: /rest/api/aks/managedclusters/listclusteradmincredentials
[api-cluster-user]: /rest/api/aks/managedclusters/listclusterusercredentials
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-account-show]: /cli/azure/account#az-account-show
[az-ad-user-show]: /cli/azure/ad/user#az-ad-user-show
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[az-role-assignment-delete]: /cli/azure/role/assignment#az-role-assignment-delete
[aad-integration]: azure-ad-integration.md
[az-ad-group-show]: /cli/azure/ad/group#az-ad-group-show
