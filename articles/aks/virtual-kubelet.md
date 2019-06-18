---
title: Virtual Kubelet uitvoeren in een cluster Azure Kubernetes Service (AKS)
description: Informatie over het gebruik van de Virtual Kubelet met Azure Kubernetes Service (AKS) uit te voeren van Linux en Windows-containers op Azure Container Instances.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/31/2019
ms.author: iainfou
ms.openlocfilehash: cc0c3becf21cb54b97a88e9ba35b38308af81a85
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66475420"
---
# <a name="use-virtual-kubelet-with-azure-kubernetes-service-aks"></a>Virtual Kubelet gebruiken met Azure Kubernetes Service (AKS)

Azure Container Instances (ACI) bieden een omgeving voor het uitvoeren van containers in Azure. Wanneer u ACI, er is niet nodig voor het beheren van de onderliggende infrastructuur van compute, Azure regelt dit beheer voor u. Bij het uitvoeren van containers in ACI, worden in rekening gebracht per seconde voor elke container die wordt uitgevoerd.

Wanneer u de Virtual Kubelet-provider voor Azure Container Instances, kunnen zowel Windows als Linux-containers worden gepland op een containerinstantie alsof het een standaard Kubernetes-knooppunt. Deze configuratie kunt u profiteren van de mogelijkheden van Kubernetes en het beheer van waarde en de kosten voordeel van containerinstanties.

> [!NOTE]
> Nu heeft ingebouwde ondersteuning voor het plannen van containers in ACI, met de naam AKS *virtuele knooppunten*. Deze virtuele knooppunten ondersteuning momenteel voor Linux-container instances. Als u nodig hebt voor het plannen van exemplaren van Windows-container, kunt u blijven Virtual Kubelet gebruiken. Anders moet u virtuele-knooppunten gebruiken in plaats van de handmatige Virtual Kubelet-instructies in dit artikel hebt genoteerd. U kunt aan de slag met virtuele knooppunten met behulp van de [Azure CLI] [ virtual-nodes-cli] of [Azure-portal][virtual-nodes-portal].
>
> Virtual Kubelet is een experimenteel open-source-project en als zodanig moet worden gebruikt. Als u wilt bijdragen, problemen met bestanden, en lees meer over de virtual kubelet, Zie de [Virtual Kubelet GitHub-project][vk-github].

## <a name="before-you-begin"></a>Voordat u begint

Dit document wordt ervan uitgegaan dat u een AKS-cluster hebt. Als u een cluster AKS nodig hebt, raadpleegt u de [Azure Kubernetes Service (AKS)-snelstartgids][aks-quick-start].

U moet ook de Azure CLI-versie **2.0.65** of hoger. Voer `az --version` uit om de versie te bekijken. Zie [Azure CLI installeren](/cli/azure/install-azure-cli) als u de CLI wilt installeren of een upgrade wilt uitvoeren.

Voor het installeren van de Virtual Kubelet, installeren en configureren van [Helm] [ aks-helm] in uw AKS-cluster. Zorg ervoor dat uw Tiller [geconfigureerd voor gebruik met Kubernetes RBAC](#for-rbac-enabled-clusters), indien nodig.

### <a name="register-container-instances-feature-provider"></a>Container Instances functie provider registreren

Als u de service Azure Container exemplaar (ACI) niet eerder hebt gebruikt, moet u de service-provider registreren met uw abonnement. U kunt de status van de ACI provider registreren met behulp van controleren de [az provider list] [ az-provider-list] opdracht, zoals wordt weergegeven in het volgende voorbeeld:

```azurecli-interactive
az provider list --query "[?contains(namespace,'Microsoft.ContainerInstance')]" -o table
```

De *Microsoft.ContainerInstance* provider wilt laten rapporteren als *geregistreerde*, zoals weergegeven in de volgende voorbeelduitvoer:

```console
Namespace                    RegistrationState
---------------------------  -------------------
Microsoft.ContainerInstance  Registered
```

Als de provider wordt weergegeven als *NotRegistered*, Registreer de provider met behulp van de [az provider register] [ az-provider-register] zoals wordt weergegeven in het volgende voorbeeld:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerInstance
```

### <a name="for-rbac-enabled-clusters"></a>Voor clusters met RBAC-ingeschakeld

Als uw AKS-cluster RBAC is ingeschakeld is, moet u een service-account en een binding van de rol voor gebruik met Tiller maken. Zie voor meer informatie, [Helm rollen gebaseerd toegangsbeheer][helm-rbac]. Maak een bestand met de naam voor het maken van een service-account en een binding van de rol, *rbac virtuele kubelet.yaml* en plak de definitie van de volgende:

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: tiller
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: tiller
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
  - kind: ServiceAccount
    name: tiller
    namespace: kube-system
```

Het serviceaccount van toepassing en binden met [kubectl toepassen] [ kubectl-apply] en geef uw *rbac virtuele kubelet.yaml* bestand, zoals weergegeven in het volgende voorbeeld:

```console
$ kubectl apply -f rbac-virtual-kubelet.yaml

clusterrolebinding.rbac.authorization.k8s.io/tiller created
```

Helm voor het gebruik van het serviceaccount tiller configureren:

```console
helm init --service-account tiller
```

U kunt nu doorgaan naar de Virtual Kubelet installeren in uw AKS-cluster.

## <a name="installation"></a>Installatie

Gebruik de [az aks install-connector] [ aks-install-connector] opdracht voor het installeren van de Virtual Kubelet. De Linux- en Windows-connector heeft geïmplementeerd in het volgende voorbeeld.

```azurecli-interactive
az aks install-connector \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --connector-name virtual-kubelet \
    --os-type Both
```

Deze argumenten zijn beschikbaar voor de [az aks install-connector] [ aks-install-connector] opdracht.

| Argument: | Description | Vereist |
|---|---|:---:|
| `--connector-name` | Naam van de ACI-Connector.| Ja |
| `--name` `-n` | Naam van de beheerde cluster. | Ja |
| `--resource-group` `-g` | De naam van de resourcegroep. | Ja |
| `--os-type` | Type in de container instances-besturingssysteem. Toegestane waarden: Both, Linux, Windows. Standaard: Linux. | Nee |
| `--aci-resource-group` | De resourcegroep waarin u wilt maken van de groepen ACI-container. | Nee |
| `--location` `-l` | De locatie voor het maken van de groepen ACI-container. | Nee |
| `--service-principal` | Service-principal gebruikt voor verificatie bij Azure-API's. | Nee |
| `--client-secret` | Het geheim die is gekoppeld aan de service-principal. | Nee |
| `--chart-url` | URL van een Helm-grafiek die ACI-Connector installeert. | Nee |
| `--image-tag` | De tag de installatiekopie van de installatiekopie van de virtual kubelet-container. | Nee |

## <a name="validate-virtual-kubelet"></a>Valideren van de Virtual Kubelet

Retourneert een lijst met behulp van Kubernetes-knooppunten om te valideren dat de Virtual Kubelet is geïnstalleerd, de [kubectl ophalen knooppunten] [ kubectl-get] opdracht:

```console
$ kubectl get nodes

NAME                                             STATUS   ROLES   AGE   VERSION
aks-nodepool1-56577038-0                         Ready    agent   11m   v1.12.8
virtual-kubelet-virtual-kubelet-linux-eastus     Ready    agent   39s   v1.13.1-vk-v0.9.0-1-g7b92d1ee-dev
virtual-kubelet-virtual-kubelet-windows-eastus   Ready    agent   37s   v1.13.1-vk-v0.9.0-1-g7b92d1ee-dev
```

## <a name="run-linux-container"></a>Linux-container uitvoeren

Maak een bestand met de naam `virtual-kubelet-linux.yaml` en kopieer de volgende YAML. Houd er rekening mee houden dat een [nodeSelector] [ node-selector] en [toleration] [ toleration] worden gebruikt voor het plannen van de container op het knooppunt.

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
        beta.kubernetes.io/os: linux
        kubernetes.io/role: agent
        type: virtual-kubelet
      tolerations:
      - key: virtual-kubelet.io/provider
        operator: Equal
        value: azure
        effect: NoSchedule
```

Voer de toepassing met de [kubectl maken] [ kubectl-create] opdracht.

```console
kubectl create -f virtual-kubelet-linux.yaml
```

Gebruik de [kubectl ophalen schillen] [ kubectl-get] opdracht met de `-o wide` argument voor een lijst met schillen met het knooppunt geplande uitvoer. U ziet dat de `aci-helloworld` schil is gepland op de `virtual-kubelet-virtual-kubelet-linux` knooppunt.

```console
$ kubectl get pods -o wide

NAME                              READY   STATUS    RESTARTS   AGE     IP               NODE
aci-helloworld-7b9ffbf946-rx87g   1/1     Running   0          22s     52.224.147.210   virtual-kubelet-virtual-kubelet-linux-eastus
```

## <a name="run-windows-container"></a>Windows-container uitvoeren

Maak een bestand met de naam `virtual-kubelet-windows.yaml` en kopieer de volgende YAML. Houd er rekening mee houden dat een [nodeSelector] [ node-selector] en [toleration] [ toleration] worden gebruikt voor het plannen van de container op het knooppunt.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nanoserver-iis
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nanoserver-iis
  template:
    metadata:
      labels:
        app: nanoserver-iis
    spec:
      containers:
      - name: nanoserver-iis
        image: microsoft/iis:nanoserver
        ports:
        - containerPort: 80
      nodeSelector:
        beta.kubernetes.io/os: windows
        kubernetes.io/role: agent
        type: virtual-kubelet
      tolerations:
      - key: virtual-kubelet.io/provider
        operator: Equal
        value: azure
        effect: NoSchedule
```

Voer de toepassing met de [kubectl maken] [ kubectl-create] opdracht.

```console
kubectl create -f virtual-kubelet-windows.yaml
```

Gebruik de [kubectl ophalen schillen] [ kubectl-get] opdracht met de `-o wide` argument voor een lijst met schillen met het knooppunt geplande uitvoer. U ziet dat de `nanoserver-iis` schil is gepland op de `virtual-kubelet-virtual-kubelet-windows` knooppunt.

```console
$ kubectl get pods -o wide

NAME                              READY   STATUS    RESTARTS   AGE     IP               NODE
nanoserver-iis-5d999b87d7-6h8s9   1/1     Running   0          47s     52.224.143.39    virtual-kubelet-virtual-kubelet-windows-eastus
```

## <a name="remove-virtual-kubelet"></a>Verwijder de Virtual Kubelet

Gebruik de [az aks remove-connector] [ aks-remove-connector] opdracht om te verwijderen van de Virtual Kubelet. Vervang de waarden van het argument met de naam van de connector, AKS-cluster en de resourcegroep van de AKS-cluster.

```azurecli-interactive
az aks remove-connector \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --connector-name virtual-kubelet \
    --os-type Both
```

> [!NOTE]
> Als u beide connectors OS verwijderen fouten optreden of alleen de connector voor Windows of Linux-besturingssysteem verwijderen, kunt u het type besturingssysteem handmatig opgeven. Voeg de `--os-type` parameter voor de vorige `az aks remove-connector` opdracht in en geef `Windows` of `Linux`.

## <a name="next-steps"></a>Volgende stappen

Zie voor mogelijke problemen met de Virtual Kubelet de [bekende quirks en tijdelijke oplossingen][vk-troubleshooting]. Om problemen te rapporteren met de Virtual Kubelet [opent u een GitHub-probleem][vk-issues].

Meer informatie over de Virtual Kubelet op de [Virtual Kubelet GitHub-project][vk-github].

<!-- LINKS - internal -->
[aks-quick-start]: ./kubernetes-walkthrough.md
[aks-remove-connector]: /cli/azure/aks#az-aks-remove-connector
[az-container-list]: /cli/azure/aks#az-aks-list
[aks-install-connector]: /cli/azure/aks#az-aks-install-connector
[virtual-nodes-cli]: virtual-nodes-cli.md
[virtual-nodes-portal]: virtual-nodes-portal.md
[aks-helm]: kubernetes-helm.md
[az-provider-list]: /cli/azure/provider#az-provider-list
[az-provider-register]: /cli/azure/provider#az-provider-register

<!-- LINKS - external -->
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[node-selector]:https://kubernetes.io/docs/concepts/configuration/assign-pod-node/
[toleration]: https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/
[vk-github]: https://github.com/virtual-kubelet/virtual-kubelet
[helm-rbac]: https://docs.helm.sh/using_helm/#role-based-access-control
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[vk-troubleshooting]: https://github.com/virtual-kubelet/virtual-kubelet#known-quirks-and-workarounds
[vk-issues]: https://github.com/virtual-kubelet/virtual-kubelet/issues
