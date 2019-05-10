---
title: Azure Log Analytics-gegevens importeren in Power BI | Microsoft Docs
description: Power BI is een cloudgebaseerde business analytics-service van Microsoft waarmee uitgebreide visualisaties en rapporten voor analyse van verschillende gegevenssets.  Dit artikel wordt beschreven hoe u configureert en Log Analytics-gegevens importeren in Power BI en configureert om automatisch te vernieuwen.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 83edc411-6886-4de1-aadd-33982147b9c3
ms.service: log-analytics
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/01/219
ms.author: bwren
ms.openlocfilehash: 2db6ddf57802f6fcf38cfc3ad7094ed94eaca3d8
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2019
ms.locfileid: "65234195"
---
# <a name="import-azure-monitor-log-data-into-power-bi"></a>Azure Monitor log-gegevens importeren in Power BI


[Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/) is een cloud op basis van business analytics-service van Microsoft waarmee uitgebreide visualisaties en rapporten voor analyse van verschillende gegevenssets.  U kunt de resultaten van een query voor Azure Monitor importeren in een Power BI-gegevensset, zodat u van de functies profiteren kunt, zoals het combineren van gegevens uit verschillende bronnen en delen van rapporten op het web en mobiele apparaten.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="overview"></a>Overzicht
Voor het importeren van gegevens uit een [Log Analytics-werkruimte](manage-access.md) in Azure Monitor in Power BI, maakt u een gegevensset in Power BI op basis van een [logboekquery](../log-query/log-query-overview.md) in Azure Monitor.  De query wordt uitgevoerd telkens wanneer die de gegevensset wordt vernieuwd.  U kunt vervolgens Power BI-rapporten waarin gegevens uit de gegevensset maken.  Voor het maken van de gegevensset in Power BI, u uw query exporteren vanuit Log Analytics [Power Query (M) taal](https://msdn.microsoft.com/library/mt807488.aspx).  U dit vervolgens gebruiken om een query in Power BI Desktop en vervolgens publiceren naar Power BI als een gegevensset.  De details van dit proces worden hieronder beschreven.

![Log Analytics met Power BI](media/powerbi/overview.png)

## <a name="export-query"></a>Query exporteren
Maak eerst een [logboekquery](../log-query/log-query-overview.md) die de gegevens retourneert waarop u wilt vullen van de Power BI-gegevensset.  U deze query te exporteren [Power Query (M) taal](https://msdn.microsoft.com/library/mt807488.aspx) die kan worden gebruikt door Power BI Desktop.

1. [De logboekquery gemaakt in Log Analytics](../log-query/get-started-portal.md) om op te halen van de gegevens voor uw gegevensset.
2. Selecteer **exporteren** > **Power BI-Query (M)**.  Hiermee exporteert u de query naar een tekstbestand met de naam **PowerBIQuery.txt**. 

    ![Zoeken in Logboeken exporteren](media/powerbi/export-analytics.png)

3. Open het bestand en kopieer de inhoud.

## <a name="import-query-into-power-bi-desktop"></a>Query importeren in Power BI Desktop
Power BI Desktop is een bureaubladtoepassing die u kunt gegevenssets en rapporten die kunnen worden gepubliceerd naar Power BI maken.  U kunt deze ook gebruiken om een query met behulp van de Power Query-taal geëxporteerd uit Azure Monitor te maken. 

1. Installeer [Power BI Desktop](https://powerbi.microsoft.com/desktop/) als u niet al op uw computer en vervolgens de toepassing open.
2. Selecteer **gegevens ophalen** > **lege Query** om een nieuwe query te openen.  Selecteer vervolgens **geavanceerde Editor** en plak de inhoud van het geëxporteerde bestand in de query. Klik op **Gereed**.

    ![Power BI Desktop query](media/powerbi/desktop-new-query.png)

5. De query wordt uitgevoerd, en de resultaten worden weergegeven.  U mogelijk gevraagd om referenties voor het verbinding maken met Azure.  
6. Typ een beschrijvende naam voor de query.  De standaardwaarde is **Query1**. Klik op **sluiten en toepassen** om toe te voegen van de gegevensset naar het rapport.

    ![Naam van de Power BI Desktop](media/powerbi/desktop-results.png)



## <a name="publish-to-power-bi"></a>Publiceren naar Power BI
Wanneer u naar Power BI publiceert, wordt een gegevensset en een rapport worden gemaakt.  Als u een rapport in Power BI Desktop maakt, zal klikt u vervolgens dit worden gepubliceerd met uw gegevens.  Als dat niet het geval is, klikt u vervolgens een leeg rapport wordt gemaakt.  U kunt het rapport in Power BI wijzigen of een nieuwe maken op basis van de gegevensset.

1. Een rapport op basis van uw gegevens maken.  Gebruik [Power BI Desktop-documentatie](https://docs.microsoft.com/power-bi/desktop-report-view) als u niet bekend met het bent.  
1. Wanneer u klaar bent om dit te verzenden naar Power BI, klikt u op **publiceren**.  
1. Wanneer u hierom wordt gevraagd, moet u een doel selecteren in uw Power BI-account.  Tenzij u een specifieke bestemming in gedachten hebt, gebruikt u **mijn werkruimte**.

    ![Power BI Desktop publiceren](media/powerbi/desktop-publish.png)

1. Wanneer het publiceren is voltooid, klikt u op **openen in Power BI** Power BI openen met uw nieuwe gegevensset.


### <a name="configure-scheduled-refresh"></a>Geplande vernieuwing configureren
De gegevensset gemaakt in Power BI heeft dezelfde gegevens die u eerder hebt gezien in Power BI Desktop.  U moet de gegevensset om de query opnieuw uitvoeren en deze vullen met de meest recente gegevens uit Azure Monitor periodiek te vernieuwen.  

1. Klik op de werkruimte waar u uw rapport en selecteer geüpload de **gegevenssets** menu. 
1. Selecteer het snelmenu naast uw nieuwe gegevensset en selecteer **instellingen**. 
1. Onder **gegevensbronreferenties** hebt u een bericht dat de referenties ongeldig zijn.  Dit is omdat u referenties voor de gegevensset moet worden gebruikt wanneer het wordt vernieuwd met de gegevens nog niet hebt opgegeven.  
1. Klik op **referenties bewerken** en geef de referenties die toegang hebben tot de Log Analytics-werkruimte in Azure Monitor. Als u tweeledige verificatie vereist, selecteert u **OAuth2** voor de **verificatiemethode** aan te melden met uw referenties wordt gevraagd.

    ![Power BI-schema](media/powerbi/powerbi-schedule.png)

5. Onder **geplande vernieuwing** inschakelen op de optie voor het **uw gegevens up-to-date te houden**.  U kunt desgewenst wijzigen de **vernieuwingsfrequentie** en een of meer specifieke tijdstippen om uit te voeren van het vernieuwen.

    ![Power BI vernieuwen](media/powerbi/powerbi-schedule-refresh.png)



## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [zoekopdrachten](../log-query/log-query-overview.md) naar query's opbouwen die kunnen worden geëxporteerd naar Power BI.
* Meer informatie over [Power BI](https://powerbi.microsoft.com) te maken van visualisaties op basis van Azure Monitor log-uitvoer.