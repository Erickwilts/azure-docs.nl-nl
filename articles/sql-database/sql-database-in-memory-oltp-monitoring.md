---
title: XTP-In-memory-opslag controleren | Microsoft Docs
description: Schatting en monitor XTP-In-memory-opslag gebruiken, capaciteit; capaciteit fout 41823 oplossen
services: sql-database
ms.service: sql-database
ms.subservice: monitor
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: juliemsft
ms.author: jrasnick
ms.reviewer: genemi
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: 7542e9fa04eb838baca37dbe13f7cdacdfaf041b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "61035719"
---
# <a name="monitor-in-memory-oltp-storage"></a>Monitor In-Memory OLTP-opslag

Bij het gebruik van [In-Memory OLTP](sql-database-in-memory.md), gegevens in tabellen geoptimaliseerd voor geheugen en tabelvariabelen zich bevindt in de In-Memory OLTP-opslag. Elke servicelaag Premium en bedrijfskritiek heeft een maximale grootte van de In-Memory OLTP-opslag. Zie [DTU gebaseerde resourcelimieten - individuele database](sql-database-dtu-resource-limits-single-databases.md), [DTU gebaseerde resourcelimieten - elastische pools](sql-database-dtu-resource-limits-elastic-pools.md),[vCore gebaseerde resourcelimieten - individuele databases](sql-database-vcore-resource-limits-single-databases.md) en [vCore gebaseerde resourcelimieten - elastische pools](sql-database-vcore-resource-limits-elastic-pools.md).

Zodra deze limiet wordt overschreden, invoeg- en bewerkingen kunnen mislukken met fout 41823 voor individuele databases en de fout 41840 voor elastische pools. Op dat moment moet u ofwel gegevens vrij geheugen, of upgraden van de servicelaag of compute van de grootte van uw database te verwijderen.

## <a name="determine-whether-data-fits-within-the-in-memory-oltp-storage-cap"></a>Bepalen of gegevens binnen de limiet van In-Memory OLTP-opslag passen

Bepaal de limieten van de opslag van de verschillende Servicelagen. Zie [DTU gebaseerde resourcelimieten - individuele database](sql-database-dtu-resource-limits-single-databases.md), [DTU gebaseerde resourcelimieten - elastische pools](sql-database-dtu-resource-limits-elastic-pools.md),[vCore gebaseerde resourcelimieten - individuele databases](sql-database-vcore-resource-limits-single-databases.md) en [vCore gebaseerde resourcelimieten - elastische pools](sql-database-vcore-resource-limits-elastic-pools.md).

Schatten van de geheugenvereisten voor een tabel geoptimaliseerd voor geheugen-works voor SQL Server dezelfde manier als dit wordt in Azure SQL Database. Een aantal duren om te controleren dat artikel op [MSDN](https://msdn.microsoft.com/library/dn282389.aspx).

Tabel en variabele tabelrijen, evenals indexen, tellen mee voor de grootte van de maximale gebruiker. Daarnaast moet ALTER TABLE voldoende ruimte voor het maken van een nieuwe versie van de hele tabel en indexen.

## <a name="monitoring-and-alerting"></a>Bewaking en waarschuwingen
U kunt In-memory-opslag gebruiken als een percentage van de opslaglimiet bewaken voor uw compute-grootte in de [Azure-portal](https://portal.azure.com/): 

1. Zoek het gebruik van resources op de blade Database en klik op bewerken.
2. Selecteer de metriek `In-Memory OLTP Storage percentage`.
3. Als u wilt een waarschuwing toevoegen, klikt u op de het gebruik van resources wordt geopend de blade met metrische gegevens, en klik vervolgens op waarschuwing toevoegen.

Of gebruik de volgende query uit om het gebruik van de opslag In-memory weer te geven:

```sql
    SELECT xtp_storage_percent FROM sys.dm_db_resource_stats
```

## <a name="correct-out-of-in-memory-oltp-storage-situations---errors-41823-and-41840"></a>Out-van-In-Memory OLTP-opslagsituaties - fouten 41823 en 41840 corrigeren

Bereikt de opslaglimiet In-Memory OLTP in de databaseresultaten van uw in invoegen, bijwerken, wijzigen en bewerkingen is mislukt met foutbericht 41823 (voor individuele databases) of fout 41840 (voor elastische pools) maken. Beide fouten leiden tot de actieve transactie om af te breken.

Foutberichten 41823 en 41840 geven aan dat de tabellen geoptimaliseerd voor geheugen en de tabelvariabelen voor de in de database of de groep van toepassingen de maximale grootte van de In-Memory OLTP-opslag hebt bereikt.

Deze fout, ofwel oplossen:

* Gegevens verwijderen uit de tabellen geoptimaliseerd voor geheugen, mogelijk offloading van de gegevens naar de traditionele, schijfgebaseerde tabellen. of,
* De servicelaag upgraden naar een met voldoende opslag in het geheugen voor de gegevens die u wilt behouden in tabellen geoptimaliseerd voor geheugen.

> [!NOTE] 
> In zeldzame gevallen kunnen fouten 41823 en 41840 zijn tijdelijke, wat betekent dat er voldoende beschikbare In-Memory OLTP-opslag, en opnieuw wordt geprobeerd de bewerking is geslaagd. Daarom wordt aangeraden beide monitor de totale beschikbare In-Memory OLTP-opslag en als eerste aangetroffen fout 41823 of 41840 opnieuw uit te voeren. Zie voor meer informatie over de logica voor opnieuw proberen, [opsporing van conflicten en logica voor opnieuw proberen met In-Memory OLTP](https://docs.microsoft.com/sql/relational-databases/In-memory-oltp/transactions-with-memory-optimized-tables#conflict-detection-and-retry-logic).

## <a name="next-steps"></a>Volgende stappen
Zie voor richtlijnen voor bewaking, [Azure SQL Database controleren met dynamic management views](sql-database-monitoring-with-dmvs.md).
