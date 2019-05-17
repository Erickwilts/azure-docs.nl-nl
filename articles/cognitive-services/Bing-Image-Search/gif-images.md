---
title: Zoeken naar GIF-afbeeldingen met behulp van de Bing afbeeldingen zoeken-API
titleSuffix: Azure Cognitive Services
description: De Bing afbeeldingen zoeken-API gebruiken om te zoeken naar GIF-afbeeldingen op Internet.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: article
ms.date: 04/24/2018
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: 4b396c3b99c143f20caa774ee3c4c6ee25f96150
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65777123"
---
# <a name="search-for-gif-images"></a>GIF-afbeeldingen zoeken 

De Bing afbeeldingen zoeken-API kunt u ook over de hele Web voor de meest relevante GIF-afbeeldingen zoeken.  Ontwikkelaars kunnen aantrekkelijke GIF-bestanden in verschillende scenario's voor conversatie integreren. 

De volgende URL is een query voor GIF-animaties.
```
https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=interesting&imageType=AnimatedGif&mkt=en-us
```
De [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#query) parameter geeft u de zoektermen.  Geeft u ook de vorige query `animatedGif` met behulp van de [imageType](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imagetype) filterparameter.

Als voorbeelden van resultaten weergeven, gebruikt u de volgende URL in op bing.com gezocht.
```
https://www.bing.com/images/search?q=interesting&qft=%20filterui%3Aphoto-animatedgif

```
## <a name="query-parameters"></a>Queryparameters

Zie voor meer informatie over queryparameters en opties voor de [afbeeldingen zoeken-API-verwijzing](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#query-parameters). Hier volgt een voorbeeld onder de kop [voorbeeld zoekt GIF-animatie met behulp van Java](#gifExample).

## <a name="tips-and-suggestions"></a>Tips en suggesties

- U kunt opgeven [maxFileSize](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#maxfilesize) en [minFileSize](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#minfilesize) parameters. Het is raadzaam instellen van de maxFileSize 2000000 = meerderheid van GIF-bestanden in de index zijn dan 2MB.  Hierdoor kunnen er ook voor het beheren van de gegevensgrootte als bandbreedte een probleem, zoals in mobiele mobiele scenario's is.
- Ter verbetering van de merkbare prestaties laden van de miniatuur van het eerste voordat het laden van de bron-url.  
- Probeer met behulp van onze trending gif-zoekacties uit voor eerste uitvoering of landingspagina pagina ervaring waar u een gebruikersquery nog hebt, het [API trending afbeeldingen](trending-images.md).
- Er zijn drie instellingen voor de [safeSearch](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#safesearch) parameter.  De `strict` optie inhoud voor volwassenen blokkeren.
- Zie [mkt](supported-countries-markets.md) voor een volledige lijst met talen en ondersteunde locaties.
- *AnimatedGifHttps* alleen retourneert bewegende GIF-afbeeldingen die afkomstig van een https-adres zijn. Voor beveiliging vereist veel toepassingen verbinding met koppelingen naar externe websites via https. De Apple App Store vereist bijvoorbeeld verbinding met de web-services via HTTPS, waarmee gebruikersgegevens veilig overgedragen worden gecodeerd.

<a name="gifExample" />

## <a name="example-search-for-animated-gif-using-java"></a>Voorbeeld zoekt GIF-animatie met behulp van Java

De volgende URL zoekt naar GIF-animaties: `q=interesting`
```
https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=interesting&imageType=AnimatedGif&mkt=en-us

```
Zoals in het volgende voorbeeld wordt weergegeven, de URL-query vereist [Ocp-Apim-Subscription-Key](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#headers) header.

Het volgende Java-voorbeeld maakt en stuurt de aanvraag.

```
package gifSearch;
import java.net.*;
import java.util.*;
import java.io.*;
import javax.net.ssl.HttpsURLConnection;

import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import com.google.gson.JsonObject;
import com.google.gson.JsonParser;

/*
 * Gson: https://github.com/google/gson
 * Maven info:
 *     groupId: com.google.code.gson
 *     artifactId: gson
 *     version: 2.8.1
 *
 * Once you have compiled or downloaded gson-2.8.1.jar, assuming you have placed it in the
 * same folder as this file (BingImageSearch.java), you can compile and run this program at
 * the command line as follows.
 *
 * javac GIFsearch.java -classpath .;gson-2.8.1.jar -encoding UTF-8
 * java -cp .;gson-2.8.1.jar GIFsearch
 */


public class GIFsearch {

    // Replace the subscriptionKey string value with your valid subscription key.
    static String subscriptionKey = "YOUR-ACCESS-KEY";

    static String host = "https://api.cognitive.microsoft.com";
    static String path = "/bing/v7.0/images/search";

    static String searchTerm = "interesting";

    public static SearchResults SearchImages (String searchQuery) throws Exception {
        // construct URL of search request (endpoint + query string)
        URL url = new URL(host + path + "?q=" +  URLEncoder.encode(searchQuery, "UTF-8") + "&imageType=AnimatedGif&mkt=en-us");
        HttpsURLConnection connection = (HttpsURLConnection)url.openConnection();
        connection.setRequestProperty("Ocp-Apim-Subscription-Key", subscriptionKey);

        // receive JSON body
        InputStream stream = connection.getInputStream();
        String response = new Scanner(stream).useDelimiter("\\A").next();

        // construct result object for return
        SearchResults results = new SearchResults(new HashMap<String, String>(), response);

        // extract Bing-related HTTP headers
        Map<String, List<String>> headers = connection.getHeaderFields();
        for (String header : headers.keySet()) {
            if (header == null) continue;      // may have null key
            if (header.startsWith("BingAPIs-") || header.startsWith("X-MSEdge-")) {
                results.relevantHeaders.put(header, headers.get(header).get(0));
            }
        }

        stream.close();
        return results;
    }

    // pretty-printer for JSON; uses GSON parser to parse and re-serialize
    public static String prettify(String json_text) {
        JsonParser parser = new JsonParser();
        JsonObject json = parser.parse(json_text).getAsJsonObject();
        Gson gson = new GsonBuilder().setPrettyPrinting().create();
        return gson.toJson(json);
    }

    public static void main (String[] args) {
        if (subscriptionKey.length() != 32) {
            System.out.println("Invalid Bing Search API subscription key!");
            System.out.println("Please paste yours into the source code.");
            System.exit(1);
        }

        try {
            System.out.println("Searching the Web for: " + searchTerm);

            SearchResults result = SearchImages(searchTerm);

            System.out.println("\nRelevant HTTP Headers:\n");
            for (String header : result.relevantHeaders.keySet())
                System.out.println(header + ": " + result.relevantHeaders.get(header));

            System.out.println("\nJSON Response:\n");
            System.out.println(prettify(result.jsonResponse));
        }
        catch (Exception e) {
            e.printStackTrace(System.out);
            System.exit(1);
        }
    }

}

//Container class for search results encapsulates relevant headers and JSON data
class SearchResults{
 HashMap<String, String> relevantHeaders;
 String jsonResponse;
 SearchResults(HashMap<String, String> headers, String json) {
     relevantHeaders = headers;
     jsonResponse = json;
 }
}

```

## <a name="results"></a>Resultaten
De code wordt de volgende resultaten als JSON-objecten:

```json
    {
      "webSearchUrl": "https://www.bing.com/images/search?view\u003ddetai...",
      "name": "Very Interesting GIF - Thats Very Interesting - ...",
      "thumbnailUrl": "https://tse1.mm.bing.net/th?id\u003dOIP.yJX6Vz345JPK...",
      "datePublished": "2017-03-12T01:35:00.0000000Z",
      "contentUrl": "https://media.contoso.co/images/c895fa573df8e493ca8d0dec7d93b/raw",
      "hostPageUrl": "https://www.contoso.co/view/thats-very-interesting-christi...",
      "contentSize": "1295633 B",
      "encodingFormat": "animatedgif",
      "hostPageDisplayUrl": "https://www.contoso.co/view/thats-very-christian...",
      "width": 440,
      "height": 186,
      "thumbnail": {
        "width": 474,
        "height": 200
      },
      "imageInsightsToken": "ccid_yJX6Vz34*mid_9FF0FFA42AADA1357F042443D2103B40EA...",
      "insightsMetadata": {
        "recipeSourcesCount": 0,
        "bestRepresentativeQuery": {
          "text": "That\u0027s Very Interesting",
          "displayText": "That\u0027s Very Interesting",
          "webSearchUrl": "https://www.bing.com/images/search?q\u003dThat..."
        },
        "pagesIncludingCount": 19,
        "availableSizesCount": 2
      },
      "imageId": "9FF0FFA42AADA1357F042443D21030EAAA225F",
      "accentColor": "62452D"
    },

```

## <a name="next-steps"></a>Volgende stappen
- [Snelstart voor C#](quickstarts/csharp.md)
- [Zelfstudie afbeeldingen zoeken-toepassing voor één pagina](tutorial-bing-image-search-single-page-app.md)
