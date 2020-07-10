---
title: Azure Data Box limieten | Microsoft Docs
description: Hierin worden systeem limieten en aanbevolen grootten voor de Microsoft Azure Data Box onderdelen en verbindingen beschreven.
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: article
ms.date: 07/10/2020
ms.author: alkohli
ms.openlocfilehash: 7d699fc47fa0a0cb57d103ff42ff17bdc0f3a82b
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2020
ms.locfileid: "86202688"
---
# <a name="azure-data-box-limits"></a>Azure Data Box limieten

Houd rekening met deze beperkingen tijdens het implementeren en uitvoeren van uw Microsoft Azure Data Box. In de volgende tabel worden deze limieten voor de Data Box beschreven.

## <a name="data-box-service-limits"></a>Data Box-Service limieten

[!INCLUDE [data-box-service-limits](../../includes/data-box-service-limits.md)]

## <a name="data-box-limits"></a>Data Box limieten

- Data Box kunt Maxi maal 500.000.000 bestanden opslaan voor importeren en exporteren.
- Data Box ondersteunt Maxi maal 512 containers of shares in de Cloud. De mappen op het hoogste niveau binnen de gebruikers share worden containers of Azure-bestands shares in de Cloud. 
- Data Box gebruiks capaciteit kan kleiner zijn dan 80 TB vanwege het gebruik van ReFS-meta gegevens ruimte.

## <a name="azure-storage-limits"></a>Limieten voor Azure Storage

[!INCLUDE [data-box-storage-limits](../../includes/data-box-storage-limits.md)]

## <a name="data-upload-caveats"></a>Voorbehouden voor het uploaden van gegevens


### <a name="for-import-order"></a>Voor import order

Data Box aanvullende opmerkingen voor een import volgorde zijn onder andere:

[!INCLUDE [data-box-data-upload-caveats](../../includes/data-box-data-upload-caveats.md)]

## <a name="for-export-order"></a>Voor export volgorde

Data Box aanvullende opmerkingen voor een export volgorde zijn onder andere:

- Data Box is een Windows-apparaat en ondersteunt geen hoofdletter gevoelige bestands namen. U kunt bijvoorbeeld twee verschillende bestanden in azure hebben met namen die alleen in de behuizing verschillen. Gebruik data box niet om dergelijke bestanden te exporteren, omdat deze worden overschreven op het apparaat.
- Als er dubbele Tags aanwezig zijn in de invoer bestanden of tags die verwijzen naar dezelfde gegevens, kan Data Box export de bestanden overs Laan of overschrijven. Het aantal bestanden en de grootte van de gegevens die op de Azure Portal worden weer gegeven, kan afwijken van de werkelijke grootte van de gegevens op het apparaat. 
- Data Box exporteert gegevens naar Windows op basis van SMB en wordt beperkt door SMB-beperkingen voor bestanden en mappen. Bestanden en mappen met niet-ondersteunde namen worden niet geëxporteerd.
- Er is een 1:1-toewijzing van het voor voegsel van de container.
- De maximale bestands grootte is 1024 tekens bestanden, bestands namen die deze lengte overschrijden, worden niet geëxporteerd.
- Dubbele voor voegsels in het *XML-* bestand (geüpload tijdens het maken van de order) worden geëxporteerd. Dubbele voor voegsels worden niet genegeerd.
- Pagina-blobs en container namen zijn hoofdletter gevoelig, dus als de behuizing niet overeenkomt, wordt de BLOB en/of container niet gevonden.
 

## <a name="azure-storage-account-size-limits"></a>Maximale grootte van Azure Storage-account

[!INCLUDE [data-box-storage-account-size-limits](../../includes/data-box-storage-account-size-limits.md)]

## <a name="azure-object-size-limits"></a>Limieten voor Azure-object grootte

[!INCLUDE [data-box-object-size-limits](../../includes/data-box-object-size-limits.md)]

## <a name="azure-block-blob-page-blob-and-file-naming-conventions"></a>Azure Block blob-, page Blob-en file naamgevings conventies

[!INCLUDE [data-box-naming-conventions](../../includes/data-box-naming-conventions.md)]
