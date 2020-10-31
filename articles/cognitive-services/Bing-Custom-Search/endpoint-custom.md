---
title: Bing Aangepaste zoekopdrachten-eindpunt
titleSuffix: Azure Cognitive Services
description: Maak aangepaste zoek functies voor onderwerpen die u bevalt. Gebruikers zien Zoek resultaten die zijn afgestemd op de inhoud die ze bezorgen.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: conceptual
ms.date: 03/04/2019
ms.author: aahi
ms.openlocfilehash: a7aa2b75a6f6e95bdeff4176ee2ad852de3d7169
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2020
ms.locfileid: "93090695"
---
# <a name="custom-search"></a>Aangepast zoeken

> [!WARNING]
> Bing Zoeken-API's van Cognitive Services naar Bing Search-Services verplaatsen. Vanaf **30 oktober 2020** moeten nieuwe exemplaren van Bing Search worden ingericht volgens het proces dat [hier](https://aka.ms/cogsvcs/bingmove)wordt beschreven.
> Bing Zoeken-API's ingericht met Cognitive Services wordt voor de komende drie jaar of tot het einde van uw Enterprise Agreement ondersteund, afhankelijk van wat het eerst gebeurt.
> Zie [Bing Search Services](https://aka.ms/cogsvcs/bingmigration)voor migratie-instructies.
Met Bing Aangepaste zoekopdrachten kunt u op maat gemaakte zoekervaringen maken voor onderwerpen die u interesseren. Uw gebruikers zien dan zoekresultaten die zijn afgestemd op de inhoud die ze interessant vinden in plaats van dat ze door pagina's met zoekresultaten moeten bladeren die niet-relevante inhoud bevatten.

## <a name="custom-search-endpoint"></a>Aangepast Zoek eindpunt
Als u resultaten wilt ophalen met behulp van de Bing Custom Search-API, stuurt `GET` u een aanvraag naar het volgende eind punt. Gebruik de para meters headers en URL om verdere specificaties te definiëren.

Eind punt: retourneert Zoek suggesties als JSON-resultaten die relevant zijn voor de invoer van de gebruiker die is gedefinieerd door `?q=""` .
```  
 GET https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search  
```

Zie de [zelf studie](https://docs.microsoft.com/azure/cognitive-services/bing-custom-search/tutorials/custom-search-web-page)voor voor beelden waarin wordt beschreven hoe u aangepaste zoek bronnen instelt. Zie de naslag informatie over [Bing Custom Search-API V7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-custom-search-api-v7-reference) voor meer informatie over kopteksten, para meters, markt codes, reactie objecten, fouten, enzovoort.

## <a name="custom-search-response-json"></a>JSON voor aangepast Zoek antwoord
Een aangepaste zoek opdracht retourneert resultaten als JSON-objecten Zie [Response Objects](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-custom-search-api-v7-reference#response-objects). 

## <a name="custom-autosuggest"></a>Aangepaste automatische suggesties
Met de aangepaste automatische suggestie-API kunt u een gedeeltelijke Zoek query term naar Bing verzenden en een lijst met voorgestelde query's weer geven die u kunt configureren. Met aangepaste automatische suggesties voegt u suggesties toe die door de API worden geretourneerd en geeft u optioneel op of u suggesties wilt toevoegen die door Bing worden gegenereerd.

## <a name="custom-autosuggest-endpoint"></a>Aangepaste automatische suggestie-eind punt
Als u aangepaste query suggesties wilt aanvragen, stuurt u een GET-aanvraag naar:

```
https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/Suggestions
```  

Zie [aangepaste zoek suggesties definiëren](define-custom-suggestions.md)voor meer informatie over het definiëren van aangepaste suggesties.

## <a name="custom-image-search"></a>Aangepaste Afbeeldingen zoeken
Met de aangepaste Afbeeldingen zoeken-API kunt u een zoek opdracht naar Bing verzenden en een lijst met relevante installatie kopieën ophalen uit uw aangepaste zoek exemplaar.

## <a name="custom-image-search-endpoint"></a>Aangepast Afbeeldingen zoeken-eind punt
Als u afbeeldingen wilt aanvragen uit uw aangepaste zoek exemplaar, verzendt u een GET-aanvraag naar de volgende URL:

```
https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/images/search
```

Zie [uw aangepaste zoek ervaring configureren](https://docs.microsoft.com/azure/cognitive-services/bing-custom-search/define-your-custom-view)voor meer informatie over het configureren van een aangepast Zoek exemplaar.

## <a name="next-steps"></a>Volgende stappen
De **Bing** api's ondersteunen zoek acties die resultaten retour neren op basis van hun type. Alle zoek eindpunten retour neren resultaten als JSON-antwoord objecten.  Alle eind punten ondersteunen query's die een specifieke taal en/of locatie retour neren met de lengte graad, breedte graad en de zoek RADIUS.

Zie de naslag pagina's voor elk type voor volledige informatie over de para meters die door elk eind punt worden ondersteund.
Zie voor voor beelden van basis aanvragen met behulp van de Aangepaste zoekopdrachten-API [aangepaste zoek opdracht snel starten](https://docs.microsoft.com/azure/cognitive-services/bing-custom-search/)
