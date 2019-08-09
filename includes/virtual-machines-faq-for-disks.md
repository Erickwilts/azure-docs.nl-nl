---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 05/13/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 21fe92bf4a33dc44545f1bd54c718db6c0a38532
ms.sourcegitcommit: 3073581d81253558f89ef560ffdf71db7e0b592b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68843182"
---
# <a name="frequently-asked-questions-about-azure-iaas-vm-disks-and-managed-and-unmanaged-premium-disks"></a>Veelgestelde vragen over Azure IaaS-VM-schijven en beheerde en onbeheerde Premium-schijven

In dit artikel vindt u antwoorden op enkele veelgestelde vragen over Azure Managed Disks en Azure Premium-SSD-schijven.

## <a name="managed-disks"></a>Beheerde schijven

**Wat is Azure Managed Disks?**

Managed Disks is een functie die schijf beheer voor Azure IaaS-Vm's vereenvoudigt door het beheer van opslag accounts voor u te verwerken. Zie [Managed disks-overzicht](../articles/virtual-machines/windows/managed-disks-overview.md)voor meer informatie.

**Als ik een standaard beheerde schijf Maak op basis van een bestaande VHD van 80 GB, hoeveel krijg ik dan de kosten?**

Een standaard beheerde schijf die is gemaakt op basis van een VHD van 80 GB, wordt beschouwd als de volgende beschik bare standaard schijf grootte, een S10-schijf. Er worden kosten in rekening gebracht op basis van de S10-schijf prijzen. Zie de pagina [prijzen](https://azure.microsoft.com/pricing/details/storage) voor meer informatie.

**Zijn er transactie kosten voor Standard Managed disks?**

Ja. Er worden kosten in rekening gebracht voor elke trans actie. Zie de pagina [prijzen](https://azure.microsoft.com/pricing/details/storage) voor meer informatie.

**Voor een Standard Managed disk worden er kosten in rekening gebracht voor de werkelijke grootte van de gegevens op de schijf of voor de ingerichte capaciteit van de schijf?**

Er worden kosten in rekening gebracht op basis van de ingerichte capaciteit van de schijf. Zie de pagina [prijzen](https://azure.microsoft.com/pricing/details/storage) voor meer informatie.

**Hoe worden de prijzen van Premium-beheerde schijven afwijkend van niet-beheerde schijven?**

De prijzen voor Premium Managed disks zijn hetzelfde als onbeheerde Premium-schijven.

**Kan ik het type opslag account (Standard of Premium) van mijn beheerde schijven wijzigen?**

Ja. U kunt het type opslag account van uw beheerde schijven wijzigen met behulp van de Azure Portal, Power shell of de Azure CLI.

**Kan ik een VHD-bestand in een Azure-opslag account gebruiken om een beheerde schijf te maken met een ander abonnement?**

Ja.

**Kan ik een VHD-bestand in een Azure-opslag account gebruiken om een beheerde schijf te maken in een andere regio?**

Nee.

**Zijn er schaal beperkingen voor klanten die gebruikmaken van beheerde schijven?**

Managed Disks elimineert de limieten die zijn gekoppeld aan opslag accounts. De maximum limiet is echter 50.000 Managed disks per regio en per schijf type voor een abonnement.

**Kan ik een incrementele moment opname van een beheerde schijf maken?**

Nee. Met de huidige mogelijkheid voor moment opnamen wordt een volledige kopie van een beheerde schijf gemaakt.

**Kunnen Vm's in een beschikbaarheidsset bestaan uit een combi natie van beheerde en onbeheerde schijven?**

Nee. De Vm's in een beschikbaarheidsset moeten alle beheerde schijven of alle niet-beheerde schijven gebruiken. Wanneer u een beschikbaarheidsset maakt, kunt u kiezen welk type schijven u wilt gebruiken.

**Is Managed Disks de standaard optie in de Azure Portal?**

Ja.

**Kan ik een lege beheerde schijf maken?**

Ja. U kunt een lege schijf maken. Een beheerde schijf kan onafhankelijk van een virtuele machine worden gemaakt, bijvoorbeeld zonder deze te koppelen aan een virtuele machine.

**Wat is het ondersteunde aantal fout domeinen voor een beschikbaarheidsset die gebruikmaakt van Managed Disks?**

Afhankelijk van de regio waar de beschikbaarheidsset met Managed Disks zich bevindt, is het aantal ondersteunde fout domeinen 2 of 3.

**Hoe wordt het standaard opslag account voor diagnostische gegevens ingesteld?**

U stelt een privé opslag account in voor de diagnostische gegevens van de virtuele machine.

**Welk soort op rollen gebaseerd Access Control ondersteuning is beschikbaar voor Managed Disks?**

Managed Disks ondersteunt drie belangrijkste standaard rollen:

* Bent Kan alles beheren, inclusief toegang
* Groep Kan alles behalve toegang beheren
* Gelezen Kan alles weer geven, maar kan geen wijzigingen aanbrengen

**Is er een manier waarop ik een beheerde schijf naar een privé opslag account kan kopiëren of exporteren?**

U kunt een alleen-lezen SAS-URI (Shared Access Signature) voor de beheerde schijf genereren en deze gebruiken om de inhoud te kopiëren naar een privé opslag account of on-premises opslag. U kunt de SAS-URI gebruiken met behulp van de Azure Portal, Azure PowerShell, de Azure CLI of [AzCopy](../articles/storage/common/storage-use-azcopy.md)

**Kan ik een kopie van mijn beheerde schijf maken?**

Klanten kunnen een moment opname van hun beheerde schijven nemen en vervolgens de moment opname gebruiken om een andere beheerde schijf te maken.

**Worden niet-beheerde schijven nog steeds ondersteund?**

Ja, zowel onbeheerde als beheerde schijven worden ondersteund. We raden u aan Managed disks te gebruiken voor nieuwe workloads en uw huidige workloads te migreren naar Managed disks.

**Kan ik onbeheerde en beheerde schijven op dezelfde virtuele machine gezamenlijk vinden?**

Nee.

**Als ik een schijf van 128 GB Maak en vervolgens de grootte tot 130 gibibytes (GiB) verg root, worden er dan kosten in rekening gebracht voor de volgende schijf grootte (256 GiB)?**

Ja.

**Kan ik lokaal redundante opslag, geografisch redundante opslag, en zone-redundante opslag beheerde schijven maken?**

Azure Managed Disks ondersteunt momenteel alleen op lokaal redundante opslag beheerde schijven.

**Kan ik mijn beheerde schijven verkleinen of krimpen?**

Nee. Deze functie wordt momenteel niet ondersteund.

**Kan ik een lease op mijn schijf opdelen?**

Nee. Dit wordt momenteel niet ondersteund omdat er geen lease aanwezig is om onbedoelde verwijdering te voor komen wanneer de schijf wordt gebruikt.

**Kan ik de eigenschap computer naam wijzigen wanneer een specialist (niet gemaakt met het hulp programma voor systeem voorbereiding of het gegeneraliseerde besturings systeem) wordt gebruikt om een virtuele machine in te richten?**

Nee. U kunt de eigenschap computer naam niet bijwerken. De nieuwe virtuele machine neemt deze over van de bovenliggende VM, die werd gebruikt voor het maken van de besturingssysteem schijf. 

**Waar vind ik voor beelden van Azure Resource Manager sjablonen voor het maken van Vm's met Managed disks?**
* [Lijst met sjablonen met behulp van Managed Disks](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md)
* https://github.com/chagarw/MDPP

**Is er bij het maken van een schijf van een BLOB een voortdurend bestaande relatie met die bron-BLOB?**

Nee, wanneer de nieuwe schijf wordt gemaakt, is het een volledige zelfstandige kopie van die Blob op dat moment en is er geen verbinding tussen de twee. Als u wilt, nadat u de schijf hebt gemaakt, kan de bron-BLOB worden verwijderd zonder dat dit van invloed is op de zojuist gemaakte schijf.

**Kan ik de naam van een beheerde of onbeheerde schijf wijzigen nadat deze is gemaakt?**

Voor beheerde schijven kunt u de naam niet wijzigen. U kunt de naam van een niet-beheerde schijf echter wijzigen als deze momenteel niet is gekoppeld aan een VHD of virtuele machine.

**Kan ik GPT-partitionering op een Azure-schijf gebruiken?**

GPT-partitionering kan alleen worden gebruikt op gegevens schijven, niet op besturingssysteem schijven. Voor besturingssysteem schijven moet de partitie stijl MBR worden gebruikt.

**Welke schijf typen ondersteunen moment opnamen?**

Moment opnamen van Premium-SSD, Standard SSD en Standard HDD-ondersteuning. Voor deze drie schijf typen worden moment opnamen ondersteund voor alle schijf grootten (inclusief schijven tot 32 TiB groot). Ultra Ssd's bieden geen ondersteuning voor moment opnamen.

## <a name="standard-ssd-disks"></a>Standard-SSD schijven

**Wat zijn Azure Standard-SSD-schijven?**
Standard-SSD schijven zijn standaard schijven die worden ondersteund door Solid-State media, geoptimaliseerd als rendabele opslag voor werk belastingen die consistente prestaties nodig hebben op lagere IOPS-niveaus.

<a id="standard-ssds-azure-regions"></a>**Wat zijn de regio's die momenteel worden ondersteund voor Standard-SSD schijven?**
Alle Azure-regio's ondersteunen nu Standard-SSD-schijven.

**Is Azure Backup beschikbaar als standaard-Ssd's wordt gebruikt?**
Ja, Azure Backup is nu beschikbaar.

**Standard-SSD schijven Hoe kan ik maken?**
U kunt Standard-SSD schijven maken met behulp van Azure Resource Manager sjablonen, SDK, Power shell of CLI. Hieronder vindt u de para meters die nodig zijn in het Resource Manager-sjabloon om Standard-SSD schijven te maken:

* *apiVersion* voor micro soft. Compute moet worden `2018-04-01` ingesteld als (of hoger)
* Geef *managedDisk. storageAccountType* op als`StandardSSD_LRS`

In het volgende voor beeld ziet u de sectie *Properties. storageProfile. osDisk* voor een virtuele machine die gebruikmaakt van Standard-SSD schijven:

```json
"osDisk": {
    "osType": "Windows",
    "name": "myOsDisk",
    "caching": "ReadWrite",
    "createOption": "FromImage",
    "managedDisk": {
        "storageAccountType": "StandardSSD_LRS"
    }
}
```

Zie [een virtuele machine maken op basis van een Windows-installatie kopie met Standard-SSD-gegevens schijven](https://github.com/azure/azure-quickstart-templates/tree/master/101-vm-with-standardssd-disk/)voor een volledig sjabloon voor beeld van het maken van een Standard-SSD schijf met een sjabloon.

**Kan ik mijn bestaande schijven converteren naar Standard-SSD?**
Ja, dat is mogelijk. Raadpleeg de [opslag voor Azure Managed disks converteren van Standard naar Premium en](https://docs.microsoft.com/azure/virtual-machines/windows/convert-disk-storage) omgekeerd voor de algemene richt lijnen voor het converteren van Managed disks. En gebruik de volgende waarde om het schijf type bij te werken naar Standard-SSD.
-AccountType StandardSSD_LRS

**Wat is het voor deel van het gebruik van Standard-SSD schijven in plaats van HDD?**
Standard-SSD schijven bieden betere latentie, consistentie, Beschik baarheid en betrouw baarheid in vergelijking met HDD-schijven. Werk belastingen van toepassingen worden veel soepeler uitgevoerd op Standard-SSD vanwege dat. Houd er rekening mee dat Premium-SSD schijven de aanbevolen oplossing zijn voor de meeste IO-intensieve productie workloads.

**Kan ik standaard Ssd's als niet-beheerde schijven gebruiken?**
Nee, standaard Ssd's-schijven zijn alleen beschikbaar als Managed Disks.

**Ondersteunen Standard-SSD schijven ' single instance VM SLA '?**
Nee, standaard Ssd's hebben geen SLA voor de VM van één exemplaar. Gebruik Premium-SSD schijven voor de SLA van de VM met één exemplaar.

## <a name="migrate-to-managed-disks"></a>Migreren naar beheerde schijven

**Is er sprake van de migratie van de Managed Disks prestaties?**

Migratie omvat het verplaatsen van de schijf van de ene opslag locatie naar de andere. Dit wordt ingedeeld via de achtergrond kopie van gegevens. Dit kan enkele uren duren, meestal minder dan 24 uur, afhankelijk van de hoeveelheid gegevens op de schijven. Gedurende die tijd kan uw toepassing hoger zijn dan de gebruikelijke lees latentie, omdat sommige Lees bewerkingen worden omgeleid naar de oorspronkelijke locatie. Dit kan langer duren. Er is geen invloed op de schrijf latentie tijdens deze periode.  

**Welke wijzigingen zijn vereist in een vooraf bestaande Azure Backup Service configuratie vóór/na de migratie naar Managed Disks?**

Er zijn geen wijzigingen vereist.

**Worden mijn VM-back-ups die zijn gemaakt via Azure Backup-service voordat de migratie blijft werken?**

Ja, back-ups werken probleemloos.

**Welke wijzigingen zijn er vereist in een reeds bestaande Azure Disk Encryption-configuratie vóór/na de migratie naar Managed Disks?**

Er zijn geen wijzigingen vereist.

**Is geautomatiseerde migratie van een bestaande virtuele-machine schaalset van niet-beheerde schijven naar Managed Disks ondersteund?**

Nee. U kunt een nieuwe schaalset maken met Managed Disks met behulp van de installatie kopie uit uw oude schaalset met niet-beheerde schijven.

**Kan ik een beheerde schijf maken op basis van een moment opname van de pagina-blob die is gemaakt vóór de migratie naar Managed Disks?**

Nee. U kunt een pagina-BLOB-moment opname exporteren als een pagina-Blob en vervolgens een beheerde schijf maken op basis van de geëxporteerde pagina-blob.

**Kan ik mijn on-premises machines die worden beveiligd door Azure Site Recovery naar een virtuele machine met Managed Disks door lopen?**

Ja, u kunt een failover naar een virtuele machine met Managed Disks.

**Zijn er gevolgen voor de migratie op Azure-Vm's die zijn beveiligd door Azure Site Recovery via Azure naar Azure-replicatie?**

Nee. Azure Site Recovery Azure naar Azure-beveiliging voor Vm's met Managed Disks is beschikbaar.

**Kan ik Vm's migreren met onbeheerde schijven die zich bevinden op opslag accounts die al of niet eerder zijn versleuteld naar Managed disks?**

Ja

## <a name="managed-disks-and-storage-service-encryption"></a>Managed Disks en Storage Service Encryption

**Is Azure Storage service versleuteling standaard ingeschakeld bij het maken van een beheerde schijf?**

Ja.

**Wie beheert de versleutelings sleutels?**

Micro soft beheert de versleutelings sleutels.

**Kan ik Storage Service Encryption voor mijn beheerde schijven uitschakelen?**

Nee.

**Is Storage Service Encryption alleen beschikbaar in bepaalde regio's?**

Nee. Het is beschikbaar in alle regio's waar Managed Disks beschikbaar zijn. Managed Disks is beschikbaar in alle open bare regio's en Duitsland. Het is ook alleen beschikbaar in China, maar alleen voor door micro soft beheerde sleutels, niet door de klant beheerde sleutels.

**Hoe kan ik erachter komen of mijn beheerde schijf is versleuteld?**

U kunt de tijd nagaan waarop een beheerde schijf is gemaakt op basis van de Azure Portal, de Azure CLI en Power shell. Als de tijd na 9 juni 2017 is, wordt uw schijf versleuteld.

**Hoe kan ik mijn bestaande schijven die zijn gemaakt vóór 10 juni 2017 versleuteld?**

Vanaf 10 juni 2017 worden nieuwe gegevens die naar bestaande beheerde schijven worden geschreven, automatisch versleuteld. We zijn ook van plan om bestaande gegevens te versleutelen en de versleuteling wordt asynchroon op de achtergrond uitgevoerd. Als u nu bestaande gegevens moet versleutelen, maakt u een kopie van uw schijf. Nieuwe schijven worden versleuteld.

* [Beheerde schijven kopiëren met behulp van de Azure CLI](../articles/virtual-machines/scripts/virtual-machines-linux-cli-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fcli%2fmodule%2ftoc.json)
* [Beheerde schijven kopiëren met behulp van Power shell](../articles/virtual-machines/scripts/virtual-machines-windows-powershell-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fcli%2fmodule%2ftoc.json)

**Worden beheerde moment opnamen en installatie kopieën versleuteld?**

Ja. Alle beheerde moment opnamen en installatie kopieën die zijn gemaakt na 9 juni 2017, worden automatisch versleuteld. 

**Kan ik Vm's converteren met onbeheerde schijven die zich bevinden op de opslag accounts die eerder zijn versleuteld op beheerde schijven?**

Ja

**Wordt een geëxporteerde VHD van een beheerde schijf of een moment opname ook versleuteld?**

Nee. Maar als u een VHD exporteert naar een versleuteld opslag account van een versleutelde, beheerde schijf of moment opname, wordt deze versleuteld. 

## <a name="premium-disks-managed-and-unmanaged"></a>Premium-schijven: Beheerd en onbeheerd

**Als een virtuele machine gebruikmaakt van een grootte reeks die Premium-SSD schijven ondersteunt, zoals een DSv2, kan ik zowel Premium-als standaard gegevens schijven koppelen?** 

Ja.

**Kan ik zowel Premium-als standaard gegevens schijven koppelen aan een grootte reeks die geen ondersteuning biedt voor Premium-SSD schijven, zoals D, dv2, G of F Series?**

Nee. U kunt alleen standaard gegevens schijven koppelen aan virtuele machines die geen grootte reeksen gebruiken die Premium-SSD schijven ondersteunt.

**Als ik een Premium-gegevens schijf Maak van een bestaande VHD die 80 GB was, hoeveel kost dat dan ook?**

Een Premium-gegevens schijf die is gemaakt op basis van een VHD van 80 GB, wordt beschouwd als de volgende beschik bare Premium-schijf grootte, een P10-schijf. Er worden kosten in rekening gebracht op basis van de P10-schijf prijzen.

**Zijn er transactie kosten om Premium-SSD schijven te gebruiken?**

Er zijn vaste kosten voor elke schijf grootte, die wordt geleverd met specifieke limieten voor IOPS en door voer. De overige kosten zijn uitgaande band breedte en moment opname capaciteit, indien van toepassing. Zie de pagina [prijzen](https://azure.microsoft.com/pricing/details/storage) voor meer informatie.

**Wat zijn de limieten voor IOPS en door Voer die ik uit de schijf cache kan halen?**

De gecombineerde limieten voor cache en lokale SSD voor een DS-serie zijn 4.000 IOPS per kern en 33 MiB per seconde per kern. De GS-serie biedt 5.000 IOPS per kern en 50 MiB per seconde per kern.

**Wordt de lokale SSD ondersteund voor een Managed Disks virtuele machine?**

De lokale SSD is tijdelijke opslag die deel uitmaakt van een Managed Disks VM. Er zijn geen extra kosten voor deze tijdelijke opslag. U wordt aangeraden deze lokale SSD niet te gebruiken om uw toepassings gegevens op te slaan, omdat deze niet is opgeslagen in Azure Blob Storage.

**Zijn er gevolgen voor het gebruik van bijsnijden op Premium-schijven?**

Er is geen nadeel van het gebruik van knippen op Azure-schijven op Premium-of Standard-schijven.

## <a name="new-disk-sizes-managed-and-unmanaged"></a>Nieuwe schijf grootten: Beheerd en onbeheerd

**Wat is de grootste beheerde schijf grootte die wordt ondersteund voor besturings systeem en gegevens schijven?**

Het partitie type dat door Azure wordt ondersteund voor een besturingssysteem schijf is de Master Boot Record (MBR). De MBR-indeling ondersteunt een schijf grootte van Maxi maal twee TiB. De maximale grootte die Azure ondersteunt voor een besturingssysteem schijf is 2 TiB. Azure biedt ondersteuning voor Maxi maal 32 TiB voor beheerde gegevens schijven in wereld wijd Azure, 4 TiB in azure soevereine Clouds.

**Wat is de grootste niet-beheerde schijf grootte die wordt ondersteund voor besturings systeem en gegevens schijven?**

Het partitie type dat door Azure wordt ondersteund voor een besturingssysteem schijf is de Master Boot Record (MBR). De MBR-indeling ondersteunt een schijf grootte van Maxi maal twee TiB. De maximale grootte die door Azure wordt ondersteund voor een onbeheerde schijf van het besturings systeem is 2 TiB. Azure biedt ondersteuning voor Maxi maal 4 TiB voor gegevens niet-beheerde schijven.

**Wat is de grootste grootte van de pagina-blob die wordt ondersteund?**

De grootste grootte van de pagina-blob die door Azure wordt ondersteund, is 8 TiB (8.191 GiB). De maximale grootte van de pagina-BLOB wanneer deze is gekoppeld aan een virtuele machine als de schijven van het gegevens-of het besturings systeem 4 TiB (4.095 GiB).

**Moet ik een nieuwe versie van de Azure-hulpprogram ma's gebruiken om schijven te maken, te koppelen, te verg Roten of te verkleinen en te uploaden die groter zijn dan 1 TiB?**

U hoeft de bestaande Azure-hulpprogram ma's niet bij te werken om schijven groter dan 1 TiB te maken, te koppelen of te wijzigen. Als u uw VHD-bestand rechtstreeks van on-premises naar Azure wilt uploaden als pagina-BLOB of een niet-beheerde schijf, moet u de meest recente programma sets gebruiken die hieronder worden weer gegeven. Er worden alleen VHD-uploads ondersteund van Maxi maal 8 TiB.

|Azure-hulpprogramma 's      | Ondersteunde versies                                |
|-----------------|---------------------------------------------------|
|Azure PowerShell | Versie nummer 4.1.0: Release van juni 2017 of hoger|
|Azure CLI v1     | Versie nummer 0.10.13: Mogelijk 2017 release of hoger|
|Azure CLI v2     | Versie nummer 2.0.12: Release van juli 2017 of hoger|
|AzCopy           | Versie nummer 6.1.0: Release van juni 2017 of hoger|

**Worden de schijf grootten van P4 en P6 ondersteund voor onbeheerde schijven of pagina-blobs?**

P4-schijf grootten (32 GiB) en P6 (64 GiB) worden niet ondersteund als de standaard schijf lagen voor onbeheerde schijven en pagina-blobs. U moet [de BLOB-laag expliciet instellen](https://docs.microsoft.com/rest/api/storageservices/set-blob-tier) op P4 en P6 om ervoor te hebben dat uw schijf aan deze lagen wordt toegewezen. Als u een niet-beheerde schijf of pagina-BLOB implementeert met de schijf grootte of de lengte van de inhoud kleiner dan 32 GiB of tussen 32 GiB tot 64 GiB zonder de BLOB-laag in te stellen, blijft u op P10 met 500 IOPS en 100 MiB/s en de toegewezen prijs categorie.

**Hoe worden er kosten in rekening gebracht als mijn bestaande Premium Managed Disk lager dan 64 GiB is gemaakt voordat de kleine schijf werd ingeschakeld (ongeveer 15 juni 2017)?**

Bestaande kleine Premium-schijven van minder dan 64 GiB worden nog steeds gefactureerd volgens de prijs categorie P10.

**Hoe kan ik de schijf laag van kleine Premium-schijven kleiner dan 64 GiB van P10 naar P4 of P6 wijzigen?**

U kunt een moment opname van uw kleine schijven nemen en vervolgens een schijf maken om de prijs categorie automatisch over te scha kelen naar P4 of P6 op basis van de ingerichte grootte.

**Kunt u het formaat van bestaande Managed Disks wijzigen van de grootte van minder dan 4 tebibytes (TiB) tot de nieuwe nieuw geïntroduceerde schijf grootten tot 32 TiB?**

Ja.

**Wat zijn de grootste schijf grootten die worden ondersteund door Azure Backup en Azure Site Recovery service?**

De grootste schijf grootte die wordt ondersteund door Azure Backup en Azure Site Recovery service is 4 TiB. Ondersteuning voor de grotere schijven tot 32 TiB is nog niet beschikbaar.

**Wat zijn de aanbevolen VM-grootten voor grotere schijf grootten (> 4 TiB) voor Standard-SSD en Standard-HDD schijven voor een geoptimaliseerde schijf-IOPS en band breedte?**

We raden u aan een nieuwe VM te implementeren vanuit een van de volgende VM-grootten om de prestaties van Standard-SSD en Standard-HDD groot aantal schijven (> 4 TiB 60 500) te verzorgen. De Vm's uit de B-serie, de DSv2-serie, de Dsv3-serie, de ESv3-serie, de FS-serie, de Fsv2-serie, de M-serie, de NCv2-serie, de NCv3-serie of de LS-serie. Als u grote schijven koppelt aan bestaande Vm's of Vm's die niet gebruikmaken van de aanbevolen groottes, kan dit de prestaties verlagen.

**Hoe kan ik een upgrade uitvoeren van mijn schijven (> 4 TiB) die zijn geïmplementeerd tijdens de preview-versie van grotere schijven om de meer IOPS & band breedte te verkrijgen bij GA?**

U kunt de virtuele machine stoppen en starten waarop de schijf is aangesloten, of de schijf loskoppelen en opnieuw koppelen. De prestatie doelen van grotere schijven zijn verhoogd voor zowel Premium-Ssd's als standaard Ssd's bij GA.

**Welke regio's zijn de beheerde schijf grootten van 8 TiB, 16 TiB en 32 TiB ondersteund in?**

De 8 TiB-, 16 TiB-en 32 TiB-schijf-Sku's worden ondersteund in alle regio's onder wereld wijd Azure, Microsoft Azure Government en Azure China 21Vianet.

**Bieden we ondersteuning voor het inschakelen van host-caching op alle schijf grootten?**

Het opslaan van alleen-lezen-en lees-en schrijf bewerkingen in de host wordt ondersteund op schijven met minder dan 4 TiB. Voor schijf grootten die meer dan 4 TiB zijn, bieden we geen ondersteuning voor het instellen van de cache optie anders dan geen. We raden u aan cache gebruik te maken van kleinere schijf grootten, waarbij u de prestaties beter kunt zien met gegevens in de cache van de virtuele machine.

## <a name="what-if-my-question-isnt-answered-here"></a>Wat gebeurt er als mijn vraag hier niet wordt beantwoord?

Als uw vraag hier niet wordt vermeld, laat het ons weten en wij helpen u een antwoord te vinden. U kunt aan het einde van dit artikel een vraag plaatsen in de opmerkingen. Gebruik het MSDN [Azure Storage-forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata)om contact op te nemen met het Azure Storage team en andere leden van de community over dit artikel.

Als u functies wilt aanvragen, moet u uw aanvragen en ideeën verzenden naar het [Feedback forum van Azure Storage](https://feedback.azure.com/forums/217298-storage).
