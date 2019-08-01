---
title: 'Quickstart: Project URL Preview, Node.js'
titlesuffix: Azure Cognitive Services
description: Ga aan de slag met URL Preview in Microsoft Cognitive Services in Azure.
services: cognitive-services
author: mikedodaro
manager: nitinme
ms.service: cognitive-services
ms.subservice: url-preview
ms.topic: quickstart
ms.date: 03/16/2018
ms.author: rosh
ms.openlocfilehash: 92124a53c871be7a83d7b276e31f000d7c674358
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2019
ms.locfileid: "68698017"
---
# <a name="quickstart-url-preview-with-nodejs"></a>Quickstart: URL Preview met Node.js 

In het volgende voorbeeld van Node wordt er een URL-voorbeeld gemaakt voor de SwiftKey-website: https://swiftkey.com/en.

## <a name="prerequisites"></a>Vereisten

Vraag een toegangssleutel aan voor de gratis proefversie van [Cognitive Services Labs](https://labs.cognitive.microsoft.com/en-us/project-answer-search)

## <a name="code-scenario"></a>Codescenario 

Met de volgende code kunt u gegevens ophalen voor een voorbeeld van een URL.
De code wordt in de volgende stappen geïmplementeerd:
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
let subscriptionKey = 'YOUR-ACCESS-KEY'; 
let host = 'api.labs.cognitive.microsoft.com';
let path = '/urlpreview/v7.0/search';

let mkt = 'en-US';
let q = 'https://swiftkey.com/';

let params = '?q=' + encodeURI(q);

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
- [Voorbeeldcode van C#](csharp.md)
- [Snelstart voor Java](java-quickstart.md)
- [Snelstart voor JavaScript](javascript.md)
- [Snelstart voor Python](python-quickstart.md)
