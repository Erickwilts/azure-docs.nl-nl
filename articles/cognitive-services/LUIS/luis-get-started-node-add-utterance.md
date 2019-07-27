---
title: Wijzigen, app trainen, node. js-LUIS
titleSuffix: Azure Cognitive Services
description: Voeg in deze Node.js-snelstart voorbeeldutterances toe aan een Home Automation-app en train de app.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
ms.date: 05/29/2019
ms.author: diberry
ms.openlocfilehash: 6f0b19c1ba8d4a72ced19e74a3807c3962989e5d
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68560573"
---
# <a name="quickstart-change-model-using-nodejs"></a>Snelstartgids: Model wijzigen met behulp van Node.js

[!INCLUDE [Quickstart prerequisites for changing model](../../../includes/cognitive-services-luis-qs-change-model-intro-para.md)]

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [Quickstart prerequisites for changing model](../../../includes/cognitive-services-luis-qs-change-model-prereq.md)]
* Meest recente [**Node.js**](https://nodejs.org/en/download/) met NPM.
* NPM-afhankelijkheden voor dit artikel: [**request**](https://www.npmjs.com/package/request), [**request-promise**](https://www.npmjs.com/package/request-promise), [**fs-extra**](https://www.npmjs.com/package/fs-extra).  
* [Visual Studio Code](https://code.visualstudio.com/).

[!INCLUDE [Code is available in Azure-Samples GitHub repo](../../../includes/cognitive-services-luis-qs-change-model-luis-repo-note.md)]

## <a name="example-utterances-json-file"></a>JSON-bestand met voorbeeldutterances

[!INCLUDE [Quickstart explanation of example utterance JSON file](../../../includes/cognitive-services-luis-qs-change-model-json-ex-utt.md)]

## <a name="create-quickstart-code"></a>Snelstartcode maken 

Voeg de NPM-afhankelijkheden toe aan het bestand `add-utterances.js`.

   [!code-javascript[NPM Dependencies](~/samples-luis/documentation-samples/quickstarts/change-model/node/add-utterances.js?range=8-11 "NPM Dependencies")]

Voeg de LUIS-constanten toe aan het bestand. Kopieer de volgende code en wijzig deze met uw creatiecode, toepassings-id en versie-id.

   [!code-javascript[LUIS key and IDs](~/samples-luis/documentation-samples/quickstarts/change-model/node/add-utterances.js?range=13-22 "LUIS key and IDs")]

Voeg de naam en locatie toe van het uploadbestand dat uw utterances bevat. 

   [!code-javascript[Add upload file](~/samples-luis/documentation-samples/quickstarts/change-model/node/add-utterances.js?range=24-26 "Add upload file")]

Voeg de methode en het object voor de functie `addUtterance` toe.

   [!code-javascript[Add configuration information for adding utterance](~/samples-luis/documentation-samples/quickstarts/change-model/node/add-utterances.js?range=28-67 "Add configuration information for adding utterance")]

Voeg de methode en het object voor de functie `train` toe.

   [!code-javascript[Add configuration information for training LUIS](~/samples-luis/documentation-samples/quickstarts/change-model/node/add-utterances.js?range=69-101 "Add configuration information for training LUIS")]

Voeg de functie `sendUtteranceToApi` toe om HTTP-aanroepen te verzenden en ontvangen. 

   [!code-javascript[Send the HTTP request](~/samples-luis/documentation-samples/quickstarts/change-model/node/add-utterances.js?range=103-119 "Send the HTTP request")]

Voeg de **main**-code toe die bepaalt welke actie wordt gekozen.

   [!code-javascript[Main function](~/samples-luis/documentation-samples/quickstarts/change-model/node/add-utterances.js?range=121-143 "Main function")]

### <a name="install-dependencies"></a>Afhankelijkheden installeren

Maak het bestand `package.json` met de volgende tekst:

   [!code-json[Package.json](~/samples-luis/documentation-samples/quickstarts/change-model/node/package.json "Package.json")]

Installeer op de opdrachtregel vanuit de directory waar package.json zich bevindt afhankelijkheden met NPM: `npm install`.

## <a name="run-code"></a>Code uitvoeren

Voer de toepassing uit vanaf een opdrachtregel met Node.js.

Door `npm start` aan te roepen worden de utterances toegevoegd, wordt de toepassing getraind en wordt de trainingsstatus opgehaald.

```console
> npm start 
```

Deze opdrachtregel toont de resultaten van het aanroepen van de API voor het toevoegen van utterances. 

[!INCLUDE [Quickstart response from API calls](../../../includes/cognitive-services-luis-qs-change-model-json-results.md)]


## <a name="clean-up-resources"></a>Resources opschonen

Verwijder wanneer u klaar bent met de snelstart alle bestanden die in de snelstart zijn gemaakt. 

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"] 
> [Programmatisch een LUIS-app bouwen](luis-tutorial-node-import-utterances-csv.md)
