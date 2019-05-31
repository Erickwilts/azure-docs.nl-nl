---
title: Azure Time Series Insights Preview use-cases | Microsoft Docs
description: Krijg inzicht in Azure Time Series Insights Preview van use cases.
author: ashannon7
ms.author: dpalled
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 04/30/2019
ms.custom: seodec18
ms.openlocfilehash: 787445d5186a173b2cba674b36cd95879cc863e5
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66390001"
---
# <a name="azure-time-series-insights-preview-use-cases"></a>Azure Time Series Insights Preview use-cases

In dit artikel bevat een overzicht van enkele algemene scenario's voor de Azure Time Series Insights Preview. De aanbevelingen in dit artikel fungeren als een beginpunt voor het ontwikkelen van uw toepassingen en oplossingen met Time Series Insights.

In dit artikel worden met name de volgende vragen:

* Wat zijn de algemene gebruiksvoorbeelden voor Time Series Insights?
* Wat zijn de voordelen van het gebruik van Time Series Insights voor [gegevensverkenning en visuele anomaliedetectie](#data-exploration-and-visual-anomaly-detection)?
* Wat zijn de voordelen van het gebruik van Time Series Insights voor [operationele analyses en efficiëntie van processen](#operational-analysis-and-driving-process-efficiency)?
* Wat zijn de voordelen van het gebruik van Time Series Insights voor [advanced analytics](#advanced-analytics)?

Een overzicht van deze scenario's gebruiken in de volgende secties wordt beschreven.

## <a name="introduction"></a>Inleiding

Azure Time Series Insights is een end-to-end platform-as-a-service-aanbieding. Het wordt gebruikt voor het verzamelen, verwerken, opslaan, analyseren en maximaal contextualized, time series geoptimaliseerd IoT-schaal gegevens op te vragen. Time Series Insights is ideaal voor ad-hoc gegevens verkennen en operationele analyse. Time Series Insights is een unieke uitbreidbare, aangepaste serviceoplossing voldoet aan de algemene behoeften van industriële IoT-implementaties.

## <a name="data-exploration-and-visual-anomaly-detection"></a>Gegevensverkenning en visuele afwijkingsdetectie

Verken en analyseer direct miljarden gebeurtenissen om afwijkingen te vinden en verborgen trends in uw gegevens te ontdekken. Time Series Insights biedt near realtime-prestaties voor uw IoT- en DevOps-analyseworkloads.

[![Data explorer](media/v2-update-use-cases/data-explorer.svg)](media/v2-update-use-cases/data-explorer.svg#lightbox)

De meeste klanten gaat ermee akkoord dat de tijd voor inzichten tussen de sterkste activa van Time Series Insights is. Time Series Insights vereist geen voorbereiding van gegevens. Het werkt als u verbinding maken met miljarden gebeurtenissen in uw Azure IoT Hub of Azure Event Hubs in minuten snel wilt. Eenmaal verbinding hebben, kunt u visualiseren en analyseren van miljarden gebeurtenissen die moeten worden afwijkingen te vinden en verborgen trends te ontdekken in uw gegevens.

Time Series Insights is gemakkelijk en eenvoudig te gebruiken. U kunt met uw gegevens werken zonder één regel code te schrijven. Er is ook geen nieuwe taal te leren. Time Series Insights biedt nauwkeurige opvragen op basis van tekst voor gevorderde gebruikers die bekend met SQL zijn. Het biedt ook de select-en-klik-mogelijkheid voor beginners.

Klanten profiteren van de snelheid voor het vaststellen van problemen met betrekking tot asset snel. Ze kunnen DevOps ophalen om de hoofdoorzaak van een bug in een IoT-oplossing te uitvoeren. Ze kunnen ook gebieden te onderzoeken voor data science initiatieven identificeren.  

Er zijn drie primaire manieren om te communiceren met gegevens die zijn opgeslagen in Time Series Insights:

- De eerste en eenvoudigste manier om te beginnen is met de Verkenner van Time Series Insights Preview. U kunt deze gebruiken voor het snel visualiseren van al uw IoT-gegevens op één plek. Het biedt hulpprogramma's zoals de heatmap om u te helpen afwijkingen in uw gegevens te vinden. Het biedt ook de perspectiefweergave van een. Gebruik dit voor het vergelijken van maximaal vier weergaven uit een of meer Time Series Insights-omgevingen in één dashboard. Het dashboard geeft u een overzicht van uw tijdseriegegevens op al uw locaties. Meer informatie over de [Verkenner van Time Series Insights Preview](./time-series-insights-update-explorer.md). Als u van plan bent om uw Time Series Insights-omgeving, wilt lezen [Time Series Insights planning](./time-series-insights-update-plan.md).

- De tweede manier om te beginnen is met de JavaScript SDK snel krachtige grafieken en diagrammen insluiten in uw webtoepassing. Met slechts een paar regels code, kunt u krachtige query's maken. Ze gebruiken voor het vullen van lijndiagrammen, cirkeldiagrammen, staafdiagrammen, heatmaps beschikbaar zijn, gegevensrasters en meer. Al deze elementen bestaat out-of-the-box met behulp van de SDK. De SDK isoleert ook Time Series Insights-query's. U kunt deze SQL-achtige predicaten query uitvoeren op de gegevens die u wilt weergeven in een dashboard maken. Time Series Insights biedt geparameteriseerde URL's voor hybride presentatielaag oplossingen. Ze bieden punten voor naadloze verbinding met de Verkenner van Time Series Insights Preview verdiepen in gegevens.

    * Lees de [Time Series Insights JS-clientbibliotheek](tutorial-explore-js-client-lib.md) en de [Time Series Insights client](https://github.com/Microsoft/tsiclient) documentatie voor meer informatie over de JavaScript-SDK.

    * Meer informatie over het delen van URL's en de nieuwe gebruikersinterface aan de hand [visualiseren van gegevens in de Verkenner van Azure Time Series Insights Preview](time-series-insights-update-explorer.md).

- De derde manier om te beginnen is met de krachtige API's om gegevens te doorzoeken in Time Series Insights opgeslagen. Time Series Insights is een tijdelijke operators zoals `from`, `to`, `first`, en `last`. Deze aggregaties en transformaties bijvoorbeeld heeft `average`, `min`, `max`, `split by`, `order by`, en `DateHistogram`. Het bevat ook filteren operators zoals `has`, `in`, `and`, `or`, `greater than`, en `REGEX`. Alle deze operators kunnen downstream toepassingen snel interessante trends en patronen in uw gegevens kunt vinden. Ze gebruiken voor het vullen van zelfgemaakte visualisaties aan afwijkingen te vinden.

## <a name="operational-analysis-and-driving-process-efficiency"></a>Operationele analyse en efficiëntere processen

Time Series Insights gebruiken voor het bewaken van de status, het gebruik en de prestaties van apparaten op schaal. Time Series Insights biedt een eenvoudige manier voor het meten van de operationele efficiëntie. Time Series Insights helpt bij het beheer van uiteenlopende en onvoorspelbare IoT-workloads, zonder de opname- en queryprestaties te verminderen.

[![Overzicht](media/v2-update-use-cases/overview.svg)](media/v2-update-use-cases/overview.svg#lightbox)

Streaming en continue verwerking van gegevens die afkomstig zijn van operationele processen met succes een gedaanteverwisseling elk bedrijf als deze wordt gecombineerd met de juiste technologie of de oplossing. Deze oplossingen zijn vaak een combinatie van meerdere systemen. Hiermee kunt verkennen en analyseren van gegevens die verandert voortdurend, met name in de realm IoT en deelt een algemene patroon.

Deze patronen beginnen vaak met IoT-platformen die miljarden gebeurtenissen van apparaten en sensoren met betrekking verschillende landinstellingen tot opnemen. Deze systemen verwerken en analyseren van streaming-gegevens als u wilt afleiden van realtime inzichten en acties. Gegevens worden meestal gearchiveerd om te oefenen en koude opslag voor bijna realtime en batch-analyses.

Gegevens die worden verzameld, verloopt via een reeks verwerking te reinigen en deze context kunnen worden geplaatst voor downstream scenario's voor query's en analyses. Azure biedt uitgebreide services die kunnen worden toegepast op IoT-scenario's zoals asset onderhoud en productie. Deze services bevatten Time Series Insights, IoT-Hub, Event Hubs, Azure Stream Analytics, Azure Functions, Azure Logic Apps, Azure Databricks, Azure Machine Learning en Power BI.

Architectuur voor een oplossing kan worden bereikt op de volgende manier:

- Opname van gegevens via de IoT-Hub of Event Hubs voor de best mogelijke beveiliging, doorvoer en latentie.
- Gegevensverwerking en -berekeningen uitvoeren. Trechter opgenomen gegevens via services zoals Stream Analytics, Logic Apps en Azure Functions. De service gebruikt, is afhankelijk van de specifieke vereisten voor de verwerking van gegevens.
- Berekende signalen van de verwerkings-pipeline worden gepusht naar Time Series Insights voor het opslaan en analytics.

Time Series Insights biedt in de buurt van real-time gegevens verkennen en inzichten op basis van een asset historische gegevens. Afhankelijk van de behoeften van uw bedrijf kunnen MapReduce en Hive-taken uitvoeren op gegevens die zijn opgeslagen in Time Series Insights door Time Series Insights verbinding te maken met Azure HDInsight. Gegevens die zijn opgeslagen in Time Series Insights is beschikbaar voor Power BI en andere toepassingen van klanten via de Time Series Insights openbare surface-query's. Deze gegevens kunnen worden gebruikt voor uitvoerige zakelijke en operationele intelligentie scenario's.

## <a name="advanced-analytics"></a>Geavanceerde analyse

Integreren met geavanceerde analyseservices zoals Machine Learning en Azure Databricks. Time Series Insights ingresses onbewerkte gegevens van miljoenen apparaten. Contextuele gegevens die naadloos kan worden gebruikt door een reeks Azure analytics-services worden toegevoegd.

[![Analytics](media/v2-update-use-cases/advanced-analytics.svg)](media/v2-update-use-cases/advanced-analytics.svg#lightbox)

Geavanceerde analyses en machine learning gebruiken en verwerken van grote hoeveelheden gegevens. Deze gegevens worden gebruikt op gegevens gebaseerde beslissingen te nemen en voorspellende analyses uitvoeren. In gevallen IoT leert algoritmen voor geavanceerde analyses van de gegevens die worden verzameld van miljoenen apparaten. Deze apparaten verzenden gegevens meerdere keren per seconde. De gegevens die worden verzameld van IoT-apparaten is onbewerkte. Contextuele informatie zoals de locatie van het apparaat en de eenheid van de meting sensor ontbreekt. Onbewerkte gegevens is als gevolg hiervan moeilijk te gebruiken voor geavanceerde analyses.

Time Series Insights een brug tussen IoT-gegevens en geavanceerde analyses in twee eenvoudige en voordelige manieren:

- Time Series Insights verzamelt eerst, onbewerkte telemetriegegevens van miljoenen apparaten met behulp van IoT Hub. Het verrijkt gegevens met contextuele informatie en gegevens worden getransformeerd naar een parquet-indeling. Deze indeling kan eenvoudig integreren met andere geavanceerde analyses services, zoals Machine Learning, Azure Databricks en toepassingen van derden.

    Time Series Insights kan fungeren als de bron is voor alle gegevens in een organisatie. Een centrale opslagplaats voor downstream analytics maakt workloads om te gebruiken. Omdat Time Series Insights is een bijna realtime storage-service, geavanceerde analyses modellen kunnen continu leren van binnenkomende telemetriegegevens van IoT. Als gevolg hiervan is het zo dat de modellen nauwkeurigere voorspellingen kunnen doen.

- Time Series Insights kan vervolgens de uitvoer van machine learning- en voorspellingsgegevens modellen om te visualiseren en opslaan van de resultaten ingevoerd. Deze procedure helpt organisaties bij om te optimaliseren en hun modellen aanpassen. Time Series Insights is het eenvoudig is om te visualiseren telemetrische gegevens op het vlak van dezelfde streaming als uitvoer van het getrainde model. Op deze manier kunnen de data scienceteams afwijkingen te vinden en patronen te identificeren.  

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de [Verkenner van Time Series Insights Preview](./time-series-insights-update-explorer.md).
- Lezen [Time Series Insights Preview planning](./time-series-insights-update-plan.md) voor het plannen van uw omgeving.
- Lees de [Time Series Insights client](https://github.com/Microsoft/tsiclient) documentatie.
