---
title: 'Zelfstudie voor Kubernetes op Azure: Toepassing schalen'
description: In deze zelfstudie over Azure Kubernetes Service (AKS) leert u hoe u knooppunten en pods schaalt in Kubernetes. Er wordt ook aandacht besteed aan het automatisch horizontaal schalen van pods.
services: container-service
author: mlearned
ms.service: container-service
ms.topic: tutorial
ms.date: 12/19/2018
ms.author: mlearned
ms.custom: mvc
ms.openlocfilehash: 5a942aa10f36df55ac232defa610102700e3995b
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/12/2019
ms.locfileid: "67614202"
---
# <a name="tutorial-scale-applications-in-azure-kubernetes-service-aks"></a>Zelfstudie: toepassingen schalen in AKS (Azure Kubernetes Service)

Als u de zelfstudies hebt gevolgd, hebt u een werkend Kubernetes-cluster in AKS en hebt u de Azure Voting-voorbeeldapp geïmplementeerd. In deze zelfstudie, deel vijf van zeven, schaalt u de schillen in de app en probeert u automatisch schalen van schillen uit. U leert ook hoe u het aantal Azure VM-knooppunten schaalt om de capaciteit van het cluster voor het hosten van werkbelastingen te wijzigen. In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * Kubernetes-knooppunten schalen
> * Handmatig Kubernetes-pods schalen waarop uw toepassing wordt uitgevoerd
> * Automatisch schalen configureren van pods die de front-end-app uitvoeren

In aanvullende zelfstudies wordt de Azure Vote-toepassing bijgewerkt naar een nieuwe versie.

## <a name="before-you-begin"></a>Voordat u begint

In eerdere zelfstudies is een toepassing verpakt in een containerinstallatiekopie. Deze installatiekopie is geüpload naar Azure Container Registry en u hebt een AKS-cluster gemaakt. De toepassing is vervolgens geïmplementeerd in het AKS-cluster. Als u deze stappen niet hebt uitgevoerd en wilt door gaan met de [zelf studie 1: container installatie kopieën maken][aks-tutorial-prepare-app].

Voor deze zelfstudie moet u Azure CLI versie 2.0.53 of hoger uitvoeren. Voer `az --version` uit om de versie te bekijken. Zie [Azure CLI installeren][azure-cli-install] als u de CLI wilt installeren of een upgrade wilt uitvoeren.

## <a name="manually-scale-pods"></a>Schillen handmatig schalen

Bij de implementatie van de front-end van Azure Vote en het Redis-exemplaar in eerdere zelfstudies, is er één replica gemaakt. Als u het aantal en de status van de peul in uw cluster wilt zien, gebruikt u de opdracht [kubectl Get][kubectl-get] als volgt:

```console
kubectl get pods
```

In het volgende voorbeeld van uitvoer ziet u één pod voor de front-end en één voor de back-end:

```
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-2549686872-4d2r5   1/1       Running   0          31m
azure-vote-front-848767080-tf34m   1/1       Running   0          31m
```

Als u het aantal peulen voor de implementatie van *Azure-stemmen-front* hand matig wilt wijzigen, gebruikt u de opdracht [kubectl Scale][kubectl-scale] . In het volgende voorbeeld wordt het aantal pods voor de front-end verhoogd naar *5*:

```console
kubectl scale --replicas=5 deployment/azure-vote-front
```

Voer [kubectl][kubectl-get] opnieuw uit om te controleren of Aks het nieuwe Peul maakt. Na ongeveer een minuut zijn de extra pods beschikbaar in uw cluster:

```console
$ kubectl get pods

                                    READY     STATUS    RESTARTS   AGE
azure-vote-back-2606967446-nmpcf    1/1       Running   0          15m
azure-vote-front-3309479140-2hfh0   1/1       Running   0          3m
azure-vote-front-3309479140-bzt05   1/1       Running   0          3m
azure-vote-front-3309479140-fvcvm   1/1       Running   0          3m
azure-vote-front-3309479140-hrbf2   1/1       Running   0          15m
azure-vote-front-3309479140-qphz8   1/1       Running   0          3m
```

## <a name="autoscale-pods"></a>Schillen automatisch schalen

Kubernetes biedt ondersteuning voor het [automatisch horizontaal schalen van schillen][kubernetes-hpa] om zo het aantal schillen in een implementatie aan te passen op basis van het CPU-gebruik of andere geselecteerde metrische gegevens. De [Metrics-server][metrics-server] wordt gebruikt om het resource gebruik te voorzien van Kubernetes en wordt automatisch GEÏMPLEMENTEERD in AKS-cluster versies 1,10 en hoger. Als u de versie van uw AKS-cluster wilt zien, gebruikt u de opdracht [AZ AKS show][az-aks-show] , zoals wordt weer gegeven in het volgende voor beeld:

```azurecli
az aks show --resource-group myResourceGroup --name myAKSCluster --query kubernetesVersion
```

Als uw AKS-cluster een lagere versie dan *1.10* heeft, installeert u de Metrics Server. Anders kunt u deze stap overslaan. Om deze te installeren, kloont u de GitHub-opslagplaats `metrics-server` en installeert u de voorbeelden van resourcedefinities. Als u de inhoud van deze YAML definities wilt weer geven, raadpleegt u [Metrics server voor Kuberenetes 1.8 +][metrics-server-github].

```console
git clone https://github.com/kubernetes-incubator/metrics-server.git
kubectl create -f metrics-server/deploy/1.8+/
```

Als u de automatische schaal functie wilt gebruiken, moeten voor alle containers in uw peul en uw peul CPU-aanvragen en-limieten zijn gedefinieerd. In de `azure-vote-front`-implementatie vraagt de front-end container al om 0,25 CPU, met een limiet van 0,5 CPU. Deze resourceaanvragen en -limieten worden gedefinieerd zoals weergegeven in het volgende voorbeeldfragment:

```yaml
resources:
  requests:
     cpu: 250m
  limits:
     cpu: 500m
```

In het volgende voor beeld wordt de opdracht [kubectl AutoScale][kubectl-autoscale] gebruikt voor het automatisch schalen van het aantal peulen in de implementatie van *Azure-stemmen-front* . Als het CPU-gebruik hoger wordt dan 50%, wordt het aantal pods automatisch verhoogd tot een maximum van *10* exemplaren. Minimaal *3* exemplaren worden dan gedefinieerd voor de implementatie:

```console
kubectl autoscale deployment azure-vote-front --cpu-percent=50 --min=3 --max=10
```

Gebruik de opdracht `kubectl get hpa` als volgt om de status van de functie voor automatisch schalen te bekijken:

```
$ kubectl get hpa

NAME               REFERENCE                     TARGETS    MINPODS   MAXPODS   REPLICAS   AGE
azure-vote-front   Deployment/azure-vote-front   0% / 50%   3         10        3          2m
```

Bij een minimale belasting van de Azure Vote-app neemt het aantal pod-replica's na enkele minuten automatisch af tot drie. U kunt `kubectl get pods` opnieuw gebruiken om te zien dat de overbodige pods worden verwijderd.

## <a name="manually-scale-aks-nodes"></a>AKS-knooppunten handmatig schalen

Als u uw Kubernetes-cluster in de vorige zelfstudie hebt gemaakt met de opdrachten uit de vorige zelfstudie, heeft het cluster één knooppunt. U kunt het aantal knooppunten handmatig aanpassen als u meer of minder containerwerkbelastingen in uw cluster plant.

In het volgende voorbeeld wordt het aantal knooppunten in het Kubernetes-cluster *myAKSCluster* verhoogd tot drie. Het uitvoeren van deze opdracht duurt enkele minuten.

```azurecli
az aks scale --resource-group myResourceGroup --name myAKSCluster --node-count 3
```

Wanneer het cluster is geschaald, is de uitvoer vergelijkbaar met het volgende voorbeeld:

```
"agentPoolProfiles": [
  {
    "count": 3,
    "dnsPrefix": null,
    "fqdn": null,
    "name": "myAKSCluster",
    "osDiskSizeGb": null,
    "osType": "Linux",
    "ports": null,
    "storageProfile": "ManagedDisks",
    "vmSize": "Standard_D2_v2",
    "vnetSubnetId": null
  }
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u verschillende schaalfuncties in uw Kubernetes-cluster gebruikt. U hebt geleerd hoe u:

> [!div class="checklist"]
> * Handmatig Kubernetes-pods schalen waarop uw toepassing wordt uitgevoerd
> * Automatisch schalen configureren van pods die de front-end-app uitvoeren
> * Kubernetes-knooppunten handmatig schalen

Ga verder met de volgende zelfstudie om te leren hoe u toepassingen bijwerkt in Kubernetes.

> [!div class="nextstepaction"]
> [Een toepassing bijwerken in Kubernetes][aks-tutorial-update-app]

<!-- LINKS - external -->
[kubectl-autoscale]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#autoscale
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-scale]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#scale
[kubernetes-hpa]: https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/
[metrics-server-github]: https://github.com/kubernetes-incubator/metrics-server/tree/master/deploy/1.8%2B
[metrics-server]: https://v1-12.docs.kubernetes.io/docs/tasks/debug-application-cluster/core-metrics-pipeline/

<!-- LINKS - internal -->
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[aks-tutorial-update-app]: ./tutorial-kubernetes-app-update.md
[az-aks-scale]: /cli/azure/aks#az-aks-scale
[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-show]: /cli/azure/aks#az-aks-show
