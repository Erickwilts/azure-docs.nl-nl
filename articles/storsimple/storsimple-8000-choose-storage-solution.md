---
title: Opties voor gegevens overdracht naar Azure met behulp van een apparaat | Microsoft Docs
description: Meer informatie over het kiezen van het juiste apparaat voor het overdragen van gegevens naar Azure
services: storsimple
author: alkohli
ms.service: storsimple
ms.topic: article
ms.date: 04/01/2019
ms.author: alkohli
ms.openlocfilehash: 9456d975ffb6d7726166f9badbcb8fe0ccbe5256
ms.sourcegitcommit: 5d6c8231eba03b78277328619b027d6852d57520
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/13/2019
ms.locfileid: "68965347"
---
# <a name="compare-storsimple-with-azure-file-sync-and-data-box-edge-data-transfer-options"></a>StorSimple vergelijken met Azure File Sync en Data Box Edge opties voor gegevens overdracht 

[!INCLUDE [storsimple-8000-eol-banner](../../includes/storsimple-8000-eol-banner.md)]
 
Dit document bevat een overzicht van de opties voor on-premises gegevens overdracht naar Azure, vergelijken: Data Box Edge versus Azure File Sync versus StorSimple 8000-serie.

- **[Data Box Edge](/azure/databox-online/data-box-edge-overview)** – data Box Edge is een on-premises netwerk apparaat waarmee gegevens worden verplaatst van en naar Azure en die een rand Compute met AI-functionaliteit heeft om gegevens vooraf te verwerken tijdens het uploaden. Data Box Gateway is een virtuele versie van het apparaat met dezelfde mogelijkheden voor gegevens overdracht.
- **[Azure file sync](/azure/storage/files/storage-sync-files-deployment-guide)** : Azure File Sync kan worden gebruikt om de bestands shares van uw organisatie in azure files te centraliseren, terwijl de flexibiliteit, prestaties en compatibiliteit van een on-premises Bestands server behouden blijven. Azure File Sync transformeert Windows Server in een snelle cache van uw Azure-bestands share. De algemene Beschik baarheid van Azure File Sync is eerder aangekondigd in 2018.
- **[StorSimple](/azure/storsimple/storsimple-overview)** – StorSimple is een hybride apparaat dat ondernemingen helpt bij het consolideren van hun opslag infrastructuur voor primaire opslag, gegevens beveiliging, archivering en herstel na nood gevallen voor een enkele oplossing door nauw keurig te integreren met Azure Storage. De levens cyclus van het product voor StorSimple vindt u [hier](https://support.microsoft.com/lifecycle/search?alpha=Azure%20StorSimple%208000%20Series).

## <a name="comparison-summary"></a>Vergelijkings overzicht

|                           |StorSimple 8000   |Azure File Sync   |Data Box Edge           |
|---------------------------|----------------------------------------|-------------------------------|-----------------------------------------|
|Overzicht         |Gelaagde hybride opslag en archivering|Algemene bestands Server opslag met Cloud lagen en multi-site synchronisatie.  |Opslag oplossing om gegevens vooraf te verwerken en deze via netwerk naar Azure te verzenden.        |
|Scenario's        |Bestands server, archivering, back-updoel |Bestands server, archivering (meerdere locaties)   |Gegevens overdracht, vooraf verwerkingen van gegevens, inclusief ML, IoT, archivering    |
|Edge-rekenproces     |Niet beschikbaar |Niet beschikbaar |Ondersteunt het uitvoeren van containers met behulp van Azure IoT Edge    |
|Vorm factor      |Fysiek apparaat   |Agent geïnstalleerd op Windows Server |Fysiek apparaat   |
|Hardware         |Fysiek apparaat dat door micro soft is meegeleverd als onderdeel van de service | Klant |Fysiek apparaat dat door micro soft is meegeleverd als onderdeel van de service  |
|Gegevensindeling      |Aangepaste notatie   |Bestanden         |Blobs of bestanden    |
|Protocol ondersteuning |iSCSI          |SMB, NFS    | SMB of NFS      |
|Prijzen          |[StorSimple](https://azure.microsoft.com/pricing/details/storsimple/) |[Azure File Sync](https://azure.microsoft.com/pricing/details/storage/files/)  |[Data Box Edge](https://azure.microsoft.com/pricing/details/storage/databox/edge/)  |

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [Azure data Box Edge](/azure/databox-online/data-box-edge-overview) en [Azure data Box gateway](/azure/databox-online/data-box-gateway-overview)
- Meer informatie over [Azure file sync](/azure/storage/files/storage-sync-files-deployment-guide)
