---
title: Dynamische Dictionary - Translator Text-API
titlesuffix: Azure Cognitive Services
description: Het gebruik van de functie dynamische woordenlijst van de Translator Text-API.
services: cognitive-services
author: rajdeep-in
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: v-pawal
ms.openlocfilehash: d3a77ee9273e9f587b6157bb6ed2294865b1fca5
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66514263"
---
# <a name="how-to-use-a-dynamic-dictionary"></a>Het gebruik van een dynamische woordenlijst

Als u de vertaling die u wilt toepassen op een woord of woordgroep al kent, kunt u deze opgeven als aantekeningen in de aanvraag. Alleen is de dynamische-woordenlijst voor samenstellingen, zoals de namen van de juiste en product veilig.

**Syntaxis:**

< mstrans:dictionary vertaling = "vertalingen van woorden" > woordgroep < / mstrans:dictionary >

**Voorbeeld: nl-nl:**

Invoer van de bron: Het woord < mstrans:dictionary vertaling =\"wordomatic\"> woord of woordgroep < / mstrans:dictionary > is een dictionary-vermelding.

Doeluitvoer: DAS Wort "wordomatic" is een Wörterbucheintrag.

Deze functie werkt op dezelfde manier met en zonder HTML-modus.

De functie moet spaarzaam worden gebruikt. De juiste en veel beter manier voor het aanpassen van de vertaling is met behulp van aangepaste Translator. Aangepaste Translator maakt volledig gebruik van context en statistische kansen. Als u hebt of trainingsgegevens waarin uw werk- of woordgroep in context kunt maken, kunt u veel betere resultaten krijgt. U vindt meer informatie over aangepaste Translator op [ https://aka.ms/CustomTranslator ](https://aka.ms/CustomTranslator).
