---
title: Snelstartgids - verzenden een query naar de Bing lokale bedrijven zoeken-API in Python | Microsoft Docs
titleSuffix: Azure Cognitive Services
description: Gebruik dit artikel om te beginnen met de Bing API voor zoeken van lokale bedrijven in Python.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: article
ms.date: 11/01/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: e6626c1243f9f05a56effe3538870a5be9ac6b34
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65796555"
---
# <a name="quickstart-send-a-query-to-the-bing-local-business-search-api-in-python"></a>Quickstart: Een query verzenden naar de Bing lokale bedrijven zoeken-API in Python

Gebruik deze Quick Start om te beginnen met het verzenden van aanvragen naar de Bing lokale bedrijven zoeken-API, dit is een Cognitive Service van Azure. Terwijl deze eenvoudige toepassing is geschreven in Python, de API is een RESTful-Web-compatibel is met elke programmeertaal die HTTP-aanvragen en parseren van JSON.

In dit voorbeeld van de toepassing lokaal antwoordgegevens worden opgehaald uit de API voor de zoekquery `hotel in Bellevue`.

## <a name="prerequisites"></a>Vereisten

* [Python](https://www.python.org/) 2.x of 3.x
 
Hebt u een [Cognitive Services-API-account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) met Bing-API's. De [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) is voldoende voor deze snelstartgids. Gebruik de toegangssleutel die is geleverd door de gratis proefversie.  Zie ook [Prijsinformatie Cognitive Services - Bing Zoeken-API](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

## <a name="run-the-complete-application"></a>De volledige toepassing uitvoeren

De volgende code haalt de gelokaliseerde resultaten. De code wordt geïmplementeerd in de volgende stappen:
1. Declareer variabelen om het eindpunt op te geven met een host en pad.
2. Geef de queryparameter. 
3. Definieer de Search-functie die wordt gemaakt van de aanvraag en de kop Ocp-Apim-Subscription-Key toegevoegd.
4. Stel de kop Ocp-Apim-Subscription-Key. 
5. Verbinding maken en verzend de aanvraag.
6. Geef de JSON-resultaten weer.

Dit is de volledige code voor deze demo:

```
import http.client, urllib.parse
import json

# Replace the subscriptionKey string value with your valid subscription key.
subscriptionKey = 'YOUR-SUBSCRIPTION-KEY'

host = 'api.cognitive.microsoft.com/bing'
path = '/v7.0/localbusinesses/search'

query = 'restaurant in Bellevue'

params = '?q=' + urllib.parse.quote (query) + '&mkt=en-us'

def get_local():
    headers = {'Ocp-Apim-Subscription-Key': subscriptionKey}
    conn = http.client.HTTPSConnection (host)
    conn.request ("GET", path + params, None, headers)
    response = conn.getresponse ()
    return response.read ()

result = get_local()
print (json.dumps(json.loads(result), indent=4))

```

## <a name="next-steps"></a>Volgende stappen
- [Lokale bedrijven zoeken Java-snelstartgids](local-search-java-quickstart.md)
- [Lokale bedrijven zoeken C# Quick Start](local-quickstart.md)
- [Lokale bedrijven zoeken Node-Quickstart](local-search-node-quickstart.md)
