---
title: Bing Aangepaste zoekopdrachten-eindpunt
titlesuffix: Azure Cognitive Services
description: Samenvatting van de Bing Custom Search-API-eindpunt.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: conceptual
ms.date: 03/04/2019
ms.author: aahi
ms.openlocfilehash: bc33d65972e12591130389e0c3cb8697898d1da9
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66390346"
---
# <a name="custom-search"></a>Aangepast zoeken
Met Bing Aangepaste zoekopdrachten kunt u op maat gemaakte zoekervaringen maken voor onderwerpen die u interesseren. Uw gebruikers zien dan zoekresultaten die zijn afgestemd op de inhoud die ze interessant vinden in plaats van dat ze door pagina's met zoekresultaten moeten bladeren die niet-relevante inhoud bevatten.

## <a name="custom-search-endpoint"></a>Eindpunt aangepast zoeken
Als u de resultaten met behulp van de Bing Custom Search-API, verzendt een `GET` aanvraag naar het volgende eindpunt. De kop- en URL-parameters gebruiken om te definiëren verder specificaties.

Eindpunt: Retourneert zoeksuggesties als JSON-resultaten die relevant voor de invoer van de gebruiker gedefinieerd zijn door `?q=""`.
```  
 GET https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search  
```

Zie voor meer voorbeelden waarin wordt beschreven hoe u aangepaste zoekopdrachten bronnen instelt, de [zelfstudie](https://docs.microsoft.com/azure/cognitive-services/bing-custom-search/tutorials/custom-search-web-page). Voor meer informatie over kopteksten, parameters, markt codes, antwoordobjecten, fouten, enz., Zie de [Bing Custom Search-API voor Bing versie 7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-custom-search-api-v7-reference) verwijzing.

## <a name="custom-search-response-json"></a>JSON-antwoord van de aangepaste zoekopdrachten
Een aangepaste zoekopdracht retourneert resultaten als JSON-objecten, Zie [antwoordobjecten](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-custom-search-api-v7-reference#response-objects). 

## <a name="custom-autosuggest"></a>Aangepaste automatische suggesties
De aangepaste automatische suggestie-API kunt u een gedeeltelijke zoekterm query verzenden naar Bing en terughalen van een lijst met voorgestelde query's die u kunt configureren. Met aangepaste automatische suggesties, suggesties die zijn geretourneerd door de API toevoegen en optioneel opgeven of u wilt opnemen die worden gegenereerd door Bing suggesties.

## <a name="custom-autosuggest-endpoint"></a>Aangepaste automatische suggesties eindpunt
Verzenden om aan te vragen van aangepaste Querysuggesties, een GET-aanvraag naar:

```
https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/Suggestions
```  

Zie voor meer informatie over het definiëren van aangepaste suggesties [definiëren van aangepaste zoeksuggesties](define-custom-suggestions.md).

## <a name="custom-image-search"></a>Aangepast zoeken voor afbeeldingen
De aangepaste afbeeldingen zoeken-API kunt u een zoekopdracht naar Bing te verzenden en een lijst met relevante afbeeldingen teruggaan van uw aangepaste zoekopdrachten-instantie.

## <a name="custom-image-search-endpoint"></a>Aangepaste afbeelding zoeken-eindpunten
Om aan te vragen afbeeldingen van uw instantie Custom Search, een GET-aanvraag te verzenden naar de volgende URL:

```
https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/images/search
```

Zie voor meer informatie over het configureren van een exemplaar voor aangepast zoeken [configureren van uw aangepaste zoekervaring](https://docs.microsoft.com/azure/cognitive-services/bing-custom-search/define-your-custom-view).

## <a name="next-steps"></a>Volgende stappen
De **Bing** API's ondersteunen zoekacties die retourneren van resultaten op basis van hun type. Alle eindpunten voor zoeken worden resultaten geretourneerd als JSON-antwoord-objecten.  Alle eindpunten ondersteuning voor query's die een specifieke taal en/of locatie door lengtegraad, breedtegraad en zoeken radius retourneren.

Zie de referentiepagina's voor elk type voor volledige informatie over de parameters die worden ondersteund door elk eindpunt.
Zie voor meer voorbeelden van basic met behulp van de Custom Search-API-aanvragen [aangepast zoeken voor snel starten](https://docs.microsoft.com/azure/cognitive-services/bing-custom-search/)
