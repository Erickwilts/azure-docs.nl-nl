---
title: bestand opnemen
description: bestand opnemen
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 12/04/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 372342611265640a2a64100f003880a430d61ca0
ms.sourcegitcommit: 8192034867ee1fd3925c4a48d890f140ca3918ce
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2020
ms.locfileid: "96620937"
---
In de preview-versie heeft NFS de volgende beperkingen:

- NFS 4,1 ondersteunt momenteel alleen de meeste functies van de [protocol specificatie](https://tools.ietf.org/html/rfc5661). Sommige functies, zoals delegaties en retour aanroepen van allerlei soorten, het vergren delen van upgrades en downgrades, Kerberos-verificatie en versleuteling, worden niet ondersteund.
- Als het meren deel van uw aanvragen meta gegevens georiënteerd is, is de latentie verergert in vergelijking met lees-en schrijf-en bijwerk bewerkingen.
- U moet een nieuw opslag account maken om een NFS-share te kunnen maken.
- Alleen de REST Api's van het beheer vlak worden ondersteund. REST-Api's voor het gegevens vlak zijn niet beschikbaar. Dit betekent dat hulpprogram ma's zoals Storage Explorer niet werken met NFS-shares en dat u niet kunt bladeren door NFS-share gegevens in de Azure Portal.
- AzCopy wordt momenteel niet ondersteund.
- Alleen beschikbaar voor de Premium-laag.
- NFS-shares accepteren alleen numerieke UID/GID. Als u wilt voor komen dat uw clients alfanumerieke UID/GID verzenden, moet u ID-toewijzing uitschakelen.
- Shares kunnen alleen worden gekoppeld vanuit één opslag account op een afzonderlijke virtuele machine, wanneer persoonlijke koppelingen worden gebruikt. Poging om shares te koppelen van andere opslag accounts, mislukt.

### <a name="azure-storage-features-not-yet-supported"></a>Azure Storage functies die nog niet worden ondersteund

Daarnaast zijn de volgende Azure Files-functies niet beschikbaar met NFS-shares:

- Verificatie op basis van identiteit
- Ondersteuning voor Azure Backup
- Momentopnamen
- Voorlopig verwijderen
- Volledige versleuteling-in-transit ondersteuning (voor meer informatie raadpleegt u [NFS-beveiliging](../articles/storage/files/storage-files-compare-protocols.md#security))
- Azure File Sync (alleen beschikbaar voor Windows-clients, die niet door NFS 4,1 worden ondersteund)
