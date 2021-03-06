---
title: Real-time conversation transcriptie Quick Start-Speech Service
titleSuffix: Azure Cognitive Services
description: Meer informatie over het gebruik van real-time conversatie-transcriptie met de spraak-SDK. Met de conversatie-transcriptie kunt u vergaderingen en andere gesp rekken transcriberen met de mogelijkheid om meerdere deel nemers toe te voegen, te verwijderen en te identificeren door audio naar de spraak service te streamen.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/20/2020
ms.author: trbye
zone_pivot_groups: acs-js-csharp
ms.openlocfilehash: ae3d7b329c55ce0be101cee73e1fc7674a2d75aa
ms.sourcegitcommit: 10d00006fec1f4b69289ce18fdd0452c3458eca5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/21/2020
ms.locfileid: "95026536"
---
# <a name="get-started-with-real-time-conversation-transcription"></a>Aan de slag met realtime gesprek transcriptie

Met de **ConversationTranscriber** -API van de Speech SDK kunt u vergaderingen en andere gesp rekken transcriberen met de mogelijkheid om meerdere deel nemers toe te voegen, te verwijderen en te identificeren door audio te streamen naar de spraak service met `PullStream` of `PushStream` . U maakt eerst spraak handtekeningen voor elke deel nemer met behulp van de REST API en vervolgens gebruikt u de spraak handtekeningen met de SDK om de conversaties te transcriberen. Zie het [overzicht](conversation-transcription.md) van het gesprek transcriptie voor meer informatie.

## <a name="limitations"></a>Beperkingen

* Alleen beschikbaar in de volgende abonnements regio's: `centralus` , `eastasia` , `eastus` , `westeurope`
* Vereist een matrix met een meerkanaals multi-microfoon met 7-Mic en een referentie stroom voor afspelen. De microfoon matrix moet voldoen aan [onze specificaties](./speech-devices-sdk-microphone.md).
* De [SDK voor spraak apparaten](speech-devices-sdk.md) biedt geschikte apparaten en een voor beeld-app die conversatie transcriptie demonstreert.

## <a name="prerequisites"></a>Vereisten

In dit artikel wordt ervan uitgegaan dat u een Azure-account en een abonnement op de Speech-service hebt. Als u geen account en abonnement hebt, [kunt u de Speech-service gratis uitproberen](overview.md#try-the-speech-service-for-free).

::: zone pivot="programming-language-javascript"
[!INCLUDE [JavaScript Basics include](includes/how-to/conversation-transcription/real-time-javascript.md)]
::: zone-end

::: zone pivot="programming-language-csharp"
[!INCLUDE [C# Basics include](includes/how-to/conversation-transcription/real-time-csharp.md)]
::: zone-end

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Asynchrone conversatie transcriptie](how-to-async-conversation-transcription.md) 
>  [Voorbeeld code](https://github.com/Azure-Samples/Cognitive-Services-Speech-Devices-SDK/blob/master/Samples/Java/Android/Speech%20Devices%20SDK%20Starter%20App/example/app/src/main/java/com/microsoft/cognitiveservices/speech/samples/sdsdkstarterapp/ConversationTranscription.java) 
>  van ROOBO-apparaat [Voorbeeld code van de Azure Kinect dev kit](https://github.com/Azure-Samples/Cognitive-Services-Speech-Devices-SDK/blob/master/Samples/Java/Windows_Linux/SampleDemo/src/com/microsoft/cognitiveservices/speech/samples/Cts.java)