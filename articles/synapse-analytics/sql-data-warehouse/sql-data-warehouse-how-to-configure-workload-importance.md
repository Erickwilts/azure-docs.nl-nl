---
title: Urgentie van werk belasting voor toegewezen SQL-groep configureren
description: Meer informatie over het instellen van de urgentie van het aanvraag niveau in azure Synapse Analytics.
services: synapse-analytics
author: ronortloff
manager: craigg
ms.service: synapse-analytics
ms.subservice: sql-dw
ms.topic: conceptual
ms.date: 05/15/2020
ms.author: rortloff
ms.reviewer: jrasnick
ms.custom: azure-synapse
ms.openlocfilehash: 067551d198f717dd40995cb8bc3e1345e82f078f
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2020
ms.locfileid: "96461919"
---
# <a name="configure-workload-importance-in-dedicated-sql-pool-for-azure-synapse-analytics"></a>Urgentie van werk belasting in toegewezen SQL-groep voor Azure Synapse Analytics configureren

Als u de urgentie instelt in de toegewezen SQL-groep voor Azure Synapse, kunt u de planning van query's beïnvloeden. Query's met een hogere urgentie worden gepland om te worden uitgevoerd vóór query's met een lagere urgentie. Als u belang rijk aan query's wilt toewijzen, moet u een classificatie voor de werk belasting maken.

## <a name="create-a-workload-classifier-with-importance"></a>Een classificatie van een werk belasting met urgentie maken

Vaak bevat een scenario voor een Data Warehouse gebruikers, in een systeem dat bezet is, die hun query's snel moeten uitvoeren.  De gebruiker kan leidinggevenden zijn van het bedrijf waarvoor rapporten moeten worden uitgevoerd of de gebruiker kan worden uitgevoerd als een adhoc-query. Als u prioriteit wilt toewijzen, maakt u een classificatie van de werk belasting en krijgt u een prioriteits toewijzing.  In de onderstaande voor beelden wordt de  [classificatie syntaxis CREATE workload](/sql/t-sql/statements/create-workload-classifier-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) gebruikt om twee classificaties te maken. `Membername` Dit kan één gebruiker of een groep zijn.  Voer de volgende opdracht uit om bestaande specifieke SQL-groeps gebruikers te zoeken:

```sql
Select name from sys.sysusers
```

Voor het maken van een classificatie van een werk belasting voor een gebruiker met een hogere urgentie:

```sql
CREATE WORKLOAD CLASSIFIER ExecReportsClassifier
    WITH (WORKLOAD_GROUP = 'xlargerc'
         ,MEMBERNAME     = 'name' 
         ,IMPORTANCE     = above_normal);
```

Voor het maken van een classificatie van werk belasting voor een gebruiker die adhoc-query's uitvoert met een lagere urgentie:  

```sql
CREATE WORKLOAD CLASSIFIER AdhocClassifier
    WITH (WORKLOAD_GROUP = 'xlargerc'
         ,MEMBERNAME     = 'name' 
         ,IMPORTANCE     = below_normal);
```

## <a name="next-steps"></a>Volgende stappen

- Zie [workload-classificatie](sql-data-warehouse-workload-classification.md) voor meer informatie over workload Management
- Zie [urgentie van werk belasting](sql-data-warehouse-workload-importance.md) voor meer informatie over belang rijkheid

> [!div class="nextstepaction"]
> [Ga naar de urgentie van het workload beheren en bewaken](sql-data-warehouse-how-to-manage-and-monitor-workload-importance.md)
