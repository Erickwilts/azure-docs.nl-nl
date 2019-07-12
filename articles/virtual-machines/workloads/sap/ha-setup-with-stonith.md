---
title: Hoge beschikbaarheid instellen met stonith instellen voor SAP HANA op Azure (grote instanties) | Microsoft Docs
description: Hoge beschikbaarheid voor SAP HANA op Azure (grote exemplaren) instellen in SUSE met behulp van de stonith instellen
services: virtual-machines-linux
documentationcenter: ''
author: saghorpa
manager: gwallace
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 11/21/2017
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c7cbec63cb04075977c167d8b21bf3128e91434f
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67710042"
---
# <a name="high-availability-set-up-in-suse-using-the-stonith"></a>Hoge beschikbaarheid in SUSE met behulp van de stonith instellen instellen
Dit document bevat de gedetailleerde stapsgewijze instructies voor het instellen van de hoge beschikbaarheid in SUSE-besturingssysteem met behulp van het apparaat stonith instellen.

**Disclaimer:** *Deze handleiding wordt berekend door het testen van de instellingen in de omgeving Microsoft HANA grote instanties, die correct werkt. Voordat het besturingssysteem biedt geen ondersteuning voor Microsoft-Service Management-team voor HANA grote instanties, moet u contact op met SUSE voor eventuele problemen op te lossen of uitleg over de besturingssysteem-laag. Microsoft service management-team stonith instellen-apparaat ingesteld en volledig ondersteunt en kan worden betrokken bij voor het oplossen van problemen met het apparaat van de stonith instellen.*
## <a name="overview"></a>Overzicht
Als u de hoge beschikbaarheid met behulp van SUSE clustering instelt, moeten aan de volgende vereisten voldoen.
### <a name="pre-requisites"></a>Vereisten
- HANA grote instanties zijn ingericht
- Besturingssysteem is geregistreerd
- HANA grote instanties servers zijn verbonden met SMT-server om op te halen van patches /-pakketten
- Besturingssysteem hebben de nieuwste patches
- NTP (tijdserver) is ingesteld
- Lezen en begrijpen van de meest recente versie van SUSE-documentatie op HA-installatie

### <a name="setup-details"></a>Details van Setup
Deze handleiding gebruikt de volgende instellingen:
- Besturingssysteem: SLES 12 SP1 voor SAP
- HANA grote instanties: 2xS192 (4-sockets, 2 TB)
- HANA-versie: HANA 2.0 SP1
- Servernamen: sapprdhdb95 (knooppunt1) en sapprdhdb96 (Knooppunt2)
- Het apparaat is stonith instellen: apparaat stonith instellen op basis van iSCSI
- NTP is ingesteld op een van de knooppunten HANA grote instantie

Bij het instellen van HANA grote instanties met HSR, kunt u Microsoft-Service Management-team om in te stellen stonith instellen aanvragen. Als u al een bestaande klant die HANA grote instanties die worden ingericht en stonith instellen-apparaat instellen voor uw bestaande blades nodig heeft, moet u de volgende informatie voor Microsoft-Service Management-team in het formulier in het service-aanvraag (SRF) opgeven. U kunt aanvragen SRF formulier via het Technical Account Manager of uw Microsoft-contactpersoon voor de voorbereiding op HANA grote instantie. De nieuwe klanten kunnen apparaat stonith instellen op het moment van de inrichting van aanvragen. De invoer zijn beschikbaar in het aanvraagformulier voor de inrichting.

- De naam van server en Server IP-adres (bijvoorbeeld myhanaserver1, 10.35.0.1)
- Locatie (bijvoorbeeld VS Oost)
- Naam van de klant (bijvoorbeeld Microsoft)
- SID - HANA systeem-id (bijvoorbeeld H11)

Nadat het apparaat stonith instellen is geconfigureerd, biedt Microsoft-Service Management-team u de SBD apparaatnaam en het IP-adres van de iSCSI-opslag, die u gebruiken kunt om te configureren instellingen stonith instellen. 

Als u de end-to-HA met stonith instellen instelt, moet de volgende stappen worden gevolgd:

1.  Identificatie van het apparaat SBD
2.  Het apparaat SBD initialiseren
3.  Het Cluster te configureren
4.  Instellen van de Softdog Watchdog
5.  Het knooppunt aan het cluster worden toegevoegd
6.  Het cluster valideren
7.  De bronnen aan het cluster configureren
8.  Het failoverproces testen

## <a name="1---identify-the-sbd-device"></a>1.   Identificatie van het apparaat SBD
In deze sectie vindt u instructies voor het vaststellen van het apparaat SBD voor uw installatie nadat Microsoft service management-team is geconfigureerd dat de stonith instellen. **In deze sectie is alleen van toepassing op de bestaande klant**. Als u een nieuwe klant bent, biedt Microsoft service management-team SBD apparaatnaam voor u en u kunt deze sectie overslaan.

1.1 wijzigen */etc/iscsi/initiatorname.isci* aan 
``` 
iqn.1996-04.de.suse:01:<Tenant><Location><SID><NodeNumber> 
```

Beheer van Microsoft-service biedt deze tekenreeks. Wijzigen van het bestand op **beide** de knooppunten, het aantal knooppunten is echter anders uit op elk knooppunt.

![initiatorname.png](media/HowToHLI/HASetupWithStonith/initiatorname.png)

1.2 wijzigen */etc/iscsi/iscsid.conf*: Stel *node.session.timeo.replacement_timeout=5* en *node.startup = automatische*. Wijzigen van het bestand op **beide** de knooppunten.

1.3 de detectie-opdracht uitvoeren, vier sessies worden weergegeven. Uitvoeren op beide knooppunten.

```
iscsiadm -m discovery -t st -p <IP address provided by Service Management>:3260
```

![iSCSIadmDiscovery.png](media/HowToHLI/HASetupWithStonith/iSCSIadmDiscovery.png)

1.4 de opdracht voor aanmelding bij de iSCSI-apparaat uitvoeren, vier sessies worden weergegeven. Uitvoeren op **beide** de knooppunten.

```
iscsiadm -m node -l
```
![iSCSIadmLogin.png](media/HowToHLI/HASetupWithStonith/iSCSIadmLogin.png)

1.5 Voer het script opnieuw scannen uit: *opnieuw scannen-scsi-bus.sh*.  Met dit script ziet u de nieuwe schijven die voor u gemaakt.  Uitvoeren op beide knooppunten. U ziet een LUN-waarde die groter is dan nul (bijvoorbeeld: 1, 2 enz.)

```
rescan-scsi-bus.sh
```
![rescanscsibus.png](media/HowToHLI/HASetupWithStonith/rescanscsibus.png)

1.6 aan de naam van het apparaat de opdracht uitvoert *fdisk – l*. Uitvoeren op beide knooppunten. Kies het apparaat met de grootte van **178 MiB**.

```
  fdisk –l
```

![fdisk-l.png](media/HowToHLI/HASetupWithStonith/fdisk-l.png)

## <a name="2---initialize-the-sbd-device"></a>2.   Het apparaat SBD initialiseren

2.1 op het apparaat SBD geïnitialiseerd **beide** de knooppunten

```
sbd -d <SBD Device Name> create
```
![sbdcreate.png](media/HowToHLI/HASetupWithStonith/sbdcreate.png)

2.2 controleren wat is geschreven naar het apparaat. Uitvoeren op **beide** de knooppunten

```
sbd -d <SBD Device Name> dump
```

## <a name="3---configuring-the-cluster"></a>3.   Het Cluster te configureren
Deze sectie beschrijft de stappen voor het instellen van het SUSE HA-cluster.
### <a name="31-package-installation"></a>3.1 installatie van het pakket
3.1.1 Controleer of ha_sles en SAPHanaSR doc-patronen zijn geïnstalleerd. Als dit niet is geïnstalleerd, installeert u deze. Installeer deze op **beide** de knooppunten.
```
zypper in -t pattern ha_sles
zypper in SAPHanaSR SAPHanaSR-doc
```
![zypperpatternha_sles.png](media/HowToHLI/HASetupWithStonith/zypperpatternha_sles.png)
![zypperpatternSAPHANASR-doc.png](media/HowToHLI/HASetupWithStonith/zypperpatternSAPHANASR-doc.png)

### <a name="32-setting-up-the-cluster"></a>3.2 instellen van het cluster
3.2.1 u kunt beide gebruiken *ha-cluster-init* opdracht of het gebruik van de wizard yast2 voor het instellen van het cluster. In dit geval wordt wordt de wizard yast2 gebruikt. Kunt u deze stap uitvoeren **alleen op het primaire knooppunt**.

Ga als volgt yast2 > hoge beschikbaarheid > Cluster ![yast-besturingselement-center.png](media/HowToHLI/HASetupWithStonith/yast-control-center.png)
![yast-hawk-install.png](media/HowToHLI/HASetupWithStonith/yast-hawk-install.png)

Klik op **annuleren** omdat het pakket halk2 al is geïnstalleerd.

![yast-hawk-continue.png](media/HowToHLI/HASetupWithStonith/yast-hawk-continue.png)

Klik op **gaan**

Verwachte waarde aantal knooppunten geïmplementeerd (in dit geval 2) = ![yast-Cluster-Security.png](media/HowToHLI/HASetupWithStonith/yast-Cluster-Security.png) klikt u op **volgende**
![yast-cluster-configureren-csync2.png](media/HowToHLI/HASetupWithStonith/yast-cluster-configure-csync2.png) toevoegen node names en klik vervolgens op ' voorgestelde bestanden toevoegen'

Klik op "Inschakelen csync2"

Klik op 'Genereren Pre-Shared-sleutels', wordt deze weergegeven onder het pop-upvenster

![yast-key-file.png](media/HowToHLI/HASetupWithStonith/yast-key-file.png)

Klik op **OK**

De verificatie wordt uitgevoerd met behulp van de IP-adressen en pre-shared-sleutels in Csync2. Het sleutelbestand wordt met csync2 -k /etc/csync2/key_hagroup gegenereerd. Het bestand key_hagroup moeten worden gekopieerd op alle leden van het cluster handmatig nadat deze gemaakt. **Zorg ervoor dat u het bestand kopiëren van knooppunt 1 naar knooppunt2**.

![yast-cluster-conntrackd.png](media/HowToHLI/HASetupWithStonith/yast-cluster-conntrackd.png)

Klik op **volgende**
![yast-cluster-service.png](media/HowToHLI/HASetupWithStonith/yast-cluster-service.png)

In de standaardoptie opgestart is uitgeschakeld, op 'aan' wijzigen zodat pacemaker bij het opstarten is gestart. U kunt kiezen op basis van uw installatie-eisen.
Klik op **volgende** en de configuratie van het cluster is voltooid.

## <a name="4---setting-up-the-softdog-watchdog"></a>4.   Instellen van de Softdog Watchdog
Deze sectie beschrijft de configuratie van de watchdog (softdog).

4.1 de volgende regel toe te voegen */etc/init.d/boot.local* op **beide** de knooppunten.
```
modprobe softdog
```
![modprobe-softdog.png](media/HowToHLI/HASetupWithStonith/modprobe-softdog.png)

4.2 bijwerken van het bestand */etc/sysconfig/sbd* op **beide** de knooppunten als volgt:
```
SBD_DEVICE="<SBD Device Name>"
```
![sbd-device.png](media/HowToHLI/HASetupWithStonith/sbd-device.png)

4.3 de kernelmodule laden op **beide** de knooppunten op basis van de volgende opdracht uit
```
modprobe softdog
```
![modprobe-softdog-command.png](media/HowToHLI/HASetupWithStonith/modprobe-softdog-command.png)

4.4 controleren en ervoor te zorgen dat softdog wordt uitgevoerd als volgt op **beide** de knooppunten:
```
lsmod | grep dog
```
![lsmod-grep-dog.png](media/HowToHLI/HASetupWithStonith/lsmod-grep-dog.png)

4.5 start het apparaat SBD op **beide** de knooppunten
```
/usr/share/sbd/sbd.sh start
```
![sbd-sh-start.png](media/HowToHLI/HASetupWithStonith/sbd-sh-start.png)

4.6 de daemon SBD testen op **beide** de knooppunten. U ziet twee vermeldingen nadat u deze configureren op **beide** de knooppunten
```
sbd -d <SBD Device Name> list
```
![sbd-list.png](media/HowToHLI/HASetupWithStonith/sbd-list.png)

4.7 een testbericht te verzenden **één** van uw knooppunten
```
sbd  -d <SBD Device Name> message <node2> <message>
```
![sbd-list.png](media/HowToHLI/HASetupWithStonith/sbd-list.png)

4.8 op de **tweede** knooppunt (Knooppunt2), kunt u de berichtstatus controleren
```
sbd  -d <SBD Device Name> list
```
![sbd-list-message.png](media/HowToHLI/HASetupWithStonith/sbd-list-message.png)

4.9 vast te stellen de configuratie van de sbd, werkt u het bestand */etc/sysconfig/sbd* als volgt. Bijwerken van het bestand op **beide** de knooppunten
```
SBD_DEVICE=" <SBD Device Name>" 
SBD_WATCHDOG="yes" 
SBD_PACEMAKER="yes" 
SBD_STARTMODE="clean" 
SBD_OPTS=""
```
4.10 start de service pacemaker op de **primaire knooppunt** (knooppunt1)
```
systemctl start pacemaker
```
![start-pacemaker.png](media/HowToHLI/HASetupWithStonith/start-pacemaker.png)

Als de service pacemaker *mislukt*, verwijzen naar *Scenario 5: Pacemaker service mislukt*

## <a name="5---joining-the-cluster"></a>5.   Lid worden van het cluster
In deze sectie vindt u instructies voor het knooppunt aan het cluster worden toegevoegd.

### <a name="51-add-the-node"></a>5.1 de knooppunt toevoegen
Voer de volgende opdracht op **Knooppunt2** om te laten Knooppunt2 toegevoegd aan het cluster.
```
ha-cluster-join
```
Als u ontvangt een *fout* tijdens het lid wordt van het cluster, raadpleegt u *Scenario 6: Knooppunt 2 kan niet deelnemen aan het cluster*.

## <a name="6---validating-the-cluster"></a>6.   Het cluster te valideren

### <a name="61-start-the-cluster-service"></a>6.1 de cluster-service starten
Om te controleren en desgewenst het cluster voor het eerst starten op **beide** knooppunten.
```
systemctl status pacemaker
systemctl start pacemaker
```
![systemctl-status-pacemaker.png](media/HowToHLI/HASetupWithStonith/systemctl-status-pacemaker.png)
### <a name="62-monitor-the-status"></a>6.2 de status controleren
Voer de opdracht *crm_mon* om ervoor te zorgen **beide** de knooppunten online zijn. U kunt uitvoeren op **een van de knooppunten** van het cluster
```
crm_mon
```
![CRM-mon.png](media/HowToHLI/HASetupWithStonith/crm-mon.png) u kunt zich ook aanmelden bij hawk om te controleren of de status van het cluster *https://\<knooppunt IP-adres >: 7630*. De standaardwaarde is hacluster en het wachtwoord is linux. Indien nodig, kunt u het wachtwoord met behulp *passwd* opdracht.

## <a name="7-configure-cluster-properties-and-resources"></a>7. De eigenschappen en Resources configureren 
Deze sectie beschrijft de stappen voor het configureren van de clusterresources.
In dit voorbeeld, instellen van de volgende resource, kan de rest worden geconfigureerd (indien nodig) door te verwijzen naar de SUSE HA-handleiding. Uitvoeren van de configuratie in **een van de knooppunten** alleen. Voer op het primaire knooppunt.

- Cluster-bootstrap
- Apparaat stonith instellen
- Het virtuele IP-adres


### <a name="71-cluster-bootstrap-and-more"></a>7.1 cluster bootstrap en meer
Cluster-bootstrap toevoegen. Maak het bestand en voeg de tekst als volgt:
```
sapprdhdb95:~ # vi crm-bs.txt
# enter the following to crm-bs.txt
property $id="cib-bootstrap-options" \
no-quorum-policy="ignore" \
stonith-enabled="true" \
stonith-action="reboot" \
stonith-timeout="150s"
rsc_defaults $id="rsc-options" \
resource-stickiness="1000" \
migration-threshold="5000"
op_defaults $id="op-options" \
timeout="600"
```
De configuratie toevoegen aan het cluster.
```
crm configure load update crm-bs.txt
```
![crm-configure-crmbs.png](media/HowToHLI/HASetupWithStonith/crm-configure-crmbs.png)

### <a name="72-stonith-device"></a>7.2 apparaat stonith instellen
Resource stonith instellen toevoegen. Maak het bestand en voeg de tekst als volgt.
```
# vi crm-sbd.txt
# enter the following to crm-sbd.txt
primitive stonith-sbd stonith:external/sbd \
params pcmk_delay_max="15"
```
De configuratie toevoegen aan het cluster.
```
crm configure load update crm-sbd.txt
```

### <a name="73-the-virtual-ip-address"></a>7.3 de virtuele IP-adres
Resource virtueel IP-adres toevoegen. Maak het bestand en voeg tekst toe zoals hieronder.
```
# vi crm-vip.txt
primitive rsc_ip_HA1_HDB10 ocf:heartbeat:IPaddr2 \
operations $id="rsc_ip_HA1_HDB10-operations" \
op monitor interval="10s" timeout="20s" \
params ip="10.35.0.197"
```
De configuratie toevoegen aan het cluster.
```
crm configure load update crm-vip.txt
```

### <a name="74-validate-the-resources"></a>7.4 de resources valideren

Wanneer u de opdracht uitvoert *crm_mon*, kunt u er de twee resources bekijken.
![crm_mon_command.png](media/HowToHLI/HASetupWithStonith/crm_mon_command.png)

Ook ziet u de status op *https://\<knooppunt IP-adres >: 7630/cib/live/status*

![hawlk-status-page.png](media/HowToHLI/HASetupWithStonith/hawlk-status-page.png)

## <a name="8-testing-the-failover-process"></a>8. Het failoverproces te testen
Als u wilt testen het failoverproces, stop de service pacemaker op knooppunt1, en de failover resources naar knooppunt2.
```
Service pacemaker stop
```
Nu, stop de service pacemaker op **Knooppunt2** en een failover naar resources **knooppunt1**

**Voordat de failover**
![voordat failover.png](media/HowToHLI/HASetupWithStonith/Before-failover.png)
**na een failover**
![na failover.png](media/HowToHLI/HASetupWithStonith/after-failover.png)
 ![ CRM-ma-na-failover.png](media/HowToHLI/HASetupWithStonith/crm-mon-after-failover.png)


## <a name="9-troubleshooting"></a>9. Problemen oplossen
Deze sectie beschrijft de zeldzame gevallen mislukt, die kunnen worden aangetroffen tijdens de installatie. U mag niet per se te maken krijgt deze problemen.

### <a name="scenario-1-cluster-node-not-online"></a>Scenario 1: Clusterknooppunt niet online
Als een van de knooppunten niet wordt weergegeven in de clustermanager online, kunt u proberen om deze online te volgen.

De iSCSI-service starten
```
service iscsid start
```

En nu moet u kunnen zich aanmelden bij de iSCSI-knooppunt
```
iscsiadm -m node -l
```
De verwachte uitvoer er ongeveer zo uitziet als volgende
```
sapprdhdb45:~ # iscsiadm -m node -l
Logging in to [iface: default, target: iqn.1992-08.com.netapp:hanadc11:1:t020, portal: 10.250.22.11,3260] (multiple)
Logging in to [iface: default, target: iqn.1992-08.com.netapp:hanadc11:1:t020, portal: 10.250.22.12,3260] (multiple)
Logging in to [iface: default, target: iqn.1992-08.com.netapp:hanadc11:1:t020, portal: 10.250.22.22,3260] (multiple)
Logging in to [iface: default, target: iqn.1992-08.com.netapp:hanadc11:1:t020, portal: 10.250.22.21,3260] (multiple)
Login to [iface: default, target: iqn.1992-08.com.netapp:hanadc11:1:t020, portal: 10.250.22.11,3260] successful.
Login to [iface: default, target: iqn.1992-08.com.netapp:hanadc11:1:t020, portal: 10.250.22.12,3260] successful.
Login to [iface: default, target: iqn.1992-08.com.netapp:hanadc11:1:t020, portal: 10.250.22.22,3260] successful.
Login to [iface: default, target: iqn.1992-08.com.netapp:hanadc11:1:t020, portal: 10.250.22.21,3260] successful.
```
### <a name="scenario-2-yast2-does-not-show-graphical-view"></a>Scenario 2: yast2 wordt niet weergegeven voor grafische weergave
Het yast2 grafische scherm wordt gebruikt voor het instellen van het cluster met hoge beschikbaarheid in dit document. Als yast2 niet met het grafische venster openen zoals wordt weergegeven en Qt fout genereert, de stappen die u als volgt uitvoeren. Als deze met het grafische venster wordt geopend, kunt u de stappen overslaan.

**Fout**

![yast2-qt-gui-error.png](media/HowToHLI/HASetupWithStonith/yast2-qt-gui-error.png)

**Verwachte uitvoer**

![yast-control-center.png](media/HowToHLI/HASetupWithStonith/yast-control-center.png)

Als de yast2 niet wordt geopend met de grafische weergave, volgt u de volgende stappen.

Installeer de vereiste pakketten. U moet zijn aangemeld als gebruiker "root" en SMT is ingesteld dat de pakketten downloaden/installeren.

Gebruik het om pakketten te installeren, yast > Software > beheer van Software > afhankelijkheden > optie 'Installeren aanbevolen pakketten...'. De volgende schermafbeelding ziet u de verwachte schermen.
>[!NOTE]
>U moet de stappen uitvoeren op beide knooppunten, zodat u toegang hebben tot de yast2 grafische weergave van beide knooppunten.

![yast-sofwaremanagement.png](media/HowToHLI/HASetupWithStonith/yast-sofwaremanagement.png)

Selecteer onder afhankelijkheden, de optie 'Installeren aanbevolen Packages' ![yast dependencies.png](media/HowToHLI/HASetupWithStonith/yast-dependencies.png)

Controleer de wijzigingen en klik op OK

![yast](media/HowToHLI/HASetupWithStonith/yast-automatic-changes.png)

Pakket-installatie voortgezet ![yast-uitvoeren-installation.png](media/HowToHLI/HASetupWithStonith/yast-performing-installation.png)

Klik op Next

![yast-installation-report.png](media/HowToHLI/HASetupWithStonith/yast-installation-report.png)

Klik op Voltooien

U moet ook de libqt4 en libyui qt-pakketten installeren.
```
zypper -n install libqt4
```
![zypper-install-libqt4.png](media/HowToHLI/HASetupWithStonith/zypper-install-libqt4.png)
```
zypper -n install libyui-qt
```
![zypper-installatie-ligyui.png](media/HowToHLI/HASetupWithStonith/zypper-install-ligyui.png)
![zypper-installatie-ligyui_part2.png](media/HowToHLI/HASetupWithStonith/zypper-install-ligyui_part2.png) Yast2 zou het mogelijk de grafische weergave nu zoals hier openen.
![yast2-control-center.png](media/HowToHLI/HASetupWithStonith/yast2-control-center.png)

### <a name="scenario-3-yast2-does-not-high-availability-option"></a>Scenario 3: yast2 biedt geen optie voor hoge beschikbaarheid
Voor de optie voor hoge beschikbaarheid zichtbaar zijn in het besturingselement yast2 center, moet u de extra pakketten installeren.

Met behulp van Yast2 > Software > beheer van Software > Selecteer de volgende patronen

- SAP HANA-server basis
- C/C++-Compiler en hulpprogramma 's
- Hoge beschikbaarheid
- SAP-toepassingsserver basis

Het volgende scherm ziet u de stappen voor het installeren van de patronen.

Met behulp van yast2 > Software > beheer van Software

![yast2-control-center.png](media/HowToHLI/HASetupWithStonith/yast2-control-center.png)

Selecteer de patronen

![yast-pattern1.png](media/HowToHLI/HASetupWithStonith/yast-pattern1.png)
![yast-pattern2.png](media/HowToHLI/HASetupWithStonith/yast-pattern2.png)

Klik op **accepteren**

![yast-gewijzigd packages.png](media/HowToHLI/HASetupWithStonith/yast-changed-packages.png)

Klik op **gaan**

![YaST2-uitvoeren-installation.png](media/HowToHLI/HASetupWithStonith/yast2-performing-installation.png)

Klik op **volgende** wanneer de installatie is voltooid

![yast2-installation-report.png](media/HowToHLI/HASetupWithStonith/yast2-installation-report.png)

### <a name="scenario-4-hana-installation-fails-with-gcc-assemblies-error"></a>Scenario 4: HANA-installatie is mislukt met de volgende fout gcc-assembly 's
De HANA-installatie mislukt met de volgende fout.

![Hana-installation-error.png](media/HowToHLI/HASetupWithStonith/Hana-installation-error.png)

Los het probleem, moet u bibliotheken installeren (libgcc_sl en libstdc ++ 6) als volgt.

![zypper-install-lib.png](media/HowToHLI/HASetupWithStonith/zypper-install-lib.png)

### <a name="scenario-5-pacemaker-service-fails"></a>Scenario 5: Pacemaker service mislukt

Het volgende probleem opgetreden tijdens de pacemaker service start.

```
sapprdhdb95:/ # systemctl start pacemaker
A dependency job for pacemaker.service failed. See 'journalctl -xn' for details.
```
```
sapprdhdb95:/ # journalctl -xn
-- Logs begin at Thu 2017-09-28 09:28:14 EDT, end at Thu 2017-09-28 21:48:27 EDT. --
Sep 28 21:48:27 sapprdhdb95 corosync[68812]: [SERV  ] Service engine unloaded: corosync configuration map
Sep 28 21:48:27 sapprdhdb95 corosync[68812]: [QB    ] withdrawing server sockets
Sep 28 21:48:27 sapprdhdb95 corosync[68812]: [SERV  ] Service engine unloaded: corosync configuration ser
Sep 28 21:48:27 sapprdhdb95 corosync[68812]: [QB    ] withdrawing server sockets
Sep 28 21:48:27 sapprdhdb95 corosync[68812]: [SERV  ] Service engine unloaded: corosync cluster closed pr
Sep 28 21:48:27 sapprdhdb95 corosync[68812]: [QB    ] withdrawing server sockets
Sep 28 21:48:27 sapprdhdb95 corosync[68812]: [SERV  ] Service engine unloaded: corosync cluster quorum se
Sep 28 21:48:27 sapprdhdb95 corosync[68812]: [SERV  ] Service engine unloaded: corosync profile loading s
Sep 28 21:48:27 sapprdhdb95 corosync[68812]: [MAIN  ] Corosync Cluster Engine exiting normally
Sep 28 21:48:27 sapprdhdb95 systemd[1]: Dependency failed for Pacemaker High Availability Cluster Manager
-- Subject: Unit pacemaker.service has failed
-- Defined-By: systemd
-- Support: https://lists.freedesktop.org/mailman/listinfo/systemd-devel
--
-- Unit pacemaker.service has failed.
--
-- The result is dependency.
```
```
sapprdhdb95:/ # tail -f /var/log/messages
2017-09-28T18:44:29.675814-04:00 sapprdhdb95 corosync[57600]:   [QB    ] withdrawing server sockets
2017-09-28T18:44:29.676023-04:00 sapprdhdb95 corosync[57600]:   [SERV  ] Service engine unloaded: corosync cluster closed process group service v1.01
2017-09-28T18:44:29.725885-04:00 sapprdhdb95 corosync[57600]:   [QB    ] withdrawing server sockets
2017-09-28T18:44:29.726069-04:00 sapprdhdb95 corosync[57600]:   [SERV  ] Service engine unloaded: corosync cluster quorum service v0.1
2017-09-28T18:44:29.726164-04:00 sapprdhdb95 corosync[57600]:   [SERV  ] Service engine unloaded: corosync profile loading service
2017-09-28T18:44:29.776349-04:00 sapprdhdb95 corosync[57600]:   [MAIN  ] Corosync Cluster Engine exiting normally
2017-09-28T18:44:29.778177-04:00 sapprdhdb95 systemd[1]: Dependency failed for Pacemaker High Availability Cluster Manager.
2017-09-28T18:44:40.141030-04:00 sapprdhdb95 systemd[1]: [/usr/lib/systemd/system/fstrim.timer:8] Unknown lvalue 'Persistent' in section 'Timer'
2017-09-28T18:45:01.275038-04:00 sapprdhdb95 cron[57995]: pam_unix(crond:session): session opened for user root by (uid=0)
2017-09-28T18:45:01.308066-04:00 sapprdhdb95 CRON[57995]: pam_unix(crond:session): session closed for user root
```

Om dit te corrigeren, verwijdert u de volgende regel uit het bestand */usr/lib/systemd/system/fstrim.timer*

```
Persistent=true
```

![Persistent.png](media/HowToHLI/HASetupWithStonith/Persistent.png)

### <a name="scenario-6-node-2-unable-to-join-the-cluster"></a>Scenario 6: Knooppunt 2 kan niet deelnemen aan het cluster

Wanneer de Knooppunt2 toevoegen aan het bestaande cluster met behulp van *ha-cluster-join* opdracht, de volgende fout is opgetreden.

```
ERROR: Can’t retrieve SSH keys from <Primary Node>
```

![ha-cluster-join-error.png](media/HowToHLI/HASetupWithStonith/ha-cluster-join-error.png)

Als u wilt oplossen, voert u het volgende op beide knooppunten

```
ssh-keygen -q -f /root/.ssh/id_rsa -C 'Cluster Internal' -N ''
cat /root/.ssh/id_rsa.pub >> /root/.ssh/authorized_keys
```

![SSH-keygen-knooppunt1. PNG](media/HowToHLI/HASetupWithStonith/ssh-keygen-node1.PNG)

![SSH-keygen-Knooppunt2. PNG](media/HowToHLI/HASetupWithStonith/ssh-keygen-node2.PNG)

Na de voorgaande oplossing moet Knooppunt2 toegevoegd aan het cluster

![ha-cluster-join-fix.png](media/HowToHLI/HASetupWithStonith/ha-cluster-join-fix.png)

## <a name="10-general-documentation"></a>10. Algemene documentatie
U vindt meer informatie over SUSE HA setup in de volgende artikelen: 

- [SAP HANA SR Performance Optimized Scenario](https://www.suse.com/docrep/documents/ir8w88iwu7/suse_linux_enterprise_server_for_sap_applications_12_sp1.pdf )
- [Eerste optie op basis van opslag](https://www.suse.com/documentation/sle_ha/book_sleha/data/sec_ha_storage_protect_fencing.html)
- [Blog - Pacemaker Cluster gebruikt voor SAP HANA - deel 1](https://blogs.sap.com/2017/11/19/be-prepared-for-using-pacemaker-cluster-for-sap-hana-part-1-basics/)
- [Blog - Pacemaker Cluster gebruikt voor SAP HANA - deel 2](https://blogs.sap.com/2017/11/19/be-prepared-for-using-pacemaker-cluster-for-sap-hana-part-2-failure-of-both-nodes/)
