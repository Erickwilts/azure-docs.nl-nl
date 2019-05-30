---
title: Azure Virtual Network | Microsoft Docs
description: Meer informatie over concepten en functies van Azure Virtual Network.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
tags: azure-resource-manager
Customer intent: As someone with a basic network background that is new to Azure, I want to understand the capabilities of Azure Virtual Network, so that my Azure resources such as VMs, can securely communicate with each other, the internet, and my on-premises resources.
ms.service: virtual-network
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/12/2018
ms.author: kumud
ms.openlocfilehash: 23093cd8bcb5793b9e5b9abc835f64233e666ce1
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66241747"
---
# <a name="what-is-azure-virtual-network"></a>Wat is Azure Virtual Network?

Via Azure Virtual Network kunnen veel soorten Azure-resources, zoals virtuele Azure-machines, veilig communiceren met elkaar, internet en on-premises netwerken. Een virtueel netwerk is afgestemd op één Azure-regio. Een Azure [regio](https://azure.microsoft.com/global-infrastructure/regions/) is een set datacenters geïmplementeerd in een latentie gedefinieerde perimeter en verbonden via een toegewezen regionaal netwerk voor lage latentie. 

Virtuele netwerken zijn opgebouwd uit subnetten. Een subnet is een bereik van IP-adressen binnen het virtuele netwerk. Subnetten, zoals virtuele netwerken zijn toegewezen aan één Azure-regio. 

Meerdere virtuele netwerken in verschillende regio's kunnen worden verbonden samen met behulp van Peering in virtuele netwerken.

Azure Virtual Network bevat de volgende kernmogelijkheden:

## <a name="isolation-and-segmentation"></a>Isolatie en segmentatie

U kunt meerdere virtuele netwerken binnen elk Azure-[abonnement](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) en elke Azure-[regio](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#region) implementeren. Elk virtuele netwerk is geïsoleerd van andere virtuele netwerken. Voor elke virtueel netwerk kunt u het volgende:
- Een aangepaste persoonlijke IP-adresruimte opgeven met behulp van openbare en persoonlijke adressen (RFC 1918). Azure wijst resources in een virtueel netwerk een persoonlijk IP-adres toe op basis van de adresruimte die u toewijst.
- Het virtuele netwerk segmenteren in een of meer subnetten en een deel van de adresruimte van het virtuele netwerk aan elk subnet toewijzen.
- Door Azure geleverd naamomzetting gebruiken of uw eigen DNS-server opgeven, voor gebruik door resources in een virtueel netwerk.

## <a name="communicate-with-the-internet"></a>Communiceren met internet

Alle resources in een virtueel netwerk kunnen standaard uitgaand communiceren met internet. U kunt binnenkomend communiceren met een resource door er een openbaar IP-adres of een openbare Load Balancer aan toe te wijzen. U kunt ook een openbaar IP-adres of een openbare Load Balancer gebruiken voor het beheren van uw uitgaande verbindingen.  Zie [Uitgaande verbindingen](../load-balancer/load-balancer-outbound-connections.md), [Openbare IP-adressen](virtual-network-public-ip-address.md) en [Load Balancer](../load-balancer/load-balancer-overview.md) voor meer informatie over uitgaande verbindingen in Azure.

>[!NOTE]
>Wanneer u alleen een interne [Standard Load Balancer](../load-balancer/load-balancer-standard-overview.md) gebruikt, is uitgaande connectiviteit niet beschikbaar totdat u definieert hoe u wilt dat [uitgaande verbindingen](../load-balancer/load-balancer-outbound-connections.md) werken met een op exemplaarniveau openbaar IP-adres of een openbare Load Balancer.

## <a name="communicate-between-azure-resources"></a>Communicatie tussen Azure-resources

Azure-resources communiceren veilig met elkaar op een van de volgende manieren:

- **Via een virtueel netwerk**: U kunt virtuele machines en diverse andere soorten Azure-resources implementeren op een virtueel netwerk, zoals Azure App Service-omgevingen, de Azure Kubernetes Service (AKS) en Azure Virtual Machine Scale Sets. Zie [Integratie van virtuele netwerkservices](virtual-network-for-azure-services.md) voor een volledige lijst met Azure-resources die u in een virtueel netwerk kunt implementeren. 
- **Via een service-eindpunt voor een virtueel netwerk**: Breid de openbare adresruimte van uw virtuele netwerk en de identiteit van uw virtuele netwerk uit naar Azure-serviceresources, zoals Azure Storage-accounts en Azure SQL-databases, via een directe verbinding. Met service-eindpunten kunt u uw kritieke Azure-serviceresources alleen beveiligen naar een virtueel netwerk. Zie [Overzicht van service-eindpunten voor virtuele netwerken](virtual-network-service-endpoints-overview.md) voor meer informatie.
 
## <a name="communicate-with-on-premises-resources"></a>Communiceren met on-premises resources

U kunt uw on-premises computers en netwerken verbinden met een virtueel netwerk via een combinatie van de volgende opties:

- **Punt-naar-site virtueel particulier netwerk (VPN):** Wordt tot stand gebracht tussen een virtueel netwerk en een enkele computer in uw netwerk. Elke computer die verbinding wil met een virtueel netwerk moet de verbinding hiervoor configureren. Dit verbindingstype is handig als u net aan de slag gaat met Azure of voor ontwikkelaars, omdat hiervoor weinig of geen wijzigingen in uw bestaande netwerk nodig zijn. De communicatie tussen uw computer en een virtueel netwerk wordt verzonden via een gecodeerde tunnel via internet. Zie [Point-to-site VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#P2S) voor meer informatie.
- **Site-to-site VPN:** Wordt tot stand gebracht tussen uw on-premises VPN-apparaat en een Azure VPN Gateway die is geïmplementeerd in een virtueel netwerk. Met dit verbindingstype krijgen alle on-premises resource die u toestemming geeft toegang tot een virtueel netwerk. De communicatie tussen uw on-premises VPN-apparaat en een Azure VPN Gateway wordt verzonden via een gecodeerde tunnel via internet. Zie [Site-to-site VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#s2smulti) voor meer informatie.
- **Azure ExpressRoute:** Wordt tot stand gebracht tussen uw netwerk en Azure, via een ExpressRoute-partner. Deze verbinding is een privéverbinding. Verkeer gaat niet via internet. Zie [ExpressRoute](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#ExpressRoute) voor meer informatie.

## <a name="filter-network-traffic"></a>Netwerkverkeer filteren
U kunt netwerkverkeer filteren tussen subnetten met behulp van een of beide van de volgende opties:
- **Beveiligingsgroepen:** Netwerkbeveiligingsgroepen en toepassingsbeveiligingsgroepen kunnen meerdere binnenkomende en uitgaande beveiligingsregels bevatten waarmee u verkeer van en naar resources kunt filteren op bron- en doel-IP-adres, poort en protocol. Zie [Netwerkbeveiligingsgroepen](security-overview.md#network-security-groups) of [Toepassingsbeveiligingsgroepen](security-overview.md#application-security-groups) voor meer informatie.
- **Virtuele netwerkapparaten:** Een virtueel netwerkapparaat is een virtuele machine die een netwerkfunctie uitvoert, zoals een firewall, WAN-optimalisatie of een andere netwerkfunctie. Zie [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances) voor meer informatie over het bekijken van een lijst met beschikbare virtuele netwerkapparaten die u in een virtueel netwerk kunt implementeren.

## <a name="route-network-traffic"></a>Netwerkverkeer routeren

Azure routeert verkeer standaard tussen subnetten, verbonden virtuele netwerken, on-premises netwerken en internet. U kunt een of beide van de volgende opties implementeren om de standaardroutes die Azure maakt te onderdrukken:
- **Routetabellen:** U kunt aangepaste routetabellen maken met routes die bepalen waar verkeer naartoe wordt doorgestuurd voor elk subnet. Meer informatie over [routetabellen](virtual-networks-udr-overview.md#user-defined).
- **BGP-routes (Border Gateway Protocol):** Als u uw virtuele netwerk verbindt met uw on-premises netwerk via een Azure VPN Gateway- of ExpressRoute-verbinding, kunt u uw lokale BGP-routes doorgegeven aan uw virtuele netwerken. Lees meer over het gebruik van BGP met [Azure VPN Gateway](../vpn-gateway/vpn-gateway-bgp-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) en [ExpressRoute](../expressroute/expressroute-routing.md?toc=%2fazure%2fvirtual-network%2ftoc.json#dynamic-route-exchange).

## <a name="connect-virtual-networks"></a>Virtuele netwerken verbinden

U kunt virtuele netwerken met elkaar verbinden, zodat resources in beide virtuele netwerken met elkaar kunnen communiceren, met behulp van peering voor virtuele netwerken. De virtuele netwerken die u met elkaar verbindt, kunnen zich in dezelfde of verschillende Azure-regio's bevinden. Zie [Peering van virtuele netwerken](virtual-network-peering-overview.md) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

U hebt nu een overzicht van Azure Virtual Network. Als u aan de slag wilt gaan met een virtueel netwerk, maakt u een virtueel netwerk, implementeert u een paar VM's en laat u de virtuele machines met elkaar communiceren. Zie de snelstart [Een virtueel netwerk maken](quick-create-portal.md) voor meer informatie.
