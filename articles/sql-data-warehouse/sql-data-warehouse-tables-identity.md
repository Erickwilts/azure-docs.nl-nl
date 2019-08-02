---
title: IDENTITEIT gebruiken om surrogaat sleutels te maken-Azure SQL Data Warehouse | Microsoft Docs
description: Aanbevelingen en voor beelden voor het gebruik van de IDENTITEITs eigenschap voor het maken van surrogaat sleutels voor tabellen in Azure SQL Data Warehouse.
services: sql-data-warehouse
author: XiaoyuMSFT
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: development
ms.date: 04/30/2019
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: 4c65bf7cc8edfa246508bb22001aed40c34414f3
ms.sourcegitcommit: f5cc71cbb9969c681a991aa4a39f1120571a6c2e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68515593"
---
# <a name="using-identity-to-create-surrogate-keys-in-azure-sql-data-warehouse"></a>IDENTITEIT gebruiken om surrogaat sleutels te maken in Azure SQL Data Warehouse

Aanbevelingen en voor beelden voor het gebruik van de IDENTITEITs eigenschap voor het maken van surrogaat sleutels voor tabellen in Azure SQL Data Warehouse.

## <a name="what-is-a-surrogate-key"></a>Wat is een surrogaat sleutel?

Een surrogaat sleutel in een tabel is een kolom met een unieke id voor elke rij. De sleutel wordt niet gegenereerd op basis van de tabel gegevens. Gegevens modelers zoals het maken van surrogaat sleutels in hun tabellen wanneer ze data warehouse modellen ontwerpen. U kunt met behulp van de IDENTITEITs eigenschap dit doel eenvoudig en effectief belasten zonder de belasting prestaties te beïnvloeden.  

## <a name="creating-a-table-with-an-identity-column"></a>Een tabel maken met een IDENTITEITs kolom

De IDENTITEITs eigenschap is ontworpen om uit te breiden over alle distributies in het Data Warehouse zonder de belasting prestaties te beïnvloeden. Daarom is de implementatie van de identiteit gericht op het bereiken van deze doel stellingen.

U kunt een tabel definiëren met de IDENTITEITs eigenschap wanneer u de tabel voor het eerst maakt met behulp van syntaxis die vergelijkbaar is met de volgende instructie:

```sql
CREATE TABLE dbo.T1
(   C1 INT IDENTITY(1,1) NOT NULL
,   C2 INT NULL
)
WITH
(   DISTRIBUTION = HASH(C2)
,   CLUSTERED COLUMNSTORE INDEX
)
;
```

U kunt vervolgens gebruiken `INSERT..SELECT` om de tabel in te vullen.

In dit rest gedeelte van deze sectie worden de nuances van de implementatie gemarkeerd, zodat u ze beter kunt begrijpen.  

### <a name="allocation-of-values"></a>Toewijzing van waarden

De IDENTITEITs eigenschap biedt geen garantie voor de volg orde waarin de surrogaat waarden worden toegewezen, die het gedrag van SQL Server en Azure SQL Database weer spie gelen. In Azure SQL Data Warehouse is de afwezigheid van een garantie echter meer uitgesp roken.

Het volgende voor beeld is een illustratie:

```sql
CREATE TABLE dbo.T1
(   C1 INT IDENTITY(1,1)    NOT NULL
,   C2 VARCHAR(30)              NULL
)
WITH
(   DISTRIBUTION = HASH(C2)
,   CLUSTERED COLUMNSTORE INDEX
)
;

INSERT INTO dbo.T1
VALUES (NULL);

INSERT INTO dbo.T1
VALUES (NULL);

SELECT *
FROM dbo.T1;

DBCC PDW_SHOWSPACEUSED('dbo.T1');
```

In het voor gaande voor beeld zijn er twee rijen gespreid over distributie 1. De eerste rij heeft de vervangende waarde 1 in kolom `C1`en de tweede rij heeft de vervangende waarde van 61. Beide waarden zijn gegenereerd door de IDENTITEITs eigenschap. De toewijzing van de waarden is echter niet aaneengesloten. Dit gedrag is standaard.

### <a name="skewed-data"></a>Scheefe gegevens

Het waarden bereik voor het gegevens type worden gelijkmatig verdeeld over de distributies. Als een gedistribueerde tabel ten koste gaat van scheefe gegevens, kan het bereik van de waarden die beschikbaar zijn voor het gegevens type, voor tijdig worden uitgeput. Als bijvoorbeeld alle gegevens in één distributie worden beëindigd, heeft de tabel in feite toegang tot slechts één Sixtieth van de waarden van het gegevens type. Daarom is de identiteits eigenschap beperkt tot `INT` en `BIGINT` alleen gegevens typen.

### <a name="selectinto"></a>SELECTEREN... SAMEN

Wanneer een bestaande IDENTITEITs kolom is geselecteerd in een nieuwe tabel, neemt de nieuwe kolom de IDENTITEITs eigenschap over, tenzij een van de volgende voor waarden waar is:

- De instructie SELECT bevat een samen voeging.
- Meerdere SELECT-instructies worden gekoppeld met behulp van UNION.
- De kolom IDENTITY wordt meer dan één keer in de selectie lijst vermeld.
- De IDENTITEITs kolom maakt deel uit van een expressie.

Als aan een van deze voor waarden wordt voldaan, wordt de kolom niet NULL gemaakt in plaats van de IDENTITEITs eigenschap over te nemen.

### <a name="create-table-as-select"></a>CREATE TABLE AS SELECT

CREATE TABLE AS SELECT (CTAS) volgt hetzelfde SQL Server gedrag dat is gedocumenteerd voor SELECT.. Samen. U kunt echter geen identiteits eigenschap opgeven in de kolom definitie van het `CREATE TABLE` onderdeel van de-instructie. U kunt de functie Identity ook niet gebruiken in `SELECT` het deel van de CTAS. Als u een tabel wilt vullen, moet u `CREATE TABLE` gebruiken om de tabel te definiëren `INSERT..SELECT` , gevolgd door om deze te vullen.

## <a name="explicitly-inserting-values-into-an-identity-column"></a>Expliciet waarden invoegen in een IDENTITEITs kolom

SQL Data Warehouse ondersteunt `SET IDENTITY_INSERT <your table> ON|OFF` syntaxis. U kunt deze syntaxis gebruiken om expliciet waarden in te voegen in de IDENTITEITs kolom.

Veel gegevens modelers, zoals het gebruik van vooraf gedefinieerde negatieve waarden voor bepaalde rijen in hun dimensies. Een voor beeld is de rij-1 of het onbekende lid.

In het volgende script ziet u hoe u deze rij expliciet kunt toevoegen met behulp van SET IDENTITY_INSERT:

```sql
SET IDENTITY_INSERT dbo.T1 ON;

INSERT INTO dbo.T1
(   C1
,   C2
)
VALUES (-1,'UNKNOWN')
;

SET IDENTITY_INSERT dbo.T1 OFF;

SELECT  *
FROM    dbo.T1
;
```

## <a name="loading-data"></a>Gegevens laden

De aanwezigheid van de IDENTITEITs eigenschap heeft enkele gevolgen voor de code voor het laden van gegevens. In deze sectie worden enkele eenvoudige patronen uitgelegd voor het laden van gegevens in tabellen met behulp van identiteit.

Als u gegevens wilt laden in een tabel en een surrogaat sleutel wilt genereren met behulp van identiteit, maakt u de tabel en gebruikt u vervolgens invoegen.. SELECTEREN of invoegen... WAARDEN om de belasting uit te voeren.

In het volgende voor beeld wordt het basis patroon gemarkeerd:

```sql
--CREATE TABLE with IDENTITY
CREATE TABLE dbo.T1
(   C1 INT IDENTITY(1,1)
,   C2 VARCHAR(30)
)
WITH
(   DISTRIBUTION = HASH(C2)
,   CLUSTERED COLUMNSTORE INDEX
)
;

--Use INSERT..SELECT to populate the table from an external table
INSERT INTO dbo.T1
(C2)
SELECT  C2
FROM    ext.T1
;

SELECT *
FROM   dbo.T1
;

DBCC PDW_SHOWSPACEUSED('dbo.T1');
```

> [!NOTE]
> Het is niet mogelijk om momenteel `CREATE TABLE AS SELECT` te gebruiken bij het laden van gegevens in een tabel met een identiteits kolom.
>

Zie voor meer informatie over het laden van gegevens [ontwerpen extra heren, laden en transformeren (ELT) voor het Azure SQL Data Warehouse](design-elt-data-loading.md) en [het laden van best practices](guidance-for-loading-data.md).

## <a name="system-views"></a>Systeemweergaven

U kunt de catalogus weergave [sys. identity_columns](/sql/relational-databases/system-catalog-views/sys-identity-columns-transact-sql) gebruiken om een kolom te identificeren die de eigenschap Identity heeft.

In dit voor beeld ziet u hoe u sys. identity_column kunt integreren met andere systeem catalogus weergaven, zodat u beter inzicht krijgt in het database schema:

```sql
SELECT  sm.name
,       tb.name
,       co.name
,       CASE WHEN ic.column_id IS NOT NULL
             THEN 1
        ELSE 0
        END AS is_identity
FROM        sys.schemas AS sm
JOIN        sys.tables  AS tb           ON  sm.schema_id = tb.schema_id
JOIN        sys.columns AS co           ON  tb.object_id = co.object_id
LEFT JOIN   sys.identity_columns AS ic  ON  co.object_id = ic.object_id
                                        AND co.column_id = ic.column_id
WHERE   sm.name = 'dbo'
AND     tb.name = 'T1'
;
```

## <a name="limitations"></a>Beperkingen

De IDENTITEITs eigenschap kan niet worden gebruikt:

- Wanneer het kolom gegevens type niet INT of BIGINT is
- Wanneer de kolom ook de distributie sleutel is
- Wanneer de tabel een externe tabel is

De volgende gerelateerde functies worden niet ondersteund in SQL Data Warehouse:

- [IDENTITY()](/sql/t-sql/functions/identity-function-transact-sql)
- [@@IDENTITY](/sql/t-sql/functions/identity-transact-sql)
- [SCOPE_IDENTITY](/sql/t-sql/functions/scope-identity-transact-sql)
- [IDENT_CURRENT](/sql/t-sql/functions/ident-current-transact-sql)
- [IDENT_INCR](/sql/t-sql/functions/ident-incr-transact-sql)
- [IDENT_SEED](/sql/t-sql/functions/ident-seed-transact-sql)

## <a name="common-tasks"></a>Algemene taken

Deze sectie bevat een aantal voorbeeld code die u kunt gebruiken om algemene taken uit te voeren wanneer u werkt met IDENTITEITs kolommen.

Kolom C1 is de identiteit in de volgende taken.

### <a name="find-the-highest-allocated-value-for-a-table"></a>De hoogste toegewezen waarde voor een tabel zoeken

Gebruik de `MAX()` functie om de hoogste toegewezen waarde voor een gedistribueerde tabel te bepalen:

```sql
SELECT MAX(C1)
FROM dbo.T1
```

### <a name="find-the-seed-and-increment-for-the-identity-property"></a>De seeding en toename voor de IDENTITEITs eigenschap zoeken

U kunt de catalogus weergaven gebruiken om de identiteits toename en Seed-configuratie waarden voor een tabel te detecteren met behulp van de volgende query:

```sql
SELECT  sm.name
,       tb.name
,       co.name
,       ic.seed_value
,       ic.increment_value
FROM        sys.schemas AS sm
JOIN        sys.tables  AS tb           ON  sm.schema_id = tb.schema_id
JOIN        sys.columns AS co           ON  tb.object_id = co.object_id
JOIN        sys.identity_columns AS ic  ON  co.object_id = ic.object_id
                                        AND co.column_id = ic.column_id
WHERE   sm.name = 'dbo'
AND     tb.name = 'T1'
;
```

## <a name="next-steps"></a>Volgende stappen

- [Tabeloverzicht](/azure/sql-data-warehouse/sql-data-warehouse-tables-overview)
- [CREATE TABLE (Transact-SQL) IDENTITY (eigenschap)](/sql/t-sql/statements/create-table-transact-sql-identity-property?view=azure-sqldw-latest)
- [DBCC CHECKINDENT](/sql/t-sql/database-console-commands/dbcc-checkident-transact-sql)
