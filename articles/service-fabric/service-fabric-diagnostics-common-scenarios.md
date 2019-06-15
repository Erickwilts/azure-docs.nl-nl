---
title: Azure Service Fabric onderzoeken algemene scenario's | Microsoft Docs
description: Informatie over het oplossen van veelvoorkomende scenario's met Azure Service Fabric
services: service-fabric
documentationcenter: .net
author: srrengar
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/25/2019
ms.author: srrengar
ms.openlocfilehash: 265aea1b8873d812859b39175c732c3e7118cbb5
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60394120"
---
# <a name="diagnose-common-scenarios-with-service-fabric"></a>Algemene scenario's met Service Fabric onderzoeken

In dit artikel ziet u algemene scenario's die gebruikers hebben aangetroffen in het gebied van controle en diagnostische gegevens met Service Fabric. De scenario's die betrekking hebben op alle 3 lagen van service fabric: Toepassings-, Cluster- en -infrastructuur. Elke oplossing maakt gebruik van Application Insights en logboeken van Azure Monitor, Azure controlehulpprogramma's, om uit te voeren van elk scenario. De stappen in elke oplossing geeft gebruikers een inleiding over het gebruik van Application Insights en Azure Monitor geregistreerd in de context van Service Fabric.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites-and-recommendations"></a>Vereisten en aanbevelingen

De oplossingen in dit artikel worden de volgende hulpprogramma's gebruiken. U wordt aangeraden dat u hebt deze ingesteld en geconfigureerd:

* [Application Insights met Service Fabric](service-fabric-tutorial-monitoring-aspnet.md)
* [Inschakelen van Azure Diagnostics in uw cluster](service-fabric-diagnostics-event-aggregation-wad.md)
* [Stel een Log Analytics-werkruimte](service-fabric-diagnostics-oms-setup.md)
* [Meld u Analytics-agent voor het volgen van prestatiemeteritems](service-fabric-diagnostics-oms-agent.md)

## <a name="how-can-i-see-unhandled-exceptions-in-my-application"></a>Hoe kan ik onverwerkte uitzonderingen in mijn toepassing zien?

1. Navigeer naar uw Application Insights-resource waarmee uw toepassing is geconfigureerd.
2. Klik op *zoeken* in de linkerbovenhoek. Klik vervolgens op filter in het volgende deelvenster.

    ![AI-overzicht](media/service-fabric-diagnostics-common-scenarios/ai-search-filter.png)

3. Hier ziet u veel van de typen gebeurtenissen (traceringen, aanvragen, aangepaste gebeurtenissen). Kies 'Uitzondering' als filter.

    ![AI-filterlijst](media/service-fabric-diagnostics-common-scenarios/ai-filter-list.png)

    Door te klikken op een uitzondering in de lijst, kunt u meer informatie, zoals de servicecontext als u de Service Fabric Application Insights SDK kijken.

    ![AI-uitzondering](media/service-fabric-diagnostics-common-scenarios/ai-exception.png)

## <a name="how-do-i-view-which-http-calls-are-used-in-my-services"></a>Hoe kan ik bekijken waarmee HTTP-aanroepen worden gebruikt in mijn services?

1. In de dezelfde Application Insights-resource, kunt u filteren op 'aanvragen' in plaats van uitzonderingen en alle aanvragen bekijken
2. Als u de Service Fabric Application Insights-SDK gebruikt, ziet u een visuele weergave van uw services met elkaar verbonden en het aantal is geslaagd en mislukte aanvragen. Klik op 'Overzicht van de toepassing' aan de linkerkant.

    ![App-kaart van AI-Blade](media/service-fabric-diagnostics-common-scenarios/app-map-blade.png) ![AI App-kaart](media/service-fabric-diagnostics-common-scenarios/app-map-new.png)

    Voor meer informatie over het toepassingsoverzicht, gaat u naar de [mapa Aplikace Pro-documentatie](../azure-monitor/app/app-map.md)

## <a name="how-do-i-create-an-alert-when-a-node-goes-down"></a>Hoe maak ik een waarschuwing wanneer een knooppunt uitgeschakeld wordt

1. Knooppunt-gebeurtenissen worden bijgehouden door uw Service Fabric-cluster. Navigeer naar de Service Fabric-analyse-oplossing-resource met de naam **ServiceFabric(NameofResourceGroup)**
2. Klik op de grafiek aan de onderkant van de blade met de titel 'Overzicht'

    ![Azure Monitor-logboeken oplossing](media/service-fabric-diagnostics-common-scenarios/oms-solution-azure-portal.png)

3. U hebt hier veel grafieken en tegels weergeven van verschillende metrische gegevens. Klik op een van de grafieken en deze gaat u naar de zoeken in Logboeken. U kunt hier een query voor elk clustergebeurtenissen of prestatiemeteritems.
4. Voer de volgende query. Deze gebeurtenis-id's vindt u in de [knooppunt gebeurtenissen-naslaginformatie](service-fabric-diagnostics-event-generation-operational.md#application-events)

    ```kusto
    ServiceFabricOperationalEvent
    | where EventID >= 25622 and EventID <= 25626
    ```

5. Klik op 'Nieuwe waarschuwingsregel' aan de bovenkant en nu telkens wanneer een gebeurtenis wordt ontvangen op basis van deze query, ontvangt u een waarschuwing in de gekozen methode voor communicatie.

    ![Azure Monitor meldt zich op een nieuwe waarschuwing](media/service-fabric-diagnostics-common-scenarios/oms-create-alert.png)

## <a name="how-can-i-be-alerted-of-application-upgrade-rollbacks"></a>Hoe kan ik gewaarschuwd van application upgrade terugdraaiacties?

1. Voer op de dezelfde zoeken in Logboeken-venster als voordat de volgende query uit voor upgrade terugdraaiacties. Deze gebeurtenis-id's zijn gevonden onder [toepassingsverwijzing gebeurtenissen](service-fabric-diagnostics-event-generation-operational.md#application-events)

    ```kusto
    ServiceFabricOperationalEvent
    | where EventID == 29623 or EventID == 29624
    ```

2. Klik op 'Nieuwe waarschuwingsregel' aan de bovenkant en telkens wanneer een gebeurtenis wordt ontvangen op basis van deze query, wordt u nu een waarschuwing ontvangt.

## <a name="how-do-i-see-container-metrics"></a>Hoe kan ik container metrische gegevens zien?

In de dezelfde weergave met alle grafieken ziet u enkele tegels weergegeven voor de prestaties van uw containers. U moet de Log Analytics-Agent en [Container Monitoring solution](service-fabric-diagnostics-oms-containers.md) voor deze tegels om in te vullen.

![Metrische Containergegevens van log Analytics](media/service-fabric-diagnostics-common-scenarios/containermetrics.png)

>[!NOTE]
>Naar instrument telemetrie van **binnen** de container die u wilt toevoegen de [Application Insights nuget-pakket voor containers](https://github.com/Microsoft/ApplicationInsights-servicefabric#microsoftapplicationinsightsservicefabric--for-service-fabric-lift-and-shift-scenarios).

## <a name="how-can-i-monitor-performance-counters"></a>Hoe kan ik prestatiemeteritems controleren?

1. Nadat u hebt de Log Analytics-agent toegevoegd aan het cluster, moet u de specifieke prestatiemeteritems die u wilt bijhouden toevoegen. Navigeer naar de pagina van de Log Analytics-werkruimte in de portal – op het tabblad werkruimte in het menu links is van de oplossing-pagina.

    ![Log Analytics Workspace Tab](media/service-fabric-diagnostics-common-scenarios/workspacetab.png)

2. Als u op de pagina van de werkruimte bent, klikt u op 'Geavanceerd' in het menu voor dezelfde links.

    ![Geavanceerde instellingen van log Analytics](media/service-fabric-diagnostics-common-scenarios/advancedsettingsoms.png)

3. Klik op gegevens > Windows-prestatiemeteritems (gegevens > Linux-prestatiemeteritems voor Linux-machines) om te beginnen met het verzamelen van specifieke prestatiemeteritems van uw knooppunten via de Log Analytics-agent. Hier volgen enkele voorbeelden van de indeling voor items om toe te voegen

   * `.NET CLR Memory(<ProcessNameHere>)\\# Total committed Bytes`
   * `Processor(_Total)\\% Processor Time`

     In de Quick Start, VotingData en VotingWeb zijn de procesnamen die worden gebruikt, dus deze items bijhouden zou er als volgt uitzien

   * `.NET CLR Memory(VotingData)\\# Total committed Bytes`
   * `.NET CLR Memory(VotingWeb)\\# Total committed Bytes`

     ![Log Analytics-prestatiemeteritems](media/service-fabric-diagnostics-common-scenarios/omsperfcounters.png)

4. Dit wordt kunt u zien hoe uw infrastructuur voor uw workloads verwerkt en relevante waarschuwingen op basis van Resourcegebruik kunt instellen. U wilt bijvoorbeeld – Stel in een waarschuwing als het totale processorgebruik meer dan 90% of minder dan 5 is %. De Tellernaam van de die u voor dit gebruiken zou is "% processortijd." U kunt dit doen door het maken van een waarschuwingsregel voor de volgende query uit:

    ```kusto
    Perf | where CounterName == "% Processor Time" and InstanceName == "_Total" | where CounterValue >= 90 or CounterValue <= 5.
    ```

## <a name="how-do-i-track-performance-of-my-reliable-services-and-actors"></a>Hoe kan ik de prestaties van mijn Reliable Services en actoren bijhouden

Voor het volgen van de prestaties van Reliable Services of actoren in uw toepassingen, moet u ook de Service Fabric-Actor, Actormethode, Service en Service-methode prestatiemeteritems verzamelen. Hier volgen enkele voorbeelden van betrouwbare service en de actor te verzamelen prestatiemeteritems

>[!NOTE]
>Service Fabric-prestatiemeteritems die momenteel door de Log Analytics-agent kan niet worden verzameld, maar kunnen worden verzameld door [andere diagnostische oplossingen](service-fabric-diagnostics-partners.md)

* `Service Fabric Service(*)\\Average milliseconds per request`
* `Service Fabric Service Method(*)\\Invocations/Sec`
* `Service Fabric Actor(*)\\Average milliseconds per request`
* `Service Fabric Actor Method(*)\\Invocations/Sec`

Controleer deze koppelingen voor de volledige lijst prestatiemeteritems op betrouwbare [Services](service-fabric-reliable-serviceremoting-diagnostics.md) en [actoren](service-fabric-reliable-actors-diagnostics.md)

## <a name="next-steps"></a>Volgende stappen

* [Stel waarschuwingen in AI](../azure-monitor/app/alerts.md) om te worden geïnformeerd over wijzigingen in de prestaties of gebruik
* [Slimme detectie in Application Insights](../azure-monitor/app/proactive-diagnostics.md) voert een proactieve analyse van de telemetrie wordt verzonden naar AI om u te waarschuwen voor mogelijke prestatieproblemen
* Meer informatie over Azure Monitor logboeken [waarschuwingen](../log-analytics/log-analytics-alerts.md) voor detectie en diagnostiek.
* Logboeken van Azure Monitor biedt een gateway (http-doorsturen Proxy) die kan worden gebruikt om gegevens te verzenden naar Azure Monitor-logboeken voor clusters van on-premises. Meer informatie over die in [computers zonder toegang tot het Internet verbinding te maken met Azure Monitor-logboeken met behulp van de Log Analytics-gateway](../azure-monitor/platform/gateway.md)
* Familiarized ophalen met de [zoeken en uitvoeren van query's](../log-analytics/log-analytics-log-searches.md) functies die worden aangeboden als onderdeel van Azure Monitor-Logboeken
* Ophalen van een meer gedetailleerd overzicht van Azure Monitor-logboeken en mogelijkheden van het startpakket, lezen [wat Azure Monitor-Logboeken is?](../operations-management-suite/operations-management-suite-overview.md)
