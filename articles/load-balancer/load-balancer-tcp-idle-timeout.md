---
title: load balancer TCP Reset en time-out voor inactiviteit in azure configureren
titleSuffix: Azure Load Balancer
description: In dit artikel vindt u informatie over het configureren van Azure Load Balancer TCP-time-out voor inactiviteit.
services: load-balancer
documentationcenter: na
author: asudbring
ms.custom: seodec18
ms.service: load-balancer
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/09/2020
ms.author: allensu
ms.openlocfilehash: b507fbad4d9089d918ae7a85c07f30efcb118476
ms.sourcegitcommit: 3bcce2e26935f523226ea269f034e0d75aa6693a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/23/2020
ms.locfileid: "92487242"
---
# <a name="configure-tcp-idle-timeout-for-azure-load-balancer"></a>TCP-time-out voor inactiviteit voor Azure Load Balancer configureren

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u PowerShell lokaal wilt installeren en gebruiken, is voor dit artikel versie 5.4.1 of hoger van de Azure PowerShell-module vereist. Voer `Get-Module -ListAvailable Az` uit om te kijken welke versie is geïnstalleerd. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-Az-ps). Als u PowerShell lokaal uitvoert, moet u ook `Connect-AzAccount` uitvoeren om verbinding te kunnen maken met Azure.

Azure Load Balancer heeft het volgende time-outbereik voor inactiviteit:

4 minuten tot 100 minuten voor uitgaande regels 4 minuten tot 30 minuten voor Load Balancer regels en binnenkomende NAT-regels

Standaard is deze ingesteld op 4 minuten. Als een periode van inactiviteit langer is dan de time-outwaarde, is er geen garantie dat de TCP-of HTTP-sessie tussen de client en de Cloud service wordt bewaard. Meer informatie over [time-out voor TCP-inactiviteit](load-balancer-tcp-reset.md).

In de volgende secties wordt beschreven hoe u time-outinstellingen voor inactiviteit wijzigt voor open bare IP-en load balancer-resources.


## <a name="configure-the-tcp-idle-timeout-for-your-public-ip"></a>De time-out voor TCP-inactiviteit voor uw open bare IP configureren

```azurepowershell-interactive
$publicIP = Get-AzPublicIpAddress -Name MyPublicIP -ResourceGroupName MyResourceGroup
$publicIP.IdleTimeoutInMinutes = "15"
Set-AzPublicIpAddress -PublicIpAddress $publicIP
```

`IdleTimeoutInMinutes` is optioneel. Als deze niet is ingesteld, is de standaard time-out 4 minuten. 

## <a name="set-the-tcp-idle-timeout-on-rules"></a>De time-out voor inactiviteit van TCP instellen op regels

Als u de time-out voor inactiviteit voor een load balancer wilt instellen, wordt de IdleTimeoutInMinutes ingesteld op de regel met gelijke taak verdeling. Bijvoorbeeld:

```azurepowershell-interactive
$lb = Get-AzLoadBalancer -Name "MyLoadBalancer" -ResourceGroup "MyResourceGroup"
$lb | Set-AzLoadBalancerRuleConfig -Name myLBrule -IdleTimeoutInMinutes 15
```

## <a name="next-steps"></a>Volgende stappen

[Overzicht van interne load balancer](load-balancer-internal-overview.md)

[Aan de slag met het configureren van een Internet gerichte load balancer](quickstart-load-balancer-standard-public-powershell.md)

[Een distributiemodus voor de load balancer configureren](load-balancer-distribution-mode.md)
