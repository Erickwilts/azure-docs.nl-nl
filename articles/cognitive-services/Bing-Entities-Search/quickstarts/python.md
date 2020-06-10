---
title: 'Snelstartgids: een zoek opdracht verzenden naar de REST API met behulp van python-Bing Entity Search'
titleSuffix: Azure Cognitive Services
description: Gebruik deze quickstart om met Python een aanvraag te verzenden naar de REST-API van Bing Entiteiten zoeken en een JSON-antwoord te ontvangen.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: quickstart
ms.date: 05/08/2020
ms.author: aahi
ms.custom: tracking-python
ms.openlocfilehash: c535683323428dadedeaefd5f76f4387569f9427
ms.sourcegitcommit: 1de57529ab349341447d77a0717f6ced5335074e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/09/2020
ms.locfileid: "84604130"
---
# <a name="quickstart-send-a-search-request-to-the-bing-entity-search-rest-api-using-python"></a>Snelstartgids: een zoek opdracht verzenden naar het Bing Entity Search REST API met behulp van python

Gebruik deze quickstart om voor het eerst de Bing Entity Search REST-API aan te roepen en het JSON-antwoord te bekijken. Deze eenvoudige Python-toepassing stuurt een query naar de API om nieuws te zoeken en geeft het antwoord weer. De bron code voor dit voor beeld is beschikbaar op [github](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/python/Search/BingEntitySearchv7.py).

Hoewel deze toepassing is geschreven in Python, is de API een REST-webservice die compatibel is met de meeste programmeer talen.

## <a name="prerequisites"></a>Vereisten

* [Python](https://www.python.org/downloads/) 2. x of 3. x

[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](../../../../includes/cognitive-services-bing-entity-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>De toepassing maken en initialiseren

1. Maak een nieuw Python-bestand in uw favoriete IDE of editor en voeg de volgende importinstructies toe. Maak variabelen voor uw abonnements sleutel, eind punt, markt en zoek query. U kunt het globale eind punt in de volgende code gebruiken of het [aangepaste subdomein](../../../cognitive-services/cognitive-services-custom-subdomains.md) eindpunt gebruiken dat wordt weer gegeven in de Azure portal voor uw resource.

    ```python
    import http.client, urllib.parse
    import json
    
    subscriptionKey = 'ENTER YOUR KEY HERE'
    host = 'api.cognitive.microsoft.com'
    path = '/bing/v7.0/entities'
    mkt = 'en-US'
    query = 'italian restaurants near me'
    ```

2. Maak een aanvraag-URL door uw marktvariabele toe te voegen aan de parameter `?mkt=`. URL: Codeer uw query en voeg deze toe aan de `&q=` para meter. 
    
    ```python
    params = '?mkt=' + mkt + '&q=' + urllib.parse.quote (query)
    ```

## <a name="send-a-request-and-get-a-response"></a>Een aanvraag verzenden en een antwoord ontvangen

1. Maak de functie `get_suggestions()`. 

2. In deze functie voegt u uw abonnements sleutel toe aan een woorden lijst met `Ocp-Apim-Subscription-Key` als sleutel.

3. Gebruiken `http.client.HTTPSConnection()` om een HTTPS-client object te maken. Verstuur een `GET`-aanvraag met `request()`, met daarin het pad en de parameters en de headergegevens.

4. Sla het antwoord op met `getresponse()` en retourneer `response.read()`.

   ```python
   def get_suggestions ():
    headers = {'Ocp-Apim-Subscription-Key': subscriptionKey}
    conn = http.client.HTTPSConnection (host)
    conn.request ("GET", path + params, None, headers)
    response = conn.getresponse ()
    return response.read()
   ```

5. Roep `get_suggestions()` het JSON-antwoord aan en druk het af.

    ```python
    result = get_suggestions ()
    print (json.dumps(json.loads(result), indent=4))
    ```

## <a name="example-json-response"></a>Voorbeeld van JSON-antwoord

Een geslaagd antwoord wordt geretourneerd in de JSON-indeling, zoals u kunt zien in het volgende voorbeeld: 

```json
{
  "_type": "SearchResponse",
  "queryContext": {
    "originalQuery": "italian restaurant near me",
    "askUserForLocation": true
  },
  "places": {
    "value": [
      {
        "_type": "LocalBusiness",
        "webSearchUrl": "https://www.bing.com/search?q=sinful+bakery&filters=local...",
        "name": "Liberty's Delightful Sinful Bakery & Cafe",
        "url": "https://www.contoso.com/",
        "entityPresentationInfo": {
          "entityScenario": "ListItem",
          "entityTypeHints": [
            "Place",
            "LocalBusiness"
          ]
        },
        "address": {
          "addressLocality": "Seattle",
          "addressRegion": "WA",
          "postalCode": "98112",
          "addressCountry": "US",
          "neighborhood": "Madison Park"
        },
        "telephone": "(800) 555-1212"
      },

      . . .
      {
        "_type": "Restaurant",
        "webSearchUrl": "https://www.bing.com/search?q=Pickles+and+Preserves...",
        "name": "Munson's Pickles and Preserves Farm",
        "url": "https://www.princi.com/",
        "entityPresentationInfo": {
          "entityScenario": "ListItem",
          "entityTypeHints": [
            "Place",
            "LocalBusiness",
            "Restaurant"
          ]
        },
        "address": {
          "addressLocality": "Seattle",
          "addressRegion": "WA",
          "postalCode": "98101",
          "addressCountry": "US",
          "neighborhood": "Capitol Hill"
        },
        "telephone": "(800) 555-1212"
      },
      
      . . .
    ]
  }
}
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een web-app van één pagina maken](../tutorial-bing-entities-search-single-page-app.md)

* [Wat is de Bing Entiteiten zoeken-API?](../search-the-web.md)
* [Bing entiteiten zoeken-API verwijzing](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference).
