---
title: 'Azure Cosmos DB: .NET Change feed processor API, SDK & resources'
description: Meer informatie over de Change feed processor API en SDK, inclusief release datums, pensioen datums en wijzigingen die zijn aangebracht tussen elke versie van de .NET Change feed processor SDK.
author: ealsur
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: reference
ms.date: 01/30/2019
ms.author: maquaran
ms.openlocfilehash: ea6de5f42910457efa5ca6c458d7af63faa38e18
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2019
ms.locfileid: "68637749"
---
# <a name="net-change-feed-processor-sdk-download-and-release-notes"></a>.NET Change feed processor SDK: Down load en release-opmerkingen

> [!div class="op_single_selector"]
>
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
> * [Bulk-uitvoerder-.NET](sql-api-sdk-bulk-executor-dot-net.md)
> * [Bulk-uitvoerder-java](sql-api-sdk-bulk-executor-java.md)

|   |   |
|---|---|
|**SDK downloaden**|[NuGet](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/)|
|**API-documentatie**|[Documentatie voor de feed processor library API-verwijzing](/dotnet/api/microsoft.azure.documents.changefeedprocessor?view=azure-dotnet)|
|**Aan de slag**|[Aan de slag met de wijziging van de .NET SDK voor de feed-processor](change-feed.md)|
|**Huidige ondersteunde framework**| [Microsoft .NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653)</br> [Microsoft .NET Core](https://www.microsoft.com/net/download/core) |

## <a name="release-notes"></a>Releaseopmerkingen

### <a name="v2-builds"></a>v2-builds

### <a name="a-name227227"></a><a name="2.2.7"/>2.2.7
* De strategie voor taak verdeling is verbeterd wanneer het ophalen van alle leases langer duurt dan het verloop interval van de lease, bijvoorbeeld door netwerk problemen:
  * In dit scenario wordt het taakverdelings algoritme gebruikt voor het onterecht beschouwen van leases als verlopen, waardoor leases van actieve eigen aars worden gestolen. Dit kan onnodig nieuwe onderverdeling van een groot aantal leases tot gevolg hebben.
  * Dit probleem wordt opgelost in deze release door te voor komen dat er een conflict optreedt tijdens het ophalen van een verlopen lease die de eigenaar niet heeft gewijzigd en posponing de verouderde lease aan de volgende taak verdelings iteratie te verkrijgen.

### <a name="a-name226226"></a><a name="2.2.6"/>2.2.6
* Verbeterde verwerking van uitzonde ringen voor waarnemers.
* Uitgebreide informatie over waarnemers fouten:
  * Wanneer een waarnemer is gesloten vanwege een uitzonde ring die is opgetreden door de ProcessChangesAsync van de waarnemer, ontvangt de CloseAsync nu de para meter reason die is ingesteld op ChangeFeedObserverCloseReason. ObserverError.
  * Er zijn traceringen toegevoegd om fouten binnen de gebruikers code in een waarnemer te identificeren.

### <a name="a-name225225"></a><a name="2.2.5"/>2.2.5
* Er is ondersteuning toegevoegd voor het verwerken van splitsen in verzamelingen die gebruikmaken van de door Voer van een gedeelde data base.
  * Deze release corrigeert een probleem dat zich kan voordoen tijdens het splitsen in verzamelingen met behulp van de door Voer van een gedeelde data base wanneer het resultaat wordt gesplitst in de herverdeling van partities waarbij slechts één onderliggend partitie sleutel bereik is gemaakt in plaats van twee. Als dit gebeurt, kan het zijn dat de feed-processor niet meer kan worden verwijderd omdat de lease voor een oud partitie sleutel bereik niet wordt gemaakt. Het probleem is opgelost in deze release.

### <a name="a-name224224"></a><a name="2.2.4"/>2.2.4
* De nieuwe eigenschap ChangeFeedProcessorOptions. StartContinuation is toegevoegd ter ondersteuning van het starten van een wijzigings feed vanuit een vervolg token voor aanvragen. Dit wordt alleen gebruikt wanneer de lease verzameling leeg is of als er voor een lease geen ContinuationToken is ingesteld. Voor leases in een lease verzameling waarvoor ContinuationToken is ingesteld, wordt de ContinuationToken gebruikt en wordt ChangeFeedProcessorOptions. StartContinuation genegeerd.

### <a name="a-name223223"></a><a name="2.2.3"/>2.2.3
* Er is ondersteuning toegevoegd voor het gebruik van een aangepast archief om vervolg tokens per partitie te behouden.
  * Zo kan een aangepast lease archief bijvoorbeeld worden Azure Cosmos DB lease-verzameling die op een aangepaste manier is gepartitioneerd.
  * Aangepaste lease archieven kunnen nieuwe uitbreidings punten ChangeFeedProcessorBuilder. WithLeaseStoreManager (ILeaseStoreManager) en ILeaseStoreManager open bare interface gebruiken.
  * De ILeaseManager-interface is in meerdere functie-interfaces gefactord.
* Secundaire breuk wijziging: ChangeFeedProcessorBuilder. WithLeaseManager (ILeaseManager) van uitbreidings punten verwijderd, gebruik in plaats daarvan ChangeFeedProcessorBuilder. WithLeaseStoreManager (ILeaseStoreManager).

### <a name="a-name222222"></a><a name="2.2.2"/>2.2.2
* Deze release corrigeert een probleem dat zich voordoet tijdens het verwerken van een splitsing in bewaakte verzameling en het gebruik van een gepartitioneerde lease verzameling. Bij het verwerken van een lease voor gesplitste partitie, mag de lease die overeenkomt met die partitie, niet worden verwijderd. Het probleem is opgelost in deze release.

### <a name="a-name221221"></a><a name="2.2.1"/>2.2.1
* Vaste Estimator-berekening voor Multi Master-accounts en de indeling van het nieuwe sessie token.

### <a name="a-name220220"></a><a name="2.2.0"/>2.2.0
* Er is ondersteuning toegevoegd voor gepartitioneerde lease verzamelingen. De partitie sleutel moet worden gedefinieerd als/id.
* Secundaire breuk wijziging: de methoden van de IChangeFeedDocumentClient-interface en de ChangeFeedDocumentClient-klasse zijn gewijzigd zodat deze de RequestOptions-en CancellationToken-para meters bevatten. IChangeFeedDocumentClient is een geavanceerd uitbreidings punt waarmee u een aangepaste implementatie van de document-client kunt leveren voor gebruik met de processor voor wijzigings invoer, bijvoorbeeld DocumentClient en alle aanroepen naar de-server onderschept om extra tracering uit te voeren, fout afhandeling Heap. Met deze update moet de code die IChangeFeedDocumentClient implementeert, worden gewijzigd zodat er nieuwe para meters in de implementatie worden toegevoegd.
* Lagere verbeteringen voor diagnostische gegevens.

### <a name="a-name210210"></a><a name="2.1.0"/>2.1.0
* Er is een nieuwe API&lt;(&lt;taak&gt; IReadOnlyList RemainingPartitionWork&gt; IRemainingWorkEstimator. GetEstimatedRemainingWorkPerPartitionAsync) toegevoegd. Dit kan worden gebruikt om de geschatte hoeveelheid werk voor elke partitie te verkrijgen.
* Supports Microsoft.Azure.DocumentDB SDK 2.0. Vereist micro soft. Azure. DocumentDB 2,0 of hoger.

### <a name="a-name206206"></a><a name="2.0.6"/>2.0.6
* De open bare eigenschap ChangeFeedEventHost. HostName is toegevoegd voor compatibiliteit met v1.

### <a name="a-name205205"></a><a name="2.0.5"/>2.0.5
* Er is een race voorwaarde opgelost die plaatsvindt tijdens het splitsen van de partitie. De race voorwaarde kan leiden tot het verkrijgen van een lease en deze onmiddellijk verliezen tijdens het splitsen van partities en leidt tot conflicten. Het probleem met de race voorwaarde is opgelost met deze release.

### <a name="a-name204204"></a><a name="2.0.4"/>2.0.4
* GA-SDK

### <a name="a-name203-prerelease203-prerelease"></a><a name="2.0.3-prerelease"/>2.0.3-Prerelease
* De volgende problemen zijn verholpen:
  * Wanneer er een partitie wordt gesplitst, kan er dubbele verwerking worden uitgevoerd van documenten die zijn gewijzigd vóór de splitsing.
  * De GetEstimatedRemainingWork-API heeft 0 geretourneerd wanneer er geen leases in de lease verzameling aanwezig waren.

* De volgende uitzonde ringen worden openbaar gemaakt. Uitbrei dingen die IPartitionProcessor implementeren, kunnen deze uitzonde ringen genereren.
  * Microsoft.Azure.Documents.ChangeFeedProcessor.Exceptions.LeaseLostException. 
  * Microsoft.Azure.Documents.ChangeFeedProcessor.Exceptions.PartitionException. 
  * Microsoft.Azure.Documents.ChangeFeedProcessor.Exceptions.PartitionNotFoundException.
  * Microsoft.Azure.Documents.ChangeFeedProcessor.Exceptions.PartitionSplitException. 

### <a name="a-name202-prerelease202-prerelease"></a><a name="2.0.2-prerelease"/>2.0.2-Prerelease
* Kleine wijzigingen in de API:
  * ChangeFeedProcessorOptions. IsAutoCheckpointEnabled is verwijderd dat als verouderd is gemarkeerd.

### <a name="a-name201-prerelease201-prerelease"></a><a name="2.0.1-prerelease"/>2.0.1-Prerelease
* Verbeteringen in stabiliteit:
  * Betere verwerking van de initialisatie van het lease-archief. Wanneer het lease archief leeg is, kan er slechts één exemplaar van de processor worden geïnitialiseerd. de andere worden wacht.
  * Stabielere/efficiënte lease vernieuwing/-release. Het vernieuwen en vrijgeven van een lease met één partitie is onafhankelijk van het vernieuwen van anderen. In v1 die voor alle partities opeenvolgend is uitgevoerd.
* Nieuwe v2-API:
  * Builder-patroon voor flexibele constructie van de processor: de ChangeFeedProcessorBuilder-klasse.
    * Kan elke combi natie van para meters hebben.
    * Kan DocumentClient-exemplaar nemen voor bewaking en/of lease verzameling (niet beschikbaar in v1).
  * IChangeFeedObserver. ProcessChangesAsync gaat nu CancellationToken.
  * IRemainingWorkEstimator-het resterende werk Estimator kan los van de processor worden gebruikt.
  * Nieuwe uitbreidings punten:
    * IPartitionLoadBalancingStrategy: voor aangepaste taak verdeling van partities tussen instanties van de processor.
    * ILease, ILeaseManager-voor aangepaste lease beheer.
    * IPartitionProcessor: voor aangepaste verwerking van wijzigingen op een partitie.
* Logging: maakt gebruik van de [LibLog](https://github.com/damianh/LibLog) -bibliotheek.
* 100% achterwaarts compatibel met v1 API.
* Nieuwe code basis.
* Compatibel met de 1.21.1 en de bovenstaande versie van [SQL .NET SDK](sql-api-sdk-dotnet.md) .

### <a name="v1-builds"></a>v1 bouwt voort

### <a name="a-name133133"></a><a name="1.3.3"/>1.3.3
* Meer logboek registratie toegevoegd.
* Er is een DocumentClient-lek opgelost tijdens het aanroepen van de schatting van het openstaande werk meerdere keren.

### <a name="a-name132132"></a><a name="1.3.2"/>1.3.2
* Oplossingen voor de schatting van werk in behandeling.

### <a name="a-name131131"></a><a name="1.3.1"/>1.3.1
* Stabiliteits verbeteringen.
  * Oplossing voor het verwerken van geannuleerde taken probleem dat kan leiden tot het stoppen van waarnemers op sommige partities.
* Ondersteuning voor hand matige controle punten.
* Compatibel met [SQL .NET SDK](sql-api-sdk-dotnet.md) versie 1,21 en hoger.

### <a name="a-name120120"></a><a name="1.2.0"/>1.2.0
* Voegt ondersteuning toe voor .NET Standard 2,0. Het pakket ondersteunt `netstandard2.0` nu en `net451` Framework-monikers.
* Compatibel met de 1.17.0 en de bovenstaande versie van [SQL .NET SDK](sql-api-sdk-dotnet.md) .
* Compatibel met [SQL .net core SDK](sql-api-sdk-dotnet-core.md) versies 1.5.1 en hoger.

### <a name="a-name111111"></a><a name="1.1.1"/>1.1.1
* Hiermee wordt een probleem opgelost met het berekenen van de schatting van de resterende hoeveelheid werk wanneer de wijzigings feed leeg was of omdat er geen werk in behandeling was.
* Compatibel met de 1.13.2 en de bovenstaande versie van [SQL .NET SDK](sql-api-sdk-dotnet.md) .

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0
* Er is een methode toegevoegd voor het verkrijgen van een schatting van het resterende werk dat moet worden verwerkt in de wijzigings feed.
* Compatibel met de 1.13.2 en de bovenstaande versie van [SQL .NET SDK](sql-api-sdk-dotnet.md) .

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0
* GA-SDK
* Compatibel met de 1.14.1 en onderstaande versies van [SQL .NET SDK](sql-api-sdk-dotnet.md) .

## <a name="release--retirement-dates"></a>Uittredings datums &

Microsoft biedt melding ten minste **12 maanden** voorafgaand aan buiten gebruik stellen van een SDK soepel te verwerken de overgang naar een nieuwere/ondersteunde versie.

Nieuwe functies en functionaliteiten en Optima Lise ringen worden alleen toegevoegd aan de huidige SDK. het wordt daarom aangeraden dat u zo snel mogelijk een upgrade naar de nieuwste SDK-versie uitvoert. 

Een aanvraag voor het Cosmos DB met behulp van een buiten gebruik gestelde SDK worden geweigerd door de service.

<br/>

| Versie | Releasedatum | Vervaldatum |
| --- | --- | --- |
| [2.2.7](#2.2.7) |14 mei 2019 |--- |
| [2.2.6](#2.2.6) |29 januari 2019 |--- |
| [2.2.5](#2.2.5) |13 december 2018 |--- |
| [2.2.4](#2.2.4) |29 november 2018 |--- |
| [2.2.3](#2.2.3) |19 november 2018 |--- |
| [2.2.2](#2.2.2) |31 oktober 2018 |--- |
| [2.2.1](#2.2.1) |24 oktober 2018 |--- |
| [1.3.3](#1.3.3) |08 mei 2018 |--- |
| [1.3.2](#1.3.2) |18 april 2018 |--- |
| [1.3.1](#1.3.1) |13 maart 2018 |--- |
| [1.2.0](#1.2.0) |31 oktober 2017 |--- |
| [1.1.1](#1.1.1) |29 augustus 2017 |--- |
| [1.1.0](#1.1.0) |13 augustus 2017 |--- |
| [1.0.0](#1.0.0) |Juli 07, 2017 |--- |

## <a name="faq"></a>Veelgestelde vragen

[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Zie ook

Zie voor meer informatie over Cosmos DB, [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) servicepagina.
