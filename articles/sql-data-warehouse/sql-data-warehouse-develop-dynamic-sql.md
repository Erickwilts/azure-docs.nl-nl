---
title: Met behulp van dynamische SQL in Azure SQL Data Warehouse | Microsoft Docs
description: Tips voor het gebruik van dynamic SQL in Azure SQL Data Warehouse om oplossingen te ontwikkelen.
services: sql-data-warehouse
author: XiaoyuL-Preview
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: query
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: 567637cab1c983992b08f65352ab9a92bd448c5a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65861810"
---
# <a name="dynamic-sql-in-sql-data-warehouse"></a>Dynamic SQL in SQL datawarehouse
Tips voor het gebruik van dynamic SQL in Azure SQL Data Warehouse om oplossingen te ontwikkelen.

## <a name="dynamic-sql-example"></a>Voorbeeld van de dynamische SQL

Bij het ontwikkelen van toepassingscode voor SQL Data Warehouse, moet u mogelijk gebruik van dynamische sql voor flexibele, algemene en modulaire oplossingen leveren. SQL Data Warehouse biedt geen ondersteuning voor blob-gegevenstypen op dit moment. Niet ondersteund door de gegevenstypen blob kan de grootte van de tekenreeksen beperken omdat de blob-gegevenstypen zijn onder andere typen zowel varchar(max) en nvarchar(max). Als u deze typen in de code van uw toepassing met grote tekenreeksen bouwen gebruikt, moet u de code op te splitsen in segmenten en gebruik in plaats daarvan de EXEC-instructie.

Een eenvoudig voorbeeld:

```sql
DECLARE @sql_fragment1 VARCHAR(8000)=' SELECT name '
,       @sql_fragment2 VARCHAR(8000)=' FROM sys.system_views '
,       @sql_fragment3 VARCHAR(8000)=' WHERE name like ''%table%''';

EXEC( @sql_fragment1 + @sql_fragment2 + @sql_fragment3);
```

Als de tekenreeks kort is, kunt u [sp_executesql](/sql/relational-databases/system-stored-procedures/sp-executesql-transact-sql) die normaal werken.

> [!NOTE]
> Instructies die worden uitgevoerd als dynamic SQL, nog steeds zijn onderworpen aan van alle TSQL-validatieregels.
> 
> 

## <a name="next-steps"></a>Volgende stappen
Zie voor meer tips voor ontwikkelaars [overzicht voor ontwikkelaars](sql-data-warehouse-overview-develop.md).

