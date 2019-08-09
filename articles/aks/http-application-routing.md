---
title: Invoeg toepassing voor het routeren van HTTP-toepassingen op de Azure Kubernetes-service (AKS)
description: Gebruik de invoeg toepassing HTTP-toepassings routering in azure Kubernetes service (AKS).
services: container-service
author: lachie83
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 08/06/2019
ms.author: laevenso
ms.openlocfilehash: f0975d0a60081b66d3d5a513954deb0c4fa1b978
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2019
ms.locfileid: "68851551"
---
# <a name="http-application-routing"></a>HTTP-toepassingsroutering

Met de oplossing voor het routeren van HTTP-toepassingen kunt u eenvoudig toegang krijgen tot toepassingen die zijn geïmplementeerd in uw Azure Kubernetes service-cluster (AKS). Wanneer de oplossing is ingeschakeld, wordt er een [ingangs controller](https://kubernetes.io/docs/concepts/services-networking/ingress-controllers/) geconfigureerd in uw AKS-cluster. Wanneer toepassingen worden geïmplementeerd, maakt de oplossing ook openbaar toegankelijke DNS-namen voor toepassings eindpunten.

Wanneer de invoeg toepassing is ingeschakeld, wordt er een DNS-zone in uw abonnement gemaakt. Zie [DNS-prijzen][dns-pricing]voor meer informatie over DNS-kosten.

> [!CAUTION]
> De invoeg toepassing voor het routeren van HTTP-toepassingen is ontworpen om snel een ingangs controller te maken en toegang te krijgen tot uw toepassingen. Deze invoeg toepassing wordt niet aanbevolen voor productie gebruik. Zie [een HTTPS ingress-controller maken](https://docs.microsoft.com/azure/aks/ingress-tls)voor implementaties van kant-en-klare ingebruiknames met meerdere REPLICA'S en TLS-ondersteuning.

## <a name="http-routing-solution-overview"></a>Overzicht van de oplossing voor HTTP-route ring

De invoeg toepassing implementeert twee onderdelen: een [Kubernetes ingress-controller][ingress] en een [externe DNS][external-dns] -controller.

- **Ingangs controller**: De ingangs controller wordt blootgesteld aan Internet door gebruik te maken van een Kubernetes-service van het type Load Balancer. De ingangs controller controleert en implementeert [Kubernetes][ingress-resource]inkomend bronnen, waarmee routes naar toepassings eindpunten worden gemaakt.
- **Externe DNS-controller**: Controleert op Kubernetes-inkomende resources en maakt DNS A-records in de cluster-specifieke DNS-zone.

## <a name="deploy-http-routing-cli"></a>HTTP-route ring implementeren: CLI

De invoeg toepassing voor het routeren van HTTP-toepassingen kan worden ingeschakeld met de Azure CLI bij het implementeren van een AKS-cluster. Als u dit wilt doen, gebruikt u de opdracht [AZ AKS Create][az-aks-create] met het `--enable-addons` argument.

```azurecli
az aks create --resource-group myResourceGroup --name myAKSCluster --enable-addons http_application_routing
```

> [!TIP]
> Als u meerdere invoeg toepassingen wilt inschakelen, geeft u ze op als een door komma's gescheiden lijst. Als u bijvoorbeeld HTTP-toepassings Routering en-bewaking wilt inschakelen, gebruikt `--enable-addons http_application_routing,monitoring`u de indeling.

U kunt ook HTTP-route ring inschakelen op een bestaand AKS-cluster met behulp van de opdracht [AZ AKS Enable-addons][az-aks-enable-addons] . Als u http-route ring wilt inschakelen op een bestaand `--addons` cluster, voegt u de para meter toe en geeft u *http_application_routing* op, zoals in het volgende voor beeld wordt getoond:

```azurecli
az aks enable-addons --resource-group myResourceGroup --name myAKSCluster --addons http_application_routing
```

Nadat het cluster is geïmplementeerd of bijgewerkt, gebruikt u de opdracht [AZ AKS show][az-aks-show] om de naam van de DNS-zone op te halen. Deze naam is nodig om toepassingen te implementeren in het AKS-cluster.

```azurecli
$ az aks show --resource-group myResourceGroup --name myAKSCluster --query addonProfiles.httpApplicationRouting.config.HTTPApplicationRoutingZoneName -o table

Result
-----------------------------------------------------
9f9c1fe7-21a1-416d-99cd-3543bb92e4c3.eastus.aksapp.io
```

## <a name="deploy-http-routing-portal"></a>HTTP-route ring implementeren: Portal

De invoeg toepassing voor het routeren van HTTP-toepassingen kan worden ingeschakeld via de Azure Portal bij het implementeren van een AKS-cluster.

![De HTTP-routerings functie inschakelen](media/http-routing/create.png)

Nadat het cluster is geïmplementeerd, bladert u naar de automatisch gemaakte AKS-resource groep en selecteert u de DNS-zone. Noteer de naam van de DNS-zone. Deze naam is nodig om toepassingen te implementeren in het AKS-cluster.

![De DNS-zone naam ophalen](media/http-routing/dns.png)

## <a name="use-http-routing"></a>HTTP-route ring gebruiken

De oplossing voor het routeren van HTTP-toepassingen kan alleen worden geactiveerd voor ingangs bronnen die als volgt worden aantekend:

```yaml
annotations:
  kubernetes.io/ingress.class: addon-http-application-routing
```

Maak een bestand met de naam **samples-http-Application-Routing. yaml** en kopieer de volgende YAML. Update `<CLUSTER_SPECIFIC_DNS_ZONE>` op regel 43 met de naam van de DNS-zone die in de vorige stap van dit artikel is verzameld.


```yaml
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: party-clippy
spec:
  template:
    metadata:
      labels:
        app: party-clippy
    spec:
      containers:
      - image: r.j3ss.co/party-clippy
        name: party-clippy
        resources:
          requests:
            cpu: 100m
            memory: 128Mi
          limits:
            cpu: 250m
            memory: 256Mi
        tty: true
        command: ["party-clippy"]
        ports:
        - containerPort: 8080
---
apiVersion: v1
kind: Service
metadata:
  name: party-clippy
spec:
  ports:
  - port: 80
    protocol: TCP
    targetPort: 8080
  selector:
    app: party-clippy
  type: ClusterIP
---
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: party-clippy
  annotations:
    kubernetes.io/ingress.class: addon-http-application-routing
spec:
  rules:
  - host: party-clippy.<CLUSTER_SPECIFIC_DNS_ZONE>
    http:
      paths:
      - backend:
          serviceName: party-clippy
          servicePort: 80
        path: /
```

Gebruik de opdracht [kubectl Toep assen][kubectl-apply] om de resources te maken.

```bash
$ kubectl apply -f samples-http-application-routing.yaml

deployment "party-clippy" created
service "party-clippy" created
ingress "party-clippy" created
```

Gebruik krul of een browser om te navigeren naar de hostnaam die is opgegeven in het gedeelte host van het bestand Samples-http-Application-Routing. yaml. De toepassing kan Maxi maal één minuut duren voordat deze beschikbaar is via internet.

```bash
$ curl party-clippy.471756a6-e744-4aa0-aa01-89c4d162a7a7.canadaeast.aksapp.io

 _________________________________
/ It looks like you're building a \
\ microservice.                   /
 ---------------------------------
 \
  \
     __
    /  \
    |  |
    @  @
    |  |
    || |/
    || ||
    |\_/|
    \___/

```

## <a name="remove-http-routing"></a>HTTP-route ring verwijderen

De oplossing voor HTTP-route ring kan worden verwijderd met behulp van de Azure CLI. Voer de volgende opdracht uit om de naam van het AKS-cluster en de resource groep te vervangen.

```azurecli
az aks disable-addons --addons http_application_routing --name myAKSCluster --resource-group myResourceGroup --no-wait
```

Wanneer de invoeg toepassing voor het routeren van HTTP-toepassingen is uitgeschakeld, kunnen sommige Kubernetes-resources in het cluster blijven. Deze bronnen zijn onder andere *configMaps* en *geheimen*, en worden gemaakt in de *uitvoeren-systeem* naam ruimte. Als u een schoon cluster wilt behouden, kunt u deze resources verwijderen.

Zoek naar *invoeg toepassingen-http-toepassings routerings* bronnen met behulp van de volgende [kubectl Get][kubectl-get] -opdrachten:

```console
kubectl get deployments --namespace kube-system
kubectl get services --namespace kube-system
kubectl get configmaps --namespace kube-system
kubectl get secrets --namespace kube-system
```

In de volgende voorbeeld uitvoer ziet u configMaps die moeten worden verwijderd:

```
$ kubectl get configmaps --namespace kube-system

NAMESPACE     NAME                                                       DATA   AGE
kube-system   addon-http-application-routing-nginx-configuration         0      9m7s
kube-system   addon-http-application-routing-tcp-services                0      9m7s
kube-system   addon-http-application-routing-udp-services                0      9m7s
```

Als u resources wilt verwijderen, gebruikt u de opdracht [kubectl verwijderen][kubectl-delete] . Geef het resource type, de resource naam en de naam ruimte op. In het volgende voor beeld wordt een van de vorige configmaps verwijderd:

```console
kubectl delete configmaps addon-http-application-routing-nginx-configuration --namespace kube-system
```

Herhaal de vorige `kubectl delete` stap voor alle *invoeg toepassingen-http-toepassings routerings* resources die in uw cluster gebleven zijn.

## <a name="troubleshoot"></a>Problemen oplossen

Gebruik de [kubectl][kubectl-logs] -logboeken opdracht om de toepassings logboeken voor de externe DNS-toepassing weer te geven. De logboeken moeten bevestigen dat een A-en TXT DNS-record is gemaakt.

```
$ kubectl logs -f deploy/addon-http-application-routing-external-dns -n kube-system

time="2018-04-26T20:36:19Z" level=info msg="Updating A record named 'party-clippy' to '52.242.28.189' for Azure DNS zone '471756a6-e744-4aa0-aa01-89c4d162a7a7.canadaeast.aksapp.io'."
time="2018-04-26T20:36:21Z" level=info msg="Updating TXT record named 'party-clippy' to '"heritage=external-dns,external-dns/owner=default"' for Azure DNS zone '471756a6-e744-4aa0-aa01-89c4d162a7a7.canadaeast.aksapp.io'."
```

Deze records kunnen ook worden weer gegeven in de bron van de DNS-zone in de Azure Portal.

![De DNS-records ophalen](media/http-routing/clippy.png)

Gebruik de [kubectl][kubectl-logs] -logboeken opdracht om de toepassings logboeken voor de nginx ingangs controller weer te geven. De logboeken moeten de `CREATE` van een ingangs bron en het opnieuw laden van de controller bevestigen. Alle HTTP-activiteiten worden vastgelegd.

```bash
$ kubectl logs -f deploy/addon-http-application-routing-nginx-ingress-controller -n kube-system

-------------------------------------------------------------------------------
NGINX Ingress controller
  Release:    0.13.0
  Build:      git-4bc943a
  Repository: https://github.com/kubernetes/ingress-nginx
-------------------------------------------------------------------------------

I0426 20:30:12.212936       9 flags.go:162] Watching for ingress class: addon-http-application-routing
W0426 20:30:12.213041       9 flags.go:165] only Ingress with class "addon-http-application-routing" will be processed by this ingress controller
W0426 20:30:12.213505       9 client_config.go:533] Neither --kubeconfig nor --master was specified.  Using the inClusterConfig.  This might not work.
I0426 20:30:12.213752       9 main.go:181] Creating API client for https://10.0.0.1:443
I0426 20:30:12.287928       9 main.go:225] Running in Kubernetes Cluster version v1.8 (v1.8.11) - git (clean) commit 1df6a8381669a6c753f79cb31ca2e3d57ee7c8a3 - platform linux/amd64
I0426 20:30:12.290988       9 main.go:84] validated kube-system/addon-http-application-routing-default-http-backend as the default backend
I0426 20:30:12.294314       9 main.go:105] service kube-system/addon-http-application-routing-nginx-ingress validated as source of Ingress status
I0426 20:30:12.426443       9 stat_collector.go:77] starting new nginx stats collector for Ingress controller running in namespace  (class addon-http-application-routing)
I0426 20:30:12.426509       9 stat_collector.go:78] collector extracting information from port 18080
I0426 20:30:12.448779       9 nginx.go:281] starting Ingress controller
I0426 20:30:12.463585       9 event.go:218] Event(v1.ObjectReference{Kind:"ConfigMap", Namespace:"kube-system", Name:"addon-http-application-routing-nginx-configuration", UID:"2588536c-4990-11e8-a5e1-0a58ac1f0ef2", APIVersion:"v1", ResourceVersion:"559", FieldPath:""}): type: 'Normal' reason: 'CREATE' ConfigMap kube-system/addon-http-application-routing-nginx-configuration
I0426 20:30:12.466945       9 event.go:218] Event(v1.ObjectReference{Kind:"ConfigMap", Namespace:"kube-system", Name:"addon-http-application-routing-tcp-services", UID:"258ca065-4990-11e8-a5e1-0a58ac1f0ef2", APIVersion:"v1", ResourceVersion:"561", FieldPath:""}): type: 'Normal' reason: 'CREATE' ConfigMap kube-system/addon-http-application-routing-tcp-services
I0426 20:30:12.467053       9 event.go:218] Event(v1.ObjectReference{Kind:"ConfigMap", Namespace:"kube-system", Name:"addon-http-application-routing-udp-services", UID:"259023bc-4990-11e8-a5e1-0a58ac1f0ef2", APIVersion:"v1", ResourceVersion:"562", FieldPath:""}): type: 'Normal' reason: 'CREATE' ConfigMap kube-system/addon-http-application-routing-udp-services
I0426 20:30:13.649195       9 nginx.go:302] starting NGINX process...
I0426 20:30:13.649347       9 leaderelection.go:175] attempting to acquire leader lease  kube-system/ingress-controller-leader-addon-http-application-routing...
I0426 20:30:13.649776       9 controller.go:170] backend reload required
I0426 20:30:13.649800       9 stat_collector.go:34] changing prometheus collector from  to default
I0426 20:30:13.662191       9 leaderelection.go:184] successfully acquired lease kube-system/ingress-controller-leader-addon-http-application-routing
I0426 20:30:13.662292       9 status.go:196] new leader elected: addon-http-application-routing-nginx-ingress-controller-5cxntd6
I0426 20:30:13.763362       9 controller.go:179] ingress backend successfully reloaded...
I0426 21:51:55.249327       9 event.go:218] Event(v1.ObjectReference{Kind:"Ingress", Namespace:"default", Name:"party-clippy", UID:"092c9599-499c-11e8-a5e1-0a58ac1f0ef2", APIVersion:"extensions", ResourceVersion:"7346", FieldPath:""}): type: 'Normal' reason: 'CREATE' Ingress default/party-clippy
W0426 21:51:57.908771       9 controller.go:775] service default/party-clippy does not have any active endpoints
I0426 21:51:57.908951       9 controller.go:170] backend reload required
I0426 21:51:58.042932       9 controller.go:179] ingress backend successfully reloaded...
167.220.24.46 - [167.220.24.46] - - [26/Apr/2018:21:53:20 +0000] "GET / HTTP/1.1" 200 234 "" "Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Trident/5.0)" 197 0.001 [default-party-clippy-80] 10.244.0.13:8080 234 0.004 200
```

## <a name="clean-up"></a>Opruimen

Verwijder de gekoppelde Kubernetes-objecten die in dit artikel zijn gemaakt.

```bash
$ kubectl delete -f samples-http-application-routing.yaml

deployment "party-clippy" deleted
service "party-clippy" deleted
ingress "party-clippy" deleted
```

## <a name="next-steps"></a>Volgende stappen

Zie [https ingress on Azure Kubernetes service (AKS) (Engelstalig)][ingress-https]voor meer informatie over het installeren van een met https Beveiligde ingangs controller in AKS.

<!-- LINKS - internal -->
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[az-aks-show]: /cli/azure/aks?view=azure-cli-latest#az-aks-show
[ingress-https]: ./ingress-tls.md
[az-aks-enable-addons]: /cli/azure/aks#az-aks-enable-addons


<!-- LINKS - external -->
[dns-pricing]: https://azure.microsoft.com/pricing/details/dns/
[external-dns]: https://github.com/kubernetes-incubator/external-dns
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-delete]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#delete
[kubectl-logs]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#logs
[ingress]: https://kubernetes.io/docs/concepts/services-networking/ingress/
[ingress-resource]: https://kubernetes.io/docs/concepts/services-networking/ingress/#the-ingress-resource
