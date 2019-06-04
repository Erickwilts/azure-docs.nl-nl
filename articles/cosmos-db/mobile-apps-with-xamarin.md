---
title: Mobiele toepassingen maken met Xamarin en Azure Cosmos DB
description: Een zelfstudie waarmee u een Xamarin iOS-, Android- of Forms-toepassing maakt met behulp van Azure Cosmos DB. Azure Cosmos DB is een snelle clouddatabase op wereldwijde schaal voor mobiele apps.
author: SnehaGunda
ms.service: cosmos-db
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 11/15/2018
ms.author: sngun
ms.openlocfilehash: a9b1952ed981b0ec522ff3bd607bbef275ff93b8
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66479629"
---
# <a name="build-mobile-applications-with-xamarin-and-azure-cosmos-db"></a>Mobiele toepassingen maken met Xamarin en Azure Cosmos DB

> [!div class="op_single_selector"]
> * [.NET](sql-api-dotnet-application.md)
> * [Java](sql-api-java-application.md)
> * [Node.js](sql-api-nodejs-application.md)
> * [Python](sql-api-python-application.md)
> * [Xamarin](mobile-apps-with-xamarin.md)
> 

De meeste mobiele apps moeten gegevens opslaan in de cloud en Azure Cosmos DB is een clouddatabase voor mobiele apps. Het biedt alles wat een mobiele ontwikkelaar nodig heeft. Het is een volledig beheerde Database as a Service waarvan de schaal op aanvraag kan worden aangepast. Het brengt uw gegevens op transparante wijze naar uw toepassing, waar ter wereld uw gebruikers zich ook bevinden. Door gebruik te maken van de [Azure Cosmos DB .NET Core SDK](sql-api-sdk-dotnet-core.md) kunt u mobiele Xamarin-apps inschakelen om rechtstreeks te werken met Azure Cosmos DB, zonder een middelste laag.

In dit artikel vindt u een zelfstudie voor het bouwen van mobiele apps met Xamarin en Azure Cosmos DB. U vindt de volledige broncode voor de zelfstudie op [Xamarin en Azure Cosmos DB op GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/xamarin), inclusief instructies voor het beheren van gebruikers en machtigingen.

## <a name="azure-cosmos-db-capabilities-for-mobile-apps"></a>Mogelijkheden van Azure Cosmos DB voor mobiele apps
Azure Cosmos DB biedt de volgende belangrijke mogelijkheden voor ontwikkelaars van mobiele apps:

![Mogelijkheden van Azure Cosmos DB voor mobiele apps](media/mobile-apps-with-xamarin/documentdb-for-mobile.png)

* Uitgebreide query's via schemaloze gegevens. Azure Cosmos DB slaat gegevens op als schemaloze JSON-documenten in heterogene verzamelingen. Het biedt [uitgebreide en snelle query's](how-to-sql-query.md) zonder u zorgen te hoeven maken over schema's of indexen.
* Snelle doorvoer. Het duurt slechts enkele milliseconden om documenten te lezen en te schrijven met Azure Cosmos DB. Ontwikkelaars kunnen de doorvoer selecteren die ze nodig hebben. Azure Cosmos DB biedt ze dan een SLA voor een beschikbaarheid van 99,99% voor alle accounts voor één regio en alle accounts voor meerdere regio's met soepele consistentie en leesbeschikbaarheid van 99,999% voor alle databaseaccounts voor meerdere regio's.
* Schalen zonder enige beperking. Uw Azure Cosmos DB-verzamelingen [groeien naarmate uw app groeit](partition-data.md). U kunt beginnen met een kleine gegevensgrootte en honderden aanvragen per seconde doorvoeren. Uw verzamelingen of databases kunnen uitgroeien tot vele petabytes aan gegevens en bijbehorende grote doorvoer met honderden miljoenen aanvragen per seconde.
* Wereldwijd verspreid. Gebruikers van mobiele apps zijn onderweg, overal ter wereld. Azure Cosmos DB is een [wereldwijd gedistribueerde database](distribute-data-globally.md). Klik op de kaart om uw gegevens toegankelijk te maken voor uw gebruikers.
* Ingebouwde uitgebreide autorisatie. Met Azure Cosmos DB kunt u eenvoudig populaire patronen implementeren zoals [gegevens per gebruiker](https://aka.ms/documentdb-xamarin-todouser) of gedeelde gegevens van meerdere gebruikers, zonder ingewikkelde aangepaste autorisatiecode.
* Georuimtelijke query's. Veel mobiele apps bieden tegenwoordig ervaringen met geografische context. Met eersteklas ondersteuning voor [georuimtelijke typen](geospatial.md) kunt u met Azure Cosmos DB deze ervaringen heel eenvoudig tot stand brengen.
* Binaire bijlagen. Uw app-gegevens bevatten vaak binaire blobs. Dankzij systeemeigen ondersteuning voor bijlagen kunt u Azure Cosmos DB eenvoudig gebruiken als allesaanbieder voor uw app-gegevens.

## <a name="azure-cosmos-db-and-xamarin-tutorial"></a>Zelfstudie over Azure Cosmos DB en Xamarin
In de volgende zelfstudie leest u hoe u een mobiele toepassing bouwt met Xamarin en Azure Cosmos DB. U vindt de volledige broncode voor de zelfstudie op [Xamarin en Azure Cosmos DB op GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/xamarin).

### <a name="get-started"></a>Aan de slag
U kunt eenvoudig aan de slag met Azure Cosmos DB. Ga naar Azure Portal en maak een nieuw Azure Cosmos DB-account. Klik op het tabblad **Snel starten**. Download het takenlijstvoorbeeld van Xamarin Forms dat al met uw Azure Cosmos DB-account is verbonden. 

![Azure Cosmos DB Snel starten voor mobiele apps](media/mobile-apps-with-xamarin/cosmos-db-quickstart.png)

Als u een bestaande Xamarin-app hebt, kunt u ook het [Azure Cosmos DB NuGet-pakket](sql-api-sdk-dotnet-core.md) toevoegen. Azure Cosmos DB ondersteunt gedeelde bibliotheken voor Xamarin.IOS, Xamarin.Android en Xamarin Forms.

### <a name="work-with-data"></a>Werken met gegevens
Uw gegevensrecords worden opgeslagen in Azure Cosmos DB als schemaloze JSON-documenten in heterogene verzamelingen. U kunt documenten met verschillende structuren opslaan in dezelfde verzameling:

```cs
    var result = await client.CreateDocumentAsync(collectionLink, todoItem);
```

In uw Xamarin-projecten kunt u query's over schemaloze gegevens met geïntegreerde taal gebruiken:

```cs
    var query = await client.CreateDocumentQuery<ToDoItem>(collectionLink)
                    .Where(todoItem => todoItem.Complete == false)
                    .AsDocumentQuery();

    Items = new List<TodoItem>();
    while (query.HasMoreResults) {
        Items.AddRange(await query.ExecuteNextAsync<TodoItem>());
    }
```
### <a name="add-users"></a>Gebruikers toevoegen
Net zoals bij veel Aan de slag-voorbeelden wordt het Azure Cosmos DB-voorbeeld dat u hebt gedownload, geverifieerd bij de service met behulp van een hoofdsleutel die in code is vastgelegd in de code van de app. Deze standaardinstelling is geen goede werkwijze voor een app die u overal wilt uitvoeren, behalve op uw lokale emulator. Als een onbevoegde gebruiker de hoofdsleutel heeft verkregen, kunnen alle gegevens in uw hele Azure Cosmos DB-account zijn getroffen. In plaats daarvan wilt u dat uw app alleen toegang heeft tot de records voor de aangemelde gebruiker. Azure Cosmos DB staat ontwikkelaars toe om lees- of lees-/schrijfmachtigingen voor toepassingen toe te wijzen aan een verzameling, aan een set documenten die middels een partitiesleutel zijn gegroepeerd of aan een specifiek document. 

Volg deze stappen om de takenlijst-app aan te passen aan een takenlijst-app voor meerdere gebruikers: 

  1. Voeg Aanmelding met Facebook, Active Directory of een andere provider toe aan uw app.

  2. Maak een Azure Cosmos DB-UserItems-verzameling met **/userId** als partitiesleutel. Wanneer u de partitiesleutel voor uw verzameling opgeeft, kan de schaal van Azure Cosmos DB oneindig worden aangepast naarmate het aantal app-gebruikers toeneemt, en tegelijkertijd snelle query's blijven aanbieden.

  3. Voeg Azure Cosmos DB Resource Token Broker toe. Deze eenvoudige Web-API verifieert gebruikers en wijst kortdurende tokens toe aan aangemelde gebruikers met alleen toegang tot de documenten binnen hun partitie. In dit voorbeeld wordt Resource Token Broker gehost in App Service.

  4. Pas de app aan om Resource Token Broker te verifiëren met Facebook en vraag de resourcetokens aan voor de aangemelde Facebook-gebruikers. U krijgt vervolgens toegang tot hun gegevens in de UserItems-verzameling.  

U vindt een volledig codevoorbeeld van dit patroon op [Resource Token Broker op GitHub](https://aka.ms/documentdb-xamarin-todouser). In dit diagram wordt de oplossing weergegeven:

![Azure Cosmos DB-gebruikers en machtigingenbroker](media/mobile-apps-with-xamarin/documentdb-resource-token-broker.png)

Als u wilt dat twee gebruikers toegang hebben tot dezelfde takenlijst, kunt u aanvullende machtigingen toevoegen aan het toegangstoken in Resource Token Broker.

### <a name="scale-on-demand"></a>Schalen op aanvraag
Azure Cosmos DB is een beheerde Database as a Service. Wanneer uw gebruikersgroep groeit, hoeft u zich geen zorgen te maken over het inrichten van VM’s of het uitbreiden van het aantal kernen. U hoeft Azure Cosmos DB alleen maar te vertellen hoeveel bewerkingen per seconde (doorvoer) uw app nodig heeft. U kunt de doorvoer opgeven via het tabblad **Schaal aanpassen** met de doorvoermeting Aanvraageenheden (RU’s) per seconde. Voor een leesbewerking van een document van 1 kB is bijvoorbeeld 1 RU vereist. U kunt ook waarschuwingen toevoegen aan de meetwaarde **Doorvoer** om de groei van het verkeer te bewaken en de doorvoer via programmacode te wijzigen wanneer waarschuwingen worden afgegeven.

![Azure Cosmos DB-doorvoer op schaal op aanvraag](media/mobile-apps-with-xamarin/cosmos-db-xamarin-scale.png)

### <a name="go-planet-scale"></a>Wereldwijde schaal
Naarmate uw app aan populariteit wint, krijgt u wellicht gebruikers van over de hele wereld. Of misschien wilt u zich voorbereiden op onvoorziene gebeurtenissen. Ga naar Azure Portal en open uw Azure Cosmos DB-account. Klik op de kaart om uw gegevens continu te laten repliceren naar elk willekeurige aantal regio's wereldwijd. Door deze mogelijkheid worden uw gegevens beschikbaar op de locatie waar uw gebruikers zich bevinden. U kunt ook failoverbeleid toevoegen zodat u bent voorbereid op diverse risico’s.

![De schaal van Azure Cosmos DB aanpassen aan geografische regio's](media/mobile-apps-with-xamarin/cosmos-db-xamarin-replicate.png)

Gefeliciteerd! U hebt de oplossing voltooid en beschikt over een mobiele app met Xamarin en Azure Cosmos DB. Volg vergelijkbare stappen voor het ontwikkelen van Cordova-apps met de Azure Cosmos DB JavaScript SDK en systeemeigen iOS/Android-apps met Azure Cosmos DB REST API’s.

## <a name="next-steps"></a>Volgende stappen
* Bekijk de broncode voor [Xamarin en Azure Cosmos DB op GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/xamarin).
* Download de [Azure Cosmos DB .NET Core SDK](sql-api-sdk-dotnet-core.md).
* Zoek meer codevoorbeelden voor [.NET-toepassingen](sql-api-dotnet-samples.md).
* Lees meer over [de uitgebreide querymogelijkheden van Azure Cosmos DB](how-to-sql-query.md).
* Lees meer over [georuimtelijke ondersteuning in Azure Cosmos DB](geospatial.md).



