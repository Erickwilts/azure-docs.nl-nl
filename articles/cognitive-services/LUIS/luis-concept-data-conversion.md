---
title: Gegevens conversie-LUIS
titleSuffix: Azure Cognitive Services
description: Meer informatie over hoe uitingen kan worden gewijzigd voordat voor spellingen in Language Understanding (LUIS)
services: cognitive-services
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 07/29/2019
ms.openlocfilehash: b305be693f59b65a62570f656a0132f4f03cf099
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "91541795"
---
# <a name="convert-data-format-of-utterances"></a>De gegevens indeling van uitingen converteren
LUIS biedt de volgende conversies van een gebruiker utterance vóór voor spelling '

* Spraak naar tekst met behulp van [Cognitive Services speech](../Speech-Service/overview.md) service.

## <a name="speech-to-text"></a>Spraak-naar-tekst

Spraak naar tekst wordt verschaft als een integratie met LUIS.

### <a name="intent-conversion-concepts"></a>Concepten van intentie conversie
Door de conversie van spraak naar tekst in LUIS kunt u gesp roken uitingen naar een eind punt verzenden en een LUIS-Voorspellings reactie ontvangen. Het proces is een integratie van de [spraak](https://docs.microsoft.com/azure/cognitive-services/Speech) service met Luis. Meer informatie over spraak naar opzet vindt u in een [zelf studie](../speech-service/how-to-recognize-intents-from-speech-csharp.md).

### <a name="key-requirements"></a>Belangrijke vereisten
U hoeft geen **Bing Speech-API** sleutel voor deze integratie te maken. Een **Language Understanding** sleutel die in de Azure Portal is gemaakt, werkt voor deze integratie. Gebruik niet de LUIS-start sleutel.

### <a name="pricing-tier"></a>Prijscategorie
Deze integratie maakt gebruik van een ander [prijs](luis-limits.md#key-limits) model dan gebruikelijk language Understanding prijs categorieën.

### <a name="quota-usage"></a>Quota gebruik
Zie de [belangrijkste limieten](luis-limits.md#key-limits) voor informatie.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Gegevens uitpakken](luis-concept-data-extraction.md)

