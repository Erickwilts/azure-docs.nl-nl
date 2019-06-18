---
title: HockeyApp-gegevens in Azure Application Insights verkennen | Microsoft Docs
description: Analyseer gebruik en prestaties van uw Azure-app met Application Insights.
services: application-insights
documentationcenter: windows
author: mrbullwinkle
manager: carmonm
ms.assetid: 97783cc6-67d6-465f-9926-cb9821f4176e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 03/30/2017
ms.author: mbullwin
ms.openlocfilehash: 79adfbfde25903bfe92c94507071c9d0fe303ef1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60898682"
---
# <a name="exploring-hockeyapp-data-in-application-insights"></a>HockeyApp-gegevens in Application Insights verkennen

> [!NOTE]
> HockeyApp is niet meer beschikbaar voor nieuwe toepassingen. Bestaande implementaties van HockeyApp blijven werken. Visual Studio App Center is nu de aanbevolen service van Microsoft voor het bewaken van de nieuwe mobiele apps. [Meer informatie over het instellen van uw apps met App Center en Application Insights](../../azure-monitor/learn/mobile-center-quickstart.md).

[HockeyApp](https://azure.microsoft.com/services/hockeyapp/) is een service voor het bewaken van live-desktop- en mobiele apps. U kunt van HockeyApp, aangepaste verzenden en traceren van telemetrie naar-gebruik controleren en te helpen bij het diagnoseproces (naast het ophalen van crashgegevens). Deze stroom van telemetrie kan worden opgevraagd met de krachtige [Analytics](../../azure-monitor/app/analytics.md) functie van [Azure Application Insights](../../azure-monitor/app/app-insights-overview.md). Bovendien kunt u [exporteren van de aangepaste en traceren van telemetrie](export-telemetry.md). Als u wilt deze functies inschakelt, moet u een brug die aangepaste HockeyApp-gegevens naar Application Insights doorstuurt instellen.

## <a name="the-hockeyapp-bridge-app"></a>De HockeyApp-Bridge-app
De HockeyApp-Bridge-App is de kern-functie waarmee u uw aangepaste HockeyApp en in Application Insights-tracetelemetrie via de functies voor analyse en continue Export. Aangepaste en tracering gebeurtenissen die worden verzameld door HockeyApp na het maken van de HockeyApp-Bridge-App toegankelijk is van deze functies. Laten we zien hoe u voor het instellen van een van deze brug-Apps.

Open in HockeyApp, accountinstellingen, [API-Tokens](https://rink.hockeyapp.net/manage/auth_tokens). Een nieuw token maken of een bestaande resourcegroep gebruiken. De minimale rechten vereist zijn 'alleen-lezen'. Neemt een kopie van de API-token.

![Een HockeyApp-API-token ophalen](./media/hockeyapp-bridge-app/01.png)

Open de Microsoft Azure portal en [maken van een Application Insights-resource](../../azure-monitor/app/create-new-resource.md ). Toepassingstype ingesteld op 'HockeyApp bridge-toepassing':

![Nieuwe Application Insights-resource](./media/hockeyapp-bridge-app/02.png)

U hoeft om in te stellen van een naam - dit wordt automatisch ingesteld vanuit de HockeyApp-naam.

De HockeyApp-bridge-velden worden weergegeven. 

![Brug velden invoeren](./media/hockeyapp-bridge-app/03.png)

Voer de HockeyApp-token dat u eerder hebt genoteerd. Deze actie wordt de 'HockeyApp-toepassing' in het vervolgkeuzemenu met al uw HockeyApp-toepassingen. Selecteer de gewenste en voltooi de rest van de velden. 

Open de nieuwe resource. 

Houd er rekening mee dat de gegevens, enige tijd duren om te beginnen stromen.

![Application Insights-resource wachten op gegevens](./media/hockeyapp-bridge-app/04.png)

Dat is alles. Aangepaste en gegevens traceren die zijn verzameld in uw app HockeyApp geïnstrumenteerd vanaf dit punt is nu ook beschikbaar in de functies voor analyse en continue Export van Application Insights.

Laten we even Bekijk elk van deze functies nu beschikbaar voor u.

## <a name="analytics"></a>Analyse
Analytics is een krachtig hulpprogramma voor ad-hoc uitvoeren van query's van uw gegevens, zodat u kunt vaststellen en uw telemetrie analyseren en snel hoofdoorzaken en patronen te ontdekken.

![Analyse](./media/hockeyapp-bridge-app/05.png)

* [Meer informatie over Analytics](../../azure-monitor/log-query/get-started-portal.md)

## <a name="continuous-export"></a>Continue export
Continue Export kunt u uw gegevens exporteren naar een Azure Blob Storage-container. Dit is zeer nuttig als u nodig hebt voor uw gegevens langer duurt dan de bewaarperiode liggen die momenteel worden aangeboden door Application Insights. U kunt houden de gegevens in blob-opslag, in een SQL-Database, of uw favoriete oplossing voor gegevensopslag te verwerken.

[Meer informatie over de continue Export](export-telemetry.md)

## <a name="next-steps"></a>Volgende stappen
* [Analyse van toepassing op uw gegevens](../../azure-monitor/log-query/get-started-portal.md)

