---
title: Instellen van waarschuwingen voor Azure Stream Analytics-taken bewaken
description: In dit artikel wordt beschreven hoe u Azure portal gebruiken voor het instellen van bewaking en waarschuwingen voor Azure Stream Analytics-taken.
services: stream-analytics
author: jseb225
ms.author: sidram
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/21/2019
ms.openlocfilehash: 26e8e004f917b1c138bc27389cac1cc52672f3d4
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2019
ms.locfileid: "67329862"
---
# <a name="set-up-alerts-for-azure-stream-analytics-jobs"></a>Waarschuwingen instellen voor Azure Stream Analytics-taken

Het is belangrijk voor het bewaken van uw Azure Stream Analytics-taak om te controleren of dat de taak continu wordt uitgevoerd zonder problemen. In dit artikel wordt beschreven hoe u waarschuwingen instellen voor algemene scenario's die moeten worden bewaakt. 

U kunt regels definiëren voor metrische gegevens van gegevens via de portal, logboeken voor bewerkingen, evenals [programmatisch](https://code.msdn.microsoft.com/windowsazure/Receive-Email-Notifications-199e2c9a).

## <a name="set-up-alerts-in-the-azure-portal"></a>Stel waarschuwingen in Azure portal
### <a name="get-alerted-when-a-job-stops-unexpectedly"></a>U gewaarschuwd wanneer een taak onverwacht afgesloten wordt

Het volgende voorbeeld ziet u hoe u waarschuwingen instellen voor wanneer de taak een mislukte status voert. Deze waarschuwing wordt aanbevolen voor alle taken.

1. Open de Stream Analytics-taak die u wilt een waarschuwing maken voor in de Azure-portal.

2. Op de **taak** pagina, gaat u naar de **bewaking** sectie.  

3. Selecteer **metrische gegevens**, en vervolgens **nieuwe waarschuwingsregel**.

   ![Azure portal Stream Analytics waarschuwingen instellen](./media/stream-analytics-set-up-alerts/stream-analytics-set-up-alerts.png)  

4. De naam van uw Stream Analytics-taak moet automatisch worden weergegeven onder **RESOURCE**. Klik op **voorwaarde toevoegen**, en selecteer **alle beheerbewerkingen** onder **signaallogica configureren**.

   ![Selecteer de signaalnaam voor Stream Analytics-waarschuwing](./media/stream-analytics-set-up-alerts/stream-analytics-condition-signal.png)  

5. Onder **signaallogica configureren**, wijzigen **gebeurtenisniveau** naar **alle** en wijzig **Status** naar **mislukt** . Laat **gebeurtenis gestart door** leeg en selecteer **gedaan**.

   ![Signaallogica voor Stream Analytics-waarschuwing configureren](./media/stream-analytics-set-up-alerts/stream-analytics-configure-signal-logic.png) 

6. Selecteer een bestaande actiegroep of maak een nieuwe groep. In dit voorbeeld wordt een nieuwe actiegroep genoemd **TIDashboardGroupActions** is gemaakt met een **e-mailberichten** actie die een e-mailbericht naar gebruikers met verzendt de **eigenaar** Azure-Resource Manager-rol.

   ![Instellen van een waarschuwing voor een Azure stream Analytics-taak](./media/stream-analytics-set-up-alerts/stream-analytics-add-group-email-action.png)

7. De **RESOURCE**, **voorwaarde**, en **ACTIEGROEPEN** krijgen een vermelding. Houd er rekening mee dat in de volgorde voor de waarschuwingen die moeten worden geactiveerd, de gedefinieerde voorwaarden moeten worden voldaan. U kunt bijvoorbeeld het gemiddelde meten van een metrische waarde over de afgelopen 15 minuten, op basis van metingen om de 5 minuten.

   ![Maken van waarschuwingsregel voor Stream Analytics](./media/stream-analytics-set-up-alerts/stream-analytics-create-alert-rule-2.png)

   Toevoegen een **naam waarschuwingsregel**, **beschrijving**, en uw **resourcegroep** naar de **WAARSCHUWINGSDETAILS** en klikt u op **waarschuwing maken regel** om de regel voor uw Stream Analytics-taak te maken.

   ![Maken van waarschuwingsregel voor Stream Analytics](./media/stream-analytics-set-up-alerts/stream-analytics-create-alert-rule.png)
   
## <a name="scenarios-to-monitor"></a>Scenario's om te controleren

De volgende waarschuwingen worden aanbevolen voor het bewaken van de prestaties van uw Stream Analytics-taak. Deze metrische gegevens moet elke minuut worden geëvalueerd ten opzichte van de afgelopen periode van 5 minuten.

|Gegevens|Voorwaarde|Tijdverzameling|Drempelwaarde|Corrigerende maatregelen|
|-|-|-|-|-|
|Gebruikspercentage|Groter dan|Maximum|80|Er zijn meerdere factoren waarmee u de SU-% gebruik te verhogen. U kunt met query-parallellisatie of verhoog het aantal streaming-eenheden. Zie [Query-parallellisatie gebruiken in Azure Stream Analytics](stream-analytics-parallelization.md) voor meer informatie.|
|Runtime-fouten|Groter dan|Totaal|0|Controleren of de activiteit of logboeken met diagnostische gegevens en de benodigde wijzigingen aanbrengen in de invoer, query of uitvoer.|
|Vertraging voor watermerk|Groter dan|Maximum|Gemiddelde waarde van deze metrische gegevens in de afgelopen 15 minuten is als groter dan laat aankomst tolerantie (in seconden). Als u de latere aankomst tolerantie niet hebt gewijzigd, wordt de standaardwaarde is ingesteld op 5 seconden.|Probeer het verhogen van het aantal su's of gebruik van uw query. Zie voor meer informatie over SUs [begrijpen en aanpassen van Streaming-eenheden](stream-analytics-streaming-unit-consumption.md#how-many-sus-are-required-for-a-job). Zie voor meer informatie over het gebruik van uw query [gebruik te maken van query-parallellisatie in Azure Stream Analytics](stream-analytics-parallelization.md).|
|Fouten in invoerdeserialisatie|Groter dan|Totaal|0|Controleren of de activiteit of logboeken met diagnostische gegevens en de benodigde wijzigingen aanbrengen in de invoer. Zie voor meer informatie over diagnostische logboeken [oplossen Azure Stream Analytics met behulp van logboeken met diagnostische gegevens](stream-analytics-job-diagnostic-logs.md)|

## <a name="get-help"></a>Help opvragen

Zie voor meer informatie over het configureren van waarschuwingen in Azure portal [meldingen van waarschuwingen ontvangen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).  

Voor verdere ondersteuning kunt u proberen onze [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Volgende stappen
* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)

