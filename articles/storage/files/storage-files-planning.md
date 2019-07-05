---
title: Planning voor de implementatie van Azure Files | Microsoft Docs
description: Meer informatie over wat u moet overwegen bij het plannen van een implementatie van Azure Files.
services: storage
author: roygara
ms.service: storage
ms.topic: article
ms.date: 04/25/2019
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 6506a93914cfbc10f37980c4b916a93aa9aad75d
ms.sourcegitcommit: d2785f020e134c3680ca1c8500aa2c0211aa1e24
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/04/2019
ms.locfileid: "67564408"
---
# <a name="planning-for-an-azure-files-deployment"></a>Planning voor de implementatie van Azure Files

[Azure Files](storage-files-introduction.md) biedt volledig beheerde bestandsshares in de cloud die toegankelijk zijn via het industriestandaard SMB-protocol. Omdat Azure Files wordt volledig beheerd, is het veel eenvoudiger dan implementeren en beheren van een bestandsserver of een NAS-apparaat dat u deze in productie scenario's is geïmplementeerd. In dit artikel komen de onderwerpen om u te overwegen bij het implementeren van een Azure-bestandsshare voor gebruik in productieomgevingen binnen uw organisatie.

## <a name="management-concepts"></a>Concepten van Management

 Het volgende diagram illustreert de concepten van Azure Files management:

![Bestandsstructuur](./media/storage-files-introduction/files-concepts.png)

* **Storage-Account**: Alle toegang tot Azure Storage vindt plaats via een opslagaccount. Zie [Azure Storage Scalability and Performance Targets](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) (Schaalbaarheids- en prestatiedoelen in Azure Storage) voor meer informatie over opslagaccountcapaciteit.

* **Share**: Een File Storage-share is een SMB-bestandsshare in Azure. Alle mappen en bestanden moeten worden gemaakt in een bovenliggende share. Een account kan een onbeperkt aantal shares bevatten en een share kan een onbeperkt aantal bestanden, tot de 5 TiB totale capaciteit van de bestandsshare.

* **Directory**: Een optionele hiërarchie van mappen.

* **Bestand**: Een bestand in de share. Een bestand mag maximaal 1 TiB in grootte.

* **URL-indeling**: Voor aanvragen voor een Azure-bestandsshare gemaakt met de File REST-protocol, bestanden kunnen worden opgevraagd met behulp van de volgende URL-indeling:

    ```
    https://<storage account>.file.core.windows.net/<share>/<directory>/<file>
    ```

## <a name="data-access-method"></a>Data access-methode

Azure Files beschikt twee ingebouwde, handige data access-methoden die u afzonderlijk of in combinatie met elkaar worden verbonden, gebruiken kunt voor toegang tot uw gegevens:

1. **Direct cloudtoegang**: Een Azure-bestandsshare kan worden gekoppeld door [Windows](storage-how-to-use-files-windows.md), [macOS](storage-how-to-use-files-mac.md), en/of [Linux](storage-how-to-use-files-linux.md) met de Server Message Block (SMB) standaardprotocol of via de File REST-API. Lees- en schrijfbewerkingen op bestanden op de share met SMB, zijn er rechtstreeks op de bestandsshare in Azure. Voor het koppelen van een virtuele machine in Azure, de SMB-client in het besturingssysteem moet ondersteuning van ten minste SMB 2.1. Koppeling maken met on-premises, zoals op een Gebruikerswerkstation de SMB-client wordt ondersteund door het werkstation moet ondersteuning van ten minste SMB 3.0 (met versleuteling). Naast SMB, nieuwe toepassingen of services kunnen rechtstreeks toegang tot de bestandsshare via de REST-bestand, waarmee u een eenvoudige en schaalbare application programming interface voor het ontwikkelen van software.
2. **Azure File Sync**: Met Azure File Sync kunnen bestandsshares worden gerepliceerd naar Windows-Servers on-premises of in Azure. Uw gebruikers zou zoals toegang tot de bestandsshare via de Windows-Server, dat via een SMB- of NFS-share. Dit is handig voor scenario's waarin gegevens worden geopend en gewijzigd ver weg van een Azure-datacenter, zoals in een scenario voor het filiaal. Gegevens kunnen worden gerepliceerd tussen meerdere eindpunten voor Windows Server, zoals tussen meerdere filialen. Ten slotte kan worden gelaagde gegevens in Azure Files, zodat alle gegevens zijn nog steeds toegankelijk via de Server, maar de Server heeft geen een volledige kopie van de gegevens. In plaats daarvan gegevens naadloos ingetrokken wanneer door de gebruiker wordt geopend.

De volgende tabel ziet u hoe uw gebruikers en toepassingen toegang uw Azure-bestandsshare tot hebben:

| | Direct cloudtoegang | Azure File Sync |
|------------------------|------------|-----------------|
| Welke protocollen moet u gebruiken? | Azure Files biedt ondersteuning voor SMB 2.1, SMB 3.0 en File REST-API. | Toegang tot uw Azure-bestandsshare via een ondersteund protocol op Windows Server (SMB, NFS, FTPS, enz.) |  
| Waar worden u uw workload uitgevoerd? | **In Azure**: Azure Files biedt directe toegang tot uw gegevens. | **On-premises met een traag netwerk**: Windows, Linux en Mac OS-clients kunnen een lokale on-premises Windows-bestandsshare koppelen als een snelle cache van uw Azure-bestandsshare. |
| Welk niveau van ACL's hebt u nodig? | Het niveau van delen en de bestandsnaam. | Het niveau van delen, bestands- en gebruiker. |

## <a name="data-security"></a>Gegevensbeveiliging

Azure Files biedt verschillende ingebouwde opties voor het garanderen van de beveiliging van gegevens:

* Ondersteuning voor codering in beide over-the-wire-protocollen: SMB 3.0-versleuteling en de REST-bestand via HTTPS. Standaard: 
    * Clients die ondersteuning bieden voor SMB 3.0-codering verzenden en ontvangen van gegevens via een versleuteld kanaal.
    * Clients die geen ondersteuning voor SMB 3.0 met-codering kunnen intra-datacenter communiceren via het SMB 2.1 of SMB 3.0 zonder versleuteling. SMB-clients zijn niet toegestaan om te communiceren tussen datacenter via SMB 2.1 of SMB 3.0 zonder versleuteling.
    * Clients kunnen communiceren via de REST-bestand met HTTP of HTTPS.
* Versleuteling at-rest ([Azure Storage-Serviceversleuteling](../common/storage-service-encryption.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)): Storage Service Encryption (SSE) is ingeschakeld voor alle opslagaccounts. Gegevens in rust worden versleuteld met een volledig beheerde sleutels. Versleuteling in rust niet verhogen van de kosten voor opslag of de prestaties negatief beïnvloeden. 
* Optionele vereiste van versleutelde gegevens die worden verzonden: als u selecteert, Azure Files toegang tot de gegevens via niet-versleutelde kanalen geweigerd. Specifiek, zijn alleen HTTPS en SMB 3.0 met versleuteling verbindingen toegestaan.

    > [!Important]  
    > Veilige overdracht van gegevens vereisen zorgt ervoor dat oudere SMB-clients niet geschikt voor communicatie met SMB 3.0 met-codering mislukken. Zie voor meer informatie, [koppelen in Windows](storage-how-to-use-files-windows.md), [koppelen in Linux](storage-how-to-use-files-linux.md), en [koppelen in macOS](storage-how-to-use-files-mac.md).

Voor een maximale beveiliging wordt aangeraden altijd inschakelen beide versleuteling in rust en inschakelen van versleuteling van gegevens die worden verzonden wanneer u van moderne clients gebruikmaakt toegang tot uw gegevens. Als u nodig hebt om te koppelen van een share op een Windows Server 2008 R2-VM, die alleen ondersteuning biedt voor SMB 2.1, moet u bijvoorbeeld niet-versleuteld verkeer naar uw storage-account toestaan omdat SMB 2.1 biedt geen ondersteuning voor versleuteling.

Als u Azure File Sync gebruikt voor toegang tot uw Azure-bestandsshare, zullen we de HTTPS en SMB 3.0 met versleuteling altijd gebruiken om te synchroniseren van uw gegevens met uw Windows-Servers, ongeacht of u versleuteling van inactieve gegevens nodig hebt.

## <a name="file-share-performance-tiers"></a>File share prestatielagen

Azure Files biedt twee prestatielagen: standard en premium.

### <a name="standard-file-shares"></a>Standard-bestandsshares

Standard-bestandsshares worden ondersteund door harde schijven (HDD's). Standard-bestandsshares bieden, betrouwbare prestaties voor i/o-werkbelastingen die minder gevoelig zijn voor variaties in prestaties, zoals voor algemeen gebruik-bestandsshares en dev/test-omgevingen. Standard-bestandsshares zijn alleen beschikbaar in een betalen per gebruik factureringsmodel.

Standard-bestandsshares maximaal 5 TiB in grootte zijn beschikbaar als een aanbieding voor algemene beschikbaarheid. Grotere bestandsshares die groter zijn dan 5 TiB, tot een maximum van 100 TiB aandelen zijn momenteel beschikbaar als een preview-aanbieding.

> [!IMPORTANT]
> Zie de [Onboard tot grotere bestandsshares (standard-laag)](#onboard-to-larger-file-shares-standard-tier) sectie voor stappen om te introduceren, evenals het bereik en de beperkingen van de Preview-versie.

### <a name="premium-file-shares"></a>Premium-bestandsshares

Premium-bestandsshares worden ondersteund door SSD-schijven (SSD's). Premium-bestandsshares bieden consistente hoge prestaties en lage latentie en binnen enkele milliseconden voor de meeste i/o-bewerkingen voor i/o-intensieve workloads. Hierdoor kunt u ze geschikt zijn voor een groot aantal workloads, zoals databases, website hosting en ontwikkelomgevingen. Premium-bestandsshares zijn alleen beschikbaar in een ingerichte factureringsmodel. Premium-bestandsshares gebruiken een implementatiemodel gescheiden van de standard-bestandsshares.

Azure Backup is beschikbaar voor premium-bestandsshares en Azure Kubernetes Service biedt ondersteuning voor premium-bestandsshares in versie 1.13 en hoger.

Als u wilt meer informatie over het maken van een premium-bestandsshare, raadpleegt u ons artikel over dit onderwerp: [Over het maken van een premium Azure file storage-account](storage-how-to-create-premium-fileshare.md).

U kunt geen op dit moment rechtstreeks converteren tussen een standard-bestandsshare en een premium-bestandsshare. Als u wilt overschakelen naar een laag, moet u een nieuwe bestandsshare maken in die laag en de gegevens van uw oorspronkelijke share handmatig kopiëren naar de nieuwe share die u hebt gemaakt. U kunt dit doen met behulp van een van de ondersteunde Azure-bestanden kopiëren's, zoals Robocopy of AzCopy.

> [!IMPORTANT]
> Premium-bestandsshares zijn alleen beschikbaar met LRS en zijn beschikbaar in de meeste regio's die worden geboden door storage-accounts. Als u wilt weten als premium-bestandsshares op dit moment beschikbaar in uw regio zijn, Zie de [producten beschikbaar per regio](https://azure.microsoft.com/global-infrastructure/services/?products=storage) pagina voor Azure.

### <a name="provisioned-shares"></a>Ingerichte shares

Premium-bestandsshares worden ingericht op basis van een vaste GiB/IOPS/doorvoer verhouding. Voor elke GiB ingericht, wordt de share één IOPS en doorvoer van 0,1 MiB/s tot de maximumlimiet per share worden uitgegeven. De minimaal toegestane inrichting is 100 GiB met minimale IOPS/doorvoer.

Alle shares kunnen beste vermogen burst maximaal drie IOP's per GiB van ingerichte opslag gedurende 60 minuten of langer, afhankelijk van de grootte van de share. Nieuwe shares beginnen met het volledige burst-tegoed op basis van de ingerichte capaciteit.

Bestandsshares moeten worden ingericht in stappen van 1 GiB. Minimale grootte is 100 GiB, de volgende grootte is 101 GiB enzovoort.

> [!TIP]
> Basislijn IOPS = 1 * GiB ingericht. (Tot maximaal 100.000 IOP's).
>
> Limiet voor burst = 3 * basislijn IOPS. (Tot maximaal 100.000 IOP's).
>
> snelheid van uitgangsgebeurtenissen = 60 MiB/s + 0,06 * ingericht GiB
>
> inkomend verkeer snelheid = 40 MiB/s + 0,04 * ingericht GiB

Grootte van de bestandsshare op elk gewenst moment kan worden vergroot, maar alleen na 24 uur sinds de laatste toename kan worden verlaagd. Na een wachttijd van 24 uur zonder een toename van de grootte, kunt u de grootte van de bestandsshare zo vaak als u wilt, totdat u deze opnieuw verhogen verminderen. IOPS/doorvoer wijzigingen worden van kracht binnen een paar minuten na het wijzigen van grootte.

Het is mogelijk te verminderen van de grootte van uw ingerichte share hieronder uw gebruikte GiB. Als u dit doet, verliest u geen gegevens maar u wordt nog steeds gefactureerd voor de gebruikte grootte en de prestaties van de ingerichte share, niet de gebruikte grootte (basislijn IOPS, doorvoer en burst IOP's) te ontvangen.

De volgende tabel ziet u enkele voorbeelden van deze formules voor de ingerichte share-grootten:

|Capaciteit (GiB) | IOPS-basislijn | Burst-IOP 's | Egress (MiB/s) | Inkomend (MiB/s) |
|---------|---------|---------|---------|---------|
|100         | 100     | Maximaal 300     | 66   | 44   |
|500         | 500     | Maximaal 1500   | 90   | 60   |
|1,024       | 1,024   | Maximaal 3,072   | 122   | 81   |
|5,120       | 5,120   | Maximaal 15,360  | 368   | 245   |
|10,240      | 10,240  | Maximaal 30,720  | 675 | 450   |
|33,792      | 33,792  | Maximaal 100.000 | 2,088 | 1,392   |
|51,200      | 51,200  | Maximaal 100.000 | 3,132 | 2,088   |
|102,400     | 100.000 | Maximaal 100.000 | 6,204 | 4,136   |

> [!NOTE]
> Prestaties van de bestandsshares is onderworpen aan de machinelimieten netwerk, de beschikbare netwerkbandbreedte, i/o-grootte, parallelle uitvoering bij veel andere factoren. Voor het bereiken van maximale prestaties, schaal, de belasting te verdelen over meerdere virtuele machines. Raadpleeg [problemen oplossen met](storage-troubleshooting-files-performance.md) voor enkele veelvoorkomende problemen met prestaties en tijdelijke oplossingen.

### <a name="bursting"></a>Bursting

Premium-bestandsshares kunnen hun IOPS tot een factor van drie burst. Bursting is geautomatiseerd en werkt op basis van een systeem tegoed. Bursting werkt op de beste vermogen en de limiet voor ' burst ' is geen garantie, bestandsshares kunnen burst *tot* de limiet.

Tegoeden worden verzameld in een bucket burst wanneer verkeer voor de bestandsshare onder basislijn IOPS is. Een 100 GiB-share heeft bijvoorbeeld 100 basislijn IOPS. Als de werkelijke hoeveelheid verkeer op de share is 40 IOP's voor een bepaald interval van 1 seconde, worden de 60 ongebruikte IOPS gecrediteerd aan de bucket van een ' burst '. Dit tegoed wordt vervolgens later worden gebruikt wanneer bewerkingen wordt de basislijn IOPs overschreden.

> [!TIP]
> Grootte van de bucket burst basislijn IOPS = * 2 * 3600.

Wanneer een share is groter dan de basislijn IOPS en tegoed in een bucket burst heeft, wordt de stap over. Shares kunnen blijven om uit te breiden, zolang tegoed resterend, maar kleiner is dan 50 TiB shares alleen op de burst-limiet voor maximaal één uur blijft. Shares die groter is dan 50 TiB kunt technisch groter zijn dan deze limiet van één uur, van twee uur, maar dit is gebaseerd op het aantal burst tegoed productiewebservice. Elke i/o buiten basislijn IOPS een tegoed verbruikt en zodra alle tegoeden worden verbruikt, wordt de share terugkeert naar basislijn IOPS.

Share-tegoed heeft drie statussen:

- Oplopen, als de bestandsshare minder is dan de basislijn IOPS.
- Weigeren, wanneer de bestandsshare is cloudbursting.
- Resterende constante, wanneer er geen tegoed of een basislijn zijn IOP's worden gebruikt.

Nieuwe bestandsshares beginnen met het volledige nummer van-credits in de bucket burst. Burst-tegoed wordt niet worden samengevoegd als de share IOPS bij minder dan basislijn IOPS vanwege een beperking van de server.

## <a name="file-share-redundancy"></a>File share redundantie

Azure-bestanden standaard-bestandsshares bieden ondersteuning voor drie opties voor gegevensredundantie: lokaal redundante opslag (LRS), zone-redundante opslag (ZRS) en geografisch redundante opslag (GRS).

Azure premium bestandsshares ondersteunen alleen lokaal redundante opslag (LRS).

De volgende secties worden de verschillen tussen de verschillende redundantieopties voor:

### <a name="locally-redundant-storage"></a>Lokaal redundante opslag

[!INCLUDE [storage-common-redundancy-LRS](../../../includes/storage-common-redundancy-LRS.md)]

### <a name="zone-redundant-storage"></a>Zone-redundante opslag

[!INCLUDE [storage-common-redundancy-ZRS](../../../includes/storage-common-redundancy-ZRS.md)]

### <a name="geo-redundant-storage"></a>Geografisch redundante opslag

> [!Warning]  
> Als u uw Azure-bestandsshare als een cloudeindpunt in een GRS-opslagaccount gebruikt, kunt u failover voor storage-account mag niet starten. In dat geval wordt oorzaak synchroniseren niet meer werkt en kan ook leiden tot onverwachte gegevensverlies in het geval van nieuwe gelaagde bestanden. In het geval van verlies van een Azure-regio, wordt de failover van de storage-account op een manier die compatibel is met Azure File Sync worden geactiveerd door Microsoft.

Geografisch redundante opslag (GRS) is zodanig ontworpen dat ten minste 99,99999999999999% (16 9's) duurzaamheid van objecten in een bepaald jaar door uw gegevens te repliceren naar een secundaire regio die grote afstand van de primaire regio. Als uw storage-account GRS is ingeschakeld heeft, zijn uw gegevens zijn duurzame zelfs in geval van een volledige regionale stroomstoring of een ramp waarbij de primaire regio niet hersteld.

Als u ervoor kiezen voor geo-redundante opslag met leestoegang (RA-GRS), moet u weten Azure-bestand biedt geen ondersteuning voor geo-redundante opslag met leestoegang (RA-GRS) in elke regio op dit moment. Bestandsshares in het RA-GRS-opslagaccount werken net als ze net als in GRS-accounts en zijn dezelfde prijzen als voor in rekening gebracht.

GRS worden uw gegevens gerepliceerd naar een ander datacenter in een secundaire regio, maar deze beschikbaar is voor alleen-lezen zijn als Microsoft een failover van de primaire naar secundaire regio initieert.

Voor een opslagaccount met GRS is ingeschakeld, worden alle gegevens eerst gerepliceerd met lokaal redundante opslag (LRS). Een update is eerst gericht op de primaire locatie en gerepliceerd met behulp van LRS. De update wordt vervolgens asynchroon gerepliceerd naar de secundaire regio met GRS. Wanneer gegevens worden geschreven naar de secundaire locatie, wordt het ook binnen die locatie met behulp van LRS gerepliceerd.

De primaire en secundaire regio replica's beheren in afzonderlijke foutdomeinen en upgrade-domeinen binnen een opslagschaaleenheid. De opslagschaaleenheid is de basic-replicatie-eenheid binnen het datacenter. Replicatie op dit niveau wordt geleverd door LRS; Zie voor meer informatie, [lokaal redundante opslag (LRS): Gegevensredundantie met lage kosten voor Azure Storage](../common/storage-redundancy-lrs.md).

Houd er rekening mee bij het bepalen van welke replicatieoptie te gebruiken:

* Zone-redundante opslag (ZRS) biedt hoge beschikbaarheid met synchrone replicatie en mogelijk een betere keuze voor enkele scenario's dan GRS. Zie voor meer informatie over ZRS [ZRS](../common/storage-redundancy-zrs.md).
* Asynchrone replicatie omvat een vertraging vanaf het moment dat gegevens worden geschreven naar de primaire regio op wanneer deze worden gerepliceerd naar de secundaire regio. In het geval van een regionaal noodgeval wijzigingen die nog niet hebt zijn gerepliceerd naar de secundaire regio mogelijk verloren als die gegevens van de primaire regio niet kan worden hersteld.
* Met GRS, de replica is niet beschikbaar voor lezen of schrijven, tenzij Microsoft een failover naar de secundaire regio initieert. In het geval van een failover, u zult hebt gelezen en schrijftoegang tot die gegevens na de failover is voltooid. Zie voor meer informatie, [Disaster recovery guidance](../common/storage-disaster-recovery-guidance.md).

## <a name="onboard-to-larger-file-shares-standard-tier"></a>Onboarding van grotere-bestandsshares (standard-laag)

In deze sectie geldt alleen voor de standard-bestandsshares. Alle bestandsshares van premium zijn beschikbaar met 100 TiB als een aanbieding voor algemene beschikbaarheid.

### <a name="restrictions"></a>Beperkingen

- Moet u een nieuwe storage-account voor algemeen gebruik is (niet uitbreiden de bestaande opslagaccounts) maken.
- LRS naar GRS-account conversie is niet mogelijk zijn op alle nieuwe opslagaccounts die zijn gemaakt nadat het abonnement wordt geaccepteerd voor de preview van de grotere file shares.

### <a name="regional-availability"></a>Regionale beschikbaarheid

Standard-bestandsshares zijn beschikbaar in alle regio's maximaal 5 TiB. In bepaalde regio's, is beschikbaar met een limiet van 100 TiB, regio's worden vermeld in de volgende tabel:

|Regio  |Ondersteunde redundantie  |Ondersteunt de bestaande opslagaccounts  |
|---------|---------|---------|
|Zuidoost-Azië     |LRS|Nee         |
|Europa -west     |LRS|Nee         |
|US - west 2     |LRS, ZRS|Nee         |


### <a name="steps-to-onboard"></a>Stappen voor onboarding

Voer de volgende PowerShell-opdrachten voor het inschrijven van uw abonnement op de preview van de grotere file shares:

```powershell
Register-AzProviderFeature -FeatureName AllowLargeFileShares -ProviderNamespace Microsoft.Storage
Register-AzResourceProvider -ProviderNamespace Microsoft.Storage
```
Uw abonnement wordt automatisch goedgekeurd als beide opdrachten worden uitgevoerd.

Als u wilt controleren of de registratiestatus van uw, kunt u de volgende opdracht uitvoeren:

```powershell
Get-AzProviderFeature -FeatureName AllowLargeFileShares -ProviderNamespace Microsoft.Storage
```

Het duurt maximaal 15 minuten voor uw status bij te werken naar **geregistreerd**. Zodra uw status is **geregistreerd**, zou het mogelijk om de functie te gebruiken.

### <a name="use-larger-file-shares"></a>Grotere-bestandsshares gebruiken

Als u wilt gebruiken, grotere bestandsshares, een nieuw opslagaccount voor algemeen gebruik v2 en een nieuwe bestandsshare te maken.

## <a name="data-growth-pattern"></a>Patroon van de groei van gegevens

Vandaag de dag de maximale grootte voor een Azure-bestandsshare is 5 TiB (100 TiB in preview). Vanwege deze huidige beperking, u moet rekening houden met groei van de verwachte gegevens bij het implementeren van een Azure-bestandsshare.

Het is mogelijk om te synchroniseren die meerdere Azure-bestandsshares op een enkele Windows-bestandsserver met Azure File Sync. Hiermee kunt u om ervoor te zorgen dat oudere, grote bestandsshares dat er on-premises naar Azure File Sync kunnen worden gebracht. Zie voor meer informatie, [plannen voor een implementatie van Azure File Sync](storage-files-planning.md).

## <a name="data-transfer-method"></a>Methode voor gegevensoverdracht

Er zijn veel eenvoudige opties voor het bulksgewijs overdracht van gegevens uit een bestaand bestand, zoals een bestandsshare on-premises naar Azure Files delen. Enkele populaire services zijn onder andere (niet-uitputtende lijst):

* **Azure File Sync**: Als onderdeel van een eerste synchronisatie tussen een Azure-bestandsshare (een "Cloudeindpunt") en een Windows-directory-naamruimte (een "eindpunt voor de Server'), wordt Azure File Sync alle gegevens van de bestaande bestandsshare repliceren naar Azure Files.
* **[Azure Import/Export](../common/storage-import-export-service.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)** : De Azure Import/Export-service kunt u veilig grote hoeveelheden gegevens overdragen naar een Azure-bestandsshare met de verzending van harde schijven naar een Azure-datacenter. 
* **[Robocopy](https://technet.microsoft.com/library/cc733145.aspx)** : Robocopy is een bekende kopie-hulpprogramma dat wordt geleverd met Windows en Windows Server. Robocopy kan worden gebruikt om gegevens over naar Azure Files te koppelen van de bestandsshare lokaal en klik vervolgens met behulp van de gekoppelde locatie als het doel in de Robocopy-opdracht.
* **[AzCopy](../common/storage-use-azcopy-v10.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)** : AzCopy is een opdrachtregelprogramma voor het kopiëren van gegevens naar en van Azure Files, evenals Azure Blob-opslag met behulp van eenvoudige opdrachten met optimale prestaties.

## <a name="next-steps"></a>Volgende stappen
* [Planning voor de implementatie van een Azure File Sync](storage-sync-files-planning.md)
* [Azure Files implementeren](storage-files-deployment-guide.md)
* [Azure File Sync implementeren](storage-sync-files-deployment-guide.md)
