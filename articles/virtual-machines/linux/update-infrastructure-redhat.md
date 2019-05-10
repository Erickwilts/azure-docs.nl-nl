---
title: Red Hat Update Infrastructure | Microsoft Docs
description: Meer informatie over Red Hat Update Infrastructure voor on-demand Red Hat Enterprise Linux-instanties in Microsoft Azure
services: virtual-machines-linux
documentationcenter: ''
author: BorisB2015
manager: jeconnoc
editor: ''
ms.assetid: f495f1b4-ae24-46b9-8d26-c617ce3daf3a
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 5/7/2019
ms.author: borisb
ms.openlocfilehash: 7909ee1dc3980a5a4ff2418d4d6790361a66a65e
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65410710"
---
# <a name="red-hat-update-infrastructure-for-on-demand-red-hat-enterprise-linux-vms-in-azure"></a>Red Hat Update Infrastructure voor on-demand Red Hat Enterprise Linux-machines in Azure
 [Red Hat Update Infrastructure](https://access.redhat.com/products/red-hat-update-infrastructure) (RHUI) kunt u cloudproviders, zoals Azure, voor het spiegelen van de inhoud van Red Hat gehoste opslagplaats, aangepaste opslagplaatsen maken met Azure-specifieke inhoud en het beschikbaar maken voor virtuele machines door eindgebruikers.

Red Hat Enterprise Linux (RHEL) betalen naar gebruik (betalen per gebruik) afbeeldingen afkomstig zijn vooraf geconfigureerd voor toegang tot Azure RHUI. Er is geen aanvullende configuratie nodig. Uitvoeren als u de meest recente updates, `sudo yum update` nadat uw RHEL-exemplaar gereed is. Deze service is opgenomen als onderdeel van de kosten voor de RHEL PAYG-software.

Meer informatie over de RHEL-installatiekopieën in Azure, met inbegrip van publicatie- en bewaarbeleid, is beschikbaar [hier](./rhel-images.md).

Informatie over het beleid voor Red Hat-ondersteuning voor alle versies van RHEL vindt u op de [Red Hat Enterprise Linux-levenscyclus](https://access.redhat.com/support/policy/updates/errata) pagina.

## <a name="important-information-about-azure-rhui"></a>Belangrijke informatie over Azure RHUI
* Azure RHUI ondersteunt momenteel alleen de nieuwste secundaire versie van de RHEL-familie (RHEL6 of RHEL7). Als u een RHEL VM-exemplaar dat is verbonden met RHUI naar de nieuwste secundaire versie upgraden, uitvoeren `sudo yum update`.

    Bijvoorbeeld, als u een virtuele machine van een installatiekopie van een RHEL 7.4 PAYG inrichten en voer `sudo yum update`, krijgt u uiteindelijk met een 7.6 RHEL VM (de nieuwste secundaire versie in de familie RHEL7).

    Om te voorkomen dat dit probleem, moet u uw eigen installatiekopie bouwen zoals wordt beschreven in de [maken en uploaden, een Red Hat gebaseerde virtuele machine voor Azure](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) artikel. Moet u verbinding maakt met een andere update-infrastructuur ([rechtstreeks naar Red Hat content delivery servers](https://access.redhat.com/solutions/253273) of een [Red Hat satelliet server](https://access.redhat.com/products/red-hat-satellite)).

* Toegang tot de RHUI wordt gehost op Azure is opgenomen in de prijs van de installatiekopie RHEL PAYG. Als u een betalen per gebruik-RHEL VM uit de RHUI wordt gehost op Azure registratie is dat de virtuele machine niet converteren naar een type bring-your-own-license (BYOL) van virtuele machine. Als u dezelfde virtuele machine met een andere bron van de updates hebt geregistreerd, kunnen kosten voor _indirecte_ dubbele kosten in rekening gebracht. U betaalt het eerst voor de kosten van Azure RHEL-software. De tweede keer voor Red Hat-abonnementen die eerder zijn gekocht, moet u betalen. Als u consistent een update-infrastructuur dan RHUI wordt gehost op Azure gebruiken wilt, houd rekening met het maken en implementeren van uw eigen installatiekopieën (BYOL-type). Dit proces wordt beschreven [maken en uploaden, een Red Hat gebaseerde virtuele machine voor Azure](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

* SAP betalen per gebruik RHEL-installatiekopieën in Azure (RHEL for SAP, RHEL for SAP HANA en RHEL for SAP Business Applications) zijn verbonden met specifieke RHUI-kanalen die op de specifieke RHEL secundaire versie zoals vereist voor de SAP-certificering blijven.

* Toegang tot Azure gehoste RHUI is beperkt tot de virtuele machines binnen de [Azure datacenter IP-adresbereiken](https://www.microsoft.com/download/details.aspx?id=41653). Als u via een proxy alle VM-verkeer via een on-premises netwerkinfrastructuur, moet u mogelijk voor het instellen van de gebruiker gedefinieerde routes voor de RHEL-betalen per gebruik virtuele machines voor toegang tot de Azure-RHUI.

## <a name="rhel-eus-and-version-locking-rhel-vms"></a>RHEL EUS en versie vergrendelen RHEL VM 's
Sommige klanten willen hun RHEL VM's naar een bepaalde kleine RHEL-versie vergrendelen. U kunt uw RHEL VM naar een specifieke secundaire versie versie-lock door bij te werken van de opslagplaatsen om te verwijzen naar de uitgebreide ondersteuning voor Update-opslagplaatsen. Gebruik de volgende instructies om te vergrendelen van een RHEL VM naar een specifieke secundaire versie:

>[!NOTE]
> Dit geldt alleen voor RHEL-versies waarvoor EUS beschikbaar is. Dit omvat RHEL 7.2 7.6 op het moment van schrijven. Meer details zijn beschikbaar op de [Red Hat Enterprise Linux-levenscyclus](https://access.redhat.com/support/policy/updates/errata) pagina.

1. Niet-EUS opslagplaatsen uitschakelen:
    ```bash
    sudo yum --disablerepo='*' remove 'rhui-azure-rhel7'
    ```

1. EUS opslagplaatsen toevoegen:
    ```bash
    yum --config='https://rhelimage.blob.core.windows.net/repositories/rhui-microsoft-azure-rhel7-eus.config' install 'rhui-azure-rhel7-eus'
    ```

1. De variabele releasever vergrendelen:
    ```bash
    echo $(. /etc/os-release && echo $VERSION_ID) > /etc/yum/vars/releasever
    ```

    >[!NOTE]
    > De bovenstaande instructies, de secundaire versie van RHEL naar de huidige secundaire versie wordt vergrendeld. Voer een specifieke secundaire versie als u wilt bijwerken en een vergrendeling van naar een hoger secundaire versie aan die niet de nieuwste versie. Bijvoorbeeld, `echo 7.5 > /etc/yum/vars/releasever` uw RHEL-versie op RHEL 7,5 wordt vergrendeld

1. Uw RHEL VM bijwerken
    ```bash
    sudo yum update
    ```

## <a name="the-ips-for-the-rhui-content-delivery-servers"></a>De IP-adressen voor de levering van inhoud RHUI servers

RHUI is beschikbaar in alle regio's waar de RHEL-installatiekopieën voor op aanvraag beschikbaar zijn. Het bevat momenteel alle openbare regio's die worden vermeld op de [Azure-statusdashboard](https://azure.microsoft.com/status/) pagina-, Azure US Government- en Microsoft Azure Duitsland-regio's.

Als u verder beperken van toegang van RHEL betalen per gebruik virtuele machines in een configuratie van het netwerk, zorg ervoor dat de volgende IP-adressen zijn toegestaan voor `yum update` om te werken, afhankelijk van de omgeving waarin u werkt in:


```
# Azure Global
13.91.47.76
40.85.190.91
52.187.75.218
52.174.163.213
52.237.203.198

# Azure US Government
13.72.186.193
13.72.14.155
52.244.249.194

# Azure Germany
51.5.243.77
51.4.228.145
```

## <a name="azure-rhui-infrastructure"></a>Azure RHUI-infrastructuur


### <a name="update-expired-rhui-client-certificate-on-a-vm"></a>Verlopen RHUI-clientcertificaat op een virtuele machine bijwerken

Als u een oudere RHEL VM-installatiekopie, bijvoorbeeld RHEL 7.4 (image-URN: `RedHat:RHEL:7.4:7.4.2018010506`), wordt er verbindingsproblemen met RHUI vanwege een nu verlopen SSL-clientcertificaat. De fout die u ziet eruit als _'SSL peer geweigerd uw certificaat als verlopen'_ of _' fout: Kan de opslagplaats metagegevens (repomd.xml) voor de opslagplaats niet ophalen:... Controleer of het pad en probeer het opnieuw"_. Werk de RHUI-clientpakket op de virtuele machine met behulp van de volgende opdracht uit om dit probleem oplossen:

```bash
sudo yum update -y --disablerepo='*' --enablerepo='*microsoft*'
```

U kunt ook uitgevoerd `sudo yum update` het clientpakket certificaat (afhankelijk van de RHEL-versie), kan ook worden bijgewerkt ondanks fouten van 'verlopen SSL-certificaat' u voor andere opslagplaatsen ziet. Als deze update geslaagd is, normale connectiviteit met andere RHUI-opslagplaatsen moet worden hersteld, zodat u kunt uitvoeren `sudo yum update` is.

Als u een 404-fout tijdens de uitvoering een `yum update`, probeert u het volgende als u wilt vernieuwen van uw yum-cache:
```bash
sudo yum clean all;
sudo yum makecache
```

### <a name="troubleshoot-connection-problems-to-azure-rhui"></a>Problemen met verbinding naar Azure RHUI
Als u problemen hebt met het verbinding maken met Azure RHUI vanuit uw virtuele machine van Azure RHEL betalen per gebruik, volg deze stappen:

1. Inspecteer de configuratie van de virtuele machine voor het Azure-RHUI-eindpunt:

    a. Controleer of de `/etc/yum.repos.d/rh-cloud.repo` bestand bevat een verwijzing naar `rhui-[1-3].microsoft.com` in de `baseurl` van de `[rhui-microsoft-azure-rhel*]` sectie van het bestand. Als dit het geval is, gebruikt u de nieuwe Azure-RHUI.

    b. Als deze naar een locatie met het volgende patroon verwijst `mirrorlist.*cds[1-4].cloudapp.net`, een configuratie-update is vereist. U de oude VM-momentopname en moet u deze om te verwijzen naar de nieuwe Azure-RHUI bij te werken.

1. Toegang tot Azure gehoste RHUI is beperkt tot virtuele machines binnen de [Azure datacenter IP-adresbereiken](https://www.microsoft.com/download/details.aspx?id=41653).

1. Als u de nieuwe configuratie, hebt gecontroleerd dat de virtuele machine verbinding vanuit de Azure-IP-adresbereik maakt en nog steeds geen verbinding met Azure RHUI, bestand een ondersteuningscase met Microsoft of Red Hat maken.

### <a name="infrastructure-update"></a>Infrastructuur-update

In September 2016 gaan we een bijgewerkte Azure RHUI geïmplementeerd. In April 2017 wordt de oude Azure RHUI afgesloten. Als u eerder hebt gebruikt de PAYG RHEL-installatiekopieën (of hun momentopnamen) vanaf September 2016 of hoger, bent u automatisch verbinding met de nieuwe Azure-RHUI. Als u oudere momentopnamen echter op uw virtuele machines hebt, moet u handmatig bijwerken hun configuratie voor toegang tot de Azure-RHUI zoals beschreven in een volgende sectie.

De nieuwe Azure RHUI-servers worden geïmplementeerd met [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/). In Traffic Manager, één eindpunt (rhui-1.microsoft.com) kan worden gebruikt door een virtuele machine, ongeacht de regio.

### <a name="manual-update-procedure-to-use-the-azure-rhui-servers"></a>Handmatige updateprocedure voor het gebruik van de Azure-RHUI-servers
Deze procedure is alleen ter informatie bedoeld. Er is al de juiste configuratie verbinding maken met Azure RHUI PAYG RHEL-installatiekopieën. Voor het handmatig bijwerken van de configuratie voor het gebruik van de Azure-RHUI-servers, voert u de volgende stappen uit:

1. De handtekening van de openbare sleutel via curl gedownload.

   ```bash
   curl -o RPM-GPG-KEY-microsoft-azure-release https://download.microsoft.com/download/9/D/9/9d945f05-541d-494f-9977-289b3ce8e774/microsoft-sign-public.asc
   ```

1. Controleer of de geldigheid van de gedownloade sleutel.

   ```bash
   gpg --list-packets --verbose < RPM-GPG-KEY-microsoft-azure-release
   ```

1. Controleer de uitvoer en controleer vervolgens of de `keyid` en de `user ID packet`.

   ```bash
   Version: GnuPG v1.4.7 (GNU/Linux)
   :public key packet:
           version 4, algo 1, created 1446074508, expires 0
           pkey[0]: [2048 bits]
           pkey[1]: [17 bits]
           keyid: EB3E94ADBE1229CF
   :user ID packet: "Microsoft (Release signing) <gpgsecurity@microsoft.com>"
   :signature packet: algo 1, keyid EB3E94ADBE1229CF
           version 4, created 1446074508, md5len 0, sigclass 0x13
           digest algo 2, begin of digest 1a 9b
           hashed subpkt 2 len 4 (sig created 2015-10-28)
           hashed subpkt 27 len 1 (key flags: 03)
           hashed subpkt 11 len 5 (pref-sym-algos: 9 8 7 3 2)
           hashed subpkt 21 len 3 (pref-hash-algos: 2 8 3)
           hashed subpkt 22 len 2 (pref-zip-algos: 2 1)
           hashed subpkt 30 len 1 (features: 01)
           hashed subpkt 23 len 1 (key server preferences: 80)
           subpkt 16 len 8 (issuer key ID EB3E94ADBE1229CF)
           data: [2047 bits]
   ```

1. Installeer de openbare sleutel.

   ```bash
   sudo install -o root -g root -m 644 RPM-GPG-KEY-microsoft-azure-release /etc/pki/rpm-gpg
   sudo rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-microsoft-azure-release
   ```

1. Downloaden, controleren en installeren van een client RPM Package Manager (RPM).

    >[!NOTE]
    >Pakketversies wijzigen. Als u handmatig aan Azure RHUI koppelt, kunt u de meest recente versie van de clientpakket voor elke serie RHEL vinden door in te richten van de meest recente installatiekopie uit de galerie.

   a. Download.

    - Voor RHEL 6:
        ```bash
        curl -o azureclient.rpm https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel6/Packages/r/rhui-azure-rhel6-2.2-74.noarch.rpm
        ```

    - Voor RHEL 7:
        ```bash
        curl -o azureclient.rpm https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel7/Packages/r/rhui-azure-rhel7-2.2-74.noarch.rpm
        ```

   b. Controleer of.

   ```bash
   rpm -Kv azureclient.rpm
   ```

   c. Controleer de uitvoer om ervoor te zorgen dat de handtekening van het pakket is in orde.

   ```bash
   azureclient.rpm:
       Header V3 RSA/SHA256 Signature, key ID be1229cf: OK
       Header SHA1 digest: OK (927a3b548146c95a3f6c1a5d5ae52258a8859ab3)
       V3 RSA/SHA256 Signature, key ID be1229cf: OK
       MD5 digest: OK (c04ff605f82f4be8c96020bf5c23b86c)
   ```

   d. Installeer het RPM.

    ```bash
    sudo rpm -U azureclient.rpm
    ```

1. Nadat u klaar bent, moet u controleren of u toegang hebt tot Azure RHUI van de virtuele machine.

## <a name="next-steps"></a>Volgende stappen
* Een Red Hat Enterprise Linux-VM maken vanaf een betalen per gebruik van Azure Marketplace-installatiekopie en RHUI wordt gehost op Azure gebruiken, gaat u naar de [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/redhat/).
* Voor meer informatie over de Red Hat-afbeeldingen in Azure, gaat u naar de [documentatiepagina](./rhel-images.md).
* Informatie over het beleid voor Red Hat-ondersteuning voor alle versies van RHEL vindt u op de [Red Hat Enterprise Linux-levenscyclus](https://access.redhat.com/support/policy/updates/errata) pagina.
