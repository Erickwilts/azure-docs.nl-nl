---
title: 'Routefilters voor Microsoft-peering configureren: Met Azure ExpressRoute - Portal | Microsoft Docs'
description: In dit artikel wordt beschreven hoe u routefilters voor Microsoft-peering met behulp van de Azure-portal configureren.
services: expressroute
author: ganesr
ms.service: expressroute
ms.topic: article
ms.date: 07/01/2019
ms.author: ganesr
ms.custom: seodec18
ms.openlocfilehash: 55b5207e4c05712b4d96d4272f9ae193cc4c1720
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2019
ms.locfileid: "67508606"
---
# <a name="configure-route-filters-for-microsoft-peering-azure-portal"></a>Routefilters voor Microsoft-peering configureren: Azure Portal
> [!div class="op_single_selector"]
> * [Azure Portal](how-to-routefilter-portal.md)
> * [Azure PowerShell](how-to-routefilter-powershell.md)
> * [Azure-CLI](how-to-routefilter-cli.md)
> 

Met routefilters kunt u een subset van ondersteunde services gebruiken via Microsoft-peering. De stappen in dit artikel kunt u configureren en beheren van routefilters voor ExpressRoute-circuits.

Dynamics 365-services, en Office 365-services zoals Exchange Online, SharePoint Online en Skype voor bedrijven en Azure-services zoals storage en SQL-database zijn toegankelijk via Microsoft-peering. Wanneer Microsoft-peering is geconfigureerd in een ExpressRoute-circuit, worden alle voorvoegsels die betrekking hebben op deze services worden geadverteerd via de BGP-sessies worden tot stand gebracht. Er wordt aan elk voorvoegsel een BGP-communitywaarde gekoppeld om de service te identificeren die via het voorvoegsel wordt aangeboden. Zie voor een lijst van de BGP-Communitywaarden en deze worden toegewezen aan services, [BGP-community's](expressroute-routing.md#bgp).

Als u de verbinding met alle services vereist, wordt een groot aantal voorvoegsels worden geadverteerd via BGP. Dit verhoogt aanzienlijk de grootte van de routetabellen onderhouden door routers in uw netwerk. Als u van plan alleen een subset van services die worden aangeboden via Microsoft bent-peering, kunt u de grootte van uw routetabellen op twee manieren verkleinen. U kunt:

- Ongewenste voorvoegsels filteren door routefilters zijn toegepast op de BGP-community's. Dit is een standaardprocedure voor netwerken en wordt vaak gebruikt in veel netwerken.

- Routefilters definiëren en deze toepassen op uw ExpressRoute-circuit. Een routefilter is een nieuwe resource waarmee u de lijst met services die u van plan bent om te gebruiken via Microsoft-peering te selecteren. ExpressRoute-routers verzenden alleen de lijst met voorvoegsels die deel uitmaken van de services die zijn geïdentificeerd in de routefilter.

### <a name="about"></a>Over routefilters

Wanneer Microsoft-peering is geconfigureerd op uw ExpressRoute-circuit, stel de Microsoft-randrouters een combinatie van een BGP-sessies met de randrouters (kunt kiezen of uw connectiviteitsprovider). Er worden geen routes geadverteerd naar uw netwerk. Als u dit wel wilt doen, moet u een routefilter koppelen.

Via een routefilter kunt u services identificeren die u wilt gebruiken via Microsoft-peering op uw ExpressRoute-circuit. Het is in feite een lijst van alle de BGP-Communitywaarden die u wilt toestaan. Zodra er een routefilter is gedefinieerd en aan een ExpressRoute-circuit is gekoppeld, worden alle voorvoegsels die overeenkomen met de BGP-communitywaarden naar uw netwerk geadverteerd.

Als u routefilters met Office 365-services op deze koppelen, moet u gemachtigd zijn om te gebruiken Office 365-services via ExpressRoute. Als u bent niet gemachtigd om te gebruiken Office 365-services via ExpressRoute, mislukt de bewerking routefilters koppelen. Zie voor meer informatie over het autorisatieproces [Azure ExpressRoute voor Office 365](https://support.office.com/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd). De verbinding met Dynamics 365-services is niet vereist voor elke toestemming.

> [!IMPORTANT]
> Microsoft-peering van ExpressRoute-circuits die zijn geconfigureerd vóór 1 augustus 2017 hebben alle service-voorvoegsels geadverteerd via Microsoft-peering, zelfs als routefilters zijn niet gedefinieerd. Microsoft-peering van ExpressRoute-circuits die zijn geconfigureerd op of na 1 augustus 2017 hebben geen voorvoegsels geadverteerd totdat een routefilter is gekoppeld aan het circuit.
> 
> 

### <a name="workflow"></a>Werkstroom

Als u verbinding kan maken met services via Microsoft-peering, moet u de volgende configuratiestappen voltooien:

- U moet een actief ExpressRoute-circuit met Microsoft-peering ingerichte hebben. U kunt de volgende instructies gebruiken voor het uitvoeren van deze taken:
  - [Maken van een ExpressRoute-circuit](expressroute-howto-circuit-portal-resource-manager.md) en laat het circuit ingeschakeld door de connectiviteitsprovider voordat u doorgaat. Het ExpressRoute-circuit moet zich in een status ingericht en zijn ingeschakeld.
  - [Microsoft-peering maken](expressroute-howto-routing-portal-resource-manager.md) als u de BGP-sessie rechtstreeks beheert. Of vraag uw connectiviteitsprovider Microsoft-peering voor uw circuit inrichten.

-  U moet maken en configureren van een routefilter.
    - Identificeer de services die u verbruikt via Microsoft-peering
    - De lijst met BGP-Communitywaarden die zijn gekoppeld aan de services identificeren
    - Een regel voor het toestaan van de lijst met voorvoegsels die overeenkomt met de BGP-Communitywaarden maken

-  U moet de routefilter koppelen aan het ExpressRoute-circuit.

## <a name="before-you-begin"></a>Voordat u begint

Voordat u begint met de configuratie, moet u voldoen aan de volgende criteria:

 - Controleer de [vereisten](expressroute-prerequisites.md) en [werkstromen](expressroute-workflows.md) voordat u begint met de configuratie.

 - U moet een actief ExpressRoute-circuit hebben. Volg de instructies voor het [maken van een ExpressRoute-circuit](expressroute-howto-circuit-portal-resource-manager.md) en laat het circuit inschakelen door de connectiviteitsprovider voordat u verder gaat. Het ExpressRoute-circuit moet zich in een status ingericht en zijn ingeschakeld.

 - U moet een actieve Microsoft-peering. Volg de instructies op [maken en de peering-configuratie wijzigen](expressroute-howto-routing-portal-resource-manager.md)


## <a name="prefixes"></a>Stap 1: Een lijst met voorvoegsels en BGP-Communitywaarden ophalen

### <a name="1-get-a-list-of-bgp-community-values"></a>1. Een overzicht van BGP-Communitywaarden

BGP-Communitywaarden die zijn gekoppeld aan services die toegankelijk zijn via Microsoft-peering is beschikbaar in de [routeringsvereisten voor ExpressRoute](expressroute-routing.md) pagina.

### <a name="2-make-a-list-of-the-values-that-you-want-to-use"></a>2. Maak een lijst van de waarden die u wilt gebruiken

Maak een lijst van [BGP-Communitywaarden](expressroute-routing.md#bgp) u wilt gebruiken in de routefilter. 

## <a name="filter"></a>Stap 2: Een routefilter en een filterregel voor een maken

Een routefilter kan slechts één regel, en de regel moet van het type 'Toestaan'. Deze regel kan een lijst met BGP-Communitywaarden die zijn gekoppeld aan deze hebben.

### <a name="1-create-a-route-filter"></a>1. Een routefilter maken
U kunt een routefilter maken door de optie om een nieuwe resource te maken. Klik op **een resource maken** > **netwerken** > **RouteFilter**, zoals wordt weergegeven in de volgende afbeelding:

![Een routefilter maken](./media/how-to-routefilter-portal/CreateRouteFilter1.png)

U moet de routefilter plaatsen in een resourcegroep. 

![Een routefilter maken](./media/how-to-routefilter-portal/CreateRouteFilter.png)

### <a name="2-create-a-filter-rule"></a>2. Een filterregel maken

U kunt toevoegen en bijwerken van regels door het tabblad van de regel beheren voor uw routefilter te selecteren.

![Een routefilter maken](./media/how-to-routefilter-portal/ManageRouteFilter.png)


De services die u wilt verbinding maken met vanuit de vervolgkeuzelijst en opslaan van de regel als u klaar bent, kunt u selecteren.

![Een routefilter maken](./media/how-to-routefilter-portal/AddRouteFilterRule.png)


## <a name="attach"></a>Stap 3: De routefilter koppelen aan een ExpressRoute-circuit

U kunt de routefilter koppelen aan een circuit door de knop 'Circuit toevoegen' te selecteren en het ExpressRoute-circuit in de vervolgkeuzelijst te selecteren.

![Een routefilter maken](./media/how-to-routefilter-portal/AddCktToRouteFilter.png)

Als de connectiviteitsprovider configureert voor uw ExpressRoute-peering circuit het circuit vernieuwen op de blade van ExpressRoute-circuit voordat u de knop 'Circuit toevoegen' selecteren.

![Een routefilter maken](./media/how-to-routefilter-portal/RefreshExpressRouteCircuit.png)

## <a name="tasks"></a>Algemene taken

### <a name="getproperties"></a>Ophalen van de eigenschappen van een routefilter

U kunt eigenschappen van een routefilter weergeven bij het openen van de resource in de portal.

![Een routefilter maken](./media/how-to-routefilter-portal/ViewRouteFilter.png)


### <a name="updateproperties"></a>De eigenschappen van een routefilter bijwerken

U kunt de lijst met BGP-Communitywaarden die zijn gekoppeld aan een circuit hiervoor de knop 'Regel beheren' bijwerken.


![Een routefilter maken](./media/how-to-routefilter-portal/ManageRouteFilter.png)

![Een routefilter maken](./media/how-to-routefilter-portal/AddRouteFilterRule.png) 


### <a name="detach"></a>Een routefilter van een ExpressRoute-circuit ontkoppelen

Als u wilt loskoppelen van een circuit van het routefilter, met de rechtermuisknop op het circuit en klik op 'koppeling'.

![Een routefilter maken](./media/how-to-routefilter-portal/DetachRouteFilter.png) 


### <a name="delete"></a>Een routefilter verwijderen

U kunt een routefilter verwijderen door de verwijderknop selecteren. 

![Een routefilter maken](./media/how-to-routefilter-portal/DeleteRouteFilter.png) 

## <a name="next-steps"></a>Volgende stappen

* Voor meer informatie over ExpressRoute raadpleegt u de [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md).

* Zie voor meer informatie over voorbeelden van routerconfiguratie [routerconfiguraties instellen en beheren van routering](expressroute-config-samples-routing.md). 
