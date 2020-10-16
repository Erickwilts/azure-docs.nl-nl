---
title: Aangepaste Sdk's voor Azure Digital Apparaatdubbels maken met auto rest
titleSuffix: Azure Digital Twins
description: Zie aangepaste Sdk's genereren voor het gebruik van Azure Digital Apparaatdubbels met andere talen dan C#.
author: baanders
ms.author: baanders
ms.date: 4/24/2020
ms.topic: how-to
ms.service: digital-twins
ms.custom: devx-track-js
ms.openlocfilehash: 53887b7487c3f0bb70c9f8cc7cd61246fabc0b37
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/13/2020
ms.locfileid: "91970126"
---
# <a name="create-custom-sdks-for-azure-digital-twins-using-autorest"></a>Aangepaste Sdk's voor Azure Digital Apparaatdubbels maken met auto rest

Op dit moment is het enige gepubliceerde gegevenslaag-Sdk's voor interactie met de Azure Digital Apparaatdubbels Api's voor .NET (C#), java script en Java. Meer informatie over deze Sdk's en de Api's in het algemeen vindt u in de [*instructies: gebruik de Azure Digital Apparaatdubbels api's en sdk's*](how-to-use-apis-sdks.md). Als u in een andere taal werkt, leert u in dit artikel hoe u uw eigen gegevenslaag SDK kunt genereren in de taal van uw keuze, met behulp van auto rest.

>[!NOTE]
> U kunt auto rest ook gebruiken om een Control vlak-SDK te genereren als u dat wilt. U doet dit door de stappen in dit artikel uit te voeren met behulp van het meest recente OpenAPI-bestand ( **Control plan Swagger** ) van de [Control vlak Swagger Folder]] ( https://github.com/Azure/azure-rest-api-specs/tree/master/specification/digitaltwins/resource-manager/Microsoft.DigitalTwins/) in plaats van het gegevens vlak One.

## <a name="set-up-your-machine"></a>Uw machine instellen

Als u een SDK wilt genereren, hebt u het volgende nodig:
* Auto [rest](https://github.com/Azure/autorest), version 2.0.4413 (versie 3 wordt momenteel niet ondersteund)
* [Node.js](https://nodejs.org) als een vereiste voor auto rest
* Het meest recente Azure Digital Apparaatdubbelse **Swagger** -bestand (OpenAPI) van de [Data vlak Swagger-map](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/digitaltwins/data-plane/Microsoft.DigitalTwins)en de bijbehorende map met voor beelden.  Down load het Swagger-bestand *digitaltwins.jsop* en de map met voor beelden naar uw lokale computer.

Zodra de computer is uitgerust met alles uit de bovenstaande lijst, kunt u auto rest gebruiken om de SDK te maken.

## <a name="create-the-sdk-with-autorest"></a>De SDK maken met auto rest 

Als Node.js is geïnstalleerd, kunt u deze opdracht uitvoeren om te controleren of u de juiste versie van auto rest hebt geïnstalleerd:
```cmd/sh
npm install -g autorest@2.0.4413
```

Voer de volgende stappen uit om auto rest te gebruiken voor het Swagger-bestand van de Azure Digital Apparaatdubbels:
1. Kopieer het Azure Digital Apparaatdubbels Swagger-bestand en de bijbehorende map met voor beelden naar een werkmap.
2. Gebruik een opdracht prompt venster om over te scha kelen naar die werkmap.
3. Voer auto rest uit met de volgende opdracht. Vervang de `<language>` tijdelijke aanduiding door de taal van uw keuze: `python` ,, `java` , enzovoort `go` . (U vindt de volledige lijst met opties in het [Leesmij-bestand](https://github.com/Azure/autorest)voor auto rest.)

```cmd/sh
autorest --input-file=digitaltwins.json --<language> --output-folder=ADTApi --add-credentials --azure-arm --namespace=ADTApi
```

Als gevolg hiervan ziet u een nieuwe map met de naam *ADTApi* in uw werkmap. De gegenereerde SDK-bestanden hebben de naam ruimte *ADTApi*. U kunt die naam ruimte blijven gebruiken via de rest van de voor beelden van het gebruik in dit artikel.

Auto rest ondersteunt een breed scala aan taal code generators.

## <a name="add-the-sdk-to-a-visual-studio-project"></a>De SDK toevoegen aan een Visual Studio-project

U kunt de bestanden die worden gegenereerd door autorest rechtstreeks toevoegen aan een .NET-oplossing. Het is echter waarschijnlijk dat u de Azure Digital Apparaatdubbels SDK wilt gebruiken in verschillende afzonderlijke projecten (uw client-apps, Azure Functions-apps, enzovoort). Daarom kan het handig zijn om een afzonderlijk project (een .NET-klassen bibliotheek) te bouwen op basis van de gegenereerde bestanden. Vervolgens kunt u dit klassen bibliotheek project in meerdere oplossingen insluiten als een project verwijzing.

In deze sectie vindt u instructies voor het bouwen van de SDK als een klassen bibliotheek. Dit is een eigen project en kan in andere projecten worden opgenomen. Deze stappen zijn afhankelijk van **Visual Studio** (u kunt de nieuwste versie [hier](https://visualstudio.microsoft.com/downloads/)installeren).

Dit zijn de stappen:

1. Een nieuwe Visual Studio-oplossing maken voor een klassen bibliotheek
2. *ADTApi* gebruiken als de project naam
3. Klik in Solution Explorer met de rechter muisknop op het *ADTApi* -project van de gegenereerde oplossing en kies *> bestaand item toevoegen...*
4. Zoek de map waar u de SDK hebt gegenereerd en selecteer de bestanden op het hoofd niveau
5. Klik op OK
6. Voeg een map toe aan het project (Klik met de rechter muisknop op het project in Solution Explorer en kies *> nieuwe map toevoegen*)
7. De mappen *modellen* een naam
8. Klik met de rechter muisknop op de map *modellen* in Solutions Explorer en selecteer *> bestaand item toevoegen...*
9. Selecteer de bestanden in de map *modellen* van de gegenereerde SDK en druk op OK

Als u de SDK wilt bouwen, moet uw project over de volgende verwijzingen beschikken:
* `Microsoft.Rest.ClientRuntime`
* `Microsoft.Rest.ClientRuntime.Azure`

Als u deze wilt toevoegen, opent u *Hulpprogram ma's > NuGet Package Manager > NuGet-pakketten beheren voor oplossing...*.

1. Controleer in het deel venster of het tabblad *Bladeren* is geselecteerd
2. Zoek naar *micro soft. rest*
3. Selecteer de `ClientRuntime` pakketten en en `ClientRuntime.Azure` Voeg deze toe aan uw oplossing

U kunt nu het project bouwen en dit toevoegen als een project verwijzing in een Azure Digital Apparaatdubbels-toepassing die u schrijft.

## <a name="general-guidelines-for-generated-sdks"></a>Algemene richt lijnen voor gegenereerde Sdk's

Deze sectie bevat algemene informatie en richt lijnen voor het gebruik van de gegenereerde SDK.

### <a name="synchronous-and-asynchronous-calls"></a>Synchrone en asynchrone aanroepen

Alle SDK-functies zijn beschikbaar in synchrone en asynchrone versies.

### <a name="typed-and-untyped-data"></a>Getypte en niet-getypte gegevens

REST API-oproepen retour neren doorgaans sterk getypte objecten. Maar omdat gebruikers met Azure Digital Apparaatdubbels hun eigen aangepaste typen kunnen definiëren voor apparaatdubbels, is het niet mogelijk om statische retour gegevens te definiëren voor veel Azure Digital Apparaatdubbels-aanroepen. In plaats daarvan retour neren de Api's sterk getypeerde wrapper types, indien van toepassing, en de aangepaste, getypte dubbele gegevens bevindt zich in Json.NET-objecten (gebruikt wanneer het gegevens type ' object ' wordt weer gegeven in de API-hand tekeningen). U kunt deze objecten op de juiste wijze casten in uw code.

### <a name="error-handling"></a>Foutafhandeling

Wanneer er een fout optreedt in de SDK (inclusief HTTP-fouten zoals 404), wordt er een uitzonde ring gegenereerd door de SDK. Daarom is het belang rijk om alle API-aanroepen te integreren binnen try/catch-blokken.

Hier volgt een code fragment dat probeert een dubbele fout op te tellen en eventuele fouten in dit proces te ondervangen:

```csharp
try
{
    await client.DigitalTwins.AddAsync(id, initData);
    Console.WriteLine($"Created a twin successfully: {id}");
}
catch (ErrorResponseException e)
{
    Console.WriteLine($"*** Error creating twin {id}: {e.Response.StatusCode}"); 
}
```

### <a name="paging"></a>Zoekresultaten oproepen

Auto rest genereert twee typen paginerings patronen voor de SDK:
* Een voor alle Api's, met uitzonde ring van de query-API
* Een voor de query-API

In de niet-query paginerings patroon zijn er twee versies van elke aanroep:
* Een versie om de eerste aanroep te maken (bijvoorbeeld `DigitalTwins.ListEdges()` )
* Een versie om de volgende pagina's op te halen. Deze aanroepen hebben het achtervoegsel ' volgende ' (bijvoorbeeld `DigitalTwins.ListEdgesNext()` )

Hier volgt een code fragment waarin wordt getoond hoe u een lijst met uitgaande relaties van Azure Digital Apparaatdubbels ophaalt:
```csharp
try
{
    // List to hold the results in
    List<object> relList = new List<object>();
    // Enumerate the IPage object returned to get the results
    // ListAsync will throw if an error occurs
    IPage<object> relPage = await client.DigitalTwins.ListEdgesAsync(id);
    relList.AddRange(relPage);
    // If there are more pages, the NextPageLink in the page is set
    while (relPage.NextPageLink != null)
    {
        // Get more pages...
        relPage = await client.DigitalTwins.ListEdgesNextAsync(relPage.NextPageLink);
        relList.AddRange(relPage);
    }
    Console.WriteLine($"Found {relList.Count} relationships on {id}");
    // Do something with each object found
    // As relationships are custom types, they are JSON.Net types
    foreach (JObject r in relList)
    {
        string relId = r.Value<string>("$edgeId");
        string relName = r.Value<string>("$relationship");
        Console.WriteLine($"Found relationship {relId} from {id}");
    }
}
catch (ErrorResponseException e)
{
    Console.WriteLine($"*** Error retrieving relationships on {id}: {e.Response.StatusCode}");
}
```

Het tweede patroon wordt alleen gegenereerd voor de query-API. Er wordt een `continuationToken` expliciete methode gebruikt.

Hier volgt een voor beeld met dit patroon:

```csharp
string query = "SELECT * FROM digitaltwins";
string conToken = null; // continuation token from the query
int page = 0;
try
{
    // Repeat the query while there are pages
    do
    {
        QuerySpecification spec = new QuerySpecification(query, conToken);
        QueryResult qr = await client.Query.QueryTwinsAsync(spec);
        page++;
        Console.WriteLine($"== Query results page {page}:");
        if (qr.Items != null)
        {
            // Query returns are JObjects
            foreach(JObject o in qr.Items)
            {
                string twinId = o.Value<string>("$dtId");
                Console.WriteLine($"  Found {twinId}");
            }
        }
        Console.WriteLine($"== End query results page {page}");
        conToken = qr.ContinuationToken;
    } while (conToken != null);
} catch (ErrorResponseException e)
{
    Console.WriteLine($"*** Error in twin query: ${e.Response.StatusCode}");
}
```

## <a name="next-steps"></a>Volgende stappen

Door loop de stappen voor het maken van een client-app waarin u uw SDK kunt gebruiken:
* [*Zelfstudie: Een client-app coderen*](tutorial-code.md)