---
title: 'Quickstart: Computer Vision 2.0 en 2.1 - Uittreksel gedrukte en handgeschreven tekst - REST, Java'
titleSuffix: Azure Cognitive Services
description: In deze quickstart haalt u gedrukte en handgeschreven tekst uit een afbeelding met behulp van de Computer Vision API met Java.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 04/14/2020
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: aaaa382d41990b801d1c451b2bf416493a7ba7c6
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2020
ms.locfileid: "81404919"
---
# <a name="quickstart-extract-printed-and-handwritten-text-using-the-computer-vision-rest-api-and-java"></a>Snelstart: Gedrukte en handgeschreven tekst extraheren met behulp van de Computer Vision REST API en Java

In deze quickstart haalt u gedrukte en/of handgeschreven tekst uit een afbeelding met behulp van de Computer Vision REST API. Met de methoden [Batch Read](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/2afb498089f74080d7ef85eb) and Read [Operation Result](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/5be108e7498a4f9ed20bf96d) u tekst in een afbeelding detecteren en herkende tekens extraheren in een machineleesbare tekenstroom. De service bepaalt welk herkenningsmodel voor elke regel tekst moet worden gebruikt, zodat deze afbeeldingen ondersteunt met zowel afgedrukte als handgeschreven tekst.

Deze functionaliteit is beschikbaar in zowel een v2.1 API als een v3.0 Public Preview API. Vergeleken met v2.1 heeft de 3.0 API:

* Verbeterde nauwkeurigheid
* Betrouwbaarheidsscores voor woorden
* Ondersteuning voor zowel Spaans als `language` Engels met de extra parameter
* Een andere uitvoerindeling

Selecteer het tabblad hieronder voor de versie die u gebruikt.

#### <a name="version-2"></a>[Versie 2](#tab/version-2)

> [!IMPORTANT]
> De [methode Batch read](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/2afb498089f74080d7ef85eb) wordt asynchroon uitgevoerd. Deze methode retourneert geen gegevens in de hoofdtekst van een geslaagd antwoord. In plaats daarvan retourneert de methode `Operation-Location` Batch read een URI in de waarde van het veld responskoptekst. U deze URI, die de API voor het [resultaat van de bewerking lezen](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/5be108e7498a4f9ed20bf96d) vertegenwoordigt, aanroepen om zowel de status te controleren als de resultaten van de aanroep batchleesmethode te retourneren.

#### <a name="version-3-public-preview"></a>[Versie 3 (openbare preview)](#tab/version-3)

> [!IMPORTANT]
> De [methode Batch read](https://westus2.dev.cognitive.microsoft.com/docs/services/5d98695995feb7853f67d6a6/operations/5d986960601faab4bf452005) wordt asynchroon uitgevoerd. Deze methode retourneert geen gegevens in de hoofdtekst van een geslaagd antwoord. In plaats daarvan retourneert de methode `Operation-Location` Batch read een URI in de waarde van het veld responskoptekst. U deze URI, die de API voor het [resultaat van de bewerking lezen](https://westus2.dev.cognitive.microsoft.com/docs/services/5d98695995feb7853f67d6a6/operations/5d9869604be85dee480c8750) vertegenwoordigt, aanroepen om zowel de status te controleren als de resultaten van de aanroep batchleesmethode te retourneren.

---

## <a name="prerequisites"></a>Vereisten

Als u geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/ai/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=cognitive-services) voordat u begint.

- U moet [Java&trade; Platform, Standard Edition Development Kit 7 of 8](https://aka.ms/azure-jdks) (JDK 7 of 8) hebben geïnstalleerd.
- U moet beschikken over een abonnementssleutel voor Computer Vision. U een gratis testsleutel krijgen van [Try Cognitive Services.](https://azure.microsoft.com/try/cognitive-services/?api=computer-vision) Of volg de instructies in [Een Cognitive Services-account maken](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) om u te abonneren op Computer Vision en uw sleutel te krijgen. Maak vervolgens [omgevingsvariabelen](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) voor de tekenreeks sleutel- `COMPUTER_VISION_SUBSCRIPTION_KEY` en `COMPUTER_VISION_ENDPOINT`serviceeindpunt, benoemd en , respectievelijk.

## <a name="create-and-run-the-sample-application"></a>De voorbeeldtoepassing maken en uitvoeren

#### <a name="version-2"></a>[Versie 2](#tab/version-2)

U kunt het voorbeeld maken en uitvoeren aan de hand van de volgende stappen:

1. Maak een nieuw Java-project in uw favoriete IDE of editor. Als de optie beschikbaar is, maakt u het Java-project vanuit een toepassingssjabloon met opdrachtregel.
1. Importeer de volgende bibliotheken in uw Java-project. Als u Maven gebruikt, worden de Maven-coördinaten opgegeven voor elke bibliotheek.
   - [Apache HTTP-client](https://hc.apache.org/downloads.cgi) (org.apache.httpcomponents:httpclient:4.5.5)
   - [Apache HTTP-core](https://hc.apache.org/downloads.cgi) (org.apache.httpcomponents:httpcore:4.4.9)
   - [JSON-bibliotheek](https://github.com/stleary/JSON-java) (org.json:json:20180130)
1. Voeg de volgende `import`-instructies toe aan het bestand met de openbare klasse `Main` voor uw project.  

   ```java
   import java.net.URI;
   import org.apache.http.HttpEntity;
   import org.apache.http.HttpResponse;
   import org.apache.http.client.methods.HttpGet;
   import org.apache.http.client.methods.HttpPost;
   import org.apache.http.client.utils.URIBuilder;
   import org.apache.http.entity.StringEntity;
   import org.apache.http.impl.client.CloseableHttpClient;
   import org.apache.http.impl.client.HttpClientBuilder;
   import org.apache.http.util.EntityUtils;
   import org.apache.http.Header;
   import org.json.JSONObject;
   ```

1. Vervang `Main` de openbare klasse door de volgende code.
1. Vervang eventueel de waarde `imageToAnalyze` van de URL van een andere afbeelding waaruit u tekst wilt extraheren.
1. Sla vervolgens het Java-project op en bouw het.
1. Als u een IDE gebruikt, voert u `Main` uit. Open anders een opdrachtpromptvenster en gebruik de opdracht `java` om de compilatieklasse uit te voeren. Bijvoorbeeld `java Main`.

```java
public class Main {
    // **********************************************
    // *** Update or verify the following values. ***
    // **********************************************

    // Add your Computer Vision subscription key and endpoint to your environment variables.
    // After setting, close and then re-open your command shell or project for the changes to take effect.
    String subscriptionKey = System.getenv("COMPUTER_VISION_SUBSCRIPTION_KEY");
    String endpoint = ("COMPUTER_VISION_ENDPOINT");

    private static final String uriBase = endpoint + 
            "vision/v2.1/read/core/asyncBatchAnalyze";

    private static final String imageToAnalyze =
        "https://upload.wikimedia.org/wikipedia/commons/thumb/d/dd/" +
        "Cursive_Writing_on_Notebook_paper.jpg/800px-Cursive_Writing_on_Notebook_paper.jpg";

    public static void main(String[] args) {
        CloseableHttpClient httpTextClient = HttpClientBuilder.create().build();
        CloseableHttpClient httpResultClient = HttpClientBuilder.create().build();;

        try {
            // This operation requires two REST API calls. One to submit the image
            // for processing, the other to retrieve the text found in the image.

            URIBuilder builder = new URIBuilder(uriBase);

            // Prepare the URI for the REST API method.
            URI uri = builder.build();
            HttpPost request = new HttpPost(uri);

            // Request headers.
            request.setHeader("Content-Type", "application/json");
            request.setHeader("Ocp-Apim-Subscription-Key", subscriptionKey);

            // Request body.
            StringEntity requestEntity =
                    new StringEntity("{\"url\":\"" + imageToAnalyze + "\"}");
            request.setEntity(requestEntity);

            // Two REST API methods are required to extract text.
            // One method to submit the image for processing, the other method
            // to retrieve the text found in the image.

            // Call the first REST API method to detect the text.
            HttpResponse response = httpTextClient.execute(request);

            // Check for success.
            if (response.getStatusLine().getStatusCode() != 202) {
                // Format and display the JSON error message.
                HttpEntity entity = response.getEntity();
                String jsonString = EntityUtils.toString(entity);
                JSONObject json = new JSONObject(jsonString);
                System.out.println("Error:\n");
                System.out.println(json.toString(2));
                return;
            }

            // Store the URI of the second REST API method.
            // This URI is where you can get the results of the first REST API method.
            String operationLocation = null;

            // The 'Operation-Location' response header value contains the URI for
            // the second REST API method.
            Header[] responseHeaders = response.getAllHeaders();
            for (Header header : responseHeaders) {
                if (header.getName().equals("Operation-Location")) {
                    operationLocation = header.getValue();
                    break;
                }
            }

            if (operationLocation == null) {
                System.out.println("\nError retrieving Operation-Location.\nExiting.");
                System.exit(1);
            }

            // If the first REST API method completes successfully, the second
            // REST API method retrieves the text written in the image.
            //
            // Note: The response may not be immediately available. Text
            // recognition is an asynchronous operation that can take a variable
            // amount of time depending on the length of the text.
            // You may need to wait or retry this operation.

            System.out.println("\nText submitted.\n" +
                    "Waiting 10 seconds to retrieve the recognized text.\n");
            Thread.sleep(10000);

            // Call the second REST API method and get the response.
            HttpGet resultRequest = new HttpGet(operationLocation);
            resultRequest.setHeader("Ocp-Apim-Subscription-Key", subscriptionKey);

            HttpResponse resultResponse = httpResultClient.execute(resultRequest);
            HttpEntity responseEntity = resultResponse.getEntity();

            if (responseEntity != null) {
                // Format and display the JSON response.
                String jsonString = EntityUtils.toString(responseEntity);
                JSONObject json = new JSONObject(jsonString);
                System.out.println("Text recognition result response: \n");
                System.out.println(json.toString(2));
            }
        } catch (Exception e) {
            System.out.println(e.getMessage());
        }
    }
}
```

#### <a name="version-3-public-preview"></a>[Versie 3 (openbare preview)](#tab/version-3)

U kunt het voorbeeld maken en uitvoeren aan de hand van de volgende stappen:

1. Maak een nieuw Java-project in uw favoriete IDE of editor. Als de optie beschikbaar is, maakt u het Java-project vanuit een toepassingssjabloon met opdrachtregel.
1. Importeer de volgende bibliotheken in uw Java-project. Als u Maven gebruikt, worden de Maven-coördinaten opgegeven voor elke bibliotheek.
   - [Apache HTTP-client](https://hc.apache.org/downloads.cgi) (org.apache.httpcomponents:httpclient:4.5.5)
   - [Apache HTTP-core](https://hc.apache.org/downloads.cgi) (org.apache.httpcomponents:httpcore:4.4.9)
   - [JSON-bibliotheek](https://github.com/stleary/JSON-java) (org.json:json:20180130)
1. Voeg de volgende `import`-instructies toe aan het bestand met de openbare klasse `Main` voor uw project.  

   ```java
    import java.net.URI;
    import org.apache.http.HttpEntity;
    import org.apache.http.HttpResponse;
    import org.apache.http.client.methods.HttpGet;
    import org.apache.http.client.methods.HttpPost;
    import org.apache.http.client.utils.URIBuilder;
    import org.apache.http.entity.StringEntity;
    import org.apache.http.impl.client.CloseableHttpClient;
    import org.apache.http.impl.client.HttpClientBuilder;
    import org.apache.http.util.EntityUtils;
    import org.apache.http.Header;
    import org.json.JSONObject;
    ```

1. Vervang `Main` de openbare klasse door de volgende code.
1. Vervang de sein `language` van de optie de waarde van de taal die u wilt herkennen. Geaccepteerde waarden zijn "en" voor Engels en "es" voor Spaans.
1. Vervang eventueel de waarde `imageToAnalyze` van de URL van een andere afbeelding waaruit u tekst wilt extraheren.
1. Sla vervolgens het Java-project op en bouw het.
1. Als u een IDE gebruikt, voert u `Main` uit. Open anders een opdrachtpromptvenster en gebruik de opdracht `java` om de compilatieklasse uit te voeren. Bijvoorbeeld `java Main`.

```java

public class Main {
    // **********************************************
    // *** Update or verify the following values. ***
    // **********************************************

    // Add your Computer Vision subscription key and endpoint to your environment variables.
    // After setting, close and then re-open your command shell or project for the changes to take effect.
    private static String subscriptionKey = System.getenv("COMPUTER_VISION_SUBSCRIPTION_KEY");
    private static String endpoint = System.getenv("COMPUTER_VISION_ENDPOINT");

    // Set the language that you want to recognize
    private static String language = "en";  // Accepted values are "en" for English, or "es" for Spanish

    private static String uriBase = endpoint +
            "/vision/v3.0-preview/read/analyze";

    private static String imageToAnalyze =
            "https://upload.wikimedia.org/wikipedia/commons/thumb/d/dd/" +
                    "Cursive_Writing_on_Notebook_paper.jpg/800px-Cursive_Writing_on_Notebook_paper.jpg";

    public static void main(String[] args) {
        CloseableHttpClient httpTextClient = HttpClientBuilder.create().build();
        CloseableHttpClient httpResultClient = HttpClientBuilder.create().build();;

        System.out.println("Endpoint:         " + endpoint);
        System.out.println("Subscription key: " + subscriptionKey);
        System.out.println("Language:         " + language);

        try {
            // This operation requires two REST API calls. One to submit the image
            // for processing, the other to retrieve the text found in the image.

            URIBuilder builder = new URIBuilder(uriBase);
            builder.setParameter("language", language);

            // Prepare the URI for the REST API method.
            URI uri = builder.build();
            HttpPost request = new HttpPost(uri);

            // Request headers.
            request.setHeader("Content-Type", "application/json");
            request.setHeader("Ocp-Apim-Subscription-Key", subscriptionKey);

            // Request body.
            StringEntity requestEntity =
                    new StringEntity("{\"url\":\"" + imageToAnalyze + "\"}");
            request.setEntity(requestEntity);

            // Two REST API methods are required to extract text.
            // One method to submit the image for processing, the other method
            // to retrieve the text found in the image.

            // Call the first REST API method to detect the text.
            HttpResponse response = httpTextClient.execute(request);

            // Check for success.
            if (response.getStatusLine().getStatusCode() != 202) {
                // Format and display the JSON error message.
                HttpEntity entity = response.getEntity();
                String jsonString = EntityUtils.toString(entity);
                JSONObject json = new JSONObject(jsonString);
                System.out.println("Error:\n");
                System.out.println(json.toString(2));
                return;
            }

            // Store the URI of the second REST API method.
            // This URI is where you can get the results of the first REST API method.
            String operationLocation = null;

            // The 'Operation-Location' response header value contains the URI for
            // the second REST API method.
            Header[] responseHeaders = response.getAllHeaders();
            for (Header header : responseHeaders) {
                if (header.getName().equals("Operation-Location")) {
                    operationLocation = header.getValue();
                    break;
                }
            }

            if (operationLocation == null) {
                System.out.println("\nError retrieving Operation-Location.\nExiting.");
                System.exit(1);
            }

            // If the first REST API method completes successfully, the second
            // REST API method retrieves the text written in the image.
            //
            // Note: The response may not be immediately available. Text
            // recognition is an asynchronous operation that can take a variable
            // amount of time depending on the length of the text.
            // You may need to wait or retry this operation.

            System.out.println("\nText submitted.\n" +
                    "Waiting 10 seconds to retrieve the recognized text.\n");
            Thread.sleep(10000);

            // Call the second REST API method and get the response.
            HttpGet resultRequest = new HttpGet(operationLocation);
            resultRequest.setHeader("Ocp-Apim-Subscription-Key", subscriptionKey);

            HttpResponse resultResponse = httpResultClient.execute(resultRequest);
            HttpEntity responseEntity = resultResponse.getEntity();

            if (responseEntity != null) {
                // Format and display the JSON response.
                String jsonString = EntityUtils.toString(responseEntity);
                JSONObject json = new JSONObject(jsonString);
                System.out.println("Text recognition result response: \n");
                System.out.println(json.toString(2));
            }
        } catch (Exception e) {
            System.out.println(e.getMessage());
        }
    }
}
```

---

## <a name="examine-the-response"></a>Het antwoord bekijken

Een geslaagd antwoord wordt geretourneerd in JSON-indeling. De voorbeeldtoepassing parseert en geeft een geslaagd antwoord weer in het consolevenster dat vergelijkbaar is met het volgende voorbeeld:

#### <a name="version-2"></a>[Versie 2](#tab/version-2)

```json
Text submitted. Waiting 10 seconds to retrieve the recognized text.

Text recognition result response:

{
  "status": "Succeeded",
  "recognitionResults": [
    {
      "page": 1,
      "clockwiseOrientation": 349.59,
      "width": 3200,
      "height": 3200,
      "unit": "pixel",
      "lines": [
        {
          "boundingBox": [202,618,2047,643,2046,840,200,813],
          "text": "Our greatest glory is not",
          "words": [
            {
              "boundingBox": [204,627,481,628,481,830,204,829],
              "text": "Our"
            },
            {
              "boundingBox": [519,628,1057,630,1057,832,518,830],
              "text": "greatest"
            },
            {
              "boundingBox": [1114,630,1549,631,1548,833,1114,832],
              "text": "glory"
            },
            {
              "boundingBox": [1586,631,1785,632,1784,834,1586,833],
              "text": "is"
            },
            {
              "boundingBox": [1822,632,2115,633,2115,835,1822,834],
              "text": "not"
            }
          ]
        },
        {
          "boundingBox": [420,1273,2954,1250,2958,1488,422,1511],
          "text": "but in rising every time we fall",
          "words": [
            {
              "boundingBox": [423,1269,634,1268,635,1507,424,1508],
              "text": "but"
            },
            {
              "boundingBox": [667,1268,808,1268,809,1506,668,1507],
              "text": "in"
            },
            {
              "boundingBox": [874,1267,1289,1265,1290,1504,875,1506],
              "text": "rising"
            },
            {
              "boundingBox": [1331,1265,1771,1263,1772,1502,1332,1504],
              "text": "every"
            },
            {
              "boundingBox": [1812, 1263, 2178, 1261, 2179, 1500, 1813, 1502],
              "text": "time"
            },
            {
              "boundingBox": [2219, 1261, 2510, 1260, 2511, 1498, 2220, 1500],
              "text": "we"
            },
            {
              "boundingBox": [2551, 1260, 3016, 1258, 3017, 1496, 2552, 1498],
              "text": "fall"
            }
          ]
        },
        {
          "boundingBox": [1612, 903, 2744, 935, 2738, 1139, 1607, 1107],
          "text": "in never failing ,",
          "words": [
            {
              "boundingBox": [1611, 934, 1707, 933, 1708, 1147, 1613, 1147],
              "text": "in"
            },
            {
              "boundingBox": [1753, 933, 2132, 930, 2133, 1144, 1754, 1146],
              "text": "never"
            },
            {
              "boundingBox": [2162, 930, 2673, 927, 2674, 1140, 2164, 1144],
              "text": "failing"
            },
            {
              "boundingBox": [2703, 926, 2788, 926, 2790, 1139, 2705, 1140],
              "text": ",",
              "confidence": "Low"
            }
          ]
        }
      ]
    }
  ]
}
```

#### <a name="version-3-public-preview"></a>[Versie 3 (openbare preview)](#tab/version-3)

```json
{
  "analyzeResult": {
    "readResults": [{
      "unit": "pixel",
      "width": 800,
      "angle": 0.8206,
      "language": "en",
      "page": 1,
      "lines": [
        {
          "boundingBox": [
            6,
            4,
            774,
            14,
            773,
            61,
            5,
            49
          ],
          "words": [
            {
              "boundingBox": [
                14,
                5,
                76,
                6,
                74,
                49,
                12,
                48
              ],
              "confidence": 0.83,
              "text": "The"
            },
            {
              "boundingBox": [
                84,
                6,
                182,
                7,
                180,
                51,
                82,
                49
              ],
              "confidence": 0.762,
              "text": "quick"
            },
            {
              "boundingBox": [
                191,
                7,
                312,
                9,
                309,
                54,
                189,
                51
              ],
              "confidence": 0.67,
              "text": "brown"
            },
            {
              "boundingBox": [
                320,
                9,
                382,
                10,
                379,
                55,
                317,
                54
              ],
              "confidence": 0.849,
              "text": "fox"
            },
            {
              "boundingBox": [
                390,
                10,
                497,
                11,
                493,
                57,
                387,
                55
              ],
              "confidence": 0.703,
              "text": "jumps"
            },
            {
              "boundingBox": [
                506,
                11,
                596,
                12,
                591,
                59,
                502,
                57
              ],
              "confidence": 0.799,
              "text": "over"
            },
            {
              "boundingBox": [
                604,
                12,
                666,
                13,
                661,
                60,
                600,
                59
              ],
              "confidence": 0.923,
              "text": "the"
            },
            {
              "boundingBox": [
                674,
                13,
                773,
                14,
                768,
                62,
                670,
                60
              ],
              "confidence": 0.863,
              "text": "lazy"
            }
          ],
          "language": "en",
          "text": "The quick brown fox jumps over the lazy"
        },
        {
          "boundingBox": [
            5,
            53,
            79,
            56,
            77,
            95,
            4,
            92
          ],
          "words": [{
            "boundingBox": [
              6,
              53,
              74,
              56,
              72,
              95,
              5,
              92
            ],
            "confidence": 0.418,
            "text": "dog"
          }],
          "language": "en",
          "text": "dog"
        },
        {
          "boundingBox": [
            0,
            90,
            787,
            95,
            787,
            145,
            0,
            136
          ],
          "words": [
            {
              "boundingBox": [
                1,
                96,
                79,
                93,
                79,
                135,
                0,
                136
              ],
              "confidence": 0.835,
              "text": "Pack"
            },
            {
              "boundingBox": [
                87,
                93,
                151,
                92,
                151,
                135,
                87,
                135
              ],
              "confidence": 0.88,
              "text": "my"
            },
            {
              "boundingBox": [
                162,
                92,
                226,
                91,
                225,
                135,
                161,
                135
              ],
              "confidence": 0.301,
              "text": "box"
            },
            {
              "boundingBox": [
                234,
                91,
                335,
                90,
                335,
                135,
                233,
                135
              ],
              "confidence": 0.959,
              "text": "with"
            },
            {
              "boundingBox": [
                346,
                91,
                418,
                91,
                417,
                136,
                345,
                135
              ],
              "confidence": 0.489,
              "text": "five"
            },
            {
              "boundingBox": [
                426,
                91,
                527,
                93,
                527,
                138,
                425,
                136
              ],
              "confidence": 0.727,
              "text": "dozen"
            },
            {
              "boundingBox": [
                554,
                94,
                687,
                98,
                687,
                143,
                553,
                139
              ],
              "confidence": 0.377,
              "text": "liquor"
            },
            {
              "boundingBox": [
                701,
                99,
                787,
                103,
                787,
                146,
                700,
                143
              ],
              "confidence": 0.693,
              "text": "jugs"
            }
          ],
          "language": "en",
          "text": "Pack my box with five dozen liquor jugs"
        }
      ],
      "height": 154
    }],
    "version": "3.0.0"
  },
  "createdDateTime": "2020-02-11T21:21:14Z",
  "lastUpdatedDateTime": "2020-02-11T21:21:19Z",
  "status": "succeeded"
}
```

---

## <a name="next-steps"></a>Volgende stappen

Verken vervolgens een Java Swing-toepassing die Computer Vision gebruikt om optische tekenherkenning (OCR) uit te voeren; slim bijgesneden miniaturen maken; en visuele functies in afbeeldingen detecteren, categoriseren, taggen en beschrijven.

> [!div class="nextstepaction"]
> [Zelfstudie voor de Computer Vision-API met Java](../Tutorials/java-tutorial.md)

* Als u snel wilt experimenteren met de Computer Vision-API, gebruikt u de [Open API-testconsole](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console).
