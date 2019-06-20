---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 01/27/2019
ms.author: cynthn
ms.openlocfilehash: ac400c86af8236ff5d67b8b6fbf99f6f4b1d36c9
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67176008"
---
U kunt profiteren van veel mogelijkheden voor het bewaken van uw virtuele machines te verzamelen, weergeven en analyseren van diagnostische en logboekgegevens. Eenvoudige doen [bewaking](../articles/azure-monitor/overview.md) van uw virtuele machine, kunt u het scherm overzicht voor de virtuele machine in Azure portal. U kunt [extensies](../articles/virtual-machines/windows/extensions-features.md) voor het configureren van diagnostische gegevens op uw VM's voor het verzamelen van aanvullende metrische gegevens. U kunt ook meer geavanceerde controle-opties, zoals gebruiken [Application Insights](../articles/azure-monitor/app/app-insights-overview.md) en [Log Analytics](../articles/azure-monitor/log-query/log-query-overview.md).

## <a name="diagnostics-and-metrics"></a>Diagnostische gegevens en metrische gegevens 

U kunt instellen en controleren van de verzameling van [diagnostische gegevens](https://docs.microsoft.com/cli/azure/vm/diagnostics) met behulp van [metrische gegevens](../articles/monitoring-and-diagnostics/monitoring-overview-metrics.md) in Azure portal, de Azure CLI, Azure PowerShell en programming toepassingen Programming Interfaces (API's). U kunt bijvoorbeeld:

- **Bekijk de eenvoudige metrische gegevens voor de virtuele machine.** Op het scherm overzicht van de Azure-portal zijn de algemene metrische gegevens weergegeven CPU-gebruik, netwerkgebruik, totaal schijfbytes en schijfbewerkingen per seconde.

- **Het verzamelen van diagnostische gegevens over opstarten inschakelen en weergeven met behulp van de Azure-portal.** Wanneer u uw eigen installatiekopie naar Azure of een van de platforminstallatiekopieën van het opstart, kunnen er diverse redenen waarom een virtuele machine in een niet-opstartbare status opgehaald. U kunt eenvoudig diagnostische gegevens over opstarten inschakelen wanneer u een virtuele machine maken door te klikken op **ingeschakeld** voor diagnostische gegevens over opstarten onder de sectie bewaking van het instellingenscherm.

    Als VM's opstart, wordt de diagnostische boot-agent opstarten uitvoer vastgelegd en opgeslagen in Azure storage. Deze gegevens kunnen worden gebruikt om opstartproblemen met virtuele machines op te lossen. Diagnostische gegevens over opstarten zijn niet automatisch ingeschakeld wanneer u een virtuele machine van opdrachtregelprogramma's maken. Voordat u diagnostische gegevens over opstarten inschakelt, moet een opslagaccount worden gemaakt voor het opslaan van de opstartlogboeken. Als u diagnostische gegevens over opstarten in Azure portal inschakelt, wordt automatisch een opslagaccount voor u gemaakt.

    Als u kunt diagnostische gegevens over opstarten niet hebt ingeschakeld, wanneer de virtuele machine is gemaakt, kunt u altijd inschakelen dit later met behulp van [Azure CLI](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics), [Azure PowerShell](https://docs.microsoft.com/powershell/module/az.compute/set-azvmbootdiagnostic), of een [Azure Resource Manager-sjabloon](../articles/virtual-machines/windows/extensions-diagnostics-template.md).

- **Het verzamelen van Gast OS diagnostische gegevens inschakelen.** Wanneer u een virtuele machine maakt, hebt u de mogelijkheid op het instellingenscherm Gast OS diagnostische gegevens inschakelen. Wanneer u het verzamelen van diagnostische gegevens, schakel de [IaaSDiagnostics-extensie voor Linux](../articles/virtual-machines/linux/diagnostic-extension.md) of de [IaaSDiagnostics-extensie voor Windows](../articles/virtual-machines/windows/ps-extensions-diagnostics.md) wordt toegevoegd aan de virtuele machine, waarmee u voor het verzamelen van aanvullende de gegevens voor de schijf, de CPU en geheugen.

    U kunt met behulp van de gegevens van de verzamelde diagnostics, automatisch schalen configureren voor uw VM's. U kunt ook de logboeken voor het opslaan van de gegevens en waarschuwingen instellen om te laten u weten wanneer prestaties helemaal niet configureren.

## <a name="alerts"></a>Waarschuwingen

U kunt maken [waarschuwingen](../articles/azure-monitor/platform/alerts-overview.md) op basis van specifieke maatstaven voor prestaties. Voorbeelden van de problemen die u kunt worden gewaarschuwd bij zijn bij het gemiddelde CPU-gebruik een bepaalde drempelwaarde overschrijdt of beschikbare vrije schijfruimte onder een bepaalde hoeveelheid komt. Waarschuwingen kunnen worden geconfigureerd in de [Azure-portal](../articles/azure-monitor/platform/alerts-classic-portal.md), met [Azure PowerShell](../articles/azure-monitor/platform/alerts-classic-portal.md#with-powershell), of de [Azure CLI](../articles/azure-monitor/platform/alerts-classic-portal.md#with-azure-cli).

## <a name="azure-service-health"></a>Azure Service Health

[Azure Service Health](../articles/service-health/service-health-overview.md) biedt gepersonaliseerde begeleiding en ondersteuning bij problemen in Azure-services gevolgen voor u, en helpt bij het voorbereiden voor toekomstige gepland onderhoud. Azure Service Health waarschuwt u en uw teams met behulp van gerichte en flexibele meldingen.

## <a name="azure-resource-health"></a>Azure Resource Health

[Azure Resource health](../articles/service-health/resource-health-overview.md) helpt u bij het diagnosticeren en ondersteuning krijgen wanneer een Azure-probleem gevolgen heeft voor uw resources. Het biedt u informatie over de huidige en eerdere status van uw resources en helpt u problemen verhelpen. Resource Health biedt technische ondersteuning als u hulp nodig heeft bij problemen met Azure-services.

## <a name="azure-activity-log"></a>Azure-activiteitenlogboek

De [Azure Activity Log](../articles/azure-monitor/platform/activity-logs-overview.md) is een abonnementlogboek die biedt inzicht in gebeurtenissen op abonnementsniveau die hebben plaatsgevonden in Azure. Het logboek omvat een scala aan gegevens van operationele gegevens van de Azure Resource Manager-updates op Service Health-gebeurtenissen. U kunt klikken op activiteitenlogboek in Azure portal om het logboek voor uw virtuele machine weer te geven.

Er zijn enkele dingen die u met het activiteitenlogboek doen kunt:

- Maak een [waarschuwing voor een activiteitenlogboek gebeurtenis](../articles/azure-monitor/platform/activity-logs-overview.md).
- [Deze Stream naar een Event Hub](../articles/azure-monitor/platform/activity-logs-stream-event-hubs.md) voor opname van een service van derden of aangepaste analyseoplossing zoals Power BI.
- Analyseren in Power BI met behulp van de [Power BI-inhoudspakket](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs/).
- [Sla deze op een storage-account](../articles/azure-monitor/platform/archive-activity-log.md) voor archivering of handmatige controle. U kunt de bewaartijd (in dagen) met behulp van het logboek-profiel opgeven.

U kunt ook toegang tot gegevens van een activiteitenlogboek via [Azure PowerShell](https://docs.microsoft.com/powershell/module/azurerm.insights/), wordt de [Azure CLI](https://docs.microsoft.com/cli/azure/monitor), of [Monitor REST API's](https://docs.microsoft.com/rest/api/monitor/).

[Diagnostische logboeken in Azure](../articles/azure-monitor/platform/diagnostic-logs-overview.md) zijn logboeken van uw virtuele machine met uitgebreide, regelmatig gegevens over de werking ervan. Diagnoselogboeken verschillen van het activiteitenlogboek door te geven inzicht in bewerkingen die zijn uitgevoerd binnen de virtuele machine.

Enkele van de dingen die u met diagnostische logboeken doen kunt zijn onder andere:

- [Opslaan naar een opslagaccount](../articles/azure-monitor/platform/archive-diagnostic-logs.md) voor controle of handmatige controle. U kunt de bewaartijd (in dagen) met behulp van de instellingen voor Resourcediagnose opgeven.
- [Deze Stream naar Event Hubs](../articles/azure-monitor/platform/diagnostic-logs-stream-event-hubs.md) voor opname van een service van derden of aangepaste analyseoplossing zoals Power BI.
- Analyseren met [Log Analytics](../articles/log-analytics/log-analytics-azure-storage.md).

## <a name="advanced-monitoring"></a>Geavanceerde controle

- [Azure Monitor](../articles/azure-monitor/overview.md) is een service die uw cloud en on-premises omgevingen voor het onderhouden van hun beschikbaarheid en prestaties. Het biedt een uitgebreide oplossing voor het verzamelen, analyseren en uitvoeren van telemetrie van uw cloud en het on-premises omgevingen. Het helpt u begrijpen hoe uw toepassingen presteren en stelt proactief problemen vast die betrekking hebben op de toepassingen en de resources waarvan ze afhankelijk zijn. U kunt een extensie installeren op een [Linux-VM](../articles/virtual-machines/linux/extensions-oms.md) of een [Windows VM](../articles/virtual-machines/windows/extensions-oms.md) waarmee de Log Analytics-agent voor het verzamelen van logboekgegevens en opslaan in een Log Analytics-werkruimte wordt geïnstalleerd.

    Voor Windows en Linux-VM's is de aanbevolen methode voor het verzamelen van Logboeken door de Log Analytics-agent te installeren. De eenvoudigste manier om de Log Analytics-agent installeren op een virtuele machine is via de [Log Analytics VM-extensie](../articles/log-analytics/log-analytics-azure-vm-extension.md). De extensie vereenvoudigt het installatieproces en configureert automatisch de agent voor het verzenden van gegevens naar de Log Analytics-werkruimte die u opgeeft. De agent wordt bovendien automatisch bijgewerkt, zodat u over de nieuwste functies en correcties beschikt.

- [Network Watcher](../articles/network-watcher/network-watcher-monitoring-overview.md) kunt u voor het bewaken van uw virtuele machine en alle bijbehorende resources zoals ze betrekking op het netwerk waarin ze zich hebben bevinden. U kunt de extensie voor Network Watcher-Agent installeren op een [Linux-VM](../articles/virtual-machines/linux/extensions-nwa.md) of een [Windows VM](../articles/virtual-machines/windows/extensions-nwa.md).

- [Azure Monitor voor virtuele machines](../articles/azure-monitor/insights/vminsights-overview.md) bewaakt uw Azure virtual machines (VM) op schaal door het analyseren van de prestaties en status van uw Windows en Linux-VM's, met inbegrip van hun verschillende processen en de gekoppelde afhankelijkheden van andere bronnen en externe processen. 

## <a name="next-steps"></a>Volgende stappen
- Doorloop de stappen in [bewaken van een Windows-Machine met Azure PowerShell](../articles/virtual-machines/windows/tutorial-monitoring.md) of [bewaken van een virtuele Linux-Machine met de Azure CLI](../articles/virtual-machines/linux/tutorial-monitoring.md).
- Meer informatie over de best practices om [controle en diagnose](https://docs.microsoft.com/azure/architecture/best-practices/monitoring).
