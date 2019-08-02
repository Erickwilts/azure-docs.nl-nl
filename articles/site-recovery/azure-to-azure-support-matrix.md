---
title: Ondersteunings matrix voor herstel na nood gevallen van Azure-Vm's tussen Azure-regio's met Azure Site Recovery | Microsoft Docs
description: Een overzicht van de vereisten en ondersteuning voor herstel na nood gevallen van virtuele Azure-machines van de ene regio naar de andere met Azure Site Recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 07/22/2019
ms.author: raynew
ms.openlocfilehash: 87362b785ecbb17c5a49a8c087b6a70715480877
ms.sourcegitcommit: 13d5eb9657adf1c69cc8df12486470e66361224e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2019
ms.locfileid: "68679381"
---
# <a name="support-matrix-for-replicating-azure-vms-from-one-region-to-another"></a>Ondersteuningsmatrix voor het repliceren van Azure-VM's van de ene naar de andere regio

In dit artikel vindt u een overzicht van de ondersteuning en vereisten bij het instellen van herstel na nood gevallen van virtuele Azure-machines van een Azure-regio naar een andere, met behulp van de [Azure site Recovery](site-recovery-overview.md) -service.


## <a name="deployment-method-support"></a>Ondersteuning voor implementatie methoden

**Implementatie** |  **Ondersteuning**
--- | ---
**Azure-portal** | Ondersteund.
**PowerShell** | Ondersteund. [Meer informatie](azure-to-azure-powershell.md)
**REST API** | Ondersteund.
**CLI** | Momenteel niet ondersteund


## <a name="resource-support"></a>Resource-ondersteuning

**Resource actie** | **Details**
--- | --- | ---
**Kluizen verplaatsen tussen resource groepen** | Niet ondersteund
**Berekenings-en opslag/netwerk bronnen verplaatsen over resource groepen** | Wordt niet ondersteund.<br/><br/> Als u een virtuele machine of gekoppelde onderdelen, zoals opslag/netwerk, verplaatst nadat de VM is gerepliceerd, moet u de replicatie voor de virtuele machine uitschakelen en opnieuw inschakelen.
**Virtuele Azure-machines van het ene naar het andere abonnement repliceren voor herstel na nood gevallen** | Ondersteund binnen dezelfde Azure Active Directory Tenant.
**Vm's migreren tussen regio's binnen ondersteunde geografische clusters (binnen en tussen abonnementen)** | Ondersteund binnen dezelfde Azure Active Directory Tenant.
**Vm's binnen dezelfde regio migreren** | Wordt niet ondersteund.

## <a name="region-support"></a>Regio ondersteuning

U kunt virtuele machines repliceren en herstellen tussen twee regio's binnen hetzelfde geografische cluster. Geografische clusters worden gedefinieerd om gegevens latentie en soevereiniteit in gedachte te houden.


**Geografisch cluster** | **Azure-regio's**
-- | --
Lopende | Canada-oost, Canada-centraal, Zuid-Centraal VS, West-Centraal VS, VS-Oost, VS-Oost 2, VS-West, VS-West 2, VS-midden, Noord-Centraal VS
Europa | UK-west, UK-zuid, Europa-noord, Europa-west, Frankrijk-centraal, Frankrijk-zuid, Zuid-Afrika-west, Zuid-Afrika-noord
Azië | India-zuid, Centraal-India, West-India, Zuidoost-Azië, Azië-oost, Japan-Oost, Japan-West, Korea-centraal, Korea-zuid, UAE-centraal, UAE-noord
Australië   | Australië-oost, Australië-zuidoost, Australië-centraal, Australië-centraal 2
Azure Government    | Amerikaanse GOVe Virginia, VS GOV Iowa, US GOV Arizona, VS GOV Texas, US DOD Oost, US DOD-centraal 
Duitsland | Duitsland-centraal, Duitsland-noordoost
China | China-oost, China-noord, China North2, China Oost2

>[!NOTE]
>
> - U kunt voor **Brazilië-Zuid**repliceren en een failover uitvoeren naar deze regio's: Zuid-Centraal VS, West-Centraal VS, VS-Oost, VS-Oost 2, VS-West, VS-West 2 en Noord-Centraal vs.
> - Brazilië-zuid kan alleen worden gebruikt als bron regio op basis waarvan Vm's kunnen worden gerepliceerd met behulp van Site Recovery. Het kan niet als doel regio fungeren. Dit komt door latentie problemen als gevolg van geografische afstanden.
> - U kunt binnen regio's werken waarvoor u de juiste toegang hebt.
> - Als de regio waarin u een kluis wilt maken, niet wordt weer gegeven, controleert u of uw abonnement toegang heeft tot het maken van resources in die regio.
> - Als u geen regio binnen een geografisch cluster ziet wanneer u replicatie inschakelt, moet u ervoor zorgen dat uw abonnement beschikt over machtigingen voor het maken van Vm's in die regio.



## <a name="cache-storage"></a>Cacheopslag

Deze tabel geeft een overzicht van de ondersteuning van het cache-opslag account dat wordt gebruikt door Site Recovery tijdens de replicatie.

**Instelling** | **Ondersteuning** | **Details**
--- | --- | ---
V2-opslag accounts voor algemeen gebruik (warme en koele laag) | Ondersteund | Het gebruik van GPv2 wordt niet aanbevolen omdat de transactie kosten voor v2 beduidend hoger zijn dan v1 opslag accounts.
Firewalls voor virtuele netwerken Azure Storage  | Ondersteund | Als u gebruikmaakt van het cache-opslag account of het doel opslag account van de firewall, moet u [vertrouwde micro soft-Services toestaan](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions).


## <a name="replicated-machine-operating-systems"></a>Besturings systemen van de gerepliceerde machine

Site Recovery ondersteunt replicatie van virtuele Azure-machines met de besturings systemen die worden vermeld in deze sectie.

### <a name="windows"></a>Windows

**Besturingssysteem** | **Details**
--- | ---
Windows Server 2019 | Server Core, server met bureaublad ervaring
Windows Server 2016  | Server Core, server met bureaublad ervaring
Windows Server 2012 R2 |
Windows Server 2012 |
Windows Server 2008 R2 | SP1 of hoger uitvoeren
Windows 10 (x64) |
Windows 8,1 (x64) |
Windows 8 (x64) |
Windows 7 (x64) | SP1 of hoger uitvoeren (Windows 7 RTM wordt niet ondersteund)

#### <a name="linux"></a>Linux

**Besturingssysteem** | **Details**
--- | ---
Red Hat Enterprise Linux | 6.7, 6.8, 6.9, 6.10, 7.0, 7.1, 7.2, 7.3, 7.4, 7.5, 7.6  
CentOS | 6.5, 6.6, 6.7, 6.8, 6.9, 6.10, 7.0, 7.1, 7.2, 7.3, 7.4, 7.5, 7.6
Ubuntu 14,04 LTS-server | [Ondersteunde kernel-versies](#supported-ubuntu-kernel-versions-for-azure-virtual-machines)
Ubuntu 16,04 LTS-server | [Ondersteunde kernel-versie](#supported-ubuntu-kernel-versions-for-azure-virtual-machines)<br/><br/> Ubuntu-servers die gebruikmaken van verificatie op basis van wacht woorden en aanmelden en het pakket Cloud-init om Cloud-Vm's te configureren, hebben mogelijk een op wacht woord gebaseerde aanmelding uitgeschakeld bij failover (afhankelijk van de cloudinit-configuratie). Aanmelden op basis van wacht woorden kan opnieuw worden ingeschakeld op de virtuele machine door het wacht woord opnieuw in te stellen in het menu > ondersteuning voor het oplossen van problemen met de >-instellingen (van de VM waarvoor een failover is uitgevoerd in de Azure Portal.
Debian 7 | [Ondersteunde kernel-versies](#supported-debian-kernel-versions-for-azure-virtual-machines)
Debian 8 | [Ondersteunde kernel-versies](#supported-debian-kernel-versions-for-azure-virtual-machines)
SUSE Linux Enterprise Server 12 | SP1, SP2, SP3, SP4. [(Ondersteunde kernel-versies)](#supported-suse-linux-enterprise-server-12-kernel-versions-for-azure-virtual-machines)
SUSE Linux Enterprise Server 11 | SP3<br/><br/> De upgrade van replicerende machines van SP3 naar SP4 wordt niet ondersteund. Als een gerepliceerde machine is bijgewerkt, moet u de replicatie uitschakelen en de replicatie opnieuw inschakelen na de upgrade.
SUSE Linux Enterprise Server 11 | SP4
Oracle Linux | 6.4, 6.5, 6.6, 6.7, 6.8, 6.9, 6.10, 7.0, 7.1, 7.2, 7.3, 7.4, 7.5, 7.6<br/><br/> Met de Red Hat compatibele kernel of een onherstelbare versie van de Enter prise kernel van 3, 4 & 5 (UEK3, UEK4, UEK5) 


#### <a name="supported-ubuntu-kernel-versions-for-azure-virtual-machines"></a>Ondersteunde Ubuntu-kernel-versies voor virtuele Azure-machines

**Heffing** | **Mobility Service-versie** | **Kernelversie** |
--- | --- | --- |
14,04 LTS | 9,27 | 3.13.0-24-generic naar 3.13.0-170-generic,<br/>3.16.0-25-generic naar 3.16.0-77-generic,<br/>3.19.0-18-generic naar 3.19.0-80-generic,<br/>4.2.0-18-generic naar 4.2.0-42-generic,<br/>4.4.0-21-generic naar 4.4.0-148-generic,<br/>4.15.0-1023-Azure naar 4.15.0-1045-Azure |
14,04 LTS | 9,26 | 3.13.0-24-generic naar 3.13.0-170-generic,<br/>3.16.0-25-generic naar 3.16.0-77-generic,<br/>3.19.0-18-generic naar 3.19.0-80-generic,<br/>4.2.0-18-generic naar 4.2.0-42-generic,<br/>4.4.0-21-generic naar 4.4.0-148-generic,<br/>4.15.0-1023-Azure naar 4.15.0-1045-Azure |
14,04 LTS | 9,25 | 3.13.0-24-algemeen naar 3.13.0-169-generic,<br/>3.16.0-25-generic naar 3.16.0-77-generic,<br/>3.19.0-18-generic naar 3.19.0-80-generic,<br/>4.2.0-18-generic naar 4.2.0-42-generic,<br/>4.4.0-21-generic naar 4.4.0-146-generic,<br/>4.15.0-1023-Azure naar 4.15.0-1042-Azure |
14,04 LTS | 9,24 | 3.13.0-24-generic naar 3.13.0-167-generic,<br/>3.16.0-25-generic naar 3.16.0-77-generic,<br/>3.19.0-18-generic naar 3.19.0-80-generic,<br/>4.2.0-18-generic naar 4.2.0-42-generic,<br/>4.4.0-21-algemeen naar 4.4.0-143-generic,<br/>4.15.0-1023-Azure naar 4.15.0-1040-Azure |
|||
16,04 LTS | 9,27 | 4.4.0-21-generic naar 4.4.0-154-generic,<br/>4.8.0-34-generic naar 4.8.0-58-generic,<br/>4.10.0-14-generic naar 4.10.0-42-generic<br/>4.11.0-13-algemeen naar 4.11.0-14-generic,<br/>4.13.0-16-generic naar 4.13.0-45-generic,<br/>4.15.0-13-algemeen tot 4.15.0-54-algemeen<br/>4.11.0-1009-Azure naar 4.11.0-1016-Azure,<br/>4.13.0-1005-Azure naar 4.13.0-1018-Azure <br/>4.15.0-1012-Azure naar 4.15.0-1050-Azure|
16,04 LTS | 9,26 | 4.4.0-21-generic naar 4.4.0-148-generic,<br/>4.8.0-34-generic naar 4.8.0-58-generic,<br/>4.10.0-14-generic naar 4.10.0-42-generic<br/>4.11.0-13-algemeen naar 4.11.0-14-generic,<br/>4.13.0-16-generic naar 4.13.0-45-generic,<br/>4.15.0-13-algemeen tot 4.15.0-50-algemeen<br/>4.11.0-1009-Azure naar 4.11.0-1016-Azure,<br/>4.13.0-1005-Azure naar 4.13.0-1018-Azure <br/>4.15.0-1012-Azure naar 4.15.0-1045-Azure|
16,04 LTS | 9,25 | 4.4.0-21-generic naar 4.4.0-146-generic,<br/>4.8.0-34-generic naar 4.8.0-58-generic,<br/>4.10.0-14-generic naar 4.10.0-42-generic,<br/>4.11.0-13-algemeen naar 4.11.0-14-generic,<br/>4.13.0-16-generic naar 4.13.0-45-generic,<br/>4.15.0-13-algemeen tot 4.15.0-48-algemeen<br/>4.11.0-1009-Azure naar 4.11.0-1016-Azure,<br/>4.13.0-1005-Azure naar 4.13.0-1018-Azure <br/>4.15.0-1012-Azure naar 4.15.0-1042-Azure|
16,04 LTS | 9,24 | 4.4.0-21-algemeen naar 4.4.0-143-generic,<br/>4.8.0-34-generic naar 4.8.0-58-generic,<br/>4.10.0-14-generic naar 4.10.0-42-generic,<br/>4.11.0-13-algemeen naar 4.11.0-14-generic,<br/>4.13.0-16-generic naar 4.13.0-45-generic,<br/>4.15.0-13-algemeen tot 4.15.0-46-algemeen<br/>4.11.0-1009-Azure naar 4.11.0-1016-Azure,<br/>4.13.0-1005-Azure naar 4.13.0-1018-Azure <br/>4.15.0-1012-Azure naar 4.15.0-1040-Azure|

#### <a name="supported-debian-kernel-versions-for-azure-virtual-machines"></a>Ondersteunde Debian-kernel-versies voor virtuele Azure-machines

**Heffing** | **Mobility Service-versie** | **Kernelversie** |
--- | --- | --- |
Debian 7 | 9.24,9.25,9.26,9.27 | 3.2.0-4-amd64 tot 3.2.0-6-amd64, 3.16.0 -0. bpo. 4-amd64 |
|||
Debian 8 | 9,27 | 3.16.0-4-amd64 tot 3.16.0-9-amd64, 4.9.0 -0. bpo. 4-amd64 tot 4.9.0 -0. bpo. 9-amd64 |
Debian 8 | 9,25, 9,26 | 3.16.0-4-amd64 tot 3.16.0-8-amd64, 4.9.0 -0. bpo. 4-amd64 tot 4.9.0 -0. bpo. 8-amd64 |
Debian 8 | 9,24 | 3.16.0-4-amd64 tot 3.16.0-7-amd64, 4.9.0 -0. bpo. 4-amd64 tot 4.9.0 -0. bpo. 8-amd64 |

#### <a name="supported-suse-linux-enterprise-server-12-kernel-versions-for-azure-virtual-machines"></a>Ondersteunde SUSE Linux Enterprise Server 12 kernel-versies voor virtuele Azure-machines

**Heffing** | **Mobility Service-versie** | **Kernelversie** |
--- | --- | --- |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3, SP4) | 9,27 | SP1 3.12.49-11: standaard ingesteld op 3.12.74-60.64.40-default</br></br> SP1 (LTSS) 3.12.74-60.64.45-standaard ingesteld op 3.12.74-60.64.115-standaard</br></br> SP2 4.4.21-69-standaard ingesteld op 4.4.120-92.70-default</br></br>SP2 (LTSS) 4.4.121-92.73-standaard voor 4.4.121-92.114-default</br></br>SP3 4.4.73-5-standaard ingesteld op 4.4.180-94.97-default</br></br>SP3 4.4.138-4.7-Azure-naar-4.4.180-4.31-Azure</br></br>SP4 4.12.14-94.41-standaard ingesteld op 4.12.14-95.19-default</br>SP4 4.12.14-6.3-Azure naar 4.12.14-6.15-Azure |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3, SP4) | 9,26 | SP1 3.12.49-11: standaard ingesteld op 3.12.74-60.64.40-default</br></br> SP1 (LTSS) 3.12.74-60.64.45-standaard ingesteld op 3.12.74-60.64.110-standaard</br></br> SP2 4.4.21-69-standaard ingesteld op 4.4.120-92.70-default</br></br>SP2 (LTSS) 4.4.121-92.73-standaard voor 4.4.121-92.109-default</br></br>SP3 4.4.73-5-standaard ingesteld op 4.4.178-94.91-default</br></br>SP3 4.4.138-4.7-Azure naar 4.4.178-4.28-Azure</br></br>SP4 4.12.14-94.41-standaard ingesteld op 4.12.14-95.16-default</br>SP4 4.12.14-6.3-Azure naar 4.12.14-6,9-Azure |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3, SP4) | 9,25 | SP1 3.12.49-11: standaard ingesteld op 3.12.74-60.64.40-default</br></br> SP1 (LTSS) 3.12.74-60.64.45-standaard ingesteld op 3.12.74-60.64.107-standaard</br></br> SP2 4.4.21-69-standaard ingesteld op 4.4.120-92.70-default</br></br>SP2 (LTSS) 4.4.121-92.73-standaard voor 4.4.121-92.104-default</br></br>SP3 4.4.73-5-standaard ingesteld op 4.4.176-94.88-default</br></br>SP3 4.4.138-4.7-Azure naar 4.4.176-4,25-Azure</br></br>SP4 4.12.14-94.41-standaard ingesteld op 4.12.14-95.13-default</br>SP4 4.12.14-6.3-Azure naar 4.12.14-6,9-Azure |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3, SP4) | 9,24 | SP1 3.12.49-11: standaard ingesteld op 3.12.74-60.64.40-default</br></br> SP1 (LTSS) 3.12.74-60.64.45-standaard ingesteld op 3.12.74-60.64.107-standaard</br></br> SP2 4.4.21-69-standaard ingesteld op 4.4.120-92.70-default</br></br>SP2 (LTSS) 4.4.121-92.73-standaard voor 4.4.121-92.104-default</br></br>SP3 4.4.73-5-standaard ingesteld op 4.4.176-94.88-default</br></br>SP4 4.12.14-94.41-standaard ingesteld op 4.12.14-95.13-default |


## <a name="replicated-machines---linux-file-systemguest-storage"></a>Gerepliceerde machines-Linux-bestands systeem/gast opslag

* Bestands systemen: ext3, ext4, ReiserFS (SuSE Linux Enter prise server), XFS, BTRFS
* Volume manager: LVM2
* Multipath-software: Toewijzing van apparaten


## <a name="replicated-machines---compute-settings"></a>Gerepliceerde machines-Compute Settings

**Instelling** | **Ondersteuning** | **Details**
--- | --- | ---
Size | Een Azure VM-grootte met ten minste twee CPU-kernen en 1 GB RAM | Controleer de [grootte van virtuele Azure-machines](../virtual-machines/windows/sizes.md).
Beschikbaarheidssets | Ondersteund | Als u replicatie inschakelt voor een virtuele machine van Azure met de standaard opties, wordt er automatisch een beschikbaarheidsset gemaakt, op basis van de instellingen van de bron regio. U kunt deze instellingen wijzigen.
Beschikbaarheidszones | Ondersteund |
Voor deel voor hybride gebruik (HUB) | Ondersteund | Als op de bron-VM een HUB-licentie is ingeschakeld, gebruikt een testfailover of een failover van de VM ook de HUB-licentie.
Schaalsets van virtuele machines | Niet ondersteund |
Installatie kopieën van Azure galerie-micro soft gepubliceerd | Ondersteund | Wordt ondersteund als de virtuele machine wordt uitgevoerd op een ondersteund besturings systeem.
Azure Gallery-installatie kopieën-derde partij gepubliceerd | Ondersteund | Wordt ondersteund als de virtuele machine wordt uitgevoerd op een ondersteund besturings systeem.
Aangepaste installatie kopieën-externe partij gepubliceerd | Ondersteund | Wordt ondersteund als de virtuele machine wordt uitgevoerd op een ondersteund besturings systeem.
Vm's die zijn gemigreerd met behulp van Site Recovery | Ondersteund | Als een VMware-VM of fysieke machine met Site Recovery is gemigreerd naar Azure, moet u de oudere versie van Mobility service op de computer verwijderen en de computer opnieuw opstarten voordat u deze naar een andere Azure-regio repliceert.
RBAC-beleid | Niet ondersteund | Beleids regels op basis van op rollen gebaseerd toegangs beheer (RBAC) op Vm's worden niet gerepliceerd naar de failover-VM in de doel regio.
Extensies | Niet ondersteund | Uitbrei dingen worden niet gerepliceerd naar de failover-VM in de doel regio. Deze moet hand matig worden geïnstalleerd na een failover.

## <a name="replicated-machines---disk-actions"></a>Gerepliceerde machines-schijf acties

**Actie** | **Details**
-- | ---
Grootte van schijf op een gerepliceerde VM wijzigen | Ondersteund
Een schijf toevoegen aan een gerepliceerde VM | Ondersteund

## <a name="replicated-machines---storage"></a>Gerepliceerde machines-opslag

Deze tabel bevat een overzicht van de ondersteuning voor de Azure VM-besturingssysteem schijf, de gegevens schijf en de tijdelijke schijf.

- Het is belang rijk om te kijken naar de limieten en doelen van de VM-schijf voor [Linux](../virtual-machines/linux/disk-scalability-targets.md) -en [Windows](../virtual-machines/windows/disk-scalability-targets.md) -vm's om prestatie problemen te voor komen.
- Als u implementeert met de standaard instellingen, maakt Site Recovery automatisch schijven en opslag accounts op basis van de bron instellingen.
- Als u aanpassingen aanpast, moet u de richt lijnen volgen.

**Onderdeel** | **Ondersteuning** | **Details**
--- | --- | ---
Maximale grootte van de besturingssysteem schijf | 2048 GB | Meer [informatie](../virtual-machines/windows/managed-disks-overview.md) over VM-schijven.
Tijdelijke schijf | Niet ondersteund | De tijdelijke schijf wordt altijd uitgesloten van replicatie.<br/><br/> Sla geen permanente gegevens op de tijdelijke schijf op. [Meer informatie](../virtual-machines/windows/managed-disks-overview.md).
Maximale grootte van gegevens schijf | 8192 GB voor beheerde schijven<br></br>4095 GB voor niet-beheerde schijven|
Minimale grootte van gegevens schijf | Geen beperking voor niet-beheerde schijven. 2 GB voor beheerde schijven | 
Maximum aantal gegevens schijven | Maxi maal 64, in overeenstemming met de ondersteuning voor een specifieke Azure VM-grootte | Meer [informatie](../virtual-machines/windows/sizes.md) over VM-grootten.
Wijzigings frequentie van gegevens schijven | Maxi maal 10 MBps per schijf voor Premium-opslag. Maxi maal 2 MBps per schijf voor standaard opslag. | Als de gemiddelde waarde voor het wijzigen van de gegevens op de schijf continu hoger is dan het maximum, wordt de replicatie niet opvangen.<br/><br/>  Als het maximum echter sporadisch wordt overschreden, kan het zijn dat de replicatie kan worden vertraagd.
Gegevens schijf-Standard-opslag account | Ondersteund |
Gegevens schijf-Premium-opslag account | Ondersteund | Als een virtuele machine schijven heeft verspreid over Premium-en Standard-opslag accounts, kunt u een ander doel opslag account voor elke schijf selecteren om ervoor te zorgen dat u dezelfde opslag configuratie in de doel regio hebt.
Beheerde schijf-standaard | Ondersteund in azure-regio's waarin Azure Site Recovery wordt ondersteund. |
Beheerde schijf-Premium | Ondersteund in azure-regio's waarin Azure Site Recovery wordt ondersteund. |
Standard - SSD | Ondersteund |
Redundantie | LRS en GRS worden ondersteund.<br/><br/> ZRS wordt niet ondersteund.
Coole en warme opslag | Niet ondersteund | VM-schijven worden niet ondersteund in coole en warme opslag
Opslag ruimten | Ondersteund |
Versleuteling op rest (SSE) | Ondersteund | SSE is de standaard instelling voor opslag accounts.   
Azure Disk Encryption (ADE) voor Windows-besturings systeem | Vm's die zijn ingeschakeld voor [versleuteling met Azure AD-App](https://aka.ms/ade-aad-app) worden ondersteund |
Azure Disk Encryption (ADE) voor Linux-besturings systeem | Niet ondersteund |
Hot add | Ondersteund | Het inschakelen van replicatie voor een gegevens schijf die u toevoegt aan een gerepliceerde Azure-VM wordt ondersteund voor virtuele machines die gebruikmaken van beheerde schijven.
Hot Remove-schijf | Niet ondersteund | Als u de gegevens schijf op de virtuele machine verwijdert, moet u de replicatie uitschakelen en de replicatie opnieuw inschakelen voor de virtuele machine.
Schijf uitsluiten | Voor. U moet [Power shell](azure-to-azure-exclude-disks.md) gebruiken om te configureren. |  Tijdelijke schijven worden standaard uitgesloten.
Storage Spaces Direct  | Ondersteund voor crash consistente herstel punten. Toepassings consistente herstel punten worden niet ondersteund. |
Scale-out Bestands server  | Ondersteund voor crash consistente herstel punten. Toepassings consistente herstel punten worden niet ondersteund. |
LRS | Ondersteund |
GRS | Ondersteund |
RA-GRS | Ondersteund |
ZRS | Niet ondersteund |
Coole en warme opslag | Niet ondersteund | Schijven van virtuele machines worden niet ondersteund in coole en warme opslag
Firewalls voor virtuele netwerken Azure Storage  | Ondersteund | Schakel [vertrouwde micro soft-Services toestaan](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions)in als u de toegang tot het virtuele netwerk beperken tot opslag accounts.
V2-opslag accounts voor algemeen gebruik (zowel warme als koud-tier) | Ja | De transactie kosten stijgen aanzienlijk vergeleken met de V1-opslag accounts voor algemeen gebruik

>[!IMPORTANT]
> Om prestatie problemen te voor komen, moet u ervoor zorgen dat u de schaal baarheid en prestatie doelen van de VM-schijf voor [Linux](../virtual-machines/linux/disk-scalability-targets.md) -of [Windows](../virtual-machines/windows/disk-scalability-targets.md) -vm's volgt. Als u de standaard instellingen gebruikt, Site Recovery de vereiste schijven en opslag accounts maken op basis van de configuratie van de bron. Als u uw eigen instellingen aanpast en selecteert, volgt u de schaal baarheid en prestaties van de schijf voor uw bron-Vm's.

## <a name="limits-and-data-change-rates"></a>Limieten en snelheid van gegevens wijzigingen

De volgende tabel geeft een overzicht van Site Recovery limieten.

- Deze limieten zijn gebaseerd op onze tests, maar uiteraard dekken niet alle mogelijke toepassings-I/O-combi Naties.
- De werkelijke resultaten kunnen variëren op basis van de I/O-mix van de app.
- Er zijn twee limieten die u moet overwegen: gegevens verloop per schijf en per gegevens verloop van de virtuele machine.
- Als we bijvoorbeeld een Premium P20-schijf gebruiken zoals wordt beschreven in de onderstaande tabel, kan Site Recovery 5 MB aan verloop per schijf verwerken, met Maxi maal vijf schijven per VM, als gevolg van de limiet van 25 MB/s totale verloop per VM.

**Opslag doel** | **Gemiddelde bron schijf-I/O** |**Gemiddeld gegevensverloop van bronschijf** | **Totale gegevensverloop van bronschijf per dag**
---|---|---|---
Standard Storage | 8 kB | 2 MB/s | 168 GB per schijf
Premium P10 of P15 schijf | 8 kB  | 2 MB/s | 168 GB per schijf
Premium P10 of P15 schijf | 16 kB | 4 MB/s |  336 GB per schijf
Premium P10 of P15 schijf | 32 kB of meer | 8 MB/s | 672 GB per schijf
Premium P20 of P30 of P40 of P50 schijf | 8 kB    | 5 MB/s | 421 GB per schijf
Premium P20 of P30 of P40 of P50 schijf | 16 kB of meer |20 MB/s | 1684 GB per schijf

## <a name="replicated-machines---networking"></a>Gerepliceerde computers-netwerken
**Instelling** | **Ondersteuning** | **Details**
--- | --- | ---
NIC | Maximum aantal dat wordt ondersteund voor een specifieke Azure VM-grootte | Nic's worden gemaakt wanneer de virtuele machine wordt gemaakt tijdens de failover.<br/><br/> Het aantal Nic's op de failover-VM is afhankelijk van het aantal Nic's op de bron-VM wanneer replicatie is ingeschakeld. Als u een NIC toevoegt of verwijdert nadat de replicatie is ingeschakeld, heeft dit geen invloed op het aantal Nic's op de gerepliceerde VM na een failover. Houd er ook rekening mee dat de volg orde van de Nic's na een failover niet gegarandeerd hetzelfde is als de oorspronkelijke volg orde.
Internet Load Balancer | Ondersteund | De vooraf geconfigureerde load balancer koppelen met behulp van een Azure Automation script in een herstel plan.
Interne Load Balancer | Ondersteund | De vooraf geconfigureerde load balancer koppelen met behulp van een Azure Automation script in een herstel plan.
Openbaar IP-adres | Ondersteund | Een bestaand openbaar IP-adres koppelen aan de NIC. U kunt ook een openbaar IP-adres maken en dit koppelen aan de NIC met behulp van een Azure Automation script in een herstel plan.
NSG op NIC | Ondersteund | Koppel de NSG aan de NIC met behulp van een Azure Automation script in een herstel plan.
NSG op subnet | Ondersteund | Koppel de NSG aan het subnet met behulp van een Azure Automation script in een herstel plan.
Gereserveerd (statisch) IP-adres | Ondersteund | Als de NIC op de bron-VM een statisch IP-adres heeft en het doel-subnet hetzelfde IP-adres beschikbaar heeft, wordt het toegewezen aan de virtuele machine waarvoor een failover is uitgevoerd.<br/><br/> Als het doel-subnet niet hetzelfde IP-adres beschikbaar heeft, is een van de beschik bare IP-adressen in het subnet gereserveerd voor de virtuele machine.<br/><br/> U kunt ook een vast IP-adres en een subnet opgeven in de**instellingen** > voor de **gerepliceerde items** > COMPUTE-**en netwerk** > **netwerk interfaces**.
Dynamisch IP-adres | Ondersteund | Als de NIC op de bron dynamische IP-adres Sering heeft, is de NIC op de virtuele machine waarvoor een failover is uitgevoerd, standaard ook dynamisch.<br/><br/> U kunt dit zo nodig wijzigen in een vast IP-adres.
Meerdere IP-adressen | Niet ondersteund | Wanneer u een failover van een virtuele machine met meerdere IP-adressen hebt uitgevoerd, wordt alleen het primaire IP-adres van de NIC in de bron regio bewaard. Als u meerdere IP-adressen wilt toewijzen, kunt u virtuele machines toevoegen aan een [herstel plan](recovery-plan-overview.md) en een script koppelen om extra IP-adressen toe te wijzen aan het plan, of u kunt de wijziging hand matig of met een script uitvoeren na een failover. 
Traffic Manager     | Ondersteund | U kunt Traffic Manager vooraf configureren zodat verkeer regel matig wordt gerouteerd naar het eind punt in de bron regio, en naar het eind punt in de doel regio in het geval van een failover.
Azure DNS | Ondersteund |
Aangepaste DNS  | Ondersteund |
Niet-geverifieerde proxy | Ondersteund | [Meer informatie](site-recovery-azure-to-azure-networking-guidance.md)    
Geverifieerde proxy | Niet ondersteund | Als de virtuele machine gebruikmaakt van een geverifieerde proxy voor uitgaande verbindingen, kan deze niet worden gerepliceerd met behulp van Azure Site Recovery.    
VPN site-naar-site-verbinding met on-premises<br/><br/>(met of zonder ExpressRoute)| Ondersteund | Zorg ervoor dat de Udr's en Nsg's zodanig zijn geconfigureerd dat de Site Recovery verkeer niet naar on-premises wordt gerouteerd. [Meer informatie](site-recovery-azure-to-azure-networking-guidance.md)    
VNET-naar-VNET-verbinding | Ondersteund | [Meer informatie](site-recovery-azure-to-azure-networking-guidance.md)  
Service-eindpunten voor virtueel netwerk | Ondersteund | Als u de toegang tot het virtuele netwerk beperkt tot opslag accounts, moet u ervoor zorgen dat de vertrouwde micro soft-Services toegang krijgen tot het opslag account.
Versnelde netwerken | Ondersteund | Versnelde netwerken moeten zijn ingeschakeld op de bron-VM. [Meer informatie](azure-vm-disaster-recovery-with-accelerated-networking.md).



## <a name="next-steps"></a>Volgende stappen
- Lees de [netwerk richtlijnen](site-recovery-azure-to-azure-networking-guidance.md) voor het repliceren van virtuele Azure-machines.
- Implementeer herstel na nood gevallen door [Azure vm's te repliceren](site-recovery-azure-to-azure.md).
