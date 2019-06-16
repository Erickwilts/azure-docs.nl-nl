---
title: Voor algemene doeleinden-servicelaag - Azure SQL Database | Microsoft Docs
description: Meer informatie over de laag van Azure SQL Database voor algemene doeleinden
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop-msft
ms.reviewer: sstein
manager: craigg
ms.date: 02/07/2019
ms.openlocfilehash: b972ea985a09457d8b6a17a292e18754761f5a6e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66479191"
---
# <a name="general-purpose-service-tier---azure-sql-database"></a>Algemeen gebruik-servicelaag - Azure SQL Database

> [!NOTE]
> De servicelaag voor algemeen gebruik-in het op vCore gebaseerde aankoopmodel wordt het serviceniveau standard in het op DTU gebaseerde aankoopmodel genoemd. Zie voor een vergelijking van de vCore gebaseerde aankoopmodel met het op DTU gebaseerde aankoopmodel [Azure SQL Database-modellen en -bronnen aanschaffen](sql-database-purchase-models.md).

Azure SQL Database is gebaseerd op voor de cloudomgeving is aangepast om ervoor te zorgen, zelfs in het geval van infrastructuuruitval voor 99,99% beschikbaarheid van SQL Server database engine-architectuur. Er zijn drie soorten Servicelagen die in Azure SQL Database, elk met verschillende architectuurmodellen worden gebruikt. Deze service-lagen zijn:

- Algemeen doel
- Bedrijfskritiek
- Hyperscale

De architectuur voor de servicelaag voor algemeen gebruik-is gebaseerd op een scheiding van berekening en opslag. Deze architectuur, is afhankelijk van hoge beschikbaarheid en betrouwbaarheid van Azure Blob-opslag die transparant worden gerepliceerd databasebestanden en zonder verlies van gegevens garandeert als de onderliggende infrastructuur fout gebeurt.

De volgende afbeelding ziet vier knooppunten in de standard-architectuur met de gescheiden reken- en storage-lagen.

![Scheiding van berekening en opslag](media/sql-database-managed-instance/general-purpose-service-tier.png)

In de architectuur voor de servicelaag voor algemeen gebruik-zijn er twee lagen:

- Een stateless compute-laag met de `sqlserver.exe` verwerken en bevat alleen tijdelijke en in de cache opgeslagen gegevens (bijvoorbeeld: plancache, buffergroep, kolom-archief van toepassingen). Deze stateless SQL Server-knooppunt wordt beheerd door Azure Service Fabric dat proces initialiseert, bepaalt de status van het knooppunt en failover naar een andere locatie uitvoert, indien nodig.
- Een stateful gegevenslaag met databasebestanden (.mdf/.ldf) die zijn opgeslagen in Azure Blob-opslag. Azure Blob-opslag zorgt ervoor dat er worden geen gegevens verloren gaan van een record die in een databasebestand wordt geplaatst. Azure Storage heeft ingebouwde beschikbaarheid/redundantie die ervoor zorgt dat elke record in een logboekbestand of de pagina in bestand behouden blijft, zelfs als SQL Server-proces vastloopt.

Wanneer de upgrade van database-engine of -besturingssysteem wordt uitgevoerd, een deel van de onderliggende infrastructuur is mislukt of als een belangrijk probleem wordt gedetecteerd in SQL Server-proces, Azure Service Fabric stateless SQL Server-proces wordt verplaatst naar een andere staatloze compute-knooppunt. Er is een set van ongebruikte knooppunten die wacht op nieuwe compute-service wordt uitgevoerd als een failover van het primaire knooppunt gebeurt om het aantal failover-tijd. Gegevens in Azure storage-laag wordt niet beïnvloed en gegevens/logboekbestanden zijn gekoppeld aan de nieuwe geïnitialiseerde SQL Server-proces. Dit proces garandeert 99,99% beschikbaarheid, maar dat er bepaalde prestatie-invloeden van zware werkbelasting die wordt uitgevoerd vanwege enige tijd en de nieuwe SQL Server-knooppunt van het feit begint met cold cache.

## <a name="when-to-choose-this-service-tier"></a>Wanneer deze servicelaag kiezen

Categorie voor algemeen gebruik-service is een standaard service-laag in Azure SQL Database die is ontworpen voor het merendeel van de algemene werkbelastingen. Als u een volledig beheerde database-engine met 99,99% SLA met een opslaglatentie tussen 5 en 10 ms die overeenkomen met de Azure SQL IaaS in de meeste gevallen moet, is de categorie Algemeen gebruik de optie voor u.

## <a name="next-steps"></a>Volgende stappen

- Resource-kenmerken (aantal kernen, i/o, geheugen) van algemeen doel/Standard-laag in [Managed Instance](sql-database-managed-instance-resource-limits.md#service-tier-characteristics), één database in [vCore-model](sql-database-vcore-resource-limits-single-databases.md#general-purpose-service-tier-storage-sizes-and-compute-sizes) of [DTU-model](sql-database-dtu-resource-limits-single-databases.md#single-database-storage-sizes-and-compute-sizes), of Elastische pool in [vCore-model](sql-database-vcore-resource-limits-elastic-pools.md#general-purpose-service-tier-storage-sizes-and-compute-sizes) en [DTU-model](sql-database-dtu-resource-limits-elastic-pools.md#standard-elastic-pool-limits).
- Meer informatie over [bedrijfskritiek](sql-database-service-tier-business-critical.md) en [grootschalige](sql-database-service-tier-hyperscale.md) lagen.
- Meer informatie over [Service Fabric](../service-fabric/service-fabric-overview.md).
- Zie voor meer opties voor hoge beschikbaarheid en herstel na noodgevallen [bedrijfscontinuïteit](sql-database-business-continuity.md).
