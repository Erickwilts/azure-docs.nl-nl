---
title: 'Snelstartgids: Zinlengtes ophalen, Python - Translator Text-API'
titleSuffix: Azure Cognitive Services
description: In deze quickstart leert u hoe u zinlengtes kunt bepalen (in tekens) met behulp van Python en de Translator Text-API.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: quickstart
ms.date: 06/04/2019
ms.author: swmachan
ms.openlocfilehash: 2aede18f3b093f52a205e053c72cac5a2e6e6b1b
ms.sourcegitcommit: a7ea412ca4411fc28431cbe7d2cc399900267585
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2019
ms.locfileid: "67357886"
---
# <a name="quickstart-use-the-translator-text-api-to-determine-sentence-length-using-python"></a>Snelstartgids: De Translator Text-API gebruiken om de zinlengte te bepalen met Python

In deze quickstart leert u hoe u zinlengtes kunt bepalen (in tekens) met behulp van Python en de Translator Text-API.

Voor deze snelstart is een [Azure Cognitive Services-account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) met een Translator Text-resource vereist. Als u geen account hebt, kunt u de [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/) gebruiken om een abonnementssleutel op te halen.

## <a name="prerequisites"></a>Vereisten

Voor deze snelstart zijn de volgende zaken vereist:

* Python 2.7.x of 3.x
* Een Azure-abonnementssleutel voor Translator Text

## <a name="create-a-project-and-import-required-modules"></a>Een project maken en de vereiste modules importeren

Maak een nieuw Python-project met uw favoriete IDE of editor. Kopieer dit codefragment naar uw project in een bestand met de naam `sentence-length.py`.

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

Het globaal eindpunt voor de Translator Text is ingesteld als de `base_url`. Met `path` wordt de `breaksentence`-route ingesteld en wordt bepaald dat we versie 3 van de API willen gebruiken.

De `params` in dit voorbeeld worden gebruikt om de taal van de opgegeven tekst in te stellen. `params` zijn niet vereist voor de route `breaksentence`. Wanneer de taal niet is vermeld in de aanvraag, wordt geprobeerd om met de API de taal van de opgegeven tekst te detecteren, en in het antwoord deze informatie te bieden samen met een betrouwbaarheidsscore.

>[!NOTE]
> Meer informatie over eindpunten, routes en aanvraagparameters vindt u in [Translator Text-API 3.0: talen](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-break-sentence).

```python
base_url = 'https://api.cognitive.microsofttranslator.com'
path = '/breaksentence?api-version=3.0'
params = '&language=en'
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

## <a name="create-a-request-to-determine-sentence-length"></a>Een aanvraag maken om de zinlengte te bepalen

Definieer de zin (of zinnen) waarvan u de lengte wilt bepalen:

```python
# You can pass more than one object in body.
body = [{
    'text': 'How are you? I am fine. What did you do today?'
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
python sentence-length.py
```

Als u uw code graag wilt vergelijken met de onze, kunt u het volledige voorbeeld vinden op [GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-Python).

## <a name="sample-response"></a>Voorbeeldantwoord

```json
[
    {
        "sentLen": [
            13,
            11,
            22
        ]
    }
]
```

## <a name="clean-up-resources"></a>Resources opschonen

Als u uw abonnementssleutel hebt vastgelegd in het programma, verwijdert u deze sleutel wanneer u klaar bent met de snelstart.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Python-voorbeelden op GitHub bekijken](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-Python)

## <a name="see-also"></a>Zie ook

Meer informatie over het gebruik van de Translator Text-API voor:

* [Tekst vertalen](quickstart-python-translate.md)
* [Tekst transcriberen](quickstart-python-transliterate.md)
* [Een taal identificeren op basis van de invoer](quickstart-python-detect.md)
* [Alternatieve vertalingen verkrijgen](quickstart-python-dictionary.md)
* [Een lijst ophalen van ondersteunde talen](quickstart-python-languages.md)
