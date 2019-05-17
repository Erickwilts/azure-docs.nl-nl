---
title: Snelstartgids - verzenden een query naar de Bing lokale bedrijven zoeken-API met behulp van Node.js | Microsoft Docs
titleSuffix: Azure Cognitive Services
description: Start met behulp van de Bing API voor zoeken van lokale bedrijven in knooppunt.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: article
ms.date: 11/01/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: d0760f89eb98955f7ebb503ce59f904192635f7a
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65796882"
---
# <a name="quickstart-send-a-query-to-the-bing-local-business-search-api-using-nodejs"></a>Quickstart: Een query verzenden naar de Bing lokale bedrijven zoeken-API met behulp van Node.js

Gebruik deze Quick Start om te beginnen met het verzenden van aanvragen naar de Bing lokale bedrijven zoeken-API, dit is een Cognitive Service van Azure. Terwijl deze eenvoudige toepassing is geschreven in Node.js, de API is een RESTful-Web-compatibel is met elke programmeertaal die HTTP-aanvragen en parseren van JSON.

In dit voorbeeld van de toepassing lokaal antwoordgegevens worden opgehaald uit de API voor de zoekquery `hotel in Bellevue`.

## <a name="prerequisites"></a>Vereisten

* Nieuwste versie van [Node.js](https://nodejs.org/en/download/).

* De [JavaScript-aanvragenbibliotheek](https://github.com/request/request)

Hebt u een [Cognitive Services-API-account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) met Bing-API's. De [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) is voldoende voor deze snelstartgids. Gebruik de toegangssleutel die is geleverd door de gratis proefversie.  Zie ook [Prijsinformatie Cognitive Services - Bing Zoeken-API](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

## <a name="code-scenario"></a>Codescenario

De volgende code haalt definieert en stuurt de aanvraag. De code wordt geïmplementeerd in de volgende stappen:

1. Declareer variabelen om het eindpunt op te geven met een host en pad.
2. Geef de query en de queryparameter toevoegen.
3. Maak een handlerfunctie voor het antwoord.
4. Definieer de Search-functie die wordt gemaakt van de aanvraag en de kop Ocp-Apim-Subscription-Key toegevoegd.
5. Voer de Search-functie uit.

Dit is de volledige code voor deze demo:

```
'use strict';

let https = require('https');

// Replace the subscriptionKey string value with your valid subscription key.
let subscriptionKey = 'your-access-key';

let host = 'api.cognitive.microsoft.com/bing';
let path = '/v7.0/localbusinesses/search';

let mkt = 'en-US';
let q = 'hotel in Bellevue';

let params = '?q=' + encodeURI(q) + "&mkt=" + mkt;

let response_handler = function (response) {
    let body = '';
    response.on('data', function (d) {
        body += d;
    });
    response.on('end', function () {
        let body_ = JSON.parse(body);
        let body__ = JSON.stringify(body_, null, '  ');
        console.log(body__);
    });
    response.on('error', function (e) {
        console.log('Error: ' + e.message);
    });
};

let Search = function () {
    let request_params = {
        method: 'GET',
        hostname: host,
        path: path + params,
        headers: {
            'Ocp-Apim-Subscription-Key': subscriptionKey,
        }
    };

    let req = https.request(request_params, response_handler);
    req.end();
}

Search();

```

## <a name="next-steps"></a>Volgende stappen

* [Snelstartgids voor lokale bedrijven zoeken](local-quickstart.md)
* [Local Business Search Java quickstart](local-search-java-quickstart.md)
* [Local Business Search Python quickstart](local-search-python-quickstart.md)
