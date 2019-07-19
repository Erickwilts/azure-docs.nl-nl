---
title: Een Web Application Firewall-beleid (WAF) configureren met aangepaste regels en standaard Ruse ingesteld voor de voor deur-Azure PowerShell
description: Meer informatie over het configureren van een WAF-beleid bestaat uit zowel aangepaste als beheerde regels voor een bestaand eind punt voor deuren.
services: frontdoor
documentationcenter: ''
author: KumudD
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/21/2019
ms.author: kumud
ms.reviewer: tyao
ms.openlocfilehash: e9509172ac96a601235cc16e0d6d83c9b2f51902
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2019
ms.locfileid: "67849123"
---
# <a name="configure-a-web-application-firewall-policy-using-azure-powershell"></a>Een Web Application Firewall beleid configureren met behulp van Azure PowerShell
Azure Web Application Firewall-beleid (WAF) definieert inspecties die vereist zijn wanneer een aanvraag aan de voor deur arriveert.
In dit artikel wordt beschreven hoe u een WAF-beleid configureert dat bestaat uit een aantal aangepaste regels en met door Azure beheerde standaard Ruse ingesteld.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten
Voordat u begint met het instellen van een beleid voor frequentie limieten, stelt u uw Power shell-omgeving in en maakt u een voor deur profiel.
### <a name="set-up-your-powershell-environment"></a>Uw PowerShell-omgeving instellen
Azure PowerShell voorziet in een set van cmdlets die gebruikmaken van het [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview)-model om uw Azure-resources te beheren. 

U kunt [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) op uw lokale computer installeren en in elke PowerShell-sessie gebruiken. Volg de instructies op de pagina om u aan te melden met uw Azure-referenties en de AZ Power shell-module te installeren.

#### <a name="sign-in-to-azure"></a>Aanmelden bij Azure
```
Connect-AzAccount

```
Voordat u de Front Door-module installeert, moet u controleren of u de nieuwste versie van PowerShellGet hebt geïnstalleerd. Voer de onderstaande opdracht uit en open PowerShell opnieuw.

```
Install-Module PowerShellGet -Force -AllowClobber
``` 

#### <a name="install-azfrontdoor-module"></a>De module AZ.-ingang installeren 

```
Install-Module -Name Az.FrontDoor
```
### <a name="create-a-front-door-profile"></a>Een voor deur profiel maken
Maak een voor deur profiel door de instructies te volgen die [worden beschreven in Quick Start: Een voor deur profiel maken](quickstart-create-front-door.md)

## <a name="custom-rule-based-on-http-parameters"></a>Aangepaste regel op basis van http-para meters

In het volgende voor beeld ziet u hoe u een aangepaste regel configureert met twee match-voor waarden met behulp van [New-AzFrontDoorWafMatchConditionObject](/powershell/module/az.frontdoor/new-azfrontdoorwafmatchconditionobject). Aanvragen zijn afkomstig van een opgegeven site, zoals gedefinieerd door de verwijzings punt, en de query reeks bevat geen ' wacht woord '. 

```powershell-interactive
$referer = New-AzFrontDoorWafMatchConditionObject -MatchVariable RequestHeader -OperatorProperty Equal -Selector "Referer" -MatchValue "www.mytrustedsites.com/referpage.html"
$password = New-AzFrontDoorWafMatchConditionObject -MatchVariable QueryString -OperatorProperty Contains -MatchValue "password"
$AllowFromTrustedSites = New-AzFrontDoorWafCustomRuleObject -Name "AllowFromTrustedSites" -RuleType MatchRule -MatchCondition $referer,$password -Action Allow -Priority 1
```

## <a name="custom-rule-based-on-http-request-method"></a>Aangepaste regel op basis van de HTTP-aanvraag methode
Maak een regel met behulp van [New-AzFrontDoorWafCustomRuleObject](/powershell/module/az.frontdoor/new-azfrontdoorwafcustomruleobject) als volgt:

```powershell-interactive
$put = New-AzFrontDoorWafMatchConditionObject -MatchVariable RequestMethod -OperatorProperty Equal -MatchValue PUT
$BlockPUT = New-AzFrontDoorWafCustomRuleObject -Name "BlockPUT" -RuleType MatchRule -MatchCondition $put -Action Block -Priority 2
```

## <a name="create-a-custom-rule-based-on-size-constraint"></a>Een aangepaste regel maken op basis van de grootte beperking

In het volgende voor beeld wordt een regel voor het blok keren van aanvragen met een URL die langer is dan 100 tekens gemaakt met behulp van Azure PowerShell:
```powershell-interactive
$url = New-AzFrontDoorWafMatchConditionObject -MatchVariable RequestUri -OperatorProperty GreaterThanOrEqual -MatchValue 100
$URLOver100 = New-AzFrontDoorWafCustomRuleObject -Name "URLOver100" -RuleType MatchRule -MatchCondition $url -Action Block -Priority 3
```
## <a name="add-managed-default-rule-set"></a>Beheerde standaard regelset toevoegen

In het volgende voor beeld wordt een beheerde standaardregelset gemaakt met behulp van Azure PowerShell:
```powershell-interactive
$managedRules =  New-AzFrontDoorWafManagedRuleObject -Type DefaultRuleSet -Version 1.0
```
## <a name="configure-a-security-policy"></a>Een beveiligings beleid configureren

Zoek de naam van de resource groep die het voorste deur profiel bevat met `Get-AzResourceGroup`behulp van. Vervolgens configureert u een beveiligings beleid met gemaakte regels in de vorige stappen met behulp van [New-AzFrontDoorWafPolicy](/powershell/module/az.frontdoor/new-azfrontdoorwafpolicy) in de opgegeven resource groep die het voorste deur profiel bevat.

```powershell-interactive
$myWAFPolicy=New-AzFrontDoorWafPolicy -Name $policyName -ResourceGroupName $resourceGroupName -Customrule $AllowFromTrustedSites,$BlockPUT,$URLOver100 -ManagedRule $managedRules -EnabledState Enabled -Mode Prevention
```

## <a name="link-policy-to-a-front-door-front-end-host"></a>Beleid koppelen aan een front-end-host voor de voor deur
Koppel het object voor het beveiligings beleid aan een bestaande front-endwebserver front-end-host en werk front-deur eigenschappen bij. Haal eerst het voorste deur object op met [Get-AzFrontDoor](/powershell/module/Az.FrontDoor/Get-AzFrontDoor).
Stel vervolgens de front-end *WebApplicationFirewallPolicyLink* -eigenschap in op de *resourceId* van ' $myWAFPolicy $ ' die in de vorige stap is gemaakt met [set-AzFrontDoor](/powershell/module/Az.FrontDoor/Set-AzFrontDoor). 

In het onderstaande voor beeld wordt de naam van de resource groep *myResourceGroupFD1* met de veronderstelling dat u het voorste deur profiel hebt gemaakt met [behulp van de instructies in de Snelstartgids: Maak een artikel voor](quickstart-create-front-door.md) de voor deur. Vervang ook in het onderstaande voor beeld $frontDoorName door de naam van uw voorste deur profiel. 

```powershell-interactive
   $FrontDoorObjectExample = Get-AzFrontDoor `
     -ResourceGroupName myResourceGroupFD1 `
     -Name $frontDoorName
   $FrontDoorObjectExample[0].FrontendEndpoints[0].WebApplicationFirewallPolicyLink = $myWAFPolicy.Id
   Set-AzFrontDoor -InputObject $FrontDoorObjectExample[0]
 ```

> [!NOTE]
> U hoeft slechts één keer op de eigenschap *WebApplicationFirewallPolicyLink* in te stellen om een beveiligings beleid te koppelen aan de front-end van de voor deur. Volgende beleids updates worden automatisch toegepast op de front-end.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de [voor deur](front-door-overview.md) 
- Meer informatie over [WAF voor de voor deur](waf-overview.md)
