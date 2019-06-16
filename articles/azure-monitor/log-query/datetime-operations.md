---
title: Werken met datum-/ tijdwaarden in Logboeken-query's van Azure Monitor | Microsoft Docs
description: Beschrijft hoe u wilt werken met de datum en tijd-gegevens in Azure Monitor logboeken-query's.
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
ms.openlocfilehash: 402511ba3c45e8bd12cb7f92ecd54f6084c8ada2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "62112354"
---
# <a name="working-with-date-time-values-in-azure-monitor-log-queries"></a>Werken met datum-/ tijdwaarden in Logboeken-query's van Azure Monitor

> [!NOTE]
> U moet voltooien [aan de slag met de Analytics-portal](get-started-portal.md) en [aan de slag met query's](get-started-queries.md) voordat het voltooien van deze les gaat uitvoeren.

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]

Dit artikel wordt beschreven hoe u werkt met de datum en tijd waarop gegevens in Azure Monitor logboeken-query's.


## <a name="date-time-basics"></a>Grondbeginselen van de datum-tijd
De Kusto-query-taal heeft twee primaire gegevenstypen die zijn gekoppeld aan de datums en tijden: datum/tijd en periode. Alle datums worden uitgedrukt in UTC. Meerdere tijdindelingen voor datum / worden ondersteund, is de ISO8601-notatie voorkeur. 

Timespans worden uitgedrukt als een decimaal getal gevolgd door een tijdeenheid:

|verkorte versie van   | tijdseenheid    |
|:---|:---|
|d           | dag          |
|h           | uur         |
|m           | Minuut       |
|s           | Tweede       |
|ms          | milliseconden  |
|wachttijden van microseconden | wachttijden van microseconden  |
|maatstreepjes        | nanoseconden   |

Datum/tijd kunnen worden gemaakt met het gebruik van een tekenreeks met de `todatetime` operator. Bijvoorbeeld: als u wilt controleren van de virtuele machine heartbeats verzonden in een specifiek tijdsbestek, gebruiken de `between` operator op die een tijdsperiode opgeven.

```Kusto
Heartbeat
| where TimeGenerated between(datetime("2018-06-30 22:46:42") .. datetime("2018-07-01 00:57:27"))
```

Een ander gebruikelijk scenario is een datum/tijd om deze te vergelijken. Bijvoorbeeld, als u wilt zien alle heartbeats in de afgelopen twee minuten, kunt u de `now` operator samen met een TimeSpan-waarde die aangeeft van twee minuten:

```Kusto
Heartbeat
| where TimeGenerated > now() - 2m
```

Een snelkoppeling is ook beschikbaar voor deze functie:
```Kusto
Heartbeat
| where TimeGenerated > now(-2m)
```

Hoewel de kortste en meest leesbare methode wordt gebruikt de `ago` operator:
```Kusto
Heartbeat
| where TimeGenerated > ago(2m)
```

Stel in plaats van de begin- en -tijd, te weten dat de begintijd en de duur. U kunt Herschrijf de query als volgt:

```Kusto
let startDatetime = todatetime("2018-06-30 20:12:42.9");
let duration = totimespan(25m);
Heartbeat
| where TimeGenerated between(startDatetime .. (startDatetime+duration) )
| extend timeFromStart = TimeGenerated - startDatetime
```

## <a name="converting-time-units"></a>Tijdseenheden converteren
U kunt een datum/tijd of de periode in een tijdeenheid dan de standaard express. Bijvoorbeeld, als u een berekende kolom hebt en foutgebeurtenissen van de laatste 30 minuten beoordeelt hoe lang geleden waarin de gebeurtenis heeft plaatsgevonden:

```Kusto
Event
| where TimeGenerated > ago(30m)
| where EventLevelName == "Error"
| extend timeAgo = now() - TimeGenerated 
```

De `timeAgo` kolom bevat waarden, zoals: "00:09:31.5118992', wat betekent dat ze zijn geformatteerd als hh:mm:ss.fffffff. Als u wilt formatteren van deze waarden naar de `numver` van minuten sinds de begintijd, deelt u die waarde door 'minuut':

```Kusto
Event
| where TimeGenerated > ago(30m)
| where EventLevelName == "Error"
| extend timeAgo = now() - TimeGenerated
| extend timeAgoMinutes = timeAgo/1m 
```


## <a name="aggregations-and-bucketing-by-time-intervals"></a>Aggregaties en bucketing door tijdsintervallen
Een ander gebruikelijk scenario is de noodzaak om statistische gegevens gedurende een bepaalde periode in een bepaald tijdsinterval. Voor dit scenario een `bin` operator kan worden gebruikt als onderdeel van een component samenvatten.

Gebruik de volgende query uit om het aantal gebeurtenissen die hebben plaatsgevonden om de 5 minuten gedurende de afgelopen half uur:

```Kusto
Event
| where TimeGenerated > ago(30m)
| summarize events_count=count() by bin(TimeGenerated, 5m) 
```

Deze query levert de volgende tabel:  

|TimeGenerated(UTC)|events_count|
|--|--|
|2018-08-01T09:30:00.000|54|
|2018-08-01T09:35:00.000|41|
|2018-08-01T09:40:00.000|42|
|2018-08-01T09:45:00.000|41|
|2018-08-01T09:50:00.000|41|
|2018-08-01T09:55:00.000|16|

Een andere manier om te maken van de buckets van resultaten, is met functies, zoals `startofday`:

```Kusto
Event
| where TimeGenerated > ago(4d)
| summarize events_count=count() by startofday(TimeGenerated) 
```

Deze query geeft de volgende resultaten:

|timestamp|count_|
|--|--|
|2018-07-28T00:00:00.000|7,136|
|2018-07-29T00:00:00.000|12,315|
|2018-07-30T00:00:00.000|16,847|
|2018-07-31T00:00:00.000|12,616|
|2018-08-01T00:00:00.000|5,416|


## <a name="time-zones"></a>Tijdzones
Omdat alle datum-/ tijdwaarden worden uitgedrukt in UTC, is het vaak nuttig om te converteren van deze waarden naar de lokale tijdzone. Deze berekening bijvoorbeeld gebruiken om te converteren van UTC naar PST tijden:

```Kusto
Event
| extend localTimestamp = TimeGenerated - 8h
```

## <a name="related-functions"></a>Verwante functies

| Category | Function |
|:---|:---|
| Gegevenstypen converteren | [todatetime](/azure/kusto/query/todatetimefunction)  [totimespan](/azure/kusto/query/totimespanfunction)  |
| Ronde waarde die u wilt de grootte van opslaglocatie | [bin](/azure/kusto/query/binfunction) |
| Ophalen van een bepaalde datum of tijd | [geleden](/azure/kusto/query/agofunction) [nu](/azure/kusto/query/nowfunction)   |
| Onderdeel van de waarde ophalen | [datetime_part](/azure/kusto/query/datetime-partfunction) [getmonth](/azure/kusto/query/getmonthfunction) [monthofyear](/azure/kusto/query/monthofyearfunction) [getyear](/azure/kusto/query/getyearfunction) [dayofmonth](/azure/kusto/query/dayofmonthfunction) [dayofweek](/azure/kusto/query/dayofweekfunction) [dayofyear](/azure/kusto/query/dayofyearfunction) [weekofyear](/azure/kusto/query/weekofyearfunction) |
| Haal de waarde van een relatieve datum  | [endofday](/azure/kusto/query/endofdayfunction) [endofweek](/azure/kusto/query/endofweekfunction) [endofmonth](/azure/kusto/query/endofmonthfunction) [endofyear](/azure/kusto/query/endofyearfunction) [startofday](/azure/kusto/query/startofdayfunction) [beginvanweek](/azure/kusto/query/startofweekfunction) [startofmonth](/azure/kusto/query/startofmonthfunction) [startofyear](/azure/kusto/query/startofyearfunction) |

## <a name="next-steps"></a>Volgende stappen
Zie andere lessen voor het gebruik van de [Kusto-querytaal](/azure/kusto/query/) met Azure Monitor gegevens vastleggen:

- [Bewerkingen op tekenreeksen uitvoeren](string-operations.md)
- [Aggregatiefuncties](aggregations.md)
- [Geavanceerde aggregaties](advanced-aggregations.md)
- [JSON en gegevensstructuren](json-data-structures.md)
- [Geavanceerde query schrijven](advanced-query-writing.md)
- [Joins](joins.md)
- [Grafieken](charts.md)