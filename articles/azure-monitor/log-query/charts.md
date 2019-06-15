---
title: Het maken van grafieken en diagrammen van Logboeken-query's van Azure Monitor | Microsoft Docs
description: Beschrijving van verschillende visualisaties in Azure Monitor om uw logboekgegevens op verschillende manieren weer te geven.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/16/2018
ms.author: bwren
ms.openlocfilehash: 07d0866bd697587da170a00e8077a57035989d32
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60594001"
---
# <a name="creating-charts-and-diagrams-from-azure-monitor-log-queries"></a>Het maken van grafieken en diagrammen van Azure Monitor logboeken-query 's

> [!NOTE]
> U moet voltooien [geavanceerde aggregaties in Logboeken-query's van Azure Monitor](advanced-aggregations.md) voordat het voltooien van deze les gaat uitvoeren.

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]

Dit artikel beschrijft de verschillende visualisaties in Azure Monitor om uw logboekgegevens op verschillende manieren weer te geven.

## <a name="charting-the-results"></a>De resultaten voor grafieken
Aan de hand van het aantal computers per besturingssysteem, in het afgelopen uur gelden starten:

```Kusto
Heartbeat
| where TimeGenerated > ago(1h)
| summarize count(Computer) by OSType  
```

Standaard worden de resultaten weergegeven als een tabel:

![Tabel](media/charts/table-display.png)

Als u een beter beeld, selecteer **grafiek**, en kies de **Pie** optie voor het visualiseren van de resultaten:

![Cirkeldiagram](media/charts/charts-and-diagrams-pie.png)


## <a name="timecharts"></a>Timecharts
De gemiddelde, het 50e en 95th percentielen van de processortijd in bins van 1 uur weergeven. De query genereert meerdere reeksen en vervolgens kunt u selecteren welke reeks om weer te geven in de grafiek:

```Kusto
Perf
| where TimeGenerated > ago(1d) 
| where CounterName == "% Processor Time" 
| summarize avg(CounterValue), percentiles(CounterValue, 50, 95)  by bin(TimeGenerated, 1h)
```

Selecteer de **regel** weergaveoptie grafiek:

![Lijndiagram](media/charts/charts-and-diagrams-multiSeries.png)

### <a name="reference-line"></a>Referentielijn

Een referentielijn kunt u eenvoudig bepalen of de metrische gegevens een bepaalde drempelwaarde wordt overschreden. Een regel toevoegen aan een grafiek, breiden de gegevensset met een constante kolom:

```Kusto
Perf
| where TimeGenerated > ago(1d) 
| where CounterName == "% Processor Time" 
| summarize avg(CounterValue), percentiles(CounterValue, 50, 95)  by bin(TimeGenerated, 1h)
| extend Threshold = 20
```

![Referentielijn](media/charts/charts-and-diagrams-multiSeriesThreshold.png)

## <a name="multiple-dimensions"></a>Meerdere dimensies
Meerdere expressies in de `by` -component van `summarize` meerdere rijen in de resultaten, één voor elke combinatie van waarden maken.

```Kusto
SecurityEvent
| where TimeGenerated > ago(1d)
| summarize count() by tostring(EventID), AccountType, bin(TimeGenerated, 1h)
```

Wanneer u de resultaten als een diagram weergeven, wordt de eerste kolom van de `by` component. Het volgende voorbeeld ziet u een gestapeld kolomdiagram grafiek met de _gebeurtenis-id._ Dimensies moet van het `string` typt, dus in dit voorbeeld de _EventID_ is die wordt geconverteerd naar een tekenreeks. 

![Staafdiagram gebeurtenis-id](media/charts/charts-and-diagrams-multiDimension1.png)

U kunt schakelen tussen door de vervolgkeuzelijst met de naam van de kolom selecteren. 

![Staafdiagram AccountType](media/charts/charts-and-diagrams-multiDimension2.png)

## <a name="next-steps"></a>Volgende stappen
Zie andere lessen voor het gebruik van de [Kusto-querytaal](/azure/kusto/query/) met Azure Monitor gegevens vastleggen:

- [Bewerkingen op tekenreeksen uitvoeren](string-operations.md)
- [Datum- en tijdbewerkingen](datetime-operations.md)
- [Aggregatiefuncties](aggregations.md)
- [Geavanceerde aggregaties](advanced-aggregations.md)
- [JSON en gegevensstructuren](json-data-structures.md)
- [Geavanceerde query schrijven](advanced-query-writing.md)
- [Joins](joins.md)