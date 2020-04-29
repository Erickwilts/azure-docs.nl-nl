---
title: Taal ondersteuning-Bing Visual Search-API
titleSuffix: Azure Cognitive Services
description: Een lijst met natuurlijke talen, landen en regio's die worden ondersteund door de Bing Visual Search-API. Het Bing Visual Search-API ondersteunt meer dan drie dozijn landen/regio's, veel met meer dan één taal.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: conceptual
ms.date: 09/25/2018
ms.author: scottwhi
ms.openlocfilehash: b17341bc234ff3dfecc2c6dcd84ef77116a95d61
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/29/2020
ms.locfileid: "68883554"
---
# <a name="language-and-region-support-for-the-bing-visual-search-api"></a>Ondersteuning van talen en regio's voor de Bing Visual Search-API

Bing Visual Search-API ondersteunt meer dan drie dozijn landen/regio's, veel met meer dan één taal. Elke aanvraag moet het land/de regio en de taal van de gebruiker bevatten. Als u de markt van de gebruiker weet, kan Bing de juiste resultaten retour neren. Als u geen land/regio en taal opgeeft, maakt Bing een beste poging om het land/de regio en taal van de gebruiker te bepalen. Omdat de resultaten mogelijk koppelingen naar Bing bevatten, is het mogelijk dat het land/de regio en de taal een voor keur hebben van een gelokaliseerde Bing gebruikers ervaring als de gebruiker op de Bing-koppelingen klikt.

Als u het land/de regio en taal wilt opgeven `mkt` , stelt u de query parameter (Market) in op een code uit de tabel **markten** hieronder. Op de markt wordt een land/regio en taal opgegeven. Als de gebruiker de weer gave van tekst in een andere taal wilt zien, `setLang` stelt u de query parameter in op de juiste taal code.

U kunt ook het land/de regio opgeven met behulp `cc` van de query-para meter. Als u een land/regio opgeeft, moet u ook een of meer taal codes opgeven met de `Accept-Language` http-header. De ondersteunde talen variëren per land/regio. ze worden voor elk land in de tabel markten gegeven.



> [!NOTE]
> De volgende beperkingen gelden voor de markt:
>
> - Aantekeningen bij afbeeldings herkenning zijn alleen beschikbaar in het Engels.
> - Recepten, winkelen en pagina's, met inbegrip van inzichten, zijn alleen beschikbaar in de en VS-markt.


## <a name="countriesregions"></a>Landen/regio's

|Land/regio|Code|
|-------|----|
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
|China|CN|
|Polen|PL|
|Portugal|PT|
|Filipijnen|PH|
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


## <a name="markets"></a>Landen

|Land/regio|Taal|Markt code|
|-------|--------|-----------|
|Argentinië|Spaans|es-AR|
|Australië|Engels|en-AU|
|Oostenrijk|Duits|de-AT|
|België|Nederlands|nl-worden|
|België|Frans|fr-worden|
|Brazilië|Portugees|pt-BR|
|Canada|Engels|en-CA|
|Canada|Frans|FR-CA|
|Chili|Spaans|es-LC|
|Denemarken|Deens|da-DK|
|Finland|Fins|fi-FI|
|Frankrijk|Frans|fr-FR|
|Duitsland|Duits|de-DE|
|Hongkong SAR|Traditioneel Chinees|zh-HK|
|India|Engels|en-IN|
|Indonesië|Engels|en-ID|
|Italië|Italiaans|it-IT|
|Japan|Japans|ja-JP|
|Korea|Koreaans|ko-KR|
|Maleisië|Engels|en-mijn|
|Mexico|Spaans|es-MX|
|Nederland|Nederlands|nl-NL|
|Nieuw-Zeeland|Engels|en-NZ|
|China|Chinees|zh-CN|
|Polen|Pools|pl-PL|
|Portugal|Portugees|pt-PT|
|Filipijnen|Engels|en-PH|
|Rusland|Russisch|ru-RU|
|Saoedi-Arabië|Arabisch|ar-SA|
|Zuid-Afrika|Engels|en-ZA|
|Spanje|Spaans|es-ES|
|Zweden|Zweeds|sv-SE|
|Zwitserland|Frans|FR-CH|
|Zwitserland|Duits|Dech|
|Taiwan|Traditioneel Chinees|zh-TW|
|Turkije|Turks|tr-TR|
|Verenigd Koninkrijk|Engels|en-GB|
|Verenigde Staten|Engels|nl-NL|
|Verenigde Staten|Spaans|es-Verenigde Staten|
