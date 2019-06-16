---
title: Azure Application Insights Telemetry Data Model - Afhankelijkheidstelemetrie | Microsoft Docs
description: Application Insights-gegevensmodel voor afhankelijkheidstelemetrie
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 04/17/2017
ms.reviewer: sergkanz
ms.author: mbullwin
ms.openlocfilehash: 3e3d6b8fdc9ac8dd28f73fecd6231e97a5645407
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60901022"
---
# <a name="dependency-telemetry-application-insights-data-model"></a>Afhankelijkheidstelemetrie: Application Insights-gegevensmodel

Afhankelijkheidstelemetrie (in [Application Insights](../../azure-monitor/app/app-insights-overview.md)) vertegenwoordigt een interactie van het bewaakte onderdeel met een externe onderdeel, zoals SQL of een HTTP-eindpunt.

## <a name="name"></a>Name

Naam van de opdracht die is gestart met deze afhankelijkheidsaanroep. De kardinaliteit van de lage waarde. Voorbeelden zijn opgeslagen procedurenaam worden opgegeven en pad van URL van de sjabloon.

## <a name="id"></a>Id

Id van een exemplaar van de aanroep van afhankelijkheid. Gebruikt voor correlatie met het verzoek telemetrie-item overeenkomt met deze afhankelijkheidsaanroep. Zie voor meer informatie, [correlatie](../../azure-monitor/app/correlation.md) pagina.

## <a name="data"></a>Gegevens

De opdracht is gestart door deze afhankelijkheidsaanroep. Voorbeelden zijn SQL-instructie en HTTP-URL met alle queryparameters.

## <a name="type"></a>Type

Naam van afhankelijkheid. De kardinaliteit van de lage waarde voor de logische groepering van de afhankelijkheden en de interpretatie van andere velden, zoals commandName en resultCode. Voorbeelden zijn SQL-, Azure table- en HTTP.

## <a name="target"></a>Doel

Doelsite van de afhankelijkheidsaanroep van een. Voorbeelden zijn de naam van de server, host-adres. Zie voor meer informatie, [correlatie](../../azure-monitor/app/correlation.md) pagina.

## <a name="duration"></a>Duration

Duur in de indeling van aanvraag: `DD.HH:MM:SS.MMMMMM`. Moet minder dan `1000` dagen.

## <a name="result-code"></a>Resultaatcode

De resultaatcode van de afhankelijkheidsaanroep van een. Voorbeelden zijn SQL-foutcode en HTTP-statuscode.

## <a name="success"></a>Geslaagd

Vermelding van geslaagde of mislukte aanroep.

## <a name="custom-properties"></a>Aangepaste eigenschappen

[!INCLUDE [application-insights-data-model-properties](../../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Aangepaste metingen

[!INCLUDE [application-insights-data-model-measurements](../../../includes/application-insights-data-model-measurements.md)]


## <a name="next-steps"></a>Volgende stappen

- Instellen voor bijhouden van afhankelijkheid [.NET](../../azure-monitor/app/asp-net-dependencies.md).
- Instellen voor bijhouden van afhankelijkheid [Java](../../azure-monitor/app/java-agent.md).
- [Schrijven van aangepaste afhankelijkheidstelemetrie](../../azure-monitor/app/api-custom-events-metrics.md#trackdependency)
- Zie [gegevensmodel](data-model.md) voor Application Insights-typen en -gegevensmodel.
- Bekijk [platforms](../../azure-monitor/app/platforms.md) ondersteund door Application Insights.
