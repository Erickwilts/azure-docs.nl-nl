---
title: Ondersteuningsmatrix voor herstel na noodgevallen van virtuele VMware-machines en fysieke servers naar Azure met Azure Site Recovery | Microsoft Docs
description: Geeft een overzicht van de ondersteunde besturingssystemen en onderdelen voor herstel na noodgevallen van virtuele VMware-machines en fysieke server naar Azure met Azure Site Recovery.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
services: site-recovery
ms.topic: conceptual
ms.date: 05/10/2019
ms.author: raynew
ms.openlocfilehash: 2d1999077f6315658dbfd69473ddf5561bd76e0b
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2019
ms.locfileid: "65540594"
---
# <a name="support-matrix-for-disaster-recovery--of-vmware-vms-and-physical-servers-to-azure"></a>Ondersteuningsmatrix voor herstel na noodgevallen van virtuele VMware-machines en fysieke servers naar Azure

In dit artikel bevat een overzicht van ondersteunde onderdelen en -instellingen voor herstel na noodgevallen van virtuele VMware-machines naar Azure met behulp van [Azure Site Recovery](site-recovery-overview.md).

Als u wilt gaan met Azure Site Recovery en het eenvoudigste scenario, gaat u naar onze [zelfstudies](tutorial-prepare-azure.md). U kunt meer informatie over Azure Site Recovery-architectuur [hier](vmware-azure-architecture.md).

## <a name="deployment-scenario"></a>Implementatiescenario

**Scenario** | **Details**
--- | ---
Herstel na noodgevallen van virtuele VMware-machines | Replicatie van on-premises VMware-machines naar Azure. U kunt dit scenario in Azure portal of met behulp van implementeren [PowerShell](vmware-azure-disaster-recovery-powershell.md).
Herstel na noodgeval voor fysieke servers | Replicatie van on-premises Windows/Linux-fysieke servers naar Azure. U kunt dit scenario in Azure portal implementeren.

## <a name="on-premises-virtualization-servers"></a>On-premises virtualisatieservers

**Server** | **Vereisten** | **Details**
--- | --- | ---
VMware | vCenter Server 6.7, 6.5, 6.0 of 5.5 of vSphere 6.7, 6.5, 6.0 of 5.5 | U wordt aangeraden dat u een vCenter-server.<br/><br/> Het is raadzaam dat vSphere-hosts en vCenter-servers bevinden zich in hetzelfde netwerk bevinden als de processerver. Wordt standaard de serveronderdelen proces uitgevoerd op de configuratieserver, dus dit is het netwerk waarin u de configuratieserver hebt ingesteld, tenzij u een speciaal toegewezen proces-server instellen.
Fysiek | N/A

## <a name="site-recovery-configuration-server"></a>Site Recovery-configuratieserver

De configuratieserver is een on-premises computer met Site Recovery-onderdelen, met inbegrip van de configuratieserver, processerver en hoofddoelserver. Voor VMware-replicatie instellen u de configuratieserver met alle vereisten, met behulp van een OVF-sjabloon om een VMware-VM te maken. Voor replicatie van fysieke server instellen u de configuratie van server-machine handmatig.

**Onderdeel** | **Vereisten**
--- |---
CPU-kernen | 8
RAM | 16 GB
Aantal schijven | 3-schijven<br/><br/> Schijven bevatten de OS-schijf, cacheschijf proces en bewaarstation voor failback.
Vrije schijfruimte | 600 GB aan ruimte vereist voor de cache van de processerver.
Vrije schijfruimte | 600 GB aan ruimte vereist voor het bewaarstation.
Besturingssysteem  | Windows Server 2012 R2 of WindowsServer 2016 |
Landinstelling van het besturingssysteem | Engels (en-us)
PowerCLI | [PowerCLI 6.0](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1 "PowerCLI 6.0") is niet vereist voor de configuratieserver met versies van [9.14](https://support.microsoft.com/help/4091311/update-rollup-23-for-azure-site-recovery).
Windows Server-functies | Niet inschakelen: <br/> - Active Directory Domain Services <br/>- Internet Information Services <br/> - Hyper-V |
Groepsbeleid| Niet inschakelen: <br/> -Toegang tot de opdrachtprompt voorkomen. <br/> -Toegang tot registerbewerkingsprogramma's voorkomen. <br/> -Logica vertrouwen voor bestandsbijlagen. <br/> -Uitvoering van Script inschakelen. <br/> [Meer informatie](https://technet.microsoft.com/library/gg176671(v=ws.10).aspx)|
IIS | Zorg ervoor dat u:<br/><br/> -Geen een bestaande standaardwebsite <br/> -Inschakelen [anonieme verificatie](https://technet.microsoft.com/library/cc731244(v=ws.10).aspx) <br/> -Inschakelen [FastCGI](https://technet.microsoft.com/library/cc753077(v=ws.10).aspx) instelling  <br/> -Bestaande website /-app niet hebt luisteren op poort 443<br/>
Type NIC | VMXNET3 (indien geïmplementeerd als een VMware-VM)
Type IP-adres | Statisch
Poorten | 443 gebruikt voor de orchestration-besturingselement)<br/>9443 gebruikt voor het gegevenstransport

## <a name="replicated-machines"></a>Gerepliceerde machines

Site Recovery biedt ondersteuning voor replicatie van alle werkbelasting die wordt uitgevoerd op een ondersteunde machine.

**Onderdeel** | **Details**
--- | ---
Instellingen van de computer | Machines die worden gerepliceerd naar Azure moeten voldoen aan [Azure-vereisten](#azure-vm-requirements).
Machine-werkbelasting | Site Recovery biedt ondersteuning voor replicatie van elke workload (bijvoorbeeld Active Directory, SQL server, enzovoort) die worden uitgevoerd op een ondersteunde machine. [Meer informatie](https://aka.ms/asr_workload).
Windows-besturingssysteem | Windows Server 2019 (van [9.22 versies](service-updates-how-to.md#links-to-currently-supported-update-rollups)), 64-bits Windows Server 2016 (Server Core, Server met Bureaubladervaring), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 met op minimaal SP1. </br> Uit [9.24 versies](https://support.microsoft.com/en-in/help/4503156), 64-bits Windows 10, Windows 8.1-64-bits, 64-bits Windows 8, 64-bits Windows 7 (Windows 7 RTM wordt niet ondersteund)</br>  [Windows Server 2008 met op minste SP2 - 32-bits en 64-bits](migrate-tutorial-windows-server-2008.md) (alleen voor de migratie). </br></br> Windows 2016 Nano Server wordt niet ondersteund.
Architectuur van de Linux-besturingssysteem | Alleen 64-bits systeem wordt ondersteund. 32-bits systeem wordt niet ondersteund.
Linux-besturingssysteem | Red Hat Enterprise Linux: 5.2-5,11<b>\*\*</b>, 6.1-6.10<b>\*\*</b>, 7.0-7,6 <br/><br/>CentOS: 5.2-5,11<b>\*\*</b>, 6.1-6.10<b>\*\*</b>, 7.0-7,6 <br/><br/>Ubuntu 14.04 LTS server [(kernel-versies ondersteund)](#ubuntu-kernel-versions)<br/><br/>Ubuntu 16.04 LTS server [(kernel-versies ondersteund)](#ubuntu-kernel-versions)<br/><br/>Debian 7/Debian 8 [(kernel-versies ondersteund)](#debian-kernel-versions)<br/><br/>SUSE Linux Enterprise Server 12 SP1, SP2, SP3, SP4 [(kernel-versies ondersteund)](#suse-linux-enterprise-server-12-supported-kernel-versions)<br/><br/>SUSE Linux Enterprise Server 11 SP3<b>\*\*</b>, SUSE Linux Enterprise Server 11 SP4 * </br></br>Oracle Linux 6.4, 6.5, 6.6, 6.7, 6,8, 6,9, 6.10, 7.0, 7.1, 7.2, 7.3, 7.4, 7.5, 7.6 met Red Hat compatibele kernel of Unbreakable Enterprise Kernel versie 3, 4 en 5 (UEK3, UEK4, UEK5) <br/><br/></br>-Upgrade uitvoeren voor gerepliceerde machines van SUSE Linux Enterprise Server 11 SP3 naar SP4 wordt niet ondersteund. Als u wilt bijwerken, replicatie uitschakelen en inschakelen na de upgrade opnieuw.</br></br> - [Meer informatie](https://support.microsoft.com/help/2941892/support-for-linux-and-open-source-technology-in-azure) over ondersteuning voor Linux en open source-technologie in Azure. Site Recovery deelt failover voor Linux-servers uitvoeren in Azure. Linux-leveranciers kunnen echter ondersteuning om alleen distributie-versies die nog niet hebt bereikt einde van de levenscyclus te beperken.<br/><br/> -In Linux-distributies, worden alleen de voorraad kernels die deel van de release-distributiepunt secundaire versie/update uitmaken ondersteund.<br/><br/> -Upgrade uitvoeren voor beveiligde machines over belangrijke Linux distributie versies wordt niet ondersteund. Als u wilt bijwerken, schakelt u replicatie uit, werk het besturingssysteem en schakelt u de replicatie opnieuw.<br/><br/> -Servers met Red Hat Enterprise Linux 5.2-5,11 of CentOS 5.2-5,11 moeten beschikken over de [onderdelen van Linux Integration Services (LIS)](https://www.microsoft.com/download/details.aspx?id=55106) geïnstalleerd voor de machines om op te starten in Azure.


### <a name="ubuntu-kernel-versions"></a>Ubuntu-kernel-versies


**Ondersteunde versie** | **Azure Site Recovery Mobility Service versie** | **Kernelversie** |
--- | --- | --- |
14.04 TNS | [9,24] [9,24 UR] | 3.13.0-24-Generic naar 3.13.0-167-generic,<br/>3.16.0-25-Generic naar 3.16.0-77-generic,<br/>3.19.0-18-Generic naar 3.19.0-80-generic,<br/>4.2.0-18-Generic naar 4.2.0-42-generic,<br/>4.4.0-21-Generic naar 4.4.0-143-generic,<br/>4.15.0-1023-Azure naar 4.15.0-1040-azure |
14.04 TNS | [9.23][9.23 UR] | 3.13.0-24-Generic naar 3.13.0-165-generic,<br/>3.16.0-25-Generic naar 3.16.0-77-generic,<br/>3.19.0-18-Generic naar 3.19.0-80-generic,<br/>4.2.0-18-Generic naar 4.2.0-42-generic,<br/>4.4.0-21-Generic naar 4.4.0-142-generic,<br/>4.15.0-1023-Azure naar 4.15.0-1037-azure |
14.04 TNS | [9.22][9.22 UR] | 3.13.0-24-Generic naar 3.13.0-164-generic,<br/>3.16.0-25-Generic naar 3.16.0-77-generic,<br/>3.19.0-18-Generic naar 3.19.0-80-generic,<br/>4.2.0-18-Generic naar 4.2.0-42-generic,<br/>4.4.0-21-Generic naar 4.4.0-140-generic,<br/>4.15.0-1023-Azure naar 4.15.0-1036-azure |
14.04 TNS | [9.21][9.21 UR] | 3.13.0-24-Generic naar 3.13.0-163-generic,<br/>3.16.0-25-Generic naar 3.16.0-77-generic,<br/>3.19.0-18-Generic naar 3.19.0-80-generic,<br/>4.2.0-18-Generic naar 4.2.0-42-generic,<br/>4.4.0-21-Generic naar 4.4.0-140-generic,<br/>4.15.0-1023-Azure naar 4.15.0-1035-azure |
|||
16.04 LTS | [9.23] [9,24 UR] | 4.4.0-21-Generic naar 4.4.0-143-generic,<br/>4.8.0-34-Generic naar 4.8.0-58-generic,<br/>4.10.0-14-algemeen tot 4.10.0-42-generic,<br/>4.11.0-13-Generic naar 4.11.0-14-generic,<br/>4.13.0-16-Generic naar 4.13.0-45-generic,<br/>4.15.0-13-Generic naar 4.15.0-46-generic<br/>4.11.0-1009-Azure naar 4.11.0-1018-azure,<br/>4.13.0-1005-Azure naar 4.13.0-1018-azure <br/>4.15.0-1012-Azure naar 4.15.0-1040-azure|
16.04 LTS | [9.23][9.23 UR] | 4.4.0-21-Generic naar 4.4.0-142-generic,<br/>4.8.0-34-Generic naar 4.8.0-58-generic,<br/>4.10.0-14-algemeen tot 4.10.0-42-generic,<br/>4.11.0-13-Generic naar 4.11.0-14-generic,<br/>4.13.0-16-Generic naar 4.13.0-45-generic,<br/>4.15.0-13-Generic naar 4.15.0-45-generic<br/>4.11.0-1009-Azure naar 4.11.0-1016-azure,<br/>4.13.0-1005-Azure naar 4.13.0-1018-azure <br/>4.15.0-1012-Azure naar 4.15.0-1037-azure|
16.04 LTS | [9.22][9.22 UR] | 4.4.0-21-Generic naar 4.4.0-140-generic,<br/>4.8.0-34-Generic naar 4.8.0-58-generic,<br/>4.10.0-14-algemeen tot 4.10.0-42-generic,<br/>4.11.0-13-Generic naar 4.11.0-14-generic,<br/>4.13.0-16-Generic naar 4.13.0-45-generic,<br/>4.15.0-13-Generic naar 4.15.0-43-generic<br/>4.11.0-1009-Azure naar 4.11.0-1016-azure,<br/>4.13.0-1005-Azure naar 4.13.0-1018-azure <br/>4.15.0-1012-Azure naar 4.15.0-1036-azure|
16.04 LTS | [9.21][9.21 UR] | 4.4.0-21-Generic naar 4.4.0-140-generic,<br/>4.8.0-34-Generic naar 4.8.0-58-generic,<br/>4.10.0-14-algemeen tot 4.10.0-42-generic,<br/>4.11.0-13-Generic naar 4.11.0-14-generic,<br/>4.13.0-16-Generic naar 4.13.0-45-generic,<br/>4.15.0-13-Generic naar 4.15.0-42-generic<br/>4.11.0-1009-Azure naar 4.11.0-1016-azure,<br/>4.13.0-1005-Azure naar 4.13.0-1018-azure <br/>4.15.0-1012-Azure naar 4.15.0-1035-azure|
16.04 LTS | [9.20][9.20 UR] | 4.4.0-21-Generic naar 4.4.0-138-generic,<br/>4.8.0-34-Generic naar 4.8.0-58-generic,<br/>4.10.0-14-algemeen tot 4.10.0-42-generic,<br/>4.11.0-13-Generic naar 4.11.0-14-generic,<br/>4.13.0-16-Generic naar 4.13.0-45-generic,<br/>4.15.0-13-Generic naar 4.15.0-38-generic<br/>4.11.0-1009-Azure naar 4.11.0-1016-azure,<br/>4.13.0-1005-Azure naar 4.13.0-1018-azure <br/>4.15.0-1012-Azure naar 4.15.0-1025-azure|

### <a name="debian-kernel-versions"></a>Debian kernelversies


**Ondersteunde versie** | **Azure Site Recovery Mobility Service versie** | **Kernelversie** |
--- | --- | --- |
Debian 7 | [9.21][9.21 UR], [9.22][9.22 UR],[9.23][9.23 UR], [9,24] [9.24 UR]| 3.2.0-4-AMD64 naar 3.2.0-6-amd64, 3.16.0-0.bpo.4-amd64 |
|||
Debian 8 | [9.21][9.21 UR],[9.22][9.22 UR],[9.23][9.23 UR], [9,24] [9.24 UR] | 3.16.0-4-amd64 to 3.16.0-7-amd64, 4.9.0-0.bpo.4-amd64 to 4.9.0-0.bpo.8-amd64 |


### <a name="suse-linux-enterprise-server-12-supported-kernel-versions"></a>SUSE Linux Enterprise Server 12 ondersteunde versies van de kernel

**Release** | **De versie van de Mobility-service** | **Kernelversie** |
--- | --- | --- |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3 en SP4) | [9,24] [9,24 UR] | SP1 3.12.49-11-default naar 3.12.74-60.64.40-default</br></br> SP1(LTSS) 3.12.74-60.64.45-default naar 3.12.74-60.64.107-default</br></br> SP2 4.4.21-69-default naar 4.4.120-92.70-default</br></br>SP2(LTSS) 4.4.121-92.73-default naar 4.4.121-92.101-default</br></br>SP3 4.4.73-5-default naar 4.4.175-94.79-default</br></br>SP4 4.12.14-94.41-default naar 4.12.14-95.6-default |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3 en SP4) | [9.23][9.23 UR] | SP1 3.12.49-11-default naar 3.12.74-60.64.40-default</br></br> SP1(LTSS) 3.12.74-60.64.45-default naar 3.12.74-60.64.107-default</br></br> SP2 4.4.21-69-default naar 4.4.120-92.70-default</br></br>SP2(LTSS) 4.4.121-92.73-default naar 4.4.121-92.101-default</br></br>SP3 4.4.73-5-default naar 4.4.162-94.69-default</br></br>SP4 4.12.14-94.41-default naar 4.12.14-95.6-default |
SUSE Linux Enterprise Server 12 (SP3 SP1, SP2) | [9.22][9.22 UR] | SP1 3.12.49-11-default naar 3.12.74-60.64.40-default</br></br> SP1(LTSS) 3.12.74-60.64.45-default naar 3.12.74-60.64.107-default</br></br> SP2 4.4.21-69-default naar 4.4.120-92.70-default</br></br>SP2(LTSS) 4.4.121-92.73-default naar 4.4.121-92.98-default</br></br>SP3 4.4.73-5-default naar 4.4.162-94.72-default |
SUSE Linux Enterprise Server 12 (SP3 SP1, SP2) | [9.21][9.21 UR] | SP1 3.12.49-11-default naar 3.12.74-60.64.40-default</br></br> SP1(LTSS) 3.12.74-60.64.45-default naar 3.12.74-60.64.107-default</br></br> SP2 4.4.21-69-default naar 4.4.120-92.70-default</br></br>SP2(LTSS) 4.4.121-92.73-default naar 4.4.121-92.98-default</br></br>SP3 4.4.73-5-default naar 4.4.156-94.72-default |


## <a name="linux-file-systemsguest-storage"></a>Linux-systemen/Gast bestandsopslag

**Onderdeel** | **Ondersteund**
--- | ---
Bestandssystemen | ext3, ext4, XFS
Volume manager | Voordat u [9.20 versie](https://support.microsoft.com/en-in/help/4478871/update-rollup-31-for-azure-site-recovery), <br/> 1. LVM wordt ondersteund. <br/> 2. bevinden op LVM-volume wordt niet ondersteund. <br/> 3. Meerdere besturingssysteemschijven worden niet ondersteund.<br/><br/>Van [9.20 versie](https://support.microsoft.com/en-in/help/4478871/update-rollup-31-for-azure-site-recovery) en hoger, bevinden op LVM wordt ondersteund. Meerdere besturingssysteemschijven worden niet ondersteund.
Geparavirtualiseerde opslagapparaten | Apparaten die zijn geëxporteerd door geparavirtualiseerde stuurprogramma's worden niet ondersteund.
Meerdere wachtrij blok i/o-apparaten | Wordt niet ondersteund.
Fysieke servers met de opslagcontroller HP CCISS | Wordt niet ondersteund.
Apparaat/koppelpunt punt naamconventie | De naam van apparaat of de naam van koppelpunt moet uniek zijn. Zorg ervoor dat twee apparaten/koppelpunten nooit dezelfde naam hebben (niet hoofdlettergevoelig). </br> Voorbeeld: Naamgeving van de twee apparaten van dezelfde virtuele machine als *device1* en *Device1* is niet toegestaan.
Mappen | Voordat u [9.20 versie](https://support.microsoft.com/en-in/help/4478871/update-rollup-31-for-azure-site-recovery), <br/> 1. De volgende mappen (indien ingesteld als afzonderlijke partities /-bestandssystemen) alle moeten op dezelfde schijf als besturingssysteem op de bronserver: / (hoofd), bevinden, / usr, / usr/local, /var, / etc.</br>2. bevinden moet zich op een partitie op schijf en niet een LVM-volume.<br/><br/> Van [9.20 versie](https://support.microsoft.com/en-in/help/4478871/update-rollup-31-for-azure-site-recovery) bovenstaande beperkingen zijn en hoger, niet van toepassing. bevinden op LVM-volume op meer dan één schijven wordt niet ondersteund.
Opstartmap | Meerdere opstartschijven op een virtuele machine wordt niet ondersteund. <br/><br/> Een virtuele machine zonder opstartschijf kan niet worden beveiligd.
Vrije schijfruimte| 2 GB op de/root-partitie <br/><br/> 250 MB op de map voor installatie
XFSv5 | XFSv5 functies op XFS bestandssystemen, zoals metagegevens controlesom worden van Mobility Service versie 9.10 stationstoewijzingen ondersteund. Gebruik het hulpprogramma xfs_info om te controleren of de superblock XFS voor de partitie. Als `ftype` is ingesteld op 1, XFSv5 functies worden gebruikt.
BTRFS |Van 9.22 versie wordt BTRFS ondersteund, met uitzondering van de volgende scenario 's</br>Als de onderliggende volume voor BTRFS file system is gewijzigd nadat bescherming is ingeschakeld, worden BTRFS wordt niet ondersteund. </br>Als het bestandssysteem BTRFS is verspreid over meerdere schijven, worden BTRFS wordt niet ondersteund.</br>Als het bestandssysteem BTRFS RAID ondersteunt, worden BTRFS wordt niet ondersteund.

## <a name="vmdisk-management"></a>Beheer van de virtuele machine/schijf

**Actie** | **Details**
--- | ---
Grootte van de schijf op gerepliceerde virtuele machine wijzigen | Ondersteund.
Schijf toevoegen op de gerepliceerde virtuele machine | Schakel de replicatie voor de virtuele machine, de schijf toevoegen en vervolgens weer inschakelen replicatie. Toevoegen van een schijf op een replicerende virtuele machine wordt momenteel niet ondersteund.

## <a name="network"></a>Netwerk

**Onderdeel** | **Ondersteund**
--- | ---
Hostnetwerk NIC-koppeling | Ondersteund voor VMware-VM's. <br/><br/>Niet ondersteund voor replicatie van fysieke machine.
Hostnetwerk VLAN | Ja.
Hostnetwerk IPv4 | Ja.
Hostnetwerk IPv6 | Nee.
Gast/server-netwerk NIC-koppeling | Nee.
Gast/server-netwerk IPv4 | Ja.
Gast/server-netwerk IPv6 | Nee.
Gast/netwerk statische IP-adres van (Windows) | Ja.
Gast/netwerk statische IP-adres van (Linux) | Ja. <br/><br/>Virtuele machines zijn geconfigureerd voor het gebruik van DHCP voor failback.
Meerdere NIC's van het netwerk Gast/host-server | Ja.


## <a name="azure-vm-network-after-failover"></a>Azure VM-netwerk (na een failover)

**Onderdeel** | **Ondersteund**
--- | ---
Azure ExpressRoute | Ja
ILB | Ja
ELB | Ja
Azure Traffic Manager | Ja
Multi-NIC | Ja
Gereserveerde IP-adressen | Ja
IPv4 | Ja
Bron-IP-adres behouden | Ja
Azure Virtual Network-service-eindpunten<br/> | Ja
Versneld netwerken | Nee

## <a name="storage"></a>Storage
**Onderdeel** | **Ondersteund**
--- | ---
Dynamische schijf | Bewerking schijf moet een standaardschijf. <br/><br/>Gegevensschijven mogen dynamische schijven
De configuratie van de docker-schijf | Nee
Host NFS | Ja voor VMware<br/><br/> Nee voor fysieke servers
Host (iSCSI/FC) SAN | Ja
Host vSAN | Ja voor VMware<br/><br/> N.V.T. voor fysieke servers
Host-MPIO (Multipath I/O) | Ja, getest met Microsoft DSM, EMC PowerPath 5.7 SP4 EMC PowerPath DSM voor CLARiiON
Virtuele Hostvolumes (VVols) | Ja voor VMware<br/><br/> N.V.T. voor fysieke servers
VMDK-Gast/server | Ja
De gedeelde clusterschijf Gast/host-server | Nee
Versleutelde schijf Gast/host-server | Nee
Gast/NFS-server | Nee
Gast/server iSCSI | Nee
Gast/SMB 3.0-server | Nee
Gast/server RDM | Ja<br/><br/> N.V.T. voor fysieke servers
Gast/server schijf > 1 TB | Ja<br/><br/>Maximaal 4095 GB<br/><br/> Schijf moet groter zijn dan 1024 MB zijn.
Gast/server-schijf met de fysieke sectorgrootte van 4K logische en 4 k | Ja
Gast/server-schijf met de 4K logische en fysieke sectorgrootte van 512 bytes | Ja
Volume van de Gast/host-server met striped schijf > 4 TB <br/><br/>Logische volumebeheer (LVM)| Ja
Gast/server - opslagruimten | Nee
Schijf Gast/server-hot toevoegen of verwijderen | Nee
Gast/server - schijf uitsluiten | Ja
Gast/server MPIO (Multipath I/O) | Nee
Gast/server EFI/UEFI opstarten | Ondersteund bij het migreren van VMware-machines of fysieke servers met Windows Server 2012 of later naar Azure.<br/><br/> U kunt alleen virtuele machines repliceren voor migratie. Failback naar on-premises wordt niet ondersteund.<br/><br/> De server mag niet meer dan vier partities hebben op de besturingssysteemschijf.<br/><br/> Mobility Service versie 9.13 of hoger vereist.<br/><br/> Alleen NTFS wordt ondersteund.

## <a name="replication-channels"></a>Replicatie-kanalen

|**Type replicatie**   |**Ondersteund**  |
|---------|---------|
|Offloaded Data Transfer (ODX)    |       Nee  |
|Offline seeding        |   Nee      |
| Azure Data Box | Nee


## <a name="azure-storage"></a>Azure Storage

**Onderdeel** | **Ondersteund**
--- | ---
Lokaal redundante opslag | Ja
Geografisch redundante opslag | Ja
Geografisch redundante opslag met leestoegang | Ja
Koude opslag | Nee
Hot storage| Nee
Blok-blobs | Nee
Versleuteling-at-rest (Storage Service Encryption)| Ja
Premium Storage | Ja
Import/export-service | Nee
Azure Storage-firewalls voor virtuele netwerken die zijn geconfigureerd voor opslag/cache-opslagaccount voor het doel (gebruikt voor het opslaan van replicatiegegevens) | Ja
Opslagaccounts voor algemeen gebruik v2 (zowel warme als koude lagen) | Nee

## <a name="azure-compute"></a>Azure-rekenen

**Functie** | **Ondersteund**
--- | ---
Beschikbaarheidssets | Ja
Beschikbaarheidszones | Nee
HUB | Ja
Managed Disks | Ja

## <a name="azure-vm-requirements"></a>Vereisten voor Azure VM 's

On-premises machines die u naar Azure repliceren, moeten voldoen aan de virtuele machine van Azure-vereisten in deze tabel wordt samengevat. Wanneer Site Recovery wordt uitgevoerd een controle van vereisten, mislukt dit als aan bepaalde vereisten zijn niet voldaan.

**Onderdeel** | **Vereisten** | **Details**
--- | --- | ---
Gast-besturingssysteem | Controleer of [ondersteunde besturingssystemen](#replicated-machines) voor gerepliceerde machines. | Controle mislukt als niet-ondersteund.
Architectuur van de Gast-besturingssysteem | 64-bit. | Controle mislukt als niet-ondersteund.
Grootte van de besturingssysteemschijf | Maximaal 2048 GB. | Controle mislukt als niet-ondersteund.
Aantal besturingssysteemschijven | 1 | Controle mislukt als niet-ondersteund.
Aantal gegevensschijven | 64 of minder. | Controle mislukt als niet-ondersteund.
Grootte van de gegevensschijf | Maximaal 4095 GB | Controle mislukt als niet-ondersteund.
Netwerkadapters | Meerdere netwerkadapters worden ondersteund. |
Gedeelde VHD | Wordt niet ondersteund. | Controle mislukt als niet-ondersteund.
FC-schijf | Wordt niet ondersteund. | Controle mislukt als niet-ondersteund.
BitLocker | Wordt niet ondersteund. | BitLocker moet worden uitgeschakeld voordat u replicatie voor een machine inschakelt. |
VM-naam | Van 1 tot 63 tekens bevatten.<br/><br/> Alleen letters, cijfers en afbreekstreepjes.<br/><br/> Naam van de machine moet beginnen en eindigen met een letter of cijfer. |  Werk de waarde in de eigenschappen van de machine in Site Recovery.

## <a name="azure-site-recovery-churn-limits"></a>Azure Site Recovery-limieten voor verloop

De volgende tabel bevat de Azure Site Recovery-limieten. Deze limieten zijn gebaseerd op onze tests, maar dekken niet alle mogelijke toepassings-I/O-combinaties. De werkelijke resultaten kunnen variëren op basis van uw toepassings-I/O-combinatie. Voor optimale resultaten, wordt aangeraden om te [tool voor implementatieplanning uitgevoerd](site-recovery-deployment-planner.md) en moet u toepassingen uitgebreid testen met behulp van een testfailover uit om de prestaties van de toepassing.

**Beoogde replicatieopslag** | **Gemiddelde I/O-grootte van bronschijf** |**Gemiddeld gegevensverloop van bronschijf** | **Totale gegevensverloop van bronschijf per dag**
---|---|---|---
Standard Storage | 8 kB | 2 MB/s | 168 GB per schijf
Premium P10 of P15 schijf | 8 kB  | 2 MB/s | 168 GB per schijf
Premium P10 of P15 schijf | 16 kB | 4 MB/s |  336 GB per schijf
Premium P10 of P15 schijf | 32 kB of meer | 8 MB/s | 672 GB per schijf
Premium P20 of P30 of P40 of P50 schijf | 8 kB    | 5 MB/s | 421 GB per schijf
Premium P20 of P30 of P40 of P50 schijf | 16 kB of meer |20 MB/s | 1684 GB per schijf

**brongegevensverloop** | **Maximumaantal**
---|---
Gemiddeld gegevensverloop per VM| 25 MB/s
Piekgegevensverloop over alle schijven op een VM | 54 MB/s
Maximumgegevensverloop per dag dat wordt ondersteund door een processerver | 2 TB

Dit zijn gemiddelden uitgaande van een I/O-overlapping van 30%. Site Recovery kan een hogere doorvoer verwerken op basis van overlappingsverhouding, grotere schrijfgrootten en daadwerkelijk workload-I/O-gedrag. De bovenstaande waarden zijn gebaseerd op een typische backlog van ongeveer vijf minuten. Dat wil zeggen dat de gegevens na het uploaden binnen vijf minuten worden verwerkt en er een herstelpunt is gemaakt.

## <a name="vault-tasks"></a>Kluis-taken

**Actie** | **Ondersteund**
--- | ---
Kluis verplaatsen tussen resourcegroepen<br/><br/> Binnen en tussen abonnementen | Nee
Verplaatsen van opslag, netwerk, Azure-VM's op resourcegroepen<br/><br/> Binnen en tussen abonnementen | Nee


## <a name="download-latest-azure-site-recovery-components"></a>Download de nieuwste Azure Site Recovery-onderdelen

**Naam** | **Beschrijving** | **Meest recente versie downloadinstructies**
--- | --- | ---
Configuratieserver | Coördineert de communicatie tussen on-premises VMware-servers en Azure <br/><br/> Geïnstalleerd op de on-premises VMware-servers | Voor meer informatie gaat u naar onze richtlijnen op [nieuwe installatie](vmware-azure-deploy-configuration-server.md) en [upgrade van het bestaande onderdeel om de meest recente versie](vmware-azure-manage-configuration-server.md#upgrade-the-configuration-server).
Processerver|standaard geïnstalleerd op de configuratieserver. Deze ontvangt replicatiegegevens; Met caching, compressie en versleuteling, optimaliseert en verzendt dit naar Azure Storage. Naarmate uw implementatie groeit, kunt u extra, afzonderlijk processervers voor het afhandelen van grotere hoeveelheden replicatieverkeer kunt toevoegen.| Voor meer informatie gaat u naar onze richtlijnen op [nieuwe installatie](vmware-azure-set-up-process-server-scale.md) en [upgrade van het bestaande onderdeel om de meest recente versie](vmware-azure-manage-process-server.md#upgrade-a-process-server).
Mobility-Service | Coördineert de replicatie tussen on-premises VMware-servers/fysieke servers en Azure/secundaire site<br/><br/> Geïnstalleerd op de VM met VMware of fysieke servers die u wilt repliceren | Voor meer informatie gaat u naar onze richtlijnen op [nieuwe installatie](vmware-azure-install-mobility-service.md) en [upgrade van het bestaande onderdeel om de meest recente versie](vmware-physical-manage-mobility-service.md#update-mobility-service-from-azure-portal).

Voor meer informatie over de nieuwste functies, gaat u naar [nieuwste opmerkingen bij de release](https://aka.ms/ASR_latest_release_notes).


## <a name="next-steps"></a>Volgende stappen
[Informatie over hoe](tutorial-prepare-azure.md) het voorbereiden van Azure voor herstel na noodgevallen van virtuele VMware-machines.

[9.23 UR]: https://support.microsoft.com/help/4489582/update-rollup-33-for-azure-site-recovery
[9.22 UR]: https://support.microsoft.com/help/4489582/update-rollup-33-for-azure-site-recovery
[9.21 UR]: https://support.microsoft.com/help/4485985/update-rollup-32-for-azure-site-recovery
[9.20 UR]: https://support.microsoft.com/help/4478871/update-rollup-31-for-azure-site-recovery
[9.19 UR]: https://support.microsoft.com/help/4468181/azure-site-recovery-update-rollup-30
[9.18 UR]: https://support.microsoft.com/help/4466466/update-rollup-29-for-azure-site-recovery
