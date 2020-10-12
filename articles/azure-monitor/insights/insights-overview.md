---
title: Overzicht van inzichten in Azure Monitor | Microsoft Docs
description: Inzichten bieden een aangepaste bewakings ervaring in Azure Monitor voor bepaalde toepassingen en services. In dit artikel vindt u een korte beschrijving van elk van de inzichten die momenteel beschikbaar zijn.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 05/22/2019
ms.openlocfilehash: e3eefb97c85b3865abbf116b9912dc45f6db8ce8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "87326865"
---
# <a name="overview-of-insights-in-azure-monitor"></a>Overzicht van inzichten in Azure Monitor
Inzichten bieden een aangepaste bewakings ervaring voor bepaalde toepassingen en services. Ze slaan gegevens op in het [Azure monitor-gegevens platform](../platform/data-platform.md) en maken gebruik van andere Azure monitor functies voor analyse en waarschuwingen, maar kunnen ook extra gegevens verzamelen en een unieke gebruikers ervaring bieden in de Azure Portal. Krijg toegang tot inzichten via de sectie **inzichten** van het menu Azure monitor in het Azure Portal.


In de volgende secties vindt u een korte beschrijving van de inzichten die momenteel beschikbaar zijn in Azure Monitor. Raadpleeg de gedetailleerde documentatie voor meer informatie.

## <a name="application-insights"></a>Application Insights
Application Insights is een uitbreidbare APM-service (Application Performance Management) voor webontwikkelaars op meerdere platforms. Hiermee kunt u uw livewebtoepassing controleren. Het werkt voor toepassingen op een groot aantal verschillende platformen, waaronder .NET, Node.js en Java EE, lokaal gehost, hybride of een open bare Cloud. Het kan ook worden geïntegreerd met uw DevOps-proces en heeft verbindings punten naar verschillende ontwikkel hulpprogramma's.

Zie [Wat is Application Insights?](../app/app-insights-overview.md).

![Application Insights](media/insights-overview/app-insights.png)

## <a name="azure-monitor-for-containers"></a>Azure Monitor voor containers
Azure Monitor voor containers bewaakt de prestaties van container werkbelastingen die zijn geïmplementeerd op Azure Container Instances of beheerde Kubernetes-clusters die worden gehost op de Azure Kubernetes-service (AKS). Het bewaken van uw containers is van cruciaal belang, vooral wanneer u een productie cluster op schaal uitvoert met meerdere toepassingen.

Zie [Azure monitor voor containers Overview](./container-insights-overview.md)(Engelstalig).

![Azure Monitor voor containers](media/insights-overview/container-insights.png)

## <a name="azure-monitor-for-resource-groups-preview"></a>Azure Monitor voor resource groepen (preview-versie)
Azure Monitor voor resource groepen helpt bij het sorteren en diagnosticeren van eventuele problemen die uw afzonderlijke bronnen ondervinden, terwijl de status en prestaties van de resource groep als geheel worden geboden.

Zie [resource groepen bewaken met Azure monitor (preview)](./resource-group-insights.md).

![Azure Monitor voor resource groepen](media/insights-overview/resource-group-insights.png)

## <a name="azure-monitor-for-vms-preview"></a>Azure Monitor voor VM's (preview-versie)
Azure Monitor voor VM's bewaakt uw virtuele Azure-machines (VM) en virtuele-machineschaalsets op schaal. De service analyseert de prestaties en status van uw Windows- en Linux-VM's en bewaakt hun processen en afhankelijkheden van andere resources en externe processen.

Zie [Wat is Azure monitor voor VM's?](vminsights-overview.md)

![Azure Monitor voor virtuele machines](media/insights-overview/vm-insights.png)

## <a name="azure-monitor-for-networks-preview"></a>Azure Monitor voor netwerken (preview-versie)
[Azure monitor voor netwerken](network-insights-overview.md) biedt een uitgebreid overzicht van de status en metrische gegevens voor al uw netwerk bronnen. De geavanceerde zoek functie helpt u bij het identificeren van bron afhankelijkheden, het inschakelen van scenario's zoals het identificeren van resources die als host fungeren voor uw website, door eenvoudigweg te zoeken naar de naam van uw website.

![Azure Monitor voor netwerken](media/insights-overview/network-insights.png)

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over het [Azure monitor-gegevens platform](../platform/data-platform.md) dat wordt gebruikt door inzichten.
* Meer informatie over de verschillende [gegevens bronnen die worden gebruikt door Azure monitor](../platform/data-sources.md) en de verschillende soorten gegevens die worden verzameld door elk van de inzichten.

