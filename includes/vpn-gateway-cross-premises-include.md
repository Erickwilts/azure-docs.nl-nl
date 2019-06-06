---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 05/16/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 8c82e3d2c978a7980d02bc686346acdcb21dd591
ms.sourcegitcommit: ef06b169f96297396fc24d97ac4223cabcf9ac33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/31/2019
ms.locfileid: "66425743"
---
|  | **Point-to-Site** | **Site-to-Site** | **ExpressRoute** |
| --- | --- | --- | --- |
| **Door Azure ondersteunde services** |Cloudservices en virtuele machines |Cloudservices en virtuele machines |[Lijst met services](../articles/expressroute/expressroute-faqs.md#supported-services) |
| **Typische bandbreedten** |Gebaseerd op de gateway-SKU |Doorgaans < 1 Gbps gecombineerd |50 Mbps, 100 Mbps, 200 Mbps, 500 Mbps, 1 Gbps, 2 Gbps, 5 Gbps, 10 Gbps |
| **Ondersteunde protocollen** |Secure Sockets Tunneling Protocol (SSTP), OpenVPN en IPsec |IPsec |Directe verbinding via VLAN's, NSP’s VPN-technologieën (MPLS, VPLS,...) |
| **Routering** |RouteBased (dynamisch) |We ondersteunen PolicyBased (statische routering) en RouteBased (dynamische routering via VPN) |BGP |
| **Verbindingstolerantie** |actief-passief |actief-passief of actief-actief |actief-actief |
| **Typische gebruiksscenario's** |Maken van een prototype, dev/testen/labscenario's voor cloudservices en virtuele machines |Dev/testen/labscenario's en kleinschalige productie-workloads voor cloudservices en virtuele machines |Toegang tot alle Azure-services (gevalideerde lijst), workloads op bedrijfsniveau en bedrijfskritieke taken, back-up, Big Data, Azure als een DR-site |
| **SLA** |[SLA](https://azure.microsoft.com/support/legal/sla/) |[SLA](https://azure.microsoft.com/support/legal/sla/) |[SLA](https://azure.microsoft.com/support/legal/sla/) |
| **Prijzen** |[Prijzen](https://azure.microsoft.com/pricing/details/vpn-gateway/) |[Prijzen](https://azure.microsoft.com/pricing/details/vpn-gateway/) |[Prijzen](https://azure.microsoft.com/pricing/details/expressroute/) |
| **Technische documentatie** |[VPN Gateway-documentatie](https://azure.microsoft.com/documentation/services/vpn-gateway/) |[VPN Gateway-documentatie](https://azure.microsoft.com/documentation/services/vpn-gateway/) |[Documentatie voor ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/) |
| **Veelgestelde vragen** |[Veelgestelde vragen VPN Gateway](../articles/vpn-gateway/vpn-gateway-vpn-faq.md) |[Veelgestelde vragen VPN Gateway](../articles/vpn-gateway/vpn-gateway-vpn-faq.md) |[Veelgestelde vragen over ExpressRoute](../articles/expressroute/expressroute-faqs.md) |
