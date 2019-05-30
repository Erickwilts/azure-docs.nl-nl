---
title: Wat is de Bing Nieuws zoeken-API?
titlesuffix: Azure Cognitive Services
description: Ontdek hoe u de Bing Nieuws zoeken-API gebruikt om op internet te zoeken naar nieuws in meerdere categorieën, waaronder in kopteksten en in populaire onderwerpen.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-news-search
ms.topic: overview
ms.date: 01/10/2019
ms.author: scottwhi
ms.custom: seodec2018
ms.openlocfilehash: e02f81ef6bdf8bf4dc39b70cb7a2d9330660d1d6
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66390299"
---
# <a name="what-is-the-bing-news-search-api"></a>Wat is de Bing Nieuws zoeken-API?

Met de Bing Nieuws zoeken-API kunt u de cognitieve zoekmogelijkheden makkelijk in uw toepassingen integreren. De API biedt een soortgelijke ervaring als [Bing News](https://www.bing.com/news). U kunt dus query's doorzoeken en relevante nieuwsartikelen ontvangen.

Let erop dat met de Bing Nieuws zoeken-API de resultaten alleen zoekresultaten over nieuws bevatten. Gebruik de [Bing Webzoekopdrachten-API](../bing-web-search/search-the-web.md), [Video's zoeken-API](../bing-video-search/search-the-web.md) en [Afbeeldingen zoeken-API](../bing-image-search/overview.md) voor andere typen webinhoud.

## <a name="bing-news-search-api-features"></a>Functies van Bing Nieuws zoeken-API

Terwijl de Bing Nieuws zoeken-API hoofdzakelijk relevante nieuwsartikelen zoekt en retourneert, biedt het een aantal functies voor het intelligent en gericht ophalen van nieuws op internet.

|Functie  |Description  |
|---------|---------|
|[Zoektermen voorstellen en gebruiken](concepts/search-for-news.md#suggest-and-use-search-terms)     | Verbeter uw zoekervaring met de [Automatische suggestie-API voor Bing](../bing-autosuggest/get-suggested-search-terms.md) om voorgestelde zoektermen weer te geven wanneer deze worden getypt.         |
|[Algemeen nieuws ophalen](concepts/search-for-news.md#get-general-news)     | Zoek nieuws door een zoekopdracht te versturen naar de Bing Nieuws zoeken-API, waarna u een lijst met relevante nieuwsartikelen retour krijgt.           |
|[Belangrijkste nieuws van vandaag](concepts/search-for-news.md#get-todays-top-news)      | Haal de belangrijkste nieuwsberichten van vandaag op, in alle categorieën.       |
|[Nieuws per categorie](concepts/search-for-news.md)     | Zoek nieuws in bepaalde categorieën.        | 
|[Nieuwskoppen](concepts/search-for-news.md)     | Zoek de belangrijkste koppen, in alle categorieën.         |

## <a name="workflow"></a>Werkstroom

De Bing Nieuws zoeken-API is een RESTful-webservice die eenvoudig kan worden aangeroepen vanuit elke programmeertaal waarmee HTTP-aanvragen kunnen worden gedaan en JSON kan worden geparseerd. U kunt de service gebruiken met de REST API of de SDK.

1. Maak een account voor Cognitive Services-API met toegang tot de Bing Zoeken-API's. Als u geen Azure-abonnement hebt, kunt u [gratis een account maken](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-news-api).

2. Verzend een aanvraag naar de API met een geldige zoekquery.

3. Verwerk de API-reactie door het geretourneerde JSON-bericht te parseren.

## <a name="next-steps"></a>Volgende stappen

Probeer eerst de [interactieve demo](https://azure.microsoft.com/services/cognitive-services/bing-news-search-api/) van de Bing Nieuws zoeken-API uit. In deze demo ziet u hoe u snel een zoekquery kunt aanpassen en nieuws op internet kunt zoeken.

Als u snel aan de slag wilt met uw eerste API-aanvraag, kunt u een quickstart proberen voor de [REST API](quickstart.md) of een van de [SDK's](sdk.md).

## <a name="see-also"></a>Zie ook

* De referentiesectie [Bing Nieuws zoeken-API v7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference) bevat definities en informatie over de eindpunten, headers, API-antwoorden en queryparameters waarmee u zoekresultaten voor afbeeldingen kunt aanvragen.

* In de [Bing-vereisten voor gebruik en weergave](./useanddisplayrequirements.md) staan het acceptabele gebruik van de inhoud en informatie die is verkregen via de Bing Zoeken-API's.
