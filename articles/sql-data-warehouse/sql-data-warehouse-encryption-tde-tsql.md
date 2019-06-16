---
title: Transparante gegevensversleuteling in SQL datawarehouse (T-SQL) | Microsoft Docs
description: Transparante gegevensversleuteling (TDE) in SQL datawarehouse (T-SQL)
services: sql-data-warehouse
author: KavithaJonnakuti
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: security
ms.date: 04/30/2019
ms.author: kavithaj
ms.reviewer: rortloff
ms.openlocfilehash: 1d62bc5cb080f2167f13bd58e9a8a334b8ae5163
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65146071"
---
# <a name="get-started-with-transparent-data-encryption-tde"></a>Aan de slag met transparante gegevensversleuteling (TDE)
> [!div class="op_single_selector"]
> * [Beveiligingsoverzicht](sql-data-warehouse-overview-manage-security.md)
> * [Verificatie](sql-data-warehouse-authentication.md)
> * [Versleuteling (Portal)](sql-data-warehouse-encryption-tde.md)
> * [Encryption (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

## <a name="required-permissions"></a>Vereiste machtigingen
Om in te schakelen transparante gegevensversleuteling (TDE), moet u een beheerder of lid zijn van de rol dbmanager.

## <a name="enabling-encryption"></a>Versleuteling is ingeschakeld
Volg deze stappen voor TDE inschakelen voor een SQL Data Warehouse:

1. Verbinding maken met de *master* database op de server die als host fungeert voor de database met behulp van een aanmelding die een beheerder of lid zijn van de **dbmanager** rol in de hoofddatabase
2. Voer de volgende instructie voor het versleutelen van de database uit.

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION ON;
```

## <a name="disabling-encryption"></a>Als u versleuteling uitschakelt
Volg deze stappen voor TDE uitschakelen voor een SQL Data Warehouse:

1. Verbinding maken met de *master* database met behulp van een aanmelding die een beheerder of lid zijn van de **dbmanager** rol in de hoofddatabase
2. Voer de volgende instructie voor het versleutelen van de database uit.

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION OFF;
```

> [!NOTE]
> Een onderbroken SQL Data Warehouse moet voordat u wijzigingen in de TDE-instellingen worden hervat.
> 
> 

## <a name="verifying-encryption"></a>Controleren van versleuteling
Om te controleren of de versleutelingsstatus van de voor een SQL Data Warehouse, de volgende stappen uit te voeren:

1. Verbinding maken met de *master* of exemplaar in de database met behulp van een aanmelding die een beheerder of lid zijn van de **dbmanager** rol in de hoofddatabase
2. Voer de volgende instructie voor het versleutelen van de database uit.

```sql
SELECT
    [name],
    [is_encrypted]
FROM
    sys.databases;
```

Een resultaat van ```1``` geeft aan dat een versleutelde database ```0``` geeft aan dat een niet-versleutelde database.

## <a name="encryption-dmvs"></a>Versleuteling DMV 's
* [sys.databases][sys.databases] 
* [sys.dm_pdw_nodes_database_encryption_keys][sys.dm_pdw_nodes_database_encryption_keys]

<!--Anchors-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[sys.databases]: https://msdn.microsoft.com/library/ms178534.aspx  
[sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx  

<!--Image references-->

<!--Link references-->
