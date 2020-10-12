---
title: Azure Monitor ondersteunde metrische gegevens per resource type
description: Een lijst met metrische gegevens die beschikbaar zijn voor elk resource type met Azure Monitor.
author: rboucher
services: azure-monitor
ms.topic: reference
ms.date: 07/16/2020
ms.author: robb
ms.subservice: metrics
ms.openlocfilehash: 59df49d320b23686a3d053335ea2b95e98125b28
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "88135552"
---
# <a name="supported-metrics-with-azure-monitor"></a>Ondersteunde metrische gegevens met Azure Monitor

> [!NOTE]
> Deze lijst wordt grotendeels automatisch gegenereerd op basis van de Azure Monitor meet waarden REST API. Alle wijzigingen die in deze lijst via GitHub zijn aangebracht, kunnen zonder waarschuwing worden geschreven. Neem contact op met de auteur van dit artikel voor meer informatie over het maken van permanente updates.

Azure Monitor biedt verschillende manieren om te communiceren met metrische gegevens, zoals het maken van grafieken in de portal, het openen ervan via de REST API of het opvragen van query's via Power shell of CLI. 

Dit artikel is een volledige lijst met alle platformen (dat wil zeggen, automatisch verzamelde) gegevens die momenteel beschikbaar zijn met de geconsolideerde metrische pijp lijn van Azure Monitor. De lijst is voor het laatst bijgewerkt op 27 maart 2020. De metrische gegevens die na deze datum zijn gewijzigd of toegevoegd, worden mogelijk niet hieronder weer gegeven. Als u de lijst met metrische gegevens programmatisch wilt opvragen en openen, gebruikt u de [2018-01-01 API-versie](/rest/api/monitor/metricdefinitions). Andere metrische gegevens die niet in deze lijst staan, zijn mogelijk niet beschikbaar in de portal of met behulp van verouderde Api's.

De metrische gegevens zijn geordend op resource providers en resource type. Zie [resource providers voor Azure-Services](../../azure-resource-manager/management/azure-services-resource-providers.md)voor een lijst met Services en de resource providers die bij hen horen. 

## <a name="exporting-platform-metrics-to-other-locations"></a>Platform metrieken exporteren naar andere locaties

U kunt de platform metrieken van de Azure monitor-pijp lijn naar andere locaties op een van de twee manieren exporteren.
1. De [metrische gegevens rest API](/rest/api/monitor/metrics/list) gebruiken
2. [Diagnostische instellingen](diagnostic-settings.md) gebruiken om platform metrieken te routeren naar 
    - Azure Storage
    - Azure Monitor Logboeken (en dus Log Analytics)
    - Event hubs, waarmee u ze kunt ophalen voor niet-micro soft-systemen 

Het gebruik van diagnostische instellingen is de eenvoudigste manier om de metrische gegevens te routeren, maar er zijn enkele beperkingen: 

- **Sommige niet-Exporteer** bare gegevens zijn exporteerbaar met behulp van de rest API, maar sommige kunnen niet worden geëxporteerd met Diagnostische instellingen vanwege complexiteit in de Azure monitor back-end. De kolom die kan worden *geëxporteerd via Diagnostische instellingen* in de onderstaande tabellen, waarmee metrische gegevens op deze manier kunnen worden geëxporteerd.  

- **Multidimensionale metrische gegevens** : het verzenden van multi-dimensionale metrische gegevens naar andere locaties via Diagnostische instellingen wordt momenteel niet ondersteund. Metrische gegevens met dimensies worden geëxporteerd als platte eendimensionale metrische gegevens, als totaal van alle dimensiewaarden. *Een voorbeeld*: de meetwaarde 'Binnenkomende berichten' voor een Event Hub kan worden verkend en uitgezet op wachtrijniveau. Wanneer de waarde wordt geëxporteerd via diagnostische instellingen, wordt deze echter voorgesteld als alle binnenkomende berichten voor alle wachtrijen in de Event Hub.

## <a name="guest-os-and-host-os-metrics"></a>Metrische gegevens van het gast besturingssysteem en het hostapparaat van het besturings systeem

> [!WARNING]
> De metrische gegevens voor het gast besturingssysteem (gast besturingssysteem) die worden uitgevoerd in azure Virtual Machines, Service Fabric en Cloud Services, worden hier **niet** weer gegeven. De metrische gegevens van het gast besturingssysteem moeten worden verzameld via een of meer agents die worden uitgevoerd op of als onderdeel van het gast besturingssysteem.  De metrische gegevens voor het gast besturingssysteem zijn onder andere prestatie meter items die het CPU-percentage van de gast of het geheugen gebruik bijhouden, die vaak worden gebruikt voor automatisch schalen of waarschuwing. 
>
> **Metrische besturingssysteem gegevens zijn beschikbaar en worden hieronder weer gegeven.** Ze zijn niet hetzelfde. De metrische gegevens van het besturings systeem van de host hebben betrekking op de Hyper-V-sessie die als host fungeert voor uw gast besturingssysteem sessie. 

> [!TIP]
> Aanbevolen procedure is om de [Azure Diagnostics-extensie](diagnostics-extension-overview.md) te gebruiken en te configureren voor het verzenden van metrische gegevens over de prestaties van een gast besturingssysteem in dezelfde Azure monitor data base waar de metrische gegevens van het platform worden opgeslagen. De uitbrei ding routeert de metrische gegevens van het gast besturingssysteem via de API voor [aangepaste metrische gegevens](metrics-custom-overview.md) . Vervolgens kunt u een grafiek, waarschuwing en andere metrische gegevens van het gast besturingssysteem gebruiken, zoals de metrische gegevens van het platform. U kunt ook de Log Analytics-agent gebruiken om de metrische gegevens van het gast besturingssysteem naar Azure Monitor logs/Log Analytics te verzenden. U kunt in combi natie met niet-metrische gegevens een query uitvoeren op deze metrische waarden. 

Zie [overzicht van bewakings agenten](agents-overview.md)voor belang rijke aanvullende informatie.    

## <a name="table-formatting"></a>Tabel opmaak

> [!IMPORTANT] 
> Met deze laatste update wordt een nieuwe kolom toegevoegd en worden de metrische gegevens gerangschikt in alfabetische volg orde. De aanvullende informatie betekent dat de tabellen hieronder een horizontale schuif balk aan de onderkant kunnen hebben, afhankelijk van de breedte van uw browser venster. Als u van mening bent dat u de informatie ontbreekt, gebruikt u de schuif balk om de volledige tabel te bekijken.


## <a name="microsoftanalysisservicesservers"></a>Micro soft. AnalysisServices/servers

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|CleanerCurrentPrice|Ja|Geheugen: huidige prijs opschonen|Aantal|Average|Huidige prijs van geheugen, $/byte/tijd), genormaliseerd naar 1000.|ServerResourceType|
|CleanerMemoryNonshrinkable|Ja|Geheugen: Removal-geheugen kan niet worden verkleind|Bytes|Average|Hoeveelheid geheugen (in bytes) die niet wordt verwijderd door de achtergrond opschoning.|ServerResourceType|
|CleanerMemoryShrinkable|Ja|Geheugen: verkleinbaar geheugen|Bytes|Average|De hoeveelheid geheugen (in bytes) die wordt verwijderd door de achtergrond opschoning.|ServerResourceType|
|CommandPoolBusyThreads|Ja|Threads: actieve threads van opdracht pool|Aantal|Average|Het aantal actieve threads in de opdracht thread pool.|ServerResourceType|
|CommandPoolIdleThreads|Ja|Threads: niet-actieve threads van opdracht pool|Aantal|Average|Het aantal niet-actieve threads in de opdracht thread pool.|ServerResourceType|
|CommandPoolJobQueueLength|Ja|Wachtrij lengte van de opdracht pool taak|Aantal|Average|Aantal taken in de wachtrij van de opdracht thread pool.|ServerResourceType|
|CurrentConnections|Ja|Verbinding: huidige verbindingen|Aantal|Average|Het huidige aantal client verbindingen dat tot stand is gebracht.|ServerResourceType|
|CurrentUserSessions|Ja|Huidige gebruikers sessies|Aantal|Average|Het huidige aantal gebruikers sessies dat tot stand is gebracht.|ServerResourceType|
|LongParsingBusyThreads|Ja|Threads: bezette threads voor lang parseren|Aantal|Average|Het aantal actieve threads in de thread pool voor lang parseren.|ServerResourceType|
|LongParsingIdleThreads|Ja|Threads: niet-actieve threads voor lang parseren|Aantal|Average|Het aantal niet-actieve threads in de thread pool voor lang parseren.|ServerResourceType|
|LongParsingJobQueueLength|Ja|Threads: lengte van taak wachtrij voor lang parseren|Aantal|Average|Aantal taken in de wachtrij van de thread pool voor lang parseren.|ServerResourceType|
|mashup_engine_memory_metric|Ja|M-engine geheugen|Bytes|Average|Geheugen gebruik door mashup-engine processen|ServerResourceType|
|mashup_engine_private_bytes_metric|Ja|M-engine-eigen bytes|Bytes|Average|Privé-bytes gebruik door mashup-engine processen.|ServerResourceType|
|mashup_engine_qpu_metric|Ja|M-engine QPU|Aantal|Average|QPU-gebruik door mashup-engine processen|ServerResourceType|
|mashup_engine_virtual_bytes_metric|Ja|M-engine virtuele bytes|Bytes|Average|Gebruik van virtuele bytes door mashup-engine processen.|ServerResourceType|
|memory_metric|Ja|Geheugen|Bytes|Average|Geheugen. Bereik 0-25 GB voor S1, 0-50 GB voor S2 en 0-100 GB voor S4|ServerResourceType|
|memory_thrashing_metric|Ja|Geheugenthrashing|Percentage|Average|Gemiddeld geheugen overbelasting.|ServerResourceType|
|MemoryLimitHard|Ja|Geheugen: vaste geheugen limiet|Bytes|Average|Vaste geheugen limiet, van configuratie bestand.|ServerResourceType|
|MemoryLimitHigh|Ja|Geheugen: hoge geheugen limiet|Bytes|Average|Hoge geheugen limiet, van configuratie bestand.|ServerResourceType|
|MemoryLimitLow|Ja|Geheugen: lage geheugen limiet|Bytes|Average|Limiet voor weinig geheugen, van configuratie bestand.|ServerResourceType|
|MemoryLimitVertiPaq|Ja|Geheugen: VertiPaq-geheugen limiet|Bytes|Average|In-Memory limiet, van configuratie bestand.|ServerResourceType|
|MemoryUsage|Ja|Geheugen: geheugen gebruik|Bytes|Average|Geheugen gebruik van het Server proces zoals gebruikt bij het berekenen van de prijs voor het schonen van geheugen. Gelijk aan Counter Process\PrivateBytes plus de grootte van gegevens die zijn toegewezen door het geheugen, waarbij elk geheugen wordt genegeerd dat is toegewezen aan of toegewezen door de xVelocity in-Memory Analytics Engine (VertiPaq) die de geheugen limiet van xVelocity-engine overschrijdt.|ServerResourceType|
|private_bytes_metric|Ja|Privé-bytes|Bytes|Average|Privé-bytes.|ServerResourceType|
|ProcessingPoolBusyIOJobThreads|Ja|Threads: bezig met verwerken van I/O-taak threads van pool|Aantal|Average|Het aantal threads waarmee I/O-taken worden uitgevoerd in de verwer kende thread pool.|ServerResourceType|
|ProcessingPoolBusyNonIOThreads|Ja|Threads: bezig met het verwerken van niet-I/O-threads van de groep|Aantal|Average|Het aantal threads met niet-I/O-taken in de verwer kende thread pool.|ServerResourceType|
|ProcessingPoolIdleIOJobThreads|Ja|Threads: niet-actieve I/O-taak threads van de groep verwerken|Aantal|Average|Het aantal niet-actieve threads voor I/O-taken in de verwer kende thread pool.|ServerResourceType|
|ProcessingPoolIdleNonIOThreads|Ja|Threads: niet-I/O-threads van de groep worden verwerkt|Aantal|Average|Het aantal niet-actieve threads in de verwer kende thread pool dat is gereserveerd voor niet-I/O-taken.|ServerResourceType|
|ProcessingPoolIOJobQueueLength|Ja|Threads: lengte van I/O-taak wachtrij voor verwerking van groep|Aantal|Average|Het aantal I/O-taken in de wachtrij van de verwer kende thread pool.|ServerResourceType|
|ProcessingPoolJobQueueLength|Ja|Wachtrij lengte van de pool taak wordt verwerkt|Aantal|Average|Het aantal niet-I/O-taken in de wachtrij van de verwer kende thread pool.|ServerResourceType|
|qpu_metric|Ja|QPU|Aantal|Average|QPU. Bereik 0-100 voor S1, 0-200 voor S2 en 0-400 voor S4|ServerResourceType|
|QueryPoolBusyThreads|Ja|Query's pool bezette threads|Aantal|Average|Het aantal actieve threads in de query thread pool.|ServerResourceType|
|QueryPoolIdleThreads|Ja|Threads: niet-actieve threads van query pool|Aantal|Average|Het aantal niet-actieve threads voor I/O-taken in de verwer kende thread pool.|ServerResourceType|
|QueryPoolJobQueueLength|Ja|Threads: lengte van taak wachtrij voor query pool|Aantal|Average|Aantal taken in de wachtrij van de query thread pool.|ServerResourceType|
|Quota|Ja|Geheugen: quotum|Bytes|Average|Huidig geheugen quotum, in bytes. Geheugen quota wordt ook wel een geheugen toekenning of geheugen reservering genoemd.|ServerResourceType|
|QuotaBlocked|Ja|Geheugen: quotum geblokkeerd|Aantal|Average|Het huidige aantal quotum aanvragen dat is geblokkeerd totdat andere geheugen quota zijn vrijgemaakt.|ServerResourceType|
|RowsConvertedPerSec|Ja|Verwerken: geconverteerde rijen per seconde|CountPerSecond|Average|Het aantal rijen dat tijdens de verwerking is geconverteerd.|ServerResourceType|
|RowsReadPerSec|Ja|Verwerken: gelezen rijen per seconde|CountPerSecond|Average|Het aantal rijen dat van alle relationele data bases is gelezen.|ServerResourceType|
|RowsWrittenPerSec|Ja|Verwerken: geschreven rijen per seconde|CountPerSecond|Average|Het aantal rijen dat tijdens de verwerking is geschreven.|ServerResourceType|
|ShortParsingBusyThreads|Ja|Threads: bezette threads voor kort parseren|Aantal|Average|Het aantal actieve threads in de thread pool voor kort parseren.|ServerResourceType|
|ShortParsingIdleThreads|Ja|Threads: niet-actieve threads voor kort parseren|Aantal|Average|Het aantal niet-actieve threads in de thread pool voor kort parseren.|ServerResourceType|
|ShortParsingJobQueueLength|Ja|Threads: lengte van taak wachtrij voor kort parseren|Aantal|Average|Aantal taken in de wachtrij van de thread pool voor kort parseren.|ServerResourceType|
|SuccessfullConnectionsPerSec|Ja|Geslaagde verbindingen per seconde|CountPerSecond|Average|Snelheid van geslaagde verbindings voltooiingen.|ServerResourceType|
|TotalConnectionFailures|Ja|Totaal aantal verbindings fouten|Aantal|Average|Totaal aantal mislukte Verbindings pogingen.|ServerResourceType|
|TotalConnectionRequests|Ja|Totaal aantal verbindings aanvragen|Aantal|Average|Totaal aantal verbindings aanvragen. Dit zijn ontvangsten.|ServerResourceType|
|VertiPaqNonpaged|Ja|Geheugen: VertiPaq niet-wisselbaar|Bytes|Average|Geheugen bytes vergrendeld in de werkset voor gebruik door de in-Memory engine.|ServerResourceType|
|VertiPaqPaged|Ja|Geheugen: VertiPaq-pagina|Bytes|Average|Bytes van wisselbaar geheugen die in gebruik zijn voor in-Memory gegevens.|ServerResourceType|
|virtual_bytes_metric|Ja|Virtuele bytes|Bytes|Average|Virtuele bytes.|ServerResourceType|


## <a name="microsoftapimanagementservice"></a>Microsoft.ApiManagement/service

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|BackendDuration|Ja|Duur van back-end-aanvragen|Milliseconden|Average|Duur van back-end-aanvragen in milliseconden|Locatie, hostnaam|
|Capaciteit|Ja|Capaciteit|Percentage|Average|De metrische gegevens over het gebruik van de ApiManagement-service|Locatie|
|Duur|Ja|Totale duur van gateway aanvragen|Milliseconden|Average|Totale duur van gateway aanvragen in milliseconden|Locatie, hostnaam|
|EventHubDroppedEvents|Ja|Verwijderde EventHub-gebeurtenissen|Aantal|Totaal|Het aantal gebeurtenissen dat is overgeslagen omdat de maximale grootte van de wachtrij is bereikt|Locatie|
|EventHubRejectedEvents|Ja|Geweigerde EventHub-gebeurtenissen|Aantal|Totaal|Aantal geweigerde EventHub-gebeurtenissen (onjuiste configuratie of niet-geautoriseerd)|Locatie|
|EventHubSuccessfulEvents|Ja|Geslaagde EventHub-gebeurtenissen|Aantal|Totaal|Aantal geslaagde EventHub-gebeurtenissen|Locatie|
|EventHubThrottledEvents|Ja|Vertraagde EventHub-gebeurtenissen|Aantal|Totaal|Aantal vertraagde EventHub-gebeurtenissen|Locatie|
|EventHubTimedoutEvents|Ja|Time-out EventHub-gebeurtenissen|Aantal|Totaal|Aantal time-out EventHub-gebeurtenissen|Locatie|
|EventHubTotalBytesSent|Ja|Grootte van EventHub-gebeurtenissen|Bytes|Totaal|Totale grootte van EventHub-gebeurtenissen in bytes|Locatie|
|EventHubTotalEvents|Ja|Totaal aantal EventHub-gebeurtenissen|Aantal|Totaal|Aantal gebeurtenissen dat naar EventHub is verzonden|Locatie|
|EventHubTotalFailedEvents|Ja|Mislukte EventHub-gebeurtenissen|Aantal|Totaal|Aantal mislukte EventHub-gebeurtenissen|Locatie|
|FailedRequests|Ja|Mislukte gateway aanvragen (afgeschaft)|Aantal|Totaal|Aantal fouten in gateway aanvragen-metrische aanvraag voor meerdere dimensies met GatewayResponseCodeCategory dimensie gebruiken|Locatie, hostnaam|
|NetworkConnectivity|Ja|Status van de netwerk verbinding van resources (preview-versie)|Aantal|Average|Status van de netwerk verbinding van de afhankelijke bron typen van de API Management-service|Locatie, resource type|
|OtherRequests|Ja|Andere gateway aanvragen (afgeschaft)|Aantal|Totaal|Aantal andere gateway aanvragen-metrische aanvraag voor meerdere dimensies met GatewayResponseCodeCategory dimensie gebruiken|Locatie, hostnaam|
|Aanvragen|Ja|Aanvragen|Aantal|Totaal|Metrische gegevens van gateway aanvragen met meerdere dimensies|Location, hostname, LastErrorReason, BackendResponseCode, GatewayResponseCode, BackendResponseCodeCategory, GatewayResponseCodeCategory|
|SuccessfulRequests|Ja|Geslaagde gateway aanvragen (afgeschaft)|Aantal|Totaal|Aantal geslaagde gateway aanvragen-metrische aanvraag voor meerdere dimensies met GatewayResponseCodeCategory dimensie gebruiken|Locatie, hostnaam|
|TotalRequests|Ja|Totaal aantal gateway aanvragen (afgeschaft)|Aantal|Totaal|Aantal gateway aanvragen-metrische aanvraag voor meerdere dimensies met GatewayResponseCodeCategory dimensie gebruiken|Locatie, hostnaam|
|UnauthorizedRequests|Ja|Niet-geautoriseerde gateway aanvragen (afgeschaft)|Aantal|Totaal|Aantal niet-geautoriseerde gateway aanvragen-metrische aanvraag voor meerdere dimensies met GatewayResponseCodeCategory dimensie gebruiken|Locatie, hostnaam|


## <a name="microsoftappconfigurationconfigurationstores"></a>Micro soft. AppConfiguration/configurationStores

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|HttpIncomingRequestCount|Ja|HttpIncomingRequestCount|Aantal|Aantal|Totaal aantal binnenkomende HTTP-aanvragen.|Status code, authenticatie|
|HttpIncomingRequestDuration|Ja|HttpIncomingRequestDuration|Aantal|Average|Latentie voor een HTTP-aanvraag.|Status code, authenticatie|
|ThrottledHttpRequestCount|Ja|ThrottledHttpRequestCount|Aantal|Aantal|Beperkte HTTP-aanvragen.|Geen dimensies|


## <a name="microsoftappplatformspring"></a>Micro soft. AppPlatform/lente

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|JVM. gc. live. data. size|Ja|JVM. gc. live. data. size|Bytes|Average|Grootte van de geheugen groep van de oude generatie na een volledige GC|Implementatie, AppName, pod|
|JVM. gc. max. data. size|Ja|JVM. gc. max. data. size|Bytes|Average|Maximale grootte van de geheugen groep voor de oude generatie|Implementatie, AppName, pod|
|JVM. gc. toegewezen geheugen|Ja|JVM. gc. toegewezen geheugen|Bytes|Maximum|Verhoogd voor een toename van de grootte van de Memory pool voor de jonge generatie na een GC tot de volgende|Implementatie, AppName, pod|
|JVM. gc. Memory. bevorderd|Ja|JVM. gc. Memory. bevorderd|Bytes|Maximum|Aantal positieve toename van de grootte van de oude generatie geheugengroep vóór GC tot na GC|Implementatie, AppName, pod|
|JVM. gc. pause. Total. Count|Ja|JVM. gc. pause. Total. Count|Aantal|Totaal|Aantal GC-onderbrekingen|Implementatie, AppName, pod|
|JVM. gc. pause. Total. time|Ja|JVM. gc. pause. Total. time|Milliseconden|Totaal|Totale tijd van de GC-onderbreking|Implementatie, AppName, pod|
|JVM. Memory. committed|Ja|JVM. Memory. committed|Bytes|Average|Geheugen toegewezen aan JVM in bytes|Implementatie, AppName, pod|
|JVM. Memory. Max|Ja|JVM. Memory. Max|Bytes|Maximum|De maximale hoeveelheid geheugen in bytes die kan worden gebruikt voor geheugen beheer|Implementatie, AppName, pod|
|JVM. Memory. used|Ja|JVM. Memory. used|Bytes|Average|Gebruikt app-geheugen in bytes|Implementatie, AppName, pod|
|proces. CPU. Usage|Ja|proces. CPU. Usage|Percentage|Average|JVM CPU-gebruiks percentage van app|Implementatie, AppName, pod|
|System. CPU. Usage|Ja|System. CPU. Usage|Percentage|Average|Het recente CPU-gebruik voor het hele systeem|Implementatie, AppName, pod|
|Tomcat. Global. error|Ja|Tomcat. Global. error|Aantal|Totaal|Tomcat Global-fout|Implementatie, AppName, pod|
|Tomcat. Global. ontvangen|Ja|Tomcat. Global. ontvangen|Bytes|Totaal|Totaal aantal bytes ontvangen Tomcat|Implementatie, AppName, pod|
|Tomcat. Global. Request. Gem. tijd|Ja|Tomcat. Global. Request. Gem. tijd|Milliseconden|Average|Gemiddelde tijd Tomcat-aanvraag|Implementatie, AppName, pod|
|Tomcat. Global. Request. Max|Ja|Tomcat. Global. Request. Max|Milliseconden|Maximum|Maximale tijd voor tomcat-aanvraag|Implementatie, AppName, pod|
|Tomcat. Global. Request. Total. Count|Ja|Tomcat. Global. Request. Total. Count|Aantal|Totaal|Totaal aantal Tomcat-aanvragen|Implementatie, AppName, pod|
|Tomcat. Global. Request. Total. time|Ja|Tomcat. Global. Request. Total. time|Milliseconden|Totaal|Totale tijd Tomcat-aanvraag|Implementatie, AppName, pod|
|Tomcat. Global. sent|Ja|Tomcat. Global. sent|Bytes|Totaal|Totaal aantal verzonden bytes in Tomcat|Implementatie, AppName, pod|
|Tomcat. Sessions. Active. current|Ja|Tomcat. Sessions. Active. current|Aantal|Totaal|Aantal actieve sessies van Tomcat|Implementatie, AppName, pod|
|Tomcat. Sessions. Active. Max|Ja|Tomcat. Sessions. Active. Max|Aantal|Totaal|Aantal actieve Tomcat-sessies|Implementatie, AppName, pod|
|Tomcat. Sessions. Alive. Max|Ja|Tomcat. Sessions. Alive. Max|Milliseconden|Maximum|Time-outperiode van Tomcat-sessie|Implementatie, AppName, pod|
|Tomcat. Sessions. created|Ja|Tomcat. Sessions. created|Aantal|Totaal|Aantal gemaakte Tomcat-sessies|Implementatie, AppName, pod|
|Tomcat. Sessions. Expires|Ja|Tomcat. Sessions. Expires|Aantal|Totaal|Aantal verlopen Tomcat-sessies|Implementatie, AppName, pod|
|Tomcat. Sessions. rejected|Ja|Tomcat. Sessions. rejected|Aantal|Totaal|Aantal geweigerde Tomcat-sessies|Implementatie, AppName, pod|
|tomcat.threads.config Max.|Ja|tomcat.threads.config Max.|Aantal|Totaal|Maximum aantal threads van Tomcat-configuratie|Implementatie, AppName, pod|
|Tomcat. threads. current|Ja|Tomcat. threads. current|Aantal|Totaal|Aantal huidige threads van Tomcat|Implementatie, AppName, pod|


## <a name="microsoftautomationautomationaccounts"></a>Micro soft. Automation/automationAccounts

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|TotalJob|Ja|Totaal aantal taken|Aantal|Totaal|Het totale aantal taken|Runbook, status|
|TotalUpdateDeploymentMachineRuns|Ja|Totaal aantal machine-uitvoeringen van update-implementaties|Aantal|Totaal|Het totale aantal uitgevoerde software-update-implementatie computers in een software-update-implementatie|SoftwareUpdateConfigurationName, status, target computer, SoftwareUpdateConfigurationRunId|
|TotalUpdateDeploymentRuns|Ja|Totaal aantal uitvoeringen van update-implementaties|Aantal|Totaal|Totale aantal uitgevoerde software-update-implementaties|SoftwareUpdateConfigurationName, status|


## <a name="microsoftbatchbatchaccounts"></a>Microsoft.Bat-CH/batchAccounts

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|CoreCount|Nee|Aantal toegewezen kernen|Aantal|Totaal|Totaal aantal toegewezen kernen in het batch-account|Geen dimensies|
|CreatingNodeCount|Nee|Aantal knoop punten maken|Aantal|Totaal|Aantal knoop punten dat wordt gemaakt|Geen dimensies|
|IdleNodeCount|Nee|Aantal niet-actieve knoop punten|Aantal|Totaal|Aantal niet-actieve knoop punten|Geen dimensies|
|JobDeleteCompleteEvent|Ja|Voltooide gebeurtenissen van taak verwijderen|Aantal|Totaal|Het totale aantal taken dat is verwijderd.|jobId|
|JobDeleteStartEvent|Ja|Taak begin gebeurtenissen verwijderen|Aantal|Totaal|Het totale aantal taken dat is aangevraagd om te worden verwijderd.|jobId|
|JobDisableCompleteEvent|Ja|Voltooide gebeurtenissen voor taak uitschakelen|Aantal|Totaal|Het totale aantal taken dat is uitgeschakeld.|jobId|
|JobDisableStartEvent|Ja|Taak start gebeurtenissen uitschakelen|Aantal|Totaal|Het totale aantal taken dat is aangevraagd om te worden uitgeschakeld.|jobId|
|JobStartEvent|Ja|Taak begin gebeurtenissen|Aantal|Totaal|Het totale aantal taken dat is gestart.|jobId|
|JobTerminateCompleteEvent|Ja|Voltooide gebeurtenissen voor taak beëindigen|Aantal|Totaal|Het totale aantal taken dat is beëindigd.|jobId|
|JobTerminateStartEvent|Ja|Taak start gebeurtenissen beëindigen|Aantal|Totaal|Het totale aantal taken dat is aangevraagd om te worden beëindigd.|jobId|
|LeavingPoolNodeCount|Nee|Aantal groeps knooppunten verlaten|Aantal|Totaal|Aantal knoop punten dat de pool verlaat|Geen dimensies|
|LowPriorityCoreCount|Nee|Aantal LowPriority kernen|Aantal|Totaal|Totaal aantal kernen met lage prioriteit in het batch-account|Geen dimensies|
|OfflineNodeCount|Nee|Aantal offline knooppunten|Aantal|Totaal|Aantal offline knooppunten|Geen dimensies|
|PoolCreateEvent|Ja|Groeps gebeurtenissen maken|Aantal|Totaal|Totaal aantal Pools dat is gemaakt|poolId|
|PoolDeleteCompleteEvent|Ja|Voltooide gebeurtenissen van groep verwijderen|Aantal|Totaal|Totaal aantal verwijderde groepen dat is voltooid|poolId|
|PoolDeleteStartEvent|Ja|Begin gebeurtenissen groep verwijderen|Aantal|Totaal|Totaal aantal verwijderde groepen dat is gestart|poolId|
|PoolResizeCompleteEvent|Ja|Volledige gebeurtenissen voor het wijzigen van de pool|Aantal|Totaal|Totaal aantal hergroottes van de groep die zijn voltooid|poolId|
|PoolResizeStartEvent|Ja|Begin gebeurtenissen van groeps grootte wijzigen|Aantal|Totaal|Totaal aantal hergroottes van de groep die zijn gestart|poolId|
|PreemptedNodeCount|Nee|Aantal knoop punten in herhaling|Aantal|Totaal|Aantal knoop punten dat is afgebroken|Geen dimensies|
|RebootingNodeCount|Nee|Aantal knoop punten opnieuw opstarten|Aantal|Totaal|Aantal knoop punten dat opnieuw wordt opgestart|Geen dimensies|
|ReimagingNodeCount|Nee|Telling van het aantal knoop punten|Aantal|Totaal|Aantal knoop punten van installatie kopieën|Geen dimensies|
|RunningNodeCount|Nee|Aantal actieve knoop punten|Aantal|Totaal|Aantal actieve knoop punten|Geen dimensies|
|StartingNodeCount|Nee|Begin aantal knoop punten|Aantal|Totaal|Aantal knoop punten dat begint|Geen dimensies|
|StartTaskFailedNodeCount|Nee|Aantal mislukte knoop punten van begin taak|Aantal|Totaal|Aantal knoop punten waarop de begin taak is mislukt|Geen dimensies|
|TaskCompleteEvent|Ja|Taak voltooid gebeurtenissen|Aantal|Totaal|Totaal aantal taken dat is voltooid|poolId, jobId|
|TaskFailEvent|Ja|Taak fout gebeurtenissen|Aantal|Totaal|Totaal aantal taken dat is voltooid met de status mislukt|poolId, jobId|
|TaskStartEvent|Ja|Taak begin gebeurtenissen|Aantal|Totaal|Totaal aantal taken dat is gestart|poolId, jobId|
|TotalLowPriorityNodeCount|Nee|Aantal Low-Priority knoop punten|Aantal|Totaal|Totaal aantal knoop punten met een lage prioriteit in het batch-account|Geen dimensies|
|TotalNodeCount|Nee|Aantal toegewezen knoop punten|Aantal|Totaal|Totaal aantal toegewezen knoop punten in het batch-account|Geen dimensies|
|UnusableNodeCount|Nee|Aantal niet-bruikbare knoop punten|Aantal|Totaal|Aantal niet-bruikbare knoop punten|Geen dimensies|
|WaitingForStartTaskNodeCount|Nee|Wachten op aantal begin taak knooppunten|Aantal|Totaal|Aantal knoop punten dat wacht tot de begin taak is voltooid|Geen dimensies|


## <a name="microsoftbatchaiworkspaces"></a>Microsoft.BatchAI/werk ruimten

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|Actieve kernen|Ja|Actieve kernen|Aantal|Average|Aantal actieve kernen|Scenario, clustername|
|Actieve knoop punten|Ja|Actieve knoop punten|Aantal|Average|Aantal actieve knoop punten|Scenario, clustername|
|Niet-actieve kernen|Ja|Niet-actieve kernen|Aantal|Average|Aantal niet-actieve kern geheugens|Scenario, clustername|
|Niet-actieve knoop punten|Ja|Niet-actieve knoop punten|Aantal|Average|Aantal niet-actieve knoop punten|Scenario, clustername|
|Taak is voltooid|Ja|Taak is voltooid|Aantal|Totaal|Aantal voltooide taken|Scenario, clustername, ResultType|
|Taak verzonden|Ja|Taak verzonden|Aantal|Totaal|Aantal verzonden taken|Scenario, clustername|
|Kernen verlaten|Ja|Kernen verlaten|Aantal|Average|Aantal te verlaten kernen|Scenario, clustername|
|Knoop punten verlaten|Ja|Knoop punten verlaten|Aantal|Average|Aantal verlaatde knoop punten|Scenario, clustername|
|Afgebroken kernen|Ja|Afgebroken kernen|Aantal|Average|Aantal afgebroken kernen|Scenario, clustername|
|Knoop punten die zijn afgebroken|Ja|Knoop punten die zijn afgebroken|Aantal|Average|Aantal knoop punten dat is afgebroken|Scenario, clustername|
|Percentage quotum gebruik|Ja|Percentage quotum gebruik|Aantal|Average|Percentage van gebruikte quota|Scenario, clustername, VmFamilyName, VmPriority|
|Totaal aantal kernen|Ja|Totaal aantal kernen|Aantal|Average|Aantal totale kernen|Scenario, clustername|
|Totaal aantal knoop punten|Ja|Totaal aantal knoop punten|Aantal|Average|Totaal aantal knoop punten|Scenario, clustername|
|Onbruikbaar aantal kern geheugens|Ja|Onbruikbaar aantal kern geheugens|Aantal|Average|Aantal niet-bruikbare kernen|Scenario, clustername|
|Niet-bruikbare knoop punten|Ja|Niet-bruikbare knoop punten|Aantal|Average|Aantal niet-bruikbare knoop punten|Scenario, clustername|


## <a name="microsoftblockchainblockchainmembers"></a>Microsoft.Blockchain/blockchainMembers

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|BroadcastProcessedCount|Ja|Aantal verwerkte broadcasts|Aantal|Average|Het aantal verwerkte trans acties|Knoop punt, kanaal, type, status|
|ConnectionAccepted|Ja|Geaccepteerde verbindingen|Aantal|Totaal|Geaccepteerde verbindingen|Knooppunt|
|ConnectionActive|Ja|Actieve verbindingen|Aantal|Average|Actieve verbindingen|Knooppunt|
|ConnectionHandled|Ja|Afgehandelde verbindingen|Aantal|Totaal|Afgehandelde verbindingen|Knooppunt|
|ConsensusEtcdraftCommittedBlockNumber|Ja|Het door Etcdraft toegekende blok nummer consensus|Aantal|Average|Het blok nummer van het laatste blok dat is doorgevoerd|Knoop punt, kanaal|
|CpuUsagePercentageInDouble|Ja|Percentage CPU-gebruik|Percentage|Maximum|Percentage CPU-gebruik|Knooppunt|
|EndorserEndorsementFailures|Ja|Goedkeurings fouten van endorser|Aantal|Average|Het aantal mislukte vermeldingen.|Node, Channel, chaincode, chaincodeerror|
|GossipLeaderElectionLeader|Ja|Leiders leider Gossip Leader|Aantal|Totaal|Peer is leider (1) of volger (0)|Knoop punt, kanaal|
|GossipMembershipTotalPeersKnown|Ja|Totaal aantal bekende peers van Gossip-lidmaatschap|Aantal|Average|Totaal aantal bekende peers|Knoop punt, kanaal|
|GossipStateHeight|Ja|Gossip-status|Aantal|Average|Huidige grootboek hoogte|Knoop punt, kanaal|
|IOReadBytes|Ja|I/o gelezen bytes|Bytes|Totaal|I/o gelezen bytes|Knooppunt|
|IOWriteBytes|Ja|I/o-schrijf bytes|Bytes|Totaal|I/o-schrijf bytes|Knooppunt|
|LedgerTransactionCount|Ja|Aantal grootboek transacties|Aantal|Average|Aantal verwerkte trans acties|Node, Channel, transaction_type, chaincode, validation_code|
|Memory limit|Ja|Geheugen limiet|Bytes|Average|Geheugen limiet|Knooppunt|
|MemoryUsage|Ja|Geheugengebruik|Bytes|Average|Geheugengebruik|Knooppunt|
|MemoryUsagePercentageInDouble|Ja|Percentage geheugen gebruik|Percentage|Average|Percentage geheugen gebruik|Knooppunt|
|PendingTransactions|Ja|Trans acties in behandeling|Aantal|Average|Trans acties in behandeling|Knooppunt|
|ProcessedBlocks|Ja|Verwerkte blokken|Aantal|Totaal|Verwerkte blokken|Knooppunt|
|ProcessedTransactions|Ja|Verwerkte trans acties|Aantal|Totaal|Verwerkte trans acties|Knooppunt|
|QueuedTransactions|Ja|Trans acties in de wachtrij|Aantal|Average|Trans acties in de wachtrij|Knooppunt|
|RequestHandled|Ja|Verwerkte aanvragen|Aantal|Totaal|Verwerkte aanvragen|Knooppunt|
|StorageUsage|Ja|Opslag gebruik|Bytes|Average|Opslag gebruik|Knooppunt|


## <a name="microsoftcacheredis"></a>Micro soft. cache/redis

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|cachehits|Ja|Cachetreffers|Aantal|Totaal||ShardId|
|cachehits0|Ja|Cache treffers (Shard 0)|Aantal|Totaal||Geen dimensies|
|cachehits1|Ja|Cache treffers (Shard 1)|Aantal|Totaal||Geen dimensies|
|cachehits2|Ja|Cache treffers (Shard 2)|Aantal|Totaal||Geen dimensies|
|cachehits3|Ja|Cache treffers (Shard 3)|Aantal|Totaal||Geen dimensies|
|cachehits4|Ja|Cache treffers (Shard 4)|Aantal|Totaal||Geen dimensies|
|cachehits5|Ja|Cache treffers (Shard 5)|Aantal|Totaal||Geen dimensies|
|cachehits6|Ja|Cache treffers (Shard 6)|Aantal|Totaal||Geen dimensies|
|cachehits7|Ja|Cache treffers (Shard 7)|Aantal|Totaal||Geen dimensies|
|cachehits8|Ja|Cache treffers (Shard 8)|Aantal|Totaal||Geen dimensies|
|cachehits9|Ja|Cache treffers (Shard 9)|Aantal|Totaal||Geen dimensies|
|cacheLatency|Ja|Cache latentie micro seconden (preview-versie)|Aantal|Average||ShardId|
|cachemisses|Ja|Cachemissers|Aantal|Totaal||ShardId|
|cachemisses0|Ja|Cache missers (Shard 0)|Aantal|Totaal||Geen dimensies|
|cachemisses1|Ja|Cache missers (Shard 1)|Aantal|Totaal||Geen dimensies|
|cachemisses2|Ja|Cache missers (Shard 2)|Aantal|Totaal||Geen dimensies|
|cachemisses3|Ja|Cache missers (Shard 3)|Aantal|Totaal||Geen dimensies|
|cachemisses4|Ja|Cache missers (Shard 4)|Aantal|Totaal||Geen dimensies|
|cachemisses5|Ja|Cache missers (Shard 5)|Aantal|Totaal||Geen dimensies|
|cachemisses6|Ja|Cache missers (Shard 6)|Aantal|Totaal||Geen dimensies|
|cachemisses7|Ja|Cache missers (Shard 7)|Aantal|Totaal||Geen dimensies|
|cachemisses8|Ja|Cache missers (Shard 8)|Aantal|Totaal||Geen dimensies|
|cachemisses9|Ja|Cache missers (Shard 9)|Aantal|Totaal||Geen dimensies|
|cachemissrate|Ja|Aantal missers in cache|Percentage|cachemissrate||ShardId|
|cacheRead|Ja|Gelezen uit cache|BytesPerSecond|Maximum||ShardId|
|cacheRead0|Ja|Cache gelezen (Shard 0)|BytesPerSecond|Maximum||Geen dimensies|
|cacheRead1|Ja|Lees bewerking in cache (Shard 1)|BytesPerSecond|Maximum||Geen dimensies|
|cacheRead2|Ja|Lees bewerking in cache (Shard 2)|BytesPerSecond|Maximum||Geen dimensies|
|cacheRead3|Ja|Lees bewerking in cache (Shard 3)|BytesPerSecond|Maximum||Geen dimensies|
|cacheRead4|Ja|Lees bewerking in cache (Shard 4)|BytesPerSecond|Maximum||Geen dimensies|
|cacheRead5|Ja|Lees bewerking in cache (Shard 5)|BytesPerSecond|Maximum||Geen dimensies|
|cacheRead6|Ja|Lees bewerking in cache (Shard 6)|BytesPerSecond|Maximum||Geen dimensies|
|cacheRead7|Ja|Lees bewerking in cache (Shard 7)|BytesPerSecond|Maximum||Geen dimensies|
|cacheRead8|Ja|Lees bewerking in cache (Shard 8)|BytesPerSecond|Maximum||Geen dimensies|
|cacheRead9|Ja|Lees bewerking in cache (Shard 9)|BytesPerSecond|Maximum||Geen dimensies|
|cacheWrite|Ja|Geschreven naar cache|BytesPerSecond|Maximum||ShardId|
|cacheWrite0|Ja|Cache schrijven (Shard 0)|BytesPerSecond|Maximum||Geen dimensies|
|cacheWrite1|Ja|Cache schrijven (Shard 1)|BytesPerSecond|Maximum||Geen dimensies|
|cacheWrite2|Ja|Cache schrijven (Shard 2)|BytesPerSecond|Maximum||Geen dimensies|
|cacheWrite3|Ja|Cache schrijven (Shard 3)|BytesPerSecond|Maximum||Geen dimensies|
|cacheWrite4|Ja|Cache schrijven (Shard 4)|BytesPerSecond|Maximum||Geen dimensies|
|cacheWrite5|Ja|Cache schrijven (Shard 5)|BytesPerSecond|Maximum||Geen dimensies|
|cacheWrite6|Ja|Cache schrijven (Shard 6)|BytesPerSecond|Maximum||Geen dimensies|
|cacheWrite7|Ja|Cache schrijven (Shard 7)|BytesPerSecond|Maximum||Geen dimensies|
|cacheWrite8|Ja|Cache schrijven (Shard 8)|BytesPerSecond|Maximum||Geen dimensies|
|cacheWrite9|Ja|Cache schrijven (Shard 9)|BytesPerSecond|Maximum||Geen dimensies|
|connectedclients|Ja|Verbonden clients|Aantal|Maximum||ShardId|
|connectedclients0|Ja|Verbonden clients (Shard 0)|Aantal|Maximum||Geen dimensies|
|connectedclients1|Ja|Verbonden clients (Shard 1)|Aantal|Maximum||Geen dimensies|
|connectedclients2|Ja|Verbonden clients (Shard 2)|Aantal|Maximum||Geen dimensies|
|connectedclients3|Ja|Verbonden clients (Shard 3)|Aantal|Maximum||Geen dimensies|
|connectedclients4|Ja|Verbonden clients (Shard 4)|Aantal|Maximum||Geen dimensies|
|connectedclients5|Ja|Verbonden clients (Shard 5)|Aantal|Maximum||Geen dimensies|
|connectedclients6|Ja|Verbonden clients (Shard 6)|Aantal|Maximum||Geen dimensies|
|connectedclients7|Ja|Verbonden clients (Shard 7)|Aantal|Maximum||Geen dimensies|
|connectedclients8|Ja|Verbonden clients (Shard 8)|Aantal|Maximum||Geen dimensies|
|connectedclients9|Ja|Verbonden clients (Shard 9)|Aantal|Maximum||Geen dimensies|
|fouten|Ja|Fouten|Aantal|Maximum||ShardId, error type|
|evictedkeys|Ja|Verwijderde sleutels|Aantal|Totaal||ShardId|
|evictedkeys0|Ja|Verwijderde sleutels (Shard 0)|Aantal|Totaal||Geen dimensies|
|evictedkeys1|Ja|Verwijderde sleutels (Shard 1)|Aantal|Totaal||Geen dimensies|
|evictedkeys2|Ja|Verwijderde sleutels (Shard 2)|Aantal|Totaal||Geen dimensies|
|evictedkeys3|Ja|Verwijderde sleutels (Shard 3)|Aantal|Totaal||Geen dimensies|
|evictedkeys4|Ja|Verwijderde sleutels (Shard 4)|Aantal|Totaal||Geen dimensies|
|evictedkeys5|Ja|Verwijderde sleutels (Shard 5)|Aantal|Totaal||Geen dimensies|
|evictedkeys6|Ja|Verwijderde sleutels (Shard 6)|Aantal|Totaal||Geen dimensies|
|evictedkeys7|Ja|Verwijderde sleutels (Shard 7)|Aantal|Totaal||Geen dimensies|
|evictedkeys8|Ja|Verwijderde sleutels (Shard 8)|Aantal|Totaal||Geen dimensies|
|evictedkeys9|Ja|Verwijderde sleutels (Shard 9)|Aantal|Totaal||Geen dimensies|
|expiredkeys|Ja|Verlopen sleutels|Aantal|Totaal||ShardId|
|expiredkeys0|Ja|Verlopen sleutels (Shard 0)|Aantal|Totaal||Geen dimensies|
|expiredkeys1|Ja|Verlopen sleutels (Shard 1)|Aantal|Totaal||Geen dimensies|
|expiredkeys2|Ja|Verlopen sleutels (Shard 2)|Aantal|Totaal||Geen dimensies|
|expiredkeys3|Ja|Verlopen sleutels (Shard 3)|Aantal|Totaal||Geen dimensies|
|expiredkeys4|Ja|Verlopen sleutels (Shard 4)|Aantal|Totaal||Geen dimensies|
|expiredkeys5|Ja|Verlopen sleutels (Shard 5)|Aantal|Totaal||Geen dimensies|
|expiredkeys6|Ja|Verlopen sleutels (Shard 6)|Aantal|Totaal||Geen dimensies|
|expiredkeys7|Ja|Verlopen sleutels (Shard 7)|Aantal|Totaal||Geen dimensies|
|expiredkeys8|Ja|Verlopen sleutels (Shard 8)|Aantal|Totaal||Geen dimensies|
|expiredkeys9|Ja|Verlopen sleutels (Shard 9)|Aantal|Totaal||Geen dimensies|
|getcommands|Ja|Ophalingen|Aantal|Totaal||ShardId|
|getcommands0|Ja|Hiermee wordt opgehaald (Shard 0)|Aantal|Totaal||Geen dimensies|
|getcommands1|Ja|Hiermee wordt opgehaald (Shard 1)|Aantal|Totaal||Geen dimensies|
|getcommands2|Ja|Hiermee wordt opgehaald (Shard 2)|Aantal|Totaal||Geen dimensies|
|getcommands3|Ja|Hiermee wordt opgehaald (Shard 3)|Aantal|Totaal||Geen dimensies|
|getcommands4|Ja|Hiermee wordt opgehaald (Shard 4)|Aantal|Totaal||Geen dimensies|
|getcommands5|Ja|Hiermee wordt opgehaald (Shard 5)|Aantal|Totaal||Geen dimensies|
|getcommands6|Ja|Hiermee wordt opgehaald (Shard 6)|Aantal|Totaal||Geen dimensies|
|getcommands7|Ja|Hiermee wordt opgehaald (Shard 7)|Aantal|Totaal||Geen dimensies|
|getcommands8|Ja|Hiermee wordt opgehaald (Shard 8)|Aantal|Totaal||Geen dimensies|
|getcommands9|Ja|Hiermee wordt opgehaald (Shard 9)|Aantal|Totaal||Geen dimensies|
|operationsPerSecond|Ja|Bewerkingen per seconde|Aantal|Maximum||ShardId|
|operationsPerSecond0|Ja|Bewerkingen per seconde (Shard 0)|Aantal|Maximum||Geen dimensies|
|operationsPerSecond1|Ja|Bewerkingen per seconde (Shard 1)|Aantal|Maximum||Geen dimensies|
|operationsPerSecond2|Ja|Bewerkingen per seconde (Shard 2)|Aantal|Maximum||Geen dimensies|
|operationsPerSecond3|Ja|Bewerkingen per seconde (Shard 3)|Aantal|Maximum||Geen dimensies|
|operationsPerSecond4|Ja|Bewerkingen per seconde (Shard 4)|Aantal|Maximum||Geen dimensies|
|operationsPerSecond5|Ja|Bewerkingen per seconde (Shard 5)|Aantal|Maximum||Geen dimensies|
|operationsPerSecond6|Ja|Bewerkingen per seconde (Shard 6)|Aantal|Maximum||Geen dimensies|
|operationsPerSecond7|Ja|Bewerkingen per seconde (Shard 7)|Aantal|Maximum||Geen dimensies|
|operationsPerSecond8|Ja|Bewerkingen per seconde (Shard 8)|Aantal|Maximum||Geen dimensies|
|operationsPerSecond9|Ja|Bewerkingen per seconde (Shard 9)|Aantal|Maximum||Geen dimensies|
|percentProcessorTime|Ja|CPU|Percentage|Maximum||ShardId|
|percentProcessorTime0|Ja|CPU (Shard 0)|Percentage|Maximum||Geen dimensies|
|percentProcessorTime1|Ja|CPU (Shard 1)|Percentage|Maximum||Geen dimensies|
|percentProcessorTime2|Ja|CPU (Shard 2)|Percentage|Maximum||Geen dimensies|
|percentProcessorTime3|Ja|CPU (Shard 3)|Percentage|Maximum||Geen dimensies|
|percentProcessorTime4|Ja|CPU (Shard 4)|Percentage|Maximum||Geen dimensies|
|percentProcessorTime5|Ja|CPU (Shard 5)|Percentage|Maximum||Geen dimensies|
|percentProcessorTime6|Ja|CPU (Shard 6)|Percentage|Maximum||Geen dimensies|
|percentProcessorTime7|Ja|CPU (Shard 7)|Percentage|Maximum||Geen dimensies|
|percentProcessorTime8|Ja|CPU (Shard 8)|Percentage|Maximum||Geen dimensies|
|percentProcessorTime9|Ja|CPU (Shard 9)|Percentage|Maximum||Geen dimensies|
|serverLoad|Ja|Serverbelasting|Percentage|Maximum||ShardId|
|serverLoad0|Ja|Server belasting (Shard 0)|Percentage|Maximum||Geen dimensies|
|serverLoad1|Ja|Server belasting (Shard 1)|Percentage|Maximum||Geen dimensies|
|serverLoad2|Ja|Server belasting (Shard 2)|Percentage|Maximum||Geen dimensies|
|serverLoad3|Ja|Server belasting (Shard 3)|Percentage|Maximum||Geen dimensies|
|serverLoad4|Ja|Server belasting (Shard 4)|Percentage|Maximum||Geen dimensies|
|serverLoad5|Ja|Server belasting (Shard 5)|Percentage|Maximum||Geen dimensies|
|serverLoad6|Ja|Server belasting (Shard 6)|Percentage|Maximum||Geen dimensies|
|serverLoad7|Ja|Server belasting (Shard 7)|Percentage|Maximum||Geen dimensies|
|serverLoad8|Ja|Server belasting (Shard 8)|Percentage|Maximum||Geen dimensies|
|serverLoad9|Ja|Server belasting (Shard 9)|Percentage|Maximum||Geen dimensies|
|setcommands|Ja|Instellingen|Aantal|Totaal||ShardId|
|setcommands0|Ja|Sets (Shard 0)|Aantal|Totaal||Geen dimensies|
|setcommands1|Ja|Sets (Shard 1)|Aantal|Totaal||Geen dimensies|
|setcommands2|Ja|Sets (Shard 2)|Aantal|Totaal||Geen dimensies|
|setcommands3|Ja|Sets (Shard 3)|Aantal|Totaal||Geen dimensies|
|setcommands4|Ja|Sets (Shard 4)|Aantal|Totaal||Geen dimensies|
|setcommands5|Ja|Sets (Shard 5)|Aantal|Totaal||Geen dimensies|
|setcommands6|Ja|Sets (Shard 6)|Aantal|Totaal||Geen dimensies|
|setcommands7|Ja|Sets (Shard 7)|Aantal|Totaal||Geen dimensies|
|setcommands8|Ja|Sets (Shard 8)|Aantal|Totaal||Geen dimensies|
|setcommands9|Ja|Sets (Shard 9)|Aantal|Totaal||Geen dimensies|
|totalcommandsprocessed|Ja|Totaalaantal bewerkingen|Aantal|Totaal||ShardId|
|totalcommandsprocessed0|Ja|Totaal aantal bewerkingen (Shard 0)|Aantal|Totaal||Geen dimensies|
|totalcommandsprocessed1|Ja|Totaal aantal bewerkingen (Shard 1)|Aantal|Totaal||Geen dimensies|
|totalcommandsprocessed2|Ja|Totaal aantal bewerkingen (Shard 2)|Aantal|Totaal||Geen dimensies|
|totalcommandsprocessed3|Ja|Totaal aantal bewerkingen (Shard 3)|Aantal|Totaal||Geen dimensies|
|totalcommandsprocessed4|Ja|Totaal aantal bewerkingen (Shard 4)|Aantal|Totaal||Geen dimensies|
|totalcommandsprocessed5|Ja|Totaal aantal bewerkingen (Shard 5)|Aantal|Totaal||Geen dimensies|
|totalcommandsprocessed6|Ja|Totaal aantal bewerkingen (Shard 6)|Aantal|Totaal||Geen dimensies|
|totalcommandsprocessed7|Ja|Totaal aantal bewerkingen (Shard 7)|Aantal|Totaal||Geen dimensies|
|totalcommandsprocessed8|Ja|Totaal aantal bewerkingen (Shard 8)|Aantal|Totaal||Geen dimensies|
|totalcommandsprocessed9|Ja|Totaal aantal bewerkingen (Shard 9)|Aantal|Totaal||Geen dimensies|
|totalkeys|Ja|Totaal aantal sleutels|Aantal|Maximum||ShardId|
|totalkeys0|Ja|Totaal aantal sleutels (Shard 0)|Aantal|Maximum||Geen dimensies|
|totalkeys1|Ja|Totaal aantal sleutels (Shard 1)|Aantal|Maximum||Geen dimensies|
|totalkeys2|Ja|Totaal aantal sleutels (Shard 2)|Aantal|Maximum||Geen dimensies|
|totalkeys3|Ja|Totaal aantal sleutels (Shard 3)|Aantal|Maximum||Geen dimensies|
|totalkeys4|Ja|Totaal aantal sleutels (Shard 4)|Aantal|Maximum||Geen dimensies|
|totalkeys5|Ja|Totaal aantal sleutels (Shard 5)|Aantal|Maximum||Geen dimensies|
|totalkeys6|Ja|Totaal aantal sleutels (Shard 6)|Aantal|Maximum||Geen dimensies|
|totalkeys7|Ja|Totaal aantal sleutels (Shard 7)|Aantal|Maximum||Geen dimensies|
|totalkeys8|Ja|Totaal aantal sleutels (Shard 8)|Aantal|Maximum||Geen dimensies|
|totalkeys9|Ja|Totaal aantal sleutels (Shard 9)|Aantal|Maximum||Geen dimensies|
|usedmemory|Ja|Gebruikt geheugen|Bytes|Maximum||ShardId|
|usedmemory0|Ja|Gebruikt geheugen (Shard 0)|Bytes|Maximum||Geen dimensies|
|usedmemory1|Ja|Gebruikt geheugen (Shard 1)|Bytes|Maximum||Geen dimensies|
|usedmemory2|Ja|Gebruikt geheugen (Shard 2)|Bytes|Maximum||Geen dimensies|
|usedmemory3|Ja|Gebruikt geheugen (Shard 3)|Bytes|Maximum||Geen dimensies|
|usedmemory4|Ja|Gebruikt geheugen (Shard 4)|Bytes|Maximum||Geen dimensies|
|usedmemory5|Ja|Gebruikt geheugen (Shard 5)|Bytes|Maximum||Geen dimensies|
|usedmemory6|Ja|Gebruikt geheugen (Shard 6)|Bytes|Maximum||Geen dimensies|
|usedmemory7|Ja|Gebruikt geheugen (Shard 7)|Bytes|Maximum||Geen dimensies|
|usedmemory8|Ja|Gebruikt geheugen (Shard 8)|Bytes|Maximum||Geen dimensies|
|usedmemory9|Ja|Gebruikt geheugen (Shard 9)|Bytes|Maximum||Geen dimensies|
|usedmemorypercentage|Ja|Percentage gebruikt geheugen|Percentage|Maximum||ShardId|
|usedmemoryRss|Ja|Gebruikte geheugen-RSS|Bytes|Maximum||ShardId|
|usedmemoryRss0|Ja|Gebruikt geheugen RSS (Shard 0)|Bytes|Maximum||Geen dimensies|
|usedmemoryRss1|Ja|Gebruikte geheugen RSS (Shard 1)|Bytes|Maximum||Geen dimensies|
|usedmemoryRss2|Ja|Gebruikte geheugen RSS (Shard 2)|Bytes|Maximum||Geen dimensies|
|usedmemoryRss3|Ja|Gebruikte geheugen RSS (Shard 3)|Bytes|Maximum||Geen dimensies|
|usedmemoryRss4|Ja|Gebruikte geheugen RSS (Shard 4)|Bytes|Maximum||Geen dimensies|
|usedmemoryRss5|Ja|Gebruikte geheugen RSS (Shard 5)|Bytes|Maximum||Geen dimensies|
|usedmemoryRss6|Ja|Gebruikte geheugen RSS (Shard 6)|Bytes|Maximum||Geen dimensies|
|usedmemoryRss7|Ja|Gebruikte geheugen RSS (Shard 7)|Bytes|Maximum||Geen dimensies|
|usedmemoryRss8|Ja|Gebruikt geheugen RSS (Shard 8)|Bytes|Maximum||Geen dimensies|
|usedmemoryRss9|Ja|Gebruikte geheugen RSS (Shard 9)|Bytes|Maximum||Geen dimensies|


## <a name="microsoftcdncdnwebapplicationfirewallpolicies"></a>Micro soft. CDN/cdnwebapplicationfirewallpolicies

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|WebApplicationFirewallRequestCount|Ja|Aantal aanvragen voor Web Application firewall|Aantal|Totaal|Het aantal client aanvragen dat is verwerkt door de Web Application firewall|Beleidsnaam, regelnaam, actie|


## <a name="microsoftclassiccomputedomainnamesslotsroles"></a>Micro soft. ClassicCompute/domein naam/sleuven/rollen

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|Gelezen bytes per seconde|Nee|Schijf lezen|BytesPerSecond|Average|Gemiddeld aantal gelezen bytes van de schijf tijdens de controle periode.|RoleInstanceId|
|Leesbewerkingen op de schijf/seconde|Ja|Leesbewerkingen op de schijf/seconde|CountPerSecond|Average|Read-IOPS van schijf.|RoleInstanceId|
|Geschreven bytes per seconde|Nee|Schijf schrijven|BytesPerSecond|Average|Gemiddeld aantal bytes dat tijdens de controle periode naar de schijf wordt geschreven.|RoleInstanceId|
|Schrijfbewerkingen op de schijf/seconde|Ja|Schrijfbewerkingen op de schijf/seconde|CountPerSecond|Average|Schijf schrijf-IOPS.|RoleInstanceId|
|Netwerk in|Ja|Netwerk in|Bytes|Totaal|Het aantal bytes dat is ontvangen op alle netwerk interfaces door de virtuele machine (s) (binnenkomend verkeer).|RoleInstanceId|
|Netwerk uit|Ja|Netwerk uit|Bytes|Totaal|Het aantal uitgaande bytes op alle netwerk interfaces door de virtuele machine (s) (uitgaand verkeer).|RoleInstanceId|
|CPU-percentage|Ja|CPU-percentage|Percentage|Average|Het percentage toegewezen reken eenheden dat momenteel wordt gebruikt door de virtuele machine (s).|RoleInstanceId|


## <a name="microsoftclassiccomputevirtualmachines"></a>Micro soft. ClassicCompute/informatie

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|Gelezen bytes per seconde|Nee|Schijf lezen|BytesPerSecond|Average|Gemiddeld aantal gelezen bytes van de schijf tijdens de controle periode.|Geen dimensies|
|Leesbewerkingen op de schijf/seconde|Ja|Leesbewerkingen op de schijf/seconde|CountPerSecond|Average|Read-IOPS van schijf.|Geen dimensies|
|Geschreven bytes per seconde|Nee|Schijf schrijven|BytesPerSecond|Average|Gemiddeld aantal bytes dat tijdens de controle periode naar de schijf wordt geschreven.|Geen dimensies|
|Schrijfbewerkingen op de schijf/seconde|Ja|Schrijfbewerkingen op de schijf/seconde|CountPerSecond|Average|Schijf schrijf-IOPS.|Geen dimensies|
|Netwerk in|Ja|Netwerk in|Bytes|Totaal|Het aantal bytes dat is ontvangen op alle netwerk interfaces door de virtuele machine (s) (binnenkomend verkeer).|Geen dimensies|
|Netwerk uit|Ja|Netwerk uit|Bytes|Totaal|Het aantal uitgaande bytes op alle netwerk interfaces door de virtuele machine (s) (uitgaand verkeer).|Geen dimensies|
|CPU-percentage|Ja|CPU-percentage|Percentage|Average|Het percentage toegewezen reken eenheden dat momenteel wordt gebruikt door de virtuele machine (s).|Geen dimensies|


## <a name="microsoftclassicstoragestorageaccounts"></a>Micro soft. ClassicStorage/Storage accounts

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|Beschikbaarheid|Ja|Beschikbaarheid|Percentage|Average|Het percentage Beschik baarheid voor de opslag service of de opgegeven API-bewerking. De beschikbaarheid wordt berekend door de waarde TotalBillableRequests te delen door het aantal van toepassing zijnde aanvragen, inclusief de aanvragen die onverwachte fouten produceren. Alle onverwachte fouten leiden tot een afgenomen beschikbaarheid voor de opslagservice of de opgegeven API-bewerking.|Geotype, ApiName, authenticatie|
|Uitgaand verkeer|Ja|Uitgaand verkeer|Bytes|Totaal|De hoeveelheid uitgangs gegevens, in bytes. Hieronder vallen de uitgaande gegevens van een externe client in Azure Storage evenals de uitgaande gegevens binnen Azure. Daarom geeft deze hoeveelheid niet de factureerbare uitgaande gegevens weer.|Geotype, ApiName, authenticatie|
|Inkomend verkeer|Ja|Inkomend verkeer|Bytes|Totaal|De hoeveelheid ingangs gegevens, in bytes. Hieronder vallen de inkomende gegevens van een externe client in Azure Storage evenals de inkomende gegevens binnen Azure.|Geotype, ApiName, authenticatie|
|SuccessE2ELatency|Ja|Success E2E Latency|Milliseconden|Average|De end-to-end latentie van geslaagde aanvragen voor een opslag service of de opgegeven API-bewerking, in milliseconden. Deze waarde bevat de vereiste verwerkingstijd in Azure Storage die nodig is om de aanvraag te lezen, het antwoord te verzenden en bevestiging van het antwoord te ontvangen.|Geotype, ApiName, authenticatie|
|SuccessServerLatency|Ja|Geslaagde serverlatentie|Milliseconden|Average|De latentie die door Azure Storage wordt gebruikt voor het verwerken van een geslaagde aanvraag, in milliseconden. Deze waarde bevat niet de netwerklatentie die is opgegeven in SuccessE2ELatency.|Geotype, ApiName, authenticatie|
|Transacties|Ja|Transacties|Aantal|Totaal|Het aantal aanvragen voor een opslagservice of de opgegeven API-bewerking. Dit is inclusief geslaagde en mislukte aanvragen, evenals aanvragen waarbij fouten zijn opgetreden. Gebruik de dimensie ResponseType voor het aantal verschillende typen reacties.|ResponseType, geotype, ApiName, authenticatie|
|UsedCapacity|Nee|Gebruikte capaciteit|Bytes|Average|Gebruikte capaciteit van account|Geen dimensies|


## <a name="microsoftclassicstoragestorageaccountsblobservices"></a>Micro soft. ClassicStorage/Storage accounts/blobServices

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|Beschikbaarheid|Ja|Beschikbaarheid|Percentage|Average|Het percentage Beschik baarheid voor de opslag service of de opgegeven API-bewerking. De beschikbaarheid wordt berekend door de waarde TotalBillableRequests te delen door het aantal van toepassing zijnde aanvragen, inclusief de aanvragen die onverwachte fouten produceren. Alle onverwachte fouten leiden tot een afgenomen beschikbaarheid voor de opslagservice of de opgegeven API-bewerking.|Geotype, ApiName, authenticatie|
|BlobCapacity|Nee|BLOB-capaciteit|Bytes|Average|De hoeveelheid opslag die wordt gebruikt door de Blob service van het opslag account in bytes.|BlobType, Tier|
|BlobCount|Nee|Aantal blobs|Aantal|Average|Het aantal blobs in de Blob service van het opslag account.|BlobType, Tier|
|ContainerCount|Ja|Aantal BLOB-containers|Aantal|Average|Het aantal containers in het Blob service van het opslag account.|Geen dimensies|
|Uitgaand verkeer|Ja|Uitgaand verkeer|Bytes|Totaal|De hoeveelheid uitgangs gegevens, in bytes. Hieronder vallen de uitgaande gegevens van een externe client in Azure Storage evenals de uitgaande gegevens binnen Azure. Daarom geeft deze hoeveelheid niet de factureerbare uitgaande gegevens weer.|Geotype, ApiName, authenticatie|
|IndexCapacity|Nee|Index capaciteit|Bytes|Average|De hoeveelheid opslag die wordt gebruikt door de index van de ADLS Gen2 (hiërarchisch) in bytes.|Geen dimensies|
|Inkomend verkeer|Ja|Inkomend verkeer|Bytes|Totaal|De hoeveelheid ingangs gegevens, in bytes. Hieronder vallen de inkomende gegevens van een externe client in Azure Storage evenals de inkomende gegevens binnen Azure.|Geotype, ApiName, authenticatie|
|SuccessE2ELatency|Ja|Success E2E Latency|Milliseconden|Average|De end-to-end latentie van geslaagde aanvragen voor een opslag service of de opgegeven API-bewerking, in milliseconden. Deze waarde bevat de vereiste verwerkingstijd in Azure Storage die nodig is om de aanvraag te lezen, het antwoord te verzenden en bevestiging van het antwoord te ontvangen.|Geotype, ApiName, authenticatie|
|SuccessServerLatency|Ja|Geslaagde serverlatentie|Milliseconden|Average|De latentie die door Azure Storage wordt gebruikt voor het verwerken van een geslaagde aanvraag, in milliseconden. Deze waarde bevat niet de netwerklatentie die is opgegeven in SuccessE2ELatency.|Geotype, ApiName, authenticatie|
|Transacties|Ja|Transacties|Aantal|Totaal|Het aantal aanvragen voor een opslagservice of de opgegeven API-bewerking. Dit is inclusief geslaagde en mislukte aanvragen, evenals aanvragen waarbij fouten zijn opgetreden. Gebruik de dimensie ResponseType voor het aantal verschillende typen reacties.|ResponseType, geotype, ApiName, authenticatie|


## <a name="microsoftclassicstoragestorageaccountsfileservices"></a>Micro soft. ClassicStorage/Storage accounts/fileServices

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|Beschikbaarheid|Ja|Beschikbaarheid|Percentage|Average|Het percentage Beschik baarheid voor de opslag service of de opgegeven API-bewerking. De beschikbaarheid wordt berekend door de waarde TotalBillableRequests te delen door het aantal van toepassing zijnde aanvragen, inclusief de aanvragen die onverwachte fouten produceren. Alle onverwachte fouten leiden tot een afgenomen beschikbaarheid voor de opslagservice of de opgegeven API-bewerking.|Geotype, ApiName, authenticatie, bestands share|
|Uitgaand verkeer|Ja|Uitgaand verkeer|Bytes|Totaal|De hoeveelheid uitgangs gegevens, in bytes. Hieronder vallen de uitgaande gegevens van een externe client in Azure Storage evenals de uitgaande gegevens binnen Azure. Daarom geeft deze hoeveelheid niet de factureerbare uitgaande gegevens weer.|Geotype, ApiName, authenticatie, bestands share|
|FileCapacity|Nee|Bestands capaciteit|Bytes|Average|De hoeveelheid opslag die wordt gebruikt door de bestands service van het opslag account in bytes.|Bestandsshare|
|FileCount|Nee|Aantal bestanden|Aantal|Average|Het aantal bestanden in de file-service van het opslag account.|Bestandsshare|
|FileShareCount|Nee|Aantal bestands shares|Aantal|Average|Het aantal bestands shares in de file-service van het opslag account.|Geen dimensies|
|FileShareQuota|Nee|Quota grootte van bestands share|Bytes|Average|De bovengrens voor de hoeveelheid opslag die kan worden gebruikt door Azure Files service in bytes.|Bestandsshare|
|FileShareSnapshotCount|Nee|Aantal moment opnamen van bestands shares|Aantal|Average|Het aantal moment opnamen dat aanwezig is op de share in de bestanden service van het opslag account.|Bestandsshare|
|FileShareSnapshotSize|Nee|Grootte van moment opname van bestands share|Bytes|Average|De hoeveelheid opslag die wordt gebruikt door de moment opnamen in de bestands service van het opslag account in bytes.|Bestandsshare|
|Inkomend verkeer|Ja|Inkomend verkeer|Bytes|Totaal|De hoeveelheid ingangs gegevens, in bytes. Hieronder vallen de inkomende gegevens van een externe client in Azure Storage evenals de inkomende gegevens binnen Azure.|Geotype, ApiName, authenticatie, bestands share|
|SuccessE2ELatency|Ja|Success E2E Latency|Milliseconden|Average|De end-to-end latentie van geslaagde aanvragen voor een opslag service of de opgegeven API-bewerking, in milliseconden. Deze waarde bevat de vereiste verwerkingstijd in Azure Storage die nodig is om de aanvraag te lezen, het antwoord te verzenden en bevestiging van het antwoord te ontvangen.|Geotype, ApiName, authenticatie, bestands share|
|SuccessServerLatency|Ja|Geslaagde serverlatentie|Milliseconden|Average|De latentie die door Azure Storage wordt gebruikt voor het verwerken van een geslaagde aanvraag, in milliseconden. Deze waarde bevat niet de netwerklatentie die is opgegeven in SuccessE2ELatency.|Geotype, ApiName, authenticatie, bestands share|
|Transacties|Ja|Transacties|Aantal|Totaal|Het aantal aanvragen voor een opslagservice of de opgegeven API-bewerking. Dit is inclusief geslaagde en mislukte aanvragen, evenals aanvragen waarbij fouten zijn opgetreden. Gebruik de dimensie ResponseType voor het aantal verschillende typen reacties.|ResponseType, geotype, ApiName, Authentication, file share|


## <a name="microsoftclassicstoragestorageaccountsqueueservices"></a>Micro soft. ClassicStorage/Storage accounts/queueServices

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|Beschikbaarheid|Ja|Beschikbaarheid|Percentage|Average|Het percentage Beschik baarheid voor de opslag service of de opgegeven API-bewerking. De beschikbaarheid wordt berekend door de waarde TotalBillableRequests te delen door het aantal van toepassing zijnde aanvragen, inclusief de aanvragen die onverwachte fouten produceren. Alle onverwachte fouten leiden tot een afgenomen beschikbaarheid voor de opslagservice of de opgegeven API-bewerking.|Geotype, ApiName, authenticatie|
|Uitgaand verkeer|Ja|Uitgaand verkeer|Bytes|Totaal|De hoeveelheid uitgangs gegevens, in bytes. Hieronder vallen de uitgaande gegevens van een externe client in Azure Storage evenals de uitgaande gegevens binnen Azure. Daarom geeft deze hoeveelheid niet de factureerbare uitgaande gegevens weer.|Geotype, ApiName, authenticatie|
|Inkomend verkeer|Ja|Inkomend verkeer|Bytes|Totaal|De hoeveelheid ingangs gegevens, in bytes. Hieronder vallen de inkomende gegevens van een externe client in Azure Storage evenals de inkomende gegevens binnen Azure.|Geotype, ApiName, authenticatie|
|QueueCapacity|Ja|Wachtrij capaciteit|Bytes|Average|De hoeveelheid opslag die wordt gebruikt door de Queue-service van het opslag account in bytes.|Geen dimensies|
|QueueCount|Ja|Aantal wachtrijen|Aantal|Average|Het aantal wacht rijen in de Queue-service van het opslag account.|Geen dimensies|
|QueueMessageCount|Ja|Aantal wachtrij berichten|Aantal|Average|Het geschatte aantal wachtrij berichten in de Queue-service van het opslag account.|Geen dimensies|
|SuccessE2ELatency|Ja|Success E2E Latency|Milliseconden|Average|De end-to-end latentie van geslaagde aanvragen voor een opslag service of de opgegeven API-bewerking, in milliseconden. Deze waarde bevat de vereiste verwerkingstijd in Azure Storage die nodig is om de aanvraag te lezen, het antwoord te verzenden en bevestiging van het antwoord te ontvangen.|Geotype, ApiName, authenticatie|
|SuccessServerLatency|Ja|Geslaagde serverlatentie|Milliseconden|Average|De latentie die door Azure Storage wordt gebruikt voor het verwerken van een geslaagde aanvraag, in milliseconden. Deze waarde bevat niet de netwerklatentie die is opgegeven in SuccessE2ELatency.|Geotype, ApiName, authenticatie|
|Transacties|Ja|Transacties|Aantal|Totaal|Het aantal aanvragen voor een opslagservice of de opgegeven API-bewerking. Dit is inclusief geslaagde en mislukte aanvragen, evenals aanvragen waarbij fouten zijn opgetreden. Gebruik de dimensie ResponseType voor het aantal verschillende typen reacties.|ResponseType, geotype, ApiName, authenticatie|


## <a name="microsoftclassicstoragestorageaccountstableservices"></a>Micro soft. ClassicStorage/Storage accounts/tableServices

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|Beschikbaarheid|Ja|Beschikbaarheid|Percentage|Average|Het percentage Beschik baarheid voor de opslag service of de opgegeven API-bewerking. De beschikbaarheid wordt berekend door de waarde TotalBillableRequests te delen door het aantal van toepassing zijnde aanvragen, inclusief de aanvragen die onverwachte fouten produceren. Alle onverwachte fouten leiden tot een afgenomen beschikbaarheid voor de opslagservice of de opgegeven API-bewerking.|Geotype, ApiName, authenticatie|
|Uitgaand verkeer|Ja|Uitgaand verkeer|Bytes|Totaal|De hoeveelheid uitgangs gegevens, in bytes. Hieronder vallen de uitgaande gegevens van een externe client in Azure Storage evenals de uitgaande gegevens binnen Azure. Daarom geeft deze hoeveelheid niet de factureerbare uitgaande gegevens weer.|Geotype, ApiName, authenticatie|
|Inkomend verkeer|Ja|Inkomend verkeer|Bytes|Totaal|De hoeveelheid ingangs gegevens, in bytes. Hieronder vallen de inkomende gegevens van een externe client in Azure Storage evenals de inkomende gegevens binnen Azure.|Geotype, ApiName, authenticatie|
|SuccessE2ELatency|Ja|Success E2E Latency|Milliseconden|Average|De end-to-end latentie van geslaagde aanvragen voor een opslag service of de opgegeven API-bewerking, in milliseconden. Deze waarde bevat de vereiste verwerkingstijd in Azure Storage die nodig is om de aanvraag te lezen, het antwoord te verzenden en bevestiging van het antwoord te ontvangen.|Geotype, ApiName, authenticatie|
|SuccessServerLatency|Ja|Geslaagde serverlatentie|Milliseconden|Average|De latentie die door Azure Storage wordt gebruikt voor het verwerken van een geslaagde aanvraag, in milliseconden. Deze waarde bevat niet de netwerklatentie die is opgegeven in SuccessE2ELatency.|Geotype, ApiName, authenticatie|
|TableCapacity|Ja|Tabel capaciteit|Bytes|Average|De hoeveelheid opslag die wordt gebruikt door de Table service van het opslag account in bytes.|Geen dimensies|
|TableCount|Ja|Aantal tabellen|Aantal|Average|Het aantal tabellen in de Table service van het opslag account.|Geen dimensies|
|TableEntityCount|Ja|Aantal tabel entiteiten|Aantal|Average|Het aantal tabel entiteiten in de Table service van het opslag account.|Geen dimensies|
|Transacties|Ja|Transacties|Aantal|Totaal|Het aantal aanvragen voor een opslagservice of de opgegeven API-bewerking. Dit is inclusief geslaagde en mislukte aanvragen, evenals aanvragen waarbij fouten zijn opgetreden. Gebruik de dimensie ResponseType voor het aantal verschillende typen reacties.|ResponseType, geotype, ApiName, authenticatie|


## <a name="microsoftcognitiveservicesaccounts"></a>Micro soft. CognitiveServices/accounts

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|BlockedCalls|Ja|Geblokkeerde aanroepen|Aantal|Totaal|Aantal aanroepen dat de frequentie of quotum limiet heeft overschreden.|ApiName, Operationname, regio|
|CharactersTrained|Ja|Getrainde tekens|Aantal|Totaal|Totaal aantal getrainde tekens.|ApiName, Operationname, regio|
|CharactersTranslated|Ja|Geconverteerde tekens|Aantal|Totaal|Totaal aantal tekens in binnenkomende-tekst aanvraag.|ApiName, Operationname, regio|
|ClientErrors|Ja|Client fouten|Aantal|Totaal|Het aantal aanroepen met een fout aan de client zijde (HTTP-antwoord code 4xx).|ApiName, Operationname, regio|
|DataIn|Ja|Gegevens in|Bytes|Totaal|Grootte van binnenkomende gegevens in bytes.|ApiName, Operationname, regio|
|DataOut|Ja|Gegevens uit|Bytes|Totaal|Grootte van uitgaande gegevens in bytes.|ApiName, Operationname, regio|
|Latentie|Ja|Latentie|Milliseconden|Average|Latentie in milliseconden.|ApiName, Operationname, regio|
|ProcessedImages|Ja|Verwerkte installatie kopieën|Aantal|Totaal| Aantal trans acties voor de verwerking van afbeeldingen.|ApiName, functienaam, UsageChannel, regio|
|ServerErrors|Ja|Server fouten|Aantal|Totaal|Het aantal aanroepen met een interne fout in de service (HTTP-antwoord code 5xx).|ApiName, Operationname, regio|
|SpeechSessionDuration|Ja|Spraak sessie duur|Seconden|Totaal|Totale duur van de spraak sessie in seconden.|ApiName, Operationname, regio|
|SuccessfulCalls|Ja|Geslaagde aanroepen|Aantal|Totaal|Aantal geslaagde aanroepen.|ApiName, Operationname, regio|
|TotalCalls|Ja|Totaal aantal aanroepen|Aantal|Totaal|Totaal aantal aanroepen.|ApiName, Operationname, regio|
|TotalErrors|Ja|Totaalaantal fouten|Aantal|Totaal|Totaal aantal aanroepen met een fout respons (HTTP-antwoord code 4xx of 5xx).|ApiName, Operationname, regio|
|TotalTokenCalls|Ja|Totaal aantal token aanroepen|Aantal|Totaal|Totaal aantal token aanroepen.|ApiName, Operationname, regio|
|TotalTransactions|Ja|Totaal aantal trans acties|Aantal|Totaal|Totaal aantal trans acties.|Geen dimensies|


## <a name="microsoftcomputevirtualmachines"></a>Microsoft.Compute/virtualMachines

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|Verbruikt CPU-tegoed|Ja|Verbruikt CPU-tegoed|Aantal|Average|Totaal aantal tegoeden dat door de virtuele machine wordt verbruikt|Geen dimensies|
|Resterend CPU-tegoed|Ja|Resterend CPU-tegoed|Aantal|Average|Totaal aantal beschik bare tegoeden voor burst|Geen dimensies|
|Wachtrijlengte van gegevensschijf|Ja|Wachtrij diepte van gegevens schijf (preview-versie)|Aantal|Average|Wachtrij diepte van gegevens schijf (of wachtrij lengte)|LUN|
|Gegevens schijf gelezen bytes per seconde|Ja|Gegevens schijf gelezen bytes per seconde (preview)|CountPerSecond|Average|Gelezen bytes per seconde van één schijf tijdens de controle periode|LUN|
|Lees bewerkingen op de gegevens schijf per seconde|Ja|Lees bewerkingen op de gegevens schijf per seconde (preview)|CountPerSecond|Average|IOPS lezen vanaf één schijf tijdens de controle periode|LUN|
|Geschreven bytes per seconde gegevens schijf|Ja|Geschreven bytes per seconde (preview) gegevens schijf|CountPerSecond|Average|Tijdens de controle periode naar één schijf geschreven bytes per seconde|LUN|
|Schrijf bewerkingen op de gegevens schijf per seconde|Ja|Schrijf bewerkingen op de gegevens schijf per seconde (preview)|CountPerSecond|Average|IOPS tijdens de controle periode van één schijf schrijven|LUN|
|Gelezen bytes op de schijf|Ja|Gelezen bytes op de schijf|Bytes|Totaal|Gelezen bytes van de schijf tijdens de controle periode|Geen dimensies|
|Leesbewerkingen op de schijf/seconde|Ja|Leesbewerkingen op de schijf/seconde|CountPerSecond|Average|Lees-IOPS schijf|Geen dimensies|
|Geschreven bytes op de schijf|Ja|Geschreven bytes op de schijf|Bytes|Totaal|Naar schijf geschreven bytes tijdens de controle periode|Geen dimensies|
|Schrijfbewerkingen op de schijf/seconde|Ja|Schrijfbewerkingen op de schijf/seconde|CountPerSecond|Average|Schijf schrijf-IOPS|Geen dimensies|
|Binnenkomende stromen|Ja|Binnenkomende stromen|Aantal|Average|Inkomende stromen zijn het aantal huidige stromen in de binnenkomende richting (verkeer dat wordt verzonden naar de virtuele machine)|Geen dimensies|
|Maximum aanmaak frequentie inkomende stromen|Ja|Maximum aanmaak frequentie inkomende stromen|CountPerSecond|Average|Het maximale aantal inkomende stromen (verkeer dat wordt verzonden naar de virtuele machine)|Geen dimensies|
|Netwerk in|Ja|Netwerk in Factureerbaar (afgeschaft)|Bytes|Totaal|Het aantal factureer bare bytes dat is ontvangen op alle netwerk interfaces door de virtuele machine (s) (binnenkomend verkeer) (afgeschaft)|Geen dimensies|
|Totaal netwerk|Ja|Totaal netwerk|Bytes|Totaal|Het aantal ontvangen bytes op alle netwerk interfaces door de virtuele machine (s) (binnenkomend verkeer)|Geen dimensies|
|Netwerk uit|Ja|Gefactureerd netwerk (afgeschaft)|Bytes|Totaal|Het aantal factureer bare bytes op alle netwerk interfaces door de virtuele machine (s) (uitgaand verkeer) (afgeschaft)|Geen dimensies|
|Totaal aantal netwerk|Ja|Totaal aantal netwerk|Bytes|Totaal|Het aantal uitgaande bytes op alle netwerk interfaces door de virtuele machine (s) (uitgaand verkeer)|Geen dimensies|
|Wachtrijlengte van besturingssysteemschijf|Ja|Wachtrij diepte van de besturingssysteem schijf (preview-versie)|Aantal|Average|Wachtrij diepte van het besturings systeem (of wachtrij lengte)|Geen dimensies|
|BESTURINGSSYSTEEM schijf gelezen bytes per seconde|Ja|BESTURINGSSYSTEEM schijf gelezen bytes per seconde (preview)|CountPerSecond|Average|Gelezen bytes per seconde van één schijf tijdens de controle periode voor de besturingssysteem schijf|Geen dimensies|
|Lees bewerkingen van de besturingssysteem schijf per seconde|Ja|Lees bewerkingen op de besturingssysteem schijf per seconde (preview)|CountPerSecond|Average|IOPS lezen vanaf één schijf tijdens de controle periode voor de besturingssysteem schijf|Geen dimensies|
|Schrijf bewerkingen op de besturingssysteem schijf per seconde|Ja|Schrijf bewerkingen op de besturingssysteem schijf per seconde (preview)|CountPerSecond|Average|Geschreven bytes per seconde tijdens de controle periode voor de besturingssysteem schijf|Geen dimensies|
|Schrijf bewerkingen op de besturingssysteem schijf per seconde|Ja|Schrijf bewerkingen op de besturingssysteem schijf per seconde (preview)|CountPerSecond|Average|IOPS tijdens de controle periode voor de besturingssysteem schijf schrijven vanaf één schijf|Geen dimensies|
|WACHTRIJ diepte voor het besturings systeem per schijf|Ja|WACHTRIJ diepte van de besturingssysteem schijf (afgeschaft)|Aantal|Average|Wachtrij diepte van het besturings systeem (of wachtrij lengte)|Geen dimensies|
|BESTURINGSSYSTEEM per schijf gelezen bytes per seconde|Ja|BESTURINGSSYSTEEM schijf gelezen bytes per seconde (afgeschaft)|CountPerSecond|Average|Gelezen bytes per seconde van één schijf tijdens de controle periode voor de besturingssysteem schijf|Geen dimensies|
|Lees bewerkingen per schijf voor het besturings systeem/sec.|Ja|Lees bewerkingen op de besturingssysteem schijf per seconde (afgeschaft)|CountPerSecond|Average|IOPS lezen vanaf één schijf tijdens de controle periode voor de besturingssysteem schijf|Geen dimensies|
|Per schijf geschreven bytes/sec.|Ja|BESTURINGSSYSTEEM schijf schrijf bewerkingen in bytes per seconde (afgeschaft)|CountPerSecond|Average|Geschreven bytes per seconde tijdens de controle periode voor de besturingssysteem schijf|Geen dimensies|
|Schrijf bewerkingen per schijf van het besturings systeem/SEC|Ja|Schrijf bewerkingen op de besturingssysteem schijf per seconde (afgeschaft)|CountPerSecond|Average|IOPS tijdens de controle periode voor de besturingssysteem schijf schrijven vanaf één schijf|Geen dimensies|
|Uitgaande stromen|Ja|Uitgaande stromen|Aantal|Average|Uitgaande stromen zijn het aantal huidige stromen in de uitgaande richting (verkeer dat uit de virtuele machine wordt verzonden)|Geen dimensies|
|Maximum aanmaak frequentie van uitgaande stromen|Ja|Maximum aanmaak frequentie van uitgaande stromen|CountPerSecond|Average|De maximale aanmaak frequentie van uitgaande stromen (verkeer dat uit de virtuele machine wordt verzonden)|Geen dimensies|
|WACHTRIJ diepte per schijf|Ja|WACHTRIJ diepte van de gegevens schijf (afgeschaft)|Aantal|Average|Wachtrij diepte van gegevens schijf (of wachtrij lengte)|SlotId|
|Per schijf gelezen bytes/sec.|Ja|Gegevens schijf gelezen bytes per seconde (afgeschaft)|CountPerSecond|Average|Gelezen bytes per seconde van één schijf tijdens de controle periode|SlotId|
|Lees bewerkingen per schijf/sec.|Ja|Lees bewerkingen op de gegevens schijf per seconde (afgeschaft)|CountPerSecond|Average|IOPS lezen vanaf één schijf tijdens de controle periode|SlotId|
|Schrijf bewerkingen per schijf/sec.|Ja|Gegevens schijf schrijf bewerkingen in bytes per seconde (afgeschaft)|CountPerSecond|Average|Tijdens de controle periode naar één schijf geschreven bytes per seconde|SlotId|
|Schrijf bewerkingen per schijf/SEC|Ja|Schrijf bewerkingen op de gegevens schijf per seconde (afgeschaft)|CountPerSecond|Average|IOPS tijdens de controle periode van één schijf schrijven|SlotId|
|CPU-percentage|Ja|CPU-percentage|Percentage|Average|Het percentage toegewezen reken eenheden dat momenteel wordt gebruikt door de virtuele machine (s)|Geen dimensies|
|Treffer voor Premium data-schijf cache lezen|Ja|Cache geheugen voor lezen van Premium-gegevens schijf (preview-versie)|Percentage|Average|Treffer voor Premium data-schijf cache lezen|LUN|
|Lees missers cache Premium-gegevens schijf|Ja|Cache voor lezen van Premium-gegevens schijf (preview)|Percentage|Average|Lees missers cache Premium-gegevens schijf|LUN|
|Treffer voor Premium-besturingssysteem schijf cache lezen|Ja|Treffer voor het lezen van een Premium-besturingssysteem schijf cache (preview-versie)|Percentage|Average|Treffer voor Premium-besturingssysteem schijf cache lezen|Geen dimensies|
|Leesmij voor Premium-besturingssysteem schijf cache lezen|Ja|Schijf cache voor Premium-Lees-missers (preview-versie)|Percentage|Average|Leesmij voor Premium-besturingssysteem schijf cache lezen|Geen dimensies|


## <a name="microsoftcomputevirtualmachinescalesets"></a>Microsoft.Compute/virtualMachineScaleSets

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|Verbruikt CPU-tegoed|Ja|Verbruikt CPU-tegoed|Aantal|Average|Totaal aantal tegoeden dat door de virtuele machine wordt verbruikt|Geen dimensies|
|Resterend CPU-tegoed|Ja|Resterend CPU-tegoed|Aantal|Average|Totaal aantal beschik bare tegoeden voor burst|Geen dimensies|
|Wachtrijlengte van gegevensschijf|Ja|Wachtrij diepte van gegevens schijf (preview-versie)|Aantal|Average|Wachtrij diepte van gegevens schijf (of wachtrij lengte)|LUN, VMName|
|Gegevens schijf gelezen bytes per seconde|Ja|Gegevens schijf gelezen bytes per seconde (preview)|CountPerSecond|Average|Gelezen bytes per seconde van één schijf tijdens de controle periode|LUN, VMName|
|Lees bewerkingen op de gegevens schijf per seconde|Ja|Lees bewerkingen op de gegevens schijf per seconde (preview)|CountPerSecond|Average|IOPS lezen vanaf één schijf tijdens de controle periode|LUN, VMName|
|Geschreven bytes per seconde gegevens schijf|Ja|Geschreven bytes per seconde (preview) gegevens schijf|CountPerSecond|Average|Tijdens de controle periode naar één schijf geschreven bytes per seconde|LUN, VMName|
|Schrijf bewerkingen op de gegevens schijf per seconde|Ja|Schrijf bewerkingen op de gegevens schijf per seconde (preview)|CountPerSecond|Average|IOPS tijdens de controle periode van één schijf schrijven|LUN, VMName|
|Gelezen bytes op de schijf|Ja|Gelezen bytes op de schijf|Bytes|Totaal|Gelezen bytes van de schijf tijdens de controle periode|VMName|
|Leesbewerkingen op de schijf/seconde|Ja|Leesbewerkingen op de schijf/seconde|CountPerSecond|Average|Lees-IOPS schijf|VMName|
|Geschreven bytes op de schijf|Ja|Geschreven bytes op de schijf|Bytes|Totaal|Naar schijf geschreven bytes tijdens de controle periode|VMName|
|Schrijfbewerkingen op de schijf/seconde|Ja|Schrijfbewerkingen op de schijf/seconde|CountPerSecond|Average|Schijf schrijf-IOPS|VMName|
|Binnenkomende stromen|Ja|Binnenkomende stromen|Aantal|Average|Inkomende stromen zijn het aantal huidige stromen in de binnenkomende richting (verkeer dat wordt verzonden naar de virtuele machine)|VMName|
|Maximum aanmaak frequentie inkomende stromen|Ja|Maximum aanmaak frequentie inkomende stromen|CountPerSecond|Average|Het maximale aantal inkomende stromen (verkeer dat wordt verzonden naar de virtuele machine)|VMName|
|Netwerk in|Ja|Netwerk in Factureerbaar (afgeschaft)|Bytes|Totaal|Het aantal factureer bare bytes dat is ontvangen op alle netwerk interfaces door de virtuele machine (s) (binnenkomend verkeer) (afgeschaft)|VMName|
|Totaal netwerk|Ja|Totaal netwerk|Bytes|Totaal|Het aantal ontvangen bytes op alle netwerk interfaces door de virtuele machine (s) (binnenkomend verkeer)|VMName|
|Netwerk uit|Ja|Gefactureerd netwerk (afgeschaft)|Bytes|Totaal|Het aantal factureer bare bytes op alle netwerk interfaces door de virtuele machine (s) (uitgaand verkeer) (afgeschaft)|VMName|
|Totaal aantal netwerk|Ja|Totaal aantal netwerk|Bytes|Totaal|Het aantal uitgaande bytes op alle netwerk interfaces door de virtuele machine (s) (uitgaand verkeer)|VMName|
|Wachtrijlengte van besturingssysteemschijf|Ja|Wachtrij diepte van de besturingssysteem schijf (preview-versie)|Aantal|Average|Wachtrij diepte van het besturings systeem (of wachtrij lengte)|VMName|
|BESTURINGSSYSTEEM schijf gelezen bytes per seconde|Ja|BESTURINGSSYSTEEM schijf gelezen bytes per seconde (preview)|CountPerSecond|Average|Gelezen bytes per seconde van één schijf tijdens de controle periode voor de besturingssysteem schijf|VMName|
|Lees bewerkingen van de besturingssysteem schijf per seconde|Ja|Lees bewerkingen op de besturingssysteem schijf per seconde (preview)|CountPerSecond|Average|IOPS lezen vanaf één schijf tijdens de controle periode voor de besturingssysteem schijf|VMName|
|Schrijf bewerkingen op de besturingssysteem schijf per seconde|Ja|Schrijf bewerkingen op de besturingssysteem schijf per seconde (preview)|CountPerSecond|Average|Geschreven bytes per seconde tijdens de controle periode voor de besturingssysteem schijf|VMName|
|Schrijf bewerkingen op de besturingssysteem schijf per seconde|Ja|Schrijf bewerkingen op de besturingssysteem schijf per seconde (preview)|CountPerSecond|Average|IOPS tijdens de controle periode voor de besturingssysteem schijf schrijven vanaf één schijf|VMName|
|WACHTRIJ diepte voor het besturings systeem per schijf|Ja|WACHTRIJ diepte van de besturingssysteem schijf (afgeschaft)|Aantal|Average|Wachtrij diepte van het besturings systeem (of wachtrij lengte)|Geen dimensies|
|BESTURINGSSYSTEEM per schijf gelezen bytes per seconde|Ja|BESTURINGSSYSTEEM schijf gelezen bytes per seconde (afgeschaft)|CountPerSecond|Average|Gelezen bytes per seconde van één schijf tijdens de controle periode voor de besturingssysteem schijf|Geen dimensies|
|Lees bewerkingen per schijf voor het besturings systeem/sec.|Ja|Lees bewerkingen op de besturingssysteem schijf per seconde (afgeschaft)|CountPerSecond|Average|IOPS lezen vanaf één schijf tijdens de controle periode voor de besturingssysteem schijf|Geen dimensies|
|Per schijf geschreven bytes/sec.|Ja|BESTURINGSSYSTEEM schijf schrijf bewerkingen in bytes per seconde (afgeschaft)|CountPerSecond|Average|Geschreven bytes per seconde tijdens de controle periode voor de besturingssysteem schijf|Geen dimensies|
|Schrijf bewerkingen per schijf van het besturings systeem/SEC|Ja|Schrijf bewerkingen op de besturingssysteem schijf per seconde (afgeschaft)|CountPerSecond|Average|IOPS tijdens de controle periode voor de besturingssysteem schijf schrijven vanaf één schijf|Geen dimensies|
|Uitgaande stromen|Ja|Uitgaande stromen|Aantal|Average|Uitgaande stromen zijn het aantal huidige stromen in de uitgaande richting (verkeer dat uit de virtuele machine wordt verzonden)|VMName|
|Maximum aanmaak frequentie van uitgaande stromen|Ja|Maximum aanmaak frequentie van uitgaande stromen|CountPerSecond|Average|De maximale aanmaak frequentie van uitgaande stromen (verkeer dat uit de virtuele machine wordt verzonden)|VMName|
|WACHTRIJ diepte per schijf|Ja|WACHTRIJ diepte van de gegevens schijf (afgeschaft)|Aantal|Average|Wachtrij diepte van gegevens schijf (of wachtrij lengte)|SlotId|
|Per schijf gelezen bytes/sec.|Ja|Gegevens schijf gelezen bytes per seconde (afgeschaft)|CountPerSecond|Average|Gelezen bytes per seconde van één schijf tijdens de controle periode|SlotId|
|Lees bewerkingen per schijf/sec.|Ja|Lees bewerkingen op de gegevens schijf per seconde (afgeschaft)|CountPerSecond|Average|IOPS lezen vanaf één schijf tijdens de controle periode|SlotId|
|Schrijf bewerkingen per schijf/sec.|Ja|Gegevens schijf schrijf bewerkingen in bytes per seconde (afgeschaft)|CountPerSecond|Average|Tijdens de controle periode naar één schijf geschreven bytes per seconde|SlotId|
|Schrijf bewerkingen per schijf/SEC|Ja|Schrijf bewerkingen op de gegevens schijf per seconde (afgeschaft)|CountPerSecond|Average|IOPS tijdens de controle periode van één schijf schrijven|SlotId|
|CPU-percentage|Ja|CPU-percentage|Percentage|Average|Het percentage toegewezen reken eenheden dat momenteel wordt gebruikt door de virtuele machine (s)|VMName|
|Treffer voor Premium data-schijf cache lezen|Ja|Cache geheugen voor lezen van Premium-gegevens schijf (preview-versie)|Percentage|Average|Treffer voor Premium data-schijf cache lezen|LUN, VMName|
|Lees missers cache Premium-gegevens schijf|Ja|Cache voor lezen van Premium-gegevens schijf (preview)|Percentage|Average|Lees missers cache Premium-gegevens schijf|LUN, VMName|
|Treffer voor Premium-besturingssysteem schijf cache lezen|Ja|Treffer voor het lezen van een Premium-besturingssysteem schijf cache (preview-versie)|Percentage|Average|Treffer voor Premium-besturingssysteem schijf cache lezen|VMName|
|Leesmij voor Premium-besturingssysteem schijf cache lezen|Ja|Schijf cache voor Premium-Lees-missers (preview-versie)|Percentage|Average|Leesmij voor Premium-besturingssysteem schijf cache lezen|VMName|


## <a name="microsoftcomputevirtualmachinescalesetsvirtualmachines"></a>Micro soft. Compute/virtualMachineScaleSets/informatie

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|Verbruikt CPU-tegoed|Ja|Verbruikt CPU-tegoed|Aantal|Average|Totaal aantal tegoeden dat door de virtuele machine wordt verbruikt|Geen dimensies|
|Resterend CPU-tegoed|Ja|Resterend CPU-tegoed|Aantal|Average|Totaal aantal beschik bare tegoeden voor burst|Geen dimensies|
|Wachtrijlengte van gegevensschijf|Ja|Wachtrij diepte van gegevens schijf (preview-versie)|Aantal|Average|Wachtrij diepte van gegevens schijf (of wachtrij lengte)|LUN|
|Gegevens schijf gelezen bytes per seconde|Ja|Gegevens schijf gelezen bytes per seconde (preview)|CountPerSecond|Average|Gelezen bytes per seconde van één schijf tijdens de controle periode|LUN|
|Lees bewerkingen op de gegevens schijf per seconde|Ja|Lees bewerkingen op de gegevens schijf per seconde (preview)|CountPerSecond|Average|IOPS lezen vanaf één schijf tijdens de controle periode|LUN|
|Geschreven bytes per seconde gegevens schijf|Ja|Geschreven bytes per seconde (preview) gegevens schijf|CountPerSecond|Average|Tijdens de controle periode naar één schijf geschreven bytes per seconde|LUN|
|Schrijf bewerkingen op de gegevens schijf per seconde|Ja|Schrijf bewerkingen op de gegevens schijf per seconde (preview)|CountPerSecond|Average|IOPS tijdens de controle periode van één schijf schrijven|LUN|
|Gelezen bytes op de schijf|Ja|Gelezen bytes op de schijf|Bytes|Totaal|Gelezen bytes van de schijf tijdens de controle periode|Geen dimensies|
|Leesbewerkingen op de schijf/seconde|Ja|Leesbewerkingen op de schijf/seconde|CountPerSecond|Average|Lees-IOPS schijf|Geen dimensies|
|Geschreven bytes op de schijf|Ja|Geschreven bytes op de schijf|Bytes|Totaal|Naar schijf geschreven bytes tijdens de controle periode|Geen dimensies|
|Schrijfbewerkingen op de schijf/seconde|Ja|Schrijfbewerkingen op de schijf/seconde|CountPerSecond|Average|Schijf schrijf-IOPS|Geen dimensies|
|Binnenkomende stromen|Ja|Binnenkomende stromen|Aantal|Average|Inkomende stromen zijn het aantal huidige stromen in de binnenkomende richting (verkeer dat wordt verzonden naar de virtuele machine)|Geen dimensies|
|Maximum aanmaak frequentie inkomende stromen|Ja|Maximum aanmaak frequentie inkomende stromen|CountPerSecond|Average|Het maximale aantal inkomende stromen (verkeer dat wordt verzonden naar de virtuele machine)|Geen dimensies|
|Netwerk in|Ja|Netwerk in Factureerbaar (afgeschaft)|Bytes|Totaal|Het aantal factureer bare bytes dat is ontvangen op alle netwerk interfaces door de virtuele machine (s) (binnenkomend verkeer) (afgeschaft)|Geen dimensies|
|Totaal netwerk|Ja|Totaal netwerk|Bytes|Totaal|Het aantal ontvangen bytes op alle netwerk interfaces door de virtuele machine (s) (binnenkomend verkeer)|Geen dimensies|
|Netwerk uit|Ja|Gefactureerd netwerk (afgeschaft)|Bytes|Totaal|Het aantal factureer bare bytes op alle netwerk interfaces door de virtuele machine (s) (uitgaand verkeer) (afgeschaft)|Geen dimensies|
|Totaal aantal netwerk|Ja|Totaal aantal netwerk|Bytes|Totaal|Het aantal uitgaande bytes op alle netwerk interfaces door de virtuele machine (s) (uitgaand verkeer)|Geen dimensies|
|Wachtrijlengte van besturingssysteemschijf|Ja|Wachtrij diepte van de besturingssysteem schijf (preview-versie)|Aantal|Average|Wachtrij diepte van het besturings systeem (of wachtrij lengte)|Geen dimensies|
|BESTURINGSSYSTEEM schijf gelezen bytes per seconde|Ja|BESTURINGSSYSTEEM schijf gelezen bytes per seconde (preview)|CountPerSecond|Average|Gelezen bytes per seconde van één schijf tijdens de controle periode voor de besturingssysteem schijf|Geen dimensies|
|Lees bewerkingen van de besturingssysteem schijf per seconde|Ja|Lees bewerkingen op de besturingssysteem schijf per seconde (preview)|CountPerSecond|Average|IOPS lezen vanaf één schijf tijdens de controle periode voor de besturingssysteem schijf|Geen dimensies|
|Schrijf bewerkingen op de besturingssysteem schijf per seconde|Ja|Schrijf bewerkingen op de besturingssysteem schijf per seconde (preview)|CountPerSecond|Average|Geschreven bytes per seconde tijdens de controle periode voor de besturingssysteem schijf|Geen dimensies|
|Schrijf bewerkingen op de besturingssysteem schijf per seconde|Ja|Schrijf bewerkingen op de besturingssysteem schijf per seconde (preview)|CountPerSecond|Average|IOPS tijdens de controle periode voor de besturingssysteem schijf schrijven vanaf één schijf|Geen dimensies|
|WACHTRIJ diepte voor het besturings systeem per schijf|Ja|WACHTRIJ diepte van de besturingssysteem schijf (afgeschaft)|Aantal|Average|Wachtrij diepte van het besturings systeem (of wachtrij lengte)|Geen dimensies|
|BESTURINGSSYSTEEM per schijf gelezen bytes per seconde|Ja|BESTURINGSSYSTEEM schijf gelezen bytes per seconde (afgeschaft)|CountPerSecond|Average|Gelezen bytes per seconde van één schijf tijdens de controle periode voor de besturingssysteem schijf|Geen dimensies|
|Lees bewerkingen per schijf voor het besturings systeem/sec.|Ja|Lees bewerkingen op de besturingssysteem schijf per seconde (afgeschaft)|CountPerSecond|Average|IOPS lezen vanaf één schijf tijdens de controle periode voor de besturingssysteem schijf|Geen dimensies|
|Per schijf geschreven bytes/sec.|Ja|BESTURINGSSYSTEEM schijf schrijf bewerkingen in bytes per seconde (afgeschaft)|CountPerSecond|Average|Geschreven bytes per seconde tijdens de controle periode voor de besturingssysteem schijf|Geen dimensies|
|Schrijf bewerkingen per schijf van het besturings systeem/SEC|Ja|Schrijf bewerkingen op de besturingssysteem schijf per seconde (afgeschaft)|CountPerSecond|Average|IOPS tijdens de controle periode voor de besturingssysteem schijf schrijven vanaf één schijf|Geen dimensies|
|Uitgaande stromen|Ja|Uitgaande stromen|Aantal|Average|Uitgaande stromen zijn het aantal huidige stromen in de uitgaande richting (verkeer dat uit de virtuele machine wordt verzonden)|Geen dimensies|
|Maximum aanmaak frequentie van uitgaande stromen|Ja|Maximum aanmaak frequentie van uitgaande stromen|CountPerSecond|Average|De maximale aanmaak frequentie van uitgaande stromen (verkeer dat uit de virtuele machine wordt verzonden)|Geen dimensies|
|WACHTRIJ diepte per schijf|Ja|WACHTRIJ diepte van de gegevens schijf (afgeschaft)|Aantal|Average|Wachtrij diepte van gegevens schijf (of wachtrij lengte)|SlotId|
|Per schijf gelezen bytes/sec.|Ja|Gegevens schijf gelezen bytes per seconde (afgeschaft)|CountPerSecond|Average|Gelezen bytes per seconde van één schijf tijdens de controle periode|SlotId|
|Lees bewerkingen per schijf/sec.|Ja|Lees bewerkingen op de gegevens schijf per seconde (afgeschaft)|CountPerSecond|Average|IOPS lezen vanaf één schijf tijdens de controle periode|SlotId|
|Schrijf bewerkingen per schijf/sec.|Ja|Gegevens schijf schrijf bewerkingen in bytes per seconde (afgeschaft)|CountPerSecond|Average|Tijdens de controle periode naar één schijf geschreven bytes per seconde|SlotId|
|Schrijf bewerkingen per schijf/SEC|Ja|Schrijf bewerkingen op de gegevens schijf per seconde (afgeschaft)|CountPerSecond|Average|IOPS tijdens de controle periode van één schijf schrijven|SlotId|
|CPU-percentage|Ja|CPU-percentage|Percentage|Average|Het percentage toegewezen reken eenheden dat momenteel wordt gebruikt door de virtuele machine (s)|Geen dimensies|
|Treffer voor Premium data-schijf cache lezen|Ja|Cache geheugen voor lezen van Premium-gegevens schijf (preview-versie)|Percentage|Average|Treffer voor Premium data-schijf cache lezen|LUN|
|Lees missers cache Premium-gegevens schijf|Ja|Cache voor lezen van Premium-gegevens schijf (preview)|Percentage|Average|Lees missers cache Premium-gegevens schijf|LUN|
|Treffer voor Premium-besturingssysteem schijf cache lezen|Ja|Treffer voor het lezen van een Premium-besturingssysteem schijf cache (preview-versie)|Percentage|Average|Treffer voor Premium-besturingssysteem schijf cache lezen|Geen dimensies|
|Leesmij voor Premium-besturingssysteem schijf cache lezen|Ja|Schijf cache voor Premium-Lees-missers (preview-versie)|Percentage|Average|Leesmij voor Premium-besturingssysteem schijf cache lezen|Geen dimensies|


## <a name="microsoftcontainerinstancecontainergroups"></a>Micro soft. ContainerInstance/containerGroups

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|CpuUsage|Ja|CPU-gebruik|Aantal|Average|CPU-gebruik op alle kernen in millicores.|containerName|
|MemoryUsage|Ja|Geheugengebruik|Bytes|Average|Totaal geheugen gebruik in bytes.|containerName|
|NetworkBytesReceivedPerSecond|Ja|Ontvangen netwerk bytes per seconde|Bytes|Average|Het netwerk ontvangen bytes per seconde.|Geen dimensies|
|NetworkBytesTransmittedPerSecond|Ja|Verzonden netwerk bytes per seconde|Bytes|Average|Het netwerk aantal verzonden bytes per seconde.|Geen dimensies|


## <a name="microsoftcontainerregistryregistries"></a>Micro soft. ContainerRegistry/registers

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|AgentPoolCPUTime|Ja|Agent pool CPU-tijd|Seconden|Totaal|Agent pool CPU-tijd in seconden|Geen dimensies|
|RunDuration|Ja|Uitvoerings duur|Milliseconden|Totaal|Uitvoerings duur in milliseconden|Geen dimensies|
|SuccessfulPullCount|Ja|Aantal geslaagde pull-bewerkingen|Aantal|Average|Aantal geslaagde installatie kopieën|Geen dimensies|
|SuccessfulPushCount|Ja|Aantal geslaagde push berichten|Aantal|Average|Aantal geslaagde pushes voor installatie kopie|Geen dimensies|
|TotalPullCount|Ja|Totaal aantal pull-bewerkingen|Aantal|Average|Aantal opgehaalde afbeeldingen in totaal|Geen dimensies|
|TotalPushCount|Ja|Totaal aantal push berichten|Aantal|Average|Aantal afbeeldings pushes in totaal|Geen dimensies|


## <a name="microsoftcontainerservicemanagedclusters"></a>Micro soft. container service/managedClusters

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|kube_node_status_allocatable_cpu_cores|Nee|Totaal aantal beschik bare CPU-kernen in een beheerd cluster|Aantal|Average|Totaal aantal beschik bare CPU-kernen in een beheerd cluster|Geen dimensies|
|kube_node_status_allocatable_memory_bytes|Nee|Totale hoeveelheid beschikbaar geheugen in een beheerd cluster|Bytes|Average|Totale hoeveelheid beschikbaar geheugen in een beheerd cluster|Geen dimensies|
|kube_node_status_condition|Nee|Statussen voor de verschillende knooppunt voorwaarden|Aantal|Average|Statussen voor de verschillende knooppunt voorwaarden|voor waarde, status, status2, knoop punt|
|kube_pod_status_phase|Nee|Aantal per fase|Aantal|Average|Aantal per fase|fase, naam ruimte, pod|
|kube_pod_status_ready|Nee|Aantal in de status gereed|Aantal|Average|Aantal in de status gereed|naam ruimte, pod|


## <a name="microsoftcustomprovidersresourceproviders"></a>Micro soft. CustomProviders/resourceproviders

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|FailedRequests|Ja|Mislukte aanvragen|Aantal|Totaal|Hiermee worden de beschik bare logboeken voor aangepaste resource providers opgehaald|HttpMethod, CallPath, status code|
|SuccessfullRequests|Ja|Geslaagde aanvragen|Aantal|Totaal|Geslaagde aanvragen van de aangepaste provider|HttpMethod, CallPath, status code|


## <a name="microsoftdataboxedgedataboxedgedevices"></a>Micro soft. DataBoxEdge/dataBoxEdgeDevices

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|Availablecapacity;)|Ja|Beschik bare capaciteit|Bytes|Average|De beschik bare capaciteit in bytes tijdens de rapportage periode.|Geen dimensies|
|BytesUploadedToCloud|Ja|Geüploade Cloud bytes (apparaat)|Bytes|Average|Het totale aantal bytes dat tijdens de rapportage periode naar Azure wordt geüpload vanaf een apparaat.|Geen dimensies|
|BytesUploadedToCloudPerShare|Ja|Geüploade Cloud bytes (delen)|Bytes|Average|Het totaal aantal bytes dat is geüpload naar Azure vanaf een share tijdens de rapportage periode.|Delen|
|CloudReadThroughput|Ja|Door Voer van Cloud downloaden|BytesPerSecond|Average|De door Voer van de Cloud downloadt naar Azure tijdens de rapportage periode.|Geen dimensies|
|CloudReadThroughputPerShare|Ja|Door Voer van Cloud downloaden (delen)|BytesPerSecond|Average|De door Voer van downloaden naar Azure vanaf een share tijdens de rapportage periode.|Delen|
|CloudUploadThroughput|Ja|Upload doorvoer van Cloud|BytesPerSecond|Average|De upload doorvoer van de Cloud naar Azure tijdens de rapportage periode.|Geen dimensies|
|CloudUploadThroughputPerShare|Ja|Upload doorvoer van Cloud (delen)|BytesPerSecond|Average|De upload doorvoer naar Azure vanaf een share tijdens de rapportage periode.|Delen|
|HyperVMemoryUtilization|Ja|Edge Compute-geheugen gebruik|Percentage|Average|Hoeveelheid RAM-geheugen in gebruik|InstanceName|
|HyperVVirtualProcessorUtilization|Ja|Edge Compute-percentage CPU|Percentage|Average|Percentage CPU-gebruik|InstanceName|
|NICReadThroughput|Ja|Lees doorvoer (netwerk)|BytesPerSecond|Average|De Lees doorvoer van de netwerk interface op het apparaat in de rapportage periode voor alle volumes in de gateway.|InstanceName|
|NICWriteThroughput|Ja|Schrijf doorvoer (netwerk)|BytesPerSecond|Average|De schrijf doorvoer van de netwerk interface op het apparaat in de rapportage periode voor alle volumes in de gateway.|InstanceName|
|TotalCapacity|Ja|Totale capaciteit|Bytes|Average|Totale capaciteit|Geen dimensies|


## <a name="microsoftdatafactorydatafactories"></a>Micro soft. DataFactory/datafactories

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|FailedRuns|Ja|Mislukte uitvoeringen|Aantal|Totaal||pipelineName, activiteitsnummer|
|SuccessfulRuns|Ja|Geslaagde uitvoeringen|Aantal|Totaal||pipelineName, activiteitsnummer|


## <a name="microsoftdatafactoryfactories"></a>Micro soft. DataFactory/fabrieken

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|ActivityCancelledRuns|Ja|Metrische gegevens voor geannuleerde activiteit uitgevoerd|Aantal|Totaal||Activity type, PipelineName, FailureType, name|
|ActivityFailedRuns|Ja|Metrische gegevens mislukte uitvoering van activiteit|Aantal|Totaal||Activity type, PipelineName, FailureType, name|
|ActivitySucceededRuns|Ja|Metrische gegevens uitvoeringen uitgevoerde activiteit|Aantal|Totaal||Activity type, PipelineName, FailureType, name|
|FactorySizeInGbUnits|Ja|Totale grootte van de fabriek (GB-eenheid)|Aantal|Maximum||Geen dimensies|
|IntegrationRuntimeAvailableMemory|Ja|Beschik bare geheugen voor Integration runtime|Bytes|Average||IntegrationRuntimeName, knooppuntnaam|
|IntegrationRuntimeAvailableNodeNumber|Ja|Aantal beschik bare knoop punten voor Integration runtime|Aantal|Average||IntegrationRuntimeName|
|IntegrationRuntimeAverageTaskPickupDelay|Ja|Duur van de wachtrij voor Integration runtime|Seconden|Average||IntegrationRuntimeName|
|IntegrationRuntimeCpuPercentage|Ja|CPU-gebruik van Integration runtime|Percentage|Average||IntegrationRuntimeName, knooppuntnaam|
|IntegrationRuntimeQueueLength|Ja|Lengte van de wachtrij voor Integration runtime|Aantal|Average||IntegrationRuntimeName|
|MaxAllowedFactorySizeInGbUnits|Ja|Maxi maal toegestane grootte van de fabriek (GB-eenheid)|Aantal|Maximum||Geen dimensies|
|MaxAllowedResourceCount|Ja|Maximum aantal toegestane entiteiten|Aantal|Maximum||Geen dimensies|
|PipelineCancelledRuns|Ja|Metrische gegevens van de pijplijn uitvoeringen geannuleerd|Aantal|Totaal||FailureType, naam|
|PipelineFailedRuns|Ja|Metrische gegevens van mislukte pijplijn uitvoeringen|Aantal|Totaal||FailureType, naam|
|PipelineSucceededRuns|Ja|Metrische uitvoerings metingen geslaagde pijp lijnen|Aantal|Totaal||FailureType, naam|
|ResourceCount|Ja|Totaal aantal entiteiten|Aantal|Maximum||Geen dimensies|
|TriggerCancelledRuns|Ja|Metrische gegevens over de trigger is geannuleerd|Aantal|Totaal||Naam, FailureType|
|TriggerFailedRuns|Ja|Meet waarden voor uitvoering van mislukte triggers|Aantal|Totaal||Naam, FailureType|
|TriggerSucceededRuns|Ja|Meet waarden voor uitvoering van geslaagde triggers|Aantal|Totaal||Naam, FailureType|


## <a name="microsoftdatalakestoreaccounts"></a>Micro soft. data Lake Store/accounts

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|DataRead|Ja|Gegevens lezen|Bytes|Totaal|De totale hoeveelheid gegevens die uit het account is gelezen.|Geen dimensies|
|DataWritten|Ja|Gegevens geschreven|Bytes|Totaal|De totale hoeveelheid gegevens die naar het account wordt geschreven.|Geen dimensies|
|ReadRequests|Ja|Aanvragen lezen|Aantal|Totaal|Aantal aanvragen voor het lezen van gegevens voor het account.|Geen dimensies|
|TotalStorage|Ja|Totale opslagruimte|Bytes|Maximum|De totale hoeveelheid gegevens die in het account is opgeslagen.|Geen dimensies|
|WriteRequests|Ja|Aanvragen schrijven|Aantal|Totaal|Het aantal aanvragen voor het schrijven van gegevens naar het account.|Geen dimensies|


## <a name="microsoftdbformariadbservers"></a>Microsoft.DBforMariaDB/servers

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|active_connections|Ja|Actieve verbindingen|Aantal|Average|Actieve verbindingen|Geen dimensies|
|backup_storage_used|Ja|Gebruikte back-upopslag|Bytes|Average|Gebruikte back-upopslag|Geen dimensies|
|connections_failed|Ja|Mislukte verbindingen|Aantal|Totaal|Mislukte verbindingen|Geen dimensies|
|cpu_percent|Ja|CPU-percentage|Percentage|Average|CPU-percentage|Geen dimensies|
|io_consumption_percent|Ja|IO-percentage|Percentage|Average|IO-percentage|Geen dimensies|
|memory_percent|Ja|Geheugen percentage|Percentage|Average|Geheugen percentage|Geen dimensies|
|network_bytes_egress|Ja|Netwerk uit|Bytes|Totaal|Netwerk uit over actieve verbindingen|Geen dimensies|
|network_bytes_ingress|Ja|Netwerk in|Bytes|Totaal|Netwerk in meerdere actieve verbindingen|Geen dimensies|
|seconds_behind_master|Ja|Replicatie vertraging in seconden|Aantal|Maximum|Replicatie vertraging in seconden|Geen dimensies|
|serverlog_storage_limit|Ja|Opslag limiet voor server logboek|Bytes|Average|Opslag limiet voor server logboek|Geen dimensies|
|serverlog_storage_percent|Ja|Percentage server logboek opslag|Percentage|Average|Percentage server logboek opslag|Geen dimensies|
|serverlog_storage_usage|Ja|Gebruikte server logboek opslag|Bytes|Average|Gebruikte server logboek opslag|Geen dimensies|
|storage_limit|Ja|Opslag limiet|Bytes|Maximum|Opslag limiet|Geen dimensies|
|storage_percent|Ja|Opslag percentage|Percentage|Average|Opslag percentage|Geen dimensies|
|storage_used|Ja|Gebruikte opslag|Bytes|Average|Gebruikte opslag|Geen dimensies|


## <a name="microsoftdbformysqlservers"></a>Microsoft.DBforMySQL/servers

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|active_connections|Ja|Actieve verbindingen|Aantal|Average|Actieve verbindingen|Geen dimensies|
|backup_storage_used|Ja|Gebruikte back-upopslag|Bytes|Average|Gebruikte back-upopslag|Geen dimensies|
|connections_failed|Ja|Mislukte verbindingen|Aantal|Totaal|Mislukte verbindingen|Geen dimensies|
|cpu_percent|Ja|CPU-percentage|Percentage|Average|CPU-percentage|Geen dimensies|
|io_consumption_percent|Ja|IO-percentage|Percentage|Average|IO-percentage|Geen dimensies|
|memory_percent|Ja|Geheugen percentage|Percentage|Average|Geheugen percentage|Geen dimensies|
|network_bytes_egress|Ja|Netwerk uit|Bytes|Totaal|Netwerk uit over actieve verbindingen|Geen dimensies|
|network_bytes_ingress|Ja|Netwerk in|Bytes|Totaal|Netwerk in meerdere actieve verbindingen|Geen dimensies|
|seconds_behind_master|Ja|Replicatie vertraging in seconden|Aantal|Maximum|Replicatie vertraging in seconden|Geen dimensies|
|serverlog_storage_limit|Ja|Opslag limiet voor server logboek|Bytes|Maximum|Opslag limiet voor server logboek|Geen dimensies|
|serverlog_storage_percent|Ja|Percentage server logboek opslag|Percentage|Average|Percentage server logboek opslag|Geen dimensies|
|serverlog_storage_usage|Ja|Gebruikte server logboek opslag|Bytes|Average|Gebruikte server logboek opslag|Geen dimensies|
|storage_limit|Ja|Opslag limiet|Bytes|Maximum|Opslag limiet|Geen dimensies|
|storage_percent|Ja|Opslag percentage|Percentage|Average|Opslag percentage|Geen dimensies|
|storage_used|Ja|Gebruikte opslag|Bytes|Average|Gebruikte opslag|Geen dimensies|


## <a name="microsoftdbforpostgresqlservers"></a>Microsoft.DBforPostgreSQL/servers

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|active_connections|Ja|Actieve verbindingen|Aantal|Average|Actieve verbindingen|Geen dimensies|
|backup_storage_used|Ja|Gebruikte back-upopslag|Bytes|Average|Gebruikte back-upopslag|Geen dimensies|
|connections_failed|Ja|Mislukte verbindingen|Aantal|Totaal|Mislukte verbindingen|Geen dimensies|
|cpu_percent|Ja|CPU-percentage|Percentage|Average|CPU-percentage|Geen dimensies|
|io_consumption_percent|Ja|IO-percentage|Percentage|Average|IO-percentage|Geen dimensies|
|memory_percent|Ja|Geheugen percentage|Percentage|Average|Geheugen percentage|Geen dimensies|
|network_bytes_egress|Ja|Netwerk uit|Bytes|Totaal|Netwerk uit over actieve verbindingen|Geen dimensies|
|network_bytes_ingress|Ja|Netwerk in|Bytes|Totaal|Netwerk in meerdere actieve verbindingen|Geen dimensies|
|pg_replica_log_delay_in_bytes|Ja|Maximale vertraging in Replica's|Bytes|Maximum|Vertraging in bytes van de meest vertraagde replica|Geen dimensies|
|pg_replica_log_delay_in_seconds|Ja|Replica vertraging|Seconden|Maximum|Replica vertraging in seconden|Geen dimensies|
|serverlog_storage_limit|Ja|Opslag limiet voor server logboek|Bytes|Maximum|Opslag limiet voor server logboek|Geen dimensies|
|serverlog_storage_percent|Ja|Percentage server logboek opslag|Percentage|Average|Percentage server logboek opslag|Geen dimensies|
|serverlog_storage_usage|Ja|Gebruikte server logboek opslag|Bytes|Average|Gebruikte server logboek opslag|Geen dimensies|
|storage_limit|Ja|Opslag limiet|Bytes|Maximum|Opslag limiet|Geen dimensies|
|storage_percent|Ja|Opslag percentage|Percentage|Average|Opslag percentage|Geen dimensies|
|storage_used|Ja|Gebruikte opslag|Bytes|Average|Gebruikte opslag|Geen dimensies|


## <a name="microsoftdbforpostgresqlserversv2"></a>Micro soft. DBforPostgreSQL/serversv2

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|active_connections|Ja|Actieve verbindingen|Aantal|Average|Actieve verbindingen|Geen dimensies|
|cpu_percent|Ja|CPU-percentage|Percentage|Average|CPU-percentage|Geen dimensies|
|IOPS|Ja|IOPS|Aantal|Average|I/o-bewerkingen per seconde|Geen dimensies|
|memory_percent|Ja|Geheugen percentage|Percentage|Average|Geheugen percentage|Geen dimensies|
|network_bytes_egress|Ja|Netwerk uit|Bytes|Totaal|Netwerk uit over actieve verbindingen|Geen dimensies|
|network_bytes_ingress|Ja|Netwerk in|Bytes|Totaal|Netwerk in meerdere actieve verbindingen|Geen dimensies|
|storage_percent|Ja|Opslag percentage|Percentage|Average|Opslag percentage|Geen dimensies|
|storage_used|Ja|Gebruikte opslag|Bytes|Average|Gebruikte opslag|Geen dimensies|


## <a name="microsoftdbforpostgresqlsingleservers"></a>Micro soft. DBforPostgreSQL/singleservers

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|active_connections|Ja|Actieve verbindingen|Aantal|Average|Actieve verbindingen|Geen dimensies|
|connections_failed|Ja|Mislukte verbindingen|Aantal|Totaal|Mislukte verbindingen|Geen dimensies|
|connections_succeeded|Ja|Geslaagde verbindingen|Aantal|Totaal|Geslaagde verbindingen|Geen dimensies|
|cpu_percent|Ja|CPU-percentage|Percentage|Average|CPU-percentage|Geen dimensies|
|IOPS|Ja|IOPS|Aantal|Average|I/o-bewerkingen per seconde|Geen dimensies|
|maximum_used_transactionIDs|Ja|Maximum aantal gebruikte trans actie-Id's|Aantal|Average|Maximum aantal gebruikte trans actie-Id's|Geen dimensies|
|memory_percent|Ja|Geheugen percentage|Percentage|Average|Geheugen percentage|Geen dimensies|
|network_bytes_egress|Ja|Netwerk uit|Bytes|Totaal|Netwerk uit over actieve verbindingen|Geen dimensies|
|network_bytes_ingress|Ja|Netwerk in|Bytes|Totaal|Netwerk in meerdere actieve verbindingen|Geen dimensies|
|storage_percent|Ja|Opslag percentage|Percentage|Average|Opslag percentage|Geen dimensies|
|storage_used|Ja|Gebruikte opslag|Bytes|Average|Gebruikte opslag|Geen dimensies|


## <a name="microsoftdevicesiothubs"></a>Micro soft. devices/IotHubs

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|C2D. commands. uitgangs. Abandon. geslaagd|Ja|C2D-berichten zijn afgebroken|Aantal|Totaal|Aantal Cloud-naar-apparaat-berichten die zijn afgebroken door het apparaat|Geen dimensies|
|C2D. commands. OUTuitgang. complete. geslaagd|Ja|C2D-bericht leveringen voltooid|Aantal|Totaal|Aantal bezorgingen van Cloud-naar-apparaat-berichten voltooid door het apparaat|Geen dimensies|
|C2D. commands. uitgangs. reject. geslaagd|Ja|Geweigerde C2D-berichten|Aantal|Totaal|Aantal Cloud-naar-apparaat-berichten dat door het apparaat is geweigerd|Geen dimensies|
|C2D. methods. failure|Ja|Mislukte directe aanroepen van methode|Aantal|Totaal|Het aantal mislukte direct-methode aanroepen.|Geen dimensies|
|C2D. methods. requestSize|Ja|Aanvraag grootte van directe-methode aanroepen|Bytes|Average|Het gemiddelde, het minimum en het maximum van alle geslaagde direct-methode aanvragen.|Geen dimensies|
|C2D. methods. responseSize|Ja|Antwoord grootte van directe methode aanroepen|Bytes|Average|Het gemiddelde, het minimum en het maximum van alle geslaagde reacties van de methode direct.|Geen dimensies|
|C2D. methods. geslaagd|Ja|Geslaagde directe aanroepen van de methode|Aantal|Totaal|Het aantal voltooide direct-methode aanroepen.|Geen dimensies|
|C2D. dubbele. Read. failure|Ja|Mislukte dubbele Lees bewerkingen van back-end|Aantal|Totaal|Het aantal mislukte back-end-geïnitieerde dubbele Lees bewerkingen.|Geen dimensies|
|C2D. dubbele. Lees. grootte|Ja|Reactie grootte van dubbele Lees bewerkingen van de back-end|Bytes|Average|Het gemiddelde, het minimum en het maximum van alle geslaagde back-end-geïnitieerde dubbele Lees bewerkingen.|Geen dimensies|
|C2D. dubbele. lezen. geslaagd|Ja|Geslaagde dubbele Lees bewerkingen van back-end|Aantal|Totaal|Het aantal geslaagde back-end-geïnitieerde dubbele Lees bewerkingen.|Geen dimensies|
|C2D. dubbele. update. failure|Ja|Mislukte dubbele updates van back-end|Aantal|Totaal|Het aantal niet-geslaagde, door de back-end geïnitieerde dubbele updates.|Geen dimensies|
|C2D. dubbele. update. grootte|Ja|Grootte van dubbele updates van back-end|Bytes|Average|Het gemiddelde, het minimum en de maximale grootte van alle geslaagde back-end-geïnitieerde dubbele updates.|Geen dimensies|
|C2D. dubbele. update. geslaagd|Ja|Geslaagde dubbele updates van back-end|Aantal|Totaal|Het aantal geslaagde, door de back-end gestarte dubbele updates.|Geen dimensies|
|C2DMessagesExpired|Ja|C2D-berichten verlopen (preview-versie)|Aantal|Totaal|Aantal verlopen Cloud-naar-apparaat-berichten|Geen dimensies|
|configuraties|Ja|Metrische configuratie gegevens|Aantal|Totaal|Metrische gegevens voor configuratie bewerkingen|Geen dimensies|
|connectedDeviceCount|Nee|Verbonden apparaten (preview-versie)|Aantal|Average|Aantal apparaten dat is verbonden met uw IoT-hub|Geen dimensies|
|D2C. endpoints. uitgangs punt. builtIn. Events|Ja|Route ring: berichten worden bezorgd bij berichten/gebeurtenissen|Aantal|Totaal|Het aantal keren dat IoT Hub route ring berichten heeft geleverd aan het ingebouwde eind punt (berichten/gebeurtenissen).|Geen dimensies|
|D2C. endpoints. uitgangs. Event hubs|Ja|Route ring: berichten worden bezorgd bij Event hub|Aantal|Totaal|Het aantal keren dat IoT Hub route ring berichten heeft geleverd aan Event hub-eind punten.|Geen dimensies|
|D2C. endpoints. uitgangs. serviceBusQueues|Ja|Route ring: berichten worden bezorgd bij Service Bus wachtrij|Aantal|Totaal|Het aantal keren dat IoT Hub route ring berichten heeft geleverd aan Service Bus-wachtrij-eind punten.|Geen dimensies|
|D2C. endpoints. uitgangs. serviceBusTopics|Ja|Route ring: berichten die worden bezorgd bij Service Bus onderwerp|Aantal|Totaal|Het aantal keren dat IoT Hub route ring berichten heeft geleverd aan Service Bus onderwerp-eind punten.|Geen dimensies|
|D2C. endpoints. outwaarde. Storage|Ja|Route ring: berichten worden bezorgd bij de opslag|Aantal|Totaal|Het aantal keren dat IoT Hub route ring berichten heeft geleverd aan de opslag eindpunten.|Geen dimensies|
|D2C. endpoints. outwaar. storage. blobs|Ja|Route ring: blobs die aan de opslag worden geleverd|Aantal|Totaal|Het aantal keren dat IoT Hub route ring blobs naar opslag eindpunten heeft geleverd.|Geen dimensies|
|D2C. endpoints. out. storage. bytes|Ja|Route ring: gegevens worden geleverd aan de opslag|Bytes|Totaal|De hoeveelheid gegevens (bytes) IoT Hub route ring die aan de opslag eindpunten wordt geleverd.|Geen dimensies|
|D2C. endpoints. latentie. builtIn. Events|Ja|Route ring: bericht latentie voor berichten/gebeurtenissen|Milliseconden|Average|De gemiddelde latentie (in milliseconden) tussen het binnenkomen van berichten naar IoT Hub en het inkomend telemetrie-bericht in het ingebouwde eind punt (berichten/gebeurtenissen).|Geen dimensies|
|D2C. endpoints. latentie. Event hubs|Ja|Route ring: bericht latentie voor Event hub|Milliseconden|Average|De gemiddelde latentie (in milliseconden) tussen het binnenkomen van berichten IoT Hub en het binnenkomen van berichten in een event hub-eind punt.|Geen dimensies|
|D2C. endpoints. latentie. serviceBusQueues|Ja|Route ring: bericht latentie voor Service Bus wachtrij|Milliseconden|Average|De gemiddelde latentie (in milliseconden) tussen het binnenkomen van berichten naar IoT Hub en telemetrie-berichten in een Service Bus wachtrij-eind punt.|Geen dimensies|
|D2C. endpoints. latentie. serviceBusTopics|Ja|Route ring: bericht latentie voor Service Bus onderwerp|Milliseconden|Average|De gemiddelde latentie (in milliseconden) tussen het binnenkomen van berichten naar IoT Hub en telemetrie-berichten in het eind punt van een Service Bus onderwerp.|Geen dimensies|
|D2C. endpoints. latentie. opslag|Ja|Route ring: bericht latentie voor opslag|Milliseconden|Average|De gemiddelde latentie (in milliseconden) tussen het binnenkomen van berichten naar IoT Hub en telemetrie-berichten in een opslag eindpunt.|Geen dimensies|
|D2C. telemetrie. uitgangs. verwijderd|Ja|Route ring: telemetrie-berichten verwijderd |Aantal|Totaal|Het aantal keren dat berichten zijn verwijderd door IoT Hub route ring vanwege Dead-eind punten. Deze waarde telt geen berichten die worden bezorgd als terugval route als berichten die worden verzonden, niet worden bezorgd.|Geen dimensies|
|D2C. telemetrie.. fallback|Ja|Route ring: berichten worden bezorgd bij terugval|Aantal|Totaal|Het aantal keren dat de route ring van berichten IoT Hub verzonden naar het eind punt dat is gekoppeld aan de terugval route.|Geen dimensies|
|D2C. telemetrie. uitgangs. ongeldig|Ja|Route ring: telemetrie-berichten incompatibel|Aantal|Totaal|Het aantal keren dat IoT Hub route ring geen berichten kan leveren als gevolg van incompatibiliteit met het eind punt. Deze waarde omvat geen nieuwe pogingen.|Geen dimensies|
|D2C. telemetrie.. zwevend|Ja|Route ring: telemetriegegevens van zwevende berichten |Aantal|Totaal|Het aantal keren dat berichten zijn zwevend door IoT Hub route ring, omdat ze niet overeenkomen met de regels voor door sturen (inclusief de terugval regel). |Geen dimensies|
|D2C. telemetrie. uitgangs. geslaagd|Ja|Route ring: telemetrie-berichten|Aantal|Totaal|Het aantal keren dat berichten zijn bezorgd bij alle eind punten met behulp van IoT Hub route ring. Als een bericht wordt doorgestuurd naar meerdere eind punten, wordt deze waarde met één verhoogd voor elke geslaagde levering. Als een bericht meerdere keren op hetzelfde eind punt wordt bezorgd, wordt deze waarde met één verhoogd voor elke geslaagde levering.|Geen dimensies|
|D2C. telemetrie. ingress. allProtocol|Ja|Verzend pogingen voor telemetrie-berichten|Aantal|Totaal|Aantal pogingen voor het verzenden van apparaat-naar-Cloud-telemetrie naar uw IoT hub|Geen dimensies|
|D2C. telemetrie. ingress. sendThrottle|Ja|Aantal beperkings fouten|Aantal|Totaal|Aantal beperkings fouten door doorvoer vertraging van apparaat|Geen dimensies|
|D2C. telemetrie. ingress. geslaagd|Ja|Verzonden telemetriegegevens|Aantal|Totaal|Aantal te verzenden apparaat-naar-Cloud-telemetrie-berichten naar uw IoT-hub|Geen dimensies|
|D2C. dubbele. Read. failure|Ja|Mislukte dubbele Lees bewerkingen van apparaten|Aantal|Totaal|Het aantal apparaten dat niet kan worden gestart, dubbele Lees bewerkingen.|Geen dimensies|
|D2C. dubbele. Lees. grootte|Ja|Reactie grootte van dubbele Lees bewerkingen van apparaten|Bytes|Average|Het gemiddelde, het minimum en het maximum van alle geslaagde apparaten-geïnitieerde dubbele Lees bewerkingen.|Geen dimensies|
|D2C. dubbele. lezen. geslaagd|Ja|Geslaagde dubbele Lees bewerkingen van apparaten|Aantal|Totaal|De telling van alle geslaagde apparaten met dubbele Lees bewerkingen.|Geen dimensies|
|D2C. dubbele. update. failure|Ja|Mislukte dubbele updates van apparaten|Aantal|Totaal|Het aantal apparaten dat door een apparaat is gestart en dubbele updates heeft uitgevoerd.|Geen dimensies|
|D2C. dubbele. update. grootte|Ja|Grootte van dubbele updates van apparaten|Bytes|Average|Het gemiddelde, het minimum en de maximale grootte van alle geslaagde, door het apparaat geïnitieerde dubbele updates.|Geen dimensies|
|D2C. dubbele. update. geslaagd|Ja|Geslaagde dubbele updates van apparaten|Aantal|Totaal|De telling van alle geslaagde, door het apparaat geïnitieerde dubbele updates.|Geen dimensies|
|dailyMessageQuotaUsed|Ja|Totaal aantal gebruikte berichten|Aantal|Maximum|Totaal aantal gebruikte berichten vandaag|Geen dimensies|
|deviceDataUsage|Ja|Totale hoeveelheid gegevens gebruik van apparaat|Bytes|Totaal|Verzonden bytes van en naar apparaten die zijn verbonden met IotHub|Geen dimensies|
|deviceDataUsageV2|Ja|Totaal gebruik van apparaatgegevens (preview-versie)|Bytes|Totaal|Verzonden bytes van en naar apparaten die zijn verbonden met IotHub|Geen dimensies|
|apparaten. connectedDevices. allProtocol|Ja|Verbonden apparaten (afgeschaft) |Aantal|Totaal|Aantal apparaten dat is verbonden met uw IoT-hub|Geen dimensies|
|apparaten. totalDevices|Ja|Totaal aantal apparaten (afgeschaft)|Aantal|Totaal|Aantal apparaten dat is geregistreerd bij uw IoT-hub|Geen dimensies|
|EventGridDeliveries|Ja|Event Grid leveringen (preview-versie)|Aantal|Totaal|Het aantal IoT Hub gebeurtenissen dat is gepubliceerd op Event Grid. Gebruik de dimensie resultaat voor het aantal geslaagde en mislukte aanvragen. De dimensie type-tekst geeft het soort gebeurtenis weer ( https://aka.ms/ioteventgrid) .|Resultaat, type gebeurtenis|
|EventGridLatency|Ja|Event Grid latentie (preview-versie)|Milliseconden|Average|De gemiddelde latentie (in milliseconden) vanaf het moment waarop de IOT hub-gebeurtenis werd gegenereerd toen de gebeurtenis werd gepubliceerd in Event Grid. Dit getal is een gemiddelde tussen alle gebeurtenis typen. Gebruik de dimensie type type om de latentie van een specifiek soort gebeurtenis weer te geven.|EventType|
|Jobs. cancelJob. failure|Ja|Mislukte taak annuleringen|Aantal|Totaal|Het aantal mislukte aanroepen om een taak te annuleren.|Geen dimensies|
|Jobs. cancelJob. geslaagd|Ja|Voltooide taak annuleringen|Aantal|Totaal|Het aantal geslaagde aanroepen om een taak te annuleren.|Geen dimensies|
|Jobs. voltooid|Ja|Voltooide taken|Aantal|Totaal|Het aantal voltooide taken.|Geen dimensies|
|Jobs. createDirectMethodJob. failure|Ja|Kan geen aanroepen van methode aanroep taken uitvoeren|Aantal|Totaal|Het aantal van alle mislukte aanroepen van directe methode aanroep taken.|Geen dimensies|
|Jobs. createDirectMethodJob. geslaagd|Ja|Geslaagde creatie van methode aanroep taken|Aantal|Totaal|Het aantal van alle geslaagde aanroepen van directe methode aanroep taken.|Geen dimensies|
|Jobs. createTwinUpdateJob. failure|Ja|Kan geen dubbele update taken uitvoeren|Aantal|Totaal|Het aantal mislukte het maken van dubbele update taken.|Geen dimensies|
|Jobs. createTwinUpdateJob. geslaagd|Ja|Geslaagde creatie van dubbele update taken|Aantal|Totaal|Het aantal van alle geslaagde taken voor het maken van dubbele updates.|Geen dimensies|
|Jobs. mislukt|Ja|Mislukte taken|Aantal|Totaal|Het aantal mislukte taken.|Geen dimensies|
|Jobs. listJobs. failure|Ja|Mislukte aanroepen naar lijst taken|Aantal|Totaal|Het aantal mislukte aanroepen naar lijst taken.|Geen dimensies|
|Jobs. listJobs. geslaagd|Ja|Geslaagde aanroepen naar lijst taken|Aantal|Totaal|Het aantal geslaagde aanroepen naar lijst taken.|Geen dimensies|
|Jobs. queryJobs. failure|Ja|Mislukte taak query's|Aantal|Totaal|Het aantal mislukte aanroepen naar query taken.|Geen dimensies|
|Jobs. queryJobs. geslaagd|Ja|Geslaagde taak query's|Aantal|Totaal|Het aantal geslaagde aanroepen naar query taken.|Geen dimensies|
|RoutingDataSizeInBytesDelivered|Ja|Grootte van bezorgings bericht in bytes (preview-versie)|Bytes|Totaal|De totale grootte in bytes van berichten die door IoT hub worden geleverd aan een eind punt. U kunt de dimensies Endpointnaam en EndpointType gebruiken om de grootte weer te geven van de berichten in bytes die aan uw verschillende eind punten worden geleverd. De waarde van de metriek neemt toe voor elk bericht dat wordt bezorgd, inclusief als het bericht wordt bezorgd bij meerdere eind punten of als het bericht meermaals aan hetzelfde eind punt wordt geleverd.|EndpointType, eindpuntnaam, RoutingSource|
|RoutingDeliveries|Ja|Route ring van bezorgingen (preview-versie)|Aantal|Totaal|Het aantal keren dat IoT Hub probeerde berichten te leveren aan alle eind punten met behulp van route ring. Als u het aantal geslaagde of mislukte pogingen wilt zien, gebruikt u de dimensie resultaat. Gebruik de dimensie FailureReasonCategory om de reden van de fout te zien, zoals ongeldig, verwijderd of zwevend. U kunt ook de dimensies Endpointnaam en EndpointType gebruiken om te begrijpen hoeveel berichten er aan uw verschillende eind punten zijn geleverd. De waarde van de metriek neemt toe met één voor elke bezorgings poging, ook als het bericht wordt bezorgd bij meerdere eind punten of als het bericht meerdere keren wordt bezorgd bij hetzelfde eind punt.|EndpointType, Endpointnaam, FailureReasonCategory, result, RoutingSource|
|RoutingDeliveryLatency|Ja|Bezorg latentie van route ring (preview-versie)|Milliseconden|Average|De gemiddelde latentie (in milliseconden) tussen het binnenkomen van berichten naar IoT Hub en het inkomend telemetrie-bericht in een eind punt. U kunt de dimensies Endpointnaam en EndpointType gebruiken om inzicht te krijgen in de latentie van uw verschillende eind punten.|EndpointType, eindpuntnaam, RoutingSource|
|totalDeviceCount|Nee|Totaal aantal apparaten (preview-versie)|Aantal|Average|Aantal apparaten dat is geregistreerd bij uw IoT-hub|Geen dimensies|
|twinQueries. failure|Ja|Mislukte dubbele query's|Aantal|Totaal|Het aantal mislukte dubbele query's.|Geen dimensies|
|twinQueries.resultSize|Ja|Resultaat grootte van dubbele query's|Bytes|Average|Het gemiddelde, het minimum en het maximum van de resultaat grootte van alle geslaagde dubbele query's.|Geen dimensies|
|twinQueries. geslaagd|Ja|Geslaagde dubbele query's|Aantal|Totaal|Het aantal geslaagde dubbele query's.|Geen dimensies|


## <a name="microsoftdevicesprovisioningservices"></a>Micro soft. devices/provisioningServices

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|AttestationAttempts|Ja|Attestation-pogingen|Aantal|Totaal|Aantal pogingen voor het uitvoeren van een apparaat|ProvisioningServiceName, status, protocol|
|DeviceAssignments|Ja|Apparaten toegewezen|Aantal|Totaal|Aantal apparaten dat is toegewezen aan een IoT-hub|ProvisioningServiceName, IotHubName|
|RegistrationAttempts|Ja|Registratie pogingen|Aantal|Totaal|Aantal pogingen voor apparaatregistratie|ProvisioningServiceName, IotHubName, status|


## <a name="microsoftdocumentdbdatabaseaccounts"></a>Microsoft.DocumentDB/databaseAccounts

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|AddRegion|Ja|Regio toegevoegd|Aantal|Aantal|Regio toegevoegd|Regio|
|AutoscaleMaxThroughput|Nee|Maximale door Voer voor automatisch schalen|Aantal|Maximum|Maximale door Voer voor automatisch schalen|DATABASENAME, verzamelnaam|
|AvailableStorage|Nee|keur Beschik bare opslag|Bytes|Totaal|"Beschik bare opslag" wordt aan het einde van 2020 september verwijderd uit Azure Monitor. Opslag grootte van Cosmos DB verzameling is nu onbeperkt. De enige beperking is dat de opslag grootte voor elke logische partitie sleutel 20 GB is. U kunt PartitionKeyStatistics inschakelen in Diagnostische logboeken om het opslag verbruik voor de belangrijkste partitie sleutels te kennen. Raadpleeg dit document voor meer informatie over Cosmos DB opslag quotum https://docs.microsoft.com/azure/cosmos-db/concepts-limits . Na de afschaffing worden de resterende waarschuwings regels die nog steeds zijn gedefinieerd voor de afgeschafte metriek, automatisch uitgeschakeld de datum van afschaffing.|Verzamelingnaam, databasenaam, regio|
|CassandraConnectionClosures|Nee|Cassandra-verbinding sluiten|Aantal|Totaal|Aantal Cassandra-verbindingen dat is gesloten, gerapporteerd met een granulatie van 1 minuut|Regio, ClosureReason|
|CassandraConnectorAvgReplicationLatency|Nee|Gemiddelde ReplicationLatency van Cassandra-connector|Milliseconden|Average|Gemiddelde ReplicationLatency van Cassandra-connector|Geen dimensies|
|CassandraConnectorReplicationHealthStatus|Nee|Status van replicatie van Cassandra-connector|Aantal|Aantal|Status van replicatie van Cassandra-connector|NotStarted, ReplicationInProgress, fout|
|CassandraKeyspaceCreate|Nee|Cassandra-opslag ruimte gemaakt|Aantal|Aantal|Cassandra-opslag ruimte gemaakt|ResourceName |
|CassandraKeyspaceDelete|Nee|Cassandra-opslag ruimte verwijderd|Aantal|Aantal|Cassandra-opslag ruimte verwijderd|ResourceName |
|CassandraKeyspaceThroughputUpdate|Nee|Cassandra voor de door Voer van de opslag ruimte bijgewerkt|Aantal|Aantal|Cassandra voor de door Voer van de opslag ruimte bijgewerkt|ResourceName |
|CassandraKeyspaceUpdate|Nee|Cassandra-opslag ruimte bijgewerkt|Aantal|Aantal|Cassandra-opslag ruimte bijgewerkt|ResourceName |
|CassandraRequestCharges|Nee|Kosten voor Cassandra-aanvragen|Aantal|Totaal|RUs gebruikt voor Cassandra-aanvragen|DATABASENAME, verzamel-, regio, OperationType, resource type|
|CassandraRequests|Nee|Cassandra aanvragen|Aantal|Aantal|Aantal gemaakte Cassandra-aanvragen|DATABASENAME, verzamel-, regio, OperationType, resource type, error code|
|CassandraTableCreate|Nee|Cassandra-tabel gemaakt|Aantal|Aantal|Cassandra-tabel gemaakt|ResourceName, ChildResourceName, |
|CassandraTableDelete|Nee|Cassandra-tabel verwijderd|Aantal|Aantal|Cassandra-tabel verwijderd|ResourceName, ChildResourceName, |
|CassandraTableThroughputUpdate|Nee|Cassandra-tabel doorvoer bijgewerkt|Aantal|Aantal|Cassandra-tabel doorvoer bijgewerkt|ResourceName, ChildResourceName, |
|CassandraTableUpdate|Nee|Cassandra-tabel bijgewerkt|Aantal|Aantal|Cassandra-tabel bijgewerkt|ResourceName, ChildResourceName, |
|CreateAccount|Ja|Account gemaakt|Aantal|Aantal|Account gemaakt|Geen dimensies|
|DataUsage|Nee|Gegevensgebruik|Bytes|Totaal|Totaal gegevens gebruik gerapporteerd bij 5 minuten granulariteit|Verzamelingnaam, databasenaam, regio|
|DeleteAccount|Ja|Account is verwijderd|Aantal|Aantal|Account is verwijderd|Geen dimensies|
|DocumentCount|Nee|Aantal documenten|Aantal|Totaal|Totaal aantal documenten gemeld bij 5 minuten granulariteit|Verzamelingnaam, databasenaam, regio|
|DocumentQuota|Nee|Document quotum|Bytes|Totaal|Totale opslag quotum gerapporteerd bij een granulatie van 5 minuten|Verzamelingnaam, databasenaam, regio|
|GremlinDatabaseCreate|Nee|Gremlin-data base gemaakt|Aantal|Aantal|Gremlin-data base gemaakt|ResourceName |
|GremlinDatabaseDelete|Nee|Gremlin-data base verwijderd|Aantal|Aantal|Gremlin-data base verwijderd|ResourceName |
|GremlinDatabaseThroughputUpdate|Nee|Gremlin-data base-door Voer bijgewerkt|Aantal|Aantal|Gremlin-data base-door Voer bijgewerkt|ResourceName |
|GremlinDatabaseUpdate|Nee|Gremlin-data base bijgewerkt|Aantal|Aantal|Gremlin-data base bijgewerkt|ResourceName |
|GremlinGraphCreate|Nee|Gremlin Graph gemaakt|Aantal|Aantal|Gremlin Graph gemaakt|ResourceName, ChildResourceName, |
|GremlinGraphDelete|Nee|Gremlin-grafiek verwijderd|Aantal|Aantal|Gremlin-grafiek verwijderd|ResourceName, ChildResourceName, |
|GremlinGraphThroughputUpdate|Nee|Gremlin-grafiek doorvoer bijgewerkt|Aantal|Aantal|Gremlin-grafiek doorvoer bijgewerkt|ResourceName, ChildResourceName, |
|GremlinGraphUpdate|Nee|Gremlin-grafiek bijgewerkt|Aantal|Aantal|Gremlin-grafiek bijgewerkt|ResourceName, ChildResourceName, |
|IndexUsage|Nee|Indexgebruik|Bytes|Totaal|Totaal gebruik van index gerapporteerd bij een granulatie van 5 minuten|Verzamelingnaam, databasenaam, regio|
|MetadataRequests|Nee|Meta gegevens aanvragen|Aantal|Aantal|Aantal aanvragen voor meta gegevens. Cosmos DB onderhoudt de verzameling van meta gegevens van het systeem voor elk account, waarmee u verzamelingen, data bases, enzovoort en hun configuraties gratis kunt inventariseren.|DATABASENAME, verzamel-, regio, status code, |
|MongoCollectionCreate|Nee|Mongo-verzameling gemaakt|Aantal|Aantal|Mongo-verzameling gemaakt|ResourceName, ChildResourceName, |
|MongoCollectionDelete|Nee|Mongo-verzameling verwijderd|Aantal|Aantal|Mongo-verzameling verwijderd|ResourceName, ChildResourceName, |
|MongoCollectionThroughputUpdate|Nee|Door Voer van Mongo-verzameling bijgewerkt|Aantal|Aantal|Door Voer van Mongo-verzameling bijgewerkt|ResourceName, ChildResourceName, |
|MongoCollectionUpdate|Nee|Mongo-verzameling bijgewerkt|Aantal|Aantal|Mongo-verzameling bijgewerkt|ResourceName, ChildResourceName, |
|MongoDatabaseDelete|Nee|Mongo-data base verwijderd|Aantal|Aantal|Mongo-data base verwijderd|ResourceName |
|MongoDatabaseThroughputUpdate|Nee|Mongo-data base-door Voer bijgewerkt|Aantal|Aantal|Mongo-data base-door Voer bijgewerkt|ResourceName |
|MongoDBDatabaseCreate|Nee|Mongo-data base gemaakt|Aantal|Aantal|Mongo-data base gemaakt|ResourceName |
|MongoDBDatabaseUpdate|Nee|Mongo-data base bijgewerkt|Aantal|Aantal|Mongo-data base bijgewerkt|ResourceName |
|MongoRequestCharge|Ja|Kosten voor Mongo-aanvragen|Aantal|Totaal|Verbruikte Mongo-aanvraag eenheden|DATABASENAME, verzamel-, regio, opdrachtnaam, error code, status|
|MongoRequests|Ja|Mongo aanvragen|Aantal|Aantal|Aantal gemaakte Mongo-aanvragen|DATABASENAME, verzamel-, regio, opdrachtnaam, error code, status|
|MongoRequestsCount|Nee|Frequentie van Mongo-aanvragen|CountPerSecond|Average|Aantal Mongo per seconde|DATABASENAME, verzamel-, regio, error code|
|MongoRequestsDelete|Nee|Aantal Mongo-aanvragen voor verwijderen|CountPerSecond|Average|Mongo-aanvraag voor verwijderen per seconde|DATABASENAME, verzamel-, regio, error code|
|MongoRequestsInsert|Nee|Aantal Mongo invoegen|CountPerSecond|Average|Aantal Mongo per seconde|DATABASENAME, verzamel-, regio, error code|
|MongoRequestsQuery|Nee|Frequentie van Mongo-query aanvragen|CountPerSecond|Average|Mongo-query aanvraag per seconde|DATABASENAME, verzamel-, regio, error code|
|MongoRequestsUpdate|Nee|Frequentie van Mongo-update aanvragen|CountPerSecond|Average|Aanvraag voor Mongo-update per seconde|DATABASENAME, verzamel-, regio, error code|
|NormalizedRUConsumption|Nee|Genormaliseerd RU-verbruik|Percentage|Maximum|Maximum aantal van RU verbruik per minuut|Verzamelingnaam, DATABASENAME, regio, PartitionKeyRangeId|
|ProvisionedThroughput|Nee|Ingerichte doorvoer|Aantal|Maximum|Ingerichte doorvoer|DATABASENAME, verzamelnaam|
|RegionFailover|Ja|Er is een failover uitgevoerd voor de regio|Aantal|Aantal|Er is een failover uitgevoerd voor de regio|Geen dimensies|
|RemoveRegion|Ja|Regio is verwijderd|Aantal|Aantal|Regio is verwijderd|Regio|
|ReplicationLatency|Ja|P99-replicatie latentie|Milliseconden|Average|Replicatie latentie van P99 voor de bron-en doel regio's voor geografisch ingeschakelde account|SourceRegion, TargetRegion|
|ServerSideLatency|Nee|Latentie aan server zijde|Milliseconden|Average|Latentie aan server zijde|DATABASENAME, verzamel-, regio, ConnectionMode, OperationType, PublicAPIType|
|ServiceAvailability|Nee|Service beschikbaarheid|Percentage|Average|Beschik baarheid van account aanvragen op één uur, dag of maand granulatie|Geen dimensies|
|SqlContainerCreate|Nee|SQL-container gemaakt|Aantal|Aantal|SQL-container gemaakt|ResourceName, ChildResourceName, |
|SqlContainerDelete|Nee|SQL-container verwijderd|Aantal|Aantal|SQL-container verwijderd|ResourceName, ChildResourceName, |
|SqlContainerThroughputUpdate|Nee|SQL-container doorvoer bijgewerkt|Aantal|Aantal|SQL-container doorvoer bijgewerkt|ResourceName, ChildResourceName, |
|SqlContainerUpdate|Nee|SQL-container is bijgewerkt|Aantal|Aantal|SQL-container is bijgewerkt|ResourceName, ChildResourceName, |
|SqlDatabaseCreate|Nee|SQL-data base gemaakt|Aantal|Aantal|SQL-data base gemaakt|ResourceName |
|SqlDatabaseDelete|Nee|SQL-data base verwijderd|Aantal|Aantal|SQL-data base verwijderd|ResourceName |
|SqlDatabaseThroughputUpdate|Nee|SQL Data Base-door Voer bijgewerkt|Aantal|Aantal|SQL Data Base-door Voer bijgewerkt|ResourceName |
|SqlDatabaseUpdate|Nee|SQL-data base bijgewerkt|Aantal|Aantal|SQL-data base bijgewerkt|ResourceName |
|TableTableCreate|Nee|AzureTable-tabel gemaakt|Aantal|Aantal|AzureTable-tabel gemaakt|ResourceName |
|TableTableDelete|Nee|AzureTable-tabel verwijderd|Aantal|Aantal|AzureTable-tabel verwijderd|ResourceName |
|TableTableThroughputUpdate|Nee|AzureTable-tabel doorvoer bijgewerkt|Aantal|Aantal|AzureTable-tabel doorvoer bijgewerkt|ResourceName |
|TableTableUpdate|Nee|AzureTable-tabel bijgewerkt|Aantal|Aantal|AzureTable-tabel bijgewerkt|ResourceName |
|TotalRequests|Ja|Totaal aantal aanvragen|Aantal|Aantal|Aantal aanvragen dat is gedaan|DATABASENAME, verzamelings instelling, regio, status code, OperationType, status|
|TotalRequestUnits|Ja|Totaal aantal aanvraag eenheden|Aantal|Totaal|Verbruikte aanvraag eenheden|DATABASENAME, verzamelings instelling, regio, status code, OperationType, status|
|UpdateAccountKeys|Ja|Bijgewerkte account sleutels|Aantal|Aantal|Bijgewerkte account sleutels|KeyType|
|UpdateAccountNetworkSettings|Ja|De netwerk instellingen voor het account zijn bijgewerkt|Aantal|Aantal|De netwerk instellingen voor het account zijn bijgewerkt|Geen dimensies|
|UpdateAccountReplicationSettings|Ja|De replicatie-instellingen voor het account zijn bijgewerkt|Aantal|Aantal|De replicatie-instellingen voor het account zijn bijgewerkt|Geen dimensies|
|UpdateDiagnosticsSettings|Nee|De diagnostische instellingen voor het account zijn bijgewerkt|Aantal|Aantal|De diagnostische instellingen voor het account zijn bijgewerkt|DiagnosticSettingsName, ResourceGroupName|


## <a name="microsofteventgriddomains"></a>Micro soft. EventGrid/domeinen

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|DeadLetteredCount|Ja|Gebeurtenissen met onbestelbare berichten|Aantal|Totaal|Totaal aantal gebeurtenissen met onbestelbare berichten die overeenkomen met dit gebeurtenis abonnement|Onderwerp, EventSubscriptionName, DomainEventSubscriptionName, DeadLetterReason|
|DeliveryAttemptFailCount|Nee|Mislukte leverings gebeurtenissen|Aantal|Totaal|Totaal aantal gebeurtenissen dat niet aan dit gebeurtenis abonnement kan worden geleverd|Onderwerp, EventSubscriptionName, DomainEventSubscriptionName, fout, error type|
|DeliverySuccessCount|Ja|Geleverde gebeurtenissen|Aantal|Totaal|Totaal aantal gebeurtenissen dat aan dit gebeurtenis abonnement is geleverd|Onderwerp, EventSubscriptionName, DomainEventSubscriptionName|
|DestinationProcessingDurationInMs|Nee|Doel verwerkings duur|Milliseconden|Average|Doel verwerkings duur in milliseconden|Onderwerp, EventSubscriptionName, DomainEventSubscriptionName|
|DroppedEventCount|Ja|Verwijderde gebeurtenissen|Aantal|Totaal|Totaal aantal verloren gebeurtenissen dat overeenkomt met dit gebeurtenis abonnement|Onderwerp, EventSubscriptionName, DomainEventSubscriptionName, DropReason|
|MatchedEventCount|Ja|Overeenkomende gebeurtenissen|Aantal|Totaal|Totaal aantal gebeurtenissen dat overeenkomt met dit gebeurtenis abonnement|Onderwerp, EventSubscriptionName, DomainEventSubscriptionName|
|PublishFailCount|Ja|Mislukte gebeurtenissen publiceren|Aantal|Totaal|Totaal aantal gebeurtenissen dat niet naar dit onderwerp kan worden gepubliceerd|Onderwerp, error type, fout|
|PublishSuccessCount|Ja|Gepubliceerde gebeurtenissen|Aantal|Totaal|Totaal aantal gebeurtenissen dat naar dit onderwerp is gepubliceerd|Onderwerp|
|PublishSuccessLatencyInMs|Ja|Latentie van publicatie geslaagd|Milliseconden|Totaal|Latentie van geslaagde publicatie in milliseconden|Geen dimensies|


## <a name="microsofteventgrideventsubscriptions"></a>Micro soft. EventGrid/eventSubscriptions

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|DeadLetteredCount|Ja|Gebeurtenissen met onbestelbare berichten|Aantal|Totaal|Totaal aantal gebeurtenissen met onbestelbare berichten die overeenkomen met dit gebeurtenis abonnement|DeadLetterReason|
|DeliveryAttemptFailCount|Nee|Mislukte leverings gebeurtenissen|Aantal|Totaal|Totaal aantal gebeurtenissen dat niet aan dit gebeurtenis abonnement kan worden geleverd|Fout, error type|
|DeliverySuccessCount|Ja|Geleverde gebeurtenissen|Aantal|Totaal|Totaal aantal gebeurtenissen dat aan dit gebeurtenis abonnement is geleverd|Geen dimensies|
|DestinationProcessingDurationInMs|Nee|Doel verwerkings duur|Milliseconden|Average|Doel verwerkings duur in milliseconden|Geen dimensies|
|DroppedEventCount|Ja|Verwijderde gebeurtenissen|Aantal|Totaal|Totaal aantal verloren gebeurtenissen dat overeenkomt met dit gebeurtenis abonnement|DropReason|
|MatchedEventCount|Ja|Overeenkomende gebeurtenissen|Aantal|Totaal|Totaal aantal gebeurtenissen dat overeenkomt met dit gebeurtenis abonnement|Geen dimensies|


## <a name="microsofteventgridextensiontopics"></a>Micro soft. EventGrid/extensionTopics

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|PublishFailCount|Ja|Mislukte gebeurtenissen publiceren|Aantal|Totaal|Totaal aantal gebeurtenissen dat niet naar dit onderwerp kan worden gepubliceerd|Error type, fout|
|PublishSuccessCount|Ja|Gepubliceerde gebeurtenissen|Aantal|Totaal|Totaal aantal gebeurtenissen dat naar dit onderwerp is gepubliceerd|Geen dimensies|
|PublishSuccessLatencyInMs|Ja|Latentie van publicatie geslaagd|Milliseconden|Totaal|Latentie van geslaagde publicatie in milliseconden|Geen dimensies|
|UnmatchedEventCount|Ja|Niet-overeenkomende gebeurtenissen|Aantal|Totaal|Totaal aantal gebeurtenissen dat niet overeenkomt met een van de gebeurtenis abonnementen voor dit onderwerp|Geen dimensies|


## <a name="microsofteventgridsystemtopics"></a>Micro soft. EventGrid/systemTopics

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|DeadLetteredCount|Ja|Gebeurtenissen met onbestelbare berichten|Aantal|Totaal|Totaal aantal gebeurtenissen met onbestelbare berichten die overeenkomen met dit gebeurtenis abonnement|DeadLetterReason, EventSubscriptionName|
|DeliveryAttemptFailCount|Nee|Mislukte leverings gebeurtenissen|Aantal|Totaal|Totaal aantal gebeurtenissen dat niet aan dit gebeurtenis abonnement kan worden geleverd|Fout, error type, EventSubscriptionName|
|DeliverySuccessCount|Ja|Geleverde gebeurtenissen|Aantal|Totaal|Totaal aantal gebeurtenissen dat aan dit gebeurtenis abonnement is geleverd|EventSubscriptionName|
|DestinationProcessingDurationInMs|Nee|Doel verwerkings duur|Milliseconden|Average|Doel verwerkings duur in milliseconden|EventSubscriptionName|
|DroppedEventCount|Ja|Verwijderde gebeurtenissen|Aantal|Totaal|Totaal aantal verloren gebeurtenissen dat overeenkomt met dit gebeurtenis abonnement|DropReason, EventSubscriptionName|
|MatchedEventCount|Ja|Overeenkomende gebeurtenissen|Aantal|Totaal|Totaal aantal gebeurtenissen dat overeenkomt met dit gebeurtenis abonnement|EventSubscriptionName|
|PublishFailCount|Ja|Mislukte gebeurtenissen publiceren|Aantal|Totaal|Totaal aantal gebeurtenissen dat niet naar dit onderwerp kan worden gepubliceerd|Error type, fout|
|PublishSuccessCount|Ja|Gepubliceerde gebeurtenissen|Aantal|Totaal|Totaal aantal gebeurtenissen dat naar dit onderwerp is gepubliceerd|Geen dimensies|
|PublishSuccessLatencyInMs|Ja|Latentie van publicatie geslaagd|Milliseconden|Totaal|Latentie van geslaagde publicatie in milliseconden|Geen dimensies|
|UnmatchedEventCount|Ja|Niet-overeenkomende gebeurtenissen|Aantal|Totaal|Totaal aantal gebeurtenissen dat niet overeenkomt met een van de gebeurtenis abonnementen voor dit onderwerp|Geen dimensies|


## <a name="microsofteventgridtopics"></a>Micro soft. EventGrid/topics

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|DeadLetteredCount|Ja|Gebeurtenissen met onbestelbare berichten|Aantal|Totaal|Totaal aantal gebeurtenissen met onbestelbare berichten die overeenkomen met dit gebeurtenis abonnement|DeadLetterReason, EventSubscriptionName|
|DeliveryAttemptFailCount|Nee|Mislukte leverings gebeurtenissen|Aantal|Totaal|Totaal aantal gebeurtenissen dat niet aan dit gebeurtenis abonnement kan worden geleverd|Fout, error type, EventSubscriptionName|
|DeliverySuccessCount|Ja|Geleverde gebeurtenissen|Aantal|Totaal|Totaal aantal gebeurtenissen dat aan dit gebeurtenis abonnement is geleverd|EventSubscriptionName|
|DestinationProcessingDurationInMs|Nee|Doel verwerkings duur|Milliseconden|Average|Doel verwerkings duur in milliseconden|EventSubscriptionName|
|DroppedEventCount|Ja|Verwijderde gebeurtenissen|Aantal|Totaal|Totaal aantal verloren gebeurtenissen dat overeenkomt met dit gebeurtenis abonnement|DropReason, EventSubscriptionName|
|MatchedEventCount|Ja|Overeenkomende gebeurtenissen|Aantal|Totaal|Totaal aantal gebeurtenissen dat overeenkomt met dit gebeurtenis abonnement|EventSubscriptionName|
|PublishFailCount|Ja|Mislukte gebeurtenissen publiceren|Aantal|Totaal|Totaal aantal gebeurtenissen dat niet naar dit onderwerp kan worden gepubliceerd|Error type, fout|
|PublishSuccessCount|Ja|Gepubliceerde gebeurtenissen|Aantal|Totaal|Totaal aantal gebeurtenissen dat naar dit onderwerp is gepubliceerd|Geen dimensies|
|PublishSuccessLatencyInMs|Ja|Latentie van publicatie geslaagd|Milliseconden|Totaal|Latentie van geslaagde publicatie in milliseconden|Geen dimensies|
|UnmatchedEventCount|Ja|Niet-overeenkomende gebeurtenissen|Aantal|Totaal|Totaal aantal gebeurtenissen dat niet overeenkomt met een van de gebeurtenis abonnementen voor dit onderwerp|Geen dimensies|


## <a name="microsofteventhubclusters"></a>Micro soft. EventHub/clusters

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|ActiveConnections|Nee|ActiveConnections|Aantal|Average|Totaal aantal actieve verbindingen voor micro soft. EventHub.|Geen dimensies|
|AvailableMemory|Nee|Beschikbaar geheugen|Percentage|Maximum|Beschikbaar geheugen voor het event hub-cluster als percentage van het totale geheugen.|Rol|
|CaptureBacklog|Nee|Achterstand vastleggen.|Aantal|Totaal|Achterstand vastleggen voor micro soft. EventHub.|Geen dimensies|
|CapturedBytes|Nee|Vastgelegde bytes.|Bytes|Totaal|Vastgelegde bytes voor micro soft. EventHub.|Geen dimensies|
|CapturedMessages|Nee|Vastgelegde berichten.|Aantal|Totaal|Vastgelegde berichten voor micro soft. EventHub.|Geen dimensies|
|ConnectionsClosed|Nee|Gesloten verbindingen.|Aantal|Average|Gesloten verbindingen voor micro soft. EventHub.|Geen dimensies|
|ConnectionsOpened|Nee|Geopende verbindingen.|Aantal|Average|Geopende verbindingen voor micro soft. EventHub.|Geen dimensies|
|CPU|Nee|CPU|Percentage|Maximum|CPU-gebruik voor het event hub-cluster als een percentage|Rol|
|IncomingBytes|Ja|Binnenkomende bytes.|Bytes|Totaal|Binnenkomende bytes voor micro soft. EventHub.|Geen dimensies|
|IncomingMessages|Ja|Binnenkomende berichten|Aantal|Totaal|Binnenkomende berichten voor micro soft. EventHub.|Geen dimensies|
|IncomingRequests|Ja|Binnenkomende aanvragen|Aantal|Totaal|Binnenkomende aanvragen voor micro soft. EventHub.|Geen dimensies|
|OutgoingBytes|Ja|Uitgaande bytes.|Bytes|Totaal|Uitgaande bytes voor micro soft. EventHub.|Geen dimensies|
|OutgoingMessages|Ja|Uitgaande berichten|Aantal|Totaal|Uitgaande berichten voor micro soft. EventHub.|Geen dimensies|
|QuotaExceededErrors|Nee|Fouten met overschreden quota.|Aantal|Totaal|Quotum overschrijdt fouten voor micro soft. EventHub.|Geen dimensies|
|ServerErrors|Nee|Serverfouten.|Aantal|Totaal|Server fouten voor micro soft. EventHub.|Geen dimensies|
|Grootte|Nee|Grootte|Bytes|Average|Grootte van een EventHub in bytes.|Rol|
|SuccessfulRequests|Nee|Geslaagde aanvragen|Aantal|Totaal|Geslaagde aanvragen voor micro soft. EventHub.|Geen dimensies|
|ThrottledRequests|Nee|Beperkte aanvragen.|Aantal|Totaal|Beperkte aanvragen voor micro soft. EventHub.|Geen dimensies|
|UserErrors|Nee|Gebruikersfouten.|Aantal|Totaal|Gebruikers fouten voor micro soft. EventHub.|Geen dimensies|


## <a name="microsofteventhubnamespaces"></a>Microsoft.EventHub/namespaces

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|ActiveConnections|Nee|ActiveConnections|Aantal|Average|Totaal aantal actieve verbindingen voor micro soft. EventHub.|Geen dimensies|
|CaptureBacklog|Nee|Achterstand vastleggen.|Aantal|Totaal|Achterstand vastleggen voor micro soft. EventHub.|EntityName|
|CapturedBytes|Nee|Vastgelegde bytes.|Bytes|Totaal|Vastgelegde bytes voor micro soft. EventHub.|EntityName|
|CapturedMessages|Nee|Vastgelegde berichten.|Aantal|Totaal|Vastgelegde berichten voor micro soft. EventHub.|EntityName|
|ConnectionsClosed|Nee|Gesloten verbindingen.|Aantal|Average|Gesloten verbindingen voor micro soft. EventHub.|EntityName|
|ConnectionsOpened|Nee|Geopende verbindingen.|Aantal|Average|Geopende verbindingen voor micro soft. EventHub.|EntityName|
|EHABL|Ja|Achterstallige berichten archiveren (afgeschaft)|Aantal|Totaal|Event hub-archief berichten in achterstand voor een naam ruimte (afgeschaft)|Geen dimensies|
|EHAMBS|Ja|Berichten doorvoer archiveren (afgeschaft)|Bytes|Totaal|Door Voer van Event hub-bericht doorvoer in een naam ruimte (afgeschaft)|Geen dimensies|
|EHAMSGS|Ja|Berichten archiveren (afgeschaft)|Aantal|Totaal|Event hub heeft berichten gearchiveerd in een naam ruimte (afgeschaft)|Geen dimensies|
|EHINBYTES|Ja|Binnenkomende bytes (afgeschaft)|Bytes|Totaal|Door Voer van inkomende berichten van Event hub voor een naam ruimte (afgeschaft)|Geen dimensies|
|EHINMBS|Ja|Binnenkomende bytes (verouderd) (afgeschaft)|Bytes|Totaal|Door Voer van binnenkomende berichten van Event hub voor een naam ruimte. Deze metriek is afgeschaft. Gebruik in plaats daarvan de metrische gegevens over binnenkomende bytes (afgeschaft)|Geen dimensies|
|EHINMSGS|Ja|Inkomende berichten (afgeschaft)|Aantal|Totaal|Totaal aantal inkomende berichten voor een naam ruimte (afgeschaft)|Geen dimensies|
|EHOUTBYTES|Ja|Uitgaande bytes (afgeschaft)|Bytes|Totaal|Door Voer van Event hub voor een naam ruimte (afgeschaft)|Geen dimensies|
|EHOUTMBS|Ja|Uitgaande bytes (verouderd) (afgeschaft)|Bytes|Totaal|Door Voer van Event hub voor een naam ruimte. Deze metriek is afgeschaft. Gebruik in plaats hiervan de metrische gegevens van uitgaande bytes (afgeschaft)|Geen dimensies|
|EHOUTMSGS|Ja|Uitgaande berichten (afgeschaft)|Aantal|Totaal|Totaal aantal uitgaande berichten voor een naam ruimte (afgeschaft)|Geen dimensies|
|FAILREQ|Ja|Mislukte aanvragen (afgeschaft)|Aantal|Totaal|Totaal aantal mislukte aanvragen voor een naam ruimte (afgeschaft)|Geen dimensies|
|IncomingBytes|Ja|Binnenkomende bytes.|Bytes|Totaal|Binnenkomende bytes voor micro soft. EventHub.|EntityName|
|IncomingMessages|Ja|Binnenkomende berichten|Aantal|Totaal|Binnenkomende berichten voor micro soft. EventHub.|EntityName|
|IncomingRequests|Ja|Binnenkomende aanvragen|Aantal|Totaal|Binnenkomende aanvragen voor micro soft. EventHub.|EntityName|
|INMSGS|Ja|Binnenkomende berichten (verouderd) (afgeschaft)|Aantal|Totaal|Totaal aantal inkomende berichten voor een naam ruimte. Deze metriek is afgeschaft. Gebruik in plaats daarvan de metriek van binnenkomende berichten (afgeschaft)|Geen dimensies|
|INREQS|Ja|Binnenkomende aanvragen (afgeschaft)|Aantal|Totaal|Totaal aantal binnenkomende verzend aanvragen voor een naam ruimte (afgeschaft)|Geen dimensies|
|INTERer|Ja|Interne server fouten (afgeschaft)|Aantal|Totaal|Totaal aantal interne server fouten voor een naam ruimte (afgeschaft)|Geen dimensies|
|MISCERR|Ja|Andere fouten (afgeschaft)|Aantal|Totaal|Totaal aantal mislukte aanvragen voor een naam ruimte (afgeschaft)|Geen dimensies|
|OutgoingBytes|Ja|Uitgaande bytes.|Bytes|Totaal|Uitgaande bytes voor micro soft. EventHub.|EntityName|
|OutgoingMessages|Ja|Uitgaande berichten|Aantal|Totaal|Uitgaande berichten voor micro soft. EventHub.|EntityName|
|OUTMSGS|Ja|Uitgaande berichten (verouderd) (afgeschaft)|Aantal|Totaal|Totaal aantal uitgaande berichten voor een naam ruimte. Deze metriek is afgeschaft. Gebruik in plaats daarvan de metrische gegevens van uitgaande berichten (afgeschaft)|Geen dimensies|
|QuotaExceededErrors|Nee|Fouten met overschreden quota.|Aantal|Totaal|Quotum overschrijdt fouten voor micro soft. EventHub.|EntityName |
|ServerErrors|Nee|Serverfouten.|Aantal|Totaal|Server fouten voor micro soft. EventHub.|EntityName |
|Grootte|Nee|Grootte|Bytes|Average|Grootte van een EventHub in bytes.|EntityName|
|SuccessfulRequests|Nee|Geslaagde aanvragen|Aantal|Totaal|Geslaagde aanvragen voor micro soft. EventHub.|EntityName |
|SUCCREQ|Ja|Geslaagde aanvragen (afgeschaft)|Aantal|Totaal|Totaal aantal geslaagde aanvragen voor een naam ruimte (afgeschaft)|Geen dimensies|
|SVRBSY|Ja|Fouten bij server bezet (afgeschaft)|Aantal|Totaal|Totaal aantal fouten bij server bezet voor een naam ruimte (afgeschaft)|Geen dimensies|
|ThrottledRequests|Nee|Beperkte aanvragen.|Aantal|Totaal|Beperkte aanvragen voor micro soft. EventHub.|EntityName |
|UserErrors|Nee|Gebruikersfouten.|Aantal|Totaal|Gebruikers fouten voor micro soft. EventHub.|EntityName |


## <a name="microsofthdinsightclusters"></a>Micro soft. HDInsight/clusters

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|CategorizedGatewayRequests|Ja|Gecategoriseerde gateway aanvragen|Aantal|Totaal|Aantal gateway aanvragen per categorie (1xx/2xx/3xx/4xx/5xx)|Http status|
|GatewayRequests|Ja|Gateway aanvragen|Aantal|Totaal|Aantal gateway-aanvragen|Http status|
|NumActiveWorkers|Ja|Aantal actieve werk rollen|Aantal|Maximum|Aantal actieve werk rollen|MetricName|


## <a name="microsoftinsightsautoscalesettings"></a>Micro soft. Insights/AutoscaleSettings

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|MetricThreshold|Ja|Drempel waarde voor metrische gegevens|Aantal|Average|De geconfigureerde drempel waarde voor automatisch schalen wanneer automatisch schalen is uitgevoerd.|MetricTriggerRule|
|ObservedCapacity|Ja|Waargenomen capaciteit|Aantal|Average|De capaciteit die is gerapporteerd voor automatisch schalen wanneer deze wordt uitgevoerd.|Geen dimensies|
|ObservedMetricValue|Ja|Waargenomen metrische waarde|Aantal|Average|De waarde die wordt berekend door automatisch schalen wanneer deze wordt uitgevoerd|MetricTriggerSource|
|ScaleActionsInitiated|Ja|Schaal acties gestart|Aantal|Totaal|De richting van de schaal bewerking.|ScaleDirection|


## <a name="microsoftinsightscomponents"></a>Micro soft. Insights/onderdelen

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|availabilityResults/availabilityPercentage|Ja|Beschikbaarheid|Percentage|Average|Percentage voltooide beschikbaarheids tests|availabilityResult/naam, availabilityResult/locatie|
|availabilityResults/aantal|Nee|Beschikbaarheidstests|Aantal|Aantal|Aantal beschikbaarheids tests|availabilityResult/naam, availabilityResult/locatie, availabilityResult/geslaagd|
|availabilityResults/duur|Ja|Duur beschikbaarheids test|Milliseconden|Average|Duur beschikbaarheids test|availabilityResult/naam, availabilityResult/locatie, availabilityResult/geslaagd|
|browserTimings/networkDuration|Ja|Netwerk verbindings tijd voor laden van pagina|Milliseconden|Average|Tijd tussen de gebruikers aanvraag en de netwerk verbinding. Inclusief DNS-Zoek-en transport verbinding.|Geen dimensies|
|browserTimings/processingDuration|Ja|Verwerkings tijd van client|Milliseconden|Average|Tijd tussen het ontvangen van de laatste byte van een document totdat de DOM is geladen. Asynchrone aanvragen kunnen nog steeds worden verwerkt.|Geen dimensies|
|browserTimings/receiveDuration|Ja|Reactie tijd van ontvangst|Milliseconden|Average|Tijd tussen de eerste en laatste bytes, of tot de verbinding wordt verbroken.|Geen dimensies|
|browserTimings/sendDuration|Ja|Aanvraag tijd verzenden|Milliseconden|Average|Tijd tussen netwerk verbinding en ontvangst van de eerste byte.|Geen dimensies|
|browserTimings/totalDuration|Ja|Laad tijd van browser pagina|Milliseconden|Average|Tijd van de gebruikers aanvraag totdat DOM, opmaak modellen, scripts en installatie kopieën worden geladen.|Geen dimensies|
|afhankelijkheden/aantal|Nee|Afhankelijkheids aanroepen|Aantal|Aantal|Aantal aanroepen van de toepassing naar externe bronnen.|afhankelijkheid/type, afhankelijkheid/requests, afhankelijkheid/geslaagd, afhankelijkheid/doel, afhankelijkheid/resultCode, bewerking/synthetisch, Cloud/roleInstance, Cloud/rolnaam|
|afhankelijkheden/duur|Ja|Duur van afhankelijkheid|Milliseconden|Average|Duur van de aanroepen van de toepassing naar externe bronnen.|afhankelijkheid/type, afhankelijkheid/requests, afhankelijkheid/geslaagd, afhankelijkheid/doel, afhankelijkheid/resultCode, bewerking/synthetisch, Cloud/roleInstance, Cloud/rolnaam|
|afhankelijkheden/mislukt|Nee|Mislukte afhankelijkheids aanroepen|Aantal|Aantal|Aantal mislukte afhankelijkheids aanroepen van de toepassing naar externe bronnen.|afhankelijkheid/type, afhankelijkheid/requests, afhankelijkheid/doel, afhankelijkheid/resultCode, bewerking/synthetisch, Cloud/roleInstance, Cloud/rolnaam|
|uitzonde ringen/browser|Nee|Browseruitzonderingen|Aantal|Aantal|Aantal niet-onderschepte uitzonde ringen dat in de browser wordt gegenereerd.|Cloud/rolnaam|
|uitzonde ringen/aantal|Ja|Uitzonderingen|Aantal|Aantal|Totaal aantal niet-onderschepte uitzonde ringen.|Cloud/rolnaam, Cloud/roleInstance, client/type|
|uitzonde ringen/server|Nee|Server uitzonderingen|Aantal|Aantal|Aantal niet-onderschepte uitzonde ringen dat is opgetreden in de server toepassing.|Cloud/rolnaam, Cloud-roleInstance|
|Page views/aantal|Ja|Pagina weergaven|Aantal|Aantal|Aantal pagina weergaven.|bewerking/synthetisch, Cloud/rolnaam|
|Page views/duur|Ja|Laad tijd pagina weergave|Milliseconden|Average|Laad tijd pagina weergave|bewerking/synthetisch, Cloud/rolnaam|
|Performance Counters/exceptionsPerSecond|Ja|Uitzonderings frequentie|CountPerSecond|Average|Aantal verwerkte en onverwerkte uitzonde ringen die worden gerapporteerd aan Windows, inclusief .NET-uitzonde ringen en onbeheerde uitzonde ringen die worden geconverteerd naar .NET-uitzonde ringen.|Cloud-roleInstance|
|Performance Counters/memoryAvailableBytes|Ja|Beschikbaar geheugen|Bytes|Average|Fysiek geheugen dat direct beschikbaar is voor toewijzing aan een proces of voor systeem gebruik.|Cloud-roleInstance|
|Performance Counters/processCpuPercentage|Ja|CPU verwerken|Percentage|Average|Het percentage van de verstreken tijd dat alle proces threads de processor hebben gebruikt om instructies uit te voeren. Dit kan variëren tussen 0 en 100. Deze metrische gegevens duiden de prestaties van alleen het W3wp-proces aan.|Cloud-roleInstance|
|Performance Counters/processIOBytesPerSecond|Ja|I/o-frequentie van processen|BytesPerSecond|Average|Totaal aantal in bestanden, netwerk en apparaten gelezen en geschreven bytes per seconde.|Cloud-roleInstance|
|Performance Counters/processorCpuPercentage|Ja|Processor tijd|Percentage|Average|Het percentage tijd dat de processor spendeert aan niet-inactieve threads.|Cloud-roleInstance|
|Performance Counters/processPrivateBytes|Ja|Privé-bytes verwerken|Bytes|Average|Geheugen dat exclusief wordt toegewezen aan de processen van de bewaakte toepassing.|Cloud-roleInstance|
|Performance Counters/requestExecutionTime|Ja|Uitvoerings tijd van de HTTP-aanvraag|Milliseconden|Average|Uitvoerings tijd van de meest recente aanvraag.|Cloud-roleInstance|
|Performance Counters/requestsInQueue|Ja|HTTP-aanvragen in de toepassings wachtrij|Aantal|Average|Lengte van de wachtrij voor toepassings aanvragen.|Cloud-roleInstance|
|Performance Counters/requestsPerSecond|Ja|Frequentie van HTTP-aanvragen|CountPerSecond|Average|Het aantal aanvragen voor de toepassing per seconde van ASP.NET.|Cloud-roleInstance|
|aanvragen/aantal|Nee|Server aanvragen|Aantal|Aantal|Aantal voltooide HTTP-aanvragen.|aanvraag-requests, aanvraag-resultCode, bewerking/synthetisch, Cloud/roleInstance, aanvraag/geslaagd, Cloud/rolnaam|
|aanvragen/duur|Ja|Server reactietijd|Milliseconden|Average|Tijd tussen het ontvangen van een HTTP-aanvraag en het volt ooien van het verzenden van het antwoord.|aanvraag-requests, aanvraag-resultCode, bewerking/synthetisch, Cloud/roleInstance, aanvraag/geslaagd, Cloud/rolnaam|
|aanvragen/mislukt|Nee|Mislukte aanvragen|Aantal|Aantal|Het aantal HTTP-aanvragen dat is gemarkeerd als mislukt. In de meeste gevallen zijn dit aanvragen met een antwoord code >= 400 en niet gelijk is aan 401.|aanvraag-requests, aanvraag-resultCode, bewerking/synthetisch, Cloud/roleInstance, Cloud/rolnaam|
|aanvragen/frequentie|Nee|Aantal server aanvragen|CountPerSecond|Average|Aantal server aanvragen per seconde|aanvraag-requests, aanvraag-resultCode, bewerking/synthetisch, Cloud/roleInstance, aanvraag/geslaagd, Cloud/rolnaam|
|traceringen/aantal|Ja|Traceringen|Aantal|Aantal|Aantal tracerings documenten|Trace/severityLevel, Operation/synthetisch, Cloud/rolnaam, Cloud/roleInstance|

## <a name="microsoftiotcentraliotapps"></a>Micro soft. IoTCentral/IoTApps

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|C2D. Property. Read. failure|Ja|Mislukte apparaat-eigenschap lezen van IoT Central|Aantal|Totaal|Het aantal mislukte eigenschaps Lees bewerkingen dat is gestart vanuit IoT Central|Geen dimensies|
|C2D. Property. Read. geslaagd|Ja|Geslaagde apparaat-eigenschap leest van IoT Central|Aantal|Totaal|Het aantal geslaagde Lees bewerkingen van eigenschappen dat is gestart vanuit IoT Central|Geen dimensies|
|C2D. Property. update. failure|Ja|Mislukte updates van Apparaatinstellingen van IoT Central|Aantal|Totaal|Het aantal mislukte updates van eigenschappen dat vanuit IoT Central is gestart|Geen dimensies|
|C2D. Property. update. geslaagd|Ja|Geslaagde updates van de apparaat-eigenschap van IoT Central|Aantal|Totaal|Het aantal geslaagde updates van alle eigenschappen dat vanaf IoT Central is gestart|Geen dimensies|
|connectedDeviceCount|Nee|Totaal aantal verbonden apparaten|Aantal|Average|Aantal apparaten dat is verbonden met IoT Central|Geen dimensies|
|D2C. Property. Read. failure|Ja|Mislukte apparaat-eigenschap lezen van apparaten|Aantal|Totaal|Het aantal mislukte lees bewerkingen van een eigenschap die vanaf apparaten worden gestart|Geen dimensies|
|D2C. Property. Read. geslaagd|Ja|Geslaagde apparaat-eigenschap is van apparaten gelezen|Aantal|Totaal|Het aantal geslaagde, lees bewerkingen van apparaten|Geen dimensies|
|D2C. Property. update. failure|Ja|Mislukte updates van Apparaatinstellingen van apparaten|Aantal|Totaal|Het aantal mislukte updates van eigenschappen dat is geïnitieerd vanaf apparaten|Geen dimensies|
|D2C. Property. update. geslaagd|Ja|Geslaagde updates van apparaat-eigenschappen van apparaten|Aantal|Totaal|Het aantal geslaagde updates van alle eigenschappen dat is gestart vanaf apparaten|Geen dimensies|


## <a name="microsoftkeyvaultvaults"></a>Micro soft.-sleutel kluis/-kluizen

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|Beschikbaarheid|Ja|Algemene Beschik baarheid van kluis|Percentage|Average|Beschik baarheid van kluis aanvragen|Activity type, Activiteitsnummer, status code, StatusCodeClass|
|SaturationShoebox|Nee|Algehele intensiteit van de kluis|Percentage|Average|Gebruikte kluis capaciteit|Activity type, Activiteitsnummer, TransactionType|
|ServiceApiHit|Ja|Totaal aantal treffers in de service-API|Aantal|Aantal|Totaal aantal treffers in de service-API|Activity type, Activiteitsnummer|
|ServiceApiLatency|Ja|Algehele latentie van Service-API|Milliseconden|Average|Algemene latentie van Service-API-aanvragen|Activity type, Activiteitsnummer, status code, StatusCodeClass|
|ServiceApiResult|Ja|Totale resultaten van Service-API|Aantal|Aantal|Totaal aantal resultaten van Service-API|Activity type, Activiteitsnummer, status code, StatusCodeClass|


## <a name="microsoftkustoclusters"></a>Micro soft. Kusto/clusters

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|BatchBlobCount|Ja|Aantal batch-blobs|Aantal|Average|Aantal gegevens bronnen in een samengevoegde batch voor opname.|Database|
|BatchDuration|Ja|Batch duur|Seconden|Average|De duur van de aggregatie fase in de opname stroom.|Database|
|BatchesProcessed|Ja|Verwerkte batches|Aantal|Average|Het aantal batches dat is geaggregeerd voor opname. Reden voor batch voltooiing: of de batch een batch-tijd, gegevens grootte of aantal bestanden heeft bereikt dat is ingesteld op batch beleid|Data Base, SealReason|
|BatchSize|Ja|Batch grootte|Bytes|Average|Ongecomprimeerde verwachte gegevens grootte in een samengevoegde batch voor opname.|Database|
|CacheUtilization|Ja|Cache gebruik|Percentage|Average|Gebruiks niveau in het cluster bereik|Geen dimensies|
|ContinuousExportMaxLatenessMinutes|Ja|Maximale achterstand voor continue export|Aantal|Maximum|De achterstand (in minuten) die is gerapporteerd door de doorlopende export taken in het cluster|Geen dimensies|
|ContinuousExportNumOfRecordsExported|Ja|Doorlopend exporteren: aantal geëxporteerde records|Aantal|Totaal|Het aantal geëxporteerde records dat wordt geactiveerd voor elk opslag artefact dat is geschreven tijdens de export bewerking|ContinuousExportName, data base|
|ContinuousExportPendingCount|Ja|Aantal doorlopend exporteren in behandeling|Aantal|Maximum|Het aantal in behandeling zijnde doorlopende export taken dat gereed is voor uitvoering|Geen dimensies|
|ContinuousExportResult|Ja|Resultaat doorlopend exporteren|Aantal|Aantal|Hiermee wordt aangegeven of continue export is geslaagd of mislukt|ContinuousExportName, resultaat, data base|
|CPU|Ja|CPU|Percentage|Average|Niveau CPU-gebruik|Geen dimensies|
|EventsProcessedForEventHubs|Ja|Verwerkte gebeurtenissen (voor gebeurtenis/IoT-hubs)|Aantal|Totaal|Aantal gebeurtenissen dat door het cluster wordt verwerkt bij het opnemen van gebeurtenis/IoT Hub|EventStatus|
|ExportUtilization|Ja|Exportgebruik|Percentage|Maximum|Gebruik exporteren|Geen dimensies|
|IngestionLatencyInSeconds|Ja|Opname latentie (in seconden)|Seconden|Average|Opname tijd van de bron (bijvoorbeeld bericht is in EventHub) naar het cluster in enkele seconden|Geen dimensies|
|IngestionResult|Ja|Opname resultaat|Aantal|Aantal|Aantal opname bewerkingen|IngestionResultDetails|
|IngestionUtilization|Ja|Opname gebruik|Percentage|Average|Verhouding van gebruikte opname sleuven in het cluster|Geen dimensies|
|IngestionVolumeInMB|Ja|Opname volume (in MB)|Aantal|Totaal|Totaal volume van opgenomen gegevens aan het cluster (in MB)|Geen dimensies|
|InstanceCount|Ja|Aantal instanties|Aantal|Average|Totaal aantal instanties|Geen dimensies|
|KeepAlive|Ja|Actief houden|Aantal|Average|Sanity-controle geeft aan dat het cluster reageert op query's|Geen dimensies|
|QueryDuration|Ja|Query duur|Milliseconden|Average|De duur van query's in seconden|QueryStatus|
|SteamingIngestRequestRate|Ja|Aanvraagsnelheid streamingopname|Aantal|RateRequestsPerSecond|Aanvraag frequentie voor streaming-opname (aanvragen per seconde)|Geen dimensies|
|StreamingIngestDataRate|Ja|Gegevenssnelheid streamingopname|Aantal|Average|Gegevens frequentie van streaming-opname (MB per seconde)|Geen dimensies|
|StreamingIngestDuration|Ja|Duur streamingopname|Milliseconden|Average|De duur van het opnemen van gegevens stromen in milliseconden|Geen dimensies|
|StreamingIngestResults|Ja|Resultaat streamingopname|Aantal|Average|Resultaat van streaming-opname|Resultaat|
|TotalNumberOfConcurrentQueries|Ja|Totaal aantal gelijktijdige query's|Aantal|Totaal|Totaal aantal gelijktijdige query's|Geen dimensies|
|TotalNumberOfExtents|Ja|Totaal aantal gebieden|Aantal|Totaal|Totaal aantal gegevens gebieden|Geen dimensies|
|TotalNumberOfThrottledCommands|Ja|Totaal aantal vertraagde opdrachten|Aantal|Totaal|Totaal aantal vertraagde opdrachten|CommandType|
|TotalNumberOfThrottledQueries|Ja|Totaal aantal vertraagde query's|Aantal|Totaal|Totaal aantal vertraagde query's|Geen dimensies|


## <a name="microsoftlogicintegrationserviceenvironments"></a>Micro soft. Logic/integrationServiceEnvironments

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|ActionLatency|Ja|Actie latentie |Seconden|Average|Latentie van voltooide werk stroom acties.|Geen dimensies|
|ActionsCompleted|Ja|Acties voltooid |Aantal|Totaal|Aantal voltooide werk stroom acties.|Geen dimensies|
|ActionsFailed|Ja|Mislukte acties |Aantal|Totaal|Aantal mislukte werk stroom acties.|Geen dimensies|
|ActionsSkipped|Ja|Overgeslagen acties |Aantal|Totaal|Aantal overgeslagen werk stroom acties.|Geen dimensies|
|ActionsStarted|Ja|Gestarte acties |Aantal|Totaal|Aantal gestarte werk stroom acties.|Geen dimensies|
|ActionsSucceeded|Ja|Acties geslaagd |Aantal|Totaal|Aantal geslaagde werk stroom acties.|Geen dimensies|
|ActionSuccessLatency|Ja|Latentie geslaagde acties |Seconden|Average|Latentie van geslaagde werk stroom acties.|Geen dimensies|
|ActionThrottledEvents|Ja|Door actie vertraagde gebeurtenissen|Aantal|Totaal|Aantal door werk stroom actie vertraagde gebeurtenissen..|Geen dimensies|
|IntegrationServiceEnvironmentConnectorMemoryUsage|Ja|Geheugen gebruik van connector voor Integratieserviceomgeving|Percentage|Average|Geheugen gebruik van connector voor de integratie service omgeving.|Geen dimensies|
|IntegrationServiceEnvironmentConnectorProcessorUsage|Ja|Processor gebruik van connector voor Integratieserviceomgeving|Percentage|Average|Het processor gebruik van de connector voor de integratie service omgeving.|Geen dimensies|
|IntegrationServiceEnvironmentWorkflowMemoryUsage|Ja|Geheugen gebruik van werk stroom voor Integratieserviceomgeving|Percentage|Average|Geheugen gebruik van werk stroom voor de integratie service omgeving.|Geen dimensies|
|IntegrationServiceEnvironmentWorkflowProcessorUsage|Ja|Gebruik van werk stroom processor voor Integratieserviceomgeving|Percentage|Average|Gebruik van werk stroom processoren voor de integratie service omgeving.|Geen dimensies|
|RunFailurePercentage|Ja|Percentage mislukte uitvoeringen|Percentage|Totaal|Percentage mislukte werk stroom uitvoeringen.|Geen dimensies|
|RunLatency|Ja|Uitvoerings latentie|Seconden|Average|Latentie van voltooide werk stroom uitvoeringen.|Geen dimensies|
|RunsCancelled|Ja|Geannuleerde uitvoeringen|Aantal|Totaal|Aantal uitgevoerde werk stroom uitvoeringen geannuleerd.|Geen dimensies|
|RunsCompleted|Ja|Uitvoeringen voltooid|Aantal|Totaal|Aantal voltooide werk stroom uitvoeringen.|Geen dimensies|
|RunsFailed|Ja|Uitvoeringen mislukt|Aantal|Totaal|Het aantal uitvoeringen van de werk stroom is mislukt.|Geen dimensies|
|RunsStarted|Ja|Uitvoeringen gestart|Aantal|Totaal|Aantal uitvoeringen van werk stroom gestart.|Geen dimensies|
|RunsSucceeded|Ja|Geslaagde uitvoeringen|Aantal|Totaal|Aantal geslaagde werk stroom uitvoeringen.|Geen dimensies|
|RunStartThrottledEvents|Ja|Vertraagde gebeurtenissen uitvoeren|Aantal|Totaal|Aantal uitgevoerde vertraagde gebeurtenissen voor de werk stroom.|Geen dimensies|
|RunSuccessLatency|Ja|Latentie van geslaagde uitvoering|Seconden|Average|Latentie van geslaagde werk stroom uitvoeringen.|Geen dimensies|
|RunThrottledEvents|Ja|Vertraagde gebeurtenissen uitvoeren|Aantal|Totaal|Aantal werk stroom acties of trigger vertraagde gebeurtenissen.|Geen dimensies|
|TriggerFireLatency|Ja|Brand latentie activeren |Seconden|Average|Latentie van geactiveerde werk stroom triggers.|Geen dimensies|
|TriggerLatency|Ja|Latentie van trigger |Seconden|Average|Latentie van voltooide werk stroom triggers.|Geen dimensies|
|TriggersCompleted|Ja|Triggers voltooid |Aantal|Totaal|Aantal voltooide werk stroom triggers.|Geen dimensies|
|TriggersFailed|Ja|Mislukte triggers |Aantal|Totaal|Aantal mislukte werk stroom triggers.|Geen dimensies|
|TriggersFired|Ja|Geactiveerde triggers |Aantal|Totaal|Aantal geactiveerde werk stroom triggers.|Geen dimensies|
|TriggersSkipped|Ja|Triggers overgeslagen|Aantal|Totaal|Aantal overgeslagen werk stroom triggers.|Geen dimensies|
|TriggersStarted|Ja|Triggers gestart |Aantal|Totaal|Aantal gestarte werk stroom triggers.|Geen dimensies|
|TriggersSucceeded|Ja|Geslaagde triggers |Aantal|Totaal|Aantal geslaagde werk stroom triggers.|Geen dimensies|
|TriggerSuccessLatency|Ja|Latentie van trigger geslaagd |Seconden|Average|Latentie van geslaagde werk stroom triggers.|Geen dimensies|
|TriggerThrottledEvents|Ja|Trigger beperkings gebeurtenissen|Aantal|Totaal|Aantal door werk stroom trigger vertraagde gebeurtenissen.|Geen dimensies|


## <a name="microsoftlogicworkflows"></a>Microsoft.Logic/workflows

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|ActionLatency|Ja|Actie latentie |Seconden|Average|Latentie van voltooide werk stroom acties.|Geen dimensies|
|ActionsCompleted|Ja|Acties voltooid |Aantal|Totaal|Aantal voltooide werk stroom acties.|Geen dimensies|
|ActionsFailed|Ja|Mislukte acties |Aantal|Totaal|Aantal mislukte werk stroom acties.|Geen dimensies|
|ActionsSkipped|Ja|Overgeslagen acties |Aantal|Totaal|Aantal overgeslagen werk stroom acties.|Geen dimensies|
|ActionsStarted|Ja|Gestarte acties |Aantal|Totaal|Aantal gestarte werk stroom acties.|Geen dimensies|
|ActionsSucceeded|Ja|Acties geslaagd |Aantal|Totaal|Aantal geslaagde werk stroom acties.|Geen dimensies|
|ActionSuccessLatency|Ja|Latentie geslaagde acties |Seconden|Average|Latentie van geslaagde werk stroom acties.|Geen dimensies|
|ActionThrottledEvents|Ja|Door actie vertraagde gebeurtenissen|Aantal|Totaal|Aantal door werk stroom actie vertraagde gebeurtenissen..|Geen dimensies|
|BillableActionExecutions|Ja|Factureer bare actie-uitvoeringen|Aantal|Totaal|Aantal uitvoeringen van werk stroom acties dat wordt gefactureerd.|Geen dimensies|
|BillableTriggerExecutions|Ja|Factureer bare trigger uitvoeringen|Aantal|Totaal|Aantal uitvoeringen van werk stroom trigger dat wordt gefactureerd.|Geen dimensies|
|BillingUsageNativeOperation|Ja|Facturerings gebruik voor uitvoering van systeem eigen bewerkingen|Aantal|Totaal|Aantal uitvoeringen van de native bewerking dat wordt gefactureerd.|Geen dimensies|
|BillingUsageStandardConnector|Ja|Facturerings gebruik voor het uitvoeren van standaard-connectors|Aantal|Totaal|Aantal uitvoeringen van Standard-connector dat wordt gefactureerd.|Geen dimensies|
|BillingUsageStorageConsumption|Ja|Facturerings gebruik voor uitvoeringen van opslag verbruik|Aantal|Totaal|Aantal uitvoeringen van opslag verbruik dat wordt gefactureerd.|Geen dimensies|
|RunFailurePercentage|Ja|Percentage mislukte uitvoeringen|Percentage|Totaal|Percentage mislukte werk stroom uitvoeringen.|Geen dimensies|
|RunLatency|Ja|Uitvoerings latentie|Seconden|Average|Latentie van voltooide werk stroom uitvoeringen.|Geen dimensies|
|RunsCancelled|Ja|Geannuleerde uitvoeringen|Aantal|Totaal|Aantal uitgevoerde werk stroom uitvoeringen geannuleerd.|Geen dimensies|
|RunsCompleted|Ja|Uitvoeringen voltooid|Aantal|Totaal|Aantal voltooide werk stroom uitvoeringen.|Geen dimensies|
|RunsFailed|Ja|Uitvoeringen mislukt|Aantal|Totaal|Het aantal uitvoeringen van de werk stroom is mislukt.|Geen dimensies|
|RunsStarted|Ja|Uitvoeringen gestart|Aantal|Totaal|Aantal uitvoeringen van werk stroom gestart.|Geen dimensies|
|RunsSucceeded|Ja|Geslaagde uitvoeringen|Aantal|Totaal|Aantal geslaagde werk stroom uitvoeringen.|Geen dimensies|
|RunStartThrottledEvents|Ja|Vertraagde gebeurtenissen uitvoeren|Aantal|Totaal|Aantal uitgevoerde vertraagde gebeurtenissen voor de werk stroom.|Geen dimensies|
|RunSuccessLatency|Ja|Latentie van geslaagde uitvoering|Seconden|Average|Latentie van geslaagde werk stroom uitvoeringen.|Geen dimensies|
|RunThrottledEvents|Ja|Vertraagde gebeurtenissen uitvoeren|Aantal|Totaal|Aantal werk stroom acties of trigger vertraagde gebeurtenissen.|Geen dimensies|
|TotalBillableExecutions|Ja|Totaal aantal factureer bare uitvoeringen|Aantal|Totaal|Aantal uitgevoerde werk stroom uitvoeringen dat wordt gefactureerd.|Geen dimensies|
|TriggerFireLatency|Ja|Brand latentie activeren |Seconden|Average|Latentie van geactiveerde werk stroom triggers.|Geen dimensies|
|TriggerLatency|Ja|Latentie van trigger |Seconden|Average|Latentie van voltooide werk stroom triggers.|Geen dimensies|
|TriggersCompleted|Ja|Triggers voltooid |Aantal|Totaal|Aantal voltooide werk stroom triggers.|Geen dimensies|
|TriggersFailed|Ja|Mislukte triggers |Aantal|Totaal|Aantal mislukte werk stroom triggers.|Geen dimensies|
|TriggersFired|Ja|Geactiveerde triggers |Aantal|Totaal|Aantal geactiveerde werk stroom triggers.|Geen dimensies|
|TriggersSkipped|Ja|Triggers overgeslagen|Aantal|Totaal|Aantal overgeslagen werk stroom triggers.|Geen dimensies|
|TriggersStarted|Ja|Triggers gestart |Aantal|Totaal|Aantal gestarte werk stroom triggers.|Geen dimensies|
|TriggersSucceeded|Ja|Geslaagde triggers |Aantal|Totaal|Aantal geslaagde werk stroom triggers.|Geen dimensies|
|TriggerSuccessLatency|Ja|Latentie van trigger geslaagd |Seconden|Average|Latentie van geslaagde werk stroom triggers.|Geen dimensies|
|TriggerThrottledEvents|Ja|Trigger beperkings gebeurtenissen|Aantal|Totaal|Aantal door werk stroom trigger vertraagde gebeurtenissen.|Geen dimensies|


## <a name="microsoftmachinelearningservicesworkspaces"></a>Micro soft. MachineLearningServices/werk ruimten

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|Actieve kernen|Ja|Actieve kernen|Aantal|Average|Aantal actieve kernen|Scenario, clustername|
|Actieve knoop punten|Ja|Actieve knoop punten|Aantal|Average|Aantal Active-knoop punten. Dit zijn de knoop punten waarop een taak actief wordt uitgevoerd.|Scenario, clustername|
|Geannuleerde uitvoeringen annuleren|Ja|Geannuleerde uitvoeringen annuleren|Aantal|Totaal|Aantal uitvoeringen waarvoor annuleren is aangevraagd voor deze werk ruimte. Het aantal wordt bijgewerkt wanneer de annulerings aanvraag is ontvangen voor een run.|Scenario, RunType, PublishedPipelineId, ComputeType, PipelineStepType|
|Geannuleerde uitvoeringen|Ja|Geannuleerde uitvoeringen|Aantal|Totaal|Het aantal uitvoeringen dat is geannuleerd voor deze werk ruimte. Het aantal wordt bijgewerkt wanneer een uitvoering is geannuleerd.|Scenario, RunType, PublishedPipelineId, ComputeType, PipelineStepType|
|Voltooide uitvoeringen|Ja|Voltooide uitvoeringen|Aantal|Totaal|Het aantal uitvoeringen dat is voltooid voor deze werk ruimte. Het aantal wordt bijgewerkt wanneer een uitvoering is voltooid en de uitvoer is verzameld.|Scenario, RunType, PublishedPipelineId, ComputeType, PipelineStepType|
|CpuUtilization|Ja|CpuUtilization|Aantal|Average|Percentage geheugen gebruik op een CPU-knoop punt. Het gebruik wordt met een interval van één minuut gerapporteerd.|Scenario, runId, NodeId, clustername|
|Fouten|Ja|Fouten|Aantal|Totaal|Aantal uitvoerings fouten in deze werk ruimte. Het aantal wordt bijgewerkt wanneer er een fout optreedt.|Scenario|
|Mislukte uitvoeringen|Ja|Mislukte uitvoeringen|Aantal|Totaal|Aantal uitvoeringen is mislukt voor deze werk ruimte. Het aantal wordt bijgewerkt wanneer een uitvoering mislukt.|Scenario, RunType, PublishedPipelineId, ComputeType, PipelineStepType|
|Uitvoeringen volt ooien|Ja|Uitvoeringen volt ooien|Aantal|Totaal|Aantal uitvoeringen dat de voltooiings status voor deze werk ruimte heeft opgegeven. Het aantal wordt bijgewerkt wanneer een uitvoering is voltooid, maar de uitvoer verzameling nog steeds wordt uitgevoerd.|Scenario, RunType, PublishedPipelineId, ComputeType, PipelineStepType|
|GpuUtilization|Ja|GpuUtilization|Aantal|Average|Percentage geheugen gebruik op een GPU-knoop punt. Het gebruik wordt met een interval van één minuut gerapporteerd.|Scenario, runId, NodeId, DeviceId, clustername|
|Niet-actieve kernen|Ja|Niet-actieve kernen|Aantal|Average|Aantal niet-actieve kern geheugens|Scenario, clustername|
|Niet-actieve knoop punten|Ja|Niet-actieve knoop punten|Aantal|Average|Aantal niet-actieve knoop punten. Niet-actieve knoop punten zijn de knoop punten waarop geen taken worden uitgevoerd, maar u kunt wel een nieuwe taak accepteren, indien beschikbaar.|Scenario, clustername|
|Kernen verlaten|Ja|Kernen verlaten|Aantal|Average|Aantal te verlaten kernen|Scenario, clustername|
|Knoop punten verlaten|Ja|Knoop punten verlaten|Aantal|Average|Aantal knoop punten dat de poort verlaat. Als u knoop punten verlaat, worden de knoop punten die zojuist de verwerking van een taak hebben voltooid, naar de niet-actieve status verzonden.|Scenario, clustername|
|Modelimplementatie is mislukt|Ja|Modelimplementatie is mislukt|Aantal|Totaal|Aantal model implementaties die zijn mislukt in deze werk ruimte|Scenario, status code|
|Modelimplementatie gestart|Ja|Modelimplementatie gestart|Aantal|Totaal|Aantal model implementaties gestart in deze werk ruimte|Scenario|
|Modelimplementatie geslaagd|Ja|Modelimplementatie geslaagd|Aantal|Totaal|Aantal model implementaties dat is geslaagd in deze werk ruimte|Scenario|
|Model register mislukt|Ja|Model register mislukt|Aantal|Totaal|Aantal model registraties dat is mislukt in deze werk ruimte|Scenario, status code|
|Model registratie is voltooid|Ja|Model registratie is voltooid|Aantal|Totaal|Aantal model registraties dat is geslaagd in deze werk ruimte|Scenario|
|Reageert niet|Ja|Reageert niet|Aantal|Totaal|Het aantal uitvoeringen dat niet reageert voor deze werk ruimte. Het aantal wordt bijgewerkt wanneer de status van een uitvoering niet reageert.|Scenario, RunType, PublishedPipelineId, ComputeType, PipelineStepType|
|Niet gestart uitvoeringen|Ja|Niet gestart uitvoeringen|Aantal|Totaal|Het aantal uitvoeringen in de status niet gestart voor deze werk ruimte. Het aantal wordt bijgewerkt wanneer een aanvraag wordt ontvangen om een uitvoering te maken, maar er is nog geen uitvoerings informatie ingevuld. |Scenario, RunType, PublishedPipelineId, ComputeType, PipelineStepType|
|Afgebroken kernen|Ja|Afgebroken kernen|Aantal|Average|Aantal afgebroken kernen|Scenario, clustername|
|Knoop punten die zijn afgebroken|Ja|Knoop punten die zijn afgebroken|Aantal|Average|Aantal knoop punten dat is afgebroken. Deze knoop punten zijn de knoop punten met een lage prioriteit die worden verwijderd uit de beschik bare knooppunt groep.|Scenario, clustername|
|Uitvoering voorbereiden|Ja|Uitvoering voorbereiden|Aantal|Totaal|Aantal uitvoeringen dat wordt voor bereid voor deze werk ruimte. Het aantal wordt bijgewerkt wanneer de status van een uitvoering wordt voor bereid terwijl de uitvoerings omgeving wordt voor bereid.|Scenario, RunType, PublishedPipelineId, ComputeType, PipelineStepType|
|Uitvoeringen van inrichting|Ja|Uitvoeringen van inrichting|Aantal|Totaal|Aantal uitvoeringen dat wordt ingericht voor deze werk ruimte. Het aantal wordt bijgewerkt wanneer een uitvoering wordt gewacht op het maken of inrichten van het reken doel.|Scenario, RunType, PublishedPipelineId, ComputeType, PipelineStepType|
|Uitvoeringen in wachtrij|Ja|Uitvoeringen in wachtrij|Aantal|Totaal|Aantal uitvoeringen dat in de wachtrij staat voor deze werk ruimte. Het aantal wordt bijgewerkt wanneer een uitvoering in de wachtrij wordt geplaatst in een compute-doel. Kan zich voordoen als er wordt gewacht tot de vereiste reken knooppunten gereed zijn.|Scenario, RunType, PublishedPipelineId, ComputeType, PipelineStepType|
|Percentage quotum gebruik|Ja|Percentage quotum gebruik|Aantal|Average|Percentage van gebruikte quota|Scenario, clustername, VmFamilyName, VmPriority|
|Gestart uitvoeringen|Ja|Gestart uitvoeringen|Aantal|Totaal|Aantal uitvoeringen dat wordt uitgevoerd voor deze werk ruimte. Het aantal wordt bijgewerkt wanneer de uitvoering wordt uitgevoerd op de vereiste resources.|Scenario, RunType, PublishedPipelineId, ComputeType, PipelineStepType|
|Uitvoeringen starten|Ja|Uitvoeringen starten|Aantal|Totaal|Aantal uitvoeringen gestart voor deze werk ruimte. Het aantal wordt bijgewerkt nadat de aanvraag voor het maken van uitvoerings-en uitvoerings gegevens, zoals de uitvoerings-id, is ingevuld|Scenario, RunType, PublishedPipelineId, ComputeType, PipelineStepType|
|Totaal aantal kernen|Ja|Totaal aantal kernen|Aantal|Average|Aantal totale kernen|Scenario, clustername|
|Totaal aantal knoop punten|Ja|Totaal aantal knoop punten|Aantal|Average|Totaal aantal knoop punten. Dit totaal omvat enkele actieve knoop punten, niet-actieve knoop punten, onbruikbaare knoop punten, Premepted knoop punten, waardoor knoop punten|Scenario, clustername|
|Onbruikbaar aantal kern geheugens|Ja|Onbruikbaar aantal kern geheugens|Aantal|Average|Aantal niet-bruikbare kernen|Scenario, clustername|
|Niet-bruikbare knoop punten|Ja|Niet-bruikbare knoop punten|Aantal|Average|Aantal niet-bruikbare knoop punten. Niet-bruikbare knoop punten zijn niet functioneel vanwege een probleem met onherleidbare. Deze knoop punten worden door Azure gerecycled.|Scenario, clustername|
|Waarschuwingen|Ja|Waarschuwingen|Aantal|Totaal|Aantal uitvoerings waarschuwingen in deze werk ruimte. Het aantal wordt bijgewerkt wanneer er een waarschuwing wordt gevonden.|Scenario|


## <a name="microsoftmapsaccounts"></a>Micro soft. Maps/accounts

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|Beschikbaarheid|Ja|Beschikbaarheid|Percentage|Average|Beschik baarheid van de Api's|ApiCategory, ApiName|
|Gebruik|Nee|Gebruik|Aantal|Aantal|Aantal API-aanroepen|ApiCategory, ApiName, ResultType, ResponseCode|


## <a name="microsoftmediamediaservices"></a>Micro soft. Media/Media Services

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|AssetCount|Ja|Aantal assets|Aantal|Average|Hoeveel activa er al zijn gemaakt in het huidige media service-account|Geen dimensies|
|AssetQuota|Ja|Activa quotum|Aantal|Average|Hoeveel assets zijn toegestaan voor het huidige media service-account|Geen dimensies|
|AssetQuotaUsedPercentage|Ja|Percentage gebruikt voor het activa quotum|Percentage|Average|Verbruikt percentage in huidige media service account|Geen dimensies|
|ContentKeyPolicyCount|Ja|Aantal beleids regels voor inhouds sleutels|Aantal|Average|Hoeveel beleids regels voor inhouds sleutels zijn al gemaakt in het huidige media service-account|Geen dimensies|
|ContentKeyPolicyQuota|Ja|Quotum voor inhouds sleutel beleid|Aantal|Average|Hoeveel beleids regels voor inhouds sleutels zijn toegestaan voor het huidige media service-account|Geen dimensies|
|ContentKeyPolicyQuotaUsedPercentage|Ja|Percentage gebruikt quotum voor inhouds sleutel beleid|Percentage|Average|Gebruikt percentage van beleid voor inhouds sleutels in het huidige media service-account|Geen dimensies|
|StreamingPolicyCount|Ja|Aantal stroomsgewijze beleids regels|Aantal|Average|Hoeveel streaming-beleids regels zijn al gemaakt in het huidige media service-account|Geen dimensies|
|StreamingPolicyQuota|Ja|Quota voor streaming-beleid|Aantal|Average|Hoeveel streaming-beleids regels zijn toegestaan voor het huidige media service-account|Geen dimensies|
|StreamingPolicyQuotaUsedPercentage|Ja|Percentage gebruikt quotum voor het streaming-beleid|Percentage|Average|Gebruikt beleid voor streamingbeleid in het huidige media service-account|Geen dimensies|


## <a name="microsoftmediamediaservicesstreamingendpoints"></a>Micro soft. Media/Media Services/streamingEndpoints

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|Uitgaand verkeer|Ja|Uitgaand verkeer|Bytes|Totaal|De hoeveelheid uitgangs gegevens, in bytes.|OutputFormat|
|Aanvragen|Ja|Aanvragen|Aantal|Totaal|Aanvragen voor een streaming-eind punt.|Output Format, HTTP status code, error code|
|SuccessE2ELatency|Ja|Geslaagde end-to-end-latentie|Milliseconden|Average|De gemiddelde latentie voor voltooide aanvragen in milliseconden.|OutputFormat|


## <a name="microsoftnetappnetappaccountscapacitypools"></a>Micro soft. NetApp/netAppAccounts/capacityPools

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|VolumePoolAllocatedSize|Ja|Grootte toegewezen geheugen|Bytes|Average|Ingerichte grootte van deze groep|Geen dimensies|
|VolumePoolAllocatedUsed|Ja|Groep toegewezen aan volume grootte|Bytes|Average|Gebruikte grootte van de pool is toegewezen|Geen dimensies|
|VolumePoolTotalLogicalSize|Ja|Verbruikte grootte van pool|Bytes|Average|Som van de logische grootte van alle volumes die deel uitmaken van de groep|Geen dimensies|
|VolumePoolTotalSnapshotSize|Ja|Totale grootte van de moment opname voor de pool|Bytes|Average|Som van de grootte van de moment opname van alle volumes in deze groep|Geen dimensies|


## <a name="microsoftnetappnetappaccountscapacitypoolsvolumes"></a>Micro soft. NetApp/netAppAccounts/capacityPools/volumes

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|AverageReadLatency|Ja|Gemiddelde lees latentie|Milliseconden|Average|Gemiddelde lees latentie in milliseconden per bewerking|Geen dimensies|
|AverageWriteLatency|Ja|Gemiddelde schrijf latentie|Milliseconden|Average|Gemiddelde schrijf latentie in milliseconden per bewerking|Geen dimensies|
|CbsVolumeBackupActive|Ja|Volume back-up actieve status|Aantal|Average|Is een back-up die momenteel is onderbroken voor het volume.|Geen dimensies|
|CbsVolumeLogicalBackupBytes|Ja|Back-ups van logische bytes|Bytes|Average|Voor dit volume is een back-up van toatl niet-gecomprimeerde/niet-versleutelde bytes.|Geen dimensies|
|CbsVolumeOperationComplete|Ja|Bewerkings status|Aantal|Average|Is de laatste back-up-of herstel bewerking is voltooid.|Geen dimensies|
|CbsVolumeOperationTransferredBytes|Ja|Verzonden bytes voor bewerking|Bytes|Average|Totaal aantal overgebrachte bytes voor de laatste back-up/herstel bewerking.|Geen dimensies|
|CbsVolumeProtected|Ja|Volume beveiligde status|Aantal|Average|Is het volume dat wordt beveiligd door de Cloud backup-service.|Geen dimensies|
|ReadIops|Ja|IOPS lezen|CountPerSecond|Average|In-en uitvoer bewerkingen per seconde|Geen dimensies|
|VolumeAllocatedSize|Ja|Grootte van toegewezen volume|Bytes|Average|De ingerichte grootte van een volume|Geen dimensies|
|VolumeLogicalSize|Ja|Grootte van gebruikt volume|Bytes|Average|Logische grootte van het volume (gebruikte bytes)|Geen dimensies|
|VolumeSnapshotSize|Ja|Grootte van moment opname van volume|Bytes|Average|Grootte van alle moment opnamen in volume|Geen dimensies|
|WriteIops|Ja|IOPS schrijven|CountPerSecond|Average|In-en uitvoer bewerkingen per seconde|Geen dimensies|
|XregionReplicationHealthy|Ja|Is de status van de volume replicatie in orde|Aantal|Average|De voor waarde van de relatie, 1 of 0.|Geen dimensies|
|XregionReplicationLagTime|Ja|Vertragings tijd voor volume replicatie|Seconden|Average|De hoeveelheid tijd in seconden waarmee de gegevens op de mirror lags achter de bron.|Geen dimensies|
|XregionReplicationLastTransferDuration|Ja|Duur van de laatste overdracht van volume replicatie|Seconden|Average|De hoeveelheid tijd in seconden die het duurt voordat de laatste overdracht is voltooid.|Geen dimensies|
|XregionReplicationLastTransferSize|Ja|Grootte van laatste overdracht volume replicatie|Bytes|Average|Het totale aantal bytes dat is overgedragen als onderdeel van de laatste overdracht.|Geen dimensies|
|XregionReplicationRelationshipProgress|Ja|Voortgang van volume replicatie|Bytes|Average|De totale hoeveelheid gegevens die wordt overgedragen voor de huidige overdrachts bewerking.|Geen dimensies|
|XregionReplicationRelationshipTransferring|Ja|Wordt de overdracht van de volume replicatie|Aantal|Average|Hiermee wordt aangegeven of de status van de volume replicatie ' Transfer ' is.|Geen dimensies|
|XregionReplicationTotalTransferBytes|Ja|Totale overdracht van volume replicatie|Bytes|Average|De cumulatieve bytes die zijn overgedragen voor de relatie.|Geen dimensies|


## <a name="microsoftnetworkapplicationgateways"></a>Micro soft. Network/applicationGateways

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|ApplicationGatewayTotalTime|Nee|Totale tijd van Application Gateway|Milliseconden|Average|De gemiddelde tijd die nodig is voor het verwerken van een aanvraag en het antwoord op verzen ding. Dit wordt berekend als gemiddelde van het interval van de tijd dat Application Gateway de eerste byte van een HTTP-aanvraag ontvangt naar het tijdstip waarop de bewerking voor het verzenden van het antwoord is voltooid. Het is belang rijk te weten dat dit doorgaans de verwerkings tijd van Application Gateway, de tijd dat de aanvraag-en antwoord pakketten op het netwerk onderweg zijn en het tijdstip waarop de back-end-server heeft gereageerd.|Listener|
|AvgRequestCountPerHealthyHost|Nee|Aanvragen per minuut per gegezonde host|Aantal|Average|Gemiddeld aantal aanvragen per minuut op een gezonde backend-host in een groep|BackendSettingsPool|
|BackendConnectTime|Nee|Moment back-end verbinding|Milliseconden|Average|Tijd die is besteed aan het tot stand brengen van een verbinding met een back-end-server|Listener, BackendServer, hosts, BackendHttpSetting|
|BackendFirstByteResponseTime|Nee|Reactie tijd eerste byte van back-end|Milliseconden|Average|Tijds interval tussen begin van het tot stand brengen van een verbinding met de back-end-server en het ontvangen van de eerste byte van de reactie header, geschatte verwerkings tijd van de back-endserver|Listener, BackendServer, hosts, BackendHttpSetting|
|BackendLastByteResponseTime|Nee|Reactie tijd laatste byte van back-end|Milliseconden|Average|Tijds interval tussen begin van het tot stand brengen van een verbinding met de back-endserver en het ontvangen van de laatste byte van de antwoord tekst|Listener, BackendServer, hosts, BackendHttpSetting|
|BackendResponseStatus|Ja|Reactie status van back-end|Aantal|Totaal|Het aantal HTTP-antwoord codes dat door de back-end-leden is gegenereerd. Dit omvat geen antwoord codes die zijn gegenereerd door de Application Gateway.|BackendServer, hosts, BackendHttpSetting, HttpStatusGroup|
|BlockedCount|Ja|Regel distributie voor door Web Application firewall geblokkeerde aanvragen|Aantal|Totaal|Regel distributie voor door Web Application firewall geblokkeerde aanvragen|RuleGroup, RuleId|
|BlockedReqCount|Ja|Aantal geblokkeerde aanvragen voor Web Application firewall|Aantal|Totaal|Aantal geblokkeerde aanvragen voor Web Application firewall|Geen dimensies|
|BytesReceived|Ja|Ontvangen bytes|Bytes|Totaal|Het totale aantal bytes dat is ontvangen door de Application Gateway van de clients|Listener|
|Bytes sent|Ja|Verzonden bytes|Bytes|Totaal|Het totale aantal bytes dat is verzonden door de Application Gateway naar de clients|Listener|
|CapacityUnits|Nee|Huidige capaciteits eenheden|Aantal|Average|Verbruikte capaciteits eenheden|Geen dimensies|
|ClientRtt|Nee|Client RTT|Milliseconden|Average|Gemiddelde Round-retour tijd tussen clients en Application Gateway. Met deze metriek wordt aangegeven hoe lang het duurt om verbindingen tot stand te brengen en bevestigingen te retour neren|Listener|
|ComputeUnits|Nee|Huidige reken eenheden|Aantal|Average|Verbruikte reken eenheden|Geen dimensies|
|CpuUtilization|Nee|CPU-gebruik|Percentage|Average|Huidig CPU-gebruik van de Application Gateway|Geen dimensies|
|CurrentConnections|Ja|Huidige verbindingen|Aantal|Totaal|Aantal actieve verbindingen dat tot stand is gebracht met Application Gateway|Geen dimensies|
|EstimatedBilledCapacityUnits|Nee|Geschatte gefactureerde capaciteits eenheden|Aantal|Average|Geschatte capaciteits eenheden waarvoor kosten in rekening worden gebracht|Geen dimensies|
|FailedRequests|Ja|Mislukte aanvragen|Aantal|Totaal|Aantal mislukte aanvragen dat Application Gateway heeft bediend|BackendSettingsPool|
|FixedBillableCapacityUnits|Nee|Vaste factureerbare capaciteitseenheden|Aantal|Average|Minimale capaciteits eenheden waarvoor kosten in rekening worden gebracht|Geen dimensies|
|HealthyHostCount|Ja|Aantal goede hosts|Aantal|Average|Aantal gezonde backend-hosts|BackendSettingsPool|
|MatchedCount|Ja|Totale regel distributie Web Application firewall|Aantal|Totaal|Totale regel distributie Web Application Firewall voor het binnenkomende verkeer|RuleGroup, RuleId|
|NewConnectionsPerSecond|Nee|Nieuwe verbindingen per seconde|CountPerSecond|Average|Nieuwe verbindingen per seconde die zijn gemaakt met Application Gateway|Geen dimensies|
|ResponseStatus|Ja|Reactie status|Aantal|Totaal|Http-antwoord status geretourneerd door Application Gateway|HttpStatusGroup|
|Doorvoer|Nee|Doorvoer|BytesPerSecond|Average|Aantal bytes per seconde dat de Application Gateway heeft bediend|Geen dimensies|
|TlsProtocol|Ja|TLS-protocol van client|Aantal|Totaal|Het aantal TLS-en niet-TLS-aanvragen dat door de client is gestart en die verbinding heeft gemaakt met de Application Gateway. Als u de TLS-protocol distributie wilt weer geven, filtert u op het TLS-protocol van de dimensie.|Listener, TlsProtocol|
|TotalRequests|Ja|Totaal aantal aanvragen|Aantal|Totaal|Aantal geslaagde aanvragen dat Application Gateway heeft bediend|BackendSettingsPool|
|UnhealthyHostCount|Ja|Aantal hosts met slechte status|Aantal|Average|Aantal beschadigde backend-hosts|BackendSettingsPool|


## <a name="microsoftnetworkazurefirewalls"></a>Micro soft. Network/azurefirewalls

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|ApplicationRuleHit|Ja|Aantal treffers toepassings regels|Aantal|Totaal|Aantal keer dat toepassings regels zijn geraakt|Status, reden, protocol|
|DataProcessed|Ja|Verwerkte gegevens|Bytes|Totaal|Totale hoeveelheid gegevens die door deze firewall worden verwerkt|Geen dimensies|
|FirewallHealth|Ja|Status van Firewall|Percentage|Average|Hiermee wordt de algemene status van deze firewall aangegeven|Status, reden|
|NetworkRuleHit|Ja|Aantal treffers in netwerk regels|Aantal|Totaal|Aantal keren dat netwerk regels zijn geraakt|Status, reden, protocol|
|SNATPortUtilization|Ja|Gebruik van SNAT-poort|Percentage|Average|Percentage van de uitgaande SNAT-poorten die momenteel in gebruik zijn|Protocol|
|Doorvoer|Nee|Doorvoer|BitsPerSecond|Average|Door voer verwerkt door deze firewall|Geen dimensies|


## <a name="microsoftnetworkconnections"></a>Micro soft. Network/Connections

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|BitsInPerSecond|Ja|BitsInPerSecond|CountPerSecond|Average|Bits die per seconde in Azure binnenkomen|Geen dimensies|
|BitsOutPerSecond|Ja|BitsOutPerSecond|CountPerSecond|Average|Bits die per seconde in Azure verlaten|Geen dimensies|


## <a name="microsoftnetworkdnszones"></a>Micro soft. Network/dnszones

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|QueryVolume|Ja|Query volume|Aantal|Totaal|Aantal verzonden query's voor een DNS-zone|Geen dimensies|
|RecordSetCapacityUtilization|Nee|Capaciteits gebruik van record sets|Percentage|Maximum|Percentage van de set-capaciteit van de record die wordt gebruikt door een DNS-zone|Geen dimensies|
|RecordSetCount|Ja|Aantal record sets|Aantal|Maximum|Aantal record sets in een DNS-zone|Geen dimensies|


## <a name="microsoftnetworkexpressroutecircuits"></a>Microsoft.Network/expressRouteCircuits

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|ArpAvailability|Ja|ARP-Beschik baarheid|Percentage|Average|ARP-Beschik baarheid van MSEE naar alle peers.|PeeringType, peer|
|BgpAvailability|Ja|BGP-Beschik baarheid|Percentage|Average|BGP-Beschik baarheid van MSEE naar alle peers.|PeeringType, peer|
|BitsInPerSecond|Nee|BitsInPerSecond|CountPerSecond|Average|Bits die per seconde in Azure binnenkomen|PeeringType|
|BitsOutPerSecond|Nee|BitsOutPerSecond|CountPerSecond|Average|Bits die per seconde in Azure verlaten|PeeringType|
|GlobalReachBitsInPerSecond|Nee|GlobalReachBitsInPerSecond|CountPerSecond|Average|Bits die per seconde in Azure binnenkomen|PeeredCircuitSKey|
|GlobalReachBitsOutPerSecond|Nee|GlobalReachBitsOutPerSecond|CountPerSecond|Average|Bits die per seconde in Azure verlaten|PeeredCircuitSKey|
|QosDropBitsInPerSecond|Nee|DroppedInBitsPerSecond|CountPerSecond|Average|Ingangs gegevens die per seconde worden verwijderd|Geen dimensies|
|QosDropBitsOutPerSecond|Nee|DroppedOutBitsPerSecond|CountPerSecond|Average|Uitgaande bits van gegevens die per seconde worden verwijderd|Geen dimensies|


## <a name="microsoftnetworkexpressroutecircuitspeerings"></a>Micro soft. Network/expressRouteCircuits/peerings

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|BitsInPerSecond|Ja|BitsInPerSecond|CountPerSecond|Average|Bits die per seconde in Azure binnenkomen|Geen dimensies|
|BitsOutPerSecond|Ja|BitsOutPerSecond|CountPerSecond|Average|Bits die per seconde in Azure verlaten|Geen dimensies|


## <a name="microsoftnetworkexpressroutegateways"></a>Micro soft. Network/expressRouteGateways

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|ErGatewayConnectionBitsInPerSecond|Nee|BitsInPerSecond|CountPerSecond|Average|Bits die per seconde in Azure binnenkomen|ConnectionName|
|ErGatewayConnectionBitsOutPerSecond|Nee|BitsOutPerSecond|CountPerSecond|Average|Bits die per seconde in Azure verlaten|ConnectionName|


## <a name="microsoftnetworkexpressrouteports"></a>Micro soft. Network/expressRoutePorts

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|AdminState|Ja|AdminState|Aantal|Average|Beheer status van de poort|Koppeling|
|LineProtocol|Ja|LineProtocol|Aantal|Average|Status van het regel Protocol van de poort|Koppeling|
|PortBitsInPerSecond|Ja|BitsInPerSecond|CountPerSecond|Average|Bits die per seconde in Azure binnenkomen|Koppeling|
|PortBitsOutPerSecond|Ja|BitsOutPerSecond|CountPerSecond|Average|Bits die per seconde in Azure verlaten|Koppeling|
|RxLightLevel|Ja|RxLightLevel|Aantal|Average|RX licht niveau in dBm|Koppeling, Lane|
|TxLightLevel|Ja|TxLightLevel|Aantal|Average|TX licht niveau in dBm|Koppeling, Lane|


## <a name="microsoftnetworkfrontdoors"></a>Micro soft. Network/frontdoors

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|BackendHealthPercentage|Ja|Back-status percentage|Percentage|Average|Het percentage geslaagde status controles van de HTTP/S-proxy naar back-end|Back-end, hosts|
|BackendRequestCount|Ja|Aantal back-aanvragen|Aantal|Totaal|Het aantal aanvragen dat is verzonden vanaf de HTTP/S-proxy naar back-end|Http status, HttpStatusGroup, back-end|
|BackendRequestLatency|Ja|Latentie van back-upaanvraag|Milliseconden|Average|De tijd die wordt berekend vanaf het moment dat de aanvraag door de HTTP/S-proxy naar de back-end werd verzonden, totdat de HTTP/S-proxy de laatste antwoord byte van de back-end heeft ontvangen|Back-end|
|BillableResponseSize|Ja|Grootte van factureer bare antwoorden|Bytes|Totaal|Het aantal factureer bare bytes (minimale 2KB per aanvraag) dat wordt verzonden als antwoorden van HTTP/S-proxy naar clients.|Http status, HttpStatusGroup, ClientRegion, ClientCountry|
|RequestCount|Ja|Aantal aanvragen|Aantal|Totaal|Het aantal client aanvragen dat wordt geleverd door de HTTP/S-proxy|Http status, HttpStatusGroup, ClientRegion, ClientCountry|
|RequestSize|Ja|Aanvraag grootte|Bytes|Totaal|Het aantal bytes dat is verzonden als aanvragen van clients naar de HTTP/S-proxy|Http status, HttpStatusGroup, ClientRegion, ClientCountry|
|ResponseSize|Ja|Grootte van antwoord|Bytes|Totaal|Het aantal bytes dat is verzonden als reacties van HTTP/S-proxy naar clients|Http status, HttpStatusGroup, ClientRegion, ClientCountry|
|TotalLatency|Ja|Totale latentie|Milliseconden|Average|De tijd die wordt berekend vanaf het moment dat de client aanvraag door de HTTP/S-proxy werd ontvangen totdat de client de laatste antwoord byte van de HTTP/S-proxy heeft bevestigd|Http status, HttpStatusGroup, ClientRegion, ClientCountry|
|WebApplicationFirewallRequestCount|Ja|Aantal aanvragen voor Web Application firewall|Aantal|Totaal|Het aantal client aanvragen dat is verwerkt door de Web Application firewall|Beleidsnaam, regelnaam, actie|


## <a name="microsoftnetworkloadbalancers"></a>Microsoft.Network/loadBalancers

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|AllocatedSnatPorts|Nee|Toegewezen SNAT-poorten|Aantal|Average|Totaal aantal toegewezen SNAT-poorten binnen tijds periode|FrontendIPAddress, BackendIPAddress, protocol type, |
|ByteCount|Ja|Aantal bytes|Aantal|Totaal|Totaal aantal verzonden bytes binnen tijds periode|FrontendIPAddress, FrontendPort, direction|
|DipAvailability|Ja|Status van de statustest|Aantal|Average|Gemiddelde status van Load Balancer Health probe per tijds duur|Protocol type, BackendPort, FrontendIPAddress, FrontendPort, BackendIPAddress|
|PacketCount|Ja|Aantal pakketten|Aantal|Totaal|Totaal aantal verzonden pakketten binnen tijds periode|FrontendIPAddress, FrontendPort, direction|
|SnatConnectionCount|Ja|Aantal SNAT-verbindingen|Aantal|Totaal|Totaal aantal nieuwe SNAT-verbindingen dat binnen een tijds periode is gemaakt|FrontendIPAddress, BackendIPAddress, ConnectionState|
|SYNCount|Ja|SYN-aantal|Aantal|Totaal|Totaal aantal SYN-pakketten verzonden binnen tijds periode|FrontendIPAddress, FrontendPort, direction|
|UsedSnatPorts|Nee|Gebruikte SNAT-poorten|Aantal|Average|Totaal aantal SNAT-poorten gebruikt binnen tijds periode|FrontendIPAddress, BackendIPAddress, protocol type, |
|VipAvailability|Ja|Beschikbaarheid van gegevenspad|Aantal|Average|Gemiddelde Beschik baarheid van gegevens paden per tijds duur van Load Balancer|FrontendIPAddress, FrontendPort|


## <a name="microsoftnetworknetworkinterfaces"></a>Microsoft.Network/networkInterfaces

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|BytesReceivedRate|Ja|Ontvangen bytes|Bytes|Totaal|Het aantal bytes dat de netwerk interface heeft ontvangen|Geen dimensies|
|BytesSentRate|Ja|Verzonden bytes|Bytes|Totaal|Het aantal bytes dat de netwerk interface heeft verzonden|Geen dimensies|
|PacketsReceivedRate|Ja|Ontvangen pakketten|Aantal|Totaal|Aantal pakketten dat de netwerk interface heeft ontvangen|Geen dimensies|
|PacketsSentRate|Ja|Verzonden pakketten|Aantal|Totaal|Aantal pakketten dat de netwerk interface heeft verzonden|Geen dimensies|


## <a name="microsoftnetworknetworkwatchersconnectionmonitors"></a>Micro soft. Network/networkWatchers/connectionMonitors

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|AverageRoundtripMs|Ja|Gem. retour tijd (MS)|Milliseconden|Average|Gemiddelde Round-retour tijd van het netwerk (MS) voor connectiviteits controle tests die zijn verzonden tussen de bron en het doel|Geen dimensies|
|ChecksFailedPercent|Ja|Percentage mislukte controles (preview-versie)|Percentage|Average|% van het controleren van de connectiviteits controle is mislukt|SourceAddress, SourceName, SourceResourceId, Source type, protocol, DestinationAddress, doelhost, DestinationResourceId, DestinationType, DestinationPort, TestGroupName, TestConfigurationName|
|ProbesFailedPercent|Ja|% Tests mislukt|Percentage|Average|% van de controles van connectiviteits controle is mislukt|Geen dimensies|
|RoundTripTimeMs|Ja|Round-Trip tijd (MS) (preview)|Milliseconden|Average|Retour tijd in milliseconden voor het controleren van de connectiviteits controle|SourceAddress, SourceName, SourceResourceId, Source type, protocol, DestinationAddress, doelhost, DestinationResourceId, DestinationType, DestinationPort, TestGroupName, TestConfigurationName|


## <a name="microsoftnetworkpublicipaddresses"></a>Microsoft.Network/publicIPAddresses

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|ByteCount|Ja|Aantal bytes|Aantal|Totaal|Totaal aantal verzonden bytes binnen tijds periode|Poort, richting|
|BytesDroppedDDoS|Ja|Binnenkomende bytes verloren DDoS|BytesPerSecond|Maximum|Binnenkomende bytes verloren DDoS|Geen dimensies|
|BytesForwardedDDoS|Ja|Doorgestuurde binnenkomende bytes DDoS|BytesPerSecond|Maximum|Doorgestuurde binnenkomende bytes DDoS|Geen dimensies|
|BytesInDDoS|Ja|Binnenkomende bytes DDoS|BytesPerSecond|Maximum|Binnenkomende bytes DDoS|Geen dimensies|
|DDoSTriggerSYNPackets|Ja|Inkomende SYN-pakketten om DDoS-beperking te activeren|CountPerSecond|Maximum|Inkomende SYN-pakketten om DDoS-beperking te activeren|Geen dimensies|
|DDoSTriggerTCPPackets|Ja|Binnenkomende TCP-pakketten om DDoS-beperking te activeren|CountPerSecond|Maximum|Binnenkomende TCP-pakketten om DDoS-beperking te activeren|Geen dimensies|
|DDoSTriggerUDPPackets|Ja|Binnenkomende UDP-pakketten om DDoS-beperking te activeren|CountPerSecond|Maximum|Binnenkomende UDP-pakketten om DDoS-beperking te activeren|Geen dimensies|
|IfUnderDDoSAttack|Ja|Onder DDoS-aanval of niet|Aantal|Maximum|Onder DDoS-aanval of niet|Geen dimensies|
|PacketCount|Ja|Aantal pakketten|Aantal|Totaal|Totaal aantal verzonden pakketten binnen tijds periode|Poort, richting|
|PacketsDroppedDDoS|Ja|DDoS inkomende pakketten verwijderd|CountPerSecond|Maximum|DDoS inkomende pakketten verwijderd|Geen dimensies|
|PacketsForwardedDDoS|Ja|DDoS inkomende pakketten doorgestuurd|CountPerSecond|Maximum|DDoS inkomende pakketten doorgestuurd|Geen dimensies|
|PacketsInDDoS|Ja|DDoS inkomende pakketten|CountPerSecond|Maximum|DDoS inkomende pakketten|Geen dimensies|
|SynCount|Ja|SYN-aantal|Aantal|Totaal|Totaal aantal SYN-pakketten verzonden binnen tijds periode|Poort, richting|
|TCPBytesDroppedDDoS|Ja|DDoS binnenkomende TCP-bytes|BytesPerSecond|Maximum|DDoS binnenkomende TCP-bytes|Geen dimensies|
|TCPBytesForwardedDDoS|Ja|DDoS doorgestuurde binnenkomende TCP-bytes|BytesPerSecond|Maximum|DDoS doorgestuurde binnenkomende TCP-bytes|Geen dimensies|
|TCPBytesInDDoS|Ja|DDoS binnenkomende TCP-bytes|BytesPerSecond|Maximum|DDoS binnenkomende TCP-bytes|Geen dimensies|
|TCPPacketsDroppedDDoS|Ja|DDoS binnenkomende TCP-pakketten|CountPerSecond|Maximum|DDoS binnenkomende TCP-pakketten|Geen dimensies|
|TCPPacketsForwardedDDoS|Ja|Doorgestuurde binnenkomende TCP-pakketten DDoS|CountPerSecond|Maximum|Doorgestuurde binnenkomende TCP-pakketten DDoS|Geen dimensies|
|TCPPacketsInDDoS|Ja|Binnenkomende TCP-pakketten DDoS|CountPerSecond|Maximum|Binnenkomende TCP-pakketten DDoS|Geen dimensies|
|UDPBytesDroppedDDoS|Ja|Binnenkomend UDP-bytes verloren DDoS|BytesPerSecond|Maximum|Binnenkomend UDP-bytes verloren DDoS|Geen dimensies|
|UDPBytesForwardedDDoS|Ja|Doorgestuurde binnenkomende UDP-bytes DDoS|BytesPerSecond|Maximum|Doorgestuurde binnenkomende UDP-bytes DDoS|Geen dimensies|
|UDPBytesInDDoS|Ja|Binnenkomende UDP-bytes DDoS|BytesPerSecond|Maximum|Binnenkomende UDP-bytes DDoS|Geen dimensies|
|UDPPacketsDroppedDDoS|Ja|Verwijderde binnenkomende UDP-pakketten DDoS|CountPerSecond|Maximum|Verwijderde binnenkomende UDP-pakketten DDoS|Geen dimensies|
|UDPPacketsForwardedDDoS|Ja|Door inkomende UDP-pakketten DDoS doorgestuurd|CountPerSecond|Maximum|Door inkomende UDP-pakketten DDoS doorgestuurd|Geen dimensies|
|UDPPacketsInDDoS|Ja|Binnenkomende UDP-pakketten DDoS|CountPerSecond|Maximum|Binnenkomende UDP-pakketten DDoS|Geen dimensies|
|VipAvailability|Ja|Beschikbaarheid van gegevenspad|Aantal|Average|Gemiddelde Beschik baarheid van IP-adressen per tijds duur|Poort|


## <a name="microsoftnetworktrafficmanagerprofiles"></a>Microsoft.Network/trafficManagerProfiles

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|ProbeAgentCurrentEndpointStateByProfileResourceId|Ja|Eindpunt status op eind punt|Aantal|Maximum|1 als de test status van een eind punt is ingeschakeld, 0 anders.|EndpointName|
|QpsByEndpoint|Ja|Query's op eind punt geretourneerd|Aantal|Totaal|Aantal keren dat een Traffic Manager eind punt is geretourneerd in het opgegeven tijds bestek|EndpointName|


## <a name="microsoftnetworkvirtualnetworkgateways"></a>Microsoft.Network/virtualNetworkGateways

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|AverageBandwidth|Ja|Gateway-S2S-band breedte|BytesPerSecond|Average|Gemiddeld aantal site-naar-site-band breedte van een gateway in bytes per seconde|Geen dimensies|
|P2SBandwidth|Ja|Gateway P2S-band breedte|BytesPerSecond|Average|Gemiddelde Point-to-site band breedte van een gateway in bytes per seconde|Geen dimensies|
|P2SConnectionCount|Ja|Aantal P2S-verbindingen|Aantal|Maximum|Aantal point-to-site-verbindingen van een gateway|Protocol|
|TunnelAverageBandwidth|Ja|Tunnelbandbreedte|BytesPerSecond|Average|Gemiddelde bandbreedte van een tunnel in bytes per seconde|ConnectionName, RemoteIP|
|TunnelEgressBytes|Ja|Uitgaande bytes in tunnel|Bytes|Totaal|Uitgaande bytes van een tunnel|ConnectionName, RemoteIP|
|TunnelEgressPacketDropTSMismatch|Ja|Uitgaande niet-overeenkomende TS-pakketten door tunnel|Aantal|Totaal|Aantal uitgaande niet-overeenkomende pakketten uit traffic selector in tunnel|ConnectionName, RemoteIP|
|TunnelEgressPackets|Ja|Uitgaande pakketten in tunnel|Aantal|Totaal|Aantal uitgaande pakketten van een tunnel|ConnectionName, RemoteIP|
|TunnelIngressBytes|Ja|Totaal aantal inkomende bytes|Bytes|Totaal|Inkomende bytes in een tunnel|ConnectionName, RemoteIP|
|TunnelIngressPacketDropTSMismatch|Ja|Inkomende niet-overeenkomende TS-pakketten door tunnel|Aantal|Totaal|Aantal inkomende niet-overeenkomende pakketten uit traffic selector in tunnel|ConnectionName, RemoteIP|
|TunnelIngressPackets|Ja|Tunnel ingangs pakketten|Aantal|Totaal|Aantal inkomende pakketten van een tunnel|ConnectionName, RemoteIP|


## <a name="microsoftnetworkvirtualnetworks"></a>Microsoft.Network/virtualNetworks

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|PingMeshAverageRoundtripMs|Ja|Retour tijd voor pings naar een virtuele machine|Milliseconden|Average|Retour tijd voor pings die naar een bestemmings-VM worden verzonden|SourceCustomerAddress, DestinationCustomerAddress|
|PingMeshProbesFailedPercent|Ja|Pingen naar een virtuele machine is mislukt|Percentage|Average|Percentage van het aantal mislukte pings naar totale aantal verzonden Pings van een doel-VM|SourceCustomerAddress, DestinationCustomerAddress|


## <a name="microsoftnotificationhubsnamespacesnotificationhubs"></a>Micro soft. notification hubs/naam ruimten/notification hubs

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|e-mail|Ja|Binnenkomende berichten|Aantal|Totaal|Het aantal geslaagde verzend-API-aanroepen. |Geen dimensies|
|inkomend. alle. failedrequests|Ja|Alle binnenkomende mislukte aanvragen|Aantal|Totaal|Totaal aantal binnenkomende mislukte aanvragen voor een notification hub|Geen dimensies|
|binnenkomende. alle. aanvragen|Ja|Alle inkomende aanvragen|Aantal|Totaal|Totaal aantal binnenkomende aanvragen voor een notification hub|Geen dimensies|
|inkomend. gepland|Ja|Geplande push meldingen verzonden|Aantal|Totaal|Geplande push meldingen geannuleerd|Geen dimensies|
|Binnenkomend. gepland. annuleren|Ja|Geplande push meldingen geannuleerd|Aantal|Totaal|Geplande push meldingen geannuleerd|Geen dimensies|
|installatie. alle|Ja|Bewerkingen voor installatie beheer|Aantal|Totaal|Bewerkingen voor installatie beheer|Geen dimensies|
|installatie. Delete|Ja|Installatie bewerkingen verwijderen|Aantal|Totaal|Installatie bewerkingen verwijderen|Geen dimensies|
|installatie. Get|Ja|Installatie bewerkingen ophalen|Aantal|Totaal|Installatie bewerkingen ophalen|Geen dimensies|
|installatie. patch|Ja|Patch-installatie bewerkingen|Aantal|Totaal|Patch-installatie bewerkingen|Geen dimensies|
|installatie. upsert|Ja|Installatie bewerkingen maken of bijwerken|Aantal|Totaal|Installatie bewerkingen maken of bijwerken|Geen dimensies|
|notificationhub. pushes|Ja|Alle uitgaande meldingen|Aantal|Totaal|Alle uitgaande meldingen van de notification hub|Geen dimensies|
|uitgaande. allpns. badorexpiredchannel|Ja|Ongeldige of verlopen kanaal fouten|Aantal|Totaal|Het aantal pushes dat is mislukt omdat het kanaal/token-registratie in de registratie is verlopen of ongeldig is.|Geen dimensies|
|uitgaande. allpns. channelerror|Ja|Kanaal fouten|Aantal|Totaal|Het aantal pushes dat is mislukt omdat het kanaal ongeldig is en niet is gekoppeld aan de juiste app beperkt of verlopen.|Geen dimensies|
|uitgaande. allpns. invalidpayload|Ja|Payload-fouten|Aantal|Totaal|Het aantal pushes dat is mislukt omdat de PNS een ongeldige Payload-fout heeft geretourneerd.|Geen dimensies|
|uitgaande. allpns. pnserror|Ja|Externe meldingen systeem fouten|Aantal|Totaal|Het aantal pushes dat is mislukt omdat er een probleem is opgetreden bij het communiceren met de PNS (er zijn verificatie problemen uitgesloten).|Geen dimensies|
|uitgaand. allpns. geslaagd|Ja|Geslaagde meldingen|Aantal|Totaal|Het aantal geslaagde meldingen.|Geen dimensies|
|uitgaand. apns. badchannel|Ja|Fout met ongeldige APNS-kanaal|Aantal|Totaal|Het aantal pushes dat is mislukt omdat het token ongeldig is (APNS-status code: 8).|Geen dimensies|
|uitgaand. apns. expiredchannel|Ja|Fout bij verlopen van APNS-kanaal|Aantal|Totaal|Het aantal tokens dat ongeldig is gemaakt door het APNS-feedback kanaal.|Geen dimensies|
|uitgaand. apns. invalidcredentials|Ja|APNS-autorisatie fouten|Aantal|Totaal|Het aantal pushes dat is mislukt omdat de PNS de opgegeven referenties niet heeft geaccepteerd of de referenties zijn geblokkeerd.|Geen dimensies|
|uitgaand. apns. invalidnotificationsize|Ja|Fout door ongeldige grootte van APNS-melding|Aantal|Totaal|Het aantal pushes dat is mislukt omdat de payload te groot is (APNS-status code: 7).|Geen dimensies|
|uitgaand. apns. pnserror|Ja|APNS-fouten|Aantal|Totaal|Het aantal pushes dat is mislukt vanwege fouten in de communicatie met APNS.|Geen dimensies|
|uitgaand. apns. geslaagd|Ja|Geslaagde meldingen van APNS|Aantal|Totaal|Het aantal geslaagde meldingen.|Geen dimensies|
|uitgaande. GCM. authenticationerror|Ja|GCM-verificatie fouten|Aantal|Totaal|Het aantal pushes dat is mislukt omdat de PNS de opgegeven referenties niet heeft geaccepteerd, de referenties zijn geblokkeerd of de SenderId niet juist is geconfigureerd in de app (GCM resultaat: MismatchedSenderId).|Geen dimensies|
|uitgaande. GCM. badchannel|Ja|GCM ongeldige kanaal fout|Aantal|Totaal|Het aantal pushes dat is mislukt omdat de registratie in de registratie niet is herkend (GCM-resultaat: ongeldige registratie).|Geen dimensies|
|uitgaande. GCM. expiredchannel|Ja|GCM-fout met verlopen kanaal|Aantal|Totaal|Het aantal pushes dat is mislukt omdat de registratie in de registratie is verlopen (GCM resultaat: NotRegistered).|Geen dimensies|
|uitgaande. GCM. invalidcredentials|Ja|GCM-verificatie fouten (ongeldige referenties)|Aantal|Totaal|Het aantal pushes dat is mislukt omdat de PNS de opgegeven referenties niet heeft geaccepteerd of de referenties zijn geblokkeerd.|Geen dimensies|
|uitgaande. GCM. invalidnotificationformat|Ja|Ongeldige indeling van GCM-melding|Aantal|Totaal|Het aantal pushes dat is mislukt omdat de payload niet juist is geformatteerd (GCM-resultaat: InvalidDataKey of InvalidTtl).|Geen dimensies|
|uitgaande. GCM. invalidnotificationsize|Ja|Fout met ongeldige grootte van GCM-melding|Aantal|Totaal|Het aantal pushes dat is mislukt omdat de payload te groot is (GCM-resultaat: MessageTooBig).|Geen dimensies|
|uitgaande. GCM. pnserror|Ja|GCM-fouten|Aantal|Totaal|Het aantal pushes dat is mislukt vanwege fouten in de communicatie met GCM.|Geen dimensies|
|uitgaand. GCM. geslaagd|Ja|Geslaagde meldingen GCM|Aantal|Totaal|Het aantal geslaagde meldingen.|Geen dimensies|
|uitgaande. GCM. throttled|Ja|GCM beperkte meldingen|Aantal|Totaal|Het aantal pushes dat is mislukt omdat GCM deze app heeft beperkt (GCM-status code: 501-599 of resultaat: niet beschikbaar).|Geen dimensies|
|uitgaande. GCM. wrongchannel|Ja|GCM onjuiste kanaal fout|Aantal|Totaal|Het aantal pushes dat is mislukt omdat de registratie in de registratie niet is gekoppeld aan de huidige app (GCM-resultaat: InvalidPackageName).|Geen dimensies|
|uitgaande. mpns. authenticationerror|Ja|MPNS-verificatie fouten|Aantal|Totaal|Het aantal pushes dat is mislukt omdat de PNS de opgegeven referenties niet heeft geaccepteerd of de referenties zijn geblokkeerd.|Geen dimensies|
|uitgaande. mpns. badchannel|Ja|MPNS ongeldige kanaal fout|Aantal|Totaal|Het aantal pushes dat is mislukt omdat de kanaal in de registratie niet is herkend (MPNS-status: 404 niet gevonden).|Geen dimensies|
|uitgaande. mpns. channeldisconnected|Ja|Verbinding met MPNS-kanaal verbroken|Aantal|Totaal|Het aantal pushes dat is mislukt omdat de kanaal van de registratie is verbroken (MPNS-status: 412 niet gevonden).|Geen dimensies|
|uitgaande. mpns. dropd|Ja|MPNS-verwijderde meldingen|Aantal|Totaal|Het aantal pushes dat is verwijderd door MPNS (MPNS-reactie header: X-notification status: QueueFull of onderdrukt).|Geen dimensies|
|uitgaande. mpns. invalidcredentials|Ja|Ongeldige referenties MPNS|Aantal|Totaal|Het aantal pushes dat is mislukt omdat de PNS de opgegeven referenties niet heeft geaccepteerd of de referenties zijn geblokkeerd.|Geen dimensies|
|uitgaande. mpns. invalidnotificationformat|Ja|Ongeldige indeling van MPNS-melding|Aantal|Totaal|Het aantal pushes dat is mislukt omdat de payload van de melding te groot is.|Geen dimensies|
|uitgaande. mpns. pnserror|Ja|MPNS-fouten|Aantal|Totaal|Het aantal pushes dat is mislukt vanwege fouten in de communicatie met MPNS.|Geen dimensies|
|uitgaand. mpns. geslaagd|Ja|Geslaagde meldingen MPNS|Aantal|Totaal|Het aantal geslaagde meldingen.|Geen dimensies|
|uitgaande. mpns. throttled|Ja|MPNS beperkte meldingen|Aantal|Totaal|Het aantal pushes dat is mislukt omdat MPNS deze app beperkt (WNS MPNS: 406 niet toegestaan).|Geen dimensies|
|uitgaande. wns. authenticationerror|Ja|WNS-verificatie fouten|Aantal|Totaal|Er is een melding niet bezorgd omdat er fouten zijn opgetreden tijdens de communicatie met Windows Live ongeldige referenties of een onjuist token.|Geen dimensies|
|uitgaande. wns. badchannel|Ja|WNS ongeldige kanaal fout|Aantal|Totaal|Het aantal pushes dat is mislukt omdat de kanaal in de registratie niet is herkend (WNS-status: 404 niet gevonden).|Geen dimensies|
|uitgaande. wns. channeldisconnected|Ja|Verbinding met WNS-kanaal verbroken|Aantal|Totaal|De melding is verwijderd omdat de kanaal in de registratie is beperkt (WNS-reactie header: X-WNS-DeviceConnectionStatus: disconnected).|Geen dimensies|
|uitgaande. wns. channelthrottled|Ja|WNS-kanaal beperkt|Aantal|Totaal|De melding is verwijderd omdat de kanaal in de registratie is beperkt (WNS-reactie header: X-WNS-notification status: channelThrottled).|Geen dimensies|
|uitgaande. wns. dropd|Ja|WNS-verwijderde meldingen|Aantal|Totaal|De melding is verwijderd omdat de kanaal in de registratie is beperkt (X-WNS-notification status: verwijderd maar niet X-WNS-DeviceConnectionStatus: disconnected).|Geen dimensies|
|uitgaande. wns. expiredchannel|Ja|WNS-fout met verlopen kanaal|Aantal|Totaal|Het aantal pushes dat is mislukt omdat de kanaal is verlopen (WNS-status: 410 verdwenen).|Geen dimensies|
|uitgaande. wns. invalidcredentials|Ja|WNS-verificatie fouten (ongeldige referenties)|Aantal|Totaal|Het aantal pushes dat is mislukt omdat de PNS de opgegeven referenties niet heeft geaccepteerd of de referenties zijn geblokkeerd. (Windows Live herkent de referenties niet).|Geen dimensies|
|uitgaande. wns. invalidnotificationformat|Ja|Ongeldige indeling van WNS-melding|Aantal|Totaal|De indeling van de melding is ongeldig (WNS-status: 400). Houd er rekening mee dat WNS niet alle ongeldige payloads weigert.|Geen dimensies|
|uitgaande. wns. invalidnotificationsize|Ja|Fout met ongeldige grootte van WNS-melding|Aantal|Totaal|De nettolading van de melding is te groot (WNS-status: 413).|Geen dimensies|
|uitgaande. wns. invalidtoken|Ja|WNS-verificatie fouten (ongeldig token)|Aantal|Totaal|Het token dat is gegeven aan WNS is niet geldig (WNS-status: 401 niet toegestaan).|Geen dimensies|
|uitgaande. wns. pnserror|Ja|WNS-fouten|Aantal|Totaal|Er is geen melding bezorgd vanwege fouten in de communicatie met WNS.|Geen dimensies|
|uitgaand. wns. geslaagd|Ja|Geslaagde meldingen WNS|Aantal|Totaal|Het aantal geslaagde meldingen.|Geen dimensies|
|uitgaande. wns. throttled|Ja|WNS beperkte meldingen|Aantal|Totaal|Het aantal pushes dat is mislukt omdat WNS deze app beperkt (WNS-status: 406 niet toegestaan).|Geen dimensies|
|uitgaande. wns. tokenproviderunreachable|Ja|WNS-verificatie fouten (onbereikbaar)|Aantal|Totaal|Windows Live is niet bereikbaar.|Geen dimensies|
|uitgaande. wns. wrongtoken|Ja|WNS-autorisatie fouten (onjuist token)|Aantal|Totaal|Het token dat is gegeven aan WNS is geldig, maar voor een andere toepassing (WNS-status: 403 verboden). Dit kan gebeuren als de kanaal in de registratie is gekoppeld aan een andere app. Controleer of de client-app is gekoppeld aan dezelfde app waarvan de referenties zich in de notification hub bevinden.|Geen dimensies|
|registratie. alle|Ja|Registratie bewerkingen|Aantal|Totaal|Het aantal voltooide registratie bewerkingen (gemaakte bijwerk query's en verwijderingen). |Geen dimensies|
|registratie. Create|Ja|Bewerkingen voor het maken van registratie|Aantal|Totaal|Het aantal gemaakte registraties.|Geen dimensies|
|registratie. Delete|Ja|Verwijderings bewerkingen voor registratie|Aantal|Totaal|Het aantal voltooide registraties dat is verwijderd.|Geen dimensies|
|registratie. ophalen|Ja|Lees bewerkingen voor registratie|Aantal|Totaal|Het aantal geslaagde registratie query's.|Geen dimensies|
|registratie. update|Ja|Registratie-update bewerkingen|Aantal|Totaal|Het aantal voltooide registratie-updates.|Geen dimensies|
|gepland. in behandeling|Ja|Geplande meldingen in behandeling|Aantal|Totaal|Geplande meldingen in behandeling|Geen dimensies|


## <a name="microsoftoperationalinsightsworkspaces"></a>Microsoft.OperationalInsights/workspaces

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|Percentage beschik bare geheugen Average_|Ja|Percentage beschikbaar geheugen|Aantal|Average|Percentage beschik bare geheugen Average_|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_ percentage beschik bare wissel ruimte|Ja|Percentage beschik bare wissel ruimte|Aantal|Average|Average_ percentage beschik bare wissel ruimte|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_% toegewezen bytes in gebruik|Ja|% Toegewezen bytes in gebruik|Aantal|Average|Average_% toegewezen bytes in gebruik|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_ percentage DPC-tijd|Ja|Percentage DPC-tijd|Aantal|Average|Average_ percentage DPC-tijd|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Percentage vrije inodes Average_|Ja|% Vrije inodes|Aantal|Average|Percentage vrije inodes Average_|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|% Beschik bare ruimte Average_|Ja|Percentage beschik bare ruimte|Aantal|Average|% Beschik bare ruimte Average_|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|% Niet-actieve tijd Average_|Ja|Percentage niet-actieve tijd|Aantal|Average|% Niet-actieve tijd Average_|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Percentage interrupt-tijd van Average_|Ja|Percentage interrupt-tijd|Aantal|Average|Percentage interrupt-tijd van Average_|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_% i/o-wacht tijd|Ja|% I/o-wacht tijd|Aantal|Average|Average_% i/o-wacht tijd|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Tijd van Average_% leuk|Ja|Percentage tijd in Nice|Aantal|Average|Tijd van Average_% leuk|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Percentage tijd in beschermde modus Average_|Ja|Percentage tijd in beschermde modus|Aantal|Average|Percentage tijd in beschermde modus Average_|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Percentage processor tijd van Average_|Ja|Percentage processortijd|Aantal|Average|Percentage processor tijd van Average_|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_% gebruikte inodes|Ja|% Gebruikte inodes|Aantal|Average|Average_% gebruikte inodes|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_ percentage gebruikt geheugen|Ja|Percentage gebruikt geheugen|Aantal|Average|Average_ percentage gebruikt geheugen|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Percentage gebruikte ruimte Average_|Ja|Percentage gebruikte ruimte|Aantal|Average|Percentage gebruikte ruimte Average_|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_ percentage gebruikte wissel ruimte|Ja|Percentage gebruikte wissel ruimte|Aantal|Average|Average_ percentage gebruikte wissel ruimte|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Percentage gebruikers tijd van Average_|Ja|Percentage gebruikers tijd|Aantal|Average|Percentage gebruikers tijd van Average_|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Available MB|Ja|Beschikbare megabytes|Aantal|Average|Average_Available MB|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Available MB geheugen|Ja|Beschikbaar geheugen in mega bytes|Aantal|Average|Average_Available MB geheugen|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Available MB wisselen|Ja|Beschik bare mega bytes wisselen|Aantal|Average|Average_Available MB wisselen|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Avg. Gelezen bytes per seconde|Ja|Gemiddelde Lees tijd schijf|Aantal|Average|Average_Avg. Gelezen bytes per seconde|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Avg. Seconde/overdracht schijf|Ja|Gemiddelde tijd schijf overdracht|Aantal|Average|Average_Avg. Seconde/overdracht schijf|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Avg. voor de fysieke schijf|Ja|Gemiddelde schrijf tijd schijf|Aantal|Average|Average_Avg. voor de fysieke schijf|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Ontvangen Average_Bytes per seconde|Ja|Ontvangen bytes per seconde|Aantal|Average|Ontvangen Average_Bytes per seconde|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Verzonden Average_Bytes per seconde|Ja|Verzonden bytes per seconde|Aantal|Average|Verzonden Average_Bytes per seconde|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Bytes totaal per seconde|Ja|Totaal aantal bytes per seconde|Aantal|Average|Average_Bytes totaal per seconde|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Wachtrij lengte van Average_Current schijf|Ja|Huidige wachtrij lengte voor de schijf|Aantal|Average|Wachtrij lengte van Average_Current schijf|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Disk Lees bewerkingen in bytes per seconde|Ja|Gelezen bytes per seconde|Aantal|Average|Average_Disk Lees bewerkingen in bytes per seconde|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Disk Lees bewerkingen per seconde|Ja|Lees bewerkingen per seconde|Aantal|Average|Average_Disk Lees bewerkingen per seconde|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Disk overdrachten per seconde|Ja|Schijf overdrachten per seconde|Aantal|Average|Average_Disk overdrachten per seconde|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Disk geschreven bytes per seconde|Ja|Geschreven bytes per seconde|Aantal|Average|Average_Disk geschreven bytes per seconde|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Disk schrijf bewerkingen per seconde|Ja|Schrijf bewerkingen per seconde|Aantal|Average|Average_Disk schrijf bewerkingen per seconde|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Free MB|Ja|Beschik bare mega bytes|Aantal|Average|Average_Free MB|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Free fysiek geheugen|Ja|Vrij fysiek geheugen|Aantal|Average|Average_Free fysiek geheugen|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Free ruimte in wissel geheugen bestanden|Ja|Vrije ruimte in wissel geheugen bestanden|Aantal|Average|Average_Free ruimte in wissel geheugen bestanden|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Virtueel geheugen Average_Free|Ja|Vrij virtueel geheugen|Aantal|Average|Virtueel geheugen Average_Free|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Logical schijf bytes per seconde|Ja|Bytes van logische schijf per seconde|Aantal|Average|Average_Logical schijf bytes per seconde|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Page Lees bewerkingen per seconde|Ja|Gelezen pagina's per seconde|Aantal|Average|Average_Page Lees bewerkingen per seconde|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Page schrijf bewerkingen per seconde|Ja|Geschreven pagina's per seconde|Aantal|Average|Average_Page schrijf bewerkingen per seconde|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Pages/sec.|Ja|Pagina's per seconde|Aantal|Average|Average_Pages/sec.|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Beschermde tijd van Average_Pct|Ja|Pct-geprivilegieerde tijd|Aantal|Average|Beschermde tijd van Average_Pct|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Gebruikers tijd van Average_Pct|Ja|Pct-gebruikers tijd|Aantal|Average|Gebruikers tijd van Average_Pct|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Physical schijf bytes per seconde|Ja|Bytes van fysieke schijf per seconde|Aantal|Average|Average_Physical schijf bytes per seconde|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Processes|Ja|Processen|Aantal|Average|Average_Processes|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Lengte van Average_Processor wachtrij|Ja|Lengte van de processor wachtrij|Aantal|Average|Lengte van Average_Processor wachtrij|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Size opgeslagen in Wissel bestanden|Ja|Grootte opgeslagen in Wissel bestanden|Aantal|Average|Average_Size opgeslagen in Wissel bestanden|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Total bytes|Ja|Totaal aantal bytes|Aantal|Average|Average_Total bytes|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Ontvangen Average_Total bytes|Ja|Totaal aantal ontvangen bytes|Aantal|Average|Ontvangen Average_Total bytes|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Verzonden Average_Total bytes|Ja|Totaal aantal verzonden bytes|Aantal|Average|Verzonden Average_Total bytes|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Total conflicten|Ja|Totaal aantal conflicten|Aantal|Average|Average_Total conflicten|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Ontvangen Average_Total pakketten|Ja|Totaal aantal ontvangen pakketten|Aantal|Average|Ontvangen Average_Total pakketten|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Verzonden Average_Total pakketten|Ja|Totaal aantal verzonden pakketten|Aantal|Average|Verzonden Average_Total pakketten|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Total RX-fouten|Ja|Totaal aantal RX-fouten|Aantal|Average|Average_Total RX-fouten|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Total TX-fouten|Ja|Totaal aantal TX-fouten|Aantal|Average|Average_Total TX-fouten|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Uptime|Ja|Uptime|Aantal|Average|Average_Uptime|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Used MB wissel ruimte|Ja|Gebruikte MB wissel ruimte|Aantal|Average|Average_Used MB wissel ruimte|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Used geheugen KB|Ja|Gebruikte geheugen-kBytes|Aantal|Average|Average_Used geheugen KB|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Used geheugen MB|Ja|Gebruikt geheugen Mbytes|Aantal|Average|Average_Used geheugen MB|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Users|Ja|Gebruikers|Aantal|Average|Average_Users|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Average_Virtual gedeeld geheugen|Ja|Virtueel gedeeld geheugen|Aantal|Average|Average_Virtual gedeeld geheugen|Computer, ObjectName, INSTANCENAME, CounterPath, hebben|
|Gebeurtenis|Ja|Gebeurtenis|Aantal|Average|Gebeurtenis|Source, EventLog, computer, EventCategory, EventLevel, EventLevelName, Event gebeurtenis|
|Hartslag|Ja|Hartslag|Aantal|Totaal|Hartslag|Computer, OSType, versie, SourceComputerId|
|Bijwerken|Ja|Bijwerken|Aantal|Average|Bijwerken|Computer, product, classificatie, update State, optioneel, goedgekeurd|


## <a name="microsoftpeeringpeerings"></a>Micro soft. peering/peering

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|EgressTrafficRate|Ja|Frequentie van uitgangs verkeer|BitsPerSecond|Average|Frequentie van uitgaand verkeer in bits per seconde|ConnectionId|
|IngressTrafficRate|Ja|Frequentie van ingangs verkeer|BitsPerSecond|Average|Ingangs verkeers frequentie in bits per seconde|ConnectionId|
|SessionAvailabilityV4|Ja|Sessie beschikbaarheid v4|Percentage|Average|Beschik baarheid van de v4-sessie|ConnectionId|
|SessionAvailabilityV6|Ja|Sessie beschikbaarheid V6|Percentage|Average|Beschik baarheid van de V6-sessie|ConnectionId|


## <a name="microsoftpeeringpeeringservices"></a>Micro soft. peering/peeringServices

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|PrefixLatency|Ja|Voorvoegsel latentie|Milliseconden|Average|Latentie van mediaan voorvoegsel|Voorvoegselnaam|


## <a name="microsoftpowerbidedicatedcapacities"></a>Micro soft. PowerBIDedicated/capaciteiten

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|memory_metric|Ja|Geheugen|Bytes|Average|Geheugen. Bereik 0-3 GB voor a1, 0-5 GB voor a2, 0-10 GB voor a3, 0-25 GB voor A4, 0-50 GB voor A5 en 0-100 GB voor A6|Geen dimensies|
|memory_thrashing_metric|Ja|Geheugen overbelasting (gegevens sets)|Percentage|Average|Gemiddeld geheugen overbelasting.|Geen dimensies|
|qpu_high_utilization_metric|Ja|Hoog QPU-gebruik|Aantal|Totaal|QPU hoog gebruik in de laatste minuut, 1 voor hoog QPU gebruik, anders 0|Geen dimensies|
|QueryDuration|Ja|Query duur (gegevens sets)|Milliseconden|Average|DAX-query duur in laatste interval|Geen dimensies|
|QueryPoolJobQueueLength|Ja|Wachtrij lengte van de taak pool voor query's (gegevens sets)|Aantal|Average|Aantal taken in de wachtrij van de query thread pool.|Geen dimensies|


## <a name="microsoftrelaynamespaces"></a>Micro soft. relay/naam ruimten

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|ActiveConnections|Nee|ActiveConnections|Aantal|Totaal|Totaal aantal ActiveConnection voor micro soft. relay.|EntityName|
|ActiveListeners|Nee|ActiveListeners|Aantal|Totaal|Totale ActiveListeners voor micro soft. relay.|EntityName|
|BytesTransferred|Ja|BytesTransferred|Aantal|Totaal|Totale BytesTransferred voor micro soft. relay.|EntityName|
|ListenerConnections-ClientError|Nee|ListenerConnections-ClientError|Aantal|Totaal|Client error op ListenerConnections voor micro soft. relay.|EntityName |
|ListenerConnections-ServerError|Nee|ListenerConnections-ServerError|Aantal|Totaal|Server error op ListenerConnections voor micro soft. relay.|EntityName |
|ListenerConnections-Success|Nee|ListenerConnections-Success|Aantal|Totaal|Geslaagde ListenerConnections voor micro soft. relay.|EntityName |
|ListenerConnections-TotalRequests|Nee|ListenerConnections-TotalRequests|Aantal|Totaal|Totale ListenerConnections voor micro soft. relay.|EntityName|
|ListenerDisconnects|Nee|ListenerDisconnects|Aantal|Totaal|Totale ListenerDisconnects voor micro soft. relay.|EntityName|
|SenderConnections-ClientError|Nee|SenderConnections-ClientError|Aantal|Totaal|Client error op SenderConnections voor micro soft. relay.|EntityName |
|SenderConnections-ServerError|Nee|SenderConnections-ServerError|Aantal|Totaal|Server error op SenderConnections voor micro soft. relay.|EntityName |
|SenderConnections-Success|Nee|SenderConnections-Success|Aantal|Totaal|Geslaagde SenderConnections voor micro soft. relay.|EntityName |
|SenderConnections-TotalRequests|Nee|SenderConnections-TotalRequests|Aantal|Totaal|Totaal aantal SenderConnections-aanvragen voor micro soft. relay.|EntityName|
|SenderDisconnects|Nee|SenderDisconnects|Aantal|Totaal|Totale SenderDisconnects voor micro soft. relay.|EntityName|


## <a name="microsoftsearchsearchservices"></a>Micro soft. Search/searchServices

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|SearchLatency|Ja|Zoek latentie|Seconden|Average|Gemiddelde Zoek latentie voor de zoek service|Geen dimensies|
|SearchQueriesPerSecond|Ja|Zoek query's per seconde|CountPerSecond|Average|Zoek query's per seconde voor de zoek service|Geen dimensies|
|ThrottledSearchQueriesPercentage|Ja|Percentage vertraagde Zoek query's|Percentage|Average|Percentage Zoek query's dat is beperkt tot de zoek service|Geen dimensies|


## <a name="microsoftservicebusnamespaces"></a>Microsoft.ServiceBus/namespaces

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|ActiveConnections|Nee|ActiveConnections|Aantal|Totaal|Totaal aantal actieve verbindingen voor micro soft. ServiceBus.|Geen dimensies|
|ActiveMessages|Nee|Aantal actieve berichten in een wachtrij/onderwerp.|Aantal|Average|Aantal actieve berichten in een wachtrij/onderwerp.|EntityName|
|ConnectionsClosed|Nee|Gesloten verbindingen.|Aantal|Average|Gesloten verbindingen voor micro soft. ServiceBus.|EntityName|
|ConnectionsOpened|Nee|Geopende verbindingen.|Aantal|Average|Geopende verbindingen voor micro soft. ServiceBus.|EntityName|
|CPUXNS|Nee|CPU (afgeschaft)|Percentage|Maximum|Metrische gegevens voor CPU-gebruik van service bus Premium-naam ruimte. Deze metrische waarde is verouderd. Gebruik in plaats daarvan de CPU-metriek (NamespaceCpuUsage).|Geen dimensies|
|DeadletteredMessages|Nee|Aantal onbestelbare berichten in een wachtrij/onderwerp.|Aantal|Average|Aantal onbestelbare berichten in een wachtrij/onderwerp.|EntityName|
|IncomingMessages|Ja|Binnenkomende berichten|Aantal|Totaal|Binnenkomende berichten voor micro soft. ServiceBus.|EntityName|
|IncomingRequests|Ja|Binnenkomende aanvragen|Aantal|Totaal|Binnenkomende aanvragen voor micro soft. ServiceBus.|EntityName|
|Berichten|Nee|Aantal berichten in een wachtrij/onderwerp.|Aantal|Average|Aantal berichten in een wachtrij/onderwerp.|EntityName|
|NamespaceCpuUsage|Nee|CPU|Percentage|Maximum|Metrische gegevens voor CPU-gebruik van service bus Premium-naam ruimte.|Geen dimensies|
|NamespaceMemoryUsage|Nee|Geheugengebruik|Percentage|Maximum|Metrische geheugen gebruik van service bus Premium-naam ruimte.|Geen dimensies|
|OutgoingMessages|Ja|Uitgaande berichten|Aantal|Totaal|Uitgaande berichten voor micro soft. ServiceBus.|EntityName|
|ScheduledMessages|Nee|Aantal geplande berichten in een wachtrij/onderwerp.|Aantal|Average|Aantal geplande berichten in een wachtrij/onderwerp.|EntityName|
|ServerErrors|Nee|Serverfouten.|Aantal|Totaal|Server fouten voor micro soft. ServiceBus.|EntityName |
|Grootte|Nee|Grootte|Bytes|Average|Grootte van een wachtrij/onderwerp in bytes.|EntityName|
|SuccessfulRequests|Nee|Geslaagde aanvragen|Aantal|Totaal|Totaal aantal geslaagde aanvragen voor een naam ruimte|EntityName |
|ThrottledRequests|Nee|Beperkte aanvragen.|Aantal|Totaal|Beperkte aanvragen voor micro soft. ServiceBus.|EntityName |
|UserErrors|Nee|Gebruikersfouten.|Aantal|Totaal|Gebruikers fouten voor micro soft. ServiceBus.|EntityName |
|WSXNS|Nee|Geheugen gebruik (afgeschaft)|Percentage|Maximum|Metrische geheugen gebruik van service bus Premium-naam ruimte. Deze metriek is afgeschaft. Gebruik in plaats daarvan de metriek geheugen gebruik (NamespaceMemoryUsage).|Geen dimensies|


## <a name="microsoftservicefabricmeshapplications"></a>Micro soft. ServiceFabricMesh/toepassingen

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|ActualCpu|Nee|ActualCpu|Aantal|Average|Werkelijk CPU-gebruik in milli-kernen|ApplicationName, ServiceName, CodePackageName, ServiceReplicaName|
|ActualMemory|Nee|ActualMemory|Bytes|Average|Werkelijk geheugen gebruik in MB|ApplicationName, ServiceName, CodePackageName, ServiceReplicaName|
|AllocatedCpu|Nee|AllocatedCpu|Aantal|Average|CPU toegewezen aan deze container in milli-kernen|ApplicationName, ServiceName, CodePackageName, ServiceReplicaName|
|AllocatedMemory|Nee|AllocatedMemory|Bytes|Average|Geheugen toegewezen aan deze container in MB|ApplicationName, ServiceName, CodePackageName, ServiceReplicaName|
|ApplicationStatus|Nee|ApplicationStatus|Aantal|Average|Status van Service Fabric mesh-toepassing|ApplicationName, status|
|Container status|Nee|Container status|Aantal|Average|Status van de container in Service Fabric mesh-toepassing|ApplicationName, ServiceName, CodePackageName, ServiceReplicaName, status|
|CpuUtilization|Nee|CpuUtilization|Percentage|Average|Gebruik van CPU voor deze container als percentage van AllocatedCpu|ApplicationName, ServiceName, CodePackageName, ServiceReplicaName|
|MemoryUtilization|Nee|MemoryUtilization|Percentage|Average|Gebruik van CPU voor deze container als percentage van AllocatedCpu|ApplicationName, ServiceName, CodePackageName, ServiceReplicaName|
|RestartCount|Nee|RestartCount|Aantal|Average|Telling van een container in Service Fabric mesh-toepassing opnieuw starten|ApplicationName, status, ServiceName, ServiceReplicaName, CodePackageName|
|ServiceReplicaStatus|Nee|ServiceReplicaStatus|Aantal|Average|Integriteits status van een service replica in Service Fabric mesh-toepassing|ApplicationName, status, ServiceName, ServiceReplicaName|
|ServiceStatus|Nee|ServiceStatus|Aantal|Average|Integriteits status van een service in Service Fabric mesh-toepassing|ApplicationName, status, servicenaam|


## <a name="microsoftsignalrservicesignalr"></a>Micro soft. SignalRService/Signa lering

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|ConnectionCount|Ja|Aantal verbindingen|Aantal|Maximum|De hoeveelheid gebruikers verbinding.|Eindpunt|
|InboundTraffic|Ja|Binnenkomend verkeer|Bytes|Totaal|Het inkomende verkeer van de service|Geen dimensies|
|MessageCount|Ja|Aantal berichten|Aantal|Totaal|De totale hoeveelheid berichten.|Geen dimensies|
|OutboundTraffic|Ja|Uitgaand verkeer|Bytes|Totaal|Het uitgaande verkeer van de service|Geen dimensies|
|SystemErrors|Ja|Systeem fouten|Percentage|Maximum|Het percentage systeem fouten|Geen dimensies|
|UserErrors|Ja|Gebruikers fouten|Percentage|Maximum|Het percentage gebruikers fouten|Geen dimensies|


## <a name="microsoftsqlmanagedinstances"></a>Micro soft. SQL/managedInstances

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|avg_cpu_percent|Ja|Gemiddeld CPU-percentage|Percentage|Average|Gemiddeld CPU-percentage|Geen dimensies|
|io_bytes_read|Ja|Gelezen IO-bytes|Bytes|Average|Gelezen IO-bytes|Geen dimensies|
|io_bytes_written|Ja|Geschreven IO-bytes|Bytes|Average|Geschreven IO-bytes|Geen dimensies|
|io_requests|Ja|Aantal i/o-aanvragen|Aantal|Average|Aantal i/o-aanvragen|Geen dimensies|
|reserved_storage_mb|Ja|Gereserveerde opslag ruimte|Aantal|Average|Gereserveerde opslag ruimte|Geen dimensies|
|storage_space_used_mb|Ja|Gebruikte opslag ruimte|Aantal|Average|Gebruikte opslag ruimte|Geen dimensies|
|virtual_core_count|Ja|Aantal virtuele kernen|Aantal|Average|Aantal virtuele kernen|Geen dimensies|


## <a name="microsoftsqlserversdatabases"></a>Microsoft.Sql/servers/databases

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|active_queries|Ja|Actieve query's|Aantal|Totaal|Actieve query's over alle werkbelasting groepen. Is alleen van toepassing op data warehouses.|Geen dimensies|
|allocated_data_storage|Ja|Toegewezen gegevens ruimte|Bytes|Average|Toegewezen gegevens opslag. Niet van toepassing op data warehouses.|Geen dimensies|
|app_cpu_billed|Ja|App CPU gefactureerd|Aantal|Totaal|App CPU gefactureerd. Is van toepassing op serverloze data bases.|Geen dimensies|
|app_cpu_percent|Ja|CPU-percentage van app|Percentage|Average|CPU-percentage van de app. Is van toepassing op serverloze data bases.|Geen dimensies|
|app_memory_percent|Ja|Percentage app-geheugen|Percentage|Average|Percentage app-geheugen. Is van toepassing op serverloze data bases.|Geen dimensies|
|base_blob_size_bytes|Ja|Basis grootte van Blob-opslag|Bytes|Maximum|Basis grootte van Blob-opslag. Is van toepassing op grootschalige-data bases.|Geen dimensies|
|blocked_by_firewall|Ja|Geblokkeerd door de firewall|Aantal|Totaal|Geblokkeerd door de firewall|Geen dimensies|
|cache_hit_percent|Ja|Percentage cache treffers|Percentage|Maximum|Percentage van cache treffer. Is alleen van toepassing op data warehouses.|Geen dimensies|
|cache_used_percent|Ja|Percentage gebruikt cache|Percentage|Maximum|Percentage gebruikt cache. Is alleen van toepassing op data warehouses.|Geen dimensies|
|connection_failed|Ja|Mislukte verbindingen|Aantal|Totaal|Mislukte verbindingen|Geen dimensies|
|connection_successful|Ja|Geslaagde verbindingen|Aantal|Totaal|Geslaagde verbindingen|Geen dimensies|
|cpu_limit|Ja|CPU-limiet|Aantal|Average|CPU-limiet. Is van toepassing op vCore-data bases.|Geen dimensies|
|cpu_percent|Ja|CPU-percentage|Percentage|Average|CPU-percentage|Geen dimensies|
|cpu_used|Ja|CPU gebruikt|Aantal|Average|CPU gebruikt. Is van toepassing op vCore-data bases.|Geen dimensies|
|constateer|Ja|Impassen|Aantal|Totaal|Impassen. Niet van toepassing op data warehouses.|Geen dimensies|
|diff_backup_size_bytes|Ja|Grootte van differentiële back-upopslag|Bytes|Maximum|Cumulatieve differentiële back-upopslag. Is van toepassing op vCore-data bases. Niet van toepassing op grootschalige-data bases.|Geen dimensies|
|dtu_consumption_percent|Ja|DTU-percentage|Percentage|Average|DTU-percentage. Is van toepassing op DTU-gebaseerde data bases.|Geen dimensies|
|dtu_limit|Ja|DTU-limiet|Aantal|Average|DTU-limiet. Is van toepassing op DTU-gebaseerde data bases.|Geen dimensies|
|dtu_used|Ja|DTU gebruikt|Aantal|Average|DTU gebruikt. Is van toepassing op DTU-gebaseerde data bases.|Geen dimensies|
|dw_backup_size_gb|Ja|Grootte van gegevens opslag (GB)|Aantal|Totaal|De grootte van de gegevens opslag bestaat uit de grootte van uw gegevens en het transactie logboek. De metrische gegevens worden geteld bij het gedeelte opslag van uw factuur. Is alleen van toepassing op data warehouses.|Geen dimensies|
|dw_geosnapshot_size_gb|Ja|Opslag grootte voor nood herstel (GB)|Aantal|Totaal|Opslag voor herstel na nood gevallen wordt weer gegeven als ' nood herstel opslag ' in uw factuur. Is alleen van toepassing op data warehouses.|Geen dimensies|
|dw_snapshot_size_gb|Ja|Grootte van de opslag voor moment opnamen (GB)|Aantal|Totaal|Opslag grootte van de moment opname is de grootte van de incrementele wijzigingen die door moment opnamen worden vastgelegd om door de gebruiker gedefinieerde en automatische herstel punten te maken. De metrische gegevens worden geteld bij het gedeelte opslag van uw factuur. Is alleen van toepassing op data warehouses.|Geen dimensies|
|dwu_consumption_percent|Ja|Percentage DWU|Percentage|Maximum|DWU-percentage. Is alleen van toepassing op data warehouses.|Geen dimensies|
|dwu_limit|Ja|Limiet voor DWU|Aantal|Maximum|DWU-limiet. Is alleen van toepassing op data warehouses.|Geen dimensies|
|dwu_used|Ja|DWU gebruikt|Aantal|Maximum|DWU gebruikt. Is alleen van toepassing op data warehouses.|Geen dimensies|
|full_backup_size_bytes|Ja|Opslag grootte voor volledige back-up|Bytes|Maximum|Cumulatieve opslag grootte voor volledige back-ups. Is van toepassing op vCore-data bases. Niet van toepassing op grootschalige-data bases.|Geen dimensies|
|local_tempdb_usage_percent|Ja|Lokaal TempDB-percentage|Percentage|Average|Lokaal TempDB-percentage. Is alleen van toepassing op data warehouses.|Geen dimensies|
|log_backup_size_bytes|Ja|Opslag grootte van logboek back-up|Bytes|Maximum|Cumulatieve opslag grootte van logboek back-up. Is van toepassing op vCore-en grootschalige-data bases.|Geen dimensies|
|log_write_percent|Ja|Logboek-IO-percentage|Percentage|Average|Logboek-IO-percentage. Niet van toepassing op data warehouses.|Geen dimensies|
|memory_usage_percent|Ja|Geheugen percentage|Percentage|Maximum|Geheugen percentage. Is alleen van toepassing op data warehouses.|Geen dimensies|
|physical_data_read_percent|Ja|Gegevens-I/O-percentage|Percentage|Average|Gegevens-I/O-percentage|Geen dimensies|
|queued_queries|Ja|Query's in de wachtrij|Aantal|Totaal|Query's in de wachtrij voor alle werkbelasting groepen. Is alleen van toepassing op data warehouses.|Geen dimensies|
|sessions_percent|Ja|Percentage sessies|Percentage|Average|Percentage sessies. Niet van toepassing op data warehouses.|Geen dimensies|
|snapshot_backup_size_bytes|Ja|Grootte van back-upopslag voor moment opname|Bytes|Maximum|Grootte van cumulatieve back-upopslag voor moment opname. Is van toepassing op grootschalige-data bases.|Geen dimensies|
|sqlserver_process_core_percent|Ja|Kern percentage van SQL Server proces|Percentage|Maximum|CPU-gebruik als percentage van het SQL DB-proces. Niet van toepassing op data warehouses.|Geen dimensies|
|sqlserver_process_memory_percent|Ja|Percentage proces geheugen SQL Server|Percentage|Maximum|Geheugen gebruik als percentage van het SQL DB-proces. Niet van toepassing op data warehouses.|Geen dimensies|
|opslag|Ja|Gebruikte gegevens ruimte|Bytes|Maximum|Gebruikte gegevens ruimte. Niet van toepassing op data warehouses.|Geen dimensies|
|storage_percent|Ja|Percentage gebruikte gegevens ruimte|Percentage|Maximum|Percentage gebruikte gegevens ruimte. Niet van toepassing op data warehouses of grootschalige-data bases.|Geen dimensies|
|tempdb_data_size|Ja|Data File grootte van tempdb|Aantal|Maximum|Data File grootte van tempdb-gegevens bestanden. Niet van toepassing op data warehouses.|Geen dimensies|
|tempdb_log_size|Ja|Grootte van logboek bestanden tempdb|Aantal|Maximum|Grootte van KB-logboek bestanden. Niet van toepassing op data warehouses.|Geen dimensies|
|tempdb_log_used_percent|Ja|Percentage gebruikt TempDB-logboek|Percentage|Maximum|Percentage gebruikt TempDB-logboek. Niet van toepassing op data warehouses.|Geen dimensies|
|wlg_active_queries|Ja|Actieve query's voor werkbelasting groep|Aantal|Totaal|Actieve query's binnen de werkbelasting groep. Is alleen van toepassing op data warehouses.|WorkloadGroupName, IsUserDefined|
|wlg_active_queries_timeouts|Ja|Time-outs query werkbelasting groep|Aantal|Totaal|Query's waarvoor een time-out is opgetreden voor de werkbelasting groep. Is alleen van toepassing op data warehouses.|WorkloadGroupName, IsUserDefined|
|wlg_allocation_relative_to_system_percent|Ja|Toewijzing van werkbelasting groep per systeem percentage|Percentage|Maximum|Toegewezen percentage resources ten opzichte van het hele systeem per werkbelasting groep. Is alleen van toepassing op data warehouses.|WorkloadGroupName, IsUserDefined|
|wlg_allocation_relative_to_wlg_effective_cap_percent|Ja|Toewijzing van werkbelasting groep per cap-resource percentage|Percentage|Maximum|Toegewezen percentage resources ten opzichte van de opgegeven Cap-resources per werkbelasting groep. Is alleen van toepassing op data warehouses.|WorkloadGroupName, IsUserDefined|
|wlg_effective_cap_resource_percent|Ja|Effectief cap-resource percentage|Percentage|Maximum|Een vaste limiet voor het percentage resources dat is toegestaan voor de werkbelasting groep, waarbij rekening wordt gehouden met een effectief min resource percentage dat is toegewezen aan andere werkbelasting groepen. Is alleen van toepassing op data warehouses.|WorkloadGroupName, IsUserDefined|
|wlg_effective_min_resource_percent|Ja|Effectief min resource percentage|Percentage|Maximum|Mini maal percentage resources dat voor de werkbelasting groep is gereserveerd en geïsoleerd, rekening houdend met het minimum service niveau. Is alleen van toepassing op data warehouses.|WorkloadGroupName, IsUserDefined|
|wlg_queued_queries|Ja|Werkbelasting groep in wachtrij geplaatste query's|Aantal|Totaal|Query's in de wachtrij in de werkbelasting groep. Is alleen van toepassing op data warehouses.|WorkloadGroupName, IsUserDefined|
|workers_percent|Ja|Percentage werk nemers|Percentage|Average|Werknemers percentage. Niet van toepassing op data warehouses.|Geen dimensies|
|xtp_storage_percent|Ja|Percentage In-Memory OLTP-opslag|Percentage|Average|In-Memory OLTP-opslag percentage. Niet van toepassing op data warehouses.|Geen dimensies|


## <a name="microsoftsqlserverselasticpools"></a>Micro soft. SQL/servers/elasticPools

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|allocated_data_storage|Ja|Toegewezen gegevens ruimte|Bytes|Average|Toegewezen gegevens ruimte|Geen dimensies|
|allocated_data_storage_percent|Ja|Percentage toegewezen gegevens ruimte|Percentage|Maximum|Percentage toegewezen gegevens ruimte|Geen dimensies|
|cpu_limit|Ja|CPU-limiet|Aantal|Average|CPU-limiet. Is van toepassing op op vCore gebaseerde elastische Pools.|Geen dimensies|
|cpu_percent|Ja|CPU-percentage|Percentage|Average|CPU-percentage|Geen dimensies|
|cpu_used|Ja|CPU gebruikt|Aantal|Average|CPU gebruikt. Is van toepassing op op vCore gebaseerde elastische Pools.|Geen dimensies|
|database_allocated_data_storage|Nee|Toegewezen gegevens ruimte|Bytes|Average|Toegewezen gegevens ruimte|DatabaseResourceId|
|database_cpu_limit|Nee|CPU-limiet|Aantal|Average|CPU-limiet|DatabaseResourceId|
|database_cpu_percent|Nee|CPU-percentage|Percentage|Average|CPU-percentage|DatabaseResourceId|
|database_cpu_used|Nee|CPU gebruikt|Aantal|Average|CPU gebruikt|DatabaseResourceId|
|database_dtu_consumption_percent|Nee|DTU-percentage|Percentage|Average|DTU-percentage|DatabaseResourceId|
|database_eDTU_used|Nee|eDTU gebruikt|Aantal|Average|eDTU gebruikt|DatabaseResourceId|
|database_log_write_percent|Nee|Logboek-IO-percentage|Percentage|Average|Logboek-IO-percentage|DatabaseResourceId|
|database_physical_data_read_percent|Nee|Gegevens-I/O-percentage|Percentage|Average|Gegevens-I/O-percentage|DatabaseResourceId|
|database_sessions_percent|Nee|Percentage sessies|Percentage|Average|Percentage sessies|DatabaseResourceId|
|database_storage_used|Nee|Gebruikte gegevens ruimte|Bytes|Average|Gebruikte gegevens ruimte|DatabaseResourceId|
|database_workers_percent|Nee|Percentage werk nemers|Percentage|Average|Percentage werk nemers|DatabaseResourceId|
|dtu_consumption_percent|Ja|DTU-percentage|Percentage|Average|DTU-percentage. Is van toepassing op op DTU gebaseerde elastische Pools.|Geen dimensies|
|eDTU_limit|Ja|eDTU-limiet|Aantal|Average|eDTU-limiet. Is van toepassing op op DTU gebaseerde elastische Pools.|Geen dimensies|
|eDTU_used|Ja|eDTU gebruikt|Aantal|Average|eDTU gebruikt. Is van toepassing op op DTU gebaseerde elastische Pools.|Geen dimensies|
|log_write_percent|Ja|Logboek-IO-percentage|Percentage|Average|Logboek-IO-percentage|Geen dimensies|
|physical_data_read_percent|Ja|Gegevens-I/O-percentage|Percentage|Average|Gegevens-I/O-percentage|Geen dimensies|
|sessions_percent|Ja|Percentage sessies|Percentage|Average|Percentage sessies|Geen dimensies|
|sqlserver_process_core_percent|Ja|Kern percentage van SQL Server proces|Percentage|Maximum|CPU-gebruik als percentage van het SQL DB-proces. Is van toepassing op elastische Pools.|Geen dimensies|
|sqlserver_process_memory_percent|Ja|Percentage proces geheugen SQL Server|Percentage|Maximum|Geheugen gebruik als percentage van het SQL DB-proces. Is van toepassing op elastische Pools.|Geen dimensies|
|storage_limit|Ja|Maximale grootte van gegevens|Bytes|Average|Maximale grootte van gegevens|Geen dimensies|
|storage_percent|Ja|Percentage gebruikte gegevens ruimte|Percentage|Average|Percentage gebruikte gegevens ruimte|Geen dimensies|
|storage_used|Ja|Gebruikte gegevens ruimte|Bytes|Average|Gebruikte gegevens ruimte|Geen dimensies|
|tempdb_data_size|Ja|Data File grootte van tempdb|Aantal|Maximum|Data File grootte van tempdb|Geen dimensies|
|tempdb_log_size|Ja|Grootte van logboek bestanden tempdb|Aantal|Maximum|Grootte van logboek bestanden tempdb|Geen dimensies|
|tempdb_log_used_percent|Ja|Percentage gebruikt TempDB-logboek|Percentage|Maximum|Percentage gebruikt TempDB-logboek|Geen dimensies|
|workers_percent|Ja|Percentage werk nemers|Percentage|Average|Percentage werk nemers|Geen dimensies|
|xtp_storage_percent|Ja|Percentage In-Memory OLTP-opslag|Percentage|Average|Percentage In-Memory OLTP-opslag|Geen dimensies|


## <a name="microsoftstoragestorageaccounts"></a>Microsoft.Storage/storageAccounts

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|Beschikbaarheid|Ja|Beschikbaarheid|Percentage|Average|Het percentage Beschik baarheid voor de opslag service of de opgegeven API-bewerking. De beschikbaarheid wordt berekend door de waarde TotalBillableRequests te delen door het aantal van toepassing zijnde aanvragen, inclusief de aanvragen die onverwachte fouten produceren. Alle onverwachte fouten leiden tot een afgenomen beschikbaarheid voor de opslagservice of de opgegeven API-bewerking.|Geotype, ApiName, authenticatie|
|Uitgaand verkeer|Ja|Uitgaand verkeer|Bytes|Totaal|De hoeveelheid uitgaande gegevens. Dit aantal omvat uitgaand verkeer naar een externe client van Azure Storage en de uitvoer van Azure. Daarom geeft deze hoeveelheid niet de factureerbare uitgaande gegevens weer.|Geotype, ApiName, authenticatie|
|Inkomend verkeer|Ja|Inkomend verkeer|Bytes|Totaal|De hoeveelheid ingangs gegevens, in bytes. Hieronder vallen de inkomende gegevens van een externe client in Azure Storage evenals de inkomende gegevens binnen Azure.|Geotype, ApiName, authenticatie|
|SuccessE2ELatency|Ja|Success E2E Latency|Milliseconden|Average|De gemiddelde end-to-end-latentie van geslaagde aanvragen voor een opslag service of de opgegeven API-bewerking, in milliseconden. Deze waarde bevat de vereiste verwerkingstijd in Azure Storage die nodig is om de aanvraag te lezen, het antwoord te verzenden en bevestiging van het antwoord te ontvangen.|Geotype, ApiName, authenticatie|
|SuccessServerLatency|Ja|Geslaagde serverlatentie|Milliseconden|Average|De gemiddelde tijd die nodig is om een aanvraag door Azure Storage te verwerken. Deze waarde bevat niet de netwerklatentie die is opgegeven in SuccessE2ELatency.|Geotype, ApiName, authenticatie|
|Transacties|Ja|Transacties|Aantal|Totaal|Het aantal aanvragen voor een opslagservice of de opgegeven API-bewerking. Dit is inclusief geslaagde en mislukte aanvragen, evenals aanvragen waarbij fouten zijn opgetreden. Gebruik de dimensie ResponseType voor het aantal verschillende typen reacties.|ResponseType, geotype, ApiName, authenticatie|
|UsedCapacity|Nee|Gebruikte capaciteit|Bytes|Average|De hoeveelheid opslag die wordt gebruikt door het opslag account. Voor standaardopslagaccounts is dit de som van de capaciteit die wordt gebruikt door blob, table, file en queue. Voor Premium Storage-accounts en Blob Storage-accounts is dit hetzelfde als BlobCapacity of FileCapacity.|Geen dimensies|


## <a name="microsoftstoragestorageaccountsblobservices"></a>Micro soft. Storage/Storage accounts/blobServices

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|Beschikbaarheid|Ja|Beschikbaarheid|Percentage|Average|Het percentage Beschik baarheid voor de opslag service of de opgegeven API-bewerking. De beschikbaarheid wordt berekend door de waarde TotalBillableRequests te delen door het aantal van toepassing zijnde aanvragen, inclusief de aanvragen die onverwachte fouten produceren. Alle onverwachte fouten leiden tot een afgenomen beschikbaarheid voor de opslagservice of de opgegeven API-bewerking.|Geotype, ApiName, authenticatie|
|BlobCapacity|Nee|BLOB-capaciteit|Bytes|Average|De hoeveelheid opslag die wordt gebruikt door de Blob service van het opslag account in bytes.|BlobType, Tier|
|BlobCount|Nee|Aantal blobs|Aantal|Average|Het aantal BLOB-objecten dat is opgeslagen in het opslag account.|BlobType, Tier|
|BlobProvisionedSize|Nee|Ingerichte grootte van BLOB|Bytes|Average|De hoeveelheid opslag die is ingericht in de Blob service van het opslag account in bytes.|BlobType, Tier|
|ContainerCount|Ja|Aantal BLOB-containers|Aantal|Average|Het aantal containers in het opslag account.|Geen dimensies|
|Uitgaand verkeer|Ja|Uitgaand verkeer|Bytes|Totaal|De hoeveelheid uitgaande gegevens. Dit aantal omvat uitgaand verkeer naar een externe client van Azure Storage en de uitvoer van Azure. Daarom geeft deze hoeveelheid niet de factureerbare uitgaande gegevens weer.|Geotype, ApiName, authenticatie|
|IndexCapacity|Nee|Index capaciteit|Bytes|Average|De hoeveelheid opslag die wordt gebruikt door Azure Data Lake Storage Gen2 hiërarchische index.|Geen dimensies|
|Inkomend verkeer|Ja|Inkomend verkeer|Bytes|Totaal|De hoeveelheid ingangs gegevens, in bytes. Hieronder vallen de inkomende gegevens van een externe client in Azure Storage evenals de inkomende gegevens binnen Azure.|Geotype, ApiName, authenticatie|
|SuccessE2ELatency|Ja|Success E2E Latency|Milliseconden|Average|De gemiddelde end-to-end-latentie van geslaagde aanvragen voor een opslag service of de opgegeven API-bewerking, in milliseconden. Deze waarde bevat de vereiste verwerkingstijd in Azure Storage die nodig is om de aanvraag te lezen, het antwoord te verzenden en bevestiging van het antwoord te ontvangen.|Geotype, ApiName, authenticatie|
|SuccessServerLatency|Ja|Geslaagde serverlatentie|Milliseconden|Average|De gemiddelde tijd die nodig is om een aanvraag door Azure Storage te verwerken. Deze waarde bevat niet de netwerklatentie die is opgegeven in SuccessE2ELatency.|Geotype, ApiName, authenticatie|
|Transacties|Ja|Transacties|Aantal|Totaal|Het aantal aanvragen voor een opslagservice of de opgegeven API-bewerking. Dit is inclusief geslaagde en mislukte aanvragen, evenals aanvragen waarbij fouten zijn opgetreden. Gebruik de dimensie ResponseType voor het aantal verschillende typen reacties.|ResponseType, geotype, ApiName, authenticatie|


## <a name="microsoftstoragestorageaccountsfileservices"></a>Micro soft. Storage/Storage accounts/fileServices

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|Beschikbaarheid|Ja|Beschikbaarheid|Percentage|Average|Het percentage Beschik baarheid voor de opslag service of de opgegeven API-bewerking. De beschikbaarheid wordt berekend door de waarde TotalBillableRequests te delen door het aantal van toepassing zijnde aanvragen, inclusief de aanvragen die onverwachte fouten produceren. Alle onverwachte fouten leiden tot een afgenomen beschikbaarheid voor de opslagservice of de opgegeven API-bewerking.|Geotype, ApiName, authenticatie, bestands share|
|Uitgaand verkeer|Ja|Uitgaand verkeer|Bytes|Totaal|De hoeveelheid uitgaande gegevens. Dit aantal omvat uitgaand verkeer naar een externe client van Azure Storage en de uitvoer van Azure. Daarom geeft deze hoeveelheid niet de factureerbare uitgaande gegevens weer.|Geotype, ApiName, authenticatie, bestands share|
|FileCapacity|Nee|Bestands capaciteit|Bytes|Average|De hoeveelheid bestands opslag die door het opslag account wordt gebruikt.|Bestandsshare|
|FileCount|Nee|Aantal bestanden|Aantal|Average|Het aantal bestanden in het opslag account.|Bestandsshare|
|FileShareCapacityQuota|Nee|Quotum voor bestands share capaciteit|Bytes|Average|De bovengrens voor de hoeveelheid opslag die kan worden gebruikt door Azure Files service in bytes.|Bestandsshare|
|FileShareCount|Nee|Aantal bestands shares|Aantal|Average|Het aantal bestands shares in het opslag account.|Geen dimensies|
|FileShareProvisionedIOPS|Nee|Door bestands share ingerichte IOPS|Bytes|Average|Het basislijn aantal ingerichte IOPS voor de Premium-bestands share in het opslag account Premium files. Dit aantal wordt berekend op basis van de inrichtings grootte (quotum) van de share capaciteit.|Bestandsshare|
|FileShareSnapshotCount|Nee|Aantal moment opnamen van bestands shares|Aantal|Average|Het aantal moment opnamen dat aanwezig is op de share in de bestanden service van het opslag account.|Bestandsshare|
|FileShareSnapshotSize|Nee|Grootte van moment opname van bestands share|Bytes|Average|De hoeveelheid opslag die wordt gebruikt door de moment opnamen in de bestands service van het opslag account in bytes.|Bestandsshare|
|Inkomend verkeer|Ja|Inkomend verkeer|Bytes|Totaal|De hoeveelheid ingangs gegevens, in bytes. Hieronder vallen de inkomende gegevens van een externe client in Azure Storage evenals de inkomende gegevens binnen Azure.|Geotype, ApiName, authenticatie, bestands share|
|SuccessE2ELatency|Ja|Success E2E Latency|Milliseconden|Average|De gemiddelde end-to-end-latentie van geslaagde aanvragen voor een opslag service of de opgegeven API-bewerking, in milliseconden. Deze waarde bevat de vereiste verwerkingstijd in Azure Storage die nodig is om de aanvraag te lezen, het antwoord te verzenden en bevestiging van het antwoord te ontvangen.|Geotype, ApiName, authenticatie, bestands share|
|SuccessServerLatency|Ja|Geslaagde serverlatentie|Milliseconden|Average|De gemiddelde tijd die nodig is om een aanvraag door Azure Storage te verwerken. Deze waarde bevat niet de netwerklatentie die is opgegeven in SuccessE2ELatency.|Geotype, ApiName, authenticatie, bestands share|
|Transacties|Ja|Transacties|Aantal|Totaal|Het aantal aanvragen voor een opslagservice of de opgegeven API-bewerking. Dit is inclusief geslaagde en mislukte aanvragen, evenals aanvragen waarbij fouten zijn opgetreden. Gebruik de dimensie ResponseType voor het aantal verschillende typen reacties.|ResponseType, geotype, ApiName, Authentication, file share|


## <a name="microsoftstoragestorageaccountsqueueservices"></a>Micro soft. Storage/Storage accounts/queueServices

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|Beschikbaarheid|Ja|Beschikbaarheid|Percentage|Average|Het percentage Beschik baarheid voor de opslag service of de opgegeven API-bewerking. De beschikbaarheid wordt berekend door de waarde TotalBillableRequests te delen door het aantal van toepassing zijnde aanvragen, inclusief de aanvragen die onverwachte fouten produceren. Alle onverwachte fouten leiden tot een afgenomen beschikbaarheid voor de opslagservice of de opgegeven API-bewerking.|Geotype, ApiName, authenticatie|
|Uitgaand verkeer|Ja|Uitgaand verkeer|Bytes|Totaal|De hoeveelheid uitgaande gegevens. Dit aantal omvat uitgaand verkeer naar een externe client van Azure Storage en de uitvoer van Azure. Daarom geeft deze hoeveelheid niet de factureerbare uitgaande gegevens weer.|Geotype, ApiName, authenticatie|
|Inkomend verkeer|Ja|Inkomend verkeer|Bytes|Totaal|De hoeveelheid ingangs gegevens, in bytes. Hieronder vallen de inkomende gegevens van een externe client in Azure Storage evenals de inkomende gegevens binnen Azure.|Geotype, ApiName, authenticatie|
|QueueCapacity|Ja|Wachtrij capaciteit|Bytes|Average|De hoeveelheid wachtrij opslag die door het opslag account wordt gebruikt.|Geen dimensies|
|QueueCount|Ja|Aantal wachtrijen|Aantal|Average|Het aantal wacht rijen in het opslag account.|Geen dimensies|
|QueueMessageCount|Ja|Aantal wachtrij berichten|Aantal|Average|Het aantal niet-verlopen wachtrij berichten in het opslag account.|Geen dimensies|
|SuccessE2ELatency|Ja|Success E2E Latency|Milliseconden|Average|De gemiddelde end-to-end-latentie van geslaagde aanvragen voor een opslag service of de opgegeven API-bewerking, in milliseconden. Deze waarde bevat de vereiste verwerkingstijd in Azure Storage die nodig is om de aanvraag te lezen, het antwoord te verzenden en bevestiging van het antwoord te ontvangen.|Geotype, ApiName, authenticatie|
|SuccessServerLatency|Ja|Geslaagde serverlatentie|Milliseconden|Average|De gemiddelde tijd die nodig is om een aanvraag door Azure Storage te verwerken. Deze waarde bevat niet de netwerklatentie die is opgegeven in SuccessE2ELatency.|Geotype, ApiName, authenticatie|
|Transacties|Ja|Transacties|Aantal|Totaal|Het aantal aanvragen voor een opslagservice of de opgegeven API-bewerking. Dit is inclusief geslaagde en mislukte aanvragen, evenals aanvragen waarbij fouten zijn opgetreden. Gebruik de dimensie ResponseType voor het aantal verschillende typen reacties.|ResponseType, geotype, ApiName, authenticatie|


## <a name="microsoftstoragestorageaccountstableservices"></a>Micro soft. Storage/Storage accounts/tableServices

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|Beschikbaarheid|Ja|Beschikbaarheid|Percentage|Average|Het percentage Beschik baarheid voor de opslag service of de opgegeven API-bewerking. De beschikbaarheid wordt berekend door de waarde TotalBillableRequests te delen door het aantal van toepassing zijnde aanvragen, inclusief de aanvragen die onverwachte fouten produceren. Alle onverwachte fouten leiden tot een afgenomen beschikbaarheid voor de opslagservice of de opgegeven API-bewerking.|Geotype, ApiName, authenticatie|
|Uitgaand verkeer|Ja|Uitgaand verkeer|Bytes|Totaal|De hoeveelheid uitgaande gegevens. Dit aantal omvat uitgaand verkeer naar een externe client van Azure Storage en de uitvoer van Azure. Daarom geeft deze hoeveelheid niet de factureerbare uitgaande gegevens weer.|Geotype, ApiName, authenticatie|
|Inkomend verkeer|Ja|Inkomend verkeer|Bytes|Totaal|De hoeveelheid ingangs gegevens, in bytes. Hieronder vallen de inkomende gegevens van een externe client in Azure Storage evenals de inkomende gegevens binnen Azure.|Geotype, ApiName, authenticatie|
|SuccessE2ELatency|Ja|Success E2E Latency|Milliseconden|Average|De gemiddelde end-to-end-latentie van geslaagde aanvragen voor een opslag service of de opgegeven API-bewerking, in milliseconden. Deze waarde bevat de vereiste verwerkingstijd in Azure Storage die nodig is om de aanvraag te lezen, het antwoord te verzenden en bevestiging van het antwoord te ontvangen.|Geotype, ApiName, authenticatie|
|SuccessServerLatency|Ja|Geslaagde serverlatentie|Milliseconden|Average|De gemiddelde tijd die nodig is om een aanvraag door Azure Storage te verwerken. Deze waarde bevat niet de netwerklatentie die is opgegeven in SuccessE2ELatency.|Geotype, ApiName, authenticatie|
|TableCapacity|Ja|Tabel capaciteit|Bytes|Average|De hoeveelheid tabel opslag die door het opslag account wordt gebruikt.|Geen dimensies|
|TableCount|Ja|Aantal tabellen|Aantal|Average|Het aantal tabellen in het opslag account.|Geen dimensies|
|TableEntityCount|Ja|Aantal tabel entiteiten|Aantal|Average|Het aantal tabel entiteiten in het opslag account.|Geen dimensies|
|Transacties|Ja|Transacties|Aantal|Totaal|Het aantal aanvragen voor een opslagservice of de opgegeven API-bewerking. Dit is inclusief geslaagde en mislukte aanvragen, evenals aanvragen waarbij fouten zijn opgetreden. Gebruik de dimensie ResponseType voor het aantal verschillende typen reacties.|ResponseType, geotype, ApiName, authenticatie|


## <a name="microsoftstoragesyncstoragesyncservices"></a>micro soft. storagesync/storageSyncServices

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|ServerSyncSessionResult|Ja|Resultaat van synchronisatie sessie|Aantal|Average|Metriek die een waarde van 1 registreert telkens wanneer het server eindpunt een synchronisatie sessie met het Cloud eindpunt heeft voltooid|SyncGroupName, ServerEndpointName, SyncDirection|
|StorageSyncBatchTransferredFileBytes|Ja|Gesynchroniseerde bytes|Bytes|Totaal|Totale bestands grootte die is overgedragen voor synchronisatie sessies|SyncGroupName, ServerEndpointName, SyncDirection|
|StorageSyncRecalledNetworkBytesByApplication|Ja|Grootte van intrekken van Cloud lagen op toepassing|Bytes|Totaal|Grootte van gegevens die door de toepassing zijn ingetrokken|SyncGroupName, servername, ApplicationName|
|StorageSyncRecalledTotalNetworkBytes|Ja|Grootte van intrekken Cloud lagen|Bytes|Totaal|Grootte van gegevens die zijn ingetrokken|SyncGroupName, servername|
|StorageSyncRecallIOTotalSizeBytes|Ja|Cloud lagen intrekken|Bytes|Totaal|De totale grootte van de gegevens die door de server zijn ingetrokken|ServerName|
|StorageSyncRecallThroughputBytesPerSecond|Ja|Door Voer van Cloud lagen intrekken|BytesPerSecond|Average|Grootte van gegevens intrekken door Voer|SyncGroupName, servername|
|StorageSyncServerHeartbeat|Ja|Online status van de server|Aantal|Maximum|Metriek die een waarde van 1 registreert telkens wanneer de resigtered-server een heartbeat met het Cloud eindpunt heeft geregistreerd|ServerName|
|StorageSyncSyncSessionAppliedFilesCount|Ja|Gesynchroniseerde bestanden|Aantal|Totaal|Aantal gesynchroniseerde bestanden|SyncGroupName, ServerEndpointName, SyncDirection|
|StorageSyncSyncSessionPerItemErrorsCount|Ja|Bestanden die niet worden gesynchroniseerd|Aantal|Totaal|Aantal bestanden dat niet kan worden gesynchroniseerd|SyncGroupName, ServerEndpointName, SyncDirection|


## <a name="microsoftstoragesyncstoragesyncservicesregisteredservers"></a>micro soft. storagesync/storageSyncServices/registeredServer

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|ServerHeartbeat|Ja|Online status van de server|Aantal|Maximum|Metriek die een waarde van 1 registreert telkens wanneer de resigtered-server een heartbeat met het Cloud eindpunt heeft geregistreerd|ServerResourceId, servername|
|ServerRecallIOTotalSizeBytes|Ja|Cloud lagen intrekken|Bytes|Totaal|De totale grootte van de gegevens die door de server zijn ingetrokken|ServerResourceId, servername|


## <a name="microsoftstoragesyncstoragesyncservicessyncgroups"></a>micro soft. storagesync/storageSyncServices/syncGroups

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|SyncGroupBatchTransferredFileBytes|Ja|Gesynchroniseerde bytes|Bytes|Totaal|Totale bestands grootte die is overgedragen voor synchronisatie sessies|SyncGroupName, ServerEndpointName, SyncDirection|
|SyncGroupSyncSessionAppliedFilesCount|Ja|Gesynchroniseerde bestanden|Aantal|Totaal|Aantal gesynchroniseerde bestanden|SyncGroupName, ServerEndpointName, SyncDirection|
|SyncGroupSyncSessionPerItemErrorsCount|Ja|Bestanden die niet worden gesynchroniseerd|Aantal|Totaal|Aantal bestanden dat niet kan worden gesynchroniseerd|SyncGroupName, ServerEndpointName, SyncDirection|


## <a name="microsoftstoragesyncstoragesyncservicessyncgroupsserverendpoints"></a>micro soft. storagesync/storageSyncServices/syncGroups/serverEndpoints

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|ServerEndpointBatchTransferredFileBytes|Ja|Gesynchroniseerde bytes|Bytes|Totaal|Totale bestands grootte die is overgedragen voor synchronisatie sessies|ServerEndpointName, SyncDirection|
|ServerEndpointSyncSessionAppliedFilesCount|Ja|Gesynchroniseerde bestanden|Aantal|Totaal|Aantal gesynchroniseerde bestanden|ServerEndpointName, SyncDirection|
|ServerEndpointSyncSessionPerItemErrorsCount|Ja|Bestanden die niet worden gesynchroniseerd|Aantal|Totaal|Aantal bestanden dat niet kan worden gesynchroniseerd|ServerEndpointName, SyncDirection|


## <a name="microsoftstreamanalyticsstreamingjobs"></a>Micro soft. StreamAnalytics/streamingjobs

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|AMLCalloutFailedRequests|Ja|Mislukte functie aanvragen|Aantal|Totaal|Mislukte functie aanvragen|Logischenaam, PartitionId|
|AMLCalloutInputEvents|Ja|Functie gebeurtenissen|Aantal|Totaal|Functie gebeurtenissen|Logischenaam, PartitionId|
|AMLCalloutRequests|Ja|Functie aanvragen|Aantal|Totaal|Functie aanvragen|Logischenaam, PartitionId|
|ConversionErrors|Ja|Gegevens conversie fouten|Aantal|Totaal|Gegevens conversie fouten|Logischenaam, PartitionId|
|DeserializationError|Ja|Fouten bij het deserialiseren van de invoer|Aantal|Totaal|Fouten bij het deserialiseren van de invoer|Logischenaam, PartitionId|
|DroppedOrAdjustedEvents|Ja|Gebeurtenissen met een andere volg orde|Aantal|Totaal|Gebeurtenissen met een andere volg orde|Logischenaam, PartitionId|
|EarlyInputEvents|Ja|Vroege invoer gebeurtenissen|Aantal|Totaal|Vroege invoer gebeurtenissen|Logischenaam, PartitionId|
|Fouten|Ja|Runtime-fouten|Aantal|Totaal|Runtime-fouten|Logischenaam, PartitionId|
|InputEventBytes|Ja|Invoer gebeurtenis bytes|Bytes|Totaal|Invoer gebeurtenis bytes|Logischenaam, PartitionId|
|InputEvents|Ja|Invoer gebeurtenissen|Aantal|Totaal|Invoer gebeurtenissen|Logischenaam, PartitionId|
|InputEventsSourcesBacklogged|Ja|Inachterstand, invoer gebeurtenissen|Aantal|Maximum|Inachterstand, invoer gebeurtenissen|Logischenaam, PartitionId|
|InputEventsSourcesPerSecond|Ja|Invoer bronnen ontvangen|Aantal|Totaal|Invoer bronnen ontvangen|Logischenaam, PartitionId|
|LateInputEvents|Ja|Late invoer gebeurtenissen|Aantal|Totaal|Late invoer gebeurtenissen|Logischenaam, PartitionId|
|OutputEvents|Ja|Uitvoer gebeurtenissen|Aantal|Totaal|Uitvoer gebeurtenissen|Logischenaam, PartitionId|
|OutputWatermarkDelaySeconds|Ja|Watermerk vertraging|Seconden|Maximum|Watermerk vertraging|Logischenaam, PartitionId|
|ResourceUtilization|Ja|% Gebruik|Percentage|Maximum|% Gebruik|Logischenaam, PartitionId|


## <a name="microsoftsynapseworkspaces"></a>Micro soft. Synapse/werk ruimten

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|OrchestrationActivityRunsEnded|Nee|Uitvoering van activiteit beëindigd|Aantal|Totaal|Aantal Orchestration-activiteiten dat is geslaagd, mislukt of geannuleerd|Resultaat, FailureType, activiteit, activity type, pijp lijn|
|OrchestrationPipelineRunsEnded|Nee|Beëindigde pijplijn uitvoeringen|Aantal|Totaal|Aantal uitvoeringen van de Orchestrator-pipeline die geslaagd, mislukt of geannuleerd zijn|Resultaat, FailureType, pijp lijn|
|OrchestrationTriggersEnded|Nee|Beëindigde triggers|Aantal|Totaal|Aantal Orchestrator-triggers dat is geslaagd, mislukt of geannuleerd|Resultaat, FailureType, trigger|
|SQLOnDemandLoginAttempts|Nee|Aanmeldings pogingen|Aantal|Totaal|Aantal aanmeldings pogingen dat is voltooid of mislukt|Resultaat|
|SQLOnDemandQueriesEnded|Nee|Beëindigde query's|Aantal|Totaal|Aantal query's dat is geslaagd, mislukt of geannuleerd|Resultaat|
|SQLOnDemandQueryProcessedBytes|Nee|Verwerkte gegevens|Bytes|Totaal|Hoeveelheid gegevens die wordt verwerkt door query's|Geen dimensies|


## <a name="microsoftsynapseworkspacesbigdatapools"></a>Micro soft. Synapse/werk ruimten/bigDataPools

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|CoresCapacity|Nee|Capaciteit van kern geheugens|Aantal|Maximum|Capaciteit van kern geheugens|Geen dimensies|
|MemoryCapacityGB|Nee|Geheugen capaciteit (GB)|Aantal|Maximum|Geheugen capaciteit (GB)|Geen dimensies|
|SparkJobsEnded|Ja|Beëindigde toepassingen|Aantal|Totaal|Aantal beëindigde toepassingen|Taak type, JobResult|


## <a name="microsoftsynapseworkspacessqlpools"></a>Micro soft. Synapse/werk ruimten/sqlPools

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|AdaptiveCacheHitPercent|Nee|Treffer percentage van adaptieve cache|Percentage|Maximum|Meet hoe goed werk belastingen gebruikmaken van de adaptieve cache. Gebruik deze metriek met de metrische gegevens in het cache geheugen om te bepalen of u een extra capaciteit wilt schalen of de werk belasting opnieuw wilt uitvoeren om de cache te hydraten|Geen dimensies|
|AdaptiveCacheUsedPercent|Nee|Percentage van adaptief cache gebruik|Percentage|Maximum|Meet hoe goed werk belastingen gebruikmaken van de adaptieve cache. Gebruik deze metriek met de waarde voor het gebruikte percentage van het cache geheugen om te bepalen of u wilt schalen op extra capaciteit of werk belastingen opnieuw wilt uitvoeren voor de cache|Geen dimensies|
|Verbindingen|Ja|Verbindingen|Aantal|Totaal|Totaal aantal aanmeldingen bij de SQL-groep|Resultaat|
|ConnectionsBlockedByFirewall|Nee|Verbindingen geblokkeerd door Firewall|Aantal|Totaal|Het aantal verbindingen dat wordt geblokkeerd door firewall regels. Ga naar het toegangs beheer beleid voor uw SQL-groep en controleer deze verbindingen als het aantal hoog is|Geen dimensies|
|DWULimit|Nee|Limiet voor DWU|Aantal|Maximum|Serviceniveau doelstelling van de SQL-groep|Geen dimensies|
|DWUUsed|Nee|DWU gebruikt|Aantal|Maximum|Vertegenwoordigt een weer gave op hoog niveau van het gebruik in de SQL-groep. Gemeten met DWU-limiet * DWU percentage|Geen dimensies|
|DWUUsedPercent|Nee|Percentage gebruikt DWU|Percentage|Maximum|Vertegenwoordigt een weer gave op hoog niveau van het gebruik in de SQL-groep. Gemeten door het maximum te nemen tussen het CPU-percentage en het IO-percentage voor gegevens|Geen dimensies|
|LocalTempDBUsedPercent|Nee|Percentage lokaal gebruikte tempdb|Percentage|Maximum|Lokaal TempDB-gebruik voor alle reken knooppunten: de waarden worden elke vijf minuten verzonden|Geen dimensies|
|MemoryUsedPercent|Nee|Percentage gebruikt geheugen|Percentage|Maximum|Geheugen gebruik op alle knoop punten in de SQL-groep|Geen dimensies|
|wlg_effective_min_resource_percent|Ja|Effectief min resource percentage|Percentage|Minimum|De instelling van het werkelijke minimale resource percentage dat is toegestaan voor het beschouwen van het service niveau en de instellingen van de werkbelasting groep. De effectief min_percentage_resource kan hoger worden aangepast op lagere service niveaus|IsUserDefined, WorkloadGroup|
|WLGActiveQueries|Nee|Actieve query's voor werkbelasting groep|Aantal|Totaal|De actieve query's binnen de werkbelasting groep. Als u deze metrische waarde onfilterd en ongesplitst gebruikt, worden alle actieve query's weer gegeven die worden uitgevoerd op het systeem|IsUserDefined, WorkloadGroup|
|WLGActiveQueriesTimeouts|Nee|Time-outs query werkbelasting groep|Aantal|Totaal|Query's voor de werkbelasting groep waarvoor een time-out is opgetreden. Querytime-time-outs die door deze metrische gegevens worden gerapporteerd, worden slechts eenmaal gestart nadat de query is uitgevoerd (de wacht tijd is niet opgenomen als gevolg van vergren deling of wachten op een resource)|IsUserDefined, WorkloadGroup|
|WLGAllocationByMaxResourcePercent|Nee|Toewijzing van werkbelasting groep op Maxi maal resource percentage|Percentage|Maximum|Geeft de percentage toewijzing van resources ten opzichte van het resource percentage van de effectief cap per werkbelasting groep. Deze metrische gegevens bieden het effectief gebruik van de werkbelasting groep|IsUserDefined, WorkloadGroup|
|WLGAllocationBySystemPercent|Nee|Toewijzing van werkbelasting groep per systeem percentage|Percentage|Maximum|Het percentage toewijzing van resources ten opzichte van het hele systeem|IsUserDefined, WorkloadGroup|
|WLGEffectiveCapResourcePercent|Ja|Effectief cap-resource percentage|Percentage|Maximum|Het werkelijke Cap-resource percentage voor de werkbelasting groep. Als er andere werkbelasting groepen zijn met min_percentage_resource > 0, wordt de effective_cap_percentage_resource proportioneel verlaagd|IsUserDefined, WorkloadGroup|
|WLGQueuedQueries|Nee|Werkbelasting groep in wachtrij geplaatste query's|Aantal|Totaal|Cumulatief aantal aanvragen dat in de wachtrij is geplaatst nadat de limiet voor het maximum aantal gelijktijdig is bereikt|IsUserDefined, WorkloadGroup|


## <a name="microsofttimeseriesinsightsenvironments"></a>Micro soft. TimeSeriesInsights/omgevingen

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|IngressReceivedBytes|Ja|Ontvangen bytes van binnenkomend verkeer|Bytes|Totaal|Het aantal gelezen bytes van alle gebeurtenis bronnen|Geen dimensies|
|IngressReceivedInvalidMessages|Ja|Ongeldige berichten ontvangen|Aantal|Totaal|Aantal ongeldige berichten gelezen uit alle gebeurtenis bronnen van Event hub of IoT hub|Geen dimensies|
|IngressReceivedMessages|Ja|Ontvangen berichten met ingang|Aantal|Totaal|Telling van berichten die zijn gelezen uit alle gebeurtenis bronnen van Event hub of IoT hub|Geen dimensies|
|IngressReceivedMessagesCountLag|Ja|Vertraging ontvangen inkomende berichten van ingang|Aantal|Average|Verschil tussen het Volg nummer van de laatste bericht in de bron partitie en het Volg nummer van de berichten die worden verwerkt in ingangs gebeurtenissen|Geen dimensies|
|IngressReceivedMessagesTimeLag|Ja|Tijds vertraging ontvangen inkomende berichten|Seconden|Maximum|Verschil tussen het tijdstip waarop het bericht in de bron van de gebeurtenis in de wachtrij staat en de tijd die wordt verwerkt in ingangs|Geen dimensies|
|IngressStoredBytes|Ja|In ingangs opgeslagen bytes|Bytes|Totaal|De totale grootte van de gebeurtenissen die zijn verwerkt en beschikbaar voor de query|Geen dimensies|
|IngressStoredEvents|Ja|Opgeslagen gebeurtenissen in ingangs|Aantal|Totaal|Aantal samengevoegde gebeurtenissen dat is verwerkt en beschikbaar is voor query|Geen dimensies|
|WarmStorageMaxProperties|Ja|Maximale eigenschappen van warme opslag|Aantal|Maximum|Maximum aantal eigenschappen dat is toegestaan door de omgeving voor de SKU van S1/S2 en het maximum aantal eigenschappen dat is toegestaan door de warme Store voor PAYG SKU|Geen dimensies|
|WarmStorageUsedProperties|Ja|Eigenschappen voor warme opslag gebruikt |Aantal|Maximum|Aantal eigenschappen dat wordt gebruikt door de omgeving voor de SKU van S1/S2 en het aantal eigenschappen dat door warme Store voor PAYG SKU wordt gebruikt|Geen dimensies|


## <a name="microsofttimeseriesinsightsenvironmentseventsources"></a>Micro soft. TimeSeriesInsights/omgevingen/eventsources

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|IngressReceivedBytes|Ja|Ontvangen bytes van binnenkomend verkeer|Bytes|Totaal|Aantal gelezen bytes van de gebeurtenis bron|Geen dimensies|
|IngressReceivedInvalidMessages|Ja|Ongeldige berichten ontvangen|Aantal|Totaal|Aantal ongeldige berichten gelezen uit de gebeurtenis bron|Geen dimensies|
|IngressReceivedMessages|Ja|Ontvangen berichten met ingang|Aantal|Totaal|Aantal berichten dat uit de gebeurtenis bron is gelezen|Geen dimensies|
|IngressReceivedMessagesCountLag|Ja|Vertraging ontvangen inkomende berichten van ingang|Aantal|Average|Verschil tussen het Volg nummer van de laatste bericht in de bron partitie en het Volg nummer van de berichten die worden verwerkt in ingangs gebeurtenissen|Geen dimensies|
|IngressReceivedMessagesTimeLag|Ja|Tijds vertraging ontvangen inkomende berichten|Seconden|Maximum|Verschil tussen het tijdstip waarop het bericht in de bron van de gebeurtenis in de wachtrij staat en de tijd die wordt verwerkt in ingangs|Geen dimensies|
|IngressStoredBytes|Ja|In ingangs opgeslagen bytes|Bytes|Totaal|De totale grootte van de gebeurtenissen die zijn verwerkt en beschikbaar voor de query|Geen dimensies|
|IngressStoredEvents|Ja|Opgeslagen gebeurtenissen in ingangs|Aantal|Totaal|Aantal samengevoegde gebeurtenissen dat is verwerkt en beschikbaar is voor query|Geen dimensies|
|WarmStorageMaxProperties|Ja|Maximale eigenschappen van warme opslag|Aantal|Maximum|Maximum aantal eigenschappen dat is toegestaan door de omgeving voor de SKU van S1/S2 en het maximum aantal eigenschappen dat is toegestaan door de warme Store voor PAYG SKU|Geen dimensies|
|WarmStorageUsedProperties|Ja|Eigenschappen voor warme opslag gebruikt |Aantal|Maximum|Aantal eigenschappen dat wordt gebruikt door de omgeving voor de SKU van S1/S2 en het aantal eigenschappen dat door warme Store voor PAYG SKU wordt gebruikt|Geen dimensies|


## <a name="microsoftvmwarecloudsimplevirtualmachines"></a>Micro soft. VMwareCloudSimple/informatie

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|Gelezen bytes op de schijf|Ja|Gelezen bytes op de schijf|Bytes|Totaal|Totale schijf doorvoer vanwege Lees bewerkingen gedurende de voorbeeld periode.|Geen dimensies|
|Leesbewerkingen op de schijf/seconde|Ja|Leesbewerkingen op de schijf/seconde|CountPerSecond|Average|Het gemiddelde aantal i/o-Lees bewerkingen in de vorige voorbeeld periode. Houd er rekening mee dat deze bewerkingen variabele grootte kunnen hebben.|Geen dimensies|
|Geschreven bytes op de schijf|Ja|Geschreven bytes op de schijf|Bytes|Totaal|Totale schijf doorvoer vanwege schrijf bewerkingen gedurende de voorbeeld periode.|Geen dimensies|
|Schrijfbewerkingen op de schijf/seconde|Ja|Schrijfbewerkingen op de schijf/seconde|CountPerSecond|Average|Het gemiddelde aantal i/o-schrijf bewerkingen in de vorige voorbeeld periode. Houd er rekening mee dat deze bewerkingen variabele grootte kunnen hebben.|Geen dimensies|
|DiskReadBytesPerSecond|Ja|Gelezen bytes per seconde|BytesPerSecond|Average|Gemiddelde door Voer van schijf vanwege Lees bewerkingen gedurende de voorbeeld periode.|Geen dimensies|
|DiskReadLatency|Ja|Lees latentie van schijf|Milliseconden|Average|Totale lees latentie. De som van de lees latentie van het apparaat en de kernel.|Geen dimensies|
|DiskReadOperations|Ja|Lees bewerkingen op de schijf|Aantal|Totaal|Het aantal i/o-Lees bewerkingen in de vorige voorbeeld periode. Houd er rekening mee dat deze bewerkingen variabele grootte kunnen hebben.|Geen dimensies|
|DiskWriteBytesPerSecond|Ja|Geschreven bytes per seconde|BytesPerSecond|Average|Gemiddelde doorvoer snelheid van schijven vanwege schrijf bewerkingen gedurende de voorbeeld periode.|Geen dimensies|
|DiskWriteLatency|Ja|Schrijf latentie schijf|Milliseconden|Average|Totale schrijf latentie. De som van de latentie voor het schrijven van apparaten en kernels.|Geen dimensies|
|DiskWriteOperations|Ja|Schrijf bewerkingen op de schijf|Aantal|Totaal|Het aantal i/o-schrijf bewerkingen in de vorige voorbeeld periode. Houd er rekening mee dat deze bewerkingen variabele grootte kunnen hebben.|Geen dimensies|
|MemoryActive|Ja|Actief geheugen|Bytes|Average|De hoeveelheid geheugen die wordt gebruikt door de virtuele machine in het verleden kleine tijd venster. Dit is het ' True ' nummer van de hoeveelheid geheugen die momenteel nodig is voor de VM. Extra, ongebruikt geheugen kan worden uitgewisseld of geballond zonder gevolgen voor de prestaties van de gast.|Geen dimensies|
|MemoryGranted|Ja|Toegewezen geheugen|Bytes|Average|De hoeveelheid geheugen die door de host is toegewezen aan de virtuele machine. Er wordt geen geheugen verleend aan de host totdat deze één keer wordt aangevallen en het toegewezen geheugen kan worden uitgewisseld of geballon weg als de VMkernel het geheugen nodig heeft.|Geen dimensies|
|MemoryUsed|Ja|Gebruikt geheugen|Bytes|Average|De hoeveelheid machine geheugen die wordt gebruikt door de VM.|Geen dimensies|
|Netwerk in|Ja|Netwerk in|Bytes|Totaal|Totale netwerk doorvoer voor ontvangen verkeer.|Geen dimensies|
|Netwerk uit|Ja|Netwerk uit|Bytes|Totaal|Totale netwerk doorvoer voor verzonden verkeer.|Geen dimensies|
|NetworkInBytesPerSecond|Ja|Netwerk in bytes per seconde|BytesPerSecond|Average|Gemiddelde netwerk doorvoer voor ontvangen verkeer.|Geen dimensies|
|NetworkOutBytesPerSecond|Ja|Netwerk uitgaande bytes per seconde|BytesPerSecond|Average|Gemiddelde netwerk doorvoer voor verzonden verkeer.|Geen dimensies|
|CPU-percentage|Ja|CPU-percentage|Percentage|Average|Het CPU-gebruik. Deze waarde wordt gerapporteerd met 100% voor alle processor kernen op het systeem. Een voor beeld: een twee richtings-VM met 50% van een systeem met vier kern geheugens is volledig met twee kernen.|Geen dimensies|
|PercentageCpuReady|Ja|Percentage CPU gereed|Milliseconden|Totaal|Beschik bare tijd is de tijd die nodig is voor het wachten op CPU ('s) in het afgelopen update-interval.|Geen dimensies|


## <a name="microsoftwebhostingenvironmentsmultirolepools"></a>Micro soft. Web/hostingEnvironments/multiRolePools

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|ActiveRequests|Ja|Actieve aanvragen|Aantal|Totaal|Actieve aanvragen|Exemplaar|
|AverageResponseTime|Ja|Gemiddelde reactie tijd|Seconden|Average|Gemiddelde reactie tijd|Exemplaar|
|BytesReceived|Ja|Gegevens in|Bytes|Totaal|Gegevens in|Exemplaar|
|Bytes sent|Ja|Gegevens uit|Bytes|Totaal|Gegevens uit|Exemplaar|
|CpuPercentage|Ja|CPU-percentage|Percentage|Average|CPU-percentage|Exemplaar|
|DiskQueueLength|Ja|Wachtrij lengte voor schijf|Aantal|Average|Wachtrij lengte voor schijf|Exemplaar|
|Http101|Ja|Http 101|Aantal|Totaal|Http 101|Exemplaar|
|Http2xx|Ja|Http-2xx|Aantal|Totaal|Http-2xx|Exemplaar|
|Http3xx|Ja|HTTP-3xx|Aantal|Totaal|HTTP-3xx|Exemplaar|
|Http401|Ja|HTTP 401|Aantal|Totaal|HTTP 401|Exemplaar|
|Http403|Ja|HTTP 403|Aantal|Totaal|HTTP 403|Exemplaar|
|Http404|Ja|Http 404|Aantal|Totaal|Http 404|Exemplaar|
|Http406|Ja|Http 406|Aantal|Totaal|Http 406|Exemplaar|
|Http4xx|Ja|Http-4xx|Aantal|Totaal|Http-4xx|Exemplaar|
|Http5xx|Ja|Http-server fouten|Aantal|Totaal|Http-server fouten|Exemplaar|
|HttpQueueLength|Ja|Lengte van http-wachtrij|Aantal|Average|Lengte van http-wachtrij|Exemplaar|
|LargeAppServicePlanInstances|Ja|Werk rollen voor grote App Service plannen|Aantal|Average|Werk rollen voor grote App Service plannen|Geen dimensies|
|MediumAppServicePlanInstances|Ja|Werk nemers met gemiddeld App Service plannen|Aantal|Average|Werk nemers met gemiddeld App Service plannen|Geen dimensies|
|MemoryPercentage|Ja|Geheugen percentage|Percentage|Average|Geheugen percentage|Exemplaar|
|Aanvragen|Ja|Aanvragen|Aantal|Totaal|Aanvragen|Exemplaar|
|SmallAppServicePlanInstances|Ja|Werk rollen voor kleine App Service plannen|Aantal|Average|Werk rollen voor kleine App Service plannen|Geen dimensies|
|TotalFrontEnds|Ja|Totale front-ends|Aantal|Average|Totale front-ends|Geen dimensies|


## <a name="microsoftwebhostingenvironmentsworkerpools"></a>Micro soft. Web/hostingEnvironments/workerPools

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|CpuPercentage|Ja|CPU-percentage|Percentage|Average|CPU-percentage|Exemplaar|
|MemoryPercentage|Ja|Geheugen percentage|Percentage|Average|Geheugen percentage|Exemplaar|
|WorkersAvailable|Ja|Beschik bare werk nemers|Aantal|Average|Beschik bare werk nemers|Geen dimensies|
|WorkersTotal|Ja|Totaal aantal werk rollen|Aantal|Average|Totaal aantal werk rollen|Geen dimensies|
|WorkersUsed|Ja|Gebruikte werk rollen|Aantal|Average|Gebruikte werk rollen|Geen dimensies|


## <a name="microsoftwebserverfarms"></a>Micro soft. web/server farms

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|BytesReceived|Ja|Gegevens in|Bytes|Totaal|Gegevens in|Exemplaar|
|Bytes sent|Ja|Gegevens uit|Bytes|Totaal|Gegevens uit|Exemplaar|
|CpuPercentage|Ja|CPU-percentage|Percentage|Average|CPU-percentage|Exemplaar|
|DiskQueueLength|Ja|Wachtrij lengte voor schijf|Aantal|Average|Wachtrij lengte voor schijf|Exemplaar|
|HttpQueueLength|Ja|Lengte van http-wachtrij|Aantal|Average|Lengte van http-wachtrij|Exemplaar|
|MemoryPercentage|Ja|Geheugen percentage|Percentage|Average|Geheugen percentage|Exemplaar|
|SocketInboundAll|Ja|SocketInboundAll|Aantal|Average|SocketInboundAll|Exemplaar|
|SocketLoopback|Ja|SocketLoopback|Aantal|Average|SocketLoopback|Exemplaar|
|SocketOutboundAll|Ja|SocketOutboundAll|Aantal|Average|SocketOutboundAll|Exemplaar|
|SocketOutboundEstablished|Ja|SocketOutboundEstablished|Aantal|Average|SocketOutboundEstablished|Exemplaar|
|SocketOutboundTimeWait|Ja|SocketOutboundTimeWait|Aantal|Average|SocketOutboundTimeWait|Exemplaar|
|TcpCloseWait|Ja|TCP-wacht tijd voor sluiten|Aantal|Average|TCP-wacht tijd voor sluiten|Exemplaar|
|TcpClosing|Ja|TCP sluiten|Aantal|Average|TCP sluiten|Exemplaar|
|TcpEstablished|Ja|TCP-verbinding|Aantal|Average|TCP-verbinding|Exemplaar|
|TcpFinWait1|Ja|TCP-FIN-wacht 1|Aantal|Average|TCP-FIN-wacht 1|Exemplaar|
|TcpFinWait2|Ja|TCP FIN WAIT 2|Aantal|Average|TCP FIN WAIT 2|Exemplaar|
|TcpLastAck|Ja|TCP laatste ACK|Aantal|Average|TCP laatste ACK|Exemplaar|
|TcpSynReceived|Ja|TCP SYN ontvangen|Aantal|Average|TCP SYN ontvangen|Exemplaar|
|TcpSynSent|Ja|TCP SYN verzonden|Aantal|Average|TCP SYN verzonden|Exemplaar|
|TcpTimeWait|Ja|Wacht tijd voor TCP-bewerking|Aantal|Average|Wacht tijd voor TCP-bewerking|Exemplaar|

## <a name="microsoftwebsites-excluding-functions"></a>Micro soft. web/sites (met uitzonde ring van functies) 

> [!NOTE]
> Het gebruik van het **Bestands systeem** is een nieuwe waarde die wereld wijd wordt getotaliseerd, er worden geen gegevens verwacht, tenzij u White List hebt voor een persoonlijke preview.

> [!IMPORTANT]
> De **gemiddelde reactie tijd** wordt afgeschaft om Verwar ring met metrische aggregaties te voor komen. Gebruik de **reactie tijd** als vervanging.

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|AppConnections|Ja|Verbindingen|Aantal|Average|Verbindingen|Exemplaar|
|AverageMemoryWorkingSet|Ja|Gemiddelde werkset geheugen|Bytes|Average|Gemiddelde werkset geheugen|Exemplaar|
|AverageResponseTime|Ja|Gemiddelde reactie tijd **(afgeschaft)**|Seconden|Average|Gemiddelde reactie tijd|Exemplaar|
|BytesReceived|Ja|Gegevens in|Bytes|Totaal|Gegevens in|Exemplaar|
|Bytes sent|Ja|Gegevens uit|Bytes|Totaal|Gegevens uit|Exemplaar|
|CpuTime|Ja|CPU-tijd|Seconden|Totaal|CPU-tijd|Exemplaar|
|CurrentAssemblies|Ja|Huidige Assembly's|Aantal|Average|Huidige Assembly's|Exemplaar|
|FileSystemUsage|Ja|Gebruik van bestands systeem|Bytes|Average|Gebruik van bestands systeem|Geen dimensies|
|Gen0Collections|Ja|Schone verzamelingen van 0 gen|Aantal|Totaal|Schone verzamelingen van 0 gen|Exemplaar|
|Gen1Collections|Ja|1 garbagecollection-verzamelingen|Aantal|Totaal|1 garbagecollection-verzamelingen|Exemplaar|
|Gen2Collections|Ja|Opschoon verzamelingen van generatie 2|Aantal|Totaal|Opschoon verzamelingen van generatie 2|Exemplaar|
|Formuleer|Ja|Aantal ingangen|Aantal|Average|Aantal ingangen|Exemplaar|
|HealthCheckStatus|Ja|Status van de status controle|Aantal|Average|Status van de status controle|Exemplaar|
|Http101|Ja|Http 101|Aantal|Totaal|Http 101|Exemplaar|
|Http2xx|Ja|Http-2xx|Aantal|Totaal|Http-2xx|Exemplaar|
|Http3xx|HTTP-3xx|Aantal|Totaal|HTTP-3xx|Exemplaar|
|Http401|Ja|HTTP 401|Aantal|Totaal|HTTP 401|Exemplaar|
|Http403|Ja|HTTP 403|Aantal|Totaal|HTTP 403|Exemplaar|
|Http404|Ja|Http 404|Aantal|Totaal|Http 404|Exemplaar|
|Http406|Ja|Http 406|Aantal|Totaal|Http 406|Exemplaar|
|Http4xx|Ja|Http-4xx|Aantal|Totaal|Http-4xx|Exemplaar|
|Http5xx|Ja|Http-server fouten|Aantal|Totaal|Http-server fouten|Exemplaar|
|HttpResponseTime|Ja|Reactie tijd|Seconden|Average|Reactie tijd|Exemplaar|
|IoOtherBytesPerSecond|Ja|Andere i/o-bytes per seconde|BytesPerSecond|Totaal|Andere i/o-bytes per seconde|Exemplaar|
|IoOtherOperationsPerSecond|Ja|Andere i/o-bewerkingen per seconde|BytesPerSecond|Totaal|Andere i/o-bewerkingen per seconde|Exemplaar|
|IoReadBytesPerSecond|Ja|I/o gelezen bytes per seconde|BytesPerSecond|Totaal|I/o gelezen bytes per seconde|Exemplaar|
|IoReadOperationsPerSecond|Ja|I/o-Lees bewerkingen per seconde|BytesPerSecond|Totaal|I/o-Lees bewerkingen per seconde|Exemplaar|
|IoWriteBytesPerSecond|Ja|I/o-schrijf bewerkingen in bytes per seconde|BytesPerSecond|Totaal|I/o-schrijf bewerkingen in bytes per seconde|Exemplaar|
|IoWriteOperationsPerSecond|Ja|I/o-schrijf bewerkingen per seconde|BytesPerSecond|Totaal|I/o-schrijf bewerkingen per seconde|Exemplaar|
|MemoryWorkingSet|Ja|Werkset geheugen|Bytes|Average|Werkset geheugen|Exemplaar|
|PrivateBytes|Ja|Privé-bytes|Bytes|Average|Privé-bytes|Exemplaar|
|Aanvragen|Ja|Aanvragen|Aantal|Totaal|Aanvragen|Exemplaar|
|RequestsInApplicationQueue|Ja|Aanvragen in de wachtrij van de toepassing|Aantal|Average|Aanvragen in de wachtrij van de toepassing|Exemplaar|
|Lijnen|Ja|Aantal threads|Aantal|Average|Aantal threads|Exemplaar|
|TotalAppDomains|Ja|Totaal aantal app-domeinen|Aantal|Average|Totaal aantal app-domeinen|Exemplaar|
|TotalAppDomainsUnloaded|Ja|Totaal aantal verwijderde app-domeinen|Aantal|Average|Totaal aantal verwijderde app-domeinen|Exemplaar|

## <a name="microsoftwebsites-functions"></a>Micro soft. web/sites (functies)

> [!NOTE]
> Het gebruik van het **Bestands systeem** is een nieuwe waarde die wereld wijd wordt getotaliseerd, er worden geen gegevens verwacht, tenzij u White List hebt voor een persoonlijke preview.

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|AverageMemoryWorkingSet|Ja|Gemiddelde werkset geheugen|Bytes|Average|Gemiddelde werkset geheugen|Exemplaar|
|BytesReceived|Ja|Gegevens in|Bytes|Totaal|Gegevens in|Exemplaar|
|Bytes sent|Ja|Gegevens uit|Bytes|Totaal|Gegevens uit|Exemplaar|
|CurrentAssemblies|Ja|Huidige Assembly's|Aantal|Average|Huidige Assembly's|Exemplaar|
|FileSystemUsage|Ja|Gebruik van bestands systeem|Bytes|Average|Gebruik van bestands systeem|Geen dimensies|
|FunctionExecutionCount|Ja|Aantal functie-uitvoeringen|Aantal|Totaal|Aantal functie-uitvoeringen|Exemplaar|
|FunctionExecutionUnits|Ja|Eenheden voor functie-uitvoering|Aantal|Totaal|[Eenheden voor functie-uitvoering](https://github.com/Azure/Azure-Functions/wiki/Consumption-Plan-Cost-Billing-FAQ#how-can-i-view-graphs-of-execution-count-and-gb-seconds)|Exemplaar|
|Gen0Collections|Ja|Schone verzamelingen van 0 gen|Aantal|Totaal|Schone verzamelingen van 0 gen|Exemplaar|
|Gen1Collections|Ja|1 garbagecollection-verzamelingen|Aantal|Totaal|1 garbagecollection-verzamelingen|Exemplaar|
|Gen2Collections|Ja|Opschoon verzamelingen van generatie 2|Aantal|Totaal|Opschoon verzamelingen van generatie 2|Exemplaar|
|HealthCheckStatus|Ja|Status van de status controle|Aantal|Average|Status van de status controle|Exemplaar|
|Http5xx|Ja|Http-server fouten|Aantal|Totaal|Http-server fouten|Exemplaar|
|IoOtherBytesPerSecond|Ja|Andere i/o-bytes per seconde|BytesPerSecond|Totaal|Andere i/o-bytes per seconde|Exemplaar|
|IoOtherOperationsPerSecond|Ja|Andere i/o-bewerkingen per seconde|BytesPerSecond|Totaal|Andere i/o-bewerkingen per seconde|Exemplaar|
|IoReadBytesPerSecond|Ja|I/o gelezen bytes per seconde|BytesPerSecond|Totaal|I/o gelezen bytes per seconde|Exemplaar|
|IoReadOperationsPerSecond|Ja|I/o-Lees bewerkingen per seconde|BytesPerSecond|Totaal|I/o-Lees bewerkingen per seconde|Exemplaar|
|IoWriteBytesPerSecond|Ja|I/o-schrijf bewerkingen in bytes per seconde|BytesPerSecond|Totaal|I/o-schrijf bewerkingen in bytes per seconde|Exemplaar|
|IoWriteOperationsPerSecond|Ja|I/o-schrijf bewerkingen per seconde|BytesPerSecond|Totaal|I/o-schrijf bewerkingen per seconde|Exemplaar|
|MemoryWorkingSet|Ja|Werkset geheugen|Bytes|Average|Werkset geheugen|Exemplaar|
|PrivateBytes|Ja|Privé-bytes|Bytes|Average|Privé-bytes|Exemplaar|
|RequestsInApplicationQueue|Ja|Aanvragen in de wachtrij van de toepassing|Aantal|Average|Aanvragen in de wachtrij van de toepassing|Exemplaar|
|TotalAppDomains|Ja|Totaal aantal app-domeinen|Aantal|Average|Totaal aantal app-domeinen|Exemplaar|
|TotalAppDomainsUnloaded|Ja|Totaal aantal verwijderde app-domeinen|Aantal|Average|Totaal aantal verwijderde app-domeinen|Exemplaar|

## <a name="microsoftwebsitesslots"></a>Micro soft. web/sites/sleuven

|Gegevens|Exporteerbaar via Diagnostische instellingen?|Weergave naam voor metrische gegevens|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|---|
|AppConnections|Ja|Verbindingen|Aantal|Average|Verbindingen|Exemplaar|
|AverageMemoryWorkingSet|Ja|Gemiddelde werkset geheugen|Bytes|Average|Gemiddelde werkset geheugen|Exemplaar|
|AverageResponseTime|Ja|Gemiddelde reactie tijd|Seconden|Average|Gemiddelde reactie tijd|Exemplaar|
|BytesReceived|Ja|Gegevens in|Bytes|Totaal|Gegevens in|Exemplaar|
|Bytes sent|Ja|Gegevens uit|Bytes|Totaal|Gegevens uit|Exemplaar|
|CpuTime|Ja|CPU-tijd|Seconden|Totaal|CPU-tijd|Exemplaar|
|CurrentAssemblies|Ja|Huidige Assembly's|Aantal|Average|Huidige Assembly's|Exemplaar|
|FileSystemUsage|Ja|Gebruik van bestands systeem|Bytes|Average|Gebruik van bestands systeem|Geen dimensies|
|FunctionExecutionCount|Ja|Aantal functie-uitvoeringen|Aantal|Totaal|Aantal functie-uitvoeringen|Exemplaar|
|FunctionExecutionUnits|Ja|Eenheden voor functie-uitvoering|Aantal|Totaal|Eenheden voor functie-uitvoering|Exemplaar|
|Gen0Collections|Ja|Schone verzamelingen van 0 gen|Aantal|Totaal|Schone verzamelingen van 0 gen|Exemplaar|
|Gen1Collections|Ja|1 garbagecollection-verzamelingen|Aantal|Totaal|1 garbagecollection-verzamelingen|Exemplaar|
|Gen2Collections|Ja|Opschoon verzamelingen van generatie 2|Aantal|Totaal|Opschoon verzamelingen van generatie 2|Exemplaar|
|Formuleer|Ja|Aantal ingangen|Aantal|Average|Aantal ingangen|Exemplaar|
|HealthCheckStatus|Ja|Status van de status controle|Aantal|Average|Status van de status controle|Exemplaar|
|Http101|Ja|Http 101|Aantal|Totaal|Http 101|Exemplaar|
|Http2xx|Ja|Http-2xx|Aantal|Totaal|Http-2xx|Exemplaar|
|Http3xx|Ja|HTTP-3xx|Aantal|Totaal|HTTP-3xx|Exemplaar|
|Http401|Ja|HTTP 401|Aantal|Totaal|HTTP 401|Exemplaar|
|Http403|Ja|HTTP 403|Aantal|Totaal|HTTP 403|Exemplaar|
|Http404|Ja|Http 404|Aantal|Totaal|Http 404|Exemplaar|
|Http406|Ja|Http 406|Aantal|Totaal|Http 406|Exemplaar|
|Http4xx|Ja|Http-4xx|Aantal|Totaal|Http-4xx|Exemplaar|
|Http5xx|Ja|Http-server fouten|Aantal|Totaal|Http-server fouten|Exemplaar|
|HttpResponseTime|Ja|Reactie tijd|Seconden|Average|Reactie tijd|Exemplaar|
|IoOtherBytesPerSecond|Ja|Andere i/o-bytes per seconde|BytesPerSecond|Totaal|Andere i/o-bytes per seconde|Exemplaar|
|IoOtherOperationsPerSecond|Ja|Andere i/o-bewerkingen per seconde|BytesPerSecond|Totaal|Andere i/o-bewerkingen per seconde|Exemplaar|
|IoReadBytesPerSecond|Ja|I/o gelezen bytes per seconde|BytesPerSecond|Totaal|I/o gelezen bytes per seconde|Exemplaar|
|IoReadOperationsPerSecond|Ja|I/o-Lees bewerkingen per seconde|BytesPerSecond|Totaal|I/o-Lees bewerkingen per seconde|Exemplaar|
|IoWriteBytesPerSecond|Ja|I/o-schrijf bewerkingen in bytes per seconde|BytesPerSecond|Totaal|I/o-schrijf bewerkingen in bytes per seconde|Exemplaar|
|IoWriteOperationsPerSecond|Ja|I/o-schrijf bewerkingen per seconde|BytesPerSecond|Totaal|I/o-schrijf bewerkingen per seconde|Exemplaar|
|MemoryWorkingSet|Ja|Werkset geheugen|Bytes|Average|Werkset geheugen|Exemplaar|
|PrivateBytes|Ja|Privé-bytes|Bytes|Average|Privé-bytes|Exemplaar|
|Aanvragen|Ja|Aanvragen|Aantal|Totaal|Aanvragen|Exemplaar|
|RequestsInApplicationQueue|Ja|Aanvragen in de wachtrij van de toepassing|Aantal|Average|Aanvragen in de wachtrij van de toepassing|Exemplaar|
|Lijnen|Ja|Aantal threads|Aantal|Average|Aantal threads|Exemplaar|
|TotalAppDomains|Ja|Totaal aantal app-domeinen|Aantal|Average|Totaal aantal app-domeinen|Exemplaar|
|TotalAppDomainsUnloaded|Ja|Totaal aantal verwijderde app-domeinen|Aantal|Average|Totaal aantal verwijderde app-domeinen|Exemplaar|


## <a name="next-steps"></a>Volgende stappen
* [Meer informatie over metrische gegevens in Azure Monitor](data-platform.md)
* [Waarschuwingen maken op basis van metrische gegevens](alerts-overview.md)
* [Metrische gegevens exporteren naar opslag, Event hub of Log Analytics](platform-logs-overview.md)
