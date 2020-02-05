---
title: host. json-verwijzing voor Azure Functions 2. x
description: Referentie documentatie voor het Azure Functions host. JSON-bestand met v2 runtime.
ms.topic: conceptual
ms.date: 01/06/2020
ms.openlocfilehash: 3ad3682e301eb98d48372c3955c6ff049422c517
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/05/2020
ms.locfileid: "77024667"
---
# <a name="hostjson-reference-for-azure-functions-2x-and-later"></a>host. json-verwijzing voor Azure Functions 2. x en hoger 

> [!div class="op_single_selector" title1="Selecteer de versie van de Azure Functions runtime die u gebruikt: "]
> * [Versie 1:](functions-host-json-v1.md)
> * [Versie 2 +](functions-host-json.md)

Het meta gegevensbestand van de *host. json* bevat globale configuratie opties die van invloed zijn op alle functies voor een functie-app. In dit artikel vindt u een lijst met de instellingen die vanaf versie 2. x van de Azure Functions runtime beschikbaar zijn.  

> [!NOTE]
> Dit artikel is voor Azure Functions 2. x en latere versies.  Voor een verwijzing van host.json in functies 1.x, Zie [naslaginformatie over host.json voor Azure Functions 1.x](functions-host-json-v1.md).

Andere opties voor de configuratie van de functie-app worden beheerd in de [app-instellingen](functions-app-settings.md).

Sommige host. json-instellingen worden alleen gebruikt wanneer lokaal wordt uitgevoerd in het bestand [Local. settings. json](functions-run-local.md#local-settings-file) .

## <a name="sample-hostjson-file"></a>Voor beeld van host. JSON-bestand

In het volgende voor beeld van een *host. json* -bestand voor versie 2. x + zijn alle mogelijke opties opgegeven (exclusief voor intern gebruik).

```json
{
    "version": "2.0",
    "aggregator": {
        "batchSize": 1000,
        "flushTimeout": "00:00:30"
    },
    "extensions": {
        "cosmosDb": {},
        "durableTask": {},
        "eventHubs": {},
        "http": {},
        "queues": {},
        "sendGrid": {},
        "serviceBus": {}
    },
    "extensionBundle": {
        "id": "Microsoft.Azure.Functions.ExtensionBundle",
        "version": "[1.*, 2.0.0)"
    },
    "functions": [ "QueueProcessor", "GitHubWebHook" ],
    "functionTimeout": "00:05:00",
    "healthMonitor": {
        "enabled": true,
        "healthCheckInterval": "00:00:10",
        "healthCheckWindow": "00:02:00",
        "healthCheckThreshold": 6,
        "counterThreshold": 0.80
    },
    "logging": {
        "fileLoggingMode": "debugOnly",
        "logLevel": {
          "Function.MyFunction": "Information",
          "default": "None"
        },
        "applicationInsights": {
            "samplingSettings": {
              "isEnabled": true,
              "maxTelemetryItemsPerSecond" : 20,
              "evaluationInterval": "01:00:00",
              "initialSamplingPercentage": 1.0, 
              "samplingPercentageIncreaseTimeout" : "00:00:01",
              "samplingPercentageDecreaseTimeout" : "00:00:01",
              "minSamplingPercentage": 0.1,
              "maxSamplingPercentage": 0.1,
              "movingAverageRatio": 1.0,
              "excludedTypes" : "Dependency;Event",
              "includedTypes" : "PageView;Trace"
            },
            "enableLiveMetrics": true,
            "enableDependencyTracking": true,
            "enablePerformanceCountersCollection": true,            
            "httpAutoCollectionOptions": {
                "enableHttpTriggerExtendedInfoCollection": true,
                "enableW3CDistributedTracing": true,
                "enableResponseHeaderInjection": true
            },
            "snapshotConfiguration": {
                "agentEndpoint": null,
                "captureSnapshotMemoryWeight": 0.5,
                "failedRequestLimit": 3,
                "handleUntrackedExceptions": true,
                "isEnabled": true,
                "isEnabledInDeveloperMode": false,
                "isEnabledWhenProfiling": true,
                "isExceptionSnappointsEnabled": false,
                "isLowPrioritySnapshotUploader": true,
                "maximumCollectionPlanSize": 50,
                "maximumSnapshotsRequired": 3,
                "problemCounterResetInterval": "24:00:00",
                "provideAnonymousTelemetry": true,
                "reconnectInterval": "00:15:00",
                "shadowCopyFolder": null,
                "shareUploaderProcess": true,
                "snapshotInLowPriorityThread": true,
                "snapshotsPerDayLimit": 30,
                "snapshotsPerTenMinutesLimit": 1,
                "tempFolder": null,
                "thresholdForSnapshotting": 1,
                "uploaderProxy": null
            }
        }
    },
    "managedDependency": {
        "enabled": true
    },
    "singleton": {
      "lockPeriod": "00:00:15",
      "listenerLockPeriod": "00:01:00",
      "listenerLockRecoveryPollingInterval": "00:01:00",
      "lockAcquisitionTimeout": "00:01:00",
      "lockAcquisitionPollingInterval": "00:00:03"
    },
    "watchDirectories": [ "Shared", "Test" ]
}
```

In de volgende secties van dit artikel wordt elke eigenschap op het hoogste niveau uitgelegd. Alle zijn optioneel, tenzij anders aangegeven.

## <a name="aggregator"></a>aggregator

[!INCLUDE [aggregator](../../includes/functions-host-json-aggregator.md)]

## <a name="applicationinsights"></a>applicationInsights

Deze instelling is een onderliggend item van [logboek registratie](#logging).

Opties voor de Application Insights, met inbegrip van [bemonsterings opties](./functions-monitoring.md#configure-sampling).

Zie het eerder volgende voor beeld van een [host. JSON-bestand](#sample-hostjson-file)voor de volledige JSON-structuur.

> [!NOTE]
> Het vastleggen van logboeken kan ertoe leiden dat sommige uitvoeringen niet worden weer gegeven op de Blade Application Insights monitor. Voeg `samplingExcludedTypes: "Request"` toe aan de `applicationInsights` waarde om te voor komen dat u zich aanmeldt.

| Eigenschap | Standaard | Beschrijving |
| --------- | --------- | --------- | 
| samplingSettings | n.v.t. | Zie [applicationInsights. samplingSettings](#applicationinsightssamplingsettings). |
| samplingExcludedTypes | null | Een door punt komma's gescheiden lijst met typen waarvan u geen steek proef wilt maken. De herkende typen zijn: afhankelijkheid, gebeurtenis, uitzonde ring, pagina weergave, aanvraag, tracering. Alle exemplaren van de opgegeven typen worden verzonden. voor de typen die niet worden opgegeven, worden steek proeven gegeven. |
| samplingIncludedTypes | null | Een door punt komma's gescheiden lijst met typen waarvan u een steek proef wilt maken; een lege lijst impliceert alle typen. Typ in `samplingExcludedTypes` overschrijvings typen die hier worden weer gegeven. De herkende typen zijn: afhankelijkheid, gebeurtenis, uitzonde ring, pagina weergave, aanvraag, tracering. Alle exemplaren van de opgegeven typen worden verzonden. voor de typen die niet worden opgegeven, worden steek proeven gegeven. |
| enableLiveMetrics | waar | Hiermee wordt de verzameling Live Metrics ingeschakeld. |
| enableDependencyTracking | waar | Hiermee schakelt u het bijhouden van afhankelijkheden in. |
| enablePerformanceCountersCollection | waar | Hiermee schakelt u de verzameling kudu-prestatie meter items. |
| liveMetricsInitializationDelay | 00:00:15 | Alleen voor intern gebruik. |
| httpAutoCollectionOptions | n.v.t. | Zie [applicationInsights. httpAutoCollectionOptions](#applicationinsightshttpautocollectionoptions). |
| snapshotConfiguration | n.v.t. | Zie [applicationInsights. snapshotConfiguration](#applicationinsightssnapshotconfiguration). |

### <a name="applicationinsightssamplingsettings"></a>applicationInsights. samplingSettings

|Eigenschap | Standaard | Beschrijving |
| --------- | --------- | --------- | 
| isEnabled | waar | Hiermee worden steek proeven in-of uitgeschakeld. | 
| maxTelemetryItemsPerSecond | 20 | Het doel aantal telemetriegegevens dat per seconde op elke server host is geregistreerd. Als uw app op meerdere hosts wordt uitgevoerd, vermindert u deze waarde zodat deze binnen het totale doel tempo van het verkeer blijft. | 
| evaluationInterval | 01:00:00 | Het interval waarmee de huidige frequentie van de telemetrie opnieuw wordt geëvalueerd. De evaluatie wordt uitgevoerd als een zwevend gemiddelde. Mogelijk wilt u dit interval verkorten als uw telemetrie op plotselinge bursts is. |
| initialSamplingPercentage| 1.0 | Het eerste bemonsterings percentage dat aan het begin van het bemonsterings proces wordt toegepast om het percentage dynamisch te variëren. Verminder de waarde niet tijdens het opsporen van fouten. |
| samplingPercentageIncreaseTimeout | 00:00:01 | Wanneer de waarde voor het sampling percentage wordt gewijzigd, bepaalt deze eigenschap hoe snel later Application Insights het steekproef percentage opnieuw mag worden gegenereerd om meer gegevens vast te leggen. |
| samplingPercentageDecreaseTimeout | 00:00:01 | Wanneer de waarde voor het sampling percentage wordt gewijzigd, bepaalt deze eigenschap hoe snel Application Insights later het sampling percentage opnieuw wordt toegestaan om minder gegevens vast te leggen. |
| minSamplingPercentage | 0.1 | Als het steekproef percentage varieert, bepaalt deze eigenschap het minimale toegestane sampling percentage. |
| maxSamplingPercentage | 0.1 | Als het steekproef percentage varieert, bepaalt deze eigenschap het Maxi maal toegestane steekproef percentage. |
| movingAverageRatio | 1.0 | Bij het berekenen van het zwevend gemiddelde wordt het gewicht toegewezen aan de meest recente waarde. Gebruik een waarde die gelijk is aan of kleiner is dan 1. Kleinere waarden zorgen ervoor dat het algoritme minder opnieuw wordt geactiveerd tot onverwachte wijzigingen. |

### <a name="applicationinsightshttpautocollectionoptions"></a>applicationInsights. httpAutoCollectionOptions

|Eigenschap | Standaard | Beschrijving |
| --------- | --------- | --------- | 
| enableHttpTriggerExtendedInfoCollection | waar | Hiermee schakelt u uitgebreide informatie over HTTP-aanvragen in of uit voor HTTP-triggers: inkomende aanvraag correlatie headers, ondersteuning voor meerdere instrumentatie sleutels, HTTP-methode, pad en antwoord. |
| enableW3CDistributedTracing | waar | Hiermee wordt ondersteuning van het W3C-protocol voor gedistribueerde tracering (en het verouderde correlatie schema ingeschakeld) in-of uitgeschakeld. Standaard ingeschakeld als `enableHttpTriggerExtendedInfoCollection` waar is. Als `enableHttpTriggerExtendedInfoCollection` False is, geldt deze vlag alleen voor uitgaande aanvragen, niet voor inkomende aanvragen. |
| enableResponseHeaderInjection | waar | Hiermee wordt de injectie van correlatie headers met meerdere onderdelen in-of uitgeschakeld. Door injectie in te scha kelen, kunt Application Insights een toepassings toewijzing maken wanneer er meerdere instrumentatie sleutels worden gebruikt. Standaard ingeschakeld als `enableHttpTriggerExtendedInfoCollection` waar is. Deze instelling is niet van toepassing als `enableHttpTriggerExtendedInfoCollection` is ingesteld op false. |

### <a name="applicationinsightssnapshotconfiguration"></a>applicationInsights. snapshotConfiguration

Voor meer informatie over moment opnamen raadpleegt u [debug-moment opnamen op uitzonde ringen in .net apps](/azure/azure-monitor/app/snapshot-debugger) en lost u problemen op bij het [inschakelen van Application Insights snapshot debugger of het weer geven van moment opnamen](/azure/azure-monitor/app/snapshot-debugger-troubleshoot).

|Eigenschap | Standaard | Beschrijving |
| --------- | --------- | --------- | 
| agentEndpoint | null | Het eind punt dat wordt gebruikt om verbinding te maken met de Application Insights Snapshot Debugger-service. Als de waarde Null is, wordt een standaard eindpunt gebruikt. |
| captureSnapshotMemoryWeight | 0,5 | Het gewicht dat aan de huidige geheugen grootte van het proces is gegeven om te controleren of er voldoende geheugen beschikbaar is om een moment opname te maken. De verwachte waarde is een groter dan 0 juiste fractie (0 < CaptureSnapshotMemoryWeight < 1). |
| failedRequestLimit | 3 | De limiet voor het aantal mislukte aanvragen voor het aanvragen van moment opnamen voordat de telemetrie-processor wordt uitgeschakeld.|
| handleUntrackedExceptions | waar | Hiermee wordt het bijhouden van uitzonde ringen die niet worden bijgehouden door Application Insights telemetrie, in-of uitgeschakeld. |
| isEnabled | waar | Hiermee wordt de momentopname verzameling in-of uitgeschakeld | 
| isEnabledInDeveloperMode | false | Hiermee wordt de momentopname verzameling ingeschakeld of uitgeschakeld in de ontwikkelaars modus. |
| isEnabledWhenProfiling | waar | Hiermee wordt het maken van een moment opname in-of uitgeschakeld, zelfs als er een gedetailleerde profilerings sessie wordt verzameld door de Application Insights Profiler. |
| isExceptionSnappointsEnabled | false | Hiermee wordt het filteren van uitzonde ringen in-of uitgeschakeld. |
| isLowPrioritySnapshotUploader | waar | Hiermee wordt bepaald of het SnapshotUploader-proces op de normale prioriteit moet worden uitgevoerd. |
| maximumCollectionPlanSize | 50 | Het maximum aantal problemen dat kan worden gevolgd op elk gewenst moment in een bereik van 1 tot en met 9999. |
| maximumSnapshotsRequired | 3 | Het maximum aantal moment opnamen dat voor één probleem wordt verzameld, in een bereik van 1 tot 999. Een probleem kan worden beschouwd als een afzonderlijke instructie throw in uw toepassing. Zodra het aantal moment opnamen dat voor een probleem is verzameld deze waarde bereikt, worden er geen moment opnamen meer verzameld voor dat probleem totdat de probleem tellers opnieuw zijn ingesteld (Zie `problemCounterResetInterval`) en de `thresholdForSnapshotting` limiet opnieuw wordt bereikt. |
| problemCounterResetInterval | 24:00:00 | Hoe vaak de probleem tellers in een bereik van één minuut tot zeven dagen opnieuw moeten worden ingesteld. Als dit interval wordt bereikt, worden alle probleem aantallen opnieuw ingesteld op nul. Bestaande problemen die de drempel voor het uitvoeren van moment opnamen al hebben bereikt, maar nog niet het aantal moment opnamen in `maximumSnapshotsRequired`hebben gegenereerd, blijven actief. |
| provideAnonymousTelemetry | waar | Hiermee wordt bepaald of anoniem gebruik en fout-telemetrie naar micro soft moet worden verzonden. Deze telemetrie kan worden gebruikt als u contact opneemt met micro soft om problemen met de Snapshot Debugger op te lossen. Het wordt ook gebruikt om gebruiks patronen te bewaken. |
| reconnectInterval | 00:15:00 | Hoe vaak opnieuw verbinding wordt gemaakt met het Snapshot Debugger-eind punt. Het toegestane bereik is één minuut op een dag. |
| shadowCopyFolder | null | Hiermee geeft u de map op die moet worden gebruikt voor binaire bestanden voor kopiëren van schaduw kopieën. Als dit niet het geval is, worden de mappen die zijn opgegeven door de volgende omgevings variabelen, in volg orde geprobeerd: Fabric_Folder_App_Temp, LOCALAPPDATA, APPDATA, TEMP. |
| shareUploaderProcess | waar | Als deze eigenschap waar is, worden met slechts één instantie van SnapshotUploader moment opnamen verzameld en geüpload voor meerdere apps die de InstrumentationKey delen. Als deze eigenschap is ingesteld op False, wordt de SnapshotUploader uniek voor elke tuple (verwerkings naam, InstrumentationKey). |
| snapshotInLowPriorityThread | waar | Hiermee wordt bepaald of moment opnamen moeten worden verwerkt in een thread met een lage IO-prioriteit. Het maken van een moment opname is een snelle bewerking, maar om een moment opname te uploaden naar de Snapshot Debugger-service, moet deze eerst naar de schijf worden geschreven als een mini dump. Dat gebeurt in het SnapshotUploader-proces. Als u deze waarde instelt op True, wordt IO met lage prioriteit gebruikt voor het schrijven van het mini dump, dat niet kan concurreren met uw toepassing voor resources. Als u deze waarde instelt op ONWAAR, versnelt u het maken van een mini maal in de kosten van het vertragen van uw toepassing. |
| snapshotsPerDayLimit | 30 | Het maximum aantal moment opnamen dat is toegestaan in één dag (24 uur). Deze limiet wordt ook afgedwongen aan de kant van de Application Insights service. Uploads zijn beperkt tot 50 per dag per toepassing (dat wil zeggen, per instrumentatie sleutel). Deze waarde helpt te voor komen dat er extra moment opnamen worden gemaakt die uiteindelijk tijdens het uploaden worden afgewezen. Met de waarde 0 wordt de limiet volledig verwijderd, wat niet wordt aanbevolen. |
| snapshotsPerTenMinutesLimit | 1 | Het maximum aantal moment opnamen dat is toegestaan in 10 minuten. Hoewel er geen bovengrens is voor deze waarde, is het belang rijk om deze te verhogen op productie werkbelastingen, omdat dit de prestaties van uw toepassing kan beïnvloeden. Het maken van een moment opname is snel, maar het maken van een mini dump van de moment opname en het uploaden naar de Snapshot Debugger-service is een veel langzamere bewerking die zal concurreren met uw toepassing voor resources (zowel CPU als I/O). |
| tempFolder | null | Hiermee geeft u de map voor het schrijven van minidumps-en Uploader-logboek bestanden. Als deze niet is ingesteld, wordt *%temp%\Dumps* gebruikt. |
| thresholdForSnapshotting | 1 | Hoe vaak Application Insights een uitzonde ring moet zien voordat er wordt gevraagd om moment opnamen. |
| uploaderProxy | null | Hiermee wordt de proxy server die wordt gebruikt in het Uploader-proces voor moment opnamen onderdrukt. Mogelijk moet u deze instelling gebruiken als uw toepassing verbinding maakt met Internet via een proxy server. De Snapshot Collector wordt uitgevoerd binnen het proces van uw toepassing en maakt gebruik van dezelfde proxy-instellingen. De moment opname Uploader wordt echter als een afzonderlijk proces uitgevoerd en u moet de proxy server mogelijk hand matig configureren. Als deze waarde Null is, probeert Snapshot Collector automatisch het adres van de proxy te detecteren door .net. WebRequest. DefaultWebProxy te onderzoeken en door te geven aan de waarde van de uploader van de moment opname. Als deze waarde niet null is, wordt er geen automatische detectie gebruikt en wordt de proxy server die hier is opgegeven, gebruikt in de uploader voor moment opnamen. |

## <a name="cosmosdb"></a>cosmosDb

De configuratie-instelling vindt u in [Cosmos DB triggers en bindingen](functions-bindings-cosmosdb-v2.md#host-json).

## <a name="durabletask"></a>durableTask

De configuratie-instelling kan worden gevonden in [bindingen voor Durable functions](durable/durable-functions-bindings.md#host-json).

## <a name="eventhub"></a>eventHub

U kunt configuratie-instellingen vinden in [Event hub-triggers en-bindingen](functions-bindings-event-hubs.md#host-json). 

## <a name="extensions"></a>Extensions

Eigenschap die een object retourneert dat alle binding-specifieke instellingen bevat, zoals [http](#http) en [eventHub](#eventhub).

## <a name="extensionbundle"></a>extensionBundle 

Met uitbreidings bundels kunt u een compatibele set van functies bindings extensies toevoegen aan uw functie-app. Zie [uitbreidings bundels voor lokale ontwikkeling voor](functions-bindings-register.md#extension-bundles)meer informatie.

[!INCLUDE [functions-extension-bundles-json](../../includes/functions-extension-bundles-json.md)]

## <a name="functions"></a>functions

Een lijst met functies die de taak host uitvoert. Een lege matrix houdt in dat alle functies worden uitgevoerd. Alleen bedoeld voor gebruik bij [lokaal uitvoeren](functions-run-local.md). In functie-apps in azure moet u in plaats daarvan de stappen volgen in [het uitschakelen van functies in azure functions](disable-function.md) om specifieke functies uit te scha kelen in plaats van deze instelling te gebruiken.

```json
{
    "functions": [ "QueueProcessor", "GitHubWebHook" ]
}
```

## <a name="functiontimeout"></a>functionTimeout

Hiermee wordt de duur van de time-out voor alle functies aangegeven. Het volgt de teken reeks notatie time span. In een serverloze verbruiks abonnement is het geldige bereik van 1 seconde tot 10 minuten en de standaard waarde is 5 minuten.  

In het Premium-abonnement ligt het geldige bereik van 1 seconde tot 60 minuten en is de standaard waarde 30 minuten.

In een speciaal (App Service)-abonnement is er geen algemene limiet, en de standaard waarde is 30 minuten. Een waarde van `-1` geeft een onbegrensde uitvoering aan, maar het behouden van een vaste bovengrens wordt aanbevolen.

```json
{
    "functionTimeout": "00:05:00"
}
```

## <a name="healthmonitor"></a>healthMonitor

Configuratie-instellingen voor de [host Health Monitor](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Host-Health-Monitor).

```
{
    "healthMonitor": {
        "enabled": true,
        "healthCheckInterval": "00:00:10",
        "healthCheckWindow": "00:02:00",
        "healthCheckThreshold": 6,
        "counterThreshold": 0.80
    }
}
```

|Eigenschap  |Standaard | Beschrijving |
|---------|---------|---------| 
|enabled|waar|Hiermee wordt aangegeven of de functie is ingeschakeld. | 
|healthCheckInterval|10 seconden|Het tijds interval tussen de periodieke status controles voor de achtergrond. | 
|healthCheckWindow|2 minuten|Een schuif tijd venster dat wordt gebruikt in combi natie met de instelling `healthCheckThreshold`.| 
|healthCheckThreshold|6|Maximum aantal keer dat de status controle kan mislukken voordat een host recyclen wordt gestart.| 
|counterThreshold|0,80|De drempel waarde waarbij een prestatie meter item wordt beschouwd als een slechte status.| 

## <a name="http"></a>http

Configuratie-instellingen vindt u in [http-triggers en-bindingen](functions-bindings-http-webhook.md#hostjson-settings).

## <a name="logging"></a>Userenv

Hiermee bepaalt u het gedrag van logboek registratie van de functie-app, met inbegrip van Application Insights.

```json
"logging": {
    "fileLoggingMode": "debugOnly"
    "logLevel": {
      "Function.MyFunction": "Information",
      "default": "None"
    },
    "console": {
        ...
    },
    "applicationInsights": {
        ...
    }
}
```

|Eigenschap  |Standaard | Beschrijving |
|---------|---------|---------|
|fileLoggingMode|debugOnly|Hiermee wordt gedefinieerd welk niveau van bestands logboek registratie is ingeschakeld.  Opties zijn `never`, `always``debugOnly`. |
|logLevel|n.v.t.|Object dat de logboek categorie filtering definieert voor functies in de app. Versie 2. x en hoger volgen de ASP.NET Core indeling voor het filteren van de logboek categorie. Met deze instelling kunt u logboek registratie voor specifieke functies filteren. Zie [logboek filtering](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#log-filtering) in de ASP.net core-documentatie voor meer informatie. |
|console|n.v.t.| De instelling voor de logboek registratie van de [console](#console) . |
|applicationInsights|n.v.t.| De instelling [applicationInsights](#applicationinsights) . |

## <a name="console"></a>console

Deze instelling is een onderliggend item van [logboek registratie](#logging). Het beheert de logboek registratie van de console als dit niet in de foutopsporingsmodus is.

```json
{
    "logging": {
    ...
        "console": {
          "isEnabled": "false"
        },
    ...
    }
}
```

|Eigenschap  |Standaard | Beschrijving |
|---------|---------|---------| 
|isEnabled|false|Hiermee wordt de logboek registratie van de console in-of uitgeschakeld.| 

## <a name="manageddependency"></a>managedDependency

Managed dependency is een functie die momenteel alleen wordt ondersteund met Power shell-functies. Hierdoor kunnen afhankelijkheden automatisch worden beheerd door de service. Wanneer de eigenschap `enabled` is ingesteld op `true`, wordt het `requirements.psd1` bestand verwerkt. Afhankelijkheden worden bijgewerkt wanneer er secundaire versies worden vrijgegeven. Zie [beheerde afhankelijkheden](functions-reference-powershell.md#dependency-management) in het Power shell-artikel voor meer informatie.

```json
{
    "managedDependency": {
        "enabled": true
    }
}
```

## <a name="queues"></a>Bestel

Configuratie-instellingen vindt u in de [opslag wachtrij Triggers en bindingen](functions-bindings-storage-queue.md#host-json).  

## <a name="sendgrid"></a>sendGrid

Configuratie-instelling vindt u in [SendGrid-triggers en-bindingen](functions-bindings-sendgrid.md#host-json).

## <a name="servicebus"></a>serviceBus

De configuratie-instelling vindt u in [Service Bus triggers en bindingen](functions-bindings-service-bus.md#host-json).

## <a name="singleton"></a>Singleton

Configuratie-instellingen voor het gedrag van Singleton-vergren deling. Zie [github-probleem over Singleton-ondersteuning](https://github.com/Azure/azure-webjobs-sdk-script/issues/912)voor meer informatie.

```json
{
    "singleton": {
      "lockPeriod": "00:00:15",
      "listenerLockPeriod": "00:01:00",
      "listenerLockRecoveryPollingInterval": "00:01:00",
      "lockAcquisitionTimeout": "00:01:00",
      "lockAcquisitionPollingInterval": "00:00:03"
    }
}
```

|Eigenschap  |Standaard | Beschrijving |
|---------|---------|---------| 
|lockPeriod|00:00:15|De periode waarin vergrendelingen op functie niveau worden uitgevoerd. De vergren delingen automatisch verlengen.| 
|listenerLockPeriod|00:01:00|De periode waarin de luister vergrendelingen worden uitgevoerd.| 
|listenerLockRecoveryPollingInterval|00:01:00|Het tijds interval dat wordt gebruikt voor het herstel van de listener-vergren deling als tijdens het opstarten geen listener-vergrendeling kan worden verkregen.| 
|lockAcquisitionTimeout|00:01:00|De maximale hoeveelheid tijd die de runtime probeert een vergren deling te verkrijgen.| 
|lockAcquisitionPollingInterval|n.v.t.|Het interval tussen overname pogingen voor vergren delen.| 

## <a name="version"></a>versie

Deze waarde geeft de schema versie van host. json aan. De versie teken reeks `"version": "2.0"` is vereist voor een functie-app die is gericht op de v2-runtime of een latere versie. Er zijn geen host. json-schema wijzigingen tussen v2 en v3.

## <a name="watchdirectories"></a>watchDirectories

Een set [gedeelde code mappen](functions-reference-csharp.md#watched-directories) die moeten worden gecontroleerd op wijzigingen.  Zorgt ervoor dat wanneer de code in deze directory's wordt gewijzigd, de wijzigingen worden opgehaald door uw functies.

```json
{
    "watchDirectories": [ "Shared" ]
}
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over het bijwerken van het bestand host. json](functions-reference.md#fileupdate)

> [!div class="nextstepaction"]
> [Algemene instellingen in omgevings variabelen weer geven](functions-app-settings.md)
