---
title: Wat is Azure Load Balancer?
titleSuffix: Azure Load Balancer
description: Overzicht van Azure Load Balancer-functies, -architectuur en -implementatie. Informatie over hoe de Load Balancer werkt en hoe u deze kunt gebruiken in de cloud.
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
Customer intent: As an IT administrator, I want to learn more about the Azure Load Balancer service and what I can use it for.
ms.devlang: na
ms.topic: overview
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 1/14/2020
ms.author: allensu
ms.openlocfilehash: 602a36964d4a6d5f3660499570327db3479c980e
ms.sourcegitcommit: 58d3b3314df4ba3cabd4d4a6016b22fa5264f05a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/02/2020
ms.locfileid: "89299198"
---
# <a name="what-is-azure-load-balancer"></a>Wat is Azure Load Balancer?

*Taakverdeling* verwijst naar het gelijkmatig verdelen van belasting (binnenkomend netwerkverkeer) in een groep back-endservers of servers. 

Azure Load Balancer werkt op laag vier van het OSI-model (Open Systems Interconnection). Dit is het enige contactpunt voor clients. Load Balancer verdeelt inkomende stromen die aankomen op de front-end-naar-back-endpoolinstanties van de Load Balancer. Deze stromen zijn gebaseerd op geconfigureerde taakverdelingsregels en statuscontroles. De exemplaren van de back-endadresgroep kunnen virtuele Azure-machines of exemplaren in een schaalset voor virtuele machines zijn.

Een **[openbare load balancer](./components.md#frontend-ip-configurations)** kan uitgaande verbindingen bieden voor virtuele machines (VM's) in uw virtuele netwerk. Deze verbindingen worden tot stand gebracht door de privé-IP-adressen te vertalen naar openbare IP-adressen. Openbare load balancers worden gebruikt voor het verdelen van internetverkeer naar uw VM's.

Een **[interne (of privé) load balancer](./components.md#frontend-ip-configurations)** wordt alleen gebruikt wanneer privé-IP's alleen op de front-end vereist zijn. Interne load balancers worden gebruikt voor het verdelen van verkeer binnen een virtueel netwerk. In een hybride scenario kan een front-end van een load balancer ook worden bereikt vanaf een on-premises netwerk.

<p align="center">
  <img src="./media/load-balancer-overview/load-balancer.svg" alt="Figure depicts both public and internal load balancers directing traffic to port 80 on multiple servers on a Web tier and port 443 on multiple servers on a business tier." width="512" title="Azure Load Balancer">
</p>

*Afbeelding: verdeling die wordt toegepast op toepassingen met meerdere lagen waarvoor zowel een openbare als een interne load balancer wordt gebruikt*

Zie [Azure Load Balancer-componenten](./components.md) voor meer informatie over de afzonderlijke componenten van de load balancer.

## <a name="why-use-azure-load-balancer"></a>Waarom Azure Load Balancer gebruiken?
Met Standard Load Balancer kunt u uw toepassingen schalen en services met hoge beschikbaarheid realiseren. Load Balancer biedt ondersteuning voor scenario's met inkomend en uitgaand verkeer. Load Balancer biedt lage latentie en hoge doorvoer en kan omhoog worden geschaald tot miljoenen stromen voor alle TCP- en UDP-toepassingen.

De belangrijkste scenario's die u kunt uitvoeren met Standard Load Balancer zijn onder meer:

- Taakverdeling van **[intern](https://docs.microsoft.com/azure/load-balancer/tutorial-load-balancer-standard-internal-portal)** en **[extern](https://docs.microsoft.com/azure/load-balancer/tutorial-load-balancer-standard-manage-portal)** verkeer naar virtuele machines van Azure.

- De beschikbaarheid vergroten door resources **[binnen](https://docs.microsoft.com/azure/load-balancer/tutorial-load-balancer-standard-public-zonal-portal)** en **[tussen](https://docs.microsoft.com/azure/load-balancer/tutorial-load-balancer-standard-public-zone-redundant-portal)** zones te distribueren.

- **[Uitgaande connectiviteit ](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections)** configureren voor virtuele machines van Azure.

- **[Statuscontroles](https://docs.microsoft.com/azure/load-balancer/load-balancer-custom-probe-overview)** gebruiken voor het bewaken van resources met taakverdeling.

- **[Port forwarding](https://docs.microsoft.com/azure/load-balancer/tutorial-load-balancer-port-forwarding-portal)** gebruiken om toegang te krijgen tot virtuele machines in een virtueel netwerk via een openbaar IP-adres en poort.

- Ondersteuning voor **[taakverdeling](https://docs.microsoft.com/azure/virtual-network/virtual-network-ipv4-ipv6-dual-stack-standard-load-balancer-powershell)** van **[IPv6](https://docs.microsoft.com/azure/virtual-network/ipv6-overview)** inschakelen.

- Standard Load Balancer biedt multidimensionale metrische gegevens via [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview).  Deze metrische gegevens kunnen worden gefilterd, gegroepeerd en uitgesplitst voor een bepaalde dimensie.  Ze bieden actuele en historische inzichten in de prestaties en status van uw service.  Resource Health wordt ook ondersteund. Raadpleeg **[Diagnostische tests van Standard Load Balancer](load-balancer-standard-diagnostics.md)** voor meer informatie.

- Taakverdeling van services toepassen op **[meerdere poorten, meerdere IP-adressen of allebei](https://docs.microsoft.com/azure/load-balancer/load-balancer-multivip-overview)** .

- **[Interne](https://docs.microsoft.com/azure/load-balancer/move-across-regions-internal-load-balancer-portal)** en **[externe](https://docs.microsoft.com/azure/load-balancer/move-across-regions-external-load-balancer-portal)** resources tussen Azure-regio's verplaatsen.

- Taakverdeling toepassen van de TCP- en UDP-stroom op alle poorten tegelijk met **[HA-poorten](https://docs.microsoft.com/azure/load-balancer/load-balancer-ha-ports-overview)** .

### <a name="secure-by-default"></a><a name="securebydefault"></a>Standaardbeveiliging

Standard Load Balancer is gebouwd op basis van het beveiligingsmodel voor vertrouwensrelaties van het netwerk. Standard Load Balancer zorgt standaard voor beveiliging en maakt deel uit van uw virtuele netwerk. Het virtuele netwerk is een privé- en geïsoleerd netwerk.  Dit betekent dat Standard Load Balancers en standaard openbare IP-adressen zijn gesloten voor inkomende stromen, tenzij ze worden geopend door netwerkbeveiligingsgroepen. Netwerkbeveiligingsgroepen worden gebruikt om toegestaan verkeer expliciet toe te staan.  Als u geen netwerkbeveiligingsgroep op een subnet of NIC van uw virtuele-machineresource hebt, mag het verkeer deze bron niet bereiken. Zie [Netwerkbeveiligingsgroepen](../virtual-network/security-overview.md) voor meer informatie over netwerkbeveiligingsgroepen en hoe u deze toepast voor uw scenario.
Basis Load Balancer is standaard open voor internet. Daarnaast slaat Load Balancer geen klantgegevens op.

## <a name="pricing-and-sla"></a>Prijzen en SLA

Ga naar de pagina [Prijzen voor Load Balancer](https://azure.microsoft.com/pricing/details/load-balancer/) voor prijsinformatie over Standard Load Balancer.
Basic Load Balancer wordt gratis aangeboden.
Zie [SLA voor Load Balancer](https://aka.ms/lbsla). Basic Load Balancer heeft geen SLA.

## <a name="whats-new"></a>Wat is nieuw?

Abonneer u op de RSS-feed en bekijk de nieuwste updates voor Azure Load Balancer-functies op de pagina [Azure-updates](https://azure.microsoft.com/updates/?category=networking&query=load%20balancer).

## <a name="next-steps"></a>Volgende stappen
Zie [Een Basic Load Balancer upgraden](upgrade-basic-standard.md) voor informatie over het upgraden van Basic Load Balancer naar Standard Load Balancer.

Zie [Een openbare Standard Load Balancer maken](quickstart-load-balancer-standard-public-portal.md) om aan de slag te gaan met een Load Balancer.

Zie [Azure Load Balancer-componenten](./components.md) en [Azure Load Balancer-concepten](./concepts.md) voor meer informatie over beperkingen en componenten van Azure Load Balancer

Zie [Overzicht van taakverdelingsopties in Azure](https://docs.microsoft.com/azure/architecture/guide/technology-choices/load-balancing-overview) voor een vergelijking van de opties voor taakverdeling van Azure.
