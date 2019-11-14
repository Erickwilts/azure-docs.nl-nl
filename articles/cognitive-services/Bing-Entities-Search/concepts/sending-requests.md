---
title: Verzenden van zoekaanvragen naar de Bing Entiteiten zoeken-API
titleSuffix: Azure cognitive Services
description: De Bing Entiteiten zoeken-API verzendt een zoekquery naar Bing en haalt resultaten op die zowel entiteiten als plaatsen bevatten.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: conceptual
ms.date: 06/27/2019
ms.author: aahi
ms.openlocfilehash: f68429a75ddb141c9e42babde3faa9f93fe949cc
ms.sourcegitcommit: a107430549622028fcd7730db84f61b0064bf52f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2019
ms.locfileid: "74072687"
---
# <a name="sending-search-requests-to-the-bing-entity-search-api"></a>Verzenden van zoekaanvragen naar de Bing Entiteiten zoeken-API

De Bing Entiteiten zoeken-API verzendt een zoekquery naar Bing en haalt resultaten op die zowel entiteiten als plaatsen bevatten. Plaatsresultaten kunnen restaurants, hotels of andere lokale bedrijven zijn. In het geval van plaatsen kan de query de naam van het lokale bedrijf opgeven of vragen om een lijst (bijvoorbeeld 'restaurants near me'). Entiteitsresultaten zijn personen, plaatsen of dingen. Plaats in deze context zijn toeristische attractions, Staten, landen/regio's enz.

## <a name="the-endpoint"></a>Het eindpunt

Als u zoekresultaten voor entiteiten en plaatsen wilt opvragen, verstuurt u een GET-aanvraag naar het volgende eindpunt:  

```
https://api.cognitive.microsoft.com/bing/v7.0/entities
```

In aanvragen moet het HTTPS-protocol worden gebruikt.

Het is raadzaam dat alle aanvragen afkomstig zijn van een server. Het distribueren van de sleutel als onderdeel van een clienttoepassing biedt een kwaadwillende gebruiker namelijk meer mogelijkheden om de sleutel te onderscheppen. Als u ervoor kiest om alle aanroepen via een server te laten lopen, beschikt u bovendien over een centraal upgradepunt voor toekomstige versies van de API.

## <a name="specifying-query-parameters-and-headers"></a>Queryparameters en headers opgeven

De aanvraag moet de parameter [q](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference#query) bevatten, met daarin de zoekterm van de gebruiker. De aanvraag moet ook de queryparameter [mkt](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference#mkt) bevatten, ter aanduiding van de markt waar u de resultaten vandaan wilt halen. Zie [Queryparameters](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference#query-parameters) voor een lijst met optionele queryparameters. U moet op alle queryparameters URL-codering toepassen.  
  
De aanvraag moet de header [Ocp-Apim-Subscription-Key](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference#subscriptionkey) bevatten. Hoewel dit optioneel is, wordt u aangeraden ook altijd deze headers op te geven:  
  
-   [User-Agent](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference#useragent)  
-   [X-MSEdge-ClientID](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference#clientid)  
-   [X-MSEdge-ClientIP](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference#clientip)  
-   [X-Search-Location](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference#location)  

De headers ClientIP en Location zijn belangrijk voor het retourneren van locatiespecifieke inhoud.  

Zie [Headers](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference#headers) voor een lijst van alle aanvraag- en antwoordheaders.

## <a name="the-request"></a>De aanvraag

Hieronder ziet u een aanvraag voor entiteiten waarin alle voorgestelde queryparameters en headers zijn opgenomen. 

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/entities?q=mount+rainier&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-Search-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

Als dit de eerste keer is van u een van de Bing-API's aanroept, moet u de header met de client-id weglaten. Voeg de client-id alleen toe als u eerder een Bing-API hebt aangeroepen en Bing een client-id heeft geretourneerd voor de combinatie van gebruiker en apparaat.

## <a name="the-response"></a>Het antwoord

Hieronder ziet u het antwoord op de vorige aanvraag. Het voorbeeld bevat ook de Bing-specifieke antwoordheaders. Zie [SearchResponse](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference#searchresponse) voor meer informatie over het responsobject.

[!INCLUDE [cognitive-services-bing-url-note](../../../../includes/cognitive-services-bing-url-note.md)]

```json
BingAPIs-TraceId: 76DD2C2549B94F9FB55B4BD6FEB6AC
X-MSEdge-ClientID: 1C3352B306E669780D58D607B96869
BingAPIs-Market: en-US

{
    "_type" : "SearchResponse",
    "queryContext" : {
        "originalQuery" : "mount rainier"
    },
    "entities" : {
        "queryScenario" : "DominantEntity",
        "value" : [{
            "contractualRules" : [{
                "_type" : "ContractualRules\/LicenseAttribution",
                "targetPropertyName" : "description",
                "mustBeCloseToContent" : true,
                "license" : {
                    "name" : "CC-BY-SA",
                    "url" : "http:\/\/creativecommons.org\/licenses\/by-sa\/3.0\/"
                },
                "licenseNotice" : "Text under CC-BY-SA license"
            },
            {
                "_type" : "ContractualRules\/LinkAttribution",
                "targetPropertyName" : "description",
                "mustBeCloseToContent" : true,
                "text" : "en.wikipedia.org",
                "url" : "http:\/\/en.wikipedia.org\/wiki\/Mount_Rainier"
            },
            {
                "_type" : "ContractualRules\/MediaAttribution",
                "targetPropertyName" : "image",
                "mustBeCloseToContent" : true,
                "url" : "http:\/\/en.wikipedia.org\/wiki\/Mount_Rainier"
            }],
            "webSearchUrl" : "https:\/\/www.bing.com\/search?q=Mount%20Rainier...",
            "name" : "Mount Rainier",
            "image" : {
                "name" : "Mount Rainier",
                "thumbnailUrl" : "https:\/\/www.bing.com\/th?id=A21890c0e1f...",
                "provider" : [{
                    "_type" : "Organization",
                    "url" : "http:\/\/en.wikipedia.org\/wiki\/Mount_Rainier"
                }],
                "hostPageUrl" : "http:\/\/upload.wikimedia.org\/wikipedia...",
                "width" : 110,
                "height" : 110
            },
            "description" : "Mount Rainier, Mount Tacoma, or Mount Tahoma is the highest...",
            "entityPresentationInfo" : {
                "entityScenario" : "DominantEntity",
                "entityTypeHints" : ["Attraction"],
                "entityTypeDisplayHint" : "Mountain"
            },
            "bingId" : "9ae3e6ca-81ea-6fa1-ffa0-42e1d78906"
        }]
    }
}
```


## <a name="next-steps"></a>Volgende stappen

* [Searching for entities with the Bing Entity API](search-for-entities.md) (Zoeken naar entiteiten met de Bing Entiteiten-API)
* [Vereisten voor gebruik en weergave van Bing-API](../use-display-requirements.md)