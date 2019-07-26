---
title: Statistieken maken, bijwerken-Azure SQL Data Warehouse | Microsoft Docs
description: Aanbevelingen en voor beelden voor het maken en bijwerken van statistieken voor het optimaliseren van query's voor tabellen in Azure SQL Data Warehouse.
services: sql-data-warehouse
author: XiaoyuMSFT
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: development
ms.date: 05/09/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seoapril2019
ms.openlocfilehash: 00643e303b3352ce9ce39e5a27fd8b42246aac51
ms.sourcegitcommit: 75a56915dce1c538dc7a921beb4a5305e79d3c7a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68479169"
---
# <a name="table-statistics-in-azure-sql-data-warehouse"></a>Tabel statistieken in Azure SQL Data Warehouse

Aanbevelingen en voor beelden voor het maken en bijwerken van statistieken voor het optimaliseren van query's voor tabellen in Azure SQL Data Warehouse.

## <a name="why-use-statistics"></a>Waarom statistieken gebruiken?

Hoe meer Azure SQL Data Warehouse weet wat uw gegevens zijn, des te sneller query's kunnen worden uitgevoerd. Wanneer u gegevens in SQL Data Warehouse hebt geladen, is het verzamelen van statistieken voor uw gegevens een van de belangrijkste dingen die u kunt doen om uw query's te optimaliseren. Het queryoptimalisatieprogramma van SQL Data Warehouse zorgt voor optimalisatie op basis van kosten. Hiermee worden de kosten van verschillende query plannen vergeleken en wordt vervolgens het abonnement met de laagste kosten gekozen. In de meeste gevallen kiest u het plan dat het snelst wordt uitgevoerd. Als de Optimizer bijvoorbeeld een schatting maakt van de datum waarop uw query wordt gefilterd, wordt er één regel geretourneerd. Als er wordt geschat dat de geselecteerde datum 1.000.000 rijen retourneert, wordt een ander plan geretourneerd.

## <a name="automatic-creation-of-statistic"></a>Automatisch statistieken maken

Wanneer de optie Data Base-AUTO_CREATE_STATISTICS is ingeschakeld, analyseert SQL Data Warehouse binnenkomende gebruikers query's voor ontbrekende statistieken. Als er statistieken ontbreken, worden in de query Optimizer statistieken gemaakt voor afzonderlijke kolommen in het query-predikaat of de samenvoegings voorwaarde om de kardinaliteit voor het query plan te verbeteren. Automatisch maken van statistieken is momenteel standaard ingeschakeld.

U kunt controleren of voor uw data warehouse AUTO_CREATE_STATISTICS is geconfigureerd door de volgende opdracht uit te voeren:

```sql
SELECT name, is_auto_create_stats_on
FROM sys.databases
```

Als voor uw data warehouse geen AUTO_CREATE_STATISTICS is geconfigureerd, raden we u aan deze eigenschap in te scha kelen door de volgende opdracht uit te voeren:

```sql
ALTER DATABASE <yourdatawarehousename>
SET AUTO_CREATE_STATISTICS ON
```

Met deze instructies wordt het automatisch maken van statistieken geactiveerd:

- SELECTEREN
- INVOEGEN-SELECTEREN
- CTAS
- UPDATE
- DELETE
- UITLEGGEN wanneer een samen voeging of aanwezigheid van een predikaat wordt gedetecteerd

> [!NOTE]
> Het automatisch maken van statistieken wordt niet gemaakt voor tijdelijke of externe tabellen.

Het automatisch maken van statistieken wordt synchroon uitgevoerd, zodat u een slechtere query prestaties kunt opdoen als in uw kolommen statistieken ontbreken. De tijd om statistieken voor één kolom te maken, is afhankelijk van de grootte van de tabel. Om te voor komen dat de prestaties verslechteren, met name bij de benchmarking van de prestaties, moet u ervoor zorgen dat de statistieken zijn gemaakt door de benchmark workload uit te voeren voordat u het systeem profileert.

> [!NOTE]
> Het maken van statistieken wordt geregistreerd in [sys. DM _pdw_exec_requests](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql?view=azure-sqldw-latest) onder een andere gebruikers context.

Wanneer er automatische statistieken worden gemaakt, wordt het volgende gevormd door het formulier: _WA_Sys_< 8-cijferige kolom-id in hexadecimale > _ < tabel-ID in hexadecimale >. U kunt de statistieken weer geven die al zijn gemaakt door de [DBCC SHOW_STATISTICS](/sql/t-sql/database-console-commands/dbcc-show-statistics-transact-sql?view=azure-sqldw-latest) -opdracht uit te voeren:

```sql
DBCC SHOW_STATISTICS (<table_name>, <target>)
```

De table_name is de naam van de tabel die de statistieken bevat die moeten worden weer gegeven. Dit kan geen externe tabel zijn. Het doel is de naam van de doel index, statistieken of kolom waarvoor statistische gegevens moeten worden weer gegeven.

## <a name="updating-statistics"></a>Statistieken bijwerken

Een best practice is het bijwerken van statistieken voor datum kolommen elke dag wanneer er nieuwe datums worden toegevoegd. Telkens wanneer er nieuwe rijen worden geladen in het Data Warehouse, worden er nieuwe belasting-of transactie datums toegevoegd. Deze wijzigen de gegevens distributie en maken de statistieken verouderd. Daarentegen kunnen statistieken voor een kolom land/regio in een tabel klant nooit worden bijgewerkt, omdat de verdeling van waarden doorgaans niet wordt gewijzigd. Ervan uitgaande dat de distributie een constante is tussen klanten, het toevoegen van nieuwe rijen aan de tabel variatie is geen wijziging van de gegevens distributie. Als uw data warehouse echter slechts één land/regio bevat en u gegevens uit een nieuw land/nieuwe regio haalt, waardoor gegevens uit meerdere landen/regio's worden opgeslagen, moet u de statistieken bijwerken in de kolom land/regio.

Hier volgen de aanbevelingen voor het bijwerken van statistieken:

|||
|-|-|
| **Frequentie van updates voor statistieken**  | Conservatief Dagelijks </br> Nadat u uw gegevens hebt geladen of getransformeerd |
| **Steekproeven** |  Minder dan 1.000.000.000 rijen, gebruik standaard steekproef (20 procent). </br> Gebruik meer dan 1.000.000.000 rijen om de steek proef van twee procent te gebruiken. |

Een van de eerste vragen die u kunt stellen wanneer u problemen met een query wilt oplossen, **zijn "zijn de statistieken bijgewerkt?"**

Deze vraag is niet een die kan worden beantwoord door de leeftijd van de gegevens. Een up-to-date statistieken object kan oud zijn als er geen wezenlijke wijzigingen zijn aangebracht in de onderliggende gegevens. Wanneer het aantal rijen ingrijpend is gewijzigd of als er sprake is van een aanmerkelijke wijziging in de verdeling van waarden voor een *kolom, is* het tijd om statistieken bij te werken.

Er is geen dynamische beheer weergave om te bepalen of de gegevens in de tabel zijn gewijzigd sinds de laatste keer dat de statistieken zijn bijgewerkt. Als u de leeftijd van uw statistieken kent, kunt u deel uitmaken van de afbeelding. U kunt de volgende query gebruiken om de laatste keer te bepalen dat uw statistieken op elke tabel zijn bijgewerkt.

> [!NOTE]
> Als er sprake is van een aanmerkelijke wijziging in de verdeling van waarden voor een kolom, moet u de statistieken bijwerken, onafhankelijk van de laatste keer dat ze zijn bijgewerkt.

```sql
SELECT
    sm.[name] AS [schema_name],
    tb.[name] AS [table_name],
    co.[name] AS [stats_column_name],
    st.[name] AS [stats_name],
    STATS_DATE(st.[object_id],st.[stats_id]) AS [stats_last_updated_date]
FROM
    sys.objects ob
    JOIN sys.stats st
        ON  ob.[object_id] = st.[object_id]
    JOIN sys.stats_columns sc
        ON  st.[stats_id] = sc.[stats_id]
        AND st.[object_id] = sc.[object_id]
    JOIN sys.columns co
        ON  sc.[column_id] = co.[column_id]
        AND sc.[object_id] = co.[object_id]
    JOIN sys.types  ty
        ON  co.[user_type_id] = ty.[user_type_id]
    JOIN sys.tables tb
        ON  co.[object_id] = tb.[object_id]
    JOIN sys.schemas sm
        ON  tb.[schema_id] = sm.[schema_id]
WHERE
    st.[user_created] = 1;
```

Voor **datum kolommen** in een Data Warehouse, bijvoorbeeld, moeten updates voor regel matige statistieken doorgaans worden bijgewerkt. Telkens wanneer er nieuwe rijen worden geladen in het Data Warehouse, worden er nieuwe belasting-of transactie datums toegevoegd. Deze wijzigen de gegevens distributie en maken de statistieken verouderd. De statistieken voor de kolom gender in een tabel Klant kunnen echter nooit worden bijgewerkt. Ervan uitgaande dat de distributie een constante is tussen klanten, het toevoegen van nieuwe rijen aan de tabel variatie is geen wijziging van de gegevens distributie. Als uw data warehouse echter slechts één gender bevat en een nieuwe vereiste resulteert in meerdere geslachten, moet u de statistieken bijwerken in de kolom gender.

Zie algemene richt lijnen voor [Statistieken](/sql/relational-databases/statistics/statistics)voor meer informatie.

## <a name="implementing-statistics-management"></a>Statistieken beheer implementeren

Het is vaak een goed idee om het proces voor het laden van gegevens uit te breiden om ervoor te zorgen dat de statistieken aan het einde van de belasting worden bijgewerkt. Het laden van gegevens is het vaak wijzigen van de grootte van de tabellen en/of de verdeling van waarden. Daarom is dit een logische plaats voor het implementeren van een aantal beheer processen.

De volgende richt lijnen zijn van toepassing op het bijwerken van uw statistieken tijdens het laad proces:

* Zorg ervoor dat voor elke geladen tabel ten minste één statistiek object is bijgewerkt. Hiermee wordt de informatie over de tabel grootte (aantal rijen en aantal pagina's) bijgewerkt als onderdeel van de statistieken-update.
* Focus op kolommen die deel uitmaken van de componenten samen voegen, groeperen op, ORDER BY en DISTINCT.
* Overweeg om kolommen met een oplopende sleutel, zoals transactie datums, vaker bij te werken, omdat deze waarden niet in het statistieken histogram worden opgenomen.
* Overweeg om kolommen met statische distributie minder vaak bij te werken.
* Houd er rekening mee dat elk statistiek object in volg orde wordt bijgewerkt. Eenvoudig implementeren `UPDATE STATISTICS <TABLE_NAME>` is niet altijd ideaal, met name voor Wide tables met veel statistieken objecten.

Zie voor meer informatie de [schatting](/sql/relational-databases/performance/cardinality-estimation-sql-server)van kardinaliteit.

## <a name="examples-create-statistics"></a>Voorbeelden: Statistieken maken

Deze voor beelden laten zien hoe u verschillende opties kunt gebruiken om statistieken te maken. De opties die u voor elke kolom gebruikt, zijn afhankelijk van de kenmerken van uw gegevens en de manier waarop de kolom wordt gebruikt in query's.

### <a name="create-single-column-statistics-with-default-options"></a>Statistieken met één kolom maken met standaard opties

Als u statistieken wilt maken voor een kolom, geeft u een naam op voor het statistiek object en de naam van de kolom.

Deze syntaxis maakt gebruik van alle standaard opties. Standaard worden SQL Data Warehouse monsters van **20 procent** van de tabel bij het maken van statistieken.

```sql
CREATE STATISTICS [statistics_name] ON [schema_name].[table_name]([column_name]);
```

Bijvoorbeeld:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1);
```

### <a name="create-single-column-statistics-by-examining-every-row"></a>Statistieken voor één kolom maken door elke rij te controleren

Het standaard sampling-percentage van 20 procent is voldoende voor de meeste situaties. U kunt de sampling frequentie echter aanpassen.

Gebruik de volgende syntaxis om een voor beeld te hebben van de volledige tabel:

```sql
CREATE STATISTICS [statistics_name] ON [schema_name].[table_name]([column_name]) WITH FULLSCAN;
```

Bijvoorbeeld:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1) WITH FULLSCAN;
```

### <a name="create-single-column-statistics-by-specifying-the-sample-size"></a>Statistieken met één kolom maken door de steekproef grootte op te geven

U kunt de steekproef grootte ook opgeven als een percentage:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1) WITH SAMPLE = 50 PERCENT;
```

### <a name="create-single-column-statistics-on-only-some-of-the-rows"></a>Statistieken voor één kolom maken op enkele rijen

U kunt ook statistieken maken voor een deel van de rijen in uw tabel. Dit wordt een gefilterde statistiek genoemd.

U kunt bijvoorbeeld gefilterde statistieken gebruiken wanneer u van plan bent om een specifieke partitie van een grote gepartitioneerde tabel op te vragen. Door alleen statistieken te maken op basis van de partitie waarden, wordt de nauw keurigheid van de statistieken verbeterd, waardoor de query prestaties worden verbeterd.

In dit voor beeld worden statistieken gemaakt voor een reeks waarden. De waarden kunnen eenvoudig worden gedefinieerd, zodat ze overeenkomen met het bereik van de waarden in een partitie.

```sql
CREATE STATISTICS stats_col1 ON table1(col1) WHERE col1 > '2000101' AND col1 < '20001231';
```

> [!NOTE]
> Voor het query optimalisatie programma om te overwegen gefilterde statistieken te gebruiken bij het kiezen van het gedistribueerde query plan, moet de query binnen de definitie van het statistiek object passen. In het vorige voor beeld moet de WHERE-component van de query Kol1 waarden opgeven tussen 2000101 en 20001231.

### <a name="create-single-column-statistics-with-all-the-options"></a>Statistieken met één kolom maken met alle opties

U kunt de opties ook samen combi neren. In het volgende voor beeld wordt een gefilterd statistiek object gemaakt met een aangepaste steekproef grootte:

```sql
CREATE STATISTICS stats_col1 ON table1 (col1) WHERE col1 > '2000101' AND col1 < '20001231' WITH SAMPLE = 50 PERCENT;
```

Zie [Statistieken maken](/sql/t-sql/statements/create-statistics-transact-sql)voor de volledige verwijzing.

### <a name="create-multi-column-statistics"></a>Statistieken over meerdere kolommen maken

Als u een statistieken object met meerdere kolommen wilt maken, gebruikt u gewoon de voor gaande voor beelden, maar geeft u meer kolommen op.

> [!NOTE]
> Het histogram dat wordt gebruikt om het aantal rijen in het query resultaat te schatten, is alleen beschikbaar voor de eerste kolom die wordt vermeld in de definitie van het statistieken-object.

In dit voor beeld is het histogram voor *de\_product categorie*. Kruis kolom statistieken worden berekend voor *product\_categorie* -en *product\_-sub_category*:

```sql
CREATE STATISTICS stats_2cols ON table1 (product_category, product_sub_category) WHERE product_category > '2000101' AND product_category < '20001231' WITH SAMPLE = 50 PERCENT;
```

Omdat er een correlatie bestaat tussen *product\_categorie* en *product\_\_subcategorie*, kan een statistieken object met meerdere kolommen nuttig zijn als deze kolommen tegelijkertijd worden gebruikt.

### <a name="create-statistics-on-all-columns-in-a-table"></a>Statistieken maken voor alle kolommen in een tabel

Een manier om statistieken te maken, is door opdrachten voor het maken van statistieken op te geven nadat u de tabel hebt gemaakt:

```sql
CREATE TABLE dbo.table1
(
   col1 int
,  col2 int
,  col3 int
)
WITH
  (
    CLUSTERED COLUMNSTORE INDEX
  )
;

CREATE STATISTICS stats_col1 on dbo.table1 (col1);
CREATE STATISTICS stats_col2 on dbo.table2 (col2);
CREATE STATISTICS stats_col3 on dbo.table3 (col3);
```

### <a name="use-a-stored-procedure-to-create-statistics-on-all-columns-in-a-database"></a>Een opgeslagen procedure gebruiken om statistieken te maken voor alle kolommen in een Data Base

SQL Data Warehouse heeft geen opgeslagen systeem procedure die gelijk is aan sp_create_stats in SQL Server. Met deze opgeslagen procedure maakt u één kolom statistieken-object op elke kolom van de data base waarvoor nog geen statistieken zijn.

Het volgende voor beeld helpt u aan de slag te gaan met uw database ontwerp. U kunt deze aanpassen aan uw behoeften:

```sql
CREATE PROCEDURE    [dbo].[prc_sqldw_create_stats]
(   @create_type    tinyint -- 1 default 2 Fullscan 3 Sample
,   @sample_pct     tinyint
)
AS

IF @create_type IS NULL
BEGIN
    SET @create_type = 1;
END;

IF @create_type NOT IN (1,2,3)
BEGIN
    THROW 151000,'Invalid value for @stats_type parameter. Valid range 1 (default), 2 (fullscan) or 3 (sample).',1;
END;

IF @sample_pct IS NULL
BEGIN;
    SET @sample_pct = 20;
END;

IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN;
    DROP TABLE #stats_ddl;
END;

CREATE TABLE #stats_ddl
WITH    (   DISTRIBUTION    = HASH([seq_nmbr])
        ,   LOCATION        = USER_DB
        )
AS
WITH T
AS
(
SELECT      t.[name]                        AS [table_name]
,           s.[name]                        AS [table_schema_name]
,           c.[name]                        AS [column_name]
,           c.[column_id]                   AS [column_id]
,           t.[object_id]                   AS [object_id]
,           ROW_NUMBER()
            OVER(ORDER BY (SELECT NULL))    AS [seq_nmbr]
FROM        sys.[tables] t
JOIN        sys.[schemas] s         ON  t.[schema_id]       = s.[schema_id]
JOIN        sys.[columns] c         ON  t.[object_id]       = c.[object_id]
LEFT JOIN   sys.[stats_columns] l   ON  l.[object_id]       = c.[object_id]
                                    AND l.[column_id]       = c.[column_id]
                                    AND l.[stats_column_id] = 1
LEFT JOIN    sys.[external_tables] e    ON    e.[object_id]        = t.[object_id]
WHERE       l.[object_id] IS NULL
AND            e.[object_id] IS NULL -- not an external table
)
SELECT  [table_schema_name]
,       [table_name]
,       [column_name]
,       [column_id]
,       [object_id]
,       [seq_nmbr]
,       CASE @create_type
        WHEN 1
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+')' AS VARCHAR(8000))
        WHEN 2
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+') WITH FULLSCAN' AS VARCHAR(8000))
        WHEN 3
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+') WITH SAMPLE '+CONVERT(varchar(4),@sample_pct)+' PERCENT' AS VARCHAR(8000))
        END AS create_stat_ddl
FROM T
;

DECLARE @i INT              = 1
,       @t INT              = (SELECT COUNT(*) FROM #stats_ddl)
,       @s NVARCHAR(4000)   = N''
;

WHILE @i <= @t
BEGIN
    SET @s=(SELECT create_stat_ddl FROM #stats_ddl WHERE seq_nmbr = @i);

    PRINT @s
    EXEC sp_executesql @s
    SET @i+=1;
END

DROP TABLE #stats_ddl;
```

Voer de opgeslagen procedure uit om statistieken te maken voor alle kolommen in de tabel met behulp van de standaard waarden.

```sql
EXEC [dbo].[prc_sqldw_create_stats] 1, NULL;
```

Als u statistieken wilt maken voor alle kolommen in de tabel met behulp van een fullscan, roept u deze procedure aan:

```sql
EXEC [dbo].[prc_sqldw_create_stats] 2, NULL;
```

Als u voorbeeld statistieken wilt maken voor alle kolommen in de tabel, typt u 3 en het steek proef percentage. Deze procedure maakt gebruik van een sample frequentie van 20 procent.

```sql
EXEC [dbo].[prc_sqldw_create_stats] 3, 20;
```

Voorbeeld statistieken maken voor alle kolommen

## <a name="examples-update-statistics"></a>Voorbeelden: Statistieken bijwerken

Als u statistieken wilt bijwerken, kunt u het volgende doen:

- Eén statistiek object bijwerken. Geef de naam op van het statistiek object dat u wilt bijwerken.
- Alle statistieken-objecten in een tabel bijwerken. Geef de naam van de tabel op in plaats van één specifiek statistiek object.

### <a name="update-one-specific-statistics-object"></a>Een specifiek statistieken object bijwerken

Gebruik de volgende syntaxis om een specifiek statistiek object bij te werken:

```sql
UPDATE STATISTICS [schema_name].[table_name]([stat_name]);
```

Bijvoorbeeld:

```sql
UPDATE STATISTICS [dbo].[table1] ([stats_col1]);
```

Door specifieke statistieken objecten bij te werken, kunt u de tijd en resources die nodig zijn om statistieken te beheren, minimaliseren. Hiervoor moet u de beste statistieken objecten kiezen om bij te werken.

### <a name="update-all-statistics-on-a-table"></a>Alle statistieken voor een tabel bijwerken

Een eenvoudige methode voor het bijwerken van alle statistieken objecten in een tabel is:

```sql
UPDATE STATISTICS [schema_name].[table_name];
```

Bijvoorbeeld:

```sql
UPDATE STATISTICS dbo.table1;
```

De instructie UPDATE STATISTICs is eenvoudig te gebruiken. Houd er rekening mee dat *alle* statistieken in de tabel worden bijgewerkt en dat daarom meer werk kan worden uitgevoerd dan nodig is. Als de prestaties geen probleem zijn, is dit de eenvoudigste en meest volledige manier om te garanderen dat statistieken up-to-date zijn.

> [!NOTE]
> Bij het bijwerken van alle statistieken voor een tabel, SQL Data Warehouse een voor beeld van de tabel voor elk statistiek object wordt gecontroleerd. Als de tabel groot is en veel kolommen en veel statistieken heeft, kan het efficiënter zijn om afzonderlijke statistieken bij te werken op basis van de behoefte.

Zie [tijdelijke tabellen](sql-data-warehouse-tables-temporary.md)voor een `UPDATE STATISTICS` implementatie van een procedure. De implementatie methode wijkt enigszins af van de voor `CREATE STATISTICS` gaande procedure, maar het resultaat is hetzelfde.

Zie [Statistieken bijwerken](/sql/t-sql/statements/update-statistics-transact-sql)voor de volledige syntaxis.

## <a name="statistics-metadata"></a>Statistieken meta gegevens

Er zijn verschillende systeem weergaven en-functies die u kunt gebruiken om informatie te vinden over statistieken. U kunt bijvoorbeeld zien of een statistiek object verouderd is door gebruik te maken van de functie voor statistieken-datum om te zien wanneer de statistieken voor het laatst zijn gemaakt of bijgewerkt.

### <a name="catalog-views-for-statistics"></a>Catalogus weergaven voor statistieken

Deze systeem weergaven bieden informatie over statistieken:

| Catalogus weergave | Description |
|:--- |:--- |
| [sys.columns](/sql/relational-databases/system-catalog-views/sys-columns-transact-sql) |Eén rij voor elke kolom. |
| [sys.objects](/sql/relational-databases/system-catalog-views/sys-objects-transact-sql) |Eén rij voor elk object in de data base. |
| [sys.schemas](/sql/relational-databases/system-catalog-views/sys-objects-transact-sql) |Eén rij voor elk schema in de data base. |
| [sys. stats](/sql/relational-databases/system-catalog-views/sys-stats-transact-sql) |Eén rij voor elk statistiek object. |
| [sys.stats_columns](/sql/relational-databases/system-catalog-views/sys-stats-columns-transact-sql) |Eén rij voor elke kolom in het statistiek object. Koppelingen terug naar sys. Columns. |
| [sys.tables](/sql/relational-databases/system-catalog-views/sys-tables-transact-sql) |Eén rij voor elke tabel (inclusief externe tabellen). |
| [sys.table_types](/sql/relational-databases/system-catalog-views/sys-table-types-transact-sql) |Eén rij voor elk gegevens type. |

### <a name="system-functions-for-statistics"></a>Systeem functies voor statistieken

Deze systeem functies zijn handig voor het werken met statistieken:

| Systeem functie | Description |
|:--- |:--- |
| [STATS_DATE](/sql/t-sql/functions/stats-date-transact-sql) |Datum waarop het statistieken object voor het laatst is bijgewerkt. |
| [DBCC SHOW_STATISTICS](/sql/t-sql/database-console-commands/dbcc-show-statistics-transact-sql) |Overzichts niveau en gedetailleerde informatie over de distributie van waarden, zoals begrepen door het statistiek object. |

### <a name="combine-statistics-columns-and-functions-into-one-view"></a>Statistische kolommen en-functies combi neren in één weer gave

Met deze weer gave worden kolommen met betrekking tot statistieken en resultaten van de functie STATS_DATE () gecombineerd.

```sql
CREATE VIEW dbo.vstats_columns
AS
SELECT
        sm.[name]                           AS [schema_name]
,       tb.[name]                           AS [table_name]
,       st.[name]                           AS [stats_name]
,       st.[filter_definition]              AS [stats_filter_definition]
,       st.[has_filter]                     AS [stats_is_filtered]
,       STATS_DATE(st.[object_id],st.[stats_id])
                                            AS [stats_last_updated_date]
,       co.[name]                           AS [stats_column_name]
,       ty.[name]                           AS [column_type]
,       co.[max_length]                     AS [column_max_length]
,       co.[precision]                      AS [column_precision]
,       co.[scale]                          AS [column_scale]
,       co.[is_nullable]                    AS [column_is_nullable]
,       co.[collation_name]                 AS [column_collation_name]
,       QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])
                                            AS two_part_name
,       QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])
                                            AS three_part_name
FROM    sys.objects                         AS ob
JOIN    sys.stats           AS st ON    ob.[object_id]      = st.[object_id]
JOIN    sys.stats_columns   AS sc ON    st.[stats_id]       = sc.[stats_id]
                            AND         st.[object_id]      = sc.[object_id]
JOIN    sys.columns         AS co ON    sc.[column_id]      = co.[column_id]
                            AND         sc.[object_id]      = co.[object_id]
JOIN    sys.types           AS ty ON    co.[user_type_id]   = ty.[user_type_id]
JOIN    sys.tables          AS tb ON  co.[object_id]        = tb.[object_id]
JOIN    sys.schemas         AS sm ON  tb.[schema_id]        = sm.[schema_id]
WHERE   1=1
AND     st.[user_created] = 1
;
```

## <a name="dbcc-showstatistics-examples"></a>DBCC SHOW_STATISTICS ()-voor beelden

DBCC SHOW_STATISTICS () toont de gegevens binnen een statistiek object. Deze gegevens zijn afkomstig uit drie delen:

- Header
- Dichtheids vector
- Histogram

De meta gegevens van de koptekst over de statistieken. In het histogram wordt de verdeling weer gegeven van de waarden in de eerste sleutel kolom van het statistiek object. De dichtheids vector meet de correlatie tussen kolommen. SQL Data Warehouse de kardinaliteit met een van de gegevens in het statistiek object berekent.

### <a name="show-header-density-and-histogram"></a>Koptekst, densiteit en histogram weer geven

In dit eenvoudige voor beeld worden alle drie delen van een statistiek object weer gegeven:

```sql
DBCC SHOW_STATISTICS([<schema_name>.<table_name>],<stats_name>)
```

Bijvoorbeeld:

```sql
DBCC SHOW_STATISTICS (dbo.table1, stats_col1);
```

### <a name="show-one-or-more-parts-of-dbcc-showstatistics"></a>Een of meer delen van DBCC SHOW_STATISTICS () weer geven

Als u alleen specifieke onderdelen wilt weer geven, gebruikt u de `WITH` -component en geeft u op welke onderdelen u wilt zien:

```sql
DBCC SHOW_STATISTICS([<schema_name>.<table_name>],<stats_name>) WITH stat_header, histogram, density_vector
```

Bijvoorbeeld:

```sql
DBCC SHOW_STATISTICS (dbo.table1, stats_col1) WITH histogram, density_vector
```

## <a name="dbcc-showstatistics-differences"></a>DBCC SHOW_STATISTICS () verschillen

DBCC SHOW_STATISTICS () is strikt geïmplementeerd in SQL Data Warehouse vergeleken met SQL Server:

- Niet-gedocumenteerde functies worden niet ondersteund.
- Kan Stats_stream niet gebruiken.
- Kan geen resultaten voor specifieke subsets met statistische gegevens samen voegen. Bijvoorbeeld STAT_HEADER toevoegen aan DENSITY_VECTOR.
- NO_INFOMSGS kan niet worden ingesteld voor het onderdrukken van berichten.
- De namen van de vier Kante haken rondom statistieken kunnen niet worden gebruikt.
- Kan geen kolom namen gebruiken om statistieken objecten te identificeren.
- Aangepaste fout 2767 wordt niet ondersteund.

## <a name="next-steps"></a>Volgende stappen

Zie [uw werk belasting controleren](sql-data-warehouse-manage-monitor.md) voor meer informatie over het verbeteren van de prestaties van query's
