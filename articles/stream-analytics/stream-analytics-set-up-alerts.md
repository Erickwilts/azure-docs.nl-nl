---
title: Bewakings waarschuwingen instellen voor Azure Stream Analytics taken
description: In dit artikel wordt beschreven hoe u de Azure Portal gebruikt voor het instellen van controle en waarschuwingen voor Azure Stream Analytics taken.
author: jseb225
ms.author: sidram
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: how-to
ms.date: 06/21/2019
ms.openlocfilehash: d701a518dfa0bd21b33aa2f13f533ad6ef49430d
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2020
ms.locfileid: "86037814"
---
# <a name="set-up-alerts-for-azure-stream-analytics-jobs"></a>Waarschuwingen instellen voor Azure Stream Analytics taken

Het is belang rijk om uw Azure Stream Analytics-taak te controleren om ervoor te zorgen dat de taak voortdurend zonder problemen wordt uitgevoerd. In dit artikel wordt beschreven hoe u waarschuwingen instelt voor algemene scenario's die moeten worden bewaakt. 

U kunt regels voor metrische gegevens van de bewerkings Logboeken definiëren via de portal en [programmatisch](https://code.msdn.microsoft.com/windowsazure/Receive-Email-Notifications-199e2c9a).

## <a name="set-up-alerts-in-the-azure-portal"></a>Waarschuwingen instellen in de Azure Portal
### <a name="get-alerted-when-a-job-stops-unexpectedly"></a>Ontvang een waarschuwing wanneer een taak onverwacht stopt

In het volgende voor beeld ziet u hoe u waarschuwingen instelt voor wanneer de status van de taak is mislukt. Deze waarschuwing wordt aanbevolen voor alle taken.

1. Open in de Azure Portal de Stream Analytics taak waarvoor u een waarschuwing wilt maken.

2. Ga op de pagina **taak** naar het gedeelte **bewaking** .  

3. Selecteer **metrische gegevens**en vervolgens **nieuwe waarschuwings regel**.

   ![Setup van Azure Portal Stream Analytics-waarschuwingen](./media/stream-analytics-set-up-alerts/stream-analytics-set-up-alerts.png)  

4. De naam van uw Stream Analytics-taak moet automatisch worden weer gegeven onder **resource**. Klik op **voor waarde toevoegen**en selecteer **alle beheer bewerkingen** onder **signaal logica configureren**.

   ![Signaal naam voor Stream Analytics waarschuwing selecteren](./media/stream-analytics-set-up-alerts/stream-analytics-condition-signal.png)  

5. Onder **signaal logica configureren**wijzigt u **het gebeurtenis niveau** in **Alles** en wijzigt u de **status** in **mislukt**. Laat de **gebeurtenis** leeg en selecteer **gereed**.

   ![Signaal logica voor Stream Analytics waarschuwing configureren](./media/stream-analytics-set-up-alerts/stream-analytics-configure-signal-logic.png) 

6. Selecteer een bestaande actie groep of maak een nieuwe groep. In dit voor beeld is een nieuwe actie groep met de naam **TIDashboardGroupActions** gemaakt met een actie e-mail **berichten** waarmee een e-mail wordt verzonden aan gebruikers met de rol van **eigenaar** Azure Resource Manager.

   ![Een waarschuwing instellen voor een Azure streaming Analytics-taak](./media/stream-analytics-set-up-alerts/stream-analytics-add-group-email-action.png)

7. De **resource**, **voor waarde**en **actie groepen** moeten elk een vermelding bevatten. Houd er rekening mee dat de gedefinieerde voor waarden moeten worden vervuld om de waarschuwingen te starten. U kunt bijvoorbeeld het gemiddelde meten van een metrische waarde over de afgelopen 15 minuten, op basis van metingen om de 5 minuten.

   ![Waarschuwings regel voor Stream Analytics maken](./media/stream-analytics-set-up-alerts/stream-analytics-create-alert-rule-2.png)

   Voeg een **waarschuwings regel naam**, **Beschrijving**en de **resource groep** toe aan de **waarschuwings Details** en klik op **waarschuwings regel maken** om de regel voor uw stream Analytics-taak te maken.

   ![Waarschuwings regel voor Stream Analytics maken](./media/stream-analytics-set-up-alerts/stream-analytics-create-alert-rule.png)
   
## <a name="scenarios-to-monitor"></a>Te bewaken scenario's

De volgende waarschuwingen worden aanbevolen voor het bewaken van de prestaties van uw Stream Analytics-taak. Deze metrische gegevens moeten elke minuut worden geëvalueerd gedurende de laatste periode van vijf minuten.

|Gegevens|Voorwaarde|Tijd aggregatie|Drempelwaarde|Corrigerende maat regelen|
|-|-|-|-|-|
|% Gebruik|Groter dan|Maximum|80|Er zijn meerdere factoren die het gebruik van SU% verhogen. U kunt schalen met query parallel Lise ring of het aantal streaming-eenheden verhogen. Zie [Query-parallellisatie gebruiken in Azure Stream Analytics](stream-analytics-parallelization.md) voor meer informatie.|
|Runtime-fouten|Groter dan|Totaal|0|Bekijk de activiteiten of de logboeken van de resource en breng de gewenste wijzigingen aan in de invoer, query of uitvoer.|
|Watermerk vertraging|Groter dan|Maximum|Wanneer de gemiddelde waarde van deze metriek over de laatste 15 minuten groter is dan de tolerantie voor de latere aankomst (in seconden). Als u de tolerantie voor late aankomst niet hebt gewijzigd, wordt de standaard waarde 5 seconden ingesteld.|Verhoog het aantal SUs of gelijktijdig uw query. Zie over het [begrijpen en aanpassen van streaming-eenheden](stream-analytics-streaming-unit-consumption.md#how-many-sus-are-required-for-a-job)voor meer informatie over SUs. Zie gelijktijdig [query parallel Lise ring in azure stream Analytics](stream-analytics-parallelization.md)voor meer informatie over het uitvoeren van uw query.|
|Fouten bij het deserialiseren van de invoer|Groter dan|Totaal|0|Bekijk de activiteiten of de logboeken van de resource en breng de gewenste wijzigingen aan in de invoer. Zie [problemen oplossen Azure stream Analytics met resource logboeken](stream-analytics-job-diagnostic-logs.md) voor meer informatie over bron logboeken|

## <a name="get-help"></a>Hulp vragen

Zie [waarschuwings meldingen ontvangen](../azure-monitor/platform/alerts-overview.md)voor meer informatie over het configureren van waarschuwingen in de Azure Portal.  

Probeer voor meer hulp onze [micro soft Q&een vraag pagina voor Azure stream Analytics](https://docs.microsoft.com/answers/topics/azure-stream-analytics.html).

## <a name="next-steps"></a>Volgende stappen
* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor Azure Stream Analytics Query](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)

