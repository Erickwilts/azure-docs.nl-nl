---
title: Een upgrade uitvoert naar de Azure Search .NET SDK versie 5 - Azure Search
description: Code migreren naar Azure Search .NET SDK versie 5 van oudere versies. Ontdek wat er nieuw is en welke wijzigingen in de code zijn vereist.
author: brjohnstmsft
manager: jlembicz
services: search
ms.service: search
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: brjohnst
ms.custom: seodec2018
ms.openlocfilehash: 8382884b4ce2965dee4acf191f82eb012b670713
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65147478"
---
# <a name="upgrading-to-the-azure-search-net-sdk-version-5"></a>Een upgrade naar de Azure Search .NET SDK versie 5

Als u versie 4.0-preview of ouder bent van de [Azure Search .NET SDK](https://aka.ms/search-sdk), in dit artikel helpt u bij het bijwerken van de toepassing kan versie 5 gebruiken.

Zie voor een meer algemeen overzicht van de SDK, inclusief voorbeelden [over het gebruik van Azure Search via een .NET-toepassing](search-howto-dotnet-sdk.md).

Versie 5 van de Azure Search .NET SDK bevat enkele wijzigingen van eerdere versies. Dit zijn doorgaans kleine, zodat u uw code wijzigt, moet alleen minimale inspanning vereisen. Zie [stappen voor het upgraden](#UpgradeSteps) voor instructies over het wijzigen van uw code voor het gebruik van de nieuwe SDK-versie.

> [!NOTE]
> Als u versie 2.0 preview of ouder bent, moet u eerst upgraden naar versie 3 en vervolgens een upgrade naar versie 5. Zie [upgraden naar de Azure Search .NET SDK versie 3](search-dotnet-sdk-migration.md) voor instructies.
>
> Het exemplaar van uw Azure Search-service ondersteunt verschillende versies van de REST-API, met inbegrip van de meest recente versie. U kunt echter ook doorgaan met een versie wanneer het is niet meer de meest recente versie, maar het is raadzaam dat u uw code voor het gebruik van de nieuwste versie migreren. Wanneer u de REST-API gebruikt, moet u de API-versie opgeven in elke aanvraag via de api-versie-parameter. Wanneer u de .NET SDK gebruikt, bepaalt de versie van de SDK die u gebruikt de corresponderende versie van de REST-API. Als u een oudere SDK gebruikt, kunt u blijven om uit te voeren die code zonder wijzigingen, zelfs als de service wordt bijgewerkt ter ondersteuning van een nieuwere API-versie.

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-5"></a>Wat is er nieuw in versie 5
Versie 5 van de Azure Search .NET SDK is gericht op de meest recente algemeen beschikbare versie van de Azure Search REST API, specifiek 2017-11-11. Dit maakt het mogelijk het gebruik van nieuwe functies van Azure Search via een .NET-toepassing, met inbegrip van het volgende:

* [Synoniemen](search-synonyms.md).
* U kunt nu programmatisch toegang verkrijgen tot waarschuwingen in de geschiedenis van indexeerfunctie worden uitgevoerd (Zie de `Warning` eigenschap van `IndexerExecutionResult` in de [naslaginformatie over .NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexerexecutionresult?view=azure-dotnet) voor meer informatie).
* Ondersteuning voor .NET Core 2.
* Nieuwe pakketstructuur ondersteunt het gebruik van alleen de onderdelen van de SDK die u nodig hebt (Zie [belangrijke wijzigingen in versie 5](#ListOfChanges) voor meer informatie).

<a name="UpgradeSteps"></a>

## <a name="steps-to-upgrade"></a>Stappen voor het bijwerken
Update eerst uw NuGet-verwijzing voor `Microsoft.Azure.Search` met behulp van de NuGet Package Manager-Console of door met de rechtermuisknop op uw projectverwijzingen en 'Beheren NuGet-pakketten...' selecteren in Visual Studio.

Nadat NuGet heeft de nieuwe pakketten en de bijbehorende afhankelijkheden gedownload, bouwt u uw project opnieuw. Afhankelijk van hoe uw code is opgebouwd, kan deze opnieuw is. Als dit het geval is, bent u klaar om te beginnen.

Als uw build mislukt, ziet u een build-fout als volgt uit:

    The name 'SuggesterSearchMode' does not exist in the current context

De volgende stap is om deze build-fout te herstellen. Zie [belangrijke wijzigingen in versie 5](#ListOfChanges) voor meer informatie over wat de fout veroorzaakt en hoe u dit herstelt.

Houd er rekening mee dat vanwege wijzigingen in de verpakking van de Azure Search .NET SDK, moet u uw toepassing om te kunnen gebruiken versie 5 opnieuw opbouwen. Deze wijzigingen worden beschreven in [belangrijke wijzigingen in versie 5](#ListOfChanges).

Mogelijk ziet u aanvullende build waarschuwingen met betrekking tot verouderde methoden of eigenschappen. De waarschuwingen bevat instructies over wat u moet gebruiken in plaats van de afgeschafte functies. Als uw toepassing gebruikt bijvoorbeeld de `IndexingParametersExtensions.DoNotFailOnUnsupportedContentType` methode, moet u ontvangt een waarschuwing met de tekst "Dit gedrag is nu standaard ingeschakeld, zodat u deze methode niet meer nodig is."

Als u een build-fouten of waarschuwingen hebt opgelost, kunt u wijzigingen aanbrengen aan uw toepassing om te profiteren van nieuwe functionaliteit als u wenst. Nieuwe functies in de SDK worden beschreven in [wat is er nieuw in versie 5](#WhatsNew).

<a name="ListOfChanges"></a>

## <a name="breaking-changes-in-version-5"></a>Belangrijke wijzigingen in versie 5

### <a name="new-package-structure"></a>Nieuwe pakketstructuur

De meest belangrijke belangrijke wijziging in versie 5 is dat de `Microsoft.Azure.Search` assembly en de inhoud ervan zijn is onderverdeeld in vier afzonderlijke assembly's die nu als vier afzonderlijke NuGet-pakketten worden gedistribueerd:

 - `Microsoft.Azure.Search`: Dit is een meta-pakket met alle andere Azure Search-pakketten als afhankelijkheden. Als u een vanaf een eerdere versie van de SDK upgrade uitvoert, moeten hoeft dit pakket bijwerken en opnieuw het bouwen van voldoende om te beginnen met de nieuwe versie.
 - `Microsoft.Azure.Search.Data`: Dit pakket gebruiken als u een .NET-toepassing met behulp van Azure Search ontwikkelt en u alleen hoeft opvragen of bijwerken van de documenten in uw indexen. Als u ook wilt maken of bijwerken van indexen, synoniementoewijzingen of andere service level-resources, gebruikt de `Microsoft.Azure.Search` in plaats daarvan het pakket.
 - `Microsoft.Azure.Search.Service`: Dit pakket gebruiken als u automation in .NET voor het beheren van Azure Search-index, synoniementoewijzingen, indexeerfuncties, gegevensbronnen of andere bronnen serviceniveau ontwikkelt. Als u alleen query- of update-documenten in uw indexen moet, gebruikt u de `Microsoft.Azure.Search.Data` in plaats daarvan het pakket. Als u de functionaliteit van Azure Search nodig hebt, gebruikt u de `Microsoft.Azure.Search` in plaats daarvan het pakket.
 - `Microsoft.Azure.Search.Common`: Algemene typen die nodig zijn voor de Azure Search .NET-bibliotheken. U hoeft niet op het gebruik van dit pakket rechtstreeks in uw toepassing. Het is alleen bedoeld als een afhankelijkheid moet worden gebruikt.
 
Deze wijziging is technisch belangrijke omdat veel typen zijn verplaatst tussen de assembly's. Dit is de reden waarom het opnieuw opbouwen van uw toepassing is nodig om te upgraden naar versie 5 van de SDK.

Er wordt een klein aantal andere belangrijke wijzigingen in versie 5 waarvoor code gewijzigd naast opnieuw opbouwen van uw toepassing.

### <a name="change-to-suggesters"></a>Wijzig in suggesties 

De `Suggester` constructor heeft niet langer een `enum` parameter voor `SuggesterSearchMode`. Deze opsomming alleen heeft één waarde, en is daarom redundante. Als er fouten als gevolg van dit bouwen, verwijdert u gewoon verwijzingen naar de `SuggesterSearchMode` parameter.

### <a name="removed-obsolete-members"></a>Verouderde leden verwijderd

U ziet mogelijk fouten met betrekking tot methoden of eigenschappen die zijn gemarkeerd als verouderd in eerdere versies en daarna verwijderd in versie 5 bouwen. Als u dergelijke fouten optreden, als volgt op te lossen:

- Als u de `IndexingParametersExtensions.IndexStorageMetadataOnly` methode gebruik `SetBlobExtractionMode(BlobExtractionMode.StorageMetadata)` in plaats daarvan.
- Als u de `IndexingParametersExtensions.SkipContent` methode gebruik `SetBlobExtractionMode(BlobExtractionMode.AllMetadata)` in plaats daarvan.

### <a name="removed-preview-features"></a>Verwijderde preview-functies

Als u een van versie 4.0 preview-versie 5 upgrade, er rekening mee dat JSON-matrix en CSV-ondersteuning voor Blob-indexeerfuncties parseren is verwijderd omdat deze functies zijn nog in preview. Met name de volgende methoden van de `IndexingParametersExtensions` klasse zijn verwijderd:

- `ParseJsonArrays`
- `ParseDelimitedTextFiles`

Als uw toepassing een vaste afhankelijkheid van deze functies heeft, wordt het niet mogelijk om bij te werken naar versie 5 van de Azure Search .NET SDK. U kunt echter ook doorgaan met versie 4.0-preview. Echter Houd er rekening mee dat **we raden niet met behulp van preview SDK's in productietoepassingen**. Preview-functies zijn voor de evaluatie alleen en kunnen worden gewijzigd.

## <a name="conclusion"></a>Conclusie
Als u meer informatie over het gebruik van de Azure Search .NET SDK nodig hebt, raadpleegt u de [.NET procedures](search-howto-dotnet-sdk.md).

We stellen uw feedback over de SDK. Als u problemen ondervindt, gerust ons vragen voor meer informatie over op [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-search). Als u een bug vinden, kunt u het bestand een probleem in de [Azure .NET SDK GitHub-opslagplaats](https://github.com/Azure/azure-sdk-for-net/issues). Zorg ervoor dat u het voorvoegsel van de titel van uw probleem met '[Azure Search]'.

Hartelijk dank voor het gebruik van Azure Search.
