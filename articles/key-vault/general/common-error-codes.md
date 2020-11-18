---
title: Algemene fout codes voor Azure Key Vault | Microsoft Docs
description: Algemene fout codes voor Azure Key Vault
services: key-vault
author: sebansal
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: reference
ms.date: 09/29/2020
ms.author: mbaldwin
ms.openlocfilehash: a543f03cb73d9eae8eaa81eeb3a37fd59e4e6a81
ms.sourcegitcommit: c157b830430f9937a7fa7a3a6666dcb66caa338b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/17/2020
ms.locfileid: "94685797"
---
# <a name="common-error-codes-for-azure-key-vault"></a>Algemene fout codes voor Azure Key Vault

De fout codes die in de volgende tabel worden weer gegeven, kunnen worden geretourneerd door een bewerking op Azure Key kluis

| Foutcode | Gebruikers bericht |
|--|--|
| VaultAlreadyExists |  De poging om een nieuwe sleutel kluis met de opgegeven naam te maken, is mislukt omdat de naam al in gebruik is. Als u onlangs een sleutel kluis met deze naam hebt verwijderd, kan deze zich nog steeds in de modus voorlopig verwijderd bevinden. U kunt [hier](https://docs.microsoft.com/azure/key-vault/general/key-vault-recovery?tabs=azure-portal#list-recover-or-purge-a-soft-deleted-key-vault) controleren of het bestaat uit de voorlopig verwijderde status |
| VaultNameNotValid |  De naam van de kluis moet 24 tekens en alfanumeriek zijn en begint met een alfabet |
| AccessDenied |  Mogelijk ontbreken er machtigingen in het toegangs beleid om deze bewerking uit te voeren. |
| ForbiddenByFirewall |  Het client adres is niet geautoriseerd en de aanroeper is geen vertrouwde service. |
| ConflictError |  U vraagt meerdere bewerkingen aan op hetzelfde item.  |
| RegionNotSupported |  De opgegeven Azure-regio wordt niet ondersteund voor deze resource. |
| SkuNotSupported |  Het opgegeven SKU-type wordt niet ondersteund voor deze resource. |
| ResourceNotFound |  De opgegeven Azure-resource is niet gevonden. |
| ResourceGroupNotFound | De opgegeven Azure-resource groep is niet gevonden. |
| CertificateExpired |  Controleer de verval datum en de geldigheids periode van het certificaat. |


## <a name="next-steps"></a>Volgende stappen

- Zie de [gids voor Azure Key Vault-ontwikkelaars](developers-guide.md) (vooralsnog Engelstalig).
- Meer informatie over het [verifiëren bij Key Vault](authentication.md)
