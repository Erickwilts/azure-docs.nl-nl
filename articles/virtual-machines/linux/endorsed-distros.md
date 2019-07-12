---
title: Linux-distributies die zijn goedgekeurd op Azure | Microsoft Docs
description: Meer informatie over Linux op door Azure onderschreven distributies, met inbegrip van richtlijnen voor Ubuntu, CentOS, Oracle en SUSE.
services: virtual-machines-linux
documentationcenter: ''
author: szarkos
manager: gwallace
editor: tysonn
tags: azure-service-management,azure-resource-manager
ms.assetid: 2777a526-c260-4cb9-a31a-bdfe1a55fffc
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/16/2019
ms.author: szark
ms.openlocfilehash: 172267af394885d0c5ac0a0717de87e968182d37
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67667854"
---
# <a name="endorsed-linux-distributions-on-azure"></a>Linux-distributies op Azure
-Partners bieden Linux-installatiekopieën in de Azure Marketplace. We werken met verschillende Linux-community's nog meer varianten toevoegen aan de lijst met distributiepunten die zijn goedgekeurd. In de tussentijd voor distributies die niet beschikbaar zijn vanuit de Marketplace, u kunt altijd brengt uw eigen Linux door de richtlijnen op [maken en uploaden van een virtuele harde schijf met het Linux-besturingssysteem](https://docs.microsoft.com/azure/virtual-machines/linux/create-upload-generic).

## <a name="supported-distributions-and-versions"></a>Ondersteunde distributies en versies
De volgende tabel bevat de Linux-distributies en versies die worden ondersteund op Azure. Raadpleeg [ondersteuning voor Linux-installatiekopieën in Microsoft Azure](https://support.microsoft.com/help/2941892/support-for-linux-and-open-source-technology-in-azure) voor meer informatie over ondersteuning voor Linux en open-source-technologie in Azure gedetailleerde.

De Linux Integration Services (LIS) stuurprogramma's voor Hyper-V en Azure zijn kernelmodules die deel uitmaakt van Microsoft rechtstreeks naar de upstream-Linux-kernel.  Aantal LIS-stuurprogramma's zijn standaard ingebouwd in kernel van de distributie. Oudere distributies die zijn gebaseerd op Red Hat Enterprise (RHEL) / CentOS zijn beschikbaar als een afzonderlijke download op [Linux Integration Services versie 4.2 voor Hyper-V en Azure](https://www.microsoft.com/download/details.aspx?id=55106). Zie [vereisten voor Linux-kernel](create-upload-generic.md#linux-kernel-requirements) voor meer informatie over de LIS-stuurprogramma's.

De Azure Linux Agent al vooraf is geïnstalleerd op de Azure Marketplace-installatiekopieën en is meestal beschikbaar vanuit de distributie van pakket-opslagplaats. Broncode is te vinden in [GitHub](https://github.com/azure/walinuxagent).


| Distributie | Version | Stuurprogramma's | Agent |
| --- | --- | --- | --- |
| CentOS |CentOS 6.3+, 7.0+ |CentOS 6.3: [LIS downloaden](https://www.microsoft.com/download/details.aspx?id=55106)<p>CentOS 6.4+: In kernel |Pakket: In [opslagplaats](http://olcentgbl.trafficmanager.net/openlogic/6/openlogic/x86_64/RPMS/) onder "WALinuxAgent" <br/>Broncode: [GitHub](https://github.com/Azure/WALinuxAgent) |
| [CoreOS](https://coreos.com/docs/running-coreos/cloud-providers/azure/) |494.4.0+ |In kernel |Broncode: [GitHub](https://github.com/coreos/coreos-overlay/tree/master/app-emulation/wa-linux-agent) |
| Debian |Debian 7,9 +, 8.2 + |In kernel |Pakket: In de opslagplaats onder 'waagent' <br/>Broncode: [GitHub](https://github.com/Azure/WALinuxAgent) |
| Oracle Linux |6.4+, 7.0+ |In kernel |Pakket: In de opslagplaats onder "WALinuxAgent" <br/>Broncode: [GitHub](https://go.microsoft.com/fwlink/p/?LinkID=250998) |
| Red Hat Enterprise Linux |RHEL 6.7+, 7.1+, 8.0+ |In kernel |Pakket: In de opslagplaats onder "WALinuxAgent" <br/>Broncode: [GitHub](https://github.com/Azure/WALinuxAgent) |
| SUSE Linux Enterprise |SLES/SLES voor SAP<br>11 SP4<br>12 SP1+<br>15|In kernel |Pakket:<p> voor 11 in [Cloud: extra](https://build.opensuse.org/project/show/Cloud:Tools) opslagplaats<br>voor 12 opgenomen in de Module 'Openbare Cloud' onder 'python-azure-agent'<br/>Broncode: [GitHub](https://go.microsoft.com/fwlink/p/?LinkID=250998) |
| openSUSE |openSUSE Leap 42.2 + |In kernel |Pakket: In [Cloud: extra](https://build.opensuse.org/project/show/Cloud:Tools) opslagplaats onder 'python-azure-agent' <br/>Broncode: [GitHub](https://github.com/Azure/WALinuxAgent) |
| Ubuntu |Ubuntu 12.04+ **<sup>1</sup>** |In kernel |Pakket: In de opslagplaats onder "walinuxagent" <br/>Broncode: [GitHub](https://github.com/Azure/WALinuxAgent) |

  - **<sup>1</sup>**  informatie over uitgebreide ondersteuning voor Ubuntu 12.04 en 14.04 vindt u hier: [Ubuntu uitgebreid beveiligingsbeheer](https://www.ubuntu.com/esm).


## <a name="image-update-cadence"></a>Installatiekopie-update uitgebracht
Azure vereist dat de uitgevers van de onderschreven Linux-distributies afbeeldingen in de Azure Marketplace regelmatig met de meest recente patches en beveiligingsproblemen, op een per kwartaal of sneller uitgebracht bijgewerkt. Bijgewerkte installatiekopieën in de Azure Marketplace zijn beschikbaar voor klanten automatisch als nieuwe versies van een installatiekopie-SKU. Meer informatie over het Linux-installatiekopieën zoeken: [Linux-VM-installatiekopieën zoeken in de Azure Marketplace](https://docs.microsoft.com/azure/virtual-machines/linux/cli-ps-findimage).

### <a name="additional-links"></a>Aanvullende koppelingen
 - [Levenscyclus van de installatiekopie SUSE openbare Cloud](https://www.suse.com/c/suse-public-cloud-image-life-cycle/)

## <a name="azure-tuned-kernels"></a>Afgestemd op de Azure kernels

Azure werkt nauw samen met verschillende onderschreven Linux-distributies het optimaliseren van de installatiekopieën die zij gepubliceerd naar de Azure Marketplace. Slechts één aspect van deze samenwerking is de ontwikkeling van 'op' Linux kernels die zijn geoptimaliseerd voor het Azure-platform en geleverd als volledig ondersteunde onderdelen van de Linux-distributie. De kernels afgestemd op de Azure opnemen van nieuwe functies en verbeterde prestaties en op een snellere (meestal kwartalen) uitgebracht in vergelijking met de standaard- of algemene kernels die beschikbaar in de distributie zijn.

In de meeste gevallen vindt u deze vooraf zijn geïnstalleerd op de standaardinstallatiekopieën in de Azure Marketplace kernels en dus Azure-klanten wordt onmiddellijk het voordeel van deze geoptimaliseerde kernels. Meer informatie over deze kernels zijn afgestemd op Azure kan worden gevonden in de volgende koppelingen:

 - CentOS afgestemd op de Azure Kernel - beschikbaar via de virtualisatie van CentOS SIG - [meer informatie](https://wiki.centos.org/SpecialInterestGroup/Virtualization)
 - Debian Cloud-Kernel - beschikbaar met de Debian 10 en de Debian 9 'backports' afbeelding op Azure - [meer Info](https://wiki.debian.org/Cloud/MicrosoftAzure)
 - SLES afgestemd op de Azure Kernel - [meer informatie](https://www.suse.com/c/a-different-builtin-kernel-for-azure-on-demand-images/)
 - Ubuntu afgestemd op de Azure Kernel - [meer informatie](https://blog.ubuntu.com/2017/09/21/microsoft-and-canonical-increase-velocity-with-azure-tailored-kernel)


## <a name="partners"></a>Partners

### <a name="coreos"></a>CoreOS
[https://coreos.com/docs/running-coreos/cloud-providers/azure/](https://coreos.com/docs/running-coreos/cloud-providers/azure/)

Vanaf de website van CoreOS:

*CoreOS is ontworpen voor beveiliging, consistentie en betrouwbaarheid. In plaats van de installatie van pakketten via yum of apt, CoreOS Linux-containers gebruikt voor het beheren van uw services op een hoger niveau van abstractie. Een enkele service code en alle afhankelijkheden zijn verpakt in een container die kan worden uitgevoerd op een of meer CoreOS-machines.*

### <a name="credativ"></a>credativ
[https://www.credativ.co.uk/credativ-blog/debian-images-microsoft-azure](https://www.credativ.co.uk/credativ-blog/debian-images-microsoft-azure)

Credativ is een onafhankelijke consulting services bedrijf en die gespecialiseerd in de ontwikkeling en implementatie van professionele oplossingen is met behulp van gratis software. Als toonaangevende open-source-specialisten heeft Credativ internationale opname met veel IT-afdelingen die gebruikmaken van de ondersteuning. In combinatie met Microsoft wordt Credativ momenteel voorbereid bijbehorende Debian installatiekopieën voor Debian 8 (Jessie) en Debian vóór 7 (Wheezy). Beide afbeeldingen zijn speciaal ontworpen om uit te voeren op Azure en eenvoudig kunnen worden beheerd via het platform. Credativ wordt ook ondersteuning voor de lange termijn onderhoud en het bijwerken van de Debian installatiekopieën voor Azure via de Open Source Support Centers.

### <a name="oracle"></a>Oracle
[https://www.oracle.com/technetwork/topics/cloud/faq-1963009.html](https://www.oracle.com/technetwork/topics/cloud/faq-1963009.html)

Van Oracle-strategie bestaat uit het bieden van een breed portfolio van oplossingen voor openbare en privéclouds. De strategie geeft klanten de keuze en flexibiliteit in hoe ze in Oracle clouds Oracle-software en andere clouds implementeren. De partnerschap van Oracle met Microsoft stelt klanten in staat Oracle-software in openbare en besloten clouds van Microsoft te implementeren in de wetenschap dat ze certificeringen en ondersteuning kunnen krijgen van Oracle.  De toezegging en investering in een Oracle-oplossingen voor openbare en privéclouds van Oracle is niet gewijzigd.

### <a name="red-hat"></a>Red Hat
[https://www.redhat.com/en/partners/strategic-alliance/microsoft](https://www.redhat.com/en/partners/strategic-alliance/microsoft)

Toonaangevende provider van de wereld van open source-oplossingen, Red Hat kan meer dan 90% van Fortune 500-bedrijven zakelijke uitdagingen, hun IT uitlijnen en strategieën voor bedrijven, en voor te bereiden voor de toekomst van technologie. Red Hat doet dit door op te geven beveiligde oplossingen via een open bedrijfsmodel en een betaalbare en voorspelbare abonnementsmodel.

### <a name="suse"></a>SUSE
[https://www.suse.com/suse-linux-enterprise-server-on-azure](https://www.suse.com/suse-linux-enterprise-server-on-azure)

SUSE Linux Enterprise Server op Azure is een bewezen platform dat biedt superieure betrouwbaarheid en beveiliging voor de cloud computing. Van SUSE veelzijdige Linux-platform wordt naadloos geïntegreerd met Azure cloudservices bieden een eenvoudige cloudomgeving. Met meer dan 9,200 gecertificeerde toepassingen van meer dan 1800 onafhankelijke softwareleveranciers voor SUSE Linux Enterprise Server, SUSE wordt ervoor gezorgd dat workloads die ondersteund worden uitgevoerd in het datacenter probleemloos kunnen worden geïmplementeerd op Azure.

### <a name="canonical"></a>Canonical
[https://www.ubuntu.com/cloud/azure](https://www.ubuntu.com/cloud/azure)

Canonieke engineering en open community governance station van Ubuntu succes in de client, server en cloud computing, waaronder persoonlijke cloudservices voor consumenten. Van canonical visie van een uniforme, gratis platform in Ubuntu, van telefoon naar de cloud, biedt een reeks samenhangend interfaces voor de telefoon, tablet, tv-programma's en desktop. Deze visie kunt u de eerste keuze voor de diverse instellingen van providers van openbare Clouds voor de makers van consumentenelektronica en een favoriet tussen afzonderlijke technologen Ubuntu.

Met ontwikkelaars en technische datacenters over de hele wereld, Canonical is in zijn soort als u wilt samenwerken met makers van hardware, inhoudsproviders en softwareontwikkelaars om oplossingen voor Ubuntu op de markt voor pc's, servers en mobiele apparaten.
