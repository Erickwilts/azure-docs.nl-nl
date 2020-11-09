---
title: Wat is Bing Automatische suggesties?
titleSuffix: Azure Cognitive Services
description: De Bing Automatische suggesties-API retourneert een lijst met voorgestelde query's op basis van de gedeeltelijke queryreeks in het zoekvak.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-autosuggest
ms.topic: overview
ms.date: 12/18/2019
ms.author: scottwhi
ms.openlocfilehash: d55ff4ed8717a449628e94a940c0896d31818da2
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2020
ms.locfileid: "93101932"
---
# <a name="what-is-bing-autosuggest"></a>Wat is Bing Automatische suggesties?

> [!WARNING]
> Bing Search-API's worden van Cognitive Services naar Bing Search Services overgezet. Vanaf **30 oktober 2020** moeten nieuwe instanties van Bing Search worden ingericht overeenkomstig het proces dat [hier](https://aka.ms/cogsvcs/bingmove) is beschreven.
> Bing Search-API's die zijn ingericht met Cognitive Services, worden voor de komende drie jaar of tot het einde van uw Enterprise Agreement ondersteund, afhankelijk van wat het eerst afloopt.
> Raadpleeg [Bing Search Services](https://aka.ms/cogsvcs/bingmigration) voor migratie-instructies.

Als uw toepassing query's verstuurt naar een van de API's van Bing Search, kunt u de Bing Automatische suggesties-API gebruiken om de ervaring van uw gebruikers te verbeteren. De Bing Automatische suggesties-API retourneert een lijst met voorgestelde query's op basis van de gedeeltelijke queryreeks in het zoekvak. Wanneer de tekens worden ingevoerd in het zoekvak, kunt u suggesties weergeven in een vervolgkeuzelijst.

## <a name="bing-autosuggest-api-features"></a>Functies van de Bing Automatische suggesties-API

| Functie                                                                                                                                                                                 | Beschrijving                                                                                                                                                            |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Zoektermen in realtime voorstellen](concepts/get-suggestions.md) | Verbeter uw app-ervaring met de Automatische suggesties-API om voorgestelde zoektermen weer te geven wanneer deze worden getypt. |

## <a name="workflow"></a>Werkstroom

De Bing Automatische suggesties-API is een RESTful-webservice die eenvoudig kan worden aangeroepen vanuit elke programmeertaal waarmee HTTP-aanvragen kunnen worden gedaan en JSON kan worden geparseerd.

1. Maak een [Account voor Cognitive Services-API](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) met toegang tot de Bing Zoeken-API's. Als u geen Azure-abonnement hebt, kunt u gratis [een account maken](https://azure.microsoft.com/free/cognitive-services/).
2. Telkens wanneer een gebruiker een nieuw teken in het zoekvak van uw toepassing typt, wordt deze API aangeroepen.
3. Verwerk de API-reactie door het geretourneerde JSON-bericht te parseren.

Normaal gesproken wordt deze API telkens wanneer een gebruiker een nieuw teken in het zoekvak van uw toepassing typt, aangeroepen. Als er meer tekens worden ingevoerd, retourneert de API steeds relevantere voorgestelde zoekquery's. Zo zijn de suggesties die de API mogelijk retourneert voor een `s` waarschijnlijk minder relevant dan de query's die worden geretourneerd voor `sail`.

In het volgende voorbeeld ziet u een zoekvak met vervolgkeuzelijst dat voorgestelde querytermen uit de Bing Automatische suggesties-API bevat.

![Vervolgkeuzelijst voor zoekvak met automatische suggesties](./media/cognitive-services-bing-autosuggest-api/bing-autosuggest-drop-down-list.PNG)

Wanneer een gebruiker een suggestie uit de vervolgkeuzelijst selecteert, kunt u deze gebruiken om te beginnen met zoeken met een van de Bing Search-API's, of gaat u rechtstreeks naar de pagina met zoekresultaten van Bing.

## <a name="next-steps"></a>Volgende stappen

Zie [Uw eerste query maken](quickstarts/csharp.md) om snel aan de slag te gaan met uw eerste aanvraag.

Lees de [naslaghandleiding over de Automatische suggestie-API voor Bing v7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-autosuggest-api-v7-reference) om goed voorbereid aan de slag te gaan. De handleiding bevat de lijst met eindpunten, headers en queryparameters die u nodig hebt om voorgestelde querytermen op te vragen, en de definities van de antwoordobjecten.

Bezoek de [hubpagina voor de Bing Search-API](../bing-web-search/search-the-web.md) om de andere beschikbare API's te verkennen.


Leer hoe u kunt zoeken op internet met behulp van de [Bing Web Search-API](../bing-web-search/search-the-web.md) en maak kennis met de overige [Bing Search-API's](../bing-web-search/index.yml).

Lees [Gebruiks- en weergavevereisten voor Bing](./useanddisplayrequirements.md) om er zeker van te zijn dat u alle regels voor het gebruik van de zoekresultaten volgt.
