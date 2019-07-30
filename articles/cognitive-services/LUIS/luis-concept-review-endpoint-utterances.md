---
title: Gebruikers utterance bekijken-LUIS
titleSuffix: Azure Cognitive Services
description: Met actief leren, uw uitingen controle-eindpunt voor de juiste intentie en entiteit. LUIS kiest eindpunt uitingen is het niet zeker weet.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: diberry
ms.openlocfilehash: a6b89b315c4cdb1438fc8256cfc01793b3c0f920
ms.sourcegitcommit: 08d3a5827065d04a2dc62371e605d4d89cf6564f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2019
ms.locfileid: "68619745"
---
# <a name="concepts-for-enabling-active-learning-by-reviewing-endpoint-utterances"></a>Concepten voor het inschakelen van actief leren aan de hand van de eindpunt-uitingen
Actief leren is een van drie strategieën voor het verbeteren van nauwkeurigheid en het gemakkelijkst te implementeren. Met actief leren, uw uitingen controle-eindpunt voor de juiste intentie en entiteit. LUIS kiest eindpunt uitingen is het niet zeker weet.

## <a name="what-is-active-learning"></a>Wat is actief leren
Actief leren is een proces in twee stappen. Eerst selecteert LUIS uitingen die wordt ontvangen op van de app-eindpunt waarvoor validatie. De tweede stap wordt uitgevoerd door de eigenaar van de app of medewerker voor het valideren van de geselecteerde uitingen voor [bekijken](luis-how-to-review-endpoint-utterances.md), met inbegrip van de juiste intentie en alle entiteiten in het doel. Bekijk de uitingen en trainen en publiceer de app opnieuw. 

## <a name="which-utterances-are-on-the-review-list"></a>Welke uitingen zijn in de lijst met controleren
Als de bovenkant activeren kunt u lezen wat een lage score heeft of de bovenste twee intenties scores te sluiten zijn, LUIS uitingen toegevoegd aan de beoordeling-lijst. 

## <a name="single-pool-for-utterances-per-app"></a>Één groep voor uitingen per app
De **bekijken eindpunt uitingen** lijst niet op basis van de versie verandert. Er is één groep uitingen om te beoordelen, ongeacht welke versie van de uiting u actief bewerkt of welke versie van de app wordt gepubliceerd op het eindpunt. 

## <a name="where-are-the-utterances-from"></a>Waar zijn de uitingen van
Eindpunt-uitingen zijn afkomstig uit de query's door eindgebruikers op HTTP-eindpunt van de toepassing. Als uw app is niet gepubliceerd of nog niet treffers ontvangen is, hoeft u niet alle uitingen om te controleren. Als er geen eindpunt treffers worden ontvangen voor een bepaald doel of de entiteit, hoeft u niet uitingen om te controleren die ze bevatten. 

## <a name="schedule-review-periodically"></a>Controleer regelmatig plannen
Controleren van de voorgestelde uitingen hoeft niet elke dag worden uitgevoerd, maar moet deel uitmaken van uw regelmatig onderhoud van LUIS. 

## <a name="delete-review-items-programmatically"></a>Objecten bekijken via een programma verwijderen
Gebruik de niet-gelabelde uitingen-API **[verwijderen](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/58b6f32139e2bb139ce823c9)** . Maak een back-up van deze uitingen vóór de verwijdering van  **[exporteren van de logboekbestanden](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c36)** .

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het [bekijken](luis-how-to-review-endpoint-utterances.md) uitingen van eindpunt
