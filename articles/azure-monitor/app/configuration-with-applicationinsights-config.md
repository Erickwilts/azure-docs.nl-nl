---
title: Naslag informatie voor ApplicationInsights. config-Azure | Microsoft Docs
description: Schakel modules voor gegevens verzameling in of uit en voeg prestatie meter items en andere para meters toe.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 6e397752-c086-46e9-8648-a1196e8078c2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 05/22/2019
ms.reviewer: olegan
ms.author: mbullwin
ms.openlocfilehash: 382f43156ab450600ff0d2e5e2db763cd6bd94df
ms.sourcegitcommit: de47a27defce58b10ef998e8991a2294175d2098
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/15/2019
ms.locfileid: "67875052"
---
# <a name="configuring-the-application-insights-sdk-with-applicationinsightsconfig-or-xml"></a>De Application Insights-SDK configureren met ApplicationInsights.config of ApplicationInsights.xml
De Application Insights .NET SDK bestaat uit een aantal NuGet-pakketten. Het [kern pakket](https://www.nuget.org/packages/Microsoft.ApplicationInsights) biedt de API voor het verzenden van telemetrie naar de Application Insights. [Aanvullende pakketten](https://www.nuget.org/packages?q=Microsoft.ApplicationInsights) bieden telemetrie- *modules* en *initialisatie functies* voor het automatisch traceren van telemetrie van uw toepassing en de bijbehorende context. Door het configuratie bestand aan te passen, kunt u telemetrie-modules en initialisatie functies in-of uitschakelen en para meters instellen voor een aantal hiervan.

Het configuratie bestand heet `ApplicationInsights.config` of `ApplicationInsights.xml`, afhankelijk van het type van uw toepassing. Het wordt automatisch toegevoegd aan uw project wanneer u [de meeste versies van de SDK installeert][start]. Het wordt ook toegevoegd aan een web-app door [status monitor op een IIS-server][redfield]. Het configuratie bestand wordt genegeerd als de [extensie voor de Azure-website](azure-web-apps.md) of- [extensie voor de virtuele machine van Azure en](azure-vm-vmss-apps.md) de VM-schaalset wordt gebruikt.

Er is geen gelijkwaardig bestand voor het beheren [van de SDK op een webpagina][client].

In dit document worden de secties beschreven die u ziet in het configuratie bestand, hoe ze de onderdelen van de SDK beheren en welke NuGet-pakketten deze onderdelen laden.

> [!NOTE]
> Instructies voor ApplicationInsights. config en. XML zijn niet van toepassing op de .NET Core SDK. Volg deze hand leiding voor [het](../../azure-monitor/app/asp-net-core.md) configureren van .net core-toepassingen.

## <a name="telemetry-modules-aspnet"></a>Telemetrie-modules (ASP.NET)
Elke telemetrie-module verzamelt een specifiek type gegevens en gebruikt de core API om de gegevens te verzenden. De modules worden geïnstalleerd door verschillende NuGet-pakketten, waarmee ook de vereiste regels aan het. config-bestand worden toegevoegd.

Er is een knoop punt in het configuratie bestand voor elke module. Als u een module wilt uitschakelen, verwijdert u het knoop punt of opmerkingen toevoegen.

### <a name="dependency-tracking"></a>Afhankelijkheden bijhouden
Bij het [bijhouden](../../azure-monitor/app/asp-net-dependencies.md) van afhankelijkheden wordt telemetrie over het oproepen van uw app naar data bases en externe services en data bases verzameld. Als u wilt toestaan dat deze module werkt in een IIS-server, moet u [status monitor installeren][redfield].

U kunt ook uw eigen tracerings code voor afhankelijkheid schrijven met behulp van de [TrackDependency-API](../../azure-monitor/app/api-custom-events-metrics.md#trackdependency).

* `Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule`
* [Micro soft. ApplicationInsights. DependencyCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector) NuGet-pakket.

Afhankelijkheden kunnen automatisch worden verzameld zonder uw code te wijzigen met behulp van op agents (zonder code) bijvoegen. Als u het wilt gebruiken in azure web apps, schakelt u de [extensie Application Insights](azure-web-apps.md)in. Als u deze wilt gebruiken in azure VM of virtuele-machine schaalset van Azure, schakelt u de [uitbrei ding toepassings bewaking in voor de VM-en virtuele-machine schaalset](azure-vm-vmss-apps.md).

### <a name="performance-collector"></a>Prestatie verzamelaar
[Verzamelt systeem prestatie meter items](../../azure-monitor/app/performance-counters.md) , zoals CPU, geheugen en netwerk belasting van IIS-installaties. U kunt opgeven welke items moeten worden verzameld, met inbegrip van de prestatie meter items die u zelf hebt ingesteld.

* `Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule`
* [Micro soft. ApplicationInsights. PerfCounterCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.PerfCounterCollector) NuGet-pakket.

### <a name="application-insights-diagnostics-telemetry"></a>Telemetrie diagnostische gegevens Application Insights
De `DiagnosticsTelemetryModule` rapporteert fouten in de Application Insights instrumentatie code zelf. Als de code bijvoorbeeld geen toegang heeft tot prestatie meter items of als `ITelemetryInitializer` een uitzonde ring optreedt. Telemetrie traceren die door deze module worden bijgehouden, wordt weer gegeven in de [Diagnostische zoek opdracht][diagnostic].

```
* `Microsoft.ApplicationInsights.Extensibility.Implementation.Tracing.DiagnosticsTelemetryModule`
* [Microsoft.ApplicationInsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights) NuGet package. If you only install this package, the ApplicationInsights.config file is not automatically created.
```

### <a name="developer-mode"></a>Ontwikkelaars modus
`DeveloperModeWithDebuggerAttachedTelemetryModule`Hiermee wordt ervoor `TelemetryChannel` gezorgd dat de Application Insights direct gegevens verzendt, één telemetrie-item wanneer een fout opsporingsprogramma aan het toepassings proces is gekoppeld. Dit vermindert de hoeveelheid tijd tussen het moment waarop uw toepassing telemetrie bijhoudt en wanneer deze wordt weer gegeven op de Application Insights Portal. Dit veroorzaakt aanzienlijke overhead in CPU-en netwerk bandbreedte.

* `Microsoft.ApplicationInsights.WindowsServer.DeveloperModeWithDebuggerAttachedTelemetryModule`
* [Application Insights Windows Server](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) NuGet-pakket

### <a name="web-request-tracking"></a>Tracering van webaanvragen
Rapporteert de [reactie tijd en de resultaat code](../../azure-monitor/app/asp-net.md) van HTTP-aanvragen.

* `Microsoft.ApplicationInsights.Web.RequestTrackingTelemetryModule`
* [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) NuGet package

### <a name="exception-tracking"></a>Uitzonde ringen bijhouden
`ExceptionTrackingTelemetryModule`Hiermee worden niet-verwerkte uitzonde ringen in uw web-app bijgehouden. Bekijk [storingen en uitzonde ringen][exceptions].

* `Microsoft.ApplicationInsights.Web.ExceptionTrackingTelemetryModule`
* [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) NuGet package
* `Microsoft.ApplicationInsights.WindowsServer.UnobservedExceptionTelemetryModule`-houdt niet- [waargenomen taak uitzonderingen](https://blogs.msdn.com/b/pfxteam/archive/2011/09/28/task-exception-handling-in-net-4-5.aspx)bij.
* `Microsoft.ApplicationInsights.WindowsServer.UnhandledExceptionTelemetryModule`-niet-verwerkte uitzonde ringen voor werk rollen, Windows-Services en console toepassingen worden bijgehouden.
* [Application Insights Windows Server](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) NuGet-pakket.

### <a name="eventsource-tracking"></a>Event source tracking
`EventSourceTelemetryModule`Hiermee kunt u Event source-gebeurtenissen configureren voor verzen ding naar Application Insights als traceringen. Zie [gebeurtenis source Events gebruiken](../../azure-monitor/app/asp-net-trace-logs.md#use-eventsource-events)voor meer informatie over het volgen van Event source-gebeurtenissen.

* `Microsoft.ApplicationInsights.EventSourceListener.EventSourceTelemetryModule`
* [Microsoft.ApplicationInsights.EventSourceListener](https://www.nuget.org/packages/Microsoft.ApplicationInsights.EventSourceListener) 

### <a name="etw-event-tracking"></a>ETW-gebeurtenis tracering
`EtwCollectorTelemetryModule`Hiermee kunt u gebeurtenissen configureren van ETW-providers die moeten worden verzonden naar Application Insights als traceringen. Zie [etw-gebeurtenissen gebruiken](../../azure-monitor/app/asp-net-trace-logs.md#use-etw-events)voor meer informatie over het volgen van etw-gebeurtenissen.

* `Microsoft.ApplicationInsights.EtwCollector.EtwCollectorTelemetryModule`
* [Microsoft.ApplicationInsights.EtwCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.EtwCollector) 

### <a name="microsoftapplicationinsights"></a>Microsoft.ApplicationInsights
Het pakket micro soft. ApplicationInsights biedt de [kern-API](https://msdn.microsoft.com/library/mt420197.aspx) van de SDK. De andere telemetrie-modules gebruiken deze en u kunt [deze ook gebruiken om uw eigen telemetrie te definiëren](../../azure-monitor/app/api-custom-events-metrics.md).

* Geen vermelding in ApplicationInsights. config.
* [Micro soft. ApplicationInsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights) NuGet-pakket. Als u alleen deze NuGet installeert, wordt er geen. config-bestand gegenereerd.

## <a name="telemetry-channel"></a>Telemetrie-kanaal
Het telemetrie- [kanaal](telemetry-channels.md) beheert het bufferen en verzenden van telemetrie naar de Application Insights-service.

* `Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel`is het standaard kanaal voor webtoepassingen. Het buffert gegevens in het geheugen en maakt gebruik van mechanismen voor opnieuw proberen en lokale schijf opslag voor betrouwbaardere telemetrie-levering.
* `Microsoft.ApplicationInsights.InMemoryChannel`is een licht gewicht-telemetrie-kanaal, dat wordt gebruikt als er geen ander kanaal is geconfigureerd. 

## <a name="telemetry-initializers-aspnet"></a>Initialisatie functies voor telemetrie (ASP.NET)
Met de initialisatie functies voor telemetrie worden context eigenschappen ingesteld die samen met elk item van telemetrie worden verzonden.

U kunt [uw eigen initialisatie functies schrijven](../../azure-monitor/app/api-filtering-sampling.md#add-properties) om context eigenschappen in te stellen.

De standaard initialisatie functies zijn allemaal ingesteld door de web-of WindowsServer NuGet-pakketten:

* `AccountIdTelemetryInitializer`Hiermee wordt de eigenschap AccountId ingesteld.
* `AuthenticatedUserIdTelemetryInitializer`Hiermee stelt u de eigenschap AuthenticatedUserId in zoals ingesteld door de Java script-SDK.
* `AzureRoleEnvironmentTelemetryInitializer`Hiermee worden `RoleName` de `RoleInstance` en eigenschappen van `Device` de context voor alle telemetriegegevens bijgewerkt met gegevens die zijn geëxtraheerd uit de Azure runtime-omgeving.
* `BuildInfoConfigComponentVersionTelemetryInitializer`Hiermee wordt `Version` de eigenschap van `Component` de context voor alle telemetrie-items bijgewerkt met de waarde `BuildInfo.config` die is geëxtraheerd uit het bestand dat wordt gegenereerd door MS build.
* `ClientIpHeaderTelemetryInitializer`Hiermee `Ip` wordt de eigenschap `Location` bijgewerkt van de context van alle telemetriegegevens op basis `X-Forwarded-For` van de http-header van de aanvraag.
* `DeviceTelemetryInitializer`Hiermee worden de volgende eigenschappen van `Device` de context voor alle telemetrie-items bijgewerkt.
  * `Type`is ingesteld op ' PC '
  * `Id`is ingesteld op de domein naam van de computer waarop de webtoepassing wordt uitgevoerd.
  * `OemName`wordt ingesteld op de waarde die is geëxtraheerd uit `Win32_ComputerSystem.Manufacturer` het veld met WMI.
  * `Model`wordt ingesteld op de waarde die is geëxtraheerd uit `Win32_ComputerSystem.Model` het veld met WMI.
  * `NetworkType`wordt ingesteld op de waarde die is geëxtraheerd uit `NetworkInterface`de.
  * `Language`is ingesteld op de naam van `CurrentCulture`.
* `DomainNameRoleInstanceTelemetryInitializer`Hiermee wordt `RoleInstance` de eigenschap van `Device` de context voor alle telemetriegegevens bijgewerkt met de domein naam van de computer waarop de webtoepassing wordt uitgevoerd.
* `OperationNameTelemetryInitializer`Hiermee wordt `Name` de eigenschap van `RequestTelemetry` `Name` de`Operation` en de eigenschap van de context van alle telemetrie-items bijgewerkt op basis van de HTTP-methode, evenals de namen van de ASP.NET MVC-controller en de actie die is aangeroepen om de aanvraag te verwerken.
* `OperationIdTelemetryInitializer`of `OperationCorrelationTelemetryInitializer` werkt de `Operation.Id` eigenschap context bij van alle telemetriegegevens die worden bijgehouden tijdens het verwerken van een aanvraag met `RequestTelemetry.Id`de automatisch gegenereerde items.
* `SessionTelemetryInitializer`Hiermee wordt `Id` de eigenschap van `Session` de context voor alle telemetrie-items bijgewerkt met de `ai_session` waarde die is geëxtraheerd uit de cookie die wordt gegenereerd door de ApplicationInsights java script Instrumentation-code die wordt uitgevoerd in de browser van de gebruiker.
* `SyntheticTelemetryInitializer`of `SyntheticUserAgentTelemetryInitializer` werkt de `User`eigenschappen `Session`,, `Operation` en contexten bij van alle telemetriegegevens die worden bijgehouden bij het afhandelen van een aanvraag van een synthetische bron, zoals een beschikbaarheids test of een zoek machine-bot. [Metrics Explorer](../../azure-monitor/app/metrics-explorer.md) worden standaard geen synthetische telemetrie weer gegeven.

    De `<Filters>` set waarmee de eigenschappen van de aanvragen worden geïdentificeerd.
* `UserTelemetryInitializer`Hiermee worden `Id` de `AcquisitionDate` en eigenschappen `User` van de context voor alle telemetriegegevens bijgewerkt met waarden die zijn geëxtraheerd `ai_user` uit de cookie die wordt gegenereerd door de Application Insights java script Instrumentation-code die wordt uitgevoerd in de gebruikers Browser.
* `WebTestTelemetryInitializer`Hiermee stelt u de gebruikers-ID, sessie-ID en synthetische bron eigenschappen in voor HTTP-aanvragen die afkomstig zijn van [beschikbaarheids testen](../../azure-monitor/app/monitor-web-app-availability.md).
  De `<Filters>` set waarmee de eigenschappen van de aanvragen worden geïdentificeerd.

Voor .NET-toepassingen die worden uitgevoerd in service Fabric, kunt `Microsoft.ApplicationInsights.ServiceFabric` u het NuGet-pakket toevoegen. Dit pakket bevat een `FabricTelemetryInitializer`, waarmee service Fabric eigenschappen aan telemetrie-items worden toegevoegd. Zie de [pagina github](https://github.com/Microsoft/ApplicationInsights-ServiceFabric/blob/master/README.md) over de eigenschappen die zijn toegevoegd door dit NuGet-pakket voor meer informatie.

## <a name="telemetry-processors-aspnet"></a>Telemetrie-processors (ASP.NET)
Telemetrie-processors kunnen elk telemetrie-item filteren en wijzigen voordat het wordt verzonden vanuit de SDK naar de portal.

U kunt [uw eigen telemetrie-processors schrijven](../../azure-monitor/app/api-filtering-sampling.md#filtering).

#### <a name="adaptive-sampling-telemetry-processor-from-200-beta3"></a>Apparaatafhankelijke bemonsterings-telemetrie-processor (van 2.0.0-beta3)
Deze optie is standaard ingeschakeld. Als uw app veel telemetrie verzendt, wordt deze door deze processor verwijderd.

```xml

    <TelemetryProcessors>
      <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.AdaptiveSamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
        <MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>
      </Add>
    </TelemetryProcessors>

```

De para meter geeft het doel aan dat het algoritme probeert te bereiken. Elk exemplaar van de SDK werkt onafhankelijk, dus als uw server een cluster is van verschillende computers, wordt het werkelijke volume van de telemetrie op basis daarvan vermenigvuldigd.

Meer [informatie over steek proeven](../../azure-monitor/app/sampling.md).

#### <a name="fixed-rate-sampling-telemetry-processor-from-200-beta1"></a>Telemetrie-sampling-processor met vaste frequentie (van 2.0.0-beta1)
Er is ook een standaard-sample-telemetrie- [processor](../../azure-monitor/app/api-filtering-sampling.md) (van 2.0.1):

```XML

    <TelemetryProcessors>
     <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.SamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">

     <!-- Set a percentage close to 100/N where N is an integer. -->
     <!-- E.g. 50 (=100/2), 33.33 (=100/3), 25 (=100/4), 20, 1 (=100/100), 0.1 (=100/1000) -->
     <SamplingPercentage>10</SamplingPercentage>
     </Add>
   </TelemetryProcessors>

```



## <a name="channel-parameters-java"></a>Kanaal parameters (Java)
Deze para meters bepalen hoe de Java-SDK de telemetriegegevens van de gegevensverzamelings gegevens moet opslaan en leegmaken.

#### <a name="maxtelemetrybuffercapacity"></a>MaxTelemetryBufferCapacity
Het aantal telemetriegegevens dat kan worden opgeslagen in de opslag ruimte in het geheugen van de SDK. Wanneer dit aantal is bereikt, wordt de telemetrie-buffer leeg gemaakt. dat wil zeggen dat de telemetriegegevens naar de Application Insights-server worden verzonden.

* Min: 1
* Aantal 1000
* Prijs 500

```

  <ApplicationInsights>
      ...
      <Channel>
       <MaxTelemetryBufferCapacity>100</MaxTelemetryBufferCapacity>
      </Channel>
      ...
  </ApplicationInsights>
```

#### <a name="flushintervalinseconds"></a>FlushIntervalInSeconds
Bepaalt hoe vaak de gegevens die in de opslag in het geheugen zijn opgeslagen, moeten worden leeg gemaakt (verzonden naar Application Insights).

* Min: 1
* Aantal 300
* Prijs 5

```

    <ApplicationInsights>
      ...
      <Channel>
        <FlushIntervalInSeconds>100</FlushIntervalInSeconds>
      </Channel>
      ...
    </ApplicationInsights>
```

#### <a name="maxtransmissionstoragecapacityinmb"></a>MaxTransmissionStorageCapacityInMB
Bepaalt de maximale grootte in MB die wordt toegewezen aan de permanente opslag op de lokale schijf. Deze opslag wordt gebruikt voor het persistent maken van telemetriegegevens van items die niet naar het Application Insights-eind punt konden worden verzonden. Wanneer aan de opslag grootte is voldaan, worden nieuwe telemetriegegevens genegeerd.

* Min: 1
* Aantal 100
* Prijs 10

```

   <ApplicationInsights>
      ...
      <Channel>
        <MaxTransmissionStorageCapacityInMB>50</MaxTransmissionStorageCapacityInMB>
      </Channel>
      ...
   </ApplicationInsights>
```

#### <a name="local-forwarder"></a>Lokale doorstuurserver

[Lokale doorstuur server](opencensus-local-forwarder.md) is een agent die de telemetrie van Application Insights of opentellingen verzamelt vanuit diverse sdk's en frameworks en deze naar Application Insights stuurt. [](https://opencensus.io/) Het kan worden uitgevoerd onder Windows en Linux. Wanneer de lokale doorstuur server is gekoppeld aan de Application Insights Java-SDK, biedt dit volledige ondersteuning voor [Live metrische gegevens](../../azure-monitor/app/live-stream.md) en adaptieve steek proeven.

```xml
<Channel type="com.microsoft.applicationinsights.channel.concrete.localforwarder.LocalForwarderTelemetryChannel">
<EndpointAddress><!-- put the hostname:port of your LocalForwarder instance here --></EndpointAddress>

<!-- The properties below are optional. The values shown are the defaults for each property -->

<FlushIntervalInSeconds>5</FlushIntervalInSeconds><!-- must be between [1, 500]. values outside the bound will be rounded to nearest bound -->
<MaxTelemetryBufferCapacity>500</MaxTelemetryBufferCapacity><!-- units=number of telemetry items; must be between [1, 1000] -->
</Channel>
```

Als u SpringBoot Starter gebruikt, voegt u het volgende toe aan uw configuratie bestand (toepassing. eigenschappen):

```yml
azure.application-insights.channel.local-forwarder.endpoint-address=<!--put the hostname:port of your LocalForwarder instance here-->
azure.application-insights.channel.local-forwarder.flush-interval-in-seconds=<!--optional-->
azure.application-insights.channel.local-forwarder.max-telemetry-buffer-capacity=<!--optional-->
```

De standaard waarden zijn hetzelfde voor SpringBoot Application. Properties en applicationinsights. xml-configuratie.

## <a name="instrumentationkey"></a>InstrumentationKey
Hiermee bepaalt u de Application Insights resource waarin uw gegevens worden weer gegeven. Doorgaans maakt u een afzonderlijke resource, met een afzonderlijke sleutel, voor elk van uw toepassingen.

Als u de sleutel dynamisch wilt instellen, bijvoorbeeld als u de resultaten van uw toepassing naar verschillende resources wilt verzenden, kunt u de sleutel uit het configuratie bestand weglaten en in plaats daarvan instellen in de code.

Als u de sleutel wilt instellen voor alle exemplaren van TelemetryClient, met inbegrip van standaard-telemetriegegevens, stelt u de sleutel in TelemetryConfiguration. Active. Doe dit in een initialisatie methode, zoals global.aspx.cs in een ASP.NET-service:

```csharp

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey =
          // - for example -
          WebConfigurationManager.AppSettings["ikey"];
      //...
```

Als u alleen een specifieke set gebeurtenissen naar een andere resource wilt verzenden, kunt u de sleutel voor een specifieke TelemetryClient instellen:

```csharp

    var tc = new TelemetryClient();
    tc.Context.InstrumentationKey = "----- my key ----";
    tc.TrackEvent("myEvent");
    // ...

```

Als u een nieuwe sleutel wilt ophalen, [maakt u een nieuwe resource in de Application Insights Portal][new].



## <a name="applicationid-provider"></a>ApplicationId Provider

_Beschikbaar vanaf v 2.6.0_

Het doel van deze provider is om een toepassings-ID te zoeken op basis van een instrumentatie sleutel. De toepassings-ID is opgenomen in RequestTelemetry en DependencyTelemetry en wordt gebruikt om de correlatie in de portal te bepalen.

Dit is beschikbaar door in `TelemetryConfiguration.ApplicationIdProvider` te stellen in code of in configuratie.

### <a name="interface-iapplicationidprovider"></a>Interface IApplicationIdProvider

```csharp
public interface IApplicationIdProvider
{
    bool TryGetApplicationId(string instrumentationKey, out string applicationId);
}
```


We bieden twee implementaties in de [micro soft. ApplicationInsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights) SDK `ApplicationInsightsApplicationIdProvider` : `DictionaryApplicationIdProvider`en.

### <a name="applicationinsightsapplicationidprovider"></a>ApplicationInsightsApplicationIdProvider

Dit is een wrapper rond onze profiel-API. Hiermee worden aanvragen en cache resultaten beperkt.

Deze provider wordt toegevoegd aan uw configuratie bestand wanneer u [micro soft. ApplicationInsights. DependencyCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector) of [micro soft. ApplicationInsights. Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) installeert

Deze klasse heeft een optionele eigenschap `ProfileQueryEndpoint`.
Deze instelling is standaard ingesteld op `https://dc.services.visualstudio.com/api/profiles/{0}/appId`.
Als u een proxy voor deze configuratie wilt configureren, is het raadzaam om het basis adres te tele/API/Profiles/en{0}en met inbegrip van '/AppID '. Opmerking: ' ' wordt vervangen tijdens runtime per aanvraag met de instrumentatie sleutel.{0}

#### <a name="example-configuration-via-applicationinsightsconfig"></a>Voorbeeld configuratie via ApplicationInsights. config:
```xml
<ApplicationInsights>
    ...
    <ApplicationIdProvider Type="Microsoft.ApplicationInsights.Extensibility.Implementation.ApplicationId.ApplicationInsightsApplicationIdProvider, Microsoft.ApplicationInsights">
        <ProfileQueryEndpoint>https://dc.services.visualstudio.com/api/profiles/{0}/appId</ProfileQueryEndpoint>
    </ApplicationIdProvider>
    ...
</ApplicationInsights>
```

#### <a name="example-configuration-via-code"></a>Voorbeeld configuratie via code:
```csharp
TelemetryConfiguration.Active.ApplicationIdProvider = new ApplicationInsightsApplicationIdProvider();
```

### <a name="dictionaryapplicationidprovider"></a>DictionaryApplicationIdProvider

Dit is een statische provider die afhankelijk is van uw geconfigureerde instrumentatie sleutel/toepassings-ID-paren.

Deze klasse heeft een eigenschap `Defined`, die een woorden lijst < teken reeks, teken reeks > van instrumentatie sleutel naar toepassings-id-paren is.

Deze klasse heeft een optionele eigenschap `Next` die kan worden gebruikt om een andere provider te configureren die moet worden gebruikt wanneer een instrumentatie sleutel wordt aangevraagd die niet voor komt in uw configuratie.

#### <a name="example-configuration-via-applicationinsightsconfig"></a>Voorbeeld configuratie via ApplicationInsights. config:
```xml
<ApplicationInsights>
    ...
    <ApplicationIdProvider Type="Microsoft.ApplicationInsights.Extensibility.Implementation.ApplicationId.DictionaryApplicationIdProvider, Microsoft.ApplicationInsights">
        <Defined>
            <Type key="InstrumentationKey_1" value="ApplicationId_1"/>
            <Type key="InstrumentationKey_2" value="ApplicationId_2"/>
        </Defined>
        <Next Type="Microsoft.ApplicationInsights.Extensibility.Implementation.ApplicationId.ApplicationInsightsApplicationIdProvider, Microsoft.ApplicationInsights" />
    </ApplicationIdProvider>
    ...
</ApplicationInsights>
```

#### <a name="example-configuration-via-code"></a>Voorbeeld configuratie via code:
```csharp
TelemetryConfiguration.Active.ApplicationIdProvider = new DictionaryApplicationIdProvider{
 Defined = new Dictionary<string, string>
    {
        {"InstrumentationKey_1", "ApplicationId_1"},
        {"InstrumentationKey_2", "ApplicationId_2"}
    }
};
```




## <a name="next-steps"></a>Volgende stappen
Meer [informatie over de API][api].

<!--Link references-->

[api]: ../../azure-monitor/app/api-custom-events-metrics.md
[client]: ../../azure-monitor/app/javascript.md
[diagnostic]: ../../azure-monitor/app/diagnostic-search.md
[exceptions]: ../../azure-monitor/app/asp-net-exceptions.md
[netlogs]: ../../azure-monitor/app/asp-net-trace-logs.md
[new]: ../../azure-monitor/app/create-new-resource.md 
[redfield]: ../../azure-monitor/app/monitor-performance-live-website-now.md
[start]: ../../azure-monitor/app/app-insights-overview.md
