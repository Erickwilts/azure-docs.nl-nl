---
title: bestand opnemen
description: bestand opnemen
services: virtual-network
author: jimdial
ms.service: virtual-network
ms.topic: include
ms.date: 04/09/2018
ms.author: jdial
ms.custom: include file
ms.openlocfilehash: 0c3bc3f2995131c7777bfc48269a17fceda33192
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67175959"
---
> [!div class="op_single_selector"]
> * [Azure Portal](../articles/virtual-network/virtual-network-multiple-ip-addresses-portal.md)
> * [PowerShell](../articles/virtual-network/virtual-network-multiple-ip-addresses-powershell.md)
> * [Azure-CLI](../articles/virtual-network/virtual-network-multiple-ip-addresses-cli.md)
>

Aan een virtuele Azure-machine zijn een of meer netwerkinterfaces (NIC's) gekoppeld. Aan elke NIC kunnen een of meer statische of dynamische openbare en privé-IP-adressen worden toegewezen. Door meerdere IP-adressen toe te wijzen aan een virtuele machine, hebt u de volgende mogelijkheden:

* Het hosten van meerdere websites of services met verschillende IP-adressen en SSL-certificaten op één server.
* Het fungeren als een virtueel netwerkapparaat, zoals een firewall of load balancer.
* De mogelijkheid om een van de privé-IP-adressen voor een van de NIC's toe te voegen aan een Azure Load Balancer-back-endgroep. In het verleden kon alleen het primaire IP-adres voor de primaire NIC worden toegevoegd aan een back-endgroep. In het artikel [Load balancing on multiple IP configurations](../articles/load-balancer/load-balancer-multiple-ip.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Taakverdeling op meerdere IP-configuraties) vindt u meer informatie over de taakverdeling in geval van meerdere IP-configuraties.

Aan elke NIC die aan een virtuele machine is gekoppeld, zijn een of meer IP-configuraties gekoppeld. Aan elke configuratie is één statisch of dynamisch privé-IP-adres toegewezen. Aan elke configuratie kan ook één resource met een openbaar IP-adres zijn gekoppeld. Aan een resource met een openbaar IP-adres is een dynamisch of een statisch openbaar IP-adres toegewezen. In het artikel [IP addresses in Azure](../articles/virtual-network/virtual-network-ip-addresses-overview-arm.md) (IP-adressen in Azure) vindt u meer informatie over IP-adressen in Azure. 

Er is een limiet aan het aantal privé IP-adressen kan worden toegewezen aan een NIC wordt gebruikt. Er is ook een limiet aan het aantal openbare IP-adressen die kunnen worden gebruikt in een Azure-abonnement. Raadpleeg het artikel [Azure limits](../articles/azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) (Beperkingen van Azure) voor meer informatie.
