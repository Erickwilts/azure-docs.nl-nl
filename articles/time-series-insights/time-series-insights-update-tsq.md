---
title: Azure Time Series Insights Preview-gegevens opvragen | Microsoft Docs
description: Azure Time Series Insights Preview gegevens uitvoeren van query's.
author: ashannon7
ms.author: dpalled
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 05/06/2019
ms.custom: seodec18
ms.openlocfilehash: bbf682df2df7a8cdc9fedb36aa4244fc5c0e9488
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66244001"
---
# <a name="data-querying"></a>Query's uitvoeren op gegevens

Azure Time Series Insights Preview kunt gegevens uitvoeren van query's op gebeurtenissen en metagegevens die zijn opgeslagen in de omgeving via openbare surface API's. Deze API's worden ook gebruikt de [Verkenner van Time Series Insights Preview](./time-series-insights-update-explorer.md).

Drie primaire API-categorieën zijn beschikbaar in Time Series Insights:

* **API's omgeving**: Hiermee kunt query's van de Time Series Insights-omgeving zelf. Voorbeelden van query's zijn de lijst van de oproepende functie toegang tot heeft omgevingen en de metagegevens van de omgeving.

* **Time Series-Model-Query (TSM-Q) API's**: Kunt maken, lezen, bijwerken en verwijderen van bewerkingen voor metagegevens die zijn opgeslagen in de omgeving van de time series-model. Voorbeelden zijn exemplaren, typen en hiërarchieën.

* **Timeseries-Query (TSQ) API's**: Voor het ophalen van gegevens van statuswijzigingsgebeurtenissen kunt u deze van de provider van de gegevensbron wordt vastgelegd. Deze API's kunnen bewerkingen voor het transformeren, combineren en uitvoeren van berekeningen op time series-gegevens uitvoeren.

De [Time Series-expressie (TSX) taal](https://docs.microsoft.com/rest/api/time-series-insights/preview-tsx) is een krachtige vierde categorie. Time Series modellen wordt gebruikt om in te schakelen van de samenstelling van de geavanceerde berekening.

## <a name="azure-time-series-insights-preview-core-apis"></a>Azure Time Series Insights Preview core API 's

De volgende core die API 's worden ondersteund.

[![Time Series-Query-overzicht](media/v2-update-tsq/tsq.png)](media/v2-update-tsq/tsq.png#lightbox)

## <a name="environment-apis"></a>Omgeving-API 's

De volgende omgeving-API's zijn beschikbaar:

* [API-omgeving](https://docs.microsoft.com/rest/api/time-series-insights/preview-env#get-environments-api): Retourneert de lijst met omgevingen dat de aanroeper is geautoriseerd voor toegang tot.
* [Profiteer van omgeving beschikbaarheid API](https://docs.microsoft.com/rest/api/time-series-insights/preview-env#get-environment-availability-api): De distributie van het aantal gebeurtenissen geretourneerd via de tijdstempel van de gebeurtenis `$ts`. Deze API kunt u bepalen of er geen gebeurtenissen in het tijdstempel zijn door te retourneren van het aantal gebeurtenissen, indien aanwezig.
* [Gebeurtenisschema in het API ophalen](https://docs.microsoft.com/rest/api/time-series-insights/preview-env#get-event-schema-api): Retourneert de metagegevens van de gebeurtenis schema voor een reeks opgegeven zoekopdracht. Deze API helpt bij het ophalen van alle metagegevens en eigenschappen die beschikbaar zijn in het schema voor de opgegeven zoekopdracht-reeks.

## <a name="time-series-model-query-tsm-q-apis"></a>Time Series Model-Query (TSM-Q) API 's

De volgende keer reeks Model-Query-API's zijn beschikbaar:

* [Instellingen voor API model](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#model-settings-api): Hiermee krijgen en hierop patches toepassen op het type en de naam van het model van de omgeving.
* [API van het type](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#types-api): Hiermee kunt u CRUD voor Time Series-typen en hun bijbehorende variabelen.
* [Hiërarchieën API](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#hierarchies-api): Hiermee kunt u CRUD voor Time Series-hiërarchieën en hun bijbehorende veld paden.
* [API-exemplaren](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#instances-api): Hiermee kunt u CRUD voor Time Series-exemplaren en de bijbehorende velden bijbehorende exemplaar.

## <a name="time-series-query-tsq-apis"></a>Time Series Query (TSQ) API 's

De volgende keer reeks Query-API's zijn beschikbaar:

* [Gebeurtenissen API](https://docs.microsoft.com/rest/api/time-series-insights/preview-query#get-events-api): Query's en voor het ophalen van Time Series Insights-gegevens van gebeurtenissen kunnen als ze zijn opgenomen in Time Series Insights van de provider van de gegevensbron.

* [Ophalen van de serie API](https://docs.microsoft.com/rest/api/time-series-insights/preview-query#get-series-api): Hiermee query's en voor het ophalen van Time Series Insights-gegevens van de vastgelegde gebeurtenissen op basis van gegevens die zijn geregistreerd op de kabel. De waarden die worden geretourneerd, zijn gebaseerd op de variabelen die zijn gedefinieerd in het model of inline opgegeven.

    >[!NOTE]
    > De aggregatie-component wordt genegeerd, zelfs als deze is opgegeven in een model of inline opgegeven.

  De reeks ophalen API retourneert een Time Series-waarde voor elke variabele voor elk interval. Een tijd-waarde uit de serie is een indeling die gebruikmaakt van Time Series Insights voor JSON-uitvoer van een query. De waarden die worden geretourneerd, zijn gebaseerd op de Time Series-ID en de set van variabelen die zijn opgegeven.

* [Reeks API aggregeren](https://docs.microsoft.com/rest/api/time-series-insights/preview-query#aggregate-series-api): Hiermee query's en voor het ophalen van Time Series Insights-gegevens van de vastgelegde gebeurtenissen door steekproeven en aggregeren opgenomen gegevens.

  De totale reeks-API retourneert een Time Series-waarde voor elke variabele voor elk interval. De waarden zijn gebaseerd op de Time Series-ID en de set van variabelen die zijn opgegeven. De totale reeks API realiseert verminderen met behulp van variabelen die zijn opgeslagen in het Tijdreeksmodel of inline aggregate of voorbeeld gegevens opgegeven.

  Cumulatieve typen ondersteund: `Min`, `Max`, `Sum`, `Count`, `Average`

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [opslag- en uitgangsclaims](./time-series-insights-update-storage-ingress.md) in Azure Time Series Insights Preview.

- Lezen van de Preview van Time Series Insights [gegevensmodellering](./time-series-insights-update-tsm.md) artikel.

- Ontdek [aanbevolen procedures bij het kiezen van een Time Series-ID](./time-series-insights-update-how-to-id.md).
