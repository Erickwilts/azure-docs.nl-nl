---
title: Oplossen van problemen met Azure Container Instances
description: Meer informatie over het oplossen van problemen met Azure Container Instances
services: container-instances
author: dlepow
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 04/25/2019
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: 9dc3e19f9429a6055a799f3f013c732538fa370d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65070855"
---
# <a name="troubleshoot-common-issues-in-azure-container-instances"></a>Algemene problemen in Azure Container Instances oplossen

In dit artikel laat zien hoe algemene problemen voor het beheren of containers implementeren in Azure Container Instances oplossen. Zie ook [Veelgestelde vragen over](container-instances-faq.md).

## <a name="naming-conventions"></a>Naamconventies

Bij het definiëren van uw container-specificatie vereisen bepaalde parameters voldoet aan de naamgeving van beperkingen. Hieronder ziet u een tabel met specifieke vereisten voor container groepseigenschappen. Zie voor meer informatie over naamgevingsregels voor Azure [naamconventies] [ azure-name-restrictions] in het Azure Architecture Center.

| Scope | Lengte | Hoofdlettergebruik | Geldige tekens | Voorgesteld patroon | Voorbeeld |
| --- | --- | --- | --- | --- | --- |
| Naam van container | 1-64 |Niet hoofdlettergevoelig |Alfanumeriek en afbreekstreepje overal behalve de eerste of laatste teken |`<name>-<role>-CG<number>` |`web-batch-CG1` |
| Containernaam | 1-64 |Niet hoofdlettergevoelig |Alfanumeriek en afbreekstreepje overal behalve de eerste of laatste teken |`<name>-<role>-CG<number>` |`web-batch-CG1` |
| Container-poorten | Tussen 1 en 65535 |Geheel getal |Geheel getal tussen 1 en 65535 zijn |`<port-number>` |`443` |
| DNS-naamlabel | 5-63 |Niet hoofdlettergevoelig |Alfanumeriek en afbreekstreepje overal behalve de eerste of laatste teken |`<name>` |`frontend-site1` |
| Omgevingsvariabele | 1-63 |Niet hoofdlettergevoelig |Alfanumeriek en een willekeurige plaats, behalve de eerste of laatste teken, onderstrepingsteken (_) |`<name>` |`MY_VARIABLE` |
| Volumenaam | 5-63 |Niet hoofdlettergevoelig |Kleine letters en cijfers en afbreekstreepjes overal behalve de eerste of laatste teken. Mag niet twee opeenvolgende afbreekstreepjes bevatten. |`<name>` |`batch-output-volume` |

## <a name="os-version-of-image-not-supported"></a>Versie van het besturingssysteem van de installatiekopie niet ondersteund

Als u een installatiekopie die biedt geen ondersteuning voor Azure Container Instances, een `OsVersionNotSupported` fout wordt geretourneerd. De fout is vergelijkbaar met de volgende, waar u `{0}` is de naam van de installatiekopie die u hebt geprobeerd te implementeren:

```json
{
  "error": {
    "code": "OsVersionNotSupported",
    "message": "The OS version of image '{0}' is not supported."
  }
}
```

Deze fout wordt meestal veroorzaakt wanneer implementatie Windows-installatiekopieën die zijn gebaseerd op semi-Annual-kanaal versie 1709 of 1803, die niet worden ondersteund. Zie voor ondersteunde Windows-installatiekopieën in Azure Container Instances, [Veelgestelde vragen over](container-instances-faq.md#what-windows-base-os-images-are-supported).

## <a name="unable-to-pull-image"></a>Kan geen pull-afbeelding

Als u Azure Container Instances is in eerste instantie kan niet voor het ophalen van uw installatiekopie, het opnieuw probeert voor een bepaalde periode. Als de afbeelding pull-bewerking blijft mislukken, ACI uiteindelijk de implementatie mislukt en ziet u mogelijk een `Failed to pull image` fout.

U lost dit probleem, de containerinstantie verwijderen en voer de implementatie opnieuw uit. Zorg ervoor dat de afbeelding bestaat in het register, en u de naam van de installatiekopie juist hebt getypt.

Als de afbeelding kan niet worden opgehaald, gebeurtenissen, zoals hieronder worden weergegeven in de uitvoer van [az container show][az-container-show]:

```bash
"events": [
  {
    "count": 3,
    "firstTimestamp": "2017-12-21T22:56:19+00:00",
    "lastTimestamp": "2017-12-21T22:57:00+00:00",
    "message": "pulling image \"mcr.microsoft.com/azuredocs/aci-hellowrld\"",
    "name": "Pulling",
    "type": "Normal"
  },
  {
    "count": 3,
    "firstTimestamp": "2017-12-21T22:56:19+00:00",
    "lastTimestamp": "2017-12-21T22:57:00+00:00",
    "message": "Failed to pull image \"mcr.microsoft.com/azuredocs/aci-hellowrld\": rpc error: code 2 desc Error: image t/aci-hellowrld:latest not found",
    "name": "Failed",
    "type": "Warning"
  },
  {
    "count": 3,
    "firstTimestamp": "2017-12-21T22:56:20+00:00",
    "lastTimestamp": "2017-12-21T22:57:16+00:00",
    "message": "Back-off pulling image \"mcr.microsoft.com/azuredocs/aci-hellowrld\"",
    "name": "BackOff",
    "type": "Normal"
  }
],
```

## <a name="container-continually-exits-and-restarts-no-long-running-process"></a>Container voortdurend wordt afgesloten en opnieuw wordt opgestart (geen langdurig proces)

Containergroepen standaard ingesteld op een [beleid voor opnieuw opstarten](container-instances-restart-policy.md) van **altijd**, zodat de containers in containergroep altijd opnieuw opstarten nadat ze volledig worden uitgevoerd. Mogelijk moet u deze optie om te wijzigen **OnFailure** of **nooit** als u van plan bent om uit te voeren van containers op basis van een taak. Als u opgeeft **OnFailure** en nog steeds Zie continu opnieuw wordt opgestart, kan er een probleem met de toepassing of het script dat wordt uitgevoerd in de container.

Bij het uitvoeren van containergroepen zonder langlopende processen ziet u mogelijk herhaalde wordt afgesloten en opnieuw opstarten met de installatiekopieën, zoals Ubuntu of Alpine. Verbinding maken via [EXEC](container-instances-exec.md) werkt niet naar de container heeft geen proces dat deze actief te houden. U lost dit probleem, omvatten een opdracht start als volgt met uw implementatie van de groep container naar de container actief blijft.

```azurecli-interactive
## Deploying a Linux container
az container create -g MyResourceGroup --name myapp --image ubuntu --command-line "tail -f /dev/null"
```

```azurecli-interactive 
## Deploying a Windows container
az container create -g myResourceGroup --name mywindowsapp --os-type Windows --image mcr.microsoft.com/windows/servercore:ltsc2019
 --command-line "ping -t localhost"
```

De Container Instances API en Azure portal bevat een `restartCount` eigenschap. Om te controleren of het aantal herstarten van apparaten voor een container, kunt u de [az container show] [ az-container-show] opdracht in de Azure CLI. In het volgende voorbeeld uitvoert (die is afgekapt voor beknoptheid), ziet u de `restartCount` eigenschap aan het einde van de uitvoer.

```json
...
 "events": [
   {
     "count": 1,
     "firstTimestamp": "2017-11-13T21:20:06+00:00",
     "lastTimestamp": "2017-11-13T21:20:06+00:00",
     "message": "Pulling: pulling image \"myregistry.azurecr.io/aci-tutorial-app:v1\"",
     "type": "Normal"
   },
   {
     "count": 1,
     "firstTimestamp": "2017-11-13T21:20:14+00:00",
     "lastTimestamp": "2017-11-13T21:20:14+00:00",
     "message": "Pulled: Successfully pulled image \"myregistry.azurecr.io/aci-tutorial-app:v1\"",
     "type": "Normal"
   },
   {
     "count": 1,
     "firstTimestamp": "2017-11-13T21:20:14+00:00",
     "lastTimestamp": "2017-11-13T21:20:14+00:00",
     "message": "Created: Created container with id bf25a6ac73a925687cafcec792c9e3723b0776f683d8d1402b20cc9fb5f66a10",
     "type": "Normal"
   },
   {
     "count": 1,
     "firstTimestamp": "2017-11-13T21:20:14+00:00",
     "lastTimestamp": "2017-11-13T21:20:14+00:00",
     "message": "Started: Started container with id bf25a6ac73a925687cafcec792c9e3723b0776f683d8d1402b20cc9fb5f66a10",
     "type": "Normal"
   }
 ],
 "previousState": null,
 "restartCount": 0
...
}
```

> [!NOTE]
> Een shell, zoals bash, instellen de meeste containerinstallatiekopieën voor Linux-distributies als de standaardopdracht. Omdat een shell op een eigen niet een langlopende-service, deze containers onmiddellijk afsluiten en kunnen worden onderverdeeld in een lus voor opnieuw opstarten wanneer geconfigureerd met de standaard **altijd** beleid voor opnieuw opstarten.

## <a name="container-takes-a-long-time-to-start"></a>Container duurt lang om te starten

De twee primaire factoren die aan de opstarttijd container in Azure Container Instances bijdragen zijn:

* [De grootte van installatiekopie](#image-size)
* [Installatiekopie-locatie](#image-location)

Windows-installatiekopieën hebben [aanvullende overwegingen](#cached-images).

### <a name="image-size"></a>De grootte van installatiekopie

Als uw container lang duurt om te starten, maar uiteindelijk slaagt, starten door te kijken naar de grootte van uw containerinstallatiekopie. Omdat Azure Container Instances uw containerinstallatiekopie op aanvraag haalt, wordt de opstarttijd u ziet direct gerelateerd aan de grootte.

U kunt de grootte van uw containerinstallatiekopie weergeven met behulp van de `docker images` opdracht in de Docker-CLI:

```console
$ docker images
REPOSITORY                                    TAG       IMAGE ID        CREATED          SIZE
mcr.microsoft.com/azuredocs/aci-helloworld    latest    7367f3256b41    15 months ago    67.6MB
```

De sleutel voor de grootte klein te houden is ervoor te zorgen dat uw uiteindelijke installatiekopie bevat alles wat is niet vereist zijn tijdens runtime. Een manier om te doen dit is met [meerfasige builds][docker-multi-stage-builds]. Meerdere fasen bouwt maken het eenvoudig om ervoor te zorgen dat de uiteindelijke installatiekopie bevat alleen de artefacten die u nodig hebt voor uw toepassing en niet een van de extra inhoud die is vereist tijdens het opbouwen.

### <a name="image-location"></a>Installatiekopie-locatie

Een andere manier om te beperken de gevolgen voor de pull-installatiekopie op de opstarttijd van de container is voor het hosten van de containerinstallatiekopie in [Azure Container Registry](/azure/container-registry/) in dezelfde regio waar u van plan bent om te containerinstanties implementeren. Dit verkort de netwerkpad dat de container-installatiekopie moet worden verzonden, aanzienlijk verkorten van de downloadtijd.

### <a name="cached-images"></a>In de cache opgeslagen afbeeldingen

Azure Container Instances gebruikt een cachemechanisme om te helpen snelheid container opstarttijd voor afbeeldingen die is gebouwd op common [Windows baseren installatiekopieën](container-instances-faq.md#what-windows-base-os-images-are-supported), waaronder `nanoserver:1809`, `servercore:ltsc2019`, en `servercore:1809`. Gebruikte Linux-installatiekopieën, zoals `ubuntu:1604` en `alpine:3.6` worden ook in de cache opgeslagen. Voor een bijgewerkte lijst van in de cache installatiekopieën en tags, gebruikt u de [in de cache installatiekopieën vermelden] [ list-cached-images] API.

> [!NOTE]
> Gebruik van Windows Server 2019-installatiekopieën in Azure Container Instances is in preview.

### <a name="windows-containers-slow-network-readiness"></a>Gereedheid voor Windows-containers langzaam netwerk

Eerste gemaakt hebben Windows-containers geen binnenkomende of uitgaande connectiviteit tot 30 seconden (of langer, in uitzonderlijke gevallen). Als uw containertoepassing een internetverbinding moet, vertraging toevoegen en Pogingslogica om toe te staan van 30 seconden tot stand brengen van verbinding met Internet. Na de initiële configuratie, moet op de juiste wijze containernetwerken hervat.

## <a name="resource-not-available-error"></a>Resource niet beschikbaar-fout

Vanwege de verschillende regionale resource laden in Azure, ontvangt u mogelijk de volgende fout wanneer u probeert om een containerexemplaar te implementeren:

`The requested resource with 'x' CPU and 'y.z' GB memory is not available in the location 'example region' at this moment. Please retry with a different resource request or in another location.`

Deze fout geeft aan dat vanwege een zware belasting in de regio waarin u probeert te implementeren, de resources die zijn opgegeven voor de container kunnen niet worden toegewezen op dat moment. Een of meer van de volgende stappen voor risicobeperking gebruiken voor het oplossen van uw probleem.

* Controleer of de implementatie-instellingen van uw container vallen binnen de gedefinieerde parameters in [beschikbaarheid in regio's voor Azure Container Instances](container-instances-region-availability.md)
* Geef instellingen op lagere CPU en geheugen voor de container
* Implementeren naar een andere Azure-regio
* Implementeren op een later tijdstip

## <a name="cannot-connect-to-underlying-docker-api-or-run-privileged-containers"></a>Kan geen verbinding maken met onderliggende Docker API of bevoegde containers uitvoeren

Azure Container Instances maakt niet beschikbaar voor directe toegang tot de onderliggende infrastructuur die als host fungeert voor groepen met containers. Dit omvat toegang tot de Docker-API die wordt uitgevoerd op de host van de container en bevoorrechte containers uitvoeren. Als u nodig hebt voor interactie met Docker, controleert u de [REST-referentiedocumentatie](https://aka.ms/aci/rest) om te zien wat de ACI API ondersteunt. Als er iets ontbreekt, een aanvraag indienen op de [ACI feedbackforums](https://aka.ms/aci/feedback).

## <a name="ips-may-not-be-accessible-due-to-mismatched-ports"></a>IP-adressen mogelijk niet toegankelijk omdat het niet-overeenkomende poorten

Azure Container Instances biedt momenteel geen ondersteuning poort toewijzen, zoals met de configuratie van de reguliere docker, maar deze hotfix op de planning is. Als u het IP-adressen zijn niet toegankelijk als u denkt dat deze moet worden gevonden, controleert u of u hebt geconfigureerd om de containerinstallatiekopie om te luisteren naar de dezelfde poorten die u beschikbaar in uw containergroep met maakt de `ports` eigenschap.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [containerlogboeken en -gebeurtenissen ophalen](container-instances-get-logs.md) om u te helpen bij foutopsporing van uw containers.

<!-- LINKS - External -->
[azure-name-restrictions]: https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions#naming-rules-and-restrictions
[windows-sac-overview]: https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview
[docker-multi-stage-builds]: https://docs.docker.com/engine/userguide/eng-image/multistage-build/
[docker-hub-windows-core]: https://hub.docker.com/_/microsoft-windows-servercore
[docker-hub-windows-nano]: https://hub.docker.com/_/microsoft-windows-nanoserver

<!-- LINKS - Internal -->
[az-container-show]: /cli/azure/container#az-container-show
[list-cached-images]: /rest/api/container-instances/listcachedimages
