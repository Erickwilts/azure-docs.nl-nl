---
title: Netwerk beveiliging voor Azure Relay
description: In dit artikel wordt beschreven hoe u toegang kunt configureren vanaf privé-eind punten
ms.topic: conceptual
ms.date: 06/23/2020
ms.openlocfilehash: a1ade21df39890b7f1c31a81fca1fffafe2acaa0
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2020
ms.locfileid: "85322165"
---
# <a name="network-security-for-azure-relay"></a>Netwerk beveiliging voor Azure Relay 
In dit artikel wordt beschreven hoe u de volgende beveiligings functies gebruikt met Azure Relay: 

- IP-firewall regels (preview-versie)
- Persoonlijke eind punten (preview-versie)

> [!NOTE]
> Azure Relay biedt geen ondersteuning voor netwerk service-eind punten. 


## <a name="ip-firewall"></a>IP-firewall 
Relay-naam ruimten zijn standaard toegankelijk vanuit Internet zolang de aanvraag een geldige verificatie en autorisatie heeft. Met IP-firewall kunt u dit nog verder beperken tot een aantal IPv4-adressen of IPv4-adresbereiken in CIDR-notatie [(Classless Inter-Domain Routing)](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) .

Deze functie is handig in scenario's waarin Azure Relay alleen toegankelijk moet zijn vanaf bepaalde bekende sites. Met firewall regels kunt u regels configureren voor het accepteren van verkeer dat afkomstig is van specifieke IPv4-adressen. Als u bijvoorbeeld relay met [Azure Express route](/azure/expressroute/expressroute-faqs#supported-services)gebruikt, kunt u een **firewall regel** maken om alleen verkeer toe te staan van uw on-premises IP-adressen van de infra structuur. 

De IP-firewall regels worden toegepast op het niveau van de relay-naam ruimte. Daarom gelden de regels voor alle verbindingen van clients die gebruikmaken van elk ondersteund protocol. Een verbindings poging van een IP-adres dat niet overeenkomt met een toegestane IP-regel op de relay-naam ruimte, wordt geweigerd als niet-geautoriseerd. De IP-regel wordt niet vermeld in het antwoord. IP-filter regels worden in volg orde toegepast en de eerste regel die overeenkomt met het IP-adres, bepaalt de accepteren of afwijzen.

Zie [IP-Firewall configureren voor een relay-naam ruimte](ip-firewall-virtual-networks.md) voor meer informatie

## <a name="private-endpoints"></a>Privé-eindpunten

Met Azure **Private Link service** kunt u toegang krijgen tot Azure-Services (bijvoorbeeld Azure Relay, Azure service bus, Azure Event Hubs, Azure Storage en Azure Cosmos DB) en Azure hostende klanten/partner services via een persoonlijk eind punt in uw virtuele netwerk. Zie [Wat is Azure private link (preview)?](../private-link/private-link-overview.md) voor meer informatie.

Een **persoonlijk eind punt** is een netwerk interface waarmee de werk belastingen die worden uitgevoerd in een virtueel netwerk, privé en veilig verbinding kunnen maken met een service die een **persoonlijke koppelings bron** heeft (bijvoorbeeld een relay-naam ruimte). Het privé-eindpunt maakt gebruik van een privé-IP-adres van uw VNet, waardoor de service feitelijk in uw VNet wordt geplaatst. Al het verkeer naar de service kan worden gerouteerd via het persoonlijke eind punt, zodat er geen gateways, NAT-apparaten, ExpressRoute, VPN-verbindingen of open bare IP-adressen nodig zijn. Verkeer tussen uw virtuele netwerk en de service wordt over het micro soft backbone-netwerk gepasseerd, waardoor de bloot stelling van het open bare Internet wordt voor komen. U kunt een niveau van granulatie in toegangs beheer geven door verbindingen met specifieke Azure Relay-naam ruimten toe te staan.

> [!NOTE]
> Deze functie is momenteel beschikbaar als **Preview-versie**. 

Zie [persoonlijke eind punten configureren](private-link-service.md) voor meer informatie


## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen:

- [IP-Firewall configureren](ip-firewall-virtual-networks.md)
- [Privé-eind punten configureren](private-link-service.md)