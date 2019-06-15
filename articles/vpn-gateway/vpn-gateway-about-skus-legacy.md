---
title: Verouderde met Azure virtual network VPN-gateway-SKU's | Microsoft Docs
description: Over het werken met de oude virtuele netwerkgateway SKU 's Basic, Standard en HighPerformance.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: jpconnock
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/10/2019
ms.author: cherylmc
ms.openlocfilehash: 00f1677e2691f9be5bb4584b07ca00340a52b1e1
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67056432"
---
# <a name="working-with-virtual-network-gateway-skus-legacy-skus"></a>Werken met virtual network gateway-SKU's (verouderde SKU's)

In dit artikel bevat informatie over het verouderde (oude) virtueel netwerkgateway SKU's. Het verouderde SKU's werkt nog steeds in beide implementatiemodellen voor VPN-gateways die al zijn gemaakt. Klassieke VPN-gateways echter ook doorgaan met de verouderde SKU's, zowel voor bestaande gateways, en voor de nieuwe gateways. Bij het maken van nieuwe Resource Manager VPN gateways, gebruikt u de nieuwe gateway-SKU's. Zie voor meer informatie over de nieuwe SKU's [over VPN-Gateway](vpn-gateway-about-vpngateways.md).

## <a name="gwsku"></a>Gateway-SKU's

[!INCLUDE [Legacy gateway SKUs](../../includes/vpn-gateway-gwsku-legacy-include.md)]

Vindt u verouderde gatewayprijzen in de **virtuele netwerkgateways** sectie, dat zich bevindt in op de [ExpressRoute pagina met prijzen](https://azure.microsoft.com/pricing/details/expressroute).

## <a name="agg"></a>Geschatte geaggregeerde doorvoer per SKU

[!INCLUDE [Aggregated throughput by legacy SKU](../../includes/vpn-gateway-table-gwtype-legacy-aggtput-include.md)]

## <a name="config"></a>Ondersteunde configuraties per SKU- en VPN-type

[!INCLUDE [Table requirements for old SKUs](../../includes/vpn-gateway-table-requirements-legacy-sku-include.md)]

## <a name="resize"></a>Formaat van een gateway

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

U kunt het formaat van uw gateway op een gateway-SKU binnen dezelfde SKU-familie. Bijvoorbeeld, als u een standaard-SKU hebt, u kunt het formaat naar een HighPerformance-SKU. U kunt geen echter grootte van uw VPN-gateway tussen de oude SKU's en de nieuwe SKU-families. U kan geen bijvoorbeeld Ga van een standaard-SKU naar een VpnGw2-SKU of een basis-SKU naar VpnGw1.

Als u wilt het formaat van een gateway voor het klassieke implementatiemodel, gebruik de volgende opdracht:

```powershell
Resize-AzureVirtualNetworkGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance
```

Als u wilt het formaat van een gateway voor het Resource Manager-implementatiemodel met behulp van PowerShell, gebruik de volgende opdracht:

```powershell
$gw = Get-AzVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance
```
U kunt ook het formaat van een gateway in Azure portal.

## <a name="change"></a>Met de nieuwe gateway SKU's wijzigen

[!INCLUDE [Change to the new SKUs](../../includes/vpn-gateway-gwsku-change-legacy-sku-include.md)]

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de nieuwe Gateway-SKU's, [Gateway-SKU's](vpn-gateway-about-vpngateways.md#gwsku).

Zie voor meer informatie over configuratie-instellingen, [over VPN-gatewayconfiguratie-instellingen](vpn-gateway-about-vpn-gateway-settings.md).
