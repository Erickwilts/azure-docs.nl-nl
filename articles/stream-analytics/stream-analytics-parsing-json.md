---
title: Het parseren van JSON en AVRO in Azure Stream Analytics
description: In dit artikel wordt beschreven hoe u werken met complexe gegevenstypen zoals matrices, JSON, opgemaakt CSV-gegevens.
services: stream-analytics
ms.service: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: ad30d363c8e3ea0264ba79db5417e572614a6739
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66496840"
---
# <a name="parse-json-and-avro-data-in-azure-stream-analytics"></a>Parseren van JSON en Avro-gegevens in Azure Stream Analytics

Azure Stream Analytics ondersteunt verwerking van gebeurtenissen in de opmaak van CSV, JSON en Avro-gegevens. JSON- en Avro-gegevens kunnen complexe typen zoals geneste objecten (records) en matrices bevatten.

## <a name="array-data-types"></a>Matrix-gegevenstypen

Matrix-gegevenstypen zijn een geordende reeks waarden. Sommige normale bewerkingen op basis van matrixwaarden worden hieronder beschreven. Deze voorbeelden wordt ervan uitgegaan dat de invoer gebeurtenissen hebben een eigenschap met de naam 'arrayField' dat wil zeggen een matrix-gegevenstype.

Deze voorbeelden gebruiken de functies [GetArrayElement](https://msdn.microsoft.com/azure/stream-analytics/reference/getarrayelement-azure-stream-analytics), [GetArrayElements](https://msdn.microsoft.com/azure/stream-analytics/reference/getarrayelements-azure-stream-analytics), [GetArrayLength](https://msdn.microsoft.com/azure/stream-analytics/reference/getarraylength-azure-stream-analytics), en de [toepassen](https://msdn.microsoft.com/azure/stream-analytics/reference/apply-azure-stream-analytics) operator.

## <a name="examples"></a>Voorbeelden
Selecteer matrixelement op een opgegeven index (waarbij u het eerste matrixelement):

```SQL
SELECT
    GetArrayElement(arrayField, 0) AS firstElement
FROM input
```

Selecteer de matrixlengte van de:

```SQL
SELECT
    GetArrayLength(arrayField) AS arrayLength
FROM input
```

Selecteer alle matrixelement als afzonderlijke gebeurtenissen. De [toepassen](https://msdn.microsoft.com/azure/stream-analytics/reference/apply-azure-stream-analytics) operator samen met de [GetArrayElements](https://msdn.microsoft.com/azure/stream-analytics/reference/getarrayelements-azure-stream-analytics) ingebouwde functie haalt alle matrixelementen als afzonderlijke gebeurtenissen:

```SQL
SELECT
    arrayElement.ArrayIndex,
    arrayElement.ArrayValue
FROM input as event
CROSS APPLY GetArrayElements(event.arrayField) AS arrayElement
```

## <a name="record-data-types"></a>Record-gegevenstypen
Record-gegevenstypen worden gebruikt om weer te geven van JSON en Avro matrices wanneer bijbehorende indelingen worden gebruikt in de invoer-gegevensstromen. Deze voorbeelden ziet u een voorbeeld van de sensor die invoer gebeurtenissen in JSON-indeling wordt gelezen. Hier volgt een voorbeeld van een enkelvoudige gebeurtenis:

```json
{
    "DeviceId" : "12345",
    "Location" :
    {
        "Lat": 47,
        "Long": 122
    },
    "SensorReadings" :
    {
        "Temperature" : 80,
        "Humidity" : 70,
        "CustomSensor01" : 5,
        "CustomSensor02" : 99
    }
}
```

## <a name="examples"></a>Voorbeelden
Gebruik een puntnotering (.) voor toegang tot geneste velden. Deze query worden bijvoorbeeld de breedtegraad en lengtegraad coördinaten onder de locatie-eigenschap in de voorgaande JSON-gegevens geselecteerd:

```SQL
SELECT
    DeviceID,
    Location.Lat,
    Location.Long
FROM input
```

Gebruik de [GetRecordPropertyValue](https://msdn.microsoft.com/azure/stream-analytics/reference/getrecordpropertyvalue-azure-stream-analytics) als naam van de eigenschap is onbekend. Denk bijvoorbeeld aan dat een voorbeeld van de gegevensstroom moet worden samengevoegd met verwijzing met gegevensdrempels voor elk apparaat sensor:

```json
{
    "DeviceId" : "12345",
    "SensorName" : "Temperature",
    "Value" : 75
}
```

```SQL
SELECT
    input.DeviceID,
    thresholds.SensorName
FROM input
JOIN thresholds
ON
    input.DeviceId = thresholds.DeviceId
WHERE
    GetRecordPropertyValue(input.SensorReadings, thresholds.SensorName) > thresholds.Value
```

Als u wilt converteren recordvelden in afzonderlijke gebeurtenissen, gebruikt u de [toepassen](https://msdn.microsoft.com/azure/stream-analytics/reference/apply-azure-stream-analytics) operator samen met de [GetRecordProperties](https://msdn.microsoft.com/azure/stream-analytics/reference/getrecordproperties-azure-stream-analytics) functie. Bijvoorbeeld, als u wilt een voorbeeldstroom converteren naar een stream van gebeurtenissen met de individuele sensorwaarden, serverlogs, kan deze query worden gebruikt:

```SQL
SELECT
    event.DeviceID,
    sensorReading.PropertyName,
    sensorReading.PropertyValue
FROM input as event
CROSS APPLY GetRecordProperties(event.SensorReadings) AS sensorReading
```

U kunt de eigenschappen van het gebruik van een geneste record selecteren ' *' Jokertekens. Kijk een naar het volgende voorbeeld:

```SQL
SELECT input.SensorReadings.*
FROM input
```

Het resultaat is:

```json
{
    "Temperature" : 80,
    "Humidity" : 70,
    "CustomSensor01" : 5,
    "CustomSensor022" : 99
}
```

## <a name="see-also"></a>Zie ook
[Gegevenstypen in Azure Stream Analytics](https://msdn.microsoft.com/azure/stream-analytics/reference/data-types-azure-stream-analytics)
