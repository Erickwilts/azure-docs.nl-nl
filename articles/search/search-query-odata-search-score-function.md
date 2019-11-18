---
title: Naslag informatie over de functie OData Search. Score
titleSuffix: Azure Cognitive Search
description: Syntaxis en referentie documentatie voor het gebruik van de functie Search. Score in azure Cognitive Search query's.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
translation.priority.mt:
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pt-br
- ru-ru
- zh-cn
- zh-tw
ms.openlocfilehash: 2439d4f03184f8dbb85b229b3908dff95013b4bc
ms.sourcegitcommit: 598c5a280a002036b1a76aa6712f79d30110b98d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2019
ms.locfileid: "74113136"
---
# <a name="odata-searchscore-function-in-azure-cognitive-search"></a>OData-`search.score` functie in azure Cognitive Search

Wanneer u een query verzendt naar Azure Cognitive Search zonder de [para meter **$OrderBy** ](search-query-odata-orderby.md), worden de resultaten die terugkomen in aflopende volg orde gesorteerd op relevantie score. Zelfs wanneer u **$OrderBy**gebruikt, wordt de relevantie score standaard gebruikt om bindingen te verstoren. Soms is het echter handig om de relevantie score te gebruiken als eerste Sorteer criterium en enkele andere criteria als de afbreeker. Met de functie `search.score` kunt u dit doen.

## <a name="syntax"></a>Syntaxis

De syntaxis voor `search.score` in **$OrderBy** is `search.score()`. De functie `search.score` voert geen para meters uit. Het kan worden gebruikt met de `asc`-of `desc` sorteer volgorde aanduiding, net als bij elke andere component in de **$OrderBy** para meter. Dit kan ergens in de lijst met Sorteer criteria worden weer gegeven.

## <a name="example"></a>Voorbeeld

Sorteer hotels in aflopende volg orde op basis van `search.score` en `rating`en vervolgens in oplopende volg orde op afstand van de opgegeven coördinaten, zodat tussen twee hotels met een identieke classificatie de eerste wordt weer gegeven:

    search.score() desc,rating desc,geo.distance(location, geography'POINT(-122.131577 47.678581)') asc

## <a name="next-steps"></a>Volgende stappen  

- [Overzicht van de OData-expressie taal voor Azure Cognitive Search](query-odata-filter-orderby-syntax.md)
- [Naslag informatie voor de syntaxis van OData-expressies voor Azure Cognitive Search](search-query-odata-syntax-reference.md)
- [Zoeken in &#40;documenten Azure Cognitive Search est-API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
