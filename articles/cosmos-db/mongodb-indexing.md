---
title: Indexering beheren in de API van Azure Cosmos DB voor MongoDB
description: Dit artikel bevat een overzicht van Azure Cosmos DB indexerings mogelijkheden met behulp van de API van Azure Cosmos DB voor MongoDB
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.devlang: nodejs
ms.topic: how-to
ms.date: 08/06/2020
author: timsander1
ms.author: tisande
ms.custom: devx-track-javascript
ms.openlocfilehash: e47b8727eccd1b185f381ae3f8474fe13a406501
ms.sourcegitcommit: 7fe8df79526a0067be4651ce6fa96fa9d4f21355
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2020
ms.locfileid: "87843807"
---
# <a name="manage-indexing-in-azure-cosmos-dbs-api-for-mongodb"></a>Indexering beheren in de API van Azure Cosmos DB voor MongoDB

Azure Cosmos DB-API voor MongoDB maakt gebruik van de belangrijkste mogelijkheden voor index beheer van Azure Cosmos DB. In dit artikel wordt uitgelegd hoe u indexen toevoegt met behulp van de API van Azure Cosmos DB voor MongoDB. U kunt ook een [overzicht van het indexeren in azure Cosmos DB](index-overview.md) lezen dat relevant is voor alle api's.

## <a name="indexing-for-mongodb-server-version-36"></a>Indexering voor MongoDB-Server versie 3,6

De API van Azure Cosmos DB voor MongoDB-Server versie 3,6 indexeert automatisch het `_id` veld dat niet kan worden verwijderd. De unieke waarde van het veld wordt automatisch afgedwongen `_id` per Shard-sleutel. In Azure Cosmos DB API voor MongoDB, sharding en indexering, zijn afzonderlijke concepten. U hoeft uw Shard-sleutel niet te indexeren. Net als bij een andere eigenschap in uw document, wordt u echter aangeraden de Shard-sleutel te indexeren als deze eigenschap een gemeen schappelijk filter is in uw query's.

Als u extra velden wilt indexeren, past u de opdrachten voor MongoDB-indexbeheer toe. Net als in MongoDB indexeert Azure Cosmos DB-API voor MongoDB automatisch het `_id` veld. Dit standaard indexeringsbeleid wijkt af van de Azure Cosmos DB SQL-API, waarmee standaard alle velden worden geïndexeerd.

Als u een sortering op een query wilt Toep assen, moet u een index maken voor de velden die in de sorteer bewerking worden gebruikt.

## <a name="index-types"></a>Indextypen

### <a name="single-field"></a>Eén veld

U kunt indexen maken voor één veld. De sorteer volgorde van de enkelvoudige veld index is niet van belang. Met de volgende opdracht maakt u een index voor het veld `name` :

`db.coll.createIndex({name:1})`

Bij een query worden meerdere enkelvoudige veld indexen gebruikt, waar beschikbaar. U kunt Maxi maal 500 enkelvoudige veld indexen per container maken.

### <a name="compound-indexes-mongodb-server-version-36"></a>Samengestelde indexen (MongoDB Server versie 3,6)

Azure Cosmos DB-API voor MongoDB ondersteunt samengestelde indexen voor accounts die gebruikmaken van versie 3,6 wire protocol. U kunt Maxi maal acht velden in een samengestelde index toevoegen. In tegens telling tot in MongoDB moet u alleen een samengestelde index maken als uw query efficiënt moet worden gesorteerd op meerdere velden tegelijk. Voor query's met meerdere filters die niet hoeven te worden gesorteerd, maakt u meerdere enkelvoudige veld indexen in plaats van één samengestelde index.

Met de volgende opdracht maakt u een samengestelde index voor de velden `name` en `age` :

`db.coll.createIndex({name:1,age:1})`

U kunt samengestelde indexen gebruiken om efficiënt te sorteren op meerdere velden tegelijk, zoals wordt weer gegeven in het volgende voor beeld:

`db.coll.find().sort({name:1,age:1})`

U kunt ook de voor gaande samengestelde index gebruiken om efficiënt te sorteren op een query met de tegenovergestelde sorteer volgorde voor alle velden. Hier volgt een voorbeeld:

`db.coll.find().sort({name:-1,age:-1})`

De volg orde van de paden in de samengestelde index moet echter exact overeenkomen met de query. Hier volgt een voor beeld van een query waarvoor een extra samengestelde index is vereist:

`db.coll.find().sort({age:1,name:1})`

### <a name="multikey-indexes"></a>MultiKey-indexen

Azure Cosmos DB maakt MultiKey-indexen om inhoud te indexeren die in matrices is opgeslagen. Als u een veld indexeert met een matrix waarde, Azure Cosmos DB automatisch elk element in de matrix geïndexeerd.

### <a name="geospatial-indexes"></a>Georuimtelijke indexen

Veel georuimtelijke Opera tors profiteren van georuimtelijke indexen. Momenteel biedt de API van Azure Cosmos DB voor MongoDB ondersteuning voor `2dsphere` indexen. De API biedt nog geen ondersteuning voor `2d` indexen.

Hier volgt een voor beeld van het maken van een georuimtelijke index op het `location` veld:

`db.coll.createIndex({ location : "2dsphere" })`

### <a name="text-indexes"></a>Tekst indexen

De API van Azure Cosmos DB voor MongoDB biedt momenteel geen ondersteuning voor tekst indexen. Voor tekst zoekopdracht query's op teken reeksen moet u [Azure Cognitive Search](https://docs.microsoft.com/azure/search/search-howto-index-cosmosdb) -integratie met Azure Cosmos DB gebruiken.

## <a name="wildcard-indexes"></a>Joker teken indexen

U kunt joker tekens gebruiken om query's te ondersteunen op onbekende velden. Stel dat u een verzameling hebt die gegevens over families bevat.

Hier maakt deel uit van een voorbeeld document in die verzameling:

```json
  "children": [
     {
         "firstName": "Henriette Thaulow",
         "grade": "5"
     }
  ]
```

Hier volgt nog een voor beeld met een iets andere set eigenschappen in `children` :

```json
  "children": [
      {
        "familyName": "Merriam",
        "givenName": "Jesse",
        "pets": [
            { "givenName": "Goofy" },
            { "givenName": "Shadow" }
      },
      {
        "familyName": "Merriam",
        "givenName": "John",
      }
  ]
```

In deze verzameling kunnen documenten verschillende mogelijke eigenschappen hebben. Als u alle gegevens in de matrix wilt indexeren `children` , hebt u twee opties: Maak afzonderlijke indexen voor elke afzonderlijke eigenschap of maak één Joker teken index voor de volledige `children` matrix.

### <a name="create-a-wildcard-index"></a>Een Joker teken index maken

Met de volgende opdracht maakt u een Joker teken index voor alle eigenschappen in `children` :

`db.coll.createIndex({"children.$**" : 1})`

**In tegens telling tot in MongoDb kunnen Joker teken indexen meerdere velden in query-predikaten ondersteunen**. Er is geen verschil in de query prestaties als u één enkele Joker teken index gebruikt in plaats van een afzonderlijke index voor elke eigenschap te maken.

U kunt de volgende index typen maken met de syntaxis Joker teken:

- Eén veld
- Georuimtelijk

### <a name="indexing-all-properties"></a>Alle eigenschappen indexeren

Hier kunt u een Joker teken index maken voor alle velden:

`db.coll.createIndex( { "$**" : 1 } )`

Wanneer u de ontwikkeling begint, is het handig om een Joker teken index te maken voor alle velden. Naarmate er meer eigenschappen in een document worden geïndexeerd, worden de kosten voor de aanvraag eenheid (RU) voor het schrijven en bijwerken van het document verhoogd. Als u een schrijf bare werk belasting hebt, moet u er dus voor kiezen om de paden afzonderlijk te indexeren, in tegens telling tot het gebruik van Joker teken indexen.

### <a name="limitations"></a>Beperkingen

Joker tekens bieden geen ondersteuning voor een van de volgende index typen of eigenschappen:

- Stelling
- TTL
- Uniek

In **tegens telling tot in MongoDb**, in de API van Azure Cosmos DB voor MongoDb **kunt u geen** Joker teken indexen gebruiken voor:

- Een Joker teken index maken die meerdere specifieke velden bevat

`db.coll.createIndex(
    { "$**" : 1 },
    { "wildcardProjection " :
        {
           "children.givenName" : 1,
           "children.grade" : 1
        }
    }
)`

- Een Joker teken index maken waarmee meerdere specifieke velden worden uitgesloten

`db.coll.createIndex(
    { "$**" : 1 },
    { "wildcardProjection" :
        {
           "children.givenName" : 0,
           "children.grade" : 0
        }
    }
)`

Als alternatief kunt u meerdere Joker teken indexen maken.

## <a name="index-properties"></a>Indexeigenschappen

De volgende bewerkingen zijn gebruikelijk voor accounts die zijn gereserveerd van wire-protocol versie 3,6 en accounts die eerdere versies bieden. Meer informatie over [ondersteunde indexen en geïndexeerde eigenschappen](mongodb-feature-support-36.md#indexes-and-index-properties)vindt u hier.

### <a name="unique-indexes"></a>Unieke indexen

[Unieke indexen](unique-keys.md) zijn handig voor het afdwingen dat twee of meer documenten niet dezelfde waarde voor geïndexeerde velden bevatten.

> [!IMPORTANT]
> Unieke indexen kunnen alleen worden gemaakt wanneer de verzameling leeg is (geen documenten bevat).

Met de volgende opdracht maakt u een unieke index voor het veld `student_id` :

```shell
globaldb:PRIMARY> db.coll.createIndex( { "student_id" : 1 }, {unique:true} )
{
        "_t" : "CreateIndexesResponse",
        "ok" : 1,
        "createdCollectionAutomatically" : false,
        "numIndexesBefore" : 1,
        "numIndexesAfter" : 4
}
```

Voor Shard-verzamelingen moet u de Shard-sleutel (Partition) opgeven om een unieke index te maken. In andere woorden, alle unieke indexen voor een shard-verzameling zijn samengestelde indexen waarbij een van de velden de partitiesleutel is.

Met de volgende opdrachten maakt u een Shard ```coll``` -verzameling (de Shard-sleutel is ```university``` ) met een unieke index voor de velden `student_id` en `university` :

```shell
globaldb:PRIMARY> db.runCommand({shardCollection: db.coll._fullName, key: { university: "hashed"}});
{
        "_t" : "ShardCollectionResponse",
        "ok" : 1,
        "collectionsharded" : "test.coll"
}
globaldb:PRIMARY> db.coll.createIndex( { "student_id" : 1, "university" : 1 }, {unique:true})
{
        "_t" : "CreateIndexesResponse",
        "ok" : 1,
        "createdCollectionAutomatically" : false,
        "numIndexesBefore" : 3,
        "numIndexesAfter" : 4
}
```

In het voor gaande voor beeld wordt met het weglaten van de ```"university":1``` component een fout geretourneerd met het volgende bericht:

```"cannot create unique index over {student_id : 1.0} with shard key pattern { university : 1.0 }"```

### <a name="ttl-indexes"></a>TTL indexen

Als u het verlopen van documenten in een bepaalde verzameling wilt inschakelen, moet u een [time-to-Live-index (TTL)](../cosmos-db/time-to-live.md)maken. Een TTL-index is een index van het `_ts` veld met een `expireAfterSeconds` waarde.

Voorbeeld:

```JavaScript
globaldb:PRIMARY> db.coll.createIndex({"_ts":1}, {expireAfterSeconds: 10})
```

Met de voor gaande opdracht worden alle documenten in de ```db.coll``` verzameling verwijderd die in de afgelopen 10 seconden niet zijn gewijzigd.

> [!NOTE]
> Het veld **_ts** is specifiek voor Azure Cosmos DB en is niet toegankelijk vanuit MongoDb-clients. Het is een gereserveerde eigenschap (systeem) die het tijds tempel van de laatste wijziging van het document bevat.

## <a name="track-index-progress"></a>Voortgang van index bijhouden

Versie 3,6 van de API van Azure Cosmos DB voor MongoDB ondersteunt de `currentOp()` opdracht voor het bijhouden van de voortgang van de index voor een Data Base-exemplaar. Met deze opdracht wordt een document geretourneerd dat informatie bevat over bewerkingen die worden uitgevoerd op een Data Base-exemplaar. U gebruikt de `currentOp` opdracht om alle bewerkingen in uitvoering bij te houden in systeem eigen MongoDb. In de API van Azure Cosmos DB voor MongoDB ondersteunt deze opdracht alleen het volgen van de index bewerking.

Hier volgen enkele voor beelden van het gebruik van de `currentOp` opdracht voor het volgen van de voortgang van de index:

* De voortgang van de index voor een verzameling ophalen:

   ```shell
   db.currentOp({"command.createIndexes": <collectionName>, "command.$db": <databaseName>})
   ```

* De voortgang van de index ophalen voor alle verzamelingen in een Data Base:

  ```shell
  db.currentOp({"command.$db": <databaseName>})
  ```

* De index voortgang ophalen voor alle data bases en verzamelingen in een Azure Cosmos-account:

  ```shell
  db.currentOp({"command.createIndexes": { $exists : true } })
  ```

### <a name="examples-of-index-progress-output"></a>Voor beelden van uitvoer van index voortgang

In de details van de index voortgang wordt het percentage voortgang van de huidige index bewerking weer gegeven. Hier volgt een voor beeld waarin de indeling van het uitvoer document wordt weer gegeven voor de verschillende stadia van de index voortgang:

- Een index bewerking op een ' foo '-verzameling en ' bar '-data base die 60 procent is voltooid, heeft het volgende uitvoer document. `Inprog[0].progress.total`In het veld wordt 100 weer gegeven als het voltooiings percentage van het doel.

   ```json
   {
        "inprog" : [
        {
                ………………...
                "command" : {
                        "createIndexes" : foo
                        "indexes" :[ ],
                        "$db" : bar
                },
                "msg" : "Index Build (background) Index Build (background): 60 %",
                "progress" : {
                        "done" : 60,
                        "total" : 100
                },
                …………..…..
        }
        ],
        "ok" : 1
   }
   ```

- Als een index bewerking zojuist is gestart op een ' foo '-verzameling en een staaf-data base, kan het uitvoer document 0 procent van de voortgang weer geven totdat het een meetbaar niveau bereikt.

   ```json
   {
        "inprog" : [
        {
                ………………...
                "command" : {
                        "createIndexes" : foo
                        "indexes" :[ ],
                        "$db" : bar
                },
                "msg" : "Index Build (background) Index Build (background): 0 %",
                "progress" : {
                        "done" : 0,
                        "total" : 100
                },
                …………..…..
        }
        ],
       "ok" : 1
   }
   ```

- Wanneer de index bewerking in uitvoering is voltooid, worden in het uitvoer document lege `inprog` bewerkingen weer gegeven.

   ```json
   {
      "inprog" : [],
      "ok" : 1
   }
   ```

## <a name="background-index-updates"></a>Achtergrond-index updates

Ongeacht de waarde die is opgegeven voor de eigenschap **achtergrond** index, worden index updates altijd op de achtergrond uitgevoerd. Omdat index updates gebruikmaken van aanvraag eenheden (RUs) met een lagere prioriteit dan andere database bewerkingen, hebben index wijzigingen geen invloed op uitval voor schrijf bewerkingen, updates of verwijderingen.

Er is geen invloed op de Lees Beschik baarheid bij het toevoegen van een nieuwe index. Met query's worden alleen nieuwe indexen gebruikt wanneer de index transformatie is voltooid. Tijdens de index transformatie zal de query-engine bestaande indexen blijven gebruiken. u ziet dan vergelijk bare Lees prestaties tijdens de indexerings transformatie om te zien wat u hebt gezien voordat de index wijziging werd geïnitieerd. Bij het toevoegen van nieuwe indexen is er ook geen risico van onvolledige of inconsistente query resultaten.

Bij het verwijderen van indexen en het direct uitvoeren van query's zijn de filters op de verwijderde indexen mogelijk inconsistent en onvolledig, totdat de index transformatie is voltooid. Als u indexen verwijdert, garandeert de query-engine geen consistente of volledige resultaten wanneer query's filteren op deze onlangs verwijderde indexen. De meeste ontwikkel aars verwijderen geen indexen en proberen deze vervolgens onmiddellijk te doorzoeken, zodat deze situatie in de praktijk niet waarschijnlijk is.

> [!NOTE]
> U kunt de voortgang van de [index bijhouden](#track-index-progress).

## <a name="migrate-collections-with-indexes"></a>Verzamelingen migreren met indexen

Op dit moment kunt u alleen unieke indexen maken als de verzameling geen documenten bevat. Populaire hulpprogram ma's voor migratie van MongoDB proberen de unieke indexen te maken nadat u de gegevens hebt geïmporteerd. U kunt dit probleem omzeilen door hand matig de overeenkomstige verzamelingen en unieke indexen te maken, in plaats van het hulp programma voor migratie uit te voeren. (U kunt dit gedrag betrekken met ```mongorestore``` behulp van de `--noIndexRestore` vlag op de opdracht regel.)

## <a name="indexing-for-mongodb-version-32"></a>Indexeren voor MongoDB-versie 3,2

Beschik bare indexerings functies en standaard waarden zijn anders voor Azure Cosmos-accounts die compatibel zijn met versie 3,2 van het MongoDB wire-protocol. U kunt [de versie van uw account controleren](mongodb-feature-support-36.md#protocol-support). U kunt een upgrade uitvoeren naar de 3,6-versie door een [ondersteunings aanvraag](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)in te dienen.

Als u versie 3,2 gebruikt, geeft deze sectie een overzicht van de belangrijkste verschillen met versie 3,6.

### <a name="dropping-default-indexes-version-32"></a>Standaard indexen verwijderen (versie 3,2)

In tegens telling tot de 3,6-versie van de API van Azure Cosmos DB voor MongoDB, indexeert versie 3,2 standaard elke eigenschap. U kunt de volgende opdracht gebruiken om deze standaard indexen voor een verzameling te verwijderen ( ```coll``` ):

```JavaScript
> db.coll.dropIndexes()
{ "_t" : "DropIndexesResponse", "ok" : 1, "nIndexesWas" : 3 }
```

Nadat u de standaard indexen hebt verwijderd, kunt u meer indexen toevoegen zoals u dat zou doen in versie 3,6.

### <a name="compound-indexes-version-32"></a>Samengestelde indexen (versie 3,2)

Samengestelde indexen bevatten verwijzingen naar meerdere velden van een document. Als u een samengestelde index wilt maken, moet u een upgrade uitvoeren naar versie 3,6 door een [ondersteunings aanvraag](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)in te dienen.

### <a name="wildcard-indexes-version-32"></a>Joker tekens-indexen (versie 3,2)

Als u een Joker teken index wilt maken, moet u een upgrade uitvoeren naar versie 3,6 door een [ondersteunings aanvraag](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)in te dienen.

## <a name="next-steps"></a>Volgende stappen

* [Indexering in Azure Cosmos DB](../cosmos-db/index-policy.md)
* [Gegevens in Azure Cosmos DB automatisch laten verlopen met TTL](../cosmos-db/time-to-live.md)
* Zie [een query uitvoeren op een Azure Cosmos-container](how-to-query-container.md) artikel voor meer informatie over de relatie tussen partitioneren en indexeren.
