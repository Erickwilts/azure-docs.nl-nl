---
title: Systeem variabelen in Azure Data Factory
description: In dit artikel worden de systeem variabelen beschreven die door Azure Data Factory worden ondersteund. U kunt deze variabelen gebruiken in expressies bij het definiëren van Data Factory entiteiten.
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 06/12/2018
ms.openlocfilehash: 0a5237336530d30c3801b13b910171e236e87a23
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73679278"
---
# <a name="system-variables-supported-by-azure-data-factory"></a>Systeem variabelen die door Azure Data Factory worden ondersteund
In dit artikel worden de systeem variabelen beschreven die door Azure Data Factory worden ondersteund. U kunt deze variabelen gebruiken in expressies bij het definiëren van Data Factory entiteiten.

## <a name="pipeline-scope"></a>Pijplijn bereik
Naar deze systeem variabelen kan ergens worden verwezen in de JSON van de pijp lijn.

| Naam variabele | Beschrijving |
| --- | --- |
| @pipeline(). DataFactory |De naam van de data factory de pijplijn uitvoering wordt uitgevoerd in |
| @pipeline(). Pijp lijn |Naam van de pijp lijn |
| @pipeline(). RunId | ID van de specifieke pijplijn uitvoering |
| @pipeline(). Trigger type | Type trigger waarmee de pijp lijn is aangeroepen (hand matig, Scheduler) |
| @pipeline(). TriggerId| ID van de trigger die de pijp lijn aanroept |
| @pipeline(). TriggerName| De naam van de trigger die de pijp lijn aanroept |
| @pipeline(). TriggerTime| Tijd waarop de trigger de pijp lijn heeft aangeroepen. De trigger tijd is de werkelijke tijd die wordt geactiveerd, niet de geplande tijd. `13:20:08.0149599Z` bijvoorbeeld wordt geretourneerd in plaats van `13:20:00.00Z` |

## <a name="schedule-trigger-scope"></a>Trigger bereik plannen
Naar deze systeem variabelen kan overal in de JSON van de trigger worden verwezen als de trigger van het volgende type is: "ScheduleTrigger."

| Naam variabele | Beschrijving |
| --- | --- |
| @trigger(). scheduledTime |Tijdstip waarop de trigger is gepland voor het aanroepen van de pijplijn uitvoering. Voor een trigger die elke vijf minuten wordt geactiveerd, retourneert deze variabele bijvoorbeeld `2017-06-01T22:20:00Z`, `2017-06-01T22:25:00Z``2017-06-01T22:30:00Z` respectievelijk.|
| @trigger(). startTime |Tijdstip waarop de trigger **daad werkelijk** is gestart om de pijplijn uitvoering aan te roepen. Voor een trigger die bijvoorbeeld elke 5 minuten wordt geactiveerd, retourneert deze variabele mogelijk iets zoals deze `2017-06-01T22:20:00.4061448Z`, `2017-06-01T22:25:00.7958577Z``2017-06-01T22:30:00.9935483Z` respectievelijk. (Opmerking: de tijds tempel is standaard in ISO 8601-indeling)|

## <a name="tumbling-window-trigger-scope"></a>Tumblingvenstertriggers-venster trigger bereik
Naar deze systeem variabelen kan overal in de JSON van de trigger worden verwezen als de trigger van het volgende type is: "TumblingWindowTrigger."
(Opmerking: de tijds tempel is standaard in ISO 8601-indeling)

| Naam variabele | Beschrijving |
| --- | --- |
| @trigger(). outputs. windowStartTime |Begin van het venster wanneer de trigger is gepland voor het aanroepen van de pijplijn uitvoering. Als de trigger van het tumblingvenstertriggers-venster een frequentie van elk uur heeft, is dit de tijd aan het begin van het uur.|
| @trigger(). outputs. windowEndTime |Het einde van het venster wanneer de trigger is gepland voor het aanroepen van de pijplijn uitvoering. Als de trigger van het tumblingvenstertriggers-venster een frequentie van elk uur heeft, is dit de tijd aan het einde van het uur.|
## <a name="next-steps"></a>Volgende stappen
Zie [expressie language & functions (Engelstalig](control-flow-expression-language-functions.md)) voor meer informatie over hoe deze variabelen worden gebruikt in expressies.
