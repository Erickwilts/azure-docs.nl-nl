---
title: 'Snelstart: een knowledge base maken - REST, Python - QnA Maker'
description: In deze op Python REST gebaseerde snelstart wordt stapsgewijs uitgelegd hoe u, met behulp van een programma, een voorbeeldexemplaar van een knowledge base in QnA Maker kunt maken, dat wordt weergegeven op het Azure-dashboard van uw account voor de Cognitive Services-API.
ms.date: 12/16/2019
ROBOTS: NOINDEX,NOFOLLOW
ms.custom: RESTCURL2020FEB27
ms.topic: conceptual
ms.openlocfilehash: bb51a47efc7bcae5014d5ea004674fed7cb33fe0
ms.sourcegitcommit: f5e4d0466b417fa511b942fd3bd206aeae0055bc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78851815"
---
# <a name="quickstart-create-a-knowledge-base-in-qna-maker-using-python"></a>Snelstart: een knowledge base maken in QnA Maker met behulp van Python

In deze snelstart wordt beschreven hoe u programmatisch een voorbeeld van een QnA Maker-knowledge base kunt maken en publiceren. Met QnA Maker worden automatisch vragen en antwoorden opgehaald uit semi-gestructureerde inhoud, zoals veelgestelde vragen, vanuit [gegevensbronnen](../Concepts/knowledge-base.md). Het model voor de knowledge base wordt gedefinieerd in de JSON die in de hoofdtekst van de API-aanvraag wordt verzonden.

In deze snelstart worden QnA Maker-API's aangeroepen:
* [KB maken](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/create)
* [Bewerkingsdetails ophalen](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/operations/getdetails)

[Referentie documentatie](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase) | [python](https://github.com/Azure-Samples/cognitive-services-qnamaker-python/blob/master/documentation-samples/quickstarts/create-knowledge-base/create-new-knowledge-base-3x.py) -voor beeld

[!INCLUDE [Custom subdomains notice](../../../../includes/cognitive-services-custom-subdomains-note.md)]

## <a name="prerequisites"></a>Vereisten

* [Python 3.7](https://www.python.org/downloads/)
* U moet een [QnA Maker-service ](../How-To/set-up-qnamaker-service-azure.md) hebben. Als u de sleutel en het eind punt (inclusief de resource naam) wilt ophalen, selecteert u **Quick** start voor uw resource in het Azure Portal.

## <a name="create-a-knowledge-base-python-file"></a>Een Python-bestand met knowledge base maken

Maak een bestand met de naam `create-new-knowledge-base-3x.py`.

## <a name="add-the-required-dependencies"></a>De vereiste afhankelijkheden toevoegen

Voeg aan het begin van `create-new-knowledge-base-3x.py` de volgende regels toe om de nodige afhankelijkheden aan het project toe te voegen:

[!code-python[Add the required dependencies](~/samples-qnamaker-python/documentation-samples/quickstarts/create-knowledge-base/create-new-knowledge-base-3x.py?range=1-1 "Add the required dependencies")]

## <a name="add-the-required-constants"></a>De vereiste constanten toevoegen
Voeg de vereiste constanten toe voor toegang tot QnA Maker na de bovenstaande vereiste afhankelijkheden. Vervang de waarde van de `<your-qna-maker-subscription-key>` en `<your-resource-name>` door uw eigen QnA Maker sleutel en resource naam.

Voeg boven aan de programma klasse de vereiste constanten toe om toegang te krijgen tot QnA Maker.

Stel de volgende waarden in:

* `<your-qna-maker-subscription-key>`: de **sleutel** is een teken reeks van 32 en is beschikbaar in de Azure Portal, op de QnA Maker resource, op de pagina Quick Start. Dit is niet hetzelfde als de Voorspellings eindpunt sleutel.
* `<your-resource-name>`: de **naam** van uw resource wordt gebruikt om de URL voor het ontwerpen van eind punten voor het ontwerpen te maken, in de indeling van `https://YOUR-RESOURCE-NAME.cognitiveservices.azure.com`. Dit is niet dezelfde URL die wordt gebruikt om een query uit te zoeken op het Voorspellings eindpunt.

[!code-python[Add the required constants](~/samples-qnamaker-python/documentation-samples/quickstarts/create-knowledge-base/create-new-knowledge-base-3x.py?range=5-13 "Add the required constants")]

## <a name="add-the-kb-model-definition"></a>De definitie van het KB-model toevoegen

Voeg na de constanten de volgende KB-modeldefinitie toe. Het model wordt na de definitie naar een tekenreeks geconverteerd.

[!code-python[Add the KB model definition](~/samples-qnamaker-python/documentation-samples/quickstarts/create-knowledge-base/create-new-knowledge-base-3x.py?range=15-41 "Add the KB model definition")]

## <a name="add-supporting-function"></a>Ondersteunende functie toevoegen

Voeg de volgende functie toe om JSON in een leesbare indeling weer te geven:

[!code-python[Add supporting function](~/samples-qnamaker-python/documentation-samples/quickstarts/create-knowledge-base/create-new-knowledge-base-3x.py?range=43-45 "Add supporting function")]

## <a name="add-function-to-create-kb"></a>Functie toevoegen om KB te maken

Voeg de volgende functie toe voor het maken van een HTTP POST-aanvraag om de knowledge base te maken.
Deze API-aanroep retourneert een JSON-antwoord dat de bewerkings-id in het headerveld **Locatie** bevat. Gebruik de bewerkings-id om te bepalen of de KB is gemaakt. De `Ocp-Apim-Subscription-Key` is de sleutel van de QnA Maker-service die wordt gebruikt voor verificatie.

[!code-python[Add function to create KB](~/samples-qnamaker-python/documentation-samples/quickstarts/create-knowledge-base/create-new-knowledge-base-3x.py?range=48-59 "Add function to create KB")]

Deze API-aanroep retourneert een JSON-antwoord dat de bewerkings-id bevat. Gebruik de bewerkings-id om te bepalen of de KB is gemaakt.

```JSON
{
  "operationState": "NotStarted",
  "createdTimestamp": "2018-09-26T05:19:01Z",
  "lastActionTimestamp": "2018-09-26T05:19:01Z",
  "userId": "XXX9549466094e1cb4fd063b646e1ad6",
  "operationId": "8dfb6a82-ae58-4bcb-95b7-d1239ae25681"
}
```

## <a name="add-function-to-check-creation-status"></a>Functie toevoegen om status van maken te controleren

De volgende functie controleert de status van het maken en verzendt de bewerkings-ID aan het eind van de URL-route. De aanroep van `check_status` bevindt zich binnen de belangrijkste _WHILE_-lus.

[!code-python[Add function to check creation status](~/samples-qnamaker-python/documentation-samples/quickstarts/create-knowledge-base/create-new-knowledge-base-3x.py?range=61-67 "Add function to check creation status")]

Deze API-aanroep retourneert een JSON-antwoord dat de bewerkingsstatus bevat:

```JSON
{
  "operationState": "NotStarted",
  "createdTimestamp": "2018-09-26T05:22:53Z",
  "lastActionTimestamp": "2018-09-26T05:22:53Z",
  "userId": "XXX9549466094e1cb4fd063b646e1ad6",
  "operationId": "177e12ff-5d04-4b73-b594-8575f9787963"
}
```

Herhaal de aanroep totdat deze lukt of mislukt:

```JSON
{
  "operationState": "Succeeded",
  "createdTimestamp": "2018-09-26T05:22:53Z",
  "lastActionTimestamp": "2018-09-26T05:23:08Z",
  "resourceLocation": "/knowledgebases/XXX7892b-10cf-47e2-a3ae-e40683adb714",
  "userId": "XXX9549466094e1cb4fd063b646e1ad6",
  "operationId": "177e12ff-5d04-4b73-b594-8575f9787963"
}
```

## <a name="add-main-code-block"></a>Belangrijkste codeblok toevoegen
De volgende lus doet periodiek navraag naar de bewerkingsstatus van het maken totdat de bewerking is voltooid.

[!code-python[Add main code block](~/samples-qnamaker-python/documentation-samples/quickstarts/create-knowledge-base/create-new-knowledge-base-3x.py?range=70-96 "Add main code block")]

## <a name="build-and-run-the-program"></a>Het programma bouwen en uitvoeren

Voer de volgende opdracht op een opdrachtregel in om het programma uit te voeren. Hiermee wordt de aanvraag verzonden naar de QnA Maker-API om de KB te maken en vervolgens worden elke 30 seconden de resultaten gecontroleerd. Elk antwoord wordt weergegeven in het consolevenster.

```bash
python create-new-knowledge-base-3x.py
```

Zodra de knowledge base is gemaakt, kunt u deze weergeven in de QnA Maker-portal op de pagina [Mijn knowledge bases](https://www.qnamaker.ai/Home/MyServices). Selecteer de naam van de knowledge base, bijvoorbeeld Veelgestelde vragen over QnA Maker, om deze weer te geven.

[!INCLUDE [Clean up files and KB](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Naslaginformatie over REST API voor QnA Maker (V4)](https://go.microsoft.com/fwlink/?linkid=2092179)