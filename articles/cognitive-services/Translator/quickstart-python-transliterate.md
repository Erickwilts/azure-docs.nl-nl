---
title: 'Snelstartgids: Teksttransliteratie, Python - Translator Text-API'
titleSuffix: Azure Cognitive Services
description: In deze quickstart leert u hoe u tekst van het ene script translitereert (converteert) naar een ander met behulp van Python en de Translator Text REST API. In dit voorbeeld is sprake van transliteratie van Japans voor gebruik van het Latijnse alfabet.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: quickstart
ms.date: 06/04/2019
ms.author: swmachan
ms.openlocfilehash: cafaf67cfaa07d27bf4569efbc7f76196222cc2a
ms.sourcegitcommit: e72073911f7635cdae6b75066b0a88ce00b9053b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2019
ms.locfileid: "68348727"
---
# <a name="quickstart-use-the-translator-text-api-to-transliterate-text-using-python"></a>Snelstartgids: De Translator Text-API gebruiken voor transliteratie van tekst met Python

In deze quickstart leert u hoe u tekst van het ene script translitereert (converteert) naar een ander met behulp van Python en de Translator Text REST API. In het gegeven voorbeeld is sprake van transliteratie van Japans voor gebruik van het Latijnse alfabet.

Voor deze snelstart is een [Azure Cognitive Services-account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) met een Translator Text-resource vereist. Als u geen account hebt, kunt u de [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/) gebruiken om een abonnementssleutel op te halen.

>[!TIP]
> Als u alle code tegelijk wilt zien, is de bron code voor dit voor beeld beschikbaar op [github](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-Python).

## <a name="prerequisites"></a>Vereisten

Voor deze snelstart zijn de volgende zaken vereist:

* Python 2.7.x of 3.x
* Een Azure-abonnementssleutel voor Translator Text

## <a name="create-a-project-and-import-required-modules"></a>Een project maken en de vereiste modules importeren

Maak een nieuw project met behulp van uw favoriete IDE of editor of een nieuwe map met een `transliterate-text.py` bestand met de naam op uw bureau blad. Kopieer vervolgens dit code fragment naar uw project/bestand:

```python
# -*- coding: utf-8 -*-
import os
import requests
import uuid
import json
```

> [!NOTE]
> Als u deze modules nog niet hebt gebruikt, moet u ze installeren voordat u het programma uitvoert. Voer voor het installeren van deze pakketten voert u `pip install requests uuid` uit.

Met de eerste opmerking laat u de Python-vertaler weten dat UTF-8-codering moet worden gebruikt. De vereiste modules worden dan geïmporteerd voor het lezen van uw abonnementssleutel uit een omgevingsvariabele, voor het opstellen van de HTTP-aanvraag, voor het maken van een unieke id en voor het verwerken van het JSON-antwoord dat door de Translator Text-API wordt geretourneerd.

## <a name="set-the-subscription-key-base-url-and-path"></a>De abonnementssleutel, de basis-URL en het pad instellen

In dit voorbeeld laat u de Translator Text-abonnementssleutel ophalen uit de omgevingsvariabele `TRANSLATOR_TEXT_KEY`. Als u niet bekend bent met omgevingsvariabelen, kunt u `subscriptionKey` als tekenreeks instellen en een opmerking plaatsen in de voorwaardelijke instructie.

Kopieer deze code naar uw project:

```python
# Checks to see if the Translator Text subscription key is available
# as an environment variable. If you are setting your subscription key as a
# string, then comment these lines out.
if 'TRANSLATOR_TEXT_KEY' in os.environ:
    subscriptionKey = os.environ['TRANSLATOR_TEXT_KEY']
else:
    print('Environment variable for TRANSLATOR_TEXT_KEY is not set.')
    exit()
# If you want to set your subscription key as a string, uncomment the line
# below and add your subscription key.
#subscriptionKey = 'put_your_key_here'
```

Het globaal eindpunt voor de Translator Text is ingesteld als de `base_url`. Met `path` wordt de `transliterate`-route ingesteld en wordt bepaald dat we versie 3 van de API willen gebruiken.

De `params` worden gebruikt om de invoertaal, en het invoer- en uitvoerscript in te stellen. In dit voorbeeld is sprake van transliteratie van Japans naar het Latijnse alfabet.

>[!NOTE]
> Meer informatie over eindpunten, routes en aanvraagparameters vindt u in [Translator Text-API 3.0: Transliteratie](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-transliterate).

```python
base_url = 'https://api.cognitive.microsofttranslator.com'
path = '/transliterate?api-version=3.0'
params = '&language=ja&fromScript=jpan&toScript=latn'
constructed_url = base_url + path + params
```

## <a name="add-headers"></a>Headers toevoegen

U kunt aanvragen het eenvoudigst verifiëren door uw abonnementssleutel op te geven als `Ocp-Apim-Subscription-Key`-header. Dat doen we in dit voorbeeld dan ook. Als alternatief kunt u in plaats van uw abonnementssleutel een toegangstoken gebruiken en het toegangstoken opgeven als `Authorization`-header voor het valideren van uw aanvraag. Zie [Verificatie](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication) voor meer informatie.

Kopieer dit codefragment naar uw project:

```python
headers = {
    'Ocp-Apim-Subscription-Key': subscriptionKey,
    'Content-type': 'application/json',
    'X-ClientTraceId': str(uuid.uuid4())
}
```

Als u een Cognitive Services abonnement op meerdere services gebruikt, moet u ook de `Ocp-Apim-Subscription-Region` in uw aanvraag parameters toevoegen. Meer [informatie over verificatie met het multi-service-abonnement](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication).

## <a name="create-a-request-to-transliterate-text"></a>Een aanvraag maken voor de transliteratie van tekst

Definieer de tekenreeks (of tekenreeksen) die u wilt translitereren:

```python
# Transliterate "good afternoon" from source Japanese.
# Note: You can pass more than one object in body.
body = [{
    'text': 'こんにちは'
}]
```

Vervolgens maakt u een POST-aanvraag met de `requests`-module. Hier zijn drie argumenten voor nodig: de samengevoegde URL, de aanvraagheaders en de aanvraagbody:

```python
request = requests.post(constructed_url, headers=headers, json=body)
response = request.json()
```

## <a name="print-the-response"></a>Het antwoord weergeven

De laatste stap is het weergeven van de antwoorden. Met dit codefragment worden de resultaten verfraaid: de sleutels worden gesorteerd, er wordt gebruikgemaakt van inspringing en er worden item- en sleutelscheidingstekens opgegeven.

```python
print(json.dumps(response, sort_keys=True, indent=4,
                 ensure_ascii=False, separators=(',', ': ')))
```

## <a name="put-it-all-together"></a>Alles samenvoegen

Dat was het. U hebt een eenvoudig programma gemaakt dat we de Translator Text-API zullen noemen. Er is een JSON-antwoord geretourneerd. Het is nu tijd om uw programma uit te voeren:

```console
python transliterate-text.py
```

Als u uw code graag wilt vergelijken met de onze, kunt u het volledige voorbeeld vinden op [GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-Python).

## <a name="sample-response"></a>Voorbeeldantwoord

```json
[
    {
        "script": "latn",
        "text": "konnichiwa"
    }
]
```

## <a name="clean-up-resources"></a>Resources opschonen

Als u uw abonnementssleutel hebt vastgelegd in het programma, verwijdert u deze sleutel wanneer u klaar bent met de snelstart.

## <a name="next-steps"></a>Volgende stappen

Bekijk de API-verwijzing voor meer informatie over wat u met de Translator Text-API kunt doen.

> [!div class="nextstepaction"]
> [API-naslaginformatie](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference)

## <a name="see-also"></a>Zie ook

Meer informatie over het gebruik van de Translator Text-API voor:

* [Tekst vertalen](quickstart-python-translate.md)
* [Een taal identificeren op basis van de invoer](quickstart-python-detect.md)
* [Alternatieve vertalingen verkrijgen](quickstart-python-dictionary.md)
* [Een lijst ophalen van ondersteunde talen](quickstart-python-languages.md)
* [De zinlengte in invoer bepalen](quickstart-python-sentences.md)
