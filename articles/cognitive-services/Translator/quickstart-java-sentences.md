---
title: 'Quickstart: Zinlengtes ophalen, Java - Translator Text-API'
titleSuffix: Azure Cognitive Services
description: In deze quickstart leert u hoe u zinlengtes bepaalt met behulp van Java en de Translator Text-API.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: quickstart
ms.date: 06/04/2019
ms.author: swmachan
ms.openlocfilehash: e47bbd4f50a1521eebc15f3f812f31699a7ddf13
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67704479"
---
# <a name="quickstart-use-the-translator-text-api-to-determine-sentence-length-using-java"></a>Quickstart: De Translator Text-API gebruiken om de zinlengte te bepalen met Java

In deze quickstart leert u hoe u zinlengtes bepaalt met behulp van Java en de Translator Text-API.

Voor deze snelstart is een [Azure Cognitive Services-account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) met een Translator Text-resource vereist. Als u geen account hebt, kunt u de [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/) gebruiken om een abonnementssleutel op te halen.

>[!TIP]
> Als u wilt alle code in één oogopslag zien, de broncode voor dit voorbeeld is beschikbaar op [GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-Java).

## <a name="prerequisites"></a>Vereisten

* [JDK 7 of hoger](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
* [Gradle](https://gradle.org/install/)
* Een Azure-abonnementssleutel voor Translator Text

## <a name="initialize-a-project-with-gradle"></a>Een project initialiseren met Gradle

We beginnen met het maken van een werkmap voor dit project. Voer de volgende opdracht uit vanaf de opdrachtregel (of terminal):

```console
mkdir length-sentence-sample
cd length-sentence-sample
```

Vervolgens gaat u een Gradle-project initialiseren. Met deze opdracht maakt u essentiële buildbestanden voor Gradle, maar nog belangrijker is dat ook `build.gradle.kts` wordt gemaakt, dat tijdens runtime wordt gebruikt om de toepassing te maken en te configureren. Voer de volgende opdracht uit vanuit uw werkmap:

```console
gradle init --type basic
```

Wanneer u wordt gevraagd om een **DSL** te kiezen, selecteert u **Kotlin**.

## <a name="configure-the-build-file"></a>Het buildbestand configureren

Zoek `build.gradle.kts` en open dit met uw favoriete IDE- of teksteditor. Kopieer het vervolgens in deze buildconfiguratie:

```
plugins {
    java
    application
}
application {
    mainClassName = "LengthSentence"
}
repositories {
    mavenCentral()
}
dependencies {
    compile("com.squareup.okhttp:okhttp:2.5.0")
    compile("com.google.code.gson:gson:2.8.5")
}
```

Opmerking: dit voorbeeld heeft afhankelijkheden op OkHttp voor HTTP-aanvragen, en Gson voor het verwerken en parseren van JSON. Zie [Nieuwe Gradle-builds maken](https://guides.gradle.org/creating-new-gradle-builds/) voor meer informatie over buildconfiguraties.

## <a name="create-a-java-file"></a>Een Java-bestand maken

We gaan een map maken voor de voorbeeld-app. Voer vanuit uw werkmap de volgende opdracht uit:

```console
mkdir -p src/main/java
```

Maak vervolgens in deze map een bestand met de naam `LengthSentence.java`.

## <a name="import-required-libraries"></a>Vereiste bibliotheken importeren

Open `LengthSentence.java` en voeg deze importinstructies toe:

```java
import java.io.*;
import java.net.*;
import java.util.*;
import com.google.gson.*;
import com.squareup.okhttp.*;
```


## <a name="define-variables"></a>Variabelen definiëren

U moet eerst een openbare klasse maken voor het project:

```java
public class LengthSentence {
  // All project code goes here...
}
```

Voeg deze regels toe aan de klasse `LengthSentence`. U ziet dat u niet alleen de `api-version`, maar ook de invoertaal kunt definiëren. In dit voorbeeld is het Engels.

```java
String subscriptionKey = "YOUR_SUBSCRIPTION_KEY";
String url = "https://api.cognitive.microsofttranslator.com/breaksentence?api-version=3.0&language=en";
```
Als u een abonnement van Cognitive Services-meerdere services gebruikt, moet u ook de `Ocp-Apim-Subscription-Region` in de parameters van de aanvraag. [Meer informatie over verificatie met het abonnement op meerdere services](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication).

## <a name="create-a-client-and-build-a-request"></a>Een client maken en een aanvraag samenstellen

Voeg deze regel toe aan de klasse `LengthSentence` om de `OkHttpClient` te instantiëren:

```java
// Instantiates the OkHttpClient.
OkHttpClient client = new OkHttpClient();
```

Stel vervolgens de POST-aanvraag samen. Als u wilt, kunt u de tekst wijzigen. De tekst moet een escape-teken bevatten.

```java
// This function performs a POST request.
public String Post() throws IOException {
    MediaType mediaType = MediaType.parse("application/json");
    RequestBody body = RequestBody.create(mediaType,
            "[{\n\t\"Text\": \"How are you? I am fine. What did you do today?\"\n}]");
    Request request = new Request.Builder()
            .url(url).post(body)
            .addHeader("Ocp-Apim-Subscription-Key", subscriptionKey)
            .addHeader("Content-type", "application/json").build();
    Response response = client.newCall(request).execute();
    return response.body().string();
}
```

## <a name="create-a-function-to-parse-the-response"></a>Een functie maken voor het parseren van het antwoord

Met deze eenvoudige functie wordt het JSON-antwoord van de Translator Text-service geparseerd en verfraaid.

```java
// This function prettifies the json response.
public static String prettify(String json_text) {
    JsonParser parser = new JsonParser();
    JsonElement json = parser.parse(json_text);
    Gson gson = new GsonBuilder().setPrettyPrinting().create();
    return gson.toJson(json);
}
```

## <a name="put-it-all-together"></a>Alles samenvoegen

De laatste stap bestaat uit het maken van een aanvraag en het ontvangen van een antwoord. Voeg deze regels toe aan het project:

```java
public static void main(String[] args) {
    try {
        LengthSentence lengthSentenceRequest = new LengthSentence();
        String response = lengthSentenceRequest.Post();
        System.out.println(prettify(response));
    } catch (Exception e) {
        System.out.println(e);
    }
}
```

## <a name="run-the-sample-app"></a>De voorbeeld-app uitvoeren

Nu kunt u de voorbeeld-app gaan uitvoeren. Ga vanaf de opdrachtregel (of terminalsessie) naar de hoofdmap van uw werkmap en voer de volgende opdracht uit:

```console
gradle build
```

Wanneer de build is voltooid, voert u het volgende uit:

```console
gradle run
```

## <a name="sample-response"></a>Voorbeeldantwoord

Een geslaagd antwoord wordt geretourneerd in de JSON-indeling, zoals u in het volgende voorbeeld kunt zien:

```json
[
  {
    "detectedLanguage": {
      "language": "en",
      "score": 1.0
    },
    "sentLen": [
      13,
      11,
      22
    ]
  }
]
```

## <a name="next-steps"></a>Volgende stappen

Bekijk de API-verwijzing om te begrijpen van alles wat die u met de Translator Text-API doen kunt.

> [!div class="nextstepaction"]
> [API-naslaginformatie](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference)

## <a name="see-also"></a>Zie ook

* [Tekst vertalen](quickstart-java-translate.md)
* [Tekst transcriberen](quickstart-java-transliterate.md)
* [Een taal identificeren op basis van de invoer](quickstart-java-detect.md)
* [Alternatieve vertalingen verkrijgen](quickstart-java-dictionary.md)
* [Een lijst ophalen van ondersteunde talen](quickstart-java-languages.md)
* [De zinlengte in invoer bepalen](quickstart-java-sentences.md)
