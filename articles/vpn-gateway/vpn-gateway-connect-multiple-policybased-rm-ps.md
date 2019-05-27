---
title: 'Verbinding maken met Azure VPN-gateways naar meerdere on-premises op beleid gebaseerde VPN-apparaten: Azure Resource Manager: PowerShell | Microsoft Docs'
description: Een Azure op route gebaseerde VPN-gateway configureren aan meerdere op beleid gebaseerde VPN-apparaten met behulp van Azure Resource Manager en PowerShell.
services: vpn-gateway
documentationcenter: na
author: yushwang
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: yushwang
ms.openlocfilehash: 9085d5ee21b1e955b7d9416a379ee730ba26ad3e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "66150161"
---
# <a name="connect-azure-vpn-gateways-to-multiple-on-premises-policy-based-vpn-devices-using-powershell"></a>Verbinding maken met Azure VPN-gateways naar meerdere on-premises op beleid gebaseerde VPN-apparaten met behulp van PowerShell

Dit artikel helpt u bij het configureren van een Azure op route gebaseerde VPN-gateway verbinding maken met meerdere on-premises op beleid gebaseerde VPN-apparaten gebruik te maken van aangepaste IPsec/IKE-beleid voor S2S-VPN-verbindingen.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="about"></a>Over op basis van beleid en op route gebaseerde VPN-gateways

Beleid - *versus* op route gebaseerde VPN-apparaten verschillen in hoe de IPsec-verkeerkiezers zijn ingesteld op een verbinding:

* **Op basis van beleid** VPN-apparaten de combinaties van voorvoegsels van beide netwerken gebruiken om te definiëren hoe verkeer wordt versleuteld/ontsleuteld via IPsec-tunnels. Het is doorgaans gebaseerd op firewallapparaten waarmee filteren van netwerkpakketten worden uitgevoerd. IPsec-tunnel versleuteling en ontsleuteling worden toegevoegd aan het pakket filter- en engine voor gebeurtenisverwerking.
* **Op route gebaseerde** VPN-apparaten met verkeerkiezers any-to-any (jokertekens) en routering/doorsturen van verkeer naar verschillende tabellen kunt IPsec-tunnels. Dit wordt gewoonlijk samengesteld op router platforms waar elke IPsec-tunnel is gemodelleerd als een netwerkinterface of VTI (virtuele tunnelinterface).

De volgende diagrammen markeert u de twee modellen:

### <a name="policy-based-vpn-example"></a>Op beleid gebaseerde VPN-voorbeeld
![op basis van beleid](./media/vpn-gateway-connect-multiple-policybased-rm-ps/policybasedmultisite.png)

### <a name="route-based-vpn-example"></a>Op route gebaseerde VPN-voorbeeld
![op route gebaseerd](./media/vpn-gateway-connect-multiple-policybased-rm-ps/routebasedmultisite.png)

### <a name="azure-support-for-policy-based-vpn"></a>Azure-ondersteuning voor op beleid gebaseerde VPN-verbinding
Op dit moment biedt Azure ondersteuning voor beide modi van VPN-gateways: op route gebaseerde VPN-gateways en VPN-gateways op basis van beleid. Ze zijn gebaseerd op verschillende interne platforms, wat leiden tot verschillende specificaties:

|                          | **PolicyBased VPN Gateway** | **RouteBased VPN Gateway**               |
| ---                      | ---                         | ---                                      |
| **Azure Gateway-SKU**    | Basis                       | Basic, Standard, HighPerformance, VpnGw1, VpnGw2, VpnGw3 |
| **IKE-versie**          | IKEv1                       | IKEv2                                    |
| **Max. S2S-verbindingen** | **1**                       | Basic/Standard: 10<br> HighPerformance: 30 |
|                          |                             |                                          |

Met de aangepast IPsec/IKE-beleid, kunt u nu Azure op route gebaseerde VPN-gateways voor het gebruik van voorvoegsel gebaseerde verkeerkiezers met optie configureren '**PolicyBasedTrafficSelectors**", verbinding maken met on-premises op beleid gebaseerde VPN-apparaten. Op deze manier kunt u verbinding maken vanaf een virtueel Azure-netwerk en VPN-gateway naar meerdere on-premises VPN-/ firewall-apparaten op basis van beleid, de limiet van één verbinding verwijderen uit de huidige Azure op beleid gebaseerde VPN-gateways.

> [!IMPORTANT]
> 1. Om in te schakelen deze connectiviteit, uw on-premises op beleid gebaseerde VPN-apparaten moeten ondersteunen **IKEv2** verbinding maken met de route gebaseerde Azure VPN-gateways. Raadpleeg de specificaties van uw VPN-apparaat.
> 2. De on-premises netwerken verbinding te maken via op beleid gebaseerde VPN-apparaten met dit mechanisme kunnen alleen verbinding maken met Azure virtual network; **ze kunnen niet worden doorgevoerd met andere on-premises netwerken of virtuele netwerken via dezelfde Azure VPN-gateway**.
> 3. De configuratieoptie is onderdeel van het aangepaste beleid voor IPsec/IKE-verbinding. Als u het verkeer op basis van beleid selector-optie inschakelt, moet u de volledige-beleid (IPsec/IKE-algoritmen voor versleuteling en integriteit, belangrijkste sterke punten en SA-levensduur) opgeven.

Het volgende diagram laat zien waarom transitroutering via Azure VPN-gateway niet met de optie op basis van beleid werkt:

![doorvoer op basis van beleid](./media/vpn-gateway-connect-multiple-policybased-rm-ps/policybasedtransit.png)

Zoals u in het diagram, heeft de Azure VPN-gateway verkeerkiezers van het virtuele netwerk aan elk van de on-premises netwerkvoorvoegsels, maar niet de voorvoegsels overlappende verbinding. Bijvoorbeeld, on-premises site 2, site 3 en 4-site kan elk communiceren naar VNet1 respectievelijk, maar kan geen verbinding maken via de Azure VPN-gateway met elkaar. Het diagram toont de cross-connect verkeerkiezers die niet beschikbaar in de Azure VPN-gateway in deze configuratie.

## <a name="configurepolicybased"></a>Op beleid gebaseerde verkeerkiezers voor een verbinding configureren

De instructies in dit artikel volgen hetzelfde voorbeeld, zoals beschreven in [configureren IPsec/IKE-beleid voor S2S- of VNet-naar-VNet-verbindingen](vpn-gateway-ipsecikepolicy-rm-powershell.md) een S2S-VPN-verbinding tot stand brengen. Dit wordt weergegeven in het volgende diagram:

![s2s-beleid](./media/vpn-gateway-connect-multiple-policybased-rm-ps/s2spolicypb.png)

De werkstroom voor het inschakelen van deze connectiviteit:
1. Het virtuele netwerk, de VPN-gateway en de lokale netwerkgateway voor uw cross-premises-verbinding maken
2. Een IPsec/IKE-beleid maken
3. Het beleid van toepassing wanneer u een S2S- of VNet-naar-VNet-verbinding, maakt en **inschakelen van de op beleid gebaseerde verkeerkiezers** via de verbinding
4. Als de verbinding al gemaakt is, kunt u van toepassing of het beleid aan een bestaande verbinding bijwerken

## <a name="before-you-begin"></a>Voordat u begint

Controleer of u een Azure-abonnement hebt. Als u nog geen Azure-abonnement hebt, kunt u [uw voordelen als MSDN-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) of [u aanmelden voor een gratis account](https://azure.microsoft.com/pricing/free-trial).

[!INCLUDE [powershell](../../includes/vpn-gateway-cloud-shell-powershell-about.md)]

## <a name="enablepolicybased"></a>Op beleid gebaseerde verkeerkiezers voor een verbinding inschakelen

Zorg ervoor dat u hebt [deel 3 van het artikel configureren IPsec/IKE-beleid](vpn-gateway-ipsecikepolicy-rm-powershell.md) voor deze sectie. Het volgende voorbeeld wordt de dezelfde parameters en de volgende stappen:

### <a name="step-1---create-the-virtual-network-vpn-gateway-and-local-network-gateway"></a>Stap 1: het virtuele netwerk, de VPN-gateway en de lokale netwerkgateway maken

#### <a name="1-connect-to-your-subscription-and-declare-your-variables"></a>1. Verbinding maken met uw abonnement en de variabelen declareren

[!INCLUDE [sign in](../../includes/vpn-gateway-cloud-shell-ps-login.md)]

Declareer uw variabelen. Voor deze oefening gebruikt u de volgende variabelen:

```azurepowershell-interactive
$Sub1          = "<YourSubscriptionName>"
$RG1           = "TestPolicyRG1"
$Location1     = "East US 2"
$VNetName1     = "TestVNet1"
$FESubName1    = "FrontEnd"
$BESubName1    = "Backend"
$GWSubName1    = "GatewaySubnet"
$VNetPrefix11  = "10.11.0.0/16"
$VNetPrefix12  = "10.12.0.0/16"
$FESubPrefix1  = "10.11.0.0/24"
$BESubPrefix1  = "10.12.0.0/24"
$GWSubPrefix1  = "10.12.255.0/27"
$DNS1          = "8.8.8.8"
$GWName1       = "VNet1GW"
$GW1IPName1    = "VNet1GWIP1"
$GW1IPconf1    = "gw1ipconf1"
$Connection16  = "VNet1toSite6"

$LNGName6      = "Site6"
$LNGPrefix61   = "10.61.0.0/16"
$LNGPrefix62   = "10.62.0.0/16"
$LNGIP6        = "131.107.72.22"
```

#### <a name="2-create-the-virtual-network-vpn-gateway-and-local-network-gateway"></a>2. Het virtuele netwerk, de VPN-gateway en de lokale netwerkgateway maken

Maak een resourcegroep.

```azurepowershell-interactive
New-AzResourceGroup -Name $RG1 -Location $Location1
```

Gebruik het volgende voorbeeld om te maken van het virtuele netwerk TestVNet1 met drie subnetten en de VPN-gateway. Als u vervangen door waarden wilt, is het belangrijk dat u altijd de naam het gatewaysubnet bijzonder GatewaySubnet. Als u een andere naam kiest, mislukt het maken van de gateway.

```azurepowershell-interactive
$fesub1 = New-AzVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
$besub1 = New-AzVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

$gw1pip1    = New-AzPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
$vnet1      = Get-AzVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1    = Get-AzVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
$gw1ipconf1 = New-AzVirtualNetworkGatewayIpConfig -Name $GW1IPconf1 -Subnet $subnet1 -PublicIpAddress $gw1pip1

New-AzVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gw1ipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance

New-AzLocalNetworkGateway -Name $LNGName6 -ResourceGroupName $RG1 -Location $Location1 -GatewayIpAddress $LNGIP6 -AddressPrefix $LNGPrefix61,$LNGPrefix62
```

### <a name="step-2---create-a-s2s-vpn-connection-with-an-ipsecike-policy"></a>Stap 2: een S2S-VPN-verbinding maken met een IPsec/IKE-beleid

#### <a name="1-create-an-ipsecike-policy"></a>1. Een IPsec/IKE-beleid maken

> [!IMPORTANT]
> U moet een IPsec/IKE-beleid maken om in te schakelen van de optie 'UsePolicyBasedTrafficSelectors' op de verbinding.

Het volgende voorbeeld wordt een IPsec/IKE-beleid met deze algoritmen en parameters:
* IKEv2: AES256, SHA384, DHGroup24
* IPsec: AES256, SHA256, PFS None, SA Lifetime 14400 seconds & 102400000KB

```azurepowershell-interactive
$ipsecpolicy6 = New-AzIpsecPolicy -IkeEncryption AES256 -IkeIntegrity SHA384 -DhGroup DHGroup24 -IpsecEncryption AES256 -IpsecIntegrity SHA256 -PfsGroup None -SALifeTimeSeconds 14400 -SADataSizeKilobytes 102400000
```

#### <a name="2-create-the-s2s-vpn-connection-with-policy-based-traffic-selectors-and-ipsecike-policy"></a>2. De S2S VPN-verbinding maken met op beleid gebaseerde verkeerkiezers en IPsec/IKE-beleid
Een S2S-VPN-verbinding maken en toepassen van het IPsec/IKE-beleid in de vorige stap hebt gemaakt. Houd rekening met de extra parameter '-UsePolicyBasedTrafficSelectors $True "waarmee op beleid gebaseerde verkeerkiezers voor de verbinding.

```azurepowershell-interactive
$vnet1gw = Get-AzVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng6 = Get-AzLocalNetworkGateway  -Name $LNGName6 -ResourceGroupName $RG1

New-AzVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng6 -Location $Location1 -ConnectionType IPsec -UsePolicyBasedTrafficSelectors $True -IpsecPolicies $ipsecpolicy6 -SharedKey 'AzureA1b2C3'
```

Na het voltooien van de stappen, de S2S VPN-verbinding gebruikt het IPsec/IKE-beleid dat is gedefinieerd, en inschakelen van op beleid gebaseerde verkeerkiezers voor de verbinding. U kunt dezelfde stappen als u wilt meer verbindingen toevoegen aan extra on-premises op beleid gebaseerde VPN-apparaten vanuit dezelfde Azure VPN-gateway herhalen.

## <a name="update-policy-based-traffic-selectors-for-a-connection"></a>Op beleid gebaseerde verkeerkiezers voor een verbinding bijwerken
De laatste sectie laat zien hoe u de optie voor het selectoren van verkeer op basis van beleid voor een bestaande S2S VPN-verbinding bijwerken.

### <a name="1-get-the-connection"></a>1. De verbinding
De verbindingsbron ophalen.

```azurepowershell-interactive
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1
```

### <a name="2-check-the-policy-based-traffic-selectors-option"></a>2. Controleer de optie voor het selectoren van verkeer op basis van beleid
De volgende regel geeft aan of de op beleid gebaseerde verkeerkiezers worden gebruikt voor de verbinding:

```azurepowershell-interactive
$connection6.UsePolicyBasedTrafficSelectors
```

Als de regel retourneert '**waar**", klikt u vervolgens op beleid gebaseerde verkeerkiezers zijn geconfigureerd op de verbinding; anders wordt geretourneerd '**False**."

### <a name="3-enabledisable-the-policy-based-traffic-selectors-on-a-connection"></a>3. De op beleid gebaseerde verkeerkiezers voor een verbinding inschakelen/uitschakelen
Als u beschikt over de verbindingsresource, kunt u inschakelen of uitschakelen van de optie.

#### <a name="to-enable-usepolicybasedtrafficselectors"></a>To Enable UsePolicyBasedTrafficSelectors
Het volgende voorbeeld wordt het verkeer op basis van beleid Selector-optie ingeschakeld, maar het IPsec/IKE-beleid ongewijzigd blijft:

```azurepowershell-interactive
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1

Set-AzVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -UsePolicyBasedTrafficSelectors $True
```

#### <a name="to-disable-usepolicybasedtrafficselectors"></a>To Disable UsePolicyBasedTrafficSelectors
In het volgende voorbeeld wordt het verkeer op basis van beleid Selector optie uitgeschakeld, maar het IPsec/IKE-beleid ongewijzigd blijft:

```azurepowershell-interactive
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1

Set-AzVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -UsePolicyBasedTrafficSelectors $False
```

## <a name="next-steps"></a>Volgende stappen
Wanneer de verbinding is voltooid, kunt u virtuele machines aan uw virtuele netwerken toevoegen. Zie [Een virtuele machine maken](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) voor de stappen.

Bekijk ook [configureren IPsec/IKE-beleid voor S2S VPN- of VNet-naar-VNet-verbindingen](vpn-gateway-ipsecikepolicy-rm-powershell.md) voor meer informatie over aangepast IPsec/IKE-beleid.
