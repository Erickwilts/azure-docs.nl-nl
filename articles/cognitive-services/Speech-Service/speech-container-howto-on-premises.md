---
title: Gebruiken met Kubernetes en Helm
titleSuffix: Azure Cognitive Services
description: Door Kubernetes en helm te gebruiken om de installatie kopieën van het spraak naar tekst-en tekst-naar-spraak-container te definiëren, maken we een Kubernetes-pakket. Dit pakket wordt on-premises geïmplementeerd op een Kubernetes-cluster.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 7/16/2019
ms.author: dapine
ms.openlocfilehash: ba292a7d3bdf58ff78764bc2095fdf4a8c486070
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2019
ms.locfileid: "68326212"
---
# <a name="use-with-kubernetes-and-helm"></a>Gebruiken met Kubernetes en Helm

Een optie voor het on-premises beheren van uw spraak containers is het gebruik van Kubernetes en helm. Door Kubernetes en helm te gebruiken om de installatie kopieën van het spraak naar tekst-en tekst-naar-spraak-container te definiëren, maken we een Kubernetes-pakket. Dit pakket wordt on-premises geïmplementeerd op een Kubernetes-cluster. Ten slotte verkennen we hoe u de geïmplementeerde Services en verschillende configuratie opties kunt testen. Zie [Install and run Speech Service containers](speech-container-howto.md)(Engelstalig) voor meer informatie over het uitvoeren van docker-containers zonder Kubernetes-indeling.

## <a name="prerequisites"></a>Vereisten

De volgende vereisten voordat u een on-premises spraak container gebruikt:

|Verplicht|Doel|
|--|--|
| Azure-Account | Als u nog geen abonnement op Azure hebt, maak dan een [gratis account][free-azure-account] aan voordat u begint. |
| Toegang Container Registry | Om ervoor te zorgen dat Kubernetes de docker-installatie kopieën in het cluster kan ophalen, moet u toegang hebben tot het container register. U moet eerst [toegang aanvragen tot het container register][speech-preview-access] . |
| Kubernetes CLI | De [KUBERNETES cli][kubernetes-cli] is vereist voor het beheren van de gedeelde referenties in het container register. Kubernetes is ook vereist voordat helm, de Kubernetes Package Manager. |
| Helm CLI | Als onderdeel van de [helm cli][helm-install] install, you'll also need to initialize Helm, which will install [Tiller][tiller-install]. |
|Spraak resource |Als u deze containers wilt gebruiken, hebt u het volgende nodig:<br><br>Een _spraak_ -Azure-resource om de gekoppelde facturerings sleutel en de URI van het facturerings eindpunt op te halen. Beide waarden zijn beschikbaar op het **spraak** overzicht van de Azure Portal en de pagina sleutels en zijn vereist om de container te starten.<br><br>**{API_KEY}** : resource sleutel<br><br>**{ENDPOINT_URI}** : voor beeld van een eind punt-URI is:`https://westus.api.cognitive.microsoft.com/sts/v1.0`|

## <a name="the-recommended-host-computer-configuration"></a>De aanbevolen configuratie van de hostcomputer

Raadpleeg de computer Details van de [Speech-Service container][speech-container-host-computer] als referentie. In deze *helm-grafiek* worden automatisch de CPU-en geheugen vereisten berekend op basis van het aantal decode (gelijktijdige aanvragen) dat de gebruiker heeft opgegeven. Daarnaast wordt deze aangepast op basis van het feit of optimalisaties voor audio/tekst invoer is `enabled`geconfigureerd als. De helm-grafiek is standaard ingesteld op twee gelijktijdige aanvragen en het uitschakelen van optimalisatie.

| Service | CPU/container | Geheugen/container |
|--|--|--|
| **Spraak naar tekst** | voor een decoder is mini maal 1.150 millicores vereist. Als de `optimizedForAudioFile` is ingeschakeld, zijn 1.950 millicores vereist. (standaard: twee decoders) | Vereist: 2 GB<br>Aantal  4 GB |
| **Tekst naar spraak** | voor één gelijktijdige aanvraag is mini maal 500 millicores vereist. Als de `optimizeForTurboMode` is ingeschakeld, zijn 1.000 millicores vereist. (standaard: twee gelijktijdige aanvragen) | Vereist: 1 GB<br> Aantal 2 GB |

## <a name="connect-to-the-kubernetes-cluster"></a>Verbinding maken met het Kubernetes-cluster

Er wordt naar verwachting een beschik bare Kubernetes-cluster op de hostcomputer. Raadpleeg deze zelf studie over het [implementeren van een Kubernetes-cluster](../../aks/tutorial-kubernetes-deploy-cluster.md) voor een conceptuele uitleg over het implementeren van een Kubernetes-cluster op een hostcomputer.

### <a name="sharing-docker-credentials-with-the-kubernetes-cluster"></a>Docker-referenties delen met het Kubernetes-cluster

Als u het Kubernetes-cluster `docker pull` naar de geconfigureerde installatie kopieën in het `containerpreview.azurecr.io` container register wilt toestaan, moet u de docker-referenties naar het cluster overdragen. Voer de [`kubectl create`][kubectl-create] onderstaande opdracht uit om een *docker-REGI ster* te maken op basis van de referenties die zijn verschaft uit de toegangs vereisten voor container register.

Voer de volgende opdracht uit vanaf de opdracht regel interface van Choice. Zorg ervoor dat u de `<username>`, `<password>`en `<email-address>` vervangt door de container register referenties.

```console
kubectl create secret docker-registry containerpreview \
    --docker-server=containerpreview.azurecr.io \
    --docker-username=<username> \
    --docker-password=<password> \
    --docker-email=<email-address>
```

> [!NOTE]
> Als u al toegang hebt tot het `containerpreview.azurecr.io` container register, kunt u in plaats daarvan een Kubernetes-geheim maken met behulp van de generieke vlag. Bekijk de volgende opdracht die wordt uitgevoerd op basis van de JSON van de docker-configuratie.
> ```console
>  kubectl create secret generic containerpreview \
>      --from-file=.dockerconfigjson=~/.docker/config.json \
>      --type=kubernetes.io/dockerconfigjson
> ```

De volgende uitvoer wordt naar de console afgedrukt wanneer het geheim is gemaakt.

```console
secret "containerpreview" created
```

Als u wilt controleren of het geheim is gemaakt, voert [`kubectl get`][kubectl-get] u uit `secrets` met de vlag.

```console
kuberctl get secrets
```

Bij het `kubectl get secrets` uitvoeren van worden alle geconfigureerde geheimen afgedrukt.

```console
NAME                  TYPE                                  DATA      AGE
containerpreview      kubernetes.io/dockerconfigjson        1         30s
```

## <a name="configure-helm-chart-values-for-deployment"></a>Helm-grafiek waarden voor implementatie configureren

Ga naar de [micro soft helm hub][ms-helm-hub] voor alle openbaar beschik bare helm-grafieken die door micro soft worden aangeboden. Vanuit de micro soft helm Hub vindt u de **on-premises grafiek van Cognitive Services speech**. De **Cognitive Services spraak on-premises** is de grafiek die we installeren, maar we moeten eerst een `config-values.yaml` bestand met expliciete configuraties maken. Laten we beginnen door de micro soft-opslag plaats toe te voegen aan ons helm-exemplaar.

```console
helm repo add microsoft https://microsoft.github.io/charts/repo
```

Nu gaan we onze helm-grafiek waarden configureren. Kopieer en plak de volgende YAML in een bestand met `config-values.yaml`de naam. Zie [helm-grafieken aanpassen](#customize-helm-charts)voor meer informatie over het aanpassen van het **helm-diagram van Cognitive Services speech**. Vervang de `billing` waarden `apikey` en door uw eigen waarde.

```yaml
# These settings are deployment specific and users can provide customizations

# speech-to-text configurations
speechToText:
  enabled: true
  numberOfConcurrentRequest: 3
  optimizeForAudioFile: true
  image:
    registry: containerpreview.azurecr.io
    repository: microsoft/cognitive-services-speech-to-text
    tag: latest
    pullSecrets:
      - containerpreview # Or an existing secret
    args:
      eula: accept
      billing: # < Your billing URL >
      apikey: # < Your API Key >

# text-to-speech configurations
textToSpeech:
  enabled: true
  numberOfConcurrentRequest: 3
  optimizeForTurboMode: true
  image:
    registry: containerpreview.azurecr.io
    repository: microsoft/cognitive-services-text-to-speech
    tag: latest
    pullSecrets:
      - containerpreview # Or an existing secret
    args:
      eula: accept
      billing: # < Your billing URL >
      apikey: # < Your API Key >
```

> [!IMPORTANT]
> Als de `billing` waarden `apikey` en niet worden weer gegeven, verlopen de Services na 15 minuten. De verificatie mislukt ook omdat de services niet beschikbaar zijn.

### <a name="the-kubernetes-package-helm-chart"></a>Het Kubernetes-pakket (helm-grafiek)

Het *helm-diagram* bevat de configuratie van de docker-installatie kopie (n) die `containerpreview.azurecr.io` moet worden opgehaald uit het container register.

> Een [helm-grafiek][helm-charts] is een verzameling bestanden waarin een gerelateerde set Kubernetes-resources wordt beschreven. Eén grafiek kan worden gebruikt om een eenvoudig te implementeren, zoals een memcached Pod, of iets complex, zoals een volledige web-app-stack met HTTP-servers, data bases, caches, enzovoort.

De meegeleverde *helm-grafieken* halen de docker-installatie kopieën van de spraak service, zowel tekst-naar-spraak als de spraak-naar-tekst `containerpreview.azurecr.io` -services uit het container register.

## <a name="install-the-helm-chart-on-the-kubernetes-cluster"></a>De helm-grafiek op het Kubernetes-cluster installeren

Als u de *helm-grafiek* wilt installeren, moet u [`helm install`][helm-install-cmd] de `<config-values.yaml>` opdracht uitvoeren en vervangen door het argument pad en de juiste bestands naam. De `microsoft/cognitive-services-speech-onpremise` helm-grafiek waarnaar hieronder wordt verwezen, is hier beschikbaar in de [micro soft helm-hub][ms-helm-hub-speech-chart].

```console
helm install microsoft/cognitive-services-speech-onpremise \
    --version 0.1.0 \
    --values <config-values.yaml> \
    --name onprem-speech
```

Hier volgt een voorbeeld uitvoer die u kunt verwachten van een geslaagde installatie-uitvoering:

```console
NAME:   onprem-speech
LAST DEPLOYED: Tue Jul  2 12:51:42 2019
NAMESPACE: default
STATUS: DEPLOYED

RESOURCES:
==> v1/Pod(related)
NAME                             READY  STATUS             RESTARTS  AGE
speech-to-text-7664f5f465-87w2d  0/1    Pending            0         0s
speech-to-text-7664f5f465-klbr8  0/1    ContainerCreating  0         0s
text-to-speech-56f8fb685b-4jtzh  0/1    ContainerCreating  0         0s
text-to-speech-56f8fb685b-frwxf  0/1    Pending            0         0s

==> v1/Service
NAME            TYPE          CLUSTER-IP    EXTERNAL-IP  PORT(S)       AGE
speech-to-text  LoadBalancer  10.0.252.106  <pending>    80:31811/TCP  1s
text-to-speech  LoadBalancer  10.0.125.187  <pending>    80:31247/TCP  0s

==> v1beta1/PodDisruptionBudget
NAME                                MIN AVAILABLE  MAX UNAVAILABLE  ALLOWED DISRUPTIONS  AGE
speech-to-text-poddisruptionbudget  N/A            20%              0                    1s
text-to-speech-poddisruptionbudget  N/A            20%              0                    1s

==> v1beta2/Deployment
NAME            READY  UP-TO-DATE  AVAILABLE  AGE
speech-to-text  0/2    2           0          0s
text-to-speech  0/2    2           0          0s

==> v2beta2/HorizontalPodAutoscaler
NAME                       REFERENCE                  TARGETS        MINPODS  MAXPODS  REPLICAS  AGE
speech-to-text-autoscaler  Deployment/speech-to-text  <unknown>/50%  2        10       0         0s
text-to-speech-autoscaler  Deployment/text-to-speech  <unknown>/50%  2        10       0         0s


NOTES:
cognitive-services-speech-onpremise has been installed!
Release is named onprem-speech
```

Het volt ooien van de implementatie van Kubernetes kan enkele minuten duren. Voer de volgende opdracht uit om te controleren of zowel de peulen als de services goed zijn geïmplementeerd en beschikbaar zijn:

```console
kubectl get all
```

U ziet dat er iets lijkt op de volgende uitvoer:

```console
NAME                                  READY     STATUS    RESTARTS   AGE
pod/speech-to-text-7664f5f465-87w2d   1/1       Running   0          34m
pod/speech-to-text-7664f5f465-klbr8   1/1       Running   0          34m
pod/text-to-speech-56f8fb685b-4jtzh   1/1       Running   0          34m
pod/text-to-speech-56f8fb685b-frwxf   1/1       Running   0          34m

NAME                     TYPE           CLUSTER-IP     EXTERNAL-IP      PORT(S)        AGE
service/kubernetes       ClusterIP      10.0.0.1       <none>           443/TCP        3h
service/speech-to-text   LoadBalancer   10.0.252.106   52.162.123.151   80:31811/TCP   34m
service/text-to-speech   LoadBalancer   10.0.125.187   65.52.233.162    80:31247/TCP   34m

NAME                             DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/speech-to-text   2         2         2            2           34m
deployment.apps/text-to-speech   2         2         2            2           34m

NAME                                        DESIRED   CURRENT   READY     AGE
replicaset.apps/speech-to-text-7664f5f465   2         2         2         34m
replicaset.apps/text-to-speech-56f8fb685b   2         2         2         34m

NAME                                                            REFERENCE                   TARGETS   MINPODS   MAXPODS   REPLICAS   AGE
horizontalpodautoscaler.autoscaling/speech-to-text-autoscaler   Deployment/speech-to-text   1%/50%    2         10        2          34m
horizontalpodautoscaler.autoscaling/text-to-speech-autoscaler   Deployment/text-to-speech   0%/50%    2         10        2          34m
```

### <a name="verify-helm-deployment-with-helm-tests"></a>Helm-implementatie met helm-tests controleren

In de geïnstalleerde helm-grafieken worden *helm-tests*gedefinieerd, die als gebruiks gemak fungeren voor verificatie. Met deze tests wordt de service gereedheid gevalideerd. Voor het controleren van de services voor **spraak naar tekst** en **tekst naar spraak** , wordt de [helm-test][helm-test] opdracht uitgevoerd.

```console
helm test onprem-speech
```

> [!IMPORTANT]
> Deze tests mislukken als de pod-status niet `Running` of als de implementatie niet wordt vermeld in de `AVAILABLE` kolom. Neem even de tijd om dit langer dan tien minuten kan duren.

Met deze tests worden verschillende status resultaten uitgevoerd:

```console
RUNNING: speech-to-text-readiness-test
PASSED: speech-to-text-readiness-test
RUNNING: text-to-speech-readiness-test
PASSED: text-to-speech-readiness-test
```

Als alternatief voor het uitvoeren van de *helm-tests*kunt u de *externe IP-* adressen en de bijbehorende poorten verzamelen `kubectl get all` van de opdracht. Open met het IP-adres en de poort een webbrowser en ga `http://<external-ip>:<port>:/swagger/index.html` naar om de API-Swagger-pagina ('s) weer te geven.

## <a name="customize-helm-charts"></a>Helm-grafieken aanpassen

Helm-grafieken zijn hiërarchisch. Als u hiërarchische structuur toestaat, is dit ook van toepassing op het concept van specificiteit, waarbij instellingen die specifiek zijn voor het overschrijven van overgenomen regels.

[!INCLUDE [Speech umbrella-helm-chart-config](includes/speech-umbrella-helm-chart-config.md)]

[!INCLUDE [Speech-to-Text Helm Chart Config](includes/speech-to-text-chart-config.md)]

[!INCLUDE [Text-to-Speech Helm Chart Config](includes/text-to-speech-chart-config.md)]

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het installeren van toepassingen met helm in azure Kubernetes service (AKS) [vindt u hier][installing-helm-apps-in-aks].

> [!div class="nextstepaction"]
> [Cognitive Services containers][cog-svcs-containers]

<!-- LINKS - external -->
[free-azure-account]: https://azure.microsoft.com/free
[git-download]: https://git-scm.com/downloads
[azure-cli]: https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest
[docker-engine]: https://www.docker.com/products/docker-engine
[kubernetes-cli]: https://kubernetes.io/docs/tasks/tools/install-kubectl
[helm-install]: https://helm.sh/docs/using_helm/#installing-helm
[helm-install-cmd]: https://helm.sh/docs/helm/#helm-install
[tiller-install]: https://helm.sh/docs/install/#installing-tiller
[helm-charts]: https://helm.sh/docs/developing_charts
[speech-preview-access]: https://aka.ms/speechcontainerspreview
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[helm-test]: https://helm.sh/docs/helm/#helm-test
[ms-helm-hub]: https://hub.helm.sh/charts/microsoft
[ms-helm-hub-speech-chart]: https://hub.helm.sh/charts/microsoft/cognitive-services-speech-onpremise

<!-- LINKS - internal -->
[speech-container-host-computer]: speech-container-howto.md#the-host-computer
[installing-helm-apps-in-aks]: ../../aks/kubernetes-helm.md
[cog-svcs-containers]: ../cognitive-services-container-support.md
