---
title: Werken met zoek resultaten-Azure Search
description: Structuur en sorteer de zoek resultaten, haal een document telling op en voeg inhouds navigatie toe aan Zoek resultaten in Azure Search.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.devlang: ''
ms.topic: conceptual
ms.date: 06/13/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: bb86a75be464cb78a16170626bc96778d43bb8b6
ms.sourcegitcommit: 6b41522dae07961f141b0a6a5d46fd1a0c43e6b2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/15/2019
ms.locfileid: "67974622"
---
# <a name="how-to-work-with-search-results-in-azure-search"></a>Werken met zoek resultaten in Azure Search
Dit artikel bevat richt lijnen voor het implementeren van standaard elementen van een pagina met zoek resultaten, zoals het totale aantal, het ophalen van documenten, het sorteren van orders en navigatie. Pagina-gerelateerde opties die gegevens of informatie bijdragen aan uw zoek resultaten, worden opgegeven via de [Zoek documenten](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) die naar uw Azure Search-service worden verzonden. 

In de REST API bestaan aanvragen uit een GET-opdracht, een pad en een query parameter die de service op de hoogte stellen van wat er wordt gevraagd en hoe u het antwoord kunt formuleren. In de .NET SDK is de equivalente API de [DocumentSearchResult-klasse](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.documentsearchresult-1).

Enkele voor beelden van code zijn een web-front-end-interface, die u hier kunt vinden: De demo-app en [CognitiveSearchFrontEnd](https://github.com/LuisCabrer/CognitiveSearchFrontEnd)van [New York City-taken](https://azjobsdemo.azurewebsites.net/) .

> [!NOTE]
> Een geldige aanvraag bevat een aantal elementen, zoals een service-URL en pad, HTTP-term `api-version`, enzovoort. Voor het kortings merk zijn de voor beelden verwijderd om alleen de syntaxis te markeren die relevant is voor de paginering. Zie [Azure Search service rest](https://docs.microsoft.com/rest/api/searchservice)(Engelstalig) voor meer informatie over de aanvraag syntaxis.
>

## <a name="total-hits-and-page-counts"></a>Totaal aantal treffers en pagina aantallen

Het totale aantal resultaten dat door een query wordt geretourneerd, wordt weer gegeven en deze resultaten worden geretourneerd in kleinere segmenten, is fundamenteel voor vrijwel alle zoek pagina's.

![][1]

In azure Search gebruikt u de `$count`para meters, `$skip` `$top`, en om deze waarden te retour neren. In het volgende voor beeld ziet u een voorbeeld aanvraag voor het totale aantal treffers in een index met de naam `@odata.count`' online-Catalog ', geretourneerd als:

    GET /indexes/online-catalog/docs?$count=true

Documenten ophalen in groepen van 15 en ook de totale treffers weer geven, beginnend bij de eerste pagina:

    GET /indexes/online-catalog/docs?search=*&$top=15&$skip=0&$count=true

Paginating-resultaten vereisen `$top` beide `$skip`en, `$top` waar geeft aan hoeveel items in een batch moeten worden geretourneerd `$skip` en geeft aan hoeveel items moeten worden overgeslagen. In het volgende voor beeld toont elke pagina de volgende 15 items, aangegeven door de incrementele sprongen in de `$skip` para meter.

    GET /indexes/online-catalog/docs?search=*&$top=15&$skip=0&$count=true

    GET /indexes/online-catalog/docs?search=*&$top=15&$skip=15&$count=true

    GET /indexes/online-catalog/docs?search=*&$top=15&$skip=30&$count=true

## <a name="layout"></a>Layout

Op een pagina met zoek resultaten wilt u mogelijk een miniatuur afbeelding, een subset van velden en een koppeling naar een volledige product pagina weer geven.

 ![][2]

In azure Search gebruikt `$select` u en een [Zoek opdracht-API-aanvraag](https://docs.microsoft.com/rest/api/searchservice/search-documents) om deze ervaring te implementeren.

Een subset van velden voor een tegel indeling retour neren:

    GET /indexes/online-catalog/docs?search=*&$select=productName,imageFile,description,price,rating

Afbeeldingen en media bestanden kunnen niet rechtstreeks worden doorzocht en moeten worden opgeslagen in een ander opslag platform, zoals Azure Blob Storage, om de kosten te verlagen. In de index en documenten definieert u een veld waarin het URL-adres van de externe inhoud wordt opgeslagen. U kunt vervolgens het veld gebruiken als verwijzing naar een afbeelding. De URL naar de afbeelding moet in het document staan.

Als u een product beschrijvings pagina voor  een geonclick gebeurtenis wilt ophalen, gebruikt u [opzoek document](https://docs.microsoft.com/rest/api/searchservice/Lookup-Document) voor het door geven van de sleutel van het document dat moet worden opgehaald. Het gegevens type van de sleutel is `Edm.String`. In dit voor beeld is dit *246810*.

    GET /indexes/online-catalog/docs/246810

## <a name="sort-by-relevance-rating-or-price"></a>Sorteren op relevantie, waardering of prijs

Sorteer orders vaak standaard voor relevantie, maar het is gebruikelijk om alternatieve Sorteer volgorden beschikbaar te maken zodat klanten snel bestaande resultaten in een andere rang orde kunnen omzetten.

 ![][3]

In azure Search is sorteren gebaseerd op de `$orderby` expressie, voor alle velden die zijn geïndexeerd als `"Sortable": true.` een `$orderby` -component is een OData-expressie. Zie de [syntaxis van de OData-expressie voor filters en order-by-componenten](query-odata-filter-orderby-syntax.md)voor meer informatie over de syntaxis.

Relevantie wordt sterk geassocieerd met Score profielen. U kunt de standaard Score gebruiken, die afhankelijk is van tekst analyse en statistische gegevens om de volg orde van alle resultaten te rangschikken, met hogere scores naar documenten met meer of betere overeenkomsten voor een zoek term.

Alternatieve Sorteer volgorden worden meestal gekoppeld aan **OnClick** -gebeurtenissen die terugkeren naar een methode die de sorteer volgorde bouwt. Op basis van dit pagina-element:

 ![][4]

U maakt een methode die de geselecteerde sorteer optie als invoer accepteert en retourneert een geordende lijst voor de criteria die aan die optie zijn gekoppeld.

 ![][5]

> [!NOTE]
> Hoewel de standaard Score voldoende is voor veel scenario's, raden we u aan om in plaats daarvan relevantie op een aangepast Score profiel te baseren. Een aangepast Score profiel biedt een manier om items te stimuleren die nuttiger zijn voor uw bedrijf. Zie [Score profielen toevoegen](index-add-scoring-profiles.md) voor meer informatie.
>

## <a name="faceted-navigation"></a>Facetnavigatie

Zoek navigatie is gebruikelijk op een resultaten pagina die zich vaak aan de zijkant of boven aan een pagina bevindt. In Azure Search heeft facet navigatie een zelfgestuurde zoek opdracht op basis van vooraf gedefinieerde filters. Zie [facet navigatie in azure Search](search-faceted-navigation.md) voor meer informatie.

## <a name="filters-at-the-page-level"></a>Filters op pagina niveau

Als uw oplossing speciaal speciale zoek pagina's voor specifieke inhouds typen bevat (bijvoorbeeld een online retail-toepassing met afdelingen die boven aan de pagina staan), kunt u een [filter expressie](search-filters.md) invoegen naast een **OnClick** -gebeurtenis aan Open een pagina in een vooraf gefilterde status.

U kunt een filter met of zonder een zoek expressie verzenden. Met de volgende aanvraag wordt bijvoorbeeld op de merk naam gefilterd, waarbij alleen de documenten worden geretourneerd die overeenkomen.

    GET /indexes/online-catalog/docs?$filter=brandname eq 'Microsoft' and category eq 'Games'

Zie [zoeken naar documenten (Azure Search-API)](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) voor meer `$filter` informatie over expressies.

## <a name="see-also"></a>Zie ook

- [Azure Search-service REST API](https://docs.microsoft.com/rest/api/searchservice)
- [Index bewerkingen](https://docs.microsoft.com/rest/api/searchservice/Index-operations)
- [Document bewerkingen](https://docs.microsoft.com/rest/api/searchservice/Document-operations)
- [Facet navigatie in Azure Search](search-faceted-navigation.md)

<!--Image references-->
[1]: ./media/search-pagination-page-layout/Pages-1-Viewing1ofNResults.PNG
[2]: ./media/search-pagination-page-layout/Pages-2-Tiled.PNG
[3]: ./media/search-pagination-page-layout/Pages-3-SortBy.png
[4]: ./media/search-pagination-page-layout/Pages-4-SortbyRelevance.png
[5]: ./media/search-pagination-page-layout/Pages-5-BuildSort.png
