---
title: Diagnostische logboekregistratie inschakelen voor apps - Azure App Service
description: Meer informatie over hoe u Diagnostische logboekregistratie inschakelen en instrumentatie kunt toevoegen aan uw toepassing, evenals hoe u toegang tot de gegevens die zijn vastgelegd door Azure.
services: app-service
documentationcenter: .net
author: cephalin
manager: erikre
editor: jimbe
ms.assetid: c9da27b2-47d4-4c33-a3cb-1819955ee43b
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2016
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: c21a923f06a768c0a9a0f2843a24583df7a7821d
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67059646"
---
# <a name="enable-diagnostics-logging-for-apps-in-azure-app-service"></a>Diagnostische logboekregistratie inschakelen voor apps in Azure App Service
## <a name="overview"></a>Overzicht
Azure bevat ingebouwde diagnosefuncties voor de ondersteuning bij foutopsporing in een [App Service-app](https://go.microsoft.com/fwlink/?LinkId=529714). In dit artikel leert u hoe u Diagnostische logboekregistratie inschakelen en instrumentatie kunt toevoegen aan uw toepassing, evenals hoe u toegang tot de gegevens die zijn vastgelegd door Azure.

In dit artikel wordt de [Azure-portal](https://portal.azure.com) en Azure CLI om te werken met diagnostische logboeken. Zie voor meer informatie over het werken met diagnostische logboeken met behulp van Visual Studio [probleemoplossing voor Azure in Visual Studio](troubleshoot-dotnet-visual-studio.md).

## <a name="whatisdiag"></a>Web server diagnostics en application diagnostics
App Service biedt diagnosefunctionaliteit voor waardevolle informatie uit de webserver en de web-App. Deze logisch zijn gescheiden in **web server diagnostische** en **toepassingsdiagnose**.

### <a name="web-server-diagnostics"></a>Web server diagnostische gegevens
U kunt inschakelen of uitschakelen van de volgende soorten logboeken:

* **Gedetailleerde fout logboekregistratie** -gedetailleerde informatie weer voor elke aanvraag die in HTTP-statuscode 400 of hoger resulteert. Deze kan informatie om te bepalen waarom de server heeft geretourneerd met de foutcode bevatten. Een HTML-bestand is gegenereerd voor elke fout in het bestandssysteem van de app, en tot wel 50 fouten (bestanden) worden bewaard. Wanneer het aantal HTML-bestanden groter zijn dan 50, wordt de oudste 26 bestanden worden automatisch verwijderd.
* **Kan geen aanvraag tracering** -gedetailleerde informatie over mislukte aanvragen, met inbegrip van een tracering van de IIS-componenten gebruikt voor het verwerken van de aanvraag en de tijd die in elk onderdeel. Dit is handig als u wilt verbeteren de prestaties van de site of een specifieke HTTP-fout isoleren. Een map wordt gegenereerd voor elke fout in het bestandssysteem van de app. Beleid voor het bewaren van bestand zijn hetzelfde als de gedetailleerde fout bij het aanmelden hierboven.
* **Web Server-logboekregistratie** -informatie over HTTP-transacties met behulp van de [uitgebreide W3C-logboekbestandsindeling](/windows/desktop/Http/w3c-logging). Dit is handig bij het bepalen van de algemene metrische sitegegevens, zoals het aantal aanvragen dat is verwerkt of het aantal aanvragen afkomstig zijn van een specifiek IP-adres.

### <a name="application-diagnostics"></a>Application diagnostics
Application diagnostics kunt u voor het vastleggen van gegevens die worden geproduceerd door een web-App. ASP.NET-toepassingen kunnen gebruikmaken van de [System.Diagnostics.Trace](/dotnet/api/system.diagnostics.trace) klasse om informatie te registreren in het toepassingslogboek van diagnostische gegevens. Bijvoorbeeld:

    System.Diagnostics.Trace.TraceError("If you're seeing this, something bad happened");

Tijdens runtime, kunt u deze logboeken om te helpen bij het oplossen van problemen ophalen. Zie voor meer informatie, [het oplossen van Azure App Service in Visual Studio](troubleshoot-dotnet-visual-studio.md).

App Service registreert ook informatie over de implementatie wanneer u inhoud naar een app publiceren. Dit gebeurt automatisch en er zijn geen configuratieinstellingen voor de implementatie van logboekregistratie. Logboekregistratie van de implementatie kunt u om te bepalen waarom een implementatie is mislukt. Als u een aangepaste implementatiescript gebruikt, kunt u bijvoorbeeld implementatie logboekregistratie gebruiken om te bepalen waarom het script is mislukt.

## <a name="enablediag"></a>Diagnostische gegevens inschakelen
Om in te schakelen van diagnostische gegevens in de [Azure-portal](https://portal.azure.com), gaat u naar de pagina voor uw app en klikt u op **instellingen > Logboeken met diagnostische gegevens**.

<!-- todo:cleanup dogfood addresses in screenshot -->
![Logboeken-onderdeel](./media/web-sites-enable-diagnostic-log/logspart.png)

Als u activeert **toepassingsdiagnose**, u er ook voor kiezen de **niveau**. De volgende tabel bevat de categorieën van elk niveau omvat ook Logboeken:

| Niveau| Opgenomen logboekcategorieën |
|-|-|
|**Uitgeschakeld** | Geen |
|**Fout** | Kritieke fout |
|**Waarschuwing** | Waarschuwing, fout, kritiek|
|**Informatie** | Info, waarschuwing, fout, kritiek|
|**uitgebreide** | Tracering, foutopsporing, Info, waarschuwing, fout, kritiek (alle categorieën) |
|-|-|

Voor **toepassingslogboeken**, kunt u de optie voor het systeem van bestand tijdelijk voor foutopsporing inschakelen. Deze optie nu schakelt automatisch in 12 uur. U kunt ook inschakelen op de optie blob storage een blobcontainer om te schrijven Logboeken om te selecteren.

> [!NOTE]
> Momenteel wordt alleen .NET-toepassingslogboeken kunnen worden geschreven naar de blob-opslag. Java, PHP, Node.js, Python toepassingslogboeken alleen kunnen worden opgeslagen op het bestandssysteem (zonder codewijzigingen om te schrijven logboeken naar de externe opslag).
>
>

Voor **webserverlogboeken**, kunt u **opslag** of **bestandssysteem**. Selecteren **opslag** kunt u een opslagaccount en een blob-container die de logboeken worden geschreven om te selecteren. 

Als u Logboeken op het bestandssysteem opslaat, kunnen de bestanden worden geopend door de FTP of gedownload als een Zip-archief met behulp van Azure CLI.

Standaard logboeken worden niet automatisch verwijderd (met uitzondering van **toepassingslogboeken (bestandssysteem)** ). Als u wilt verwijderen automatisch logboeken, instellen de **bewaarperiode (dagen)** veld.

> [!NOTE]
> Als u [opnieuw genereren van toegangssleutels voor uw opslagaccount](../storage/common/storage-create-storage-account.md), moet u de configuratie van de respectieve logboekregistratie voor het gebruik van de bijgewerkte sleutels opnieuw instellen. Om dit te doen:
>
> 1. In de **configureren** tabblad, stelt u de functie voor de respectieve logboekregistratie op **uit**. Sla de instelling.
> 2. Logboekregistratie naar de blob storage-account opnieuw inschakelen. Sla de instelling.
>
>

Een combinatie van file system- of blob storage kan worden ingeschakeld op hetzelfde moment en configuraties van het afzonderlijke log niveau hebben. U wilt bijvoorbeeld aanmelden fouten en waarschuwingen naar de blobopslag als een oplossing op lange termijn logboekregistratie, tijdens het inschakelen van logboekregistratie in systeem met een niveau van uitgebreide.

Hoewel beide opslaglocaties de dezelfde algemene informatie voor de vastgelegde gebeurtenissen bieden **blobopslag** registreert aanvullende informatie zoals de exemplaar-ID, thread-ID en een meer gedetailleerd tijdstempel (maatstreepjes-indeling) dan logboekregistratie naar **bestandssysteem**.

> [!NOTE]
> Gegevens die zijn opgeslagen **blobopslag** kunnen alleen worden geopend met behulp van een storage-client of een toepassing die u rechtstreeks met deze systemen voor opslag werken kunt. Bijvoorbeeld, Visual Studio 2013 bevat een Opslagverkenner die kan worden gebruikt voor het verkennen van blob-opslag en HDInsight toegang tot de gegevens die zijn opgeslagen in blob-opslag. U kunt ook schrijven met een toepassing die toegang heeft tot Azure Storage met behulp van een van de [Azure-SDK's](https://azure.microsoft.com/downloads/).
>

## <a name="download"></a> Procedures: Logboeken downloaden
Diagnostische gegevens die zijn opgeslagen in het app-bestandssysteem zijn toegankelijk via FTP. Het kan ook worden gedownload als een Zip-archief met behulp van Azure CLI.

De mapstructuur die de logboeken worden opgeslagen in is als volgt:

* **Toepassingslogboeken** -/LogFiles/toepassing /. Deze map bevat een of meer tekstbestanden met gegevens die worden geproduceerd door logboekregistratie van toepassingen.
* **Kan geen aanvraag traceringen** -/ logboekbestanden/W3SVC ### /. Deze map bevat een XSL-bestand en een of meer XML-bestanden. Zorg ervoor dat u de XSL-bestand in dezelfde map downloaden als het XML-bestand bestand(en) omdat het XSL-bestand functionaliteit biedt voor het formatteren en filteren van de inhoud van de XML-bestanden wanneer deze wordt bekeken in Internet Explorer.
* **Gedetailleerde foutenlogboeken** -/LogFiles/DetailedErrors /. Deze map bevat een of meer htm-bestanden met uitgebreide informatie voor HTTP-fouten die zich hebben voorgedaan.
* **Web Server-logboeken** -/LogFiles/http/RawLogs. Deze map bevat een of meer tekstbestanden geformatteerd met het [uitgebreide W3C-logboekbestandsindeling](/windows/desktop/Http/w3c-logging).
* **Implementatielogboeken** -/ logboekbestanden/Git. Deze map bevat de logboeken die worden gegenereerd door de interne implementatieprocessen die worden gebruikt door Azure App Service, evenals de logboeken voor Git-implementaties. U vindt hier ook Logboeken onder D:\home\site\deployments-implementatie.

### <a name="ftp"></a>FTP

Zie het openen van een FTP-verbinding met de FTP-server van uw app [uw app implementeren in Azure App Service met behulp van FTP/S](deploy-ftp.md).

Eenmaal verbinding hebben met FTP/S-server van uw app, opent u de **logboekbestanden** map waar de logbestanden worden opgeslagen.

### <a name="download-with-azure-cli"></a>Met Azure CLI downloaden
De logboekbestanden met behulp van de Azure-opdrachtregelinterface downloaden, opent u een nieuwe opdrachtprompt, PowerShell, Bash of Terminal-sessie en voer de volgende opdracht:

    az webapp log download --resource-group resourcegroupname --name appname

Deze opdracht slaat u de logboeken voor de app met de naam 'appname' naar een bestand met de naam **webapp_logs.zip** in de huidige map.

> [!NOTE]
> Als u Azure CLI nog niet hebt geïnstalleerd, of nog niet hebt geconfigureerd voor het gebruik van uw Azure-abonnement, Zie [over het gebruik van Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest).
>
>

## <a name="how-to-view-logs-in-application-insights"></a>Procedure: Logboeken weergeven in Application Insights
Visual Studio Application Insights biedt hulpprogramma's voor het filteren en zoeken in Logboeken en voor de logboeken correleren met aanvragen en andere gebeurtenissen.

1. De Application Insights SDK toevoegen aan uw project in Visual Studio.
   * Klik in Solution Explorer met de rechtermuisknop op uw project en kies Application Insights toevoegen. De interface leidt u door de stappen, waaronder het maken van een Application Insights-resource. [Meer informatie](../azure-monitor/app/asp-net.md)
2. De Trace Listener-pakket toevoegen aan uw project.
   * Met de rechtermuisknop op uw project en kies NuGet-pakketten beheren. Selecteer `Microsoft.ApplicationInsights.TraceListener` [meer informatie](../azure-monitor/app/asp-net-trace-logs.md)
3. Uploaden van uw project en voer dit voor het genereren van logboekgegevens.
4. In de [Azure-portal](https://portal.azure.com/), blader naar de nieuwe Application Insights-resource en open **zoeken**. U ziet uw logboekgegevens, samen met de aanvraag, gebruik en andere telemetrie. Telemetrie ontvangen van een paar minuten kan duren: klik op vernieuwen. [Meer informatie](../azure-monitor/app/diagnostic-search.md)

[Meer informatie over de prestaties bijhouden met Application Insights](../azure-monitor/app/azure-web-apps.md)

## <a name="streamlogs"></a> Procedures: Logboeken streamen
Tijdens het ontwikkelen van een toepassing, is het vaak nuttig om te zien van logboekgegevens in near-real-time. U kunt de logboekinformatie voor streamen naar uw ontwikkelomgeving met behulp van Azure CLI.

> [!NOTE]
> Bepaalde typen logboekregistratie buffer schrijven naar het logboekbestand, dit in niet-geordende gebeurtenissen in de stroom resulteren kan. De logboekvermelding van een toepassing die wordt uitgevoerd wanneer een gebruiker een pagina bezoeken kan bijvoorbeeld worden weergegeven in de stroom voordat de bijbehorende HTTP-vermelding voor de pagina-aanvraag.
>
> [!NOTE]
> Logboekstreaming ook streams voor gegevens die worden geschreven naar een tekstbestand die zijn opgeslagen in de **D:\\home\\logboekbestanden\\**  map.
>
>

### <a name="streaming-with-azure-cli"></a>Streaming met Azure CLI
Om te streamen van logboekgegevens, opent u een nieuwe opdrachtprompt, PowerShell, Bash of Terminal-sessie en voer de volgende opdracht:

    az webapp log tail --name appname --resource-group myResourceGroup

Met deze opdracht maakt verbinding met de app met de naam "appname" en beginnen met streamen van gegevens naar het venster zoals gebeurtenissen op de app plaatsvinden. Alle informatie geschreven naar hebben die eindigt op .txt, .log of htm-bestanden die zijn opgeslagen in de map /LogFiles (d:/home/logboekbestanden) worden gestreamd naar de lokale console.

Om te filteren op specifieke gebeurtenissen, zoals fouten, gebruikt u de **--Filter** parameter. Bijvoorbeeld:

    az webapp log tail --name appname --resource-group myResourceGroup --filter Error

Om te filteren op specifieke logboek typen, zoals HTTP, gebruikt u de **--pad** parameter. Bijvoorbeeld:

    az webapp log tail --name appname --resource-group myResourceGroup --path http

> [!NOTE]
> Als u Azure CLI nog niet hebt geïnstalleerd, of nog niet hebt geconfigureerd voor het gebruik van uw Azure-abonnement, Zie [over het gebruik van Azure CLI](../cli-install-nodejs.md).
>
>

## <a name="understandlogs"></a> Procedures: Diagnostische logboeken begrijpen
### <a name="application-diagnostics-logs"></a>Application diagnostics-Logboeken
Application diagnostics wordt informatie opgeslagen in een specifieke indeling voor .NET-toepassingen, afhankelijk van of het opslaan van logboeken naar de file system- of blob storage. 

De basisset met gegevens die zijn opgeslagen is hetzelfde voor beide opslagtypen: de datum en tijd is die de gebeurtenis heeft plaatsgevonden, wordt de proces-ID die de gebeurtenis, het gebeurtenistype (informatie, waarschuwing, fout) en het bericht van de gebeurtenis heeft geproduceerd. Met behulp van het bestandssysteem voor logboekopslag is handig wanneer u directe toegang tot het oplossen van een probleem omdat de logboekbestanden worden bijgewerkt in de buurt onmiddellijk nodig hebt. BLOB-opslag wordt gebruikt voor archiveringsdoeleinden omdat de bestanden worden opgeslagen in de cache en dat vervolgens in de opslagcontainer volgens een schema wordt leeggemaakt.

**Bestandssysteem**

Elke regel geregistreerd in het bestandssysteem of ontvangen met behulp van streaming is in de volgende indeling:

    {Date}  PID[{process ID}] {event type/level} {message}

Bijvoorbeeld, wordt een foutgebeurtenis weergegeven die vergelijkbaar is met het volgende voorbeeld:

    2014-01-30T16:36:59  PID[3096] Error       Fatal error on the page!

Logboekregistratie naar het bestandssysteem bevat de meest elementaire informatie van de drie beschikbare methoden, alleen de tijd, proces-ID, gebeurtenisniveau en bericht te leveren.

**Blob Storage**

Tijdens de registratie van BLOB storage, kunnen gegevens worden opgeslagen in de indeling met door komma's gescheiden waarden (CSV). Aanvullende velden worden geregistreerd voor gedetailleerdere informatie over de gebeurtenis. De volgende eigenschappen worden gebruikt voor elke rij in de CSV:

| Naam van eigenschap | Waarde-indeling |
| --- | --- |
| Date |De datum en tijd waarop de gebeurtenis heeft plaatsgevonden |
| Niveau |Gebeurtenisniveau (bijvoorbeeld fout, waarschuwing, informatie) |
| ApplicationName |Naam van de app |
| InstanceId |Exemplaar van de app die de gebeurtenis heeft plaatsgevonden op |
| EventTickCount |De datum en tijd waarop de gebeurtenis heeft plaatsgevonden, in de indeling van de maatstreepjes (grotere precisie) |
| Gebeurtenis-id |De gebeurtenis-ID van deze gebeurtenis<p><p>Standaard ingesteld op 0 als er niets opgegeven |
| PID |Proces-ID |
| TID |De thread-ID van de thread die de gebeurtenis heeft geproduceerd |
| Message |Details van gebeurtenisbericht |

De gegevens die zijn opgeslagen in een blob wordt het volgende voorbeeld als volgt uitzien:

    date,level,applicationName,instanceId,eventTickCount,eventId,pid,tid,message
    2014-01-30T16:36:52,Error,mywebapp,6ee38a,635266966128818593,0,3096,9,An error occurred

> [!NOTE]
> Voor ASP.NET Core, logboekregistratie wordt gerealiseerd met behulp van de [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider deze provider deposito's aanvullende in de blob-container logboekbestanden. Zie voor meer informatie, [ASP.NET Core logboekregistratie in Azure](/aspnet/core/fundamentals/logging).
>
>

### <a name="failed-request-traces"></a>Traceringen van de aanvraag is mislukt
Traceringen van mislukte aanvragen worden opgeslagen in de XML-bestanden met de naam **fr ### .xml**. Als u wilt maken het gemakkelijker om de geregistreerde gegevens weer te geven, een XSL-opmaakmodel met de naam **freb.xsl** is opgegeven in dezelfde map als de XML-bestanden. Als u een van de XML-bestanden in Internet Explorer openen, Internet Explorer het XSL-opmaakmodel gebruikt voor een opgemaakte weergave van de trace-informatie, zoals in het volgende voorbeeld:

![mislukte aanvragen die worden weergegeven in de browser](./media/web-sites-enable-diagnostic-log/tws-failedrequestinbrowser.png)

> [!NOTE]
> Er is een eenvoudige manier om de traceringen opgemaakte mislukte aanvragen weer te geven om te navigeren naar de pagina van uw app in de portal. Selecteer in het menu links **vaststellen en oplossen van problemen met**, zoek vervolgens naar **mislukt aanvragen traceerlogboeken**, klik vervolgens op het pictogram om te bladeren en de trace die u wilt weergeven.
>

### <a name="detailed-error-logs"></a>Logboeken met details
Logboeken met details zijn HTML-documenten die bieden meer gedetailleerde informatie over HTTP-fouten die zijn opgetreden. Omdat ze gewoon HTML-documenten, kunnen ze worden weergegeven via een webbrowser.

### <a name="web-server-logs"></a>Webserverlogboeken
De web server-logboeken zijn geformatteerd met het [uitgebreide W3C-logboekbestandsindeling](/windows/desktop/Http/w3c-logging). Deze informatie kan worden gelezen met behulp van een teksteditor of met behulp van hulpprogramma's zoals geparseerd [Logboekparser](https://go.microsoft.com/fwlink/?LinkId=246619).

> [!NOTE]
> De logboeken die worden geproduceerd door Azure App Service bieden geen ondersteuning voor de **s-computername**, **s-IP-** , of **cs-version** velden.
>
>

## <a name="nextsteps"></a> Volgende stappen
* [Azure App Service bewaken](web-sites-monitor.md)
* [Oplossen van problemen met Azure App Service in Visual Studio](troubleshoot-dotnet-visual-studio.md)
* [App-Logboeken in HDInsight analyseren](https://gallery.technet.microsoft.com/scriptcenter/Analyses-Windows-Azure-web-0b27d413)
