---
title: 'ExpressRoute-connectiviteitsmodellen: Azure| Microsoft Docs'
description: In dit artikel leest u over de verschillende modi voor connectiviteit tussen het netwerk van de klant en Microsoft Azure, Office 365 en Dynamics 365. Klanten kunnen gebruikmaken van MPLS-providers, cloudexchanges en Ethernet-providers.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: cherylmc
ms.custom: seodec18
ms.openlocfilehash: ef53f9d3cefa35624d6f94dc98db4ebe8b6646cd
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60367440"
---
# <a name="expressroute-connectivity-models"></a>ExpressRoute-connectiviteitsmodellen
U kunt op drie verschillende manieren een verbinding maken tussen uw on-premises netwerk en Microsoft Cloud: [CloudExchange-co-locatie](#CloudExchange), [point-to-point Ethernet-verbinding](#Ethernet) en [any-to-any (IPVPN) verbinding](#IPVPN). Connectiviteitsproviders kunnen een of meer connectiviteitsmodellen bieden. Overleg met uw connectiviteitsprovider om na te gaan welk model voor u het meest geschikt is.
<br><br>

![Diagram van ExpressRoute-connectiviteitsmodellen](./media/expressroute-connectivity-models/expressroute-connectivity-models-diagram.png)

## <a name="CloudExchange"></a>Co-locatie op een cloudexchange
Als u zich bevindt op dezelfde locatie als een exchange-cloud, kunt u virtuele overlappende verbindingen met de Microsoft Cloud aanvragen via de Ethernet exchange van de co-locatieprovider. Co-locatieproviders kunnen Laag-2-overlappende verbindingen of beheerde Laag-3 overlappende verbindingen tussen uw infrastructuur in de co-locatiefaciliteit en de Microsoft Cloud aanbieden.

## <a name="Ethernet"></a>Point-to-Point Ethernet-verbindingen
U kunt uw on-premises datacenters/kantoren met de Microsoft Cloud verbinden via point-to-point Ethernet-koppelingen. Point-to-point Ethernet-providers kunnen Laag-2-verbindingen of beheerde Laag-3-verbindingen bieden tussen uw locatie en de Microsoft Cloud.

## <a name="IPVPN"></a>Any-to-any (IPVPN) netwerken
U kunt uw WAN integreren met de Microsoft Cloud. IPVPN-providers (doorgaans MPLS VPN) bieden any-to-any connectiviteit tussen uw filialen en datacenters. De Microsoft Cloud kan ook worden verbonden met uw WAN, zodat het er net zo uitziet als een filiaal. WAN-providers bieden doorgaans beheerde Laag-3-connectiviteit. ExpressRoute-functies en -mogelijkheden zijn identiek in alle bovenstaande connectiviteitsmodellen. 

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over ExpressRoute-verbindingen en -routeringsdomeinen. Zie [ExpressRoute circuits and routing domains](expressroute-circuit-peerings.md) (ExpressRoute-circuits en -routeringsdomeinen).
* Meer informatie over ExpressRoute-functies. Zie [Technisch overzicht van ExpressRoute](expressroute-introduction.md)
* Zoek een serviceprovider Zie [ExpressRoute partners and peering locations](expressroute-locations.md) (ExpressRoute-partners en -peeringlocaties).
* Controleer of aan alle vereisten is voldaan. Zie [ExpressRoute prerequisites](expressroute-prerequisites.md) (Vereisten voor ExpressRoute).
* Raadpleeg de vereisten voor [Routering](expressroute-routing.md), [NAT](expressroute-nat.md) en [QoS](expressroute-qos.md).
* Configureer uw ExpressRoute-verbinding.
  * [Een ExpressRoute-circuit maken](expressroute-howto-circuit-portal-resource-manager.md)
  * [Routering configureren](expressroute-howto-routing-portal-resource-manager.md)
  * [Een VNet koppelen aan een ExpressRoute-circuit](expressroute-howto-linkvnet-portal-resource-manager.md)