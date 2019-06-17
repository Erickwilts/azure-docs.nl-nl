---
title: Afbeeldingopslag in Azure Container Registry
description: Als u meer informatie over hoe uw Docker-containerinstallatiekopieën worden opgeslagen in Azure Container Registry, met inbegrip van beveiliging, redundantie en capaciteit.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 03/21/2018
ms.author: danlep
ms.openlocfilehash: 55c84907ab41f6da9d7a0989c68a1c1f90c5e424
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60827269"
---
# <a name="container-image-storage-in-azure-container-registry"></a>Afbeeldingopslag container in Azure Container Registry

Elke [Basic, Standard en Premium](container-registry-skus.md) Azure container registry voordelen van Azure storage geavanceerde functies zoals versleuteling-at-rest voor beveiliging van de installatiekopie van gegevens en geografische redundantie voor de installatiekopie van de beveiliging van gegevens. De volgende secties worden de functies en de grenzen van de installatiekopie van opslag in Azure Container Registry (ACR).

## <a name="encryption-at-rest"></a>Encryption-at-rest

Alle containerinstallatiekopieën in het register worden in rust versleuteld. Azure automatisch een installatiekopie van een versleuteld voordat deze worden opgeslagen en ontsleutelt deze op het begeven wanneer u of uw toepassingen en services Haal de installatiekopie.

## <a name="geo-redundant-storage"></a>Geografisch redundante opslag

Azure maakt gebruik van een schema voor geografisch redundante opslag ter bescherming tegen verlies van uw containerinstallatiekopieën. Azure Container Registry repliceert automatisch uw containerinstallatiekopieën naar meerdere geografisch verafgelegen datacenters, hun gegevensverlies voorkomen in het geval van een regionale optreedt.

## <a name="geo-replication"></a>Geo-replicatie

Voor scenario's vereisen nog meer zekerheid van hoge beschikbaarheid kunt u overwegen de [geo-replicatie](container-registry-geo-replication.md) functie van Premium-registers. Geo-replicatie zorgt voor de bescherming tegen het verlies van toegang naar het register in geval van een *totale* regionaal, niet alleen een optreedt. Geo-replicatie biedt andere voordelen, te, zoals in afbeelding dichtbij het netwerk opslag voor sneller pushes en ophaalt in gedistribueerde scenario's voor ontwikkeling of implementatie.

## <a name="image-limits"></a>Installatiekopie-limieten

De volgende tabel beschrijft de container-installatiekopie en de opslaglimieten voor Azure container Registry.

| Resource | Limiet |
| -------- | :---- |
| Opslagplaatsen | Geen limiet |
| Installatiekopieën | Geen limiet |
| Lagen | Geen limiet |
| Tags | Geen limiet|
| Opslag | 5 TB |

Zeer groot aantal opslagplaatsen en tags kan invloed hebben op de prestaties van uw register. Regelmatig ongebruikte opslagplaatsen, labels en afbeeldingen als onderdeel van uw register onderhoud routine verwijderen. Register-resources, zoals opslagplaatsen, installatiekopieën en tags verwijderd *kan geen* worden hersteld na verwijdering. Zie voor meer informatie over het verwijderen van register resources [verwijderen van installatiekopieën van containers in Azure Container Registry](container-registry-delete.md).

## <a name="storage-cost"></a>Opslagkosten

Zie voor meer informatie over de prijzen [prijzen voor Azure Container Registry][pricing].

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de verschillende Azure Container Registry-SKU's (Basic, Standard, Premium), [Azure Container Registry-SKU's](container-registry-skus.md).

<!-- IMAGES -->

<!-- LINKS - External -->
[portal]: https://portal.azure.com
[pricing]: https://aka.ms/acr/pricing

<!-- LINKS - Internal -->
