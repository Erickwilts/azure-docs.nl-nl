---
title: Wat is?
titleSuffix: Azure Machine Learning Studio
description: Azure Machine Learning Studio is een hulpprogramma waarmee u met slepen en neerzetten snel modellen kunt ontwikkelen met behulp van een kant-en-klare bibliotheek van algoritmen en modules.
services: machine-learning
documentationcenter: ''
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: seodec18
ms.assetid: e65c8fe1-7991-4a2a-86ef-fd80a7a06269
ms.service: machine-learning
ms.subservice: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 04/20/2019
ms.openlocfilehash: ce1e5f349f55074b53cf447126c411a7a1cd3394
ms.sourcegitcommit: f5cc71cbb9969c681a991aa4a39f1120571a6c2e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68516913"
---
# <a name="what-is-azure-machine-learning-studio"></a>Wat is Azure Machine Learning Studio?
Microsoft Azure Machine Learning Studio is een hulpprogramma met functionaliteit op basis van slepen en neerzetten, waarmee u in samenwerkingsverband predictive analytics-oplossingen voor uw gegevens kunt ontwikkelen, testen en implementeren. Machine Learning Studio publiceert modellen als webservices die eenvoudig kunnen worden gebruikt door aangepaste apps of BI-hulpprogramma's zoals Excel.

In Machine Learning Studio komen gegevens, wetenschap, predictive analytics, cloudresources en uw gegevens samen.


## <a name="the-machine-learning-studio-interactive-workspace"></a>De interactieve werkruimte van Machine Learning Studio
Voor het ontwikkelen van een voorspellend analyse model gebruikt u doorgaans gegevens uit een of meer bronnen, transformeert en analyseert u die gegevens via verschillende gegevens manipulatie en statistische functies en genereert u een set resultaten. Het ontwikkelen van een model als dit is een iteratief proces. Terwijl u de verschillende functies en de bijbehorende parameters aanpast, worden de resultaten geconvergeerd tot u een afdoende getraind en doeltreffend model hebt.

**Azure Machine Learning Studio** beschikt over een interactieve, visuele werkruimte om eenvoudig een predictive analytics-model te bouwen, te testen en te herhalen. U sleept ***gegevenssets*** en analyse***modules*** naar een interactief canvas en verbindt deze met elkaar om een ***experiment*** op te zetten, dat u vervolgens uitvoert in Machine Learning Studio. Als u het modelontwerp wilt herhalen, kunt u het experiment bewerken, desgewenst een kopie ervan opslaan en het opnieuw uitvoeren. Wanneer u klaar bent, kunt u het ***trainingsexperiment*** converteren naar een ***voorspellend experiment*** en dit vervolgens ***publiceren*** als webservice, zodat het model ook voor anderen toegankelijk is.

U hoeft niets te programmeren. U hoeft alleen gegevenssets en modules visueel met elkaar te verbinden om een predictive analytics-model op te zetten.

![Azure Machine Learning Studio-diagram: Zet experimenten op, lees gegevens uit verschillende bronnen, schrijf beoordeelde gegevens weg, maak modellen.](./media/what-is-ml-studio/azure-ml-studio-diagram.jpg)

## <a name="download-the-machine-learning-studio-overview-diagram"></a>Het overzichtsdiagram voor Machine Learning Studio downloaden
Download het diagram **Microsoft Azure Machine Learning Studio Capabilities Overview** (Overzicht van de mogelijkheden van Azure Machine Learning Studio) voor een algemeen overzicht van de mogelijkheden van Machine Learning Studio. Als u het diagram altijd bij de hand wilt hebben, kunt u het in A3-formaat afdrukken.

**Download het diagram hier: [Overzicht van de mogelijkheden van Microsoft Azure Machine Learning Studio](https://download.microsoft.com/download/C/4/6/C4606116-522F-428A-BE04-B6D3213E9E52/ml_studio_overview_v1.1.pdf)** 
![Overzicht van de mogelijkheden van Microsoft Azure Machine Learning Studio](./media/what-is-ml-studio/ml_studio_overview_v1.1.png)

## <a name="get-started-with-machine-learning-studio"></a>Aan de slag met Machine Learning Studio
Wanneer u machine learning Studio voor het eerst invoert, https://studio.azureml.net) ] (u ziet de **Start** pagina. Vanaf deze pagina kunt u documentatie, video's en webinars bekijken en andere waardevolle informatie zoeken.

Klik linksboven op ![Menu](./media/what-is-ml-studio/menu.png) om verschillende opties te bekijken.
### <a name="azure-machine-learning-studio"></a>Azure Machine Learning Studio
Hier staan twee opties: **Home**, de pagina waar u bent begonnen, en **Studio**.

Klik op **Studio**. U gaat dan naar de **Azure Machine Learning Studio**. U wordt eerst gevraagd om u aan te melden met uw Microsoft-account of met uw werk- of schoolaccount. Wanneer u bent aangemeld, ziet u links de volgende tabbladen:

* **PROJECTS**: verzamelingen van experimenten, gegevenssets, kladblokken en andere resources die één project vertegenwoordigen
* **EXPERIMENTS**: experimenten die u hebt gemaakt en uitgevoerd, of die u hebt opgeslagen als concept
* **WEB SERVICES**: webservices die u hebt geïmplementeerd vanuit uw experimenten
* **NOTEBOOKS**: Jupyter-notitieblokken die u hebt gemaakt
* **DATASETS**: gegevenssets die u naar Studio hebt geüpload
* **TRAINED MODELS**: modellen die u hebt getraind in experimenten en hebt opgeslagen in Studio
* **SETTINGS**: een verzameling instellingen waarmee u uw account en resources kunt configureren

### <a name="gallery"></a>Galerie
Klik op **Gallery** om de **[Azure AI Gallery](https://gallery.azure.ai/)** te openen. In deze galerie deelt een community van gegevenswetenschappers en ontwikkelaars oplossingen die zijn gemaakt met onderdelen van de Cortana Intelligence Suite.

Voor meer informatie over de Gallery raadpleegt u [Share and discover solutions in the Cortana Intelligence Gallery](gallery-how-to-use-contribute-publish.md) (Oplossingen in de Azure AI Gallery delen en ontdekken).

## <a name="components-of-an-experiment"></a>Onderdelen van een experiment
Een experiment bestaat uit gegevenssets die gegevens leveren aan analytische modules. Deze kunt u met elkaar verbinden in een predictive analytics-model. Een geldig experiment heeft de volgende kenmerken:

* Het experiment heeft minimaal één gegevensset en één module
* Gegevenssets mogen alleen worden verbonden met modules
* Modules mogen worden verbonden met gegevenssets of andere modules
* Alle ingangspoorten voor modules moeten een verbinding hebben met de gegevensstroom
* Voor elke module moeten alle vereiste parameters zijn ingesteld

U kunt een geheel nieuw experiment maken, maar u kunt ook een bestaand voorbeeldexperiment als sjabloon gebruiken. Zie [Voorbeeldexperimenten kopiëren om nieuwe experimenten voor Machine Learning te maken](sample-experiments.md) voor meer informatie.

Voor een voorbeeld van het maken van een eenvoudig experiment raadpleegt u [Create a simple experiment in Azure Machine Learning Studio](create-experiment.md) (Een eenvoudig experiment maken in Azure Machine Learning Studio).

Zie [Een predictive analytics-oplossing maken met Azure Machine Learning Studio](tutorial-part1-credit-risk.md) voor de volledige procedure voor het maken van een predictive analytics-oplossing.

### <a name="datasets"></a>Gegevenssets
Een gegevensset bestaat uit gegevens die zijn geüpload naar Machine Learning Studio, zodat ze kunnen worden gebruikt in het modelleringsproces. In Machine Learning Studio is een aantal voorbeeldgegevenssets opgenomen waarmee u kunt experimenteren. U kunt meer gegevenssets uploaden als dat nodig is. Hier volgen enkele voorbeelden van opgenomen gegevenssets:

* **MPG-gegevens voor verschillende auto's**: MPG-waarden (mijl per gallon) voor auto's, geïdentificeerd met het aantal cilinders, paardenkracht, enzovoort.
* **Borstkankergegevens**: gegevens voor borstkankerdiagnose.
* **Bosbrandgegevens**: omvang van bosbranden in het noordoosten van Portugal.

Wanneer u een experiment bouwt, kunt u kiezen uit de lijst met gegevens sets die aan de linkerkant van het canvas beschikbaar zijn.

Voor een lijst van voorbeeldgegevenssets die zijn opgenomen in Machine Learning Studio, raadpleegt u [Use the sample data sets in Azure Machine Learning Studio](use-sample-datasets.md) (De voorbeeldgegevenssets in Azure Machine Learning Studio gebruiken).

### <a name="modules"></a>Modules
Een module is een algoritme dat u met uw gegevens kunt uitvoeren. Machine Learning Studio beschikt over diverse modules, variërend van Ingress-functies tot processen voor training, beoordeling en validatie. Hier volgen enkele voorbeelden van opgenomen modules:

* [Converteren naar ARFF][convert-to-arff] : converteert een door .net geserialiseerde gegevensset naar kenmerk-relation File Format (ARFF).
* [Berekende elementaire statistieken][elementary-statistics] : berekent elementaire statistieken, zoals het gemiddelde, de standaard afwijking, enzovoort.
* [Lineaire regressie][linear-regression] : maakt een online, op Daal gebaseerd lineair regressie model op basis van kleur overgang.
* [Score model][score-model] : scoret een getrainde classificatie of een regressie model.

Terwijl u een experiment maakt, kunt u links in het canvas kiezen uit de lijst met beschikbare modules.

Een module kan een reeks parameters hebben waarmee u de interne algoritmen van de module kunt configureren. Wanneer u een module op het canvas selecteert, worden de parameters van de module weergegeven in het deelvenster **Properties**, rechts van het canvas. U kunt de parameters in dit deelvenster wijzigen om het model af te stemmen.

Zie [Algoritmen kiezen voor Microsoft Azure Machine Learning Studio](algorithm-choice.md) voor hulp bij het navigeren door het uitgebreide scala aan beschikbare machine learning-algoritmen.

## <a name="deploying-a-predictive-analytics-web-service"></a>Een predictive analytics-web service implementeren
Wanneer uw predictive analytics-model klaar is, kunt u het direct vanuit Machine Learning Studio implementeren als webservice. Voor meer informatie over dit proces raadpleegt u [Deploy an Azure Machine Learning web service](publish-a-machine-learning-web-service.md) (Een Azure Machine Learning-webservice implementeren).

<a name="compare"></a>
## <a name="how-is-machine-learning-studio-different-from-azure-machine-learning-service"></a>Waarin verschilt Azure Machine Learning Service van Azure Machine Learning Studio?

[Azure machine learning-service](../service/overview-what-is-azure-ml.md) biedt zowel sdk's **als** een visuele interface (preview) om snel gegevens te kunnen prepen, machine learning modellen te trainen en te implementeren. Deze visuele interface (preview) biedt een vergelijk bare functionaliteit voor slepen en neerzetten in Studio. Maar in tegens telling tot het eigen reken platform van Studio gebruikt de Visual Interface uw eigen reken bronnen en is deze volledig geïntegreerd in Azure Machine Learning service.

Hier volgt een snelle vergelijking.

|| Machine Learning Studio | Azure Machine Learning-service:<br/>Visuele interface|
|---| --- | --- |
|| Algemeen beschikbaar (GA) | In preview|
|Modules voor Interface| Allerlei | Initiële set populaire modules|
|Doelen van de trainings compute| Eigen reken doel, alleen CPU-ondersteuning| Ondersteunt Azure Machine Learning compute, GPU of CPU.<br/>(Andere reken processen die worden ondersteund in SDK)|
|Doelen voor implementatie compute| De indeling van de oorspronkelijke webservice, niet aanpasbaar | Beveiligings opties voor ondernemingen & Azure Kubernetes-service. <br/>([Andere reken](../service/how-to-deploy-and-where.md) processen die worden ondersteund in SDK) |
|Automatische model training en afstemming tuning | Nee | Nog niet in de visuele interface. <br/> (Ondersteund in de SDK en Azure Portal.) | 

Probeer de visuele interface (preview) uit met [Quick Start: Gegevens voorbereiden en visualiseren zonder code te schrijven](../service/ui-quickstart-run-experiment.md)

> [!NOTE]
> Modellen die zijn gemaakt in Studio, kunnen niet worden geïmplementeerd of beheerd door Azure Machine Learning service. Modellen die zijn gemaakt en geïmplementeerd in de service Visual Interface kunnen echter worden beheerd via de werk ruimte van de Azure Machine Learning-service.

## <a name="free-trial"></a>Gratis proefversie

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]


## <a name="next-steps"></a>Volgende stappen
U kunt zich de basiskennis van predictive analytics en machine learning eigen maken aan de hand van een [Stapsgewijze quickstart](create-experiment.md) en [door voorbeelden verder uit te werken](sample-experiments.md).

<!-- Module References -->
[convert-to-arff]: https://msdn.microsoft.com/library/azure/62d2cece-d832-4a7a-a0bd-f01f03af0960/
[elementary-statistics]: https://msdn.microsoft.com/library/azure/3086b8d4-c895-45ba-8aa9-34f0c944d4d3/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
