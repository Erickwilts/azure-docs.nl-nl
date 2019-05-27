---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 12/03/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 72ddd0b6cd6c3e12417d3698c403f89312b531f4
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66121183"
---
De VNet-naar-VNet-Veelgestelde vragen is van toepassing op VPN-gatewayverbindingen. Zie voor meer informatie over VNet-peering [peering van virtuele netwerken](../articles/virtual-network/virtual-network-peering-overview.md).

### <a name="does-azure-charge-for-traffic-between-vnets"></a>Worden er door Azure kosten in rekening gebracht voor verkeer tussen VNets?

VNet-naar-VNet-verkeer binnen dezelfde regio is gratis in beide richtingen wanneer u een VPN-gatewayverbinding. Regio-overschrijdende VNet-naar-VNet uitgaande verkeer wordt in rekening gebracht met de uitgaande inter-VNet standaardtarieven voor gegevensoverdracht op basis van de bron-regio's. Zie voor meer informatie, [pagina met prijzen voor VPN-Gateway](https://azure.microsoft.com/pricing/details/vpn-gateway/). Als u uw VNets verbinden bent met behulp van VNet-peering in plaats van een VPN-gateway, Zie [prijzen voor Virtual network](https://azure.microsoft.com/pricing/details/virtual-network/).

### <a name="does-vnet-to-vnet-traffic-travel-across-the-internet"></a>VNet-naar-VNet-verkeer zich via internet?

Nee. VNet-naar-VNet-verkeer via de Microsoft Azure-backbone, niet via internet wordt verzonden.

### <a name="can-i-establish-a-vnet-to-vnet-connection-across-azure-active-directory-aad-tenants"></a>Kan ik een VNet-naar-VNet-verbinding maken voor tenants van Azure Active Directory (AAD)?

Ja, werkt de VNet-naar-VNet-verbindingen die gebruikmaken van Azure VPN-gateways in AAD-tenants.

### <a name="is-vnet-to-vnet-traffic-secure"></a>Is het VNet-naar-VNet-verkeer beveiligd?

Ja, het beveiligd met IPsec/IKE-versleuteling.

### <a name="do-i-need-a-vpn-device-to-connect-vnets-together"></a>Heb ik een VPN-apparaat nodig om VNets met elkaar te verbinden?

Nee. Om meerdere virtuele netwerken van Azure met elkaar te verbinden, hebt u geen VPN-apparaat nodig, tenzij cross-premises connectiviteit is vereist.

### <a name="do-my-vnets-need-to-be-in-the-same-region"></a>Moeten mijn VNets zich in dezelfde regio bevinden?

Nee. De virtuele netwerken kunnen zich in dezelfde of verschillende Azure-regio's (locaties) bevinden.

### <a name="if-the-vnets-arent-in-the-same-subscription-do-the-subscriptions-need-to-be-associated-with-the-same-active-directory-tenant"></a>Als de VNets niet in hetzelfde abonnement, de abonnementen moet worden gekoppeld aan dezelfde Active Directory-tenant?

Nee.

### <a name="can-i-use-vnet-to-vnet-to-connect-virtual-networks-in-separate-azure-instances"></a>Kan ik VNet-naar-VNet-verbindingen gebruiken tussen virtuele netwerken in verschillende Azure-exemplaren? 

Nee. VNet-naar-VNet ondersteunt het verbinden van virtuele netwerken binnen hetzelfde Azure-exemplaar. Bijvoorbeeld, u een verbinding tussen de globale Azure- en Chinees/Duits/Amerikaanse overheid, Azure kan niet maken exemplaren. Overweeg het gebruik van een Site-naar-Site VPN-verbinding voor deze scenario's.

### <a name="can-i-use-vnet-to-vnet-along-with-multi-site-connections"></a>Kan ik VNet-naar-VNet- én multi-site-verbindingen gebruiken?

Ja. U kunt virtuele-netwerkverbindingen tegelijk gebruiken met multi-site-VPN’s.

### <a name="how-many-on-premises-sites-and-virtual-networks-can-one-virtual-network-connect-to"></a>Met hoeveel on-premises sites en virtuele netwerken kan één virtueel netwerk verbinding maken?

Zie de [gatewayvereisten](../articles/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#requirements) tabel.

### <a name="can-i-use-vnet-to-vnet-to-connect-vms-or-cloud-services-outside-of-a-vnet"></a>Kan ik VNet-naar-VNet gebruiken om virtuele machines of cloudservices met elkaar te verbinden buiten een VNet?

Nee. VNet naar VNet ondersteunt het verbinden van virtuele netwerken. Het biedt geen ondersteuning voor verbinden van virtuele machines of cloudservices die zich niet in een virtueel netwerk.

### <a name="can-a-cloud-service-or-a-load-balancing-endpoint-span-vnets"></a>Kan een cloudservice of een load balancing-eindpunt taakverdelingseindpunt vnets overbruggen?

Nee. Een cloudservice of een load balancing-eindpunt kan geen virtuele netwerken overbruggen, zelfs als ze met elkaar zijn verbonden.

### <a name="can-i-use-a-policybased-vpn-type-for-vnet-to-vnet-or-multi-site-connections"></a>Kan ik een PolicyBased VPN-type voor VNet-naar-VNet- of multi-site-verbindingen gebruiken?

Nee. VNet-naar-VNet- en multi-site-verbindingen vereisen Azure VPN-gateways met op route gebaseerde (voorheen dynamische routering genoemd) VPN-typen.

### <a name="can-i-connect-a-vnet-with-a-routebased-vpn-type-to-another-vnet-with-a-policybased-vpn-type"></a>Kan ik een VNet met een op route gebaseerd VPN-type verbinden met een op beleid gebaseerd VPN-type?

Nee, beide virtuele netwerken moeten route gebaseerde (voorheen dynamische routering genoemd) VPN's gebruiken.

### <a name="do-vpn-tunnels-share-bandwidth"></a>Delen VPN-tunnels bandbreedte?

Ja. Alle VPN-tunnels van het virtuele netwerk delen de beschikbare bandbreedte op de Azure VPN-gateway en dezelfde SLA voor VPN-gatewaybedrijfstijd in Azure.

### <a name="are-redundant-tunnels-supported"></a>Worden redundante tunnels ondersteund?

Redundante tunnels tussen twee virtuele netwerken worden ondersteund, mits één virtuele-netwerkgateway is geconfigureerd als actief-actief.

### <a name="can-i-have-overlapping-address-spaces-for-vnet-to-vnet-configurations"></a>Mogen er overlappende adresruimten zijn voor VNet-naar-VNet-configuraties?

Nee. Er mag geen sprake zijn van overlappende IP-adresbereiken.

### <a name="can-there-be-overlapping-address-spaces-among-connected-virtual-networks-and-on-premises-local-sites"></a>Mogen er overlappende adresruimten zijn tussen de verbonden virtuele netwerken en on-premises lokale sites?

Nee. Er mag geen sprake zijn van overlappende IP-adresbereiken.



