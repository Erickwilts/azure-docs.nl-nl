---
title: Wat is een Azure SQL Database single database | Microsoft Docs
description: Meer informatie over individuele database in Azure SQL Database
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 04/08/2019
ms.openlocfilehash: 36b1fb96ac1dd89375588a65063ce729f6ac825d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65794394"
---
# <a name="what-is-a-single-database-in-azure-sql-database"></a>Wat is een individuele database in Azure SQL Database

De Implementatieoptie voor één database maakt u een database in Azure SQL Database met een eigen set resources en wordt beheerd via een SQL Database-server. Met een individuele database, elke database is geïsoleerd van elkaar en draagbaar, elk met een eigen service tier binnen de [DTU gebaseerde aankoopmodel](sql-database-service-tiers-dtu.md) of [vCore gebaseerde aankoopmodel](sql-database-service-tiers-vcore.md) en een gegarandeerde COMPUTE-grootte.

> [!IMPORTANT]
> Een individuele database is een van drie implementatieopties voor Azure SQL Database. De andere twee [elastische pools](sql-database-elastic-pool.md) en [beheerd exemplaar](sql-database-managed-instance.md).
> [!NOTE]
> Zie voor een verklarende woordenlijst van termen in Azure SQL Database, [SQL-Database verklarende woordenlijst](sql-database-glossary-terms.md)

## <a name="dynamic-scalability"></a>Dynamische schaalbaarheid

U kunt uw eerste app ontwikkelen op een kleine, één database op lage kosten in de rekenlaag zonder server (preview) of een kleine rekeninstantie-grootte in de ingerichte Computing-laag. U wijzigt de [compute of de service tier](sql-database-single-database-scale.md) handmatig of via een programma op elk gewenst moment om te voldoen aan de behoeften van uw oplossing. U kunt het prestatieniveau aanpassen zonder uitvaltijd voor uw app of voor uw klanten. Dankzij dynamische schaalbaarheid kan uw database op een transparante manier snel reageren op veranderende resourcevereisten en betaalt u alleen voor de resources die u nodig hebt wanneer u ze nodig.

## <a name="single-databases-and-elastic-pools"></a>Individuele databases en elastische pools

Een individuele database kan worden verplaatst naar of van een [elastische pool](sql-database-elastic-pool.md) voor het delen van de resource. Voor veel bedrijven en toepassingen is het kunnen maken van enkele databases en het naar wens omhoog of omlaag schalen van de prestaties al voldoende, vooral als de gebruikspatronen redelijk voorspelbaar zijn. Bij onvoorspelbare gebruikspatronen kan het echter lastig zijn uw kosten en bedrijfsmodel effectief te beheren. Elastische pools zijn ontworpen om dit probleem te verhelpen. Het concept is eenvoudig. U wijst prestatieresources toe aan een pool in plaats van een individuele database en betaalt voor de collectieve prestatieresources van de pool in plaats van afzonderlijke databaseprestaties.

## <a name="monitoring-and-alerting"></a>Bewaking en waarschuwingen

U gebruikt de ingebouwde [prestatiebewaking](sql-database-performance.md) en [meldingsprogramma's](sql-database-insights-alerts-portal.md), gecombineerd met de prestatiebeoordelingen. Met behulp van deze tools kunt u snel beoordelen wat de impact is van het aanpassen van de schaal op basis van uw huidige prestatiebehoeften of de prestatiebehoeften van uw project. Daarnaast kan SQL Database [metrische gegevens en diagnostische logboeken verzenden](sql-database-metrics-diag-logging.md) die de bewaking vergemakkelijken.

## <a name="availability-capabilities"></a>Beschikbaarheid

Individuele databases, elastische pools en beheerde exemplaren bieden veel beschikbare kenmerken. Zie voor meer informatie, [beschikbare kenmerken](sql-database-technical-overview.md#availability-capabilities).

## <a name="transact-sql-differences"></a>Verschillen Transact-SQL

De meeste Transact-SQL-functies die gebruikmaken van toepassingen worden volledig ondersteund in zowel Microsoft SQL Server en Azure SQL Database. Bijvoorbeeld, werken de belangrijkste SQL-onderdelen, zoals gegevenstypen, operators, string, rekenkundige, logische en functies van de cursor, identiek in SQL Server en SQL-Database. Er zijn echter enkele T-SQL-verschillen in DDL (data definition language) en DML (gegevens manipuleren taal) elementen, wat resulteert in T-SQL-instructies en query's die worden slechts gedeeltelijk ondersteund (die wordt besproken hoe verderop in dit artikel).
Bovendien zijn sommige functies en -syntaxis die niet wordt ondersteund op alle omdat Azure SQL Database is ontworpen om functies van afhankelijkheden in de hoofddatabase en het besturingssysteem te isoleren. De meeste op serverniveau activiteiten zijn daarom niet geschikt zijn voor SQL-Database. T-SQL-instructies en opties zijn niet beschikbaar als het niveau van de server opties, besturingssysteemonderdelen, configureren of de bestandssysteemconfiguratie. Wanneer deze mogelijkheden vereist zijn, is vaak een geschikt alternatief beschikbaar zijn in een andere manier van SQL-Database of een andere Azure-functie of -service.

Zie voor meer informatie, [het omzetten van Transact-SQL-verschillen tijdens de migratie naar SQL Database](sql-database-transact-sql-information.md).

## <a name="security"></a>Beveiliging

SQL Database biedt een scala aan [ingebouwde beveiliging en naleving](sql-database-security-overview.md) functies waarmee uw toepassing te voldoen aan verschillende vereisten voor beveiliging en naleving.

> [!IMPORTANT]
> Azure SQL-Database (alle implementatieopties voor), is gecertificeerd volgens diverse nalevingsstandaarden. Zie voor meer informatie de [Microsoft Azure Trust Center](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942) waar u de meest recente lijst met SQL Database-nalevingscertificeringen kunt vinden.

## <a name="next-steps"></a>Volgende stappen

- Als u wilt snel aan de slag met een individuele database, beginnen met de [Single database snelstartgids guide.md](sql-database-single-database-quickstart-guide.md).
- Zie voor meer informatie over het migreren van een SQL Server-database naar Azure, [migreren naar Azure SQL Database](sql-database-single-database-migrate.md).
- Zie [Functies](sql-database-features.md) voor meer informatie over ondersteunde functies.