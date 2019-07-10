---
title: 'Quickstart: Teksttaal detecteren, Node.js - Translator Text-API'
titleSuffix: Azure Cognitive Services
description: In deze quickstart leert u hoe u de taal van opgegeven tekst kunt identificeren met behulp van Node.js en de Translator Text-API.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: quickstart
ms.date: 06/04/2019
ms.author: swmachan
ms.openlocfilehash: 59a779fe27dd9f83e50df34e5d2febd46011d4aa
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67705534"
---
# <a name="quickstart-use-the-translator-text-api-to-detect-text-language-with-nodejs"></a>Quickstart: De Translator Text-API gebruiken om teksttaal te detecteren met Node.js

In deze quickstart leert u hoe u de taal van opgegeven tekst kunt detecteren met behulp van Node.js en de Translator Text-API.

Voor deze snelstart is een [Azure Cognitive Services-account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) met een Translator Text-resource vereist. Als u geen account hebt, kunt u de [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/) gebruiken om een abonnementssleutel op te halen.

>[!TIP]
> Als u wilt alle code in één oogopslag zien, de broncode voor dit voorbeeld is beschikbaar op [GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-NodeJS).

## <a name="prerequisites"></a>Vereisten

Voor deze snelstart zijn de volgende zaken vereist:

* [Node 8.12.x of hoger](https://nodejs.org/en/)
* Een Azure-abonnementssleutel voor Translator Text

## <a name="create-a-project-and-import-required-modules"></a>Een project maken en de vereiste modules importeren

Maak een nieuw project met uw favoriete IDE of editor. Kopieer dit codefragment naar uw project in een bestand met de naam `detect.js`.

```javascript
const request = require('request');
const uuidv4 = require('uuid/v4');
```

> [!NOTE]
> Als u deze modules nog niet hebt gebruikt, moet u ze installeren voordat u het programma uitvoert. Voer voor het installeren van deze pakketten voert u `npm install request uuidv4` uit.

Deze modules zijn vereist om de HTTP-aanvraag te maken en om een unieke id voor de `'X-ClientTraceId'`-header te maken.

## <a name="set-the-subscription-key"></a>Abonnementssleutel instellen

Deze code wordt gebruikt om te proberen de Translator Text-abonnementssleutel te lezen uit de omgevingsvariabele `TRANSLATOR_TEXT_KEY`. Als u niet bekend bent met omgevingsvariabelen, kunt u `subscriptionKey` als tekenreeks instellen en een opmerking plaatsen in de voorwaardelijke instructie.

Kopieer deze code naar uw project:

```javascript
/* Checks to see if the subscription key is available
as an environment variable. If you are setting your subscription key as a
string, then comment these lines out.

If you want to set your subscription key as a string, replace the value for
the Ocp-Apim-Subscription-Key header as a string. */
const subscriptionKey = process.env.TRANSLATOR_TEXT_KEY;
if (!subscriptionKey) {
  throw new Error('Environment variable for your subscription key is not set.')
};
```

## <a name="configure-the-request"></a>Aanvraag configureren

Met de methode `request()`, beschikbaar gesteld via de aanvraagmodule, kunt u de HTTP-methode, URL, aanvraagparameters, headers en de JSON-hoofdtekst doorgeven als een `options`-object. In dit codefragment configureert u de aanvraag:

>[!NOTE]
> Meer informatie over eindpunten, routes en aanvraagparameters vindt u in [Translator Text-API 3.0: detecteren](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-detect).

```javascript
let options = {
    method: 'POST',
    baseUrl: 'https://api.cognitive.microsofttranslator.com/',
    url: 'detect',
    qs: {
      'api-version': '3.0',
    },
    headers: {
      'Ocp-Apim-Subscription-Key': subscriptionKey,
      'Content-type': 'application/json',
      'X-ClientTraceId': uuidv4().toString()
    },
    body: [{
          'text': 'Salve, mondo!'
    }],
    json: true,
};
```
U kunt aanvragen het eenvoudigst verifiëren door uw abonnementssleutel op te geven als `Ocp-Apim-Subscription-Key`-header. Dat doen we in dit voorbeeld dan ook. Als alternatief kunt u in plaats van uw abonnementssleutel een toegangstoken gebruiken en het toegangstoken opgeven als `Authorization`-header voor het valideren van uw aanvraag.

Als u een abonnement van Cognitive Services-meerdere services gebruikt, moet u ook de `Ocp-Apim-Subscription-Region` in uw aanvraagheaders.

Zie [Verificatie](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication) voor meer informatie.

## <a name="make-the-request-and-print-the-response"></a>De aanvraag maken en het antwoord afdrukken

Vervolgens maakt u de aanvraag via de methode `request()`. Deze gebruikt het `options`-object dat in de vorige sectie als eerste argument is gemaakt, waarna het opgemaakte JSON-antwoord wordt afgedrukt.

```javascript
request(options, function(err, res, body){
    console.log(JSON.stringify(body, null, 4));
});
```

>[!NOTE]
> In dit voorbeeld definieert u de HTTP-aanvraag in het `options`-object. De aanvraagmodule ondersteunt echter ook helpermethoden, zoals `.post` en `.get`. Zie [Helpermethoden](https://github.com/request/request#convenience-methods) voor meer informatie.

## <a name="put-it-all-together"></a>Alles samenvoegen

Dat was het. U hebt een eenvoudig programma gemaakt dat we de Translator Text-API zullen noemen. Er is een JSON-antwoord geretourneerd. Het is nu tijd om uw programma uit te voeren:

```console
node detect.js
```

Als u uw code graag wilt vergelijken met de onze, kunt u het volledige voorbeeld vinden op [GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-NodeJS).

## <a name="sample-response"></a>Voorbeeldantwoord

De afkorting van land/regio niet vinden in dit [lijst met talen](https://docs.microsoft.com/azure/cognitive-services/translator/language-support).

```json
[
    {
        "alternatives": [
            {
                "isTranslationSupported": true,
                "isTransliterationSupported": false,
                "language": "pt",
                "score": 1.0
            },
            {
                "isTranslationSupported": true,
                "isTransliterationSupported": false,
                "language": "en",
                "score": 1.0
            }
        ],
        "isTranslationSupported": true,
        "isTransliterationSupported": false,
        "language": "it",
        "score": 1.0
    }
]
```

## <a name="clean-up-resources"></a>Resources opschonen

Als u uw abonnementssleutel hebt vastgelegd in het programma, verwijdert u deze sleutel wanneer u klaar bent met de snelstart.

## <a name="next-steps"></a>Volgende stappen

Bekijk de API-verwijzing om te begrijpen van alles wat die u met de Translator Text-API doen kunt.

> [!div class="nextstepaction"]
> [API-naslaginformatie](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference)

## <a name="see-also"></a>Zie ook

Naast taaldetectie kunt u de Translator Text-API ook gebruiken voor het volgende:

* [Tekst vertalen](quickstart-nodejs-translate.md)
* [Tekst transcriberen](quickstart-nodejs-transliterate.md)
* [Alternatieve vertalingen verkrijgen](quickstart-nodejs-dictionary.md)
* [Een lijst ophalen van ondersteunde talen](quickstart-nodejs-languages.md)
* [De zinlengte in invoer bepalen](quickstart-nodejs-sentences.md)
