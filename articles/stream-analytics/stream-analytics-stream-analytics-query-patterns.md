---
title: Veelvoorkomende querypatronen in Azure Stream Analytics
description: Dit artikel beschrijft een aantal veelvoorkomende querypatronen en modellen die nuttig in Azure Stream Analytics-taken zijn.
services: stream-analytics
author: jseb225
ms.author: jeanb
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/16/2019
ms.openlocfilehash: f6971038be7404850d958de67eb4755ae7d21a29
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65761976"
---
# <a name="query-examples-for-common-stream-analytics-usage-patterns"></a>Voorbeelden van algemene patronen voor het gebruik van Stream Analytics query

Query's in Azure Stream Analytics worden uitgedrukt in een SQL-achtige querytaal. De taalconstructies worden beschreven in de [Stream Analytics query language reference](/stream-analytics-query/stream-analytics-query-language-reference) handleiding. 

Ontwerp van de query kan eenvoudige Pass Through-logica voor het verplaatsen van gebeurtenisgegevens uit één invoerstroom in een gegevensarchief uitvoer express, of deze uitgebreide patroon overeenkomende en tijdelijke analyse voor het berekenen van statistische functies via verschillende tijdvensters in kunt doen de [IoT bouwen oplossing met behulp van Stream Analytics](stream-analytics-build-an-iot-solution-using-stream-analytics.md) handleiding. U kunt deelnemen aan gegevens van meerdere invoergegevens het combineren van streaming-gebeurtenissen en u kunt dit doen voor lookups op basis van statische referentiegegevens te verrijken van de waarden van de gebeurtenis. U kunt ook gegevens schrijven naar meerdere uitvoer.

In dit artikel bevat een overzicht van oplossingen voor enkele veelvoorkomende querypatronen op basis van echte scenario's.

## <a name="work-with-complex-data-types-in-json-and-avro"></a>Werken met complexe gegevenstypen in JSON en Avro

Azure Stream Analytics ondersteunt verwerking van gebeurtenissen in de opmaak van CSV, JSON en Avro-gegevens.

JSON- en Avro mag complexe typen zoals geneste objecten (records) of matrices zijn. Raadpleeg voor meer informatie over het werken met deze complexe gegevenstypen de [gegevens parseren van JSON en AVRO](stream-analytics-parsing-json.md) artikel.

## <a name="query-example-convert-data-types"></a>Voorbeeld: Gegevenstypen converteren

**Beschrijving**: Definieert de typen eigenschappen voor de invoerstroom. Bijvoorbeeld: het gewicht auto is afkomstig uit de invoerstroom als tekenreeksen en moet worden geconverteerd naar **INT** om uit te voeren **som**.

**Invoer**:

| Maken | Time | Gewicht |
| --- | --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |"1000" |
| Honda |2015-01-01T00:00:02.0000000Z |"2000" |

**Uitvoer**:

| Maken | Gewicht |
| --- | --- |
| Honda |3000 |

**Oplossing**:

```SQL
    SELECT
        Make,
        SUM(CAST(Weight AS BIGINT)) AS Weight
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)
```

**Uitleg bij**: Gebruik een **CAST** -instructie in de **gewicht** veld naar het gegevenstype opgeven. Zie de lijst met ondersteunde gegevenstypen in [gegevenstypen (Azure Stream Analytics)](/stream-analytics-query/data-types-azure-stream-analytics).

## <a name="query-example-use-likenot-like-to-do-pattern-matching"></a>Voorbeeld: Gebruik LIKE/NOT zoals als u wilt patroon die overeenkomt met

**Beschrijving**: Controleer of de waarde van een veld van de gebeurtenis overeenkomt met een bepaalde patroon.
Bijvoorbeeld, Controleer het resultaat licentie platen die beginnen met A en eindigen met 9.

**Invoer**:

| Maken | LicensePlate | Time |
| --- | --- | --- |
| Honda |ABC-123 |2015-01-01T00:00:01.0000000Z |
| Toyota |AAA-999 |2015-01-01T00:00:02.0000000Z |
| Nissan |ABC-369 |2015-01-01T00:00:03.0000000Z |

**Uitvoer**:

| Maken | LicensePlate | Time |
| --- | --- | --- |
| Toyota |AAA-999 |2015-01-01T00:00:02.0000000Z |
| Nissan |ABC-369 |2015-01-01T00:00:03.0000000Z |

**Oplossing**:

```SQL
    SELECT
        *
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LicensePlate LIKE 'A%9'
```

**Uitleg bij**: Gebruik de **zoals** instructie om te controleren of de **LicensePlate** veld waarde. Beginnen met de letter A, moet en vervolgens hebt een willekeurige tekenreeks van nul of meer tekens, en vervolgens eindigen met het getal 9. 

## <a name="query-example-specify-logic-for-different-casesvalues-case-statements"></a>Voorbeeld: Geef de logica voor verschillende aanvragen/waarden (CASE-instructies)

**Beschrijving**: Geef een andere berekening voor een veld, op basis van een bepaald criterium. Bijvoorbeeld, Geef een beschrijving voor het aantal auto's van dezelfde maken die is doorgegeven, met een speciaal geval voor 1.

**Invoer**:

| Maken | Time |
| --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |
| Toyota |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:03.0000000Z |

**Uitvoer**:

| CarsPassed | Time |
| --- | --- |
| 1 Honda |2015-01-01T00:00:10.0000000Z |
| 2 Toyotas |2015-01-01T00:00:10.0000000Z |

**Oplossing**:

```SQL
    SELECT
        CASE
            WHEN COUNT(*) = 1 THEN CONCAT('1 ', Make)
            ELSE CONCAT(CAST(COUNT(*) AS NVARCHAR(MAX)), ' ', Make, 's')
        END AS CarsPassed,
        System.TimeStamp() AS Time
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)
```

**Uitleg bij**: De **geval** expressie vergelijkt een expressie naar een set eenvoudige expressies om te bepalen van het resultaat. In dit voorbeeld maakt vehicle met een telling van 1, de beschrijving van een andere tekenreeks geretourneerd dan vehicle met een telling dan 1 maakt.

## <a name="query-example-send-data-to-multiple-outputs"></a>Voorbeeld: Gegevens verzenden naar meerdere uitvoer

**Beschrijving**: Gegevens verzenden naar meerdere doelen van de uitvoer van een eenmalige taak. Bijvoorbeeld: analyseren van gegevens voor een waarschuwing op basis van een drempelwaarde en alle gebeurtenissen naar blob-opslag te archiveren.

**Invoer**:

| Maken | Time |
| --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |
| Honda |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:01.0000000Z |
| Toyota |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:03.0000000Z |

**Output1**:

| Maken | Time |
| --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |
| Honda |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:01.0000000Z |
| Toyota |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:03.0000000Z |

**Output2**:

| Maken | Time | Count |
| --- | --- | --- |
| Toyota |2015-01-01T00:00:10.0000000Z |3 |

**Oplossing**:

```SQL
    SELECT
        *
    INTO
        ArchiveOutput
    FROM
        Input TIMESTAMP BY Time

    SELECT
        Make,
        System.TimeStamp() AS Time,
        COUNT(*) AS [Count]
    INTO
        AlertOutput
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)
    HAVING
        [Count] >= 3
```

**Uitleg bij**: De **INTO** component Stream Analytics wordt uitgelegd welke van de uitvoer naar het schrijven van de gegevens uit deze instructie. De eerste query is een Pass Through-query van de gegevens naar een uitvoer met de naam ontvangen **ArchiveOutput**. De tweede query biedt een eenvoudige aggregatie en filteren en deze stuurt de resultaten naar een downstream waarschuwingssysteem **AlertOutput**.

Houd er rekening mee dat u de resultaten van de algemene tabelexpressies (CTE's) ook opnieuw kunt gebruiken (zoals **WITH** instructies) in meerdere uitvoer-instructies. Deze optie heeft het voordeel van het openen van de invoerbron minder lezers.

Bijvoorbeeld: 

```SQL
    WITH AllRedCars AS (
        SELECT
            *
        FROM
            Input TIMESTAMP BY Time
        WHERE
            Color = 'red'
    )
    SELECT * INTO HondaOutput FROM AllRedCars WHERE Make = 'Honda'
    SELECT * INTO ToyotaOutput FROM AllRedCars WHERE Make = 'Toyota'
```

## <a name="query-example-count-unique-values"></a>Voorbeeld: De unieke waarden tellen

**Beschrijving**: Telt het aantal unieke waarden die worden weergegeven in de stroom binnen een periode. Bijvoorbeeld, het aantal unieke maakt van auto's doorgegeven aan de stand nummer in een venster 2 seconden?

**Invoer**:

| Maken | Time |
| --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |
| Honda |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:01.0000000Z |
| Toyota |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:03.0000000Z |

**De uitvoer:**

| CountMake | Time |
| --- | --- |
| 2 |2015-01-01T00:00:02.000Z |
| 1 |2015-01-01T00:00:04.000Z |

**Oplossing:**

```SQL
SELECT
     COUNT(DISTINCT Make) AS CountMake,
     System.TIMESTAMP() AS TIME
FROM Input TIMESTAMP BY TIME
GROUP BY 
     TumblingWindow(second, 2)
```


**Uitleg:**
**aantal (uniek zorg)** retourneert het aantal distinctieve waarden in de **maken** kolom binnen een periode.

## <a name="query-example-determine-if-a-value-has-changed"></a>Voorbeeld: Bepalen of een waarde is gewijzigd

**Beschrijving**: Bekijk een vorige waarde om te bepalen of deze anders is dan de huidige waarde. Bijvoorbeeld, is de vorige auto nummer onderweg de dezelfde maken als de huidige auto?

**Invoer**:

| Maken | Time |
| --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |
| Toyota |2015-01-01T00:00:02.0000000Z |

**Uitvoer**:

| Maken | Time |
| --- | --- |
| Toyota |2015-01-01T00:00:02.0000000Z |

**Oplossing**:

```SQL
    SELECT
        Make,
        Time
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LAG(Make, 1) OVER (LIMIT DURATION(minute, 1)) <> Make
```

**Uitleg bij**: Gebruik **LAG** bekijken in de invoerstroom één gebeurtenis weer, waarna de **maken** waarde. Vergelijk deze naar de **maken** -waarde op de huidige gebeurtenis en de uitvoer van de gebeurtenis als deze verschillend zijn.

## <a name="query-example-find-the-first-event-in-a-window"></a>Voorbeeld: Zoek de eerste gebeurtenis in een venster

**Beschrijving**: De eerste auto niet vinden in de interval van elke 10 minuten.

**Invoer**:

| LicensePlate | Maken | Time |
| --- | --- | --- |
| DXE 5291 |Honda |2015-07-27T00:00:05.0000000Z |
| YZK 5704 |Ford |2015-07-27T00:02:17.0000000Z |
| RMV 8282 |Honda |2015-07-27T00:05:01.0000000Z |
| YHN 6970 |Toyota |2015-07-27T00:06:00.0000000Z |
| VFE 1616 |Toyota |2015-07-27T00:09:31.0000000Z |
| QYF 9358 |Honda |2015-07-27T00:12:02.0000000Z |
| MDR 6128 |BMW |2015-07-27T00:13:45.0000000Z |

**Uitvoer**:

| LicensePlate | Maken | Time |
| --- | --- | --- |
| DXE 5291 |Honda |2015-07-27T00:00:05.0000000Z |
| QYF 9358 |Honda |2015-07-27T00:12:02.0000000Z |

**Oplossing**:

```SQL
    SELECT 
        LicensePlate,
        Make,
        Time
    FROM 
        Input TIMESTAMP BY Time
    WHERE 
        IsFirst(minute, 10) = 1
```

Nu gaan we het probleem wijzigen en de eerste auto van een bepaalde versie zoeken in een interval van elke 10 minuten.

| LicensePlate | Maken | Time |
| --- | --- | --- |
| DXE 5291 |Honda |2015-07-27T00:00:05.0000000Z |
| YZK 5704 |Ford |2015-07-27T00:02:17.0000000Z |
| YHN 6970 |Toyota |2015-07-27T00:06:00.0000000Z |
| QYF 9358 |Honda |2015-07-27T00:12:02.0000000Z |
| MDR 6128 |BMW |2015-07-27T00:13:45.0000000Z |

**Oplossing**:

```SQL
    SELECT 
        LicensePlate,
        Make,
        Time
    FROM 
        Input TIMESTAMP BY Time
    WHERE 
        IsFirst(minute, 10) OVER (PARTITION BY Make) = 1
```

## <a name="query-example-find-the-last-event-in-a-window"></a>Voorbeeld: Zoek de laatste gebeurtenis in een venster

**Beschrijving**: De laatste auto niet vinden in de interval van elke 10 minuten.

**Invoer**:

| LicensePlate | Maken | Time |
| --- | --- | --- |
| DXE 5291 |Honda |2015-07-27T00:00:05.0000000Z |
| YZK 5704 |Ford |2015-07-27T00:02:17.0000000Z |
| RMV 8282 |Honda |2015-07-27T00:05:01.0000000Z |
| YHN 6970 |Toyota |2015-07-27T00:06:00.0000000Z |
| VFE 1616 |Toyota |2015-07-27T00:09:31.0000000Z |
| QYF 9358 |Honda |2015-07-27T00:12:02.0000000Z |
| MDR 6128 |BMW |2015-07-27T00:13:45.0000000Z |

**Uitvoer**:

| LicensePlate | Maken | Time |
| --- | --- | --- |
| VFE 1616 |Toyota |2015-07-27T00:09:31.0000000Z |
| MDR 6128 |BMW |2015-07-27T00:13:45.0000000Z |

**Oplossing**:

```SQL
    WITH LastInWindow AS
    (
        SELECT 
            MAX(Time) AS LastEventTime
        FROM 
            Input TIMESTAMP BY Time
        GROUP BY 
            TumblingWindow(minute, 10)
    )
    SELECT 
        Input.LicensePlate,
        Input.Make,
        Input.Time
    FROM
        Input TIMESTAMP BY Time 
        INNER JOIN LastInWindow
        ON DATEDIFF(minute, Input, LastInWindow) BETWEEN 0 AND 10
        AND Input.Time = LastInWindow.LastEventTime
```

**Uitleg bij**: Er zijn twee stappen in de query. Het eerste item vindt de meest recente tijdstempel in windows 10 minuten. De tweede stap koppelt de resultaten van de eerste query met de oorspronkelijke stroom om de gebeurtenissen die overeenkomen met de laatste tijdstempels in elk venster te zoeken. 

## <a name="query-example-detect-the-absence-of-events"></a>Voorbeeld: Detecteren van de afwezigheid van gebeurtenissen

**Beschrijving**: Controleer of een stroom heeft geen waarde die overeenkomt met een bepaald criterium.
Bijvoorbeeld, hebt 2 opeenvolgende auto's uit het hetzelfde merk mobiel nummer ingevoerd in de afgelopen 90 seconden

**Invoer**:

| Maken | LicensePlate | Time |
| --- | --- | --- |
| Honda |ABC-123 |2015-01-01T00:00:01.0000000Z |
| Honda |AAA-999 |2015-01-01T00:00:02.0000000Z |
| Toyota |DEF-987 |2015-01-01T00:00:03.0000000Z |
| Honda |GHI-345 |2015-01-01T00:00:04.0000000Z |

**Uitvoer**:

| Maken | Time | CurrentCarLicensePlate | FirstCarLicensePlate | FirstCarTime |
| --- | --- | --- | --- | --- |
| Honda |2015-01-01T00:00:02.0000000Z |AAA-999 |ABC-123 |2015-01-01T00:00:01.0000000Z |

**Oplossing**:

```SQL
    SELECT
        Make,
        Time,
        LicensePlate AS CurrentCarLicensePlate,
        LAG(LicensePlate, 1) OVER (LIMIT DURATION(second, 90)) AS FirstCarLicensePlate,
        LAG(Time, 1) OVER (LIMIT DURATION(second, 90)) AS FirstCarTime
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LAG(Make, 1) OVER (LIMIT DURATION(second, 90)) = Make
```

**Uitleg bij**: Gebruik **LAG** bekijken in de invoerstroom één gebeurtenis weer, waarna de **maken** waarde. Vergelijk deze met de **maken** waarde in de huidige gebeurtenis, en vervolgens de gebeurtenis uitvoer als ze hetzelfde zijn. U kunt ook **LAG** ophalen van gegevens over de vorige auto.

## <a name="query-example-detect-the-duration-between-events"></a>Voorbeeld: De duur tussen gebeurtenissen te detecteren

**Beschrijving**: De duur van een bepaalde gebeurtenis vinden. Bijvoorbeeld, een web-clickstream worden gegeven, bepalen de tijd die op een functie.

**Invoer**:  

| Gebruiker | Functie | Gebeurtenis | Time |
| --- | --- | --- | --- |
| user@location.com |RightMenu |Starten |2015-01-01T00:00:01.0000000Z |
| user@location.com |RightMenu |Einde |2015-01-01T00:00:08.0000000Z |

**Uitvoer**:  

| Gebruiker | Functie | Duur |
| --- | --- | --- |
| user@location.com |RightMenu |7 |

**Oplossing**:

```SQL
    SELECT
        [user], feature, DATEDIFF(second, LAST(Time) OVER (PARTITION BY [user], feature LIMIT DURATION(hour, 1) WHEN Event = 'start'), Time) as duration
    FROM input TIMESTAMP BY Time
    WHERE
        Event = 'end'
```

**Uitleg bij**: Gebruik de **laatste** functie voor het ophalen van de laatste **tijd** waarde wanneer het gebeurtenistype is **Start**. De **laatste** functie maakt gebruik **PARTITION BY [user]** om aan te geven dat het resultaat wordt berekend per unieke gebruiker. De query heeft een maximale drempelwaarde van 1 uur voor het tijdsverschil tussen **Start** en **stoppen** gebeurtenissen, maar kan worden geconfigureerd naar behoefte **(LIMIET DURATION(hour, 1)**.

## <a name="query-example-detect-the-duration-of-a-condition"></a>Voorbeeld: De duur van een voorwaarde detecteren
**Beschrijving**: Zoek uit hoe lang die een voorwaarde is opgetreden.
Stel bijvoorbeeld dat een fout heeft geresulteerd in alle auto's met een onjuiste gewicht (meer dan 20.000 pond) en de duur van die fout moet worden berekend.

**Invoer**:

| Maken | Time | Gewicht |
| --- | --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |2000 |
| Toyota |2015-01-01T00:00:02.0000000Z |25000 |
| Honda |2015-01-01T00:00:03.0000000Z |26000 |
| Toyota |2015-01-01T00:00:04.0000000Z |25000 |
| Honda |2015-01-01T00:00:05.0000000Z |26000 |
| Toyota |2015-01-01T00:00:06.0000000Z |25000 |
| Honda |2015-01-01T00:00:07.0000000Z |26000 |
| Toyota |2015-01-01T00:00:08.0000000Z |2000 |

**Uitvoer**:

| StartFault | EndFault |
| --- | --- |
| 2015-01-01T00:00:02.000Z |2015-01-01T00:00:07.000Z |

**Oplossing**:

```SQL
    WITH SelectPreviousEvent AS
    (
    SELECT
    *,
        LAG([time]) OVER (LIMIT DURATION(hour, 24)) as previousTime,
        LAG([weight]) OVER (LIMIT DURATION(hour, 24)) as previousWeight
    FROM input TIMESTAMP BY [time]
    )

    SELECT 
        LAG(time) OVER (LIMIT DURATION(hour, 24) WHEN previousWeight < 20000 ) [StartFault],
        previousTime [EndFault]
    FROM SelectPreviousEvent
    WHERE
        [weight] < 20000
        AND previousWeight > 20000
```

**Uitleg bij**: Gebruik **LAG** voor het weergeven van de invoerstroom 24 uur en zoek naar waar u exemplaren **StartFault** en **StopFault** wordt omspannen door het gewicht < 20000.

## <a name="query-example-fill-missing-values"></a>Voorbeeld: Vul de ontbrekende waarden

**Beschrijving**: Voor het streamen van gebeurtenissen met ontbrekende waarden produceren van een stroom gebeurtenissen met regelmatige intervallen. Bijvoorbeeld, Genereer een gebeurtenis om de vijf seconden die het meest recent gezien gegevenspunt rapporteert.

**Invoer**:

| t | value |
| --- | --- |
| "2014-01-01T06:01:00" |1 |
| "2014-01-01T06:01:05" |2 |
| "2014-01-01T06:01:10" |3 |
| "2014-01-01T06:01:15" |4 |
| "2014-01-01T06:01:30" |5 |
| "2014-01-01T06:01:35" |6 |

**Uitvoer (eerste 10 rijen)**:

| windowend | lastevent.t | lastevent.value |
| --- | --- | --- |
| 2014-01-01T14:01:00.000Z |2014-01-01T14:01:00.000Z |1 |
| 2014-01-01T14:01:05.000Z |2014-01-01T14:01:05.000Z |2 |
| 2014-01-01T14:01:10.000Z |2014-01-01T14:01:10.000Z |3 |
| 2014-01-01T14:01:15.000Z |2014-01-01T14:01:15.000Z |4 |
| 2014-01-01T14:01:20.000Z |2014-01-01T14:01:15.000Z |4 |
| 2014-01-01T14:01:25.000Z |2014-01-01T14:01:15.000Z |4 |
| 2014-01-01T14:01:30.000Z |2014-01-01T14:01:30.000Z |5 |
| 2014-01-01T14:01:35.000Z |2014-01-01T14:01:35.000Z |6 |
| 2014-01-01T14:01:40.000Z |2014-01-01T14:01:35.000Z |6 |
| 2014-01-01T14:01:45.000Z |2014-01-01T14:01:35.000Z |6 |

**Oplossing**:

```SQL
    SELECT
        System.Timestamp() AS windowEnd,
        TopOne() OVER (ORDER BY t DESC) AS lastEvent
    FROM
        input TIMESTAMP BY t
    GROUP BY HOPPINGWINDOW(second, 300, 5)
```

**Uitleg bij**: Deze query gebeurtenissen worden gegenereerd om de vijf seconden en voert de laatste gebeurtenis die eerder is ontvangen. De [Hopping venster](/stream-analytics-query/hopping-window-azure-stream-analytics) duur bepaalt hoe ver terug de query ziet er uit als u wilt zoeken van de meest recente gebeurtenis (300 seconden in dit voorbeeld).


## <a name="query-example-correlate-two-event-types-within-the-same-stream"></a>Voorbeeld: Correleren van twee typen gebeurtenissen binnen de dezelfde stroom

**Beschrijving**: Soms moeten waarschuwingen worden gegenereerd op basis van meerdere typen van gebeurtenissen die zijn opgetreden in een bepaalde periode. Bijvoorbeeld in een IoT-scenario voor thuis weerstaan, een waarschuwing moet worden gegenereerd wanneer de temperatuur ventilator lager dan 40 is en de maximale kracht gedurende de laatste 3 minuten minder dan 10 is.

**Invoer**:

| time | deviceId | sensorName | value |
| --- | --- | --- | --- |
| "2018-01-01T16:01:00" | "Oven1" | "temp" |120 |
| "2018-01-01T16:01:00" | "Oven1" | "power" |15 |
| "2018-01-01T16:02:00" | "Oven1" | "temp" |100 |
| "2018-01-01T16:02:00" | "Oven1" | "power" |15 |
| "2018-01-01T16:03:00" | "Oven1" | "temp" |70 |
| "2018-01-01T16:03:00" | "Oven1" | "power" |15 |
| "2018-01-01T16:04:00" | "Oven1" | "temp" |50 |
| "2018-01-01T16:04:00" | "Oven1" | "power" |15 |
| "2018-01-01T16:05:00" | "Oven1" | "temp" |30 |
| "2018-01-01T16:05:00" | "Oven1" | "power" |8 |
| "2018-01-01T16:06:00" | "Oven1" | "temp" |20 |
| "2018-01-01T16:06:00" | "Oven1" | "power" |8 |
| "2018-01-01T16:07:00" | "Oven1" | "temp" |20 |
| "2018-01-01T16:07:00" | "Oven1" | "power" |8 |
| "2018-01-01T16:08:00" | "Oven1" | "temp" |20 |
| "2018-01-01T16:08:00" | "Oven1" | "power" |8 |

**Uitvoer**:

| eventTime | deviceId | temp | alertMessage | maxPowerDuringLast3mins |
| --- | --- | --- | --- | --- | 
| "2018-01-01T16:05:00" | "Oven1" |30 | "Korte circuit verwarming elementen" |15 |
| "2018-01-01T16:06:00" | "Oven1" |20 | "Korte circuit verwarming elementen" |15 |
| "2018-01-01T16:07:00" | "Oven1" |20 | "Korte circuit verwarming elementen" |15 |

**Oplossing**:

```SQL
WITH max_power_during_last_3_mins AS (
    SELECT 
        System.TimeStamp() AS windowTime,
        deviceId,
        max(value) as maxPower
    FROM
        input TIMESTAMP BY t
    WHERE 
        sensorName = 'power' 
    GROUP BY 
        deviceId, 
        SlidingWindow(minute, 3) 
)

SELECT 
    t1.t AS eventTime,
    t1.deviceId, 
    t1.value AS temp,
    'Short circuit heating elements' as alertMessage,
    t2.maxPower AS maxPowerDuringLast3mins
    
INTO resultsr

FROM input t1 TIMESTAMP BY t
JOIN max_power_during_last_3_mins t2
    ON t1.deviceId = t2.deviceId 
    AND t1.t = t2.windowTime
    AND DATEDIFF(minute,t1,t2) between 0 and 3
    
WHERE
    t1.sensorName = 'temp'
    AND t1.value <= 40
    AND t2.maxPower > 10
```

**Uitleg bij**: De eerste query `max_power_during_last_3_mins`, maakt gebruik van de [schuifregelaar venster](/stream-analytics-query/sliding-window-azure-stream-analytics) gezocht naar de maximale waarde van de sensor power voor elk apparaat tijdens de laatste 3 minuten. De tweede query is gekoppeld aan de eerste query gezocht naar de power waarde in het venster van de meest recente relevant zijn voor de huidige gebeurtenis. En vervolgens, mits de voorwaarden wordt voldaan, wordt een waarschuwing gegenereerd voor het apparaat.

## <a name="query-example-process-events-independent-of-device-clock-skew-substreams"></a>Voorbeeld: Gebeurtenissen verwerken onafhankelijk van het apparaat klok scheeftrekken (substreams)

**Beschrijving**: Gebeurtenissen kunnen worden gemeld of niet-geordende gevolg van tijdsverschillen tussen gebeurtenisproducers, klok Hiermee laat u overhellen tussen partities of netwerklatentie. In het volgende voorbeeld wordt de apparaatklok van uw voor TollID 2 is vijf seconden na TollID 1 en de apparaatklok van uw voor TollID 3 is tien seconden achter TollID 1. 

**Invoer**:

| LicensePlate | Maken | Time | TollID |
| --- | --- | --- | --- |
| DXE 5291 |Honda |2015-07-27T00:00:01.0000000Z | 1 |
| YHN 6970 |Toyota |2015-07-27T00:00:05.0000000Z | 1 |
| QYF 9358 |Honda |2015-07-27T00:00:01.0000000Z | 2 |
| GXF 9462 |BMW |2015-07-27T00:00:04.0000000Z | 2 |
| VFE 1616 |Toyota |2015-07-27T00:00:10.0000000Z | 1 |
| RMV 8282 |Honda |2015-07-27T00:00:03.0000000Z | 3 |
| MDR 6128 |BMW |2015-07-27T00:00:11.0000000Z | 2 |
| YZK 5704 |Ford |2015-07-27T00:00:07.0000000Z | 3 |

**Uitvoer**:

| TollID | Count |
| --- | --- |
| 1 | 2 |
| 2 | 2 |
| 1 | 1 |
| 3 | 1 |
| 2 | 1 |
| 3 | 1 |

**Oplossing**:

```SQL
SELECT
      TollId,
      COUNT(*) AS Count
FROM input
      TIMESTAMP BY Time OVER TollId
GROUP BY TUMBLINGWINDOW(second, 5), TollId
```

**Uitleg bij**: De [TIMESTAMP BY OVER](/stream-analytics-query/timestamp-by-azure-stream-analytics#over-clause-interacts-with-event-ordering) component kijkt naar de tijdlijn van elk apparaat afzonderlijk met substreams. De uitvoergebeurtenissen voor elke TollID worden gegenereerd als ze worden berekend, wat betekent dat de gebeurtenissen in volgorde met betrekking tot elke TollID in plaats van de volgorde van worden alsof alle apparaten op de dezelfde klok wordt gewijzigd.

## <a name="query-example-remove-duplicate-events-in-a-window"></a>Voorbeeld: Verwijderen van dubbele gebeurtenissen in een venster

**Beschrijving**: Bij het uitvoeren van een bewerking zoals het berekenen van gemiddelden over gebeurtenissen in een bepaalde periode, moeten dubbele gebeurtenissen worden gefilterd. In het volgende voorbeeld is de tweede gebeurtenis een duplicaat van de eerste.

**Invoer**:  

| DeviceId | Time | Kenmerk | Value |
| --- | --- | --- | --- |
| 1 |2018-07-27T00:00:01.0000000Z |Temperatuur |50 |
| 1 |2018-07-27T00:00:01.0000000Z |Temperatuur |50 |
| 2 |2018-07-27T00:00:01.0000000Z |Temperatuur |40 |
| 1 |2018-07-27T00:00:05.0000000Z |Temperatuur |60 |
| 2 |2018-07-27T00:00:05.0000000Z |Temperatuur |50 |
| 1 |2018-07-27T00:00:10.0000000Z |Temperatuur |100 |

**Uitvoer**:  

| AverageValue | DeviceId |
| --- | --- |
| 70 | 1 |
|45 | 2 |

**Oplossing**:

```SQL
With Temp AS (
    SELECT
        COUNT(DISTINCT Time) AS CountTime,
        Value,
        DeviceId
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Value,
        DeviceId,
        SYSTEM.TIMESTAMP()
)

SELECT
    AVG(Value) AS AverageValue, DeviceId
INTO Output
FROM Temp
GROUP BY DeviceId,TumblingWindow(minute, 5)
```

**Uitleg bij**: [AANTAL (uniek tijd)](/stream-analytics-query/count-azure-stream-analytics) retourneert het aantal unieke waarden in de Time-kolom binnen een periode. U kunt vervolgens de uitvoer van deze stap gebruiken voor het berekenen van de gemiddelde per apparaat door het verwijderen van duplicaten.

## <a name="get-help"></a>Hulp vragen

Voor verdere ondersteuning kunt u proberen onze [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Volgende stappen
* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)

