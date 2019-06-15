---
title: Microsoft Dynamics CRM en Azure Application Insights | Microsoft Docs
description: Telemetrie ophalen uit Microsoft Dynamics CRM Online met behulp van Application Insights. Overzicht van de installatie, ophalen van gegevens, visualisatie en exporteren.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 04c66338-687e-49e5-9975-be935f98f156
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 03/16/2018
ms.reviewer: mazhar
ms.author: mbullwin
ms.openlocfilehash: 6119f1116d255f7cd2a2bfc20e86eeca9e5dfe82
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60523283"
---
# <a name="walkthrough-enabling-telemetry-for-microsoft-dynamics-crm-online-using-application-insights"></a>Walkthrough: Telemetrie inschakelen voor Microsoft Dynamics CRM Online met behulp van Application Insights
Dit artikel leest u hoe u aan de telemetriegegevens van [Microsoft Dynamics CRM Online](https://www.dynamics.com/) met behulp van [Azure Application Insights](https://azure.microsoft.com/services/application-insights/). Behandelen we het complete proces van het Application Insights-script toevoegen aan uw toepassing, het vastleggen van gegevens en gegevensvisualisatie.

> [!NOTE]
> [De Voorbeeldoplossing Bladeren](https://dynamicsandappinsights.codeplex.com/).
> 
> 

## <a name="add-application-insights-to-new-or-existing-crm-online-instance"></a>Application Insights toevoegen aan nieuwe of bestaande CRM Online-instantie
Voor het bewaken van uw toepassing, kunt u een Application Insights-SDK toevoegen aan uw toepassing. De SDK telemetrie verzendt naar de [Application Insights-portal](https://portal.azure.com), kunt u onze krachtige analyse- en diagnostische hulpprogramma's gebruiken, of de gegevens naar opslag exporteren.

### <a name="create-an-application-insights-resource-in-azure"></a>Een Application Insights-resource maken in Azure
1. Ophalen [een account in Microsoft Azure](https://azure.com/pricing). 
2. Meld u aan bij de [Azure-portal](https://portal.azure.com) en een nieuwe Application Insights-resource toevoegen. Dit is waar uw gegevens worden verwerkt en weergegeven.

    ![Klik op +, Ontwikkelaarsservices, Application Insights.](./media/sample-mscrm/01.png)

    Kies ASP.NET als het toepassingstype.
3. Volg de instructies voor [verkrijgen van de JavaScript SDK-script voor uw app](../../azure-monitor/app/javascript.md#set-up-application-insights-for-your-web-page), Kopieer het JavaScript-fragment en zorg ervoor dat u de Instrumentatiesleutel vervangen door de juiste waarde voor uw Application Insights-resource.

### <a name="create-a-javascript-web-resource-in-microsoft-dynamics-crm"></a>Een JavaScript-webresource in Microsoft Dynamics CRM maken
1. Open uw Online CRM-exemplaar en meld u aan met administratorbevoegdheden.
2. Open Microsoft Dynamics CRM-instellingen, aanpassingen, aanpassen het systeem

    ![Microsoft Dynamics CRM-instellingen](./media/sample-mscrm/00001.png)

    ![Instellingen > aanpassingen](./media/sample-mscrm/00002.png)

1. Een JavaScript-resource maken.

    ![Dialoogvenster voor een nieuwe Web-Resource](./media/sample-mscrm/07.png)

    Geef deze een naam, selecteer **Script (JScript)** en open de teksteditor.

    ![Open de teksteditor](./media/sample-mscrm/00004.png)
2. Kopieer de code uit de Application Insights JavaScript SDK waarin u de Instrumentatiesleutel voordat hebt geconfigureerd. Zorg dat u script-tags genegeerd tijdens het kopiëren. Raadpleeg de onderstaande schermafbeelding:

    ![Stel de instrumentatiesleutel](./media/sample-mscrm/000005.png)

    De code bevat de instrumentatiesleutel die de Application insights-resource identificeert.
3. Opslaan en publiceren.

    ![Opslaan en publiceren](./media/sample-mscrm/00006.png)

### <a name="instrument-forms"></a>Instrument formulieren
1. Open het formulier Account in Microsoft CRM Online

    ![Accountformulier](./media/sample-mscrm/00007.png)
2. Open de eigenschappen van het formulier

    ![Eigenschappen van formulier](./media/sample-mscrm/00008.png)
3. Toevoegen van de JavaScript-webresource die u hebt gemaakt

    ![Menu Toevoegen](./media/sample-mscrm/13.png)

4. Opslaan en publiceren van uw aanpassingen van formulieren.

## <a name="metrics-captured"></a>Metrische gegevens vastgelegd
U hebt nu telemetrie vastleggen voor het formulier ingesteld. Wanneer deze wordt gebruikt, worden de gegevens worden verzonden naar uw Application Insights-resource.

Hier vindt u voorbeelden van de gegevens die u ziet.

#### <a name="application-health"></a>Status van de toepassing
![Laadtijd voor voorbeeld](./media/sample-mscrm/15.png)

![Voorbeeld pagina weergaven grafiek](./media/sample-mscrm/16.png)

Browseruitzonderingen:

![Grafiek met serveruitzonderingen klikken browser](./media/sample-mscrm/17.png)

Klik op de grafiek om meer details:

![Lijst met uitzonderingen](./media/sample-mscrm/18.png)

#### <a name="usage"></a>Gebruik
![Gebruikers, sessies en paginaweergaven](./media/sample-mscrm/19.png)

![Sessie-grafieken](./media/sample-mscrm/20.png)

![Browserversies](./media/sample-mscrm/21.png)

#### <a name="browsers"></a>Browsers
![Uitsplitsing van de laadtijd van browserpagina](./media/sample-mscrm/22.png)

![Aantal sessies per browserversie](./media/sample-mscrm/23.png)

#### <a name="geolocation"></a>Geolocatie
![Aantal sessies per land](./media/sample-mscrm/24.png)

![Sessies en gebruikers per land](./media/sample-mscrm/25.png)

#### <a name="inside-page-view-request"></a>Aanvraag voor binnen pagina weergeven
![Overzicht van de pagina weergeven](./media/sample-mscrm/26.png)

![Zoeken op pagina-gebeurtenissen weergeven](./media/sample-mscrm/27.png)

![Soortgelijke paginaweergaven](./media/sample-mscrm/28.png)

![Eigenschappen van paginaweergaven](./media/sample-mscrm/29.png)

![Pagina's per sessie](./media/sample-mscrm/30.png)

## <a name="sample-code"></a>Voorbeeldcode
[De voorbeeldcode Bladeren](https://dynamicsandappinsights.codeplex.com/).

## <a name="power-bi"></a>Power BI
U kunt zelfs diepere analyse doen als u [exporteren van gegevens naar Microsoft Power BI](../../azure-monitor/app/export-power-bi.md ).

## <a name="sample-microsoft-dynamics-crm-solution"></a>Voorbeeld van Microsoft Dynamics CRM-oplossing
[Hier volgt de Voorbeeldoplossing geïmplementeerd in Microsoft Dynamics CRM](https://dynamicsandappinsights.codeplex.com/).

## <a name="learn-more"></a>Meer informatie
* [Wat is Application Insights?](../../azure-monitor/app/app-insights-overview.md)
* [Application Insights voor webpagina 's](../../azure-monitor/app/javascript.md)
* [Meer voorbeelden en walkthroughs](../../azure-monitor/app/app-insights-overview.md)
