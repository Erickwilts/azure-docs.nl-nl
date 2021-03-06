---
title: IP-groepen in Azure Firewall
description: Met IP-groepen kunt u IP-adressen voor Azure Firewall regels groeperen en beheren.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: conceptual
ms.date: 07/30/2020
ms.author: victorh
ms.openlocfilehash: c56298d68bddd326ed2b32215f30a9057fa51e85
ms.sourcegitcommit: 8e7316bd4c4991de62ea485adca30065e5b86c67
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/17/2020
ms.locfileid: "94653110"
---
# <a name="ip-groups-in-azure-firewall"></a>IP-groepen in Azure Firewall

Met IP-groepen kunt u IP-adressen voor Azure Firewall regels op de volgende manieren groeperen en beheren:

- Als bron adres in DNAT-regels
- Als bron-of doel adres in netwerk regels
- Als bron adres in toepassings regels


Een IP-groep kan één IP-adres, meerdere IP-adressen of een of meer IP-adresbereiken hebben.

IP-groepen kunnen opnieuw worden gebruikt in Azure Firewall DNAT-, netwerk-en toepassings regels voor meerdere firewalls voor verschillende regio's en abonnementen in Azure. Groeps namen moeten uniek zijn. U kunt een IP-groep configureren in de Azure Portal, Azure CLI of REST API. Er wordt een voorbeeld sjabloon gegeven om u te helpen aan de slag te gaan.

## <a name="sample-format"></a>Sample-indeling

De volgende IPv4-adres notatie voorbeelden zijn geldig voor gebruik in IP-groepen:

- Eén adres: 10.0.0.0
- CIDR-notatie: 10.1.0.0/32
- Adres bereik: 10.2.0.0-10.2.0.31

## <a name="create-an-ip-group"></a>Een IP-groep maken

Een IP-groep kan worden gemaakt met behulp van de Azure Portal, Azure CLI of REST API. Zie [een IP-groep maken](create-ip-group.md)voor meer informatie.

## <a name="browse-ip-groups"></a>Bladeren door IP-groepen
1. In de Azure Portal zoek balk typt u **IP-groepen** en selecteert u deze. U kunt de lijst met IP-groepen zien of u kunt **toevoegen** selecteren om een nieuwe IP-groep te maken.
2. Selecteer een IP-groep om de pagina overzicht te openen. U kunt IP-adressen of IP-groepen bewerken, toevoegen of verwijderen.

   ![Overzicht van IP-groepen](media/ip-groups/overview.png)

## <a name="manage-an-ip-group"></a>Een IP-groep beheren

U kunt alle IP-adressen in de IP-groep en de bijbehorende regels of bronnen weer geven. Als u een IP-groep wilt verwijderen, moet u eerst de IP-groep ontkoppelen van de resource die deze gebruikt.

1. Als u de IP-adressen wilt weer geven of bewerken, selecteert u **IP-adressen** onder **instellingen** in het linkerdeel venster.
2. Als u één of meerdere IP-adressen wilt toevoegen, selecteert u **IP-adressen toevoegen**. Hiermee opent u de pagina **slepen of bladeren** voor een upload, of u kunt het adres hand matig invoeren.
3.    Selecteer de beletsel tekens (**...**) aan de rechter kant om IP-adressen te bewerken of te verwijderen. Als u meerdere IP-adressen wilt bewerken of verwijderen, selecteert u de vakken en selecteert u bovenaan **bewerken** of **verwijderen** .
4. Ten slotte kan het bestand in de CSV-bestands indeling worden geëxporteerd.

> [!NOTE]
> Als u alle IP-adressen in een IP-groep verwijdert terwijl deze nog steeds in een regel wordt gebruikt, wordt die regel overgeslagen.


## <a name="use-an-ip-group"></a>Een IP-groep gebruiken

U kunt nu **IP-groep** selecteren als **bron type** of **doel type** voor IP-adres (sen) wanneer u Azure firewall DNAT-, toepassings-of netwerk regels maakt.

![IP-groepen in de firewall](media/ip-groups/fw-ipgroup.png)

## <a name="region-availability"></a>Beschikbaarheid in regio’s

IP-groepen zijn beschikbaar in alle open bare Cloud regio's.

## <a name="ip-address-limits"></a>IP-adres limieten

U kunt Maxi maal 100 IP-groepen per firewall hebben met een maximum van 5000 afzonderlijke IP-adressen of IP-voor voegsels per IP-groep.

## <a name="related-azure-powershell-cmdlets"></a>Gerelateerde Azure PowerShell-cmdlets

De volgende Azure PowerShell-cmdlets kunnen worden gebruikt voor het maken en beheren van IP-groepen:

- [New-AzIpGroup](/powershell/module/az.network/new-azipgroup?view=azps-3.4.0)
- [Remove-AzIPGroup](/powershell/module/az.network/remove-azipgroup?view=azps-3.4.0)
- [Get-AzIpGroup](/powershell/module/az.network/get-azipgroup?view=azps-3.4.0)
- [Set-AzIpGroup](/powershell/module/az.network/set-azipgroup?view=azps-3.4.0)
- [New-AzFirewallNetworkRule](/powershell/module/az.network/new-azfirewallnetworkrule?view=azps-3.4.0)
- [New-AzFirewallApplicationRule](/powershell/module/az.network/new-azfirewallapplicationrule?view=azps-3.4.0)
- [New-AzFirewallNatRule](/powershell/module/az.network/new-azfirewallnatrule?view=azps-3.4.0)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [implementeren en configureren van een Azure firewall](tutorial-firewall-deploy-portal.md).