---
title: Snelstartgids - verzenden een query naar de Bing lokale bedrijven zoeken-API met behulp van Java | Microsoft Docs
titleSuffix: Azure Cognitive Services
description: Gebruik dit artikel om te beginnen met de Bing lokale bedrijven zoeken-API in Java.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: quickstart
ms.date: 11/01/2018
ms.author: rosh
ms.openlocfilehash: eff7bbc8a7600667254bc9d49627e707b063bc41
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67592743"
---
# <a name="quickstart-send-a-query-to-the-bing-local-business-search-api-using-java"></a>Quickstart: Een query verzenden naar de Bing lokale bedrijven zoeken-API met behulp van Java

Gebruik deze Quick Start om te beginnen met het verzenden van aanvragen naar de Bing lokale bedrijven zoeken-API, dit is een Cognitive Service van Azure. Terwijl deze eenvoudige toepassing is geschreven in Java, de API is een RESTful-Web-compatibel is met elke programmeertaal die HTTP-aanvragen en parseren van JSON.

In dit voorbeeld van de toepassing lokaal antwoordgegevens worden opgehaald uit de API voor de zoekquery `hotel in Bellevue`.

## <a name="prerequisites"></a>Vereisten

* De [Java Development Kit (JDK)](https://www.oracle.com/technetwork/java/javase/downloads/index.html)

U moet beschikken over een [account voor de Cognitive Services-API](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) met Bing Zoeken-API's. De [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) is voldoende voor deze snelstartgids. U hebt de toegangssleutel nodig die is verstrekt tijdens het activeren van uw gratis proefversie.  Zie ook [Prijsinformatie Cognitive Services - Bing Zoeken-API](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

In dit voorbeeld van de toepassing lokaal antwoordgegevens worden opgehaald uit de query voor een *hotel in Bellevue*.

## <a name="create-the-request"></a>De aanvraag maken 

De volgende code maakt een `WebRequest`, stelt u de koptekst van de toegang en wordt een querytekenreeks voor 'hotel in Bellevue' toegevoegd.  Vervolgens wordt de aanvraag verzonden en het antwoord toegewezen aan een tekenreeks die de JSON-tekst moet bevatten.

```
    // construct URL of search request (endpoint + query string)
     URL url = new URL(host + path + "?q=" +  URLEncoder.encode(searchQuery, "UTF-8") + &mkt=en-us");
    HttpsURLConnection connection = (HttpsURLConnection)url.openConnection();
    connection.setRequestProperty("Ocp-Apim-Subscription-Key", subscriptionKey);

    // receive JSON body
    InputStream stream = connection.getInputStream();
    String response = new Scanner(stream).useDelimiter("\\A").next();

    // construct result object for return
    SearchResults results = new SearchResults(new HashMap<String, String>(), response);
```

## <a name="run-the-complete-application"></a>De volledige toepassing uitvoeren

De Bing lokale bedrijven zoeken-API retourneert de resultaten van de Bing zoeken-engine.
1. Download of installeer de gson-bibliotheek.
2. Maak een nieuw Java-project in uw favoriete IDE of editor.
3. Voeg de onderstaande code toe.
4. Vervang de waarde van subscriptionKey door een geldige toegangssleutel voor uw abonnement.
5. Voer het programma uit.

```
package localSearch;
import java.net.*;
import java.util.*;
import java.io.*;
import javax.net.ssl.HttpsURLConnection;

/*
 * Gson: https://github.com/google/gson
 * Maven info:
 *     groupId: com.google.code.gson
 *     artifactId: gson
 *     version: 2.8.1
 *
 * Once you have compiled or downloaded gson-2.8.1.jar, assuming you have placed it in the
 * same folder as this file (localSearch.java), you can compile and run this program at
 * the command line as follows.
 *
 * javac localSearch.java -classpath .;gson-2.8.1.jar -encoding UTF-8
 * java -cp .;gson-2.8.1.jar localSearch
 */
import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import com.google.gson.JsonObject;
import com.google.gson.JsonParser;

public class LocalSearchCls {

    // ***********************************************
    // *** Update or verify the following values. ***
    // **********************************************

        // Replace the subscriptionKey string value with your valid subscription key.
        static String subscriptionKey = "YOUR-ACCESS-KEY";

        static String host = "https://api.cognitive.microsoft.com/bing";
        static String path = "/v7.0/localbusinesses/search";

        static String searchTerm = "Hotel in Bellevue";

        public static SearchResults SearchLocal (String searchQuery) throws Exception {
            // construct URL of search request (endpoint + query string)
            URL url = new URL(host + path + "?q=" +  URLEncoder.encode(searchQuery, "UTF-8") + "&mkt=en-us");
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
            try {
                System.out.println("Searching the Web for: " + searchTerm);

                SearchResults result = SearchLocal(searchTerm);

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

    // Container class for search results encapsulates relevant headers and JSON data
    class SearchResults{
        HashMap<String, String> relevantHeaders;
        String jsonResponse;
        SearchResults(HashMap<String, String> headers, String json) {
            relevantHeaders = headers;
            jsonResponse = json;
        }
    }

```

## <a name="next-steps"></a>Volgende stappen
- [Snelstartgids voor lokale bedrijven zoeken](local-quickstart.md)
- [Lokale bedrijven zoeken Node-quickstart](local-search-node-quickstart.md)
- [Local Business Search Python quickstart](local-search-python-quickstart.md)
