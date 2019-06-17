---
title: Werken met JavaScript language integrated query API in Azure Cosmos DB
description: Dit artikel bevat de concepten voor JavaScript language integrated query API voor het maken van opgeslagen procedures en triggers in Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: cc1815ce4a7a9ed40848e4a67a7fd9e032c1daa1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66226197"
---
# <a name="javascript-query-api-in-azure-cosmos-db"></a>JavaScript API-query in Azure Cosmos DB

Naast het uitgeven van query's met behulp van de SQL-API in Azure Cosmos DB, de [Cosmos DB-server-side SDK](https://azure.github.io/azure-cosmosdb-js-server/) kunt u geoptimaliseerde query's uitvoeren met een JavaScript-interface. U hebt geen rekening met de SQL-taal gebruiken deze JavaScript-interface. De JavaScript-query-API u query's via een programma kunt maken kunt met het doorgeven van predikaat functies in volgorde van de functie aanroept, met een syntaxis die bekend is bij de matrix dient te worden en populaire JavaScript-bibliotheken zoals Lodash van ECMAScript5. Query's worden geparseerd door de JavaScript-runtime en efficiënt uitgevoerd met behulp van Azure Cosmos DB-indexen.

## <a name="supported-javascript-functions"></a>Ondersteunde JavaScript-functies

| **Functie** | **Beschrijving** |
|---------|---------|
|`chain() ... .value([callback] [, options])`|Start een keten-aanroep die moet worden afgesloten met value().|
|`filter(predicateFunction [, options] [, callback])`|Filtert de invoer met behulp van een predicaat functie die resulteert in waar/onwaar om te filteren in/uit-invoer documenten in de resulterende set. Deze functie werkt die vergelijkbaar is met een WHERE-component in SQL.|
|`flatten([isShallow] [, options] [, callback])`|Combineert en worden matrices van elke invoer-item in een matrix samengevoegd. Deze functie werkt die vergelijkbaar is met SelectMany in LINQ.|
|`map(transformationFunction [, options] [, callback])`|Van toepassing is een projectie een transformatiefunctie die elk item invoer wordt toegewezen aan een JavaScript-object of een waarde opgegeven. Deze functie werkt die vergelijkbaar is met een SELECT-component in SQL.|
|`pluck([propertyName] [, options] [, callback])`|Deze functie is een snelkoppeling voor een kaart die de waarde van één eigenschap van elk item invoer geëxtraheerd.|
|`sortBy([predicate] [, options] [, callback])`|Maakt een nieuwe set documenten door te sorteren van de documenten in de stroom invoerdocument in oplopende volgorde met behulp van de opgegeven predicaat. Deze functie werkt die vergelijkbaar is met een ORDER BY-component in SQL.|
|`sortByDescending([predicate] [, options] [, callback])`|Maakt een nieuwe set documenten door te sorteren van de documenten in de stroom invoerdocument in aflopende volgorde met behulp van de opgegeven predicaat. Deze functie werkt die vergelijkbaar is met een component ORDER BY x DESC in SQL.|
|`unwind(collectionSelector, [resultSelector], [options], [callback])`|Voert een self-join met binnenste matrix en voegt de resultaten van beide zijden als tuples toe aan de resultaat-projectie. Bijvoorbeeld, lid worden van een persoon-document met person.pets geeft als resultaat [persoon, huisdier] tuples. Dit is vergelijkbaar met SelectMany in .NET-koppeling.|

Wanneer ze zijn opgenomen in het predikaat en/of de selectie van functies, worden de volgende JavaScript-constructies automatisch geoptimaliseerd om te worden uitgevoerd op Azure Cosmos DB-indexen:

- Eenvoudige operators: `=` `+` `-` `*` `/` `%` `|` `^` `&` `==` `!=` `===` `!===` `<` `>` `<=` `>=` `||` `&&` `<<` `>>` `>>>!` `~`
- Letterlijke waarden, met inbegrip van de letterlijke object: {}
- var, Ga terug

De volgende JavaScript-constructies kunnen niet worden geoptimaliseerd voor Azure Cosmos DB-indexen:

- Controlestroom (bijvoorbeeld, als voor, tijdens)
- Functieaanroepen

Zie voor meer informatie de [documentatie voor Cosmos DB-Server Side JavaScript](https://azure.github.io/azure-cosmosdb-js-server/).

## <a name="sql-to-javascript-cheat-sheet"></a>SQL voor het referentiemateriaal voor JavaScript

De volgende tabel bevat verschillende SQL-query's en de bijbehorende JavaScript-query's. Eigenschappen (bijvoorbeeld item.id) zijn net als bij SQL-query's hoofdlettergevoelig.

> [!NOTE]
> `__` (dubbele onderstreping) is een alias naar `getContext().getCollection()` bij gebruik van de JavaScript-query-API.

|**SQL**|**JavaScript API-Query**|**Beschrijving**|
|---|---|---|
|SELECTEER *<br>VAN docs| __.map(Function(doc) { <br>&nbsp;&nbsp;&nbsp;&nbsp;retourneren van doc-bestand;<br>});|Resultaten in alle documenten (gepagineerde met vervolgtoken) als is.|
|SELECT <br>&nbsp;&nbsp;&nbsp;Docs.id,<br>&nbsp;&nbsp;&nbsp;Docs.Message als msg,<br>&nbsp;&nbsp;&nbsp;Docs.Actions <br>VAN docs|__.map(Function(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;{retourneren<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ID: doc.id,<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;msg: doc.message,<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Actions:doc.Actions<br>&nbsp;&nbsp;&nbsp;&nbsp;};<br>});|De-id, een bericht (alias voor msg) en een actie uit alle documenten projecten.|
|SELECTEER *<br>VAN docs<br>WHERE<br>&nbsp;&nbsp;&nbsp;docs.id="X998_Y998"|__.filter(Function(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;retourneren van doc.id === 'X998_Y998';<br>});|Query's voor documenten met het predicaat: id = 'X998_Y998'.|
|SELECTEER *<br>VAN docs<br>WHERE<br>&nbsp;&nbsp;&nbsp;ARRAY_CONTAINS (docs. -Tags, 123)|__.filter(Function(x) {<br>&nbsp;&nbsp;&nbsp;&nbsp;retourneren van x.Tags & & x.Tags.indexOf(123) > -1;<br>});|Query's voor documenten die een eigenschap Tags en labels hebben, is een matrix met de waarde 123.|
|SELECT<br>&nbsp;&nbsp;&nbsp;Docs.id,<br>&nbsp;&nbsp;&nbsp;Docs.Message als msg<br>VAN docs<br>WHERE<br>&nbsp;&nbsp;&nbsp;docs.id="X998_Y998"|__.chain()<br>&nbsp;&nbsp;&nbsp;&nbsp;.filter(Function(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;retourneren van doc.id === 'X998_Y998';<br>&nbsp;&nbsp;&nbsp;&nbsp;})<br>&nbsp;&nbsp;&nbsp;&nbsp;.map(Function(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{retourneren<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ID: doc.id,<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;msg: doc.message<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;};<br>&nbsp;&nbsp;&nbsp;&nbsp;})<br>.value();|Query's voor documenten met een predicaat, id = 'X998_Y998', en vervolgens de id en het bericht (alias voor msg)-projecten.|
|SELECT VALUE-tag<br>VAN docs<br>Neem deel aan een tag IN docs. Tags<br>ORDER BY docs._ts|__.chain()<br>&nbsp;&nbsp;&nbsp;&nbsp;.filter(Function(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;document retourneren. Labels & & Array.isArray (doc-bestand. -Tags);<br>&nbsp;&nbsp;&nbsp;&nbsp;})<br>&nbsp;&nbsp;&nbsp;&nbsp;.sortBy(function(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;retourneren van doc._ts;<br>&nbsp;&nbsp;&nbsp;&nbsp;})<br>&nbsp;&nbsp;&nbsp;&nbsp;.pluck("Tags")<br>&nbsp;&nbsp;&nbsp;&nbsp;.flatten()<br>&nbsp;&nbsp;&nbsp;&nbsp;.value()|Filters voor documenten met een matrixeigenschap, labels, en sorteert de resulterende documenten door de eigenschap _ts timestamp-systeem en projecten + de matrix Tags worden samengevoegd.|

## <a name="next-steps"></a>Volgende stappen

Meer informatie concepten en procedures voor schrijven en opgeslagen procedures, triggers en gebruikersgedefinieerde functies gebruiken in Azure Cosmos DB:

- [Over het schrijven van opgeslagen procedures en triggers die met Javascript-Query-API](how-to-write-javascript-query-api.md)
- [Werken met Azure Cosmos DB opgeslagen procedures, triggers en gebruikersgedefinieerde functies](stored-procedures-triggers-udfs.md)
- [Over het gebruik van opgeslagen procedures, triggers, de gebruiker gedefinieerde functies in Azure Cosmos DB](how-to-use-stored-procedures-triggers-udfs.md)
- [Azure Cosmos DB JavaScript-serverzijde API-naslaginformatie](https://azure.github.io/azure-cosmosdb-js-server)
- [JavaScript ES6 (ECMA 2015)](https://www.ecma-international.org/ecma-262/6.0/)
