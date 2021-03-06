---
title: Over virtuele hub-routering
titleSuffix: Azure Virtual WAN
description: In dit artikel wordt de route ring van virtuele hub beschreven
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 09/22/2020
ms.author: cherylmc
ms.custom: fasttrack-edit
ms.openlocfilehash: 51480a49aab2c1277eeb846c593fcb2bc858d1f0
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "90983711"
---
# <a name="about-virtual-hub-routing"></a>Over virtuele hub-routering

De routerings mogelijkheden in een virtuele hub worden geboden door een router die alle route ring beheert tussen gateways met behulp van Border Gateway Protocol (BGP). Een virtuele hub kan meerdere gateways bevatten, zoals een site-naar-site-VPN-gateway, ExpressRoute-gateway, punt-naar-site-gateway Azure Firewall. Deze router biedt ook transit connectiviteit tussen virtuele netwerken die verbinding maken met een virtuele hub en die een geaggregeerde door Voer van 50 Gbps kan ondersteunen. Deze routerings mogelijkheden zijn van toepassing op standaard virtuele WAN-klanten. 

Zie [route ring van virtuele hub configureren voor meer informatie over het](how-to-virtual-hub-routing.md)configureren van route ring.

## <a name="routing-concepts"></a><a name="concepts"></a>Routerings concepten

In de volgende secties worden de belangrijkste concepten in virtuele hub-route ring beschreven.

### <a name="hub-route-table"></a><a name="hub-route"></a>Route tabel van de hub

Een route tabel van de virtuele hub kan een of meer routes bevatten. Een route bevat de naam, een label, een doel type, een lijst met doel voorvoegsels en de volgende hop-informatie voor een pakket dat moet worden doorgestuurd. Een **verbinding** heeft meestal een routerings configuratie die aan een route tabel is gekoppeld of door gegeven

### <a name="connection"></a><a name="connection"></a>Verbinding

Verbindingen zijn Resource Manager-resources die een routerings configuratie hebben. De vier typen verbindingen zijn:

* **VPN-verbinding**: Hiermee wordt een VPN-site verbonden met een virtuele hub VPN-gateway.
* **ExpressRoute-verbinding**: Hiermee verbindt u een ExpressRoute-circuit met een virtuele hub ExpressRoute-gateway.
* **P2S-configuratie verbinding**: verbindt een VPN-configuratie (punt-naar-site) met een virtuele-hub-gebruikers-VPN (punt-naar-site)-gateway.
* **Virtuele hub-netwerk verbinding**: Hiermee verbindt u virtuele netwerken met een virtuele hub.

U kunt de routerings configuratie voor een virtuele netwerk verbinding instellen tijdens de installatie. Standaard worden alle verbindingen gekoppeld en door gegeven aan de standaard route tabel.

### <a name="association"></a><a name="association"></a>Organisatie

Elke verbinding is gekoppeld aan één route tabel. Als u een verbinding met een route tabel koppelt, kan het verkeer worden verzonden naar de bestemming die wordt aangegeven als routes in de route tabel. In de routerings configuratie van de verbinding wordt de bijbehorende route tabel weer gegeven.  Meerdere verbindingen kunnen worden gekoppeld aan dezelfde route tabel. Alle VPN-, ExpressRoute-en gebruikers-VPN-verbindingen zijn gekoppeld aan dezelfde routerings tabel (standaard).

Standaard zijn alle verbindingen gekoppeld aan een **standaard route tabel** in een virtuele hub. Elke virtuele hub heeft zijn eigen standaard route tabel, die kan worden bewerkt om een statische route (s) toe te voegen. Routes die statisch worden toegevoegd, hebben voor rang boven dynamisch geleerde routes voor dezelfde voor voegsels.

:::image type="content" source="./media/about-virtual-hub-routing/concepts-association.png" alt-text="Organisatie":::

### <a name="propagation"></a><a name="propagation"></a>Doorgifte

Verbindingen geven dynamische routes door aan een route tabel. Met een VPN-verbinding, een ExpressRoute-verbinding of een P2S-configuratie verbinding worden routes door gegeven van de virtuele hub naar de on-premises router met behulp van BGP. Routes kunnen worden door gegeven aan een of meer route tabellen.

Er is ook een **route tabel geen** beschikbaar voor elke virtuele hub. Door door te geven aan de geen route tabel, houdt in dat er geen routes moeten worden door gegeven van de verbinding. VPN-, ExpressRoute-en gebruikers-VPN-verbindingen sturen routes door naar dezelfde set route tabellen.

:::image type="content" source="./media/about-virtual-hub-routing/concepts-propagation.png" alt-text="Organisatie":::

### <a name="labels"></a><a name="static"></a>Labels
Labels bieden een mechanisme voor het logisch groeperen van route tabellen. Dit is met name handig tijdens het door geven van routes van verbindingen naar meerdere route tabellen. De standaard route tabel heeft bijvoorbeeld een ingebouwd label met de naam default. Wanneer gebruikers verbindings routes door geven naar een standaard label, wordt deze automatisch toegepast op alle standaard route tabellen voor elke hub in het virtuele WAN. 

### <a name="configuring-static-routes-in-a-virtual-network-connection"></a><a name="static"></a>Statische routes configureren in een virtuele netwerk verbinding

Het configureren van statische routes biedt een mechanisme voor het stuur verkeer via een volgende hop-IP. Dit kan een virtueel netwerk apparaat (NVA) zijn dat is ingericht in een spoke-VNet dat is gekoppeld aan een virtuele hub. De statische route bestaat uit een route naam, een lijst met doel voorvoegsels en het volgende hop-IP-adres.

## <a name="reset-hub"></a><a name="route"></a>Hub opnieuw instellen
Deze optie is alleen beschikbaar in de Azure Portal, maar biedt de gebruiker een manier om mislukte resources zoals route tabellen, hub-router of de virtuele-hub-resource zelf terug te brengen naar de inrichtings status Rightful. Dit is een extra optie waarmee de gebruiker rekening moet houden voordat u contact opneemt met micro soft voor ondersteuning. Met deze bewerking worden de gateways in een virtuele hub niet opnieuw ingesteld. 

## <a name="route-tables-in-basic-and-standard-virtual-wans-prior-to-the-feature-set-of-association-and-propagation"></a><a name="route"></a>Route tabellen in de Basic-en Standard-standaard-virtuele Wan's vóór de functieset van koppeling en doorgifte

Routeringstabellen hebben nu functies voor koppeling en doorgifte. Een vooraf bestaande routeringstabel is een routeringstabel die deze functies niet heeft. Als u al bestaande routes in Hub Routing hebt en u de nieuwe mogelijkheden wilt gebruiken, moet u rekening houden met het volgende:

* **Standaard virtuele WAN-klanten met al bestaande routes in de virtuele hub**:

Als u al bestaande routes in de sectie route ring voor de hub in Azure Portal hebt, moet u deze eerst verwijderen en vervolgens proberen nieuwe route tabellen te maken (beschikbaar in de sectie route tabellen van de hub in Azure Portal)

* **Eenvoudige virtuele WAN-klanten met reeds bestaande routes in de virtuele hub**: als u al bestaande routes in de sectie route ring voor de hub in azure Portal hebt, moet u deze eerst verwijderen en vervolgens uw virtuele standaard-WAN **upgraden** naar standaard virtuele WAN. Zie [Een virtueel WAN upgraden van Basic naar Standard](upgrade-virtual-wan.md).

## <a name="virtual-wan-routing-considerations"></a><a name="considerations"></a>Aandachtspunten voor virtuele WAN-route ring

Houd rekening met het volgende bij het configureren van virtuele WAN-route ring:

* Alle vertakkings verbindingen (punt-naar-site, site-naar-site-en ExpressRoute) moeten aan de standaard route tabel zijn gekoppeld. Op die manier komen alle branches overeen met dezelfde voor voegsels.
* Alle vertakkings verbindingen moeten hun routes door geven aan dezelfde set route tabellen. Als u bijvoorbeeld besluit dat vertakkingen moeten worden door gegeven aan de standaard route tabel, moet deze configuratie consistent zijn voor alle vertakkingen. Als gevolg hiervan kunnen alle verbindingen die aan de standaard route tabel zijn gekoppeld, alle vertakkingen bereiken.
* Vertakking-naar-vertakking via Azure Firewall wordt momenteel niet ondersteund.
* Wanneer u Azure Firewall in meerdere regio's gebruikt, moeten alle spoke-virtuele netwerken aan dezelfde route tabel zijn gekoppeld. Als er bijvoorbeeld een subset van de VNets door de Azure Firewall, terwijl andere VNets overs Laan, is het niet mogelijk om de Azure Firewall in dezelfde virtuele hub over te slaan.
* Eén volgende hop-IP kan per VNet-verbinding worden geconfigureerd.
* Virtuele hub biedt geen ondersteuning voor een statische route voor 0.0.0.0/0 en de volgende hop Virtual Network verbinding (of een IP-adres van een apparaat in de VNet-verbinding)
* Alle informatie met betrekking tot de route 0.0.0.0/0 wordt beperkt tot de route tabel van een lokale hub. Deze route wordt niet door gegeven tussen hubs.

## <a name="next-steps"></a>Volgende stappen

Zie [route ring van virtuele hub configureren voor meer informatie over het](how-to-virtual-hub-routing.md)configureren van route ring.

Raadpleeg de [Veelgestelde vragen](virtual-wan-faq.md)voor meer informatie over Virtual WAN.
