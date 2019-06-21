---
title: Azure bestanden schaalbaarheids- en prestatiedoelen | Microsoft Docs
description: Meer informatie over de schaalbaarheids- en prestatiedoelen voor Azure Files, met inbegrip van de capaciteit, snelheid van aanvragen en bandbreedtelimieten voor binnenkomende en uitgaande.
services: storage
author: roygara
ms.service: storage
ms.topic: article
ms.date: 5/5/2019
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 5e2f7b52ca3cb8a4d472b353e844bdad3aa1b0c1
ms.sourcegitcommit: 6e6813f8e5fa1f6f4661a640a49dc4c864f8a6cb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/17/2019
ms.locfileid: "67150453"
---
# <a name="azure-files-scalability-and-performance-targets"></a>Azure bestanden schaalbaarheids- en prestatiedoelen

[Azure Files](storage-files-introduction.md) biedt volledig beheerde bestandsshares in de cloud die toegankelijk zijn via het industriestandaard SMB-protocol. Dit artikel worden de schaalbaarheids- en prestatiedoelen voor Azure Files en Azure File Sync.

De schaalbaarheids- en prestatiedoelen die hier worden vermeld geavanceerde doelen zijn, maar ook worden beïnvloed door andere variabelen in uw implementatie. Bijvoorbeeld, kan de doorvoer voor een bestand ook worden beperkt door de beschikbare netwerkbandbreedte, niet alleen de servers die als host fungeert voor de Azure Files-service. Het is raadzaam testen van uw gebruikspatroon om te bepalen of de schaalbaarheid en prestaties van Azure Files voldoen aan uw vereisten. We zijn ook toegewezen aan het verhogen van deze limieten na verloop van tijd. Kunt u ons feedback, hetzij in de opmerkingen hieronder of op de [Azure bestanden UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files), over welke beperkingen u graag zou willen zien we verhogen.

## <a name="azure-storage-account-scale-targets"></a>Schaal prestatiedoelen voor Azure storage-account

De bovenliggende resource voor een Azure-bestandsshare is een Azure storage-account. Een storage-account vertegenwoordigt een opslaggroep in Azure die kan worden gebruikt door meerdere opslagservices, met inbegrip van Azure Files, voor het opslaan van gegevens. Andere services die gegevens opslaan in storage-accounts zijn Azure Blob storage, Azure Queue storage en Azure-tabelopslag. De volgende doelen gelden voor alle opslagservices opslaan van gegevens in een storage-account:

[!INCLUDE [azure-storage-limits](../../../includes/azure-storage-limits.md)]

[!INCLUDE [azure-storage-limits-azure-resource-manager](../../../includes/azure-storage-limits-azure-resource-manager.md)]

> [!Important]  
> Algemeen gebruik van account de opslag van andere opslagservices is van invloed op uw Azure-bestandsshares in uw opslagaccount. Bijvoorbeeld, als de maximale opslagaccountcapaciteit met Azure Blob-opslag is bereikt, kunt u zich niet kunnen maken van nieuwe bestanden op uw Azure-bestandsshare, zelfs als uw Azure-bestandsshare dan de grootte van de maximale bestandsshare is.

## <a name="azure-files-scale-targets"></a>Azure bestanden schaal doelen

### <a name="premium-scale-targets"></a>Premium schaal doelen

Er zijn drie soorten beperkingen van premium-bestandsshares: storage-accounts, bestandsshares en bestanden.

Bijvoorbeeld: Een afzonderlijke share kan maar liefst 100.000 IOPS en maximaal 5000 IOP's in een enkel bestand kan worden geschaald. Dus als u drie bestanden in één share hebt, is de maximale IOPS die u uit de share ophalen kunt bijvoorbeeld 15.000.

### <a name="premium-filestorage-account-limits"></a>Limieten voor Premium FileStorage-account

Premium-bestandsshares zijn ingericht in een speciale storage-account met de naam **filestorage (preview)** . Dit account heeft enigszins schaal doelen dan de storage-account gebruikt voor de standard-bestandsshares. Raadpleeg voor de prestatiedoelen van storage-account schalen aan de tabel in de [prestatiedoelen voor Azure storage-account schalen](#azure-storage-account-scale-targets) sectie.

> [!IMPORTANT]
> Storage-accountlimieten gelden voor alle shares. Schalen tot de maximale waarde voor FileStorage accounts is alleen haalbare als er slechts één share per FileStorage-account.

### <a name="file-share-and-file-scale-targets"></a>Bestandsshare en schaal bestandsdoelen

> [!NOTE]
> Standard-bestandsshares groter is dan 5 TiB zijn in preview en bepaalde beperkingen hebben.
> Zie voor een lijst met beperkingen en voor Onboarding van de Preview-versie van deze grotere bestanden delen, de [Standard bestandsshares](storage-files-planning.md#standard-file-shares) sectie van de handleiding voor capaciteitsplanning.

[!INCLUDE [storage-files-scale-targets](../../../includes/storage-files-scale-targets.md)]

[!INCLUDE [storage-files-premium-scale-targets](../../../includes/storage-files-premium-scale-targets.md)]

## <a name="azure-file-sync-scale-targets"></a>Azure File Sync schaal doelen

Azure File Sync is ontworpen met het doel van onbeperkt gebruik, maar onbeperkte gebruik is niet altijd mogelijk. De volgende tabel geeft aan dat de grenzen van het testen van Microsoft en geeft ook aan welke doelen vaste limieten zijn:

[!INCLUDE [storage-sync-files-scale-targets](../../../includes/storage-sync-files-scale-targets.md)]

### <a name="azure-file-sync-performance-metrics"></a>Azure File Sync-maatstaven voor prestaties

De effectieve synchronisatie prestaties omdat de Azure File Sync-agent wordt uitgevoerd op een Windows Server-computer die verbinding met de Azure-bestandsshares maakt, is afhankelijk van een aantal factoren in uw infrastructuur: Windows Server en de configuratie van onderliggende schijf, netwerkbandbreedte tussen de server en de Azure-opslag, het bestand grootte, de grootte van de totale gegevensset en de activiteit op de gegevensset. Omdat Azure File Sync werkt op bestandsniveau, wordt de prestatiekenmerken van een oplossing op basis van Azure File Sync beter gemeten in het aantal objecten (bestanden en mappen) dat per seconde wordt verwerkt.

Voor Azure File Sync is prestaties van essentieel belang is in twee fasen:

1. **Eerste eenmalige inrichting**: Raadpleeg voor optimale prestaties op de eerste inrichting [Onboarding met Azure File Sync](storage-sync-files-deployment-guide.md#onboarding-with-azure-file-sync) voor de details van de optimale implementatie.
2. **Doorlopende synchronisatie**: Nadat de gegevens in eerste instantie wordt gemaakt in de Azure-bestandsshares, houdt Azure File Sync meerdere eindpunten gesynchroniseerd.

Voor hulp bij het plannen van de implementatie voor elk van de fasen, worden hieronder de resultaten waargenomen tijdens het interne testen op een systeem met een configuratie

| Systeemconfiguratie |  |
|-|-|
| CPU | 64 virtuele kernen met 64 MiB L3-cache |
| Geheugen | 128 GiB |
| Schijf | SAS-schijven met RAID 10 met accu back cache |
| Netwerk | 1 Gbps-netwerk |
| Workload | Bestandsserver voor algemeen gebruik|

| Eerste eenmalige inrichting  |  |
|-|-|
| Aantal objecten | 25 miljoen objecten |
| Grootte van de gegevensset| ~4.7 TiB |
| Gemiddelde grootte | ~ 200 KiB (grootste bestand: 100 GiB) |
| Uploaden van doorvoer | 20 objecten per seconde |
| Namespace downloaden doorvoer * | 400 objecten per seconde |

\* Wanneer een nieuw servereindpunt wordt gemaakt, kan de Azure File Sync-agent een van de bestandsinhoud wordt niet gedownload. Het de volledige naamruimte voor het eerst synchroniseert en vervolgens triggers op de achtergrond intrekken om de bestanden in hun geheel te downloaden of, indien het cloud-opslaglagen is ingeschakeld, naar de cloud beleidsinstelling voor lagen ingesteld op het servereindpunt.

| Doorlopende synchronisatie  |   |
|-|--|
| Aantal objecten die zijn gesynchroniseerd| 125,000 objecten (~ 1% verloop) |
| Grootte van de gegevensset| 50 giB |
| Gemiddelde grootte | ~ 500 KiB |
| Uploaden van doorvoer | 30 objecten per seconde |
| Volledige Download doorvoer * | 60 objecten per seconde |

\* Als cloud tiering is ingeschakeld, kunt u waarschijnlijk betere prestaties als slechts een deel van het bestand gegevens gedownload in acht nemen. De gegevens van de bestanden in de cache downloadt met Azure File Sync alleen wanneer ze worden gewijzigd op een van de eindpunten. Voor gelaagde of de zojuist gemaakte bestanden, de agent niet de gegevens uit een bestand wordt gedownload en synchroniseert de naamruimte op alle servereindpunten in plaats daarvan alleen. De agent biedt ook ondersteuning voor gedeeltelijke downloads van gelaagde bestanden, zoals ze worden gebruikt door de gebruiker. 

> [!Note]  
> De bovenstaande getallen zijn niet een indicatie van de prestaties die u ondervindt. De werkelijke prestaties wordt afhankelijk zijn van meerdere factoren, zoals wordt beschreven in het begin van deze sectie.

Als algemene richtlijn voor uw implementatie, moet u een aantal dingen waarmee u rekening moet houden:

- De doorvoer van het object is ongeveer wordt geschaald naar gelang het aantal synchronisatiegroepen op de server. Gegevens splitsen in meerdere synchronisatiegroepen op een server resulteert in betere doorvoer, wat ook wordt beperkt door de server en het netwerk.
- De doorvoer van het object is omgekeerd evenredig aan de MiB per seconde doorvoer. Voor kleinere bestanden worden er hogere doorvoer in termen van het aantal objecten dat per seconde, maar lagere MiB per seconde doorvoer wordt verwerkt. Daarentegen voor grotere bestanden krijgt u minder objecten dat per seconde, maar hoger MiB per seconde doorvoer wordt verwerkt. De MiB per seconde doorvoer wordt beperkt door de doelen van de schaal Azure Files.

## <a name="see-also"></a>Zie ook

- [Implementatie van Azure Files plannen](storage-files-planning.md)
- [Planning voor de implementatie van een Azure File Sync](storage-sync-files-planning.md)
- [Schaalbaarheids- en prestatiedoelen voor andere storage-services](../common/storage-scalability-targets.md)
