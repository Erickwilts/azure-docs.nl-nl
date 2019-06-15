---
title: Voorbeeldgegevens worden opgenomen in Azure Data Explorer
description: Meer informatie over het opnemen van (load) weergerelateerde voorbeeldgegevens in Azure Data Explorer.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: e80322cda671e2145cf3e65aa1457f1fa1827737
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60759274"
---
# <a name="ingest-sample-data-into-azure-data-explorer"></a>Voorbeeldgegevens worden opgenomen in Azure Data Explorer

Dit artikel ziet u hoe u voorbeeldgegevens (load) opnemen in de database van een Azure Data Explorer. Er zijn [verschillende manieren voor opname van gegevens](ingest-data-overview.md); in dit artikel is gericht op een eenvoudige benadering die geschikt is voor testdoeleinden.

> [!NOTE]
> U hebt al deze gegevens als u voltooid [Quick Start: Opname van gegevens met behulp van de Azure Data Explorer Python-clientbibliotheek](python-ingest-data.md).

## <a name="prerequisites"></a>Vereisten

[Een cluster en database voor testdoeleinden](create-cluster-database-portal.md)

## <a name="ingest-data"></a>Gegevens opnemen

De set met voorbeeldgegevens **StormEvents** bevat gegevens van het weer afkomstig van de [National Centers for Environmental Information](https://www.ncdc.noaa.gov/stormevents/).

1. Meld u aan bij [https://dataexplorer.azure.com](https://dataexplorer.azure.com).

1. Selecteer in de linkerbovenhoek van de toepassing **Add Cluster**.

1. In de **toevoegen cluster** dialoogvenster vak, voert u uw cluster-URL in het formulier `https://<ClusterName>.<Region>.kusto.windows.net/`en selecteer vervolgens **toevoegen**.

1. Plak in de volgende opdracht uit en selecteer **uitvoeren**.

    ```Kusto
    .create table StormEvents (StartTime: datetime, EndTime: datetime, EpisodeId: int, EventId: int, State: string, EventType: string, InjuriesDirect: int, InjuriesIndirect: int, DeathsDirect: int, DeathsIndirect: int, DamageProperty: int, DamageCrops: int, Source: string, BeginLocation: string, EndLocation: string, BeginLat: real, BeginLon: real, EndLat: real, EndLon: real, EpisodeNarrative: string, EventNarrative: string, StormSummary: dynamic)

    .ingest into table StormEvents h'https://kustosamplefiles.blob.core.windows.net/samplefiles/StormEvents.csv?st=2018-08-31T22%3A02%3A25Z&se=2020-09-01T22%3A02%3A00Z&sp=r&sv=2018-03-28&sr=b&sig=LQIbomcKI8Ooz425hWtjeq6d61uEaq21UVX7YrM61N4%3D' with (ignoreFirstRecord=true)
    ```

1. Nadat de opname is voltooid, plak in de volgende query, selecteert u de query in het venster en selecteer **uitvoeren**.

    ```Kusto
    StormEvents
    | sort by StartTime desc
    | take 10
    ```
    De query retourneert de volgende resultaten van de opgenomen voorbeeldgegevens.

    ![Queryresultaten](media/ingest-sample-data/query-results.png)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Snelstart: query's uitvoeren op gegevens in Azure Data Explorer](web-query-data.md)

> [!div class="nextstepaction"]
> [Query's schrijven](write-queries.md)

> [!div class="nextstepaction"]
> [Gegevensopname met Azure Data Explorer](ingest-data-overview.md)