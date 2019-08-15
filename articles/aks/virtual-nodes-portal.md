---
title: Virtuele knoop punten maken met behulp van de portal in azure Kubernetes Services (AKS)
description: Meer informatie over het gebruik van de Azure Portal om een AKS-cluster (Azure Kubernetes Services) te maken dat virtuele knoop punten gebruikt voor het uitvoeren van een Peul.
services: container-service
author: mlearned
ms.topic: conceptual
ms.service: container-service
ms.date: 05/06/2019
ms.author: mlearned
ms.openlocfilehash: 8752d888e24e7135d488be6d1b377070a30fe4eb
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/12/2019
ms.locfileid: "67613832"
---
# <a name="create-and-configure-an-azure-kubernetes-services-aks-cluster-to-use-virtual-nodes-in-the-azure-portal"></a>Een AKS-cluster (Azure Kubernetes Services) maken en configureren voor het gebruik van virtuele knoop punten in de Azure Portal

Als u werk belastingen snel wilt implementeren in een Azure Kubernetes service (AKS)-cluster, kunt u virtuele knoop punten gebruiken. Met virtuele knoop punten hebt u snel de inrichting van Peul en betaalt u alleen per seconde voor de uitvoerings tijd. In een schaal scenario hoeft u niet te wachten tot de Kubernetes van het cluster voor het implementeren van VM-reken knooppunten om het andere peul uit te voeren. Virtuele knoop punten worden alleen ondersteund met Linux-en knoop punten.

In dit artikel wordt beschreven hoe u de virtuele netwerk resources en een AKS-cluster maakt en configureert waarop virtuele knoop punten zijn ingeschakeld.

## <a name="before-you-begin"></a>Voordat u begint

Virtuele knoop punten scha kelen netwerk communicatie in tussen de peulen die worden uitgevoerd in ACI en het AKS-cluster. Om deze communicatie te bieden, wordt een subnet van een virtueel netwerk gemaakt en worden gedelegeerde machtigingen toegewezen. Virtuele knoop punten werken alleen met AKS-clusters die zijn gemaakt met *Geavanceerde* netwerken. Standaard worden AKS-clusters gemaakt met een *basis* netwerk. In dit artikel wordt beschreven hoe u een virtueel netwerk en subnetten maakt en hoe u een AKS-cluster implementeert dat gebruikmaakt van geavanceerde netwerken.

Als u geen ACI eerder hebt gebruikt, registreert u de service provider bij uw abonnement. U kunt de status van de ACI-provider registratie controleren met de opdracht [AZ provider List][az-provider-list] , zoals wordt weer gegeven in het volgende voor beeld:

```azurecli-interactive
az provider list --query "[?contains(namespace,'Microsoft.ContainerInstance')]" -o table
```

De provider van *micro soft. ContainerInstance* meldt zich aan als *geregistreerd*, zoals wordt weer gegeven in de volgende voorbeeld uitvoer:

```
Namespace                    RegistrationState
---------------------------  -------------------
Microsoft.ContainerInstance  Registered
```

Als de provider wordt weer gegeven als *NotRegistered*, registreert u de provider met behulp van de [AZ provider REGI ster] [AZ-provider-REGI ster], zoals wordt weer gegeven in het volgende voor beeld:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerInstance
```

## <a name="regional-availability"></a>Regionale beschikbaarheid

De volgende regio's worden ondersteund voor implementaties van virtuele knoop punten:

* Australië-oost (australiaeast)
* VS-Midden (centraal)
* VS-Oost (Oost)
* VS-Oost 2 (eastus2)
* Japan-Oost (japaneast)
* Europa-noord (northeurope)
* Zuidoost-Azië (southeastasia)
* VS-West-Centraal (westcentralus)
* Europa-west (Europa West)
* VS-West (westus)
* VS-West 2 (westus2)

## <a name="known-limitations"></a>Bekende beperkingen
De functionaliteit van virtuele knoop punten is sterk afhankelijk van de functieset van ACI. De volgende scenario's worden nog niet ondersteund met virtuele knoop punten

* Service-Principal gebruiken voor het ophalen van ACR-installatie kopieën. [Tijdelijke oplossing](https://github.com/virtual-kubelet/virtual-kubelet/blob/master/providers/azure/README.md#Private-registry) is het gebruik van [Kubernetes-geheimen](https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/#create-a-secret-by-providing-credentials-on-the-command-line)
* [Virtual Network beperkingen](../container-instances/container-instances-vnet.md) , waaronder VNet-peering, Kubernetes-netwerk beleid en uitgaand verkeer naar Internet met netwerk beveiligings groepen.
* Init-containers
* [Host-aliassen](https://kubernetes.io/docs/concepts/services-networking/add-entries-to-pod-etc-hosts-with-host-aliases/)
* [Argumenten](../container-instances/container-instances-exec.md#restrictions) voor exec in ACI
* [Daemonsets](concepts-clusters-workloads.md#statefulsets-and-daemonsets) implementeert geen peuling voor het virtuele knoop punt
* [Windows Server-knoop punten (momenteel in de preview-versie van AKS)](windows-container-cli.md) worden niet ondersteund naast virtuele knoop punten. U kunt virtuele knoop punten gebruiken om Windows Server-containers te plannen zonder dat Windows Server-knoop punten nodig zijn in een AKS-cluster.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij Azure Portal op https://portal.azure.com.

## <a name="create-an-aks-cluster"></a>Een AKS-cluster maken

Selecteer in de linkerbovenhoek van de Azure-portal **Een resource maken** > **Kubernetes-service**.

Configureer op de pagina **basis beginselen** de volgende opties:

- *PROJECTGEGEVENS*: Selecteer een Azure-abonnement, en selecteer of maak vervolgens een Azure-resourcegroep, zoals *myResourceGroup*. Voer een **Kubernetes-clusternaam** in, zoals *myAKSCluster*.
- *CLUSTERDETAILS*: Selecteer een regio, Kubernetes-versie en DNS-naamvoorvoegsel voor het AKS-cluster.
- *PRIMAIRE KNOOPPUNT GROEP*: Selecteer een VM-grootte voor de AKS-knooppunten. De VM-grootte kan **niet** meer worden gewijzigd als een AKS-cluster eenmaal is geïmplementeerd.
     - Selecteer het aantal knooppunten dat u in het cluster wilt implementeren. Voor dit artikel stelt u het **aantal knoop punten** in op *1*. Het aantal knooppunten kan nog **wel** worden gewijzigd als het cluster is geïmplementeerd.

Klik op **Next: Schalen**.

Selecteer op de pagina **schalen** de optie *ingeschakeld* onder **virtuele knoop punten**.

![AKS-cluster maken en de virtuele knoop punten inschakelen](media/virtual-nodes-portal/enable-virtual-nodes.png)

Standaard wordt een Azure Active Directory Service-Principal gemaakt. Deze service-principal wordt gebruikt voor cluster communicatie en integratie met andere Azure-Services.

Het cluster is ook geconfigureerd voor geavanceerde netwerken. De virtuele knoop punten zijn geconfigureerd voor het gebruik van hun eigen subnet van het virtuele netwerk van Azure. Dit subnet heeft gedelegeerde machtigingen voor het verbinden van Azure-resources tussen het AKS-cluster. Als u nog geen overgedragen subnet hebt, wordt het virtuele Azure-netwerk en-subnet door de Azure Portal gemaakt en geconfigureerd voor gebruik met de virtuele knoop punten.

Selecteer **Controleren + maken**. Nadat de validatie is voltooid, selecteert u **maken**.

Het duurt enkele minuten om het AKS-cluster te maken en voor te bereiden voor gebruik.

## <a name="connect-to-the-cluster"></a>Verbinding maken met het cluster

Azure Cloud Shell is een gratis interactieve shell waarmee u de stappen in dit artikel kunt uitvoeren. In deze shell zijn algemene Azure-hulpprogramma's vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account. Als u een Kubernetes-cluster wilt beheren, gebruikt u [kubectl][kubectl], de Kubernetes-opdrachtregelclient. De client `kubectl` is vooraf geïnstalleerd in Azure Cloud Shell.

Als u de Cloud Shell wilt openen, selecteert u **deze** in de rechter bovenhoek van een code blok. U kunt Cloud Shell ook openen in een afzonderlijk browsertabblad door naar [https://shell.azure.com/bash](https://shell.azure.com/bash) te gaan. Klik op **Kopiëren** om de codeblokken te kopiëren, plak deze in Cloud Shell en druk vervolgens op Enter om de code uit te voeren.

Gebruik de opdracht [AZ AKS Get-credentials][az-aks-get-credentials] om `kubectl` te configureren om verbinding te maken met uw Kubernetes-cluster. In het volgende voorbeeld worden de referenties opgehaald voor de clusternaam *myAKSCluster* in de resourcegroep met de naam *myResourceGroup*:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Als u de verbinding met uw cluster wilt controleren, gebruikt u de opdracht [kubectl get][kubectl-get] om een lijst met clusterknooppunten te retourneren.

```azurecli-interactive
kubectl get nodes
```

In de volgende voorbeeld uitvoer ziet u het knoop punt met één VM dat is gemaakt en vervolgens het virtuele knoop punt voor Linux, *Virtual-node-ACI-Linux*:

```
$ kubectl get nodes

NAME                           STATUS    ROLES     AGE       VERSION
virtual-node-aci-linux         Ready     agent     28m       v1.11.2
aks-agentpool-14693408-0       Ready     agent     32m       v1.11.2
```

## <a name="deploy-a-sample-app"></a>Een voor beeld-app implementeren

In de Azure Cloud Shell maakt u een bestand met `virtual-node.yaml` de naam en kopieert u dit in de volgende YAML. Als u de container op het knoop punt wilt plannen, worden er een [nodeSelector][node-selector] en- [tolerantie][toleration] gedefinieerd. Met deze instellingen kan de pod worden gepland op het virtuele knoop punt en wordt gecontroleerd of de functie is ingeschakeld.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: aci-helloworld
spec:
  replicas: 1
  selector:
    matchLabels:
      app: aci-helloworld
  template:
    metadata:
      labels:
        app: aci-helloworld
    spec:
      containers:
      - name: aci-helloworld
        image: microsoft/aci-helloworld
        ports:
        - containerPort: 80
      nodeSelector:
        kubernetes.io/role: agent
        beta.kubernetes.io/os: linux
        type: virtual-kubelet
      tolerations:
      - key: virtual-kubelet.io/provider
        operator: Exists
      - key: azure.com/aci
        effect: NoSchedule
```

Voer de toepassing uit met de opdracht [kubectl apply][kubectl-apply] .

```azurecli-interactive
kubectl apply -f virtual-node.yaml
```

Gebruik de opdracht [kubectl Get peul][kubectl-get] with het `-o wide` argument voor het uitvoeren van een lijst met een van de peulen en het geplande knoop punt. U ziet dat `virtual-node-helloworld` de Pod is gepland op het `virtual-node-linux` knoop punt.

```
$ kubectl get pods -o wide

NAME                                     READY     STATUS    RESTARTS   AGE       IP           NODE
virtual-node-helloworld-9b55975f-bnmfl   1/1       Running   0          4m        10.241.0.4   virtual-node-aci-linux
```

Aan de Pod wordt een intern IP-adres toegewezen vanuit het subnet van het virtuele netwerk van Azure dat is gedelegeerd voor gebruik met virtuele knoop punten.

> [!NOTE]
> [Configureer en gebruik een Kubernetes-geheim][acr-aks-secrets]als u installatie kopieën gebruikt die zijn opgeslagen in azure container Registry. Een huidige beperking van virtuele knoop punten is dat u geen geïntegreerde Azure AD-Service-Principal-verificatie kunt gebruiken. Als u geen geheim gebruikt, kan er geen peulen gepland op virtuele knoop punten worden gestart en `HTTP response status code 400 error code "InaccessibleImage"`wordt de fout gerapporteerd.

## <a name="test-the-virtual-node-pod"></a>De pod van het virtuele knoop punt testen

Als u de pod die op het virtuele knoop punt wordt uitgevoerd wilt testen, bladert u naar de demo toepassing met een webclient. Als aan de pod een intern IP-adres is toegewezen, kunt u deze verbinding snel testen vanaf een andere pod op het AKS-cluster. Een test pod maken en een terminal sessie hieraan koppelen:

```azurecli-interactive
kubectl run -it --rm virtual-node-test --image=debian
```

Installeer `curl` in de Pod met `apt-get`:

```azurecli-interactive
apt-get update && apt-get install -y curl
```

U hebt nu toegang tot het adres van `curl`uw Pod met *http://10.241.0.4* , zoals. Geef uw eigen interne IP-adres op dat wordt `kubectl get pods` weer gegeven in de vorige opdracht:

```azurecli-interactive
curl -L http://10.241.0.4
```

De voorbeeld toepassing wordt weer gegeven, zoals wordt weer gegeven in de volgende verkorte voorbeeld uitvoer:

```
$ curl -L 10.241.0.4

<html>
<head>
  <title>Welcome to Azure Container Instances!</title>
</head>
[...]
```

Sluit de terminal sessie met uw test pod met `exit`. Wanneer de sessie is beëindigd, wordt de pod verwijderd.

## <a name="next-steps"></a>Volgende stappen

In dit artikel is een pod gepland op het virtuele knoop punt en een persoonlijk, intern IP-adres toegewezen. U kunt in plaats daarvan een service-implementatie maken en verkeer naar uw Pod sturen via een load balancer-of ingangs controller. Zie [een Basic ingress-controller maken in AKS][aks-basic-ingress]voor meer informatie.

Virtuele knoop punten zijn één onderdeel van een schaal oplossing in AKS. Raadpleeg de volgende artikelen voor meer informatie over het schalen van oplossingen:

- [De Kubernetes-functie voor horizontale pod automatisch schalen gebruiken][aks-hpa]
- [De automatische schaal functie van het Kubernetes-cluster gebruiken][aks-cluster-autoscaler]
- [Bekijk het voor beeld van automatisch schalen voor virtuele knoop punten][virtual-node-autoscale]
- [Meer informatie over de open source-bibliotheek van de virtuele-Kubelet][virtual-kubelet-repo]

<!-- LINKS - external -->
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[node-selector]:https://kubernetes.io/docs/concepts/configuration/assign-pod-node/
[toleration]: https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/
[azure-cni]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[aks-github]: https://github.com/azure/aks/issues]
[virtual-node-autoscale]: https://github.com/Azure-Samples/virtual-node-autoscale
[virtual-kubelet-repo]: https://github.com/virtual-kubelet/virtual-kubelet

<!-- LINKS - internal -->
[aks-network]: ./networking-overview.md
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[aks-hpa]: tutorial-kubernetes-scale.md
[aks-cluster-autoscaler]: cluster-autoscaler.md
[aks-basic-ingress]: ingress-basic.md
[acr-aks-secrets]: ../container-registry/container-registry-auth-aks.md#access-with-kubernetes-secret
[az-provider-list]: /cli/azure/provider#az-provider-list