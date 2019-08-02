---
title: Een Basic ingangs controller maken in azure Kubernetes service (AKS)
description: Meer informatie over het installeren en configureren van een Basic NGINX ingress-controller in een Azure Kubernetes service-cluster (AKS).
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 05/24/2019
ms.author: mlearned
ms.openlocfilehash: 1e5c3aa7ed4ec990dba07fb24830fae243141ad5
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "67615584"
---
# <a name="create-an-ingress-controller-in-azure-kubernetes-service-aks"></a>Een ingangs controller maken in azure Kubernetes service (AKS)

Een controller voor inkomend verkeer is een stukje software dat omgekeerde proxy’s, configureerbare verkeersroutering en TLS-beëindiging voor Kubernetes-services biedt. Kubernetes-resources voor inkomend verkeer worden gebruikt om de regels en routes voor uitgaand verkeer worden geconfigureerd voor individuele Kubernetes-services. Met behulp van een controller en regels voor inkomend verkeer kan er één enkel IP-adres worden gebruikt voor het routeren van verkeer naar meerdere services in een Kubernetes-cluster.

In dit artikel wordt beschreven hoe u de [NGINX ingress-controller][nginx-ingress] implementeert in een Azure Kubernetes service-cluster (AKS). Vervolgens worden er twee toepassingen uitgevoerd in het AKS-cluster, die allemaal toegankelijk zijn via het ene IP-adres.

U kunt ook het volgende doen:

- [De invoeg toepassing voor het routeren van HTTP-toepassingen inschakelen][aks-http-app-routing]
- [Een ingangs controller maken die gebruikmaakt van een intern, privé netwerk en IP-adres][aks-ingress-internal]
- [Een ingangs controller maken die gebruikmaakt van uw eigen TLS-certificaten][aks-ingress-own-tls]
- Een ingangs controller maken die gebruikmaakt van een versleuteling om automatisch TLS-certificaten te genereren [met een dynamisch openbaar IP-adres][aks-ingress-tls] of [met een statisch openbaar IP-adres][aks-ingress-static-tls]

## <a name="before-you-begin"></a>Voordat u begint

In dit artikel wordt gebruikgemaakt van helm voor het installeren van de NGINX ingress-controller, CERT-Manager en een voor beeld-web-app. U moet helm hebben geïnitialiseerd binnen uw AKS-cluster en gebruikmaken van een service account voor Tiller. Zie [Installing Applications with helm in azure Kubernetes service (AKS) (Engelstalig)][use-helm]voor meer informatie over het configureren en gebruiken van helm.

Voor dit artikel moet u ook de Azure CLI-versie 2.0.64 of hoger uitvoeren. Voer `az --version` uit om de versie te bekijken. Zie [Azure CLI installeren][azure-cli-install] als u de CLI wilt installeren of een upgrade wilt uitvoeren.

## <a name="create-an-ingress-controller"></a>Een ingangs controller maken

Als u de ingangs controller wilt maken `Helm` , gebruikt u voor het installeren van *nginx-ingang*. Voor toegevoegde redundantie worden twee replica's van de NGINX ingress-controllers geïmplementeerd met de `--set controller.replicaCount` para meter. Om volledig te profiteren van het uitvoeren van replica's van de ingangs controller, moet u ervoor zorgen dat er meer dan één knoop punt in uw AKS-cluster is.

De ingangs controller moet ook worden gepland op een Linux-knoop punt. Windows Server-knoop punten (momenteel in de preview-versie van AKS) mogen de ingangs controller niet uitvoeren. Een knooppunt kiezer wordt opgegeven met behulp `--set nodeSelector` van de para meter om aan te geven dat de Kubernetes scheduler de NGINX ingangs controller moet uitvoeren op een Linux-knoop punt.

> [!TIP]
> In het volgende voor beeld wordt een Kubernetes-naam ruimte gemaakt voor de ingangs resources met de naam *ingress-Basic*. Geef waar nodig een naam ruimte op voor uw eigen omgeving. Als op uw AKS-cluster geen RBAC is ingeschakeld `--set rbac.create=false` , voegt u toe aan de helm-opdrachten.

> [!TIP]
> Als u [IP-behoud van client bronnen][client-source-ip] wilt inschakelen voor aanvragen voor containers in uw cluster, voegt `--set controller.service.externalTrafficPolicy=Local` u toe aan de helm-installatie opdracht. Het bron-IP-adres van de client wordt opgeslagen in de aanvraag header onder *X-doorgestuurd-voor*. Bij gebruik van een ingangs controller waarvoor IP-behoud door client bronnen is ingeschakeld, werkt SSL Pass-Through niet.

```console
# Create a namespace for your ingress resources
kubectl create namespace ingress-basic

# Use Helm to deploy an NGINX ingress controller
helm install stable/nginx-ingress \
    --namespace ingress-basic \
    --set controller.replicaCount=2 \
    --set controller.nodeSelector."beta\.kubernetes\.io/os"=linux \
    --set defaultBackend.nodeSelector."beta\.kubernetes\.io/os"=linux
```

Wanneer de Kubernetes load balancer-service is gemaakt voor de NGINX ingress-controller, wordt een dynamisch openbaar IP-adres toegewezen, zoals wordt weer gegeven in de volgende voorbeeld uitvoer:

```
$ kubectl get service -l app=nginx-ingress --namespace ingress-basic

NAME                                                 TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)                      AGE
aspiring-labradoodle-nginx-ingress-controller        LoadBalancer   10.0.61.144    40.117.74.8   80:30386/TCP,443:32276/TCP   6m2s
aspiring-labradoodle-nginx-ingress-default-backend   ClusterIP      10.0.192.145   <none>        80/TCP                       6m2s
```

Er zijn nog geen regels voor binnenkomend verkeer gemaakt, dus de standaard 404-pagina van de NGINX ingress-controller wordt weer gegeven als u naar het interne IP-adres bladert. Ingangs regels worden in de volgende stappen geconfigureerd.

## <a name="run-demo-applications"></a>Demo toepassingen uitvoeren

Als u de ingangs controller in actie wilt zien, gaan we twee demonstratie toepassingen uitvoeren in uw AKS-cluster. In dit voor beeld wordt helm gebruikt om twee exemplaren van een eenvoudige ' Hallo wereld '-toepassing te implementeren.

Voordat u de voor beelden van helm-grafieken kunt installeren, moet u de opslag plaats voor Azure samples als volgt toevoegen aan uw helm-omgeving:

```console
helm repo add azure-samples https://azure-samples.github.io/helm-charts/
```

Maak de eerste demo toepassing vanuit een helm-diagram met de volgende opdracht:

```console
helm install azure-samples/aks-helloworld --namespace ingress-basic
```

Installeer nu een tweede exemplaar van de voorbeeld toepassing. Voor het tweede exemplaar geeft u een nieuwe titel op zodat de twee toepassingen visueel worden onderscheiden. U geeft ook een unieke service naam op:

```console
helm install azure-samples/aks-helloworld \
    --namespace ingress-basic \
    --set title="AKS Ingress Demo" \
    --set serviceName="ingress-demo"
```

## <a name="create-an-ingress-route"></a>Een ingangs route maken

Beide toepassingen worden nu uitgevoerd op uw Kubernetes-cluster. Als u verkeer naar elke toepassing wilt routeren, maakt u een Kubernetes-ingangs bron. De ingangs resource configureert de regels die verkeer routeren naar een van de twee toepassingen.

In het volgende voor beeld wordt verkeer naar het `http://40.117.74.8/` adres gerouteerd naar de service met de `aks-helloworld`naam. Verkeer naar het adres `http://40.117.74.8/hello-world-two` wordt doorgestuurd naar de `ingress-demo` service.

Maak een bestand met `hello-world-ingress.yaml` de naam en kopieer het in het volgende voor beeld YAML.

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: hello-world-ingress
  namespace: ingress-basic
  annotations:
    kubernetes.io/ingress.class: nginx
    nginx.ingress.kubernetes.io/ssl-redirect: "false"
    nginx.ingress.kubernetes.io/rewrite-target: /$1
spec:
  rules:
  - http:
      paths:
      - backend:
          serviceName: aks-helloworld
          servicePort: 80
        path: /(.*)
      - backend:
          serviceName: ingress-demo
          servicePort: 80
        path: /hello-world-two(/|$)(.*)
```

Maak de ingangs resource met behulp van de `kubectl apply -f hello-world-ingress.yaml` opdracht.

```
$ kubectl apply -f hello-world-ingress.yaml

ingress.extensions/hello-world-ingress created
```

## <a name="test-the-ingress-controller"></a>De ingangs controller testen

Als u de routes voor de ingangs controller wilt testen, bladert u naar de twee toepassingen. Open een webbrowser op het IP-adres van uw NGINX ingress-controller, zoals *http://40.117.74.8* . De eerste demo toepassing wordt weer gegeven in de webbrowser, zoals wordt weer gegeven in het volgende voor beeld:

![De eerste app die achter de ingangs controller wordt uitgevoerd](media/ingress-basic/app-one.png)

Voeg nu het */Hello-World-Two* -pad toe aan het IP-adres *http://40.117.74.8/hello-world-two* , zoals. De tweede demo toepassing met de aangepaste titel wordt weer gegeven:

![Tweede app die achter de ingangs controller wordt uitgevoerd](media/ingress-basic/app-two.png)

## <a name="clean-up-resources"></a>Resources opschonen

In dit artikel wordt helm gebruikt voor het installeren van de ingangs onderdelen en voor beeld-apps. Wanneer u een helm-grafiek implementeert, worden er een aantal Kubernetes-resources gemaakt. Deze resources omvatten peulen, implementaties en services. Als u deze resources wilt opschonen, kunt u de volledige voorbeeld naam ruimte of de afzonderlijke resources verwijderen.

### <a name="delete-the-sample-namespace-and-all-resources"></a>De voorbeeld naam ruimte en alle resources verwijderen

Als u de volledige voorbeeld naam ruimte wilt verwijderen `kubectl delete` , gebruikt u de opdracht en geeft u de naam van de naam ruimte op. Alle resources in de naam ruimte worden verwijderd.

```console
kubectl delete namespace ingress-basic
```

Verwijder vervolgens de helm-opslag plaats voor de AKS Hallo wereld-app:

```console
helm repo remove azure-samples
```

### <a name="delete-resources-individually"></a>Resources afzonderlijk verwijderen

U kunt ook een nauw keurigere benadering van de gemaakte afzonderlijke resources verwijderen. Vermeld de helm-releases met `helm list` de opdracht. Zoek naar grafieken met de naam *nginx-ingangs* en *AKS-HelloWorld*, zoals wordt weer gegeven in de volgende voorbeeld uitvoer:

```
$ helm list

NAME                    REVISION    UPDATED                     STATUS      CHART                   APP VERSION NAMESPACE
aspiring-labradoodle    1           Wed Mar 27 19:55:37 2019    DEPLOYED    nginx-ingress-1.3.1     0.22.0      ingress-basic
esteemed-koala          1           Wed Mar 27 19:59:18 2019    DEPLOYED    aks-helloworld-0.1.0                ingress-basic
wonderful-puma          1           Wed Mar 27 19:59:07 2019    DEPLOYED    aks-helloworld-0.1.0                ingress-basic
```

Verwijder de releases met de `helm delete` opdracht. In het volgende voor beeld worden de implementaties van NGINX-inkomend en de twee voor beelden van de AKS Hello World-apps verwijderd.

```
$ helm delete aspiring-labradoodle esteemed-koala wonderful-puma

release "aspiring-labradoodle" deleted
release "esteemed-koala" deleted
release "wonderful-puma" deleted
```

Verwijder vervolgens de helm-opslag plaats voor de AKS Hallo wereld-app:

```console
helm repo remove azure-samples
```

Verwijder de ingangs route die het verkeer naar de voor beeld-apps doorstuurt:

```console
kubectl delete -f hello-world-ingress.yaml
```

Ten slotte kunt u de zelf naam ruimte verwijderen. Gebruik de `kubectl delete` opdracht en geef de naam van de naam ruimte op:

```console
kubectl delete namespace ingress-basic
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel zijn enkele externe onderdelen opgenomen in AKS. Zie de volgende project pagina's voor meer informatie over deze onderdelen:

- [Helm CLI][helm-cli]
- [NGINX ingress-controller][nginx-ingress]

U kunt ook het volgende doen:

- [De invoeg toepassing voor het routeren van HTTP-toepassingen inschakelen][aks-http-app-routing]
- [Een ingangs controller maken die gebruikmaakt van een intern, privé netwerk en IP-adres][aks-ingress-internal]
- [Een ingangs controller maken die gebruikmaakt van uw eigen TLS-certificaten][aks-ingress-own-tls]
- Een ingangs controller maken die gebruikmaakt van een versleuteling om automatisch TLS-certificaten te genereren [met een dynamisch openbaar IP-adres][aks-ingress-tls] of [met een statisch openbaar IP-adres][aks-ingress-static-tls]

<!-- LINKS - external -->
[helm-cli]: https://docs.microsoft.com/azure/aks/kubernetes-helm
[nginx-ingress]: https://github.com/kubernetes/ingress-nginx

<!-- LINKS - internal -->
[use-helm]: kubernetes-helm.md
[azure-cli-install]: /cli/azure/install-azure-cli
[aks-ingress-internal]: ingress-internal-ip.md
[aks-ingress-tls]: ingress-tls.md
[aks-ingress-static-tls]: ingress-static-ip.md
[aks-http-app-routing]: http-application-routing.md
[aks-ingress-own-tls]: ingress-own-tls.md
[client-source-ip]: concepts-network.md#ingress-controllers
