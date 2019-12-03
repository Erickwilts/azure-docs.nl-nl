---
title: Aanbevolen procedures en probleem oplossing voor node. js
description: Meer informatie over de aanbevolen procedures en stappen voor het oplossen van problemen met node. js-toepassingen die worden uitgevoerd in Azure App Service.
author: ranjithr
ms.assetid: 387ea217-7910-4468-8987-9a1022a99bef
ms.devlang: nodejs
ms.topic: article
ms.date: 11/09/2017
ms.author: bwren
ms.custom: seodec18
ms.openlocfilehash: 75195bd7ad228bb66dfd21d2c65997cc8c02680e
ms.sourcegitcommit: 265f1d6f3f4703daa8d0fc8a85cbd8acf0a17d30
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/02/2019
ms.locfileid: "74672050"
---
# <a name="best-practices-and-troubleshooting-guide-for-node-applications-on-azure-app-service-windows"></a>Aanbevolen procedures en richt lijnen voor probleem oplossing voor knooppunt toepassingen op Azure App Service Windows

In dit artikel leert u aanbevolen procedures en stappen voor probleem oplossing voor [knooppunt toepassingen](app-service-web-get-started-nodejs.md) die worden uitgevoerd op Azure app service (met [iisnode](https://github.com/azure/iisnode)).

> [!WARNING]
> Wees voorzichtig met het gebruik van stappen voor probleem oplossing op uw productie site. Aanbeveling voor het oplossen van problemen met uw app in een niet-productie-configuratie, bijvoorbeeld uw staging-sleuf en wanneer het probleem is opgelost, kunt u uw staging-sleuf vervangen door uw productie sleuf.
>

## <a name="iisnode-configuration"></a>IISNODE-configuratie

In dit [schema bestand](https://github.com/Azure/iisnode/blob/master/src/config/iisnode_schema_x64.xml) worden alle instellingen weer gegeven die u kunt configureren voor iisnode. Enkele van de instellingen die nuttig zijn voor uw toepassing:

### <a name="nodeprocesscountperapplication"></a>nodeProcessCountPerApplication

Met deze instelling bepaalt u het aantal knooppunt processen dat per IIS-toepassing wordt gestart. De standaard waarde is 1. U kunt net zoveel node. exe starten als het aantal vCPU van uw virtuele machine door de waarde te wijzigen in 0. De aanbevolen waarde is 0 voor de meeste toepassingen, zodat u alle Vcpu's op uw computer kunt gebruiken. Node. exe heeft één thread, waardoor één node. exe een maximum van 1 vCPU verbruikt. Als u de maximale prestaties van uw knooppunt toepassing wilt ontvangen, wilt u alle Vcpu's gebruiken.

### <a name="nodeprocesscommandline"></a>nodeProcessCommandLine

Met deze instelling bepaalt u het pad naar het knoop punt. exe. U kunt deze waarde instellen om naar de versie van node. exe te verwijzen.

### <a name="maxconcurrentrequestsperprocess"></a>maxConcurrentRequestsPerProcess

Met deze instelling bepaalt u het maximum aantal gelijktijdige aanvragen dat door iisnode wordt verzonden naar elk node. exe. Op Azure App Service is de standaard waarde oneindig. U kunt de waarde configureren, afhankelijk van het aantal aanvragen dat door uw toepassing wordt ontvangen en hoe snel uw toepassing elke aanvraag verwerkt.

### <a name="maxnamedpipeconnectionretry"></a>maxNamedPipeConnectionRetry

Met deze instelling bepaalt u het maximum aantal keren dat iisnode nieuwe pogingen om de verbinding te maken op de named pipe om de aanvragen naar node. exe te verzenden. Deze instelling in combi natie met namedPipeConnectionRetryDelay bepaalt de totale time-out van elke aanvraag in iisnode. De standaard waarde is 200 op Azure App Service. Totale time-out in seconden = (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay)/1000

### <a name="namedpipeconnectionretrydelay"></a>namedPipeConnectionRetryDelay

Met deze instelling bepaalt u de hoeveelheid tijd (in MS) iisnode wacht op elke poging om de aanvraag te verzenden naar node. exe via de named pipe. De standaard waarde is 250 ms.
Totale time-out in seconden = (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay)/1000

Standaard is de totale time-out in iisnode op Azure App Service 200 \* 250 MS = 50 seconden.

### <a name="logdirectory"></a>logDirectory

Met deze instelling bepaalt u de map waarin iisnode de logboeken stdout/stderr. De standaard waarde is iisnode, die relatief is ten opzichte van de hoofdmap van het script (directory waarin main server. js aanwezig is)

### <a name="debuggerextensiondll"></a>debuggerExtensionDll

Met deze instelling bepaalt u welke versie van node-Inspector iisnode wordt gebruikt bij het opsporen van fouten in uw knooppunt toepassing. Momenteel zijn iisnode-Inspector-0.7.3. dll en iisnode-Inspector. dll de enige twee geldige waarden voor deze instelling. De standaard waarde is iisnode-Inspector-0.7.3. dll. De iisnode-Inspector-0.7.3. dll-versie maakt gebruik van node-Inspector-0.7.3 en maakt gebruik van websockets. Schakel Web sockets in op uw Azure webapp om deze versie te gebruiken. Zie <https://ranjithblogs.azurewebsites.net/?p=98> voor meer informatie over het configureren van iisnode voor het gebruik van het nieuwe knoop punt-Inspector.

### <a name="flushresponse"></a>flushResponse

Het standaard gedrag van IIS is dat er antwoord gegevens worden gebufferd tot 4 MB vóór het leegmaken of tot aan het einde van de reactie, afhankelijk van wat het eerste komt. iisnode biedt een configuratie-instelling om dit gedrag te negeren: als u een fragment van de hoofd tekst van de antwoord entiteit wilt leegmaken zodra iisnode het ontvangt van node. exe, moet u het kenmerk iisnode/@flushResponse in web. config instellen op ' True ':

```xml
<configuration>
    <system.webServer>
        <!-- ... -->
        <iisnode flushResponse="true" />
    </system.webServer>
</configuration>
```

Als u het leegmaken van elk fragment van de hoofd tekst van de antwoord functie inschakelt, worden er prestatie overhead kosten in mindering geplaatst die de door Voer van het systeem reduceert door ~ 5% (vanaf v 0.1.13). U kunt deze instelling het beste alleen bereiken voor eind punten waarvoor het streamen van antwoorden is vereist (bijvoorbeeld het `<location>`-element in web. config).

Daarnaast moet u voor streaming-toepassingen ook responseBufferLimit van uw iisnode-handler instellen op 0.

```xml
<handlers>
    <add name="iisnode" path="app.js" verb="\*" modules="iisnode" responseBufferLimit="0"/>
</handlers>
```

### <a name="watchedfiles"></a>watchedFiles

Een door punt komma's gescheiden lijst met bestanden die worden bekeken voor wijzigingen. Elke wijziging in een bestand zorgt ervoor dat de toepassing wordt gerecycled. Elk item bestaat uit een optionele mapnaam en een vereiste bestands naam, die relatief is ten opzichte van de map waar het hoofd toepassings ingangs punt zich bevindt. Joker tekens zijn alleen toegestaan in het gedeelte bestands naam. De standaard waarde is `*.js;iisnode.yml`

### <a name="recyclesignalenabled"></a>recycleSignalEnabled

De standaard waarde is False. Als deze functie is ingeschakeld, kan de knooppunt toepassing verbinding maken met een named pipe (omgevings variabele IISNODE\_besturings\_PIPE) en een ' recycle ' bericht verzenden. Dit zorgt ervoor dat de W3wp zonder problemen wordt gerecycled.

### <a name="idlepageouttimeperiod"></a>idlePageOutTimePeriod

De standaard waarde is 0, wat betekent dat deze functie is uitgeschakeld. Wanneer u instelt op een waarde groter dan 0, worden in milliseconden alle onderliggende processen door iisnode op de pagina ' idlePageOutTimePeriod '. Raadpleeg de [documentatie](/windows/desktop/api/psapi/nf-psapi-emptyworkingset) voor meer informatie over wat de pagina betekent. Deze instelling is handig voor toepassingen die een grote hoeveelheid geheugen verbruiken en die af en toe geheugen voor de schijf nodig hebben om RAM vrij te maken.

> [!WARNING]
> Wees voorzichtig bij het inschakelen van de volgende configuratie-instellingen voor productie toepassingen. Het is aan te raden om deze niet in te scha kelen op live productie toepassingen.
>

### <a name="debugheaderenabled"></a>debugHeaderEnabled

De standaard waarde is False. Als deze eigenschap is ingesteld op True, voegt iisnode een HTTP-antwoord header toe `iisnode-debug` toe aan elke HTTP-reactie die de `iisnode-debug` header-waarde verzendt een URL. Afzonderlijke stukjes diagnostische gegevens kunnen worden verkregen door te kijken naar het URL-fragment, maar een visualisatie is beschikbaar door de URL in een browser te openen.

### <a name="loggingenabled"></a>loggingEnabled

Met deze instelling bepaalt u de logboek registratie van stdout en stderr door iisnode. Iisnode legt stdout/stderr vast van het knoop punt dat wordt gestart en schrijft naar de map die is opgegeven in de instelling ' logDirectory '. Zodra dit is ingeschakeld, schrijft uw toepassing Logboeken naar het bestands systeem en afhankelijk van de hoeveelheid logboek registratie die door de toepassing is uitgevoerd, kunnen de prestaties van het probleem consequent zijn.

### <a name="deverrorsenabled"></a>devErrorsEnabled

De standaard waarde is False. Als deze eigenschap is ingesteld op True, worden in iisnode de HTTP-status code en de Win32-fout code in uw browser weer gegeven. De Win32-code is handig bij het opsporen van fouten in bepaalde typen problemen.

### <a name="debuggingenabled-do-not-enable-on-live-production-site"></a>debuggingEnabled (niet inschakelen op de live productie site)

Met deze instelling wordt de functie fout opsporing bepaald. Iisnode is geïntegreerd met node-Inspector. Als u deze instelling inschakelt, schakelt u fout opsporing van de knooppunt toepassing in. Wanneer u deze instelling inschakelt, maakt iisnode knooppunt-Inspector-bestanden in de map debuggerVirtualDir op de eerste aanvraag voor fout opsporing naar uw knooppunt toepassing. U kunt de node-Inspector laden door een aanvraag naar `http://yoursite/server.js/debug`te verzenden. U kunt het fout opsporingsprogramma-URL-segment beheren met de instelling ' debuggerPathSegment '. Standaard debuggerPathSegment = ' debug '. U kunt `debuggerPathSegment` instellen op een GUID, bijvoorbeeld zodat het lastiger wordt gedetecteerd door anderen.

Lees [debug node. js-toepassingen in Windows](https://tomasz.janczuk.org/2011/11/debug-nodejs-applications-on-windows.html) voor meer informatie over fout opsporing.

## <a name="scenarios-and-recommendationstroubleshooting"></a>Scenario's en aanbevelingen/probleem oplossing

### <a name="my-node-application-is-making-excessive-outbound-calls"></a>Mijn knooppunt toepassing maakt buitensporige uitgaande gesp rekken

Veel toepassingen zouden uitgaande verbindingen willen maken als onderdeel van hun normale werking. Als er bijvoorbeeld een aanvraag binnenkomt in, zou uw node-app graag contact willen opnemen met een REST API elders en zo informatie ontvangen om de aanvraag te verwerken. U wilt een Keep Alive-agent gebruiken bij het maken van http-of https-aanroepen. U kunt de agentkeepalive-module gebruiken als Keep Alive-agent wanneer u deze uitgaande aanroepen maakt.

De agentkeepalive-module zorgt ervoor dat sockets opnieuw worden gebruikt op uw Azure webapp VM. Bij het maken van een nieuwe socket voor elke uitgaande aanvraag, wordt de overhead toegevoegd aan uw toepassing. Als u uw toepassing sockets voor uitgaande aanvragen opnieuw wilt gebruiken, zorgt u ervoor dat uw toepassing niet groter is dan de maxSockets die worden toegewezen per VM. De aanbeveling op Azure App Service is om de agentKeepAlive maxSockets-waarde in te stellen op een totaal van (4 exemplaren van node. exe \* 40 maxSockets/instance) 160-sockets per VM.

Voor beeld van [agentKeepALive](https://www.npmjs.com/package/agentkeepalive) -configuratie:

```nodejs
let keepaliveAgent = new Agent({
    maxSockets: 40,
    maxFreeSockets: 10,
    timeout: 60000,
    keepAliveTimeout: 300000
});
```

> [!IMPORTANT]
> In dit voor beeld wordt ervan uitgegaan dat er 4 node. exe op uw virtuele machine wordt uitgevoerd. Als er een ander aantal knoop punten. exe wordt uitgevoerd op de VM, moet u de maxSockets-instelling dienovereenkomstig aanpassen.
>

#### <a name="my-node-application-is-consuming-too-much-cpu"></a>Mijn knooppunt toepassing verbruikt te veel CPU

U ontvangt mogelijk een aanbeveling van Azure App Service in uw portal over een hoog CPU-verbruik. U kunt ook monitors instellen om te controleren op bepaalde [metrische gegevens](web-sites-monitor.md). Wanneer u het CPU-gebruik op het [dash board](../azure-monitor/app/web-monitor-performance.md)van de Azure-Portal controleert, controleert u de maximum waarden voor CPU, zodat u de piek waarden niet kunt missen.
Als u van mening bent dat uw toepassing te veel CPU verbruikt en u niet kunt uitleggen waarom u de node-app wilt gebruiken om erachter te komen.

#### <a name="profiling-your-node-application-on-azure-app-service-with-v8-profiler"></a>Uw knooppunt toepassing in Azure App Service profileren met V8-Profiler

Stel bijvoorbeeld dat u een Hello World-app hebt die u wilt profiel als volgt:

```nodejs
const http = require('http');
function WriteConsoleLog() {
    for(let i=0;i<99999;++i) {
        console.log('hello world');
    }
}

function HandleRequest() {
    WriteConsoleLog();
}

http.createServer(function (req, res) {
    res.writeHead(200, {'Content-Type': 'text/html'});
    HandleRequest();
    res.end('Hello world!');
}).listen(process.env.PORT);
```

Ga naar de site van de console fout opsporing `https://yoursite.scm.azurewebsites.net/DebugConsole`

Ga naar de map van uw site/wwwroot. U ziet een opdracht prompt zoals in het volgende voor beeld wordt weer gegeven:

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/scm_install_v8.png)

Voer de opdracht `npm install v8-profiler` uit.

Met deze opdracht wordt de V8-Profiler geïnstalleerd onder het knoop punt\_modules-map en alle bijbehorende afhankelijkheden.
Bewerk nu uw server. js om de toepassing te profileren.

```nodejs
const http = require('http');
const profiler = require('v8-profiler');
const fs = require('fs');

function WriteConsoleLog() {
    for(let i=0;i<99999;++i) {
        console.log('hello world');
    }
}

function HandleRequest() {
    profiler.startProfiling('HandleRequest');
    WriteConsoleLog();
    fs.writeFileSync('profile.cpuprofile', JSON.stringify(profiler.stopProfiling('HandleRequest')));
}

http.createServer(function (req, res) {
    res.writeHead(200, {'Content-Type': 'text/html'});
    HandleRequest();
    res.end('Hello world!');
}).listen(process.env.PORT);
```

Met de voor gaande code worden de WriteConsoleLog-functie en vervolgens de profiel uitvoer naar het bestand profile. cpuprofile onder uw site wwwroot geschreven. Een aanvraag verzenden naar uw toepassing. U ziet een ' profile. cpuprofile-bestand dat is gemaakt onder uw site wwwroot.

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/scm_profile.cpuprofile.png)

Down load dit bestand en open het met Chrome F12-Hulpprogram Ma's. Druk op F12 op Chrome en kies vervolgens het tabblad **profielen** . Klik op de knop **laden** . Selecteer uw profiel. cpuprofile-bestand dat u hebt gedownload. Klik op het profiel dat u zojuist hebt geladen.

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/chrome_tools_view.png)

U kunt zien dat 95% van de tijd is verbruikt door de functie WriteConsoleLog. In de uitvoer ziet u ook de exacte regel nummers en bron bestanden die het probleem hebben veroorzaakt.

### <a name="my-node-application-is-consuming-too-much-memory"></a>Mijn knooppunt toepassing verbruikt te veel geheugen

Als uw toepassing te veel geheugen verbruikt, wordt er een melding weer gegeven van Azure App Service in uw portal over hoog geheugen gebruik. U kunt monitors instellen om te controleren op bepaalde [metrische gegevens](web-sites-monitor.md). Controleer bij het controleren van het geheugen gebruik in het [dash board](../azure-monitor/app/web-monitor-performance.md)van de Azure-Portal de maximum waarden voor het geheugen, zodat u de piek waarden niet kunt missen.

#### <a name="leak-detection-and-heap-diff-for-nodejs"></a>Lekkage detectie en heap-diff voor node. js

U kunt [node-memwatch](https://github.com/lloyd/node-memwatch) gebruiken om geheugen lekkages te identificeren.
U kunt `memwatch` net als V8-Profiler installeren en uw code bewerken om heaps vast te leggen en te vergelijken om de geheugen lekken in uw toepassing te identificeren.

### <a name="my-nodeexes-are-getting-killed-randomly"></a>De knoop punten van mijn node. exe worden wille keurig gedood

Er zijn een aantal redenen waarom node. exe wille keurig wordt afgesloten:

1. Uw toepassing ontstaat niet-onderschepte uitzonde ringen – check d:\\start\\logboeken\\toepassing\\logging-Errors. txt-bestand voor meer informatie over de uitzonde ring die is opgetreden. Dit bestand heeft de stack-tracering om uw toepassing te debuggen en te herstellen.
2. Uw toepassing verbruikt te veel geheugen, wat invloed heeft op andere processen om aan de slag te gaan. Als het totale geheugen van de virtuele machine bijna 100% is, kan het knoop punt. exe worden afgebroken door de proces beheerder. Proces beheer beëindigt een aantal processen om andere processen in staat te stellen bepaalde werkzaamheden uit te voeren. U kunt dit probleem oplossen door uw toepassing te profileren voor geheugen lekkages. Als uw toepassing grote hoeveel heden geheugen vereist, schaalt u tot een grotere VM (waardoor het RAM-geheugen voor de virtuele machine groter wordt).

### <a name="my-node-application-does-not-start"></a>Mijn knooppunt toepassing wordt niet gestart

Als uw toepassing 500 fouten retourneert wanneer deze wordt gestart, kan dit een aantal oorzaken hebben:

1. Node. exe is niet aanwezig op de juiste locatie. Controleer de instelling van nodeProcessCommandLine.
2. Het hoofd script bestand is niet aanwezig op de juiste locatie. Controleer web. config en controleer of de naam van het hoofd script bestand in de sectie handlers overeenkomt met het hoofd script bestand.
3. De configuratie van het web. config-bestand is onjuist. Controleer de instellingen namen/waarden.
4. Koud start: uw toepassing duurt te lang om te starten. Als uw toepassing langer duurt dan (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay)/1000 seconden, retourneert iisnode een 500-fout. Verhoog de waarden van deze instellingen zodat deze overeenkomen met de start tijd van uw toepassing om te voor komen dat iisnode uit time-out optreedt en de 500-fout te retour neren.

### <a name="my-node-application-crashed"></a>Mijn knooppunt toepassing is vastgelopen

Uw toepassing ontstaat niet-onderschepte uitzonde ringen. Controleer `d:\\home\\LogFiles\\Application\\logging-errors.txt`-bestand voor meer informatie over de uitzonde ring die is opgetreden. Dit bestand heeft de stack-tracering om uw toepassing te diagnosticeren en op te lossen.

### <a name="my-node-application-takes-too-much-time-to-start-cold-start"></a>Het starten van mijn knooppunt toepassing vergt te veel tijd (koude start)

De veelvoorkomende oorzaak van de start tijden van toepassingen is een groot aantal bestanden in het knoop punt\_modules. De toepassing probeert het meren deel van deze bestanden te laden tijdens het starten. Omdat uw bestanden standaard worden opgeslagen op de netwerk share op Azure App Service, kan het laden van veel bestanden tijd duren.
Enkele oplossingen om dit proces sneller uit te voeren:

1. Zorg ervoor dat u een platte afhankelijkheids structuur hebt en geen dubbele afhankelijkheden met behulp van npm3 om uw modules te installeren.
2. Probeer uw knooppunt\_modules te laden en laad niet alle modules in de start van de toepassing. Voor Lazy load modules moet de aanroep to vereist (' module ') worden uitgevoerd wanneer u de module in de functie daad werkelijk nodig hebt vóór de eerste uitvoering van module code.
3. Azure App Service biedt een functie met de naam lokale cache. Deze functie kopieert uw inhoud van de netwerk share naar de lokale schijf op de virtuele machine. Omdat de bestanden zich op een lokale locatie bevinden, is de laad tijd van het knoop punt\_modules veel sneller.

## <a name="iisnode-http-status-and-substatus"></a>Http-status en-substatus IISNODE

In het [bron bestand](https://github.com/Azure/iisnode/blob/master/src/iisnode/cnodeconstants.h) van `cnodeconstants` worden alle mogelijke combi Naties van status-en Substatussen weer gegeven die iisnode kunnen retour neren vanwege een fout.

Schakel FREB in voor uw toepassing om de Win32-fout code te bekijken (zorg ervoor dat u FREB alleen op niet-productie sites inschakelt om de prestaties te verbeteren).

| Http-status | Http-substatus | Mogelijke reden? |
| --- | --- | --- |
| 500 |1000 |Er is een probleem opgetreden bij het verzenden van de aanvraag naar IISNODE: Controleer of node. exe is gestart. Node. exe is tijdens het starten gecrasht. Controleer de web. config-configuratie op fouten. |
| 500 |1001 |-Win32Error 0x2: de app reageert niet op de URL. Controleer de regels voor het herschrijven van de URL of Controleer of uw Express-app de juiste routes heeft gedefinieerd. -Win32Error 0x6d-named pipe is bezet: node. exe accepteert geen aanvragen omdat de pipe bezet is. Controleer het hoge CPU-gebruik. -Andere fouten: Controleer of node. exe is vastgelopen. |
| 500 |1002 |Node. exe is vastgelopen – Controleer d:\\start\\logboek bestanden voor Stack tracering\\logging-Errors. txt. |
| 500 |1003 |Probleem met de configuratie van de pipe: de configuratie van de named pipe is onjuist. |
| 500 |1004-1018 |Er is een fout opgetreden tijdens het verzenden van de aanvraag of het verwerken van de reactie van node. exe. Controleer of node. exe is vastgelopen. Controleer d:\\start\\logboek bestanden\\logging-Errors. txt voor Stack tracering. |
| 503 |1000 |Er is onvoldoende geheugen om meer named pipe verbindingen toe te wijzen. Controleer waarom uw app zoveel geheugen verbruikt. Controleer de waarde van de maxConcurrentRequestsPerProcess-instelling. Als het niet oneindig is en u veel aanvragen hebt, verhoogt u deze waarde om deze fout te voor komen. |
| 503 |1001 |De aanvraag kan niet worden verzonden naar node. exe omdat de toepassing wordt gerecycled. Nadat de toepassing is gerecycled, moeten aanvragen normaal worden aangeboden. |
| 503 |1002 |Controleer de Win32-fout code op werkelijke reden: de aanvraag kan niet worden verzonden naar een node. exe. |
| 503 |1003 |Named pipe is bezet: Controleer of het knoop punt. exe buitensporige CPU verbruikt |

NODE. exe heeft een instelling met de naam `NODE_PENDING_PIPE_INSTANCES`. Op Azure App Service is deze waarde ingesteld op 5000. Dit betekent dat node. exe 5000 aanvragen tegelijk kan accepteren op de named pipe. Deze waarde moet voldoende zijn voor de meeste knooppunt toepassingen die op Azure App Service worden uitgevoerd. U moet 503,1003 niet zien op Azure App Service vanwege de hoge waarde voor de `NODE_PENDING_PIPE_INSTANCES`

## <a name="more-resources"></a>Meer informatiebronnen

Volg deze koppelingen voor meer informatie over node. js-toepassingen op Azure App Service.

* [Aan de slag met Node.js-web-apps in Azure App Service](app-service-web-get-started-nodejs.md)
* [Fouten opsporen in een Node.js web-app in Azure App Service](https://blogs.msdn.microsoft.com/azureossds/2018/08/03/debugging-node-js-apps-on-azure-app-services/)
* [Node.js-modules gebruiken met Azure-toepassingen](../nodejs-use-node-modules-azure-apps.md)
* [Web-apps van Azure App Service: Node.js](https://blogs.msdn.microsoft.com/silverlining/2012/06/14/windows-azure-websites-node-js/)
* [Node.js Developer Center](../nodejs-use-node-modules-azure-apps.md)
* [De geheimen van de Kudu-console voor foutopsporing](https://azure.microsoft.com/documentation/videos/super-secret-kudu-debug-console-for-azure-web-sites/)
