---
title: Azure Monitor ondersteunde metrische gegevens per resource type
description: Een lijst met metrische gegevens die beschikbaar zijn voor elk resource type met Azure Monitor.
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: reference
ms.date: 05/20/2019
ms.author: ancav
ms.subservice: metrics
ms.openlocfilehash: 49780ec342ea168d27ab8a029c41a1c18a6ffcc4
ms.sourcegitcommit: b12a25fc93559820cd9c925f9d0766d6a8963703
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/14/2019
ms.locfileid: "69019052"
---
# <a name="supported-metrics-with-azure-monitor"></a>Ondersteunde metrische gegevens met Azure Monitor

Azure Monitor biedt verschillende manieren om te communiceren met metrische gegevens, zoals het maken van grafieken in de portal, het openen ervan via de REST API of het opvragen van query's via Power shell of CLI. Hieronder vindt u een volledige lijst met alle metrische gegevens die momenteel beschikbaar zijn met de metrische pijp lijn van de Azure Monitor. Andere metrische gegevens zijn mogelijk beschikbaar in de portal of met behulp van verouderde Api's. Deze lijst hieronder bevat alleen metrische gegevens die beschikbaar zijn via de geconsolideerde Azure Monitor metrische pijp lijn. Als u deze metrische gegevens wilt opvragen en openen, gebruikt u de [2018-01-01-API-versie](https://docs.microsoft.com/rest/api/monitor/metricdefinitions)

> [!NOTE]
> Het verzenden van multidimensionale metrische gegevens via diagnostische instellingen wordt momenteel niet ondersteund. Metrische gegevens met dimensies worden geëxporteerd als platte eendimensionale metrische gegevens, als totaal van alle dimensiewaarden.
>
> *Bijvoorbeeld*: De metriek 'Binnenkomende berichten' voor een Event Hub kan worden verkend en uitgezet op wachtrijniveau. Wanneer de waarde wordt geëxporteerd via diagnostische instellingen, wordt deze echter voorgesteld als alle binnenkomende berichten voor alle wachtrijen in de Event Hub.
>
>

## <a name="microsoftanalysisservicesservers"></a>Microsoft.AnalysisServices/servers

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|qpu_metric|QPU|Count|Average|QPU. Bereik 0-100 voor S1, 0-200 voor S2 en 0-400 voor S4|ServerResourceType|
|memory_metric|Geheugen|Bytes|Average|Geheugenmetabase. Bereik 0-25 GB voor S1, 0-50 GB voor S2 en 0-100 GB voor S4|ServerResourceType|
|TotalConnectionRequests|Totaal aantal verbindings aanvragen|Count|Average|Totaal aantal verbindings aanvragen. Dit zijn ontvangsten.|ServerResourceType|
|SuccessfullConnectionsPerSec|Geslaagde verbindingen per seconde|CountPerSecond|Average|Snelheid van geslaagde verbindings voltooiingen.|ServerResourceType|
|TotalConnectionFailures|Totaal aantal verbindings fouten|Count|Average|Totaal aantal mislukte Verbindings pogingen.|ServerResourceType|
|CurrentUserSessions|Huidige gebruikers sessies|Count|Average|Het huidige aantal gebruikers sessies dat tot stand is gebracht.|ServerResourceType|
|QueryPoolBusyThreads|Query's pool bezette threads|Count|Average|Het aantal actieve threads in de query thread pool.|ServerResourceType|
|CommandPoolJobQueueLength|Wachtrij lengte van de opdracht pool taak|Count|Average|Aantal taken in de wachtrij van de opdracht thread pool.|ServerResourceType|
|ProcessingPoolJobQueueLength|Wachtrij lengte van de pool taak wordt verwerkt|Count|Average|Het aantal niet-I/O-taken in de wachtrij van de verwer kende thread pool.|ServerResourceType|
|CurrentConnections|Verbinding: Huidige verbindingen|Count|Average|Het huidige aantal client verbindingen dat tot stand is gebracht.|ServerResourceType|
|CleanerCurrentPrice|Geheugen: Huidige prijs verduidelijken|Count|Average|Huidige prijs van geheugen, $/byte/tijd), genormaliseerd naar 1000.|ServerResourceType|
|CleanerMemoryShrinkable|Geheugen: Verkleinbaar geheugen|Bytes|Average|De hoeveelheid geheugen (in bytes) die wordt verwijderd door de achtergrond opschoning.|ServerResourceType|
|CleanerMemoryNonshrinkable|Geheugen: Ruim geheugen dat niet kan worden verkleind|Bytes|Average|Hoeveelheid geheugen (in bytes) die niet wordt verwijderd door de achtergrond opschoning.|ServerResourceType|
|MemoryUsage|Geheugen: Geheugengebruik|Bytes|Average|Geheugen gebruik van het Server proces zoals gebruikt bij het berekenen van de prijs voor het schonen van geheugen. Gelijk aan Counter Process\PrivateBytes plus de grootte van gegevens die zijn toegewezen door het geheugen, waarbij elk geheugen wordt genegeerd dat is toegewezen aan of toegewezen door de xVelocity in-Memory Analytics Engine (VertiPaq) die de geheugen limiet van xVelocity-engine overschrijdt.|ServerResourceType|
|MemoryLimitHard|Geheugen: Geheugen limiet hard|Bytes|Average|Vaste geheugen limiet, van configuratie bestand.|ServerResourceType|
|MemoryLimitHigh|Geheugen: Hoge geheugen limiet|Bytes|Average|Hoge geheugen limiet, van configuratie bestand.|ServerResourceType|
|MemoryLimitLow|Geheugen: Geheugen limiet laag|Bytes|Average|Limiet voor weinig geheugen, van configuratie bestand.|ServerResourceType|
|MemoryLimitVertiPaq|Geheugen: Geheugen limiet VertiPaq|Bytes|Average|In-Memory limiet, van configuratie bestand.|ServerResourceType|
|Quota|Geheugen: Quota|Bytes|Average|Huidig geheugen quotum, in bytes. Geheugen quota wordt ook wel een geheugen toekenning of geheugen reservering genoemd.|ServerResourceType|
|QuotaBlocked|Geheugen: Quota geblokkeerd|Count|Average|Het huidige aantal quotum aanvragen dat is geblokkeerd totdat andere geheugen quota zijn vrijgemaakt.|ServerResourceType|
|VertiPaqNonpaged|Geheugen: VertiPaq Nonpaged|Bytes|Average|Geheugen bytes vergrendeld in de werkset voor gebruik door de in-Memory engine.|ServerResourceType|
|VertiPaqPaged|Geheugen: VertiPaq Paged|Bytes|Average|Bytes van wisselbaar geheugen die in gebruik zijn voor in-Memory gegevens.|ServerResourceType|
|RowsReadPerSec|Verwerken: Gelezen rijen per seconde|CountPerSecond|Average|Het aantal rijen dat van alle relationele data bases is gelezen.|ServerResourceType|
|RowsConvertedPerSec|Verwerken: Geconverteerde rijen per seconde|CountPerSecond|Average|Het aantal rijen dat tijdens de verwerking is geconverteerd.|ServerResourceType|
|RowsWrittenPerSec|Verwerken: Aantal geschreven rijen per seconde|CountPerSecond|Average|Het aantal rijen dat tijdens de verwerking is geschreven.|ServerResourceType|
|CommandPoolBusyThreads|Lijnen Bezette threads van opdracht pool|Count|Average|Het aantal actieve threads in de opdracht thread pool.|ServerResourceType|
|CommandPoolIdleThreads|Lijnen Niet-actieve threads van opdracht pool|Count|Average|Het aantal niet-actieve threads in de opdracht thread pool.|ServerResourceType|
|LongParsingBusyThreads|Lijnen Lange threads voor het parseren|Count|Average|Het aantal actieve threads in de thread pool voor lang parseren.|ServerResourceType|
|LongParsingIdleThreads|Lijnen Lang niet-actieve threads parseren|Count|Average|Het aantal niet-actieve threads in de thread pool voor lang parseren.|ServerResourceType|
|LongParsingJobQueueLength|Lijnen Lengte van taak wachtrij voor lang parseren|Count|Average|Aantal taken in de wachtrij van de thread pool voor lang parseren.|ServerResourceType|
|ProcessingPoolBusyIOJobThreads|Lijnen Bezig met verwerken van werk groep-I/O-taak threads|Count|Average|Het aantal threads waarmee I/O-taken worden uitgevoerd in de verwer kende thread pool.|ServerResourceType|
|ProcessingPoolBusyNonIOThreads|Lijnen Bezig met verwerken van niet-I/O-threads van groep|Count|Average|Het aantal threads met niet-I/O-taken in de verwer kende thread pool.|ServerResourceType|
|ProcessingPoolIOJobQueueLength|Lijnen Wachtrij lengte van I/O-taak verwerking van groep|Count|Average|Het aantal I/O-taken in de wachtrij van de verwer kende thread pool.|ServerResourceType|
|ProcessingPoolIdleIOJobThreads|Lijnen Niet-actieve I/O-taak threads van groep verwerken|Count|Average|Het aantal niet-actieve threads voor I/O-taken in de verwer kende thread pool.|ServerResourceType|
|ProcessingPoolIdleNonIOThreads|Lijnen Niet-I/O-threads van de groep worden verwerkt|Count|Average|Het aantal niet-actieve threads in de verwer kende thread pool dat is gereserveerd voor niet-I/O-taken.|ServerResourceType|
|QueryPoolIdleThreads|Lijnen Niet-actieve threads van query pool|Count|Average|Het aantal niet-actieve threads voor I/O-taken in de verwer kende thread pool.|ServerResourceType|
|QueryPoolJobQueueLength|Lijnen Wachtrij lengte van de taak pool voor query's|Count|Average|Aantal taken in de wachtrij van de query thread pool.|ServerResourceType|
|ShortParsingBusyThreads|Lijnen Kortere threads voor het parseren|Count|Average|Het aantal actieve threads in de thread pool voor kort parseren.|ServerResourceType|
|ShortParsingIdleThreads|Lijnen Niet-actieve threads voor kort parseren|Count|Average|Het aantal niet-actieve threads in de thread pool voor kort parseren.|ServerResourceType|
|ShortParsingJobQueueLength|Lijnen Korte lengte van taak wachtrij voor parseren|Count|Average|Aantal taken in de wachtrij van de thread pool voor kort parseren.|ServerResourceType|
|memory_thrashing_metric|Geheugen overbelasting|Percent|Average|Gemiddeld geheugen overbelasting.|ServerResourceType|
|mashup_engine_qpu_metric|M-engine QPU|Count|Average|QPU-gebruik door mashup-engine processen|ServerResourceType|
|mashup_engine_memory_metric|M-engine geheugen|Bytes|Average|Geheugen gebruik door mashup-engine processen|ServerResourceType|

## <a name="microsoftapimanagementservice"></a>Microsoft.ApiManagement/service

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|Aanvragen|Aanvragen|Count|Totaal|Het totale aantal gateway aanvragen in een bepaalde periode. Het kan worden gesegmenteerd door verschillende dimensies om u te helpen bij het vaststellen van problemen. |Locatie, BackendResponseCode, LastErrorReason, GatewayResponseCode|
|TotalRequests|Totaal aantal gateway-aanvragen|Count|Totaal|Het totale aantal gateway aanvragen in een bepaalde periode. Deze metriek is afgeschaft, wij raden u aan de `Requests` nieuwe metrische gegevens te gebruiken. |Locatie, hostnaam|
|SuccessfulRequests|Geslaagde gateway-aanvragen|Count|Totaal|Het totaal aantal geslaagde gateway aanvragen in een bepaalde periode. Deze metriek is afgeschaft, wij raden u aan de `Requests` nieuwe metrische gegevens te gebruiken.|Locatie, hostnaam|
|UnauthorizedRequests|Niet-geautoriseerde gateway-aanvragen|Count|Totaal| Het totale aantal niet-geautoriseerde gateway aanvragen in een bepaalde periode. Deze metriek is afgeschaft, wij raden u aan de `Requests` nieuwe metrische gegevens te gebruiken.|Locatie, hostnaam|
|FailedRequests|Mislukte gateway-aanvragen|Count|Totaal|Het totale aantal mislukte gateway aanvragen in een bepaalde periode. Deze metriek is afgeschaft, wij raden u aan de `Requests` nieuwe metrische gegevens te gebruiken.|Locatie, hostnaam|
|OtherRequests|Andere gateway aanvragen|Count|Totaal|Het totale aantal gateway aanvragen in een bepaalde periode die niet in geslaagde, niet-geautoriseerde of mislukte categorieën vallen. Deze metriek is afgeschaft, wij raden u aan de `Requests` nieuwe metrische gegevens te gebruiken. |Locatie, hostnaam|
|Duration|Totale duur van gateway aanvragen|Milliseconden|Average|De tijd tussen het API Management van een aanvraag van een client en het retour neren van een reactie op de client.|Locatie, hostnaam|
|Capaciteit|Capaciteit|Percent|Average|De belasting indicator voor een API Management-exemplaar voor het nemen van gefundeerde beslissingen, ongeacht of u het exemplaar wilt schalen om meer belasting te bieden.|Location|
|EventHubTotalEvents|Totaal aantal EventHub-gebeurtenissen|Count|Totaal|Het totale aantal gebeurtenissen dat is verzonden naar EventHub van API Management in een bepaalde periode.|Location|
|EventHubSuccessfulEvents|Geslaagde EventHub-gebeurtenissen|Count|Totaal|Het totale aantal geslaagde EventHub-gebeurtenissen in een bepaalde periode.|Location|
|EventHubTotalFailedEvents|Mislukte EventHub-gebeurtenissen|Count|Totaal|Het totale aantal mislukte EventHub-gebeurtenissen in een bepaalde periode.|Location|
|EventHubRejectedEvents|Geweigerde EventHub-gebeurtenissen|Count|Totaal|Het totale aantal geweigerde EventHub-gebeurtenissen (onjuiste configuratie of niet-geautoriseerd) in een bepaalde periode.|Location|
|EventHubThrottledEvents|Vertraagde EventHub-gebeurtenissen|Count|Totaal|Het totale aantal vertraagde EventHub-gebeurtenissen in een bepaalde periode.|Location|
|EventHubTimedoutEvents|Time-out EventHub-gebeurtenissen|Count|Totaal|Het totale aantal time-out EventHub-gebeurtenissen in een bepaalde periode.|Location|
|EventHubDroppedEvents|Verwijderde EventHub-gebeurtenissen|Count|Totaal|Het totale aantal gebeurtenissen dat is overgeslagen omdat de maximale grootte van de wachtrij is bereikt in een bepaalde periode.|Location|
|EventHubTotalBytesSent|Grootte van EventHub-gebeurtenissen|Bytes|Totaal|De totale grootte van EventHub-gebeurtenissen in bytes in een bepaalde periode.|Location|


## <a name="microsoftautomationautomationaccounts"></a>Microsoft.Automation/automationAccounts

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|TotalJob|Totaal aantal taken|Count|Totaal|Het totale aantal taken|Runbook, status|
|TotalUpdateDeploymentRuns|Totaal aantal uitgevoerde update-implementaties|Count|Totaal|Totale aantal uitgevoerde software-update-implementaties|SoftwareUpdateConfigurationName, Status|
|TotalUpdateDeploymentMachineRuns|Totaal aantal uitgevoerde update-implementatie computers|Count|Totaal|Het totale aantal uitgevoerde software-update-implementatie computers in een software-update-implementatie|SoftwareUpdateConfigurationName, Status, TargetComputer, SoftwareUpdateConfigurationRunId|

## <a name="microsoftbatchbatchaccounts"></a>Microsoft.Batch/batchAccounts

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|CoreCount|Aantal toegewezen kernen|Count|Totaal|Totaal aantal toegewezen kernen in het batch-account|Er zijn geen dimensies|
|TotalNodeCount|Aantal toegewezen knoop punten|Count|Totaal|Totaal aantal toegewezen knoop punten in het batch-account|Er zijn geen dimensies|
|LowPriorityCoreCount|Aantal LowPriority kernen|Count|Totaal|Totaal aantal kernen met lage prioriteit in het batch-account|Er zijn geen dimensies|
|TotalLowPriorityNodeCount|Aantal knoop punten met een lage prioriteit|Count|Totaal|Totaal aantal knoop punten met een lage prioriteit in het batch-account|Er zijn geen dimensies|
|CreatingNodeCount|Aantal knoop punten maken|Count|Totaal|Aantal knoop punten dat wordt gemaakt|Er zijn geen dimensies|
|StartingNodeCount|Begin aantal knoop punten|Count|Totaal|Aantal knoop punten dat begint|Er zijn geen dimensies|
|WaitingForStartTaskNodeCount|Wachten op aantal begin taak knooppunten|Count|Totaal|Aantal knoop punten dat wacht tot de begin taak is voltooid|Er zijn geen dimensies|
|StartTaskFailedNodeCount|Aantal mislukte knoop punten van begin taak|Count|Totaal|Aantal knoop punten waarop de begin taak is mislukt|Er zijn geen dimensies|
|IdleNodeCount|Aantal niet-actieve knoop punten|Count|Totaal|Aantal niet-actieve knoop punten|Er zijn geen dimensies|
|OfflineNodeCount|Aantal offline knooppunten|Count|Totaal|Aantal offline knooppunten|Er zijn geen dimensies|
|RebootingNodeCount|Aantal knoop punten opnieuw opstarten|Count|Totaal|Aantal knoop punten dat opnieuw wordt opgestart|Er zijn geen dimensies|
|ReimagingNodeCount|Telling van het aantal knoop punten|Count|Totaal|Aantal knoop punten van installatie kopieën|Er zijn geen dimensies|
|RunningNodeCount|Aantal actieve knoop punten|Count|Totaal|Aantal actieve knoop punten|Er zijn geen dimensies|
|LeavingPoolNodeCount|Aantal groeps knooppunten verlaten|Count|Totaal|Aantal knoop punten dat de pool verlaat|Er zijn geen dimensies|
|UnusableNodeCount|Aantal niet-bruikbare knoop punten|Count|Totaal|Aantal niet-bruikbare knoop punten|Er zijn geen dimensies|
|PreemptedNodeCount|Aantal knoop punten in herhaling|Count|Totaal|Aantal knoop punten dat is afgebroken|Er zijn geen dimensies|
|TaskStartEvent|Taak begin gebeurtenissen|Count|Totaal|Totaal aantal taken dat is gestart|Er zijn geen dimensies|
|TaskCompleteEvent|Taak voltooid gebeurtenissen|Count|Totaal|Totaal aantal taken dat is voltooid|Er zijn geen dimensies|
|TaskFailEvent|Taak fout gebeurtenissen|Count|Totaal|Totaal aantal taken dat is voltooid met de status mislukt|Er zijn geen dimensies|
|PoolCreateEvent|Groeps gebeurtenissen maken|Count|Totaal|Totaal aantal Pools dat is gemaakt|Er zijn geen dimensies|
|PoolResizeStartEvent|Begin gebeurtenissen van groeps grootte wijzigen|Count|Totaal|Totaal aantal hergroottes van de groep die zijn gestart|Er zijn geen dimensies|
|PoolResizeCompleteEvent|Volledige gebeurtenissen voor het wijzigen van de pool|Count|Totaal|Totaal aantal hergroottes van de groep die zijn voltooid|Er zijn geen dimensies|
|PoolDeleteStartEvent|Begin gebeurtenissen groep verwijderen|Count|Totaal|Totaal aantal verwijderde groepen dat is gestart|Er zijn geen dimensies|
|PoolDeleteCompleteEvent|Voltooide gebeurtenissen van groep verwijderen|Count|Totaal|Totaal aantal verwijderde groepen dat is voltooid|Er zijn geen dimensies|
|JobDeleteCompleteEvent|Voltooide gebeurtenissen van taak verwijderen|Count|Totaal|Het totale aantal taken dat is verwijderd.|Er zijn geen dimensies|
|JobDeleteStartEvent|Taak begin gebeurtenissen verwijderen|Count|Totaal|Het totale aantal taken dat is aangevraagd om te worden verwijderd.|Er zijn geen dimensies|
|JobDisableCompleteEvent|Voltooide gebeurtenissen voor taak uitschakelen|Count|Totaal|Het totale aantal taken dat is uitgeschakeld.|Er zijn geen dimensies|
|JobDisableStartEvent|Taak start gebeurtenissen uitschakelen|Count|Totaal|Het totale aantal taken dat is aangevraagd om te worden uitgeschakeld.|Er zijn geen dimensies|
|JobStartEvent|Taak begin gebeurtenissen|Count|Totaal|Het totale aantal taken dat is gestart.|Er zijn geen dimensies|
|JobTerminateCompleteEvent|Voltooide gebeurtenissen voor taak beëindigen|Count|Totaal|Het totale aantal taken dat is beëindigd.|Er zijn geen dimensies|
|JobTerminateStartEvent|Taak start gebeurtenissen beëindigen|Count|Totaal|Het totale aantal taken dat is aangevraagd om te worden beëindigd.|Er zijn geen dimensies|

## <a name="microsoftcacheredis"></a>Microsoft.Cache/redis

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|connectedclients|Verbonden clients|Count|Maximum||ShardId|
|totalcommandsprocessed|Totaal aantal bewerkingen|Count|Totaal||ShardId|
|cachehits|Cachetreffers|Count|Totaal||ShardId|
|cachemisses|Cachemissers|Count|Totaal||ShardId|
|getcommands|Opgehaalde items|Count|Totaal||ShardId|
|setcommands|Sets|Count|Totaal||ShardId|
|operationsPerSecond|Bewerkingen per seconde|Count|Maximum||ShardId|
|evictedkeys|Verwijderde sleutels|Count|Totaal||ShardId|
|totalkeys|Totaal aantal sleutels|Count|Maximum||ShardId|
|expiredkeys|Verlopen sleutels|Count|Totaal||ShardId|
|usedmemory|Gebruikt geheugen|Bytes|Maximum||ShardId|
|usedmemorypercentage|Percentage gebruikt geheugen|Percent|Maximum||ShardId|
|usedmemoryRss|Gebruikte geheugen-RSS|Bytes|Maximum||ShardId|
|serverLoad|Serverbelasting|Percent|Maximum||ShardId|
|cacheWrite|Cache schrijven|BytesPerSecond|Maximum||ShardId|
|cacheRead|Gelezen uit cache|BytesPerSecond|Maximum||ShardId|
|percentProcessorTime|CPU|Percent|Maximum||ShardId|
|cacheLatency|Cache latentie micro seconden (preview-versie)|Count|Average||ShardId, SampleType|
|fouten|Fouten|Count|Maximum||ShardId, error type|
|connectedclients0|Verbonden clients (Shard 0)|Count|Maximum||Er zijn geen dimensies|
|totalcommandsprocessed0|Totaal aantal bewerkingen (Shard 0)|Count|Totaal||Er zijn geen dimensies|
|cachehits0|Cache treffers (Shard 0)|Count|Totaal||Er zijn geen dimensies|
|cachemisses0|Cache missers (Shard 0)|Count|Totaal||Er zijn geen dimensies|
|getcommands0|Hiermee wordt opgehaald (Shard 0)|Count|Totaal||Er zijn geen dimensies|
|setcommands0|Sets (Shard 0)|Count|Totaal||Er zijn geen dimensies|
|operationsPerSecond0|Bewerkingen per seconde (Shard 0)|Count|Maximum||Er zijn geen dimensies|
|evictedkeys0|Verwijderde sleutels (Shard 0)|Count|Totaal||Er zijn geen dimensies|
|totalkeys0|Totaal aantal sleutels (Shard 0)|Count|Maximum||Er zijn geen dimensies|
|expiredkeys0|Verlopen sleutels (Shard 0)|Count|Totaal||Er zijn geen dimensies|
|usedmemory0|Gebruikt geheugen (Shard 0)|Bytes|Maximum||Er zijn geen dimensies|
|usedmemoryRss0|Gebruikt geheugen RSS (Shard 0)|Bytes|Maximum||Er zijn geen dimensies|
|serverLoad0|Server belasting (Shard 0)|Percent|Maximum||Er zijn geen dimensies|
|cacheWrite0|Cache schrijven (Shard 0)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|cacheRead0|Cache gelezen (Shard 0)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|percentProcessorTime0|CPU (Shard 0)|Percent|Maximum||Er zijn geen dimensies|
|connectedclients1|Verbonden clients (Shard 1)|Count|Maximum||Er zijn geen dimensies|
|totalcommandsprocessed1|Totaal aantal bewerkingen (Shard 1)|Count|Totaal||Er zijn geen dimensies|
|cachehits1|Cache treffers (Shard 1)|Count|Totaal||Er zijn geen dimensies|
|cachemisses1|Cache missers (Shard 1)|Count|Totaal||Er zijn geen dimensies|
|getcommands1|Hiermee wordt opgehaald (Shard 1)|Count|Totaal||Er zijn geen dimensies|
|setcommands1|Sets (Shard 1)|Count|Totaal||Er zijn geen dimensies|
|operationsPerSecond1|Bewerkingen per seconde (Shard 1)|Count|Maximum||Er zijn geen dimensies|
|evictedkeys1|Verwijderde sleutels (Shard 1)|Count|Totaal||Er zijn geen dimensies|
|totalkeys1|Totaal aantal sleutels (Shard 1)|Count|Maximum||Er zijn geen dimensies|
|expiredkeys1|Verlopen sleutels (Shard 1)|Count|Totaal||Er zijn geen dimensies|
|usedmemory1|Gebruikt geheugen (Shard 1)|Bytes|Maximum||Er zijn geen dimensies|
|usedmemoryRss1|Gebruikte geheugen RSS (Shard 1)|Bytes|Maximum||Er zijn geen dimensies|
|serverLoad1|Server belasting (Shard 1)|Percent|Maximum||Er zijn geen dimensies|
|cacheWrite1|Cache schrijven (Shard 1)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|cacheRead1|Lees bewerking in cache (Shard 1)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|percentProcessorTime1|CPU (Shard 1)|Percent|Maximum||Er zijn geen dimensies|
|connectedclients2|Verbonden clients (Shard 2)|Count|Maximum||Er zijn geen dimensies|
|totalcommandsprocessed2|Totaal aantal bewerkingen (Shard 2)|Count|Totaal||Er zijn geen dimensies|
|cachehits2|Cache treffers (Shard 2)|Count|Totaal||Er zijn geen dimensies|
|cachemisses2|Cache missers (Shard 2)|Count|Totaal||Er zijn geen dimensies|
|getcommands2|Hiermee wordt opgehaald (Shard 2)|Count|Totaal||Er zijn geen dimensies|
|setcommands2|Sets (Shard 2)|Count|Totaal||Er zijn geen dimensies|
|operationsPerSecond2|Bewerkingen per seconde (Shard 2)|Count|Maximum||Er zijn geen dimensies|
|evictedkeys2|Verwijderde sleutels (Shard 2)|Count|Totaal||Er zijn geen dimensies|
|totalkeys2|Totaal aantal sleutels (Shard 2)|Count|Maximum||Er zijn geen dimensies|
|expiredkeys2|Verlopen sleutels (Shard 2)|Count|Totaal||Er zijn geen dimensies|
|usedmemory2|Gebruikt geheugen (Shard 2)|Bytes|Maximum||Er zijn geen dimensies|
|usedmemoryRss2|Gebruikte geheugen RSS (Shard 2)|Bytes|Maximum||Er zijn geen dimensies|
|serverLoad2|Server belasting (Shard 2)|Percent|Maximum||Er zijn geen dimensies|
|cacheWrite2|Cache schrijven (Shard 2)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|cacheRead2|Lees bewerking in cache (Shard 2)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|percentProcessorTime2|CPU (Shard 2)|Percent|Maximum||Er zijn geen dimensies|
|connectedclients3|Verbonden clients (Shard 3)|Count|Maximum||Er zijn geen dimensies|
|totalcommandsprocessed3|Totaal aantal bewerkingen (Shard 3)|Count|Totaal||Er zijn geen dimensies|
|cachehits3|Cache treffers (Shard 3)|Count|Totaal||Er zijn geen dimensies|
|cachemisses3|Cache missers (Shard 3)|Count|Totaal||Er zijn geen dimensies|
|getcommands3|Hiermee wordt opgehaald (Shard 3)|Count|Totaal||Er zijn geen dimensies|
|setcommands3|Sets (Shard 3)|Count|Totaal||Er zijn geen dimensies|
|operationsPerSecond3|Bewerkingen per seconde (Shard 3)|Count|Maximum||Er zijn geen dimensies|
|evictedkeys3|Verwijderde sleutels (Shard 3)|Count|Totaal||Er zijn geen dimensies|
|totalkeys3|Totaal aantal sleutels (Shard 3)|Count|Maximum||Er zijn geen dimensies|
|expiredkeys3|Verlopen sleutels (Shard 3)|Count|Totaal||Er zijn geen dimensies|
|usedmemory3|Gebruikt geheugen (Shard 3)|Bytes|Maximum||Er zijn geen dimensies|
|usedmemoryRss3|Gebruikte geheugen RSS (Shard 3)|Bytes|Maximum||Er zijn geen dimensies|
|serverLoad3|Server belasting (Shard 3)|Percent|Maximum||Er zijn geen dimensies|
|cacheWrite3|Cache schrijven (Shard 3)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|cacheRead3|Lees bewerking in cache (Shard 3)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|percentProcessorTime3|CPU (Shard 3)|Percent|Maximum||Er zijn geen dimensies|
|connectedclients4|Verbonden clients (Shard 4)|Count|Maximum||Er zijn geen dimensies|
|totalcommandsprocessed4|Totaal aantal bewerkingen (Shard 4)|Count|Totaal||Er zijn geen dimensies|
|cachehits4|Cache treffers (Shard 4)|Count|Totaal||Er zijn geen dimensies|
|cachemisses4|Cache missers (Shard 4)|Count|Totaal||Er zijn geen dimensies|
|getcommands4|Hiermee wordt opgehaald (Shard 4)|Count|Totaal||Er zijn geen dimensies|
|setcommands4|Sets (Shard 4)|Count|Totaal||Er zijn geen dimensies|
|operationsPerSecond4|Bewerkingen per seconde (Shard 4)|Count|Maximum||Er zijn geen dimensies|
|evictedkeys4|Verwijderde sleutels (Shard 4)|Count|Totaal||Er zijn geen dimensies|
|totalkeys4|Totaal aantal sleutels (Shard 4)|Count|Maximum||Er zijn geen dimensies|
|expiredkeys4|Verlopen sleutels (Shard 4)|Count|Totaal||Er zijn geen dimensies|
|usedmemory4|Gebruikt geheugen (Shard 4)|Bytes|Maximum||Er zijn geen dimensies|
|usedmemoryRss4|Gebruikte geheugen RSS (Shard 4)|Bytes|Maximum||Er zijn geen dimensies|
|serverLoad4|Server belasting (Shard 4)|Percent|Maximum||Er zijn geen dimensies|
|cacheWrite4|Cache schrijven (Shard 4)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|cacheRead4|Lees bewerking in cache (Shard 4)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|percentProcessorTime4|CPU (Shard 4)|Percent|Maximum||Er zijn geen dimensies|
|connectedclients5|Verbonden clients (Shard 5)|Count|Maximum||Er zijn geen dimensies|
|totalcommandsprocessed5|Totaal aantal bewerkingen (Shard 5)|Count|Totaal||Er zijn geen dimensies|
|cachehits5|Cache treffers (Shard 5)|Count|Totaal||Er zijn geen dimensies|
|cachemisses5|Cache missers (Shard 5)|Count|Totaal||Er zijn geen dimensies|
|getcommands5|Hiermee wordt opgehaald (Shard 5)|Count|Totaal||Er zijn geen dimensies|
|setcommands5|Sets (Shard 5)|Count|Totaal||Er zijn geen dimensies|
|operationsPerSecond5|Bewerkingen per seconde (Shard 5)|Count|Maximum||Er zijn geen dimensies|
|evictedkeys5|Verwijderde sleutels (Shard 5)|Count|Totaal||Er zijn geen dimensies|
|totalkeys5|Totaal aantal sleutels (Shard 5)|Count|Maximum||Er zijn geen dimensies|
|expiredkeys5|Verlopen sleutels (Shard 5)|Count|Totaal||Er zijn geen dimensies|
|usedmemory5|Gebruikt geheugen (Shard 5)|Bytes|Maximum||Er zijn geen dimensies|
|usedmemoryRss5|Gebruikte geheugen RSS (Shard 5)|Bytes|Maximum||Er zijn geen dimensies|
|serverLoad5|Server belasting (Shard 5)|Percent|Maximum||Er zijn geen dimensies|
|cacheWrite5|Cache schrijven (Shard 5)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|cacheRead5|Lees bewerking in cache (Shard 5)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|percentProcessorTime5|CPU (Shard 5)|Percent|Maximum||Er zijn geen dimensies|
|connectedclients6|Verbonden clients (Shard 6)|Count|Maximum||Er zijn geen dimensies|
|totalcommandsprocessed6|Totaal aantal bewerkingen (Shard 6)|Count|Totaal||Er zijn geen dimensies|
|cachehits6|Cache treffers (Shard 6)|Count|Totaal||Er zijn geen dimensies|
|cachemisses6|Cache missers (Shard 6)|Count|Totaal||Er zijn geen dimensies|
|getcommands6|Hiermee wordt opgehaald (Shard 6)|Count|Totaal||Er zijn geen dimensies|
|setcommands6|Sets (Shard 6)|Count|Totaal||Er zijn geen dimensies|
|operationsPerSecond6|Bewerkingen per seconde (Shard 6)|Count|Maximum||Er zijn geen dimensies|
|evictedkeys6|Verwijderde sleutels (Shard 6)|Count|Totaal||Er zijn geen dimensies|
|totalkeys6|Totaal aantal sleutels (Shard 6)|Count|Maximum||Er zijn geen dimensies|
|expiredkeys6|Verlopen sleutels (Shard 6)|Count|Totaal||Er zijn geen dimensies|
|usedmemory6|Gebruikt geheugen (Shard 6)|Bytes|Maximum||Er zijn geen dimensies|
|usedmemoryRss6|Gebruikte geheugen RSS (Shard 6)|Bytes|Maximum||Er zijn geen dimensies|
|serverLoad6|Server belasting (Shard 6)|Percent|Maximum||Er zijn geen dimensies|
|cacheWrite6|Cache schrijven (Shard 6)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|cacheRead6|Lees bewerking in cache (Shard 6)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|percentProcessorTime6|CPU (Shard 6)|Percent|Maximum||Er zijn geen dimensies|
|connectedclients7|Verbonden clients (Shard 7)|Count|Maximum||Er zijn geen dimensies|
|totalcommandsprocessed7|Totaal aantal bewerkingen (Shard 7)|Count|Totaal||Er zijn geen dimensies|
|cachehits7|Cache treffers (Shard 7)|Count|Totaal||Er zijn geen dimensies|
|cachemisses7|Cache missers (Shard 7)|Count|Totaal||Er zijn geen dimensies|
|getcommands7|Hiermee wordt opgehaald (Shard 7)|Count|Totaal||Er zijn geen dimensies|
|setcommands7|Sets (Shard 7)|Count|Totaal||Er zijn geen dimensies|
|operationsPerSecond7|Bewerkingen per seconde (Shard 7)|Count|Maximum||Er zijn geen dimensies|
|evictedkeys7|Verwijderde sleutels (Shard 7)|Count|Totaal||Er zijn geen dimensies|
|totalkeys7|Totaal aantal sleutels (Shard 7)|Count|Maximum||Er zijn geen dimensies|
|expiredkeys7|Verlopen sleutels (Shard 7)|Count|Totaal||Er zijn geen dimensies|
|usedmemory7|Gebruikt geheugen (Shard 7)|Bytes|Maximum||Er zijn geen dimensies|
|usedmemoryRss7|Gebruikte geheugen RSS (Shard 7)|Bytes|Maximum||Er zijn geen dimensies|
|serverLoad7|Server belasting (Shard 7)|Percent|Maximum||Er zijn geen dimensies|
|cacheWrite7|Cache schrijven (Shard 7)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|cacheRead7|Lees bewerking in cache (Shard 7)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|percentProcessorTime7|CPU (Shard 7)|Percent|Maximum||Er zijn geen dimensies|
|connectedclients8|Verbonden clients (Shard 8)|Count|Maximum||Er zijn geen dimensies|
|totalcommandsprocessed8|Totaal aantal bewerkingen (Shard 8)|Count|Totaal||Er zijn geen dimensies|
|cachehits8|Cache treffers (Shard 8)|Count|Totaal||Er zijn geen dimensies|
|cachemisses8|Cache missers (Shard 8)|Count|Totaal||Er zijn geen dimensies|
|getcommands8|Hiermee wordt opgehaald (Shard 8)|Count|Totaal||Er zijn geen dimensies|
|setcommands8|Sets (Shard 8)|Count|Totaal||Er zijn geen dimensies|
|operationsPerSecond8|Bewerkingen per seconde (Shard 8)|Count|Maximum||Er zijn geen dimensies|
|evictedkeys8|Verwijderde sleutels (Shard 8)|Count|Totaal||Er zijn geen dimensies|
|totalkeys8|Totaal aantal sleutels (Shard 8)|Count|Maximum||Er zijn geen dimensies|
|expiredkeys8|Verlopen sleutels (Shard 8)|Count|Totaal||Er zijn geen dimensies|
|usedmemory8|Gebruikt geheugen (Shard 8)|Bytes|Maximum||Er zijn geen dimensies|
|usedmemoryRss8|Gebruikt geheugen RSS (Shard 8)|Bytes|Maximum||Er zijn geen dimensies|
|serverLoad8|Server belasting (Shard 8)|Percent|Maximum||Er zijn geen dimensies|
|cacheWrite8|Cache schrijven (Shard 8)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|cacheRead8|Lees bewerking in cache (Shard 8)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|percentProcessorTime8|CPU (Shard 8)|Percent|Maximum||Er zijn geen dimensies|
|connectedclients9|Verbonden clients (Shard 9)|Count|Maximum||Er zijn geen dimensies|
|totalcommandsprocessed9|Totaal aantal bewerkingen (Shard 9)|Count|Totaal||Er zijn geen dimensies|
|cachehits9|Cache treffers (Shard 9)|Count|Totaal||Er zijn geen dimensies|
|cachemisses9|Cache missers (Shard 9)|Count|Totaal||Er zijn geen dimensies|
|getcommands9|Hiermee wordt opgehaald (Shard 9)|Count|Totaal||Er zijn geen dimensies|
|setcommands9|Sets (Shard 9)|Count|Totaal||Er zijn geen dimensies|
|operationsPerSecond9|Bewerkingen per seconde (Shard 9)|Count|Maximum||Er zijn geen dimensies|
|evictedkeys9|Verwijderde sleutels (Shard 9)|Count|Totaal||Er zijn geen dimensies|
|totalkeys9|Totaal aantal sleutels (Shard 9)|Count|Maximum||Er zijn geen dimensies|
|expiredkeys9|Verlopen sleutels (Shard 9)|Count|Totaal||Er zijn geen dimensies|
|usedmemory9|Gebruikt geheugen (Shard 9)|Bytes|Maximum||Er zijn geen dimensies|
|usedmemoryRss9|Gebruikte geheugen RSS (Shard 9)|Bytes|Maximum||Er zijn geen dimensies|
|serverLoad9|Server belasting (Shard 9)|Percent|Maximum||Er zijn geen dimensies|
|cacheWrite9|Cache schrijven (Shard 9)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|cacheRead9|Lees bewerking in cache (Shard 9)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|percentProcessorTime9|CPU (Shard 9)|Percent|Maximum||Er zijn geen dimensies|

## <a name="microsoftclassiccomputevirtualmachines"></a>Microsoft.ClassicCompute/virtualMachines

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|Percentage CPU|Percentage CPU|Procent|Average|Het percentage toegewezen berekeningseenheden dat momenteel door de virtuele machine(s) wordt gebruikt.|Er zijn geen dimensies|
|Netwerk in|Netwerk in|Bytes|Totaal|Het aantal ontvangen bytes op alle netwerkinterfaces door de virtuele machine(s) (binnenkomend verkeer).|Er zijn geen dimensies|
|Netwerk uit|Netwerk uit|Bytes|Totaal|Het aantal verzonden bytes op alle netwerkinterfaces door de virtuele machine(s) (uitgaand verkeer).|Er zijn geen dimensies|
|Gelezen bytes per seconde op de schijf|Schijf lezen|BytesPerSecond|Average|Gemiddeld aantal gelezen bytes op de schijf tijdens de bewakingsperiode.|Er zijn geen dimensies|
|Geschreven bytes per seconde|Schijf schrijven|BytesPerSecond|Average|Gemiddeld aantal geschreven bytes op de schijf tijdens de bewakingsperiode.|Er zijn geen dimensies|
|Leesbewerkingen op de schijf/seconde|Leesbewerkingen op de schijf/seconde|CountPerSecond|Average|Gelezen IOP's op de schijf.|Er zijn geen dimensies|
|Schrijfbewerkingen op de schijf/seconde|Schrijfbewerkingen op de schijf/seconde|CountPerSecond|Average|Geschreven IOP's op de schijf.|Er zijn geen dimensies|

## <a name="microsoftclassiccomputedomainnamesslotsroles"></a>Micro soft. ClassicCompute/domein naam/sleuven/rollen

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|Percentage CPU|Percentage CPU|Procent|Average|Het percentage toegewezen berekeningseenheden dat momenteel door de virtuele machine(s) wordt gebruikt.|RoleInstanceId|
|Netwerk in|Netwerk in|Bytes|Totaal|Het aantal ontvangen bytes op alle netwerkinterfaces door de virtuele machine(s) (binnenkomend verkeer).|RoleInstanceId|
|Netwerk uit|Netwerk uit|Bytes|Totaal|Het aantal verzonden bytes op alle netwerkinterfaces door de virtuele machine(s) (uitgaand verkeer).|RoleInstanceId|
|Gelezen bytes per seconde op de schijf|Schijf lezen|BytesPerSecond|Average|Gemiddeld aantal gelezen bytes op de schijf tijdens de bewakingsperiode.|RoleInstanceId|
|Geschreven bytes per seconde|Schijf schrijven|BytesPerSecond|Average|Gemiddeld aantal geschreven bytes op de schijf tijdens de bewakingsperiode.|RoleInstanceId|
|Leesbewerkingen op de schijf/seconde|Leesbewerkingen op de schijf/seconde|CountPerSecond|Average|Gelezen IOP's op de schijf.|RoleInstanceId|
|Schrijfbewerkingen op de schijf/seconde|Schrijfbewerkingen op de schijf/seconde|CountPerSecond|Average|Geschreven IOP's op de schijf.|RoleInstanceId|

## <a name="microsoftcognitiveservicesaccounts"></a>Microsoft.CognitiveServices/accounts

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|TotalCalls|Totaal aantal aanroepen|Count|Totaal|Totaal aantal aanroepen.|ApiName, OperationName, Region|
|SuccessfulCalls|Geslaagde aanroepen|Count|Totaal|Aantal geslaagde aanroepen.|ApiName, OperationName, Region|
|TotalErrors|Totaal aantal fouten|Count|Totaal|Het totaal aantal aanroepen met een foutbericht (HTTP-antwoordcode 4xx of 5xx).|ApiName, OperationName, Region|
|BlockedCalls|Geblokkeerde aanroepen|Count|Totaal|Het aantal aanroepen die de tarief- of quotumlimiet hebben overschreden.|ApiName, OperationName, Region|
|ServerErrors|Serverfouten|Count|Totaal|Het aantal aanroepen met een interne servicefout (HTTP-responscode 5xx).|ApiName, OperationName, Region|
|ClientErrors|Clientfouten|Count|Totaal|Het aantal aanroepen met een fout aan de clientzijde (HTTP-responscode 4xx).|ApiName, OperationName, Region|
|DataIn|Inkomende gegevens|Bytes|Totaal|Grootte van inkomende gegevens in bytes.|ApiName, OperationName, Region|
|DataOut|Uitgaande gegevens|Bytes|Totaal|Grootte van uitgaande gegevens in bytes.|ApiName, OperationName, Region|
|Latentie|Latentie|MilliSeconds|Average|Latentie in milliseconden|ApiName, OperationName, Region|
|CharactersTranslated|Tekens vertaald|Count|Totaal|Totaal aantal tekens in inkomende tekstaanvraag.|ApiName, OperationName, Region|
|CharactersTrained|Getrainde tekens|Count|Totaal|Totaal aantal getrainde tekens.|ApiName, OperationName, Region|
|SpeechSessionDuration|Duur van de spraaksessie|Seconden|Totaal|Totale duur van de spraaksessie in seconden.|ApiName, OperationName, Region|
|TotalTransactions|Totaal van transacties|Count|Totaal|Het totale aantal transacties.|Er zijn geen dimensies|
|TotalTokenCalls|Totaal aantal tokenaanroepen|Count|Totaal|Het totale aantal tokenaanroepen.|ApiName, OperationName, Region|

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.Compute/virtualMachines

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|Percentage CPU|Percentage CPU|Procent|Gemiddeld|Het percentage toegewezen berekeningseenheden dat momenteel door de virtuele machine(s) wordt gebruikt|Er zijn geen dimensies|
|Netwerk in|Netwerk inkomend - factureerbaar|Bytes|Totaal|Het aantal ontvangen factureerbare bytes op alle netwerkinterfaces door de virtuele machine(s) (binnenkomend verkeer)|Er zijn geen dimensies|
|Netwerk uit|Netwerk uitgaand - factureerbaar|Bytes|Totaal|Het aantal verzonden factureerbare bytes op alle netwerkinterfaces door de virtuele machine(s) (uitgaand verkeer)|Er zijn geen dimensies|
|Gelezen bytes op de schijf|Gelezen bytes op de schijf|Bytes|Totaal|Het aantal vanaf schijf gelezen bytes tijdens de bewakingsperiode|Er zijn geen dimensies|
|Geschreven bytes op de schijf|Geschreven bytes op de schijf|Bytes|Totaal|Het aantal naar schijf geschreven bytes tijdens de bewakingsperiode|Er zijn geen dimensies|
|Leesbewerkingen op de schijf/seconde|Leesbewerkingen op de schijf/seconde|CountPerSecond|Average|Gelezen IOP's op de schijf|Er zijn geen dimensies|
|Schrijfbewerkingen op de schijf/seconde|Schrijfbewerkingen op de schijf/seconde|CountPerSecond|Average|Geschreven IOP's op de schijf|Er zijn geen dimensies|
|Resterend CPU-tegoed|Resterend CPU-tegoed|Count|Average|Totale tegoed beschikbaar voor burst|Er zijn geen dimensies|
|Verbruikt CPU-tegoed|Verbruikt CPU-tegoed|Count|Average|Totale tegoed gebruikt door de virtuele machine|Er zijn geen dimensies|
|Per schijf gelezen bytes per seconde|Vanaf de gegevensschijf gelezen bytes per seconde (afgeschaft)|CountPerSecond|Average|Het aantal vanaf één schijf gelezen bytes per seconde tijdens de bewakingsperiode|SlotId|
|Per schijf geschreven bytes per seconde|Naar de gegevensschijf geschreven bytes per seconde (afgeschaft)|CountPerSecond|Average|Het aantal naar één schijf geschreven bytes per seconde tijdens de bewakingsperiode|SlotId|
|Leesbewerkingen per schijf per seconde|Leesbewerkingen op de gegevensschijf per seconde (afgeschaft)|CountPerSecond|Average|Het aantal van één schijf gelezen IOPS tijdens de bewakingsperiode|SlotId|
|Schrijfbewerkingen per schijf per seconde|Schrijfbewerkingen op de gegevensschijf per seconde (afgeschaft)|CountPerSecond|Average|Het aantal naar één schijf geschreven IOPS tijdens de bewakingsperiode|SlotId|
|Wachtrijomvang per schijf|Wachtrijomvang van de gegevensschijf (afgeschaft)|Count|Average|Wachtrijomvang (of wachtrijlengte) voor de gegevensschijf|SlotId|
|Per besturingssysteemschijf gelezen bytes per seconde|Vanaf de besturingssysteemschijf gelezen bytes per seconde (afgeschaft)|CountPerSecond|Average|Het aantal vanaf één schijf gelezen bytes per seconde tijdens de bewakingsperiode voor de besturingssysteemschijf|Er zijn geen dimensies|
|Per besturingssysteemschijf geschreven bytes per seconde|Naar de besturingssysteemschijf geschreven bytes per seconde (afgeschaft)|CountPerSecond|Average|Het aantal naar één schijf geschreven bytes per seconde tijdens de bewakingsperiode voor de besturingssysteemschijf|Er zijn geen dimensies|
|Leesbewerkingen per besturingssysteemschijf per seconde|Leesbewerkingen op de besturingssysteemschijf per seconde (afgeschaft)|CountPerSecond|Average|Het aantal van één schijf gelezen IOPS tijdens de bewakingsperiode voor de besturingssysteemschijf|Er zijn geen dimensies|
|Schrijfbewerkingen per besturingssysteemschijf per seconde|Schrijfbewerkingen op de besturingssysteemschijf per seconde (afgeschaft)|CountPerSecond|Average|Het aantal naar één schijf geschreven IOPS tijdens de bewakingsperiode voor de besturingssysteemschijf|Er zijn geen dimensies|
|Wachtrijomvang per besturingssysteemschijf|Wachtrijomvang van de besturingssysteemschijf (afgeschaft)|Count|Average|Wachtrijomvang (of wachtrijlengte) voor de besturingssysteemschijf|Er zijn geen dimensies|
|Vanaf de gegevensschijf gelezen bytes per seconde|Van de gegevensschijf gelezen bytes per seconde (preview)|CountPerSecond|Average|Het aantal vanaf één schijf gelezen bytes per seconde tijdens de bewakingsperiode|LUN|
|Naar de gegevensschijf geschreven bytes per seconde|Naar de gegevensschijf geschreven bytes per seconde (preview)|CountPerSecond|Average|Het aantal naar één schijf geschreven bytes per seconde tijdens de bewakingsperiode|LUN|
|Leesbewerkingen op de gegevensschijf per seconde|Leesbewerkingen op de gegevensschijf per seconde (preview)|CountPerSecond|Average|Het aantal van één schijf gelezen IOPS tijdens de bewakingsperiode|LUN|
|Schrijfbewerkingen op de gegevensschijf per seconde|Schrijfbewerkingen op de gegevensschijf per seconde (preview)|CountPerSecond|Average|Het aantal naar één schijf geschreven IOPS tijdens de bewakingsperiode|LUN|
|Wachtrijlengte van gegevensschijf|Wachtrijlengte van gegevensschijf (preview)|Count|Average|Wachtrijomvang (of wachtrijlengte) voor de gegevensschijf|LUN|
|Vanaf de besturingssysteemschijf gelezen bytes per seconde|Van de besturingssysteemschijf gelezen bytes per seconde (preview)|CountPerSecond|Average|Het aantal vanaf één schijf gelezen bytes per seconde tijdens de bewakingsperiode voor de besturingssysteemschijf|Er zijn geen dimensies|
|Naar de besturingssysteemschijf geschreven bytes per seconde|Naar de besturingssysteemschijf geschreven bytes per seconde (preview)|CountPerSecond|Average|Het aantal naar één schijf geschreven bytes per seconde tijdens de bewakingsperiode voor de besturingssysteemschijf|Er zijn geen dimensies|
|Leesbewerkingen op de besturingssysteemschijf per seconde|Leesbewerkingen op de besturingssysteemschijf per seconde (preview)|CountPerSecond|Average|Het aantal van één schijf gelezen IOPS tijdens de bewakingsperiode voor de besturingssysteemschijf|Er zijn geen dimensies|
|Schrijfbewerkingen op de besturingssysteemschijf per seconde|Schrijfbewerkingen op de besturingssysteemschijf per seconde (preview)|CountPerSecond|Average|Het aantal naar één schijf geschreven IOPS tijdens de bewakingsperiode voor de besturingssysteemschijf|Er zijn geen dimensies|
|Wachtrijlengte van besturingssysteemschijf|Wachtrijlengte van besturingssysteemschijf (preview)|Count|Average|Wachtrijomvang (of wachtrijlengte) voor de besturingssysteemschijf|Er zijn geen dimensies|
|Binnenkomende stromen|Binnenkomende stromen (preview-versie)|Count|Average|Binnenkomende stromen is het aantal huidige stromen in de binnenkomende richting (verkeer dat naar de virtuele machine gaat)|Er zijn geen dimensies|
|Uitgaande stromen|Uitgaande stromen (preview-versie)|Count|Average|Uitgaande stromen is het aantal huidige stromen in de uitgaande richting (verkeer dat uit de virtuele machine gaat)|Er zijn geen dimensies|
|Maximale maakfrequentie van binnenkomende stromen|Maximale maakfrequentie van binnenkomende stromen (preview)|CountPerSecond|Average|De maximale maakfrequentie van binnenkomende stromen (verkeer dat naar de virtuele machine gaat)|Er zijn geen dimensies|
|Maximale maakfrequentie van uitgaande stromen|Maximale maakfrequentie van uitgaande stromen (preview)|CountPerSecond|Average|De maximale maakfrequentie van uitgaande stromen (verkeer dat uit de virtuele machine komt)|Er zijn geen dimensies|
|Cachetreffers voor lezen van Premium-gegevensschijf|Cachetreffers voor lezen van Premium-gegevensschijf (preview)|Percent|Average|Cachetreffers voor lezen van Premium-gegevensschijf|LUN|
|Cachemissers voor lezen van Premium-gegevensschijf|Cachemissers voor lezen van Premium-gegevensschijf (preview)|Percent|Average|Cachemissers voor lezen van Premium-gegevensschijf|LUN|
|Cachetreffers voor lezen van Premium-besturingssysteemschijf|Cachetreffers voor lezen van Premium-besturingssysteemschijf (preview)|Percent|Average|Cachetreffers voor lezen van Premium-besturingssysteemschijf|Er zijn geen dimensies|
|Cachemissers voor lezen van Premium-besturingssysteemschijf|Cachemissers voor lezen van Premium-besturingssysteemschijf (preview)|Percent|Average|Cachemissers voor lezen van Premium-besturingssysteemschijf|Er zijn geen dimensies|
|Netwerk inkomend - totaal|Netwerk inkomend - totaal|Bytes|Totaal|Het aantal ontvangen bytes op alle netwerkinterfaces door de virtuele machine(s) (binnenkomend verkeer)|Er zijn geen dimensies|
|Netwerk uitgaand - totaal|Netwerk uitgaand - totaal|Bytes|Totaal|Het aantal verzonden bytes op alle netwerkinterfaces door de virtuele machine(s) (uitgaand verkeer)|Er zijn geen dimensies|

## <a name="microsoftcomputevirtualmachinescalesets"></a>Microsoft.Compute/virtualMachineScaleSets

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|Percentage CPU|Percentage CPU|Procent|Gemiddeld|Het percentage toegewezen berekeningseenheden dat momenteel door de virtuele machine(s) wordt gebruikt|VMName|
|Netwerk in|Netwerk inkomend - factureerbaar|Bytes|Totaal|Het aantal ontvangen factureerbare bytes op alle netwerkinterfaces door de virtuele machine(s) (binnenkomend verkeer)|VMName|
|Netwerk uit|Netwerk uitgaand - factureerbaar|Bytes|Totaal|Het aantal verzonden factureerbare bytes op alle netwerkinterfaces door de virtuele machine(s) (uitgaand verkeer)|VMName|
|Gelezen bytes op de schijf|Gelezen bytes op de schijf|Bytes|Totaal|Het aantal vanaf schijf gelezen bytes tijdens de bewakingsperiode|VMName|
|Geschreven bytes op de schijf|Geschreven bytes op de schijf|Bytes|Totaal|Het aantal naar schijf geschreven bytes tijdens de bewakingsperiode|VMName|
|Leesbewerkingen op de schijf/seconde|Leesbewerkingen op de schijf/seconde|CountPerSecond|Average|Gelezen IOP's op de schijf|VMName|
|Schrijfbewerkingen op de schijf/seconde|Schrijfbewerkingen op de schijf/seconde|CountPerSecond|Average|Geschreven IOP's op de schijf|VMName|
|Resterend CPU-tegoed|Resterend CPU-tegoed|Count|Average|Totale tegoed beschikbaar voor burst|Er zijn geen dimensies|
|Verbruikt CPU-tegoed|Verbruikt CPU-tegoed|Count|Average|Totale tegoed gebruikt door de virtuele machine|Er zijn geen dimensies|
|Per schijf gelezen bytes per seconde|Vanaf de gegevensschijf gelezen bytes per seconde (afgeschaft)|CountPerSecond|Average|Het aantal vanaf één schijf gelezen bytes per seconde tijdens de bewakingsperiode|SlotId|
|Per schijf geschreven bytes per seconde|Naar de gegevensschijf geschreven bytes per seconde (afgeschaft)|CountPerSecond|Average|Het aantal naar één schijf geschreven bytes per seconde tijdens de bewakingsperiode|SlotId|
|Leesbewerkingen per schijf per seconde|Leesbewerkingen op de gegevensschijf per seconde (afgeschaft)|CountPerSecond|Average|Het aantal van één schijf gelezen IOPS tijdens de bewakingsperiode|SlotId|
|Schrijfbewerkingen per schijf per seconde|Schrijfbewerkingen op de gegevensschijf per seconde (afgeschaft)|CountPerSecond|Average|Het aantal naar één schijf geschreven IOPS tijdens de bewakingsperiode|SlotId|
|Wachtrijomvang per schijf|Wachtrijomvang van de gegevensschijf (afgeschaft)|Count|Average|Wachtrijomvang (of wachtrijlengte) voor de gegevensschijf|SlotId|
|Per besturingssysteemschijf gelezen bytes per seconde|Vanaf de besturingssysteemschijf gelezen bytes per seconde (afgeschaft)|CountPerSecond|Average|Het aantal vanaf één schijf gelezen bytes per seconde tijdens de bewakingsperiode voor de besturingssysteemschijf|Er zijn geen dimensies|
|Per besturingssysteemschijf geschreven bytes per seconde|Naar de besturingssysteemschijf geschreven bytes per seconde (afgeschaft)|CountPerSecond|Average|Het aantal naar één schijf geschreven bytes per seconde tijdens de bewakingsperiode voor de besturingssysteemschijf|Er zijn geen dimensies|
|Leesbewerkingen per besturingssysteemschijf per seconde|Leesbewerkingen op de besturingssysteemschijf per seconde (afgeschaft)|CountPerSecond|Average|Het aantal van één schijf gelezen IOPS tijdens de bewakingsperiode voor de besturingssysteemschijf|Er zijn geen dimensies|
|Schrijfbewerkingen per besturingssysteemschijf per seconde|Schrijfbewerkingen op de besturingssysteemschijf per seconde (afgeschaft)|CountPerSecond|Average|Het aantal naar één schijf geschreven IOPS tijdens de bewakingsperiode voor de besturingssysteemschijf|Er zijn geen dimensies|
|Wachtrijomvang per besturingssysteemschijf|Wachtrijomvang van de besturingssysteemschijf (afgeschaft)|Count|Average|Wachtrijomvang (of wachtrijlengte) voor de besturingssysteemschijf|Er zijn geen dimensies|
|Vanaf de gegevensschijf gelezen bytes per seconde|Van de gegevensschijf gelezen bytes per seconde (preview)|CountPerSecond|Average|Het aantal vanaf één schijf gelezen bytes per seconde tijdens de bewakingsperiode|LUN, VMName|
|Naar de gegevensschijf geschreven bytes per seconde|Naar de gegevensschijf geschreven bytes per seconde (preview)|CountPerSecond|Average|Het aantal naar één schijf geschreven bytes per seconde tijdens de bewakingsperiode|LUN, VMName|
|Leesbewerkingen op de gegevensschijf per seconde|Leesbewerkingen op de gegevensschijf per seconde (preview)|CountPerSecond|Average|Het aantal van één schijf gelezen IOPS tijdens de bewakingsperiode|LUN, VMName|
|Schrijfbewerkingen op de gegevensschijf per seconde|Schrijfbewerkingen op de gegevensschijf per seconde (preview)|CountPerSecond|Average|Het aantal naar één schijf geschreven IOPS tijdens de bewakingsperiode|LUN, VMName|
|Wachtrijlengte van gegevensschijf|Wachtrijlengte van gegevensschijf (preview)|Count|Average|Wachtrijomvang (of wachtrijlengte) voor de gegevensschijf|LUN, VMName|
|Vanaf de besturingssysteemschijf gelezen bytes per seconde|Van de besturingssysteemschijf gelezen bytes per seconde (preview)|CountPerSecond|Average|Het aantal vanaf één schijf gelezen bytes per seconde tijdens de bewakingsperiode voor de besturingssysteemschijf|VMName|
|Naar de besturingssysteemschijf geschreven bytes per seconde|Naar de besturingssysteemschijf geschreven bytes per seconde (preview)|CountPerSecond|Average|Het aantal naar één schijf geschreven bytes per seconde tijdens de bewakingsperiode voor de besturingssysteemschijf|VMName|
|Leesbewerkingen op de besturingssysteemschijf per seconde|Leesbewerkingen op de besturingssysteemschijf per seconde (preview)|CountPerSecond|Average|Het aantal van één schijf gelezen IOPS tijdens de bewakingsperiode voor de besturingssysteemschijf|VMName|
|Schrijfbewerkingen op de besturingssysteemschijf per seconde|Schrijfbewerkingen op de besturingssysteemschijf per seconde (preview)|CountPerSecond|Average|Het aantal naar één schijf geschreven IOPS tijdens de bewakingsperiode voor de besturingssysteemschijf|VMName|
|Wachtrijlengte van besturingssysteemschijf|Wachtrijlengte van besturingssysteemschijf (preview)|Count|Average|Wachtrijomvang (of wachtrijlengte) voor de besturingssysteemschijf|VMName|
|Binnenkomende stromen|Binnenkomende stromen (preview-versie)|Count|Average|Binnenkomende stromen is het aantal huidige stromen in de binnenkomende richting (verkeer dat naar de virtuele machine gaat)|VMName|
|Uitgaande stromen|Uitgaande stromen (preview-versie)|Count|Average|Uitgaande stromen is het aantal huidige stromen in de uitgaande richting (verkeer dat uit de virtuele machine gaat)|VMName|
|Maximale maakfrequentie van binnenkomende stromen|Maximale maakfrequentie van binnenkomende stromen (preview)|CountPerSecond|Average|De maximale maakfrequentie van binnenkomende stromen (verkeer dat naar de virtuele machine gaat)|VMName|
|Maximale maakfrequentie van uitgaande stromen|Maximale maakfrequentie van uitgaande stromen (preview)|CountPerSecond|Average|De maximale maakfrequentie van uitgaande stromen (verkeer dat uit de virtuele machine komt)|VMName|
|Cachetreffers voor lezen van Premium-gegevensschijf|Cachetreffers voor lezen van Premium-gegevensschijf (preview)|Percent|Average|Cachetreffers voor lezen van Premium-gegevensschijf|LUN, VMName|
|Cachemissers voor lezen van Premium-gegevensschijf|Cachemissers voor lezen van Premium-gegevensschijf (preview)|Percent|Average|Cachemissers voor lezen van Premium-gegevensschijf|LUN, VMName|
|Cachetreffers voor lezen van Premium-besturingssysteemschijf|Cachetreffers voor lezen van Premium-besturingssysteemschijf (preview)|Percent|Average|Cachetreffers voor lezen van Premium-besturingssysteemschijf|VMName|
|Cachemissers voor lezen van Premium-besturingssysteemschijf|Cachemissers voor lezen van Premium-besturingssysteemschijf (preview)|Percent|Average|Cachemissers voor lezen van Premium-besturingssysteemschijf|VMName|
|Netwerk inkomend - totaal|Netwerk inkomend - totaal|Bytes|Totaal|Het aantal ontvangen bytes op alle netwerkinterfaces door de virtuele machine(s) (binnenkomend verkeer)|VMName|
|Netwerk uitgaand - totaal|Netwerk uitgaand - totaal|Bytes|Totaal|Het aantal verzonden bytes op alle netwerkinterfaces door de virtuele machine(s) (uitgaand verkeer)|VMName|

## <a name="microsoftcontainerinstancecontainergroups"></a>Microsoft.ContainerInstance/containerGroups

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|CpuUsage|CPU-gebruik|Count|Average|CPU-gebruik op alle kerngeheugens in millicores.|containerName|
|MemoryUsage|Geheugengebruik|Bytes|Average|Totaal geheugengebruik in bytes.|containerName|
|NetworkBytesReceivedPerSecond|Netwerkbytes ontvangen per seconde|Bytes|Average|Het aantal bytes dat het netwerk per seconde heeft ontvangen.|Er zijn geen dimensies|
|NetworkBytesTransmittedPerSecond|Netwerkbytes verzonden per seconde|Bytes|Average|Het aantal bytes dat het netwerk per seconde heeft verzonden.|Er zijn geen dimensies|

## <a name="microsoftcontainerregistryregistries"></a>Micro soft. ContainerRegistry/registers

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|TotalPullCount|Totaal aantal pull-bewerkingen|Count|Average|Aantal opgehaalde afbeeldingen in totaal|Er zijn geen dimensies|
|SuccessfulPullCount|Aantal geslaagde pull-bewerkingen|Count|Average|Aantal geslaagde installatie kopieën|Er zijn geen dimensies|
|TotalPushCount|Totaal aantal push berichten|Count|Average|Aantal afbeeldings pushes in totaal|Er zijn geen dimensies|
|SuccessfulPushCount|Aantal geslaagde push berichten|Count|Average|Aantal geslaagde pushes voor installatie kopie|Er zijn geen dimensies|
|RunDuration|Uitvoerings duur|Milliseconden|Totaal|Uitvoerings duur in milliseconden|Er zijn geen dimensies|

## <a name="microsoftcontainerservicemanagedclusters"></a>Microsoft.ContainerService/managedClusters

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|kube_node_status_allocatable_cpu_cores|Totaal aantal beschik bare CPU-kernen in een beheerd cluster|Count|Totaal|Totaal aantal beschik bare CPU-kernen in een beheerd cluster|Er zijn geen dimensies|
|kube_node_status_allocatable_memory_bytes|Totale hoeveelheid beschikbaar geheugen in een beheerd cluster|Bytes|Totaal|Totale hoeveelheid beschikbaar geheugen in een beheerd cluster|Er zijn geen dimensies|
|kube_pod_status_ready|Aantal in de status gereed|Count|Totaal|Aantal in de status gereed|naam ruimte, pod|
|kube_node_status_condition|Statussen voor de verschillende knooppunt voorwaarden|Count|Totaal|Statussen voor de verschillende knooppunt voorwaarden|voor waarde, status, status2, knoop punt|
|kube_pod_status_phase|Aantal per fase|Count|Totaal|Aantal per fase|fase, naam ruimte, pod|

## <a name="microsoftcustomerinsightshubs"></a>Microsoft.CustomerInsights/hubs

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|DCIApiCalls|Customer Insights-API-aanroepen|Count|Totaal||Er zijn geen dimensies|
|DCIMappingImportOperationSuccessfulLines|Voltooide regels voor het importeren van de toewijzing|Count|Totaal||Er zijn geen dimensies|
|DCIMappingImportOperationFailedLines|De regels voor het importeren van de toewijzing zijn mislukt|Count|Totaal||Er zijn geen dimensies|
|DCIMappingImportOperationTotalLines|Totaal regels voor import bewerking toewijzen|Count|Totaal||Er zijn geen dimensies|
|DCIMappingImportOperationRuntimeInSeconds|Runtime van import bewerking in seconden toewijzen|Seconden|Totaal||Er zijn geen dimensies|
|DCIOutboundProfileExportSucceeded|Het uitgaande profiel is geëxporteerd|Count|Totaal||Er zijn geen dimensies|
|DCIOutboundProfileExportFailed|Exporteren van uitgaand profiel is mislukt|Count|Totaal||Er zijn geen dimensies|
|DCIOutboundProfileExportDuration|Export duur van uitgaand profiel|Seconden|Totaal||Er zijn geen dimensies|
|DCIOutboundKpiExportSucceeded|Het exporteren van de uitgaande KPI is voltooid|Count|Totaal||Er zijn geen dimensies|
|DCIOutboundKpiExportFailed|Het exporteren van de uitgaande KPI is mislukt|Count|Totaal||Er zijn geen dimensies|
|DCIOutboundKpiExportDuration|Export duur van uitgaande KPI|Seconden|Totaal||Er zijn geen dimensies|
|DCIOutboundKpiExportStarted|Het exporteren van de uitgaande KPI is gestart|Seconden|Totaal||Er zijn geen dimensies|
|DCIOutboundKpiRecordCount|Aantal uitgaande KPI-records|Seconden|Totaal||Er zijn geen dimensies|
|DCIOutboundProfileExportCount|Aantal export uitgaande profielen|Seconden|Totaal||Er zijn geen dimensies|
|DCIOutboundInitialProfileExportFailed|Kan het uitgaande initiële profiel niet exporteren|Seconden|Totaal||Er zijn geen dimensies|
|DCIOutboundInitialProfileExportSucceeded|Het uitgaande initiële profiel is geëxporteerd|Seconden|Totaal||Er zijn geen dimensies|
|DCIOutboundInitialKpiExportFailed|Het exporteren van de uitgaande begin KPI is mislukt|Seconden|Totaal||Er zijn geen dimensies|
|DCIOutboundInitialKpiExportSucceeded|Het exporteren van de uitgaande begin KPI is voltooid|Seconden|Totaal||Er zijn geen dimensies|
|DCIOutboundInitialProfileExportDurationInSeconds|Export duur van het uitgaande begin profiel in enkele seconden|Seconden|Totaal||Er zijn geen dimensies|
|AdlaJobForStandardKpiFailed|De Adla-taak voor de standaard-KPI is in enkele seconden mislukt|Seconden|Totaal||Er zijn geen dimensies|
|AdlaJobForStandardKpiTimeOut|Adla-taak voor standaard-KPI-time-out in seconden|Seconden|Totaal||Er zijn geen dimensies|
|AdlaJobForStandardKpiCompleted|De Adla-taak voor de standaard-KPI is in enkele seconden voltooid|Seconden|Totaal||Er zijn geen dimensies|
|ImportASAValuesFailed|Aantal mislukte import-waarden van ASA|Count|Totaal||Er zijn geen dimensies|
|ImportASAValuesSucceeded|Aantal geslaagde activeringen voor ASA|Count|Totaal||Er zijn geen dimensies|
|DCIProfilesCount|Aantal exemplaren van profiel|Count|Laatste||Er zijn geen dimensies|
|DCIInteractionsPerMonthCount|Aantal interacties per maand|Count|Laatste||Er zijn geen dimensies|
|DCIKpisCount|Aantal KPI'S|Count|Laatste||Er zijn geen dimensies|
|DCISegmentsCount|Aantal segmenten|Count|Laatste||Er zijn geen dimensies|
|DCIPredictiveMatchPoliciesCount|Aantal voorspellende overeenkomsten|Count|Laatste||Er zijn geen dimensies|
|DCIPredictionsCount|Aantal voorspellingen|Count|Laatste||Er zijn geen dimensies|

## <a name="microsoftdataboxedgedataboxedgedevices"></a>Microsoft.DataBoxEdge/dataBoxEdgeDevices

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|NICReadThroughput|Leesdoorvoer (netwerk)|BytesPerSecond|Average|De leesdoorvoer van de netwerkinterface op het apparaat in de rapportageperiode voor alle volumes in de gateway.|InstanceName|
|NICWriteThroughput|Schrijfdoorvoer (netwerk)|BytesPerSecond|Average|De schrijfdoorvoer van de netwerkinterface op het apparaat in de rapportageperiode voor alle volumes in de gateway.|InstanceName|
|CloudReadThroughputPerShare|Downloaddoorvoer cloud (share)|BytesPerSecond|Average|De downloaddoorvoer naar Azure vanaf een share tijdens de rapportageperiode.|Delen|
|CloudUploadThroughputPerShare|Uploaddoorvoer cloud (share)|BytesPerSecond|Average|De uploaddoorvoer naar Azure vanaf een share tijdens de rapportageperiode.|Delen|
|BytesUploadedToCloudPerShare|Geüploade cloudbytes (share)|Bytes|Average|Het totale aantal bytes dat naar Azure is geüpload vanaf een share tijdens de rapportageperiode.|Delen|
|TotalCapacity|Totale capaciteit|Bytes|Average|Totale capaciteit|Er zijn geen dimensies|
|Availablecapacity;)|Beschikbare capaciteit|Bytes|Average|De beschikbare capaciteit in bytes tijdens de rapportageperiode.|Er zijn geen dimensies|
|CloudUploadThroughput|Uploaddoorvoer cloud|BytesPerSecond|Average|De uploaddoorvoer van de cloud naar Azure tijdens de rapportageperiode.|Er zijn geen dimensies|
|CloudReadThroughput|Downloaddoorvoer cloud|BytesPerSecond|Average|De downloaddoorvoer van de cloud naar Azure tijdens de rapportageperiode.|Er zijn geen dimensies|
|BytesUploadedToCloud|Geüploade cloudbytes (apparaat)|Bytes|Average|Het totale aantal bytes dat naar Azure is geüpload vanaf een apparaat tijdens de rapportageperiode.|Er zijn geen dimensies|
|HyperVVirtualProcessorUtilization|Rand Compute - CPU-Percentage|Percent|Average|Percentage CPU-gebruik|InstanceName|
|HyperVMemoryUtilization|Compute - geheugengebruik rand|Percent|Average|Hoeveelheid RAM in gebruik|InstanceName|

## <a name="microsoftdatafactorydatafactories"></a>Microsoft.DataFactory/datafactories

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|FailedRuns|Mislukte verwerkingen|Count|Totaal||pipelineName, activiteitsnummer|
|SuccessfulRuns|Geslaagde verwerkingen|Count|Totaal||pipelineName, activiteitsnummer|

## <a name="microsoftdatafactoryfactories"></a>Microsoft.DataFactory/factories

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|PipelineFailedRuns|Metrische gegevens van mislukte pijplijn uitvoeringen|Count|Totaal||FailureType, naam|
|PipelineSucceededRuns|Metrische uitvoerings metingen geslaagde pijp lijnen|Count|Totaal||FailureType, naam|
|ActivityFailedRuns|Metrische gegevens mislukte uitvoering van activiteit|Count|Totaal||Activity type, PipelineName, FailureType, name|
|ActivitySucceededRuns|Metrische gegevens uitvoeringen uitgevoerde activiteit|Count|Totaal||Activity type, PipelineName, FailureType, name|
|TriggerFailedRuns|Meet waarden voor uitvoering van mislukte triggers|Count|Totaal||Naam, FailureType|
|TriggerSucceededRuns|Meet waarden voor uitvoering van geslaagde triggers|Count|Totaal||Naam, FailureType|
|IntegrationRuntimeCpuPercentage|CPU-gebruik van Integration runtime|Percent|Average||IntegrationRuntimeName, knooppuntnaam|
|IntegrationRuntimeAvailableMemory|Beschik bare geheugen voor Integration runtime|Bytes|Average||IntegrationRuntimeName, knooppuntnaam|
|MaxAllowedResourceCount|Maximum aantal toegestane entiteiten|Count|Maximum||Er zijn geen dimensies|
|MaxAllowedFactorySizeInGbUnits|Maxi maal toegestane grootte van de fabriek (GB-eenheid)|Count|Maximum||Er zijn geen dimensies|
|ResourceCount|Totaal aantal entiteiten|Count|Maximum||Er zijn geen dimensies|
|FactorySizeInGbUnits|Totale grootte van de fabriek (GB-eenheid)|Count|Maximum||Er zijn geen dimensies|

## <a name="microsoftdatalakeanalyticsaccounts"></a>Microsoft.DataLakeAnalytics/accounts

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|JobEndedSuccess|Geslaagde taken|Count|Totaal|Aantal geslaagde taken.|Er zijn geen dimensies|
|JobEndedFailure|Mislukte taken|Count|Totaal|Aantal mislukte taken.|Er zijn geen dimensies|
|JobEndedCancelled|Geannuleerde taken|Count|Totaal|Aantal geannuleerde taken.|Er zijn geen dimensies|
|JobAUEndedSuccess|Geslaagde AU-tijd|Seconden|Totaal|Totale AU-tijd voor voltooide taken.|Er zijn geen dimensies|
|JobAUEndedFailure|Mislukte AU-tijd|Seconden|Totaal|Totale AU-tijd voor mislukte taken.|Er zijn geen dimensies|
|JobAUEndedCancelled|Geannuleerde AU-tijd|Seconden|Totaal|Totale AU-tijd voor geannuleerde taken.|Er zijn geen dimensies|

## <a name="microsoftdatalakestoreaccounts"></a>Microsoft.DataLakeStore/accounts

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|TotalStorage|Totale opslag|Bytes|Maximum|De totale hoeveelheid gegevens die in het account is opgeslagen.|Er zijn geen dimensies|
|DataWritten|Gegevens geschreven|Bytes|Totaal|De totale hoeveelheid gegevens die naar het account wordt geschreven.|Er zijn geen dimensies|
|DataRead|Gegevens lezen|Bytes|Totaal|De totale hoeveelheid gegevens die uit het account is gelezen.|Er zijn geen dimensies|
|WriteRequests|Aanvragen schrijven|Count|Totaal|Het aantal aanvragen voor het schrijven van gegevens naar het account.|Er zijn geen dimensies|
|ReadRequests|Aanvragen lezen|Count|Totaal|Aantal aanvragen voor het lezen van gegevens voor het account.|Er zijn geen dimensies|

## <a name="microsoftdbformariadbservers"></a>Microsoft.DBforMariaDB/servers

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|cpu_percent|CPU-percentage|Percent|Average|CPU-percentage|Er zijn geen dimensies|
|memory_percent|Geheugen percentage|Percent|Average|Geheugen percentage|Er zijn geen dimensies|
|io_consumption_percent|IO-percentage|Percent|Average|IO-percentage|Er zijn geen dimensies|
|storage_percent|Opslag percentage|Percent|Average|Opslag percentage|Er zijn geen dimensies|
|storage_used|Opslag gebruikt|Bytes|Average|Opslag gebruikt|Er zijn geen dimensies|
|storage_limit|Opslag limiet|Bytes|Average|Opslag limiet|Er zijn geen dimensies|
|serverlog_storage_percent|Percentage server logboek opslag|Percent|Average|Percentage server logboek opslag|Er zijn geen dimensies|
|serverlog_storage_usage|Gebruikte server logboek opslag|Bytes|Average|Gebruikte server logboek opslag|Er zijn geen dimensies|
|serverlog_storage_limit|Opslag limiet voor server logboek|Bytes|Average|Opslag limiet voor server logboek|Er zijn geen dimensies|
|active_connections|Actieve verbindingen|Count|Average|Actieve verbindingen|Er zijn geen dimensies|
|connections_failed|Mislukte verbindingen|Count|Totaal|Mislukte verbindingen|Er zijn geen dimensies|
|seconds_behind_master|Replicatie vertraging in seconden|Count|Average|Replicatie vertraging in seconden|Er zijn geen dimensies|
|backup_storage_used|Gebruikte back-upopslag|Bytes|Average|Gebruikte back-upopslag|Er zijn geen dimensies|
|network_bytes_egress|Netwerk uit|Bytes|Totaal|Netwerk uit over actieve verbindingen|Er zijn geen dimensies|
|network_bytes_ingress|Netwerk in|Bytes|Totaal|Netwerk in meerdere actieve verbindingen|Er zijn geen dimensies|

## <a name="microsoftdbformysqlservers"></a>Microsoft.DBforMySQL/servers

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|cpu_percent|CPU-percentage|Percent|Average|CPU-percentage|Er zijn geen dimensies|
|memory_percent|Geheugen percentage|Percent|Average|Geheugen percentage|Er zijn geen dimensies|
|io_consumption_percent|IO-percentage|Percent|Average|IO-percentage|Er zijn geen dimensies|
|storage_percent|Opslag percentage|Percent|Average|Opslag percentage|Er zijn geen dimensies|
|storage_used|Opslag gebruikt|Bytes|Average|Opslag gebruikt|Er zijn geen dimensies|
|storage_limit|Opslag limiet|Bytes|Average|Opslag limiet|Er zijn geen dimensies|
|serverlog_storage_percent|Percentage server logboek opslag|Percent|Average|Percentage server logboek opslag|Er zijn geen dimensies|
|serverlog_storage_usage|Gebruikte server logboek opslag|Bytes|Average|Gebruikte server logboek opslag|Er zijn geen dimensies|
|serverlog_storage_limit|Opslag limiet voor server logboek|Bytes|Average|Opslag limiet voor server logboek|Er zijn geen dimensies|
|active_connections|Actieve verbindingen|Count|Average|Actieve verbindingen|Er zijn geen dimensies|
|connections_failed|Mislukte verbindingen|Count|Totaal|Mislukte verbindingen|Er zijn geen dimensies|
|seconds_behind_master|Replicatie vertraging in seconden|Count|Average|Replicatie vertraging in seconden|Er zijn geen dimensies|
|backup_storage_used|Gebruikte back-upopslag|Bytes|Average|Gebruikte back-upopslag|Er zijn geen dimensies|
|network_bytes_egress|Netwerk uit|Bytes|Totaal|Netwerk uit over actieve verbindingen|Er zijn geen dimensies|
|network_bytes_ingress|Netwerk in|Bytes|Totaal|Netwerk in meerdere actieve verbindingen|Er zijn geen dimensies|

## <a name="microsoftdbforpostgresqlservers"></a>Microsoft.DBforPostgreSQL/servers

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|cpu_percent|CPU-percentage|Percent|Average|CPU-percentage|Er zijn geen dimensies|
|memory_percent|Geheugen percentage|Percent|Average|Geheugen percentage|Er zijn geen dimensies|
|io_consumption_percent|IO-percentage|Percent|Average|IO-percentage|Er zijn geen dimensies|
|storage_percent|Opslag percentage|Percent|Average|Opslag percentage|Er zijn geen dimensies|
|storage_used|Opslag gebruikt|Bytes|Average|Opslag gebruikt|Er zijn geen dimensies|
|storage_limit|Opslag limiet|Bytes|Average|Opslag limiet|Er zijn geen dimensies|
|serverlog_storage_percent|Percentage server logboek opslag|Percent|Average|Percentage server logboek opslag|Er zijn geen dimensies|
|serverlog_storage_usage|Gebruikte server logboek opslag|Bytes|Average|Gebruikte server logboek opslag|Er zijn geen dimensies|
|serverlog_storage_limit|Opslag limiet voor server logboek|Bytes|Average|Opslag limiet voor server logboek|Er zijn geen dimensies|
|active_connections|Actieve verbindingen|Count|Average|Actieve verbindingen|Er zijn geen dimensies|
|connections_failed|Mislukte verbindingen|Count|Totaal|Mislukte verbindingen|Er zijn geen dimensies|
|backup_storage_used|Gebruikte back-upopslag|Bytes|Average|Gebruikte back-upopslag|Er zijn geen dimensies|
|network_bytes_egress|Netwerk uit|Bytes|Totaal|Netwerk uit over actieve verbindingen|Er zijn geen dimensies|
|network_bytes_ingress|Netwerk in|Bytes|Totaal|Netwerk in meerdere actieve verbindingen|Er zijn geen dimensies|
|pg_replica_log_delay_in_seconds|Replica vertraging|Seconden|Maximum|Replica vertraging in seconden|Er zijn geen dimensies|
|pg_replica_log_delay_in_bytes|Maximale vertraging in Replica's|Bytes|Maximum|Vertraging in bytes van de meest vertraagde replica|Er zijn geen dimensies|

## <a name="microsoftdbforpostgresqlserversv2"></a>Microsoft.DBforPostgreSQL/serversv2

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|cpu_percent|CPU-percentage|Percent|Average|CPU-percentage|Er zijn geen dimensies|
|memory_percent|Geheugen percentage|Percent|Average|Geheugen percentage|Er zijn geen dimensies|
|IOPS|IOPS|Count|Average|I/o-bewerkingen per seconde|Er zijn geen dimensies|
|storage_percent|Opslag percentage|Percent|Average|Opslag percentage|Er zijn geen dimensies|
|storage_used|Opslag gebruikt|Bytes|Average|Opslag gebruikt|Er zijn geen dimensies|
|active_connections|Actieve verbindingen|Count|Average|Actieve verbindingen|Er zijn geen dimensies|
|network_bytes_egress|Netwerk uit|Bytes|Totaal|Netwerk uit over actieve verbindingen|Er zijn geen dimensies|
|network_bytes_ingress|Netwerk in|Bytes|Totaal|Netwerk in meerdere actieve verbindingen|Er zijn geen dimensies|

## <a name="microsoftdevicesiothubs"></a>Microsoft.Devices/IotHubs

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|d2c.telemetry.ingress.allProtocol|Verzend pogingen voor telemetrie-berichten|Count|Totaal|Aantal pogingen voor het verzenden van apparaat-naar-Cloud-telemetrie naar uw IoT hub|Er zijn geen dimensies|
|d2c.telemetry.ingress.success|Verzonden telemetriegegevens|Count|Totaal|Aantal te verzenden apparaat-naar-Cloud-telemetrie-berichten naar uw IoT-hub|Er zijn geen dimensies|
|C2D. commands. OUTuitgang. complete. geslaagd|Opdrachten voltooid|Count|Totaal|Aantal Cloud-naar-apparaat-opdrachten die zijn voltooid door het apparaat|Er zijn geen dimensies|
|c2d.commands.egress.abandon.success|Opdrachten afgebroken|Count|Totaal|Aantal Cloud-naar-apparaat-opdrachten die zijn afgebroken door het apparaat|Er zijn geen dimensies|
|c2d.commands.egress.reject.success|Geweigerde opdrachten|Count|Totaal|Aantal Cloud-naar-apparaat-opdrachten dat door het apparaat is afgewezen|Er zijn geen dimensies|
|devices.totalDevices|Totaal aantal apparaten (afgeschaft)|Count|Totaal|Aantal apparaten dat is geregistreerd bij uw IoT-hub|Er zijn geen dimensies|
|devices.connectedDevices.allProtocol|Verbonden apparaten (afgeschaft) |Count|Totaal|Aantal apparaten dat is verbonden met uw IoT-hub|Er zijn geen dimensies|
|d2c.telemetry.egress.success|Route ring: telemetrie-berichten|Count|Totaal|Het aantal keren dat berichten zijn bezorgd bij alle eind punten met behulp van IoT Hub route ring. Als een bericht wordt doorgestuurd naar meerdere eind punten, wordt deze waarde met één verhoogd voor elke geslaagde levering. Als een bericht meerdere keren op hetzelfde eind punt wordt bezorgd, wordt deze waarde met één verhoogd voor elke geslaagde levering.|Er zijn geen dimensies|
|d2c.telemetry.egress.dropped|Route ring: telemetrie-berichten verwijderd |Count|Totaal|Het aantal keren dat berichten zijn verwijderd door IoT Hub route ring vanwege Dead-eind punten. Deze waarde telt geen berichten die worden bezorgd als terugval route als berichten die worden verzonden, niet worden bezorgd.|Er zijn geen dimensies|
|d2c.telemetry.egress.orphaned|Route ring: telemetriegegevens van zwevende berichten |Count|Totaal|Het aantal keren dat berichten zijn zwevend door IoT Hub route ring, omdat ze niet overeenkomen met de regels voor door sturen (inclusief de terugval regel). |Er zijn geen dimensies|
|d2c.telemetry.egress.invalid|Route ring: telemetrie-berichten incompatibel|Count|Totaal|Het aantal keren dat IoT Hub route ring geen berichten kan leveren als gevolg van incompatibiliteit met het eind punt. Deze waarde omvat geen nieuwe pogingen.|Er zijn geen dimensies|
|d2c.telemetry.egress.fallback|Route ring: berichten worden bezorgd bij terugval|Count|Totaal|Het aantal keren dat de route ring van berichten IoT Hub verzonden naar het eind punt dat is gekoppeld aan de terugval route.|Er zijn geen dimensies|
|d2c.endpoints.egress.eventHubs|Route ring: berichten worden bezorgd bij Event hub|Count|Totaal|Het aantal keren dat IoT Hub route ring berichten heeft geleverd aan Event hub-eind punten.|Er zijn geen dimensies|
|d2c.endpoints.latency.eventHubs|Route ring: bericht latentie voor Event hub|Milliseconden|Average|De gemiddelde latentie (in milliseconden) tussen het binnenkomen van berichten IoT Hub en het binnenkomen van berichten in een event hub-eind punt.|Er zijn geen dimensies|
|d2c.endpoints.egress.serviceBusQueues|Route ring: berichten worden bezorgd bij Service Bus wachtrij|Count|Totaal|Het aantal keren dat IoT Hub route ring berichten heeft geleverd aan Service Bus-wachtrij-eind punten.|Er zijn geen dimensies|
|d2c.endpoints.latency.serviceBusQueues|Route ring: bericht latentie voor Service Bus wachtrij|Milliseconden|Average|De gemiddelde latentie (in milliseconden) tussen het binnenkomen van berichten naar IoT Hub en telemetrie-berichten in een Service Bus wachtrij-eind punt.|Er zijn geen dimensies|
|d2c.endpoints.egress.serviceBusTopics|Route ring: berichten die worden bezorgd bij Service Bus onderwerp|Count|Totaal|Het aantal keren dat IoT Hub route ring berichten heeft geleverd aan Service Bus onderwerp-eind punten.|Er zijn geen dimensies|
|d2c.endpoints.latency.serviceBusTopics|Route ring: bericht latentie voor Service Bus onderwerp|Milliseconden|Average|De gemiddelde latentie (in milliseconden) tussen het binnenkomen van berichten naar IoT Hub en telemetrie-berichten in het eind punt van een Service Bus onderwerp.|Er zijn geen dimensies|
|D2C. endpoints. uitgangs punt. builtIn. Events|Route ring: berichten worden bezorgd bij berichten/gebeurtenissen|Count|Totaal|Het aantal keren dat IoT Hub route ring berichten heeft geleverd aan het ingebouwde eind punt (berichten/gebeurtenissen). Deze metrische gegevens worden alleen gestart als route ring is https://aka.ms/iotrouting) ingeschakeld (voor de IOT-hub.|Er zijn geen dimensies|
|d2c.endpoints.latency.builtIn.events|Route ring: bericht latentie voor berichten/gebeurtenissen|Milliseconden|Average|De gemiddelde latentie (in milliseconden) tussen het binnenkomen van berichten naar IoT Hub en het inkomend telemetrie-bericht in het ingebouwde eind punt (berichten/gebeurtenissen). Deze metrische gegevens worden alleen gestart als route ring is https://aka.ms/iotrouting) ingeschakeld (voor de IOT-hub.|Er zijn geen dimensies|
|d2c.endpoints.egress.storage|Route ring: berichten worden bezorgd bij de opslag|Count|Totaal|Het aantal keren dat IoT Hub route ring berichten heeft geleverd aan de opslag eindpunten.|Er zijn geen dimensies|
|d2c.endpoints.latency.storage|Route ring: bericht latentie voor opslag|Milliseconden|Average|De gemiddelde latentie (in milliseconden) tussen het binnenkomen van berichten naar IoT Hub en telemetrie-berichten in een opslag eindpunt.|Er zijn geen dimensies|
|D2C. endpoints. out. storage. bytes|Route ring: gegevens worden geleverd aan de opslag|Bytes|Totaal|De hoeveelheid gegevens (bytes) IoT Hub route ring die aan de opslag eindpunten wordt geleverd.|Er zijn geen dimensies|
|d2c.endpoints.egress.storage.blobs|Route ring: blobs die aan de opslag worden geleverd|Count|Totaal|Het aantal keren dat IoT Hub route ring blobs naar opslag eindpunten heeft geleverd.|Er zijn geen dimensies|
|EventGridDeliveries|Event Grid leveringen (preview-versie)|Count|Totaal|Het aantal IoT Hub gebeurtenissen dat is gepubliceerd op Event Grid. Gebruik de dimensie resultaat voor het aantal geslaagde en mislukte aanvragen. De dimensie type-tekst geeft het soort https://aka.ms/ioteventgrid) gebeurtenis weer (.|Resultaat, type gebeurtenis|
|EventGridLatency|De gemiddelde latentie (in milliseconden) vanaf het moment waarop de IOT hub-gebeurtenis werd gegenereerd toen de gebeurtenis werd gepubliceerd in Event Grid. Dit getal is een gemiddelde tussen alle gebeurtenis typen. Gebruik de dimensie type type om de latentie van een specifiek soort gebeurtenis weer te geven.|EventType|
|d2c.twin.read.success|Geslaagde dubbele Lees bewerkingen van apparaten|Count|Totaal|De telling van alle geslaagde apparaten met dubbele Lees bewerkingen.|Er zijn geen dimensies|
|D2C. dubbele. Read. failure|Mislukte dubbele Lees bewerkingen van apparaten|Count|Totaal|Het aantal apparaten dat niet kan worden gestart, dubbele Lees bewerkingen.|Er zijn geen dimensies|
|d2c.twin.read.size|Reactie grootte van dubbele Lees bewerkingen van apparaten|Bytes|Average|Het gemiddelde, het minimum en het maximum van alle geslaagde apparaten-geïnitieerde dubbele Lees bewerkingen.|Er zijn geen dimensies|
|d2c.twin.update.success|Geslaagde dubbele updates van apparaten|Count|Totaal|De telling van alle geslaagde, door het apparaat geïnitieerde dubbele updates.|Er zijn geen dimensies|
|D2C. dubbele. update. failure|Mislukte dubbele updates van apparaten|Count|Totaal|Het aantal apparaten dat door een apparaat is gestart en dubbele updates heeft uitgevoerd.|Er zijn geen dimensies|
|D2C. dubbele. update. grootte|Grootte van dubbele updates van apparaten|Bytes|Average|Het gemiddelde, het minimum en de maximale grootte van alle geslaagde, door het apparaat geïnitieerde dubbele updates.|Er zijn geen dimensies|
|C2D. methods. geslaagd|Geslaagde directe aanroepen van de methode|Count|Totaal|Het aantal voltooide direct-methode aanroepen.|Er zijn geen dimensies|
|C2D. methods. failure|Mislukte directe aanroepen van methode|Count|Totaal|Het aantal mislukte direct-methode aanroepen.|Er zijn geen dimensies|
|C2D. methods. requestSize|Aanvraag grootte van directe-methode aanroepen|Bytes|Average|Het gemiddelde, het minimum en het maximum van alle geslaagde direct-methode aanvragen.|Er zijn geen dimensies|
|C2D. methods. responseSize|Antwoord grootte van directe methode aanroepen|Bytes|Average|Het gemiddelde, het minimum en het maximum van alle geslaagde reacties van de methode direct.|Er zijn geen dimensies|
|c2d.twin.read.success|Geslaagde dubbele Lees bewerkingen van back-end|Count|Totaal|Het aantal geslaagde back-end-geïnitieerde dubbele Lees bewerkingen.|Er zijn geen dimensies|
|c2d.twin.read.failure|Mislukte dubbele Lees bewerkingen van back-end|Count|Totaal|Het aantal mislukte back-end-geïnitieerde dubbele Lees bewerkingen.|Er zijn geen dimensies|
|c2d.twin.read.size|Reactie grootte van dubbele Lees bewerkingen van de back-end|Bytes|Average|Het gemiddelde, het minimum en het maximum van alle geslaagde back-end-geïnitieerde dubbele Lees bewerkingen.|Er zijn geen dimensies|
|c2d.twin.update.success|Geslaagde dubbele updates van back-end|Count|Totaal|Het aantal geslaagde, door de back-end gestarte dubbele updates.|Er zijn geen dimensies|
|C2D. dubbele. update. failure|Mislukte dubbele updates van back-end|Count|Totaal|Het aantal niet-geslaagde, door de back-end geïnitieerde dubbele updates.|Er zijn geen dimensies|
|C2D. dubbele. update. grootte|Grootte van dubbele updates van back-end|Bytes|Average|Het gemiddelde, het minimum en de maximale grootte van alle geslaagde back-end-geïnitieerde dubbele updates.|Er zijn geen dimensies|
|twinQueries.success|Geslaagde dubbele query's|Count|Totaal|Het aantal geslaagde dubbele query's.|Er zijn geen dimensies|
|twinQueries.failure|Mislukte dubbele query's|Count|Totaal|Het aantal mislukte dubbele query's.|Er zijn geen dimensies|
|twinQueries.resultSize|Resultaat grootte van dubbele query's|Bytes|Average|Het gemiddelde, het minimum en het maximum van de resultaat grootte van alle geslaagde dubbele query's.|Er zijn geen dimensies|
|jobs.createTwinUpdateJob.success|Geslaagde creatie van dubbele update taken|Count|Totaal|Het aantal van alle geslaagde taken voor het maken van dubbele updates.|Er zijn geen dimensies|
|jobs.createTwinUpdateJob.failure|Kan geen dubbele update taken uitvoeren|Count|Totaal|Het aantal mislukte het maken van dubbele update taken.|Er zijn geen dimensies|
|jobs.createDirectMethodJob.success|Geslaagde creatie van methode aanroep taken|Count|Totaal|Het aantal van alle geslaagde aanroepen van directe methode aanroep taken.|Er zijn geen dimensies|
|jobs.createDirectMethodJob.failure|Kan geen aanroepen van methode aanroep taken uitvoeren|Count|Totaal|Het aantal van alle mislukte aanroepen van directe methode aanroep taken.|Er zijn geen dimensies|
|jobs.listJobs.success|Geslaagde aanroepen naar lijst taken|Count|Totaal|Het aantal geslaagde aanroepen naar lijst taken.|Er zijn geen dimensies|
|jobs.listJobs.failure|Mislukte aanroepen naar lijst taken|Count|Totaal|Het aantal mislukte aanroepen naar lijst taken.|Er zijn geen dimensies|
|jobs.cancelJob.success|Voltooide taak annuleringen|Count|Totaal|Het aantal geslaagde aanroepen om een taak te annuleren.|Er zijn geen dimensies|
|jobs.cancelJob.failure|Mislukte taak annuleringen|Count|Totaal|Het aantal mislukte aanroepen om een taak te annuleren.|Er zijn geen dimensies|
|jobs.queryJobs.success|Geslaagde taak query's|Count|Totaal|Het aantal geslaagde aanroepen naar query taken.|Er zijn geen dimensies|
|jobs.queryJobs.failure|Mislukte taak query's|Count|Totaal|Het aantal mislukte aanroepen naar query taken.|Er zijn geen dimensies|
|Jobs. voltooid|Voltooide taken|Count|Totaal|Het aantal voltooide taken.|Er zijn geen dimensies|
|Jobs. mislukt|Mislukte taken|Count|Totaal|Het aantal mislukte taken.|Er zijn geen dimensies|
|d2c.telemetry.ingress.sendThrottle|Aantal beperkings fouten|Count|Totaal|Aantal beperkings fouten door doorvoer vertraging van apparaat|Er zijn geen dimensies|
|dailyMessageQuotaUsed|Totaal aantal gebruikte berichten|Count|Average|Het totale aantal berichten dat momenteel wordt gebruikt. Dit is een cumulatieve waarde die elke dag opnieuw wordt ingesteld op nul om 00:00 UTC.|Er zijn geen dimensies|
|deviceDataUsage|Totale hoeveelheid gegevens gebruik van apparaat|Bytes|Totaal|Verzonden bytes van en naar apparaten die zijn verbonden met IotHub|Er zijn geen dimensies|
|totalDeviceCount|Totaal aantal apparaten (preview-versie)|Count|Average|Aantal apparaten dat is geregistreerd bij uw IoT-hub|Er zijn geen dimensies|
|connectedDeviceCount|Verbonden apparaten (preview-versie)|Count|Average|Aantal apparaten dat is verbonden met uw IoT-hub|Er zijn geen dimensies|
|configuraties|Metrische configuratie gegevens|Count|Totaal|Metrische gegevens voor configuratie bewerkingen|Er zijn geen dimensies|

## <a name="microsoftdevicesprovisioningservices"></a>Microsoft.Devices/provisioningServices

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|RegistrationAttempts|Registratie pogingen|Count|Totaal|Aantal pogingen voor apparaatregistratie|ProvisioningServiceName, IotHubName, Status|
|DeviceAssignments|Apparaten toegewezen|Count|Totaal|Aantal apparaten dat is toegewezen aan een IoT-hub|ProvisioningServiceName, IotHubName|
|AttestationAttempts|Attestation-pogingen|Count|Totaal|Aantal pogingen voor het uitvoeren van een apparaat|ProvisioningServiceName, Status, Protocol|

## <a name="microsoftdocumentdbdatabaseaccounts"></a>Microsoft.DocumentDB/databaseAccounts

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|AvailableStorage|Beschik bare opslag|Bytes|Totaal|Totale beschik bare opslag gerapporteerd bij een granulatie van 5 minuten|Verzamelingnaam, databasenaam, regio|
|CassandraConnectionClosures|Cassandra-verbinding sluiten|Count|Totaal|Aantal Cassandra-verbindingen dat is gesloten, gerapporteerd met een granulatie van 1 minuut|Regio, ClosureReason|
|CassandraRequestCharges|Kosten voor Cassandra-aanvragen|Count|Totaal|RUs gebruikt voor Cassandra-aanvragen|DATABASENAME, verzamel-, regio, OperationType, resource type|
|CassandraRequests|Cassandra aanvragen|Count|Count|Aantal gemaakte Cassandra-aanvragen|DATABASENAME, verzamel-, regio, OperationType, resource type, error code|
|DataUsage|Gegevensgebruik|Bytes|Totaal|Totaal gegevens gebruik gerapporteerd bij 5 minuten granulariteit|Verzamelingnaam, databasenaam, regio|
|DocumentCount|Aantal documenten|Count|Totaal|Totaal aantal documenten gemeld bij 5 minuten granulariteit|Verzamelingnaam, databasenaam, regio|
|DocumentQuota|Document quotum|Bytes|Totaal|Totale opslag quotum gerapporteerd bij een granulatie van 5 minuten|Verzamelingnaam, databasenaam, regio|
|IndexUsage|Index gebruik|Bytes|Totaal|Totaal gebruik van index gerapporteerd bij een granulatie van 5 minuten|Verzamelingnaam, databasenaam, regio|
|MetadataRequests|Meta gegevens aanvragen|Count|Count|Aantal aanvragen voor meta gegevens. Cosmos DB onderhoudt de verzameling van meta gegevens van het systeem voor elk account, waarmee u verzamelingen, data bases, enzovoort en hun configuraties gratis kunt inventariseren.|DATABASENAME, verzamel-, regio, status code, |
|MongoRequestCharge|Kosten voor Mongo-aanvragen|Count|Totaal|Verbruikte Mongo-aanvraag eenheden|DATABASENAME, verzamel-, regio, opdrachtnaam, error code|
|MongoRequests|Mongo aanvragen|Count|Count|Aantal gemaakte Mongo-aanvragen|DATABASENAME, verzamel-, regio, opdrachtnaam, error code|
|ProvisionedThroughput|Ingerichte doorvoer|Count|Maximum|Ingerichte doorvoer|DATABASENAME, verzamelnaam|
|ReplicationLatency|P99-replicatie latentie|MilliSeconds|Average|Replicatie latentie van P99 voor de bron-en doel regio's voor geografisch ingeschakelde account|SourceRegion, TargetRegion|
|ServiceAvailability|Service beschikbaarheid|Percent|Average|Beschik baarheid van account aanvragen op één uur, dag of maand granulatie|Er zijn geen dimensies|
|TotalRequestUnits|Totaal aantal aanvraag eenheden|Count|Totaal|Verbruikte aanvraag eenheden|DATABASENAME, verzamel-, regio, status code, OperationType|
|TotalRequests|Totaal aantal aanvragen|Count|Count|Aantal aanvragen dat is gedaan|DATABASENAME, verzamel-, regio, status code, OperationType|

## <a name="microsofteventgridtopics"></a>Micro soft. EventGrid/topics

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|PublishSuccessCount|Gepubliceerde gebeurtenissen|Count|Totaal|Totaal aantal gebeurtenissen dat naar dit onderwerp is gepubliceerd|Er zijn geen dimensies|
|PublishFailCount|Mislukte gebeurtenissen publiceren|Count|Totaal|Totaal aantal gebeurtenissen dat niet naar dit onderwerp kan worden gepubliceerd|ErrorType, Error|
|UnmatchedEventCount|Niet-overeenkomende gebeurtenissen|Count|Totaal|Totaal aantal gebeurtenissen dat niet overeenkomt met een van de gebeurtenis abonnementen voor dit onderwerp|Er zijn geen dimensies|
|PublishSuccessLatencyInMs|Latentie van publicatie geslaagd|Count|Totaal|Latentie van geslaagde publicatie in milliseconden|Er zijn geen dimensies|

## <a name="microsofteventgrideventsubscriptions"></a>Microsoft.EventGrid/eventSubscriptions

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|MatchedEventCount|Overeenkomende gebeurtenissen|Count|Totaal|Totaal aantal gebeurtenissen dat overeenkomt met dit gebeurtenis abonnement|Er zijn geen dimensies|
|DeliveryAttemptFailCount|Mislukte leverings gebeurtenissen|Count|Totaal|Totaal aantal gebeurtenissen dat niet aan dit gebeurtenis abonnement kan worden geleverd|Error, ErrorType|
|DeliverySuccessCount|Geleverde gebeurtenissen|Count|Totaal|Totaal aantal gebeurtenissen dat aan dit gebeurtenis abonnement is geleverd|Er zijn geen dimensies|
|DestinationProcessingDurationInMs|Doel verwerkings duur|Milliseconden|Average|Doel verwerkings duur in milliseconden|Er zijn geen dimensies|
|DroppedEventCount|Verwijderde gebeurtenissen|Count|Totaal|Totaal aantal verloren gebeurtenissen dat overeenkomt met dit gebeurtenis abonnement|DropReason|
|DeadLetteredCount|Gebeurtenissen met onbestelbare berichten|Count|Totaal|Totaal aantal gebeurtenissen met onbestelbare berichten die overeenkomen met dit gebeurtenis abonnement|DeadLetterReason|

## <a name="microsofteventgridextensiontopics"></a>Microsoft.EventGrid/extensionTopics

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|PublishSuccessCount|Gepubliceerde gebeurtenissen|Count|Totaal|Totaal aantal gebeurtenissen dat naar dit onderwerp is gepubliceerd|Er zijn geen dimensies|
|PublishFailCount|Mislukte gebeurtenissen|Count|Totaal|Totaal aantal gebeurtenissen dat niet naar dit onderwerp kan worden gepubliceerd|ErrorType, Error|
|UnmatchedEventCount|Niet-overeenkomende gebeurtenissen|Count|Totaal|Totaal aantal gebeurtenissen dat niet overeenkomt met een van de gebeurtenis abonnementen voor dit onderwerp|Er zijn geen dimensies|
|PublishSuccessLatencyInMs|Latentie van publicatie geslaagd|Count|Totaal|Latentie van geslaagde publicatie in milliseconden|Er zijn geen dimensies|

## <a name="microsofteventhubnamespaces"></a>Microsoft.EventHub/namespaces

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|SuccessfulRequests|Geslaagde aanvragen|Count|Totaal|Voltooide aanvragen voor Microsoft.EventHub.|EntityName |
|ServerErrors|Serverfouten.|Count|Totaal|Serverfouten voor Microsoft.EventHub.|EntityName |
|UserErrors|Gebruikersfouten.|Count|Totaal|Gebruikersfouten voor Microsoft.EventHub.|EntityName |
|QuotaExceededErrors|Fout met overschreden quotum.|Count|Totaal|Fout met overschreden quotum voor Microsoft.EventHub.|EntityName |
|ThrottledRequests|Beperkte aanvragen.|Count|Totaal|Beperkte aanvragen voor Microsoft.EventHub.|EntityName |
|IncomingRequests|Binnenkomende aanvragen|Count|Totaal|Binnenkomende aanvragen voor Microsoft.EventHub.|EntityName|
|IncomingMessages|Binnenkomende berichten|Count|Totaal|Binnenkomende berichten voor Microsoft.EventHub.|EntityName|
|OutgoingMessages|Uitgaande berichten|Count|Totaal|Uitgaande berichten voor Microsoft.EventHub.|EntityName|
|IncomingBytes|Binnenkomende bytes.|Bytes|Totaal|Binnenkomende bytes voor Microsoft.EventHub.|EntityName|
|OutgoingBytes|Uitgaande bytes.|Bytes|Totaal|Uitgaande bytes voor Microsoft.EventHub.|EntityName|
|ActiveConnections|ActiveConnections|Count|Average|Totaal aantal actieve verbindingen voor Microsoft.EventHub.|Er zijn geen dimensies|
|ConnectionsOpened|Geopende verbindingen.|Count|Average|Geopende verbindingen voor Microsoft.EventHub.|EntityName|
|ConnectionsClosed|Gesloten verbindingen.|Count|Average|Gesloten verbindingen voor Microsoft.EventHub.|EntityName|
|CaptureBacklog|Achterstand vastleggen.|Count|Totaal|Achterstand vastleggen voor Microsoft.EventHub.|EntityName|
|CapturedMessages|Vastgelegde berichten.|Count|Totaal|Vastgelegde berichten voor Microsoft.EventHub.|EntityName|
|CapturedBytes|Vastgelegde bytes.|Bytes|Totaal|Vastgelegde bytes voor Microsoft.EventHub.|EntityName|
|Size|Size|Bytes|Average|Grootte van een EventHub in bytes.|EntityName|
|INREQS|Binnenkomende aanvragen (afgeschaft)|Count|Totaal|Totaal aantal binnenkomende verzend aanvragen voor een naam ruimte (afgeschaft)|Er zijn geen dimensies|
|SUCCREQ|Geslaagde aanvragen (afgeschaft)|Count|Totaal|Totaal aantal geslaagde aanvragen voor een naam ruimte (afgeschaft)|Er zijn geen dimensies|
|FAILREQ|Mislukte aanvragen (afgeschaft)|Count|Totaal|Totaal aantal mislukte aanvragen voor een naam ruimte (afgeschaft)|Er zijn geen dimensies|
|SVRBSY|Fouten bij server bezet (afgeschaft)|Count|Totaal|Totaal aantal fouten bij server bezet voor een naam ruimte (afgeschaft)|Er zijn geen dimensies|
|INTERER|Interne server fouten (afgeschaft)|Count|Totaal|Totaal aantal interne server fouten voor een naam ruimte (afgeschaft)|Er zijn geen dimensies|
|MISCERR|Andere fouten (afgeschaft)|Count|Totaal|Totaal aantal mislukte aanvragen voor een naam ruimte (afgeschaft)|Er zijn geen dimensies|
|INMSGS|Binnenkomende berichten (afgeschaft) (afgeschaft)|Count|Totaal|Totaal aantal inkomende berichten voor een naam ruimte. Deze metriek is afgeschaft. Gebruik in plaats daarvan de metriek van binnenkomende berichten (afgeschaft)|Er zijn geen dimensies|
|EHINMSGS|Binnenkomende berichten (afgeschaft)|Count|Totaal|Totaal aantal inkomende berichten voor een naam ruimte (afgeschaft)|Er zijn geen dimensies|
|OUTMSGS|Uitgaande berichten (afgeschaft) (afgeschaft)|Count|Totaal|Totaal aantal uitgaande berichten voor een naam ruimte. Deze metriek is afgeschaft. Gebruik in plaats daarvan de metrische gegevens van uitgaande berichten (afgeschaft)|Er zijn geen dimensies|
|EHOUTMSGS|Uitgaande berichten (afgeschaft)|Count|Totaal|Totaal aantal uitgaande berichten voor een naam ruimte (afgeschaft)|Er zijn geen dimensies|
|EHINMBS|Binnenkomende bytes (afgeschaft) (afgeschaft)|Bytes|Totaal|Door Voer van binnenkomende berichten van Event hub voor een naam ruimte. Deze metriek is afgeschaft. Gebruik in plaats daarvan de metrische gegevens over binnenkomende bytes (afgeschaft)|Er zijn geen dimensies|
|EHINBYTES|Binnenkomende bytes (afgeschaft)|Bytes|Totaal|Door Voer van inkomende berichten van Event hub voor een naam ruimte (afgeschaft)|Er zijn geen dimensies|
|EHOUTMBS|Uitgaande bytes (afgeschaft) (afgeschaft)|Bytes|Totaal|Door Voer van Event hub voor een naam ruimte. Deze metriek is afgeschaft. Gebruik in plaats hiervan de metrische gegevens van uitgaande bytes (afgeschaft)|Er zijn geen dimensies|
|EHOUTBYTES|Uitgaande bytes (afgeschaft)|Bytes|Totaal|Door Voer van Event hub voor een naam ruimte (afgeschaft)|Er zijn geen dimensies|
|EHABL|Achterstallige berichten archiveren (afgeschaft)|Count|Totaal|Event hub-archief berichten in achterstand voor een naam ruimte (afgeschaft)|Er zijn geen dimensies|
|EHAMSGS|Berichten archiveren (afgeschaft)|Count|Totaal|Event hub heeft berichten gearchiveerd in een naam ruimte (afgeschaft)|Er zijn geen dimensies|
|EHAMBS|Berichten doorvoer archiveren (afgeschaft)|Bytes|Totaal|Door Voer van Event hub-bericht doorvoer in een naam ruimte (afgeschaft)|Er zijn geen dimensies|

## <a name="microsofteventhubclusters"></a>Microsoft.EventHub/clusters

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|SuccessfulRequests|Geslaagde aanvragen (preview-versie)|Count|Totaal|Voltooide aanvragen voor Microsoft.EventHub. (Preview-versie)|Er zijn geen dimensies|
|ServerErrors|Serverfouten. (Preview-versie)|Count|Totaal|Serverfouten voor Microsoft.EventHub. (Preview-versie)|Er zijn geen dimensies|
|UserErrors|Gebruikersfouten. (Preview-versie)|Count|Totaal|Gebruikersfouten voor Microsoft.EventHub. (Preview-versie)|Er zijn geen dimensies|
|QuotaExceededErrors|Fout met overschreden quotum. (Preview-versie)|Count|Totaal|Fout met overschreden quotum voor Microsoft.EventHub. (Preview-versie)|Er zijn geen dimensies|
|ThrottledRequests|Beperkte aanvragen. (Preview-versie)|Count|Totaal|Beperkte aanvragen voor Microsoft.EventHub. (Preview-versie)|Er zijn geen dimensies|
|IncomingRequests|Inkomende aanvragen (preview-versie)|Count|Totaal|Binnenkomende aanvragen voor Microsoft.EventHub. (Preview-versie)|Er zijn geen dimensies|
|IncomingMessages|Inkomende berichten (preview-versie)|Count|Totaal|Binnenkomende berichten voor Microsoft.EventHub. (Preview-versie)|Er zijn geen dimensies|
|OutgoingMessages|Uitgaande berichten (preview-versie)|Count|Totaal|Uitgaande berichten voor Microsoft.EventHub. (Preview-versie)|Er zijn geen dimensies|
|IncomingBytes|Binnenkomende bytes. (Preview-versie)|Bytes|Totaal|Binnenkomende bytes voor Microsoft.EventHub. (Preview-versie)|Er zijn geen dimensies|
|OutgoingBytes|Uitgaande bytes. (Preview-versie)|Bytes|Totaal|Uitgaande bytes voor Microsoft.EventHub. (Preview-versie)|Er zijn geen dimensies|
|ActiveConnections|ActiveConnection (preview-versie)|Count|Average|Totaal aantal actieve verbindingen voor Microsoft.EventHub. (Preview-versie)|Er zijn geen dimensies|
|ConnectionsOpened|Geopende verbindingen. (Preview-versie)|Count|Average|Geopende verbindingen voor Microsoft.EventHub. (Preview-versie)|Er zijn geen dimensies|
|ConnectionsClosed|Gesloten verbindingen. (Preview-versie)|Count|Average|Gesloten verbindingen voor Microsoft.EventHub. (Preview-versie)|Er zijn geen dimensies|
|CaptureBacklog|Achterstand vastleggen. (Preview-versie)|Count|Totaal|Achterstand vastleggen voor Microsoft.EventHub. (Preview-versie)|Er zijn geen dimensies|
|CapturedMessages|Vastgelegde berichten. (Preview-versie)|Count|Totaal|Vastgelegde berichten voor Microsoft.EventHub. (Preview-versie)|Er zijn geen dimensies|
|CapturedBytes|Vastgelegde bytes. (Preview-versie)|Bytes|Totaal|Vastgelegde bytes voor Microsoft.EventHub. (Preview-versie)|Er zijn geen dimensies|
|CPU|CPU (preview-versie)|Percent|Maximum|CPU-gebruik voor het Event Hub-cluster als percentage|Role|
|AvailableMemory|Beschikbaar geheugen (preview-versie)|Count|Maximum|Beschikbaar geheugen voor het Event Hub-cluster in bytes|Role|

## <a name="microsofthdinsightclusters"></a>Microsoft.HDInsight/clusters

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|GatewayRequests|Gateway aanvragen|Count|Totaal|Aantal gateway-aanvragen|ClusterDnsName, HTTP status|
|CategorizedGatewayRequests|Gecategoriseerde gateway aanvragen|Count|Totaal|Aantal gateway aanvragen per categorie (1xx/2xx/3xx/4xx/5xx)|ClusterDnsName, HTTP status|
|NumActiveWorkers|Aantal actieve werk rollen|Count|Maximum|Aantal actieve werk rollen|ClusterDnsName, MetricName|

## <a name="microsoftinsightsautoscalesettings"></a>Microsoft.Insights/AutoscaleSettings

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|ObservedMetricValue|Waargenomen metrische waarde|Count|Average|De waarde die is berekend door automatische schaalaanpassing wanneer deze is uitgevoerd|MetricTriggerSource|
|MetricThreshold|Drempel voor een metrische waarde|Count|Average|De geconfigureerde drempelwaarde voor automatische schaalaanpassing wanneer automatische schaalaanpassing is uitgevoerd.|MetricTriggerRule|
|ObservedCapacity|Waargenomen capaciteit|Count|Average|De capaciteit die is gerapporteerd aan automatische schaalaanpassing wanneer dit is uitgevoerd.|Er zijn geen dimensies|
|ScaleActionsInitiated|Schaalacties geactiveerd|Count|Totaal|De richting van de schaalbewerking.|ScaleDirection|

## <a name="microsoftinsightscomponents"></a>Microsoft.Insights/Components

(Open bare preview)

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|availabilityResults/availabilityPercentage|Beschikbaarheid|Procent|Average|Percentage van voltooide beschikbaarheidstesten|availabilityResult/naam, availabilityResult/locatie|
|availabilityResults/aantal|Beschikbaarheidstesten|Count|Count|Aantal beschikbaarheidstesten|availabilityResult/naam, availabilityResult/locatie, availabilityResult/geslaagd|
|availabilityResults/duur|Duur beschikbaarheidstest|MilliSeconds|Average|Duur beschikbaarheidstest|availabilityResult/naam, availabilityResult/locatie, availabilityResult/geslaagd|
|browserTimings/networkDuration|Netwerkverbindingstijd voor laden van pagina|MilliSeconds|Average|Tijd tussen de gebruikers aanvraag en de netwerk verbinding. Inclusief DNS-Zoek-en transport verbinding.|Er zijn geen dimensies|
|browserTimings/processingDuration|Verwerkingstijd client|MilliSeconds|Average|Tijd tussen het ontvangen van de laatste byte van een document totdat de DOM is geladen. Asynchrone aanvragen kunnen nog steeds worden verwerkt.|Er zijn geen dimensies|
|browserTimings/receiveDuration|Reactietijd voor ontvangen|MilliSeconds|Average|Tijd tussen de eerste en de laatste byte of tot verbreking van de verbinding.|Er zijn geen dimensies|
|browserTimings/sendDuration|Verzoektijd voor verzenden|MilliSeconds|Average|Tijd tussen netwerkverbinding en ontvangst van eerste byte.|Er zijn geen dimensies|
|browserTimings/totalDuration|Laadtijd van browserpagina|MilliSeconds|Average|Tijd vanaf gebruikersaanvraag totdat DOM, opmaakmodellen, scripts en afbeeldingen zijn geladen.|Er zijn geen dimensies|
|afhankelijkheden/aantal|Afhankelijkheidsaanroepen|Count|Count|Aantal aanroepen van de toepassing naar externe bronnen.|afhankelijkheid/type, afhankelijkheid/requests, afhankelijkheid/geslaagd, bewerking/synthetisch, Cloud/roleInstance, Cloud/rolnaam|
|afhankelijkheden/duur|Duur van afhankelijkheid|MilliSeconds|Average|Duur van de aanroepen van de toepassing naar externe bronnen.|afhankelijkheid/type, afhankelijkheid/requests, afhankelijkheid/geslaagd, bewerking/synthetisch, Cloud/roleInstance, Cloud/rolnaam|
|afhankelijkheden/mislukt|Mislukte afhankelijkheidsaanroepen|Count|Count|Aantal mislukte afhankelijkheidsaanroepen van de toepassing naar externe resources.|afhankelijkheid/type, afhankelijkheid/requests, bewerking/synthetisch, Cloud/roleInstance, Cloud/rolnaam|
|Page views/aantal|Paginaweergaven|Count|Count|Aantal paginaweergaven.|bewerking/synthetisch|
|Page views/duur|Laadtijd voor paginaweergave|MilliSeconds|Average|Laadtijd voor paginaweergave|bewerking/synthetisch|
|performanceCounters/requestExecutionTime|Uitvoeringstijd van de HTTP-aanvraag|MilliSeconds|Average|Uitvoeringstijd van de meest recente aanvraag.|Cloud-roleInstance|
|Performance Counters/requestsInQueue|HTTP-aanvragen in de toepassingswachtrij|Count|Average|Lengte van de rij met toepassingsaanvragen.|Cloud-roleInstance|
|performanceCounters/requestsPerSecond|HTTP-aanvraagsnelheid|CountPerSecond|Average|Snelheid per seconde van alle aanvragen aan de toepassing vanaf ASP.NET.|Cloud-roleInstance|
|performanceCounters/exceptionsPerSecond|Aantal uitzonderingen|CountPerSecond|Average|Het aantal verwerkte en onverwerkte uitzonderingen dat is gerapporteerd aan Windows, inclusief .NET-uitzonderingen en onbeheerde uitzonderingen die zijn geconverteerd naar .NET-uitzonderingen.|Cloud-roleInstance|
|performanceCounters/processIOBytesPerSecond|I/O-snelheid voor proces|BytesPerSecond|Average|Totaal aantal per seconde gelezen en naar bestanden, netwerk en apparaten geschreven bytes.|Cloud-roleInstance|
|performanceCounters/processCpuPercentage|CPU voor proces|Percent|Average|Het percentage van de verstreken tijd dat alle proces threads de processor hebben gebruikt om instructies uit te voeren. Dit kan variëren tussen 0 en 100. Deze metrische gegevens duiden de prestaties van alleen het W3wp-proces aan.|Cloud-roleInstance|
|performanceCounters/processorCpuPercentage|Processortijd|Percent|Average|Het tijdspercentage dat de processor spendeert aan niet-inactieve threads.|Cloud-roleInstance|
|performanceCounters/memoryAvailableBytes|Beschikbaar geheugen|Bytes|Average|Fysiek geheugen dat direct beschikbaar is voor toewijzing aan een proces of voor systeemgebruik.|Cloud-roleInstance|
|performanceCounters/processPrivateBytes|Proceseigen bytes|Bytes|Average|Geheugen exclusief toegewezen aan de processen van de gecontroleerde toepassing.|Cloud-roleInstance|
|aanvragen/duur|Serverreactietijd|MilliSeconds|Average|Tijd tussen de ontvangst van een HTTP-aanvraag en het voltooien van het verzenden van de reactie.|aanvraag-requests, aanvraag-resultCode, bewerking/synthetisch, Cloud/roleInstance, aanvraag/geslaagd, Cloud/rolnaam|
|aanvragen/aantal|Serveraanvragen|Count|Count|Aantal voltooide HTTP-aanvragen.|aanvraag-requests, aanvraag-resultCode, bewerking/synthetisch, Cloud/roleInstance, aanvraag/geslaagd, Cloud/rolnaam|
|aanvragen/mislukt|Mislukte aanvragen|Count|Count|Het aantal HTTP-aanvragen dat is gemarkeerd als mislukt. In de meeste gevallen zijn dit aanvragen met een antwoord code > = 400 en niet gelijk is aan 401.|aanvraag-requests, aanvraag-resultCode, bewerking/synthetisch, Cloud/roleInstance, Cloud/rolnaam|
|aanvragen/frequentie|Aantal server aanvragen|CountPerSecond|Average|Aantal server aanvragen per seconde|aanvraag-requests, aanvraag-resultCode, bewerking/synthetisch, Cloud/roleInstance, aanvraag/geslaagd, Cloud/rolnaam|
|uitzonde ringen/aantal|Uitzonderingen|Count|Count|Gecombineerd aantal niet-onderschepte uitzonderingen.|Cloud/rolnaam, Cloud/roleInstance, client/type|
|uitzonde ringen/browser|Browseruitzonderingen|Count|Count|Aantal niet-onderschepte uitzonderingen in de browser.|Er zijn geen dimensies|
|uitzonde ringen/server|Serveruitzonderingen|Count|Count|Aantal niet-onderschepte uitzonderingen in de servertoepassing.|Cloud/rolnaam, Cloud-roleInstance|
|traceringen/aantal|Traces|Count|Count|Aantal traceerdocumenten|Trace/severityLevel, Operation/synthetisch, Cloud/rolnaam, Cloud/roleInstance|

## <a name="microsoftkeyvaultvaults"></a>Microsoft.KeyVault/vaults

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|ServiceApiHit|Totaalaantal treffers service-API|Count|Count|Totaalaantal treffers voor service-API|ActivityType, ActivityName|
|ServiceApiLatency|Algemene latentie service-API|Milliseconden|Average|Algemene latentie van service-API-aanvragen|Activity type, Activiteitsnummer, status code|
|ServiceApiResult|Totaalaantal resultaten service-API|Count|Count|Totaalaantal resultaten voor service-API|Activity type, Activiteitsnummer, status code|

## <a name="microsoftkustoclusters"></a>Microsoft.Kusto/Clusters

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|CacheUtilization|Cache gebruik|Percent|Average|Gebruiks niveau in het cluster bereik|Geen|
|QueryDuration|Query duur|Milliseconden|Average|De duur van query's in seconden|Querystatus|
|IngestionUtilization|Opname gebruik|Percent|Average|Verhouding van gebruikte opname sleuven in het cluster|Geen|
|KeepAlive|Actief houden|Count|Average|Sanity-controle geeft aan dat het cluster reageert op query's|Geen|
|IngestionVolumeInMB|Opname volume (in MB)|Count|Totaal|Totaal volume van opgenomen gegevens aan het cluster (in MB)|Database|
|IngestionLatencyInSeconds|Opname latentie (in seconden)|Seconden|Average|Opname tijd van de bron (bijvoorbeeld bericht is in EventHub) naar het cluster in enkele seconden|Geen|
|EventProcessedForEventHubs|Verwerkte gebeurtenissen (voor Event Hubs)|Count|Totaal|Aantal gebeurtenissen dat door het cluster wordt verwerkt bij het opnemen vanuit Event hub|Geen|
|IngestionResult|Opname resultaat|Count|Count|Aantal opname bewerkingen|Status|
|CPU|CPU|Percent|Average|Niveau CPU-gebruik|Geen|
| ContinuousExportNumOfRecordsExported | Het aantal records dat wordt geëxporteerd in continue export | Count | Totaal | Het aantal records dat wordt geëxporteerd voor elk opslag artefact dat is geschreven tijdens de export bewerking  | Geen |
| ExportUtilization | Gebruik exporteren | Percent | Maximum | Gebruik exporteren | Geen |
| ContinuousExportPendingCount | Aantal doorlopend exporteren in behandeling | Count | Maximum | Het aantal in behandeling zijnde doorlopende export taken dat gereed is voor uitvoering | Geen |
| ContinuousExportMaxLatenessMinutes | Maximale achterstand voor continue export | Count | Maximum | De maximale tijd in minuten van alle doorlopende uitvoer die in behandeling zijn en gereed zijn voor uitvoering | Geen |

## <a name="microsoftlocationbasedservicesaccounts"></a>Microsoft.LocationBasedServices/accounts

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|Gebruik|Gebruik|Count|Count|Aantal API-aanroepen|ApiCategory, ApiName|

## <a name="microsoftlogicworkflows"></a>Microsoft.Logic/workflows

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|RunsStarted|Gestarte uitvoeringen|Count|Totaal|Aantal gestarte werkstroomuitvoeringen.|Er zijn geen dimensies|
|RunsCompleted|Voltooide uitvoeringen|Count|Totaal|Aantal voltooide werkstroomuitvoeringen.|Er zijn geen dimensies|
|RunsSucceeded|Geslaagde uitvoeringen|Count|Totaal|Aantal geslaagde werkstroomuitvoeringen.|Er zijn geen dimensies|
|RunsFailed|Mislukte uitvoeringen|Count|Totaal|Aantal mislukte werkstroomuitvoeringen.|Er zijn geen dimensies|
|RunsCancelled|Uitvoeringen geannuleerd|Count|Totaal|Aantal uitgevoerde werk stroom uitvoeringen geannuleerd.|Er zijn geen dimensies|
|RunLatency|Latentie van uitvoeringen|Seconden|Average|Latentie van voltooide werkstroomuitvoeringen.|Er zijn geen dimensies|
|RunSuccessLatency|Latentie van geslaagde uitvoeringen|Seconden|Average|Latentie van geslaagde werkstroomuitvoeringen.|Er zijn geen dimensies|
|RunThrottledEvents|Door uitvoering vertraagde gebeurtenissen|Count|Totaal|Aantal door werkstroomactie of trigger vertraagde gebeurtenissen.|Er zijn geen dimensies|
|RunFailurePercentage|Percentage mislukte uitvoeringen|Percent|Totaal|Percentage mislukte werkstroomuitvoeringen.|Er zijn geen dimensies|
|ActionsStarted|Gestarte acties |Count|Totaal|Aantal gestarte werkstroomacties.|Er zijn geen dimensies|
|ActionsCompleted|Voltooide acties |Count|Totaal|Aantal voltooide werkstroomacties.|Er zijn geen dimensies|
|ActionsSucceeded|Geslaagde acties |Count|Totaal|Aantal geslaagde werkstroomacties.|Er zijn geen dimensies|
|ActionsFailed|Mislukte acties|Count|Totaal|Aantal mislukte werkstroomacties.|Er zijn geen dimensies|
|ActionsSkipped|Overgeslagen acties |Count|Totaal|Aantal overgeslagen werkstroomacties.|Er zijn geen dimensies|
|ActionLatency|Actielatentie |Seconden|Average|Latentie van voltooide werkstroomacties.|Er zijn geen dimensies|
|ActionSuccessLatency|Latentie geslaagde acties |Seconden|Average|Latentie van geslaagde werkstroomacties.|Er zijn geen dimensies|
|ActionThrottledEvents|Door actie vertraagde gebeurtenissen|Count|Totaal|Aantal door werkstroomactie vertraagde gebeurtenissen.|Er zijn geen dimensies|
|TriggersStarted|Gestarte triggers |Count|Totaal|Aantal gestarte werkstroomtriggers.|Er zijn geen dimensies|
|TriggersCompleted|Voltooide triggers |Count|Totaal|Aantal voltooide werkstroomtriggers.|Er zijn geen dimensies|
|TriggersSucceeded|Geslaagde triggers |Count|Totaal|Aantal geslaagde werkstroomtriggers.|Er zijn geen dimensies|
|TriggersFailed|Mislukte triggers |Count|Totaal|Aantal mislukte werkstroomtriggers.|Er zijn geen dimensies|
|TriggersSkipped|Overgeslagen triggers|Count|Totaal|Aantal overgeslagen werkstroomtriggers.|Er zijn geen dimensies|
|TriggersFired|Geactiveerde triggers |Count|Totaal|Aantal geactiveerde werkstroomtriggers.|Er zijn geen dimensies|
|TriggerLatency|Latentie van trigger |Seconden|Average|Latentie van voltooide werkstroomtriggers.|Er zijn geen dimensies|
|TriggerFireLatency|Latentie van triggeractivering |Seconden|Average|Latentie van geactiveerde werkstroomtriggers.|Er zijn geen dimensies|
|TriggerSuccessLatency|Latentie van geslaagde triggers |Seconden|Average|Latentie van geslaagde werkstroomtriggers.|Er zijn geen dimensies|
|TriggerThrottledEvents|Door trigger vertraagde gebeurtenissen|Count|Totaal|Aantal door werkstroomtrigger vertraagde gebeurtenissen.|Er zijn geen dimensies|
|BillableActionExecutions|Factureerbare uitvoeringen van acties|Count|Totaal|Aantal uitvoeringen van werkstroomacties dat wordt gefactureerd.|Er zijn geen dimensies|
|BillableTriggerExecutions|Factureerbare uitvoeringen van activeringen|Count|Totaal|Aantal uitvoeringen van werkstroomactiveringen dat wordt gefactureerd.|Er zijn geen dimensies|
|TotalBillableExecutions|Totaal aantal factureerbare uitvoeringen|Count|Totaal|Aantal werkstroomuitvoeringen dat wordt gefactureerd.|Er zijn geen dimensies|
|BillingUsageNativeOperation|Facturering van gebruik voor systeemeigen uitvoerbewerkingen|Count|Totaal|Aantal systeemeigen uitvoerbewerkingen dat wordt gefactureerd.|Er zijn geen dimensies|
|BillingUsageStandardConnector|Facturering van gebruik voor uitvoeringen voor Standard-connector|Count|Totaal|Aantal uitvoeringen van de standaardconnector dat wordt gefactureerd.|Er zijn geen dimensies|
|BillingUsageStorageConsumption|Facturering van gebruik voor uitvoeringen van opslagverbruik|Count|Totaal|Aantal uitvoeringen van het opslagverbruik dat wordt gefactureerd.|Er zijn geen dimensies|
|BillingUsageNativeOperation|Facturering van gebruik voor systeemeigen uitvoerbewerkingen|Count|Totaal|Aantal systeemeigen uitvoerbewerkingen dat wordt gefactureerd.|Er zijn geen dimensies|
|BillingUsageStandardConnector|Facturering van gebruik voor uitvoeringen voor Standard-connector|Count|Totaal|Aantal uitvoeringen van de standaardconnector dat wordt gefactureerd.|Er zijn geen dimensies|
|BillingUsageStorageConsumption|Facturering van gebruik voor uitvoeringen van opslagverbruik|Count|Totaal|Aantal uitvoeringen van het opslagverbruik dat wordt gefactureerd.|Er zijn geen dimensies|

## <a name="microsoftlogicintegrationserviceenvironments"></a>Microsoft.Logic/integrationServiceEnvironments

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|RunsStarted|Gestarte uitvoeringen|Count|Totaal|Aantal gestarte werkstroomuitvoeringen.|Er zijn geen dimensies|
|RunsCompleted|Voltooide uitvoeringen|Count|Totaal|Aantal voltooide werkstroomuitvoeringen.|Er zijn geen dimensies|
|RunsSucceeded|Geslaagde uitvoeringen|Count|Totaal|Aantal geslaagde werkstroomuitvoeringen.|Er zijn geen dimensies|
|RunsFailed|Mislukte uitvoeringen|Count|Totaal|Aantal mislukte werkstroomuitvoeringen.|Er zijn geen dimensies|
|RunsCancelled|Uitvoeringen geannuleerd|Count|Totaal|Aantal uitgevoerde werk stroom uitvoeringen geannuleerd.|Er zijn geen dimensies|
|RunLatency|Latentie van uitvoeringen|Seconden|Average|Latentie van voltooide werkstroomuitvoeringen.|Er zijn geen dimensies|
|RunSuccessLatency|Latentie van geslaagde uitvoeringen|Seconden|Average|Latentie van geslaagde werkstroomuitvoeringen.|Er zijn geen dimensies|
|RunThrottledEvents|Door uitvoering vertraagde gebeurtenissen|Count|Totaal|Aantal door werkstroomactie of trigger vertraagde gebeurtenissen.|Er zijn geen dimensies|
|RunStartThrottledEvents|Vertraagde gebeurtenissen uitvoeren|Count|Totaal|Aantal uitgevoerde vertraagde gebeurtenissen voor de werk stroom.|Er zijn geen dimensies|
|RunFailurePercentage|Percentage mislukte uitvoeringen|Percent|Totaal|Percentage mislukte werkstroomuitvoeringen.|Er zijn geen dimensies|
|ActionsStarted|Gestarte acties |Count|Totaal|Aantal gestarte werkstroomacties.|Er zijn geen dimensies|
|ActionsCompleted|Voltooide acties |Count|Totaal|Aantal voltooide werkstroomacties.|Er zijn geen dimensies|
|ActionsSucceeded|Geslaagde acties |Count|Totaal|Aantal geslaagde werkstroomacties.|Er zijn geen dimensies|
|ActionsFailed|Mislukte acties |Count|Totaal|Aantal mislukte werkstroomacties.|Er zijn geen dimensies|
|ActionsSkipped|Overgeslagen acties |Count|Totaal|Aantal overgeslagen werkstroomacties.|Er zijn geen dimensies|
|ActionLatency|Actielatentie |Seconden|Average|Latentie van voltooide werkstroomacties.|Er zijn geen dimensies|
|ActionSuccessLatency|Latentie geslaagde acties |Seconden|Average|Latentie van geslaagde werkstroomacties.|Er zijn geen dimensies|
|ActionThrottledEvents|Door actie vertraagde gebeurtenissen|Count|Totaal|Aantal door werkstroomactie vertraagde gebeurtenissen.|Er zijn geen dimensies|
|TriggersStarted|Gestarte triggers |Count|Totaal|Aantal gestarte werkstroomtriggers.|Er zijn geen dimensies|
|TriggersCompleted|Voltooide triggers |Count|Totaal|Aantal voltooide werkstroomtriggers.|Er zijn geen dimensies|
|TriggersSucceeded|Geslaagde triggers |Count|Totaal|Aantal geslaagde werkstroomtriggers.|Er zijn geen dimensies|
|TriggersFailed|Mislukte triggers |Count|Totaal|Aantal mislukte werkstroomtriggers.|Er zijn geen dimensies|
|TriggersSkipped|Overgeslagen triggers|Count|Totaal|Aantal overgeslagen werkstroomtriggers.|Er zijn geen dimensies|
|TriggersFired|Geactiveerde triggers |Count|Totaal|Aantal geactiveerde werkstroomtriggers.|Er zijn geen dimensies|
|TriggerLatency|Latentie van trigger |Seconden|Average|Latentie van voltooide werkstroomtriggers.|Er zijn geen dimensies|
|TriggerFireLatency|Latentie van triggeractivering |Seconden|Average|Latentie van geactiveerde werkstroomtriggers.|Er zijn geen dimensies|
|TriggerSuccessLatency|Latentie van geslaagde triggers |Seconden|Average|Latentie van geslaagde werkstroomtriggers.|Er zijn geen dimensies|
|TriggerThrottledEvents|Door trigger vertraagde gebeurtenissen|Count|Totaal|Aantal door werkstroomtrigger vertraagde gebeurtenissen.|Er zijn geen dimensies|
|IntegrationServiceEnvironmentWorkflowProcessorUsage|Het werkstroomprocessorgebruik voor de integratieserviceomgeving|Percent|Average|Het werkstroomprocessorgebruik voor de integratieserviceomgeving.|Er zijn geen dimensies|
|IntegrationServiceEnvironmentWorkflowMemoryUsage|Het werkstroomgeheugengebruik voor de integratieserviceomgeving|Percent|Average|Het werkstroomgeheugengebruik voor de integratieserviceomgeving.|Er zijn geen dimensies|
|IntegrationServiceEnvironmentConnectorProcessorUsage|Het connectorprocessorgebruik voor de integratieserviceomgeving|Percent|Average|Het connectorprocessorgebruik voor de integratieserviceomgeving.|Er zijn geen dimensies|
|IntegrationServiceEnvironmentConnectorMemoryUsage|Het connectorgeheugengebruik voor de integratieserviceomgeving|Percent|Average|Het connectorgeheugengebruik voor de integratieserviceomgeving.|Er zijn geen dimensies|

## <a name="microsoftmachinelearningservicesworkspaces"></a>Microsoft.MachineLearningServices/workspaces

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|Voltooide uitvoeringen|Voltooide uitvoeringen|Count|Totaal|Het aantal uitvoeringen dat is voltooid voor deze werk ruimte|Scenario|
|Gestart uitvoeringen|Gestart uitvoeringen|Count|Totaal|Aantal uitvoeringen gestart voor deze werk ruimte|Scenario|
|Mislukte verwerkingen|Mislukte verwerkingen|Count|Totaal|Aantal uitvoeringen is mislukt voor deze werk ruimte|Scenario|

## <a name="microsoftmapsaccounts"></a>Microsoft.Maps/accounts

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|Gebruik|Gebruik|Count|Count|Aantal API-aanroepen|ApiCategory, ApiName, ResultType, ResponseCode|
|Beschikbaarheid|Beschikbaarheid|Procent|Average|Beschik baarheid van de Api's|ApiCategory, ApiName|

## <a name="microsoftnetappnetappaccountscapacitypoolsvolumes"></a>Microsoft.NetApp/netAppAccounts/capacityPools/Volumes

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|AverageOtherLatency|Gemiddelde andere latentie|ms/op|Average|De gemiddelde andere latentie (die niet lezen of schrijven is) in milliseconden per bewerking|Er zijn geen dimensies|
|AverageReadLatency|Gemiddelde lees latentie|ms/op|Average|Gemiddelde lees latentie in milliseconden per bewerking|Er zijn geen dimensies|
|AverageTotalLatency|Gemiddelde latentie in milliseconden|ms/op|Average|Gemiddelde totale latentie in milliseconden per bewerking|Er zijn geen dimensies|
|AverageWriteLatency|Gemiddelde schrijf latentie|ms/op|Average|Gemiddelde schrijf latentie in milliseconden per bewerking|Er zijn geen dimensies|
|FilesystemOtherOps|Andere OPS-bestands systeem|OPS|Average|Aantal andere bewerkingen van het bestands systeem (dat is niet lezen of schrijven)|Er zijn geen dimensies|
|FilesystemReadOps|OPS-lezen van bestands systeem|OPS|Average|Aantal lees bewerkingen van het bestands systeem|Er zijn geen dimensies|
|FilesystemTotalOps|Totaal aantal OPS-bestands systeem|OPS|Average|Som van alle bestandssysteem bewerkingen|Er zijn geen dimensies|
|FilesystemWriteOps|File System write OPS|OPS|Average|Aantal schrijf bewerkingen van het bestands systeem|Er zijn geen dimensies|
|IoBytesPerOtherOps|I/o-bytes per andere OPS|bytes/op|Average|Aantal in-en uitgaande bytes per andere bewerking (dat is niet lezen of schrijven)|Er zijn geen dimensies|
|IoBytesPerReadOps|I/o bytes per Read OPS|bytes/op|Average|Aantal bytes in/uit per Lees bewerking|Er zijn geen dimensies|
|IoBytesPerTotalOps|I/o-bytes per op alle bewerkingen|bytes/op|Average|Som van alle bewerkingen in/uit bytes|Er zijn geen dimensies|
|IoBytesPerWriteOps|I/o-bytes per write OPS|bytes/op|Average|Aantal in/uit bytes per schrijf bewerking|Er zijn geen dimensies|
|OtherIops|Andere IOPS|bewerkingen per seconde|Average|Andere in/uit-bewerking per seconde|Er zijn geen dimensies|
|OtherThroughput|Andere door Voer|MB|Average|Andere door Voer (dat is niet lezen of schrijven) in mega bytes per seconde|Er zijn geen dimensies|
|ReadIops|IOPS lezen|bewerkingen per seconde|Average|In-en uitvoer bewerkingen per seconde|Er zijn geen dimensies|
|ReadThroughput|Lees doorvoer|MB|Average|Lees doorvoer in mega bytes per seconde|Er zijn geen dimensies|
|TotalIops|Totaal aantal IOPS|bewerkingen per seconde|Average|Som van alle in-en uitvoer bewerkingen per seconde|Er zijn geen dimensies|
|TotalThroughput|Totale doorvoer|MB|Average|Som van alle door Voer in mega bytes per seconde|Er zijn geen dimensies|
|VolumeAllocatedSize|Grootte van toegewezen volume|bytes|Average|Toegewezen grootte van het volume (niet de werkelijk gebruikte bytes)|Er zijn geen dimensies|
|VolumeLogicalSize|Logische volume grootte|bytes|Average|Logische grootte van het volume (gebruikte bytes)|Er zijn geen dimensies|
|VolumeSnapshotSize|Grootte van moment opname van volume|bytes|Average|Grootte van alle moment opnamen in volume|Er zijn geen dimensies|
|WriteIops|IOPS schrijven|bewerkingen per seconde|Average|In-en uitvoer bewerkingen per seconde|Er zijn geen dimensies|
|WriteThroughput|Schrijf doorvoer|MB|Average|Schrijf doorvoer in mega bytes per seconde|Er zijn geen dimensies|

## <a name="microsoftnetappnetappaccountscapacitypools"></a>Microsoft.NetApp/netAppAccounts/capacityPools

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|VolumePoolAllocatedSize|Grootte toegewezen volume groep|bytes|Average|Toegewezen grootte van de pool (niet de werkelijk gebruikte bytes)|Er zijn geen dimensies|
|VolumePoolAllocatedUsed|Gebruikte volume groep|bytes|Average|Gebruikte grootte van de pool is toegewezen|Er zijn geen dimensies|
|VolumePoolTotalLogicalSize|Totale logische grootte van volume groep|bytes|Average|Som van de logische grootte van alle volumes die deel uitmaken van de groep|Er zijn geen dimensies|
|VolumePoolTotalSnapshotSize|Grootte van de moment opname van de volume groep|bytes|Average|Som van alle moment opnamen in de pool|Er zijn geen dimensies|

## <a name="microsoftnetworknetworkinterfaces"></a>Microsoft.Network/networkInterfaces

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|BytesSentRate|Verzonden bytes|Count|Totaal|Het aantal bytes dat de netwerk interface heeft verzonden|Er zijn geen dimensies|
|BytesReceivedRate|Ontvangen bytes|Count|Totaal|Het aantal bytes dat de netwerk interface heeft ontvangen|Er zijn geen dimensies|
|PacketsSentRate|Verzonden pakketten|Count|Totaal|Aantal pakketten dat de netwerk interface heeft verzonden|Er zijn geen dimensies|
|PacketsReceivedRate|Ontvangen pakketten|Count|Totaal|Aantal pakketten dat de netwerk interface heeft ontvangen|Er zijn geen dimensies|

## <a name="microsoftnetworkloadbalancers"></a>Microsoft.Network/loadBalancers

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|VipAvailability|Beschik baarheid gegevenspad|Count|Average|Gemiddelde Beschik baarheid van gegevens paden per tijds duur van Load Balancer|FrontendIPAddress, FrontendPort|
|DipAvailability|Status van Health probe|Count|Average|Gemiddelde status van Load Balancer Health probe per tijds duur|ProtocolType, BackendPort, FrontendIPAddress, FrontendPort, BackendIPAddress|
|ByteCount|Aantal bytes|Count|Totaal|Totaal aantal verzonden bytes binnen tijds periode|FrontendIPAddress, FrontendPort, direction|
|PacketCount|Aantal pakketten|Count|Totaal|Totaal aantal verzonden pakketten binnen tijds periode|FrontendIPAddress, FrontendPort, direction|
|SYNCount|SYN-aantal|Count|Totaal|Totaal aantal SYN-pakketten verzonden binnen tijds periode|FrontendIPAddress, FrontendPort, direction|
|SnatConnectionCount|Aantal SNAT-verbindingen|Count|Totaal|Totaal aantal nieuwe SNAT-verbindingen dat binnen een tijds periode is gemaakt|FrontendIPAddress, BackendIPAddress, ConnectionState|
|AllocatedSnatPorts|Toegewezen SNAT-poorten (preview-versie)|Count|Totaal|Totaal aantal toegewezen SNAT-poorten binnen tijds periode|FrontendIPAddress, BackendIPAddress, ProtocolType|
|UsedSnatPorts|Gebruikte SNAT-poorten (preview-versie)|Count|Totaal|Totaal aantal SNAT-poorten gebruikt binnen tijds periode|FrontendIPAddress, BackendIPAddress, ProtocolType|

## <a name="microsoftnetworkdnszones"></a>Microsoft.Network/dnszones

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|QueryVolume|Query volume|Count|Totaal|Aantal verzonden query's voor een DNS-zone|Er zijn geen dimensies|
|RecordSetCount|Aantal record sets|Count|Maximum|Aantal record sets in een DNS-zone|Er zijn geen dimensies|
|RecordSetCapacityUtilization|Capaciteits gebruik van record sets|Percent|Maximum|Percentage van de set-capaciteit van de record die wordt gebruikt door een DNS-zone|Er zijn geen dimensies|

## <a name="microsoftnetworkpublicipaddresses"></a>Microsoft.Network/publicIPAddresses

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|PacketsInDDoS|DDoS inkomende pakketten|CountPerSecond|Maximum|DDoS inkomende pakketten|Er zijn geen dimensies|
|PacketsDroppedDDoS|DDoS inkomende pakketten verwijderd|CountPerSecond|Maximum|DDoS inkomende pakketten verwijderd|Er zijn geen dimensies|
|PacketsForwardedDDoS|DDoS inkomende pakketten doorgestuurd|CountPerSecond|Maximum|DDoS inkomende pakketten doorgestuurd|Er zijn geen dimensies|
|TCPPacketsInDDoS|Binnenkomende TCP-pakketten DDoS|CountPerSecond|Maximum|Binnenkomende TCP-pakketten DDoS|Er zijn geen dimensies|
|TCPPacketsDroppedDDoS|DDoS binnenkomende TCP-pakketten|CountPerSecond|Maximum|DDoS binnenkomende TCP-pakketten|Er zijn geen dimensies|
|TCPPacketsForwardedDDoS|Doorgestuurde binnenkomende TCP-pakketten DDoS|CountPerSecond|Maximum|Doorgestuurde binnenkomende TCP-pakketten DDoS|Er zijn geen dimensies|
|UDPPacketsInDDoS|Binnenkomende UDP-pakketten DDoS|CountPerSecond|Maximum|Binnenkomende UDP-pakketten DDoS|Er zijn geen dimensies|
|UDPPacketsDroppedDDoS|Verwijderde binnenkomende UDP-pakketten DDoS|CountPerSecond|Maximum|Verwijderde binnenkomende UDP-pakketten DDoS|Er zijn geen dimensies|
|UDPPacketsForwardedDDoS|Door inkomende UDP-pakketten DDoS doorgestuurd|CountPerSecond|Maximum|Door inkomende UDP-pakketten DDoS doorgestuurd|Er zijn geen dimensies|
|BytesInDDoS|Binnenkomende bytes DDoS|BytesPerSecond|Maximum|Binnenkomende bytes DDoS|Er zijn geen dimensies|
|BytesDroppedDDoS|Binnenkomende bytes verloren DDoS|BytesPerSecond|Maximum|Binnenkomende bytes verloren DDoS|Er zijn geen dimensies|
|BytesForwardedDDoS|Doorgestuurde binnenkomende bytes DDoS|BytesPerSecond|Maximum|Doorgestuurde binnenkomende bytes DDoS|Er zijn geen dimensies|
|TCPBytesInDDoS|DDoS binnenkomende TCP-bytes|BytesPerSecond|Maximum|DDoS binnenkomende TCP-bytes|Er zijn geen dimensies|
|TCPBytesDroppedDDoS|DDoS binnenkomende TCP-bytes|BytesPerSecond|Maximum|DDoS binnenkomende TCP-bytes|Er zijn geen dimensies|
|TCPBytesForwardedDDoS|DDoS doorgestuurde binnenkomende TCP-bytes|BytesPerSecond|Maximum|DDoS doorgestuurde binnenkomende TCP-bytes|Er zijn geen dimensies|
|UDPBytesInDDoS|Binnenkomende UDP-bytes DDoS|BytesPerSecond|Maximum|Binnenkomende UDP-bytes DDoS|Er zijn geen dimensies|
|UDPBytesDroppedDDoS|Binnenkomend UDP-bytes verloren DDoS|BytesPerSecond|Maximum|Binnenkomend UDP-bytes verloren DDoS|Er zijn geen dimensies|
|UDPBytesForwardedDDoS|Doorgestuurde binnenkomende UDP-bytes DDoS|BytesPerSecond|Maximum|Doorgestuurde binnenkomende UDP-bytes DDoS|Er zijn geen dimensies|
|IfUnderDDoSAttack|Onder DDoS-aanval of niet|Count|Maximum|Onder DDoS-aanval of niet|Er zijn geen dimensies|
|DDoSTriggerTCPPackets|Binnenkomende TCP-pakketten om DDoS-beperking te activeren|CountPerSecond|Maximum|Binnenkomende TCP-pakketten om DDoS-beperking te activeren|Er zijn geen dimensies|
|DDoSTriggerUDPPackets|Binnenkomende UDP-pakketten om DDoS-beperking te activeren|CountPerSecond|Maximum|Binnenkomende UDP-pakketten om DDoS-beperking te activeren|Er zijn geen dimensies|
|DDoSTriggerSYNPackets|Inkomende SYN-pakketten om DDoS-beperking te activeren|CountPerSecond|Maximum|Inkomende SYN-pakketten om DDoS-beperking te activeren|Er zijn geen dimensies|
|VipAvailability|Beschik baarheid gegevenspad|Count|Average|Gemiddelde Beschik baarheid van IP-adressen per tijds duur|Port|
|ByteCount|Aantal bytes|Count|Totaal|Totaal aantal verzonden bytes binnen tijds periode|Poort, richting|
|PacketCount|Aantal pakketten|Count|Totaal|Totaal aantal verzonden pakketten binnen tijds periode|Poort, richting|
|SynCount|SYN-aantal|Count|Totaal|Totaal aantal SYN-pakketten verzonden binnen tijds periode|Poort, richting|

## <a name="microsoftnetworkazurefirewalls"></a>Microsoft.Network/azurefirewalls

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|ApplicationRuleHit|Aantal treffers toepassings regels|Count|Totaal|Aantal keer dat toepassings regels zijn geraakt|Status, Reason, Protocol|
|NetworkRuleHit|Aantal treffers in netwerk regels|Count|Totaal|Aantal keren dat netwerk regels zijn geraakt|Status, Reason, Protocol|

## <a name="microsoftnetworkapplicationgateways"></a>Microsoft.Network/applicationGateways

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|Doorvoer|Doorvoer|BytesPerSecond|Totaal|Aantal bytes per seconde dat de Application Gateway heeft bediend|Er zijn geen dimensies|
|UnhealthyHostCount|Aantal hosts met slechte status|Count|Average|Aantal beschadigde backend-hosts|BackendSettingsPool|
|HealthyHostCount|Aantal goede hosts|Count|Average|Aantal gezonde backend-hosts|BackendSettingsPool|
|TotalRequests|Totaal aantal aanvragen|Count|Totaal|Aantal geslaagde aanvragen dat Application Gateway heeft bediend|BackendSettingsPool|
|FailedRequests|Mislukte aanvragen|Count|Totaal|Aantal mislukte aanvragen dat Application Gateway heeft bediend|BackendSettingsPool|
|ResponseStatus|Reactie status|Count|Totaal|Http-antwoord status geretourneerd door Application Gateway|HttpStatusGroup|
|CurrentConnections|Huidige verbindingen|Count|Totaal|Aantal actieve verbindingen dat tot stand is gebracht met Application Gateway|Er zijn geen dimensies|
|CapacityUnits|Huidige capaciteits eenheden|Count|Average|Verbruikte capaciteits eenheden|Er zijn geen dimensies|

## <a name="microsoftnetworkvirtualnetworkgateways"></a>Microsoft.Network/virtualNetworkGateways

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|AverageBandwidth|Gateway-S2S-band breedte|BytesPerSecond|Average|Gemiddeld aantal site-naar-site-band breedte van een gateway in bytes per seconde|Er zijn geen dimensies|
|P2SBandwidth|Gateway P2S-band breedte|BytesPerSecond|Average|Gemiddelde Point-to-site band breedte van een gateway in bytes per seconde|Er zijn geen dimensies|
|P2SConnectionCount|Aantal P2S-verbindingen|Count|Maximum|Aantal Point-to-site-verbindingen van een gateway|Protocol|
|TunnelAverageBandwidth|Tunnel bandbreedte|BytesPerSecond|Average|Gemiddelde band breedte van een tunnel in bytes per seconde|ConnectionName, RemoteIP|
|TunnelEgressBytes|Bytes voor uitgaand tunnels|Bytes|Totaal|Uitgaande bytes van een tunnel|ConnectionName, RemoteIP|
|TunnelIngressBytes|Bytes van de tunnel ingang|Bytes|Totaal|Binnenkomende bytes van een tunnel|ConnectionName, RemoteIP|
|TunnelEgressPackets|Tunnel-uituitgangs pakketten|Count|Totaal|Aantal uitgaande pakketten van een tunnel|ConnectionName, RemoteIP|
|TunnelIngressPackets|Tunnel ingangs pakketten|Count|Totaal|Binnenkomend pakket aantal van een tunnel|ConnectionName, RemoteIP|
|TunnelEgressPacketDropTSMismatch|Uitschakeling van niet-overeenkomende TS-pakketten door tunnel|Count|Totaal|Aantal uitgevallen uitgaande pakketten van de selectie van de verkeers kiezer komen niet overeen met een tunnel|ConnectionName, RemoteIP|
|TunnelIngressPacketDropTSMismatch|Verloren gegane pakketten door de tunnel ingang TS komen niet overeen|Count|Totaal|Aantal inkomende pakketten in de verkeers selectie niet overeenkomen met een tunnel|ConnectionName, RemoteIP|

## <a name="microsoftnetworkexpressroutecircuits"></a>Microsoft.Network/expressRouteCircuits

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|BitsInPerSecond|BitsInPerSecond|CountPerSecond|Average|Bits die Azure per seconde binnenkomen|PeeringType|
|BitsOutPerSecond|BitsOutPerSecond|CountPerSecond|Average|Bits egressing Azure per seconde|PeeringType|

## <a name="microsoftnetworkexpressroutecircuitspeerings"></a>Microsoft.Network/expressRouteCircuits/peerings

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|BitsInPerSecond|BitsInPerSecond|CountPerSecond|Average|Bits die Azure per seconde binnenkomen|Er zijn geen dimensies|
|BitsOutPerSecond|BitsOutPerSecond|CountPerSecond|Average|Bits egressing Azure per seconde|Er zijn geen dimensies|

## <a name="microsoftnetworkconnections"></a>Microsoft.Network/connections

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|BitsInPerSecond|BitsInPerSecond|CountPerSecond|Average|Bits die Azure per seconde binnenkomen|Er zijn geen dimensies|
|BitsOutPerSecond|BitsOutPerSecond|CountPerSecond|Average|Bits egressing Azure per seconde|Er zijn geen dimensies|

## <a name="microsoftnetworktrafficmanagerprofiles"></a>Microsoft.Network/trafficManagerProfiles

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|QpsByEndpoint|Query's op eind punt geretourneerd|Count|Totaal|Aantal keren dat een Traffic Manager eind punt is geretourneerd in het opgegeven tijds bestek|EndpointName|
|ProbeAgentCurrentEndpointStateByProfileResourceId|Eindpunt status op eind punt|Count|Maximum|1 als de test status van een eind punt is ingeschakeld, 0 anders.|EndpointName|

## <a name="microsoftnetworknetworkwatchersconnectionmonitors"></a>Microsoft.Network/networkWatchers/connectionMonitors

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|ProbesFailedPercent|% Tests mislukt|Percent|Average|% van de controles van connectiviteits controle is mislukt|Er zijn geen dimensies|
|AverageRoundtripMs|Gem. Retour tijd (MS)|MilliSeconds|Average|Gemiddelde Round-retour tijd van het netwerk (MS) voor connectiviteits controle tests die zijn verzonden tussen de bron en het doel|Er zijn geen dimensies|

## <a name="microsoftnetworkfrontdoors"></a>Microsoft.Network/frontdoors

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|RequestCount|Aantal aanvragen|Count|Totaal|Het aantal client aanvragen dat wordt geleverd door de HTTP/S-proxy|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry|
|RequestSize|Aanvraag grootte|Bytes|Totaal|Het aantal bytes dat is verzonden als aanvragen van clients naar de HTTP/S-proxy|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry|
|ResponseSize|Grootte van antwoord|Bytes|Totaal|Het aantal bytes dat is verzonden als reacties van HTTP/S-proxy naar clients|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry|
|BackendRequestCount|Aantal back-aanvragen|Count|Totaal|Het aantal aanvragen dat is verzonden vanaf de HTTP/S-proxy naar back-end|Http status, HttpStatusGroup, back-end|
|BackendRequestLatency|Latentie van back-upaanvraag|MilliSeconds|Average|De tijd die wordt berekend vanaf het moment dat de aanvraag door de HTTP/S-proxy naar de back-end werd verzonden, totdat de HTTP/S-proxy de laatste antwoord byte van de back-end heeft ontvangen|Back-end|
|TotalLatency|Totale latentie|MilliSeconds|Average|De tijd die wordt berekend vanaf het moment dat de client aanvraag door de HTTP/S-proxy werd ontvangen totdat de client de laatste antwoord byte van de HTTP/S-proxy heeft bevestigd|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry|
|BackendHealthPercentage|Back-status percentage|Percent|Average|Het percentage geslaagde status controles van de HTTP/S-proxy naar back-end|Back-end, hosts|
|WebApplicationFirewallRequestCount|Aantal aanvragen voor Web Application firewall|Count|Totaal|Het aantal client aanvragen dat is verwerkt door de Web Application firewall|Beleidsnaam, regelnaam, actie|

## <a name="microsoftnotificationhubsnamespacesnotificationhubs"></a>Microsoft.NotificationHubs/Namespaces/NotificationHubs

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|registratie. alle|Registratiebewerkingen|Count|Totaal|Het aantal registratiebewerkingen (maken, bijwerken, query's uitvoeren en verwijderen). |Er zijn geen dimensies|
|registratie. Create|Bewerkingen voor het maken van registraties|Count|Totaal|Het aantal gemaakte registraties.|Er zijn geen dimensies|
|registratie. update|Bewerkingen voor het bijwerken van registraties|Count|Totaal|Het aantal voltooide registratie-updates.|Er zijn geen dimensies|
|registratie. ophalen|Bewerkingen voor het lezen van registraties|Count|Totaal|Het aantal uitgevoerde registratiequery's.|Er zijn geen dimensies|
|registratie. Delete|Bewerkingen voor het verwijderen van registraties|Count|Totaal|Het aantal verwijderde registraties.|Er zijn geen dimensies|
|e-mail|Binnenkomende berichten|Count|Totaal|Het aantal voltooide verzonden API-aanroepen. |Er zijn geen dimensies|
|inkomend. gepland|Geplande pushmeldingen verzonden|Count|Totaal|Geplande push meldingen geannuleerd|Er zijn geen dimensies|
|Binnenkomend. gepland. annuleren|Geplande push meldingen geannuleerd|Count|Totaal|Geplande push meldingen geannuleerd|Er zijn geen dimensies|
|gepland. in behandeling|Geplande meldingen in behandeling|Count|Totaal|Geplande meldingen in behandeling|Er zijn geen dimensies|
|installatie. alle|Installatiebeheerbewerkingen|Count|Totaal|Installatiebeheerbewerkingen|Er zijn geen dimensies|
|installation.get|Installatiebewerkingen ophalen|Count|Totaal|Installatiebewerkingen ophalen|Er zijn geen dimensies|
|installatie. upsert|Installatiebewerkingen maken of bijwerken|Count|Totaal|Installatiebewerkingen maken of bijwerken|Er zijn geen dimensies|
|installatie. patch|Installatiebewerkingen patchen|Count|Totaal|Installatiebewerkingen patchen|Er zijn geen dimensies|
|installation.delete|Installatiebewerkingen verwijderen|Count|Totaal|Installatiebewerkingen verwijderen|Er zijn geen dimensies|
|outgoing.allpns.success|Voltooide meldingen|Count|Totaal|Het aantal voltooide meldingen.|Er zijn geen dimensies|
|outgoing.allpns.invalidpayload|Fouten met nettolading|Count|Totaal|Het aantal pushes dat is mislukt omdat PNS een fout met een onjuiste payload heeft geretourneerd.|Er zijn geen dimensies|
|uitgaande. allpns. pnserror|Fouten met het externe meldingssysteem|Count|Totaal|Het aantal pushes dat is mislukt vanwege een probleem in de communicatie met PNS (exclusief verificatieproblemen).|Er zijn geen dimensies|
|uitgaande. allpns. channelerror|Fouten met kanaal|Count|Totaal|Het aantal pushes dat is mislukt omdat het kanaal ongeldig is, niet is gekoppeld aan de juiste app, beperkt is of verlopen is.|Er zijn geen dimensies|
|outgoing.allpns.badorexpiredchannel|Fouten met slecht of verlopen kanaal|Count|Totaal|Het aantal pushes dat is mislukt omdat het kanaal/het token/de registratie-id in de registratie is verlopen of ongeldig is.|Er zijn geen dimensies|
|outgoing.wns.success|Meldingen over voltooide WNS|Count|Totaal|Het aantal voltooide meldingen.|Er zijn geen dimensies|
|outgoing.wns.invalidcredentials|WNS-verificatiefouten (ongeldige referenties)|Count|Totaal|Het aantal pushes dat is mislukt omdat PNS de opgegeven referenties niet accepteert of omdat de referenties zijn geblokkeerd. (Windows Live herkent de referenties niet).|Er zijn geen dimensies|
|outgoing.wns.badchannel|WNS-fout wegens slecht kanaal|Count|Totaal|Het aantal pushes dat is mislukt omdat de kanaal in de registratie niet is herkend (WNS-status: 404 niet gevonden).|Er zijn geen dimensies|
|outgoing.wns.expiredchannel|WNS-fout wegens verlopen kanaal|Count|Totaal|Het aantal pushes dat is mislukt omdat de kanaal is verlopen (WNS-status: 410 verdwenen).|Er zijn geen dimensies|
|uitgaande. wns. throttled|Meldingen over beperkte WNS|Count|Totaal|Het aantal pushes dat is mislukt omdat WNS deze app beperkt (WNS-status: 406 niet toegestaan).|Er zijn geen dimensies|
|outgoing.wns.tokenproviderunreachable|WNS-verificatiefouten (onbereikbaar)|Count|Totaal|Windows Live is niet bereikbaar.|Er zijn geen dimensies|
|uitgaande. wns. invalidtoken|WNS-verificatiefouten (ongeldig token)|Count|Totaal|Het token dat is door gegeven aan WNS is niet geldig (WNS-status: 401 niet gemachtigd).|Er zijn geen dimensies|
|outgoing.wns.wrongtoken|WNS-verificatiefouten (onjuist token)|Count|Totaal|Het token dat is door gegeven aan WNS is geldig, maar voor een andere toepassing (WNS-status: 403 verboden). Dit kan gebeuren als de kanaal in de registratie is gekoppeld aan een andere app. Controleer of de client-app is gekoppeld aan dezelfde app waarvan de referenties zich in de notification hub bevinden.|Er zijn geen dimensies|
|outgoing.wns.invalidnotificationformat|Ongeldige indeling van de WNS-melding|Count|Totaal|De indeling van de melding is ongeldig (WNS-status: 400). Houd er rekening mee dat WNS niet alle ongeldige payloads weigert.|Er zijn geen dimensies|
|outgoing.wns.invalidnotificationsize|Fout met ongeldige grootte van de WNS-melding|Count|Totaal|De meldings lading is te groot (WNS-status: 413).|Er zijn geen dimensies|
|outgoing.wns.channelthrottled|De verbinding met het WNS-kanaal is beperkt|Count|Totaal|De melding is verwijderd omdat de kanaal in de registratie is beperkt (WNS-reactie header: X-WNS-notification status: channelThrottled).|Er zijn geen dimensies|
|outgoing.wns.channeldisconnected|De verbinding met het WNS-kanaal is verbroken|Count|Totaal|De melding is verwijderd omdat de kanaal in de registratie is beperkt (WNS-reactie header: X-WNS-DeviceConnectionStatus: verbinding verbroken).|Er zijn geen dimensies|
|uitgaande. wns. dropd|Meldingen over verloren WNS|Count|Totaal|De melding is verwijderd omdat de kanaal-URI in de registratie is beperkt (X-WNS-NotificationStatus: dropped, maar niet X-WNS-DeviceConnectionStatus: disconnected).|Er zijn geen dimensies|
|uitgaande. wns. pnserror|WNS-fouten|Count|Totaal|De melding is niet afgeleverd omdat er fouten zijn opgetreden in de communicatie met WNS.|Er zijn geen dimensies|
|uitgaande. wns. authenticationerror|WNS-verificatiefouten|Count|Totaal|De melding is niet afgeleverd omdat er fouten zijn opgetreden in de communicatie met Windows Live, vanwege ongeldige referenties of vanwege een onjuist token.|Er zijn geen dimensies|
|outgoing.apns.success|Meldingen over voltooide APNS|Count|Totaal|Het aantal voltooide meldingen.|Er zijn geen dimensies|
|outgoing.apns.invalidcredentials|APNS-verificatiefouten|Count|Totaal|Het aantal pushes dat is mislukt omdat PNS de opgegeven referenties niet accepteert of omdat de referenties zijn geblokkeerd.|Er zijn geen dimensies|
|outgoing.apns.badchannel|APNS-fout wegens slecht kanaal|Count|Totaal|Het aantal pushes dat is mislukt omdat het token ongeldig is (status code voor het binaire APNS-Protocol: 8. Status code van APNS-HTTP-protocol: 400 met "BadDeviceToken").|Er zijn geen dimensies|
|outgoing.apns.expiredchannel|APNS-fout wegens verlopen kanaal|Count|Totaal|Het aantal tokens dat ongeldig is gemaakt door het APNS-feedbackkanaal.|Er zijn geen dimensies|
|outgoing.apns.invalidnotificationsize|Fout met ongeldige grootte van APNS-melding|Count|Totaal|Het aantal pushes dat is mislukt omdat de payload te groot is (status code van het binaire APNS-Protocol: 7).|Er zijn geen dimensies|
|outgoing.apns.pnserror|APNS-fouten|Count|Totaal|Het aantal pushes dat is mislukt vanwege fouten in de communicatie met APNS.|Er zijn geen dimensies|
|outgoing.gcm.success|Meldingen over voltooide GCM|Count|Totaal|Het aantal voltooide meldingen.|Er zijn geen dimensies|
|outgoing.gcm.invalidcredentials|GCM-verificatiefouten (ongeldige referenties)|Count|Totaal|Het aantal pushes dat is mislukt omdat PNS de opgegeven referenties niet accepteert of omdat de referenties zijn geblokkeerd.|Er zijn geen dimensies|
|outgoing.gcm.badchannel|GCM-fout wegens slecht kanaal|Count|Totaal|Het aantal pushes dat is mislukt omdat de registratie in de registratie niet is herkend (GCM-resultaat: Ongeldige registratie).|Er zijn geen dimensies|
|outgoing.gcm.expiredchannel|GCM-fout wegens verlopen kanaal|Count|Totaal|Het aantal pushes dat is mislukt omdat de registratie in de registratie is verlopen (GCM resultaat: NotRegistered).|Er zijn geen dimensies|
|uitgaande. GCM. throttled|Meldingen over beperkte GCM|Count|Totaal|Het aantal pushes dat is mislukt omdat GCM deze app heeft beperkt (GCM-status code: 501-599 of resultaat: niet beschikbaar).|Er zijn geen dimensies|
|uitgaande. GCM. invalidnotificationformat|Ongeldige indeling van GCM-melding|Count|Totaal|Het aantal pushes dat is mislukt omdat de payload niet juist is geformatteerd (GCM-resultaat: InvalidDataKey of InvalidTtl).|Er zijn geen dimensies|
|outgoing.gcm.invalidnotificationsize|Fout met ongeldige grootte van GCM-melding|Count|Totaal|Het aantal pushes dat is mislukt omdat de payload te groot is (GCM-resultaat: MessageTooBig).|Er zijn geen dimensies|
|outgoing.gcm.wrongchannel|GCM-fout wegens onjuist kanaal|Count|Totaal|Het aantal pushes dat is mislukt omdat de registratie in de registratie niet is gekoppeld aan de huidige app (GCM-resultaat: InvalidPackageName).|Er zijn geen dimensies|
|uitgaande. GCM. pnserror|GCM-fouten|Count|Totaal|Het aantal pushes dat is mislukt vanwege fouten in de communicatie met GCM.|Er zijn geen dimensies|
|uitgaande. GCM. authenticationerror|GCM-verificatiefouten|Count|Totaal|Het aantal pushes dat is mislukt omdat de PNS de opgegeven referenties niet heeft geaccepteerd, de referenties zijn geblokkeerd of de SenderId niet juist is geconfigureerd in de app (GCM-resultaat: MismatchedSenderId).|Er zijn geen dimensies|
|uitgaand. mpns. geslaagd|Meldingen over voltooide MPNS|Count|Totaal|Het aantal voltooide meldingen.|Er zijn geen dimensies|
|outgoing.mpns.invalidcredentials|Ongeldige MPNS-referenties|Count|Totaal|Het aantal pushes dat is mislukt omdat PNS de opgegeven referenties niet accepteert of omdat de referenties zijn geblokkeerd.|Er zijn geen dimensies|
|outgoing.mpns.badchannel|MPNS-fout wegens slecht kanaal|Count|Totaal|Het aantal pushes dat is mislukt omdat de kanaal in de registratie niet is herkend (MPNS-status: 404 niet gevonden).|Er zijn geen dimensies|
|uitgaande. mpns. throttled|Meldingen over beperkte MPNS|Count|Totaal|Het aantal pushes dat is mislukt omdat MPNS deze app beperkt (WNS MPNS: 406 niet toegestaan).|Er zijn geen dimensies|
|uitgaande. mpns. invalidnotificationformat|Ongeldige indeling van de MPNS-melding|Count|Totaal|Het aantal pushes dat is mislukt omdat de payload van de melding te groot is.|Er zijn geen dimensies|
|outgoing.mpns.channeldisconnected|De verbinding met het MPNS-kanaal is verbroken|Count|Totaal|Het aantal pushes dat is mislukt omdat de kanaal van de registratie is verbroken (MPNS-status: 412 niet gevonden).|Er zijn geen dimensies|
|uitgaande. mpns. dropd|Meldingen over verloren MPNS|Count|Totaal|Het aantal pushes dat is verwijderd door MPNS (MPNS-reactie header: X-notification status: QueueFull of onderdrukt).|Er zijn geen dimensies|
|uitgaande. mpns. pnserror|MPNS-fouten|Count|Totaal|Het aantal pushes dat is mislukt vanwege fouten in de communicatie met MPNS.|Er zijn geen dimensies|
|uitgaande. mpns. authenticationerror|MPNS-verificatiefouten|Count|Totaal|Het aantal pushes dat is mislukt omdat PNS de opgegeven referenties niet accepteert of omdat de referenties zijn geblokkeerd.|Er zijn geen dimensies|
|notificationhub. pushes|Alle uitgaande meldingen|Count|Totaal|Alle uitgaande meldingen van de Notification Hub|Er zijn geen dimensies|
|binnenkomende. alle. aanvragen|Alle binnenkomende aanvragen|Count|Totaal|Totaal aantal binnenkomende aanvragen voor een Notification Hub|Er zijn geen dimensies|
|inkomend. alle. failedrequests|Alle binnenkomende mislukte aanvragen|Count|Totaal|Totaal aantal binnenkomende mislukte aanvragen voor een Notification Hub|Er zijn geen dimensies|

## <a name="microsoftoperationalinsightsworkspaces"></a>Microsoft.OperationalInsights/workspaces

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|Average_% vrije inodes|% Vrije inodes|Count|Average|Average_% vrije inodes|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_% beschik bare ruimte|Percentage vrije ruimte|Count|Average|Average_% beschik bare ruimte|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_% gebruikte inodes|% Gebruikte inodes|Count|Average|Average_% gebruikte inodes|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_ percentage gebruikte ruimte|Percentage gebruikte ruimte|Count|Average|Average_ percentage gebruikte ruimte|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Disk Lees bewerkingen in bytes per seconde|Bytes gelezen op schijf/sec|Count|Average|Average_Disk Lees bewerkingen in bytes per seconde|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Disk Reads/sec|Schijf lezen per seconde|Count|Average|Average_Disk Reads/sec|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Disk-overdrachten per seconde|Schijfoverdrachten per seconde|Count|Average|Average_Disk-overdrachten per seconde|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Disk geschreven bytes per seconde|Bytes geschreven naar schijf/sec|Count|Average|Average_Disk geschreven bytes per seconde|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Disk schrijf bewerkingen per seconde|Schijf schrijven per seconde|Count|Average|Average_Disk schrijf bewerkingen per seconde|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Free Megabytes|Beschikbare Megabytes|Count|Average|Average_Free Megabytes|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Logical schijf bytes per seconde|Logische schijf Bytes per seconde|Count|Average|Average_Logical schijf bytes per seconde|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_ percentage beschikbaar geheugen|Percentage beschikbaar geheugen|Count|Average|Average_ percentage beschikbaar geheugen|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_% beschik bare wissel ruimte|Percentage beschik bare wissel ruimte|Count|Average|Average_% beschik bare wissel ruimte|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_ percentage gebruikt geheugen|Percentage gebruikt geheugen|Count|Average|Average_ percentage gebruikt geheugen|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_% gebruikte wissel ruimte|Percentage gebruikte wissel ruimte|Count|Average|Average_% gebruikte wissel ruimte|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Available MB geheugen|Beschikbaar geheugen in megabytes|Count|Average|Average_Available MB geheugen|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Available mega bytes swap|Beschik bare mega bytes wisselen|Count|Average|Average_Available mega bytes swap|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Page Lees bewerkingen per seconde|Gelezen pagina's per seconde|Count|Average|Average_Page Lees bewerkingen per seconde|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Page schrijf bewerkingen per seconde|Geschreven pagina's per seconde|Count|Average|Average_Page schrijf bewerkingen per seconde|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Pages per seconde|Pagina's per seconde|Count|Average|Average_Pages per seconde|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Used mega bytes swap-ruimte|Gebruikte MB wissel ruimte|Count|Average|Average_Used mega bytes swap-ruimte|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Used Memory MB|Gebruikt geheugen Mbytes|Count|Average|Average_Used Memory MB|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Verzonden Average_Total bytes|Totaal aantal verzonden Bytes|Count|Average|Verzonden Average_Total bytes|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Ontvangen Average_Total bytes|Totaal aantal ontvangen Bytes|Count|Average|Ontvangen Average_Total bytes|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Total bytes|Totaal aantal bytes|Count|Average|Average_Total bytes|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Verzonden Average_Total-pakketten|Totaal aantal verzonden pakketten|Count|Average|Verzonden Average_Total-pakketten|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Ontvangen Average_Total-pakketten|Totaal aantal ontvangen pakketten|Count|Average|Ontvangen Average_Total-pakketten|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Total RX-fouten|Totaal aantal RX-fouten|Count|Average|Average_Total RX-fouten|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Total TX-fouten|Totaal aantal TX-fouten|Count|Average|Average_Total TX-fouten|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Total-conflicten|Totaal aantal conflicten|Count|Average|Average_Total-conflicten|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Avg. Schijf sec/lezen|Gem. Schijf sec/lezen|Count|Average|Average_Avg. Schijf sec/lezen|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Avg. Schijfoverdrachten per seconde|Gem. Schijfoverdrachten per seconde|Count|Average|Average_Avg. Schijfoverdrachten per seconde|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Avg. Schijf sec/schrijven|Gem. Schijf sec/schrijven|Count|Average|Average_Avg. Schijf sec/schrijven|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Physical schijf bytes per seconde|Bytes van fysieke schijf per seconde|Count|Average|Average_Physical schijf bytes per seconde|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Pct geprivilegieerde tijd|Pct-geprivilegieerde tijd|Count|Average|Average_Pct geprivilegieerde tijd|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Pct gebruikers tijd|Pct-gebruikers tijd|Count|Average|Average_Pct gebruikers tijd|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Used geheugen kBytes|Gebruikte geheugen-kBytes|Count|Average|Average_Used geheugen kBytes|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Gedeeld Average_Virtual-geheugen|Virtueel gedeeld geheugen|Count|Average|Gedeeld Average_Virtual-geheugen|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_ percentage DPC-tijd|Percentage DPC-tijd|Count|Average|Average_ percentage DPC-tijd|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_ percentage niet-actieve tijd|Percentage niet-actieve tijd|Count|Average|Average_ percentage niet-actieve tijd|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Percentage interrupt-tijd Average_|Percentage interrupt-tijd|Count|Average|Percentage interrupt-tijd Average_|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_% i/o-wacht tijd|% I/o-wacht tijd|Count|Average|Average_% i/o-wacht tijd|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_% Nice tijd|Percentage tijd in Nice|Count|Average|Average_% Nice tijd|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_ percentage tijd in beschermde modus|Percentage tijd in beschermde modus|Count|Average|Average_ percentage tijd in beschermde modus|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_% processor tijd|% Processortijd|Count|Average|Average_% processor tijd|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_% gebruikers tijd|Percentage gebruikers tijd|Count|Average|Average_% gebruikers tijd|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Free fysiek geheugen|Vrij fysiek geheugen|Count|Average|Average_Free fysiek geheugen|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Free ruimte in Wissel bestanden|Vrije ruimte in wissel geheugen bestanden|Count|Average|Average_Free ruimte in Wissel bestanden|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Virtueel geheugen Average_Free|Vrij virtueel geheugen|Count|Average|Virtueel geheugen Average_Free|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Processes|Processen|Count|Average|Average_Processes|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Size opgeslagen in Wissel bestanden|Grootte opgeslagen in Wissel bestanden|Count|Average|Average_Size opgeslagen in Wissel bestanden|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Uptime|Actieve tijdsduur|Count|Average|Average_Uptime|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Users|Gebruikers|Count|Average|Average_Users|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Avg. Schijf sec/lezen|Gem. Schijf sec/lezen|Count|Average|Average_Avg. Schijf sec/lezen|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Avg. Schijf sec/schrijven|Gem. Schijf sec/schrijven|Count|Average|Average_Avg. Schijf sec/schrijven|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Wachtrij lengte van Average_Current-schijf|Huidige wachtrij lengte voor de schijf|Count|Average|Wachtrij lengte van Average_Current-schijf|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Disk Reads/sec|Schijf lezen per seconde|Count|Average|Average_Disk Reads/sec|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Disk-overdrachten per seconde|Schijfoverdrachten per seconde|Count|Average|Average_Disk-overdrachten per seconde|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Disk schrijf bewerkingen per seconde|Schijf schrijven per seconde|Count|Average|Average_Disk schrijf bewerkingen per seconde|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Free Megabytes|Beschikbare Megabytes|Count|Average|Average_Free Megabytes|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_% beschik bare ruimte|Percentage vrije ruimte|Count|Average|Average_% beschik bare ruimte|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Available mega bytes|Beschikbare megabytes (MB)|Count|Average|Average_Available mega bytes|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_% toegewezen bytes in gebruik|% Toegewezen bytes in gebruik|Count|Average|Average_% toegewezen bytes in gebruik|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Ontvangen Average_Bytes per seconde|Ontvangen bytes per seconde|Count|Average|Ontvangen Average_Bytes per seconde|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Verzonden Average_Bytes per seconde|Verzonden bytes per seconde|Count|Average|Verzonden Average_Bytes per seconde|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Totaal aantal Average_Bytes per seconde|Totaal aantal bytes per seconde|Count|Average|Totaal aantal Average_Bytes per seconde|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_% processor tijd|% Processortijd|Count|Average|Average_% processor tijd|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Lengte van Average_Processor-wachtrij|Lengte van de processor wachtrij|Count|Average|Lengte van Average_Processor-wachtrij|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Heartbeat|Heartbeat|Count|Totaal|Heartbeat|Computer, OSType, versie, SourceComputerId|
|Update|Update|Count|Average|Update|Computer, product, classificatie, update State, optioneel, goedgekeurd|
|Gebeurtenis|Gebeurtenis|Count|Average|Gebeurtenis|Source, EventLog, computer, EventCategory, EventLevel, EventLevelName, Event gebeurtenis|

## <a name="microsoftpowerbidedicatedcapacities"></a>Micro soft. PowerBIDedicated/capaciteiten

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|QueryDuration|Query duur|Milliseconden|Average|DAX-query duur in laatste interval|Er zijn geen dimensies|
|QueryPoolJobQueueLength|Lijnen Wachtrij lengte van de taak pool voor query's|Count|Average|Aantal taken in de wachtrij van de query thread pool.|Er zijn geen dimensies|
|qpu_high_utilization_metric|Hoog gebruik van QPU|Count|Totaal|QPU hoog gebruik in de laatste minuut, 1 voor hoog QPU gebruik, anders 0|Er zijn geen dimensies|
|memory_metric|Geheugen|Bytes|Average|Geheugenmetabase. Bereik 0-3 GB voor a1, 0-5 GB voor a2, 0-10 GB voor a3, 0-25 GB voor A4, 0-50 GB voor A5 en 0-100 GB voor A6|Er zijn geen dimensies|
|memory_thrashing_metric|Geheugen overbelasting|Percent|Average|Gemiddeld geheugen overbelasting.|Er zijn geen dimensies|

## <a name="microsoftrelaynamespaces"></a>Micro soft. relay/naam ruimten

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|ListenerConnections-Success|ListenerConnections-Success|Count|Totaal|Voltooide ListenerConnections voor Microsoft.Relay.|EntityName|
|ListenerConnections-ClientError|ListenerConnections-ClientError|Count|Totaal|ClientError op ListenerConnections voor Microsoft.Relay.|EntityName|
|ListenerConnections-ServerError|ListenerConnections-ServerError|Count|Totaal|ServerError op ListenerConnections voor Microsoft.Relay.|EntityName|
|SenderConnections-Success|SenderConnections-Success|Count|Totaal|Voltooide SenderConnections voor Microsoft.Relay.|EntityName|
|SenderConnections-ClientError|SenderConnections-ClientError|Count|Totaal|ClientError op SenderConnections voor Microsoft.Relay.|EntityName|
|SenderConnections-ServerError|SenderConnections-ServerError|Count|Totaal|ServerError op SenderConnections voor Microsoft.Relay.|EntityName|
|ListenerConnections-TotalRequests|ListenerConnections-TotalRequests|Count|Totaal|Totale ListenerConnections voor Microsoft.Relay.|EntityName|
|SenderConnections-TotalRequests|SenderConnections-TotalRequests|Count|Totaal|Totale SenderConnections-aanvragen voor Microsoft.Relay.|EntityName|
|ActiveConnections|ActiveConnections|Count|Totaal|Totale ActiveConnections voor Microsoft.Relay.|EntityName|
|ActiveListeners|ActiveListeners|Count|Totaal|Totale ActiveListeners voor Microsoft.Relay.|EntityName|
|BytesTransferred|BytesTransferred|Count|Totaal|Totale BytesTransferred voor Microsoft.Relay.|EntityName|
|ListenerDisconnects|ListenerDisconnects|Count|Totaal|Totale ListenerDisconnects voor Microsoft.Relay.|EntityName|
|SenderDisconnects|SenderDisconnects|Count|Totaal|Total SenderDisconnects for Microsoft.Relay.|EntityName|

## <a name="microsoftsearchsearchservices"></a>Microsoft.Search/searchServices

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|SearchLatency|Zoek latentie|Seconden|Average|Gemiddelde Zoek latentie voor de zoek service|Er zijn geen dimensies|
|SearchQueriesPerSecond|Zoekquery's per seconde|CountPerSecond|Average|Zoek query's per seconde voor de zoek service|Er zijn geen dimensies|
|ThrottledSearchQueriesPercentage|Percentage beperkte zoekquery's|Percent|Average|Percentage Zoek query's dat is beperkt tot de zoek service|Er zijn geen dimensies|

## <a name="microsoftservicebusnamespaces"></a>Microsoft.ServiceBus/namespaces

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|SuccessfulRequests|Geslaagde aanvragen (preview-versie)|Count|Totaal|Totaal aantal geslaagde aanvragen voor een naam ruimte (preview-versie)|EntityName|
|ServerErrors|Serverfouten. (Preview-versie)|Count|Totaal|Serverfouten voor Microsoft.ServiceBus. (Preview-versie)|EntityName|
|UserErrors|Gebruikersfouten. (Preview-versie)|Count|Totaal|Gebruikersfouten voor Microsoft.ServiceBus. (Preview-versie)|EntityName|
|ThrottledRequests|Beperkte aanvragen. (Preview-versie)|Count|Totaal|Beperkte aanvragen voor Microsoft.ServiceBus. (Preview-versie)|EntityName|
|IncomingRequests|Inkomende aanvragen (preview-versie)|Count|Totaal|Binnenkomende aanvragen voor Microsoft.ServiceBus. (Preview-versie)|EntityName|
|IncomingMessages|Inkomende berichten (preview-versie)|Count|Totaal|Binnenkomende berichten voor Microsoft.ServiceBus. (Preview-versie)|EntityName|
|OutgoingMessages|Uitgaande berichten (preview-versie)|Count|Totaal|Uitgaande berichten voor Microsoft.ServiceBus. (Preview-versie)|EntityName|
|ActiveConnections|ActiveConnection (preview-versie)|Count|Totaal|Totaal aantal actieve verbindingen voor Microsoft.ServiceBus. (Preview-versie)|Er zijn geen dimensies|
|Size|Grootte (preview-versie)|Bytes|Average|Grootte van een wachtrij/onderwerp in bytes. (Preview-versie)|EntityName|
|Berichten|Aantal berichten in een wachtrij/onderwerp. (Preview-versie)|Count|Average|Aantal berichten in een wachtrij/onderwerp. (Preview-versie)|EntityName|
|ActiveMessages|Aantal actieve berichten in een wachtrij/onderwerp. (Preview-versie)|Count|Average|Aantal actieve berichten in een wachtrij/onderwerp. (Preview-versie)|EntityName|
|DeadletteredMessages|Het aantal onbestelbare berichten in een wachtrij/onderwerp. (Preview-versie)|Count|Average|Het aantal onbestelbare berichten in een wachtrij/onderwerp. (Preview-versie)|EntityName|
|ScheduledMessages|Het aantal geplande berichten in een wachtrij/onderwerp. (Preview-versie)|Count|Average|Het aantal geplande berichten in een wachtrij/onderwerp. (Preview-versie)|EntityName|
|CPUXNS|CPU-gebruik per naamruimte|Percent|Maximum|Metrische gegevens voor het CPU-gebruik van de Premium-naamruimte voor Service Bus|Er zijn geen dimensies|
|WSXNS|Geheugengebruik per naamruimte|Percent|Maximum|Metrische gegevens voor het geheugengebruik van de Premium-naamruimte voor Service Bus|Er zijn geen dimensies|

## <a name="microsoftservicefabricmeshapplications"></a>Microsoft.ServiceFabricMesh/applications

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|AllocatedCpu|AllocatedCpu|Count|Average|CPU toegewezen aan deze container in millicores|ApplicationName, ServiceName, CodePackageName, ServiceReplicaName|
|AllocatedMemory|AllocatedMemory|Bytes|Average|Geheugen toegewezen aan deze container in MB|ApplicationName, ServiceName, CodePackageName, ServiceReplicaName|
|ActualCpu|ActualCpu|Count|Average|Werkelijk CPU-gebruik in millicores|ApplicationName, ServiceName, CodePackageName, ServiceReplicaName|
|ActualMemory|ActualMemory|Bytes|Average|Werkelijk geheugen gebruik in MB|ApplicationName, ServiceName, CodePackageName, ServiceReplicaName|
|CpuUtilization|CpuUtilization|Percent|Average|Gebruik van CPU voor deze container als percentage van AllocatedCpu|ApplicationName, ServiceName, CodePackageName, ServiceReplicaName|
|MemoryUtilization|MemoryUtilization|Percent|Average|Gebruik van CPU voor deze container als percentage van AllocatedCpu|ApplicationName, ServiceName, CodePackageName, ServiceReplicaName|
|ApplicationStatus|ApplicationStatus|Count|Average|Status van Service Fabric mesh-toepassing|ApplicationName, status|
|ServiceStatus|ServiceStatus|Count|Average|Integriteits status van een service in Service Fabric mesh-toepassing|ApplicationName, Status, ServiceName|
|ServiceReplicaStatus|ServiceReplicaStatus|Count|Average|Integriteits status van een service replica in Service Fabric mesh-toepassing|ApplicationName, Status, ServiceName, ServiceReplicaName|
|Container status|Container status|Count|Average|Status van de container in Service Fabric mesh-toepassing|ApplicationName, ServiceName, CodePackageName, ServiceReplicaName, Status|
|RestartCount|RestartCount|Count|Average|Telling van een container in Service Fabric mesh-toepassing opnieuw starten|ApplicationName, Status, ServiceName, ServiceReplicaName, CodePackageName|

## <a name="microsoftsignalrservicesignalr"></a>Microsoft.SignalRService/SignalR

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|ConnectionCount|Aantal verbindingen|Count|Maximum|De hoeveelheid gebruikers verbinding.|Eindpunt|
|MessageCount|Aantal berichten|Count|Totaal|De totale hoeveelheid berichten.|Er zijn geen dimensies|
|InboundTraffic|Binnenkomend verkeer|Bytes|Totaal|Het inkomende verkeer van de service|Er zijn geen dimensies|
|OutboundTraffic|Uitgaand verkeer|Bytes|Totaal|Het uitgaande verkeer van de service|Er zijn geen dimensies|
|UserErrors|Gebruikers fouten|Percent|Maximum|Het percentage gebruikers fouten|Er zijn geen dimensies|
|SystemErrors|Systeem fouten|Percent|Maximum|Het percentage systeem fouten|Er zijn geen dimensies|

## <a name="microsoftsqlserversdatabases"></a>Microsoft.Sql/servers/databases

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|cpu_percent|Processorpercentage|Percent|Average|Processorpercentage|Er zijn geen dimensies|
|physical_data_read_percent|Percentage gegevens-IO|Percent|Average|Percentage gegevens-IO|Er zijn geen dimensies|
|log_write_percent|Percentage logboek-IO|Percent|Average|Percentage logboek-IO|Er zijn geen dimensies|
|dtu_consumption_percent|DTU-percentage|Percent|Average|DTU-percentage|Er zijn geen dimensies|
|opslag|Gebruikte gegevensruimte|Bytes|Maximum|Totale database grootte|Er zijn geen dimensies|
|connection_successful|Geslaagde verbindingen|Count|Totaal|Geslaagde verbindingen|Er zijn geen dimensies|
|connection_failed|Mislukte verbindingen|Count|Totaal|Mislukte verbindingen|Er zijn geen dimensies|
|blocked_by_firewall|Geblokkeerd door de firewall|Count|Totaal|Geblokkeerd door de firewall|Er zijn geen dimensies|
|constateer|Deadlocks|Count|Totaal|Deadlocks|Er zijn geen dimensies|
|storage_percent|Percentage gebruikte gegevens ruimte|Percent|Maximum|Databaseomvangpercentage|Er zijn geen dimensies|
|xtp_storage_percent|Percentage OLTP-opslag in het geheugen|Percent|Average|Percentage OLTP-opslag in het geheugen|Er zijn geen dimensies|
|workers_percent|Werknemerspercentage|Percent|Average|Werknemerspercentage|Er zijn geen dimensies|
|sessions_percent|Sessiepercentage|Percent|Average|Sessiepercentage|Er zijn geen dimensies|
|dtu_limit|DTU-limiet|Count|Average|DTU-limiet|Er zijn geen dimensies|
|dtu_used|DTU gebruikt|Count|Average|DTU gebruikt|Er zijn geen dimensies|
|cpu_limit|CPU-limiet|Count|Average|CPU-limiet|Er zijn geen dimensies|
|cpu_used|CPU gebruikt|Count|Average|CPU gebruikt|Er zijn geen dimensies|
|dwu_limit|Limiet voor DWU|Count|Maximum|Limiet voor DWU|Er zijn geen dimensies|
|dwu_consumption_percent|Percentage DWU|Percent|Maximum|Percentage DWU|Er zijn geen dimensies|
|dwu_used|DWU gebruikt|Count|Maximum|DWU gebruikt|Er zijn geen dimensies|
|dw_cpu_percent|CPU-percentage van DW-knooppunt niveau|Percent|Average|CPU-percentage van DW-knooppunt niveau|DwLogicalNodeId|
|dw_physical_data_read_percent|IO-percentage gegevens-i/o-knooppunt niveau|Percent|Average|IO-percentage gegevens-i/o-knooppunt niveau|DwLogicalNodeId|
    |cache_hit_percent|Percentage cache treffers|Percent|Maximum|Percentage cache treffers|Er zijn geen dimensies|
|cache_used_percent|Percentage gebruikt cache|Percent|Maximum|Percentage gebruikt cache|Er zijn geen dimensies|
|local_tempdb_usage_percent|Lokaal TempDB-percentage|Percent|Average|Lokaal TempDB-percentage|Er zijn geen dimensies|
|app_cpu_billed|Gefactureerde app-CPU|Count|Totaal|Gefactureerde app-CPU|Er zijn geen dimensies|
|app_cpu_percent|CPU-percentage van app|Percent|Average|CPU-percentage van app|Er zijn geen dimensies|
|app_memory_percent|Percentage gebruikt geheugen van app|Percent|Average|Percentage gebruikt geheugen van app|Er zijn geen dimensies|
|allocated_data_storage|Toegewezen gegevensruimte|Bytes|Average|Toegewezen gegevensruimte|Er zijn geen dimensies|

## <a name="microsoftsqlserverselasticpools"></a>Microsoft.Sql/servers/elasticPools

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|cpu_percent|Processorpercentage|Percent|Average|Processorpercentage|Er zijn geen dimensies|
|physical_data_read_percent|Percentage gegevens-IO|Percent|Average|Percentage gegevens-IO|Er zijn geen dimensies|
|log_write_percent|Percentage logboek-IO|Percent|Average|Percentage logboek-IO|Er zijn geen dimensies|
|dtu_consumption_percent|DTU-percentage|Percent|Average|DTU-percentage|Er zijn geen dimensies|
|storage_percent|Percentage gebruikte gegevens ruimte||Percent|Average|Opslag percentage|Er zijn geen dimensies|
|workers_percent|Werknemerspercentage|Percent|Average|Werknemerspercentage|Er zijn geen dimensies|
|sessions_percent|Sessiepercentage|Percent|Average|Sessiepercentage|Er zijn geen dimensies|
|eDTU_limit|eDTU-limiet|Count|Average|eDTU-limiet|Er zijn geen dimensies|
|storage_limit|Maximale grootte van gegevens|Bytes|Average|Opslag limiet|Er zijn geen dimensies|
|eDTU_used|eDTU gebruikt|Count|Average|eDTU gebruikt|Er zijn geen dimensies|
|storage_used|Gebruikte gegevensruimte|Bytes|Average|Opslag gebruikt|Er zijn geen dimensies|
|xtp_storage_percent|Percentage OLTP-opslag in het geheugen|Percent|Average|Percentage OLTP-opslag in het geheugen|Er zijn geen dimensies|
|cpu_limit|CPU-limiet|Count|Average|CPU-limiet|Er zijn geen dimensies|
|cpu_used|CPU gebruikt|Count|Average|CPU gebruikt|Er zijn geen dimensies|
|allocated_data_storage|Toegewezen gegevensruimte|Bytes|Average|Toegewezen gegevensruimte|Er zijn geen dimensies|
|allocated_data_storage_percent|Percentage toegewezen gegevens ruimte|Percent|Maximum|Percentage toegewezen gegevens ruimte|Er zijn geen dimensies|

## <a name="microsoftsqlmanagedinstances"></a>Micro soft. SQL/managedInstances

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|virtual_core_count|Aantal virtuele kernen|Count|Average|Aantal virtuele kernen|Er zijn geen dimensies|
|avg_cpu_percent|Gemiddeld CPU-percentage|Percent|Average|Gemiddeld CPU-percentage|Er zijn geen dimensies|
|reserved_storage_mb|Gereserveerde opslag ruimte|Count|Average|Gereserveerde opslag ruimte|Er zijn geen dimensies|
|storage_space_used_mb|Gebruikte opslag ruimte|Count|Average|Gebruikte opslag ruimte|Er zijn geen dimensies|
|io_requests|Aantal i/o-aanvragen|Count|Average|Aantal i/o-aanvragen|Er zijn geen dimensies|
|io_bytes_read|Gelezen IO-bytes|Bytes|Average|Gelezen IO-bytes|Er zijn geen dimensies|
|io_bytes_written|Geschreven IO-bytes|Bytes|Average|Geschreven IO-bytes|Er zijn geen dimensies|

## <a name="microsoftstoragestorageaccounts"></a>Microsoft.Storage/storageAccounts

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|UsedCapacity|Gebruikte capaciteit|Bytes|Gemiddeld|Gebruikte capaciteit van account|Er zijn geen dimensies|
|Transacties|Transacties|Count|Totaal|Het aantal aanvragen voor een opslagservice of de opgegeven API-bewerking. Dit is inclusief geslaagde en mislukte aanvragen evenals aanvragen die tot fouten. Gebruik de ResponseType-dimensie voor het aantal verschillende type reactie.|ResponseType, geotype, ApiName, authenticatie|
|Inkomend verkeer|Inkomend verkeer|Bytes|Totaal|De hoeveelheid inkomende gegevens in bytes. Hieronder vallen de inkomende gegevens van een externe client in Azure Storage evenals de inkomende gegevens binnen Azure.|Geotype, ApiName, authenticatie|
|Uitgaand verkeer|Uitgaand verkeer|Bytes|Totaal|De hoeveelheid uitgaande gegevens in bytes. Hieronder vallen de uitgaande gegevens van een externe client in Azure Storage evenals de uitgaande gegevens binnen Azure. Daarom geeft deze hoeveelheid niet de factureerbare uitgaande gegevens weer.|Geotype, ApiName, authenticatie|
|SuccessServerLatency|Geslaagde serverlatentie|Milliseconden|Gemiddeld|De gemiddelde latentie die wordt gebruikt door Azure Storage voor het verwerken van een aanvraag is gelukt, in milliseconden. Deze waarde omvat niet de netwerklatentie die in AverageE2ELatency is opgegeven.|Geotype, ApiName, authenticatie|
|SuccessE2ELatency|Geslaagde E2E-latentie|Milliseconden|Gemiddeld|De gemiddelde end-to-end latentie van geslaagde aanvragen voor een opslagservice of de opgegeven API-bewerking, in milliseconden. Deze waarde bevat de vereiste verwerkingstijd in Azure Storage die nodig is om de aanvraag te lezen, het antwoord te verzenden en bevestiging van het antwoord te ontvangen.|Geotype, ApiName, authenticatie|
|Beschikbaarheid|Beschikbaarheid|Procent|Gemiddeld|Het percentage van de beschikbaarheid van de opslagservice of de opgegeven API-bewerking. Beschikbaarheid wordt berekend door de TotalBillableRequests-waarde maken en delen door het aantal toepasselijke aanvragen, met inbegrip van die onverwachte fouten zijn opgetreden. Alle onverwachte fouten leiden tot verminderde beschikbaarheid voor de opslagservice of de opgegeven API-bewerking.|Geotype, ApiName, authenticatie|

## <a name="microsoftstoragestorageaccountsblobservices"></a>Microsoft.Storage/storageAccounts/blobServices

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|BlobCapacity|Blobcapaciteit|Bytes|Average|De hoeveelheid opslag die wordt gebruikt door de Blob-service van het opslagaccount (in bytes).|BlobType, Tier|
|BlobCount|Aantal blobs|Count|Totaal|Het aantal blobs in de Blob-service van het opslagaccount.|BlobType|       |BlobCount|Aantal blobs|Count|Average|Het aantal blobs in de Blob-service van het opslagaccount.|BlobType, Tier|
|ContainerCount|Aantal blobcontainers|Count|Gemiddeld|Het aantal containers in de Blob-service van het opslagaccount.|Er zijn geen dimensies|
|IndexCapacity|Indexcapaciteit|Bytes|Average|De hoeveelheid opslagruimte gebruikt door ADLS Gen2-index (hiërarchisch) in bytes.|Er zijn geen dimensies|
|Transacties|Transacties|Count|Totaal|Het aantal aanvragen voor een opslagservice of de opgegeven API-bewerking. Dit is inclusief geslaagde en mislukte aanvragen evenals aanvragen die tot fouten. Gebruik de ResponseType-dimensie voor het aantal verschillende type reactie.|ResponseType, geotype, ApiName, authenticatie|
|Inkomend verkeer|Inkomend verkeer|Bytes|Totaal|De hoeveelheid inkomende gegevens in bytes. Hieronder vallen de inkomende gegevens van een externe client in Azure Storage evenals de inkomende gegevens binnen Azure.|Geotype, ApiName, authenticatie|
|Uitgaand verkeer|Uitgaand verkeer|Bytes|Totaal|De hoeveelheid uitgaande gegevens in bytes. Hieronder vallen de uitgaande gegevens van een externe client in Azure Storage evenals de uitgaande gegevens binnen Azure. Daarom geeft deze hoeveelheid niet de factureerbare uitgaande gegevens weer.|Geotype, ApiName, authenticatie|
|SuccessServerLatency|Geslaagde serverlatentie|Milliseconden|Gemiddeld|De gemiddelde latentie die wordt gebruikt door Azure Storage voor het verwerken van een aanvraag is gelukt, in milliseconden. Deze waarde omvat niet de netwerklatentie die in AverageE2ELatency is opgegeven.|Geotype, ApiName, authenticatie|
|SuccessE2ELatency|Geslaagde E2E-latentie|Milliseconden|Gemiddeld|De gemiddelde end-to-end latentie van geslaagde aanvragen voor een opslagservice of de opgegeven API-bewerking, in milliseconden. Deze waarde bevat de vereiste verwerkingstijd in Azure Storage die nodig is om de aanvraag te lezen, het antwoord te verzenden en bevestiging van het antwoord te ontvangen.|Geotype, ApiName, authenticatie|
|Beschikbaarheid|Beschikbaarheid|Procent|Gemiddeld|Het percentage van de beschikbaarheid van de opslagservice of de opgegeven API-bewerking. Beschikbaarheid wordt berekend door de TotalBillableRequests-waarde maken en delen door het aantal toepasselijke aanvragen, met inbegrip van die onverwachte fouten zijn opgetreden. Alle onverwachte fouten leiden tot verminderde beschikbaarheid voor de opslagservice of de opgegeven API-bewerking.|Geotype, ApiName, authenticatie|

## <a name="microsoftstoragestorageaccountsfileservices"></a>Microsoft.Storage/storageAccounts/fileServices

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|FileCapacity|Bestandscapaciteit|Bytes|Average|De hoeveelheid opslag die wordt gebruikt door de File-service van het opslagaccount (in bytes).|Er zijn geen dimensies|
|FileCount|Aantal bestanden|Count|Average|Het aantal bestanden in de File-service van het opslagaccount.|Er zijn geen dimensies|
|FileShareCount|Aantal bestandsshares|Count|Average|Het aantal bestandsshares in de File-service van het opslagaccount.|Er zijn geen dimensies|
|Transacties|Transacties|Count|Totaal|Het aantal aanvragen voor een opslagservice of de opgegeven API-bewerking. Dit is inclusief geslaagde en mislukte aanvragen evenals aanvragen die tot fouten. Gebruik de ResponseType-dimensie voor het aantal verschillende type reactie.|ResponseType, geotype, ApiName, authenticatie|
|Inkomend verkeer|Inkomend verkeer|Bytes|Totaal|De hoeveelheid inkomende gegevens in bytes. Hieronder vallen de inkomende gegevens van een externe client in Azure Storage evenals de inkomende gegevens binnen Azure.|Geotype, ApiName, authenticatie|
|Uitgaand verkeer|Uitgaand verkeer|Bytes|Totaal|De hoeveelheid uitgaande gegevens in bytes. Hieronder vallen de uitgaande gegevens van een externe client in Azure Storage evenals de uitgaande gegevens binnen Azure. Daarom geeft deze hoeveelheid niet de factureerbare uitgaande gegevens weer.|Geotype, ApiName, authenticatie|
|SuccessServerLatency|Geslaagde serverlatentie|Milliseconden|Gemiddeld|De gemiddelde latentie die wordt gebruikt door Azure Storage voor het verwerken van een aanvraag is gelukt, in milliseconden. Deze waarde omvat niet de netwerklatentie die in AverageE2ELatency is opgegeven.|Geotype, ApiName, authenticatie|
|SuccessE2ELatency|Geslaagde E2E-latentie|Milliseconden|Gemiddeld|De gemiddelde end-to-end latentie van geslaagde aanvragen voor een opslagservice of de opgegeven API-bewerking, in milliseconden. Deze waarde bevat de vereiste verwerkingstijd in Azure Storage die nodig is om de aanvraag te lezen, het antwoord te verzenden en bevestiging van het antwoord te ontvangen.|Geotype, ApiName, authenticatie|
|Beschikbaarheid|Beschikbaarheid|Procent|Gemiddeld|Het percentage van de beschikbaarheid van de opslagservice of de opgegeven API-bewerking. Beschikbaarheid wordt berekend door de TotalBillableRequests-waarde maken en delen door het aantal toepasselijke aanvragen, met inbegrip van die onverwachte fouten zijn opgetreden. Alle onverwachte fouten leiden tot verminderde beschikbaarheid voor de opslagservice of de opgegeven API-bewerking.|Geotype, ApiName, authenticatie|

## <a name="microsoftstoragestorageaccountsqueueservices"></a>Microsoft.Storage/storageAccounts/queueServices

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|QueueCapacity|Wachtrijcapaciteit|Bytes|Gemiddeld|De hoeveelheid opslag die wordt gebruikt door de Queue-service van het opslagaccount (in bytes).|Er zijn geen dimensies|
|QueueCount|Aantal wachtrijen|Count|Gemiddeld|Het aantal wachtrijen in de Queue-service van het opslagaccount.|Er zijn geen dimensies|
|QueueMessageCount|Aantal wachtrijberichten|Count|Gemiddeld|Het geschatte aantal wachtrijberichten in de Queue-service van het opslagaccount.|Er zijn geen dimensies|
|Transacties|Transacties|Count|Totaal|Het aantal aanvragen voor een opslagservice of de opgegeven API-bewerking. Dit is inclusief geslaagde en mislukte aanvragen evenals aanvragen die tot fouten. Gebruik de ResponseType-dimensie voor het aantal verschillende type reactie.|ResponseType, geotype, ApiName, authenticatie|
|Inkomend verkeer|Inkomend verkeer|Bytes|Totaal|De hoeveelheid inkomende gegevens in bytes. Hieronder vallen de inkomende gegevens van een externe client in Azure Storage evenals de inkomende gegevens binnen Azure.|Geotype, ApiName, authenticatie|
|Uitgaand verkeer|Uitgaand verkeer|Bytes|Totaal|De hoeveelheid uitgaande gegevens in bytes. Hieronder vallen de uitgaande gegevens van een externe client in Azure Storage evenals de uitgaande gegevens binnen Azure. Daarom geeft deze hoeveelheid niet de factureerbare uitgaande gegevens weer.|Geotype, ApiName, authenticatie|
|SuccessServerLatency|Geslaagde serverlatentie|Milliseconden|Gemiddeld|De gemiddelde latentie die wordt gebruikt door Azure Storage voor het verwerken van een aanvraag is gelukt, in milliseconden. Deze waarde omvat niet de netwerklatentie die in AverageE2ELatency is opgegeven.|Geotype, ApiName, authenticatie|
|SuccessE2ELatency|Geslaagde E2E-latentie|Milliseconden|Gemiddeld|De gemiddelde end-to-end latentie van geslaagde aanvragen voor een opslagservice of de opgegeven API-bewerking, in milliseconden. Deze waarde bevat de vereiste verwerkingstijd in Azure Storage die nodig is om de aanvraag te lezen, het antwoord te verzenden en bevestiging van het antwoord te ontvangen.|Geotype, ApiName, authenticatie|
|Beschikbaarheid|Beschikbaarheid|Procent|Gemiddeld|Het percentage van de beschikbaarheid van de opslagservice of de opgegeven API-bewerking. Beschikbaarheid wordt berekend door de TotalBillableRequests-waarde maken en delen door het aantal toepasselijke aanvragen, met inbegrip van die onverwachte fouten zijn opgetreden. Alle onverwachte fouten leiden tot verminderde beschikbaarheid voor de opslagservice of de opgegeven API-bewerking.|Geotype, ApiName, authenticatie|

## <a name="microsoftstoragestorageaccountstableservices"></a>Microsoft.Storage/storageAccounts/tableServices

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|TableCapacity|Tabelcapaciteit|Bytes|Gemiddeld|De hoeveelheid opslag die wordt gebruikt door de Table-service van het opslagaccount (in bytes).|Er zijn geen dimensies|
|TableCount|Aantal tabellen|Count|Gemiddeld|Het aantal tabellen in de Table-service van het opslagaccount.|Er zijn geen dimensies|
|TableEntityCount|Aantal tabelentiteiten|Count|Gemiddeld|Het aantal tabelentiteiten in de Table-service van het opslagaccount.|Er zijn geen dimensies|
|Transacties|Transacties|Count|Totaal|Het aantal aanvragen voor een opslagservice of de opgegeven API-bewerking. Dit is inclusief geslaagde en mislukte aanvragen evenals aanvragen die tot fouten. Gebruik de ResponseType-dimensie voor het aantal verschillende type reactie.|ResponseType, geotype, ApiName, authenticatie|
|Inkomend verkeer|Inkomend verkeer|Bytes|Totaal|De hoeveelheid inkomende gegevens in bytes. Hieronder vallen de inkomende gegevens van een externe client in Azure Storage evenals de inkomende gegevens binnen Azure.|Geotype, ApiName, authenticatie|
|Uitgaand verkeer|Uitgaand verkeer|Bytes|Totaal|De hoeveelheid uitgaande gegevens in bytes. Hieronder vallen de uitgaande gegevens van een externe client in Azure Storage evenals de uitgaande gegevens binnen Azure. Daarom geeft deze hoeveelheid niet de factureerbare uitgaande gegevens weer.|Geotype, ApiName, authenticatie|
|SuccessServerLatency|Geslaagde serverlatentie|Milliseconden|Gemiddeld|De gemiddelde latentie die wordt gebruikt door Azure Storage voor het verwerken van een aanvraag is gelukt, in milliseconden. Deze waarde omvat niet de netwerklatentie die in AverageE2ELatency is opgegeven.|Geotype, ApiName, authenticatie|
|SuccessE2ELatency|Geslaagde E2E-latentie|Milliseconden|Gemiddeld|De gemiddelde end-to-end latentie van geslaagde aanvragen voor een opslagservice of de opgegeven API-bewerking, in milliseconden. Deze waarde bevat de vereiste verwerkingstijd in Azure Storage die nodig is om de aanvraag te lezen, het antwoord te verzenden en bevestiging van het antwoord te ontvangen.|Geotype, ApiName, authenticatie|
|Beschikbaarheid|Beschikbaarheid|Procent|Gemiddeld|Het percentage van de beschikbaarheid van de opslagservice of de opgegeven API-bewerking. Beschikbaarheid wordt berekend door de TotalBillableRequests-waarde maken en delen door het aantal toepasselijke aanvragen, met inbegrip van die onverwachte fouten zijn opgetreden. Alle onverwachte fouten leiden tot verminderde beschikbaarheid voor de opslagservice of de opgegeven API-bewerking.|Geotype, ApiName, authenticatie|

## <a name="microsoftstoragesyncstoragesyncservices"></a>microsoft.storagesync/storageSyncServices

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|ServerSyncSessionResult|Resultaat van synchronisatie sessie|Count|Average|Metriek die een waarde van 1 registreert telkens wanneer het server eindpunt een synchronisatie sessie met het Cloud eindpunt heeft voltooid|SyncGroupName, ServerEndpointName, SyncDirection|
|StorageSyncSyncSessionAppliedFilesCount|Gesynchroniseerde bestanden|Count|Totaal|Aantal gesynchroniseerde bestanden|SyncGroupName, ServerEndpointName, SyncDirection|
|StorageSyncSyncSessionPerItemErrorsCount|Bestanden die niet worden gesynchroniseerd|Count|Totaal|Aantal bestanden dat niet kan worden gesynchroniseerd|SyncGroupName, ServerEndpointName, SyncDirection|
|StorageSyncBatchTransferredFileBytes|Gesynchroniseerde bytes|Bytes|Totaal|Totale bestands grootte die is overgedragen voor synchronisatie sessies|SyncGroupName, ServerEndpointName, SyncDirection|
|StorageSyncServerHeartbeat|Onlinestatus van de server|Count|Maximum|Metriek die een waarde van 1 registreert telkens wanneer de geregistreerde server een heartbeat met het Cloud eindpunt heeft vastgelegd|ServerName|
|StorageSyncRecallIOTotalSizeBytes|Cloud lagen intrekken|Bytes|Totaal|De totale grootte van de gegevens die door de server zijn ingetrokken|ServerName|

## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft.StreamAnalytics/streamingjobs

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|ResourceUtilization|Gebruikspercentage voor Streaming-eenheden|Percent|Maximum|Gebruikspercentage voor Streaming-eenheden|LogicalName, PartitionId|
|InputEvents|Invoergebeurtenissen|Count|Totaal|Invoergebeurtenissen|LogicalName, PartitionId|
|InputEventBytes|Invoergebeurtenisbytes|Bytes|Totaal|Invoergebeurtenisbytes|LogicalName, PartitionId|
|LateInputEvents|Late invoergebeurtenissen|Count|Totaal|Late invoergebeurtenissen|LogicalName, PartitionId|
|OutputEvents|Uitvoergebeurtenis|Count|Totaal|Uitvoergebeurtenis|LogicalName, PartitionId|
|ConversionErrors|Gegevensconversiefouten|Count|Totaal|Gegevensconversiefouten|LogicalName, PartitionId|
|Fouten|Runtimefouten|Count|Totaal|Runtimefouten|LogicalName, PartitionId|
|DroppedOrAdjustedEvents|Gebeurtenissen met een andere volgorde|Count|Totaal|Gebeurtenissen met een andere volgorde|LogicalName, PartitionId|
|AMLCalloutRequests|Functieaanvragen|Count|Totaal|Functieaanvragen|LogicalName, PartitionId|
|AMLCalloutFailedRequests|Mislukte functieaanvragen|Count|Totaal|Mislukte functieaanvragen|LogicalName, PartitionId|
|AMLCalloutInputEvents|Functiegebeurtenissen|Count|Totaal|Functiegebeurtenissen|LogicalName, PartitionId|
|DeserializationError|Fouten in invoerdeserialisatie|Count|Totaal|Fouten in invoerdeserialisatie|LogicalName, PartitionId|
|EarlyInputEvents|Vroege-invoergebeurtenissen|Count|Totaal|Vroege-invoergebeurtenissen|LogicalName, PartitionId|
|OutputWatermarkDelaySeconds|Watermerkvertraging|Seconden|Maximum|Watermerkvertraging|LogicalName, PartitionId|
|InputEventsSourcesBacklogged|Invoervelden met achterstand|Count|Maximum|Invoervelden met achterstand|LogicalName, PartitionId|
|InputEventsSourcesPerSecond|Ontvangen invoerbronnen|Count|Totaal|Ontvangen invoerbronnen|LogicalName, PartitionId|

## <a name="microsofttimeseriesinsightsenvironments"></a>Microsoft.TimeSeriesInsights/environments

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|IngressReceivedMessages|Ontvangen berichten met ingang|Count|Totaal|Telling van berichten die zijn gelezen uit alle gebeurtenis bronnen van Event hub of IoT hub|Er zijn geen dimensies|
|IngressReceivedInvalidMessages|Ongeldige berichten ontvangen|Count|Totaal|Aantal ongeldige berichten gelezen uit alle gebeurtenis bronnen van Event hub of IoT hub|Er zijn geen dimensies|
|IngressReceivedBytes|Ontvangen bytes van binnenkomend verkeer|Bytes|Totaal|Het aantal gelezen bytes van alle gebeurtenis bronnen|Er zijn geen dimensies|
|IngressStoredBytes|In ingangs opgeslagen bytes|Bytes|Totaal|De totale grootte van de gebeurtenissen die zijn verwerkt en beschikbaar voor de query|Er zijn geen dimensies|
|IngressStoredEvents|Opgeslagen gebeurtenissen in ingangs|Count|Totaal|Aantal samengevoegde gebeurtenissen dat is verwerkt en beschikbaar is voor query|Er zijn geen dimensies|
|IngressReceivedMessagesTimeLag|Tijds vertraging ontvangen inkomende berichten|Seconden|Maximum|Verschil tussen het tijdstip waarop het bericht in de bron van de gebeurtenis in de wachtrij staat en de tijd die wordt verwerkt in ingangs|Er zijn geen dimensies|
|IngressReceivedMessagesCountLag|Vertraging ontvangen inkomende berichten van ingang|Count|Average|Verschil tussen het Volg nummer van de laatste bericht in de bron partitie en het Volg nummer van het bericht dat wordt verwerkt in ingangs gebeurtenissen|Er zijn geen dimensies|
|WarmStorageMaxProperties|Maximale eigenschappen van warme opslag|Count|Maximum|Maximum aantal eigenschappen dat is toegestaan door de omgeving voor de SKU van S1/S2 en het maximum aantal eigenschappen dat is toegestaan door de warme Store voor PAYG SKU|Er zijn geen dimensies|
|WarmStorageUsedProperties|Eigenschappen voor warme opslag gebruikt |Count|Maximum|Aantal eigenschappen dat wordt gebruikt door de omgeving voor de SKU van S1/S2 en het aantal eigenschappen dat door warme Store voor PAYG SKU wordt gebruikt|Er zijn geen dimensies|

## <a name="microsofttimeseriesinsightsenvironmentseventsources"></a>Microsoft.TimeSeriesInsights/environments/eventsources

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|IngressReceivedMessages|Ontvangen berichten met ingang|Count|Totaal|Aantal berichten dat uit de gebeurtenis bron is gelezen|Er zijn geen dimensies|
|IngressReceivedInvalidMessages|Ongeldige berichten ontvangen|Count|Totaal|Aantal ongeldige berichten gelezen uit de gebeurtenis bron|Er zijn geen dimensies|
|IngressReceivedBytes|Ontvangen bytes van binnenkomend verkeer|Bytes|Totaal|Aantal gelezen bytes van de gebeurtenis bron|Er zijn geen dimensies|
|IngressStoredBytes|In ingangs opgeslagen bytes|Bytes|Totaal|De totale grootte van de gebeurtenissen die zijn verwerkt en beschikbaar voor de query|Er zijn geen dimensies|
|IngressStoredEvents|Opgeslagen gebeurtenissen in ingangs|Count|Totaal|Aantal samengevoegde gebeurtenissen dat is verwerkt en beschikbaar is voor query|Er zijn geen dimensies|
|IngressReceivedMessagesTimeLag|Tijds vertraging ontvangen inkomende berichten|Seconden|Maximum|Verschil tussen het tijdstip waarop het bericht in de bron van de gebeurtenis in de wachtrij staat en de tijd die wordt verwerkt in ingangs|Er zijn geen dimensies|
|IngressReceivedMessagesCountLag|Vertraging ontvangen inkomende berichten van ingang|Count|Average|Verschil tussen het Volg nummer van de laatste bericht in de bron partitie en het Volg nummer van het bericht dat wordt verwerkt in ingangs gebeurtenissen|Er zijn geen dimensies|
|WarmStorageMaxProperties|Maximale eigenschappen van warme opslag|Count|Maximum|Maximum aantal eigenschappen dat is toegestaan door de omgeving voor de SKU van S1/S2 en het maximum aantal eigenschappen dat is toegestaan door de warme Store voor PAYG SKU|Er zijn geen dimensies|
|WarmStorageUsedProperties|Eigenschappen voor warme opslag gebruikt |Count|Maximum|Aantal eigenschappen dat wordt gebruikt door de omgeving voor de SKU van S1/S2 en het aantal eigenschappen dat door warme Store voor PAYG SKU wordt gebruikt|Er zijn geen dimensies|

## <a name="microsoftvmwarecloudsimplevirtualmachines"></a>Microsoft.VMwareCloudSimple/virtualMachines

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|DiskReadBytesPerSecond|Gelezen bytes per seconde op de schijf|BytesPerSecond|Average|Gemiddelde door Voer van schijf vanwege Lees bewerkingen gedurende de voorbeeld periode.|Er zijn geen dimensies|
|DiskWriteBytesPerSecond|Geschreven bytes per seconde|BytesPerSecond|Average|Gemiddelde doorvoer snelheid van schijven vanwege schrijf bewerkingen gedurende de voorbeeld periode.|Er zijn geen dimensies|
|Gelezen bytes op de schijf|Gelezen bytes op de schijf|Bytes|Totaal|Totale schijf doorvoer vanwege Lees bewerkingen gedurende de voorbeeld periode.|Er zijn geen dimensies|
|Geschreven bytes op de schijf|Geschreven bytes op de schijf|Bytes|Totaal|Totale schijf doorvoer vanwege schrijf bewerkingen gedurende de voorbeeld periode.|Er zijn geen dimensies|
|DiskReadOperations|Lees bewerkingen op de schijf|Count|Totaal|Het aantal i/o-Lees bewerkingen in de vorige voorbeeld periode. Houd er rekening mee dat deze bewerkingen variabele grootte kunnen hebben.|Er zijn geen dimensies|
|DiskWriteOperations|Schrijf bewerkingen op de schijf|Count|Totaal|Het aantal i/o-schrijf bewerkingen in de vorige voorbeeld periode. Houd er rekening mee dat deze bewerkingen variabele grootte kunnen hebben.|Er zijn geen dimensies|
|Leesbewerkingen op de schijf/seconde|Leesbewerkingen op de schijf/seconde|CountPerSecond|Average|Het gemiddelde aantal i/o-Lees bewerkingen in de vorige voorbeeld periode. Houd er rekening mee dat deze bewerkingen variabele grootte kunnen hebben.|Er zijn geen dimensies|
|Schrijfbewerkingen op de schijf/seconde|Schrijfbewerkingen op de schijf/seconde|CountPerSecond|Average|Het gemiddelde aantal i/o-schrijf bewerkingen in de vorige voorbeeld periode. Houd er rekening mee dat deze bewerkingen variabele grootte kunnen hebben.|Er zijn geen dimensies|
|DiskReadLatency|Lees latentie van schijf|Milliseconden|Average|Totale lees latentie. De som van de lees latentie van het apparaat en de kernel.|Er zijn geen dimensies|
|DiskWriteLatency|Schrijf latentie schijf|Milliseconden|Average|Totale schrijf latentie. De som van de latentie voor het schrijven van apparaten en kernels.|Er zijn geen dimensies|
|NetworkInBytesPerSecond|Netwerk in bytes per seconde|BytesPerSecond|Average|Gemiddelde netwerk doorvoer voor ontvangen verkeer.|Er zijn geen dimensies|
|NetworkOutBytesPerSecond|Netwerk uitgaande bytes per seconde|BytesPerSecond|Average|Gemiddelde netwerk doorvoer voor verzonden verkeer.|Er zijn geen dimensies|
|Netwerk in|Netwerk in|Bytes|Totaal|Totale netwerk doorvoer voor ontvangen verkeer.|Er zijn geen dimensies|
|Netwerk uit|Netwerk uit|Bytes|Totaal|Totale netwerk doorvoer voor verzonden verkeer.|Er zijn geen dimensies|
|MemoryUsed|Gebruikt geheugen|Bytes|Average|De hoeveelheid machine geheugen die wordt gebruikt door de VM.|Er zijn geen dimensies|
|MemoryGranted|Toegewezen geheugen|Bytes|Average|De hoeveelheid geheugen die door de host is toegewezen aan de virtuele machine. Er wordt geen geheugen verleend aan de host totdat deze één keer wordt aangevallen en het toegewezen geheugen kan worden uitgewisseld of geballon weg als de VMkernel het geheugen nodig heeft.|Er zijn geen dimensies|
|MemoryActive|Actief geheugen|Bytes|Average|De hoeveelheid geheugen die wordt gebruikt door de virtuele machine in het verleden kleine tijd venster. Dit is het ' True ' nummer van de hoeveelheid geheugen die momenteel nodig is voor de VM. Extra, ongebruikt geheugen kan worden uitgewisseld of geballond zonder gevolgen voor de prestaties van de gast.|Er zijn geen dimensies|
|Percentage CPU|Percentage CPU|Procent|Average|Het CPU-gebruik. Deze waarde wordt gerapporteerd met 100% voor alle processor kernen op het systeem. Een voor beeld: een twee richtings-VM met 50% van een systeem met vier kern geheugens is volledig met twee kernen.|Er zijn geen dimensies|
|PercentageCpuReady|Percentage CPU gereed|Milliseconden|Totaal|Beschik bare tijd is de tijd die nodig is voor het wachten op CPU ('s) in het afgelopen update-interval.|Er zijn geen dimensies|

## <a name="microsoftwebserverfarms"></a>Microsoft.Web/serverfarms

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|CpuPercentage|CPU-percentage|Percent|Average|CPU-percentage|Exemplaar|
|MemoryPercentage|Geheugenpercentage|Percent|Average|Geheugenpercentage|Exemplaar|
|DiskQueueLength|Lengte van de wachtrij voor de schijf|Count|Average|Lengte van de wachtrij voor de schijf|Exemplaar|
|HttpQueueLength|Lengte van de HTTP-wachtrij|Count|Average|Lengte van de HTTP-wachtrij|Exemplaar|
|BytesReceived|Inkomende gegevens|Bytes|Totaal|Inkomende gegevens|Exemplaar|
|BytesSent|Uitgaande gegevens|Bytes|Totaal|Uitgaande gegevens|Exemplaar|

## <a name="microsoftwebsites-excluding-functions"></a>Micro soft. web/sites (met uitzonde ring van functies)

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|CpuTime|CPU-tijd|Seconden|Totaal|CPU-tijd|Exemplaar|
|Aanvragen|Aanvragen|Count|Totaal|Aanvragen|Exemplaar|
|BytesReceived|Inkomende gegevens|Bytes|Totaal|Inkomende gegevens|Exemplaar|
|BytesSent|Uitgaande gegevens|Bytes|Totaal|Uitgaande gegevens|Exemplaar|
|Http101|HTTP-fout 101|Count|Totaal|HTTP-fout 101|Exemplaar|
|Http2xx|HTTP 2xx|Count|Totaal|HTTP 2xx|Exemplaar|
|Http3xx|HTTP 3xx|Count|Totaal|HTTP 3xx|Exemplaar|
|Http401|HTTP 401|Count|Totaal|HTTP 401|Exemplaar|
|Http403|HTTP-fout 403|Count|Totaal|HTTP-fout 403|Exemplaar|
|Http404|HTTP-fout 404|Count|Totaal|HTTP-fout 404|Exemplaar|
|Http406|HTTP-fout 406|Count|Totaal|HTTP-fout 406|Exemplaar|
|Http4xx|Http 4xx|Count|Totaal|Http 4xx|Exemplaar|
|Http5xx|HTTP-serverfouten|Count|Totaal|HTTP-serverfouten|Exemplaar|
|MemoryWorkingSet|Geheugenwerkset|Bytes|Average|Geheugenwerkset|Exemplaar|
|AverageMemoryWorkingSet|Gemiddeld geheugenwerkset|Bytes|Average|Gemiddeld geheugenwerkset|Exemplaar|
|AverageResponseTime|Gemiddelde reactietijd|Seconden|Average|Gemiddelde reactietijd|Exemplaar|
|AppConnections|Verbindingen|Count|Average|Verbindingen|Exemplaar|
|Ingangen|Aantal ingangen|Count|Average|Aantal ingangen|Exemplaar|
|Threads|Aantal threads|Count|Average|Aantal threads|Exemplaar|
|PrivateBytes|Privébytes|Bytes|Average|Privébytes|Exemplaar|
|IoReadBytesPerSecond|I/O gelezen bytes per seconde|BytesPerSecond|Totaal|I/O gelezen bytes per seconde|Exemplaar|
|IoWriteBytesPerSecond|I/O geschreven bytes per seconde|BytesPerSecond|Totaal|I/O geschreven bytes per seconde|Exemplaar|
|IoOtherBytesPerSecond|I/O overige bytes per seconde|BytesPerSecond|Totaal|I/O overige bytes per seconde|Exemplaar|
|IoReadOperationsPerSecond|I/O gelezen bewerkingen per seconde|BytesPerSecond|Totaal|I/O gelezen bewerkingen per seconde|Exemplaar|
|IoWriteOperationsPerSecond|I/O geschreven bewerkingen per seconde|BytesPerSecond|Totaal|I/O geschreven bewerkingen per seconde|Exemplaar|
|IoOtherOperationsPerSecond|I/O overige bewerkingen per seconde|BytesPerSecond|Totaal|I/O overige bewerkingen per seconde|Exemplaar|
|RequestsInApplicationQueue|Aanvragen in de toepassingswachtrij|Count|Average|Aanvragen in de toepassingswachtrij|Exemplaar|
|CurrentAssemblies|Huidige assembly's|Count|Average|Huidige assembly's|Exemplaar|
|TotalAppDomains|Totaal aantal appdomeinen|Count|Average|Totaal aantal appdomeinen|Exemplaar|
|TotalAppDomainsUnloaded|Totaal aantal appdomeinen verwijderd|Count|Average|Totaal aantal appdomeinen verwijderd|Exemplaar|
|Gen0Collections|Garbagecollections van generatie 0|Count|Totaal|Garbagecollections van generatie 0|Exemplaar|
|Gen1Collections|Garbagecollections van generatie 1|Count|Totaal|Garbagecollections van generatie 1|Exemplaar|
|Gen2Collections|Garbagecollections van generatie 2|Count|Totaal|Garbagecollections van generatie 2|Exemplaar|

## <a name="microsoftwebsites-functions"></a>Micro soft. web/sites (functies)

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|BytesReceived|Inkomende gegevens|Bytes|Totaal|Inkomende gegevens|Exemplaar|
|BytesSent|Uitgaande gegevens|Bytes|Totaal|Uitgaande gegevens|Exemplaar|
|Http5xx|HTTP-serverfouten|Count|Totaal|HTTP-serverfouten|Exemplaar|
|MemoryWorkingSet|Geheugenwerkset|Bytes|Average|Geheugenwerkset|Exemplaar|
|AverageMemoryWorkingSet|Gemiddeld geheugenwerkset|Bytes|Average|Gemiddeld geheugenwerkset|Exemplaar|
|FunctionExecutionUnits|Functie-uitvoeringseenheden|MB/milliseconden|Totaal|[Eenheden voor functie-uitvoering](https://github.com/Azure/Azure-Functions/wiki/Consumption-Plan-Cost-Billing-FAQ#how-can-i-view-graphs-of-execution-count-and-gb-seconds)|Exemplaar|
|FunctionExecutionCount|Aantal uitvoeringen van functie|Count|Totaal|Aantal uitvoeringen van functie|Exemplaar|
|PrivateBytes|Privébytes|Bytes|Average|Privébytes|Exemplaar|
|IoReadBytesPerSecond|I/O gelezen bytes per seconde|BytesPerSecond|Totaal|I/O gelezen bytes per seconde|Exemplaar|
|IoWriteBytesPerSecond|I/O geschreven bytes per seconde|BytesPerSecond|Totaal|I/O geschreven bytes per seconde|Exemplaar|
|IoOtherBytesPerSecond|I/O overige bytes per seconde|BytesPerSecond|Totaal|I/O overige bytes per seconde|Exemplaar|
|IoReadOperationsPerSecond|I/O gelezen bewerkingen per seconde|BytesPerSecond|Totaal|I/O gelezen bewerkingen per seconde|Exemplaar|
|IoWriteOperationsPerSecond|I/O geschreven bewerkingen per seconde|BytesPerSecond|Totaal|I/O geschreven bewerkingen per seconde|Exemplaar|
|IoOtherOperationsPerSecond|I/O overige bewerkingen per seconde|BytesPerSecond|Totaal|I/O overige bewerkingen per seconde|Exemplaar|
|RequestsInApplicationQueue|Aanvragen in de toepassingswachtrij|Count|Average|Aanvragen in de toepassingswachtrij|Exemplaar|
|CurrentAssemblies|Huidige assembly's|Count|Average|Huidige assembly's|Exemplaar|
|TotalAppDomains|Totaal aantal appdomeinen|Count|Average|Totaal aantal appdomeinen|Exemplaar|
|TotalAppDomainsUnloaded|Totaal aantal appdomeinen verwijderd|Count|Average|Totaal aantal appdomeinen verwijderd|Exemplaar|
|Gen0Collections|Garbagecollections van generatie 0|Count|Totaal|Garbagecollections van generatie 0|Exemplaar|
|Gen1Collections|Garbagecollections van generatie 1|Count|Totaal|Garbagecollections van generatie 1|Exemplaar|
|Gen2Collections|Garbagecollections van generatie 2|Count|Totaal|Garbagecollections van generatie 2|Exemplaar|

## <a name="microsoftwebsitesslots"></a>Microsoft.Web/sites/slots

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|CpuTime|CPU-tijd|Seconden|Totaal|CPU-tijd|Exemplaar|
|Aanvragen|Aanvragen|Count|Totaal|Aanvragen|Exemplaar|
|BytesReceived|Inkomende gegevens|Bytes|Totaal|Inkomende gegevens|Exemplaar|
|BytesSent|Uitgaande gegevens|Bytes|Totaal|Uitgaande gegevens|Exemplaar|
|Http101|HTTP-fout 101|Count|Totaal|HTTP-fout 101|Exemplaar|
|Http2xx|HTTP 2xx|Count|Totaal|HTTP 2xx|Exemplaar|
|Http3xx|HTTP 3xx|Count|Totaal|HTTP 3xx|Exemplaar|
|Http401|HTTP 401|Count|Totaal|HTTP 401|Exemplaar|
|Http403|HTTP-fout 403|Count|Totaal|HTTP-fout 403|Exemplaar|
|Http404|HTTP-fout 404|Count|Totaal|HTTP-fout 404|Exemplaar|
|Http406|HTTP-fout 406|Count|Totaal|HTTP-fout 406|Exemplaar|
|Http4xx|Http 4xx|Count|Totaal|Http 4xx|Exemplaar|
|Http5xx|HTTP-serverfouten|Count|Totaal|HTTP-serverfouten|Exemplaar|
|MemoryWorkingSet|Geheugenwerkset|Bytes|Average|Geheugenwerkset|Exemplaar|
|AverageMemoryWorkingSet|Gemiddeld geheugenwerkset|Bytes|Average|Gemiddeld geheugenwerkset|Exemplaar|
|AverageResponseTime|Gemiddelde reactietijd|Seconden|Average|Gemiddelde reactietijd|Exemplaar|
|FunctionExecutionUnits|Functie-uitvoeringseenheden|Count|Totaal|Functie-uitvoeringseenheden|Exemplaar|
|FunctionExecutionCount|Aantal uitvoeringen van functie|Count|Totaal|Aantal uitvoeringen van functie|Exemplaar|
|AppConnections|Verbindingen|Count|Average|Verbindingen|Exemplaar|
|Ingangen|Aantal ingangen|Count|Average|Aantal ingangen|Exemplaar|
|Threads|Aantal threads|Count|Average|Aantal threads|Exemplaar|
|PrivateBytes|Privébytes|Bytes|Average|Privébytes|Exemplaar|
|IoReadBytesPerSecond|I/O gelezen bytes per seconde|BytesPerSecond|Totaal|I/O gelezen bytes per seconde|Exemplaar|
|IoWriteBytesPerSecond|I/O geschreven bytes per seconde|BytesPerSecond|Totaal|I/O geschreven bytes per seconde|Exemplaar|
|IoOtherBytesPerSecond|I/O overige bytes per seconde|BytesPerSecond|Totaal|I/O overige bytes per seconde|Exemplaar|
|IoReadOperationsPerSecond|I/O gelezen bewerkingen per seconde|BytesPerSecond|Totaal|I/O gelezen bewerkingen per seconde|Exemplaar|
|IoWriteOperationsPerSecond|I/O geschreven bewerkingen per seconde|BytesPerSecond|Totaal|I/O geschreven bewerkingen per seconde|Exemplaar|
|IoOtherOperationsPerSecond|I/O overige bewerkingen per seconde|BytesPerSecond|Totaal|I/O overige bewerkingen per seconde|Exemplaar|
|RequestsInApplicationQueue|Aanvragen in de toepassingswachtrij|Count|Average|Aanvragen in de toepassingswachtrij|Exemplaar|
|CurrentAssemblies|Huidige assembly's|Count|Average|Huidige assembly's|Exemplaar|
|TotalAppDomains|Totaal aantal appdomeinen|Count|Average|Totaal aantal appdomeinen|Exemplaar|
|TotalAppDomainsUnloaded|Totaal aantal appdomeinen verwijderd|Count|Average|Totaal aantal appdomeinen verwijderd|Exemplaar|
|Gen0Collections|Garbagecollections van generatie 0|Count|Totaal|Garbagecollections van generatie 0|Exemplaar|
|Gen1Collections|Garbagecollections van generatie 1|Count|Totaal|Garbagecollections van generatie 1|Exemplaar|
|Gen2Collections|Garbagecollections van generatie 2|Count|Totaal|Garbagecollections van generatie 2|Exemplaar|

## <a name="microsoftwebhostingenvironmentsmultirolepools"></a>Microsoft.Web/hostingEnvironments/multiRolePools

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|Aanvragen|Aanvragen|Count|Totaal|Aanvragen|Exemplaar|
|BytesReceived|Inkomende gegevens|Bytes|Totaal|Inkomende gegevens|Exemplaar|
|BytesSent|Uitgaande gegevens|Bytes|Totaal|Uitgaande gegevens|Exemplaar|
|Http101|HTTP-fout 101|Count|Totaal|HTTP-fout 101|Exemplaar|
|Http2xx|HTTP 2xx|Count|Totaal|HTTP 2xx|Exemplaar|
|Http3xx|HTTP 3xx|Count|Totaal|HTTP 3xx|Exemplaar|
|Http401|HTTP 401|Count|Totaal|HTTP 401|Exemplaar|
|Http403|HTTP-fout 403|Count|Totaal|HTTP-fout 403|Exemplaar|
|Http404|HTTP-fout 404|Count|Totaal|HTTP-fout 404|Exemplaar|
|Http406|HTTP-fout 406|Count|Totaal|HTTP-fout 406|Exemplaar|
|Http4xx|Http 4xx|Count|Totaal|Http 4xx|Exemplaar|
|Http5xx|HTTP-serverfouten|Count|Totaal|HTTP-serverfouten|Exemplaar|
|AverageResponseTime|Gemiddelde reactietijd|Seconden|Average|Gemiddelde reactietijd|Exemplaar|
|CpuPercentage|CPU-percentage|Percent|Average|CPU-percentage|Exemplaar|
|MemoryPercentage|Geheugenpercentage|Percent|Average|Geheugenpercentage|Exemplaar|
|DiskQueueLength|Lengte van de wachtrij voor de schijf|Count|Average|Lengte van de wachtrij voor de schijf|Exemplaar|
|HttpQueueLength|Lengte van de HTTP-wachtrij|Count|Average|Lengte van de HTTP-wachtrij|Exemplaar|
|ActiveRequests|Actieve aanvragen|Count|Totaal|Actieve aanvragen|Exemplaar|
|TotalFrontEnds|Totaalaantal front-ends|Count|Average|Totaalaantal front-ends|Er zijn geen dimensies|
|SmallAppServicePlanInstances|Werkrollen kleine App Plan Service-plan|Count|Average|Werkrollen kleine App Plan Service-plan|Er zijn geen dimensies|
|MediumAppServicePlanInstances|Werkrollen middelgrote App Service-plan|Count|Average|Werkrollen middelgrote App Service-plan|Er zijn geen dimensies|
|LargeAppServicePlanInstances|Werkrollen grote App Service-plan|Count|Average|Werkrollen grote App Service-plan|Er zijn geen dimensies|

## <a name="microsoftwebhostingenvironmentsworkerpools"></a>Microsoft.Web/hostingEnvironments/workerPools

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|WorkersTotal|Totaalaantal werkrollen|Count|Average|Totaalaantal werkrollen|Er zijn geen dimensies|
|WorkersAvailable|Beschikbare werkrollen|Count|Average|Beschikbare werkrollen|Er zijn geen dimensies|
|WorkersUsed|Gebruikte werkrollen|Count|Average|Gebruikte werkrollen|Er zijn geen dimensies|
|CpuPercentage|CPU-percentage|Percent|Average|CPU-percentage|Exemplaar|
|MemoryPercentage|Geheugenpercentage|Percent|Average|Geheugenpercentage|Exemplaar|

## <a name="next-steps"></a>Volgende stappen
* [Meer informatie over metrische gegevens in Azure Monitor](data-platform.md)
* [Waarschuwingen maken voor metrische gegevens](alerts-overview.md)
* [Metrische gegevens exporteren naar opslag, Event hub of Log Analytics](diagnostic-logs-overview.md)
