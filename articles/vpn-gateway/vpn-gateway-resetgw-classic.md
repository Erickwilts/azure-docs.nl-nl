---
title: Een Azure VPN-gateway opnieuw instellen om IPsec-tunnels te maken | Microsoft Docs
description: In dit artikel wordt uitgelegd hoe u uw Azure VPN Gateway opnieuw instelt om IPsec-tunnels te herstellen. Het artikel is van toepassing op VPN-gateways in zowel het klassieke als het Resource Manager-implementatie model.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 07/05/2019
ms.author: cherylmc
ms.openlocfilehash: 359773dad53f333b2f052dd5b5481645c72746da
ms.sourcegitcommit: 040abc24f031ac9d4d44dbdd832e5d99b34a8c61
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2019
ms.locfileid: "69533932"
---
# <a name="reset-a-vpn-gateway"></a>Een VPN-gateway opnieuw instellen

Het opnieuw instellen van een Azure VPN-gateway is handig als u cross-premises VPN-connectiviteit verliest in een of meer Site-to-Site VPN-tunnels. In een dergelijke situatie functioneren al uw on-premises VPN-apparaten naar behoren, maar kunnen ze geen IPSec-tunnels tot stand brengen met de Azure VPN-gateways. Dit artikel helpt u bij het opnieuw instellen van uw VPN-gateway.

### <a name="what-happens-during-a-reset"></a>Wat gebeurt er tijdens het opnieuw instellen?

Een VPN-gateway bestaat uit twee VM-exemplaren die worden uitgevoerd in een configuratie met de modus actief/stand-by. Wanneer u de gateway opnieuw instelt, wordt de gateway opnieuw opgestart. vervolgens worden de cross-premises configuraties opnieuw toegepast. De gateway behoudt hetzelfde openbare IP-adres. Dat betekent dat u de VPN-routerconfiguratie niet hoeft bij te werken met een nieuw openbaar IP-adres voor de Azure VPN-gateway.

Wanneer u de opdracht geeft om de gateway opnieuw in te stellen, wordt de huidige actieve instantie van de Azure VPN-gateway onmiddellijk opnieuw opgestart. Tijdens de failover van het actieve exemplaar (dat opnieuw wordt opgestart) is er een korte ruimte beschikbaar voor de stand-by-instantie. Deze onderbreking zou niet langer dan een minuut moeten duren.

Als de verbinding na de eerste keer opnieuw opstarten niet is hersteld, voert u deze opdracht opnieuw uit om de tweede VM-instantie (de nieuwe actieve gateway) opnieuw op te starten. Als de twee keer opnieuw opstarten direct na elkaar wordt aangevraagd, duurt het iets langer omdat beide VM-exemplaren (actief en stand-by) opnieuw worden opgestart. Dit leidt tot een langere ruimte op de VPN-verbinding, tot 30 tot 45 minuten voor Vm's om het opnieuw opstarten te volt ooien.

Nadat twee keer opnieuw is opgestart, kunt u, als u nog steeds problemen hebt met cross-premises connectiviteit, een ondersteunings aanvraag openen via de Azure Portal.

## <a name="before"></a>Voordat u begint

Controleer voordat u de gateway opnieuw instelt de hieronder vermelde belangrijke zaken voor elke S2S-VPN-tunnel (site-naar-site) met IPsec. Als items niet overeenkomen, wordt de verbinding met S2SVPN-tunnels verbroken. Door de configuraties voor uw on-premises en Azure VPN-gateways te controleren en te corrigeren, bespaart u onnodig opnieuw opstarten en onderbrekingen voor de andere werkende verbindingen op de gateways.

Controleer de volgende items voordat u de gateway opnieuw instelt:

* Controleer of de internet-IP-adressen (VIP's) voor zowel de Azure VPN-gateway als de on-premises VPN-gateway correct zijn geconfigureerd in zowel het Azure- als het lokale VPN-beleid.
* De vooraf gedeelde sleutel moet hetzelfde zijn in de Azure- en de on-premises VPN-gateway.
* Als u een specifieke IPsec/IKE-configuratie, zoals versleuteling, hash-algoritmen en PFS (Perfect Forward Secrecy) toepast, moet u controleren of de Azure- en on-premises VPN-gateway dezelfde configuratie hebben.

## <a name="portal"></a>Azure-portal

U kunt een resource manager VPN-gateway opnieuw instellen met behulp van de Azure Portal. Als u een klassieke Gateway opnieuw wilt instellen, raadpleegt u de [Power shell](#resetclassic) -stappen.

### <a name="resource-manager-deployment-model"></a>Resource Manager-implementatiemodel

1. Open de [Azure Portal](https://portal.azure.com) en navigeer naar de gateway van het Resource Manager-virtuele netwerk die u opnieuw wilt instellen.
2. Klik op de Blade voor de gateway van het virtuele netwerk op opnieuw instellen.

   ![VPN Gateway Blade opnieuw instellen](./media/vpn-gateway-howto-reset-gateway/reset-vpn-gateway-portal.png)
3. Klik op de Blade opnieuw instellen op de knop **opnieuw instellen** .

## <a name="ps"></a>PowerShell

### <a name="resource-manager-deployment-model"></a>Resource Manager-implementatiemodel

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

De cmdlet voor het opnieuw instellen van een gateway is **Reset-AzVirtualNetworkGateway**. Zorg ervoor dat u beschikt over de nieuwste versie van de [Power shell AZ](https://docs.microsoft.com/powershell/module/az.network)-cmdlets voordat u de herstel bewerking uitvoert. In het volgende voor beeld wordt de gateway van een virtueel netwerk met de naam VNet1GW in de resource groep TestRG1 opnieuw ingesteld:

```powershell
$gw = Get-AzVirtualNetworkGateway -Name VNet1GW -ResourceGroupName TestRG1
Reset-AzVirtualNetworkGateway -VirtualNetworkGateway $gw
```

Daardoor

Wanneer u een retour resultaat ontvangt, kunt u aannemen dat de gateway opnieuw is ingesteld. Er is echter niets in het retour resultaat dat aangeeft dat het opnieuw instellen is geslaagd. Als u de geschiedenis nauw keurig wilt zien wanneer de gateway opnieuw is ingesteld, kunt u deze informatie bekijken in de [Azure Portal](https://portal.azure.com). Navigeer in de portal naar **' gatewaynaam '-> resource Health**.

### <a name="resetclassic"></a>Klassiek implementatie model

De cmdlet voor het opnieuw instellen van een gateway is **Reset-azurevnetgateway gebruikt**. De Azure PowerShell-cmdlets voor service management moeten lokaal op uw bureau blad zijn geïnstalleerd. U kunt Azure Cloud Shell niet gebruiken. Zorg ervoor dat u beschikt over de nieuwste versie van de [Power shell-cmdlets voor Service Management (SM)](https://docs.microsoft.com/powershell/azure/servicemanagement/install-azure-ps?view=azuresmps-4.0.0#azure-service-management-cmdlets)voordat u de herstel bewerking uitvoert. In het volgende voor beeld wordt de gateway voor een virtueel netwerk met de naam ' ContosoVNet ' opnieuw ingesteld:

```powershell
Reset-AzureVNetGateway –VnetName “ContosoVNet”
```

Daardoor

```powershell
Error          :
HttpStatusCode : OK
Id             : f1600632-c819-4b2f-ac0e-f4126bec1ff8
Status         : Successful
RequestId      : 9ca273de2c4d01e986480ce1ffa4d6d9
StatusCode     : OK
```

## <a name="cli"></a>Azure CLI

Als u de gateway opnieuw wilt instellen, gebruikt u de opdracht [AZ Network vnet-gateway reset](https://docs.microsoft.com/cli/azure/network/vnet-gateway) . In het volgende voor beeld wordt de gateway van een virtueel netwerk met de naam VNet5GW in de resource groep TestRG5 opnieuw ingesteld:

```azurecli
az network vnet-gateway reset -n VNet5GW -g TestRG5
```

Daardoor

Wanneer u een retour resultaat ontvangt, kunt u aannemen dat de gateway opnieuw is ingesteld. Er is echter niets in het retour resultaat dat aangeeft dat het opnieuw instellen is geslaagd. Als u de geschiedenis nauw keurig wilt zien wanneer de gateway opnieuw is ingesteld, kunt u deze informatie bekijken in de [Azure Portal](https://portal.azure.com). Navigeer in de portal naar **' gatewaynaam '-> resource Health**.
