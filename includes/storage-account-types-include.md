---
title: bestand opnemen
description: bestand opnemen
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 03/23/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: d4f57eca89cbb68d61546c6d5ce5bcd04f9256e7
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66114946"
---
Azure Storage biedt verschillende typen opslagaccounts. Elk type biedt ondersteuning voor verschillende functies en heeft een eigen prijsmodel. Houd rekening met deze verschillen voordat u een opslagaccount om te bepalen welk type account dat wordt aanbevolen voor uw toepassingen kunt maken. De typen opslagaccounts zijn:

- **Voor algemeen gebruik v2-accounts**: Basic opslagaccounttype voor blobs, bestanden, wachtrijen en tabellen. Aanbevolen voor de meeste scenario's met behulp van Azure Storage.
- **Accounts voor algemeen gebruik v1**: Verouderde accounttype voor blobs, bestanden, wachtrijen en tabellen. Voor algemeen gebruik v2-accounts gebruiken, indien mogelijk.
- **Blob storage-accounts blokkeren**: Alleen-BLOB storage-accounts met premium-prestatiekenmerken. Aanbevolen voor scenario's met hoge transacties-tarieven, met behulp van kleinere objecten of het vereisen van consistent met lage opslaglatentie.
- **FileStorage (preview) storage-accounts**: Bestanden alleen-lezen-storage-accounts met premium-prestatiekenmerken. Aanbevolen voor enterprise- of schaaltoepassingen met hoge prestaties.
- **BLOB storage-accounts**: Alleen-BLOB storage-accounts. Voor algemeen gebruik v2-accounts gebruiken, indien mogelijk.

De volgende tabel beschrijft de soorten opslagaccounts en de bijbehorende mogelijkheden:

| Type opslagaccount | Ondersteunde services                       | Ondersteunde prestatielagen      | Ondersteunde toegangslagen         | Opties voor gegevensreplicatie               | Implementatiemodel<sup>1</sup> | Versleuteling<sup>2</sup> |
|----------------------|------------------------------------------|-----------------------------|--------------------------------|-----------------------------------|------------------------------|------------------------|
| Algemeen gebruik V2   | BLOB, bestand, wachtrij, tabel en schijf       | Standard, Premium<sup>5</sup> | Hot, Cool, Archive<sup>3</sup> | LRS-, ZRS<sup>4</sup>, GRS, RA-GRS | Resource Manager             | Versleuteld              |
| Algemeen gebruik V1   | BLOB, bestand, wachtrij, tabel en schijf       | Standard, Premium<sup>5</sup> | N/A                            | LRS, GRS, RA-GRS                  | Resource Manager, klassiek    | Versleuteld              |
| Blok-blobopslag   | BLOB (blok-blobs en toevoeg-blobs alleen) | Premium                       | N/A                            | LRS                               | Resource Manager             | Versleuteld              |
| FileStorage (preview)   | U kunt alleen bestanden | Premium                       | N/A                            | LRS                               | Resource Manager             | Versleuteld              |
| Blob Storage         | BLOB (blok-blobs en toevoeg-blobs alleen) | Standard                      | Hot, Cool, Archive<sup>3</sup> | LRS, GRS, RA-GRS                  | Resource Manager             | Versleuteld              |

<sup>1</sup>met het Azure Resource Manager-implementatiemodel wordt aanbevolen. Opslagaccounts met behulp van het klassieke implementatiemodel kunnen nog steeds worden gemaakt op bepaalde locaties en bestaande klassieke accounts worden nog steeds ondersteund. Zie voor meer informatie, [Azure Resource Manager en klassieke implementatie: Implementatiemodellen en de status van uw resources begrijpen](../articles/azure-resource-manager/resource-manager-deployment-model.md).

<sup>2</sup>alle opslagaccounts zijn versleuteld met behulp van Storage Service Encryption (SSE) voor data-at-rest. Zie voor meer informatie, [Azure Storage-Serviceversleuteling voor Data-at-Rest](../articles/storage/common/storage-service-encryption.md).

<sup>3</sup>de Archive-laag is beschikbaar op het niveau van een afzonderlijke blob alleen, niet op het niveau van de storage-account. Alleen blok-blobs en toevoeg-blobs kunnen worden gearchiveerd. Zie voor meer informatie, [Azure Blob-opslag: Hot, Cool, en Archiefopslaglaag](../articles/storage/blobs/storage-blob-storage-tiers.md).

<sup>4</sup>zone-redundante opslag (ZRS) is alleen beschikbaar voor algemeen gebruik v2 standard storage-accounts. Zie voor meer informatie over ZRS [Zone-redundante opslag (ZRS): Maximaal beschikbare toepassingen voor Azure Storage](../articles/storage/common/storage-redundancy-zrs.md). Zie voor meer informatie over andere opties voor gegevensreplicatie [Azure Storage-replicatie](../articles/storage/common/storage-redundancy.md).

<sup>5</sup> premium-prestaties voor algemeen gebruik v2 en algemeen gebruik v1-accounts is beschikbaar voor schijven en pagina-blob.