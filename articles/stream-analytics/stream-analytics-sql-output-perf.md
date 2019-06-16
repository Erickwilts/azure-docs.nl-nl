---
title: Azure Stream Analytics-uitvoer naar Azure SQL Database
description: Meer informatie over het uitvoeren van gegevens naar SQL Azure en Azure Stream Analytics en bereikt hogere schrijven doorvoersnelheden.
services: stream-analytics
author: chetanmsft
ms.author: chetanmsft
manager: katiiceva
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 3/18/2019
ms.openlocfilehash: 4be73554df0b6bddaafe3910c80c855e127d79f1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60771648"
---
# <a name="azure-stream-analytics-output-to-azure-sql-database"></a>Azure Stream Analytics-uitvoer naar Azure SQL Database

Dit artikel worden besproken tips voor betere prestaties voor schrijven-doorvoer bereiken wanneer u het laden van gegevens in SQL Azure-Database met behulp van Azure Stream Analytics.

SQL-uitvoer in Azure Stream Analytics biedt ondersteuning voor schrijven parallel als een optie. Deze optie [volledig parallelle](stream-analytics-parallelization.md#embarrassingly-parallel-jobs) topologieën, waarbij meerdere partities voor uitvoer naar de doeltabel parallel schrijft taak. Als u deze optie in Azure Stream Analytics inschakelt echter mogelijk niet voldoende voor een hogere doorvoercapaciteit, omdat deze afhankelijk aanzienlijk uw configuratie voor SQL Azure-database en de tabelschema. De keuze van de indexen, clustering sleutel, index-opvulfactor en compressie wordt een invloed hebben op de tijd voor het laden van tabellen. Zie voor meer informatie over het optimaliseren van uw SQL Azure database voor het verbeteren van query en prestaties op basis van interne benchmarks laden [prestatierichtlijnen voor SQL database](../sql-database/sql-database-performance-guidance.md). Ordening van schrijfbewerkingen is niet noodzakelijkerwijs bij het schrijven van parallel met SQL Azure-Database.

Hier volgen enkele configuraties binnen elke service die u kan helpen verbeteren de algehele doorvoer van uw oplossing.

## <a name="azure-stream-analytics"></a>Azure Stream Analytics

- **Partitioneren overnemen** : deze SQL-uitvoer configuratie optie kunnen overnemen van het partitieschema van de vorige querystap of invoer van. Met deze optie is ingeschakeld, schrijven naar een op schijf gebaseerde tabel en hebben een [volledig parallelle](stream-analytics-parallelization.md#embarrassingly-parallel-jobs) topologie voor uw project, verwacht te zien van betere doorvoer. Deze partitioneren al automatisch gebeurt voor tal van andere [levert](stream-analytics-parallelization.md#partitions-in-sources-and-sinks). Bulksgewijs invoegen is gemaakt met deze optie is ook vergrendeling (TABLOCK) van de tabel uitgeschakeld.

> [!NOTE] 
> Wanneer er meer dan 8 invoer partities, overgenomen van de invoer partitieschema mogelijk niet de juiste keuze. Deze limiet is waargenomen in een tabel met een kolom met één identiteit en een geclusterde index. In dit geval kunt u overwegen [INTO](https://docs.microsoft.com/stream-analytics-query/into-azure-stream-analytics#into-shard-count) 8 in de query, het aantal uitvoer schrijvers expliciet opgeven. Op basis van uw schema en de keuze van indexen, kunnen uw opmerkingen variëren.

- **Batchgrootte** -SQL output-configuratie kunt u de maximale batchgrootte opgeven in de uitvoer van een Azure Stream Analytics SQL op basis van de aard van de doel-tabel/werkbelasting. Batchgrootte is het maximum aantal records dat met elke bulksgewijs verzonden invoegen transactie. In de geclusterde columnstore-indexen, batch-grootten rond [100 KB](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-data-loading-guidance) toestaan voor meer parallelle uitvoering, minimale logboekregistratie en optimalisaties vergrendelen. In schijfgebaseerde tabellen, 10K (standaard) of een lagere mogelijk optimaal is voor uw oplossing, zoals hogere batch grootten vergrendelingsescalatie tijdens bulksgewijs invoegen kunnen activeren.

- **Voer bericht afstemmen** : als u bent geoptimaliseerd met behulp van partitionering en batch grootte overnemen, waardoor het aantal invoergebeurtenissen per bericht per partitie zorgt verder pushen van de doorvoer van schrijfbewerkingen. Invoerbericht afstemmen, kunt batch-grootten in Azure Stream Analytics worden tot aan de opgegeven Batch-grootte, waardoor de doorvoer. Dit kan worden bereikt met behulp van [compressie](stream-analytics-define-inputs.md) of een uitbreiding van invoerbericht-grootten die in Event hub- of Blob.

## <a name="sql-azure"></a>SQL Azure

- **Gepartitioneerde tabellen en indexen** – met een [gepartitioneerde](https://docs.microsoft.com/sql/relational-databases/partitions/partitioned-tables-and-indexes?view=sql-server-2017) SQL-tabel en gepartitioneerde indexen voor de tabel met dezelfde kolom als de partitiesleutel (bijvoorbeeld PartitionId) contentions tussen aanzienlijk kunnen verminderen partities tijdens schrijfbewerkingen. Voor een gepartitioneerde tabel moet u maakt een [partitiefunctie](https://docs.microsoft.com/sql/t-sql/statements/create-partition-function-transact-sql?view=sql-server-2017) en een [partitieschema](https://docs.microsoft.com/sql/t-sql/statements/create-partition-scheme-transact-sql?view=sql-server-2017) op de primaire bestandsgroep. Dit wordt ook de beschikbaarheid van de bestaande gegevens verhogen bij het laden van nieuwe gegevens. Logboek i/o-limiet kan worden bereikt op basis van het aantal partities, die kan worden verhoogd met de upgrade voor de SKU.

- **Unieke sleutel schendingen te voorkomen dat** : als u een [meerdere sleutelconflict waarschuwingsberichten](stream-analytics-common-troubleshooting-issues.md#handle-duplicate-records-in-azure-sql-database-output) Zorg ervoor dat de taak wordt niet beïnvloed door de unique-beperking-schendingen die zijn waarschijnlijk, in het activiteitenlogboek voor Azure Stream Analytics tijdens het herstel gevallen. Dit kan worden vermeden door in te stellen de [negeren\_dubbele\_sleutel](stream-analytics-common-troubleshooting-issues.md#handle-duplicate-records-in-azure-sql-database-output) optie op uw indexen.

## <a name="azure-data-factory-and-in-memory-tables"></a>Azure Data Factory en In-Memory-tabellen

- **De tabel in het geheugen als tijdelijke tabel** – [geheugentabellen](/sql/relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization) toestaan voor laden van gegevens met zeer snelle, maar de gegevens moeten passen in het geheugen. Benchmarks show bulksgewijs laden uit een tabel in het geheugen aan een tabel op basis van een schijf is ongeveer 10 keer sneller dan rechtstreeks bulksgewijs invoegen in de tabel op basis van een schijf met een id-kolom en een geclusterde index met behulp van één schrijver. Als u wilt gebruikmaken van de prestaties van deze bulksgewijs invoegen, instellen van een [taak voor het kopiëren met behulp van Azure Data Factory](../data-factory/connector-azure-sql-database.md) die gegevens uit de tabel in het geheugen worden gekopieerd naar de tabel op basis van een schijf.

## <a name="avoiding-performance-pitfalls"></a>Prestatieproblemen te voorkomen
Bulksgewijs invoegen van gegevens is veel sneller dan het laden van gegevens met één ingevoegd omdat de herhaalde overhead van het overdragen van de gegevens, het parseren van de instructie insert, de instructie uitgevoerd en uitgeven van een transactierecord wordt vermeden. In plaats daarvan wordt een efficiëntere pad in de opslag-engine gebruikt voor het streamen van de gegevens. De kosten voor de installatie van dit pad is echter veel hoger dan een enkele insert-instructie in een tabel op basis van een schijf. Het Break-evenpoint is meestal ongeveer 100 rijen, dan welke bulksgewijs laden bijna altijd meer is efficiënte. 

Als de gebeurtenissen frequentie van binnenkomst laag is, dat kan eenvoudig worden gemaakt batch grootten lager is dan 100 rijen, dit maakt bulksgewijs invoegen inefficiënt en maakt gebruik van te veel ruimte op schijf. U kunt deze beperking omzeilen, kunt u een van deze acties doen:
* Maken van een INSTEAD OF [trigger](/sql/t-sql/statements/create-trigger-transact-sql) met eenvoudige invoegen voor elke rij.
* Gebruik een tijdelijke tabel In-Memory zoals beschreven in de vorige sectie.

Een ander dergelijke scenario treedt op bij het schrijven naar een niet-geclusterde columnstore-index (NCCI), waarbij kleinere bulksgewijs invoegen te veel segmenten, die de index kunnen vastlopen kunnen maken. In dit geval is de aanbeveling voor een geclusterde Columnstore-index in plaats daarvan gebruiken.

## <a name="summary"></a>Samenvatting

Kortom, met de functie gepartitioneerde uitvoer in Azure Stream Analytics voor SQL-uitvoer geeft uitgelijnde parallellisering van uw taak met een gepartitioneerde tabel in SQL Azure doorvoer aanzienlijke verbeteringen. Gebruik te maken van Azure Data Factory voor het indelen van verplaatsing van gegevens uit een In-Memory-tabel in schijfgebaseerde tabellen kunt factor doorvoer winsten geven. Zo mogelijk kan verbeteren dichtheid bericht ook worden belangrijke factor bij het verbeteren van de algemene doorvoer.
