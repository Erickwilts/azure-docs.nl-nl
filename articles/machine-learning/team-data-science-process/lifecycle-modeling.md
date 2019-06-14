---
title: Modellering van fase van de levenscyclus van het Team Data Science Process
description: De doelen, taken en producten voor de fase van het modelleren van uw data-science-projecten
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/04/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: c22c75b4fe900ecb96d016251c09e9ad6ec31f7c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60306221"
---
# <a name="modeling-stage-of-the-team-data-science-process-lifecycle"></a>Modellering van fase van de levenscyclus van het Team Data Science Process

In dit artikel bevat een overzicht van de doelen, taken en producten die zijn gekoppeld aan de model-fase van het Team Data Science Process (TDSP). Deze procedure biedt de levensduur van een aanbevolen die u gebruiken kunt voor het structureren van uw data-science-projecten. De levenscyclus van geeft een overzicht van de belangrijke fasen die projecten doorgaans worden uitgevoerd, vaak iteratief:

   1. **Inzicht in het bedrijf**
   2. **Gegevens verzamelen en begrijpen**
   3. **Modeling**
   4. **Implementatie**
   5. **Aanvaarding van de klant**

Hier volgt een visuele representatie van de TDSP-levenscyclus:

![TDSP-levenscyclus](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goals"></a>Doelstellingen
* Bepaalt de optimale data-functies voor de machine learning-model.
* Maak een informatieve machine learning-model waarmee het doel nauwkeurigst worden voorspeld.
* Maak een machine learning-model dat geschikt is voor productie.

## <a name="how-to-do-it"></a>Hoe voer ik
Er zijn drie belangrijkste taken in deze fase behandeld:

  * **functie-engineering**: Functies van de onbewerkte gegevens te vergemakkelijken modeltraining maken.
  * **Training model**: Zoek het model dat de meest nauwkeurige antwoord op de vraag door het vergelijken van hun maatstaven voor succes.
  * Vaststellen of uw model **geschikt is voor productie.**

### <a name="feature-engineering"></a>Functie-engineering
Feature-engineering omvat de opname, aggregatie en transformatie van onbewerkte variabelen te maken van de functies die in de analyse worden gebruikt. Als u inzicht in wat vraagt om een model wilt, moet u begrijpen hoe de functies in verband met elkaar en hoe de machine learning-algoritmes zijn om deze functies te gebruiken. 

Deze stap moet een creatieve combinatie van expertise in verschillende domeinen en de inzichten die zijn verkregen van de gegevens verkennen stap. Feature-engineering is een evenwichtsoefening te vinden en informatieve variabelen inclusief, maar op hetzelfde moment probeert om te voorkomen dat te veel niet-gerelateerde variabelen. Informatieve variabelen verbetering van uw resultaat; niet-gerelateerde variabelen introduceren onnodige ' ruis ' in het model. Ook moet u deze functies voor alle nieuwe gegevens die zijn verkregen tijdens het scoring-genereren. Als gevolg hiervan kan de generatie van deze functies alleen afhankelijk van gegevens die beschikbaar is op het moment van scores. 

Voor technische richtlijnen voor de functie engineering bij maken van verschillende gegevenstechnologieën die Azure gebruiken, raadpleegt u [Functietechniek in het data science process](create-features.md). 

### <a name="model-training"></a>Modeltraining
Afhankelijk van het type vraag die u probeert te beantwoorden, zijn er veel modellen algoritmen beschikbaar. Zie voor instructies over het kiezen van de algoritmen [algoritmen kiezen voor Microsoft Azure Machine Learning](../studio/algorithm-choice.md). Hoewel dit artikel wordt gebruikgemaakt van Azure Machine Learning, is de richtlijnen biedt handig voor de machine learning-projecten. 

Het proces voor het trainen van het model bevat de volgende stappen uit: 

   * **De ingevoerde gegevens splitsen** willekeurig voor modellen in een set trainingsgegevens en een test-gegevensset.
   * **De modellen bouwen** met behulp van de set trainingsgegevens.
   * **Evalueren** de training en de testgegevensset. Gebruik van een reeks concurrerende machine learning-algoritmen, samen met de verschillende parameters voor de bijbehorende afstemmen (ook wel een *parameteropschoning*) die zijn afgestemd op het beantwoorden van de vraag van belang zijn met de actuele gegevens.
   * **Bepalen welke oplossing voor de 'aanbevolen'** aan de vraag worden beantwoord door het vergelijken van de maatstaven voor succes tussen alternatieve methoden.

> [!NOTE]
> **Te voorkomen dat lekken**: Als u gegevens die zich buiten de set trainingsgegevens waarmee een model of de machine-learning-algoritme om onrealistisch goede voorspellingen te doen, kunt u gegevenslekken veroorzaken. Lekken van gegevens is een veelvoorkomende reden waarom data scientists zenuwstelsel ontvangen wanneer ze voorspellende resultaten die ontvangt lijken te mooi om waar te zijn. Deze afhankelijkheden zijn moeilijk te detecteren. Om u te vaak te voorkomen dat lekken vereist digitaler tussen het bouwen van een gegevensset voor analyse, het maken van een model en de nauwkeurigheid van de resultaten evalueren. 
> 
> 

We bieden een [geautomatiseerde modelleren en rapportagetool](https://github.com/Azure/Azure-TDSP-Utilities/blob/master/DataScienceUtilities/Modeling) met TDSP die kan worden uitgevoerd via meerdere algoritmen en de parameter sweeps te produceren van een basislijn-model. Het genereert ook een basislijn voor het modelleren van rapport met een overzicht van de prestaties van elke combinatie model en parameter, met inbegrip van de variabele belang is. Dit proces is ook iteratieve als het verdere feature-engineering kunt uitbreiden. 

## <a name="artifacts"></a>Artefacten
De artefacten die in deze fase worden geproduceerd, zijn onder andere:

   * [Functie sets](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/DataReport/Data%20Defintion.md#feature-sets): De functies die zijn ontwikkeld voor het maken van modellering worden beschreven in de **functie sets** sectie van de **gegevensdefinitie** rapport. Het bevat verwijzingen naar de code voor het genereren van de functies en een beschrijving van hoe de functie is gegenereerd.
   * [Rapport model](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Model/Model%201/Model%20Report.md): Voor elk model dat wordt geprobeerd een standaard, wordt op basis van sjabloon rapport vindt u informatie over elke experiment geproduceerd.
   * **Controlepunt besluit**: Evalueer of het model goed genoeg functioneert om deze te implementeren naar een productiesysteem. Er zijn een aantal belangrijke vragen stellen:
     * Het model de vraag worden beantwoord met voldoende vertrouwen gegeven van de testgegevens? 
     * Moet u een alternatieve methoden proberen? Moet u aanvullende gegevens verzamelen, doen meer feature-engineering of experimenteren met andere algoritmen?

## <a name="next-steps"></a>Volgende stappen

Hier vindt u koppelingen naar elke stap in de levenscyclus van de TDSP:

   1. [Inzicht in het bedrijf](lifecycle-business-understanding.md)
   2. [Gegevens verzamelen en begrijpen](lifecycle-data.md)
   3. [Modeling](lifecycle-modeling.md)
   4. [Implementatie](lifecycle-deployment.md)
   5. [Aanvaarding van de klant](lifecycle-acceptance.md)

We bieden volledige end-to-end-scenario's die laten zien van alle de stappen in het proces voor het specifieke scenario's. De [voorbeeld walkthroughs](walkthroughs.md) artikel geeft een lijst van de scenario's met koppelingen en beschrijvingen van miniaturen. De scenario's laten zien hoe u cloud, on-premises hulpprogramma's en services combineren in een werkstroom of een pijplijn te maken van een intelligente toepassingen. 

Zie voor meer voorbeelden van hoe u het uitvoeren van de stappen in TDSPs die gebruikmaken van Azure Machine Learning Studio [de TDSP gebruiken met Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/). 
