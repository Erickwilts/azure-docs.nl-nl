---
title: Taalondersteuning - Bing nieuws zoeken-API
titleSuffix: Azure Cognitive Services
description: Een lijst van natuurlijke talen, landen en regio's die worden ondersteund door de Bing nieuws zoeken-API.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-news-search
ms.topic: conceptual
ms.date: 1/10/2019
ms.author: aahi
ms.openlocfilehash: d15058126f43fff328acfc563ffd081164a69a90
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66384974"
---
# <a name="language-and-region-support-for-the-bing-news-search-api"></a>Ondersteuning voor taal en regio voor de Bing nieuws zoeken-API

De Bing nieuws zoeken-API biedt ondersteuning voor verschillende landen/regio's, veel met meer dan één taal. Een land/regio met een query op te geven fungeert voornamelijk om zoekresultaten te verfijnen op basis van de interesses in dat land/regio. Bovendien worden de resultaten kunnen koppelingen naar Bing bevatten en deze koppelingen kunnen lokaliseren van de gebruikerservaring van Bing op basis van het opgegeven land/regio of taal.

U kunt geeft u een land/regio met de `cc` queryparameter. Als u een land/regio opgeeft, moet u ook opgeven een of meer taalcodes die met behulp van de `Accept-Language` HTTP-header. De ondersteunde talen zijn afhankelijk van countr/de regio; voor elk land/regio in de tabel markten wordt verstrekt.

U kunt ook opgeven de markt met behulp van de `mkt` query parameter en een code van de **markten** tabel. Tegelijkertijd een markt op te geven, geeft een land/regio en taal van voorkeur. De `setLang` queryparameter kan worden ingesteld op een taalcode die in dit geval; dit is doorgaans de dezelfde taal die is opgegeven door `mkt` , tenzij de gebruiker geeft de voorkeur aan om te zien van Bing in een andere taal.

## <a name="supported-markets-for-news-search-endpoint"></a>Ondersteunde markten voor nieuws zoeken-eindpunten

Voor de `/news/search` eindpunt, de volgende tabel bevat de markt code-waarden die u gebruiken kunt om op te geven de `mkt` queryparameter. Bing retourneert de inhoud voor alleen deze markten. De lijst is onderhevig aan wijzigingen.  

Voor een lijst van land/regio codes die u kunt opgeven de `cc` queryparameter, Zie [landcodes](#countrycodes).  

|Land/regio|Taal|Code van de markt|  
|---------------------|--------------|-----------------|
|Denemarken|Deens|da-DK|
|Oostenrijk|Duits|de-AT|
|Zwitserland|Duits|de CH|
|Duitsland|Duits|de-DE|
|Australië|Nederlands|en-AU|
|Canada|Nederlands|NL-CA|
|Verenigd Koninkrijk|Nederlands|en-GB|
|Indonesië|Nederlands|NL-ID|
|Ierland|Nederlands|NL-Internet Explorer|
|India|Nederlands|NL-IN|
|Maleisië|Nederlands|en Mijn|
|Nieuw-Zeeland|Nederlands|NL-NZ|
|Republiek der Filippijnen|Nederlands|NL-PH|
|Singapore|Nederlands|en-SG|
|Verenigde Staten|Nederlands|en-US|
|Nederlands|Algemene|en-WW|
|Nederlands|Algemene|en-XA|
|Zuid-Afrika|Nederlands|en-ZA|
|Argentinië|Spaans|es-AR|
|Chili|Spaans|es-CL|
|Spanje|Spaans|es-ES|
|Mexico|Spaans|es-MX|
|Verenigde Staten|Spaans|es-US|
|Spaans|Algemene|ES-Excel|
|Finland|Fins|fi-FI|  
|Frankrijk|Frans|fr-BE|
|Canada|Frans|fr-CA|
|België|Nederlands|nl-BE|
|Zwitserland|Frans|FR-h|
|Frankrijk|Frans|fr-FR|  
|Italië|Italiaans|IT-IT|
|Hongkong SAR|Traditioneel Chinees|zh-HK|  
|Taiwan|Traditioneel Chinees|zh-TW|
|Japan|Japans|ja-JP|  
|Korea|Koreaans|ko-KR|  
|Nederland|Nederlands|NL-NL|  
|Volksrepubliek China|Chinees|zh-CN|  
|Brazilië|Portugees|pt-BR|
|Rusland|Russisch|ru-RU|  
|Zweden|Zweeds|SV-SE|  
|Turkije|Turks|tr-TR|  

## <a name="supported-markets-for-news-endpoint"></a>Ondersteunde markten voor nieuws-eindpunt
Voor de `/news` eindpunt, de volgende tabel bevat de markt code-waarden die u gebruiken kunt om op te geven de `mkt` queryparameter. Bing retourneert de inhoud voor alleen deze markten. De lijst is onderhevig aan wijzigingen.  

Voor een lijst van land/regio codes die u kunt opgeven de `cc` queryparameter, Zie [landcodes](#countrycodes).  

|Land/regio|Taal|Code van de markt|  
|---------------------|--------------|-----------------|
|Denemarken|Deens|da-DK|
|Duitsland|Duits|de-DE|
|Australië|Nederlands|en-AU|
|Verenigd Koninkrijk|Nederlands|en-GB|
|Verenigde Staten|Nederlands|en-US|
|Nederlands|Algemene|en-WW|
|Chili|Spaans|es-CL|
|Mexico|Spaans|es-MX|
|Verenigde Staten|Spaans|es-US|
|Finland|Fins|fi-FI|  
|Canada|Frans|fr-CA|
|Frankrijk|Frans|fr-FR|  
|Italië|Italiaans|IT-IT|
|Brazilië|Portugees|pt-BR|
|Volksrepubliek China|Chinees|zh-CN|

## <a name="supported-markets-for-news-trending-endpoint"></a>Ondersteunde markten voor trending nieuws-eindpunt
Voor de `/news/trendingtopics` eindpunt, de volgende tabel bevat de markt code-waarden die u gebruiken kunt om op te geven de `mkt` queryparameter. Bing retourneert de inhoud voor alleen deze markten. De lijst is onderhevig aan wijzigingen.  

Voor een lijst van land/regio codes die u kunt opgeven de `cc` queryparameter, Zie [landcodes](#countrycodes).  

|Land/regio|Taal|Code van de markt|  
|---------------------|--------------|-----------------|
|Duitsland|Duits|de-DE|
|Australië|Nederlands|en-AU|
|Verenigd Koninkrijk|Nederlands|en-GB|
|Verenigde Staten|Nederlands|en-US|
|Canada|Nederlands|NL-CA|
|India|Nederlands|NL-IN|
|Frankrijk|Frans|fr-FR|
|Canada|Frans|fr-CA|
|Brazilië|Portugees|pt-BR|
|Volksrepubliek China|Chinees|zh-CN|


<a name="countrycodes"></a>   
### <a name="country-codes"></a>Landcodes  

Hieronder vindt u het land/regio-codes die u kunt opgeven in de `cc` queryparameter. De lijst is onderhevig aan wijzigingen.  

|Land/regio|Landnummer|  
|---------------------|------------------|  
|Argentinië|AR|  
|Australië|AU|  
|Oostenrijk|AT|  
|België|BE|  
|Brazilië|BR|  
|Canada|CA|  
|Chili|CL|  
|Denemarken|DK|  
|Finland|FI|  
|Frankrijk|FR|  
|Duitsland|DE|  
|Hongkong SAR|HK|  
|India|IN|  
|Indonesië|Id|  
|Italië|IT|  
|Japan|JP|  
|Korea|KR|  
|Maleisië|MY|  
|Mexico|MX|  
|Nederland|NL|  
|Nieuw-Zeeland|NZ|  
|Noorwegen|NO|  
|Volksrepubliek China|CN|  
|Polen|PL|  
|Portugal|PT|  
|Republiek der Filippijnen|PH|  
|Rusland|RU|  
|Saoedi-Arabië|SA|  
|Zuid-Afrika|ZA|  
|Spanje|ES|  
|Zweden|SE|  
|Zwitserland|CH|  
|Taiwan|TW|  
|Turkije|TR|  
|Verenigd Koninkrijk|GB|  
|Verenigde Staten|VS|

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het zoeken in Bing nieuws-eindpunten [nieuws zoeken-API voor Bing versie 7 verwijzing](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference).
