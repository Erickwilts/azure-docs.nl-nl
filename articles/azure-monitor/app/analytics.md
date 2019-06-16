---
title: Analytics - de krachtige zoek- en query-hulpprogramma van Azure Application Insights | Microsoft Docs
description: 'Overzicht van analyse, het hulpprogramma krachtige diagnostische gegevens doorzoeken van Application Insights. '
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 0a2f6011-5bcf-47b7-8450-40f284274b24
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 02/02/2019
ms.author: mbullwin
ms.openlocfilehash: f5819194e7967b5921f34223cad299752460de30
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66255645"
---
# <a name="analytics-in-application-insights"></a>Analyses in Application Insights
Analytics is het krachtig hulpprogramma voor zoekopdrachten en query's van [Application Insights](app-insights-overview.md). Analytics is een web-hulpprogramma, zodat er geen installatie vereist is.
Als u Application Insights al hebt geconfigureerd voor een van uw apps kunt u de gegevens van uw app analyseren door te Analytics openen vanuit de blade overzicht van uw app.

![Portal.azure.com Open, open uw Application Insights-resource en klik op Analytics.](./media/analytics/001.png)

U kunt ook de [Analytics Speelplaats](https://go.microsoft.com/fwlink/?linkid=859557) dit is een gratis demo-omgeving met een groot aantal voorbeeldgegevens.

## <a name="relation-to-azure-monitor-logs"></a>De relatie met Azure Monitor-Logboeken
Application Insights analytics is gebaseerd op [Azure Data Explorer](/azure/data-explorer) zoals Azure Monitor-logboeken en maakt ook gebruik van de [Kusto-querytaal](/azure/kusto/query). Het maakt gebruik van dezelfde [log analytics-portal](../log-query/get-started-portal.md) zoals Azure Monitor zich aanmeldt, hoewel de gegevens worden opgeslagen in een afzonderlijke partitie.

U niet rechtstreeks toegang tot gegevens in een Log Analytics-werkruimte van Application Insights analytics, en ook kunt u rechtstreeks toegang tot toepassingsgegevens van log analytics. Schrijven van query's op beide sets met gegevens samen, een [query's in log analytics](../log-query/log-query-overview.md) en het gebruik de [app() expressie](../log-query/app-expression.md) voor het openen van toepassing.


## <a name="query-data-in-analytics"></a>Query uitvoeren op gegevens in Analytics
Een typische query begint met een tabelnaam wordt opgegeven gevolgd door een reeks *operators* gescheiden door `|`.
Bijvoorbeeld, we willen weten hoeveel onze app heeft ontvangen van andere landen/regio's tijdens de afgelopen 3 uur aanvragen:
```AIQL
requests
| where timestamp > ago(3h)
| summarize count() by client_CountryOrRegion
| render piechart
```

We beginnen met de naam van de tabel *aanvragen* en toe te voegen elementen doorgesluisd naar behoefte.  Eerst definiëren we een tijdfilter om te controleren van alleen de records van de afgelopen 3 uur.
We telt het aantal records per land (of gegevens die worden gevonden in de kolom *client_CountryOrRegion*). We weergegeven ten slotte de resultaten in een cirkeldiagram.
<br>

![Queryresultaten](./media/analytics/030.png)

De taal heeft veel voordelen bieden:

* [Filter](/azure/kusto/query/whereoperator) uw onbewerkte app-telemetrie met velden, met inbegrip van uw aangepaste eigenschappen en metrische gegevens.
* [Deelnemen aan](/azure/kusto/query/joinoperator) meerdere tabellen - te correleren met paginaweergaven, afhankelijkheidsaanroepen, uitzonderingen en logboektraceringen aanvragen.
* Krachtige statistische [aggregaties](/azure/kusto/query/summarizeoperator).
* Direct en krachtige visualisaties.
* [REST-API](https://dev.applicationinsights.io/) waarmee u kunt query's uitvoeren via een programma, bijvoorbeeld vanuit PowerShell.

De [Naslaggids voor volledige](https://go.microsoft.com/fwlink/?linkid=856079) details van elke opdracht die wordt ondersteund en regelmatig bijgewerkt.

## <a name="next-steps"></a>Volgende stappen
* Aan de slag met de [Analytics-portal](https://go.microsoft.com/fwlink/?linkid=856587)
* Aan de slag [schrijven van query's](https://go.microsoft.com/fwlink/?linkid=856078)
* Controleer de [overzichtskaart van SQL-gebruikers](https://aka.ms/sql-analytics) voor vertalingen van de meest voorkomende idioms.
* Test drive Analytics op onze [Speelplaats](https://analytics.applicationinsights.io/demo) als uw app wordt niet van gegevens naar Application Insights nog verzenden.
* Bekijk de [inleidende video](https://applicationanalytics-media.azureedge.net/home_page_video.mp4).
