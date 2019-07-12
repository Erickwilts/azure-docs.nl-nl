---
title: App Service on Linux FAQ - Azure | Microsoft Docs
description: Azure App Service on Linux Veelgestelde vragen over.
keywords: Azure appservice, web-app, veelgestelde vragen over, linux, oss, web-app voor containers, meerdere containers, multicontainer
services: app-service
documentationCenter: ''
author: msangapu-msft
manager: stefsch
editor: ''
ms.assetid: ''
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/30/2018
ms.author: msangapu
ms.custom: seodec18
ms.openlocfilehash: ec571555415a912a31b094722bd47f67210a0372
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2019
ms.locfileid: "67617359"
---
# <a name="azure-app-service-on-linux-faq"></a>Azure App Service on Linux Veelgestelde vragen

Er wordt geprobeerd met het uitbrengen van App Service onder Linux, over het toevoegen van functies en verbeteringen aangebracht aan ons platform. In dit artikel vindt u antwoorden op vragen die onze klanten die is aangevraagd door ons onlangs.

Als u een vraag hebt, een reactie op in dit artikel.

## <a name="built-in-images"></a>Ingebouwde installatiekopieën

**Ik wil een fork van de ingebouwde Docker-containers die het platform biedt. Waar kan ik deze bestanden vinden?**

U vindt alle Docker-bestanden op [GitHub](https://github.com/azure-app-service). U vindt alle Docker-containers op [Docker Hub](https://hub.docker.com/u/appsvc/).

<a id="#startup-file"></a>

**Wat zijn de verwachte waarden voor de sectie opstartbestand wanneer ik de runtimestack configureren?**

| Stack           | Verwachte waarde                                                                         |
|-----------------|----------------------------------------------------------------------------------------|
| Java SE         | de opdracht om het JAR-app te starten (bijvoorbeeld `java -jar my-app.jar --server.port=80`) |
| Gebruik van Tomcat, Wildfly | de locatie van een script om uit te voeren van een vereiste configuratie-instellingen (bijvoorbeeld `/home/site/deployments/tools/startup_script.sh`)          |
| Node.js         | het PM2-configuratiebestand of het scriptbestand                                |
| .NET core       | de naam van de gecompileerde dll-bestand als `dotnet <myapp>.dll`                                 |
| Ruby            | de Ruby-script dat u wilt uw app met initialiseren                     |

Deze opdrachten of scripts worden uitgevoerd nadat de ingebouwde Docker-container wordt gestart, maar voordat u uw toepassing code is gestart.

## <a name="management"></a>Beheer

**Wat gebeurt er wanneer ik klik op de knop opnieuw opstarten in Azure portal?**

Deze actie is hetzelfde als een Docker-opnieuw opstarten.

**Kan ik Secure Shell (SSH) gebruiken voor het verbinding maken met de app-container virtuele machine (VM)?**

Ja, kunt u dat doen via de site source control management (SCM).

> [!NOTE]
> U kunt ook rechtstreeks vanaf uw lokale ontwikkelcomputer verbinding maken met de app-container via SSH, SFTP of Visual Studio Code (voor live foutopsporing gebruikt u Node.js-apps). Zie [Foutopsporing op afstand en SSH in App Service in Linux](https://aka.ms/linux-debug) voor meer informatie.
>

**Hoe kan ik een Linux App Service-abonnement via een SDK of een Azure Resource Manager-sjabloon maken?**

U moet instellen de **gereserveerde** veld van de appservice om *waar*.

## <a name="continuous-integration-and-deployment"></a>Continue integratie en implementatie

**Mijn WebApp gebruikt nog steeds een oude Docker-containerinstallatiekopie nadat ik de afbeelding op Docker Hub hebt bijgewerkt. Ondersteunen u continue integratie en implementatie van aangepaste containers?**

Ja, instellen van continue integratie/implementatie voor Azure Container Registry of DockerHub, door volgende [doorlopende implementatie met Web App for Containers](./app-service-linux-ci-cd.md). U kunt de container voor persoonlijke registers vernieuwen door te stoppen en vervolgens uw web-app te starten. Of u kunt wijzigen of toevoegen van een dummy toepassingsinstelling om af te dwingen een vernieuwing van de container.

**Faseringsomgeving ondersteund?**

Ja.

**Kan ik gebruiken *WebDeploy/MSDeploy* mijn web-app implementeren?**

Ja, moet u een app-instelling met de naam instellen `WEBSITE_WEBDEPLOY_USE_SCM` naar *false*.

**GIT-implementatie van mijn toepassing mislukt bij het gebruik van Linux-web-app. Hoe kan ik het probleem omzeilen?**

Als uw Linux-web-app niet Git-implementatie, kies een van de volgende opties om de toepassingscode van uw te implementeren:

- Gebruik de functie continue levering (Preview): U kunt de broncode van uw app opslaan in een Azure DevOps Git-repo of GitHub-opslagplaats naar Azure onafgebroken levering gebruikt. Zie voor meer informatie, [continue levering configureren voor Linux-web-app](https://blogs.msdn.microsoft.com/devops/2017/05/10/use-azure-portal-to-setup-continuous-delivery-for-web-app-on-linux/).

- Gebruik de [ZIP API implementeren](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file): Gebruik deze API [SSH in uw web-app](https://docs.microsoft.com/azure/app-service/containers/app-service-linux-ssh-support) en Ga naar de map waar u om uw code te implementeren. Voer de volgende code:

   ```bash
   curl -X POST -u <user> --data-binary @<zipfile> https://{your-sitename}.scm.azurewebsites.net/api/zipdeploy
   ```

   Als u een foutmelding dat krijgt het `curl` opdracht is niet gevonden, zorg ervoor dat u curl installeren met behulp van `apt-get install curl` voordat u de vorige `curl` opdracht.

## <a name="language-support"></a>Taalondersteuning

**Ik wil websockets in mijn Node.js-toepassing, eventuele speciale instellingen of configuraties gebruiken om in te stellen?**

Ja, uitschakelen `perMessageDeflate` in uw Node.js-code op de server. Bijvoorbeeld, als u met behulp van socket.io, gebruik de volgende code:

```nodejs
const io = require('socket.io')(server,{
  perMessageDeflate :false
});
```

**Niet-gecompileerde .NET Core-apps ondersteund?**

Ja.

**Ondersteund Composer als een voor Afhankelijkheidsbeheer voor PHP-apps?**

Tijdens een Git-implementatie moet Kudu Ja, detecteert dat u een PHP-toepassing (dankzij de aanwezigheid van een bestand composer.lock) implementeert en Kudu wordt vervolgens geactiveerd voor een composer-installatie.

## <a name="custom-containers"></a>Aangepaste containers

**Ik ben mijn eigen aangepaste container gebruiken. Ik wil het platform, een SMB-share koppelen voor de `/home/` directory.**

Als `WEBSITES_ENABLE_APP_SERVICE_STORAGE` instelling is **niet nader omschreven** of is ingesteld op *waar*, wordt de `/home/` directory **wordt gedeeld** tussen exemplaren van de schaal, en bestanden die worden weggeschreven **blijven behouden** via opnieuw wordt opgestart. Expliciet `WEBSITES_ENABLE_APP_SERVICE_STORAGE` naar *false* het koppelen wordt uitgeschakeld.

**Mijn aangepaste container lang duurt om te starten en het platform de container opnieuw wordt opgestart voordat het is gestart.**

U kunt de hoeveelheid tijd die het platform wordt gewacht voordat opnieuw wordt gestart van de container configureren. Om dit te doen, stel de `WEBSITES_CONTAINER_START_TIME_LIMIT` app-instelling op de gewenste waarde. De standaardwaarde is 230 seconden en de maximumwaarde is 1800 seconden.

**Wat is de indeling voor de URL van de persoonlijke register-server?**

Geef de URL volledige register met inbegrip van `http://` of `https://`.

**Wat is de indeling voor de naam van de installatiekopie in de optie persoonlijk register?**

De naam van de volledige installatiekopie, met inbegrip van het persoonlijke register-URL (bijvoorbeeld myacr.azurecr.io/dotnet:latest) toevoegen. Afbeelding van namen die gebruikmaken van een aangepaste poort [kan niet worden ingevoerd via de portal](https://feedback.azure.com/forums/169385-web-apps/suggestions/31304650). Om in te stellen `docker-custom-image-name`, gebruikt u de [ `az` opdrachtregelprogramma](https://docs.microsoft.com/cli/azure/webapp/config/container?view=azure-cli-latest#az-webapp-config-container-set).

**Kan ik meer dan één poort weergeven op mijn aangepaste containerinstallatiekopie?**

We bieden geen ondersteuning meer dan één poort.

**Kan ik mijn eigen opslag overbrengen?**

Ja, [toevoegen van uw eigen opslag](https://docs.microsoft.com/azure/app-service/containers/how-to-serve-content-from-azure-storage) is beschikbaar als preview.

**Waarom kan ik mijn aangepaste container bestand system of met processen van de SCM-site niet doorzoeken?**

De SCM-site wordt uitgevoerd in een afzonderlijke container. De bestand system of met de processen van de app-container kan niet worden gecontroleerd.

**Mijn aangepaste container luistert naar een andere poort dan poort 80. Hoe kan ik mijn app voor het routeren van aanvragen naar die poort configureren?**

We hebben poort automatisch detecteren. U kunt ook een app-instelling met de naam opgeven *WEBSITES_PORT* en wijs hieraan de waarde van het verwachte poortnummer. Voorheen was het platform dat wordt gebruikt de *poort* app-instelling. Binnenkort wordt op termijn afschaffen van deze app-instelling en het gebruik van *WEBSITES_PORT* exclusief.

**Heb ik nodig voor het implementeren van HTTPS in Mijn aangepaste container?**

Nee, het platform verwerkt voor HTTPS-beëindiging op de gedeelde front-ends.

## <a name="multi-container-with-docker-compose"></a>Meerdere containers met Docker Compose

**Hoe configureer ik Azure Container Registry (ACR) om met meerdere containers te gebruiken?**

Als u wilt ACR gebruiken met meerdere containers **alle containerinstallatiekopieën** moet worden gehost op dezelfde server voor de ACR-register. Zodra ze zich op dezelfde Registerserver bevinden, moet u toepassingsinstellingen maken en vervolgens het configuratiebestand Docker Compose zodanig dat de naam van de ACR-installatiekopie bijwerken.

Maak de volgende instellingen:

- DOCKER_REGISTRY_SERVER_USERNAME
- DOCKER_REGISTRY_SERVER_URL (volledige URL, bijvoorbeeld: `https://<server-name>.azurecr.io`)
- DOCKER_REGISTRY_SERVER_PASSWORD (beheerderstoegang in de ACR-instellingen inschakelen)

In het configuratiebestand verwijzen naar uw ACR-installatiekopie, zoals in het volgende voorbeeld:

```yaml
image: <server-name>.azurecr.io/<image-name>:<tag>
```

**Hoe weet ik welke container is toegankelijk is via internet?**

- Slechts één container kan worden geopend voor toegang
- Alleen poort 80 en 8080 is toegankelijk (blootgestelde poorten)

Hier volgen de regels om te bepalen waarop de container is toegankelijk - in volgorde van prioriteit:

- Toepassingsinstelling `WEBSITES_WEB_CONTAINER_NAME` ingesteld op de containernaam van de
- De eerste container voor het definiëren van poort 80 of 8080
- Als geen van de bovenstaande true is, de eerste container gedefinieerd in het bestand is toegankelijk (beschikbaar)

## <a name="pricing-and-sla"></a>Prijzen en SLA

**Wat zijn de prijzen, nu dat de service algemeen beschikbaar is?**

De kosten worden berekend met de normale prijzen Azure App Service voor het aantal uren dat uw app wordt uitgevoerd.

## <a name="other-questions"></a>Andere vragen

**Wat zijn de ondersteunde tekens in namen van de instellingen van toepassing?**

U kunt alleen letters (A-Z, a-z), cijfers (0-9) en het onderstrepingsteken (_) gebruiken voor toepassingsinstellingen.

**Waar kan ik vragen om nieuwe functies?**

U kunt uw idee op indienen de [forum met feedback van Web-Apps](https://aka.ms/webapps-uservoice). '[Linux]' toevoegen aan de titel van uw idee.

## <a name="next-steps"></a>Volgende stappen

- [Wat is Azure App Service on Linux?](app-service-linux-intro.md)
- [Faseringsomgevingen in Azure App Service instellen](../../app-service/deploy-staging-slots.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
- [Doorlopende implementatie met WebApp for Containers](./app-service-linux-ci-cd.md)
