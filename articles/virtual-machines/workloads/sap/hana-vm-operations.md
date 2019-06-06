---
title: Configuraties van SAP HANA-infrastructuur en bewerkingen op Azure | Microsoft Docs
description: Bedieningshandleiding voor SAP HANA-systemen die zijn geïmplementeerd op Azure virtual machines.
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/05/2019
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 156bb4cbf43dc71627f9db785dba574f25139285
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/06/2019
ms.locfileid: "66733827"
---
# <a name="sap-hana-infrastructure-configurations-and-operations-on-azure"></a>Configuraties en bewerkingen van SAP HANA-infrastructuur in Azure
Dit document biedt richtlijnen voor het configureren van Azure-infrastructuur en SAP HANA besturingssystemen die zijn geïmplementeerd op virtuele machines van Azure (VM's). Het document bevat ook informatie over de configuratie voor SAP HANA scale-out voor de M128s VM-SKU. Dit document is niet bedoeld als vervanging van de standaard SAP-documentatie, waaronder de volgende inhoud:

- [Gebruikershandleiding voor SAP](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/330e5550b09d4f0f8b6cceb14a64cd22.html)
- [SAP-installatiehandleidingen](https://service.sap.com/instguides)
- [SAP-opmerkingen](https://sservice.sap.com/notes)

## <a name="prerequisites"></a>Vereisten
Voor het gebruik van deze handleiding, moet u basiskennis hebt van de volgende Azure-onderdelen:

- [Virtuele machines van Azure](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-vm)
- [Azure-netwerken en virtuele netwerken](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-virtual-network)
- [Azure Storage](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-disks)

Zie voor meer informatie over SAP NetWeaver en andere onderdelen van SAP op Azure, de [SAP op Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started) sectie van de [documentatie voor Azure](https://docs.microsoft.com/azure/).

## <a name="basic-setup-considerations"></a>Overwegingen bij de eenvoudige installatie
De volgende secties beschrijven de basisconfiguratie van overwegingen voor het implementeren van systemen voor SAP HANA op Azure Virtual machines.

### <a name="connect-into-azure-virtual-machines"></a>Verbinding maken met virtuele machines van Azure
Zoals beschreven in de [Planningshandleiding virtuele Azure-machines](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide), er zijn twee basismethoden om verbinding te maken in Azure-VM's:

- Verbinding maken via het internet en openbare eindpunten op een VM gaan of op de virtuele machine die SAP HANA wordt uitgevoerd.
- Verbinding maken via een [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal) of Azure [ExpressRoute](https://azure.microsoft.com/services/expressroute/).

Site-naar-site-connectiviteit via VPN of ExpressRoute is nodig voor productiescenario's. Dit type verbinding is ook nodig voor niet-productie scenario's die in productiescenario's waarop SAP-software wordt gebruikt. De volgende afbeelding toont een voorbeeld van cross-site-verbinding:

![Cross-site-connectiviteit](media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png)


### <a name="choose-azure-vm-types"></a>Kies typen Azure VM 's
De typen Azure VM's die kunnen worden gebruikt voor productiescenario's worden vermeld in de [SAP-documentatie voor IAAS](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html). Voor niet-productie scenario's is een groter aantal systeemeigen Azure-VM-typen beschikbaar.

>[!NOTE]
> Voor niet-productie scenario's, gebruikt u de VM-typen die worden vermeld in de [SAP-notitie 1928533 #](https://launchpad.support.sap.com/#/notes/1928533). Controleer voor het gebruik van Azure-VM's voor productiescenario's, voor SAP HANA-virtuele machines in de SAP gepubliceerd gecertificeerde [lijst met gecertificeerde IaaS Platforms](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure).

Implementeer de virtuele machines in Azure met behulp van:

- De Azure-portal.
- Azure PowerShell-cmdlets.
- De Azure CLI.

U kunt een volledige SAP HANA-platform is geïnstalleerd op de virtuele machine van Azure-services via ook implementeren de [SAP Cloud platform](https://cal.sap.com/). Het installatieproces wordt beschreven in [implementeren SAP S/4HANA of BW/4HANA on Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h) of met de automatisering die zijn uitgebracht [hier](https://github.com/AzureCAT-GSI/SAP-HANA-ARM).

### <a name="storage-configuration-for-sap-hana"></a>De opslagconfiguratie voor SAP HANA
Voor opslagconfiguraties en opslagtypen moet worden gebruikt met SAP HANA in Azure, leest u het document [opslagconfiguraties voor SAP HANA Azure virtuele machine](./hana-vm-operations-storage.md)


### <a name="set-up-azure-virtual-networks"></a>Virtuele Azure-netwerken instellen
Als u site-naar-site-verbinding naar Azure via VPN of ExpressRoute hebt, moet u ten minste één Azure-netwerk dat is verbonden via een virtuele Gateway naar het VPN of ExpressRoute-circuit hebben. In eenvoudige implementaties kan de virtuele Gateway worden geïmplementeerd in een subnet van de Azure-netwerk (VNet) dat als host fungeert voor de SAP HANA-instanties. Voor het installeren van SAP HANA, maakt u twee extra subnetten binnen het Azure-netwerk. Eén subnet als host fungeert voor de virtuele machines om uit te voeren van de SAP HANA-instanties. Het andere subnet voert Jumpbox of beheer-VM's voor het hosten van SAP HANA Studio of andere software voor beheer van de toepassingssoftware van uw.

> [!IMPORTANT]
> Buiten-functionaliteit, maar meer belangrijke uit prestatieoverwegingen wordt niet ondersteund voor het configureren van [Azure Network Virtual Appliances](https://azure.microsoft.com/solutions/network-appliances/) in het communicatiepad tussen de SAP-toepassing en de DBMS-laag van een SAP NetWeaver Hybris of S/4HANA op basis van SAP-systeem. De communicatie tussen de SAP-toepassingslaag en de DBMS-laag moet een direct. De beperking bevat geen [Azure ASG en NSG-regels](https://docs.microsoft.com/azure/virtual-network/security-overview) , zolang deze ASG en NSG-regels toestaan een rechtstreekse communicatie. Aanvullende scenario's waarbij NVA's worden niet ondersteund in de communicatiepaden van de tussen Azure-VM's die staan voor de clusterknooppunten Linux Pacemaker en SBD apparaten zoals beschreven in zijn [hoge beschikbaarheid voor SAP NetWeaver op Azure VM's in SUSE Linux Enterprise Server voor SAP-toepassingen](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse). Of in de communicatie paden tussen Azure VM's en Windows Server SOFS instellen maximaal zoals beschreven in [Cluster een SAP ASCS/SCS-exemplaar op een Windows-failovercluster met behulp van een bestandsshare in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-file-share). NVA's in communicatie paden kunnen eenvoudig de netwerklatentie tussen twee communicatie partners dubbele, doorvoer in kritieke paden tussen het niveau van de SAP-toepassing en de DBMS-laag kunt beperken. In sommige scenario's met klanten in acht genomen, NVA's kunnen leiden tot Pacemaker Linux-clusters in gevallen waarin de communicatie tussen de knooppunten van het Linux-Pacemaker om te communiceren met hun apparaat SBD via een NVA is mislukt.  
> 

> [!IMPORTANT]
> Een andere ontwerp is de **niet** de scheiding van het niveau van de SAP-toepassing en de DBMS-laag in verschillende virtuele netwerken die niet wordt ondersteund is [gekoppeld](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) met elkaar. Het verdient aanbeveling om te scheiden van de SAP-toepassingslaag en DBMS-laag met behulp van de subnetten binnen een virtueel Azure-netwerk in plaats van verschillende virtuele netwerken. Als u besluit niet op de aanbeveling volgen en scheiden in plaats daarvan de twee lagen in een ander virtueel netwerk, de twee virtuele netwerken moeten worden [gekoppeld](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview). Houd er rekening mee dat netwerkverkeer tussen twee [gekoppeld](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) virtuele netwerken van Azure zijn onderwerp van de kosten van gegevensoverdracht. Met de enorme gegevensvolume in vele Terabytes die worden uitgewisseld tussen de SAP-toepassingslaag en DBMS laag aanzienlijke kosten kunnen worden samengevoegd als de SAP-toepassingslaag en DBMS laag is gescheiden tussen twee gekoppelde virtuele netwerken in Azure. 

Wanneer u de virtuele machines om uit te voeren van SAP HANA installeert, moeten de virtuele machines:

- Twee virtuele NIC's geïnstalleerd: één NIC verbinding maken met het beheersubnet en één NIC verbinding maken tussen de on-premises netwerk of andere netwerken, en de SAP HANA-instantie in de Azure-VM.
- Statische privé IP-adressen die zijn geïmplementeerd voor zowel virtuele NIC's.

> [!NOTE]
> Moet u statische IP-adressen via Azure middelen toewijzen aan afzonderlijke vnic's. U moet statische IP-adressen binnen het gastbesturingssysteem niet toewijzen aan een vNIC. Sommige Azure-services zoals Azure Backup-Service is afhankelijk van het feit dat op minimaal de primaire vNIC is ingesteld op DHCP- en niet op de vaste IP-adressen. Zie ook het document [los problemen met Azure virtuele machine back-up](https://docs.microsoft.com/azure/backup/backup-azure-vms-troubleshoot#networking). Als u meerdere statische IP-adressen toewijzen aan een virtuele machine wilt, moet u meerdere vnic's toewijzen aan een virtuele machine.
>
>

Voor implementaties die zijn onmisbaar, moet u echter een virtueel datacenter netwerkarchitectuur maken in Azure. Deze architectuur wordt aanbevolen de scheiding van de Azure-VNet-Gateway die verbinding maakt on-premises in een afzonderlijke Azure-VNet. Deze apart VNet moet al het verkeer dat een naar on-premises hosten of tot het internet. Deze benadering kunt u software voor controle en logboekregistratie-verkeer dat het virtuele datacenter in Azure in deze afzonderlijke hub VNet invoert te implementeren. Daarom moet u een VNet die als host fungeert voor de software en configuraties die is gekoppeld aan in- en uitgaand verkeer naar uw Azure-implementatie.

De artikelen [Azure Virtual Datacenter: Een Netwerkperspectief](https://docs.microsoft.com/azure/architecture/vdc/networking-virtual-datacenter) en [Azure Virtual Datacenter en de Enterprise-Controlelaag](https://docs.microsoft.com/azure/architecture/vdc/) geven u meer informatie over de virtual datacenter-aanpak en gerelateerde Azure-VNet-ontwerp.


>[!NOTE]
>Verkeer tussen een VNet-hub en spoke met behulp van VNet [Azure VNet-peering](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) onderwerp is van aanvullende [kosten](https://azure.microsoft.com/pricing/details/virtual-network/). Op basis van de kosten, moet u mogelijk overwegen compromissen tussen een strikte hub en spoke-netwerkontwerp uitgevoerd en op meerdere [Azure ExpressRoute-Gateways](https://docs.microsoft.com/azure/expressroute/expressroute-about-virtual-network-gateways) dat u verbinding met 'knooppunten' maken om het overslaan van VNet-peering. Echter, Azure ExpressRoute-Gateways geïntroduceerd extra [kosten](https://azure.microsoft.com/pricing/details/vpn-gateway/) ook. U kunt ook extra kosten voor software van derden die u voor het netwerkverkeer logboekregistratie gebruikt, controleren en bewaken tegenkomen. Afhankelijk van de kosten voor het uitwisselen van gegevens via VNet-peering op de een-zijde en de kosten die zijn gemaakt door aanvullende Azure ExpressRoute-Gateways en aanvullende softwarelicenties, kunt u beslissen voor micro-segmentatie binnen een VNet met behulp van subnetten als isolatie-eenheid in plaats van vnet's.


Zie voor een overzicht van de verschillende methoden voor het toewijzen van IP-adressen, [IP-adres adrestypen en toewijzingsmethoden in Azure](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm). 

Voor VM's het uitvoeren van SAP HANA, dient u werken met statische IP-adressen die zijn toegewezen. Reden is dat bepaalde configuratiekenmerken voor HANA verwijzen naar IP-adressen.

[Azure Netwerkbeveiligingsgroepen (nsg's)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) worden gebruikt voor het verkeer wordt gerouteerd naar de SAP HANA-exemplaar of de jumpbox. De nsg's en uiteindelijk [Toepassingsbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/security-overview#application-security-groups) zijn gekoppeld aan de SAP HANA-subnet en het beheersubnet.

De volgende afbeelding toont een overzicht van een schema ruwe implementatie voor SAP HANA een hub en spoke-VNet-architectuur te volgen:

![Ruwe implementatie-schema voor SAP HANA](media/hana-vm-operations/hana-simple-networking.PNG)

Voor het implementeren van SAP HANA in Azure zonder dat een site-naar-site-verbinding, wilt u nog steeds afschermen van het SAP HANA-exemplaar van het openbare internet en te verbergen achter een proxy voor doorsturen. In dit eenvoudige scenario, is de implementatie maakt gebruik van Azure services in de ingebouwde DNS-hostnamen omzetten. Azure ingebouwde DNS-services zijn in een meer complexe implementatie waarbij openbare IP-adressen worden gebruikt, met name belangrijk. Gebruik Azure nsg's en [NVA's in Azure](https://azure.microsoft.com/solutions/network-appliances/) beheren, controleert de routering via internet in uw Azure-VNet-architectuur in Azure. De volgende afbeelding toont een ruwe schema voor het implementeren van SAP HANA zonder een site-naar-site-verbinding in een hub en spoke-architectuur VNet:
  
![Ruwe implementatie schema voor SAP HANA als een site-naar-site-verbinding](media/hana-vm-operations/hana-simple-networking2.PNG)
 

Een andere beschrijving over het NVA's in Azure gebruiken voor toegang tot beheren en te controleren van Internet zonder de hub- en -spokenetwerktopologie VNet-architectuur kunt u vinden in het artikel [maximaal beschikbare virtuele netwerkapparaten implementeren](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha).


## <a name="configuring-azure-infrastructure-for-sap-hana-scale-out"></a>Configureren van Azure-infrastructuur voor SAP HANA-uitschalen
Microsoft heeft een M-serie VM-SKU die is gecertificeerd voor de configuratie van een SAP HANA-scale-out. De VM-type M128s is gecertificeerd voor een scale-out van maximaal 16 knooppunten. Controleren op wijzigingen in scale-out-certificeringen voor SAP HANA op Azure Virtual machines, [lijst met gecertificeerde IaaS Platforms](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure).

De minimale OS-versies voor het implementeren van scale-out-configuraties in Azure VM's zijn:

- SUSE Linux 12 SP3
- Red hat Linux 7.4

Van de certificering van de scale-out 16 knooppunten

- Een knooppunt is het hoofdknooppunt
- Een maximum van 15 knooppunten zijn worker-knooppunten

>[!NOTE]
>In Azure VM scale-out implementaties is geen mogelijkheid voor het gebruik van een stand-by-knooppunt
>

De reden voor het niet meer kunt configureren van een stand-by-knooppunt zijn tweeledig:

- Azure heeft op dit moment geen systeemeigen NFS-service. Als gevolg hiervan moeten de NFS-shares kan worden geconfigureerd met behulp van de functionaliteit van andere leveranciers.
- Geen van de NFS-configuraties van derden kunnen om te voldoen aan de criteria van de latentie van opslag voor SAP HANA met hun oplossingen die zijn geïmplementeerd op Azure.

Als gevolg hiervan **/hana/gegevens** en **/hana/log** volumes kunnen niet worden gedeeld. Niet delen van deze volumes van de afzonderlijke knooppunten, voorkomt u dat het gebruik van een stand-by-SAP HANA-knooppunt in de configuratie van een scale-out.

Het ontwerp van de basis voor een enkel knooppunt in een scale-out-configuratie is als gevolg hiervan gaan er als volgt uitzien:

![Basisprincipes van de scale-out van één knooppunt](media/hana-vm-operations/scale-out-basics.PNG)

De basisconfiguratie van een VM-knooppunt voor SAP HANA scale-out ziet eruit zoals:

- Voor **/hana/gedeelde**, u een maximaal beschikbare NFS-cluster op basis van SUSE Linux 12 SP3 bouwen. Deze clusterhosts de **/hana/gedeelde** NFS share (s) van uw scale-out-configuratie en SAP NetWeaver of BW/4HANA Central Services. Documentatie voor het bouwen van een dergelijke configuratie is beschikbaar in het artikel [hoge beschikbaarheid voor NFS op Azure VM's in SUSE Linux Enterprise Server](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs)
- Alle volumes op andere schijven zijn **niet** gedeeld tussen de verschillende knooppunten en worden **niet** op basis van NFS. Installatie van configuraties en stappen voor het scale-out HANA installaties met niet-gedeelde **/hana/gegevens** en **/hana/log** verderop in dit document wordt geleverd.

>[!NOTE]
>De maximaal beschikbare NFS-cluster wordt zoals weergegeven in de afbeeldingen tot nu toe ondersteund met SUSE Linux alleen. Een maximaal beschikbare NFS-oplossing op basis van Red Hat gaat later worden gesteld.

Grootte van de volumes voor de knooppunten is dezelfde als scale-up, met uitzondering van **/hana/gedeelde**. Voor de SKU van de virtuele machine M128s de voorgestelde grootte en de typen er als volgt uitzien:

| VM-SKU | RAM | Met maximaal VM-I/O<br /> Doorvoer | /hana/data | / hana/log | / Root-volume | / usr/sap | Hana/back-up |
| --- | --- | --- | --- | --- | --- | --- | --- |
| M128s | 2000 GiB | 2000 MB/s |3 x P30 | 2 x P20 | 1 x P6 | 1 x P6 | 2 x P40 |


Controleer of de opslagdoorvoer van de voor de verschillende voorgestelde volumes voldoet aan de werkbelasting die u wilt uitvoeren. Als de workload grotere volumes voor **/hana/gegevens** en **/hana/log**, moet u het aantal Azure Premium Storage-VHD's verhogen. Formaat van een volume met meer VHD's dan de vermelde verhoogt de IOPS-waarde, en i/o-doorvoer binnen de grenzen van het type virtuele machine van Azure. Gelden ook Azure Write Accelerator voor de schijven dat formulier de **/hana/log** volume.
 
In het document [opslagvereisten voor SAP HANA TDI](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html), een formule met de naam die de grootte van definieert de **/hana/gedeelde** volume voor scale-out als de geheugengrootte van een knooppunt één werknemer per vier worker-knooppunten.

Ervan uitgaande dat u rekening houden met de SAP HANA scale-out gecertificeerde M128s Azure VM met ongeveer 2 TB geheugen, worden de SAP-aanbevelingen samengevat, zoals:

- Een hoofdknooppunt en maximaal vier worker-knooppunt, de **/hana/gedeelde** volume zou moeten zijn van de grootte van 2 TB. 
- Een hoofdknooppunt en vijf tot acht worker-knooppunten, de grootte van **/hana/gedeelde** moet 4 TB. 
- Een hoofdknooppunt en 9 12 worker-knooppunten, een grootte van 6 TB opslagruimte voor **/hana/gedeelde** is vereist. 
- Een hoofdknooppunt en gebruik te maken tussen 12 en 15 worker-knooppunten, u bent op te geven een **/hana/gedeelde** volume dat is 8 TB in grootte.

Het andere belangrijke ontwerp dat wordt weergegeven in de afbeeldingen van de configuratie met één knooppunt voor een scale-out SAP HANA-virtuele machine is het VNet of beter de subnetconfiguratie. SAP raadt een scheiding van de client-/ toepassingsbeleid gerichte verkeer van de communicatie tussen de HANA-knooppunten. Zoals wordt weergegeven in de afbeeldingen, wordt dit doel wordt bereikt door twee verschillende vnic's die zijn gekoppeld aan de virtuele machine. Beide vnic's zich in verschillende subnetten, hebt u twee verschillende IP-adressen. U kunt vervolgens de verkeersstroom beheren met regels voor doorsturen met nsg's of de gebruiker gedefinieerde routes.

Met name in Azure, moet u er geen middelen en methoden om af te dwingen quality of service en quota's voor specifieke vnic's zijn. Als gevolg hiervan de scheiding van client-/ toepassingsbeleid gerichte en intraknooppuntcommunicatie communicatie niet wordt geopend elke mogelijkheden om één verkeer stream prioriteit boven de andere. In plaats daarvan blijft de scheiding tussen een meting van de beveiliging in de intra-node-communicatie van de scale-out-configuraties afscherming.  

>[!IMPORTANT]
>SAP raadt het scheiden van netwerkverkeer naar de toepassing/client-zijde en intraknooppuntcommunicatie verkeer zoals beschreven in dit document. Daarom een architectuur plaatsen in plaats van zoals wordt weergegeven in de laatste afbeeldingen wordt sterk aanbevolen.
>

Vanaf een netwerk standpunt de minimale vereiste netwerkarchitectuur zou er als volgt uitzien:

![Basisprincipes van de scale-out van één knooppunt](media/hana-vm-operations/scale-out-networking-overview.PNG)

De limieten ondersteund tot nu toe zijn 15 werknemer als aanvulling op het één hoofdknooppunt.

Vanuit een opslag-oogpunt van de opslagarchitectuur zou er als volgt uitzien:


![Basisprincipes van de scale-out van één knooppunt](media/hana-vm-operations/scale-out-storage-overview.PNG)

De **/hana/gedeelde** volume bevindt zich op de maximaal beschikbare configuratie van de NFS-share. Terwijl alle andere stations zijn 'lokaal' gekoppeld aan de afzonderlijke virtuele machines. 

### <a name="highly-available-nfs-share"></a>Maximaal beschikbare NFS-share
De maximaal beschikbare NFS-cluster tot nu toe werkt samen met SUSE Linux alleen. Het document [hoge beschikbaarheid voor NFS op Azure VM's in SUSE Linux Enterprise Server](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs) wordt beschreven hoe u jenkins instelt. Als u het NFS-cluster niet met andere HANA configuraties buiten het azure VNet met de SAP HANA-instanties delen, kunt u dit in hetzelfde VNet installeren. Installeer het in een eigen subnet en zorg ervoor dat het subnet toegang hebben tot willekeurige niet al het verkeer. In plaats daarvan wilt u het verkeer op dat subnet naar het IP-adressen van de virtuele machine die het verkeer om te worden uitgevoerd beperken **/hana/gedeelde** volume.

Met betrekking tot de vNIC van een HANA scale-out-virtuele machine die moet routeren de **/hana/gedeelde** verkeer, de aanbevelingen zijn:

- Sinds het verkeer naar **/hana/gedeelde** is normaal, rondsturen via de vNIC die is toegewezen aan de clientnetwerk in de minimale configuratie
- Uiteindelijk, voor het verkeer naar **/hana/gedeelde**, het implementeren van een derde subnet in het VNet die u implementeert de SAP HANA uitbreidbare configuratie en het toewijzen van een derde vNIC die wordt gehost in dat subnet. Gebruik de derde vNIC en de bijbehorende IP-adres voor het verkeer naar de NFS-share. Vervolgens kunt u afzonderlijke toegang en routeringsregels toepassen.

>[!IMPORTANT]
>Netwerkverkeer tussen de VM's waarop SAP HANA op een uitbreidbare manier geïmplementeerd en de maximaal beschikbare NFS kan onder geen enkele omstandigheid worden gerouteerd via een [NVA](https://azure.microsoft.com/solutions/network-appliances/) of vergelijkbare virtuele apparaten. Terwijl Azure nsg's geen dergelijke apparaten zijn. Controleer uw routeringsregels om ervoor te zorgen dat de NVA's of vergelijkbare virtuele apparaten zijn detoured wanneer toegang krijgen tot de maximaal beschikbare NFS-share van de virtuele machines met SAP HANA.
> 

Als u delen van de maximaal beschikbare NFS-cluster tussen SAP HANA-configuraties wilt, verplaatst u alle die HANA-configuraties in hetzelfde VNet. 
 

### <a name="installing-sap-hana-scale-out-n-azure"></a>SAP HANA scale-out n Azure installeren
U installeert een scale-out SAP-configuratie, die u wilt uitvoeren van ruwe stappen:

- Implementatie van nieuwe of aanpassen van de nieuwe Azure-VNet-infrastructuur
- Implementatie van de nieuwe VM's met behulp van Azure Managed Premium Storage-volumes
- Implementatie van een nieuwe of een bestaande maximaal beschikbare NFS-cluster aanpassen
- Aanpassing van de routering van om ervoor te zorgen dat bijvoorbeeld intraknooppuntcommunicatie communicatie tussen VM's wordt niet doorgestuurd via een [NVA](https://azure.microsoft.com/solutions/network-appliances/). Hetzelfde geldt voor verkeer tussen de virtuele machines en het maximaal beschikbare NFS-cluster.
- Installeer het hoofdknooppunt van SAP HANA.
- Parameters voor de configuratie van het hoofdknooppunt van SAP HANA aanpassen
- Ga door met de installatie van de SAP HANA worker-knooppunten

#### <a name="installation-of-sap-hana-in-scale-out-configuration"></a>Installatie van SAP HANA in scale-out-configuratie
Als uw Azure-VM-infrastructuur is geïmplementeerd en alle andere voorbereidingen klaar bent, moet u de scale-out-configuraties van SAP HANA installeren in de volgende stappen uit:

- Installeren van het hoofdknooppunt van SAP HANA op basis van de SAP-documentatie
- **Na de installatie, moet u het bestand global.ini wijzigen en toevoegen van de parameter ' basepath_shared = Nee ' aan de global.ini**. Met deze parameter kunt u SAP HANA om uit te voeren in scale-out zonder 'gedeelde' **/hana/gegevens** en **/hana/log** volumes tussen de knooppunten. Details worden gedocumenteerd in [SAP Opmerking #2080991](https://launchpad.support.sap.com/#/notes/2080991).
- Na het wijzigen van de parameter global.ini, de SAP HANA-exemplaar opnieuw opstarten
- Toevoegen van extra werkknooppunten. Zie ook <https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.00/en-US/0d9fe701e2214e98ad4f8721f6558c34.html>. Geef het interne netwerk voor SAP HANA de communicatie tussen knooppunten tijdens de installatie van of later gebruikt, bijvoorbeeld, de lokale hdblcm. Zie voor meer documentatie gedetailleerde, ook [SAP Opmerking #2183363](https://launchpad.support.sap.com/#/notes/2183363). 

Na deze routine setup gaat de configuratie van de scale-out die u hebt geïnstalleerd voor het gebruik van niet-gedeelde schijven voor het werken met **/hana/gegevens** en **/hana/log**. Terwijl de **/hana/gedeelde** volume worden geplaatst op de maximaal beschikbare gaat de NFS-share.


## <a name="sap-hana-dynamic-tiering-20-for-azure-virtual-machines"></a>SAP HANA dynamische Opslaglagen 2.0 voor virtuele machines van Azure

Naast de certificeringen die SAP HANA op Azure uit de M-serie VM's, ook SAP HANA dynamische Opslaglagen 2.0 wordt ondersteund op Microsoft Azure (Zie de documentatie van SAP HANA dynamische Opslaglagen koppelingen verder naar beneden). Hoewel er geen verschil in het product installeert of het besturingssysteem, bijvoorbeeld zijn via SAP HANA Cockpit binnen een Azure-Machine, er enkele belangrijke items, die verplicht voor officiële ondersteuning op Azure zijn. Deze belangrijke punten worden hieronder beschreven. In het artikel is de afkorting van "IT 2.0" moet worden gebruikt in plaats van de volledige naam dynamische Opslaglagen 2.0 gaan.

SAP HANA dynamische Opslaglagen 2.0 wordt niet ondersteund door SAP BW of S4HANA. Belangrijkste use-cases zijn nu systeemeigen HANA-toepassingen.


### <a name="overview"></a>Overzicht

De volgende afbeelding geeft een overzicht met betrekking tot DT 2.0-ondersteuning op Microsoft Azure. Er is een set verplichte vereisten dat moet worden gevolgd om te voldoen aan de officiële certificeringen:

- DT 2.0 moet worden geïnstalleerd op een specifieke Azure-VM. Het kan niet worden uitgevoerd op dezelfde virtuele machine waarop SAP HANA wordt uitgevoerd
- SAP HANA en DT 2.0-VM's moeten worden geïmplementeerd in hetzelfde Azure-Vnet
- De SAP HANA en DT 2.0-VM's moeten worden geïmplementeerd met Azure versneld netwerkondersteuning ingeschakeld
- Opslagtype voor de DT 2.0 virtuele machines moet Azure Premium Storage
- Meerdere Azure-schijven moeten worden gekoppeld aan de DT 2.0-VM
- Dit is vereist voor het maken van een software-raid / striped volumes (ofwel via een lvm of mdadm) maakt gebruik van gesegmenteerd te verdelen over de Azure-schijven

Meer informatie gaat in de volgende secties worden uitgelegd.

![Overzicht van de SAP HANA DT 2.0-architectuur](media/hana-vm-operations/hana-dt-20.PNG)



### <a name="dedicated-azure-vm-for-sap-hana-dt-20"></a>Toegewezen virtuele machines van Azure voor SAP HANA DT 2.0

Op Azure IaaS, wordt DT 2.0 alleen ondersteund op een specifieke virtuele machine. Het is niet toegestaan om uit te voeren DT 2.0 op de dezelfde Azure-VM waarop de HANA-instantie wordt uitgevoerd. In eerste instantie twee VM-typen kunnen worden gebruikt voor het uitvoeren van SAP HANA DT 2.0:

- M64-32ms 
- E32sv3 

Zie de beschrijving van de VM-type [hier](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory)

Gezien het uitgangspunt van DT 2.0, die over offloading van 'warme' gegevens om op te slaan kosten is het verstandig om het gebruik van de bijbehorende VM-grootten. Er is geen strikte regel al met betrekking tot de mogelijke combinaties. Dat hangt ervan af op de workload van een specifieke klant.

Aanbevolen configuraties zijn:

| SAP HANA VM-type | DT 2.0 VM-type |
| --- | --- | 
| M128ms | M64-32ms |
| M128s | M64-32ms |
| M64ms | E32sv3 |
| M64s | E32sv3 |


Alle combinaties van SAP HANA-gecertificeerde M-serie VM's met ondersteunde DT 2.0-VM's (M64 32ms en E32sv3) zijn mogelijk.


### <a name="azure-networking-and-sap-hana-dt-20"></a>Azure-netwerken en SAP HANA DT 2.0

DT 2.0 installeren op een specifieke virtuele machine is netwerkdoorvoer tussen de DT 2.0 virtuele machine en de SAP HANA-VM van 10 Gb minimaal vereist. Daarom is het verplicht te plaatsen van alle virtuele machines binnen hetzelfde Azure-Vnet en het inschakelen van Azure-versnelde netwerken.

Meer informatie over Azure-versnelde netwerken [hier](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli)

### <a name="vm-storage-for-sap-hana-dt-20"></a>VM-opslag voor SAP HANA DT 2.0

Op basis van DT 2.0 richtlijnen voor best practices moet de schijf-i/o-doorvoer ten minste 50 MB per seconde per fysieke kern. De specificaties voor de twee typen Azure VM's bekijkt, worden die ondersteund voor DT 2.0 is de maximale schijf-i/o-doorvoerlimiet voor het uiterlijk van de virtuele machine, zoals:

- E32sv3    :   768 MB per seconde (zonder caching) wat betekent een ratio van 48 MB per seconde per fysieke kern dat
- M64-32MS:  1000 MB per seconde (zonder caching) wat betekent een ratio van 62,5 MB per seconde per fysieke kern dat

Dit is vereist voor meerdere Azure-schijven koppelen aan de DT 2.0 virtuele machine en het maken van een software-raid (gesegmenteerd te verdelen) op besturingssysteemniveau te bereiken van het maximum aantal schijfdoorvoer per virtuele machine. Een enkel Azure-schijf opgeven niet de doorvoer voor het bereiken van de maximale limiet voor de virtuele machine in dit opzicht. Azure Premium storage is verplicht om uit te voeren DT 2.0. 

- Meer informatie over beschikbare Azure-schijftypes vindt [hier](../../windows/disks-types.md)
- Meer informatie over het maken van software-raid via mdadm vindt [hier](https://docs.microsoft.com/azure/virtual-machines/linux/configure-raid)
- Meer informatie over het configureren van LVM voor het maken van een striped volume voor maximale doorvoer vindt [hier](https://docs.microsoft.com/azure/virtual-machines/linux/configure-lvm)

Afhankelijk van de vereisten voor de grootte zijn er verschillende opties voor het bereiken van de maximale doorvoer van een virtuele machine. Hier vindt u mogelijk gegevens volume schijfconfiguraties voor elk type DT 2.0 VM om de bovengrens voor doorvoer van virtuele machine. De E32sv3 virtuele machine moet worden beschouwd als een post-niveau voor kleinere workloads. In het geval uit te schakelen is niet snel genoeg kan het nodig zijn om het formaat van de virtuele machine M64 32ms aan.
Als de VM M64 32ms veel geheugen heeft, kan de i/o-belasting de limiet voor lees-intensieve workloads met name niet bereiken. Daarom kunnen minder schijven in de stripeset met voldoende zijn afhankelijk van de workload van een specifieke klant zijn. Maar als u wilt worden voor de zekerheid van de schijf volgende configuraties zijn gekozen omdat de maximale doorvoer garanderen:


| VM-SKU | Configuratie van de schijf 1 | Configuratie van de schijf 2 | Schijf-configuratie 3 | Schijf Config 4 | Schijf Config 5 | 
| ---- | ---- | ---- | ---- | ---- | ---- | 
| M64-32ms | 4 x P50 -> 16 TB | 4 x P40 -> 8 TB | 5 x P30 -> 5 TB | 7 x P20 -> 3.5 TB | 8 x P15 -> 2 TB | 
| E32sv3 | 3 x P50 -> 12 TB | 3 x P40 -> 6 TB | 4 x P30 -> 4 TB | 5 x P20 -> 2.5 TB | 6 x P15 -> 1.5 TB | 


Met name als de werkbelasting Lees-intensieve kan het i/o-prestaties om in te schakelen op Azure-host cache 'alleen-lezen-zoals aanbevolen voor de gegevensvolumes van database-software te verbeteren. Dat voor de transactie log schijfcache voor Azure-host moet 'none'. 

Met betrekking tot de grootte van het logboekvolume is een aanbevolen beginpunt een heuristiek van 15% van de grootte van de gegevens. Het maken van het logboekvolume kan worden bereikt met behulp van verschillende Azure-schijftypes, afhankelijk van de vereisten voor kosten en doorvoer. Volume hoge i/o-doorvoer is vereist voor het logboek.  Typ in het geval met behulp van de virtuele machine M64-32ms het sterk aanbevolen wordt om in te schakelen [Write Accelerator](https://docs.microsoft.com/azure/virtual-machines/linux/how-to-enable-write-accelerator). Azure Write Accelerator biedt schrijflatentie van optimale schijf voor het transactielogboek (alleen beschikbaar voor M-serie). Er zijn enkele items rekening houden met al, zoals het maximum aantal schijven per VM-type. Meer informatie over Write Accelerator vindt [hier](https://docs.microsoft.com/azure/virtual-machines/windows/how-to-enable-write-accelerator)


Hier volgen enkele voorbeelden over het formaat van het logboekvolume:

| het volume grootte en de schijf-gegevenstype | Typ config 1 logboekvolume en schijf | Typ configuratie 2 logboekvolume en schijf |
| --- | --- | --- |
| 4 x P50 -> 16 TB | 5 x P20 -> 2.5 TB | 3 x P30 -> 3 TB |
| 6 x P15 -> 1.5 TB | 4 x P6 -> 256 GB | 1 x P15 -> 256 GB |


Net als voor SAP HANA-scale-out heeft de map /hana/shared om te worden gedeeld tussen de SAP HANA en de DT 2.0 VM's. De architectuur van dezelfde als voor SAP HANA met behulp van scale-out toegewezen virtuele machines die fungeren als een maximaal beschikbare NFS-server wordt aanbevolen. Om te voorzien van een gedeeld volume back-up, kan het ontwerp van de identieke worden gebruikt. Maar het is aan de klant als HA nodig zou zijn of alleen een specifieke virtuele machine met voldoende opslagcapaciteit gebruiken om te fungeren als een back-upserver voldoende is.



### <a name="links-to-dt-20-documentation"></a>Koppelingen naar DT 2.0-documentatie 

- [Dynamische Opslaglagen voor SAP HANA-installatie- en update voor](https://help.sap.com/viewer/88f82e0d010e4da1bc8963f18346f46e/2.0.03/en-US)
- [SAP HANA dynamische lagen zelfstudies en bronnen](https://help.sap.com/viewer/fb9c3779f9d1412b8de6dd0788fa167b/2.0.03/en-US)
- [SAP HANA Dynamic Tiering PoC](https://blogs.sap.com/2017/12/08/sap-hana-dynamic-tiering-delivering-on-low-tco-with-impressive-performance/)
- [SAP HANA 2.0 SP's 02 dynamische cloudlagen verbeteringen](https://blogs.sap.com/2017/07/31/sap-hana-2.0-sps-02-dynamic-tiering-enhancements/)




## <a name="operations-for-deploying-sap-hana-on-azure-vms"></a>Bewerkingen voor het implementeren van SAP HANA op Azure Virtual machines
De volgende secties beschrijven enkele bewerkingen weergegeven die betrekking hebben op de implementatie van systemen voor SAP HANA op Azure Virtual machines.

### <a name="back-up-and-restore-operations-on-azure-vms"></a>Back-up en herstelbewerkingen op Azure Virtual machines
De volgende documenten wordt beschreven hoe u back-up en herstellen van uw SAP HANA-implementatie:

- [Overzicht van back-ups van SAP HANA](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide)
- [Back-up van SAP HANA op bestandsniveau](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-file-level)
- [Momentopname van de SAP HANA opslag-benchmark](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-storage-snapshots)


### <a name="start-and-restart-vms-that-contain-sap-hana"></a>Starten en opnieuw opstarten van virtuele machines met SAP HANA
Een prominente functie van de openbare Azure-cloud is dat u alleen voor uw computeromgeving minuten betaalt. Bijvoorbeeld, wanneer u een virtuele machine waarop SAP HANA wordt afgesloten, wordt u gefactureerd voor de kosten voor opslag gedurende die tijd. Een andere functie is beschikbaar als u statische IP-adressen voor uw VM's in uw initiële implementatie opgeven. Wanneer u een virtuele machine met SAP HANA opnieuw start, wordt de virtuele machine wordt opnieuw gestart met de voorgaande IP-adressen. 


### <a name="use-saprouter-for-sap-remote-support"></a>SAProuter gebruiken voor externe ondersteuning voor SAP
Als u een site-naar-site-verbinding tussen uw on-premises locaties en Azure, en u SAP-onderdelen uitvoert, bent u waarschijnlijk al SAProuter uitgevoerd. In dit geval voert u de volgende artikelen voor ondersteuning voor externe:

- Het privé- en statische IP-adres van de virtuele machine die als host fungeert SAP HANA in de configuratie van SAProuter onderhouden.
- Configureer de NSG van het subnet dat als host fungeert voor de HANA VM voor het toestaan van verkeer via TCP/IP-poort 3299.

Als u verbinding met Azure via het internet maakt en u geen een SAP-router voor de virtuele machine met SAP HANA hebt, moet u het onderdeel te installeren. Installeer SAProuter in een afzonderlijke virtuele machine in het beheersubnet. De volgende afbeelding toont een ruwe schema voor het implementeren van SAP HANA met SAProuter en zonder een site-naar-site-verbinding:

![Implementatie-schema voor SAP HANA ruwe zonder dat u een site-naar-site-verbinding en SAProuter](media/hana-vm-operations/hana-simple-networking3.PNG)

Zorg ervoor dat SAProuter installeren in een afzonderlijke virtuele machine en niet in uw Jumpbox-VM. De afzonderlijke virtuele machine moet een statisch IP-adres hebben. Neem contact op met SAP voor een IP-adres voor uw SAProuter verbinding met de SAProuter die wordt gehost door SAP. (De SAProuter die wordt gehost door SAP is het equivalent van de SAProuter-instantie die u op de virtuele machine installeert.) Het IP-adres van het SAP gebruiken om te configureren van uw SAProuter-exemplaar. In de configuratie-instellingen is het alleen de benodigde poort TCP-poort 3299.

Zie voor meer informatie over het instellen en onderhouden van ondersteuning voor externe verbindingen via SAProuter de [SAP-documentatie](https://support.sap.com/en/tools/connectivity-tools/remote-support.html).

### <a name="high-availability-with-sap-hana-on-azure-native-vms"></a>Hoge beschikbaarheid met SAP HANA op Azure systeemeigen VM 's
Als u SUSE Linux Enterprise Server voor SAP-toepassingen 12 SP1 of hoger uitvoert, kunt u een cluster Pacemaker kunt maken met stonith instellen-apparaten. U kunt de apparaten gebruiken voor het instellen van een SAP HANA-configuratie die gebruikmaakt van synchrone replicatie met HANA-Systeemreplicatie en automatische failover. Zie voor meer informatie over de installatieprocedure [hoge beschikbaarheid van SAP HANA-handleiding voor Azure virtual machines](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-overview).
