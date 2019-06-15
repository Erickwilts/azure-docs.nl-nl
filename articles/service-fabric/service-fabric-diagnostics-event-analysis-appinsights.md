---
title: Azure Service Fabric-Gebeurtenisanalyse met Application Insights | Microsoft Docs
description: Meer informatie over het visualiseren en analyseren van gebeurtenissen met Application Insights voor controle en diagnose van Azure Service Fabric-clusters.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/21/2018
ms.author: srrengar
ms.openlocfilehash: f4c620bbb0e17abfacb504866230786a971ff409
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60393158"
---
# <a name="event-analysis-and-visualization-with-application-insights"></a>Gebeurtenis analyses en visualisatie met Application Insights

Onderdeel van Azure Monitor, Application Insights is een uitbreidbaar platform voor de toepassingsbewaking en diagnostische gegevens. Het bevat een krachtige analyses en het uitvoeren van query's hulpprogramma, aanpasbaar dashboard en visualisaties en meer opties, waaronder automatische waarschuwingen. De Application Insights-integratie met Service Fabric bevat verschillende hulpprogramma's voor Visual Studio en Azure portal, evenals Service Fabric-specifieke metrische gegevens, biedt een uitgebreide logboekregistratie voor out-of-the-box-ervaring. Hoewel veel logboeken worden automatisch gemaakt en die voor u worden verzameld met Application Insights, raden wij u aan meer aangepaste logboekregistratie voor uw toepassingen voor het maken van een rijkere ervaring voor diagnostische gegevens.

Dit artikel helpt de volgende veelgestelde vragen-adres:

* Hoe weet ik wat er gebeurt in mijn toepassingen en services en verzamelen telemetrie?
* Hoe los ik mijn toepassing, met name services met elkaar communiceren?
* Hoe krijg ik metrische gegevens over hoe mijn services uitvoert, bijvoorbeeld, de laadtijd van browserpagina, HTTP-aanvragen?

Het doel van dit artikel is om te laten zien hoe u inzicht en oplossen van in Application Insights. Als u wilt meer informatie over het instellen en configureren van Application Insights met Service Fabric, bekijk dan deze [zelfstudie](service-fabric-tutorial-monitoring-aspnet.md).

## <a name="monitoring-in-application-insights"></a>Bewaken in Application Insights

Application Insights is een geavanceerde buiten de box-ervaring bij het gebruik van Service Fabric. De pagina overzicht van biedt Application Insights belangrijke informatie over uw service, zoals de reactietijd en het aantal aanvragen dat is verwerkt. Door te klikken op de knop 'Zoeken' aan de bovenkant, ziet u een lijst met recente aanvragen in uw toepassing. Bovendien kunt u zou zich hier mislukte aanvragen weergeven en een diagnose kunnen welke fouten zijn opgetreden.

![Overzicht van Application Insights](media/service-fabric-diagnostics-event-analysis-appinsights/ai-overview.png)

In het rechterdeelvenster in de vorige afbeelding, er zijn twee soorten gegevens in de lijst: aanvragen en gebeurtenissen. Aanvragen zijn oproepen naar de API via HTTP-aanvragen van de app in dit geval en gebeurtenissen zijn aangepaste gebeurtenissen, die fungeren als telemetrie die u overal in uw code kunt toevoegen. U kunt verder verkennen instrumenteren van uw toepassingen in [Application Insights-API voor aangepaste gebeurtenissen en metrische gegevens](../azure-monitor/app/api-custom-events-metrics.md). Te klikken op een aanvraag zou meer details weergeven zoals wordt weergegeven in de volgende afbeelding, met inbegrip van gegevens die specifiek zijn voor Service Fabric, dat wordt verzameld in het nuget-pakket voor Application Insights-Service Fabric. Deze informatie is nuttig voor het oplossen van problemen en weet wat de status van uw toepassing is, en al deze gegevens wordt gezocht binnen de Application Insights

![Details van de aanvraag Application Insights](media/service-fabric-diagnostics-event-analysis-appinsights/ai-request-details.png)

Application Insights heeft een aangewezen weergave voor het uitvoeren van query's op alle gegevens die wordt geleverd in. Klik op 'Metrics Explorer' boven aan de pagina overzicht om te navigeren naar de Application Insights-portal. Hier kunt u query's uitvoeren op basis van aangepaste gebeurtenissen al eerder vermeld, aanvragen, uitzonderingen, prestatiemeteritems en andere metrische gegevens met behulp van de Kusto-query-taal. Het volgende voorbeeld ziet alle aanvragen in het afgelopen uur.

![Details van de aanvraag Application Insights](media/service-fabric-diagnostics-event-analysis-appinsights/ai-metrics-explorer.png)

Als u wilt de mogelijkheden van de Application Insights-portal verder verkennen, Ga naar de [documentatie voor Application Insights-portal](../azure-monitor/app/app-insights-dashboards.md).

### <a name="configuring-application-insights-with-eventflow"></a>Configuratie van Application Insights samenstellen met EventFlow

Als u van EventFlow naar gebeurtenissen gebruikmaakt, controleert u of voor het importeren van de `Microsoft.Diagnostics.EventFlow.Output.ApplicationInsights`NuGet-pakket. De volgende code is vereist in de *levert* sectie van de *eventFlowConfig.json*:

```json
"outputs": [
    {
        "type": "ApplicationInsights",
        "instrumentationKey": "***ADD INSTRUMENTATION KEY HERE***"
    }
]
```

Zorg ervoor dat u de benodigde wijzigingen aanbrengen in de filters, evenals alle andere invoer (samen met hun respectieve NuGet-pakketten) bevatten.

## <a name="application-insights-sdk"></a>Application Insights SDK

Het verdient aanbeveling met EventFlow en WAD aggregatieoplossingen, omdat ze toestaan voor een meer modulaire benadering voor diagnose en controle, dat wil zeggen als u wilt wijzigen van de uitvoer van EventFlow, zonder veranderingen in uw werkelijke instrumentation vereist alleen een eenvoudige wijziging aan het configuratiebestand. Als u echter besluit om door te investeren in met behulp van Application Insights en waarschijnlijk niet wijzigen in een ander platform, moet u zoeken naar nieuwe Application Insights-SDK gebruiken voor het aggregeren van gebeurtenissen en deze te verzenden naar Application Insights. Dit betekent dat u niet langer hoeft EventFlow voor het verzenden van uw gegevens naar Application Insights configureren, maar in plaats daarvan de ApplicationInsight van Service Fabric NuGet-pakket wordt geïnstalleerd. Meer informatie over het pakket kunnen worden gevonden [hier](https://github.com/Microsoft/ApplicationInsights-ServiceFabric).

[Application Insights-ondersteuning voor Microservices en Containers](https://azure.microsoft.com/blog/app-insights-microservices/) ziet u enkele van de nieuwe functies die zijn wordt gewerkt (momenteel nog steeds in beta), waarmee u kunt uitgebreidere controle out-of-the-box-opties met Application Insights hebt. Hieronder vallen afhankelijkheid bijhouden (gebruikt voor het bouwen van een toepassingstoewijzing van uw services en toepassingen in een cluster en de communicatie ertussen) en betere correlatie van traceringen die afkomstig zijn van uw services (helpt u bij het beter een probleem in de werkstroom van dicht een toepassing of service).

Als u in .NET ontwikkelt en zal waarschijnlijk worden met behulp van enkele van Service Fabric-programmeermodellen en bereid Application Insights gebruiken als uw platform voor het visualiseren en analyseren van de gebeurtenis-en log, klikt u vervolgens raadzaam dat u via de Application Insights gaat SDK-route als de werkstroom voor controle en diagnostische gegevens. Lezen [dit](../azure-monitor/app/asp-net-more.md) en [dit](../azure-monitor/app/asp-net-trace-logs.md) aan de slag met het gebruik van Application Insights te verzamelen en weergeven van uw Logboeken.

## <a name="navigating-the-application-insights-resource-in-azure-portal"></a>Navigeren in de Application Insights-resource in Azure portal

Als u Application Insights hebt geconfigureerd als uitvoer voor uw gebeurtenissen en Logboeken, moet informatie worden weergegeven in uw Application Insights-resource in een paar minuten beginnen. Navigeer naar de Application Insights-resource, die u naar de Application Insights-resource-dashboard gaat. Klik op **zoeken** in de taakbalk van Application Insights om te zien van de meest recente traceringen die is ontvangen en kunnen ze filteren.

*Metrics Explorer* een handig hulpmiddel voor het maken van aangepaste dashboards op basis van metrische gegevens die uw toepassingen, services en -cluster kunnen rapporteren. Zie [verkennen van metrische gegevens in Application Insights](../azure-monitor/app/metrics-explorer.md) voor het instellen van een aantal grafieken voor zelf is gebaseerd op de gegevens die u verzamelt.

Te klikken op **Analytics** gaat u naar de Application Insights Analytics-portal, waar u kunt een query gebeurtenissen en traceringen met groter bereik en optionaliteit. Meer informatie over deze [analyses in Application Insights](../azure-monitor/app/analytics.md).

## <a name="next-steps"></a>Volgende stappen

* [Stel waarschuwingen in AI](../azure-monitor/app/alerts.md) om te worden geïnformeerd over wijzigingen in de prestaties of gebruik
* [Slimme detectie in Application Insights](../azure-monitor/app/proactive-diagnostics.md) voert een proactieve analyse van de telemetrie wordt verzonden naar Application Insights om te waarschuwen voor mogelijke prestatieproblemen
