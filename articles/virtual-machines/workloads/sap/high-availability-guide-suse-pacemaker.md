---
title: Instellen van Pacemaker op SUSE Linux Enterprise Server in Azure | Microsoft Docs
description: Pacemaker op SUSE Linux Enterprise Server in Azure instellen
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: mssedusch
manager: jeconnoc
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 08/16/2018
ms.author: sedusch
ms.openlocfilehash: 9a23f13947c4c7a77460ff389861e1dcc1de3c7f
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/21/2019
ms.locfileid: "65992112"
---
# <a name="setting-up-pacemaker-on-suse-linux-enterprise-server-in-azure"></a>Pacemaker op SUSE Linux Enterprise Server in Azure instellen

[planning-guide]:planning-guide.md
[deployment-guide]:deployment-guide.md
[dbms-guide]:dbms-guide.md
[sap-hana-ha]:sap-hana-high-availability.md
[virtual-machines-linux-maintenance]:../../linux/maintenance-and-updates.md#maintenance-that-doesnt-require-a-reboot
[virtual-machines-windows-maintenance]:../../windows/maintenance-and-updates.md#maintenance-that-doesnt-require-a-reboot
[sles-nfs-guide]:high-availability-guide-suse-nfs.md
[sles-guide]:high-availability-guide-suse.md

Er zijn twee opties voor het instellen van een cluster Pacemaker in Azure. U kunt een agent de eerste optie, die zorgt dat er opnieuw starten van een knooppunt via de Azure API's gebruiken of kunt u een apparaat SBD.

De SBD vereist ten minste één extra virtuele machine die fungeert als een iSCSI-doelserver en biedt een SBD-apparaat. Deze iSCSI-doelservers kunnen echter worden gedeeld met andere Pacemaker-clusters. Het voordeel van het gebruik van een apparaat SBD is een snellere failover en, als u SBD apparaten on-premises niet vereist dat alle wijzigingen in hoe u het cluster pacemaker werken. U kunt maximaal drie SBD apparaten voor een cluster Pacemaker gebruiken om toe te staan een SBD-apparaat niet meer beschikbaar zijn, bijvoorbeeld tijdens het patchen van het besturingssysteem van de iSCSI-doelserver. Als u meer dan één SBD apparaat per Pacemaker gebruiken wilt, moet u het implementeren van meerdere iSCSI-doelservers en verbinding maken met een SBD van elke iSCSI-doelserver. Het is raadzaam om met behulp van één SBD apparaat of drie. Pacemaker zich niet op een clusterknooppunt automatisch omheining als u slechts twee SBD apparaten configureren en een van beide niet beschikbaar is. Als u wilt dat omheining wanneer een iSCSI-doelserver niet actief is, hebt u drie SBD apparaten en daarom de drie iSCSI-doelservers gebruiken.

Als u niet investeren in één extra virtuele machine wilt, kunt u ook de omheining Azure-agent. Het nadeel is dat een failover tussen 10 tot 15 minuten duren kan als een resource stoppen is mislukt of als de clusterknooppunten kunnen niet worden gecommuniceerd die elkaar meer.

![Pacemaker op SLES-overzicht](./media/high-availability-guide-suse-pacemaker/pacemaker.png)

>[!IMPORTANT]
> Bij het plannen en implementeren van Linux Pacemaker knooppunten en SBD apparaten geclusterde, is het essentieel is voor de algehele betrouwbaarheid van de volledige clusterconfiguratie die de routering tussen de virtuele machines die betrokken zijn en de VM (s) die als host fungeert voor de apparaten SBD wordt niet doorgegeven via alle andere apparaten, zoals [NVA's](https://azure.microsoft.com/solutions/network-appliances/). Anders, problemen en onderhoudsgebeurtenissen met de NVA kunnen een nadelige invloed op de stabiliteit en betrouwbaarheid van de algehele clusterconfiguratie hebben. Om te voorkomen dat dergelijke obstakels, regels voor doorsturen van NVA's niet bepalen of [gebruiker gedefinieerde routeringsregels](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview) die verkeer routeren tussen geclusterde knooppunten en SBD apparaten via de NVA's en vergelijkbare apparaten bij het plannen en implementeren van Linux Pacemaker geclusterde knooppunten en SBD apparaten. 
>

## <a name="sbd-fencing"></a>De eerste optie SBD

Volg deze stappen als u wilt een SBD-apparaat gebruiken voor de eerste optie.

### <a name="set-up-iscsi-target-servers"></a>ISCSI-doelservers instellen

U moet eerst de iSCSI-doel-virtuele machines maken. iSCSI-doelservers kunnen worden gedeeld met meerdere Pacemaker-clusters.

1. Implementeer nieuwe SLES 12 SP1 of hoger virtuele machines en maak er verbinding mee via ssh. De machines hoeft niet te groot. De grootte van een virtuele machine, zoals Standard_E2s_v3 of Standard_D2s_v3 is voldoende. Zorg ervoor dat u de besturingssysteemschijf van de Premium-opslag.

Voer de volgende opdrachten uit op alle **iSCSI-doel-virtuele machines**.

1. SLES bijwerken

   <pre><code>sudo zypper update
   </code></pre>

1. Verwijderen van pakketten

   Om te voorkomen dat een bekend probleem met targetcli en SLES 12 SP3, verwijdert u de volgende pakketten. U kunt fouten over pakketten die niet is gevonden negeren

   <pre><code>sudo zypper remove lio-utils python-rtslib python-configshell targetcli
   </code></pre>

1. ISCSI-doel-pakketten installeren

   <pre><code>sudo zypper install targetcli-fb dbus-1-python
   </code></pre>

1. De iSCSI-doelservice inschakelen

   <pre><code>sudo systemctl enable targetcli
   sudo systemctl start targetcli
   </code></pre>

### <a name="create-iscsi-device-on-iscsi-target-server"></a>ISCSI-apparaat maakt op de iSCSI-doelserver

Voer de volgende opdrachten uit op alle **iSCSI-doel-virtuele machines** te maken van de iSCSI-schijven voor de clusters die worden gebruikt door uw SAP-systemen. In het volgende voorbeeld worden SBD apparaten voor meerdere clusters gemaakt. Hier ziet u hoe u een iSCSI-doelserver wilt gebruiken voor meerdere clusters. De apparaten SBD worden geplaatst op de besturingssysteemschijf. Zorg ervoor dat u er genoeg ruimte is.

**`nfs`** wordt gebruikt voor het identificeren van het cluster NFS **ascsnw1** wordt gebruikt voor het identificeren van het cluster met ASCS **NW1**, **dbnw1** wordt gebruikt voor het identificeren van de databasecluster van **NW1** , **nfs-0** en **nfs-1** zijn de hostnamen van de clusterknooppunten NFS **nw1-xscs-0** en **nw1-xscs-1**zijn de hostnamen van de **NW1** ASCS clusterknooppunten, en **nw1-db-0** en **nw1-db-1** zijn de hostnamen van de database clusterknooppunten. Vervang deze door de hostnamen van de clusterknooppunten en wordt de SID van uw SAP-systeem.

<pre><code># Create the root folder for all SBD devices
sudo mkdir /sbd

# Create the SBD device for the NFS server
sudo targetcli backstores/fileio create sbdnfs /sbd/sbdnfs 50M write_back=false
sudo targetcli iscsi/ create iqn.2006-04.nfs.local:nfs
sudo targetcli iscsi/iqn.2006-04.nfs.local:nfs/tpg1/luns/ create /backstores/fileio/sbdnfs
sudo targetcli iscsi/iqn.2006-04.nfs.local:nfs/tpg1/acls/ create iqn.2006-04.<b>nfs-0.local:nfs-0</b>
sudo targetcli iscsi/iqn.2006-04.nfs.local:nfs/tpg1/acls/ create iqn.2006-04.<b>nfs-1.local:nfs-1</b>

# Create the SBD device for the ASCS server of SAP System NW1
sudo targetcli backstores/fileio create sbdascs<b>nw1</b> /sbd/sbdascs<b>nw1</b> 50M write_back=false
sudo targetcli iscsi/ create iqn.2006-04.ascs<b>nw1</b>.local:ascs<b>nw1</b>
sudo targetcli iscsi/iqn.2006-04.ascs<b>nw1</b>.local:ascs<b>nw1</b>/tpg1/luns/ create /backstores/fileio/sbdascs<b>nw1</b>
sudo targetcli iscsi/iqn.2006-04.ascs<b>nw1</b>.local:ascs<b>nw1</b>/tpg1/acls/ create iqn.2006-04.<b>nw1-xscs-0.local:nw1-xscs-0</b>
sudo targetcli iscsi/iqn.2006-04.ascs<b>nw1</b>.local:ascs<b>nw1</b>/tpg1/acls/ create iqn.2006-04.<b>nw1-xscs-1.local:nw1-xscs-1</b>

# Create the SBD device for the database cluster of SAP System NW1
sudo targetcli backstores/fileio create sbddb<b>nw1</b> /sbd/sbddb<b>nw1</b> 50M write_back=false
sudo targetcli iscsi/ create iqn.2006-04.db<b>nw1</b>.local:db<b>nw1</b>
sudo targetcli iscsi/iqn.2006-04.db<b>nw1</b>.local:db<b>nw1</b>/tpg1/luns/ create /backstores/fileio/sbddb<b>nw1</b>
sudo targetcli iscsi/iqn.2006-04.db<b>nw1</b>.local:db<b>nw1</b>/tpg1/acls/ create iqn.2006-04.<b>nw1-db-0.local:nw1-db-0</b>
sudo targetcli iscsi/iqn.2006-04.db<b>nw1</b>.local:db<b>nw1</b>/tpg1/acls/ create iqn.2006-04.<b>nw1-db-1.local:nw1-db-1</b>

# save the targetcli changes
sudo targetcli saveconfig
</code></pre>

U kunt controleren als alles correct is ingesteld met

<pre><code>sudo targetcli ls

o- / .......................................................................................................... [...]
  o- backstores ............................................................................................... [...]
  | o- block ................................................................................... [Storage Objects: 0]
  | o- fileio .................................................................................. [Storage Objects: 3]
  | | o- <b>sbdascsnw1</b> ................................................ [/sbd/sbdascsnw1 (50.0MiB) write-thru activated]
  | | | o- alua .................................................................................... [ALUA Groups: 1]
  | | |   o- default_tg_pt_gp ........................................................ [ALUA state: Active/optimized]
  | | o- <b>sbddbnw1</b> .................................................... [/sbd/sbddbnw1 (50.0MiB) write-thru activated]
  | | | o- alua .................................................................................... [ALUA Groups: 1]
  | | |   o- default_tg_pt_gp ........................................................ [ALUA state: Active/optimized]
  | | o- <b>sbdnfs</b> ........................................................ [/sbd/sbdnfs (50.0MiB) write-thru activated]
  | |   o- alua .................................................................................... [ALUA Groups: 1]
  | |     o- default_tg_pt_gp ........................................................ [ALUA state: Active/optimized]
  | o- pscsi ................................................................................... [Storage Objects: 0]
  | o- ramdisk ................................................................................. [Storage Objects: 0]
  o- iscsi ............................................................................................. [Targets: 3]
  | o- <b>iqn.2006-04.ascsnw1.local:ascsnw1</b> .................................................................. [TPGs: 1]
  | | o- tpg1 ................................................................................ [no-gen-acls, no-auth]
  | |   o- acls ........................................................................................... [ACLs: 2]
  | |   | o- <b>iqn.2006-04.nw1-xscs-0.local:nw1-xscs-0</b> ............................................... [Mapped LUNs: 1]
  | |   | | o- mapped_lun0 ............................................................ [lun0 fileio/<b>sbdascsnw1</b> (rw)]
  | |   | o- <b>iqn.2006-04.nw1-xscs-1.local:nw1-xscs-1</b> ............................................... [Mapped LUNs: 1]
  | |   |   o- mapped_lun0 ............................................................ [lun0 fileio/<b>sbdascsnw1</b> (rw)]
  | |   o- luns ........................................................................................... [LUNs: 1]
  | |   | o- lun0 .......................................... [fileio/sbdascsnw1 (/sbd/sbdascsnw1) (default_tg_pt_gp)]
  | |   o- portals ..................................................................................... [Portals: 1]
  | |     o- 0.0.0.0:3260 ...................................................................................... [OK]
  | o- <b>iqn.2006-04.dbnw1.local:dbnw1</b> ...................................................................... [TPGs: 1]
  | | o- tpg1 ................................................................................ [no-gen-acls, no-auth]
  | |   o- acls ........................................................................................... [ACLs: 2]
  | |   | o- <b>iqn.2006-04.nw1-db-0.local:nw1-db-0</b> ................................................... [Mapped LUNs: 1]
  | |   | | o- mapped_lun0 .............................................................. [lun0 fileio/<b>sbddbnw1</b> (rw)]
  | |   | o- <b>iqn.2006-04.nw1-db-1.local:nw1-db-1</b> ................................................... [Mapped LUNs: 1]
  | |   |   o- mapped_lun0 .............................................................. [lun0 fileio/<b>sbddbnw1</b> (rw)]
  | |   o- luns ........................................................................................... [LUNs: 1]
  | |   | o- lun0 .............................................. [fileio/sbddbnw1 (/sbd/sbddbnw1) (default_tg_pt_gp)]
  | |   o- portals ..................................................................................... [Portals: 1]
  | |     o- 0.0.0.0:3260 ...................................................................................... [OK]
  | o- <b>iqn.2006-04.nfs.local:nfs</b> .......................................................................... [TPGs: 1]
  |   o- tpg1 ................................................................................ [no-gen-acls, no-auth]
  |     o- acls ........................................................................................... [ACLs: 2]
  |     | o- <b>iqn.2006-04.nfs-0.local:nfs-0</b> ......................................................... [Mapped LUNs: 1]
  |     | | o- mapped_lun0 ................................................................ [lun0 fileio/<b>sbdnfs</b> (rw)]
  |     | o- <b>iqn.2006-04.nfs-1.local:nfs-1</b> ......................................................... [Mapped LUNs: 1]
  |     |   o- mapped_lun0 ................................................................ [lun0 fileio/<b>sbdnfs</b> (rw)]
  |     o- luns ........................................................................................... [LUNs: 1]
  |     | o- lun0 .................................................. [fileio/sbdnfs (/sbd/sbdnfs) (default_tg_pt_gp)]
  |     o- portals ..................................................................................... [Portals: 1]
  |       o- 0.0.0.0:3260 ...................................................................................... [OK]
  o- loopback .......................................................................................... [Targets: 0]
  o- vhost ............................................................................................. [Targets: 0]
  o- xen-pvscsi ........................................................................................ [Targets: 0]
</code></pre>

### <a name="set-up-sbd-device"></a>SBD apparaat instellen

Verbinding maken met het iSCSI-apparaat dat is gemaakt in de vorige stap uit het cluster.
Voer de volgende opdrachten op de knooppunten van het nieuwe cluster die u wilt maken.
De volgende items worden voorafgegaan door een **[A]** : van toepassing op alle knooppunten **[1]** - alleen van toepassing op knooppunt 1 of **[2]** - alleen van toepassing op knooppunt 2.

1. **[A]**  Verbinding maken met iSCSI-apparaten

   Het iSCSI- en SBD services eerst inschakelen.

   <pre><code>sudo systemctl enable iscsid
   sudo systemctl enable iscsi
   sudo systemctl enable sbd
   </code></pre>

1. **[1]**  Wijzigt u de initiatornaam op het eerste knooppunt

   <pre><code>sudo vi /etc/iscsi/initiatorname.iscsi
   </code></pre>

   De inhoud van het bestand moet overeenkomen met de ACL's die u hebt gebruikt bij het maken van het iSCSI-apparaat op de iSCSI-doelserver, bijvoorbeeld voor de NFS-server wijzigen.

   <pre><code>InitiatorName=<b>iqn.2006-04.nfs-0.local:nfs-0</b>
   </code></pre>

1. **[2]**  Wijzig de initiatornaam op het tweede knooppunt

   <pre><code>sudo vi /etc/iscsi/initiatorname.iscsi
   </code></pre>

   De inhoud van het bestand moet overeenkomen met de ACL's die u hebt gebruikt bij het maken van het iSCSI-apparaat op de iSCSI-doelserver wijzigen

   <pre><code>InitiatorName=<b>iqn.2006-04.nfs-1.local:nfs-1</b>
   </code></pre>

1. **[A]**  De iSCSI-service opnieuw starten

   Nu opnieuw opstarten van de iSCSI-service als de wijziging wilt toepassen

   <pre><code>sudo systemctl restart iscsid
   sudo systemctl restart iscsi
   </code></pre>

   Verbinding maken met iSCSI-apparaten. In het onderstaande voorbeeld 10.0.0.17 is het IP-adres van de iSCSI-doelserver en 3260 is de standaardpoort. <b>IQN.2006 04.nfs.local:nfs</b> is een van de doelnamen die wordt weergegeven wanneer u de eerste opdracht hieronder (iscsiadm -m-discovery) uitvoert.

   <pre><code>sudo iscsiadm -m discovery --type=st --portal=<b>10.0.0.17:3260</b>   
   sudo iscsiadm -m node -T <b>iqn.2006-04.nfs.local:nfs</b> --login --portal=<b>10.0.0.17:3260</b>
   sudo iscsiadm -m node -p <b>10.0.0.17:3260</b> --op=update --name=node.startup --value=automatic
   
   # If you want to use multiple SBD devices, also connect to the second iSCSI target server
   sudo iscsiadm -m discovery --type=st --portal=<b>10.0.0.18:3260</b>   
   sudo iscsiadm -m node -T <b>iqn.2006-04.nfs.local:nfs</b> --login --portal=<b>10.0.0.18:3260</b>
   sudo iscsiadm -m node -p <b>10.0.0.18:3260</b> --op=update --name=node.startup --value=automatic
   
   # If you want to use multiple SBD devices, also connect to the third iSCSI target server
   sudo iscsiadm -m discovery --type=st --portal=<b>10.0.0.19:3260</b>   
   sudo iscsiadm -m node -T <b>iqn.2006-04.nfs.local:nfs</b> --login --portal=<b>10.0.0.19:3260</b>
   sudo iscsiadm -m node -p <b>10.0.0.19:3260</b> --op=update --name=node.startup --value=automatic
   </code></pre>

   Zorg ervoor dat de apparaten van de iSCSI-beschikbaar zijn en noteer de naam van het apparaat (in het volgende voorbeeld/dev/sde)

   <pre><code>lsscsi
   
   # [2:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sda
   # [3:0:1:0]    disk    Msft     Virtual Disk     1.0   /dev/sdb
   # [5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc
   # [5:0:0:1]    disk    Msft     Virtual Disk     1.0   /dev/sdd
   # <b>[6:0:0:0]    disk    LIO-ORG  sbdnfs           4.0   /dev/sdd</b>
   # <b>[7:0:0:0]    disk    LIO-ORG  sbdnfs           4.0   /dev/sde</b>
   # <b>[8:0:0:0]    disk    LIO-ORG  sbdnfs           4.0   /dev/sdf</b>
   </code></pre>

   Nu de id's van de iSCSI-apparaten worden opgehaald.

   <pre><code>ls -l /dev/disk/by-id/scsi-* | grep <b>sdd</b>
   
   # lrwxrwxrwx 1 root root  9 Aug  9 13:20 /dev/disk/by-id/scsi-1LIO-ORG_sbdnfs:afb0ba8d-3a3c-413b-8cc2-cca03e63ef42 -> ../../sdd
   # <b>lrwxrwxrwx 1 root root  9 Aug  9 13:20 /dev/disk/by-id/scsi-36001405afb0ba8d3a3c413b8cc2cca03 -> ../../sdd</b>
   # lrwxrwxrwx 1 root root  9 Aug  9 13:20 /dev/disk/by-id/scsi-SLIO-ORG_sbdnfs_afb0ba8d-3a3c-413b-8cc2-cca03e63ef42 -> ../../sdd
   
   ls -l /dev/disk/by-id/scsi-* | grep <b>sde</b>
   
   # lrwxrwxrwx 1 root root  9 Feb  7 12:39 /dev/disk/by-id/scsi-1LIO-ORG_cl1:3fe4da37-1a5a-4bb6-9a41-9a4df57770e4 -> ../../sde
   # <b>lrwxrwxrwx 1 root root  9 Feb  7 12:39 /dev/disk/by-id/scsi-360014053fe4da371a5a4bb69a419a4df -> ../../sde</b>
   # lrwxrwxrwx 1 root root  9 Feb  7 12:39 /dev/disk/by-id/scsi-SLIO-ORG_cl1_3fe4da37-1a5a-4bb6-9a41-9a4df57770e4 -> ../../sde
   
   ls -l /dev/disk/by-id/scsi-* | grep <b>sdf</b>
   
   # lrwxrwxrwx 1 root root  9 Aug  9 13:32 /dev/disk/by-id/scsi-1LIO-ORG_sbdnfs:f88f30e7-c968-4678-bc87-fe7bfcbdb625 -> ../../sdf
   # <b>lrwxrwxrwx 1 root root  9 Aug  9 13:32 /dev/disk/by-id/scsi-36001405f88f30e7c9684678bc87fe7bf -> ../../sdf</b>
   # lrwxrwxrwx 1 root root  9 Aug  9 13:32 /dev/disk/by-id/scsi-SLIO-ORG_sbdnfs_f88f30e7-c968-4678-bc87-fe7bfcbdb625 -> ../../sdf
   </code></pre>

   De opdracht lijst drie apparaat-id's voor elk apparaat SBD. Het is raadzaam het gebruik van de ID die met scsi-3, in het bovenstaande dit voorbeeld begint is

   * **/dev/Disk/by-id/SCSI-36001405afb0ba8d3a3c413b8cc2cca03**
   * **/dev/disk/by-id/scsi-360014053fe4da371a5a4bb69a419a4df**
   * **/dev/Disk/by-id/SCSI-36001405f88f30e7c9684678bc87fe7bf**

1. **[1]**  Maken van het apparaat SBD

   De apparaat-ID van de iSCSI-apparaten gebruiken om te maken van de nieuwe SBD apparaten op het eerste clusterknooppunt.

   <pre><code>sudo sbd -d <b>/dev/disk/by-id/scsi-36001405afb0ba8d3a3c413b8cc2cca03</b> -1 60 -4 120 create

   # Also create the second and third SBD devices if you want to use more than one.
   sudo sbd -d <b>/dev/disk/by-id/scsi-360014053fe4da371a5a4bb69a419a4df</b> -1 60 -4 120 create
   sudo sbd -d <b>/dev/disk/by-id/scsi-36001405f88f30e7c9684678bc87fe7bf</b> -1 60 -4 120 create
   </code></pre>

1. **[A]**  SBD config aanpassen

   Open het configuratiebestand SBD

   <pre><code>sudo vi /etc/sysconfig/sbd
   </code></pre>

   De eigenschap van het apparaat SBD wijzigen, de integratie van pacemaker inschakelen en de startmodus van SBD wijzigen.

   <pre><code>[...]
   <b>SBD_DEVICE="/dev/disk/by-id/scsi-36001405afb0ba8d3a3c413b8cc2cca03;/dev/disk/by-id/scsi-360014053fe4da371a5a4bb69a419a4df;/dev/disk/by-id/scsi-36001405f88f30e7c9684678bc87fe7bf"</b>
   [...]
   <b>SBD_PACEMAKER="yes"</b>
   [...]
   <b>SBD_STARTMODE="always"</b>
   [...]
   <b>SBD_WATCHDOG="yes"</b>
   </code></pre>

   Maak de `softdog` configuratiebestand

   <pre><code>echo softdog | sudo tee /etc/modules-load.d/softdog.conf
   </code></pre>

   Nu de module niet laden

   <pre><code>sudo modprobe -v softdog
   </code></pre>

## <a name="cluster-installation"></a>Clusterinstallatie van

De volgende items worden voorafgegaan door een **[A]** : van toepassing op alle knooppunten **[1]** - alleen van toepassing op knooppunt 1 of **[2]** - alleen van toepassing op knooppunt 2.

1. **[A]**  SLES bijwerken

   <pre><code>sudo zypper update
   </code></pre>

1. **[A]**  Configureren van het besturingssysteem

   In sommige gevallen Pacemaker maakt veel processen en daardoor creditbedrag het toegestane aantal processen. In dat geval kan een heartbeat tussen de knooppunten van het mislukken en leiden tot failover van uw resources. Het is raadzaam om de maximale toegestane processen verhogen door de volgende parameter.

   <pre><code># Edit the configuration file
   sudo vi /etc/systemd/system.conf
   
   # Change the DefaultTasksMax
   #DefaultTasksMax=512
   DefaultTasksMax=4096
   
   #and to activate this setting
   sudo systemctl daemon-reload
   
   # test if the change was successful
   sudo systemctl --no-pager show | grep DefaultTasksMax
   </code></pre>

   Reduceer de grootte van de vervuilde cache. Zie voor meer informatie, [laag schrijfvaardigheden op SLES 11/12 servers met grote RAM](https://www.suse.com/support/kb/doc/?id=7010287).

   <pre><code>sudo vi /etc/sysctl.conf

   # Change/set the following settings
   vm.dirty_bytes = 629145600
   vm.dirty_background_bytes = 314572800
   </code></pre>

1. **[A]**  Cloud-netconfig azure configureren voor HA-Cluster

   Wijzig het configuratiebestand voor de netwerkinterface, zoals hieronder wordt weergegeven om te voorkomen dat de netwerk-invoegtoepassing voor cloud verwijderen van het virtuele IP-adres (Pacemaker moet beheren van de VIP-toewijzing). Zie voor meer informatie [SUSE KB 7023633](https://www.suse.com/support/kb/doc/?id=7023633). 

   <pre><code># Edit the configuration file
   sudo vi /etc/sysconfig/network/ifcfg-eth0 
   
   # Change CLOUD_NETCONFIG_MANAGE
   # CLOUD_NETCONFIG_MANAGE="yes"
   CLOUD_NETCONFIG_MANAGE="no"
   </code></pre>

1. **[1]**  Ssh toegang inschakelen

   <pre><code>sudo ssh-keygen
   
   # Enter file in which to save the key (/root/.ssh/id_rsa): -> Press ENTER
   # Enter passphrase (empty for no passphrase): -> Press ENTER
   # Enter same passphrase again: -> Press ENTER
   
   # copy the public key
   sudo cat /root/.ssh/id_rsa.pub
   </code></pre>

1. **[2]**  Ssh toegang inschakelen

   <pre><code># insert the public key you copied in the last step into the authorized keys file on the second server
   sudo vi /root/.ssh/authorized_keys
   
   sudo ssh-keygen

   # Enter file in which to save the key (/root/.ssh/id_rsa): -> Press ENTER
   # Enter passphrase (empty for no passphrase): -> Press ENTER
   # Enter same passphrase again: -> Press ENTER
   
   # copy the public key
   sudo cat /root/.ssh/id_rsa.pub
   </code></pre>

1. **[1]**  Ssh toegang inschakelen

   <pre><code># insert the public key you copied in the last step into the authorized keys file on the first server
   sudo vi /root/.ssh/authorized_keys
   </code></pre>

1. **[A]**  Omheining installatie van agents
   
   <pre><code>sudo zypper install fence-agents
   </code></pre>

1. **[A]**  Omzetten van de hostnaam instellen

   U kunt een DNS-server gebruiken of aanpassen van de/etc/hosts op alle knooppunten. In dit voorbeeld laat zien hoe u het bestand/etc/hosts gebruikt.
   Vervang het IP-adres en de hostnaam in de volgende opdrachten. Het voordeel van het gebruik van/etc/hosts is dat het cluster wordt onafhankelijk van DNS, wat erop kan een single point of fouten te.

   <pre><code>sudo vi /etc/hosts
   </code></pre>

   Voeg de volgende regels/etc/hosts. De IP-adres en hostnaam zodat deze overeenkomen met uw omgeving wijzigen   

   <pre><code># IP address of the first cluster node
   <b>10.0.0.6 prod-cl1-0</b>
   # IP address of the second cluster node
   <b>10.0.0.7 prod-cl1-1</b>
   </code></pre>

1. **[1]**  Cluster installeren

   <pre><code>sudo ha-cluster-init
   
   # ! NTP is not configured to start at system boot.
   # Do you want to continue anyway (y/n)? <b>y</b>
   # /root/.ssh/id_rsa already exists - overwrite (y/n)? <b>n</b>
   # Network address to bind to (e.g.: 192.168.1.0) [10.0.0.0] <b>Press ENTER</b>
   # Multicast address (e.g.: 239.x.x.x) [239.232.97.43] <b>Press ENTER</b>
   # Multicast port [5405] <b>Press ENTER</b>
   # SBD is already configured to use /dev/disk/by-id/scsi-36001405639245768818458b930abdf69;/dev/disk/by-id/scsi-36001405afb0ba8d3a3c413b8cc2cca03;/dev/disk/by-id/scsi-36001405f88f30e7c9684678bc87fe7bf - overwrite (y/n)? <b>n</b>
   # Do you wish to configure an administration IP (y/n)? <b>n</b>
   </code></pre>

1. **[2]**  Knooppunt aan cluster toevoegen

   <pre><code>sudo ha-cluster-join
   
   # ! NTP is not configured to start at system boot.
   # Do you want to continue anyway (y/n)? <b>y</b>
   # IP address or hostname of existing node (e.g.: 192.168.1.1) []<b>10.0.0.6</b>
   # /root/.ssh/id_rsa already exists - overwrite (y/n)? <b>n</b>
   </code></pre>

1. **[A]**  Hacluster wachtwoord wijzigen naar hetzelfde wachtwoord

   <pre><code>sudo passwd hacluster
   </code></pre>

1. **[A]**  Corosync voor het gebruik van andere transport en nodelist toevoegen configureren. Cluster werkt niet anders.

   <pre><code>sudo vi /etc/corosync/corosync.conf
   </code></pre>

   De volgende vet inhoud toevoegen aan het bestand als de waarden niet er of een ander. Zorg ervoor dat u het token 30000 waarmee onderhoud met statusbehoud geheugen wijzigen. Zie voor meer informatie, [in dit artikel voor Linux] [ virtual-machines-linux-maintenance] of [Windows][virtual-machines-windows-maintenance]. Zorg ervoor dat u de parameter-mcastaddr verwijderen.

   <pre><code>[...]
     <b>token:          30000
     token_retransmits_before_loss_const: 10
     join:           60
     consensus:      36000
     max_messages:   20</b>
     
     interface { 
        [...] 
     }
     <b>transport:      udpu</b>
     # remove parameter mcastaddr
     <b># mcastaddr: IP</b>
   } 
   <b>nodelist {
     node {
      # IP address of <b>prod-cl1-0</b>
      ring0_addr:10.0.0.6
     }
     node {
      # IP address of <b>prod-cl1-1</b>
      ring0_addr:10.0.0.7
     } 
   }</b>
   logging {
     [...]
   }
   quorum {
        # Enable and configure quorum subsystem (default: off)
        # see also corosync.conf.5 and votequorum.5
        provider: corosync_votequorum
        <b>expected_votes: 2</b>
        <b>two_node: 1</b>
   }
   </code></pre>

   Start de service corosync

   <pre><code>sudo service corosync restart
   </code></pre>

## <a name="create-azure-fence-agent-stonith-device"></a>Azure omheining agent stonith instellen apparaat maken

Het stonith instellen-apparaat maakt gebruik van een Service-Principal te autoriseren op basis van Microsoft Azure. Volg deze stappen voor het maken van een Service-Principal.

1. Ga naar [https://portal.azure.com](https://portal.azure.com)
1. Open de Azure Active Directory-blade  
   Ga naar eigenschappen en noteer de map-ID. Dit is de **tenant-ID**.
1. Klik op App-registraties
1. Klik op Add.
1. Voer een naam in, selecteert u het Type toepassing 'Web-app/API', voer een aanmeldings-URL (bijvoorbeeld http\://localhost) en klik op maken
1. De aanmeldings-URL wordt niet gebruikt en kan geldige URL zijn
1. Selecteer de nieuwe App en sleutels op in het tabblad instellingen
1. Voer een beschrijving in voor een nieuwe sleutel, selecteer 'Verloopt nooit' en klik op Opslaan
1. Noteer de waarde in. Deze wordt gebruikt als de **wachtwoord** voor de Service-Principal
1. Noteer de toepassings-ID. Deze wordt gebruikt als de gebruikersnaam (**aanmeldings-ID** in de onderstaande stappen) van de Service-Principal

### <a name="1-create-a-custom-role-for-the-fence-agent"></a>**[1]**  Een aangepaste rol maken voor de agent omheining

De Service-Principal heeft geen machtigingen voor toegang tot uw Azure-resources standaard. U hoeft op te geven van de Service-Principal machtigingen voor starten en stoppen (toewijzing ongedaan maken) alle virtuele machines van het cluster. Als u de aangepaste rol die niet al hebt gemaakt, kunt u maken met behulp van [PowerShell](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-powershell) of [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli)

Gebruik de volgende inhoud voor het invoerbestand. U moet de inhoud voor uw abonnementen die is aangepast, c276fc76-9cd4-44c9-99a7-4fd71546436e en e91d47c4-76f3-4271-a796-21b4ecfe3624 vervangen door de id's van uw abonnement. Als u slechts één abonnement hebt, verwijdert u de tweede vermelding in AssignableScopes.

```json
{
  "Name": "Linux Fence Agent Role",
  "Id": null,
  "IsCustom": true,
  "Description": "Allows to deallocate and start virtual machines",
  "Actions": [
    "Microsoft.Compute/*/read",
    "Microsoft.Compute/virtualMachines/deallocate/action",
    "Microsoft.Compute/virtualMachines/start/action"
  ],
  "NotActions": [
  ],
  "AssignableScopes": [
    "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
    "/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624"
  ]
}
```

### <a name="a-assign-the-custom-role-to-the-service-principal"></a>**[A]**  De aangepaste rol toewijzen aan de Service-Principal

De aangepaste rol 'Linux omheining Agent rol' die is gemaakt in het vorige hoofdstuk aan de Service-Principal toewijzen. De rol van eigenaar niet meer gebruiken.

1. Ga naar [https://portal.azure.com](https://portal.azure.com)
1. Open de blade alle resources
1. Selecteer de virtuele machine van het eerste clusterknooppunt
1. Klik op de Access control (IAM)
1. Klik op de roltoewijzing toevoegen
1. Selecteer de rol 'Linux omheining Agent rol'
1. Voer de naam van de toepassing die u hierboven hebt gemaakt
1. Op Opslaan klikken

Herhaal de bovenstaande stappen voor het tweede clusterknooppunt.

### <a name="1-create-the-stonith-devices"></a>**[1]**  Maken de apparaten stonith instellen

Nadat u de machtigingen voor de virtuele machines hebt bewerkt, kunt u de apparaten stonith instellen in het cluster configureren.

<pre><code># replace the bold string with your subscription ID, resource group, tenant ID, service principal ID and password
sudo crm configure primitive rsc_st_azure stonith:fence_azure_arm \
   params subscriptionId="<b>subscription ID</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant ID</b>" login="<b>login ID</b>" passwd="<b>password</b>"

sudo crm configure property stonith-timeout=900
sudo crm configure property stonith-enabled=true
</code></pre>

## <a name="default-pacemaker-configuration-for-sbd"></a>Pacemaker standaardconfiguratie voor SBD

1. **[1]**  Stelt u het gebruik van een apparaat stonith instellen en de vertraging omheining

<pre><code>sudo crm configure property stonith-timeout=144
sudo crm configure property stonith-enabled=true

# List the resources to find the name of the SBD device
sudo crm resource list
sudo crm resource stop stonith-sbd
sudo crm configure delete <b>stonith-sbd</b>
sudo crm configure primitive <b>stonith-sbd</b> stonith:external/sbd \
   params pcmk_delay_max="15" \
   op monitor interval="15" timeout="15"
</code></pre>

## <a name="pacemaker-configuration-for-azure-scheduled-events"></a>Pacemaker configuratie voor Azure geplande gebeurtenissen

Azure biedt [geplande gebeurtenissen](https://docs.microsoft.com/azure/virtual-machines/linux/scheduled-events). Geplande gebeurtenissen worden geleverd via meta-data-service en om voldoende tijd voor de toepassing om voor te bereiden voor gebeurtenissen, zoals VM-uitschakelproces, nieuwe VM-implementatie, enzovoort. Resource-agent **[azure-gebeurtenissen](https://github.com/ClusterLabs/resource-agents/pull/1161)** monitors voor geplande Azure-evenementen. Als er gebeurtenissen zijn gedetecteerd, probeert de agent stoppen van alle resources op de betrokken virtuele machine en ze te verplaatsen naar een ander knooppunt in het cluster. Voor het bereiken van aanvullende Pacemaker resources moeten worden geconfigureerd. 

1. **[A]**  Installeren de **azure-gebeurtenissen** agent. 

<pre><code>sudo zypper install resource-agents
</code></pre>

2. **[1]**  De resources in Pacemaker configureren. 

<pre><code>
#Place the cluster in maintenance mode
sudo crm configure property maintenance-mode=true

#Create Pacemaker resources for the Azure agent
sudo crm configure primitive rsc_azure-events ocf:heartbeat:azure-events op monitor interval=10s
sudo crm configure clone cln_azure-events rsc_azure-events

#Take the cluster out of maintenance mode
sudo crm configure property maintenance-mode=false
</code></pre>

   > [!NOTE]
   > Nadat u de resources Pacemaker voor agent voor azure-evenementen, geconfigureerd wanneer u het cluster in of uit de onderhoudsmodus plaatst, krijgt u mogelijk waarschuwingen, zoals:  
     Waarschuwing: cib-bootstrap-opties: onbekend kenmerk ' hostName_  <strong>hostnaam</strong>'  
     Waarschuwing: cib-bootstrap-opties: onbekend kenmerk 'azure-events_globalPullState'  
     Waarschuwing: cib-bootstrap-opties: onbekend kenmerk ' hostName_ <strong>hostnaam</strong>'  
   > Deze waarschuwingen kunnen worden genegeerd.

## <a name="next-steps"></a>Volgende stappen

* [Azure virtuele Machines, planning en implementatie van SAP][planning-guide]
* [Azure Virtual Machines-implementatie voor SAP][deployment-guide]
* [Azure virtuele Machines DBMS-implementatie voor SAP][dbms-guide]
* [Hoge beschikbaarheid voor NFS op Azure VM's in SUSE Linux Enterprise Server][sles-nfs-guide]
* [Hoge beschikbaarheid voor SAP NetWeaver op Azure VM's in SUSE Linux Enterprise Server voor SAP-toepassingen][sles-guide]
* Zie voor meer informatie over het opzetten van hoge beschikbaarheid en plan voor herstel na noodgevallen van SAP HANA op Azure Virtual machines, [hoge beschikbaarheid van SAP HANA op Azure Virtual Machines (VM's)][sap-hana-ha]
