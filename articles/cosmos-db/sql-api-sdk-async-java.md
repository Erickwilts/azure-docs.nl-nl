---
title: 'Azure Cosmos DB: SQL Async Java-API, SDK en resources'
description: Meer informatie over de SQL Async Java-API en SDK, inclusief release datums, buiten gebruik stellen datums en wijzigingen die zijn aangebracht tussen elke versie van de Azure Cosmos DB SQL Async Java SDK.
author: moderakh
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: java
ms.topic: reference
ms.date: 3/5/2019
ms.author: moderakh
ms.openlocfilehash: 356838f16f7f13506657326bae5dbe994d54bdd5
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "57570093"
---
# <a name="azure-cosmos-db-async-java-sdk-for-sql-api-release-notes-and-resources"></a>Azure Cosmos DB Async Java SDK voor SQL-API: Opmerkingen bij de release en resources
> [!div class="op_single_selector"]
> * [.NET](sql-api-sdk-dotnet.md)
> * [.NET-Wijzigingenfeed](sql-api-sdk-dotnet-changefeed.md)
> * [.NET Core](sql-api-sdk-dotnet-core.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Async Java](sql-api-sdk-async-java.md)
> * [Java](sql-api-sdk-java.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](https://docs.microsoft.com/rest/api/cosmos-db/)
> * [REST-resourceprovider](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/)
> * [SQL](sql-api-query-reference.md)
> * [BulkExecutor - .NET](sql-api-sdk-bulk-executor-dot-net.md)
> * [BulkExecutor - Java](sql-api-sdk-bulk-executor-java.md)

De SQL-SDK voor Java-API asynchrone wijkt af van de Java-SDK van de SQL-API door te geven van asynchrone bewerkingen met ondersteuning van de [Netty bibliotheek](https://netty.io/). De vooraf bestaande [SQL API Java SDK](sql-api-sdk-java.md) biedt geen ondersteuning voor asynchrone bewerkingen. 

| |  |
|---|---|
| **SDK downloaden** | [Maven](https://mvnrepository.com/artifact/com.microsoft.azure/azure-cosmosdb) |
|**API-documentatie** |[Java API-referentiedocumentatie](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx.asyncdocumentclient?view=azure-java-stable) | 
|**Bijdragen aan de SDK** | [GitHub](https://github.com/Azure/azure-cosmosdb-java) | 
|**Aan de slag** | [Aan de slag met de Async Java-SDK](https://github.com/Azure-Samples/azure-cosmos-db-sql-api-async-java-getting-started) | 
|**Voorbeeld van code** | [GitHub](https://github.com/Azure/azure-cosmosdb-java#usage-code-sample)| 
| **Tips voor prestaties**| [GitHub Leesmij-bestand](https://github.com/Azure/azure-cosmosdb-java#guide-for-prod)| 
| **Minimaal ondersteunde runtime**|[JDK 8](https://aka.ms/azure-jdks) | 

## <a name="release-notes"></a>Releaseopmerkingen

### <a name="a-name243243"></a><a name="2.4.3"/>2.4.3
* Bugfix voor resource-geheugenlek op client#close() ([github #88](https://github.com/Azure/azure-cosmosdb-java/issues/88)).

### <a name="a-name242242"></a><a name="2.4.2"/>2.4.2
* Voortzetting van toegevoegde token ondersteuning voor cross-partitie query's.

### <a name="a-name241241"></a><a name="2.4.1"/>2.4.1
* Enkele fouten opgelost in de directe modus.
* Dankzij de verbeterde logboekregistratie in de directe modus.
* Beheer van de verbeterde verbinding.

### <a name="a-name240240"></a><a name="2.4.0"/>2.4.0
* De modus direct connectiviteit is nu algemeen Available(GA). Zie voor een voorbeeld dat gebruikmaakt van de modus direct connectiviteit, [azure-cosmosdb-java](https://github.com/Azure/azure-cosmosdb-java) GitHub-opslagplaats.
* Ondersteuning toegevoegd voor QueryMetrics.
* De API's accepteren java.util.Collection waarvoor volgorde is belangrijk om te accepteren java.util.List in plaats daarvan gewijzigd. Now ConnectionPolicy#getPreferredLocations(), JsonSerialization, and PartitionKey(.) accept List.

### <a name="a-name240-beta-1240-beta-1"></a><a name="2.4.0-beta-1"/>2.4.0-beta-1
* Er is ondersteuning toegevoegd voor de modus direct connectiviteit.
* De API's accepteren java.util.Collection waarvoor volgorde is belangrijk om te accepteren java.util.List in plaats daarvan gewijzigd.
  Now ConnectionPolicy#getPreferredLocations(), JsonSerialization, and PartitionKey(.) accept List.
* Probleem opgelost een sessie voor document-query in de modus van de gateway.
* Afhankelijkheden bijgewerkt (netty 0.4.20 [github #79](https://github.com/Azure/azure-cosmosdb-java/issues/79), RxJava 1.3.8).

### <a name="a-name231231"></a><a name="2.3.1"/>2.3.1
* Oplossingen afhandeling van reacties op zeer grote query's.
* Verwerking van de resource-token worden opgelost bij het instantiëren van client ([github #78](https://github.com/Azure/azure-cosmosdb-java/issues/78)).
* Kwetsbaar afhankelijkheid jackson-databind bijgewerkt ([github #77](https://github.com/Azure/azure-cosmosdb-java/pull/77)).

### <a name="a-name230230"></a><a name="2.3.0"/>2.3.0
* Een resource geheugenlek bug opgelost.
* Er is ondersteuning toegevoegd voor MultiPolygon
* Ondersteuning toegevoegd voor aangepaste kopteksten in RequestOptions.

### <a name="a-name222222"></a><a name="2.2.2"/>2.2.2
* Een bug pakketten verholpen.

### <a name="a-name221221"></a><a name="2.2.1"/>2.2.1
* Een NFE opgelost in het pad voor schrijven probeer het opnieuw.
* Eindpunt management, een bug NFE vast.
* Kwetsbaar afhankelijkheden bijgewerkt ([GitHub #68](https://github.com/Azure/azure-cosmosdb-java/issues/68)).
* Ondersteuning toegevoegd voor Netty netwerk logboekregistratie voor het oplossen van problemen.

### <a name="a-name220220"></a><a name="2.2.0"/>2.2.0
* Er is ondersteuning toegevoegd voor meerdere regio's schrijven.

### <a name="a-name210210"></a><a name="2.1.0"/>2.1.0
* Er is ondersteuning toegevoegd voor de Proxy.
* Ondersteuning toegevoegd voor bronautorisatie token.
* Een probleem opgelost in grote partitiesleutels verwerken ([GitHub #63](https://github.com/Azure/azure-cosmosdb-java/issues/63)).
* Documentatie is verbeterd.
* De SDK is geherstructureerd in gedetailleerdere modules.

### <a name="a-name201201"></a><a name="2.0.1"/>2.0.1
* Een probleem opgelost voor niet-Engelse landinstellingen ([GitHub #51](https://github.com/Azure/azure-cosmosdb-java/issues/51)).
* Toegevoegde helpermethoden in Conflict Resource.

### <a name="a-name200200"></a><a name="2.0.0"/>2.0.0
* Org.json afhankelijkheid vervangen door jackson vanwege prestatieredenen en licentieverlening ([GitHub #29](https://github.com/Azure/azure-cosmosdb-java/issues/29)).
* Afgeschafte OfferV2 klasse is verwijderd.
* Toegevoegde accessor-methode voor het aanbod klasse voor de inhoud van de doorvoer.
* Een methode in Document/Resource org.json typen gewijzigd om terug te keren een jackson objecttype retourneren.
* methode getObject(.) van klassen uitbreiden JsonSerializable gewijzigd om terug te keren een ObjectNode jackson typt.
* getCollection(.) methode om te retourneren van de verzameling van ObjectNode gewijzigd.
* Verwijderde JsonSerializable subklassen constructors met org.json.JSONObject arg.
* JsonSerializable.toJson (SerializationFormattingPolicy.Indented) maakt nu gebruik van twee spaties voor inspringen.
  
### <a name="a-name102102"></a><a name="1.0.2"/>1.0.2
* Er is ondersteuning toegevoegd voor de unieke Index beleid.
* Ondersteuning toegevoegd voor het beperken van de reactie voortzetting van token grootte in de opties voor invoer.
* Ondersteuning toegevoegd voor partitie splitsen in meerdere Partitiequery.
* Een probleem opgelost in Json-serialisatie voor tijdstempel ([GitHub #32](https://github.com/Azure/azure-cosmosdb-java/issues/32)).
* Een bug opgelost in Json-serialisatie voor enum.
* Een probleem opgelost bij het beheren van documenten van de grootte van 2MB ([GitHub #33](https://github.com/Azure/azure-cosmosdb-java/issues/33)).
* Afhankelijkheid com.fasterxml.jackson.core:jackson-databind bijgewerkt naar 2.9.5 vanwege een fout ([jackson databind: GitHub #1599](https://github.com/FasterXML/jackson-databind/issues/1599))
* Afhankelijkheid van een upgrade uitgevoerd naar 0.8.0.17 vanwege een bug rxjava-extra's ([rxjava-extra's: GitHub #30](https://github.com/davidmoten/rxjava-extras/issues/30)).
* De beschrijving van de metagegevens in het pom-bestand bijgewerkt in verband met inline met de rest van documentatie zijn.
* Syntaxis van de gebruikerservaring ([GitHub #41](https://github.com/Azure/azure-cosmosdb-java/issues/41)), ([GitHub #40](https://github.com/Azure/azure-cosmosdb-java/issues/40)).

### <a name="a-name101101"></a><a name="1.0.1"/>1.0.1
* Toegevoegde ondersteuning voor back-druk in de query.
* Ondersteuning toegevoegd voor partitie-id sleutelbereik in de query.
* Oplossing voor het toestaan van grotere vervolgtoken in de aanvraagheader (bugfix GitHub # 24 uur per dag).
* Een upgrade uitgevoerd naar 4.1.22.Final om ervoor te zorgen JVM-netty afhankelijkheid afgesloten nadat de hoofdthread is voltooid.
* Om te voorkomen dat sessietoken wordt doorgegeven bij het lezen van de master-resources oplossen.
* Meer voorbeelden toegevoegd.
* Meer scenario's voor benchmarking toegevoegd.
* Header-bestanden van de vaste Java voor het genereren van de juiste java doc-bestand.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0
* GA-SDK met end-to-end-ondersteuning voor het gebruik van i/o-niet-blokkerende de [Netty bibliotheek](https://netty.io/) in de modus van de gateway. 

## <a name="release-and-retirement-dates"></a>Release-en buiten gebruik stellen
Microsoft biedt melding ten minste **12 maanden** voorafgaand aan buiten gebruik stellen van een SDK soepel te verwerken de overgang naar een nieuwere/ondersteunde versie.

Nieuwe functies en functionaliteit en -optimalisatie worden alleen toegevoegd aan de huidige SDK. Daarom wordt het aanbevolen dat u altijd een upgrade naar de nieuwste versie van de SDK zo vroeg mogelijk uitvoert.

Een aanvraag voor het Cosmos DB met behulp van een buiten gebruik gestelde SDK worden geweigerd door de service.

<br/>

| Versie | Releasedatum | Vervaldatum |
| --- | --- | --- |
| [2.4.3](#2.4.3) |5 mrt 2019|--- |
| [2.4.2](#2.4.2) |1 maart 2019|--- |
| [2.4.1](#2.4.1) |20 februari 2019|--- |
| [2.4.0](#2.4.0) |8 februari 2019|--- |
| [2.4.0-beta-1](#2.4.0-beta-1) |4 februari 2019|--- |
| [2.3.1](#2.3.1) |15 januari 2019|--- |
| [2.3.0](#2.3.0) |29 november 2018|--- |
| [2.2.2](#2.2.2) |8 november 2018|--- |
| [2.2.1](#2.2.1) |2 november 2018|--- |
| [2.2.0](#2.2.0) |22 september 2018|--- |
| [2.1.0](#2.1.0) |5 september 2018|--- |
| [2.0.1](#2.0.1) |16 augustus 2018|--- |
| [2.0.0](#2.0.0) |20 juni 2018|--- |
| [1.0.2](#1.0.2) |18 mei 2018|--- |
| [1.0.1](#1.0.1) |20 april 2018|--- |
| [1.0.0](#1.0.0) |27 februari, 2018|--- |

## <a name="faq"></a>Veelgestelde vragen
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Zie ook
Zie voor meer informatie over Cosmos DB, [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) servicepagina.

