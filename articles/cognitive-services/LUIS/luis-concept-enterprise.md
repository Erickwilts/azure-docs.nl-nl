---
title: Zakelijke concepten-LUIS
titleSuffix: Azure Cognitive Services
description: Ontwerpconcepten voor grote LUIS-apps of meerdere apps, met inbegrip van LUIS en QnA Maker samen begrijpen.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 01/09/2019
ms.author: diberry
ms.openlocfilehash: 0d51778473dc033bce3c58b1572f1e514a8b6327
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68560766"
---
# <a name="enterprise-strategies-for-a-luis-app"></a>Enterprise-strategieën voor een LUIS-app
Bekijk deze ontwerpstrategieën voor uw enterprise-app.

## <a name="when-you-expect-luis-requests-beyond-the-quota"></a>Wanneer u verwacht dat het quotum LUIS aanvragen
Als uw aanvraagsnelheid van LUIS-app het toegestane aantal overschrijdt [quotum tarief](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/), de belasting op meer LUIS-apps met verdelen de [dezelfde definitie van de app](#use-multiple-apps-with-same-app-definition) of maak en [meerdere sleutels toewijzen](#assign-multiple-luis-keys-to-same-app) naar de -App. 

### <a name="use-multiple-apps-with-same-app-definition"></a>Meerdere apps met dezelfde definitie van de app gebruiken
De oorspronkelijke LUIS-app exporteren en importeren van de app terug in afzonderlijke apps. Elke app heeft een eigen app-ID. Wanneer u publiceert, in plaats van met dezelfde sleutel in alle apps, maakt u een aparte sleutel voor elke app. Verdelen in alle apps zodanig dat geen enkele app wordt overbelast. Voeg [Application Insights](luis-tutorial-bot-csharp-appinsights.md) om het gebruik bewaken. 

Om toegang te krijgen van de dezelfde bovenste intentie tussen alle apps, zorg ervoor dat de intentie voorspelling tussen het eerste en tweede doel is groot genoeg dat LUIS niet worden verward is, die verschillende resultaten oplevert tussen apps voor kleine variaties in uitingen. 

Een enkele app aanwijzen als de master. Alle uitingen die worden voorgesteld voor controle moeten worden toegevoegd aan de hoofd-app vervolgens verplaatst naar alle andere apps. Dit is een volledige export van de app, of het laden van de gelabelde uitingen van het model aan de onderliggende objecten. Laden kan worden gedaan vanuit de [LUIS](luis-reference-regions.md) website of de API ontwerpen voor een [enkel utterance](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c08) of voor een [batch](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c09). 

Plan een periodieke [uitingen eindpunt voor een nadere](luis-how-to-review-endpoint-utterances.md) voor actief leren, zoals elke twee weken, klikt u vervolgens opnieuw trainen en opnieuw publiceren. 

### <a name="assign-multiple-luis-keys-to-same-app"></a>Meerdere LUIS sleutels aan dezelfde app toewijzen
Als uw LUIS-app meer eindpunt treffers dan is toegestaan door uw één sleutel quotum ontvangt, maken en toewijzen van meer sleutels aan de LUIS-app. Maken van een traffic manager of load balancer voor het beheren van de eindpunt-query's in de eindpunt-sleutels. 

## <a name="when-your-monolithic-app-returns-wrong-intent"></a>Wanneer uw monolithische app verkeerde bedoelingen retourneert
Als uw app is bedoeld om te voorspellen van een groot aantal uitingen van de gebruiker, Overweeg de implementatie van de [verzending model](#dispatch-tool-and-model). Een monolithische app worden opgesplitst kunt LUIS focus detectie tussen intents is in plaats van de tussen intents via de app bovenliggende en onderliggende apps maar verwarrend. 

Plan een periodieke [uitingen eindpunt voor een nadere](luis-how-to-review-endpoint-utterances.md) voor actief leren, zoals elke twee weken, klikt u vervolgens opnieuw trainen en opnieuw publiceren. 

## <a name="when-you-need-to-have-more-than-500-intents"></a>Als u wilt hebben van meer dan 500 intents
Stel dat u een office-assistent met meer dan 500 intents ontwikkelt. Als 200 intents verwant zijn aan de vergaderingen plannen, 200 herinneringen zijn, 200 zijn over het ophalen van informatie over collega's, en 200 zijn voor het verzenden van e-mailbericht, groep intents zodat elke groep is in een enkele app vervolgens maakt u een op het hoogste niveau app die elk doel. Gebruik de [verzending hulpprogramma en architectuur](#dispatch-tool-and-model) om de app op het hoogste niveau te maken. Wijzig vervolgens uw bot om de trapsgewijze aanroep te gebruiken zoals weer geven in de [Verzend zelf studie][dispatcher-application-tutorial]. 

## <a name="when-you-need-to-combine-several-luis-and-qna-maker-apps"></a>Wanneer u moet verschillende LUIS en QnA maker toepassingen combineren
Hebt u verschillende LUIS en QnA maker-apps die u nodig hebt om te reageren op een bot, gebruik de [verzending hulpprogramma](#dispatch-tool-and-model) om de app op het hoogste niveau te maken. Wijzig vervolgens uw bot om de trapsgewijze aanroep te gebruiken zoals weer geven in de [Verzend zelf studie][dispatcher-application-tutorial]. 

## <a name="dispatch-tool-and-model"></a>Hulpprogramma voor verzending en het model
Gebruik het opdracht regel programma voor [verzen ding][dispatch-tool] , gevonden in [BotBuilder-hulpprogram MA'S](https://github.com/Microsoft/botbuilder-tools) om meerdere LUIS en/of QnA Maker apps te combi NEREN in een bovenliggende Luis-app. Deze aanpak kunt u een bovenliggende domein met inbegrip van alle onderwerpen en verschillende onderliggende onderwerpsdomeinen in afzonderlijke apps. 

![Conceptuele afbeelding van verzending van de architectuur](./media/luis-concept-enterprise/dispatch-architecture.png)

Het bovenliggende domein wordt vermeld in LUIS met een versie met de naam `Dispatch` in de lijst met apps. 

De chatbot ontvangt de utterance en vervolgens verzendt naar de bovenliggende LUIS-app voor voorspelling. De bovenste voorspelde intentie van de bovenliggende app bepaalt welke onderliggende LUIS-app wordt vervolgens genoemd. De chatbot verzendt de utterance naar de onderliggende-app voor een meer specifieke voorspelling.

Meer informatie over hoe deze hiërarchie van aanroepen wordt gedaan vanuit de Bot Builder v4 [-verzender-toepassing-zelf studie][dispatcher-application-tutorial].  

### <a name="intent-limits-in-dispatch-model"></a>Intentie limieten in model voor verzending
Een toepassing verzending heeft 500 verzending bronnen, gelijk aan 500 intents als het maximum. 

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het [testen van een batch](luis-how-to-batch-test.md)

[dispatcher-application-tutorial]: https://aka.ms/bot-dispatch
[dispatch-tool]: https://aka.ms/dispatch-tool
