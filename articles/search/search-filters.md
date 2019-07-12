---
title: Filters voor het vaststellen van zoekresultaten in een index - Azure Search
description: Filteren op beveiligings-id van gebruiker, taal, geo-locatie of numerieke waarden te verminderen van de zoekresultaten op query's in Azure Search, een gehoste cloud search-service op Microsoft Azure.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.topic: conceptual
ms.date: 06/13/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 4b5d198506473c598f058c881f781a06e191df88
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/08/2019
ms.locfileid: "67653441"
---
# <a name="filters-in-azure-search"></a>Filters in Azure Search 

Een *filter* vindt u de criteria voor het selecteren van documenten die worden gebruikt in een Azure Search-query. Zoeken in ongefilterde omvat alle documenten in de index. Een filter het bereik van een zoekopdracht op een subset van documenten. Een filter kan zoeken in volledige tekst bijvoorbeeld beperken tot alleen de producten die met een specifiek merk of de kleur op Prijspunten boven een bepaalde drempelwaarde komt.

Sommige zoekervaringen opleggen filter vereisten als onderdeel van de implementatie, maar u kunt filters gebruiken wanneer u wilt beperken met behulp van search *op basis van een waarde* criteria (scoping zoekt naar producttype 'boeken' categorie' niet-fiction"gepubliceerd door 'Simon & Schuster').

Als het doel in plaats daarvan gerichte zoekopdracht op specifieke gegevens is *structuren* (scoping zoeken naar een veld klant-beoordelingen), er zijn andere methoden, zoals hieronder wordt beschreven.

## <a name="when-to-use-a-filter"></a>Wanneer u een filter

Filters zijn fundamentele naar verschillende zoekervaringen, met inbegrip van ' in mijn buurt zoeken', meervoudige navigatie en beveiliging filters waarin alleen de documenten die een gebruiker is toegestaan om te zien. Als u een van deze ervaringen implementeert, is een filter is vereist. Dit is het filter dat is gekoppeld aan de query die de coördinaten geolocatie, biedt het facet categorie geselecteerd door de gebruiker of de beveiligings-ID van de aanvrager.

Scenario's met voorbeelden omvatten het volgende:

1. Gebruik een filter voor het segmenteren van uw index op basis van gegevenswaarden in de index. Uitgaande van een schema met stad, huisvesting type en faciliteiten, kunt u een filter kunt u documenten die voldoen aan uw criteria (in Seattle, condos, afgebakend) expliciet selecteren maken. 

   Zoeken in volledige tekst met de dezelfde invoer vaak dezelfde resultaten oplevert, maar een filter is nauwkeuriger in die exact overeenkomen met de filterterm op basis van inhoud in uw index moet. 

2. Een filter gebruiken als de zoekervaring wordt geleverd met een vereiste filter:

   * [Facetnavigatie](search-faceted-navigation.md) maakt gebruik van een filter om door te geven weer de facet categorie geselecteerd door de gebruiker.
   * Geografische locaties zoeken maakt gebruik van een filter om door te geven coördinaten van de huidige locatie in 'in mijn buurt zoeken' apps. 
   * Beveiligingsfilters geven beveiligings-id's als de filtercriteria, waarbij een overeenkomst in de index fungeert als een proxy voor de toegangsrechten tot het document.

3. Een filter gebruiken als u wilt dat de zoekcriteria op een numeriek veld. 

   Numerieke velden worden opgehaald in het document en kunnen worden weergegeven in zoekresultaten, maar ze zijn niet afzonderlijk doorzoekbare (afhankelijk van zoeken in volledige tekst). Als u selectiecriteria op basis van numerieke gegevens nodig hebt, gebruikt u een filter.

### <a name="alternative-methods-for-reducing-scope"></a>Alternatieve methoden voor het verminderen van bereik

Als u een beperkende effect in uw lijst met zoekresultaten wilt, zijn filters niet alleen keuze. Deze alternatieven mogelijk beter geschikt zijn, afhankelijk van uw doel:

 + `searchFields` queryparameter wasknijpers zoeken naar specifieke velden. Als uw index afzonderlijke velden voor Engelse en Spaanse beschrijvingen biedt, kunt u bijvoorbeeld searchFields gebruiken om u te richten op welke velden u wilt gebruiken voor zoeken in volledige tekst. 

+ `$select` parameter wordt gebruikt om op te geven welke velden u wilt opnemen in een resultaat ingesteld, het antwoord effectief bijsnijden voordat deze naar de aanroepende toepassing verzonden. Deze parameter niet wordt de query te verfijnen of beperken van de documentverzameling, maar als een kleinere antwoord het doel is, deze parameter is een optie om te overwegen. 

Zie voor meer informatie over de parameter [documenten zoeken > aanvragen > queryparameters](https://docs.microsoft.com/rest/api/searchservice/search-documents#request).


## <a name="how-filters-are-executed"></a>Hoe filters worden uitgevoerd

De expressie converteert naar atomic Booleaanse expressies die worden weergegeven als een boomstructuur en evalueert vervolgens de structuur van de via filterbare velden in een index op het moment dat de query, een filter-parser criteria als invoer accepteert.

Filteren vindt plaats in combinatie met zoeken, in aanmerking komende welke documenten worden opgenomen in de downstream-verwerkingen voor het ophalen van document en relevantie scoren. In combinatie met een zoekreeks, vermindert het filter effectief de set intrekken van de volgende search-bewerking. Als u alleen wordt gebruikt (bijvoorbeeld wanneer de query-tekenreeks leeg waar is `search=*`), de filtercriteria is de enige invoer. 

## <a name="defining-filters"></a>Filters definiëren

Filters zijn OData-expressies, gelede met behulp van een [subset van de OData-V4-syntaxis ondersteund in Azure Search](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search). 

U kunt één filter opgeven voor elke **zoeken** bewerking, maar het filter zelf kan bevatten meerdere velden, meerdere criteria en als u een **ismatch** functie, meerdere expressies voor zoeken in volledige tekst. U kunt in een meerdelige filterexpressie predicaten in willekeurige volgorde (afhankelijk van de regels voor operatoren) opgeven. Er is geen noemenswaardig voordeel in de prestaties als u probeert te predicaten in een bepaalde volgorde rangschikken.

Een van de limieten voor een filterexpressie is de maximale grootte van de aanvraag. De volledige aanvraag, inclusief het filter, mag maximaal 16 MB voor POST of 8 KB voor GET. Er is ook een limiet voor het aantal in de filterexpressie van de EU. Een goede vuistregel is dat als u honderden componenten hebt, u risico van het uitvoeren van de limiet. Het is raadzaam om het ontwerpen van uw toepassing zodanig dat deze filters van niet-gebonden grootte wordt gegenereerd.

De volgende voorbeelden geven prototypische filterdefinities in verschillende API's.

```http
# Option 1:  Use $filter for GET
GET https://[service name].search.windows.net/indexes/hotels/docs?search=*&$filter=baseRate lt 150&$select=hotelId,description&api-version=2019-05-06

# Option 2: Use filter for POST and pass it in the request body
POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2019-05-06
{
    "search": "*",
    "filter": "baseRate lt 150",
    "select": "hotelId,description"
}
```

```csharp
    parameters =
        new SearchParameters()
        {
            Filter = "baseRate lt 150",
            Select = new[] { "hotelId", "description" }
        };

    var results = searchIndexClient.Documents.Search("*", parameters);
```

## <a name="filter-usage-patterns"></a>Filteren van gebruikspatronen

De volgende voorbeelden ziet u verschillende gebruikspatronen voor filter scenario's. Zie voor meer ideeën [syntaxis voor OData-expressie > voorbeelden](https://docs.microsoft.com/azure/search/search-query-odata-filter#examples).

+ Zelfstandige **$filter**, zonder een queryreeks, dit is nuttig wanneer de filterexpressie in staat om volledig te kwalificeren documenten van belang is. Zonder een queryreeks is geen lexicale of linguïstische analyse, geen beoordeling en er is geen classificatie. U ziet dat de zoektekenreeks is alleen een sterretje, wat betekent 'overeenkomstig met alle documenten'.

   ```
   search=*&$filter=(baseRate ge 60 and baseRate lt 300) and accommodation eq 'Hotel' and city eq 'Nogales'
   ```

+ Combinatie van query-tekenreeks en **$filter**, waarbij het filter de subset maakt, en de queryreeks de invoer van de termijn voor zoeken in volledige tekst via de gefilterde subset bevat. Met behulp van een filter met een queryreeks is het meest voorkomende gebruikspatroon.

   ```
   search=hotels ocean$filter=(baseRate ge 60 and baseRate lt 300) and city eq 'Los Angeles'
   ```

+ Samengestelde query's, gescheiden door 'of', elk met een eigen filtercriteria (bijvoorbeeld ' beagles' in 'hond') of 'siamese' in 'cat'. Expressies gecombineerd met `or` afzonderlijk worden geëvalueerd met de samenvoeging van documenten die overeenkomen met elke expressie terug in het antwoord verzonden. Dit gebruikspatroon wordt bereikt door de `search.ismatchscoring` functie. U kunt ook de versie niet scoren `search.ismatch`.

   ```
   # Match on hostels rated higher than 4 OR 5-star motels.
   $filter=search.ismatchscoring('hostel') and rating ge 4 or search.ismatchscoring('motel') and rating eq 5

   # Match on 'luxury' or 'high-end' in the description field OR on category exactly equal to 'Luxury'.
   $filter=search.ismatchscoring('luxury | high-end', 'description') or category eq 'Luxury'
   ```

  Het is ook mogelijk om te zoeken in volledige tekst via combineren `search.ismatchscoring` met filters toepassen met `and` in plaats van `or`, maar dit is functioneel equivalent met behulp van de `search` en `$filter` parameters in een zoekaanvraag. Bijvoorbeeld, opleveren de volgende twee query's hetzelfde resultaat:

  ```
  $filter=search.ismatchscoring('pool') and rating ge 4

  search=pool&$filter=rating ge 4
  ```

Opgevolgd met de volgende artikelen voor uitgebreide richtlijnen specifieke gebruiksvoorbeelden:

+ [Facetfilters](search-filters-facets.md)
+ [Taalfilters](search-filters-language.md)
+ [Beveiligingsbeperkingen](search-security-trimming-for-azure-search.md) 

## <a name="field-requirements-for-filtering"></a>Vereisten voor het filteren van veld

In de REST-API, Filterbaar is *op* standaard voor eenvoudige velden. Bij filterbare velden vergroten index; Zorg ervoor dat u instelt `"filterable": false` voor velden die u niet wilt gebruiken in een filter. Zie voor meer informatie over instellingen voor velddefinities [Create Index](https://docs.microsoft.com/rest/api/searchservice/create-index).

In de .NET SDK, de Filterbaar is *uit* standaard. Kunt u een veld Filterbaar door in te stellen de [IsFilterable eigenschap](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.field.isfilterable?view=azure-dotnet) van de bijbehorende [veld](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.field?view=azure-dotnet) object `true`. U kunt dit ook doen declaratief met behulp van de [IsFilterable kenmerk](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.isfilterableattribute). In het volgende voorbeeld wordt het kenmerk is ingesteld op de `BaseRate` eigenschap van een modelklasse die is toegewezen aan de indexdefinitie.

```csharp
    [IsFilterable, IsSortable, IsFacetable]
    public double? BaseRate { get; set; }
```

### <a name="making-an-existing-field-filterable"></a>Maken van een bestaand veld Filterbaar

U kunt bestaande velden zodat ze Filterbaar niet wijzigen. In plaats daarvan moet u een nieuw veld toevoegt of de index opnieuw opbouwen. Zie voor meer informatie over het opnieuw opbouwen van een index of opnieuw importeert velden [het opnieuw opbouwen van een Azure Search-index](search-howto-reindex.md).

## <a name="text-filter-fundamentals"></a>Tekst filter grondbeginselen

Tekstfilters komt overeen met tekenreeksvelden tegen letterlijke tekenreeksen die u in het filter opgeeft. In tegenstelling tot zoeken in volledige tekst is er geen lexicale analyse of woordafbreking voor tekstfilters, dus vergelijkingen voor alleen exacte overeenkomsten gelden. Bijvoorbeeld, wordt ervan uitgegaan dat een veld *f* 'zonnige dag', bevat `$filter=f eq 'Sunny'` komt niet overeen met, maar `$filter=f eq 'sunny day'` wordt. 

Tekenreeksen zijn hoofdlettergevoelig. Er is geen kleine-hoofdlettergebruik van hoofdletters, woorden: `$filter=f eq 'Sunny day'` 'zonnige dag' niet vinden.

### <a name="approaches-for-filtering-on-text"></a>Methoden voor het filteren op tekst

| Methode | Description | Wanneer gebruikt u dit? |
|----------|-------------|-------------|
| [`search.in`](search-query-odata-search-in-function.md) | Een functie die overeenkomt met een veld op basis van een door tekens gescheiden lijst met tekenreeksen. | Aanbevolen voor [beveiligingsfilters](search-security-trimming-for-azure-search.md) en voor eventuele filters waar veel waarden voor onbewerkte tekst moeten worden vergeleken met een tekenreeksveld. De **search.in** functie is ontworpen voor snelheid en is veel sneller dan het vergelijken van het veld op basis van elke tekenreeks met behulp expliciet `eq` en `or`. | 
| [`search.ismatch`](search-query-odata-full-text-search-functions.md) | Een functie waarmee u kunt zoeken in volledige tekst bewerkingen met strikt Boolean-filter bewerkingen in de dezelfde filterexpressie combineren. | Gebruik **search.ismatch** (of het scoring-equivalent, **search.ismatchscoring**) als u wilt dat meerdere zoekfilter combinaties in één aanvraag. U kunt ook gebruiken voor een *bevat* om te filteren op een gedeeltelijke tekenreeks binnen een grotere tekenreeks. |
| [`$filter=field operator string`](search-query-odata-comparison-operators.md) | De expressie voor een gebruiker gedefinieerde bestaat uit velden, operators en waarden. | Gebruik deze optie als u wilt zoeken naar exacte overeenkomsten tussen een tekenreeksveld en een string-waarde. |

## <a name="numeric-filter-fundamentals"></a>Grondbeginselen van numerieke filter

Numerieke velden zijn geen `searchable` in de context van zoeken in volledige tekst. Alleen strings worden zoekopdracht in volledige tekst. Bijvoorbeeld, als u 99,99 als een zoekterm invoert, wordt niet krijgt u terug items 99,99 prijs. In plaats daarvan ziet u artikelen die het aantal 99 in tekenreeksvelden van het document hebben. Hebt u een numerieke gegevens, is de veronderstelling dus dat u ze voor filters gebruiken wilt, met inbegrip van bereiken, facetten, groepen, enzovoort. 

Documenten die numerieke velden (prijs, grootte, SKU, -ID) bevatten die waarden in de zoekresultaten opgeven als het veld is gemarkeerd als `retrievable`. Hier is dat zoeken in volledige tekst zelf is niet van toepassing op numeriek veldtypen.

## <a name="next-steps"></a>Volgende stappen

Probeer eerst **Search explorer** in de portal voor het indienen van query's met **$filter** parameters. De [real-goed-voorbeeldindex](search-get-started-portal.md) biedt interessante resultaten voor de volgende query's worden gefilterd wanneer u ze in de zoekbalk plakken:

```
# Geo-filter returning documents within 5 kilometers of Redmond, Washington state
# Use $count=true to get a number of hits returned by the query
# Use $select to trim results, showing values for named fields only
# Use search=* for an empty query string. The filter is the sole input

search=*&$count=true&$select=description,city,postCode&$filter=geo.distance(location,geography'POINT(-122.121513 47.673988)') le 5

# Numeric filters use comparison like greater than (gt), less than (lt), not equal (ne)
# Include "and" to filter on multiple fields (baths and bed)
# Full text search is on John Leclerc, matching on John or Leclerc

search=John Leclerc&$count=true&$select=source,city,postCode,baths,beds&$filter=baths gt 3 and beds gt 4

# Text filters can also use comparison operators
# Wrap text in single or double quotes and use the correct case
# Full text search is on John Leclerc, matching on John or Leclerc

search=John Leclerc&$count=true&$select=source,city,postCode,baths,beds&$filter=city gt 'Seattle'
```

Met meer voorbeelden wilt werken, Zie [OData-Filterexpressiesyntaxis > voorbeelden](https://docs.microsoft.com/azure/search/search-query-odata-filter#examples).

## <a name="see-also"></a>Zie ook

+ [Hoe vol tekstzoekopdrachten werkt in Azure Search](search-lucene-query-architecture.md)
+ [REST-API voor Search-documenten](https://docs.microsoft.com/rest/api/searchservice/search-documents)
+ [Vereenvoudigde querysyntaxis](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search)
+ [Lucene-querysyntaxis](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search)
+ [Ondersteunde gegevenstypen](https://docs.microsoft.com/rest/api/searchservice/supported-data-types)
