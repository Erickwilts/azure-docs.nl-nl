---
title: 'VPN-gatewayoverdracht configureren voor peering voor virtuele netwerken: Azure Resource Manager | Microsoft Docs'
description: VPN-gatewayoverdracht configureren voor peering voor virtuele netwerken.
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: ''
tags: azure-resource-manager
ms.assetid: 0683c664-9c03-40a4-b198-a6529bf1ce8b
ms.service: vpn-gateway
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/25/2018
ms.author: yushwang
ms.openlocfilehash: d5e62bf1838c8f07068208019d28d7273c28bd63
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60457371"
---
# <a name="configure-vpn-gateway-transit-for-virtual-network-peering"></a>VPN-gatewayoverdracht configureren voor peering voor virtuele netwerken

Dit artikel helpt u bij het configureren van gatewayoverdracht voor peering voor virtuele netwerken. [Peering voor virtuele netwerken](../virtual-network/virtual-network-peering-overview.md) maakt naadloos een verbinding tussen twee virtuele Azure-netwerken, waarbij de twee virtuele netwerken worden samengevoegd tot één netwerk voor connectiviteitsdoeleinden. [Gatewayoverdracht](../virtual-network/virtual-network-peering-overview.md#gateways-and-on-premises-connectivity) is een peeringeigenschap die zorgt dat één virtueel netwerk de VPN-gateway in het als peer ingestelde virtuele netwerk kan gebruiken voor cross-premises- of VNet-naar-VNet-connectiviteit. Het volgende diagram toont hoe gatewayoverdracht werkt met peering voor virtuele netwerken.

![gatewayoverdracht](./media/vpn-gateway-peering-gateway-transit/gatewaytransit.png)

In het diagram zorgt gatewayoverdracht dat de als peer ingestelde virtuele netwerken de Azure VPN-gateway kunnen gebruiken in Hub-RM. De connectiviteit beschikbaar op de VPN-gateway, met inbegrip van de S2S-, P2S- en VNet-naar-VNet-verbindingen, geldt voor alle drie de virtuele netwerken. De overdrachtoptie is beschikbaar voor peering tussen de dezelfde of verschillende implementatiemodellen. De beperking is dat de VPN-gateway zich alleen mag bevinden in het virtuele netwerk met het Resource Manager-implementatiemodel, zoals wordt weergegeven in het diagram.

In de hub en spoke-netwerkarchitectuur zorgt gatewayoverdracht dat spoke virtuele netwerken de VPN-gateway in de hub kunnen delen, in plaats van VPN-gateways in elk spoke virtueel netwerk te moeten implementeren. Routes naar met de gateway verbonden virtuele netwerken of on-premises netwerken worden doorgegeven aan de routeringstabellen voor de als peer ingestelde virtuele netwerken met behulp van gatewayoverdracht. U kunt de automatische routedoorgifte van de VPN-gateway uitschakelen. Maak een routeringstabel met de optie **Doorgifte van BGP-route uitschakelen** en koppel de routeringstabel met de subnets om de routedistributie naar die subnetten te voorkomen. Zie [Tabel voor routering van virtuele netwerken](../virtual-network/manage-route-table.md) voor meer informatie.

In dit document worden twee scenario's beschreven:

1. Beide virtuele netwerken gebruiken het implementatiemodel van Resource Manager
2. Het spoke virtuele netwerk is Classic en het hub virtuele netwerk met de gateway is in de Resource Manager

## <a name="requirements"></a>Vereisten

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

In het voorbeeld in dit document moeten de volgende resources worden gemaakt:

1. Hub-RM virtueel netwerk met een VPN-gateway
2. Spoke-RM virtueel netwerk
3. Spoke-Classic virtueel netwerk met het klassieke implementatiemodel
4. Het account dat u gebruikt moet de vereiste rollen en machtigingen hebben. Zie de sectie [Machtigingen](#permissions) van dit artikel voor meer informatie.

Raadpleeg de volgende documenten voor instructies:

1. [Een VPN-gateway in een virtueel netwerk maken](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
2. [Peering voor virtuele netwerken maken met hetzelfde implementatiemodel](../virtual-network/tutorial-connect-virtual-networks-portal.md)
3. [Peering voor virtuele netwerken maken met verschillende implementatiemodellen](../virtual-network/create-peering-different-deployment-models.md)

## <a name="permissions"></a>Machtigingen

De accounts die u gebruikt voor het maken van peering voor virtuele netwerken, moeten de benodigde rollen of machtigingen hebben. In het onderstaande voorbeeld, waar twee virtuele netwerken genaamd Hub-RM en Spoke-Classic als peer worden ingesteld, moet uw account de volgende rollen of machtigingen hebben voor elke virtueel netwerk:
    
|Virtueel netwerk|Implementatiemodel|Rol|Machtigingen|
|---|---|---|---|
|Hub-RM|Resource Manager|[Inzender voor netwerken](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write|
| |Klassiek|[Inzender voor klassieke netwerken](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|N/A|
|Spoke-Classic|Resource Manager|[Inzender voor netwerken](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/peer|
||Klassiek|[Inzender voor klassieke netwerken](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|Microsoft.ClassicNetwork/virtualNetworks/peer|

Meer informatie over [ingebouwde rollen](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) en het toewijzen van specifieke machtigingen voor [aangepaste rollen](../active-directory/role-based-access-control-custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (alleen Resource Manager).

## <a name="resource-manager-to-resource-manager-peering-with-gateway-transit"></a>Peering van Resource Manager naar Resource Manager met gatewayoverdracht

Volg de instructies voor het maken of bijwerken van peering voor virtuele netwerken om gatewayoverdracht in te schakelen.

1. Maak of wijzig de peering van Spoke-RM naar Hub-RM via de Azure Portal. Navigeer naar de virtuele netwerkresource Spoke-RM, klik op Peerings en vervolgens op Toevoegen:
    - Stel de optie Resource Manager in
    - Selecteer het virtuele netwerk Hub-RM in het desbetreffende abonnement
    - Zorg dat Toegang tot het virtuele netwerk toestaan is ingeschakeld
    - Stel de optie **Externe gateways gebruiken** in
    - Klik op OK

      ![spokerm-to-hubrm](./media/vpn-gateway-peering-gateway-transit/spokerm-hubrm-peering.png)

2. Als peering al is gemaakt, gaat u naar de peeringresource en schakelt u **Externe gateways gebruiken** in (vergelijkbaar met de optie die wordt weergegeven in de schermafbeelding in stap 1)

3. Maak of wijzig de peering van Hub-RM naar Spoke-RM via de Azure Portal. Navigeer naar de virtuele netwerkresource Hub-RM, klik op Peerings en vervolgens op Toevoegen:
    - Stel de optie Resource Manager in
    - Zorg dat Toegang tot het virtuele netwerk toestaan is ingeschakeld
    - Selecteer het virtuele netwerk Spoke-RM in het desbetreffende abonnement
    - Stel de optie **Gatewayoverdracht toestaan** in
    - Klik op OK

      ![hubrm-to-spokerm](./media/vpn-gateway-peering-gateway-transit/hubrm-spokerm-peering.png)

4. Als peering al is gemaakt, gaat u naar de peeringresource en schakelt u **Gatewayoverdracht toestaan** in (vergelijkbaar met de optie die wordt weergegeven in de schermafbeelding in stap 3)

5. Controleer of beide virtuele netwerken de status **Verbonden** hebben

### <a name="powershell-sample"></a>Voorbeeld van PowerShell

U kunt ook PowerShell gebruiken voor het maken of bijwerken van de peering in het bovenstaande voorbeeld. Vervang de variabelen door de namen van uw virtuele netwerken en resourcegroepen.

```azurepowershell-interactive
$SpokeRG = "SpokeRG1"
$SpokeRM = "Spoke-RM"
$HubRG   = "HubRG1"
$HubRM   = "Hub-RM"

$spokermvnet = Get-AzVirtualNetwork -Name $SpokeRM -ResourceGroup $SpokeRG
$hubrmvnet   = Get-AzVirtualNetwork -Name $HubRM -ResourceGroup $HubRG

Add-AzVirtualNetworkPeering `
  -Name SpokeRMtoHubRM `
  -VirtualNetwork $spokermvnet `
  -RemoteVirtualNetworkId $hubrmvnet.Id `
  -UseRemoteGateways

Add-AzVirtualNetworkPeering `
  -Name HubRMToSpokeRM `
  -VirtualNetwork $hubrmvnet `
  -RemoteVirtualNetworkId $spokermvnet.Id `
  -AllowGatewayTransit
```

## <a name="classic-to-resource-manager-peering-with-gateway-transit"></a>Peering van Classic naar Resource Manager met gatewayoverdracht

De stappen zijn vergelijkbaar met de stappen in het voorbeeld van Resource Manager, behalve dat de bewerkingen alleen op het Hub-RM virtuele netwerk worden toegepast.

1. Maak of wijzig de peering van Hub-RM naar Spoke-RM via de Azure Portal. Navigeer naar de virtuele netwerkresource Hub-RM, klik op Peerings en vervolgens op Toevoegen:
   - Stel de optie Classic in voor het implementatiemodel van virtuele netwerken
   - Selecteer het virtuele netwerk Spoke-Classic in het desbetreffende abonnement
   - Zorg dat Toegang tot het virtuele netwerk toestaan is ingeschakeld
   - Stel de optie **Gatewayoverdracht toestaan** in
   - Klik op OK

     ![hubrm-to-spokeclassic](./media/vpn-gateway-peering-gateway-transit/hubrm-spokeclassic-peering.png)

2. Als peering al is gemaakt, gaat u naar de peeringresource en schakelt u **Gatewayoverdracht toestaan** in (vergelijkbaar met de optie die wordt weergegeven in de schermafbeelding in stap 1)

3. Er is geen bewerking op het virtuele netwerk Spoke-Classic

4. Controleer of het Hub-RM virtuele netwerk de status **Verbonden** heeft

Zodra de status 'Verbonden' aangeeft, kunnen de spoke virtuele netwerken starten met behulp van VNet-naar-VNet- of cross-premises-connectiviteit via de VPN-gateway in het hub virtuele netwerk.

### <a name="powershell-sample"></a>Voorbeeld van PowerShell

U kunt ook PowerShell gebruiken voor het maken of bijwerken van de peering in het bovenstaande voorbeeld. Vervang de variabelen en de abonnements-ID door de waarden van uw virtuele netwerk, resourcegroepen en abonnement. U hoeft alleen peering voor virtuele netwerken te maken op het hub virtuele netwerk.

```azurepowershell-interactive
$HubRG   = "HubRG1"
$HubRM   = "Hub-RM"

$hubrmvnet   = Get-AzVirtualNetwork -Name $HubRM -ResourceGroup $HubRG

Add-AzVirtualNetworkPeering `
  -Name HubRMToSpokeRM `
  -VirtualNetwork $hubrmvnet `
  -RemoteVirtualNetworkId "/subscriptions/<subscription Id>/resourceGroups/Default-Networking/providers/Microsoft.ClassicNetwork/virtualNetworks/Spoke-Classic" `
  -AllowGatewayTransit
```

## <a name="next-steps"></a>Volgende stappen

* Raadpleeg [beperkingen en gedrag van peering voor virtuele netwerken](../virtual-network/virtual-network-manage-peering.md#requirements-and-constraints) en [instellingen voor peering voor virtuele netwerken](../virtual-network/virtual-network-manage-peering.md#create-a-peering) voordat u peering voor virtuele netwerken instelt voor gebruik in productie.
* Meer informatie over het [maken van een hub en spoke netwerktopologie](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) met peering voor virtuele netwerken en gatewayoverdracht
