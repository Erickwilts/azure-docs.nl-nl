---
title: Herstel na noodgevallen van virtuele VMware-machines of fysieke servers naar een secundaire site met Azure Site Recovery instellen | Microsoft Docs
description: Meer informatie over het instellen van herstel na noodgeval voor VMware-VM's, of Windows en Linux-fysieke servers, naar een secundaire site met Azure Site Recovery.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
services: site-recovery
ms.topic: conceptual
ms.date: 05/30/2019
ms.author: raynew
ms.openlocfilehash: 9a1cb63bd2a209c72af608d23515723a63b180e1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66417726"
---
# <a name="set-up-disaster-recovery-of-on-premises-vmware-virtual-machines-or-physical-servers-to-a-secondary-site"></a>Herstel na noodgevallen van on-premises virtuele VMware-machines of fysieke servers naar een secundaire site instellen

InMage Scout in [Azure Site Recovery](site-recovery-overview.md) biedt realtime replicatie tussen on-premises VMware-sites. InMage Scout is opgenomen in de Azure Site Recovery-service-abonnementen.

## <a name="end-of-support-announcement"></a>Aankondiging van de end-van-ondersteuning

Het scenario met Azure Site Recovery voor replicatie tussen on-premises VMware of fysieke datacenters bereikt einde van ondersteuning.

-   Vanaf augustus 2018 het scenario kan niet worden geconfigureerd in de Recovery Services-kluis en de service InMage Scout-software kan niet worden gedownload uit de kluis. Bestaande implementaties worden ondersteund. 
-   Vanaf December 31 2020, wordt niet het scenario ondersteund.
- Bestaande partners kunnen Onboarding van nieuwe klanten aan het scenario tot het einde van ondersteuning.

Tijdens de 2018 en 2019 moet worden twee updates vrijgegeven: 

-   Update 7: Configuratie en compatibiliteit netwerkproblemen opgelost, en biedt ondersteuning voor TLS 1.2.
-   Bijwerken van 8: Ondersteuning voor Linux-besturingssystemen RHEL/CentOS 7.3/7.4/7.5 en SUSE 12 toevoegen

Na de Update 8, worden er kunnen geen nieuwe updates uitgebracht. Er is beperkte hotfix-ondersteuning voor de besturingssystemen die zijn toegevoegd in Update 8, en oplossingen voor problemen op basis van best-effort.

Azure Site Recovery blijft om te innoveren door te geven van VMware en Hyper-V-klanten een naadloze en de best mogelijke DRaaS-oplossing met Azure als een site voor noodherstel. Microsoft raadt aan dat bestaande InMage / ASR Scout klanten Overweeg het gebruik van Azure Site Recovery-VMware naar Azure voor hun zakelijke continuïteit. Azure Site Recovery van VMware naar Azure is een enterprise-klasse DR-oplossing voor VMware-toepassingen, biedt RPO en RTO van minuten, ondersteuning voor replicatie van multi-VM-toepassing en het herstel, naadloze onboarding uitgebreide bewaking en belangrijk voordeel van de totale Eigendomskosten.

### <a name="scenario-migration"></a>Scenario-migratie
Als alternatief, wordt aangeraden instellen van herstel na noodgevallen voor on-premises VMware-machines en fysieke machines door ze te repliceren naar Azure. Doe dit als volgt:

1.  Bekijk de onderstaande snelle vergelijking. Voordat u on-premises machines repliceren kunt, moet u controleren of ze voldoen aan [vereisten](./vmware-physical-azure-support-matrix.md#replicated-machines) voor replicatie naar Azure. Als u virtuele VMware-machines repliceert, raden wij aan dat u bekijkt [richtlijnen voor capaciteitsplanning](./site-recovery-plan-capacity-vmware.md), en voer de [hulpprogramma Deployment Planner](./site-recovery-deployment-planner.md) aan identiteit capaciteitsvereisten, en controleer of de naleving.
2.  Nadat de Deployment Planner is uitgevoerd, kunt u replicatie instellen: o voor VMware-VM's, volgt u deze zelfstudies [Azure voorbereiden](./tutorial-prepare-azure.md), [voorbereiden van uw on-premises VMware-omgeving](./vmware-azure-tutorial-prepare-on-premises.md), en [instellen herstel na noodgevallen](./vmware-azure-tutorial-prepare-on-premises.md).
o voor fysieke computers, volgt u deze [zelfstudie](./physical-azure-disaster-recovery.md).
3.  Als u machines repliceren naar Azure, kunt u uitvoeren een [Dr-herstelanalyse](./site-recovery-test-failover-to-azure.md) om te controleren of alles werkt zoals verwacht.

### <a name="quick-comparison"></a>Snelle vergelijking

**Functie** | **Replicatie naar Azure** |**Replicatie tussen datacentra met VMware**
--|--|--
**Vereiste onderdelen** |Mobility-service op gerepliceerde machines. On-premises configuratieserver, processerver, hoofddoelserver. Tijdelijke processerver in Azure voor failback.|Mobility-service, processerver, de configuratieserver en de Hoofddoelserver
**Configuratie en indeling** |Recovery Services-kluis in Azure portal | Met behulp van vContinuum 
**Gerepliceerd** |Schijf (Windows en Linux) |Volume-Windows<br> Disk-Linux
**Gedeeld schijfcluster** |Niet ondersteund|Ondersteund
**Gegevensverloop limieten (gemiddeld)** |Gegevens van 10 MB/s per schijf<br> Gegevens van 25MB/s per virtuele machine<br> [Meer informatie](./site-recovery-vmware-deployment-planner-analyze-report.md#azure-site-recovery-limits) | Gegevens voor > 10 MB/s per schijf  <br> De gegevens > 25 MB/s per virtuele machine
**Controle** |Vanuit Azure portal|Van CX (configuratieserver)
**Ondersteuningsmatrix** | [Klik hier voor meer informatie](./vmware-physical-azure-support-matrix.md)|[ASR Scout compatibel matrix downloaden](https://aka.ms/asr-scout-cm)


## <a name="prerequisites"></a>Vereisten
Vereisten voor het voltooien van deze zelfstudie:

- [Beoordeling](vmware-physical-secondary-support-matrix.md) de vereisten van het ondersteuningsteam voor alle onderdelen.
- Zorg ervoor dat de machines die u wilt repliceren, aan de voldoet [gerepliceerde machine ondersteuning](vmware-physical-secondary-support-matrix.md#replicated-vm-support).


## <a name="download-and-install-component-updates"></a>Onderdeelupdates downloaden en installeren

 Bekijk en installeer de meest recente [updates](#updates). Updates moeten worden geïnstalleerd op servers in de volgende volgorde:

1. RX-server (indien van toepassing)
2. Configuratieservers
3. Processervers
4. Doelservers van master
5. vContinuum-servers
6. Bronserver (zowel Windows als Linux-Servers)

Als volgt te werk om de updates te installeren:

> [!NOTE]
>Updateversie alle Scout-onderdelen mogelijk niet hetzelfde als in het ZIP-bestand bijwerken. De oudere versie geven aan dat er is geen wijziging in de component sinds de vorige update voor deze update.

Download de [bijwerken](https://aka.ms/asr-scout-update7) ZIP-bestand en de [MySQL en PHP upgrade](https://aka.ms/asr-scout-u7-mysql-php-manualupgrade) -configuratiebestanden. De update-ZIP-bestand bevat de alle base binaire bestanden en de cumulatieve upgrade binaire bestanden van de volgende onderdelen: 
- InMage_ScoutCloud_RX_8.0.1.0_RHEL6-64_GA_02Mar2015.tar.gz
- RX_8.0.7.0_GA_Update_7_2965621_28Dec18.tar.gz
- InMage_CX_8.0.1.0_Windows_GA_26Feb2015_release.exe
- InMage_CX_TP_8.0.1.0_Windows_GA_26Feb2015_release.exe
- CX_Windows_8.0.7.0_GA_Update_7_2965621_28Dec18.exe
- InMage_PI_8.0.1.0_Windows_GA_26Feb2015_release.exe
- InMage_Scout_vContinuum_MT_8.0.7.0_Windows_GA_27Dec2018_release.exe
- InMage_UA_8.0.7.0_Windows_GA_27Dec2018_release.exe
- InMage_UA_8.0.7.0_OL5-32_GA_03Dec2018_release.tar.gz
- InMage_UA_8.0.7.0_OL5-64_GA_03Dec2018_release.tar.gz
- InMage_UA_8.0.7.0_OL6-32_GA_03Dec2018_release.tar.gz
- InMage_UA_8.0.7.0_OL6-64_GA_03Dec2018_release.tar.gz
- InMage_UA_8.0.7.0_RHEL5-32_GA_03Dec2018_release.tar.gz
- InMage_UA_8.0.7.0_RHEL5-64_GA_03Dec2018_release.tar.gz
- InMage_UA_8.0.7.0_RHEL6-32_GA_03Dec2018_release.tar.gz
- InMage_UA_8.0.7.0_RHEL6-64_GA_03Dec2018_release.tar.gz
- InMage_UA_8.0.7.0_RHEL7-64_GA_03Dec2018_release.tar.gz
- InMage_UA_8.0.7.0_SLES10-32_GA_03Dec2018_release.tar.gz
- InMage_UA_8.0.7.0_SLES10-64_GA_03Dec2018_release.tar.gz
- InMage_UA_8.0.7.0_SLES10-SP1-32_GA_03Dec2018_release.tar.gz
- InMage_UA_8.0.7.0_SLES10-SP1-64_GA_03Dec2018_release.tar.gz
- InMage_UA_8.0.7.0_SLES10-SP2-32_GA_03Dec2018_release.tar.gz
- InMage_UA_8.0.7.0_SLES10-SP2-64_GA_03Dec2018_release.tar.gz
- InMage_UA_8.0.7.0_SLES10-SP3-32_GA_03Dec2018_release.tar.gz
- InMage_UA_8.0.7.0_SLES10-SP3-64_GA_03Dec2018_release.tar.gz
- InMage_UA_8.0.7.0_SLES10-SP4-32_GA_03Dec2018_release.tar.gz
- InMage_UA_8.0.7.0_SLES10-SP4-64_GA_03Dec2018_release.tar.gz
- InMage_UA_8.0.7.0_SLES11-32_GA_03Dec2018_release.tar.gz
- InMage_UA_8.0.7.0_SLES11-64_GA_04Dec2018_release.tar.gz
- InMage_UA_8.0.7.0_SLES11-SP1-32_GA_03Dec2018_release.tar.gz
- InMage_UA_8.0.7.0_SLES11-SP1-64_GA_04Dec2018_release.tar.gz
- InMage_UA_8.0.7.0_SLES11-SP2-32_GA_03Dec2018_release.tar.gz
- InMage_UA_8.0.7.0_SLES11-SP2-64_GA_03Dec2018_release.tar.gz
- InMage_UA_8.0.7.0_SLES11-SP3-32_GA_03Dec2018_release.tar.gz
- InMage_UA_8.0.7.0_SLES11-SP3-64_GA_03Dec2018_release.tar.gz
- InMage_UA_8.0.7.0_SLES11-SP4-64_GA_03Dec2018_release.tar.gz
  1. Pak de ZIP-bestanden.
  2. **RX server**: Kopie **RX_8.0.7.0_GA_Update_7_2965621_28Dec18.tar.gz** naar de server RX, en pak het uit. Voer in de uitgepakte map **/Install**.
  3. **Configuratieserver en processerver**: Kopie **CX_Windows_8.0.7.0_GA_Update_7_2965621_28Dec18.exe** op de configuratieserver en processerver. Dubbelklik op uit te voeren.<br>
  4. **Windows hoofddoelserver**: Voor het bijwerken van de unified agent kopiëren **InMage_UA_8.0.7.0_Windows_GA_27Dec2018_release.exe** naar de server. Dubbelklik erop uit te voeren. Hetzelfde bestand kan ook worden gebruikt voor nieuwe installatie. De unified agentupdate is ook van toepassing op de bronserver.
  De update niet wilt toepassen op de Master target voorbereid met **InMage_Scout_vContinuum_MT_8.0.7.0_Windows_GA_27Dec2018_release.exe** omdat deze nieuwe algemene beschikbaarheid installatieprogramma met de meest recente wijzigingen.
  5. **vContinuum-server**:  Kopie **InMage_Scout_vContinuum_MT_8.0.7.0_Windows_GA_27Dec2018_release.exe** naar de server.  Zorg ervoor dat u de wizard vContinuum hebt gesloten. Dubbelklik op het bestand uit te voeren.
  6. **Linux-hoofddoelserver**: Voor het bijwerken van de unified agent kopiëren **InMage_UA_8.0.7.0_RHEL6-64_GA_03Dec2018_release.tar.gz** naar de server voor Linux-hoofddoel en pak het uit. Voer in de uitgepakte map **/Install**.
  7. **Windows-bronserver**: Voor het bijwerken van de unified agent kopiëren **InMage_UA_8.0.7.0_Windows_GA_27Dec2018_release.exe** naar de bronserver. Dubbelklik op het bestand uit te voeren. 
  8. **Linux-bronserver**: De unified agent bijwerken, de bijbehorende versie van het bestand de unified agent kopiëren naar de Linux-server en pak het uit. Voer in de uitgepakte map **/Install**.  Voorbeeld: Voor RHEL 6.7 64-bits-server, kopie **InMage_UA_8.0.7.0_RHEL6-64_GA_03Dec2018_release.tar.gz** naar de server, en pak het uit. Voer in de uitgepakte map **/Install**.
  9. Na de upgrade configuratieserver, processerver en RX-server met de hierboven genoemde installatieprogramma's, de PHP- en MySQL-bibliotheken moet handmatig worden bijgewerkt met de stappen in de sectie 7.4 van de [handleiding voor snelle installatie](https://aka.ms/asr-scout-quick-install-guide).

## <a name="enable-replication"></a>Replicatie inschakelen

1. Instellen van replicatie tussen de bron en doel van VMware-sites.
2. Raadpleeg de volgende documenten voor meer informatie over installatie, beveiliging en herstel:

   * [Releaseopmerkingen](https://aka.ms/asr-scout-release-notes)
   * [Compatibiliteitsmatrix](https://aka.ms/asr-scout-cm)
   * [Gebruikershandleiding](https://aka.ms/asr-scout-user-guide)
   * [RX-gebruikershandleiding](https://aka.ms/asr-scout-rx-user-guide)
   * [Handleiding voor snelle installatie](https://aka.ms/asr-scout-quick-install-guide)
   * [Een upgrade van MYSQL en PHP-bibliotheken](https://aka.ms/asr-scout-u7-mysql-php-manualupgrade)

## <a name="updates"></a>Updates

### <a name="site-recovery-scout-801-update-7"></a>Site Recovery Scout 8.0.1 Update 7 
Bijgewerkt: En met 31 december 2018 downloaden [Scout update 7](https://aka.ms/asr-scout-update7).
Scout Update 7 is een volledige installatieprogramma die kan worden gebruikt voor nieuwe installatie ook over het bijwerken van bestaande agents/MT die zich in de vorige updates (van Update 1 tot en met Update 6). Deze bevat alle oplossingen van Update 1 voor Update 6 plus de nieuwe oplossingen en verbeteringen die hieronder worden beschreven.
 
#### <a name="new-features"></a>Nieuwe functies
* PCI-naleving
* TLS 1.2-ondersteuning

#### <a name="bug-and-security-fixes"></a>Fout- en beveiligingsproblemen
* Opgelost: Windows Cluster/zelfstandige Machines hebben onjuiste IP-configuratie van herstel/DR-oefening.
* Opgelost: Soms mislukt schijfbewerking toevoegen voor V2V-cluster.
* Opgelost: vContinuum Wizard vastloopt tijdens de herstelfase als het hoofddoel Windows Server 2016 wordt
* Opgelost: MySQL-beveiligingsproblemen worden verholpen door de MySQL te upgraden naar versie 5.7.23

#### <a name="manual-upgrade-for-php-and-mysql-on-csps-and-rx"></a>Handmatige Upgrade voor PHP en MySQL in CS-, PS- en RX
De PHP-scriptplatform dat moet worden bijgewerkt naar versie 7.2.10 op de configuratieserver, processerver en RX-Server.
Het MySQL-database management systeem moet worden bijgewerkt naar versie 5.7.23 op de configuratieserver, processerver en RX-Server.
Volg de handmatige stappen die in de [handleiding voor snelle installatie](https://aka.ms/asr-scout-quick-install-guide) om bij te werken van PHP en MySQL-versies.

### <a name="site-recovery-scout-801-update-6"></a>Site Recovery Scout 8.0.1 Update 6 
Bijgewerkt: 12 oktober 2017

Download [Scout update 6](https://aka.ms/asr-scout-update6).

Scout Update 6 is een cumulatieve update. Deze bevat alle oplossingen van Update 1 voor Update 5 plus de nieuwe oplossingen en verbeteringen die hieronder worden beschreven. 

#### <a name="new-platform-support"></a>Platformondersteuning voor nieuwe
* Is er ondersteuning toegevoegd voor bron Windows Server 2016
* Is er ondersteuning toegevoegd voor de volgende Linux-besturingssystemen:
    - Red Hat Enterprise Linux (RHEL) 6.9
    - CentOS 6.9
    - Oracle Linux 5.11
    - Oracle Linux 6,8
* Is er ondersteuning toegevoegd voor VMware Center 6.5

Als volgt te werk om de updates te installeren:

> [!NOTE]
>Updateversie alle Scout-onderdelen mogelijk niet hetzelfde als in het ZIP-bestand bijwerken. De oudere versie geven aan dat er is geen wijziging in de component sinds de vorige update voor deze update.

Download de [bijwerken](https://aka.ms/asr-scout-update6) ZIP-bestand. Het bestand bevat de volgende onderdelen: 
- RX_8.0.4.0_GA_Update_4_8725872_16Sep16.tar.gz
- CX_Windows_8.0.6.0_GA_Update_6_13746667_18Sep17.exe
- UA_Windows_8.0.5.0_GA_Update_5_11525802_20Apr17.exe
- UA_RHEL6-64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz
- vCon_Windows_8.0.6.0_GA_Update_6_11525767_21Sep17.exe
- UA update4 bits voor RHEL5, OL5, OL6, SUSE-10, SUSE-11: UA_\<Linux OS>_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz
  1. Pak de ZIP-bestanden.
  2. **RX server**: Kopie **RX_8.0.4.0_GA_Update_4_8725872_16Sep16.tar.gz** naar de server RX, en pak het uit. Voer in de uitgepakte map **/Install**.
  3. **Configuratieserver en processerver**: Kopie **CX_Windows_8.0.6.0_GA_Update_6_13746667_18Sep17.exe** op de configuratieserver en processerver. Dubbelklik op uit te voeren.<br>
  4. **Windows hoofddoelserver**: Voor het bijwerken van de unified agent kopiëren **UA_Windows_8.0.5.0_GA_Update_5_11525802_20Apr17.exe** naar de server. Dubbelklik erop uit te voeren. De unified agentupdate is ook van toepassing op de bronserver. Als bron nog niet is bijgewerkt naar Update 4, moet u de unified agent bijwerken.
  De update niet wilt toepassen op de Master target voorbereid met **InMage_Scout_vContinuum_MT_8.0.1.0_Windows_GA_10Oct2017_release.exe** omdat deze nieuwe algemene beschikbaarheid installatieprogramma met de meest recente wijzigingen.
  5. **vContinuum-server**:  Copy **vCon_Windows_8.0.6.0_GA_Update_6_11525767_21Sep17.exe** to the server.  Zorg ervoor dat u de wizard vContinuum hebt gesloten. Dubbelklik op het bestand uit te voeren.
  De update niet wilt toepassen op de Hoofddoelserver voorbereid met **InMage_Scout_vContinuum_MT_8.0.1.0_Windows_GA_10Oct2017_release.exe** omdat deze nieuwe algemene beschikbaarheid installatieprogramma met de meest recente wijzigingen.
  6. **Linux-hoofddoelserver**: Voor het bijwerken van de unified agent kopiëren **UA_RHEL6 64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz** naar het hoofdniveau doelserver en pak het uit. Voer in de uitgepakte map **/Install**.
  7. **Windows-bronserver**: Voor het bijwerken van de unified agent kopiëren **UA_Windows_8.0.5.0_GA_Update_5_11525802_20Apr17.exe** naar de bronserver. Dubbelklik op het bestand uit te voeren. 
  U hoeft te installeren van de Update 5-agent op de bronserver als deze al is bijgewerkt naar Update 4 of bronagent is geïnstalleerd met het nieuwste installatieprogramma van base **InMage_UA_8.0.1.0_Windows_GA_28Sep2017_release.exe**.
  8. **Linux-bronserver**: De unified agent bijwerken, de bijbehorende versie van het bestand de unified agent kopiëren naar de Linux-server en pak het uit. Voer in de uitgepakte map **/Install**.  Voorbeeld: Voor RHEL 6.7 64-bits-server, kopie **UA_RHEL6 64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz** naar de server, en pak het uit. Voer in de uitgepakte map **/Install**.


> [!NOTE]
> * Basis Agent(UA) Unified installer voor Windows is vernieuwd voor ondersteuning van Windows Server 2016. Het nieuwe installatieprogramma **InMage_UA_8.0.1.0_Windows_GA_28Sep2017_release.exe** wordt geleverd met de basis Scout GA-pakket (**InMage_Scout_Standard_8.0.1 GA-Oct17.zip**). Het installatieprogramma van dezelfde wordt gebruikt voor alle ondersteunde Windows-versie. 
> * Basis Windows vContinuum & hoofddoel installer is vernieuwd voor ondersteuning van Windows Server 2016. Het nieuwe installatieprogramma **InMage_Scout_vContinuum_MT_8.0.1.0_Windows_GA_10Oct2017_release.exe** wordt geleverd met de basis Scout GA-pakket (**InMage_Scout_Standard_8.0.1 GA-Oct17.zip**). Het installatieprogramma van hetzelfde hoofddoel voor Windows 2016 en Windows 2012R2 hoofddoel implementeren gebruikt.
> * Windows server 2016 op de fysieke server wordt niet ondersteund door ASR Scout. Het ondersteunt alleen Windows Server 2016 virtuele VMware-machine. 
>

#### <a name="bug-fixes-and-enhancements"></a>Oplossingen voor problemen en verbeteringen
- Failback beveiliging mislukt voor Linux-VM met de lijst met schijven worden gerepliceerd, is leeg aan het einde van de configuratie.

### <a name="site-recovery-scout-801-update-5"></a>Site Recovery Scout 8.0.1 Update 5
Scout Update 5 is een cumulatieve update. Het bevat alle correcties uit Update 1 naar Update 4 en de oplossingen die hieronder worden beschreven.
- Oplossingen van Site Recovery Scout Update 4 tot en met Update 5 zijn specifiek voor de master en doel en de vContinuum-onderdelen.
- Als de bronservers, het hoofddoel, configuratie, proces en RX-servers worden al Update 4 worden uitgevoerd, klikt u vervolgens van deze toepassing alleen op de hoofddoelserver. 

#### <a name="new-platform-support"></a>Platformondersteuning voor nieuwe
* SUSE Linux Enterprise Server 11 Service Pack 4(SP4)
* SLES 11 SP4 64 bits **InMage_UA_8.0.1.0_SLES11-SP4-64_GA_13Apr2017_release.tar.gz** wordt geleverd met de basis Scout GA-pakket (**InMage_Scout_Standard_8.0.1 GA.zip**). Download het pakket voor algemene beschikbaarheid van de portal, zoals beschreven in een kluis maken.


#### <a name="bug-fixes-and-enhancements"></a>Oplossingen voor problemen en verbeteringen

* Oplossingen voor verbeterde Windows cluster bieden ondersteuning voor betrouwbaarheid:
    * Opgelost: enkele van de P2V-MSCS clusterschijven worden RAW na het herstel.
    * Fixed-P2V MSCS-cluster herstellen mislukt vanwege een niet-overeenkomend schijf volgorde.
    * Vaste - de MSCS-cluster bewerking voor het toevoegen van schijven mislukt met een fout met niet overeenkomende grootte voor schijf.
    * Vaste de gereedheid controleren op de bron MSCS-cluster met de toewijzing van RDM-LUN's in de controle van het formaat is mislukt.
    * Fixed-één knooppunt cluster beveiliging mislukt vanwege een probleem met de SCSI-probleem. 
    * Fixed-opnieuw beveiliging van de P2V-Windows-clusterserver mislukt als clusterschijven doel aanwezig zijn. 
    
* Opgelost: Tijdens failback beveiliging, als de geselecteerde master doelserver bevindt zich niet op dezelfde ESXi-server als de beveiligde bron-VM (tijdens forward beveiliging), en vervolgens de verkeerde hoofddoelserver vContinuum tijdens failback herstel en het herstel neemt bewerking is mislukt.

> [!NOTE]
> * De P2V-cluster-oplossingen zijn alleen van toepassing op fysieke MSCS-clusters die onlangs zijn beveiligd met Site Recovery Scout Update 5. Als u wilt de cluster-oplossingen op beveiligde P2V MSCS-clusters met oudere updates hebt geïnstalleerd, volgt u de upgrade stappen in de sectie 12 van de [opmerkingen bij de Site Recovery Scout Release](https://aka.ms/asr-scout-release-notes).
> * Als op het moment van opnieuw beveiliging, dezelfde set schijven actief op elk van de clusterknooppunten zijn zoals ze in eerste instantie beveiligd waren, klikt u vervolgens opnieuw beveiliging van een fysieke MSCS-cluster kan alleen opnieuw gebruiken bestaande doel-schijven. Als dat niet het geval is, klikt u vervolgens de handmatige stappen gebruiken in de sectie 12 van [opmerkingen bij de Site Recovery Scout Release](https://aka.ms/asr-scout-release-notes), de doel-kant schijven verplaatsen naar het juiste gegevensarchief pad, voor gebruik tijdens opnieuw beveiliging. Als u opnieuw de MSCS-cluster in de modus voor P2V beveiligen zonder dat de upgrade stappen te volgen, maakt een nieuwe schijf op de doel-ESXi-server. U moet de oude schijven handmatig verwijderen uit het gegevensarchief.
> * Wanneer een bron SLES11 of SLES11 (met alle servicepacks)-server opnieuw is gestart zonder problemen handmatig markeert vervolgens de **hoofdmap** replicatieparen voor hernieuwde synchronisatie schijf. Er is geen melding in de CX-interface. Als u niet de hoofdmap-schijf voor hersynchronisatie markeren, ziet u mogelijk problemen met de gegevensintegriteit.


### <a name="azure-site-recovery-scout-801-update-4"></a>Azure Site Recovery Scout 8.0.1 Update 4
Scout Update 4 is een cumulatieve update. Deze bevat alle oplossingen van Update 1 voor Update 3 en de oplossingen die hieronder worden beschreven.

#### <a name="new-platform-support"></a>Platformondersteuning voor nieuwe

* Is er ondersteuning toegevoegd voor vCenter/vSphere 6.0, 6.1 en 6.2
* Is er ondersteuning toegevoegd voor deze Linux-besturingssystemen:
  * Red Hat Enterprise Linux (RHEL) 7.0, 7.1 en 7.2
  * CentOS 7.0, 7.1 en 7.2
  * Red Hat Enterprise Linux (RHEL) 6.8
  * CentOS 6,8

> [!NOTE]
> RHEL/CentOS 7 64-bits **InMage_UA_8.0.1.0_RHEL7-64_GA_06Oct2016_release.tar.gz** wordt geleverd met het Basispakket voor Scout GA **InMage_Scout_Standard_8.0.1 GA.zip**. Download het pakket Scout algemeen beschikbaar vanaf de portal zoals wordt beschreven in een kluis maken.

#### <a name="bug-fixes-and-enhancements"></a>Oplossingen voor problemen en verbeteringen

* Verbeterde verwerking voor de volgende Linux-besturingssystemen en klonen, om te voorkomen dat ongewenste hersynchronisatie problemen wordt afgesloten:
    * Red Hat Enterprise Linux (RHEL) 6.x
    * Oracle Linux (OL) 6.x
* Voor Linux, zijn nu alle toegang tot machtigingen voor mappen in de installatiemap van de unified agent wordt beperkt tot alleen de lokale gebruiker.
* Op Windows, een oplossing voor een time-out bij probleem dat is opgetreden bij de uitgifte van algemene consistentie van gedistribueerde bladwijzers belast op zwaar gedistribueerde toepassingen zoals SQL Server en SharePoint-clusters.
* Een oplossing voor gerelateerde log in het installatieprogramma voor een basis van de configuratie van server.
* Een downloadkoppeling naar VMware vCLI 6.0 is toegevoegd aan het installatieprogramma van Windows-hoofddoel-basis.
* Aanvullende controles en logboeken zijn toegevoegd voor wijzigingen in de configuratie tijdens failover en herstel na noodgeval noodhersteloefeningen netwerk.
* Een oplossing voor een probleem waardoor retentie-informatie niet te worden gerapporteerd aan de configuratieserver.  
* Fysieke clusters, een oplossing voor een probleem waardoor volume vergroten of verkleinen als u wilt in de wizard vContinuum mislukt tijdens het verkleinen van het bronvolume.
* Een oplossing voor beveiliging probleem met een cluster dat is mislukt met fout: 'Is mislukt voor het vinden van de schijfhandtekening' wanneer de clusterschijf is een PRDM-schijf.
* Een oplossing voor cxps transport-server vastlopen van een veroorzaakt door een uitzondering van buiten het bereik.
* Naam en IP-adres kolommen zijn nu vergroten/verkleinen in de **Push-installatie** pagina van de wizard vContinuum.
* RX-API-uitbreidingen:
  * De vijf meest recente beschikbare algemene consistentie verwijst nu beschikbaar (alleen gegarandeerd tags).
  * Capaciteit en beschikbare ruimte details worden weergegeven voor alle beveiligde apparaten.
  * Scout stuurprogramma staat op de bronserver is beschikbaar.

> [!NOTE]
> * **InMage_Scout_Standard_8.0.1_GA.zip** basispakket heeft:
>     * Het installatieprogramma voor een basis van een bijgewerkte configuratie server (**InMage_CX_8.0.1.0_Windows_GA_26Feb2015_release.exe**)
>     * A Windows master target base installer (**InMage_Scout_vContinuum_MT_8.0.1.0_Windows_GA_26Feb2015_release.exe**).
>     * Gebruik voor alle nieuwe installaties van de nieuwe configuratie- en Windows hoofddoel GA bits.
> * Update 4 kan worden toegepast op 8.0.1 algemene beschikbaarheid.
> * De configuratieserver en RX updates kunnen niet worden hersteld nadat ze zijn toegepast.


### <a name="azure-site-recovery-scout-801-update-3"></a>Azure Site Recovery Scout 8.0.1 Update 3

Alle Site Recovery-updates zijn cumulatief. Update 3 bevat alle oplossingen van de Update 1 en 2 van de Update. Update 3 kan rechtstreeks worden toegepast op 8.0.1 algemene beschikbaarheid. De configuratieserver en RX updates kunnen niet worden hersteld nadat ze zijn toegepast.

#### <a name="bug-fixes-and-enhancements"></a>Oplossingen voor problemen en verbeteringen
Update 3 worden de volgende problemen:

* De configuratieserver en RX worden niet geregistreerd in de kluis als ze achter de proxy.
* Het aantal uren waarin het beoogde herstelpunt (RPO) niet is bereikt, wordt niet bijgewerkt in het statusrapport.
* De configuratieserver is niet gesynchroniseerd met RX wanneer de ESX-hardwaregegevens of netwerkdetails, UTF-8 tekens bevatten.
* Windows Server 2008 R2-domeincontrollers starten niet na het herstel.
* Offline synchronisatie werkt niet zoals verwacht.
* Niet na een failover van de virtuele machine voortgang gedurende een lange periode replicatie-paar verwijderen in de console van de configuratie-server. Gebruikers kunnen de failback voltooien of bewerkingen hervatten.
* Algemene momentopnamebewerkingen met de consistentietaak zijn geoptimaliseerd, zodat de toepassing verminderen de verbinding verbreekt, zoals SQL Server-clients.
* Consistentie hulpprogramma (VACP.exe) prestaties zijn verbeterd. Geheugengebruik is vereist voor het maken van momentopnamen op Windows, is verminderd.
* De push-installatie van service loopt vast bij het wachtwoord is groter dan 16 tekens.
* vContinuum niet controleren en nieuwe vCenter referenties gevraagd wanneer referenties zijn gewijzigd.
* Op Linux, is niet hoofddoel Cachebeheer (cachemgr) downloaden van bestanden vanaf de processerver. Dit resulteert in replicatie paar beperking.
* Als de fysieke failover cluster (MSCS)-schijforder is niet hetzelfde zijn op alle knooppunten, moet replicatie is niet ingesteld voor enkele van de clustervolumes. Het cluster moet opnieuw worden beveiligd om te profiteren van deze oplossing.  
* SMTP-functionaliteit werkt niet zoals verwacht, nadat RX Scout 7.1 is bijgewerkt naar Scout 8.0.1.
* Meer statistieken zijn toegevoegd in het logboek voor de terugdraaibewerking voor het bijhouden van de tijd die nodig om deze te voltooien.
* Is er ondersteuning toegevoegd voor Linux-besturingssystemen op de bronserver:
  * Red Hat Enterprise Linux (RHEL) 6 update 7
  * CentOS 6 update 7
* De configuratieserver en RX consoles nu meldingen weergeven voor het paar, krijgt de bitmapmodus.
* De volgende oplossingen zijn toegevoegd in RX:
    * Autorisatie negeren via de parameter manipulatie: Beperkte toegang tot gebruikers die niet van toepassing zijn.
    * Aanvraag voor cross-site kunnen worden vervalst: Het concept van pagina-token is geïmplementeerd en wordt willekeurig gegenereerd voor elke pagina. Dit betekent dat er is slechts één aanmelding exemplaar voor dezelfde gebruiker en de pagina vernieuwen werkt niet. In plaats daarvan wordt hij omgeleid naar het dashboard.
    * Schadelijk bestand uploaden: Bestanden zijn beperkt tot specifieke extensies: z, aiff, AVP, avi, bmp, csv, doc, docx, fla, FLV-, GIF-, gz, gzip, JPEG-, jpg-, logboek, mid mov, mp3, mp4, mpc, mpeg, mpg, ods odt, PDF-, png, ppt, pptx, pxd, qt, ram, rar, DB, rmi, rmvb, RTF-, sdc, sitd, swf , sxc, sxw, tar, tgz, tif, TIFF-, txt, vsd, wav, wma, wmv, xls, xlsx, xml en postcode.
    * Permanente cross-site scripting: Invoer validaties zijn toegevoegd.

### <a name="azure-site-recovery-scout-801-update-2-update-03dec15"></a>Azure Site Recovery Scout 8.0.1 Update 2 (Update 03Dec15)

Verbeteringen in Update 2 zijn onder andere:

* **Configuratieserver**: Problemen waardoor de functie 31 dagen gratis voor softwarelicentiecontrole niet werkt zoals verwacht wordt bij de configuratieserver is geregistreerd bij Azure Site Recovery-kluis.
* **De Unified agent**: Oplossing voor een probleem in Update 1 dat heeft geresulteerd in de update niet op de hoofddoelserver geïnstalleerd tijdens de upgrade van versie 8.0-8.0.1.

### <a name="azure-site-recovery-scout-801-update-1"></a>Azure Site Recovery Scout 8.0.1 Update 1
Update 1 omvat de volgende oplossingen voor problemen en nieuwe functies:

* 31 dagen gratis beveiliging per server-exemplaar. Hiermee kunt u functionaliteit voor het testen, of stel een proof-of-concept.
* Alle bewerkingen op de server, met inbegrip van failover en failback, zijn voor de eerste 31 dagen gratis. De tijd wordt gestart wanneer een server eerst is beveiligd met Site Recovery Scout. Vanaf de 32e dag, elke beschermde server wordt in rekening gebracht tegen het tarief standaardinstantie voor Site Recovery-beveiliging met een site klanten.
* Het aantal beveiligde servers die momenteel in rekening gebracht op elk gewenst moment is beschikbaar op de **Dashboard** in de kluis.
* Ondersteuning is toegevoegd voor vSphere-opdrachtregelinterface (vCLI) 5.5 Update 2.
* Er is ondersteuning toegevoegd voor deze Linux-besturingssystemen op de bronserver:
    * RHEL 6 Update 6
    * RHEL 5 Update 11
    * CentOS 6 Update 6
    * CentOS 5 Update 11
* Problemen opgelost voor de volgende problemen op te lossen:
  * Het registreren van de kluis is mislukt voor de configuratieserver of RX-server.
  * Clustervolumes worden niet weergegeven als verwachte als geclusterde dat virtuele machines opnieuw zijn beveiligd als ze hervatten.
  * Failback mislukt wanneer de hoofddoelserver wordt gehost op een andere ESXi-server van de productie-on-premises VM's.
  * Bestandsmachtigingen configuratie worden gewijzigd wanneer u een naar 8.0.1 upgrade. Deze wijziging is van invloed op beveiliging en bewerkingen.
  * De drempelwaarde voor hersynchronisatie wordt niet afgedwongen zoals verwacht, waardoor replicatie inconsistent gedrag.
  * De RPO-instellingen weergegeven niet correct in de console van de configuratie-server. De waarde van de niet-gecomprimeerde gegevens geeft ten onrechte de gecomprimeerde waarde.
  * De verwijderbewerking niet zoals verwacht in de wizard vContinuum verwijderd en replicatie wordt niet verwijderd uit de console van de configuratie-server.
  * In de wizard vContinuum de schijf is automatisch uitgeschakeld wanneer u klikt op **Details** in de weergave schijf tijdens de beveiliging van virtuele machines MSCS.
  * Vereiste HP-services (zoals CIMnotify en CqMgHost) worden niet in het scenario voor fysiek naar virtueel (P2V) verplaatst naar de handleiding bij het herstellen van de virtuele machine. Dit probleem resulteert in extra opstarten.
  * Linux VM-beveiliging mislukt wanneer er meer dan 26 schijven op de hoofddoelserver zijn.

