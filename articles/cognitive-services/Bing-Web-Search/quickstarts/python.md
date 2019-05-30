---
title: 'Snelstartgids: Een zoekopdracht uitvoeren met Python - Bing Webzoekopdrachten-API'
titleSuffix: Azure Cognitive Services
description: Gebruik deze snelstartgids om aanvragen naar de REST API van Bing Web Search te verzenden via Python en een JSON-antwoord te ontvangen
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: quickstart
ms.date: 03/12/2019
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: 2f26392bdac34dd831e04c772e5357f5e41fc746
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66390214"
---
# <a name="quickstart-use-python-to-call-the-bing-web-search-api"></a>Snelstartgids: Python gebruiken voor het aanroepen van de Bing Webzoekopdrachten-API  

Gebruik deze quickstart om voor het eerst de Bing Webzoekopdrachten-API aan te roepen en het JSON-antwoord te ontvangen. Deze Python-toepassing een zoekaanvraag verzendt naar de API en het antwoord. Hoewel deze toepassing in Python is geschreven, is de API een RESTful-webservice die compatibel is met vrijwel elke programmeertaal.

Dit voorbeeld wordt uitgevoerd als een Jupyter-notitieblok in [MyBinder](https://mybinder.org). Selecteer de binder-badge starten:

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingWebSearchAPI.ipynb)

## <a name="prerequisites"></a>Vereisten

* [Python 2.x of 3.x](https://www.python.org/)

[!INCLUDE [bing-web-search-quickstart-signup](../../../../includes/bing-web-search-quickstart-signup.md)]

## <a name="define-variables"></a>Variabelen definiëren

Vervang de waarde `subscription_key` door een geldige abonnementssleutel uit uw Azure-account.

```python
subscription_key = "YOUR_ACCESS_KEY"
assert subscription_key
```

Declareer het eindpunt voor de Bing Webzoekopdrachten-API. Als er autorisatiefouten optreden, moet u deze waarde controleren ten aanzien van het eindpunt voor zoeken met Bing in uw Azure-dashboard.

```python
search_url = "https://api.cognitive.microsoft.com/bing/v7.0/search"
```

U kunt de zoekquery gerust aanpassen door de waarde voor `search_term` te vervangen.

```python
search_term = "Azure Cognitive Services"
```

## <a name="make-a-request"></a>Een aanvraag maken

Dit blok maakt gebruik van de `requests`-bibliotheek om de Bing Webzoekopdrachten-API aan te roepen en de resultaten als een JSON-object te retourneren. De API-sleutel wordt doorgegeven in de `headers`-woordenlijst, en de zoekterm en queryparameters worden doorgegeven in de `params`-woordenlijst. Raadpleeg de documentatie voor [Bing Webzoekopdrachten-API v7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference) voor een volledige lijst met opties en parameters.

```python
import requests

headers = {"Ocp-Apim-Subscription-Key" : subscription_key}
params  = {"q": search_term, "textDecorations":True, "textFormat":"HTML"}
response = requests.get(search_url, headers=headers, params=params)
response.raise_for_status()
search_results = response.json()
```

## <a name="format-and-display-the-response"></a>Het antwoord opmaken en weergeven

De `search_results` object bevat de lijst met zoekresultaten en metagegevens, zoals gerelateerde query's en pagina's. Deze code maakt gebruikt van de `IPython.display`-bibliotheek om het antwoord in uw browser op te maken en weer te geven.

```python
from IPython.display import HTML

rows = "\n".join(["""<tr>
                       <td><a href=\"{0}\">{1}</a></td>
                       <td>{2}</td>
                     </tr>""".format(v["url"],v["name"],v["snippet"]) \
                  for v in search_results["webPages"]["value"]])
HTML("<table>{0}</table>".format(rows))
```

## <a name="sample-code-on-github"></a>Voorbeeldcode in GitHub

Als u deze code lokaal wilt uitvoeren, vindt u het volledige [voorbeeld op GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/python/Search/BingWebSearchv7.py).

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Zelfstudie voor app met één pagina voor Bing Web Search](../tutorial-bing-web-search-single-page-app.md)

[!INCLUDE [bing-web-search-quickstart-see-also](../../../../includes/bing-web-search-quickstart-see-also.md)]
