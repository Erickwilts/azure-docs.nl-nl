---
title: Inzicht in de taak bewaken in Azure Stream Analytics
description: In dit artikel wordt beschreven hoe u Azure Stream Analytics-taken in Azure portal controleren.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/07/2018
ms.custom: seodec18
ms.openlocfilehash: 43dd8be998e0f8f3b5a2b783c6a01d5b5ef3da12
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65506926"
---
# <a name="understand-stream-analytics-job-monitoring-and-how-to-monitor-queries"></a>Informatie over Stream Analytics-taak controleren en bewaken van query 's

## <a name="introduction-the-monitor-page"></a>Introductie: De monitor-pagina
De Azure portal beide surface prestatie-metrische gegevens die kunnen worden gebruikt voor bewaking en probleemoplossing van de prestaties van uw query's en -taak. Als u wilt deze metrische gegevens zien, bladert u naar de Stream Analytics-taak u geïnteresseerd bent in de metrische gegevens voor zien en bekijk de **bewaking** sectie op de pagina overzicht.  

![Stream Analytics-taak bewaking van koppeling](./media/stream-analytics-monitoring/02-stream-analytics-monitoring-block.png)

Het venster wordt weergegeven, zoals wordt weergegeven:

![Stream Analytics-taak bewaking dashboard](./media/stream-analytics-monitoring/01-stream-analytics-monitoring.png)  

## <a name="metrics-available-for-stream-analytics"></a>Metrische gegevens beschikbaar voor Stream Analytics
| Gegevens                 | Definitie                               |
| ---------------------- | ---------------------------------------- |
| Invoervelden met achterstand       | Het aantal invoer gebeurtenissen die zijn nog moeten worden uitgevoerd. Een andere waarde dan nul voor deze metrische gegevens geeft aan dat de taak niet kunt houden met het aantal binnenkomende gebeurtenissen. Als deze waarde langzaam toeneemt of consistent dan nul is, moet u uw taak uitschalen. U kunt meer informatie in [begrijpen en aanpassen van Streaming-eenheden](stream-analytics-streaming-unit-consumption.md). |
| Gegevensconversiefouten | Het aantal uitvoergebeurtenissen dat kan niet worden geconverteerd naar het schema van de verwachte uitvoer. Foutbeleid kan worden gewijzigd in 'Drop' laten vallen van gebeurtenissen die in dit scenario optreden. |
| Vroege-invoergebeurtenissen       | Gebeurtenissen waarvan tijdstempel ouder dan Aankomsttijd door meer dan vijf minuten is. |
| Mislukte functieaanvragen | Aantal mislukte functie van Azure Machine Learning-aanroepen (indien aanwezig). |
| Functiegebeurtenissen        | Aantal gebeurtenissen verzonden naar de Azure Machine Learning-functie (indien aanwezig). |
| Functieaanvragen      | Het aantal aanroepen naar de Azure Machine Learning-functie (indien aanwezig). |
| Fouten in invoerdeserialisatie       | Het aantal invoer-gebeurtenissen die kunnen niet worden gedeserialiseerd.  |
| Invoergebeurtenisbytes      | Hoeveelheid gegevens die zijn ontvangen door de Stream Analytics-taak gemaakt in bytes. Dit kan worden gebruikt om te valideren dat gebeurtenissen worden verzonden naar de invoerbron. |
| Invoergebeurtenissen           | Aantal records van de invoer gebeurtenissen gedeserialiseerd. Dit aantal bevat geen binnenkomende gebeurtenissen die in fouten deserialisatie resulteren. |
| Ontvangen invoerbronnen       | Het aantal berichten dat is ontvangen door de taak. Een bericht is voor Event Hub, een enkel EventData. Een bericht is voor de Blob, één blob. Houd er rekening mee dat invoer bronnen worden geteld voor deserialisatie. Als er deserialisatie fouten zijn, kunnen invoerbronnen groter is dan invoergebeurtenissen zijn. Anders kan het zijn kleiner dan of gelijk zijn voor het invoeren van gebeurtenissen, omdat elk bericht meerdere gebeurtenissen kan bevatten. |
| Late invoergebeurtenissen      | Gebeurtenissen die hoger is dan de geconfigureerde laat aankomst tolerantie venster is aangekomen. Meer informatie over [overwegingen bij de volgorde van de Azure Stream Analytics gebeurtenis](stream-analytics-out-of-order-and-late-events.md) . |
| Out-van-Order gebeurtenissen    | Het aantal gebeurtenissen ontvangen buiten de volgorde die zijn verwijderd of een gecorrigeerde timestamp, op basis van het beleid voor het bestellen van gebeurtenis gegeven. Dit kan worden beïnvloed door de configuratie van de instelling van de kant van tolerantie venster. |
| Uitvoergebeurtenis          | De hoeveelheid gegevens die door de Stream Analytics-taak wordt verzonden naar de bestemming voor uitvoer, in aantal gebeurtenissen. |
| Runtimefouten         | Totaal aantal fouten met betrekking tot de verwerking van query's (met uitzondering van fouten gevonden tijdens het opnemen van gebeurtenissen of uitvoeren van resultaten) |
| Gebruikspercentage voor Streaming-eenheden       | Het gebruik van de Streaming-eenheid/eenheden toegewezen aan een taak op het tabblad schalen van de taak. Moet deze indicator 80% bereikt of hierboven, is zeer waarschijnlijk dat de verwerking van gebeurtenissen kan worden vertraagd of geen voortgang meer maakt. |
| Watermerkvertraging       | De maximale watermerk vertraging voor alle partities van alle uitvoer van de taak. |

U kunt deze metrische gegevens voor [bewaken van de prestaties van uw Stream Analytics-taak](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-set-up-alerts#scenarios-to-monitor). 

## <a name="customizing-monitoring-in-the-azure-portal"></a>Bewaking aanpassen in Azure portal
U kunt het type van de grafiek, metrische gegevens die worden weergegeven, aanpassen en tijdsbereik in de instellingen van de grafiek bewerken. Zie voor meer informatie, [over het aanpassen van bewaking](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

  ![Stream Analytics query uitvoeren op het diagram met de monitor](./media/stream-analytics-monitoring/08-stream-analytics-monitoring.png)  


## <a name="latest-output"></a>Laatste uitvoer
Een andere interessante gegevenspunt voor het bewaken van uw taak is de tijd van de laatste uitvoer, die wordt weergegeven op de pagina overzicht.
Dit is de tijd van de toepassing (dat wil zeggen de tijd met behulp van de tijdstempel van de gebeurtenisgegevens) van de meest recente uitvoer van uw taak.

## <a name="get-help"></a>Help opvragen
Voor verdere hulp kunt u mogelijk terecht op het [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Volgende stappen
* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)

