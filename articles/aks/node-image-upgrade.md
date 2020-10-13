---
title: Installatie kopieën van het AKS-knoop punt (Azure Kubernetes service) upgraden
description: Meer informatie over het bijwerken van de installatie kopieën op AKS-cluster knooppunten en-knooppunt groepen.
author: laurenhughes
ms.author: lahugh
ms.service: container-service
ms.topic: conceptual
ms.date: 08/17/2020
ms.openlocfilehash: 744e62f8a2207cff400a96069fc6ea82866f6e2d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "89055682"
---
# <a name="azure-kubernetes-service-aks-node-image-upgrade"></a>Upgrade van installatie kopie van knoop punt Azure Kubernetes service (AKS)

AKS biedt ondersteuning voor het upgraden van installatie kopieën op een knoop punt zodat u up-to-date bent met de nieuwste besturings systemen en runtime-updates. AKS biedt een nieuwe installatie kopie per week met de nieuwste updates. het is dus handig om de installatie kopieën van uw knoop punt regel matig bij te werken voor de nieuwste functies, waaronder Linux-of Windows-patches. In dit artikel wordt beschreven hoe u installatie kopieën van AKS-cluster knooppunten bijwerkt en hoe u installatie kopieën van groeps knooppunten bijwerkt zonder de versie van Kubernetes bij te werken.

Als u wilt weten over de nieuwste installatie kopieën van AKS, raadpleegt u de [opmerkingen](https://github.com/Azure/AKS/releases) bij de release van AKS voor meer informatie.

Zie [een AKS-cluster upgraden][upgrade-cluster]voor informatie over het bijwerken van de Kubernetes-versie voor uw cluster.

## <a name="limitations"></a>Beperkingen

* Het AKS-cluster moet virtuele-machine schaal sets gebruiken voor de knoop punten.

## <a name="install-the-aks-cli-extension"></a>De AKS CLI-extensie installeren

Voordat de volgende kern CLI-versie wordt uitgebracht, hebt u de *AKS-preview cli-* extensie nodig om de upgrade van de knooppunt installatie kopie te gebruiken. Gebruik de opdracht [AZ extension add][az-extension-add] en controleer vervolgens of er beschik bare updates zijn met behulp van de opdracht [AZ extension update][az-extension-update] :

```azurecli
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```

## <a name="upgrade-all-nodes-in-all-node-pools"></a>Alle knoop punten in alle knooppunt groepen upgraden

De upgrade van de knooppunt installatie kopie wordt uitgevoerd met `az aks upgrade` . Als u de installatie kopie van het knoop punt wilt bijwerken, gebruikt u de volgende opdracht:

```azurecli
az aks upgrade \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-image-only
```

Controleer tijdens de upgrade de status van de installatie kopie van het knoop punt met de volgende `kubectl` opdracht om de labels op te halen en de gegevens van de huidige knooppunt afbeelding te filteren:

```azurecli
kubectl get nodes -o jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.metadata.labels.kubernetes\.azure\.com\/node-image-version}{"\n"}{end}'
```

Wanneer de upgrade is voltooid, gebruikt `az aks show` u om de bijgewerkte Details van de knooppunt groep op te halen. De huidige knooppunt afbeelding wordt weer gegeven in de `nodeImageVersion` eigenschap.

```azurecli
az aks show \
    --resource-group myResourceGroup \
    --name myAKSCluster
```

## <a name="upgrade-a-specific-node-pool"></a>Een specifieke knooppunt groep upgraden

Het bijwerken van de installatie kopie op een knooppunt groep is vergelijkbaar met het bijwerken van de installatie kopie in een cluster.

Als u de installatie kopie van het besturings systeem van de knooppunt groep wilt bijwerken zonder een Kubernetes-cluster upgrade uit te voeren, gebruikt u de `--node-image-only` optie in het volgende voor beeld:

```azurecli
az aks nodepool upgrade \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool \
    --node-image-only
```

Controleer tijdens de upgrade de status van de installatie kopie van het knoop punt met de volgende `kubectl` opdracht om de labels op te halen en de gegevens van de huidige knooppunt afbeelding te filteren:

```azurecli
kubectl get nodes -o jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.metadata.labels.kubernetes\.azure\.com\/node-image-version}{"\n"}{end}'
```

Wanneer de upgrade is voltooid, gebruikt `az aks nodepool show` u om de bijgewerkte Details van de knooppunt groep op te halen. De huidige knooppunt afbeelding wordt weer gegeven in de `nodeImageVersion` eigenschap.

```azurecli
az aks nodepool show \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool
```

## <a name="upgrade-node-images-with-node-surge"></a>Knooppunt installatie kopieën bijwerken met overspanning van knoop punt

Als u het upgrade proces van de knooppunt installatie kopie wilt versnellen, kunt u uw knooppunt installatie kopieën bijwerken met behulp van een aanpas bare overspannings waarde. AKS gebruikt standaard één extra knoop punt voor het configureren van upgrades.

Als u de snelheid van upgrades wilt verhogen, gebruikt u de `--max-surge` waarde voor het configureren van het aantal knoop punten dat moet worden gebruikt voor upgrades zodat ze sneller kunnen worden uitgevoerd. Zie de upgrade van het `--max-surge` [knoop punt piek aanpassen][max-surge]voor meer informatie over de verschillende instellingen.

Met de volgende opdracht stelt u de maximale piek waarde in voor het uitvoeren van een upgrade van een knooppunt installatie kopie:

```azurecli
az aks nodepool upgrade \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool \
    --max-surge 33% \
    --node-image-only \
    --no-wait
```

Controleer tijdens de upgrade de status van de installatie kopie van het knoop punt met de volgende `kubectl` opdracht om de labels op te halen en de gegevens van de huidige knooppunt afbeelding te filteren:

```azurecli
kubectl get nodes -o jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.metadata.labels.kubernetes\.azure\.com\/node-image-version}{"\n"}{end}'
```

Gebruiken `az aks nodepool show` om de bijgewerkte Details van de knooppunt groep op te halen. De huidige knooppunt afbeelding wordt weer gegeven in de `nodeImageVersion` eigenschap.

```azurecli
az aks nodepool show \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool
```

## <a name="next-steps"></a>Volgende stappen

- Zie de [opmerkingen](https://github.com/Azure/AKS/releases) bij de release van AKS voor informatie over de nieuwste knooppunt installatie kopieën.
- Meer informatie over het bijwerken van de Kubernetes-versie met [een upgrade voor een AKS-cluster][upgrade-cluster].
- [Beveiligings-en kernel-updates Toep assen op Linux-knoop punten in azure Kubernetes service (AKS)][security-update]
- Meer informatie over meerdere knooppunt Pools en het upgraden van knooppunt Pools met het [maken en beheren van meerdere knooppunt groepen][use-multiple-node-pools].

<!-- LINKS - internal -->
[upgrade-cluster]: upgrade-cluster.md
[security-update]: node-updates-kured.md
[use-multiple-node-pools]: use-multiple-node-pools.md
[max-surge]: upgrade-cluster.md#customize-node-surge-upgrade-preview
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
