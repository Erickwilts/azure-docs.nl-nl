---
title: 'Snelstartgids: Nieuws zoeken met Node.js - REST API voor Bing News Search'
titlesuffix: Azure Cognitive Services
description: Gebruik deze snelstartgids om een aanvraag naar de REST API van Bing News Search te verzenden via Node.js en een JSON-antwoord te ontvangen.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-news-search
ms.topic: quickstart
ms.date: 6/18/2019
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: 1fb2676fb1513ab4955ffa35c559b2ae36e0efd0
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67203318"
---
# <a name="quickstart-perform-a-news-search-using-nodejs-and-the-bing-news-search-rest-api"></a>Snelstartgids: Nieuws zoeken met Node.js en de REST API voor Bing News Search

Gebruik deze snelstart om voor het eerst de Bing Afbeeldingen zoeken-API aan te roepen en een JSON-antwoord te ontvangen. Deze eenvoudige JavaScript-toepassing stuurt een zoekquery naar de API en toont de onbewerkte resultaten.

Hoewel deze toepassing in JavaScript is geschreven en wordt uitgevoerd in Node.js, is de API een RESTful-webservice die compatibel is met vrijwel elke programmeertaal.

De broncode voor dit voorbeeld is beschikbaar op [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/nodejs/Search/BingNewsSearchv7.js).

## <a name="prerequisites"></a>Vereisten

* Nieuwste versie van [Node.js](https://nodejs.org/en/download/).

* De [JavaScript-aanvragenbibliotheek](https://github.com/request/request)

[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](../../../includes/cognitive-services-bing-news-search-signup-requirements.md)]

Zie ook [Prijsinformatie Cognitive Services - Bing Zoeken-API](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

## <a name="create-and-initialize-the-application"></a>De toepassing maken en initialiseren

1. Maak een nieuw JavaScript-bestand in uw favoriete IDE of editor, en stel de vereisten voor striktheid en https in.

    ```javascript
    'use strict';
    let https = require('https');
    ```

2. Maak variabelen voor het API-eindpunt, het zoekpad voor de afbeeldings-API, uw abonnementssleutel en de zoekterm.
    ```javascript
    let subscriptionKey = 'enter key here';
    let host = 'api.cognitive.microsoft.com';
    let path = '/bing/v7.0/news/search';
    let term = 'Microsoft';
    ```

## <a name="handle-and-parse-the-response"></a>Het antwoord verwerken en parseren

1. definieer een functie met de naam `response_handler` en als parameter de HTTP-aanroep `response`. voer binnen deze functie de volgende stappen uit:

    1. Definieer een variabele voor de hoofdtekst van het JSON-antwoord.  
        ```javascript
        let response_handler = function (response) {
            let body = '';
        };
        ```

    2. De hoofdtekst van het antwoord opslaan wanneer de **gegevens**vlag wordt aangeroepen
        ```javascript
        response.on('data', function (d) {
            body += d;
        });
        ```

    3. Wanneer een **end** markering wordt doorgegeven, de JSON en headers kunnen worden weergegeven.

        ```javascript
        response.on('end', function () {
            console.log('\nRelevant Headers:\n');
            for (var header in response.headers)
                // header keys are lower-cased by Node.js
                if (header.startsWith("bingapis-") || header.startsWith("x-msedge-"))
                     console.log(header + ": " + response.headers[header]);
            body = JSON.stringify(JSON.parse(body), null, '  ');
            console.log('\nJSON Response:\n');
            console.log(body);
         });
        ```

## <a name="json-response"></a>JSON-antwoord

Een geslaagd antwoord wordt geretourneerd in de JSON-indeling, zoals u kunt zien in het volgende voorbeeld: 

```json
{
   "_type": "News",
   "readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/news\/search?q=Microsoft",
   "totalEstimatedMatches": 36,
   "sort": [
      {
         "name": "Best match",
         "id": "relevance",
         "isSelected": true,
         "url": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/news\/search?q=Microsoft"
      },
      {
         "name": "Most recent",
         "id": "date",
         "isSelected": false,
         "url": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/news\/search?q=Microsoft&sortby=date"
      }
   ],
   "value": [
      {
         "name": "Microsoft to open flagship London brick-and-mortar retail store",
         "url": "http:\/\/www.contoso.com\/article\/microsoft-to-open-flagshi...",
         "image": {
            "thumbnail": {
               "contentUrl": "https:\/\/www.bing.com\/th?id=ON.F9E4A49EC010417...",
               "width": 220,
               "height": 146
            }
         },
         "description": "After years of rumors about Microsoft opening a brick-and-mortar...", 
         "about": [
           {
             "readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/entiti...", 
             "name": "Microsoft"
           }, 
           {
             "readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/entit...", 
             "name": "London"
           }
         ], 
         "provider": [
           {
             "_type": "Organization", 
             "name": "Contoso"
           }
         ], 
          "datePublished": "2017-09-21T21:16:00.0000000Z", 
          "category": "ScienceAndTechnology"
      }, 

      . . .
      
      {
         "name": "Microsoft adds Availability Zones to its Azure cloud platform",
         "url": "https:\/\/contoso.com\/2017\/09\/21\/microsoft-adds-availability...",
         "image": {
            "thumbnail": {
               "contentUrl": "https:\/\/www.bing.com\/th?id=ON.0AE7595B9720...",
               "width": 700,
               "height": 466
            }
         },
         "description": "Microsoft has begun adding Availability Zones to its...",
         "about": [
            {
               "readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/entities\/a093e9b...",
               "name": "Microsoft"
            },
            {
               "readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/entities\/cf3abf7d-e379-...",
               "name": "Windows Azure"
            },
            {
               "readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/entities\/9cdd061c-1fae-d0...",
               "name": "Cloud"
            }
         ],
         "provider": [
            {
               "_type": "Organization",
               "name": "Contoso"
            }
         ],
         "datePublished": "2017-09-21T09:01:00.0000000Z",
         "category": "ScienceAndTechnology"
      }
   ]
}
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een web-app met één pagina maken](tutorial-bing-news-search-single-page-app.md)
