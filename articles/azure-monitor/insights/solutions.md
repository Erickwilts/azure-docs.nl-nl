---
title: Bewakings oplossingen in Azure Monitor | Microsoft Docs
description: Bewakings oplossingen in Azure Monitor zijn een verzameling regels voor logica, visualisatie en gegevens aanschaf die gegevens over een bepaald probleem gebied hebben gedraaid.  Dit artikel bevat informatie over het installeren en gebruiken van bewakings oplossingen.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: f029dd6d-58ae-42c5-ad27-e6cc92352b3b
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 08/13/2019
ms.author: bwren
ms.openlocfilehash: 6932f9f302bf1816842d78a662f01ebaafed8686
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/13/2019
ms.locfileid: "68989208"
---
# <a name="monitoring-solutions-in-azure-monitor"></a>Bewakings oplossingen in Azure Monitor
Bewakings oplossingen maken gebruik van services in azure om meer inzicht te krijgen in de werking van een bepaalde toepassing of service. Dit artikel bevat een kort overzicht van de bewakings oplossingen in Azure en informatie over het gebruik en de installatie ervan.

> [!NOTE]
> Bewakings oplossingen werden voorheen beheer oplossingen genoemd.

Bewakings oplossingen verzamelen doorgaans logboek gegevens en bieden query's en weer gaven om verzamelde gegevens te analyseren. Ze kunnen ook gebruikmaken van andere services zoals Azure Automation om uit te voeren met betrekking tot de toepassing of service.

U kunt bewakings oplossingen toevoegen aan Azure Monitor voor alle toepassingen en services die u gebruikt. Ze zijn doorgaans beschikbaar er worden geen kosten, maar verzamelen gegevens kosten voor het gebruik kan worden aangeroepen. Naast de oplossingen die worden geleverd door Microsoft, partners en klanten kunnen [beheeroplossingen maken](solutions-creating.md) moet worden gebruikt in hun eigen omgeving of beschikbaar gesteld aan klanten via de community.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="use-monitoring-solutions"></a>Bewakingsoplossingen gebruiken
Open de pagina **overzicht** in azure monitor om een tegel weer te geven voor elke oplossing die in de werk ruimte is geïnstalleerd. 

1. Meld u aan bij Azure Portal.
1. Open **alle services** en zoek **Monitor**.
1. Onder de **Insights** in het menu **meer**.
1. Gebruik de vervolgkeuzelijsten boven aan het scherm om de werkruimte of het tijdsbereik gebruikt voor de tegels te wijzigen.
1. Klik op de tegel voor een oplossing voor het openen van de weergave waarin meer gedetailleerde analyse de verzamelde gegevens.

![Overzicht](media/solutions/overview.png)

Bewakings oplossingen kunnen meerdere typen Azure-resources bevatten en u kunt alle resources die zijn opgenomen in een oplossing, net als elke andere resource bekijken. Alle logboek query's die in de oplossing zijn opgenomen, worden bijvoorbeeld vermeld onder **oplossingen query's** in [query Explorer](../log-query/get-started-portal.md#load-queries) . u kunt deze query's gebruiken bij het uitvoeren van ad hoc analyses met [logboek query's](../log-query/log-query-overview.md).

## <a name="list-installed-monitoring-solutions"></a>Geïnstalleerde bewakings oplossingen weer geven 
Gebruik de volgende procedure om de bewakings oplossingen weer te geven die in uw abonnement zijn geïnstalleerd.

1. Meld u aan bij Azure Portal.
1. Open **alle services** en zoek **oplossingen**.
4. Oplossingen die zijn geïnstalleerd in al uw werkruimten worden weergegeven. De naam van de oplossing wordt gevolgd door de naam van de werk ruimte waarin deze is geïnstalleerd.
1. Gebruik de vervolgkeuzelijsten boven aan het scherm om te filteren op abonnement of resourcegroep.


![Vermeld alle oplossingen](media/solutions/list-solutions-all.png)

Klik op de naam van een oplossing voor het openen van de pagina overzicht. Deze pagina wordt weergegeven van alle weergaven die zijn opgenomen in de oplossing en biedt verschillende opties voor de oplossing zelf en de bijbehorende werkruimte. De overzichtspagina voor een oplossing met behulp van een van de procedures boven aan de lijst met oplossingen weergeven en klik vervolgens op de naam van de oplossing.

![Eigenschappen van oplossing](media/solutions/solution-properties.png)



## <a name="install-a-monitoring-solution"></a>Een bewakings oplossing installeren
Bewakings oplossingen van micro soft en partners zijn beschikbaar via [Azure Marketplace](https://azuremarketplace.microsoft.com). U kunt zoeken naar beschikbare oplossingen en installeren met behulp van de volgende procedure. Wanneer u een oplossing installeert, moet u een [Log Analytics-werkruimte](../platform/manage-access.md) installatie van de oplossing en waar de gegevens worden verzameld.

1. Uit de [lijst met oplossingen voor uw abonnement](#list-installed-monitoring-solutions), klikt u op **toevoegen**.
1. Blader of zoek naar een oplossing. U kunt ook naar oplossingen bladeren via [deze zoek koppeling](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/management-tools?page=1&subcategories=management-solutions).
1. Zoek de gewenste bewakings oplossing en lees de beschrijving.
1. Klik op **maken** om het installatieproces te starten.
1. Wanneer het installatie proces wordt gestart, wordt u gevraagd om de Log Analytics-werk ruimte op te geven en de vereiste configuratie voor de oplossing op te geven.

![Installeren van een oplossing](media/solutions/install-solution.png)

### <a name="install-a-solution-from-the-community"></a>Installeren van een oplossing van de community
Leden van de community kunnen oplossingen voor gegevensbeheer voor Azure-Snelstartsjablonen indienen. U kunt deze oplossingen rechtstreeks installeren of download deze sjablonen later installeren.

1. Volg de procedure beschreven in [Log Analytics-werkruimte en het Automation-account](#log-analytics-workspace-and-automation-account) om een werkruimte en account te koppelen.
2. Ga naar [Azure-Snelstartsjablonen](https://azure.microsoft.com/documentation/templates/). 
3. Zoeken naar een oplossing waarin u geïnteresseerd bent.
4. Selecteer de oplossing in de resultaten om de details ervan weer te geven.
5. Klik op de **implementeren in Azure** knop.
6. U wordt gevraagd om informatie te geven, zoals de resourcegroep en locatie samen met waarden voor parameters die in de oplossing.
7. Klik op **aankoop** voor het installeren van de oplossing.


## <a name="log-analytics-workspace-and-automation-account"></a>Log Analytics-werkruimte en het Automation-account
Voor alle bewakings oplossingen is een [log Analytics-werk ruimte](../platform/manage-access.md) vereist voor het opslaan van gegevens die zijn verzameld door de oplossing en het vinden van zoek opdrachten in Logboeken en weer gaven. Sommige oplossingen ook vereisen een [Automation-account](../../automation/automation-security-overview.md#automation-account-overview) runbooks en verwante resources bevat. De werkruimte en account moeten voldoen aan de volgende vereisten.

* Elke installatie van een oplossing kan alleen een Log Analytics-werkruimte en een Automation-account gebruiken. U kunt de oplossing afzonderlijk installeren in meerdere werkruimten.
* Als een oplossing een Automation-account vereist, moeten klikt u vervolgens de Log Analytics-werkruimte en het Automation-account worden gekoppeld aan elkaar. Een Log Analytics-werkruimte kan alleen worden gekoppeld aan een Automation-account en een Automation-account kan slechts aan één Log Analytics-werkruimte zijn gekoppeld.
* De Log Analytics-werkruimte en het Automation-account om te worden gekoppeld, moeten zich in dezelfde resourcegroep en regio. De uitzondering is een werkruimte in de regio VS-Oost en Automation-account in VS-Oost 2.

### <a name="create-a-link-between-a-log-analytics-workspace-and-automation-account"></a>Een koppeling tussen een Log Analytics-werkruimte en een Automation-account maken
Hoe u de Log Analytics-werkruimte en het Automation-account opgeven, is afhankelijk van de installatiemethode voor uw oplossing.

* Wanneer u een oplossing via Azure Marketplace installeert, wordt u gevraagd voor een werkruimte en het Automation-account. De koppeling tussen beide wordt gemaakt als ze niet al is gekoppeld.
* Voor oplossingen buiten de Azure Marketplace, moet u de Log Analytics-werkruimte en het Automation-account koppelen voordat de installatie van de oplossing. U kunt dit doen door het selecteren van een oplossing in de Azure Marketplace en de Log Analytics-werkruimte en het Automation-account te selecteren. U hebt geen daadwerkelijk de oplossing installeren omdat de koppeling wordt gemaakt als de Log Analytics-werkruimte en het Automation-account zijn geselecteerd. Nadat de koppeling is gemaakt, kunt u die Log Analytics-werkruimte en het Automation-account gebruiken voor elke oplossing.

### <a name="verify-the-link-between-a-log-analytics-workspace-and-automation-account"></a>Controleer of de koppeling tussen een Log Analytics-werkruimte en een Automation-account
U kunt controleren of de koppeling tussen een Log Analytics-werkruimte en een Automation-account met behulp van de volgende procedure.

1. Selecteer het Automation-account in Azure portal.
1. Schuif naar de **gerelateerde Resources** gedeelte van het menu.
1. Als de **werkruimte** instelling is ingeschakeld, wordt deze account is gekoppeld aan een Log Analytics-werkruimte. U kunt klikken op **werkruimte** om de details van de werkruimte weer te geven.

## <a name="remove-a-monitoring-solution"></a>Een bewakings oplossing verwijderen
Als u wilt verwijderen van een oplossing voor is geïnstalleerd, zoekt u deze in de [lijst met geïnstalleerde oplossingen](#list-installed-monitoring-solutions). Klik op de naam van de oplossing voor het openen van de pagina Samenvatting en klik vervolgens op **verwijderen**.


## <a name="next-steps"></a>Volgende stappen
* Een [lijst met bewakings oplossingen van micro soft](solutions-inventory.md)ophalen.
* Meer informatie over het [maken van query's](../log-query/log-query-overview.md) voor het analyseren van gegevens die zijn verzameld door bewakings oplossingen.

