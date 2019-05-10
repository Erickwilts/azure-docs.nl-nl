---
title: Veelgestelde vragen over Azure-netwerk
titlesuffix: Azure Virtual Network
description: Antwoorden op veelgestelde vragen over virtuele netwerken van Microsoft Azure.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/12/2019
ms.author: kumud
ms.openlocfilehash: b072314bdbec1d5a6184e6f20e98c35a9135a5b7
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/09/2019
ms.locfileid: "65508409"
---
# <a name="azure-virtual-network-frequently-asked-questions-faq"></a>Veelgestelde vragen (FAQ) over virtuele Azure-netwerk

## <a name="virtual-network-basics"></a>Grondbeginselen van het virtuele netwerk

### <a name="what-is-an-azure-virtual-network-vnet"></a>Wat is een Azure Virtual Network (VNet)?
Een Azure Virtual Network (VNet) is een weergave van uw eigen netwerk in de cloud. Het is een logische isolatie van de Azure-cloud die is toegewezen aan uw abonnement. U kunt vnet's gebruiken om in te richten en beheren van virtuele particuliere netwerken (VPN's) in Azure en, koppelen (optioneel) de VNets met andere VNets in Azure, of met uw on-premises IT-infrastructuur om cross-premises of hybride oplossingen te maken. Elk VNet die u maakt een eigen CIDR-blok is en kan worden gekoppeld aan andere vnet's en on-premises netwerken, zolang de CIDR-blokken niet overlappen. U hebt ook beheer van DNS-serverinstellingen voor VNets en segmentering van het VNet in subnetten.

Vnet's te gebruiken:

* Maak een speciale persoonlijke alleen in de cloud VNet soms u vereisen een cross-premises-configuratie voor uw oplossing. Wanneer u een VNet maakt, uw services en virtuele machines in uw VNet kunnen rechtstreeks en veilig communiceren met elkaar in de cloud. U kunt nog steeds endpoint-verbindingen voor de virtuele machines en services die communicatie met Internet, als onderdeel van uw oplossing nodig configureren.
* Veilig uitbreiden van uw datacenter met VNets, kunt u traditionele site-naar-site (S2S) VPN-verbindingen als u wilt schalen veilig capaciteit van uw datacenter maken. IPSEC S2S VPN's gebruiken om een beveiligde verbinding tussen uw zakelijke VPN-gateway en Azure.
* Inschakelen voor hybride cloudmodellen vnet's bieden u de flexibiliteit voor de ondersteuning van tal van hybride cloud-scenario's. U kunt veilig verbinding maken in cloud-gebaseerde toepassingen naar een on-premises systeem, zoals mainframes en Unix-systemen.

### <a name="how-do-i-get-started"></a>Hoe ga ik aan de slag?
Ga naar de [Virtual network: documentatie](https://docs.microsoft.com/azure/virtual-network/) aan de slag. Deze inhoud biedt een overzicht en implementatie-informatie voor alle van de VNet-functies.

### <a name="can-i-use-vnets-without-cross-premises-connectivity"></a>Kan ik vnet's zonder cross-premises connectiviteit gebruiken?
Ja. U kunt een VNet gebruiken zonder deze te koppelen aan uw locatie. U kunt bijvoorbeeld Microsoft Windows Server Active Directory-domeincontrollers en SharePoint-farms uitvoeren uitsluitend in een Azure VNet.

### <a name="can-i-perform-wan-optimization-between-vnets-or-a-vnet-and-my-on-premises-data-center"></a>Kan ik de WAN-optimalisatie tussen vnet's of een VNet en mijn on-premises Datacenter uitvoeren?
Ja. U kunt implementeren een [virtueel netwerkapparaat WAN-optimalisatie](https://azure.microsoft.com/marketplace/?term=wan+optimization) van verschillende leveranciers via Azure Marketplace.

## <a name="configuration"></a>Configuratie

### <a name="what-tools-do-i-use-to-create-a-vnet"></a>Welke hulpprogramma's gebruik ik een VNet maken?
U kunt de volgende hulpprogramma's gebruiken om te maken of een VNet configureren:

* Azure Portal
* PowerShell
* Azure-CLI
* Een netwerkconfiguratiebestand (netcfg - voor alleen klassieke VNets). Zie de [een VNet configureren met een netwerkconfiguratiebestand](virtual-networks-using-network-configuration-file.md) artikel.

### <a name="what-address-ranges-can-i-use-in-my-vnets"></a>Welke adresbereiken kan ik gebruiken in mijn VNets?
Elk IP-adresbereik is gedefinieerd in [RFC 1918](https://tools.ietf.org/html/rfc1918). Bijvoorbeeld: 10.0.0.0/16. U kunt de volgende adresbereiken niet toevoegen:
* 224.0.0.0/4 (Multicast)
* 255.255.255.255/32 (Broadcast)
* 127.0.0.0/8 (Loopback)
* 169.254.0.0/16 (Link-local)
* 168.63.129.16/32 (interne DNS)

### <a name="can-i-have-public-ip-addresses-in-my-vnets"></a>Kan ik openbare IP-adressen hebben in mijn VNets?
Ja. Zie voor meer informatie over het openbare IP-adresbereiken [maken van een virtueel netwerk](manage-virtual-network.md#create-a-virtual-network). Openbare IP-adressen zijn niet rechtstreeks toegankelijk via internet.

### <a name="is-there-a-limit-to-the-number-of-subnets-in-my-vnet"></a>Is er een limiet aan het aantal subnetten in mijn VNet?
Ja. Zie [Azure-limieten](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#networking-limits) voor meer informatie. Subnet-adresruimten elkaar niet overlappen elkaar.

### <a name="are-there-any-restrictions-on-using-ip-addresses-within-these-subnets"></a>Zijn er beperkingen voor het gebruik van IP-adressen binnen deze subnetten?
Ja. In Azure worden vijf IP-adressen gereserveerd in elk subnet. Dit zijn x.x.x.0 x.x.x.3 en het laatste adres van het subnet.    
- x.x.x.0 en het laatste adres van het subnet is gereserveerd voor conformiteit van protocol.
- x.x.x.1 x.x.x.3 is gereserveerd in elk subnet voor Azure-services.

### <a name="how-small-and-how-large-can-vnets-and-subnets-be"></a>Hoe klein en hoe groot kunnen zijn VNets en subnetten?
Het kleinste ondersteunde subnet /29 en de grootste/8 die is (met behulp van definities van de CIDR-subnetmasker).

### <a name="can-i-bring-my-vlans-to-azure-using-vnets"></a>Kan ik overbrengen mijn VLAN's naar Azure met behulp van vnet's?
Nee. Vnet's zich laag-3-overlays. Azure biedt geen ondersteuning voor alle semantiek Layer-2.

### <a name="can-i-specify-custom-routing-policies-on-my-vnets-and-subnets"></a>Kan ik aangepaste routeringsbeleid op mijn VNets en subnetten opgeven?
Ja. U kunt een routetabel maken en deze te koppelen aan een subnet. Zie voor meer informatie over routering in Azure, [routeringoverzicht](virtual-networks-udr-overview.md#custom-routes).

### <a name="do-vnets-support-multicast-or-broadcast"></a>Ondersteunen VNets multicast- of broadcastberichten?
Nee. Multicast en uitzenden worden niet ondersteund.

### <a name="what-protocols-can-i-use-within-vnets"></a>Welke protocollen kan ik gebruiken binnen VNets?
U kunt TCP, UDP en ICMP-TCP/IP-protocollen in vnet's gebruiken. Unicast binnen VNets, met uitzondering van Dynamic Host Configuration Protocol (DHCP) via Unicast wordt ondersteund (poort UDP/68 bron / bestemming poort UDP/67). Multicast, broadcast, IP-in-IP-encapsulated pakketten en Generic Routing Encapsulation (GRE) pakketten worden binnen VNets geblokkeerd. 

### <a name="can-i-ping-my-default-routers-within-a-vnet"></a>Kan ik mijn routers standaard binnen een VNet pingen?
Nee.

### <a name="can-i-use-tracert-to-diagnose-connectivity"></a>Kan ik tracert gebruiken om op te sporen connectiviteit?
Nee.

### <a name="can-i-add-subnets-after-the-vnet-is-created"></a>Kan ik subnetten toevoegen nadat het VNet is gemaakt?
Ja. Subnetten kunnen worden toegevoegd aan de VNets op elk gewenst moment, zolang het adresbereik van subnet geen deel uit van een ander subnet maakt en er beschikbare ruimte in het adresbereik van het virtuele netwerk is.

### <a name="can-i-modify-the-size-of-my-subnet-after-i-create-it"></a>Kan ik de grootte van mijn subnet wijzigen nadat ik deze heb gemaakt?
Ja. U kunt toevoegen, verwijderen, uitbreiden of verkleinen van een subnet als er zijn geen virtuele machines of services die zijn geïmplementeerd in het.

### <a name="can-i-modify-subnets-after-i-created-them"></a>Kan ik subnetten wijzigen nadat ik ze zijn gemaakt?
Ja. U kunt toevoegen, verwijderen en aanpassen van de CIDR-blokken die worden gebruikt door een VNet.

### <a name="if-i-am-running-my-services-in-a-vnet-can-i-connect-to-the-internet"></a>Als ik ben mijn services in een VNet, kan ik verbinding maken met internet?
Ja. Alle services die zijn geïmplementeerd in een VNet kunnen uitgaand verbinding maken met internet. Zie voor meer informatie over uitgaande verbindingen via internet in Azure, [uitgaande verbindingen](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Als u wilt om verbinding te maken op een resource die is geïmplementeerd via Resource Manager, moet de resource een openbare IP-adres is toegewezen. Zie voor meer informatie over het openbare IP-adressen, [openbare IP-adressen](virtual-network-public-ip-address.md). Elke Azure-Cloudservice die is geïmplementeerd in Azure heeft een vrij adresseerbare VIP-adres toegewezen. U definieert invoereindpunten voor PaaS-rollen en -eindpunten voor virtuele machines voor het inschakelen van deze services om verbindingen via internet te accepteren.

### <a name="do-vnets-support-ipv6"></a>Vnet's bieden ondersteuning voor IPv6?
Nee. U kunt IPv6 met vnet's niet gebruiken op dit moment. U kunt wel, wijzen IPv6-adressen voor Azure-load balancers laden taken verdelen tussen virtuele machines. Zie voor meer informatie, [overzicht van IPv6 voor Azure Load Balancer](../load-balancer/load-balancer-ipv6-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

### <a name="can-a-vnet-span-regions"></a>Kan een VNet regio's omvatten?
Nee. Een VNet is beperkt tot één regio. Een virtueel netwerk wordt echter beschikbaarheidszones omvatten. Zie [Overzicht van beschikbaarheidszones](../availability-zones/az-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) voor meer informatie over beschikbaarheidszones. U kunt virtuele netwerken in verschillende regio's verbinden met virtueel-netwerkpeering. Zie voor meer informatie, [overzicht van Virtual network-peering](virtual-network-peering-overview.md)

### <a name="can-i-connect-a-vnet-to-another-vnet-in-azure"></a>Kan ik een VNet verbinden met een ander VNet in Azure?
Ja. U kunt een VNet verbinden met een ander VNet met behulp van:
- **Peering op virtueel netwerk**: Zie voor meer informatie, [overzicht van VNet-peering](virtual-network-peering-overview.md)
- **Een Azure VPN-Gateway**: Zie voor meer informatie, [een VNet-naar-VNet-verbinding configureren](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json). 

## <a name="name-resolution-dns"></a>Naamomzetting (DNS)

### <a name="what-are-my-dns-options-for-vnets"></a>Wat zijn mijn DNS-opties voor VNets?
Gebruik van de tabel besluit op de [naamomzetting voor VM's en Rolexemplaren](virtual-networks-name-resolution-for-vms-and-role-instances.md) pagina om u te begeleiden u bij de DNS-Server opties beschikbaar.

### <a name="can-i-specify-dns-servers-for-a-vnet"></a>Kan ik DNS-servers opgeven voor een VNet?
Ja. U kunt DNS-server IP-adressen opgeven in de VNet-instellingen. De instelling wordt toegepast als de standaard DNS-server (s) voor alle virtuele machines in het VNet.

### <a name="how-many-dns-servers-can-i-specify"></a>Het aantal DNS-servers kan ik opgeven?
Naslaginformatie over [Azure-limieten](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#networking-limits).

### <a name="can-i-modify-my-dns-servers-after-i-have-created-the-network"></a>Kan ik mijn DNS-servers wijzigen nadat ik het netwerk hebt gemaakt?
Ja. U kunt de lijst met DNS-servers voor uw VNet op elk gewenst moment wijzigen. Als u uw DNS-serverlijst wijzigt, moet u elk van de virtuele machines in uw VNet zodat ze om op te halen de nieuwe DNS-server opnieuw te starten.

### <a name="what-is-azure-provided-dns-and-does-it-work-with-vnets"></a>Wat is Azure DNS- en werkt het met VNets?
Azure verschafte DNS is een DNS-service met meerdere tenants die worden aangeboden door Microsoft. Azure wordt al uw virtuele machines en rolinstanties voor cloud-service geregistreerd bij deze service. Deze service biedt naamomzetting door hostnaam voor virtuele machines en rolinstanties die zich in dezelfde cloudservice bevinden en FQDN-naam voor de virtuele machines en rollen in hetzelfde VNet. Zie voor meer informatie over DNS, [naamomzetting voor virtuele machines en Cloud Services-rolexemplaren](virtual-networks-name-resolution-for-vms-and-role-instances.md).

Er geldt een beperking voor de eerste 100 cloudservices in een VNet voor de naamomzetting tussen tenants met behulp van Azure verschafte DNS. Als u uw eigen DNS-server gebruikt, is deze beperking niet van toepassing.

### <a name="can-i-override-my-dns-settings-on-a-per-vm-or-cloud-service-basis"></a>Kan ik mijn DNS-instellingen op basis van per-VM of cloud service overschrijven?
Ja. U kunt DNS-servers per virtuele machine of cloud service te overschrijven de standaardinstellingen van het netwerk instellen. Het wordt echter aanbevolen dat u het hele netwerk DNS zo veel mogelijk.

### <a name="can-i-bring-my-own-dns-suffix"></a>Kan ik mijn eigen DNS-achtervoegsel overbrengen?
Nee. U kunt een aangepaste DNS-achtervoegsel opgeven voor uw vnet's.

## <a name="connecting-virtual-machines"></a>Verbinden van virtuele machines

### <a name="can-i-deploy-vms-to-a-vnet"></a>Kan ik virtuele machines implementeren met een VNet?
Ja. Alle netwerkinterfaces (NIC) die zijn gekoppeld aan een virtuele machine geïmplementeerd via het Resource Manager-implementatiemodel moeten zijn verbonden met een VNet. Virtuele machines die worden geïmplementeerd via het klassieke implementatiemodel kunnen optioneel worden verbonden met een VNet.

### <a name="what-are-the-different-types-of-ip-addresses-i-can-assign-to-vms"></a>Wat zijn de verschillende typen IP-adressen die ik aan virtuele machines toewijzen kan?
* **Persoonlijke:** Toegewezen aan elke NIC in elke virtuele machine. Het adres wordt toegewezen met behulp van de methode statisch of dynamisch. Privé-IP-adressen worden toegewezen uit het bereik dat u hebt opgegeven in de subnet-instellingen van uw VNet. Resources die zijn geïmplementeerd via het klassieke implementatiemodel kunnen IP-adressen zijn toegewezen, zelfs als ze niet met een VNet verbonden bent. Het gedrag van de toewijzingsmethode is verschillend, afhankelijk van of een resource is geïmplementeerd met resourcemanager of klassieke implementatiemodel: 

  - **Resource Manager**: Een privé IP-adres toegewezen met de dynamische of statische methode blijft toegewezen aan een virtuele machine (Resource Manager), totdat de resource wordt verwijderd. Het verschil is dat u het adres om toe te wijzen bij het gebruik van statische selecteren en Azure bij het gebruik van dynamische kiest. 
  - **Klassieke**: Een privé IP-adres toegewezen met de dynamische methode kan worden gewijzigd wanneer een virtuele machine (klassiek) virtuele machine opnieuw wordt opgestart nadat zij gestopt (toewijzing opgeheven). Als u nodig hebt om ervoor te zorgen dat de privé IP-adres voor een resource die is geïmplementeerd via het klassieke implementatiemodel nooit gewijzigd, moet u een privé IP-adres toewijzen met de statische methode.

* **Openbaar:** (Optioneel) toegewezen aan de NIC's die zijn gekoppeld aan virtuele machines die worden geïmplementeerd via het Azure Resource Manager-implementatiemodel. Het adres kan worden toegewezen met de statische of dynamische toewijzingsmethode. Alle virtuele machines en Cloud Services-rolexemplaren die worden geïmplementeerd via het klassieke implementatiemodel bestaan binnen een cloudservice die is toegewezen een *dynamische*, openbare virtuele IP-adres (VIP)-adres. Een openbare *statische* IP-adres, met de naam een [gereserveerde IP-adres](virtual-networks-reserved-public-ip.md), kunnen desgewenst worden toegewezen als een VIP-adres. U kunt openbare IP-adressen toewijzen aan afzonderlijke virtuele machines of Cloud Services-rolinstanties zijn geïmplementeerd via het klassieke implementatiemodel. Deze adressen heten [niveau openbare IP-exemplaar (ILPIP](virtual-networks-instance-level-public-ip.md) -adressen en kunnen dynamisch worden toegewezen.

### <a name="can-i-reserve-a-private-ip-address-for-a-vm-that-i-will-create-at-a-later-time"></a>Kan ik een privé IP-adres reserveren voor een virtuele machine die ik op een later tijdstip wilt maken?
Nee. U kunt een privé IP-adres kan niet reserveren. Als een privé IP-adres beschikbaar is, wordt deze toegewezen aan een rol of VM-exemplaar van de DHCP-server. De virtuele machine kan of mogelijk niet de optie die u wilt dat het privé IP-adres toegewezen aan. U kunt echter de privé IP-adres van een gemaakte VM wijzigen op alle beschikbare privé IP-adres.

### <a name="do-private-ip-addresses-change-for-vms-in-a-vnet"></a>Privé IP-adressen wijzigen voor virtuele machines in een VNet doen?
Dat hangt ervan af. Als de virtuele machine is geïmplementeerd via Resource Manager, Nee, ongeacht of het IP-adres is toegewezen met de statische of dynamische toewijzingsmethode. Als de virtuele machine is geïmplementeerd via het klassieke implementatiemodel, worden dynamische IP-adressen kunnen wijzigen wanneer een virtuele machine wordt gestart nadat deze zijn gestopt (toewijzing opgeheven). Het adres wordt vrijgegeven van een virtuele machine geïmplementeerd via een van beide implementatiemodel wanneer de virtuele machine wordt verwijderd.

### <a name="can-i-manually-assign-ip-addresses-to-nics-within-the-vm-operating-system"></a>Kan ik handmatig IP-adressen toewijzen aan NIC's binnen het besturingssysteem van de virtuele machine?
Ja, maar dit wordt niet aanbevolen, tenzij die nodig zijn, zoals wanneer meerdere IP-toewijzen aan een virtuele machine adressen. Zie voor meer informatie, [toevoegen van meerdere IP-adressen aan een virtuele machine](virtual-network-multiple-ip-addresses-portal.md#os-config). Het IP-adres toegewezen aan een Azure-NIC die is gekoppeld aan een virtuele machine wijzigingen en het IP-adres binnen het besturingssysteem van de virtuele machine anders is, verliest u connectiviteit met de virtuele machine.

### <a name="if-i-stop-a-cloud-service-deployment-slot-or-shutdown-a-vm-from-within-the-operating-system-what-happens-to-my-ip-addresses"></a>Als ik de afgesloten een virtuele machine via het besturingssysteem of een implementatiesite voor Cloud-Service stopt, wat gebeurt er met Mijn IP-adressen?
Er is niets. De IP-adressen (openbare VIP en openbare en persoonlijke) blijft toegewezen aan de implementatiesite voor cloud-service of VM.

### <a name="can-i-move-vms-from-one-subnet-to-another-subnet-in-a-vnet-without-redeploying"></a>Kan ik overstappen VM's van het ene subnet naar een ander subnet in een VNet zonder opnieuw te implementeren?
Ja. U vindt meer informatie in de [verplaatsen van een rol of VM-exemplaar naar een ander subnet](virtual-networks-move-vm-role-to-subnet.md) artikel.

### <a name="can-i-configure-a-static-mac-address-for-my-vm"></a>Kan ik een statisch MAC-adres configureren voor mijn VM?
Nee. Een MAC-adres kan niet statisch zijn geconfigureerd.

### <a name="will-the-mac-address-remain-the-same-for-my-vm-once-its-created"></a>Het MAC-adres blijven hetzelfde voor mijn virtuele machine nadat deze gemaakt?
Ja, het MAC-adres blijft ongewijzigd, voor een virtuele machine geïmplementeerd via het resourcemanager en klassieke implementatiemodellen totdat deze wordt verwijderd. Het MAC-adres is eerder vrijgegeven als de virtuele machine is gestopt (toewijzing opgeheven), maar nu het MAC-adres worden bewaard, zelfs wanneer de virtuele machine in de status van de toewijzing ongedaan gemaakt. Het MAC-adres blijft toegewezen aan de netwerkinterface totdat de netwerkinterface wordt verwijderd of de privé IP-adres toegewezen aan de primaire IP-adresconfiguratie van de primaire netwerkinterface is gewijzigd. 

### <a name="can-i-connect-to-the-internet-from-a-vm-in-a-vnet"></a>Kan ik verbinding maken met het internet van een virtuele machine in een VNet?
Ja. Alle virtuele machines en Cloud Services-rolexemplaren die binnen een VNet zijn geïmplementeerd, kunnen verbinding maken met Internet.

## <a name="azure-services-that-connect-to-vnets"></a>Azure-services die verbinding met VNets maken

### <a name="can-i-use-azure-app-service-web-apps-with-a-vnet"></a>Kan ik Azure App Service Web Apps met een VNet gebruiken?
Ja. U kunt Web-Apps binnen een VNet met behulp van een as-omgeving (App Service Environment) implementeren. Als u een punt-naar-site-verbinding geconfigureerd voor uw VNet hebt, kunnen veilig alle Web-Apps verbinding maken en toegang tot bronnen in het VNet. Raadpleeg voor meer informatie de volgende artikelen:

* [Web-Apps maken in een App Service-omgeving](../app-service/environment/app-service-web-how-to-create-a-web-app-in-an-ase.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
* [Uw app integreren met een Azure-netwerk](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
* [Met behulp van VNet-integratie en hybride verbindingen met Web-Apps](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json#hybrid-connections-and-app-service-environments)

### <a name="can-i-deploy-cloud-services-with-web-and-worker-roles-paas-in-a-vnet"></a>Kan ik Cloud Services implementeren met web- en werkrollen (PaaS) in een VNet?
Ja. (Optioneel) kunt u Cloud Services-rolexemplaren in vnet's implementeren. Om dit te doen, geeft u de VNet-naam en de rol/subnet-toewijzingen in de sectie netwerk configuratie van de configuratie van uw service. U hoeft niet een van de binaire bestanden bijwerken.

### <a name="can-i-connect-a-virtual-machine-scale-set-vmss-to-a-vnet"></a>Kan ik een virtuele Machine schaal ingesteld (VMSS) verbinden met een VNet?
Ja. U moet een VMSS verbinding maken met een VNet.

### <a name="is-there-a-complete-list-of-azure-services-that-can-i-deploy-resources-from-into-a-vnet"></a>Er is dat een volledig overzicht van Azure-services dat ik resources uit in een VNet implementeren kan?
Ja, voor meer informatie, Zie [integratie van virtuele netwerken voor Azure-services](virtual-network-for-azure-services.md).

### <a name="which-azure-paas-resources-can-i-restrict-access-to-from-a-vnet"></a>Welke Azure PaaS-resources kan ik toegang tot van een VNet beperken?

Resources die zijn geïmplementeerd via sommige Azure PaaS-services (zoals Azure Storage en Azure SQL Database), kunt u toegang tot het netwerk beperken tot alleen resources in een VNet door het gebruik van service-eindpunten. Zie voor meer informatie, [overzicht van Virtual network service endpoints](virtual-network-service-endpoints-overview.md).

### <a name="can-i-move-my-services-in-and-out-of-vnets"></a>Kan ik mijn services in en uit de VNets verplaatsen?
Nee. U kunt services in en uit de VNets niet verplaatsen. U hebt om een bron naar een ander VNet verplaatsen, verwijderen en opnieuw implementeren van de resource.

## <a name="security"></a>Beveiliging

### <a name="what-is-the-security-model-for-vnets"></a>Wat is het beveiligingsmodel voor VNets?
Vnet's zijn geïsoleerd van elkaar en andere services die worden gehost in de Azure-infrastructuur. Een VNet is een grens van een vertrouwensrelatie.

### <a name="can-i-restrict-inbound-or-outbound-traffic-flow-to-vnet-connected-resources"></a>Kan ik binnenkomend of uitgaand netwerkverkeer beperken tot resources VNet-verbinding?
Ja. U kunt toepassen [Netwerkbeveiligingsgroepen](security-overview.md) aan afzonderlijke subnetten binnen een VNet, NIC's die zijn gekoppeld aan een VNet, of beide.

### <a name="can-i-implement-a-firewall-between-vnet-connected-resources"></a>Kan ik een firewall tussen VNet-verbinding-resources implementeren?
Ja. U kunt implementeren een [virtueel netwerkapparaat met firewall](https://azure.microsoft.com/marketplace/?term=firewall) van verschillende leveranciers via Azure Marketplace.

### <a name="is-there-information-available-about-securing-vnets"></a>Is er informatie beschikbaar over het beveiligen van vnet's?
Ja. Zie voor meer informatie, [overzicht van Azure Network Security](../security/security-network-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="apis-schemas-and-tools"></a>API's, schema's en hulpprogramma 's

### <a name="can-i-manage-vnets-from-code"></a>Kan ik vnet's beheren vanuit code?
Ja. U kunt de REST-API's gebruiken voor vnet's in de [Azure Resource Manager](/rest/api/virtual-network) en [klassieke (Service Management)](https://go.microsoft.com/fwlink/?LinkId=296833) implementatiemodellen.

### <a name="is-there-tooling-support-for-vnets"></a>Is er ondersteuning voor hulpprogramma's voor VNets?
Ja. Meer informatie over het gebruik van:
- De Azure portal om te implementeren VNets via de [Azure Resource Manager](manage-virtual-network.md#create-a-virtual-network) en [klassieke](virtual-networks-create-vnet-classic-pportal.md) implementatiemodellen.
- PowerShell voor het beheren van VNets die zijn geïmplementeerd via de [Resource Manager](/powershell/module/az.network) en [klassieke](/powershell/module/servicemanagement/azure/?view=azuresmps-3.7.0) implementatiemodellen.
- De Azure-opdrachtregelinterface (CLI) te implementeren en beheren van de VNets die zijn geïmplementeerd via de [Resource Manager](/cli/azure/network/vnet) en [klassieke](../virtual-machines/azure-cli-arm-commands.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-network-commands-to-manage-network-resources) implementatiemodellen.  

## <a name="vnet-peering"></a>VNet-peering

### <a name="what-is-vnet-peering"></a>Wat is de VNet-peering?
VNet-peering (of peering op virtueel netwerk), kunt u virtuele netwerken verbinden. Een VNet-peeringverbinding tussen virtuele netwerken kunt u het routeren van verkeer tussen privé via IPv4-adressen. Virtuele machines in gekoppelde VNets kunnen met elkaar communiceren, alsof ze zich in hetzelfde netwerk. Deze virtuele netwerken kunnen zich in dezelfde regio of in verschillende regio's (ook wel bekend als wereldwijde VNet-Peering). VNet-peeringverbindingen kunnen ook worden gemaakt voor Azure-abonnementen.

### <a name="can-i-create-a-peering-connection-to-a-vnet-in-a-different-region"></a>Kan ik een peering verbinding met een VNet maken in een andere regio?
Ja. Wereldwijde VNet-peering, kunt u peer-VNets in verschillende regio's. Wereldwijde VNet-peering is beschikbaar in alle Azure-openbare regio's, China-cloud-regio's en Government cloud-regio's. U kan niet uit Azure openbare regio's wereldwijd koppelen aan nationale cloud-regio's.

### <a name="what-are-the-constraints-related-to-global-vnet-peering-and-load-balancers"></a>Wat zijn de beperkingen met betrekking tot wereldwijde VNet-Peering en Load Balancers?
Als de twee virtuele netwerken zich in verschillende regio's (wereldwijde VNet-Peering), kunt u geen verbinding maken met bronnen die gebruikmaken van de basisversie van Load Balancer. U kunt verbinding maken met bronnen die gebruikmaken van Standard Load Balancer.
De volgende bronnen gebruiken Basic Load Balancers, wat betekent dat u aan hen kan niet communiceren via het wereldwijde VNet-Peering:
- Virtuele machines achter de basisversie van Load Balancers
- VM-Schaalsets met basisversie van Load Balancers 
- Redis-cache 
- Application Gateway (v1) SKU
- Service Fabric
- SQL MI
- API Management
- Active Directory Domain Services (ADDS)
- Logic Apps
- HD Insight
-   Azure Batch
- AKS
- App Service Environment

U kunt verbinding maken met deze resource via ExpressRoute of VNet-naar-VNet via VNet-Gateways.

### <a name="can-i-enable-vnet-peering-if-my-virtual-networks-belong-to-subscriptions-within-different-azure-active-directory-tenants"></a>Kan ik inschakelen VNet-Peering als mijn virtuele netwerken deel uitmaken van abonnementen in verschillende Azure Active Directory-tenants?
Ja. Het is mogelijk te maken van VNet-Peering (lokaal of globaal) als uw abonnementen tot verschillende Azure Active Directory-tenants behoren. U kunt dit doen via PowerShell of CLI. Portal is nog niet ondersteund.

### <a name="my-vnet-peering-connection-is-in-initiated-state-why-cant-i-connect"></a>Mijn VNet peering verbinding *gestart* staat, waarom kan ik geen verbinding?
Als de peeringverbinding de status gestart is, betekent dit dat u hebt slechts één koppeling gemaakt. Een koppeling in twee richtingen moet worden gemaakt om een geslaagde verbinding tot stand brengen. Bijvoorbeeld, als u wilt het peer-VNet A naar VNet B, moet een koppeling worden gemaakt van VNetA aan VNetB en als VNetB naar VNetA. Het maken van beide koppelingen, verandert de status om te *verbonden.*

### <a name="my-vnet-peering-connection-is-in-disconnected-state-why-cant-i-create-a-peering-connection"></a>Mijn VNet peering verbinding *verbroken* staat, waarom kan ik een peering de verbinding maken?
Als uw VNet-peeringverbinding in een niet-verbonden toestand is, betekent dit een van de koppelingen die gemaakt is verwijderd. Als u wilt opnieuw peering verbinding maken, moet u de koppeling verwijderen en opnieuw maken.

### <a name="can-i-peer-my-vnet-with-a-vnet-in-a-different-subscription"></a>Kan ik mijn VNet met een VNet in een ander abonnement koppelen?
Ja. U kunt VNets koppelen tussen abonnementen en regio's.

### <a name="can-i-peer-two-vnets-with-matching-or-overlapping-address-ranges"></a>Kan ik twee VNets met overeenkomende of overlappende adresbereiken koppelen?
Nee. Adresruimten mag niet overlappen om in te schakelen VNet-Peering.

### <a name="how-much-do-vnet-peering-links-cost"></a>VNet-peering kosten koppelingen hoeveel doen?
Er zijn geen kosten voor het maken van een VNet-peeringverbinding. Gegevens worden overgebracht via peering-verbindingen wordt in rekening gebracht. [Hier ziet](https://azure.microsoft.com/pricing/details/virtual-network/).

### <a name="is-vnet-peering-traffic-encrypted"></a>Is de VNet-peering verkeer versleuteld?
Nee. Verkeer tussen resources in gekoppelde VNets wordt privé en geïsoleerd. Deze blijven volledig op de Microsoft-Backbone.

### <a name="why-is-my-peering-connection-in-a-disconnected-state"></a>Waarom is mijn peeringverbinding in een niet-verbonden status?
VNet-peeringverbindingen gaan naar *verbroken* status wanneer een VNet-peeringkoppeling is verwijderd. Als u wilt herstellen van een peering verbinding is geslaagd, moet u beide koppelingen verwijderen.

### <a name="if-i-peer-vneta-to-vnetb-and-i-peer-vnetb-to-vnetc-does-that-mean-vneta-and-vnetc-are-peered"></a>Als ik het peer-VNetA aan VNetB en peer ik VNetB aan VNetC, betekent dit VNetA en c. aan elkaar zijn gekoppeld?
Nee. Transitieve peering wordt niet ondersteund. U moet het peer-VNetA en c. voor deze om te worden uitgevoerd.

### <a name="are-there-any-bandwidth-limitations-for-peering-connections"></a>Zijn er bandbreedtebeperkingen voor peering-verbindingen?
Nee. VNet-peering, lokaal of globaal, legt bandbreedtebeperkingen voor. Bandbreedte wordt alleen beperkt door de virtuele machine of de compute-resource.

### <a name="how-can-i-troubleshoot-vnet-peering-issues"></a>Hoe kan ik VNet-Peering problemen oplossen?
Hier volgt een [gids voor problemen oplossen] (https://support.microsoft.com/en-us/help/4486956/troubleshooter-for-virtual-network-peering-issues) kunt u proberen.

## <a name="virtual-network-tap"></a>Virtual Network TAP

### <a name="which-azure-regions-are-available-for-virtual-network-tap"></a>Welke Azure-regio's zijn beschikbaar voor het virtuele netwerk-TAP?
Virtual network TAP preview is beschikbaar in alle Azure-regio's. De bewaakte netwerkinterfaces, de bron van virtueel netwerk te TIKKEN en de collector of analytics-oplossing moeten worden geïmplementeerd in dezelfde regio.

### <a name="does-virtual-network-tap-support-any-filtering-capabilities-on-the-mirrored-packets"></a>Virtual Network TAP ondersteunt filters gebruiken om mogelijkheden van de gespiegelde pakketten?
Filtermogelijkheden worden niet ondersteund met de preview van virtueel netwerk te TIKKEN. Wanneer een Tik-configuratie wordt toegevoegd aan een netwerkinterface een grondige kopie van alle inkomende en uitgaande verkeer op de netwerkinterface wordt gestreamd naar de bestemming te TIKKEN.

### <a name="can-multiple-tap-configurations-be-added-to-a-monitored-network-interface"></a>Kunnen de configuraties met meerdere Tik worden toegevoegd aan een bewaakte netwerkinterface?
Een bewaakte netwerkinterface kan slechts één tik configuratie hebben. Neem contact op met de afzonderlijke [partneroplossingen](virtual-network-tap-overview.md#virtual-network-tap-partner-solutions) voor de mogelijkheid om te streamen van meerdere exemplaren van het verkeer te TIKKEN naar de analytics-hulpprogramma's van uw keuze.

### <a name="can-the-same-virtual-network-tap-resource-aggregate-traffic-from-monitored-network-interfaces-in-more-than-one-virtual-network"></a>Kan de dezelfde tikt u op de virtueelnetwerkresource verkeer aggregeren van bewaakte netwerkinterfaces in meer dan één virtueel netwerk
Ja. Hetzelfde virtuele netwerk te TIKKEN resource kan worden gebruikt voor statistische gespiegelde netwerkverkeer van bewaakte netwerkinterfaces in gekoppelde virtuele netwerken in hetzelfde abonnement of een ander abonnement. De bron van virtueel netwerk te TIKKEN en de doel-load balancers of doel-netwerkinterface moet zich in hetzelfde abonnement. Alle abonnementen moeten zich in dezelfde Azure Active Directory-tenant.

### <a name="are-there-any-performance-considerations-on-production-traffic-if-i-enable-a-virtual-network-tap-configuration-on-a-network-interface"></a>Zijn er prestatie-overwegingen on productieverkeer als ik de configuratie van een virtueel netwerk te TIKKEN op een netwerkinterface inschakelen?

Virtueel netwerk TIKKEN is in preview. Tijdens de preview is er geen serviceovereenkomst. De mogelijkheid mag niet worden gebruikt voor werkbelastingen voor productie. Wanneer de netwerkinterface van een virtuele machine met een Tik-configuratie is ingeschakeld, worden dezelfde resources op de azure-host toegewezen aan de virtuele machine om de productieverkeer te verzenden wordt gebruikt om de mirroring functie uitvoeren en het verzenden van de gespiegelde pakketten. Selecteer de juiste [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) of [Windows](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) grootte van de virtuele machine om ervoor te zorgen dat er voldoende bronnen beschikbaar zijn voor de virtuele machine voor het verzenden van productieverkeer van de en de gespiegelde zijn.

### <a name="is-accelerated-networking-for-linuxcreate-vm-accelerated-networking-climd-or-windowscreate-vm-accelerated-networking-powershellmd-supported-with-virtual-network-tap"></a>Versneld netwerken voor [Linux](create-vm-accelerated-networking-cli.md) of [Windows](create-vm-accelerated-networking-powershell.md) met virtual network TAP ondersteund?

U kunt zich een Tik-configuratie toevoegen aan een netwerkinterface die is gekoppeld aan een virtuele machine die met versneld netwerken is ingeschakeld. Maar de prestaties en de latentie voor de virtuele machine worden beïnvloed door het toevoegen van de configuratie te TIKKEN omdat de offload voor het spiegelen van verkeer wordt momenteel niet ondersteund door Azure accelerated networking.

## <a name="virtual-network-service-endpoints"></a>Service-eindpunten voor virtueel netwerk

### <a name="what-is-the-right-sequence-of-operations-to-set-up-service-endpoints-to-an-azure-service"></a>Wat is de juiste volgorde van bewerkingen voor het instellen van service-eindpunten naar een Azure-service?
Er zijn twee stappen voor het beveiligen van een Azure-Serviceresource via service-eindpunten:
1. Service-eindpunten voor de Azure-service inschakelen.
2. VNet-ACL's instellen voor de Azure-service.

De eerste stap is een netwerkbewerking aan de clientzijde en de tweede stap is een bewerking service resource aan de clientzijde. Beide stappen kunnen worden uitgevoerd door de beheerder van dezelfde of verschillende beheerders op basis van de RBAC-machtigingen verleend aan de beheerdersrol. Het is raadzaam dat u eerst service-eindpunten voor het virtuele netwerk inschakelen voordat het instellen van VNet-ACL's op Azure-service aan clientzijde. Daarom moeten de stappen worden uitgevoerd in de reeks bovenstaande voor het instellen van VNet-service-eindpunten.

>[!NOTE]
> Zowel de bewerkingen die hierboven worden beschreven moeten worden voltooid voordat u de Azure-Servicetoegang tot de toegestane VNet en subnet kunt beperken. Alleen inschakelen van service-eindpunten voor de Azure-service op het netwerk aan clientzijde biedt niet u de beperkte toegang. Bovendien moet u ook instellen VNet ACL's aan de Azure-service.

Bepaalde services (zoals SQL en cosmos DB) kunnen uitzonderingen aan de bovenstaande reeks via de **IgnoreMissingVnetServiceEndpoint** vlag. Zodra de vlag is ingesteld op **waar**, VNet ACL's kunnen worden ingesteld voor de Azure-service kant voordat het instellen van de service-eindpunten op het netwerk aan clientzijde. Azure-services bieden met deze markering om klanten in gevallen waarbij de specifieke IP-firewalls op Azure-services zijn geconfigureerd en inschakelen van de service-eindpunten aan de kant van het netwerk kan leiden tot een afname van de verbinding, omdat het bron-IP-adres wordt gewijzigd van een openbaar IPv4-adres aan te een privé-adres. Instellen van VNet-ACL's aan de Azure-service voordat u de service-eindpunten aan de kant van het netwerk, kunt u voorkomen dat op een afname van de verbinding.

### <a name="do-all-azure-services-reside-in-the-azure-virtual-network-provided-by-the-customer-how-does-vnet-service-endpoint-work-with-azure-services"></a>Alle Azure-services bevinden zich in de Azure-netwerk dat is opgegeven door de klant? Hoe werkt VNet service-eindpunt met Azure-services?

Nee, niet alle Azure-services bevinden zich in het virtuele netwerk van de klant. Meerderheid van de gegevens van de Azure-services zoals Azure Storage, Azure SQL en Azure Cosmos DB, multitenant-services die zijn toegankelijk via openbare IP-adressen zijn. U kunt meer informatie over de integratie van virtueel netwerk voor Azure-services [hier](virtual-network-for-azure-services.md). 

Wanneer u de functie VNet service endpoints (VNet-service-eindpunt op het netwerk aan clientzijde inschakelen en instellen van de juiste ACL's een VNet aan de Azure-service), wordt de toegang tot een Azure-service van een toegestane VNet en subnet beperkt.

### <a name="how-does-vnet-service-endpoint-provide-security"></a>Hoe biedt VNet service-eindpunt beveiliging?

De Azure-Servicetoegang tot toegestane VNet en subnet, zodat een netwerkbeveiliging en isolatie van Hiermee beperkt u de VNet-service-eindpunt-functie (VNet-service-eindpunt op het netwerk aan clientzijde inschakelen en instellen van de juiste ACL's een VNet aan de Azure-service) de Azure-serviceverkeer. Microsoft-backbone, zodat een andere laag van de isolatie van het openbare internet loopt al het verkeer met behulp van VNet-service-eindpunten. Bovendien kunnen klanten kiezen voor het openbare internettoegang tot de Azure-serviceresources volledig te verwijderen en toe te staan alleen verkeer vanaf hun virtuele netwerk via een combinatie van IP-firewall- en VNet-ACL's, dus de Azure-serviceresources beveiligen tegen onbevoegde de toegang.      

### <a name="what-does-the-vnet-service-endpoint-protect---vnet-resources-or-azure-service"></a>Wat doet de VNet-service-eindpunt beveiligen - VNet-resources of Azure-service?
VNet-service-eindpunten helpen beschermen van de Azure-serviceresources. VNet-resources die worden beschermd met Netwerkbeveiligingsgroepen (nsg's).

### <a name="is-there-any-cost-for-using-vnet-service-endpoints"></a>Is er geen kosten voor het gebruik van VNet-service-eindpunten?

Nee, er is geen extra kosten voor het gebruik van VNet-service-eindpunten.

### <a name="can-i-turn-on-vnet-service-endpoints-and-set-up-vnet-acls-if-the-virtual-network-and-the-azure-service-resources-belong-to-different-subscriptions"></a>Kan ik VNet-service-eindpunten inschakelen en instellen van VNet-ACL's als het virtuele netwerk en de Azure-serviceresources tot verschillende abonnementen behoren?

Ja, dat is mogelijk. Virtuele netwerken en Azure-serviceresources kunnen zich in dezelfde of verschillende abonnementen zijn. De enige vereiste is dat het virtuele netwerk en de Azure-serviceresources onder dezelfde Active Directory (AD)-tenant moeten.

### <a name="can-i-turn-on-vnet-service-endpoints-and-set-up-vnet-acls-if-the-virtual-network-and-the-azure-service-resources-belong-to-different-ad-tenants"></a>Kan ik VNet-service-eindpunten inschakelen en instellen van VNet-ACL's als het virtuele netwerk en de Azure-serviceresources tot verschillende AD-tenants behoren?
VNet-service-eindpunten en VNet-ACL's worden Nee, niet ondersteund voor AD-tenants.

### <a name="can-an-on-premises-devices-ip-address-that-is-connected-through-azure-virtual-network-gateway-vpn-or-express-route-gateway-access-azure-paas-service-over-vnet-service-endpoints"></a>IP-adres van een on-premises apparaat dat is verbonden via Azure Virtual Network gateway (VPN) of Express route-gateway via toegang tot Azure PaaS-Service VNet-service-eindpunten?
Standaard zijn Azure-serviceresources die zijn beveiligd naar virtuele netwerken, niet bereikbaar vanaf on-premises netwerken. Als u verkeer toestaan vanaf on-premises wilt, moet u ook toestaan (doorgaans NAT) openbare IP-adressen uit uw on-premises of ExpressRoute. Deze IP-adressen kunnen worden toegevoegd via de firewall voor IP-configuratie voor de Azure-serviceresources.

### <a name="can-i-use-vnet-service-endpoint-feature-to-secure-azure-service-to-multiple-subnets-with-in-a-virtual-network-or-across-multiple-virtual-networks"></a>Kan ik VNet-Service-eindpunt-functie gebruiken voor het beveiligen van Azure-service met meerdere subnetten met meerdere virtuele netwerken of in een virtueel netwerk?
Als u wilt beveiligen in Azure-services naar meerdere subnetten binnen een virtueel netwerk of op meerdere virtuele netwerken, service-eindpunten op het netwerk aan clientzijde op elk van de subnetten onafhankelijk van elkaar inschakelen en vervolgens Azure-serviceresources beveiligen naar alle van de subnetten door in te stellen juiste VNet ACL's aan Azure-service.
 
### <a name="how-can-i-filter-outbound-traffic-from-a-virtual-network-to-azure-services-and-still-use-service-endpoints"></a>Hoe kan ik filteren van uitgaand verkeer vanuit een virtueel netwerk naar Azure-services en nog steeds gebruikmaken van service-eindpunten?
Als u het verkeer dat is bestemd voor een Azure-service vanaf het virtuele netwerk, wilt controleren of filteren, kunt u een virtueel netwerkapparaat implementeren binnen het virtuele netwerk. U kunt service-eindpunten toepassen op het subnet waarin het virtuele netwerkapparaat geïmplementeerd en beveiligde Azure-serviceresources alleen naar dit subnet via VNet-ACL's is. In dit scenario kan ook nuttig zijn als u wilt toegang tot Azure-service vanaf uw virtuele netwerk te beperken tot specifieke Azure-resources met behulp van virtueel-netwerkapparaatfilters. Zie [Egress with network virtual appliances](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha) (Uitgaand verkeer met virtueel-netwerkapparaten) voor meer informatie.

### <a name="what-happens-when-you-access-an-azure-service-account-that-has-virtual-network-access-control-list-acl-enabled-from-outside-the-vnet"></a>Wat gebeurt er wanneer u een Azure-service-account met virtueel netwerk toegangsbeheerlijst (ACL) ingeschakeld van buiten het VNet?
De HTTP 403- of HTTP 404-fout is geretourneerd.

### <a name="are-subnets-of-a-virtual-network-created-in-different-regions-allowed-to-access-an-azure-service-account-in-another-region"></a>Zijn de subnetten van een virtueel netwerk hebt gemaakt in verschillende regio's die toegang hebben tot een Azure-service-account in een andere regio? 
Ja, voor het merendeel van de Azure-services, virtuele netwerken die zijn gemaakt in verschillende regio's hebben toegang tot Azure-services in een andere regio via de VNet-service-eindpunten. Bijvoorbeeld, een Azure Cosmos DB-account is in VS-West of VS-Oost en virtuele netwerken in meerdere regio's zijn, het virtuele netwerk toegang tot Azure Cosmos DB. Storage en SQL zijn uitzonderingen en regionale van aard zijn en zowel het virtuele netwerk en de Azure-service moeten zich in dezelfde regio bevinden.
  
### <a name="can-an-azure-service-have-both-vnet-acl-and-an-ip-firewall"></a>Kan een Azure-service hebben zowel ACL VNet en een IP-firewall?
Ja, VNet-ACL en een IP-firewall kunnen naast elkaar bestaan. Beide functies vullen elkaar om ervoor te zorgen isolatie en beveiliging.
 
### <a name="what-happens-if-you-delete-a-virtual-network-or-subnet-that-has-service-endpoint-turned-on-for-azure-service"></a>Wat gebeurt er als u een virtueel netwerk of subnet waarvoor service-eindpunt ingeschakeld voor Azure-service verwijderen?
Het verwijderen van vnet's en subnetten onafhankelijke bewerkingen zijn en worden ondersteund, zelfs wanneer de service-eindpunten zijn ingeschakeld voor Azure-services. In gevallen waarin de Azure-services VNet-ACL's instellen, voor de VNets en subnetten, hebben de ACL's voor VNet-gegevens die zijn gekoppeld met die Azure-service wordt uitgeschakeld wanneer u een VNet of subnet met VNet-service-eindpunt ingeschakeld is verwijderd.
 
### <a name="what-happens-if-azure-service-account-that-has-vnet-service-endpoint-enabled-is-deleted"></a>Wat gebeurt er als Azure-service-account met de VNet-Service-eindpunt ingeschakeld wordt verwijderd?
De verwijdering van de Azure-service-account is een onafhankelijke bewerking en wordt ondersteund, zelfs wanneer het service-eindpunt is ingeschakeld op het netwerk aan clientzijde en VNet-ACL's zijn ingesteld op Azure-service aan clientzijde. 

### <a name="what-happens-to-the-source-ip-address-of-a-resource-like-a-vm-in-a-subnet-that-has-vnet-service-endpoint-enabled"></a>Wat gebeurt er met de bron-IP-adres van een resource (zoals een virtuele machine in een subnet) met VNet-service-eindpunt ingeschakeld?
Wanneer service-eindpunten zijn ingeschakeld, schakelt u de bron-IP-adressen van de resources in uw virtuele netwerk subnet van het gebruik van openbare IPV4-adressen in het Azure-netwerk privé IP-adressen voor verkeer naar Azure-service. Houd er rekening mee dat dit leiden specifieke IP-firewall die zijn ingesteld op openbaar IPV4-adres eerder op de Azure-services tot kan mislukken. 

### <a name="does-service-endpoint-route-always-take-precedence"></a>Service-eindpuntroute hebben altijd voorrang?
Service-eindpunten toevoegen een systeemroute heeft voorrang op de BGP-routes en bieden een optimale routering voor het service-eindpunt-verkeer. Service-eindpunten altijd dat serviceverkeer rechtstreeks van uw virtuele netwerk naar de service op de Microsoft Azure-backbone-netwerk. Zie voor meer informatie over hoe Azure een route selecteert, [verkeersroutering van Azure Virtual network](virtual-networks-udr-overview.md).
 
### <a name="how-does-nsg-on-a-subnet-work-with-service-endpoints"></a>Hoe werkt NSG op een subnet met service-eindpunten?
Als u wilt de Azure-service bereiken, moeten nsg's uitgaande connectiviteit toegestaan. Als uw nsg's zijn met alle uitgaande internetverkeer geopend, klikt u vervolgens werkt het service-eindpunt verkeer. U kunt ook het uitgaande verkeer naar de service IP-adressen alleen met behulp van de servicetags beperken.  
 
### <a name="what-permissions-do-i-need-to-set-up-service-endpoints"></a>Welke machtigingen heb ik nodig voor het instellen van service-eindpunten?
Service-eindpunten kunnen afzonderlijk worden geconfigureerd in een virtueel netwerk door een gebruiker met schrijftoegang tot het virtuele netwerk. Als u Azure-serviceresources aan een VNet wilt koppelen, moet u machtigingen hebben voor **Microsoft.Network/JoinServicetoaSubnet** voor de subnetten die worden toegevoegd. Deze machtiging is standaard opgenomen in de rol van de ingebouwde service-beheerder en kan worden gewijzigd door aangepaste rollen te maken. Meer informatie over ingebouwde rollen en het toewijzen van specifieke machtigingen voor [aangepaste rollen](https://docs.microsoft.com/azure/role-based-access-control/custom-roles?toc=%2fazure%2fvirtual-network%2ftoc.json).
 

### <a name="can-i-filter-virtual-network-traffic-to-azure-services-allowing-only-specific-azure-service-resources-over-vnet-service-endpoints"></a>Kan ik virtuele netwerkverkeer filtert naar Azure-services, zodat alleen bepaalde azure-serviceresources, via VNet-service-eindpunten? 

Service-eindpuntbeleid virtueel netwerk (VNet) kunnen u voor het filteren van verkeer in virtuele netwerken voor Azure-services, zodat alleen bepaalde Azure-serviceresources via de service-eindpunten. Eindpuntbeleid biedt gedetailleerd toegangsbeheer van het virtuele netwerkverkeer voor de Azure-services. U kunt meer informatie over de service-eindpuntbeleid [hier](virtual-network-service-endpoint-policies-overview.md).
 
### <a name="are-there-any-limits-on-how-many-vnet-service-endpoints-i-can-set-up-from-my-vnet"></a>Zijn er beperkingen met betrekking op het aantal VNet-service-eindpunten ik in mijn VNet instellen kunt?
Er is geen limiet voor het totale aantal VNet-service-eindpunten in een virtueel netwerk. Voor een Azure-serviceresource (zoals een Azure Storage-account) kunnen services beperkingen hebben met betrekking tot het aantal subnetten dat wordt gebruikt voor het koppelen van de resource. De volgende tabel ziet u enkele beperkingen voorbeeld: 

|||
|---|---|
|Azure-service| Limieten voor VNet-regels|
|Azure Storage| 100|
|Azure SQL| 128|
|Azure SQL Data Warehouse|  128|
|Azure KeyVault|    127|
|Azure Cosmos DB|   64|
|Azure Event Hub|   128|
|Azure Service Bus| 128|
|Azure Data Lake Store V1|  100|
 
>[!NOTE]
> De limieten zijn onderworpen aan wijzigingen op de keuze van de Azure-service. Raadpleeg de servicedocumentatie van de betreffende voor services informatie. 




  



