---
title: Verschillen in T-SQL-functies in Synapse SQL
description: Lijst met Transact-SQL-functies die beschikbaar zijn in Synapse SQL.
services: synapse analytics
author: jovanpop-msft
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: sql
ms.date: 04/15/2020
ms.author: jovanpop
ms.reviewer: jrasnick
ms.openlocfilehash: f159e38eb66e1758feaf743c32d8de30c614b234
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2020
ms.locfileid: "91288508"
---
# <a name="transact-sql-features-supported-in-azure-synapse-sql"></a>Transact-SQL-functies die worden ondersteund in Azure Synapse SQL

Azure Synapse SQL is een analyseservice voor big data waarmee u uw gegevens kunt opvragen en analyseren met behulp van de taal T-SQL. U kunt voor gegevensanalyse een standaard ANSI-compatibel dialect gebruiken van een SQL-taal die op SQL Server en Azure SQL Database wordt gebruikt. 

De Transact-SQL-taal die in serverloze en ingerichte Synapse SQL-modellen wordt gebruikt, kan verwijzen naar verschillende objecten; de set ondersteunde functies kan onderling enigszins verschillen. Op deze pagina vindt u een overzicht van de belangrijkste verschillen in de Transact-SQL-taal tussen de verbruiksmodellen van Synapse SQL.

## <a name="database-objects"></a>Databaseobjecten

Met verbruiksmodellen in Synapse SQL kunt u verschillende databaseobjecten gebruiken. De vergelijking van ondersteunde objecttypen wordt weergegeven in de volgende tabel:

|   | Ingericht | Serverloos |
| --- | --- | --- |
| **Tabellen** | [Ja](/sql/t-sql/statements/create-table-azure-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) | Nee, een serverloos model kan alleen query's uitvoeren op externe gegevens in [Azure Storage](#storage-options) |
| **Weergaven** | [Ja](/sql/t-sql/statements/create-view-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true). Weergaven kunnen [querytaalelementen](#query-language) gebruiken die beschikbaar zijn in het ingerichte model. | [Ja](/sql/t-sql/statements/create-view-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true). Weergaven kunnen [querytaalelementen](#query-language) gebruiken die beschikbaar zijn in een serverloos model. |
| **Schema's** | [Ja](/sql/t-sql/statements/create-schema-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) | [Ja](/sql/t-sql/statements/create-schema-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) |
| **Tijdelijke tabellen** | [Ja](../sql-data-warehouse/sql-data-warehouse-tables-temporary.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) | Nee |
| **Procedures** | [Ja](/sql/t-sql/statements/create-procedure-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) | Nee |
| **Functies** | [Ja](/sql/t-sql/statements/create-function-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) | Ja, alleen inline tabelfuncties. |
| **Triggers** | Nee | Nee |
| **Externe tabellen** | [Ja](/sql/t-sql/statements/create-external-table-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true). Zie ondersteunde [gegevensindelingen](#data-formats). | [Ja](/sql/t-sql/statements/create-external-table-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true). Zie ondersteunde [gegevensindelingen](#data-formats). |
| **Query's opslaan in de cache** | Ja, meerdere formulieren (op SSD gebaseerde caching, in-memory, caching van resultatensets). Daarnaast wordt Gerealiseerde weergave ondersteund | Nee |
| **Tabelvariabelen** | [Nee](/sql/t-sql/data-types/table-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true), gebruik tijdelijke tabellen | Nee |
| **[Tabeldistributie](../sql-data-warehouse/sql-data-warehouse-tables-distribute.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)**               | Ja | Nee |
| **[Tabelindexen](../sql-data-warehouse/sql-data-warehouse-tables-index.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)**                           | Ja | Nee |
| **[Tabelpartities](../sql-data-warehouse/sql-data-warehouse-tables-partition.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)**                     | Ja | Nee |
| **[Statistieken](develop-tables-statistics.md)**            | Ja | Ja |
| **[Werkbelastingbeheer, resourceklassen en gelijktijdigheidsbeheer](../sql-data-warehouse/resource-classes-for-workload-management.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)** | Ja    | Nee |

## <a name="query-language"></a>Querytaal

Querytalen die in Synapse SQL worden gebruikt, kunnen afhankelijk van het verbruiksmodel verschillende ondersteunde functies hebben. De volgende tabel toont een overzicht van de belangrijkste verschillen in de querytaal in Transact-SQL-dialecten:

|   | Ingericht | Serverloos |
| --- | --- | --- |
| **SELECT-instructie** | Ja. De Transact-SQL-querycomponenten [FOR XML/FOR JSON](/sql/t-sql/queries/select-for-clause-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) en [MATCH](/sql/t-sql/queries/match-sql-graph?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) worden niet ondersteund. | Ja. Transact-SQL-querycomponenten [FOR XML](/sql/t-sql/queries/select-for-clause-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true), [MATCH](/sql/t-sql/queries/match-sql-graph?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true), [PREDICT](/sql/t-sql/queries/predict-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) en queryhints worden niet ondersteund. [OFFSET/FETCH](/sql/t-sql/queries/select-order-by-clause-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest#using-offset-and-fetch-to-limit-the-rows-returned) en [PIVOT/UNPIVOT](/sql/t-sql/queries/from-using-pivot-and-unpivot?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) kunnen worden gebruikt om systeemobjecten (geen externe gegevens) op te vragen. |
| **INSERT-instructie** | Ja | Nee |
| **UPDATE-instructie** | Ja | Nee |
| **DELETE-instructie** | Ja | Nee |
| **MERGE-instructie** | Nee | Nee |
| **[Transacties](develop-transactions.md)** | Ja | Nee |
| **[Labels](develop-label.md)** | Ja | Nee |
| **Gegevens laden** | Ja. Het hulpprogramma dat de voorkeur heeft, is de [COPY](/sql/t-sql/statements/copy-into-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true)-instructie, maar het systeem ondersteunt zowel BULKsgewijs laden (BCP) als [CETAS](/sql/t-sql/statements/create-external-table-as-select-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) voor het laden van gegevens. | Nee |
| **Gegevensexport** | Ja. Gebruik van [CETAS](/sql/t-sql/statements/create-external-table-as-select-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true). | Ja. Gebruik van [CETAS](/sql/t-sql/statements/create-external-table-as-select-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true). |
| **Typen** | Ja, alle Transact-SQL-typen behalve [cursor](/sql/t-sql/data-types/cursor-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true), [hierarchyid](/sql/t-sql/data-types/hierarchyid-data-type-method-reference?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true), [ntext, text en image](/sql/t-sql/data-types/ntext-text-and-image-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true), [rowversion](/sql/t-sql/data-types/rowversion-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true), [ruimtelijke typen](/sql/t-sql/spatial-geometry/spatial-types-geometry-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true), [sql\_variant](/sql/t-sql/data-types/sql-variant-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) en [xml](/sql/t-sql/xml/xml-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) | Ja, alle Transact-SQL-typen behalve [cursor](/sql/t-sql/data-types/cursor-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true), [hierarchyid](/sql/t-sql/data-types/hierarchyid-data-type-method-reference?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true), [ntext, text en image](/sql/t-sql/data-types/ntext-text-and-image-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true), [rowversion](/sql/t-sql/data-types/rowversion-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true), [ruimtelijke typen](/sql/t-sql/spatial-geometry/spatial-types-geometry-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true), [sql\_variant](/sql/t-sql/data-types/sql-variant-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true), [xml](/sql/t-sql/xml/xml-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) en Table-type |
| **Query's tussen databases** | Nee | Ja, met inbegrip van de [USE](/sql/t-sql/language-elements/use-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true)-instructie. |
| **Ingebouwde functies (analyse)** | Ja, alle [analyse-](/sql/t-sql/functions/analytic-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true), conversie-, [datum- en tijd-](/sql/t-sql/functions/date-and-time-data-types-and-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true), logische en [wiskundige](/sql/t-sql/functions/mathematical-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) functies van Transact-SQL-functies, behalve [CHOOSE](/sql/t-sql/functions/logical-functions-choose-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true), [IIF](/sql/t-sql/functions/logical-functions-iif-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) en [PARSE](/sql/t-sql/functions/parse-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) | Ja, alle [analyse-](/sql/t-sql/functions/analytic-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true), conversie-, [datum- en tijd-](/sql/t-sql/functions/date-and-time-data-types-and-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true), logische en [wiskundige](/sql/t-sql/functions/mathematical-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) functies van Transact-SQL. |
| **Ingebouwde functies (tekst)** | Ja. Alle [tekenreeks-](/sql/t-sql/functions/string-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true), [JSON-](/sql/t-sql/functions/json-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) en sorteringsfuncties van Transact-SQL, behalve [STRING_ESCAPE](/sql/t-sql/functions/string-escape-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) en [TRANSLATE](/sql/t-sql/functions/translate-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) | Ja. Alle [tekenreeks-](/sql/t-sql/functions/string-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true), [JSON-](/sql/t-sql/functions/json-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) en sorteringsfuncties van Transact-SQL. |
| **Ingebouwde functies voor tabelwaarden** | Ja, [Transact-SQL-rijensetfuncties](/sql/t-sql/functions/functions?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest#rowset-functions), behalve [OPENXML](/sql/t-sql/functions/openxml-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true), [OPENDATASOURCE](/sql/t-sql/functions/opendatasource-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true), [OPENQUERY](/sql/t-sql/functions/openquery-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) en [OPENROWSET](/sql/t-sql/functions/openrowset-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) | Ja, [Transact-SQL-rijensetfuncties](/sql/t-sql/functions/functions?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest#rowset-functions), behalve [OPENXML](/sql/t-sql/functions/openxml-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true), [OPENDATASOURCE](/sql/t-sql/functions/opendatasource-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) en [OPENQUERY](/sql/t-sql/functions/openquery-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true)  |
| **Aggregaties** |  Ingebouwde aggregaties van Transact-SQL, behalve [CHECKSUM_AGG](/sql/t-sql/functions/checksum-agg-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) en [GROUPING_ID](/sql/t-sql/functions/grouping-id-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) | Ingebouwde aggregaties van Transact-SQL, behalve [STRING\_AGG](/sql/t-sql/functions/string-agg-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) |
| **Operators** | Ja, alle [Transact-SQL-operators](/sql/t-sql/language-elements/operators-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) behalve [! >](/sql/t-sql/language-elements/not-greater-than-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) en [! <](/sql/t-sql/language-elements/not-less-than-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) | Ja, alle [Transact-SQL-operators](/sql/t-sql/language-elements/operators-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true)  |
| **Stroombesturing** | Ja. Alle [stroombesturingsinstructies van Transact-SQL](/sql/t-sql/language-elements/control-of-flow?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) behalve [CONTINUE](/sql/t-sql/language-elements/continue-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true), [GOTO](/sql/t-sql/language-elements/goto-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true), [RETURN](/sql/t-sql/language-elements/return-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true), [USE](/sql/t-sql/language-elements/use-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) en [WAITFOR](/sql/t-sql/language-elements/waitfor-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) | Ja. Alle [stroombesturingsinstructies van Transact-SQL](/sql/t-sql/language-elements/control-of-flow?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) behalve [RETURN](/sql/t-sql/language-elements/return-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) en SELECT query in `WHILE (...)`-voorwaarde |
| **DDL-instructies (CREATE, ALTER, DROP)** | Ja. Alle DDL-instructies van Transact-SQL die van toepassing zijn op de ondersteunde objecttypen | Ja. Alle DDL-instructies van Transact-SQL die van toepassing zijn op de ondersteunde objecttypen |

## <a name="security"></a>Beveiliging

Met Synapse SQL kunt u ingebouwde beveiligingsfuncties gebruiken om uw gegevens te beveiligen en de toegang te beheren. In de volgende tabel worden de belangrijkste verschillen vergeleken tussen de Synapse SQL-verbruiksmodellen.

|   | Ingericht | Serverloos |
| --- | --- | --- |
| **Aanmeldingen** | N.v.t. (alleen opgenomen gebruikers worden ondersteund in databases) | Ja |
| **Gebruikers** |  N.v.t. (alleen opgenomen gebruikers worden ondersteund in databases) | Ja |
| **[Ingesloten gebruikers](/sql/relational-databases/security/contained-database-users-making-your-database-portable?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true)** | Ja. **Opmerking:** slechts één Azure Active Directory-gebruiker kan een onbeperkte beheerder zijn | Ja |
| **Verificatie van SQL-gebruikersnaam/-wachtwoord**| Ja | Ja |
| **AAD-verificatie (Azure Active Directory)**| Ja, Azure AD-gebruikers | Ja, Azure AD-aanmeldingen en -gebruikers |
| **Passthrough-verificatie voor opslag voor Azure Active Directory (AAD)** | Ja | Ja |
| **SAS-tokenverificatie voor opslag** | Nee | Ja, met behulp van [DATABASE SCOPED CREDENTIAL](/sql/t-sql/statements/create-database-scoped-credential-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) in [EXTERNAL DATA SOURCE](/sql/t-sql/statements/create-external-data-source-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) of [CREDENTIAL](/sql/t-sql/statements/create-credential-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) op exemplaarniveau. |
| **Verificatie met toegangssleutel voor opslag** | Ja, met behulp van [DATABASE SCOPED CREDENTIAL](/sql/t-sql/statements/create-database-scoped-credential-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) in [EXTERNAL DATA SOURCE](/sql/t-sql/statements/create-external-data-source-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) | Nee |
| **Verificatie van [beheerde identiteit](../security/synapse-workspace-managed-identity.md) voor opslag** | Ja, met behulp van [Managed Service Identity-referenties](../../sql-database/sql-database-vnet-service-endpoint-rule-overview.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) | Ja, met `Managed Identity`-referenties. |
| **Verificatie van toepassingsidentiteit voor opslag** | [Ja](/sql/t-sql/statements/create-external-data-source-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) | Nee |
| **Machtigingen - objectniveau** | Ja, inclusief de mogelijkheid om machtigingen voor gebruikers toe te kennen, te weigeren en in te trekken | Ja, inclusief de mogelijkheid om machtigingen toe te kennen, te weigeren en in te trekken voor gebruikers/aanmeldingen op de ondersteunde systeemobjecten |
| **Machtigingen - schemaniveau** | Ja, inclusief de mogelijkheid om machtigingen voor gebruikers/aanmeldingen in het schema toe te kennen, te weigeren en in te trekken | Ja, inclusief de mogelijkheid om machtigingen voor gebruikers/aanmeldingen in het schema toe te kennen, te weigeren en in te trekken |
| **Machtigingen - [databaseniveau](/sql/relational-databases/security/authentication-access/database-level-roles?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true)** | Ja | Ja |
| **Machtigingen - [serverniveau](/sql/relational-databases/security/authentication-access/server-level-roles)** | Nee | Ja, sysadmin en andere serverrollen worden ondersteund |
| **Machtigingen - [Beveiliging op kolomniveau](/azure/synapse-analytics/sql-data-warehouse/column-level-security?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)** | Ja | Ja |
| **Rollen/groepen** | Ja (databasebereik) | Ja (zowel server- als databasebereik) |
| **Beveiligings- &amp; identiteitsfuncties** | Enkele Transact-SQL-beveiligingsfuncties en -operators: `CURRENT_USER`, `HAS_DBACCESS`, `IS_MEMBER`, `IS_ROLEMEMBER`, `SESSION_USER`, `SUSER_NAME`, `SUSER_SNAME`, `SYSTEM_USER`, `USER`, `USER_NAME`, `EXECUTE AS`, `OPEN/CLOSE MASTER KEY` | Enkele Transact-SQL-beveiligingsfuncties en -operators: `CURRENT_USER`, `HAS_DBACCESS`, `HAS_PERMS_BY_NAME`, `IS_MEMBER', 'IS_ROLEMEMBER`, `IS_SRVROLEMEMBER`, `SESSION_USER`, `SUSER_NAME`, `SUSER_SNAME`, `SYSTEM_USER`, `USER`, `USER_NAME`, `EXECUTE AS` en `REVERT`. Beveiligingsfuncties kunnen niet worden gebruikt om externe gegevens op te vragen (sla het resultaat op in een variabele die in de query kan worden gebruikt).  |
| **REFERENTIES MET DATABASEBEREIK** | Ja | Ja |
| **REFERENTIES MET SERVERBEREIK** | Nee | Ja |
| **Beveiliging op rijniveau** | [Ja](https://docs.microsoft.com/sql/relational-databases/security/row-level-security?toc=%2Fazure%2Fsynapse-analytics%2Ftoc.json&bc=%2Fazure%2Fsynapse-analytics%2Fbreadcrumb%2Ftoc.json&view=sql-server-ver15) | Nee |
| **TDE (Transparent Data Encryption)** | [Ja](/azure/sql-database/transparent-data-encryption-azure-sql?toc=%2Fazure%2Fsynapse-analytics%2Ftoc.json&bc=%2Fazure%2Fsynapse-analytics%2Fbreadcrumb%2Ftoc.json&tabs=azure-portal) | Nee | 
| **Gegevensdetectie en -classificatie** | [Ja](/azure/sql-database/sql-database-data-discovery-and-classification?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) | Nee |
| **Beoordeling van beveiligingsproblemen** | [Ja](/azure/sql-database/sql-vulnerability-assessment?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) | Nee |
| **Advanced Threat Protection** | [Ja](/azure/sql-database/sql-database-threat-detection-overview?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
| **Controle** | [Ja](/azure/sql-database/sql-database-auditing?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) | Nee |
| **[Firewallregels](../security/synapse-workspace-ip-firewall.md)**| Ja | Ja |
| **[Privé-eindpunt](../security/synapse-workspace-managed-private-endpoints.md)**| Ja | Ja |

SQL-pool en SQL op aanvraag gebruiken de standaard Transact-SQL-taal om gegevens op te vragen. Raadpleeg de [Transact-SQL-taalverwijzing](/sql/t-sql/language-reference) voor gedetailleerde verschillen.

## <a name="tools"></a>Hulpprogramma's

U kunt verschillende hulpprogramma's gebruiken om verbinding te maken met Synapse SQL om gegevens op te vragen.

|   | Ingericht | Serverloos |
| --- | --- | --- |
| **Synapse Studio** | Ja, SQL-scripts | Ja, SQL-scripts |
| **Power BI** | Ja | [Ja](tutorial-connect-power-bi-desktop.md) |
| **Azure Analysis Service** | Ja | Ja |
| **Azure Data Studio** | Ja | Ja, versie 1.18.0 of hoger. SQL-scripts en SQL-notebooks worden ondersteund. |
| **SQL Server Management Studio** | Ja | Ja, versie 18.5 of hoger |

> [!NOTE]
> U kunt SSMS gebruiken om verbinding te maken met SQL op aanvraag (preview) en query's. Het wordt gedeeltelijk ondersteund vanaf versie 18.5, maar u kunt de tool gebruiken om alleen verbinding te maken en query's uit te voeren.

De meeste toepassingen gebruiken de standaard Transact-SQL-taal om gegevens op te vragen bij zowel ingerichte als serverloze verbruiksmodellen van Synapse SQL.

## <a name="storage-options"></a>Opslagopties

Geanalyseerde gegevens kunnen op verschillende opslagtypen worden opgeslagen. De volgende tabel bevat een lijst van alle beschikbare opslagopties:

|   | Ingericht | Serverloos |
| --- | --- | --- |
| **Interne opslag** | Ja | Nee |
| **Azure Data Lake v2** | Ja | Ja |
| **Azure Blob Storage** | Ja | Ja |
| **Azure CosmosDB analytical storage** | Nee | Ja, met [Synapse-koppeling](/azure/cosmos-db/synapse-link?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) (onder [gated preview](/azure/cosmos-db/synapse-link?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json#limitations)) |

## <a name="data-formats"></a>Bestandsindelingen

Geanalyseerde gegevens kunnen in verschillende bestandsindelingen worden opgeslagen. De volgende tabel bevat een lijst van alle beschikbare gegevensindelingen die kunnen worden geanalyseerd:

|   | Ingericht | Serverloos |
| --- | --- | --- |
| **Met scheidingstekens** | [Ja](/sql/t-sql/statements/create-external-file-format-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) | [Ja](query-single-csv-file.md) |
| **CSV** | Ja (het gebruik van meerdere scheidingstekens wordt niet ondersteund) | [Ja](query-single-csv-file.md) |
| **Parquet** | [Ja](/sql/t-sql/statements/create-external-file-format-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) | [Ja](query-parquet-files.md), inclusief bestanden met [geneste typen](query-parquet-nested-types.md) |
| **Hive ORC** | [Ja](/sql/t-sql/statements/create-external-file-format-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) | Nee |
| **Hive RC** | [Ja](/sql/t-sql/statements/create-external-file-format-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) | Nee |
| **JSON** | Ja | [Ja](query-json-files.md) |
| **Avro** | Nee | Nee |
| **[Delta-lake](https://delta.io/)** | Nee | Nee |
| **[CDM](https://docs.microsoft.com/common-data-model/)** | Nee | Nee |

## <a name="next-steps"></a>Volgende stappen
Meer informatie over best practices voor SQL-pool en SQL on-demand vindt u in de volgende artikelen:

- [Best practices voor SQL-pool](best-practices-sql-pool.md)
- [Best practices voor SQL on-demand](best-practices-sql-on-demand.md)
