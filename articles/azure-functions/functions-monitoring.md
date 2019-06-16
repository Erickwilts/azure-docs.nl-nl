---
title: Azure Functions controleren
description: Informatie over het gebruik van Azure Application Insights met Azure Functions voor het bewaken van een functie wordt uitgevoerd.
services: functions
author: ggailey777
manager: jeconnoc
keywords: Azure-functies, functies, gebeurtenisverwerking, webhooks, dynamisch berekenen, architectuur zonder server
ms.assetid: 501722c3-f2f7-4224-a220-6d59da08a320
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 04/04/2019
ms.author: glenga
ms.openlocfilehash: fecf6759dd7b277dda10fa2656e6ae9407490370
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64922970"
---
# <a name="monitor-azure-functions"></a>Azure Functions controleren

[Azure Functions](functions-overview.md) biedt ingebouwde integratie met [Azure Application Insights](../azure-monitor/app/app-insights-overview.md) voor het bewaken van functies. In dit artikel leest u hoe het configureren van Azure Functions voor het verzenden van het systeem gegenereerde logboekbestanden naar Application Insights.

Het is raadzaam om met behulp van Application Insights, omdat het logboek, prestaties en foutgegevens verzamelt. Automatisch detecteert afwijkende prestaties en bevat krachtige analysehulpmiddelen om u te helpen u problemen identificeren en om te begrijpen hoe uw functies worden gebruikt. Het is bedoeld om u te helpen de prestaties en bruikbaarheid continu te verbeteren. U kunt zelfs Application Insights gebruiken tijdens het ontwikkelen van lokale functie-app-project. Zie voor meer informatie, [wat is Application Insights?](../azure-monitor/app/app-insights-overview.md).

Als de vereiste instrumentatie van Application Insights is gratis ingebouwd in Azure Functions, hoeft u een geldige instrumentatiesleutel uw functie-app verbinden met een Application Insights-resource is.

## <a name="application-insights-pricing-and-limits"></a>Prijzen van Application Insights en limieten

U kunt gratis Application Insights-integratie met de functie-Apps uitproberen. Er is een dagelijkse limiet voor hoeveel gegevens kan gratis worden verwerkt. U kunt deze limiet tijdens het testen mogelijk bereikt. Als uw dagelijkse limiet bijna is bereikt, biedt Azure portal en e-mailmeldingen. Als u deze waarschuwingen mist en de limiet bereikt, worden nieuwe logboeken wordt niet weergegeven in Application Insights-query's. Houd rekening met de limiet om te voorkomen dat onnodig tijd voor het oplossen van problemen. Zie voor meer informatie, [beheren van prijzen en gegevensvolumes in Application Insights](../azure-monitor/app/pricing.md).

## <a name="enable-application-insights-integration"></a>Application Insights-integratie inschakelen

Voor een functie-app om gegevens te verzenden naar Application Insights, moet deze weten de instrumentatiesleutel van een Application Insights-resource. De sleutel moet zich in een app-instelling met de naam **APPINSIGHTS_INSTRUMENTATIONKEY**.

### <a name="new-function-app-in-the-portal"></a>Nieuwe functie-app in de portal

Wanneer u [uw functie-app maken in Azure portal](functions-create-first-azure-function.md), Application Insights-integratie is standaard ingeschakeld. De Application Insights-resource heeft dezelfde naam als uw functie-app en deze gemaakt in dezelfde regio of in de dichtstbijzijnde regio.

Als u wilt controleren van de Application Insights-resource wordt gemaakt, selecteert u deze om uit te breiden de **Application Insights** venster. U kunt wijzigen de **nieuwe resourcenaam** of kies een andere **locatie** in een [Azure-Geografie](https://azure.microsoft.com/global-infrastructure/geographies/) waar u uw gegevens kunt opslaan.

![Application Insights inschakelen tijdens het maken van een functie-app](media/functions-monitoring/enable-ai-new-function-app.png)

Als u ervoor kiest **maken**, een Application Insights-resource wordt gemaakt met uw functie-app met de `APPINSIGHTS_INSTRUMENTATIONKEY` instellen in de toepassingsinstellingen. Alles is klaar om te beginnen.

<a id="manually-connect-an-app-insights-resource"></a>
### <a name="add-to-an-existing-function-app"></a>Toevoegen aan een bestaande functie-app 

Wanneer u maakt een functie-app via de [Azure CLI](functions-create-first-azure-function-azure-cli.md), [Visual Studio](functions-create-your-first-function-visual-studio.md), of [Visual Studio Code](functions-create-first-function-vs-code.md), moet u de Application Insights-resource maken. U kunt vervolgens de instrumentatiesleutel van die resource toevoegen als een toepassingsinstelling van de in uw functie-app.

[!INCLUDE [functions-connect-new-app-insights.md](../../includes/functions-connect-new-app-insights.md)]

Vroege versies van functies gebruikt ingebouwde controle, die niet meer wordt aanbevolen. Bij het inschakelen van Application Insights-integratie voor dergelijke een functie-app, moet u ook [ingebouwde logboekregistratie uitschakelen](#disable-built-in-logging).  

## <a name="view-telemetry-in-monitor-tab"></a>Telemetrie weergeven in het tabblad Monitor

Met [Application Insights-integratie ingeschakeld](#enable-application-insights-integration), vindt u telemetriegegevens in de **Monitor** tabblad.

1. Selecteer een functie die ten minste eenmaal uitgevoerd wanneer de Application Insights is geconfigureerd in de pagina functie-app. Selecteer vervolgens de **Monitor** tabblad.

   ![Tabblad Monitor selecteren](media/functions-monitoring/monitor-tab.png)

1. Selecteer **vernieuwen** periodiek, totdat de lijst van functieaanroepen wordt weergegeven.

   Het kan tot vijf minuten voor de lijst wordt geopend terwijl de telemetrieclient batches gegevens voor verzending naar de server op te nemen. (De vertraging is niet van toepassing op de [Live Metrics Stream](../azure-monitor/app/live-stream.md). Deze service maakt verbinding met de Functions-host wanneer u de pagina laadt, zodat logboeken rechtstreeks naar de pagina worden gestreamd.)

   ![Lijst met aanroepen](media/functions-monitoring/monitor-tab-ai-invocations.png)

1. De logboeken voor een bepaalde functie-aanroep, selecteer de **datum** kolomkoppeling voor deze aanroep.

   ![Aanroepdetails koppelen](media/functions-monitoring/invocation-details-link-ai.png)

   De uitvoer van de logboekregistratie voor deze aanroep wordt weergegeven in een nieuwe pagina.

   ![Aanroepdetails](media/functions-monitoring/invocation-details-ai.png)

U kunt zien dat beide pagina's hebben een **uitvoeren in Application Insights** koppeling naar de Application Insights Analytics-query die de gegevens worden opgehaald.

![Uitvoeren in Application Insights](media/functions-monitoring/run-in-ai.png)

De volgende query wordt weergegeven. U kunt zien dat de aanroep-lijst beperkt tot de laatste 30 dagen is. De lijst ziet u niet meer dan 20 rijen (`where timestamp > ago(30d) | take 20`). De lijst van de details van aanroepen is voor de afgelopen 30 dagen zonder limiet.

![Lijst met Application Insights Analytics aanroep](media/functions-monitoring/ai-analytics-invocation-list.png)

Zie voor meer informatie, [telemetriegegevens op te vragen](#query-telemetry-data) verderop in dit artikel.

## <a name="view-telemetry-in-application-insights"></a>Telemetrie in Application Insights weergeven

Voor Application Insights openen vanuit een functie-app in Azure portal, gaat u naar de functie-app **overzicht** pagina. Onder **functies geconfigureerd**, selecteer **Application Insights**.

![Application Insights openen vanuit de pagina overzicht van functie-app](media/functions-monitoring/ai-link.png)

Zie voor meer informatie over het gebruik van Application Insights de [documentatie voor Application Insights](https://docs.microsoft.com/azure/application-insights/). In deze sectie ziet u enkele voorbeelden van hoe u gegevens weergeven in Application Insights. Als u al bekend met Application Insights bent, gaat u rechtstreeks naar [in de secties over het configureren en aanpassen van de telemetriegegevens](#configure-categories-and-log-levels).

![Tabblad van Application Insights-overzicht](media/functions-monitoring/metrics-explorer.png)

De volgende gebieden van Application Insights kunnen nuttig zijn bij het evalueren van het gedrag, prestaties en fouten in uw functies:

| tabblad | Description |
| ---- | ----------- |
| **[fouten](../azure-monitor/app/asp-net-exceptions.md)** |  Maak grafieken en waarschuwingen op basis van functie fouten en uitzonderingen voor servers. De **bewerkingsnaam** is de naam van de functie. Fouten in de afhankelijkheden worden niet weergegeven, tenzij u aangepaste telemetrie voor afhankelijkheden implementeren. |
| **[Performance](../azure-monitor/app/performance-counters.md)** | Prestatieproblemen analyseren. |
| **Servers** | Gebruik van resources en de doorvoer per server weergeven. Deze gegevens is handig voor het opsporen van fouten in scenario's waarbij functies worden bogging uw onderliggende resources. Servers worden aangeduid als **rolinstanties in de Cloud**. |
| **[Metrische gegevens](../azure-monitor/app/metrics-explorer.md)** | Maak grafieken en waarschuwingen die zijn gebaseerd op de metrische gegevens. Metrische gegevens behoren het aantal functieaanroepen, uitvoeringstijd en succespercentages. |
| **[Live Metrics Stream](../azure-monitor/app/live-stream.md)** | Metrische gegevens weergeven zoals deze gemaakt in realtime. |

## <a name="query-telemetry-data"></a>Telemetriegegevens op te vragen

[Application Insights Analytics](../azure-monitor/app/analytics.md) krijgt u toegang tot alle telemetriegegevens in de vorm van tabellen in een database. Analytics biedt een querytaal voor uitpakken, bewerken en de gegevens te visualiseren.

![Selecteer Analytics](media/functions-monitoring/select-analytics.png)

![Voorbeeld van Analytics](media/functions-monitoring/analytics-traces.png)

Hier volgt een voorbeeld waarin de distributie van aanvragen per worker in de afgelopen 30 minuten.

```
requests
| where timestamp > ago(30m) 
| summarize count() by cloud_RoleInstance, bin(timestamp, 1m)
| render timechart
```

De tabellen die beschikbaar zijn worden weergegeven in de **Schema** tabblad aan de linkerkant. Hier vindt u gegevens die zijn gegenereerd door de functieaanroepen in de volgende tabellen:

| Tabel | Description |
| ----- | ----------- |
| **traceringen** | Logboeken die zijn gemaakt door de runtime en functiecode aan te geven. |
| **Aanvragen** | Een aanvraag voor elke functieaanroep. |
| **exceptions** | Alle uitzonderingen die door de runtime. |
| **customMetrics** | Het aantal geslaagde en mislukte aanroepen, slagingspercentage en duur. |
| **customEvents** | Gebeurtenissen bijgehouden door de runtime, bijvoorbeeld: HTTP-aanvragen die een functie geactiveerd. |
| **performanceCounters** | Informatie over de prestaties van de servers die de functies worden uitgevoerd op. |

De andere tabellen zijn voor beschikbaarheidstests en client- en browserbeheer telemetrie. U kunt aangepaste telemetrie om toe te voegen gegevens om ze te implementeren.

Binnen elke tabel die een deel van de Functions-specifieke gegevens is in een `customDimensions` veld.  Bijvoorbeeld, de volgende query haalt alle traceringen waarvoor logboek-niveau `Error`.

```
traces 
| where customDimensions.LogLevel == "Error"
```

De runtime biedt de `customDimensions.LogLevel` en `customDimensions.Category` velden. U kunt opgeven dat aanvullende velden in de logboeken die u in uw functiecode aan te geven. Zie [gestructureerde logboekregistratie](#structured-logging) verderop in dit artikel.

## <a name="configure-categories-and-log-levels"></a>Configureren van de categorieën en meld u niveaus

U kunt Application Insights gebruiken zonder aangepaste configuratie. De standaardconfiguratie kan resulteren in grote hoeveelheden gegevens. Als u een Azure van Visual Studio-abonnement, kunt u mogelijk uw datalimiet bereikt voor Application Insights. Verderop in dit artikel leert u hoe u voor het configureren en aanpassen van de gegevens die uw functies naar Application Insights verzenden. Voor een functie-app logboekregistratie is geconfigureerd in de [host.json] bestand.

### <a name="categories"></a>Categories

De logger Azure Functions bevat een *categorie* voor elk logboek. De categorie geeft aan welk deel van de runtimecode of uw functiecode aan te geven het logboek geschreven. 

De Functions-runtime maakt u zich aanmeldt met een categorie die beginnen met 'Host'. De '-functie aan de slag,""functie uitgevoerd' en ' voltooid ' functielogboeken hebben de categorie "Host.Executor." 

Als u Logboeken in uw functiecode aan te geven schrijven, is de categorie "Functie."

### <a name="log-levels"></a>Logboekniveaus

Het Azure Functions-logboek bevat ook een *melden niveau* met elk logboek. [LogLevel](/dotnet/api/microsoft.extensions.logging.loglevel) een opsomming, en de code geheel getal geeft aan dat het relatieve belang:

|LogLevel    |Code|
|------------|---|
|Tracering       | 0 |
|fouten opsporen       | 1 |
|Informatie | 2 |
|Waarschuwing     | 3 |
|Fout       | 4 |
|Kritiek    | 5 |
|Geen        | 6 |

Meld u niveau `None` in de volgende sectie wordt uitgelegd. 

### <a name="log-configuration-in-hostjson"></a>Logboekconfiguratie van het in host.json

De [host.json] bestand configureert hoeveel logboekregistratie een functie-app wordt verzonden naar Application Insights. Voor elke categorie, geeft u aan het minimale logboek-niveau te verzenden. Er zijn twee voorbeelden: het eerste voorbeeld-doelen de [runtime van Functions versie 2.x](functions-versions.md#version-2x) (.NET Core) en het tweede voorbeeld is voor de versie 1.x-runtime.

### <a name="version-2x"></a>Versie 2.x

De runtime v2.x gebruikt de [.NET Core logboekregistratie filterhiërarchie](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#log-filtering). 

```json
{
  "logging": {
    "fileLoggingMode": "always",
    "logLevel": {
      "default": "Information",
      "Host.Results": "Error",
      "Function": "Error",
      "Host.Aggregator": "Trace"
    }
  }
}
```

### <a name="version-1x"></a>Versie 1.x

```json
{
  "logger": {
    "categoryFilter": {
      "defaultLevel": "Information",
      "categoryLevels": {
        "Host.Results": "Error",
        "Function": "Error",
        "Host.Aggregator": "Trace"
      }
    }
  }
}
```

In dit voorbeeld stelt u de volgende regels:

* Voor logboeken met categorie `Host.Results` of `Function`, alleen verzenden `Error` niveau en van boven naar Application Insights. De logboeken voor `Warning` niveau en hieronder worden genegeerd.
* Voor de logboeken met categorie `Host.Aggregator`, alle logboeken verzenden naar Application Insights. De `Trace` logboek-niveau is hetzelfde als wat u kunt sommige noemen `Verbose`, maar gebruik `Trace` in de [host.json] bestand.
* Voor alle andere logboeken verzenden alleen `Information` niveau en van boven naar Application Insights.

De categoriewaarde in [host.json] bepaalt de logboekregistratie voor alle categorieën die met dezelfde waarde beginnen. `Host` in [host.json] besturingselementen voor logboekregistratie `Host.General`, `Host.Executor`, `Host.Results`, enzovoort.

Als [host.json] omvat meerdere categorieën die met de dezelfde tekenreeks beginnen langer die eerst worden vergeleken. Stel dat u wilt dat alles, van de runtime, behalve `Host.Aggregator` om aan te melden bij `Error` niveau, maar u wilt dat `Host.Aggregator` om aan te melden bij de `Information` niveau:

### <a name="version-2x"></a>Versie 2.x 

```json
{
  "logging": {
    "fileLoggingMode": "always",
    "logLevel": {
      "default": "Information",
      "Host": "Error",
      "Function": "Error",
      "Host.Aggregator": "Information"
    }
  }
}
```

### <a name="version-1x"></a>Versie 1.x 

```json
{
  "logger": {
    "categoryFilter": {
      "defaultLevel": "Information",
      "categoryLevels": {
        "Host": "Error",
        "Function": "Error",
        "Host.Aggregator": "Information"
      }
    }
  }
}
```

Als u wilt onderdrukken alle logboeken voor een categorie, kunt u logboek-niveau `None`. Er zijn geen logboeken worden geschreven in die categorie en er is geen logboek-niveau erboven.

De volgende secties worden de belangrijkste categorieën van logboeken die de runtime wordt gemaakt. 

### <a name="category-hostresults"></a>Categorie Host.Results

Deze logboeken weergegeven als 'requests' in Application Insights. Ze geven aan slagen of mislukken van een functie.

![Grafiek van aanvragen](media/functions-monitoring/requests-chart.png)

Al deze logboeken worden geschreven op `Information` niveau. Als u filtert op `Warning` of hoger, ziet u niet een van deze gegevens.

### <a name="category-hostaggregator"></a>Categorie Host.Aggregator

Deze logboeken bieden aantallen en gemiddelde van de functieaanroepen via een [configureerbare](#configure-the-aggregator) periode tijd wordt opgelost. De standaardperiode is 30 seconden of 1000 resultaten, afhankelijk van wat het eerste komt. 

De logboeken zijn beschikbaar in de **customMetrics** tabel in Application Insights. Voorbeelden zijn het aantal uitvoeringen, slagingspercentage en duur.

![customMetrics query](media/functions-monitoring/custom-metrics-query.png)

Al deze logboeken worden geschreven op `Information` niveau. Als u filtert op `Warning` of hoger, ziet u niet een van deze gegevens.

### <a name="other-categories"></a>Andere categorieën

Alle logboeken voor categorieën dan degene die al zijn beschikbaar in de **traceringen** tabel in Application Insights.

![traceringen query](media/functions-monitoring/analytics-traces.png)

Alle logboeken met categorieën die met beginnen `Host` worden geschreven door de Functions-runtime. De 'Functie gestart' en ' voltooid ' functielogboeken categorie hebben `Host.Executor`. Voor geslaagde uitvoeringen, deze logboeken worden `Information` niveau. Uitzonderingen zijn vastgelegd bij `Error` niveau. De runtime maakt ook `Warning` niveau zich aanmeldt, bijvoorbeeld: verzonden naar de wachtrij voor beheer van Onverwerkbare berichten in de wachtrij.

Logboeken geschreven door uw functiecode categorie hebben `Function` en mag logboek-niveau.

## <a name="configure-the-aggregator"></a>De aggregator configureren

Als in de vorige sectie hebt genoteerd, combineert de runtime gegevens over de functie-uitvoeringen gedurende een bepaalde periode. De standaardperiode is 30 seconden of 1000 wordt uitgevoerd, afhankelijk van wat het eerste komt. U kunt deze instelling in configureren de [host.json] bestand.  Hier volgt een voorbeeld:

```json
{
    "aggregator": {
      "batchSize": 1000,
      "flushTimeout": "00:00:30"
    }
}
```

## <a name="configure-sampling"></a>Steekproeven configureren

Application Insights is een [steekproeven](../azure-monitor/app/sampling.md) functie die u beschermen kunt tegen te veel telemetriegegevens produceren op voltooid uitvoeringen soms van piekbelasting. Wanneer het aantal uitvoeringen door een binnenkomende een opgegeven drempelwaarde overschrijdt, wordt de Application Insights willekeurig negeren enkele van de binnenkomende uitvoeringen. De standaardinstelling voor het maximum aantal uitvoeringen per seconde is 20 (vijf in versie 1.x). U kunt configureren van lijnen in [host.json].  Hier volgt een voorbeeld:

### <a name="version-2x"></a>Versie 2.x 

```json
{
  "logging": {
    "applicationInsights": {
      "samplingSettings": {
        "isEnabled": true,
        "maxTelemetryItemsPerSecond" : 20
      }
    }
  }
}
```

### <a name="version-1x"></a>Versie 1.x 

```json
{
  "applicationInsights": {
    "sampling": {
      "isEnabled": true,
      "maxTelemetryItemsPerSecond" : 5
    }
  }
}
```

> [!NOTE]
> [Sampling](../azure-monitor/app/sampling.md) is standaard ingeschakeld. Als u blijken te ontbreken gegevens, moet u mogelijk de instellingen sampling voor uw specifieke scenario bewaking aanpassen.

## <a name="write-logs-in-c-functions"></a>Schrijven Logboeken in C#-functies

U kunt Logboeken in uw functiecode aan te geven die worden weergegeven als traceringen in Application Insights schrijven.

### <a name="ilogger"></a>ILogger

Gebruik een [ILogger](https://docs.microsoft.com/dotnet/api/microsoft.extensions.logging.ilogger) parameter in uw functies in plaats van een `TraceWriter` parameter. Logboeken die zijn gemaakt met behulp van `TraceWriter` Ga naar Application Insights, maar `ILogger` kunt u doen [gestructureerde logboekregistratie](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).

Met een `ILogger` -object, u kunt aanroepen `Log<level>` [uitbreidingsmethoden op ILogger](https://docs.microsoft.com/dotnet/api/microsoft.extensions.logging.loggerextensions#methods) logboeken maken. De volgende code schrijft `Information` logboeken met categorie "-functie."

```cs
public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, ILogger logger)
{
    logger.LogInformation("Request for item with key={itemKey}.", id);
```

### <a name="structured-logging"></a>Gestructureerde logboekregistratie

De volgorde van de tijdelijke aanduidingen, niet de namen, bepaalt welke parameters worden gebruikt in het logboekbericht. Stel dat u hebt de volgende code:

```csharp
string partitionKey = "partitionKey";
string rowKey = "rowKey";
logger.LogInformation("partitionKey={partitionKey}, rowKey={rowKey}", partitionKey, rowKey);
```

Als u de dezelfde tekenreeks behouden en de volgorde van de parameters, zijn de tekst van het resulterende waarden in op de verkeerde plaatsen.

Tijdelijke aanduidingen worden op deze manier afgehandeld, zodat u dit gestructureerde logboekregistratie doen kunt. Application Insights worden opgeslagen voor de parameter naam / waarde-paren en de bericht-tekenreeks. Het resultaat is dat de Berichtargumenten velden die u kunt een query worden op.

Als de aanroep van de methode logger er als in het vorige voorbeeld uitziet, kunt u een query het veld `customDimensions.prop__rowKey`. De `prop__` voorvoegsel wordt toegevoegd om te controleren of er zijn geen conflicten tussen de velden de runtime wordt toegevoegd en uw functiecode aan te geven velden wordt toegevoegd.

U kunt ook een query op de tekenreeks voor het oorspronkelijke bericht door te verwijzen naar het veld `customDimensions.prop__{OriginalFormat}`.  

Hier volgt een voorbeeld-JSON-weergave van `customDimensions` gegevens:

```json
{
  customDimensions: {
    "prop__{OriginalFormat}":"C# Queue trigger function processed: {message}",
    "Category":"Function",
    "LogLevel":"Information",
    "prop__message":"c9519cbf-b1e6-4b9b-bf24-cb7d10b1bb89"
  }
}
```

### <a name="custom-metrics-logging"></a>Aangepaste metrische gegevens vastleggen

In C#-script-functies, kunt u de `LogMetric` uitbreidingsmethode op `ILogger` te maken van aangepaste metrische gegevens in Application Insights. Hier volgt een voorbeeld-methodeaanroep:

```csharp
logger.LogMetric("TestMetric", 1234);
```

Deze code is een alternatief voor aanroepen `TrackMetric` met behulp van de Application Insights-API voor .NET.

## <a name="write-logs-in-javascript-functions"></a>Schrijven Logboeken in JavaScript-functies

Gebruik in Node.js-functies, `context.log` om te schrijven Logboeken. Gestructureerde logboekregistratie is niet ingeschakeld.

```
context.log('JavaScript HTTP trigger function processed a request.' + context.invocationId);
```

### <a name="custom-metrics-logging"></a>Aangepaste metrische gegevens vastleggen

Wanneer u uitvoert op [versie 1.x](functions-versions.md#creating-1x-apps) van de Functions-runtime, Node.js-functies kunnen gebruiken de `context.log.metric` methode voor het maken van aangepaste metrische gegevens in Application Insights. Deze methode wordt momenteel niet ondersteund in versie 2.x. Hier volgt een voorbeeld-methodeaanroep:

```javascript
context.log.metric("TestMetric", 1234);
```

Deze code is een alternatief voor aanroepen `trackMetric` met behulp van de Node.js-SDK voor Application Insights.

## <a name="log-custom-telemetry-in-c-functions"></a>Meld u aangepaste telemetrie C# functies

U kunt de [Microsoft.ApplicationInsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) NuGet-pakket aan aangepaste telemetrische gegevens verzenden naar Application Insights. De volgende C# voorbeeld wordt de [aangepaste telemetrie-API](../azure-monitor/app/api-custom-events-metrics.md). Het voorbeeld is voor een .NET-klassebibliotheek, maar de Application Insights-code is hetzelfde voor C#-script.

### <a name="version-2x"></a>Versie 2.x

De runtime versie 2.x maakt gebruik van nieuwe functies in Application Insights automatisch telemetrie te correleren met de huidige bewerking. Hoeft niet handmatig in te stellen de bewerking `Id`, `ParentId`, of `Name` velden.

```cs
using System;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.DataContracts;
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Extensions.Logging;

namespace functionapp0915
{
    public class HttpTrigger2
    {
        private readonly TelemetryClient telemetryClient;

        /// Using dependency injection will guarantee that you use the same configuration for telemetry collected automatically and manually.
        public HttpTrigger2(TelemetryConfiguration telemetryConfiguration)
        {
            this.telemetryClient = new TelemetryClient(telemetryConfiguration);
        }

        [FunctionName("HttpTrigger2")]
        public Task<IActionResult> Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", Route = null)]
            HttpRequest req, ExecutionContext context, ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");
            DateTime start = DateTime.UtcNow;

            // Parse query parameter
            string name = req.Query
                .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
                .Value;

            // Track an Event
            var evt = new EventTelemetry("Function called");
            evt.Context.User.Id = name;
            this.telemetryClient.TrackEvent(evt);

            // Track a Metric
            var metric = new MetricTelemetry("Test Metric", DateTime.Now.Millisecond);
            metric.Context.User.Id = name;
            this.telemetryClient.TrackMetric(metric);

            // Track a Dependency
            var dependency = new DependencyTelemetry
            {
                Name = "GET api/planets/1/",
                Target = "swapi.co",
                Data = "https://swapi.co/api/planets/1/",
                Timestamp = start,
                Duration = DateTime.UtcNow - start,
                Success = true
            };
            dependency.Context.User.Id = name;
            this.telemetryClient.TrackDependency(dependency);

            return Task.FromResult<IActionResult>(new OkResult());
        }
    }
}
```

### <a name="version-1x"></a>Versie 1.x

```cs
using System;
using System.Net;
using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.DataContracts;
using Microsoft.ApplicationInsights.Extensibility;
using Microsoft.Azure.WebJobs;
using System.Net.Http;
using System.Threading.Tasks;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Extensions.Logging;
using System.Linq;

namespace functionapp0915
{
    public static class HttpTrigger2
    {
        private static string key = TelemetryConfiguration.Active.InstrumentationKey = 
            System.Environment.GetEnvironmentVariable(
                "APPINSIGHTS_INSTRUMENTATIONKEY", EnvironmentVariableTarget.Process);

        private static TelemetryClient telemetryClient = 
            new TelemetryClient() { InstrumentationKey = key };

        [FunctionName("HttpTrigger2")]
        public static async Task<HttpResponseMessage> Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]
            HttpRequestMessage req, ExecutionContext context, ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");
            DateTime start = DateTime.UtcNow;

            // Parse query parameter
            string name = req.GetQueryNameValuePairs()
                .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
                .Value;

            // Get request body
            dynamic data = await req.Content.ReadAsAsync<object>();

            // Set name to query string or body data
            name = name ?? data?.name;
         
            // Track an Event
            var evt = new EventTelemetry("Function called");
            UpdateTelemetryContext(evt.Context, context, name);
            telemetryClient.TrackEvent(evt);
            
            // Track a Metric
            var metric = new MetricTelemetry("Test Metric", DateTime.Now.Millisecond);
            UpdateTelemetryContext(metric.Context, context, name);
            telemetryClient.TrackMetric(metric);
            
            // Track a Dependency
            var dependency = new DependencyTelemetry
                {
                    Name = "GET api/planets/1/",
                    Target = "swapi.co",
                    Data = "https://swapi.co/api/planets/1/",
                    Timestamp = start,
                    Duration = DateTime.UtcNow - start,
                    Success = true
                };
            UpdateTelemetryContext(dependency.Context, context, name);
            telemetryClient.TrackDependency(dependency);
        }
        
        // Correlate all telemetry with the current Function invocation
        private static void UpdateTelemetryContext(TelemetryContext context, ExecutionContext functionContext, string userName)
        {
            context.Operation.Id = functionContext.InvocationId.ToString();
            context.Operation.ParentId = functionContext.InvocationId.ToString();
            context.Operation.Name = functionContext.FunctionName;
            context.User.Id = userName;
        }
    }    
}
```

Remove() niet aanroepen `TrackRequest` of `StartOperation<RequestTelemetry>` omdat u dubbele aanvragen voor een functie-aanroep ziet.  Aanvragen automatisch worden bijgehouden in de Functions-runtime.

Stel `telemetryClient.Context.Operation.Id`. Deze algemene instelling zorgt ervoor dat onjuist correlatie wanneer er veel functies gelijktijdig worden uitgevoerd. In plaats daarvan, maak een nieuw exemplaar van de telemetrie (`DependencyTelemetry`, `EventTelemetry`) en wijzig de `Context` eigenschap. Geeft u in het exemplaar van de telemetrie naar de bijbehorende `Track` methode voor `TelemetryClient` (`TrackDependency()`, `TrackEvent()`). Deze methode zorgt ervoor dat de telemetrische gegevens de juiste correlatiedetails voor de huidige functie-aanroep is.

## <a name="log-custom-telemetry-in-javascript-functions"></a>Aangepaste logboektelemetrie in JavaScript-functies

De [Application Insights Node.js SDK](https://www.npmjs.com/package/applicationinsights) is momenteel in de bètafase. Hier volgt een voorbeeld van code waarmee aangepaste telemetrie wordt verzonden naar Application Insights:

```javascript
const appInsights = require("applicationinsights");
appInsights.setup();
const client = appInsights.defaultClient;

module.exports = function (context, req) {
    context.log('JavaScript HTTP trigger function processed a request.');

    client.trackEvent({name: "my custom event", tagOverrides:{"ai.operation.id": context.invocationId}, properties: {customProperty2: "custom property value"}});
    client.trackException({exception: new Error("handled exceptions can be logged with this method"), tagOverrides:{"ai.operation.id": context.invocationId}});
    client.trackMetric({name: "custom metric", value: 3, tagOverrides:{"ai.operation.id": context.invocationId}});
    client.trackTrace({message: "trace message", tagOverrides:{"ai.operation.id": context.invocationId}});
    client.trackDependency({target:"http://dbname", name:"select customers proc", data:"SELECT * FROM Customers", duration:231, resultCode:0, success: true, dependencyTypeName: "ZSQL", tagOverrides:{"ai.operation.id": context.invocationId}});
    client.trackRequest({name:"GET /customers", url:"http://myserver/customers", duration:309, resultCode:200, success:true, tagOverrides:{"ai.operation.id": context.invocationId}});

    context.done();
};
```

De `tagOverrides` parametersets de `operation_Id` aan van de functie aanroep-ID. Deze instelling kunt u correlaties zichtbaar maken tussen alle van de automatisch gegenereerde en aangepaste telemetrie voor een bepaalde functie-aanroep.

## <a name="dependencies"></a>Afhankelijkheden

Functies v2 verzamelt automatisch afhankelijkheden voor HTTP-aanvragen, service bus en SQL.

U kunt aangepaste code om de afhankelijkheden weer te schrijven. Zie voor voorbeelden van de voorbeeldcode in de [ C# aangepaste telemetrie sectie](#log-custom-telemetry-in-c-functions). De voorbeeldcode resulteert in een *toepassingsoverzicht* in Application Insights die vergelijkbaar is de volgende afbeelding:

![Overzicht van de toepassing](./media/functions-monitoring/app-map.png)

## <a name="report-issues"></a>Problemen rapporteren

Om een probleem met Application Insights-integratie in Functions te melden of een suggestie of de aanvraag, [maken van een probleem in GitHub](https://github.com/Azure/Azure-Functions/issues/new).

## <a name="streaming-logs"></a>Streaminglogboeken

Tijdens het ontwikkelen van een toepassing, is het vaak nuttig om te zien van logboekgegevens in near-real-time. Hier vindt u een stream van logboekbestanden wordt gegenereerd door uw functies in Azure portal of in een opdrachtregel-sessie op de lokale computer.

Dit is gelijk aan de uitvoer weergegeven wanneer u uw functies tijdens de foutopsporing [lokale ontwikkeling](functions-develop-local.md). Zie voor meer informatie, [over het streamen van logboeken](../app-service/troubleshoot-diagnostic-logs.md#streamlogs).

> [!NOTE]
> Streaminglogboeken ondersteunen slechts één exemplaar van de host functies. Wanneer uw functie wordt geschaald naar meerdere exemplaren, worden gegevens van andere exemplaren niet weergegeven in het logboekstream. De [Live Metrics Stream](../azure-monitor/app/live-stream.md) in Application Insights meerdere exemplaren ondersteund. Terwijl u ook in bijna real-time, Stream analytics zijn ook gebaseerd op [voorbeeldgegevens](#configure-sampling).

### <a name="portal"></a>Portal

Als u wilt streaminglogboeken bekijken in de portal, selecteert u de **platformfuncties** tabblad in uw functie-app. Klik vervolgens onder **bewaking**, kiest u **logboekstreaming**.

![Streaming logboekbestanden in de portal inschakelen](./media/functions-monitoring/enable-streaming-logs-portal.png)

Hierdoor wordt uw app verbonden met het logboek voor de service en toepassingslogboeken worden weergegeven in het venster. U kunt schakelen tussen **toepassingslogboeken** en **Web server-logboeken**.  

![Streaminglogboeken bekijken in de portal](./media/functions-monitoring/streaming-logs-window.png)

### <a name="azure-cli"></a>Azure-CLI

U kunt streaminglogboeken inschakelen met behulp van de [Azure CLI](/cli/azure/install-azure-cli). Gebruik de volgende opdrachten als u zich aanmeldt, kiest u uw abonnement en de stream-logboekbestanden:

```azurecli
az login
az account list
az account set --subscription <subscriptionNameOrId>
az webapp log tail --resource-group <RESOURCE_GROUP_NAME> --name <FUNCTION_APP_NAME>
```

### <a name="azure-powershell"></a>Azure PowerShell

U kunt streaminglogboeken inschakelen met behulp van [Azure PowerShell](/powershell/azure/overview). Gebruik de volgende opdrachten naar uw Azure-account toevoegen, kiest u uw abonnement en de stream-logboekbestanden voor PowerShell:

```powershell
Add-AzAccount
Get-AzSubscription
Get-AzSubscription -SubscriptionName "<subscription name>" | Select-AzSubscription
Get-AzWebSiteLog -Name <FUNCTION_APP_NAME> -Tail
```

## <a name="disable-built-in-logging"></a>Ingebouwde logboekregistratie uitschakelen

Als u Application Insights inschakelt, schakelt u de ingebouwde logboekregistratie die gebruikmaakt van Azure Storage. De ingebouwde logboekregistratie is handig voor het testen met lichte workloads, maar is niet bedoeld voor gebruik in productieomgevingen hoge belasting. Voor het bewaken van de productie, wordt aangeraden Application Insights. Als ingebouwde logboekregistratie in productie wordt gebruikt, de record logboekregistratie mogelijk onvolledig vanwege beperkingen in Azure Storage.

Als u wilt ingebouwde logboekregistratie uitschakelen, verwijdert de `AzureWebJobsDashboard` app-instelling. Zie voor meer informatie over het verwijderen van app-instellingen in de Azure-portal, de **toepassingsinstellingen** sectie van [over het beheren van een functie-app](functions-how-to-use-azure-function-app-settings.md#settings). Voordat u de app-instelling verwijderen, zorg ervoor dat er zijn geen bestaande functies in dezelfde functie-app gebruikt de instelling voor Azure Storage-triggers of bindingen.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende bronnen voor meer informatie:

* [Application Insights](/azure/application-insights/)
* [ASP.NET Core-logboekregistratie](/aspnet/core/fundamentals/logging/)

[host.json]: functions-host-json.md
