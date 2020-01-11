---
title: Azure Virtual Network | Microsoft Docs
description: Meer informatie over concepten en functies van Azure Virtual Network.
services: virtual-network
documentationcenter: na
author: anavinahar
tags: azure-resource-manager
Customer intent: As someone with a basic network background that is new to Azure, I want to understand the capabilities of Azure Virtual Network, so that my Azure resources such as VMs, can securely communicate with each other, the internet, and my on-premises resources.
ms.service: virtual-network
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2019
ms.author: anavin
ms.openlocfilehash: 768d01346b7697953d44742458c7e507ae7c115a
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2020
ms.locfileid: "75894595"
---
# <a name="what-is-azure-virtual-network"></a>Wat is Azure Virtual Network?

Azure Virtual Network (VNet) is de fundamentele bouw steen voor uw particuliere netwerk in Azure. Met VNet kunnen veel typen Azure-resources, zoals Azure Virtual Machines (VM), veilig communiceren met elkaar, Internet en on-premises netwerken. VNet is vergelijkbaar met een traditioneel netwerk dat u in uw eigen Data Center zou uitvoeren, maar biedt extra voor delen van de infra structuur van Azure, zoals schaal, Beschik baarheid en isolatie.

## <a name="vnet-concepts"></a>VNet-concepten

- **Adres ruimte:** Wanneer u een VNet maakt, moet u een aangepaste privé-IP-adres ruimte opgeven met open bare en privé-adressen (RFC 1918). Azure wijst resources in een virtueel netwerk een persoonlijk IP-adres toe op basis van de adresruimte die u toewijst. Als u bijvoorbeeld een virtuele machine in een VNet implementeert met adres ruimte, 10.0.0.0/16, wordt aan de virtuele machine een privé-IP-adres, zoals 10.0.0.4, toegewezen.
- **Subnetten:** Met subnetten kunt u het virtuele netwerk segmenteren in een of meer subnetwerken en een deel van de adres ruimte van het virtuele netwerk toewijzen aan elk subnet. Vervolgens kunt u Azure-resources implementeren in een specifiek subnet. Net als in een traditioneel netwerk kunt u met subnetten uw VNet-adres ruimte segmenteren in segmenten die geschikt zijn voor het interne netwerk van de organisatie. Dit verbetert ook de efficiëntie van de adres toewijzing. U kunt bronnen binnen subnetten beveiligen met behulp van netwerk beveiligings groepen. Zie [beveiligings groepen](security-overview.md)voor meer informatie.
- **Regio's**: VNet bevindt zich in één regio/locatie. meerdere virtuele netwerken van verschillende regio's kunnen echter samen worden verbonden met Virtual Network peering.
- **Abonnement:** VNet bevindt zich in een abonnement. U kunt meerdere virtuele netwerken binnen elk Azure-[abonnement](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) en elke Azure-[regio](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#region) implementeren.

## <a name="best-practices"></a>Aanbevolen procedures

Wanneer u uw netwerk in azure bouwt, is het belang rijk om de volgende universele ontwerp principes te onthouden:

- Zorg ervoor dat de adres ruimten niet overlappen. Zorg ervoor dat uw VNet-adres ruimte (CIDR-blok) niet overlapt met de andere netwerkbereiken van uw organisatie.
- Uw subnetten mogen niet de gehele adres ruimte van het VNet beslaan. Plan vooruit en reserveer enkele adres ruimte voor de toekomst.
- Het is raadzaam om minder grote VNets te hebben dan meerdere kleine VNets. Dit voor komt beheer overhead.
- Beveilig uw VNet met behulp van netwerk beveiligings groepen (Nsg's).

## <a name="communicate-with-the-internet"></a>Communiceren met internet

Alle resources in een VNet kunnen standaard uitgaand verkeer naar Internet communiceren. U kunt binnenkomend communiceren met een resource door er een openbaar IP-adres of een openbare Load Balancer aan toe te wijzen. U kunt ook een openbaar IP-adres of een openbare Load Balancer gebruiken voor het beheren van uw uitgaande verbindingen.  Zie [Uitgaande verbindingen](../load-balancer/load-balancer-outbound-connections.md), [Openbare IP-adressen](virtual-network-public-ip-address.md) en [Load Balancer](../load-balancer/load-balancer-overview.md) voor meer informatie over uitgaande verbindingen in Azure.

>[!NOTE]
>Wanneer u alleen een interne [Standard Load Balancer](../load-balancer/load-balancer-standard-overview.md) gebruikt, is uitgaande connectiviteit niet beschikbaar totdat u definieert hoe u wilt dat [uitgaande verbindingen](../load-balancer/load-balancer-outbound-connections.md) werken met een op exemplaarniveau openbaar IP-adres of een openbare Load Balancer.

## <a name="communicate-between-azure-resources"></a>Communicatie tussen Azure-resources

Azure-resources communiceren veilig met elkaar op een van de volgende manieren:

- **Via een virtueel netwerk**: u kunt virtuele machines en diverse andere soorten Azure-resources implementeren op een virtueel netwerk, zoals Azure App Service-omgevingen, de Azure Kubernetes Service (AKS) en Azure Virtual Machine Scale Sets. Zie [Integratie van virtuele netwerkservices](virtual-network-for-azure-services.md) voor een volledige lijst met Azure-resources die u in een virtueel netwerk kunt implementeren.
- **Via een service-eindpunt voor een virtueel netwerk**: breid de openbare adresruimte van uw virtuele netwerk en de identiteit van uw virtuele netwerk uit naar Azure-serviceresources, zoals Azure Storage-accounts en Azure SQL-databases, via een directe verbinding. Met service-eindpunten kunt u uw kritieke Azure-serviceresources alleen beveiligen naar een virtueel netwerk. Zie [Overzicht van service-eindpunten voor virtuele netwerken](virtual-network-service-endpoints-overview.md) voor meer informatie.
- **Via VNet-peering**: u kunt virtuele netwerken met elkaar verbinden, waardoor resources in beide virtuele netwerken met elkaar kunnen communiceren, met behulp van peering op virtueel netwerk. De virtuele netwerken die u met elkaar verbindt, kunnen zich in dezelfde of verschillende Azure-regio's bevinden. Zie [Peering van virtuele netwerken](virtual-network-peering-overview.md) voor meer informatie.

## <a name="communicate-with-on-premises-resources"></a>Communiceren met on-premises resources

U kunt uw on-premises computers en netwerken verbinden met een virtueel netwerk via een combinatie van de volgende opties:

- **Point-to-site virtueel particulier netwerk (VPN):** wordt tot stand gebracht tussen een virtueel netwerk en een enkele computer in uw netwerk. Elke computer die verbinding wil met een virtueel netwerk moet de verbinding hiervoor configureren. Dit verbindingstype is handig als u net aan de slag gaat met Azure of voor ontwikkelaars, omdat hiervoor weinig of geen wijzigingen in uw bestaande netwerk nodig zijn. De communicatie tussen uw computer en een virtueel netwerk wordt verzonden via een gecodeerde tunnel via internet. Zie [Point-to-site VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#P2S) voor meer informatie.
- **Site-to-site VPN:** wordt tot stand gebracht tussen uw on-premises VPN-apparaat en een Azure VPN Gateway die is geïmplementeerd in een virtueel netwerk. Met dit verbindingstype krijgen alle on-premises resource die u toestemming geeft toegang tot een virtueel netwerk. De communicatie tussen uw on-premises VPN-apparaat en een Azure VPN Gateway wordt verzonden via een gecodeerde tunnel via internet. Zie [Site-to-site VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#s2smulti) voor meer informatie.
- **Azure ExpressRoute:** wordt tot stand gebracht tussen uw netwerk en Azure, via een ExpressRoute-partner. Deze verbinding is een privéverbinding. Verkeer gaat niet via internet. Zie [ExpressRoute](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#ExpressRoute) voor meer informatie.

## <a name="filter-network-traffic"></a>Netwerkverkeer filteren

U kunt netwerkverkeer filteren tussen subnetten met behulp van een of beide van de volgende opties:

- **Beveiligings groepen:** Netwerk beveiligings groepen en toepassings beveiligings groepen kunnen meerdere binnenkomende en uitgaande beveiligings regels bevatten waarmee u verkeer van en naar resources kunt filteren op bron-en doel-IP-adres, poort en protocol. Zie [Netwerkbeveiligingsgroepen](security-overview.md#network-security-groups) of [Toepassingsbeveiligingsgroepen](security-overview.md#application-security-groups) voor meer informatie.
- **Virtuele apparaten:** een virtueel netwerkapparaat is een virtuele machine die een netwerkfunctie uitvoert, zoals een firewall, WAN-optimalisatie of een andere netwerkfunctie. Zie [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances) voor meer informatie over het bekijken van een lijst met beschikbare virtuele netwerkapparaten die u in een virtueel netwerk kunt implementeren.

## <a name="route-network-traffic"></a>Netwerkverkeer routeren

Azure routeert verkeer standaard tussen subnetten, verbonden virtuele netwerken, on-premises netwerken en internet. U kunt een of beide van de volgende opties implementeren om de standaardroutes die Azure maakt te onderdrukken:

- **Routetabellen:** u kunt aangepaste routetabellen maken met routes die bepalen waar verkeer naartoe wordt doorgestuurd voor elk subnet. Meer informatie over [routetabellen](virtual-networks-udr-overview.md#user-defined).
- **BGP-routes (Border Gateway Protocol):** als u uw virtuele netwerk verbindt met uw on-premises netwerk via een Azure VPN Gateway- of ExpressRoute-verbinding, kunt u uw lokale BGP-routes doorgegeven aan uw virtuele netwerken. Lees meer over het gebruik van BGP met [Azure VPN Gateway](../vpn-gateway/vpn-gateway-bgp-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) en [ExpressRoute](../expressroute/expressroute-routing.md?toc=%2fazure%2fvirtual-network%2ftoc.json#dynamic-route-exchange).

## <a name="azure-vnet-limits"></a>Limieten voor Azure VNet

Er zijn bepaalde limieten rond het aantal Azure-resources dat u kunt implementeren. De meeste Azure-netwerk limieten bevinden zich in de maximum waarden. U kunt [bepaalde netwerk limieten echter verhogen](../azure-portal/supportability/networking-quota-requests.md) zoals opgegeven op de [pagina VNet-limieten](../azure-resource-manager/management/azure-subscription-service-limits.md#networking-limits). 

## <a name="pricing"></a>Prijzen

Er worden geen kosten in rekening gebracht voor het gebruik van Azure VNet. het is gratis. Standaard kosten zijn van toepassing op resources, zoals Virtual Machines (Vm's) en andere producten. Zie voor meer informatie [VNet-prijzen](https://azure.microsoft.com/pricing/details/virtual-network/) en de Azure- [prijs calculator](https://azure.microsoft.com/pricing/calculator/).

## <a name="next-steps"></a>Volgende stappen

 Als u aan de slag wilt gaan met een virtueel netwerk, maakt u een virtueel netwerk, implementeert u een paar VM's en laat u de virtuele machines met elkaar communiceren. Zie de snelstart [Een virtueel netwerk maken](quick-create-portal.md) voor meer informatie.
