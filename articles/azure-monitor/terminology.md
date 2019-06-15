---
title: Updates van de terminologie Azure Monitor | Microsoft Docs
description: Recente terminologie wijzigingen aangebracht in de bewakingsservices van Azure beschreven.
author: bwren
manager: carmonm
editor: tysonn
services: azure-monitor
documentationcenter: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/18/2019
ms.author: bwren
ms.openlocfilehash: 8f645f7d569546a8362d0149806a2b4636567fd0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "61086737"
---
# <a name="azure-monitor-naming-and-terminology-changes"></a>Azure Monitor-wijzigingen voor naamgeving en terminologie
Belangrijke wijzigingen zijn aangebracht naar Azure Monitor onlangs met verschillende services worden geconsolideerd vereenvoudigt bewaking voor Azure-klanten. In dit artikel beschrijft de naam van de recente en terminologie wijzigingen in de documentatie bij Azure Monitor.

## <a name="february-2019---log-analytics-terminology"></a>Februari 2019 - Log Analytics-terminologie
Na de consolidatie van andere services onder Azure Monitor geleid we de volgende stap door het wijzigen van de belangrijkste termen in onze documentatie om te beschrijven beter de Azure Monitor-service en de verschillende onderdelen. 

### <a name="log-analytics"></a>Log Analytics
Azure Monitor-logboek nog steeds gegevens is die zijn opgeslagen in een Log Analytics-werkruimte en nog steeds worden verzameld en geanalyseerd door de dezelfde Log Analytics-service, maar we wijzigen de term _Log Analytics_ op talloze plaatsen op _Azure Monitor-Logboeken_ . Deze term beter de functie in Azure Monitor en biedt betere consistentie [metrische gegevens in Azure Monitor](platform/data-platform-metrics.md).

De term _melden analytics_ nu is voornamelijk van toepassing op de pagina in Azure portal gebruikt om te schrijven en query's uitvoeren en analyseren van logboekgegevens. Het is functioneel equivalent aan het [metrics explorer](platform/metrics-charts.md), dit is de pagina in Azure portal gebruikt om metrische gegevens te analyseren.

### <a name="log-analytics-workspaces"></a>Log Analytics-werkruimten
[Werkruimten](platform/manage-access.md) die houdt logboekgegevens in Azure Monitor nog steeds worden aangeduid als Log Analytics-werkruimten. De **Log Analytics** in het menu in de Azure-portal is gewijzigd in **Log Analytics-werkruimten** en is de locatie waar u [maken van nieuwe werkruimten](learn/quick-create-workspace.md) en gegevensbronnen configureren. Analyseer uw logboeken en andere bewakingsgegevens in **Azure Monitor** en configureren uw werkruimte in **Log Analytics-werkruimten**.

### <a name="management-solutions"></a>Beheeroplossingen
[Beheeroplossingen](insights/solutions.md) hebt gewijzigd in _bewakingsoplossingen_, waarin de functionaliteit hiervan beter wordt beschreven.


## <a name="august-2018---consolidation-of-monitoring-services-into-azure-monitor"></a>Augustus 2018 - consolidatie van het bewaken van services in Azure Monitor
Log Analytics en Application Insights zijn samengevoegd in Azure Monitor voor één geïntegreerde ervaring voor het bewaken van Azure-resources en hybride omgevingen. Er is geen functionaliteit is verwijderd uit deze services en gebruikers kunnen de dezelfde scenario's die ze hebben altijd voltooid zonder gegevensverlies of inbreuk op een van de functies uitvoeren.

Documentatie voor elk van deze services is geconsolideerd in één set van inhoud voor Azure Monitor. Dit kan helpen de lezer bij het vinden van alle inhoud voor een bepaald controle scenario op één locatie in plaats van dat u hoeft te verwijzen naar meerdere sets met inhoud. Als de geconsolideerde service zich verder ontwikkelt, wordt de inhoud beter worden geïntegreerd.

Andere functies die als onderdeel van Log Analytics zoals agents en weergaven zijn beschouwd is als functies van Azure Monitor ook verplaatst. De functionaliteit is niet veranderd dan potentiële verbeteringen hun ervaring in Azure portal.


## <a name="april-2018---retirement-of-operations-management-suite-brand"></a>April 2018 - merk buiten gebruik stellen van Operations Management Suite
Operations Management Suite (OMS) is een bundeling van de volgende Azure management-services voor licentieverlening doeleinden:

- Application Insights
- Azure Automation
- Azure Backup
- Log Analytics
- Site Recovery

[Nieuwe prijzen is geïntroduceerd voor deze services](https://azure.microsoft.com/blog/introducing-a-new-way-to-purchase-azure-monitoring-services/), en de OMS-bundeling is niet meer beschikbaar voor nieuwe klanten. Geen van de services die deel van OMS uitmaken hebt gewijzigd, met uitzondering van de consolidatie in Azure Monitor die hierboven worden beschreven. 




## <a name="next-steps"></a>Volgende stappen

- Lees een [overzicht van Azure Monitor](overview.md) met de beschrijving van de verschillende onderdelen en functies.
- Meer informatie over de [overgang van de OMS-portal](../log-analytics/log-analytics-oms-portal-transition.md).