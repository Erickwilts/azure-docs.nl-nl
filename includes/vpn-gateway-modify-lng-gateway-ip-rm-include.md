---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 01/15/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 6a90b23c10e08e8b14a18f9619cff5aaeb003cab
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "76045706"
---
### <a name="to-modify-the-local-network-gateway-gatewayipaddress---no-gateway-connection"></a><a name="gwipnoconnection"></a> Het 'gatewayIpAddress' van de lokale netwerkgateway wijzigen - geen gatewayverbinding

Als van het VPN-apparaat waarmee u verbinding wilt maken het openbare IP-adres is gewijzigd, moet u de gateway van het lokale netwerk aanpassen met deze wijziging. Gebruik het voorbeeld om een lokale netwerkgateway die geen gatewayverbinding heeft te wijzigen.

Wanneer u deze waarde wijzigt, kunt u tegelijkertijd ook de adresvoorvoegsels wijzigen. Zorg ervoor dat u de bestaande naam van de gateway van uw lokale netwerk gebruikt om de huidige instellingen te overschrijven. Als u een andere naam gebruikt, maakt u een nieuwe lokale netwerkgateway in plaats van de bestaande gateway te overschrijven.

```azurepowershell-interactive
New-AzLocalNetworkGateway -Name Site1 `
-Location "East US" -AddressPrefix @('10.101.0.0/24','10.101.1.0/24') `
-GatewayIpAddress "5.4.3.2" -ResourceGroupName TestRG1
```

### <a name="to-modify-the-local-network-gateway-gatewayipaddress---existing-gateway-connection"></a><a name="gwipwithconnection"></a>Het 'gatewayIpAddress' van de lokale netwerkgateway wijzigen - bestaande gatewayverbinding

Als van het VPN-apparaat waarmee u verbinding wilt maken het openbare IP-adres is gewijzigd, moet u de gateway van het lokale netwerk aanpassen met deze wijziging. Als er al een gatewayverbinding bestaat, moet u die verbinding eerst verwijderen. Nadat de verbinding is verwijderd, kunt u het IP-adres van de gateway wijzigen en een nieuwe verbinding maken. U kunt tegelijkertijd ook de adresvoorvoegsels wijzigen. Dit veroorzaakt enige downtime in uw VPN-verbinding. Als u het IP-adres van de gateway wijzigt, hoeft u de VPN-gateway niet te verwijderen. U hoeft alleen de verbinding te verwijderen.
 

1. Verwijder de verbinding. U kunt de naam van uw verbinding vinden met behulp van de cmdlet ' Get-AzVirtualNetworkGatewayConnection '.

   ```azurepowershell-interactive
   Remove-AzVirtualNetworkGatewayConnection -Name VNet1toSite1 `
   -ResourceGroupName TestRG1
   ```
2. Wijzig de waarde 'GatewayIpAddress'. U kunt tegelijkertijd ook de adresvoorvoegsels wijzigen. Zorg ervoor dat u de bestaande naam van de gateway van uw lokale netwerk gebruikt om de huidige instellingen te overschrijven. Als u dit niet doet, maakt u een nieuwe lokale netwerkgateway in plaats van de bestaande gateway te overschrijven.

   ```azurepowershell-interactive
   New-AzLocalNetworkGateway -Name Site1 `
   -Location "East US" -AddressPrefix @('10.101.0.0/24','10.101.1.0/24') `
   -GatewayIpAddress "104.40.81.124" -ResourceGroupName TestRG1
   ```
3. Maak de verbinding. In dit voorbeeld configureren we een IPsec-verbindingstype. Wanneer u uw verbinding opnieuw maakt, gebruikt u het verbindingstype dat is opgegeven voor uw configuratie. Zie de pagina [PowerShell-cmdlet](https://msdn.microsoft.com/library/mt603611.aspx) voor aanvullende verbindingstypen.  Als u de naam van de VirtualNetworkGateway wilt ophalen, kunt u de cmdlet Get-AzVirtualNetworkGateway uitvoeren.
   
    Stel de variabelen in.

   ```azurepowershell-interactive
   $local = Get-AzLocalNetworkGateway -Name Site1 -ResourceGroupName TestRG1

   $vnetgw = Get-AzVirtualNetworkGateway -Name VNet1GW -ResourceGroupName TestRG1
   ```
   
    Maak de verbinding.

   ```azurepowershell-interactive 
   New-AzVirtualNetworkGatewayConnection -Name VNet1Site1 -ResourceGroupName TestRG1 `
   -Location "East US" `
   -VirtualNetworkGateway1 $vnetgw `
   -LocalNetworkGateway2 $local `
   -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
   ```