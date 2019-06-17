---
title: Overzicht van de toepassing in Azure Application Insights | Microsoft Docs
description: Topologieën voor complexe toepassingen met het overzicht van de toepassing bewaken
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 3bf37fe9-70d7-4229-98d6-4f624d256c36
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 03/15/2019
ms.reviewer: sdash
ms.author: mbullwin
ms.openlocfilehash: 70d1f54aed5e83801b1d1e249d7a412dd6d9a49a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65964033"
---
# <a name="application-map-triage-distributed-applications"></a>Overzicht van de toepassing: Sorteren van gedistribueerde toepassingen

Overzicht van de toepassing kunt u spot knelpunten of fout hotspots voor alle onderdelen van de gedistribueerde toepassing. Elk knooppunt op de kaart vertegenwoordigt een toepassingsonderdeel of de bijbehorende afhankelijkheden; en health KPI heeft en de status van waarschuwingen. U kunt de via elk onderdeel klikken voor meer gedetailleerde diagnostische gegevens, zoals Application Insights-gebeurtenissen. Als uw app gebruikmaakt van Azure-services, kunt u ook verder klikken naar Azure diagnostics, zoals SQL Database Advisor-aanbevelingen.

## <a name="what-is-a-component"></a>Wat is een onderdeel?

Onderdelen zijn onafhankelijk implementeerbare onderdelen van uw toepassing gedistribueerd/microservices. Ontwikkelaars en uitvoerende teams hebben zichtbaarheid van de code op serverniveau of toegang tot telemetrie die is gegenereerd door de onderdelen van deze toepassing. 

* Onderdelen zijn anders dan 'waargenomen' externe afhankelijkheden, zoals SQL, EventHub enz. die uw team/organisatie mogelijk geen toegang tot (code of telemetrie).
* Onderdelen worden uitgevoerd op een willekeurig aantal exemplaren van de rol-server-container.
* Onderdelen kunnen worden afzonderlijke Application Insights-instrumentatiesleutels (zelfs als abonnementen verschillen) of verschillende rollen die rapporteren aan een enkele Application Insights-instrumentatiesleutel. De ervaring van de kaart voorbeeld ziet u de onderdelen onafhankelijk van de manier waarop ze zijn ingesteld.

## <a name="composite-application-map"></a>Samengestelde toepassingstoewijzing

U ziet de volledige toepassing-topologie voor meerdere niveaus van gerelateerde toepassingsonderdelen. Onderdelen mogelijk op verschillende Application Insights-resources of verschillende rollen in een enkele resource. Het toepassingsoverzicht vindt onderdelen door de volgende HTTP-afhankelijkheidsaanroepen tussen servers met de Application Insights-SDK geïnstalleerd. 

Deze ervaring wordt gestart met progressieve detectie van de onderdelen. Wanneer u eerst het toepassingsoverzicht laadt, wordt een set van query's voor het detecteren van de onderdelen die betrekking hebben op dit onderdeel geactiveerd. Een knop in de linkerbovenhoek wordt bijgewerkt met het aantal onderdelen in uw toepassing zodra ze worden gedetecteerd. 

Op 'Toewijzingsonderdelen bijwerken' klikt, wordt de kaart met alle onderdelen die zijn gedetecteerd tot vernieuwd. Dit kan een minuut laden duren afhankelijk van de complexiteit van uw toepassing.

Deze detectiestap is niet vereist als alle onderdelen van rollen binnen één Application Insights-resource. Het initiële laden voor dergelijke toepassing hebben alle onderdelen daarvan.

![Schermafbeelding van de toepassing-kaart](media/app-map/app-map-001.png)

Een van de belangrijkste doelstellingen met deze ervaring is het kunnen complexe topologieën met honderden onderdelen te visualiseren.

Klik op een onderdeel om te zien van verwante inzichten en gaat u naar de prestaties en de fout sorteren ervaring voor het betreffende onderdeel.

![Flyout](media/app-map/application-map-002.png)

### <a name="investigate-failures"></a>Mislukte pogingen onderzoeken

Selecteer **mislukte pogingen onderzoeken** om te starten van het deelvenster met fouten.

![Schermafbeelding van de knop fouten onderzoeken](media/app-map/investigate-failures.png)

![Schermafbeelding van de ervaring van fouten](media/app-map/failures.png)

### <a name="investigate-performance"></a>Prestaties onderzoeken

Selecteer voor het oplossen van problemen met prestaties, **prestaties onderzoeken**.

![Schermafdruk van knop prestaties onderzoeken](media/app-map/investigate-performance.png)

![Schermafbeelding van prestaties](media/app-map/performance.png)

### <a name="go-to-details"></a>Ga naar details

Selecteer **Ga naar details** verkennen van de transactie voor end-to-end-ervaring, weergaven op het niveau van call stack kan bieden.

![Schermafbeelding van de knop Ga voor meer informatie](media/app-map/go-to-details.png)

![Schermafbeelding van end-to-end-transactiedetails](media/app-map/end-to-end-transaction.png)

### <a name="view-in-analytics"></a>Weergeven in Analytics

Als u wilt opvragen en de gegevens van uw toepassingen verder te onderzoeken, klikt u op **weergeven in analytics**.

![Schermafbeelding van de weergave in de knop analytics](media/app-map/view-in-analytics.png)

![Schermafbeelding van de analytics-ervaring](media/app-map/analytics.png)

### <a name="alerts"></a>Waarschuwingen

Als u wilt weergeven van actieve waarschuwingen en de onderliggende regels die ervoor zorgen dat de waarschuwingen worden geactiveerd, selecteer **waarschuwingen**.

![Schermafbeelding van de knop waarschuwingen](media/app-map/alerts.png)

![Schermafbeelding van de analytics-ervaring](media/app-map/alerts-view.png)

## <a name="set-cloud-role-name"></a>Set-cloudrolnaam

Overzicht van de toepassing maakt gebruik van de **cloudrolnaam** eigenschap om de onderdelen op de kaart te identificeren. De Application Insights-SDK wordt automatisch de naameigenschap van de cloud-rol toegevoegd aan de telemetrie die is verzonden door onderdelen. Bijvoorbeeld, wordt de SDK een naam van website of naam van de rol service toevoegen aan de naameigenschap van de cloud-rol. Er zijn echter gevallen waar kunt u de standaardwaarde overschrijven. Cloudrolnaam overschrijven en wat wordt weergegeven op de kaart van de toepassing wijzigen:

### <a name="netnet-core"></a>.NET/.NET Core

**Schrijf aangepaste TelemetryInitializer zoals hieronder.**

```csharp
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.Extensibility;

namespace CustomInitializer.Telemetry
{
    public class MyTelemetryInitializer : ITelemetryInitializer
    {
        public void Initialize(ITelemetry telemetry)
        {
            if (string.IsNullOrEmpty(telemetry.Context.Cloud.RoleName))
            {
                //set custom role name here
                telemetry.Context.Cloud.RoleName = "Custom RoleName";
                telemetry.Context.Cloud.RoleInstance = "Custom RoleInstance";
            }
        }
    }
}
```

**Initialisatieprogramma, zodat de actieve TelemetryConfiguration laden**

In ApplicationInsights.config:

```xml
    <ApplicationInsights>
      <TelemetryInitializers>
        <!-- Fully qualified type name, assembly name: -->
        <Add Type="CustomInitializer.Telemetry.MyTelemetryInitializer, CustomInitializer"/>
        ...
      </TelemetryInitializers>
    </ApplicationInsights>
```

> [!NOTE]
> Toe te voegen initialisatiefunctie met behulp van `ApplicationInsights.config` is niet geldig voor ASP.NET Core-toepassingen.

Een alternatieve methode voor ASP.NET-Web-apps is het instantiëren van de initialisatiefunctie in code, bijvoorbeeld in Global.aspx.cs:

```csharp
 using Microsoft.ApplicationInsights.Extensibility;
 using CustomInitializer.Telemetry;

    protected void Application_Start()
    {
        // ...
        TelemetryConfiguration.Active.TelemetryInitializers.Add(new MyTelemetryInitializer());
    }
```

Voor [ASP.NET Core](asp-net-core.md#adding-telemetryinitializers) toepassingen toe te voegen een nieuwe `TelemetryInitializer` wordt uitgevoerd door toe te voegen aan de container Afhankelijkheidsinjectie, zoals hieronder wordt weergegeven. Dit wordt gedaan `ConfigureServices` -methode van uw `Startup.cs` klasse.

```csharp
 using Microsoft.ApplicationInsights.Extensibility;
 using CustomInitializer.Telemetry;
 public void ConfigureServices(IServiceCollection services)
    {
        services.AddSingleton<ITelemetryInitializer, MyCustomTelemetryInitializer>();
    }
```

### <a name="nodejs"></a>Node.js

```javascript
var appInsights = require("applicationinsights");
appInsights.setup('INSTRUMENTATION_KEY').start();
appInsights.defaultClient.context.tags["ai.cloud.role"] = "your role name";
appInsights.defaultClient.context.tags["ai.cloud.roleInstance"] = "your role instance";
```

### <a name="alternate-method-for-nodejs"></a>Alternatieve methode voor Node.js

```javascript
var appInsights = require("applicationinsights");
appInsights.setup('INSTRUMENTATION_KEY').start();

appInsights.defaultClient.addTelemetryProcessor(envelope => {
    envelope.tags["ai.cloud.role"] = "your role name";
    envelope.tags["ai.cloud.roleInstance"] = "your role instance"
});
```

### <a name="java"></a>Java

Als u Spring Boot aan de Application Insights Spring Boot starter gebruiken, wordt de enige vereiste wijziging is het instellen van uw aangepaste naam voor de toepassing in de application.properties-bestand.

`spring.application.name=<name-of-app>`

De Spring Boot starter wordt cloudrolnaam automatisch toegewezen aan de opgegeven waarde voor de eigenschap spring.application.name.

Voor meer informatie over de Java correlatie- en het configureren van cloudrol naam voor de afhandeling van niet-SpringBoot toepassingen [sectie](https://docs.microsoft.com/azure/application-insights/application-insights-correlation#role-name) op correlatie.

### <a name="clientbrowser-side-javascript"></a>Browser-client-side-JavaScript

```javascript
appInsights.queue.push(() => {
appInsights.context.addTelemetryInitializer((envelope) => {
  envelope.tags["ai.cloud.role"] = "your role name";
  envelope.tags["ai.cloud.roleInstance"] = "your role instance";
});
});
```

### <a name="understanding-cloud-role-name-within-the-context-of-the-application-map"></a>Cloudrolnaam inzicht in de context van het Toepassingsoverzicht

Zo denken over het **cloudrolnaam**, kan het handig om te kijken naar een toepassingstoewijzing met meerdere cloud functienamen aanwezig zijn:

![Schermafbeelding van de toepassing-kaart](media/app-map/cloud-rolename.png)

In het Toepassingsoverzicht boven elk van de namen in groen vakken zijn cloud rol waarden voor verschillende aspecten van deze bepaalde gedistribueerde toepassing. Dus voor deze app bijbehorende rollen bestaan uit: `Authentication`, `acmefrontend`, `Inventory Management`, een `Payment Processing Worker Role`. 

In het geval van deze app vertegenwoordigt elk van deze cloud-functienamen ook een andere unieke Application Insights-resource met hun eigen instrumentatiesleutels. Omdat de eigenaar van deze toepassing toegang tot elk van deze vier verschillende Application Insights-resources heeft, kan overzicht van de toepassing een overzicht van de onderliggende relaties samen te voegen.

Voor de [officiële definities](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/39a5ef23d834777eefdd72149de705a016eb06b0/Schema/PublicSchema/ContextTagKeys.bond#L93):

```
   [Description("Name of the role the application is a part of. Maps directly to the role name in azure.")]
    [MaxStringLength("256")]
    705: string      CloudRole = "ai.cloud.role";
    
    [Description("Name of the instance where the application is running. Computer name for on-premises, instance name for Azure.")]
    [MaxStringLength("256")]
    715: string      CloudRoleInstance = "ai.cloud.roleInstance";
```

U kunt ook **cloudrolinstantie** kan nuttig zijn voor scenario's waarbij **cloudrolnaam** leest u het probleem is ergens in uw web-front-end, maar u uw web-front-end via kan worden uitgevoerd meerdere servers met Netwerktaakverdeling dus is het kunnen inzoomen in een laag diepere via Kusto-query's en als het probleem is van invloed op alle web-front-servers/exemplaren of slechts een zeer belangrijk kan zijn.

Een scenario waarin u mogelijk wilt overschrijven van de waarde voor cloudrolinstantie kan zijn als uw app wordt uitgevoerd in een omgeving met beperkte waarbij alleen de afzonderlijke server weet mogelijk niet voldoende informatie te vinden van een bepaald probleem.

Zie voor meer informatie over het onderdrukken van de naameigenschap van de cloud-rol met telemetrie initializers, [eigenschappen toevoegen: ITelemetryInitializer](api-filtering-sampling.md#add-properties-itelemetryinitializer).

## <a name="troubleshooting"></a>Problemen oplossen

Als u problemen bij het ophalen van Toepassingsoverzicht ondervindt naar behoren werkt, probeert u deze stappen:

### <a name="general"></a>Algemeen

1. Zorg ervoor dat u een officieel ondersteunde SDK gebruikt. Niet-ondersteunde SDK's of community-SDK's bieden mogelijk geen ondersteuning voor correlatie.

    Verwijzen naar dit [artikel](https://docs.microsoft.com/azure/application-insights/app-insights-platforms) voor een lijst met ondersteunde SDK's.

2. Alle onderdelen van een upgrade uitvoert naar de nieuwste versie van de SDK.

3. Als u Azure Functions met C#, een upgrade uitvoeren naar [functies V2](https://docs.microsoft.com/azure/azure-functions/functions-versions).

4. Controleer of [cloudrolnaam](#set-cloud-role-name) correct is geconfigureerd.

5. Als er een afhankelijkheid ontbreekt, zorgt u ervoor dat deze in de lijst met [automatisch verzamelde afhankelijkheden](https://docs.microsoft.com/azure/application-insights/auto-collect-dependencies) staat. Als dat niet zo is, kunt u de afhankelijkheid nog steeds handmatig traceren met een [aanroep voor het traceren van de afhankelijkheid](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackdependency).

### <a name="too-many-nodes-on-the-map"></a>Te veel knooppunten op de kaart

Overzicht van de toepassing wordt een toepassing voor elke unieke cloudrolnaam die aanwezig zijn in de aanvraagtelemetrie van uw en de knooppunten van een afhankelijkheid voor elke unieke combinatie van type, doel en de cloudrolnaam in uw afhankelijkheidstelemetrie. Als er meer dan 10.000 knooppunten in uw telemetrie, pas overzicht van de toepassing weer voor het ophalen van de knooppunten en koppelingen, zodat de kaart wordt niet volledig. Als dit het geval is, wordt een waarschuwing weergegeven bij het weergeven van de kaart.

Overzicht van de toepassing ondersteunt bovendien alleen maximaal 1000 afzonderlijke niet-gegroepeerde knooppunten in één keer weergegeven. Overzicht van de toepassing vermindert de visuele complexiteit door het groeperen van afhankelijkheden met hetzelfde type en bellers, maar als uw telemetrie te veel unieke cloud role-namen of typen te veel afhankelijkheden heeft, deze groepering niet toereikend zijn en de kaart kan niet worden weergegeven.

U kunt dit verhelpen, moet u uw instrumentation om in te stellen juist de cloudrolnaam, afhankelijkheidstype en afhankelijkheid doelvelden wijzigen.

* Doel van afhankelijkheid moet de logische naam van een afhankelijkheid vertegenwoordigen. In veel gevallen is het equivalent zijn aan de server of de resourcenaam van de afhankelijkheid. Bijvoorbeeld, wordt deze in het geval van een HTTP-afhankelijkheden ook ingesteld op de hostnaam. Het mag geen unieke id's of van een aanvraag naar de andere wijzigen parameters bevatten.

* Afhankelijkheid van het type moet het logische type van een afhankelijkheid vertegenwoordigen. HTTP-, SQL of Azure Blob zijn bijvoorbeeld typen typische afhankelijkheden. Het mag geen unieke id's bevatten.

* Het doel van cloud-rolnaam wordt beschreven in de [boven de sectie](https://docs.microsoft.com/azure/azure-monitor/app/app-map#set-cloud-role-name).

## <a name="portal-feedback"></a>Portal feedback

Als u feedback wilt, gebruikt u de feedbackoptie.

![MapLink-1 image](./media/app-map/14-updated.png)

## <a name="next-steps"></a>Volgende stappen

* [Informatie over correlatie](https://docs.microsoft.com/azure/application-insights/application-insights-correlation)
