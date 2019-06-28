---
title: Een interne load balancer maken in Azure Kubernetes Service (AKS)
description: Informatie over het maken en gebruiken van een interne load balancer om uw services met Azure Kubernetes Service (AKS) zichtbaar te maken.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 03/04/2019
ms.author: iainfou
ms.openlocfilehash: 50a2161be4ee70f7ae5c8baa3816eb9f9943a5d2
ms.sourcegitcommit: a7ea412ca4411fc28431cbe7d2cc399900267585
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2019
ms.locfileid: "67358008"
---
# <a name="use-an-internal-load-balancer-with-azure-kubernetes-service-aks"></a>Een interne load balancer gebruiken met Azure Kubernetes Service (AKS)

Om toegang te beperken tot uw toepassingen in Azure Kubernetes Service (AKS), kunt u maken en gebruiken van een interne load balancer. Een interne load balancer maakt een Kubernetes-service alleen toegankelijk is voor toepassingen die worden uitgevoerd in hetzelfde virtuele netwerk bevinden als het Kubernetes-cluster. Dit artikel leest u hoe het maken en gebruiken van een interne load balancer met Azure Kubernetes Service (AKS).

> [!NOTE]
> Azure Load Balancer is beschikbaar in twee SKU's - *Basic* en *Standard*. Standaard de *Basic* SKU wordt gebruikt wanneer een servicemanifest wordt gebruikt voor het maken van een load balancer in AKS. Zie voor meer informatie, [vergelijking van Azure load balancer SKU][azure-lb-comparison].

## <a name="before-you-begin"></a>Voordat u begint

In dit artikel wordt ervan uitgegaan dat u een bestaand AKS-cluster hebt. Als u een cluster AKS nodig hebt, raadpleegt u de Quick Start voor AKS [met de Azure CLI][aks-quickstart-cli] or [using the Azure portal][aks-quickstart-portal].

U ook moet de Azure CLI versie 2.0.59 of later geïnstalleerd en geconfigureerd. Voer  `az --version` uit om de versie te bekijken. Als u wilt installeren of upgraden, Zie [Azure CLI installeren][install-azure-cli].

De AKS-clusterservice-principal moet de machtiging voor het beheren van netwerkbronnen als u een bestaand subnet of de resourcegroep. In het algemeen toewijzen de *Inzender voor netwerken* rol aan uw service-principal op de gedeelde resources. Zie voor meer informatie over machtigingen [AKS gemachtigde toegang tot andere Azure-resources][aks-sp].

## <a name="create-an-internal-load-balancer"></a>Een interne load balancer maken

Voor het maken van een interne load balancer, maakt u een servicemanifest met de naam `internal-lb.yaml` met het servicetype *LoadBalancer* en de *azure-load-balancer-interne* aantekening zoals wordt weergegeven in de volgende Voorbeeld:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: internal-app
  annotations:
    service.beta.kubernetes.io/azure-load-balancer-internal: "true"
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: internal-app
```

Implementatie van de interne load balancer met behulp van de [kubectl toepassen][kubectl-apply] en geef de naam van uw YAML-manifest:

```console
kubectl apply -f internal-lb.yaml
```

Een Azure load balancer is gemaakt in de resourcegroep van het knooppunt en verbonden met hetzelfde virtuele netwerk bevinden als het AKS-cluster.

Wanneer u de details van de service bekijkt, het IP-adres van de interne load balancer wordt weergegeven in de *externe IP-adres* kolom. In deze context *externe* zich ten opzichte van de externe interface van de load balancer, niet dat het een openbare, externe IP-adres ontvangt. Het duurt een minuut of twee voor het IP-adres te wijzigen van *\<in behandeling\>* naar een werkelijke intern IP-adres, zoals weergegeven in het volgende voorbeeld:

```
$ kubectl get service internal-app

NAME           TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
internal-app   LoadBalancer   10.0.248.59   10.240.0.7    80:30555/TCP   2m
```

## <a name="specify-an-ip-address"></a>Geef een IP-adres

Als u gebruiken van een specifiek IP-adres met de interne load balancer wilt, voegt u toe de *loadBalancerIP* eigenschap in op de load balancer YAML-manifest. Het opgegeven IP-adres moet zich bevinden in hetzelfde subnet als het AKS-cluster en moet niet worden toegewezen aan een resource.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: internal-app
  annotations:
    service.beta.kubernetes.io/azure-load-balancer-internal: "true"
spec:
  type: LoadBalancer
  loadBalancerIP: 10.240.0.25
  ports:
  - port: 80
  selector:
    app: internal-app
```

Bij de implementatie en weergeven van details van de service, het IP-adres in de *externe IP-adres* kolom geeft de opgegeven IP-adres:

```
$ kubectl get service internal-app

NAME           TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
internal-app   LoadBalancer   10.0.184.168   10.240.0.25   80:30225/TCP   4m
```

## <a name="use-private-networks"></a>Particuliere netwerken gebruikt

Wanneer u uw AKS-cluster maakt, kunt u geavanceerde netwerkinstellingen. Deze aanpak kunt u het cluster implementeren in een bestaande Azure-netwerk en subnetten. Een scenario is het implementeren van uw AKS-cluster in een particulier netwerk verbonden met uw on-premises omgeving en services die alleen toegankelijk zijn intern uitvoeren. Zie voor meer informatie, configureren van uw eigen virtuele subnetten met [Kubenet][use-kubenet] or [Azure CNI][advanced-networking].

Geen wijzigingen aangebracht in de vorige stappen zijn nodig voor het implementeren van een interne load balancer in een AKS-cluster dat gebruik maakt van een particulier netwerk. De load balancer is gemaakt in dezelfde resourcegroep bevinden als uw AKS-cluster, maar verbonden met uw persoonlijke virtueel netwerk en subnet, zoals wordt weergegeven in het volgende voorbeeld:

```
$ kubectl get service internal-app

NAME           TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
internal-app   LoadBalancer   10.1.15.188   10.0.0.35     80:31669/TCP   1m
```

> [!NOTE]
> Mogelijk moet u de service-principal voor uw AKS-cluster verlenen de *Inzender voor netwerken* rol aan de resourcegroep waar uw virtuele netwerk van Azure-resources worden geïmplementeerd. Weergeven van de service-principal met [az aks show][az-aks-show], zoals `az aks show --resource-group myResourceGroup --name myAKSCluster --query "servicePrincipalProfile.clientId"`. Gebruik voor het maken van een roltoewijzing de [az roltoewijzing maken][az-role-assignment-create] opdracht.

## <a name="specify-a-different-subnet"></a>Geef een ander subnet

Als u een subnet voor de load balancer, voeg de *azure-load-balancer-interne-subnet* aantekening met uw service. Het opgegeven subnet moet zich in hetzelfde virtuele netwerk bevinden als uw AKS-cluster. Bij de implementatie door de load balancer *externe IP-adres* adres deel uitmaakt van het opgegeven subnet.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: internal-app
  annotations:
    service.beta.kubernetes.io/azure-load-balancer-internal: "true"
    service.beta.kubernetes.io/azure-load-balancer-internal-subnet: "apps-subnet"
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: internal-app
```

## <a name="delete-the-load-balancer"></a>De load balancer verwijderen

Wanneer alle services die gebruikmaken van de interne load balancer zijn verwijderd, wordt de load balancer zelf ook verwijderd.

U kunt ook rechtstreeks een service als met een Kubernetes-resource, zoals verwijderen `kubectl delete service internal-app`, die dan ook worden de onderliggende Azure load balancer verwijderd.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over Kubernetes-services op de [documentatie voor Kubernetes services][kubernetes-services].

<!-- LINKS - External -->
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubernetes-services]: https://kubernetes.io/docs/concepts/services-networking/service/
[aks-engine]: https://github.com/Azure/aks-engine

<!-- LINKS - Internal -->
[advanced-networking]: configure-azure-cni.md
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[azure-lb-comparison]: ../load-balancer/load-balancer-overview.md#skus
[use-kubenet]: configure-kubenet.md
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
[aks-sp]: kubernetes-service-principal.md#delegate-access-to-other-azure-resources
