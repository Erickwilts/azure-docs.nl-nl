---
title: 'Een virtueel netwerk koppelen aan een ExpressRoute-circuit: PowerShell: klassiek: Azure | Microsoft Docs'
description: Dit document bevat een overzicht van virtuele netwerken (VNets) koppelen aan ExpressRoute-circuits met behulp van het klassieke implementatiemodel en PowerShell.
services: expressroute
documentationcenter: na
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 07/27/2018
ms.author: cherylmc
ms.openlocfilehash: 21676ff329613f792d6570713f044bb7440e58d4
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60370625"
---
# <a name="connect-a-virtual-network-to-an-expressroute-circuit-using-powershell-classic"></a>Een virtueel netwerk verbinden met een ExpressRoute-circuit met behulp van PowerShell (klassiek)
> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-linkvnet-arm.md)
> * [Azure-CLI](howto-linkvnet-cli.md)
> * [Video - Azure portal](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [PowerShell (klassiek)](expressroute-howto-linkvnet-classic.md)
>

In dit artikel krijgt u virtuele netwerken (VNets) koppelen aan Azure ExpressRoute-circuits met behulp van PowerShell. Een enkel VNet kan worden gekoppeld aan maximaal vier ExpressRoute-circuits. Gebruik de stappen in dit artikel om te maken van een nieuwe koppeling naar elk ExpressRoute-circuit dat u verbinding maakt. De ExpressRoute-circuits kunnen zich in hetzelfde abonnement, verschillende abonnementen of een combinatie van beide. In dit artikel is van toepassing op virtuele netwerken die zijn gemaakt met het klassieke implementatiemodel.

U kunt maximaal 10 virtuele netwerken koppelen aan een ExpressRoute-circuit. Alle virtuele netwerken moeten zich in dezelfde geopolitieke regio. U kunt een groter aantal virtuele netwerken aan uw ExpressRoute-circuit of virtuele netwerken koppelen die zich in andere geopolitieke regio's als u de invoegtoepassing ExpressRoute premium inschakelen koppelen. Controleer de [Veelgestelde vragen over](expressroute-faqs.md) voor meer informatie over de premium-invoegtoepassing.

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

**Over Azure-implementatiemodellen**

[!INCLUDE [vpn-gateway-classic-rm](../../includes/vpn-gateway-classic-rm-include.md)]


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="configuration-prerequisites"></a>Configuratievereisten

* Controleer de [vereisten](expressroute-prerequisites.md), [routeringsvereisten](expressroute-routing.md), en [werkstromen](expressroute-workflows.md) voordat u begint met de configuratie.
* U moet een actief ExpressRoute-circuit hebben.
   * Volg de instructies voor [maken van een ExpressRoute-circuit](expressroute-howto-circuit-classic.md) en vraag uw connectiviteitsprovider het circuit inschakelen.
   * Zorg ervoor dat u Azure private peering is geconfigureerd voor uw circuit hebt. Zie de [configureren routering](expressroute-howto-routing-classic.md) artikel voor routeringsinstructies.
   * Zorg ervoor dat de persoonlijke Azure-peering is geconfigureerd en van de BGP-peering tussen uw netwerk en Microsoft is, zodat u end-to-end-connectiviteit kunt inschakelen.
   * Hebt u een virtueel netwerk en een virtuele netwerkgateway gemaakt en volledig is ingericht. Volg de instructies voor [een virtueel netwerk configureren voor ExpressRoute](expressroute-howto-vnet-portal-classic.md).

### <a name="download-the-latest-powershell-cmdlets"></a>Download de meest recente PowerShell-cmdlets

Installeer de nieuwste versies van de Azure SM (Service Management) PowerShell-modules en de ExpressRoute-module. Wanneer u het volgende voorbeeld, houd er rekening mee dat het versienummer (in dit voorbeeld 5.1.1) worden gewijzigd als nieuwere versies van de cmdlets worden vrijgegeven.

```powershell
Import-Module 'C:\Program Files\WindowsPowerShell\Modules\Azure\5.1.1\Azure\Azure.psd1'
Import-Module 'C:\Program Files\WindowsPowerShell\Modules\Azure\5.1.1\ExpressRoute\ExpressRoute.psd1'
```

Als u meer informatie over Azure PowerShell, Zie [aan de slag met Azure PowerShell-cmdlets](/powershell/azure/overview) voor stapsgewijze instructies over het configureren van uw computer voor het gebruik van de Azure PowerShell-modules.

### <a name="sign-in"></a>Aanmelden

Als u wilt aanmelden bij uw Azure-account, moet u de volgende voorbeelden gebruiken:

1. Open de PowerShell-console met verhoogde rechten en maak verbinding met uw account.

   ```powershell
   Connect-AzAccount
   ```
2. Controleer de abonnementen voor het account.

   ```powershell
   Get-AzSubscription
   ```
3. Als u meerdere abonnementen hebt, selecteert u het abonnement dat u wilt gebruiken.

   ```powershell
   Select-AzSubscription -SubscriptionName "Replace_with_your_subscription_name"
   ```

4. Gebruik vervolgens de volgende cmdlet uit uw Azure-abonnement toevoegen aan PowerShell voor het klassieke implementatiemodel.

   ```powershell
   Add-AzureAccount
   ```

## <a name="connect-a-virtual-network-in-the-same-subscription-to-a-circuit"></a>Een virtueel netwerk in hetzelfde abonnement verbinden met een circuit
U kunt een virtueel netwerk koppelen aan een ExpressRoute-circuit met behulp van de volgende cmdlet. Zorg ervoor dat de virtuele netwerkgateway is gemaakt en gereed is voor het koppelen voordat u de cmdlet uitvoert.

```powershell
New-AzureDedicatedCircuitLink -ServiceKey "*****************************" -VNetName "MyVNet"
Provisioned
```
    
## <a name="remove-a-virtual-network-link-to-a-circuit"></a>Een virtueel netwerk koppelen aan een circuit verwijderen
U kunt een virtueel netwerk koppelen aan een ExpressRoute-circuit verwijderen met behulp van de volgende cmdlet. Zorg ervoor dat het huidige abonnement is geselecteerd voor het opgegeven virtuele netwerk. 

```powershell
Remove-AzureDedicatedCircuitLink -ServiceKey "*****************************" -VNetName "MyVNet"
```
 

## <a name="connect-a-virtual-network-in-a-different-subscription-to-a-circuit"></a>Een virtueel netwerk in een ander abonnement verbinden met een circuit
U kunt een ExpressRoute-circuit delen voor meerdere abonnementen. De volgende afbeelding toont een eenvoudig schema van hoe delen werkt voor ExpressRoute-circuits voor meerdere abonnementen.

Elk van de kleinere clouds binnen de grote cloud wordt gebruikt voor abonnementen die deel uitmaken van verschillende afdelingen binnen een organisatie. Elk van de afdelingen binnen de organisatie kan hun eigen abonnement gebruiken voor het implementeren van hun services, maar de afdelingen een enkel ExpressRoute-circuit terugverbinding maken met uw on-premises netwerk kan delen. Één afdeling (in dit voorbeeld: IT) kunt eigenaar van het ExpressRoute-circuit. Andere abonnementen binnen de organisatie kunnen het ExpressRoute-circuit gebruiken.

> [!NOTE]
> Connectiviteit en de bandbreedte kosten in rekening gebracht voor het toegewezen circuit wordt toegepast op de eigenaar van het ExpressRoute-circuit. Alle virtuele netwerken delen de dezelfde bandbreedte.
> 
> 

![Abonnementoverschrijdende connectiviteit](./media/expressroute-howto-linkvnet-classic/cross-subscription.png)

### <a name="administration"></a>Beheer
De *circuiteigenaar* is de beheerder/CO-beheerder van het abonnement waarin het ExpressRoute-circuit is gemaakt. De circuiteigenaar van het kunt autoriseren beheerders/medebeheerders van andere abonnementen, aangeduid als *circuit gebruikers*moeten worden gebruikt het toegewezen circuit waarvan ze eigenaar. Circuitgebruikers die zijn gemachtigd voor het gebruik van de organisatie ExpressRoute-circuit kunnen het virtuele netwerk in het abonnement koppelen aan het ExpressRoute-circuit nadat ze zijn gemachtigd.

De circuiteigenaar van het heeft de mogelijkheid om te wijzigen en autorisaties op elk gewenst moment intrekken. Een autorisatie intrekken resulteert in alle koppelingen wordt verwijderd uit het abonnement waarvan toegang is ingetrokken.

### <a name="circuit-owner-operations"></a>Circuit eigenaar bewerkingen

**Het maken van een autorisatie**

De circuiteigenaar van het machtigt de beheerders van andere abonnementen het opgegeven circuit gebruiken. In het volgende voorbeeld kan de beheerder van het circuit (Contoso IT) de beheerder van een ander abonnement (Dev-Test) tot twee virtuele netwerken koppelen aan het circuit. De Contoso IT-beheerder kan dit door op te geven de ontwikkel-en Microsoft-account. De cmdlet verzendt geen e-mailbericht naar de opgegeven id van Microsoft. De circuiteigenaar van het moet expliciet op de hoogte stellen andere eigenaar van het abonnement dat de autorisatie voltooid is.

```powershell
New-AzureDedicatedCircuitLinkAuthorization -ServiceKey "**************************" -Description "Dev-Test Links" -Limit 2 -MicrosoftIds 'devtest@contoso.com'
```

  Resultaat:

  ```powershell
  Description         : Dev-Test Links
  Limit               : 2
  LinkAuthorizationId : **********************************
  MicrosoftIds        : devtest@contoso.com
  Used                : 0
  ```

**Machtigingen controleren**

De circuiteigenaar van het kunt bekijken van alle machtigingen die op een bepaalde circuit zijn uitgegeven door de volgende cmdlet uit te voeren:

```powershell
Get-AzureDedicatedCircuitLinkAuthorization -ServiceKey: "**************************"
```
  Resultaat:

  ```powershell
  Description         : EngineeringTeam
  Limit               : 3
  LinkAuthorizationId : ####################################
  MicrosoftIds        : engadmin@contoso.com
  Used                : 1

  Description         : MarketingTeam
  Limit               : 1
  LinkAuthorizationId : @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
  MicrosoftIds        : marketingadmin@contoso.com
  Used                : 0

  Description         : Dev-Test Links
  Limit               : 2
  LinkAuthorizationId : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
  MicrosoftIds        : salesadmin@contoso.com
  Used                : 2
  ```

**Bijwerken van autorisaties**

De circuiteigenaar van het kunt autorisaties wijzigen met behulp van de volgende cmdlet:

```powershell
Set-AzureDedicatedCircuitLinkAuthorization -ServiceKey "**************************" -AuthorizationId "&&&&&&&&&&&&&&&&&&&&&&&&&&&&"-Limit 5
```

  Resultaat:

  ```powershell
  Description         : Dev-Test Links
  Limit               : 5
  LinkAuthorizationId : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
  MicrosoftIds        : devtest@contoso.com
  Used                : 0
  ```

**Verwijderen van autorisaties**

De circuiteigenaar van het kunt intrekken/verwijderen autorisaties voor de gebruiker door de volgende cmdlet:

```powershell
Remove-AzureDedicatedCircuitLinkAuthorization -ServiceKey "*****************************" -AuthorizationId "###############################"
```

### <a name="circuit-user-operations"></a>Circuit gebruikersbewerkingen

**Machtigingen controleren**

De gebruiker circuit kunt autorisaties bekijken met behulp van de volgende cmdlet:

```powershell
Get-AzureAuthorizedDedicatedCircuit
```

  Resultaat:

  ```powershell
  Bandwidth                        : 200
  CircuitName                      : ContosoIT
  Location                         : Washington DC
  MaximumAllowedLinks              : 2
  ServiceKey                       : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
  ServiceProviderName              : equinix
  ServiceProviderProvisioningState : Provisioned
  Status                           : Enabled
  UsedLinks                        : 0
  ```

**Autorisaties voor koppelingen met inwisselen**

De gebruiker circuit kan de volgende cmdlet als u wilt een koppeling autorisatie inwisselen uitvoeren:

```powershell
New-AzureDedicatedCircuitLink –servicekey "&&&&&&&&&&&&&&&&&&&&&&&&&&" –VnetName 'SalesVNET1'
```

  Resultaat:

  ```powershell
  State VnetName
  ----- --------
  Provisioned SalesVNET1
  ```

Voer deze opdracht uit in de zojuist gekoppelde abonnement voor het virtuele netwerk:

```powershell
New-AzureDedicatedCircuitLink -ServiceKey "*****************************" -VNetName "MyVNet"
```

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over ExpressRoute raadpleegt u de [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md).
