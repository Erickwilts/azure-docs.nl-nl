---
title: 'Quickstart: Een Custom Voice-assistent maken -Speech-service'
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: trrwilson
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 03/20/2020
ms.author: travisw
ms.openlocfilehash: bb88ef7f048b2454f05ef33286e59277070c3ea0
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2020
ms.locfileid: "80241141"
---
In deze quickstart gebruikt u de [Speech SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) om een aangepaste toepassing voor de spraakassistenten te maken die verbinding maakt met een bot die u al hebt gemaakt en geconfigureerd. Als u een bot wilt maken, raadpleegt u [de bijbehorende zelfstudie](~/articles/cognitive-services/speech-service/tutorial-voice-enable-your-bot-speech-sdk.md) voor een uitgebreidere handleiding.

Als u aan de vereisten hebt voldaan, kunt u de aangepaste spraakassistent in een paar stappen koppelen:
> [!div class="checklist"]
> * Maak een `BotFrameworkConfig`-object op basis van uw abonnementssleutel en -regio.
> * Maak een `DialogServiceConnector`-object met bovenstaande `BotFrameworkConfig`-object.
> * Gebruik het `DialogServiceConnector`-object om het luisterproces voor één uiting te starten.
> * Inspecteer de geretourneerde `ActivityReceivedEventArgs`.
