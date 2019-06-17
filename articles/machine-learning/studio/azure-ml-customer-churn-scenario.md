---
title: Klantverloop analyseren
titleSuffix: Azure Machine Learning Studio
description: Casestudy van het ontwikkelen van een geïntegreerde model voor het analyseren en scoring-klantverloop met behulp van Azure Machine Learning Studio.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: seodec18
ms.date: 12/18/2017
ms.openlocfilehash: e6a7eaa94e7196c830a66b2d77023bd562119c92
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64699446"
---
# <a name="analyze-customer-churn-using-azure-machine-learning-studio"></a>Analyseren traject van de klant met behulp van Azure Machine Learning Studio
## <a name="overview"></a>Overzicht
In dit artikel geeft een referentie-implementatie van een klantverloop analysis-project dat is gebouwd met behulp van Azure Machine Learning Studio. In dit artikel wordt besproken hoe gekoppelde algemene modellen voor het oplossen van het probleem van het verloop van industriële klanten zuinigste. We ook meet de nauwkeurigheid van modellen die zijn gebouwd met behulp van Machine Learning en richtlijnen voor de verdere ontwikkeling beoordelen.  

### <a name="acknowledgements"></a>Bevestigingen
Dit experiment is ontwikkeld en getest door Serge Berger, Gegevenswetenschapper Principal bij Microsoft en Roger Barga, voorheen Product Manager voor Microsoft Azure Machine Learning Studio. De Azure-documentatieteam dank erkent hun ervaring en ze Bedankt voor het delen van deze whitepaper.

> [!NOTE]
> De gegevens die worden gebruikt voor dit experiment is niet openbaar beschikbaar. Zie voor een voorbeeld van het bouwen van een machine learning-model voor verloop analyse: [Detailhandel verloop model sjabloon](https://gallery.azure.ai/Collection/Retail-Customer-Churn-Prediction-Template-1) in [Azure AI Gallery](https://gallery.azure.ai/)
> 
> 



## <a name="the-problem-of-customer-churn"></a>Het probleem van klantverloop
Bedrijven in de markt consumenten en in alle enterprise-sectoren hebben om op te lossen verloop. Soms verloop uitzonderlijk veel en is van invloed op beleidsbeslissingen. De traditionele oplossing is te hoog-neiging zullen zijn churners voorspellen en hun behoeften via een service concierge marketingcampagnes, of door het toepassen van speciale dispensaties. Deze methoden kunnen variëren branche branche. Ze kunnen zelfs binnen één bedrijfstak (bijvoorbeeld telecommunicatie) uit een cluster van bepaalde consumenten verschillen.

De algemene factor is dat ondernemingen nodig om deze bewaarduur van een speciale klant. Zo zou een natuurlijke methodologie score van elke klant met de kans van verloop en de bovenste N die verhelpen. De belangrijkste klanten mogelijk het meest winstgevend zijn. Bijvoorbeeld, meer geavanceerde scenario's een functie winst in dienst is bij de selectie van kandidaten voor speciale dispensatie. Deze overwegingen zijn echter slechts een deel van de complete strategie voor het omgaan met verloop. Bedrijven hebben ook rekening account risico's (en bijbehorende risicotolerantie), wordt het niveau en de kosten van de tussenkomst van de en aannemelijke klantsegmentatie.  

## <a name="industry-outlook-and-approaches"></a>De bedrijfstak van outlook en benaderingen
Geavanceerde verwerking van het verloop is een teken van een goed ontwikkelde bedrijfstak. Het klassieke voorbeeld is de branche telecommunicatie waar abonnees bekend is dat ze vaak overschakelen van één provider naar de andere. Dit verloop niet verplicht is een primair probleem. Bovendien providers aanzienlijke kennis hebben verzameld over *verloop van stuurprogramma's*, zijn de factoren die klanten om over te schakelen.

Telefoon of het apparaat keuze is bijvoorbeeld een bekende stuurprogramma van het verloop in het bedrijf mobiele telefoon. Als gevolg hiervan is een populaire beleid voor de prijs van een toestel subsidize voor nieuwe abonnees en een volledige prijs voor bestaande klanten voor een upgrade. In het verleden heeft dit beleid geleid tot klanten hopping van één provider naar een andere nieuwe korting te krijgen. Deze heeft providers om hun strategieën te verfijnen op zijn beurt gevraagd.

Hoge volatiliteit in aanbiedingen die telefoon is een factor die snel worden ongeldig modellen van het verloop die zijn gebaseerd op de huidige telefoon modellen. Bovendien mobiele telefoons zijn niet alleen apparaten die telecommunicatie, ze zijn ook mode-instructies (Houd rekening met de iPhone). Deze sociale voorspellingsfactoren vallen buiten het bereik van reguliere telecommunicatie-gegevenssets.

Het resultaat voor het maken van modellering is dat u een geluid beleid kan niet devise gewoon door het elimineren van bekende redenen voor het verloop. Een strategie voor continue modellen, met inbegrip van klassieke modellen die kwantificeren variabelen voor de categorische (zoals beslisbomen), is in feite **verplichte**.

Met behulp van big data-sets op hun klanten, uitvoert organisaties big data-analyses (met name, verloop detectie op basis van big data) als een effectieve benadering voor het probleem. U vindt meer informatie over de big data-benadering tot het probleem van het verloop in de aanbevelingen van ETL-sectie.  

## <a name="methodology-to-model-customer-churn"></a>Methodologie aan klantverloop model
Een algemene proces oplossen van problemen op te lossen klantverloop wordt weergegeven in afbeelding 1-3:  

1. Een model risico's kunt u rekening houden met de invloed van acties op waarschijnlijkheid en risico's.
2. Een model tussenkomst van de kunt u overwegen hoe het niveau van de tussenkomst van invloed kan zijn op de kans van verloop en het bedrag van de klant levensduurwaarde (CLV).
3. Deze analyse gepaard met een kwalitatieve analyse die een proactieve marketingcampagne die gericht is op klantsegmenten voor het leveren van de optimale aanbieding is geëscaleerd.  

![Diagram waarin wordt getoond hoe tolerantie plus besluit risicomodellen resulteert in bruikbare inzichten](./media/azure-ml-customer-churn-scenario/churn-1.png)

Deze vooruit uitziende aanpak is de beste manier om het verloop behandelen, maar wordt geleverd met complexiteit: we hebben een multi-modeldatabase archetype en tracering afhankelijkheden tussen de modellen te ontwikkelen. De interactie tussen modellen kan worden ingekapseld, zoals wordt weergegeven in het volgende diagram:  

![Diagram van interactie verloop](./media/azure-ml-customer-churn-scenario/churn-2.png)

*Afbeelding 4: Geïntegreerde multi-modeldatabase archetype*  

Interactie tussen de modellen is essentieel als we een holistische benadering om aan te leveren klantretentie. Elk model vermindert per se de na verloop van tijd; de architectuur is daarom een impliciete lus (vergelijkbaar met de archetype ingesteld door de heldere-DM-standaard voor datamining, [***3***]).  

De algehele cyclus van risico-besluit-marketing segmentering/ontleding is nog steeds een gegeneraliseerde structuur, van toepassing op tal van zakelijke problemen is. Verloop analyse is gewoon een sterke vertegenwoordiger van deze groep van problemen, omdat deze de kenmerken van een complexe zakelijke probleem dat is niet toegestaan een vereenvoudigd voorspellende oplossing vertoont. De sociale aspecten van de moderne aanpak van verloop zijn met name niet gemarkeerd in de benadering, maar de sociale aspecten worden ingekapseld in de archetype modelleren als ze op een model zou zijn.  

Hier een interessante toevoeging is analyse van big data. Vandaag telecommunicatie en detailhandelaren uitgebreide gegevens verzamelen over hun klanten en we kunnen eenvoudig voorzien de noodzaak voor meerdere modellen connectiviteit wordt een algemene trend, bepaalde opkomende trends, zoals het Internet of Things en alomtegenwoordige apparaten, waardoor business-to-intelligente oplossingen op meerdere lagen gebruiken.  

 

## <a name="implementing-the-modeling-archetype-in-machine-learning-studio"></a>De archetype modellen implementeren in Machine Learning Studio
Gezien het probleem dat wordt beschreven, wat is de beste manier om een geïntegreerde modelleren en scoring-aanpak te implementeren? In deze sectie wordt we laten zien hoe we dit doen met behulp van Azure Machine Learning Studio.  

De multi-modeldatabase aanpak is een moet bij het ontwerpen van een globale archetype voor verloop. Ook de scoring (voorspellende) deel van de aanpak moet meerdere modellen.  

Het volgende diagram toont het prototype dat we hebben gemaakt, die de veiligheidsmaatregelen voor vier scoring algoritmen in Machine Learning Studio om te voorspellen verloop. De reden voor het gebruik van een multi-modeldatabase benadering is niet alleen voor het maken van een classificatie ensembles voor betere nauwkeurigheid, maar ook om te beveiligen tegen sprake van redundante aanpassing van labels en prescriptieve Functieselectie verbeteren.  

![Schermafbeelding van een complexe Studio-werkruimte met veel onderling verbonden modules](./media/azure-ml-customer-churn-scenario/churn-3.png)

*Afbeelding 5: Prototype van een benadering van modellering verloop*  

De volgende secties vindt u meer informatie over het model scoren model dat wordt geïmplementeerd met behulp van Machine Learning Studio.  

### <a name="data-selection-and-preparation"></a>Gegevens selecteren en voorbereiden
De gegevens die worden gebruikt om de modellen te bouwen en klanten van de score is verkregen van een verticale CRM-oplossing met de gegevens die zijn verborgen voor het beveiligen van de privacy van klanten. De gegevens bevat informatie over de 8000-abonnementen in de Verenigde Staten en drie bronnen worden gecombineerd: inrichten van gegevens (metagegevens van het abonnement), gegevens over gebruikersactiviteiten (gebruik van het systeem) en gegevens van de klant ondersteuning. De gegevens omvat niet alle bedrijfsgerelateerde gegevens over de klanten; Deze omvatten bijvoorbeeld geen loyaliteit metagegevens of tegoed scores.  

Voor het gemak bent ETL en processen voor opschonen van gegevens buiten het bereik omdat we ervan uitgaan dat het voorbereiden van gegevens is al is klaar ergens anders.

Functies selecteren voor het maken van modellering is gebaseerd op het voorlopige significante scoren van de set voorspellingsfactoren, opgenomen in het proces dat gebruikmaakt van de module willekeurige forest. We berekend voor de implementatie in Machine Learning Studio, het gemiddelde, mediaan en bereiken voor representatieve functies. We hebben toegevoegd bijvoorbeeld statistische functies voor de kwalitatieve gegevens, zoals de minimale en maximale waarden voor gebruikersactiviteit.

We hebben ook vastgelegd tijdelijke gegevens voor de meest recente zes maanden. Gegevens geanalyseerd voor één jaar en wij tot stand gebracht, zelfs als er statistisch significant trends waren, wordt de gevolgen zijn voor verloop aanzienlijk verminderd na zes maanden.  

Het belangrijkste punt is dat het hele proces, zoals ETL, feature selection en modelleren in Machine Learning Studio, met behulp van gegevensbronnen in Microsoft Azure is geïmplementeerd.   

De volgende diagrammen ziet u de gegevens die is gebruikt.  

![Schermopname van een voorbeeld van de gegevens die worden gebruikt met onbewerkte waarden](./media/azure-ml-customer-churn-scenario/churn-4.png)

*Afbeelding 6: Fragment van een gegevensbron (verborgen)*  

![Schermopname van statistische functies die zijn geëxtraheerd uit de gegevensbron](./media/azure-ml-customer-churn-scenario/churn-5.png)

*Afbeelding 7: Functies die zijn geëxtraheerd uit de gegevensbron*
 

> Houd er rekening mee dat deze gegevens privé is; daarom het model en de gegevens kunnen niet worden gedeeld.
> Zie, voor een vergelijkbaar model met behulp van de openbaar beschikbare gegevens, in dit voorbeeld een experiment uit in de [Azure AI Gallery](https://gallery.azure.ai/): [Telco Customer Churn](https://gallery.azure.ai/Experiment/31c19425ee874f628c847f7e2d93e383).
> 
> Voor meer informatie over hoe u een verloop Analytics-model met behulp van Cortana Intelligence Suite kunt implementeren, wordt ook aangeraden [in deze video](https://info.microsoft.com/Webinar-Harness-Predictive-Customer-Churn-Model.html) door Senior Program Manager Westerse Hyong Tok. 
> 
> 

### <a name="algorithms-used-in-the-prototype"></a>Die worden gebruikt in het model
We de volgende vier machine learning-algoritmen gebruikt voor het bouwen van het model (geen aanpassing):  

1. Logistieke regressie (LR)
2. Boosted-beslisboom (BT)
3. Gemiddelde perceptron (AP)
4. Voor ondersteuningsvectormachines (SVM)  

Het volgende diagram illustreert een deel van het ontwerpvenster experiment, waarmee wordt aangegeven van de volgorde waarin de modellen zijn gemaakt:  

![Schermafbeelding van een klein gedeelte van de studio-experiment canvas](./media/azure-ml-customer-churn-scenario/churn-6.png)  

*Afbeelding 8: Het maken van modellen in Machine Learning Studio*  

### <a name="scoring-methods"></a>Scoring-methoden
We vier modellen hebt beoordeeld met behulp van een gegevensset met gelabelde training.  

We ook verzonden de scoring gegevensset met een vergelijkbare model gebouwd met behulp van de desktop-editie van SAS Enterprise hierop 12. We de nauwkeurigheid van het SAS-model en alle vier modellen voor Machine Learning Studio gemeten.  

## <a name="results"></a>Resultaten
In deze sectie geven we onze bevindingen over de nauwkeurigheid van de modellen, op basis van de scoring-gegevensset.  

### <a name="accuracy-and-precision-of-scoring"></a>Nauwkeurigheid en precisie van score
De implementatie in Azure Machine Learning Studio is over het algemeen achter SAS nauwkeurigheid met ongeveer 10-15% (gebied onder Curve of AUC).  

De belangrijkste metrische gegevens in verloop is echter de snelheid misclassification: dat wil zeggen, van de top N-churners als voorspelde door de classificatie, welke van deze daadwerkelijk is **niet** verloop, en nog speciale behandeling ontvangen? Het volgende diagram worden deze snelheid misclassification voor alle modellen vergeleken:  

![Gebied onder de grafiek van de curve vergelijken de prestaties van 4 algoritmen](./media/azure-ml-customer-churn-scenario/churn-7.png)

*Afbeelding 9: Passau prototype gebied onder curve*

### <a name="using-auc-to-compare-results"></a>Met behulp van AUC om resultaten te vergelijken
Gebied onder Curve (AUC) is een metrische waarde die staat voor een globale maateenheid *Afscheidbaarheid* tussen de verdeling van scores voor positieve en negatieve populaties. Het is vergelijkbaar met de traditionele ontvanger Operator kenmerk (ROC)-grafiek, maar één belangrijk verschil is dat de metriek AUC vereist niet dat u een waarde voor drempel kiezen. In plaats daarvan het bevat een overzicht van de resultaten **alle** keuzemogelijkheden. Daarentegen de traditionele ROC-grafiek toont de-positief-ratio op de verticale as en de fout-positief-ratio op de horizontale as en is afhankelijk van de drempelwaarde voor classificatie.   

AUC wordt gebruikt als een meting van de waard is om voor verschillende algoritmen (of andere systemen), omdat hierdoor modellen worden vergeleken met behulp van de waarden van hun AUC. Dit is een populaire benadering in branches zoals meteorologie en biosciences. Dus vertegenwoordigt AUC een populair hulpprogramma voor het beoordelen van de prestaties van de classificatie.  

### <a name="comparing-misclassification-rates"></a>Vergelijking van de tarieven voor misclassification
We vergeleken de misclassification tarieven op de gegevensset in kwestie met behulp van de CRM-gegevens van ongeveer 8.000 abonnementen.  

* De snelheid van de misclassification SAS is 10-15%.
* De snelheid van Machine Learning Studio misclassification is 15-20% van de top 200 en 300 churners.  

In de branche telecommunicatie is het belangrijk om alleen die klanten met het hoogste risico voor het verloop door aan te bieden ze een concierge-service of andere speciale behandeling. In dat opzicht realiseert de uitvoering van Machine Learning Studio resultaten gelijk aan het SAS-model.  

Evenzo, is nauwkeurigheid belangrijker dan de precisie omdat we voornamelijk geïnteresseerd bent in goed mogelijke churners classificeren.  

Het volgende diagram van Wikipedia ziet u de relatie in een afbeelding levendige, eenvoudig te begrijpen:  

![Twee doelen. Toont één doel merken losjes gegroepeerd bereikt, maar in de buurt van de stieren-ogen gemarkeerd als ' laag nauwkeurigheid: goede juistheid, slechte precisie. Een ander doel nauw gegroepeerd maar ver ligt de stieren-ogen gemarkeerd als ' laag nauwkeurigheid: slechte juistheid, goede precisie "](./media/azure-ml-customer-churn-scenario/churn-8.png)

*Afbeelding 10: Verhouding tussen de nauwkeurigheid en precisie*

### <a name="accuracy-and-precision-results-for-boosted-decision-tree-model"></a>Resultaten nauwkeurigheid en precisie voor boosted decision tree model
Het volgende diagram wordt de onbewerkte resultaten van het scoring-met behulp van de Machine Learning-model voor de boosted decision tree-model, wat gebeurt er met de meest nauwkeurige tussen de vier modellen worden weergegeven:  

![Tabel fragment van nauwkeurigheid, precisie, zoals eerder vermeld, F-Score, AUC, gemiddelde logboek verlies en Training Log gegevensverlies voor vier algoritmen](./media/azure-ml-customer-churn-scenario/churn-9.png)

*Afbeelding 11: Boosted decision tree model kenmerken*

## <a name="performance-comparison"></a>Vergelijking van de prestaties
We ten opzichte van de snelheid waarmee gegevens is beoordeeld met behulp van de modellen voor Machine Learning Studio en een vergelijkbare model dat is gemaakt met behulp van de desktop-editie van SAS Enterprise hierop 12.1.  

De volgende tabel geeft een overzicht van de prestaties van de algoritmen:  

*Tabel 1. Algemene prestaties (nauwkeurigheid) van de algoritmen*

| LR | BT | AP | SVM |
| --- | --- | --- | --- |
| Gemiddelde Model |Het beste Model |Attenderen |Gemiddelde Model |

De modellen die worden gehost in Machine Learning Studio presteerde SAS met 15-25% van de snelheid van kan worden uitgevoerd, maar de nauwkeurigheid grotendeels afhankelijk par is.  

## <a name="discussion-and-recommendations"></a>Bespreking en aanbevelingen
In de branche telecommunicatie verschillende procedures voor het analyseren van verloop, is ontstaan met inbegrip van:  

* Metrische gegevens voor vier fundamentele categorieën worden afgeleid:
  * **Entiteit (bijvoorbeeld een abonnement)** . Algemene informatie over het abonnement en/of de klant die het onderwerp van het verloop inrichten.
  * **Activiteit**. Alle informatie over het gebruik mogelijk die is gerelateerd aan de entiteit, bijvoorbeeld het aantal aanmeldingen verkrijgen.
  * **Klantondersteuning**. Verzamelt gegevens uit logboeken van de klant ondersteuning om aan te geven of het abonnement problemen of interactie met de klantondersteuning heeft.
  * **Concurrerende en zakelijke gegevens**. Verkrijgen van mogelijk informatie over de klant (bijvoorbeeld kunnen zijn niet beschikbaar of moeilijk zijn om bij te houden).
* Gebruik van belang voor het station functies selecteren. Dit betekent dat het boosted decision tree model altijd een veelbelovende benadering is.  

Het gebruik van de volgende vier categorieën wordt de illusie die een eenvoudige *deterministische* benadering, op basis van de indexen op redelijke factoren per categorie, een onjuiste indeling moet voldoende zijn voor het identificeren van klanten op risico's voor verloop. Helaas, hoewel dit begrip plausibele lijkt, is een false begrip. De reden is dat verloop een tijdelijke effect is en de factoren die bijdragen aan het verloop meestal in tijdelijke Staten zijn. Wat kunt u vandaag nog klanten potentiële klanten kan afwijken morgen en het zeker worden verschillende zes maanden vanaf nu. Daarom een *probabilistic* model is noodzakelijk.  

Observatie van dit belangrijk is vaak over het hoofd gezien in het bedrijfsleven, die in het algemeen de voorkeur geeft aan een business intelligence-georiënteerde benadering met analytics, meestal omdat het een eenvoudiger verkopen en admits eenvoudig automatiseren.  

De belofte van selfservice-analyses met behulp van Machine Learning Studio is echter dat de vier categorieën van informatie, ingedeeld in de afdeling een waardevolle bron voor machine learning over verloop geworden.  

Een andere interessante mogelijkheden die afkomstig zijn in Azure Machine Learning Studio is de mogelijkheid om een aangepaste module toevoegen aan de opslagplaats van de vooraf gedefinieerde modules die al beschikbaar zijn. Deze mogelijkheid maakt in feite een mogelijkheid voor het selecteren van bibliotheken en sjablonen maken voor verticale markten. Het is een belangrijk kenmerk van Azure Machine Learning Studio op de markt.  

We hopen dat om door te gaan in dit onderwerp in de toekomst, met name met betrekking tot de analyse van big data.
  

## <a name="conclusion"></a>Conclusie
Dit document beschrijft een functionele aanpak voor het aanpakken van het algemene probleem van klantverloop met behulp van een algemene framework. We beschouwd als een prototype voor het scoren van modellen en geïmplementeerd, met behulp van Azure Machine Learning Studio. Ten slotte wordt de nauwkeurigheid en de prestaties van de oplossing prototype met betrekking tot vergelijkbare algoritmen in SAS beoordeeld.  

 

## <a name="references"></a>Verwijzingen
[1] predictive Analytics: Naast de voorspellingen W. McKnight Information Management, juli/augustus 2011 p.18-20.  

[2] Wikipedia-artikel: [Nauwkeurigheid en precisie](https://en.wikipedia.org/wiki/Accuracy_and_precision)

[3] [CRISP-DM 1.0: Stapsgewijze datamining Guide](https://www.the-modeling-agency.com/crisp-dm.pdf)   

[4] [Big Data Marketing: Bied uw klanten effectiever en stimuleren van waarde](https://www.amazon.com/Big-Data-Marketing-Customers-Effectively/dp/1118733894/ref=sr_1_12?ie=UTF8&qid=1387541531&sr=8-12&keywords=customer+churn)

[5] [Telco verloop model sjabloon](https://gallery.azure.ai/Experiment/Telco-Customer-Churn-5) in [Azure AI Gallery](https://gallery.azure.ai/) 
 

## <a name="appendix"></a>Bijlage
![Momentopname van een presentatie op verloop prototype](./media/azure-ml-customer-churn-scenario/churn-10.png)

*Afbeelding 12: Momentopname van een presentatie op verloop prototype*
