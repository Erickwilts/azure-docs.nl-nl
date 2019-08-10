---
title: Vooraf ontwikkelde domeinen voor Language Understanding
titleSuffix: Azure Cognitive Services
description: LUIS bevat een set met vooraf gemaakte domeinen voor het snel toevoegen van algemene, gesprekken gebruikersscenario's.
services: cognitive-services
author: diberry
ms.custom: seodec18
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: diberry
ms.openlocfilehash: e5d5de6a6f348fa3708b1e39fe2bdd3aa8693dee
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2019
ms.locfileid: "68932658"
---
# <a name="add-prebuilt-domains-for-common-usage-scenarios"></a>Vooraf gemaakte domeinen voor veelvoorkomende gebruiksscenario's toevoegen 

LUIS bevat een set vooraf gedefinieerde intents van de vooraf gemaakte domeinen voor het snel toevoegen van algemene intents en uitingen. Dit is een snelle en eenvoudige manier om mogelijkheden toevoegen aan uw eigen client-app zonder te hoeven ontwerpen van de modellen voor deze mogelijkheden. 

## <a name="add-a-prebuilt-domain"></a>Een vooraf gedefinieerd domein toevoegen

1. Op de **mijn Apps** pagina, selecteert u uw app. Hiermee opent u uw app naar de **bouwen** sectie van de app. 

1. Op de **Intents** weergeeft, schakelt **vooraf gemaakte domeinen toevoegen** links van het laagste werkbalk. 

1. Selecteer de **agenda** intentie en selecteer vervolgens **domein toevoegen** knop.

    ![Vooraf gedefinieerde agenda-domein toevoegen](./media/luis-prebuilt-domains/add-prebuilt-domain.png)

1. Selecteer **Intents** in de navigatiebalk links om de agenda intenties weer te geven. Elk doel van dit domein wordt voorafgegaan door `Calendar.`. Samen met uitingen, twee entiteiten voor dit domein zijn toegevoegd aan de app: `Calendar.Location` en `Calendar.Subject`. 

## <a name="train-and-publish"></a>Trainen en publiceren

1. Nadat het domein is toegevoegd, de app trainen door te selecteren **trainen** rechts in de bovenste werkbalk. 

1. Selecteer in de bovenste werkbalk **publiceren**. Publiceren naar **productie**. 

1. Wanneer de melding groen geslaagd wordt weergegeven, selecteert u de **verwijzen naar de lijst met eindpunten** koppeling naar de eindpunten.

1. Selecteer een eindpunt. Er wordt een nieuw browsertabblad geopend naar dit eindpunt. Houd het browsertabblad geopend en Ga door met de **Test** sectie.

## <a name="test"></a>Testen

Test de nieuwe doel op het eindpunt toegevoegd door een waarde voor de **q** parameter: `Schedule a meeting with John Smith in Seattle next week`.

LUIS retourneert de juiste intentie en onderwerp:

```json
{
  "query": "Schedule a meeting with John Smith in Seattle next week",
  "topScoringIntent": {
    "intent": "Calendar.Add",
    "score": 0.824783146
  },
  "entities": [
    {
      "entity": "a meeting with john smith",
      "type": "Calendar.Subject",
      "startIndex": 9,
      "endIndex": 33,
      "score": 0.484055847
    }
  ]
}
```

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Naslaginformatie voor vooraf gedefinieerde](./luis-reference-prebuilt-domains.md)
