---
title: Een op CentOS gebaseerde Linux-VHD maken en uploaden in azure
description: Meer informatie over het maken en uploaden van een virtuele harde schijf (VHD) van Azure die een CentOS Linux-besturings systeem bevat.
services: virtual-machines-linux
documentationcenter: ''
author: MicahMcKittrick-MSFT
manager: gwallace
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 11/25/2019
ms.author: mimckitt
ms.openlocfilehash: a5a6dd7e451d3204c8ab2bd2eae95db93c2f0837
ms.sourcegitcommit: 380e3c893dfeed631b4d8f5983c02f978f3188bf
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2020
ms.locfileid: "75750225"
---
# <a name="prepare-a-centos-based-virtual-machine-for-azure"></a>Een op CentOS gebaseerde virtuele Azure-machine voorbereiden

Meer informatie over het maken en uploaden van een virtuele harde schijf (VHD) van Azure die een CentOS Linux-besturings systeem bevat.

* [Een virtuele machine met CentOS 6. x voorbereiden voor Azure](#centos-6x)
* [Een CentOS 7.0 +-virtuele machine voorbereiden voor Azure](#centos-70)


## <a name="prerequisites"></a>Vereisten

In dit artikel wordt ervan uitgegaan dat u al een CentOS-Linux-besturings systeem (of een vergelijkbaar afgeleide derivaten) hebt geïnstalleerd op een virtuele harde schijf. Er zijn meerdere hulpprogram ma's voor het maken van VHD-bestanden, zoals Hyper-V. Zie [de Hyper-V-functie installeren en een virtuele machine configureren](https://technet.microsoft.com/library/hh846766.aspx)voor instructies.

**CentOS-installatie notities**

* Zie ook [algemene Linux-installatie notities](create-upload-generic.md#general-linux-installation-notes) voor meer tips over het voorbereiden van Linux voor Azure.
* De VHDX-indeling wordt niet ondersteund in azure, alleen **vaste VHD**.  U kunt de schijf converteren naar VHD-indeling met behulp van Hyper-V-beheer of de cmdlet Convert-VHD. Als u VirtualBox gebruikt, betekent dit het selecteren van een **vaste grootte** in plaats van de standaard waarde die dynamisch wordt toegewezen bij het maken van de schijf.
* Bij de installatie van het Linux-systeem wordt *Aanbevolen* om standaard partities te gebruiken in plaats van LVM (vaak de standaard instelling voor veel installaties). Hiermee wordt voor komen dat LVM naam strijdig is met gekloonde Vm's, met name als een besturingssysteem schijf ooit moet worden gekoppeld aan een andere identieke virtuele machine voor het oplossen van problemen. [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) of [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) kan worden gebruikt op gegevens schijven.
* Kernel-ondersteuning voor het koppelen van UDF-bestands systemen is vereist. Bij de eerste keer opstarten in azure wordt de inrichtings configuratie door gegeven aan de Linux-VM via met UDF geformatteerde media die aan de gast zijn gekoppeld. De Azure Linux-agent moet het UDF-bestands systeem kunnen koppelen om de configuratie te lezen en de virtuele machine in te richten.
* De Linux-kernel-versies onder 2.6.37 bieden geen ondersteuning voor NUMA op Hyper-V met grotere VM-grootten. Dit probleem heeft voornamelijk betrekking op oudere distributies met behulp van de upstream Red Hat 2.6.32 kernel en is opgelost in RHEL 6,6 (kernel-2.6.32-504). Systemen met aangepaste kernels die ouder zijn dan 2.6.37 of op RHEL gebaseerde kernels die ouder zijn dan 2.6.32-504, moeten de opstart parameter `numa=off` instellen op de kernel-opdracht regel in grub. conf. Zie Red Hat [KB 436883](https://access.redhat.com/solutions/436883)voor meer informatie.
* Configureer geen swap partitie op de besturingssysteem schijf. De Linux-agent kan worden geconfigureerd voor het maken van een wissel bestand op de tijdelijke bron schijf.  Meer informatie hierover vindt u in de volgende stappen.
* Alle Vhd's op Azure moeten een virtuele grootte hebben die is afgestemd op 1 MB. Wanneer u van een onbewerkte schijf naar VHD converteert, moet u ervoor zorgen dat de onbewerkte schijf grootte een meervoud van 1MB is vóór de conversie. Zie [installatie notities voor Linux](create-upload-generic.md#general-linux-installation-notes) voor meer informatie.

## <a name="centos-6x"></a>CentOS 6. x

1. Selecteer de virtuele machine in Hyper-V-beheer.

2. Klik op **verbinding maken** om een console venster voor de virtuele machine te openen.

3. In CentOS 6 kan NetworkManager conflicteren met de Azure Linux-agent. Verwijder dit pakket door de volgende opdracht uit te voeren:

    ```bash
    sudo rpm -e --nodeps NetworkManager
    ```

4. Maak of bewerk het bestand `/etc/sysconfig/network` en voeg de volgende tekst toe:

    ```console
    NETWORKING=yes
    HOSTNAME=localhost.localdomain
    ```

5. Maak of bewerk het bestand `/etc/sysconfig/network-scripts/ifcfg-eth0` en voeg de volgende tekst toe:

    ```console
    DEVICE=eth0
    ONBOOT=yes
    BOOTPROTO=dhcp
    TYPE=Ethernet
    USERCTL=no
    PEERDNS=yes
    IPV6INIT=no
    ```

6. Wijzig de udev-regels om te voor komen dat er statische regels voor de Ethernet-interface (s) worden gegenereerd. Deze regels kunnen problemen veroorzaken bij het klonen van een virtuele machine in Microsoft Azure of Hyper-V:

    ```bash
    sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
    sudo rm -f /etc/udev/rules.d/70-persistent-net.rules
    ```

7. Zorg ervoor dat de netwerk service wordt gestart op het moment dat de computer wordt opgestart door de volgende opdracht uit te voeren:

    ```bash
    sudo chkconfig network on
    ```

8. Als u de open Logic mirrors wilt gebruiken die worden gehost in de Azure-data centers, vervangt u het `/etc/yum.repos.d/CentOS-Base.repo` bestand door de volgende opslag plaatsen.  Hiermee wordt ook de opslag plaats **[open Logic]** toegevoegd die aanvullende pakketten bevat, zoals de Azure Linux-agent:

   ```console
   [openlogic]
   name=CentOS-$releasever - openlogic packages for $basearch
   baseurl=http://olcentgbl.trafficmanager.net/openlogic/$releasever/openlogic/$basearch/
   enabled=1
   gpgcheck=0

   [base]
   name=CentOS-$releasever - Base
   #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os&infra=$infra
   baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/os/$basearch/
   gpgcheck=1
   gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

   #released updates
   [updates]
   name=CentOS-$releasever - Updates
   #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=updates&infra=$infra
   baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/updates/$basearch/
   gpgcheck=1
   gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

   #additional packages that may be useful
   [extras]
   name=CentOS-$releasever - Extras
   #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=extras&infra=$infra
   baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/extras/$basearch/
   gpgcheck=1
   gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

   #additional packages that extend functionality of existing packages
   [centosplus]
   name=CentOS-$releasever - Plus
   #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=centosplus&infra=$infra
   baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/centosplus/$basearch/
   gpgcheck=1
   enabled=0
   gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

   #contrib - packages by Centos Users
   [contrib]
   name=CentOS-$releasever - Contrib
   #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=contrib&infra=$infra
   baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/contrib/$basearch/
   gpgcheck=1
   enabled=0
   gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
   ```

    > [!Note]
    > In de rest van deze hand leiding wordt ervan uitgegaan dat u ten minste de `[openlogic]`-opslag plaats gebruikt, die zal gebruiken om de onderstaande Azure Linux-agent te installeren.

9. Voeg de volgende regel toe aan/etc/yum.conf:

    ```console
    http_caching=packages
    ```

10. Voer de volgende opdracht uit om de huidige yum-meta gegevens te wissen en het systeem bij te werken met de meest recente pakketten:

    ```bash
    yum clean all
    ```

    Tenzij u een installatie kopie voor een oudere versie van CentOS maakt, wordt aangeraden alle pakketten naar het meest recente bestand bij te werken:

    ```bash
    sudo yum -y update
    ```

    Na het uitvoeren van deze opdracht is mogelijk opnieuw opstarten vereist.

11. Beschrijving Installeer de Stuur Programma's voor de LIS (Linux Integration Services).

    > [!IMPORTANT]
    > De stap is **vereist** voor CentOS 6,3 en eerder, en is optioneel voor latere releases.

    ```bash
    sudo rpm -e hypervkvpd  ## (may return error if not installed, that's OK)
    sudo yum install microsoft-hyper-v
    ```

    U kunt ook de hand matige installatie-instructies op de [Lis-pagina](https://www.microsoft.com/download/details.aspx?id=51612) volgen om de rpm te installeren op uw VM.

12. Installeer de Azure Linux-agent en-afhankelijkheden. De waagent-service starten en inschakelen:

    ```bash
    sudo yum install python-pyasn1 WALinuxAgent
    sudo service waagent start
    sudo chkconfig waagent on
    ```


    Het WALinuxAgent-pakket verwijdert de NetworkManager-en NetworkManager-gnome-pakketten als deze nog niet zijn verwijderd, zoals beschreven in stap 3.

13. Wijzig de kernel-opstart regel in de grub-configuratie zodat er aanvullende kernel-para meters voor Azure zijn. Als u dit wilt doen, opent u `/boot/grub/menu.lst` in een tekst editor en zorgt u ervoor dat de standaard-kernel de volgende para meters bevat:

    ```console
    console=ttyS0 earlyprintk=ttyS0 rootdelay=300
    ```

    Dit zorgt ervoor dat alle console berichten worden verzonden naar de eerste seriële poort, die ondersteuning voor Azure kan helpen bij het oplossen van problemen.

    Naast het bovenstaande wordt het aanbevolen om de volgende para meters te *verwijderen* :

    ```console
    rhgb quiet crashkernel=auto
    ```

    Grafisch en stil opstarten zijn niet handig in een cloud omgeving waar alle logboeken moeten worden verzonden naar de seriële poort.  De optie `crashkernel` kan eventueel worden geconfigureerd, maar houd er rekening mee dat met deze para meter de hoeveelheid beschikbaar geheugen in de VM met 128 MB of meer wordt verminderd, wat kan leiden tot een probleem met de kleinere VM-grootten.

    > [!Important]
    > CentOS 6,5 en lager moeten ook de para meter kernel `numa=off`instellen. Zie Red Hat [KB 436883](https://access.redhat.com/solutions/436883).

14. Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te starten bij het opstarten.  Dit is doorgaans de standaard instelling.

15. Maak geen wissel ruimte op de besturingssysteem schijf.

    De Azure Linux-agent kan tijdens het inrichten van Azure automatisch wissel ruimte configureren met de lokale bron schijf die aan de VM is gekoppeld. Houd er rekening mee dat de lokale bron schijf een *tijdelijke* schijf is en kan worden leeg gemaakt wanneer de inrichting van de virtuele machine wordt opheffen. Nadat u de Azure Linux-agent hebt geïnstalleerd (Zie de vorige stap), wijzigt u de volgende para meters in `/etc/waagent.conf` op de juiste manier:

    ```console
    ResourceDisk.Format=y
    ResourceDisk.Filesystem=ext4
    ResourceDisk.MountPoint=/mnt/resource
    ResourceDisk.EnableSwap=y
    ResourceDisk.SwapSizeMB=2048 ## NOTE: set this to whatever you need it to be.
    ```

16. Voer de volgende opdrachten uit om de inrichting van de virtuele machine ongedaan te maken en deze voor te bereiden voor de inrichting van Azure:

    ```bash
    sudo waagent -force -deprovision
    export HISTSIZE=0
    logout
    ```

17. Klik op **actie-> afgesloten** in Hyper-V-beheer. Uw Linux-VHD is nu gereed om te worden geüpload naar Azure.



## <a name="centos-70"></a>CentOS 7.0+

**Wijzigingen in CentOS 7 (en vergelijk bare derivaten)**

Het voorbereiden van een virtuele machine voor CentOS 7 voor Azure lijkt sterk op CentOS 6, maar er zijn echter verschillende belang rijke verschillen:

* Het NetworkManager-pakket is niet meer strijdig met de Azure Linux-agent. Dit pakket wordt standaard geïnstalleerd en wordt aangeraden dat het niet wordt verwijderd.
* GRUB2 wordt nu gebruikt als de standaard-bootloader, dus de procedure voor het bewerken van kernel-para meters is gewijzigd (zie hieronder).
* XFS is nu het standaard bestandssysteem. Het ext4-bestands systeem kan nog steeds worden gebruikt, indien gewenst.

**Configuratie stappen**

1. Selecteer de virtuele machine in Hyper-V-beheer.

2. Klik op **verbinding maken** om een console venster voor de virtuele machine te openen.

3. Maak of bewerk het bestand `/etc/sysconfig/network` en voeg de volgende tekst toe:

    ```console
    NETWORKING=yes
    HOSTNAME=localhost.localdomain
    ```

4. Maak of bewerk het bestand `/etc/sysconfig/network-scripts/ifcfg-eth0` en voeg de volgende tekst toe:

    ```console
    DEVICE=eth0
    ONBOOT=yes
    BOOTPROTO=dhcp
    TYPE=Ethernet
    USERCTL=no
    PEERDNS=yes
    IPV6INIT=no
    NM_CONTROLLED=no
    ```

5. Wijzig de udev-regels om te voor komen dat er statische regels voor de Ethernet-interface (s) worden gegenereerd. Deze regels kunnen problemen veroorzaken bij het klonen van een virtuele machine in Microsoft Azure of Hyper-V:

    ```bash
    sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
    ```

6. Als u de open Logic mirrors wilt gebruiken die worden gehost in de Azure-data centers, vervangt u het `/etc/yum.repos.d/CentOS-Base.repo` bestand door de volgende opslag plaatsen.  Hiermee wordt ook de opslag plaats **[open Logic]** toegevoegd die pakketten bevat voor de Azure Linux-agent:

   ```console
   [openlogic]
   name=CentOS-$releasever - openlogic packages for $basearch
   baseurl=http://olcentgbl.trafficmanager.net/openlogic/$releasever/openlogic/$basearch/
   enabled=1
   gpgcheck=0
    
   [base]
   name=CentOS-$releasever - Base
   #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os&infra=$infra
   baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/os/$basearch/
   gpgcheck=1
   gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
    
   #released updates
   [updates]
   name=CentOS-$releasever - Updates
   #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=updates&infra=$infra
   baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/updates/$basearch/
   gpgcheck=1
   gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
    
   #additional packages that may be useful
   [extras]
   name=CentOS-$releasever - Extras
   #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=extras&infra=$infra
   baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/extras/$basearch/
   gpgcheck=1
   gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
    
   #additional packages that extend functionality of existing packages
   [centosplus]
   name=CentOS-$releasever - Plus
   #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=centosplus&infra=$infra
   baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/centosplus/$basearch/
   gpgcheck=1
   enabled=0
   gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
   ```
    
   > [!Note]
   > In de rest van deze hand leiding wordt ervan uitgegaan dat u ten minste de `[openlogic]`-opslag plaats gebruikt, die zal gebruiken om de onderstaande Azure Linux-agent te installeren.

7. Voer de volgende opdracht uit om de huidige yum-meta gegevens te wissen en updates te installeren:

    ```bash
    sudo yum clean all
    ```

    Tenzij u een installatie kopie voor een oudere versie van CentOS maakt, wordt aangeraden alle pakketten naar het meest recente bestand bij te werken:

    ```bash
    sudo yum -y update
    ```

    Opnieuw opstarten is mogelijk vereist na het uitvoeren van deze opdracht.

8. Wijzig de kernel-opstart regel in de grub-configuratie zodat er aanvullende kernel-para meters voor Azure zijn. Als u dit wilt doen, opent u `/etc/default/grub` in een tekst editor en bewerkt u de para meter `GRUB_CMDLINE_LINUX`, bijvoorbeeld:

    ```console
    GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
    ```

   Dit zorgt ervoor dat alle console berichten worden verzonden naar de eerste seriële poort, die ondersteuning voor Azure kan helpen bij het oplossen van problemen. De nieuwe CentOS 7-naamgevings conventies voor Nic's worden ook uitgeschakeld. Naast het bovenstaande wordt het aanbevolen om de volgende para meters te *verwijderen* :

    ```console
    rhgb quiet crashkernel=auto
    ```

    Grafisch en stil opstarten zijn niet handig in een cloud omgeving waar alle logboeken moeten worden verzonden naar de seriële poort. De optie `crashkernel` kan eventueel worden geconfigureerd, maar houd er rekening mee dat met deze para meter de hoeveelheid beschikbaar geheugen in de VM met 128 MB of meer wordt verminderd, wat kan leiden tot een probleem met de kleinere VM-grootten.

9. Als u klaar bent met het bewerken van `/etc/default/grub` per hierboven, voert u de volgende opdracht uit om de grub-configuratie opnieuw samen te stellen:

    ```bash
    sudo grub2-mkconfig -o /boot/grub2/grub.cfg
    ```

10. Als u de installatie kopie bouwt van **VMware, VirtualBox of KVM:** Controleer of de Hyper-V-Stuur Programma's zijn opgenomen in de initramfs:

    `/etc/dracut.conf`bewerken, inhoud toevoegen:

    ```console
    add_drivers+=" hv_vmbus hv_netvsc hv_storvsc "
    ```

    De initramfs opnieuw samen stellen:

    ```bash
    sudo dracut -f -v
    ```

11. De Azure Linux-agent en-afhankelijkheden installeren:

    ```bash
    sudo yum install python-pyasn1 WALinuxAgent
    sudo systemctl enable waagent
    ```

12. Maak geen wissel ruimte op de besturingssysteem schijf.

    De Azure Linux-agent kan tijdens het inrichten van Azure automatisch wissel ruimte configureren met de lokale bron schijf die aan de VM is gekoppeld. Houd er rekening mee dat de lokale bron schijf een *tijdelijke* schijf is en kan worden leeg gemaakt wanneer de inrichting van de virtuele machine wordt opheffen. Nadat u de Azure Linux-agent hebt geïnstalleerd (Zie de vorige stap), wijzigt u de volgende para meters in `/etc/waagent.conf` op de juiste manier:

    ```console
    ResourceDisk.Format=y
    ResourceDisk.Filesystem=ext4
    ResourceDisk.MountPoint=/mnt/resource
    ResourceDisk.EnableSwap=y
    ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.
    ```

13. Voer de volgende opdrachten uit om de inrichting van de virtuele machine ongedaan te maken en deze voor te bereiden voor de inrichting van Azure:

    ```bash
    sudo waagent -force -deprovision
    export HISTSIZE=0
    logout
    ```

14. Klik op **actie-> afgesloten** in Hyper-V-beheer. Uw Linux-VHD is nu gereed om te worden geüpload naar Azure.

## <a name="next-steps"></a>Volgende stappen

U kunt nu de virtuele harde schijf van CentOS Linux gebruiken om nieuwe virtuele machines te maken in Azure. Zie [een virtuele Linux-machine maken op basis van een aangepaste schijf](upload-vhd.md#option-1-upload-a-vhd)als dit de eerste keer is dat u het VHD-bestand naar Azure uploadt.
