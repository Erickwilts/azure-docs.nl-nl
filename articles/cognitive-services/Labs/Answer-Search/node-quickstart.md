---
title: 'Quickstart: Project Answer Search, Node'
description: Aan de slag te gaan met Project Answer Search en Node.
services: cognitive-services
author: mikedodaro
manager: nitinme
ms.service: cognitive-services
ms.subservice: answer-search
ms.topic: quickstart
ms.date: 04/13/2018
ms.author: rosh
ROBOTS: NOINDEX
ms.openlocfilehash: ace83b9bdc8003e2e38d5b0a188582bc0cc2147c
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68707098"
---
# <a name="quickstart-project-answer-search-with-node"></a>Quickstart: Project Answer Search met Node

In het volgende Node-voorbeeld wordt een query gemaakt voor informatie over Yosemite National Park.

## <a name="prerequisites"></a>Vereisten

Vraag een toegangssleutel aan voor de gratis proefversie van [Cognitive Services Labs](https://labs.cognitive.microsoft.com/en-us/project-answer-search)

In dit voorbeeld wordt Node v8.9.4 gebruikt

## <a name="code-scenario"></a>Codescenario 

Met de volgende code worden antwoorden verkregen.
De code wordt geïmplementeerd in de volgende stappen:
1. Declareer variabelen om het eindpunt op te geven met een host en pad.
2. Geef de query-URL op waarvan u een voorbeeld wilt maken en voeg de queryparameter toe.  
3. Maak een handlerfunctie voor het antwoord.
4. Definieer de Search-functie die de aanvraag maakt en de header *Ocp-Apim-Subscription-Key* toevoegt.
5. Voer de Search-functie uit. 

Dit is de volledige code voor deze demo:

```
'use strict';

let https = require('https');

// Replace the subscriptionKey string value with your valid subscription key.
let subscriptionKey = 'YOUR-SUBSCRIPTION-KEY'; 

let host = 'api.labs.cognitive.microsoft.com';
let path = '/answerSearch/v7.0/search';

let mkt = 'en-us';
let q = 'Yosemite National Park';

let params = '?q=' + encodeURI(q) + '&mkt= + mkt;

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
- [Voorbeeldcode van C#](c-sharp-quickstart.md)
- [Snelstartgids voor Java](java-quickstart.md)
- [Snelstartgids voor Python](python-quickstart.md)
