---
title: bestand opnemen
description: bestand opnemen
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 11/05/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 7c3438631dca921989309bb8701e113cb5ce3ff2
ms.sourcegitcommit: d0541eccc35549db6381fa762cd17bc8e72b3423
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/09/2020
ms.locfileid: "89569991"
---
Azure Blob Storage is Microsoft's oplossing voor de opslag van objecten in de cloud. Blob Storage is geoptimaliseerd voor het opslaan van enorme hoeveelheden niet-structureerde gegevens. Ongestructureerde gegevens zijn gegevens die niet voldoen aan een bepaald gegevensmodel of bepaalde definitie, zoals tekst of binaire gegevens.

## <a name="about-blob-storage"></a>Over Blob Storage

BLOB Storage is ontworpen voor:

* Het rechtstreeks aan een browser leveren van afbeeldingen of documenten.
* De opslag van bestanden voor gedistribueerde toegang.
* Streaming van video en audio.
* Schrijven naar logboekbestanden.
* De opslag van gegevens voor back-up en herstel, herstel na noodgevallen en archivering.
* De opslag van gegevens voor analyse door een on-premises of in Azure gehoste service.

Gebruikers of clienttoepassingen hebben, overal ter wereld, toegang tot objecten in Blob Storage via HTTP/HTTPS. Objecten in Blob Storage zijn toegankelijk via de [REST-API van Azure Storage](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api), [Azure PowerShell](https://docs.microsoft.com/powershell/module/az.storage), [Azure CLI](https://docs.microsoft.com/cli/azure/storage) of een Azure Storage-clientbibliotheek. Clientbibliotheken zijn beschikbaar voor verschillende talen, waaronder:

* [.NET](/dotnet/api/overview/azure/storage?view=azure-dotnet)
* [Java](https://docs.microsoft.com/java/api/overview/azure/storage)
* [Node.js](https://azure.github.io/azure-storage-node)
* [Python](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-python)
* [Go](https://github.com/azure/azure-storage-blob-go/)
* [PHP](https://azure.github.io/azure-storage-php/)
* [Ruby](https://azure.github.io/azure-storage-ruby)

## <a name="about-azure-data-lake-storage-gen2"></a>Over Azure Data Lake Storage Gen2

Blob Storage ondersteunt Azure Data Lake Storage Gen2, de zakelijke oplossing van Microsoft voor analyse van big data voor de cloud. Azure Data Lake Storage Gen2 biedt een hiërarchisch bestandssysteem, evenals de voordelen van Blob-opslag, waaronder:

* Goedkope, gelaagde opslag
* Hoge beschikbaarheid
* Hoge consistentie
* Mogelijkheden voor herstel na noodgevallen

Raadpleeg [Inleiding tot Azure Data Lake Storage Gen2](../articles/storage/data-lake-storage/introduction.md) voor meer informatie over Data Lake Storage Gen2.
