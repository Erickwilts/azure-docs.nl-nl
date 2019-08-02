---
title: Opnieuw partitioneren gebruiken om de verwerking met Azure Stream Analytics te optimaliseren
description: In dit artikel wordt beschreven hoe u opnieuw partitioneren gebruikt om Azure Stream Analytics taken te optimaliseren die niet kunnen worden geparallelleerd.
ms.service: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.date: 07/26/2019
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: 9c802e6d23daf502da351549c66a7dae1247c068
ms.sourcegitcommit: f5cc71cbb9969c681a991aa4a39f1120571a6c2e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68517434"
---
# <a name="use-repartitioning-to-optimize-processing-with-azure-stream-analytics"></a>Opnieuw partitioneren gebruiken om de verwerking met Azure Stream Analytics te optimaliseren

Dit artikel laat u zien hoe u opnieuw partitioneren kunt gebruiken om uw Azure Stream Analytics-query te schalen op scenario's [](stream-analytics-scale-jobs.md)die niet volledig kunnen worden geparallelleerd.

U kunt [parallel Lise ring](stream-analytics-parallelization.md) mogelijk niet gebruiken als:

* U hebt geen controle over de partitie sleutel voor de invoer stroom.
* De bron ' besproeit ' op meerdere partities die later moeten worden samengevoegd. 

## <a name="how-to-repartition"></a>Opnieuw partitioneren

Wanneer u gegevens verwerkt op een stroom die niet Shard is volgens een natuurlijk invoer schema, zoals **PartitionId** voor Event hubs, moet u opnieuw partitioneren of de volg orde opnieuw opgeven. Wanneer u de partitie opnieuw partitioneert, kan elke Shard onafhankelijk worden verwerkt, zodat u de streaming-pijp lijn lineair kunt schalen.

Als u opnieuw wilt partitioneren, gebruikt u het tref woord **in** na een **partitie op** instructie in uw query. In het volgende voor beeld worden de gegevens door **DeviceID** gepartitioneerd tot een aantal van 10. Hashing van **DeviceID** wordt gebruikt om te bepalen welke partitie moet worden geaccepteerd door welke substream. De gegevens worden onafhankelijk van elke gepartitioneerde stroom leeg gemaakt, ervan uitgaande dat de uitvoer gepartitioneerde schrijf bewerkingen ondersteunt en 10 partities heeft.

```sql
SELECT * 
INTO output
FROM input
PARTITION BY DeviceID 
INTO 10
```

Met de volgende voorbeeld query worden twee gegevens stromen van gepartitioneerde gegevens samengevoegd. Wanneer u twee streams van gepartitioneerde gegevens samenvoegt, moeten de streams dezelfde partitie sleutel en hetzelfde aantal hebben. Het resultaat is een stroom met hetzelfde partitie schema.

```sql
WITH step1 AS (SELECT * FROM input1 PARTITION BY DeviceID INTO 10),
step2 AS (SELECT * FROM input2 PARTITION BY DeviceID INTO 10)

SELECT * INTO output FROM step1 PARTITION BY DeviceID UNION step2 PARTITION BY DeviceID
```

Het uitvoer schema moet overeenkomen met de sleutel en het aantal van het stroom schema, zodat elke substream onafhankelijk kan worden leeg gemaakt. De stroom kan ook opnieuw worden samengevoegd en opnieuw worden gepartitioneerd met een ander schema voordat deze wordt leeg gemaakt, maar u moet deze methode vermijden omdat deze wordt toegevoegd aan de algemene latentie van de verwerking en het resource gebruik verhoogt.

## <a name="streaming-units-for-repartitions"></a>Streaming-eenheden voor het opnieuw partitioneren

Experimenteer en Bekijk het resource gebruik van uw taak om het exacte aantal partities te bepalen dat u nodig hebt. Het aantal [streaming-eenheden (su)](stream-analytics-streaming-unit-consumption.md) moet worden aangepast op basis van de fysieke resources die nodig zijn voor elke partitie. Over het algemeen is zes SUs vereist voor elke partitie. Als er onvoldoende resources aan de taak zijn toegewezen, past het systeem alleen de herpartitie toe als de taak wordt voor deel.

## <a name="repartitions-for-sql-output"></a>Opnieuw partitioneert voor SQL-uitvoer

Wanneer uw taak SQL database voor uitvoer gebruikt, gebruikt u expliciete herpartitionering om het maximale aantal partities te maximaliseren. Omdat SQL het meest geschikt is voor acht schrijvers, moet u de stroom opnieuw partitioneren naar acht vóór het leegmaken of ergens anders, waardoor de taak prestaties kunnen worden verzorgd. Zie [Azure stream Analytics uitvoer naar Azure SQL database](stream-analytics-sql-output-perf.md)voor meer informatie.


## <a name="next-steps"></a>Volgende stappen

* [Aan de slag met Azure Stream Analytics](stream-analytics-introduction.md)
* [Gebruik query parallel Lise ring in Azure Stream Analytics](stream-analytics-parallelization.md)