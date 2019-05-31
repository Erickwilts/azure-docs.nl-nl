---
title: Een Kubernetes-Node.js-ontwikkelomgeving in de cloud met VS-Code maken
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
author: zr-msft
ms.author: zarhoads
ms.date: 09/26/2018
ms.topic: tutorial
description: Snelle Kubernetes-ontwikkeling met containers en microservices in Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, containers, Helm, NET service, service mesh-routering, kubectl, k8s
ms.openlocfilehash: e461f210dc5b2d0dda0eabd5ea80dfcdc9ccebfb
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66392795"
---
# <a name="get-started-on-azure-dev-spaces-with-nodejs"></a>Aan de slag in Azure Dev Spaces met behulp van Node.js

In deze handleiding leert u het volgende:

- Een Kubernetes-omgeving maakt in Azure die is geoptimaliseerd voor ontwikkeling - een _dev-ruimte_.
- Iteratief code ontwikkelt in containers met behulp van VS Code en de opdrachtregel.
- Uw code op een productieve manier ontwikkelen en testen in een teamomgeving.

> [!Note]
> **Als u zitten** op elk gewenst moment, Zie de [probleemoplossing](troubleshooting.md) sectie.

## <a name="install-the-azure-cli"></a>Azure-CLI installeren
Azure Dev Spaces vereist minimale instellingen voor de lokale computer. De configuratie van uw ontwikkelomgeving wordt grotendeels opgeslagen in de cloud en kan worden gedeeld met andere gebruikers. Begin met het downloaden en uitvoeren van de [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest).

### <a name="sign-in-to-azure-cli"></a>Aanmelden bij Azure CLI
Meld u aan bij Azure. Typ de volgende opdracht in een terminalvenster:

```cmd
az login
```

> [!Note]
> Als u geen Azure-abonnement hebt, kunt u een [gratis account](https://azure.microsoft.com/free) maken.

#### <a name="if-you-have-multiple-azure-subscriptions"></a>Als u meerdere Azure-abonnementen hebt...
U kunt uw abonnementen bekijken door het volgende uit te voeren: 

```cmd
az account list
```
Zoek naar het abonnement dat `isDefault: true` heeft in de JSON-uitvoer.
Als dit niet het abonnement is dat u wilt gebruiken, kunt u het standaardabonnement wijzigen:

```cmd
az account set --subscription <subscription ID>
```

## <a name="create-a-kubernetes-cluster-enabled-for-azure-dev-spaces"></a>Een Kubernetes-cluster maken dat is ingeschakeld voor Azure Dev Spaces

Bij de opdrachtprompt, maak de resourcegroep in een [regio die ondersteuning biedt voor Azure Dev spaties][supported-regions].

```cmd
az group create --name MyResourceGroup --location <region>
```

Maak een Kubernetes-cluster met de volgende opdracht:

```cmd
az aks create -g MyResourceGroup -n MyAKS --location <region> --disable-rbac --generate-ssh-keys
```

Het duurt een paar minuten om het cluster te maken.

### <a name="configure-your-aks-cluster-to-use-azure-dev-spaces"></a>Uw AKS-cluster configureren voor het gebruik van Azure Dev Spaces

Voer de volgende Azure CLI-opdracht in. Gebruik daarbij de resourcegroep die uw AKS-cluster en de naam van uw AKS-cluster bevat. Met de opdracht wordt uw cluster geconfigureerd met ondersteuning voor Azure Dev Spaces.

   ```cmd
   az aks use-dev-spaces -g MyResourceGroup -n MyAKS
   ```

> [!IMPORTANT]
> Met het Azure Dev Spaces-configuratieproces wordt de eventueel aanwezige `azds`-naamruimte in het cluster verwijderd.

## <a name="get-kubernetes-debugging-for-vs-code"></a>Kubernetes-foutopsporing voor VS Code downloaden
Uitgebreide functies, zoals Kubernetes-foutopsporing, zijn beschikbaar voor .NET Core en Node.js-ontwikkelaars die VS Code gebruiken.

1. Installeer [VS Code](https://code.visualstudio.com/Download) als u dit nog niet hebt.
1. Download en installeer de [extensie VS Azure Dev Spaces](https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds). Klik eenmaal op Installeren op de Microsoft Azure Marketplace-pagina van de extensie en nog eens in VS Code. 

## <a name="create-a-nodejs-container-in-kubernetes"></a>Een Node.js-container maken in Kubernetes

In deze sectie maakt u een Node.js-web-app en voert u deze uit in een container in Kubernetes.

### <a name="create-a-nodejs-web-app"></a>Een Node.js-web-app maken
Download code vanuit GitHub door naar https://github.com/Azure/dev-spaces te navigeren. Selecteer vervolgens **Clone or Download** om de GitHub-opslagplaats te downloaden naar de lokale omgeving. De code voor deze handleiding bevindt zich in `samples/nodejs/getting-started/webfrontend`.

## <a name="prepare-code-for-docker-and-kubernetes-development"></a>Code voorbereiden voor ontwikkeling met Docker en Kubernetes
U hebt nu een eenvoudige web-app die lokaal kan worden uitgevoerd. U gaat hier nu een container van maken door assets te maken waarmee de container van de app wordt gedefinieerd evenals hoe deze in Kubernetes wordt geïmplementeerd. Deze taak kan eenvoudig worden uitgevoerd met Azure Dev Spaces: 

1. Start VS Code en open de map `webfrontend`. (U kunt eventuele standaardprompts negeren om foutopsporingsassets toe te voegen of om het project te herstellen.)
1. Open de geïntegreerde terminal in VS Code (via het menu **Weergave > Geïntegreerde Terminal**).
1. Voer de volgende opdracht uit (controleer of **webfrontend** de huidige map is):

    ```cmd
    azds prep --public
    ```

Met de opdracht `azds prep` van Azure-CLI worden Docker- en Kubernetes-assets met standaardinstellingen gemaakt:
* In `./Dockerfile` wordt beschreven wat de containerinstallatiekopie van de app is, en hoe de broncode is opgebouwd en wordt uitgevoerd in de container.
* Met een [Helm-grafiek](https://docs.helm.sh) die zich onder `./charts/webfrontend` bevindt, wordt beschreven hoe de container in Kubernetes moet worden geïmplementeerd.

Op dit moment is het nog niet nodig om de volledige inhoud van deze bestanden te begrijpen. Wat u wel moet weten is dat **de dezelfde assets voor 'configuratie als code' in Docker en Kubernetes kunnen worden gebruikt van ontwikkeling tot productie, waardoor er meer consistentie tussen de verschillende omgevingen bestaat.**
 
Er wordt met de opdracht `prep` ook een bestand met de naam `./azds.yaml` gegenereerd. Dit is het configuratiebestand voor Azure Dev Spaces. Dit vormt een aanvulling op de Docker- en Kubernetes-artefacten met extra configuraties die herhaalbare ontwikkelingsmogelijkheden in Azure bieden.

## <a name="build-and-run-code-in-kubernetes"></a>Code schrijven en uitvoeren in Kubernetes
We gaan onze code uitvoeren! In het terminalvenster voert u deze opdracht uit vanuit de **hoofdcodemap**, webfrontend:

```cmd
azds up
```

Houd de uitvoer van de opdracht in de gaten. Er zijn meerdere zaken die u zullen opvallen tijdens de uitvoering:
- De broncode is gesynchroniseerd met de dev-ruimte in Azure.
- Er wordt een containerinstallatiekopie gemaakt in Azure, zoals is aangegeven door de Docker-assets in uw codemap.
- Er worden Kubernetes-objecten gemaakt die de containerinstallatiekopie gebruiken zoals wordt aangegeven in de Helm-grafiek in uw codemap.
- Er wordt informatie over het eindpunt of de eindpunten van de container weergegeven. In ons geval verwachten we een openbare HTTP-URL.
- Ervan uitgaande dat de fases hierboven goed worden afgerond, moet u nu `stdout`-uitvoer (en `stderr`) gaan zien terwijl de container wordt opgestart.

> [!Note]
> Deze stappen nemen de eerste keer dat de opdracht `up` wordt uitgevoerd, meer tijd in beslag, maar latere uitvoeringen zullen sneller verlopen.

### <a name="test-the-web-app"></a>De web-app testen
Scan de console-uitvoer voor informatie over de openbare URL die door de opdracht `up` is gemaakt. Het zal in deze vorm te zien zijn: 

```
(pending registration) Service 'webfrontend' port 'http' will be available at <url>
Service 'webfrontend' port 80 (TCP) is available at 'http://localhost:<port>'
```

Open deze URL in een browservenster. Dan ziet u dat de web-app wordt geladen. Terwijl de container wordt uitgevoerd, wordt `stdout`- en `stderr`-uitvoer naar het terminalvenster gestreamd.

> [!Note]
> Bij de eerste uitvoering kan het enkele minuten duren voordat de openbare DNS gereed is. Als de openbare URL niet is opgelost, kunt u de alternatieve `http://localhost:<portnumber>` URL die wordt weergegeven in de console-uitvoer. Als u de localhost-URL gebruikt, lijkt het misschien alsof de container lokaal wordt uitgevoerd, maar wordt deze feitelijk uitgevoerd in AKS. Voor uw gemak en om interactie met de service mogelijk te maken vanaf de lokale computer, wordt in Azure Dev Spaces een tijdelijke SSH-tunnel gemaakt naar de container die wordt uitgevoerd in Azure. U kunt later terugkomen en de openbare URL proberen wanneer de DNS-record gereed is.

### <a name="update-a-content-file"></a>Een inhoudsbestand bijwerken
Azure Dev Spaces draait niet alleen om het ophalen van code die wordt uitgevoerd in Kubernetes. Het gaat er om dat u de codewijzigingen snel en iteratief toegepast kunt zien in een Kubernetes-omgeving in de cloud.

1. Zoek het bestand `./public/index.html` en bewerk de HTML-code. Wijzig bijvoorbeeld de achtergrondkleur van de pagina in een blauwtint:

    ```html
    <body style="background-color: #95B9C7; margin-left:10px; margin-right:10px;">
    ```

2. Sla het bestand op. Enkele ogenblikken later ziet u in het terminalvenster een bericht met de melding dat een bestand in de actieve container is bijgewerkt.
1. Ga naar de browser en vernieuw de pagina. U ziet nu de bijgewerkte kleur.

Wat is er gebeurd? Voor bewerkingen van inhoudsbestanden, zoals HTML en CSS, is niet vereist dat het Node.js-proces opnieuw wordt opgestart. Door een actieve `azds up`-opdracht worden gewijzigde inhoudsbestanden rechtstreeks automatisch gesynchroniseerd in de actieve container in Azure. Zo kunt u de bewerkingen van uw inhoud snel zien.

### <a name="test-from-a-mobile-device"></a>Testen via een mobiel apparaat
Open de web-app op een mobiel apparaat die de openbare URL als webfrontend gebruikt. Mogelijk wilt u de URL kopiëren en vanaf het bureaublad naar uw apparaat verzenden zodat u het lange adres niet hoeft in te voeren. Wanneer de web-app op een mobiel apparaat wordt geladen, ziet u dat de gebruikersinterface niet correct wordt weergegeven op een klein apparaat.

Om dit probleem te verhelpen voegt u een `viewport`-metatag toe:
1. Open het bestand `./public/index.html`
1. Voeg een `viewport`-metatag toe aan het bestaande `head`-element:

    ```html
    <head>
        <!-- Add this line -->
        <meta name="viewport" content="width=device-width, initial-scale=1">
    </head>
    ```

1. Sla het bestand op.
1. Vernieuw de browser op het apparaat. U ziet nu dat de web-app correct wordt weergegeven. 

Dit is een voorbeeld van hoe bepaalde problemen pas worden gevonden als u test op de apparaten waarop de app moet worden gebruikt. Met Azure Dev Spaces kunt u code snel opnieuw uitvoeren en eventuele wijzigingen op doelapparaten valideren.

### <a name="update-a-code-file"></a>Een codebestand bijwerken
Het bijwerken van codebestanden aan de serverzijde vereist iets meer werk, omdat een Node.js-app opnieuw moet worden opgestart.

1. Druk in het terminalvenster op `Ctrl+C` (om `azds up` te stoppen).
1. Open het codebestand met de naam `server.js` en bewerk het hallo-bericht van de service: 

    ```javascript
    res.send('Hello from webfrontend running in Azure!');
    ```

3. Sla het bestand op.
1. Voer `azds up` uit in het terminalvenster. 

Met deze opdracht wordt de installatiekopie van de container opnieuw gebouwd en het Helm-diagram opnieuw geïmplementeerd. Laad de browserpagina om de codewijzigingen door te voeren.

Er bestaat echter een nog *snellere methode* voor het ontwikkelen van code. Deze methode gaat u in de volgende sectie verkennen. 

## <a name="debug-a-container-in-kubernetes"></a>Fouten opsporen in een Kubernetes-container

In deze sectie gaat u VS Code gebruiken om direct fouten op te sporen in de container die in Azure wordt uitgevoerd. U leert ook hoe u een cyclus voor bewerken-uitvoeren-testen sneller kunt uitvoeren.

![](media/common/edit-refresh-see.png)

> [!Note]
> **Als u op enig moment niet verder kunt**, kunt u de [probleemoplossingssectie](troubleshooting.md) raadplegen of een opmerking op deze pagina plaatsen.

### <a name="initialize-debug-assets-with-the-vs-code-extension"></a>Foutopsporingsassets initialiseren met de VS Code-extensie
U moet eerst uw codeproject configureren, zodat de VS Code met onze dev-ruimte in Azure gaat communiceren. De VS Code-extensie voor Azure Dev Spaces bevat een helpopdracht om de foutopsporingsconfiguratie in te stellen. 

Open het **Opdrachtenpalet** (via het menu **Beeld | Opdrachtenpalet**), en gebruik automatisch aanvullen om te typen en deze opdracht te selecteren: `Azure Dev Spaces: Prepare configuration files for Azure Dev Spaces`. 

Hiermee wordt de foutopsporingsconfiguratie voor Azure Dev Spaces toegevoegd onder de map `.vscode`. Deze opdracht moet niet worden verward met de opdracht `azds prep`, waarmee u het project configureert voor implementatie.

![](media/common/command-palette.png)

### <a name="select-the-azds-debug-configuration"></a>Selecteer de AZDS-foutopsporingsconfiguratie
1. Om de foutopsporingsweergave te openen, klikt u op het pictogram Foutopsporing in de **activiteitenbalk** van VS Code.
1. Selecteer **Launch Program (AZDS)** als de actieve foutopsporingsconfiguratie.

![](media/get-started-node/debug-configuration-nodejs2.png)

> [!Note]
> Als u geen Azure Dev Spaces-opdrachten ziet in het Opdrachtenpalet, controleert u of u [de VS Code-extensie voor Azure Dev Spaces hebt geïnstalleerd](get-started-nodejs.md#get-kubernetes-debugging-for-vs-code).

### <a name="debug-the-container-in-kubernetes"></a>Fouten opsporen in de container in Kubernetes
Druk op **F5** om fouten in uw code in Kubernetes op te sporen.

Net als bij de opdracht `up` wordt de code gesynchroniseerd met de ontwikkelomgeving wanneer u foutopsporing start en wordt een container gemaakt en geïmplementeerd in Kubernetes. Op dit moment is het foutopsporingsprogramma gekoppeld aan de externe container.

> [!Tip]
> Op de VS Code-statusbalk wordt een klikbare URL weergegeven.

![](media/common/vscode-status-bar-url.png)

Stel een onderbrekingspunt in een codebestand aan de serverzijde in, bijvoorbeeld binnen de `app.get('/api'...` in `server.js`. Vernieuw de browserpagina of druk op de knop Say It Again. U komt dan op het onderbrekingspunt terecht en kunt de code doorlopen.

U hebt volledige toegang tot foutopsporingsgegevens, net alsof de code lokaal wordt uitgevoerd. Denk hierbij aan de aanroep-stack, lokale variabelen en informatie over uitzonderingen, enzovoort.

### <a name="edit-code-and-refresh-the-debug-session"></a>Code bewerken en de foutopsporingssessie vernieuwen
Breng met het actieve foutopsporingsprogramma een codewijziging aan. Wijzig bijvoorbeeld het hallo-bericht opnieuw:

```javascript
app.get('/api', function (req, res) {
    res.send('**** Hello from webfrontend running in Azure! ****');
});
```

Sla het bestand op en klik in het deelvenster **Debug actions** op de knop **Refresh**. 

![](media/get-started-node/debug-action-refresh-nodejs.png)

In plaats van telkens wanneer codewijzigingen zijn aangebracht een nieuwe containerinstallatiekopie te bouwen en opnieuw te implementeren, wat meestal veel tijd kost, wordt het Node.js-proces tussen foutopsporingssessies opnieuw opgestart in Azure Dev Spaces voor een snellere bewerkings-/foutopsporingslus.

Vernieuw de web-app in de browser of druk op de knop *Say It Again*. U ziet dat uw aangepaste bericht wordt weergegeven in de gebruikersinterface.

### <a name="use-nodemon-to-develop-even-faster"></a>NodeMon gebruiken om nog sneller te ontwikkelen
*Nodemon* is een populair hulpprogramma dat Node.js-ontwikkelaars gebruiken voor snelle ontwikkeling. In plaats van het Node-proces handmatig opnieuw te starten telkens als er een wijziging aan de serverzijde is doorgevoerd, configureren ontwikkelaars hun Node-project vaak om bestandswijzigingen te laten controleren met *nodemon* en het serverproces automatisch opnieuw te starten. Bij deze manier van werken vernieuwt de ontwikkelaar alleen de browser nadat een codewijziging is aangebracht.

Met Azure Dev Spaces kunt u veel dezelfde ontwikkelwerkstromen gebruiken die u ook gebruikt wanneer u lokaal ontwikkelt. Ter illustratie hiervan is het `webfrontend`-voorbeeldproject geconfigureerd voor gebruik van *nodemon* (het is geconfigureerd als een apparaatafhankelijkheid in `package.json`).

Voer de volgende stappen uit:
1. Stop het VS Code-foutopsporingsprogramma.
1. Klik op het pictogram Foutopsporing in de **activiteitenbalk** van VS Code. 
1. Selecteer **Attach (AZDS)** als de actieve foutopsporingsconfiguratie.
1. Druk op F5.

In deze configuratie is de container geconfigureerd voor het starten van *nodemon*. Wanneer servercodewijzigingen worden doorgevoerd, wordt met *nodemon* het Node-proces automatisch opnieuw opgestart, net zoals wanneer u het lokaal ontwikkelt. 
1. Bewerk het hallo-bericht opnieuw in `server.js` en sla het bestand op.
1. Vernieuw de browser of klik op de knop *Say It Again* om de wijzigingen door te voeren.

**U beschikt nu over een methode om code snel te ontwikkelen en foutopsporing rechtstreeks uit te voeren in Kubernetes.** Hierna ziet u hoe u een tweede container kunt maken en aanroepen.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Informatie over ontwikkelen van meerdere services](multi-service-nodejs.md)


[supported-regions]: about.md#supported-regions-and-configurations