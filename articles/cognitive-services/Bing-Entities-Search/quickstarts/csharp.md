---
title: 'Quickstart: Een zoekaanvraag naar de REST-API van Bing Entiteiten zoeken sturen met C#'
titlesuffix: Azure Cognitive Services
description: Gebruik deze quickstart om een aanvraag naar de Bing Entiteiten zoeken-REST API te verzenden via C# en een JSON-antwoord te ontvangen.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: quickstart
ms.date: 03/12/2019
ms.author: aahi
ms.openlocfilehash: e0e595f6bbc1ed241aefe48eb766c04a790a102e
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66384596"
---
# <a name="quickstart-send-a-search-request-to-the-bing-entity-search-rest-api-using-c"></a>Quickstart: Een zoekaanvraag naar de REST-API van Bing Entiteiten zoeken sturen met C#

Gebruik deze quickstart om voor het eerst de Bing Entiteiten zoeken-API aan te roepen en het JSON-antwoord te bekijken. Deze eenvoudige C#-toepassing stuurt een query naar de API om nieuws te zoeken en geeft het antwoord weer. De broncode voor deze toepassing is beschikbaar [op GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/dotnet/Search/BingEntitySearchv7.cs).

Hoewel deze toepassing in C# is geschreven, is de API een RESTful-webservice die compatibel is met vrijwel elke programmeertaal.


## <a name="prerequisites"></a>Vereisten

- Een versie van [Visual Studio 2017 of later](https://www.visualstudio.com/downloads/).

- Het [Json.NET](https://www.newtonsoft.com/json)-framework, beschikbaar als NuGet-pakket. Het NuGet-pakket installeren in Visual Studio:

   1. Klik met de rechtermuisknop op uw project in **Solution Explorer**.
   2. Selecteer **NuGet-pakketten beheren**.
   3. Zoeken naar *Newtonsoft.Json* en installeer het pakket.

- Als u Linux/MacOS, deze toepassing kan worden uitgevoerd met behulp van [Mono](https://www.mono-project.com/).


[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](../../../../includes/cognitive-services-bing-entity-search-signup-requirements.md)]

## <a name="create-and-initialize-a-project"></a>Een project maken en initialiseren

1. Maak een nieuwe C#-console-oplossing in Visual Studio. Voeg de volgende naamruimten in het hoofdcodebestand in.
    
    ```csharp
    using Newtonsoft.Json;
    using System;
    using System.Net.Http;
    using System.Text;
    ```

2. Maak een nieuwe klasse, en voeg variabelen toe voor het API-eindpunt, uw abonnementssleutel en de zoekterm.

    ```csharp
    namespace EntitySearchSample
    {
        class Program
        {
            static string host = "https://api.cognitive.microsoft.com";
            static string path = "/bing/v7.0/entities";
    
            static string market = "en-US";
    
            // NOTE: Replace this example key with a valid subscription key.
            static string key = "ENTER YOUR KEY HERE";
    
            static string query = "italian restaurant near me";
        //...
        }
    }
    ```

## <a name="send-a-request-and-get-the-api-response"></a>Een aanvraag verzenden en het API-antwoord ontvangen

1. Maak in de klasse een functie met de naam `Search()`. Maak een nieuw `Ocp-Apim-Subscription-Key`-object, en voeg uw abonnementssleutel toe aan de `HttpClient`-header.

   1. Maak de URI voor uw aanvraag door de host en het pad te combineren. Voeg vervolgens uw markt toe en pas URL-codering toe op uw query.
   2. Await op `client.GetAsync()` voor een HTTP-antwoord heeft opgehaald en sla vervolgens het json-antwoord op via een await op `ReadAsStringAsync()`.
   3. Indeling van de JSON-tekenreeks met `JsonConvert.DeserializeObject()` en af te drukken naar de console.

      ```csharp
      async static void Search()
      {
       //...
       HttpClient client = new HttpClient();
       client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", key);

       string uri = host + path + "?mkt=" + market + "&q=" + System.Net.WebUtility.UrlEncode(query);

       HttpResponseMessage response = await client.GetAsync(uri);

       string contentString = await response.Content.ReadAsStringAsync();
       dynamic parsedJson = JsonConvert.DeserializeObject(contentString);
       Console.WriteLine(parsedJson);
      }
      ```

2. Roep in de main-methode van uw toepassing de functie `Search()` aan.
    
    ```csharp
    static void Main(string[] args)
    {
        Search();
        Console.ReadLine();
    }
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
> [Een web-app van één pagina maken](../tutorial-bing-entities-search-single-page-app.md).

* [Wat is de Bing Entiteiten zoeken-API?](../overview.md )
* [Naslaghandleiding Bing Entiteiten zoeken-API](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference)
