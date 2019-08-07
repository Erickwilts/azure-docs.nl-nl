---
title: Problemen vaststellen en oplossen bij het gebruik van Azure Functions trigger voor Cosmos DB
description: Veelvoorkomende problemen, tijdelijke oplossingen en diagnostische stappen bij het gebruik van de Azure Functions trigger voor Cosmos DB
author: ealsur
ms.service: cosmos-db
ms.date: 07/17/2019
ms.author: maquaran
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: 17fa443c3b0113d80a020f2a43c7099cf5a832d2
ms.sourcegitcommit: 4b5dcdcd80860764e291f18de081a41753946ec9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2019
ms.locfileid: "68772903"
---
# <a name="diagnose-and-troubleshoot-issues-when-using-azure-functions-trigger-for-cosmos-db"></a>Problemen vaststellen en oplossen bij het gebruik van Azure Functions trigger voor Cosmos DB

In dit artikel worden veelvoorkomende problemen, tijdelijke oplossingen en diagnostische stappen behandeld wanneer u de [Azure functions trigger voor Cosmos DB](change-feed-functions.md)gebruikt.

## <a name="dependencies"></a>Afhankelijkheden

De Azure Functions trigger en bindingen voor Cosmos DB zijn afhankelijk van de extensie pakketten in de basis Azure Functions runtime. Zorg ervoor dat deze pakketten altijd worden bijgewerkt, omdat deze mogelijk oplossingen en nieuwe functies bevatten die kunnen leiden tot mogelijke problemen die optreden:

* Zie voor Azure Functions v2 [micro soft. Azure. webjobs. Extensions. CosmosDB](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.CosmosDB).
* Zie voor Azure Functions v1 [micro soft. Azure. webjobs. Extensions. DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DocumentDB).

In dit artikel wordt altijd verwezen naar Azure Functions v2 wanneer de runtime wordt vermeld, tenzij expliciet is opgegeven.

## <a name="consume-the-azure-cosmos-db-sdk-independently"></a>De Azure Cosmos DB SDK onafhankelijk gebruiken

De belangrijkste functionaliteit van het uitbreidings pakket is om ondersteuning te bieden voor de Azure Functions trigger en bindingen voor Cosmos DB. Het bevat ook de [Azure Cosmos db .NET SDK](sql-api-sdk-dotnet-core.md). Dit is handig als u via een programma wilt communiceren met Azure Cosmos DB zonder de trigger en bindingen te gebruiken.

Als u de Azure Cosmos DB SDK wilt gebruiken, moet u ervoor zorgen dat u niet nog een NuGet-pakket verwijzing aan uw project toevoegt. In plaats daarvan **kunt u de SDK-referentie oplossen via het uitbreidings pakket van de Azure functions**. De Azure Cosmos DB SDK afzonderlijk van de trigger en bindingen gebruiken

Als u hand matig uw eigen exemplaar van de [Azure Cosmos DB SDK-client](./sql-api-sdk-dotnet-core.md)maakt, moet u ook het patroon volgen van slechts één exemplaar van de client [met behulp van een singleton patroon benadering](../azure-functions/manage-connections.md#documentclient-code-example-c). Dit proces voor komt de potentiële socket problemen in uw bewerkingen.

## <a name="common-scenarios-and-workarounds"></a>Algemene scenario's en tijdelijke oplossingen

### <a name="azure-function-fails-with-error-message-collection-doesnt-exist"></a>De Azure-functie is mislukt omdat de verzameling voor het fout bericht niet bestaat

De Azure-functie is mislukt met het fout bericht: de verzamelings naam van de bron verzameling (in de data base-naam van de database) of de lease verzameling ' Collection2-name ' (in de data base ' Database2-name ') bestaat niet. Beide verzamelingen moeten bestaan voordat de listener wordt gestart. Als u de lease verzameling automatisch wilt maken, stelt u ' CreateLeaseCollectionIfNotExists ' in op ' True '

Dit betekent dat een of beide Azure Cosmos-containers die vereist zijn voor de trigger voor werk, niet bestaan of niet bereikbaar zijn voor de Azure-functie. **De fout zelf vertelt u welke Azure Cosmos-data base en-containers de trigger is die** op basis van uw configuratie moet worden gezocht.

1. Controleer het `ConnectionStringSetting` kenmerk en of dit **verwijst naar een instelling die voor komt in uw Azure functie-app**. De waarde van dit kenmerk mag niet de verbindings reeks zelf zijn, maar de naam van de configuratie-instelling.
2. Controleer of de `databaseName` en `collectionName` aanwezig zijn in uw Azure Cosmos-account. Als u automatische vervanging van waarden (met `%settingName%` patronen) gebruikt, moet u ervoor zorgen dat de naam van de instelling bestaat in uw Azure functie-app.
3. Als u geen opgeeft `LeaseCollectionName/leaseCollectionName`, wordt de standaard waarde leases. Controleer of deze container bestaat. U kunt desgewenst het `CreateLeaseCollectionIfNotExists` kenmerk in de trigger instellen om `true` het automatisch te maken.
4. Controleer de [firewall configuratie van uw Azure Cosmos-account](how-to-configure-firewall.md) om te zien of deze niet de Azure-functie blokkeert.

### <a name="azure-function-fails-to-start-with-shared-throughput-collection-should-have-a-partition-key"></a>De Azure-functie kan niet worden gestart met de verzameling gedeelde door voer moet een partitie sleutel hebben.

De vorige versies van de Azure Cosmos DB-extensie bieden geen ondersteuning voor het gebruik van een lease-container die is gemaakt in een [gedeelde doorvoer database](./set-throughput.md#set-throughput-on-a-database). Als u dit probleem wilt oplossen, werkt u de extensie [micro soft. Azure. webjobs. Extensions. CosmosDB](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.CosmosDB) bij om de nieuwste versie te downloaden.

### <a name="azure-function-fails-to-start-with-the-lease-collection-if-partitioned-must-have-partition-key-equal-to-id"></a>De Azure-functie kan niet worden gestart met ' de lease-verzameling, indien gepartitioneerd, moet een partitie sleutel hebben die gelijk is aan id. '

Deze fout houdt in dat uw huidige leases-container is gepartitioneerd, maar het pad naar `/id`de partitie sleutel niet. Om dit probleem op te lossen, moet u de leases-container `/id` opnieuw maken met als de partitie sleutel.

### <a name="you-see-a-value-cannot-be-null-parameter-name-o-in-your-azure-functions-logs-when-you-try-to-run-the-trigger"></a>U ziet een ' waarde kan niet null zijn. Parameter naam: o "in uw Azure Functions-Logboeken wanneer u probeert de trigger uit te voeren

Dit probleem wordt weer gegeven als u de Azure Portal gebruikt en u probeert de knop **uitvoeren** op het scherm te selecteren bij het inspecteren van een Azure-functie die gebruikmaakt van de trigger. Het is niet nodig om uitvoeren te selecteren om te starten. de trigger wordt automatisch gestart wanneer de Azure-functie wordt geïmplementeerd. Als u de logboek stroom van de Azure function wilt controleren op het Azure Portal, gaat u naar de bewaakte container en voegt u enkele nieuwe items in, dan wordt de trigger die wordt uitgevoerd automatisch weer geven.

### <a name="my-changes-take-too-long-be-received"></a>Het duurt te lang om mijn wijzigingen te ontvangen

Dit scenario kan meerdere oorzaken hebben en moeten allemaal worden gecontroleerd:

1. Is uw Azure-functie geïmplementeerd in dezelfde regio als uw Azure Cosmos-account? Voor een optimale netwerk latentie moeten zowel de Azure-functie als uw Azure Cosmos-account zich in dezelfde Azure-regio bevinden.
2. Zijn de wijzigingen in uw Azure Cosmos-container doorlopend of sporadisch?
Als dit het geval is, kan er een vertraging optreden tussen de wijzigingen die worden opgeslagen en de Azure-functie die ze ophaalt. Dit komt doordat tijdens het uitvoeren van de trigger wordt gecontroleerd op wijzigingen in uw Azure Cosmos-container en er geen in behandeling is om te lezen, de slaap stand gedurende een Configureer bare hoeveelheid tijd (standaard 5 seconden) voordat er wordt gecontroleerd op nieuwe wijzigingen (om te voor komen dat het gebruik van hoge RU wordt uitgevoerd). U kunt deze vertraging configureren via de `FeedPollDelay/feedPollDelay` instelling in de [configuratie](../azure-functions/functions-bindings-cosmosdb-v2.md#trigger---configuration) van de trigger (de waarde wordt verwacht in milliseconden).
3. Uw Azure Cosmos-container kan een [beperkt aantal](./request-units.md)zijn.
4. U kunt het `PreferredLocations` kenmerk in de trigger gebruiken om een door komma's gescheiden lijst van Azure-regio's op te geven voor het definiëren van een aangepaste voorkeurs volgorde voor verbinding.

### <a name="some-changes-are-missing-in-my-trigger"></a>Er ontbreken enkele wijzigingen in de trigger

Als u merkt dat sommige wijzigingen die in de Azure Cosmos-container zijn aangebracht, niet worden opgehaald door de Azure function, is er een eerste onderzoek stap die moet worden uitgevoerd.

Als uw Azure-functie de wijzigingen ontvangt, worden deze vaak verwerkt en kunnen eventueel het resultaat naar een andere bestemming worden verzonden. Wanneer u ontbrekende wijzigingen onderzoekt, moet u **meten welke wijzigingen worden ontvangen op het opname punt** (wanneer de Azure-functie wordt gestart), niet op de bestemming.

Als sommige wijzigingen op de bestemming ontbreken, kan dit betekenen dat er een fout optreedt tijdens de uitvoering van de Azure-functie nadat de wijzigingen zijn ontvangen.

In dit scenario is het mogelijk om een actie toe te voegen `try/catch blocks` in uw code en in de lussen die de wijzigingen kunnen verwerken, om eventuele fouten voor een bepaalde subset van items te detecteren en deze dienovereenkomstig te verwerken (naar een andere opslag locatie sturen). analyse of nieuwe poging). 

> [!NOTE]
> Als er een onverwerkte uitzonde ring is opgetreden tijdens het uitvoeren van de code, wordt de Azure Functions trigger voor Cosmos DB standaard niet opnieuw geprobeerd een batch wijzigingen uit te voeren. Dit betekent dat de wijzigingen niet zijn doorgevoerd op de bestemming omdat u deze niet kunt verwerken.

Als u merkt dat er helemaal geen wijzigingen zijn ontvangen door de trigger, is het meest voorkomende scenario dat er **een andere Azure-functie wordt uitgevoerd**. Het kan een andere Azure-functie zijn die is geïmplementeerd in azure of een Azure-functie die lokaal wordt uitgevoerd op de computer van een ontwikkelaar en die **exact dezelfde configuratie** heeft (dezelfde bewaakte en lease-containers). deze Azure-functie stelen een subset van de wijzigingen die u verwacht dat uw Azure-functie wordt verwerkt.

Daarnaast kunt u het scenario valideren als u weet hoeveel exemplaren van Azure functie-app u uitvoert. Als u uw leases-container inspecteert en het aantal lease-items binnen hebt geteld, moeten de `Owner` afzonderlijke waarden van de eigenschap in deze worden ingesteld op het aantal exemplaren van uw functie-app. Als er meer eigen aren zijn dan de bekende exemplaren van Azure functie-app, betekent dit dat de andere eigen aren de wijzigingen ' stelen ' hebben.

Een eenvoudige manier om deze situatie op te lossen, is `LeaseCollectionPrefix/leaseCollectionPrefix` om een toe te passen op uw functie met een nieuwe/andere waarde of door te testen met een nieuwe lease-container.

### <a name="need-to-restart-and-re-process-all-the-items-in-my-container-from-the-beginning"></a>De items in mijn container moeten opnieuw worden opgestart en opnieuw worden verwerkt vanaf het begin 
Alle items in een container opnieuw verwerken vanaf het begin:
1. Stop uw Azure-functie als deze momenteel wordt uitgevoerd. 
1. De documenten in de lease verzameling verwijderen (of de lease verzameling verwijderen en opnieuw maken, zodat deze leeg is)
1. Stel het kenmerk [StartFromBeginning](../azure-functions/functions-bindings-cosmosdb-v2.md#trigger---configuration) CosmosDBTrigger in uw functie in op waar. 
1. Start de Azure-functie opnieuw. Er worden nu alle wijzigingen van het begin gelezen en verwerkt. 

Als u [StartFromBeginning](../azure-functions/functions-bindings-cosmosdb-v2.md#trigger---configuration) instelt op True, wordt de Azure-functie gestart met het lezen van wijzigingen aan het begin van de geschiedenis van de verzameling in plaats van de huidige tijd. Dit werkt alleen als er geen leases zijn (bijvoorbeeld documenten in de lease-verzameling). Als u deze eigenschap instelt op waar wanneer er al leases zijn gemaakt, heeft dit geen effect. Wanneer een functie wordt gestopt en opnieuw wordt gestart, wordt in dit scenario begonnen met het lezen van het laatste controle punt, zoals gedefinieerd in de verzameling leases. Volg de bovenstaande stappen 1-4 om vanaf het begin opnieuw te verwerken.  

### <a name="binding-can-only-be-done-with-ireadonlylistdocument-or-jarray"></a>U kunt de binding alleen doen met\<IReadOnlyList document > of JArray

Deze fout treedt op als uw Azure Functions project (of een project waarnaar wordt verwezen) een hand matige NuGet-verwijzing naar de Azure Cosmos DB SDK bevat met een andere versie dan die van de [uitbrei ding Azure Functions Cosmos DB](./troubleshoot-changefeed-functions.md#dependencies).

U kunt dit probleem omzeilen door de hand matige NuGet verwijzing die is toegevoegd te verwijderen en de Azure Cosmos DB SDK-referentie op te lossen via het Azure Functions Cosmos DB-uitbreidings pakket.

## <a name="next-steps"></a>Volgende stappen

* [Controle inschakelen voor uw Azure Functions](../azure-functions/functions-monitoring.md)
* [Problemen met Azure Cosmos DB .NET SDK oplossen](./troubleshoot-dot-net-sdk.md)
