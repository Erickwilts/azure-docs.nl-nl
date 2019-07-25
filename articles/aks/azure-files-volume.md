---
title: Een statisch volume maken voor meerdere peulen in azure Kubernetes service (AKS)
description: Meer informatie over het hand matig maken van een volume met Azure Files voor gebruik met meerdere gelijktijdige peulen in azure Kubernetes service (AKS)
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 03/01/2019
ms.author: mlearned
ms.openlocfilehash: ad80b738058b4048fa1a51144a37eb4f62b538c0
ms.sourcegitcommit: bafb70af41ad1326adf3b7f8db50493e20a64926
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/25/2019
ms.locfileid: "67616025"
---
# <a name="manually-create-and-use-a-volume-with-azure-files-share-in-azure-kubernetes-service-aks"></a>Hand matig een volume maken en gebruiken met Azure Files share in azure Kubernetes service (AKS)

Op containers gebaseerde toepassingen moeten vaak toegang hebben tot gegevens en deze persistent maken in een extern gegevens volume. Als meerdere peulen gelijktijdig gelijktijdige toegang tot hetzelfde opslag volume nodig hebben, kunt u Azure Files gebruiken om verbinding te maken met het [SMB-protocol (Server Message Block)][smb-overview]. In dit artikel wordt beschreven hoe u hand matig een Azure Files share maakt en deze koppelt aan een pod in AKS.

Zie [opslag opties voor toepassingen in AKS][concepts-storage]voor meer informatie over Kubernetes-volumes.

## <a name="before-you-begin"></a>Voordat u begint

In dit artikel wordt ervan uitgegaan dat u beschikt over een bestaand AKS-cluster. Als u een AKS-cluster nodig hebt, raadpleegt u de AKS Quick Start [met behulp van de Azure cli][aks-quickstart-cli] of [met behulp van de Azure Portal][aks-quickstart-portal].

Ook moet de Azure CLI-versie 2.0.59 of hoger zijn geïnstalleerd en geconfigureerd. Voer  `az --version` uit om de versie te bekijken. Als u wilt installeren of upgraden, raadpleegt u [Azure cli installeren][install-azure-cli].

## <a name="create-an-azure-file-share"></a>Een Azure-bestandsshare maken

Voordat u Azure Files als een Kubernetes-volume kunt gebruiken, moet u een Azure Storage-account en de bestands share maken. Met de volgende opdrachten maakt u een resource groep met de naam *myAKSShare*, een opslag account en een bestands share met de naam *aksshare*:

```azurecli-interactive
# Change these four parameters as needed for your own environment
AKS_PERS_STORAGE_ACCOUNT_NAME=mystorageaccount$RANDOM
AKS_PERS_RESOURCE_GROUP=myAKSShare
AKS_PERS_LOCATION=eastus
AKS_PERS_SHARE_NAME=aksshare

# Create a resource group
az group create --name $AKS_PERS_RESOURCE_GROUP --location $AKS_PERS_LOCATION

# Create a storage account
az storage account create -n $AKS_PERS_STORAGE_ACCOUNT_NAME -g $AKS_PERS_RESOURCE_GROUP -l $AKS_PERS_LOCATION --sku Standard_LRS

# Export the connection string as an environment variable, this is used when creating the Azure file share
export AZURE_STORAGE_CONNECTION_STRING=`az storage account show-connection-string -n $AKS_PERS_STORAGE_ACCOUNT_NAME -g $AKS_PERS_RESOURCE_GROUP -o tsv`

# Create the file share
az storage share create -n $AKS_PERS_SHARE_NAME --connection-string $AZURE_STORAGE_CONNECTION_STRING

# Get storage account key
STORAGE_KEY=$(az storage account keys list --resource-group $AKS_PERS_RESOURCE_GROUP --account-name $AKS_PERS_STORAGE_ACCOUNT_NAME --query "[0].value" -o tsv)

# Echo storage account name and key
echo Storage account name: $AKS_PERS_STORAGE_ACCOUNT_NAME
echo Storage account key: $STORAGE_KEY
```

Noteer de naam van het opslag account en de sleutel die aan het einde van de script uitvoer wordt weer gegeven. Deze waarden zijn nodig wanneer u het Kubernetes-volume maakt in een van de volgende stappen.

## <a name="create-a-kubernetes-secret"></a>Een Kubernetes-geheim maken

Kubernetes heeft referenties nodig om toegang te krijgen tot de bestands share die u in de vorige stap hebt gemaakt. Deze referenties worden opgeslagen in een [Kubernetes-geheim][kubernetes-secret], waarnaar wordt verwezen bij het maken van een Kubernetes-pod.

Gebruik de `kubectl create secret` opdracht om het geheim te maken. In het volgende voor beeld wordt een gedeeld *Azure-geheim* gemaakt en worden de *azurestorageaccountname* en *azurestorageaccountkey* van de vorige stap gevuld. Als u een bestaand Azure Storage-account wilt gebruiken, geeft u de account naam en de sleutel op.

```console
kubectl create secret generic azure-secret --from-literal=azurestorageaccountname=$AKS_PERS_STORAGE_ACCOUNT_NAME --from-literal=azurestorageaccountkey=$STORAGE_KEY
```

## <a name="mount-the-file-share-as-a-volume"></a>De bestands share koppelen als een volume

Als u de Azure Files share in uw Pod wilt koppelen, configureert u het volume in de container specificatie. Maak een nieuw bestand met `azure-files-pod.yaml` de naam met de volgende inhoud. Als u de naam van de bestands share of geheime naam hebt gewijzigd, werkt u de *sharename* en de *secretnaam*bij. Als dat gewenst is, `mountPath`werkt u de, die het pad is naar de bestands share in de pod. Voor Windows Server-containers (momenteel in de preview-versie van AKS) geeft u een *mountPath* op met behulp van de Windows Path-Conventie, zoals *: '* .

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
  - image: nginx:1.15.5
    name: mypod
    resources:
      requests:
        cpu: 100m
        memory: 128Mi
      limits:
        cpu: 250m
        memory: 256Mi
    volumeMounts:
      - name: azure
        mountPath: /mnt/azure
  volumes:
  - name: azure
    azureFile:
      secretName: azure-secret
      shareName: aksshare
      readOnly: false
```

Gebruik de `kubectl` opdracht om de Pod te maken.

```console
kubectl apply -f azure-files-pod.yaml
```

U hebt nu een actieve pod met een Azure Files-share gekoppeld op */mnt/Azure*. U kunt gebruiken `kubectl describe pod mypod` om te controleren of de share is gekoppeld. De volgende gecomprimeerde voorbeeld uitvoer toont het volume dat in de container is gekoppeld:

```
Containers:
  mypod:
    Container ID:   docker://86d244cfc7c4822401e88f55fd75217d213aa9c3c6a3df169e76e8e25ed28166
    Image:          nginx:1.15.5
    Image ID:       docker-pullable://nginx@sha256:9ad0746d8f2ea6df3a17ba89eca40b48c47066dfab55a75e08e2b70fc80d929e
    State:          Running
      Started:      Sat, 02 Mar 2019 00:05:47 +0000
    Ready:          True
    Mounts:
      /mnt/azure from azure (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-z5sd7 (ro)
[...]
Volumes:
  azure:
    Type:        AzureFile (an Azure File Service mount on the host and bind mount to the pod)
    SecretName:  azure-secret
    ShareName:   aksshare
    ReadOnly:    false
  default-token-z5sd7:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-z5sd7
[...]
```

## <a name="mount-options"></a>Koppelingsopties

Standaard waarden voor *file mode* en *DirMode* verschillen van Kubernetes-versies, zoals beschreven in de volgende tabel.

| version | value |
| ---- | ---- |
| v1.6.x, v1.7.x | 0777 |
| v1.8.0-v1.8.5 | 0700 |
| v 1.8.6 of hoger | 0755 |
| v1.9.0 | 0700 |
| v 1.9.1 of hoger | 0755 |

Als u een cluster van versie 1.8.5 of hoger gebruikt en het permanente volume statisch maakt, moet u de koppelings opties opgeven voor het *PersistentVolume* -object.

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: azurefile
spec:
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteMany
  azureFile:
    secretName: azure-secret
    shareName: azurefile
    readOnly: false
  mountOptions:
  - dir_mode=0777
  - file_mode=0777
  - uid=1000
  - gid=1000
```

Als u een cluster van versie 1.8.0-1.8.4 gebruikt, kan een beveiligings context worden opgegeven met de *runAsUser* -waarde ingesteld op *0*. Zie [Configure a security context][kubernetes-security-context](Engelstalig) voor meer informatie over pod-beveiligings context.

## <a name="next-steps"></a>Volgende stappen

Zie [Aanbevolen procedures voor opslag en back-ups in AKS][operator-best-practices-storage]voor gekoppelde aanbevolen procedures.

Zie de [Kubernetes-invoeg toepassing voor Azure files][kubernetes-files]voor meer informatie over AKS-clusters die communiceren met Azure files.

<!-- LINKS - external -->
[kubectl-create]: https://kubernetes.io/docs/user-guide/kubectl/v1.8/#create
[kubernetes-files]: https://github.com/kubernetes/examples/blob/master/staging/volumes/azure_file/README.md
[kubernetes-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/volumes/
[smb-overview]: /windows/desktop/FileIO/microsoft-smb-protocol-and-cifs-protocol-overview
[kubernetes-security-context]: https://kubernetes.io/docs/tasks/configure-pod-container/security-context/

<!-- LINKS - internal -->
[az-group-create]: /cli/azure/group#az-group-create
[az-storage-create]: /cli/azure/storage/account#az-storage-account-create
[az-storage-key-list]: /cli/azure/storage/account/keys#az-storage-account-keys-list
[az-storage-share-create]: /cli/azure/storage/share#az-storage-share-create
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
[operator-best-practices-storage]: operator-best-practices-storage.md
[concepts-storage]: concepts-storage.md
