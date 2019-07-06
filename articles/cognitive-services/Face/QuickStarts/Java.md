---
title: 'Quickstart: Gezichten in een afbeelding detecteren met de Azure REST API en Java'
titleSuffix: Azure Cognitive Services
description: In deze snelstart gebruikt u de Azure Face REST API met Java om gezichten in een afbeelding te detecteren.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: quickstart
ms.date: 07/03/2019
ms.author: pafarley
ms.openlocfilehash: d014785a0e866301e228458fe3742b899bd1f192
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67606963"
---
# <a name="quickstart-detect-faces-in-an-image-using-the-rest-api-and-java"></a>Quickstart: Gezichten in een afbeelding detecteren met de REST API en Java

In deze snelstart gebruikt u de Azure Face REST API met Java om menselijke gezichten in een afbeelding te detecteren.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint. 

## <a name="prerequisites"></a>Vereisten

- Een Face-API-abonnementssleutel. U kunt een abonnementssleutel voor een gratis proefversie downloaden van [Cognitive Services proberen](https://azure.microsoft.com/try/cognitive-services/?api=face-api). Of volg de instructies in [Een Cognitive Services-account maken](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) om u te abonneren op de Face-API-service en uw sleutel op te halen.
- Een Java IDE naar keuze.

## <a name="create-the-java-project"></a>Het Java-project maken

1. Een nieuw opdrachtregelprogramma Java-app maken in uw IDE en voeg een **Main** klasse met een **belangrijkste** methode.
1. Importeer de volgende bibliotheken in uw Java-project. Als u Maven gebruikt, worden de Maven-coördinaten opgegeven voor elke bibliotheek.
   - [Apache HTTP-client](https://hc.apache.org/downloads.cgi) (org.apache.httpcomponents:httpclient:4.5.6)
   - [Apache HTTP-core](https://hc.apache.org/downloads.cgi) (org.apache.httpcomponents:httpcore:4.4.10)
   - [JSON-bibliotheek](https://github.com/stleary/JSON-java) (org.json:json:20180130)
   - [Apache Commons logboekregistratie](https://commons.apache.org/proper/commons-logging/download_logging.cgi) (commons-logboekregistratie: commons-logboekregistratie: 1.1.2)

## <a name="add-face-detection-code"></a>Gezichtsdetectiecode toevoegen

Open de klasse Main van uw project. Hier voegt u de code toe voor het laden van afbeeldingen en detecteren van gezichten.

### <a name="import-packages"></a>Pakketten importeren

Voeg boven aan het bestand de volgende `import`-instructies toe.

```java
// This sample uses Apache HttpComponents:
// http://hc.apache.org/httpcomponents-core-ga/httpcore/apidocs/
// https://hc.apache.org/httpcomponents-client-ga/httpclient/apidocs/

import java.net.URI;
import org.apache.http.HttpEntity;
import org.apache.http.HttpResponse;
import org.apache.http.client.HttpClient;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.StringEntity;
import org.apache.http.client.utils.URIBuilder;
import org.apache.http.impl.client.HttpClientBuilder;
import org.apache.http.util.EntityUtils;
import org.json.JSONArray;
import org.json.JSONObject;
```

### <a name="add-essential-fields"></a>Essentiële velden toevoegen

Vervang de **Main** klasse met de volgende code. Met deze gegevens wordt aangegeven hoe de Face-service moet worden verbonden en waar de invoergegevens kunnen worden opgehaald. U moet het veld `subscriptionKey` bijwerken met de waarde van de abonnementssleutel en mogelijk moet u de tekenreeks `uriBase` wijzigen, zodat deze de juiste regio-id bevat (zie de [Face API-documentatie](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) voor een lijst met alle regio-eindpunten). U kunt ook de waarde `imageWithFaces` instellen voor een pad dat naar een ander afbeeldingsbestand verwijst.

Het veld `faceAttributes` is gewoon een lijst met bepaalde kenmerktypen. Dit veld geeft aan welke informatie over de gedetecteerde gezichten moet worden opgehaald.

```Java
public class Main {
    // Replace <Subscription Key> with your valid subscription key.
    private static final String subscriptionKey = "<Subscription Key>";

    // NOTE: You must use the same region in your REST call as you used to
    // obtain your subscription keys. For example, if you obtained your
    // subscription keys from westus, replace "westcentralus" in the URL
    // below with "westus".
    //
    // Free trial subscription keys are generated in the "westus" region. If you
    // use a free trial subscription key, you shouldn't need to change this region.
    private static final String uriBase =
        "https://westcentralus.api.cognitive.microsoft.com/face/v1.0/detect";

    private static final String imageWithFaces =
        "{\"url\":\"https://upload.wikimedia.org/wikipedia/commons/c/c3/RH_Louise_Lillian_Gish.jpg\"}";

    private static final String faceAttributes =
        "age,gender,headPose,smile,facialHair,glasses,emotion,hair,makeup,occlusion,accessories,blur,exposure,noise";
```

### <a name="call-the-face-detection-rest-api"></a>De REST API voor de gezichtsdetectie aanroepen

Voeg de **belangrijkste** methode met de volgende code. Hiermee wordt een REST-aanroep naar de Face-API gemaakt om informatie over het gezicht te detecteren in de externe afbeelding (de tekenreeks `faceAttributes` geeft aan welke gezichtskenmerken moeten worden opgehaald). Vervolgens worden de uitvoergegevens naar een JSON-tekenreeks geschreven.

```Java
    public static void main(String[] args) {
        HttpClient httpclient = HttpClientBuilder.create().build();

        try
        {
            URIBuilder builder = new URIBuilder(uriBase);

            // Request parameters. All of them are optional.
            builder.setParameter("returnFaceId", "true");
            builder.setParameter("returnFaceLandmarks", "false");
            builder.setParameter("returnFaceAttributes", faceAttributes);

            // Prepare the URI for the REST API call.
            URI uri = builder.build();
            HttpPost request = new HttpPost(uri);

            // Request headers.
            request.setHeader("Content-Type", "application/json");
            request.setHeader("Ocp-Apim-Subscription-Key", subscriptionKey);

            // Request body.
            StringEntity reqEntity = new StringEntity(imageWithFaces);
            request.setEntity(reqEntity);

            // Execute the REST API call and get the response entity.
            HttpResponse response = httpclient.execute(request);
            HttpEntity entity = response.getEntity();
```

### <a name="parse-the-json-response"></a>Het JSON-antwoord parseren

Direct onder de vorige code voegt u het volgende blok toe. Hiermee worden de geretourneerde JSON-gegevens omgezet in een meer leesbare indeling voordat ze in de console worden weergegeven. Ten slotte de trycatch-blokken, sluiten de **belangrijkste** methode, en de **Main** klasse.

```Java
            if (entity != null)
            {
                // Format and display the JSON response.
                System.out.println("REST Response:\n");

                String jsonString = EntityUtils.toString(entity).trim();
                if (jsonString.charAt(0) == '[') {
                    JSONArray jsonArray = new JSONArray(jsonString);
                    System.out.println(jsonArray.toString(2));
                }
                else if (jsonString.charAt(0) == '{') {
                    JSONObject jsonObject = new JSONObject(jsonString);
                    System.out.println(jsonObject.toString(2));
                } else {
                    System.out.println(jsonString);
                }
            }
        }
        catch (Exception e)
        {
            // Display error message.
            System.out.println(e.getMessage());
        }
    }
}
```

## <a name="run-the-app"></a>De app uitvoeren

Compileer de code en voer deze uit. Bij een geslaagd antwoord worden de Face-gegevens in de console weergegeven in een makkelijk leesbare JSON-indeling. Bijvoorbeeld:

```json
[{
  "faceRectangle": {
    "top": 131,
    "left": 177,
    "width": 162,
    "height": 162
  },
  "faceAttributes": {
    "makeup": {
      "eyeMakeup": true,
      "lipMakeup": true
    },
    "facialHair": {
      "sideburns": 0,
      "beard": 0,
      "moustache": 0
    },
    "gender": "female",
    "accessories": [],
    "blur": {
      "blurLevel": "low",
      "value": 0.06
    },
    "headPose": {
      "roll": 0.1,
      "pitch": 0,
      "yaw": -32.9
    },
    "smile": 0,
    "glasses": "NoGlasses",
    "hair": {
      "bald": 0,
      "invisible": false,
      "hairColor": [
        {
          "color": "brown",
          "confidence": 1
        },
        {
          "color": "black",
          "confidence": 0.87
        },
        {
          "color": "other",
          "confidence": 0.51
        },
        {
          "color": "blond",
          "confidence": 0.08
        },
        {
          "color": "red",
          "confidence": 0.08
        },
        {
          "color": "gray",
          "confidence": 0.02
        }
      ]
    },
    "emotion": {
      "contempt": 0,
      "surprise": 0.005,
      "happiness": 0,
      "neutral": 0.986,
      "sadness": 0.009,
      "disgust": 0,
      "anger": 0,
      "fear": 0
    },
    "exposure": {
      "value": 0.67,
      "exposureLevel": "goodExposure"
    },
    "occlusion": {
      "eyeOccluded": false,
      "mouthOccluded": false,
      "foreheadOccluded": false
    },
    "noise": {
      "noiseLevel": "low",
      "value": 0
    },
    "age": 22.9
  },
  "faceId": "49d55c17-e018-4a42-ba7b-8cbbdfae7c6f"
}]
```

## <a name="next-steps"></a>Volgende stappen

In deze snelstart hebt u een eenvoudige Java-consoletoepassing gemaakt die gebruikmaakt van REST-aanroepen met de Azure Face-API voor het detecteren van gezichten in een afbeelding en het retourneren van de kenmerken. Vervolgens leert u hoe u meer kunt doen met deze functionaliteit in een Android-toepassing.

> [!div class="nextstepaction"]
> [Zelfstudie: Een Android-app maken om gezichten te herkennen en omlijsten](../Tutorials/FaceAPIinJavaForAndroidTutorial.md)
