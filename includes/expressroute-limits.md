---
title: bestand opnemen
description: bestand opnemen
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: include
ms.date: 06/12/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: c025c431d826d3a2951a9eb5c09308695e172887
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66170683"
---
| Resource | Standaard-/ maximumlimiet |
| --- | --- |
| ExpressRoute-circuits per abonnement |10 |
| ExpressRoute-circuits per regio per abonnement, met Azure Resource Manager |10 |
| Maximum aantal routes voor persoonlijke Azure-peering met ExpressRoute Standard |4,000 |
| Maximum aantal routes voor persoonlijke Azure-peering met ExpressRoute Premium-invoegonderdeel |10.000 |
| Maximum aantal routes voor persoonlijke Azure-peering van de VNet-adresruimte voor een ExpressRoute-verbinding |200 | 
| Maximum aantal routes voor Microsoft Azure-peering met ExpressRoute Standard |200 |
| Maximum aantal routes voor Microsoft Azure-peering met ExpressRoute Premium-invoegonderdeel |200 |
| Maximum aantal ExpressRoute-circuits die zijn gekoppeld aan hetzelfde virtuele netwerk op dezelfde locatie bevinden peering |4 |
| Maximum aantal ExpressRoute-circuits die zijn gekoppeld aan hetzelfde virtuele netwerk op verschillende locaties voor peering |4 |
| Aantal virtuele netwerk-koppelingen per ExpressRoute-circuit toegestaan |Zie de volgende tabel. |

#### <a name="number-of-virtual-networks-per-expressroute-circuit"></a>Aantal virtuele netwerken per ExpressRoute-circuit
| **De grootte van het circuit** | **Aantal koppelingen tussen virtuele netwerken voor Standard** | **Aantal virtuele netwerkkoppelingen met Premium-invoegonderdeel** |
| --- | --- | --- |
| 50 Mbps |10 |20 |
| 100 Mbps |10 |25 |
| 200 Mbps |10 |25 |
| 500 Mbps |10 |40 |
| 1 Gbps |10 |50 |
| 2 Gbps |10 |60 |
| 5 Gbps |10 |75 |
| 10 Gbps |10 |100 |
| 40 Gbps* |10 |100 |
| 100 Gbps* |10 |100 |

* Alleen Direct ExpressRoute
