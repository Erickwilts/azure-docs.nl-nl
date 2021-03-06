---
title: Azure-opslagaccounts
titleSuffix: Azure Media Services
description: Meer informatie over het maken van een Azure Storage-account voor gebruik met Azure Media Services.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: conceptual
ms.date: 01/05/2021
ms.author: inhenkel
ms.openlocfilehash: 36593efd4f2583f28dffbb4f5b491fbeec7ba21d
ms.sourcegitcommit: 67b44a02af0c8d615b35ec5e57a29d21419d7668
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/06/2021
ms.locfileid: "97916317"
---
# <a name="azure-storage-accounts"></a>Azure Storage-accounts

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Als u media-inhoud in azure wilt beheren, versleutelen, coderen, analyseren en streamen, moet u een Media Services-account maken. Als u een Media Services-account gaat maken, moet u de naam van een Azure Storage-accountresource opgeven. Het opgegeven opslagaccount wordt gekoppeld aan uw Media Services-account.

Het Media Services-account en alle gekoppelde opslagaccounts moeten zich in hetzelfde Azure-abonnement bevinden. Het wordt ten zeerste aanbevolen om opslag accounts te gebruiken op dezelfde locatie als het Media Services-account om extra latentie en kosten voor gegevens uitvoer te voor komen.

U kunt maar één **primaire** opslagaccount koppelen aan uw Media Services-account, maar een onbeperkt aantal **secundaire** opslagaccounts. Media Services ondersteunt **GPv2**-accounts (General-purpose v2) of **GPv1**-accounts (General-purpose v1). Alleen BLOB-accounts zijn niet toegestaan als **primair**.

We raden u aan GPv2 te gebruiken, zodat u kunt profiteren van de nieuwste functies en prestaties. Zie [Azure Storage account overview](../../storage/common/storage-account-overview.md)(Engelstalig) voor meer informatie over opslag accounts.

> [!NOTE]
> Alleen de laag Hot Access wordt ondersteund voor gebruik met Azure Media Services, hoewel de andere toegangs lagen kunnen worden gebruikt om de opslag kosten te verlagen voor inhoud die niet actief wordt gebruikt.

Er zijn verschillende Sku's die u kunt kiezen voor uw opslag account. Zie [Opslagaccounts](/cli/azure/storage/account?view=azure-cli-latest) voor meer informatie. Als u wilt experimenteren met opslagaccounts, gebruikt u `--sku Standard_LRS`. Wanneer u echter een SKU voor productie gaat picken, moet u overwegen om een `--sku Standard_RAGRS` geografische replicatie voor bedrijfs continuïteit te bieden.

## <a name="assets-in-a-storage-account"></a>Assets in een opslag account

In Media Services v3 worden de opslag-Api's gebruikt voor het uploaden van bestanden naar assets. Zie [assets in azure Media Services v3](assets-concept.md)voor meer informatie.

> [!Note]
> Probeer niet de inhoud te wijzigen van BLOB-containers die zijn gegenereerd door de Media Services SDK zonder gebruik te maken van Media Services-Api's.

## <a name="storage-side-encryption"></a>Versleuteling van opslag side

Ter bescherming van uw assets op rest moeten de activa worden versleuteld door de versleuteling van de opslag side. In de volgende tabel ziet u hoe de opslag side-versleuteling werkt in Media Services V3:

|Versleutelings optie|Beschrijving|Media Services v3|
|---|---|---|
|Opslag versleuteling Media Services| AES-256-versleuteling, sleutel beheerd door Media Services. |Niet ondersteund. <sup>(1)</sup>|
|[Versleuteling van de opslag service voor Data-at-rest](../../storage/common/storage-service-encryption.md)|Versleuteling aan de server zijde die wordt aangeboden door Azure Storage, sleutel die wordt beheerd door Azure of door de klant.|Ondersteund.|
|[Versleuteling van de opslag aan de client zijde](../../storage/common/storage-client-side-encryption.md)|Versleuteling aan de client zijde die wordt aangeboden door Azure Storage, sleutel die wordt beheerd door de klant in Key Vault.|Niet ondersteund.|

<sup>1</sup> in Media Services V3 wordt opslag VERSLEUTELING (AES-256-versleuteling) alleen ondersteund voor achterwaartse compatibiliteit wanneer uw assets zijn gemaakt met Media Services v2, wat betekent dat v3 werkt met bestaande, versleutelde opslag elementen, maar niet om nieuwe te maken.

## <a name="double-encryption"></a>Dubbele versleuteling
Media Services ondersteunt dubbele versleuteling.  Zie voor meer informatie over dubbele versleuteling [Azure Double Encryption](https://docs.microsoft.com/azure/security/fundamentals/double-encryption)(Engelstalig).

## <a name="storage-account-errors"></a>Fouten in het opslag account

De status 'Verbinding verbroken' voor een Media Services-account geeft aan dat het account geen toegang meer heeft tot een of meer van de gekoppelde opslagaccounts vanwege een wijziging van de toegangssleutels voor opslag. Bijgewerkte toegangssleutels voor opslag zijn noodzakelijk voor Media Services om allerlei taken te kunnen uitvoeren in het account.

Hieronder vindt u de belangrijkste scenario's waarin een Media Services-account geen toegang krijgt tot gekoppelde opslagaccounts.

|Probleem|Oplossing|
|---|---|
|Het Media Services-account of een of meer gekoppelde opslagaccounts zijn gemigreerd naar afzonderlijke abonnementen. |Migreer de opslag account (s) of het Media Services-account, zodat ze allemaal in hetzelfde abonnement zijn. |
|Het Media Services-account gebruikt een gekoppeld opslagaccount in een ander abonnement, aangezien het een vroeg Media Services-account betreft waarvoor dit nog werd ondersteund. Alle Early Media Services-accounts zijn geconverteerd naar moderne Azure Resources Manager-accounts en hebben een status die niet meer is verbonden. |Migreer het opslag account of het Media Services account zo dat ze allemaal in hetzelfde abonnement zijn.|

## <a name="azure-storage-firewall"></a>Azure Storage firewall

Azure Media Services biedt geen ondersteuning voor opslag accounts waarbij de Azure Storage firewall of [persoonlijke eind punten](../../storage/common/storage-network-security.md) zijn ingeschakeld.

## <a name="next-steps"></a>Volgende stappen

Zie [een account maken](./create-account-howto.md)voor meer informatie over het koppelen van een opslag account aan uw Media Services-account.
