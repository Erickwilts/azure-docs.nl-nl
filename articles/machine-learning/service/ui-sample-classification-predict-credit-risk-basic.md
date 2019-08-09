---
title: Ontbreekt Kredietrisico voorspellen
titleSuffix: Azure Machine Learning service
description: Meer informatie over het bouwen van een machine learning classificatie zonder een enkele regel code te schrijven met behulp van de visuele interface.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
author: xiaoharper
ms.author: zhanxia
ms.reviewer: sgilley
ms.date: 05/10/2019
ms.openlocfilehash: d1b3b8fa61492f3cf18e44e3e3046f32ea9c9b03
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2019
ms.locfileid: "68855991"
---
# <a name="sample-3---classification-predict-credit-risk"></a>Voor beeld 3-classificatie: Kredietrisico voorspellen

Meer informatie over het bouwen van een machine learning classificatie zonder een enkele regel code te schrijven met behulp van de visuele interface. In dit voor beeld wordt een geboostte **beslissings structuur met twee klassen** getraind om het krediet risico te voors pellen (hoog of laag) op basis van gegevens over krediet aanvragen, zoals de krediet geschiedenis, leeftijd en het aantal credit cards.

Omdat we proberen de vraag "welke?" te beantwoorden Dit wordt een probleem met de classificatie genoemd. U kunt echter hetzelfde fundamentele proces Toep assen om elk type machine learning probleem aan te pakken, of het nu gaat om een regressie, classificatie, Clustering, enzovoort.

Hier volgt de voltooide grafiek voor dit experiment:

![Grafiek van het experiment](media/ui-sample-classification-predict-credit-risk-basic/overall-graph.png)

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [aml-ui-prereq](../../../includes/aml-ui-prereq.md)]

4. Selecteer de knop **openen** voor het voor beeld 3 experiment:

    ![Het experiment openen](media/ui-sample-classification-predict-credit-risk-basic/open-sample3.png)

## <a name="related-sample"></a>Gerelateerd voor beeld

[Voor beeld 4-classificatie: Een voor spelling van het krediet risico (kosten](ui-sample-classification-predict-credit-risk-cost-sensitive.md) gevoelig) biedt een geavanceerd experiment waarmee hetzelfde probleem als dit experiment wordt opgelost. U ziet hoe u de _kosten gevoelige_ classificatie kunt uitvoeren met behulp van een **python-script module uitvoeren** en de prestaties van twee binaire classificatie algoritmen kunt vergelijken. Raadpleeg het onderwerp als u meer wilt weten over het bouwen van classificatie experimenten.

## <a name="data"></a>Data

We gebruiken de Duitse creditcard gegevensset uit de UC Irvine-opslag plaats.
De gegevensset bevat 1.000 voor beelden met 20 functies en één label. Elk voor beeld vertegenwoordigt een persoon. De functies omvatten numerieke en categorische-functies. Zie de [website van icb's](https://archive.ics.uci.edu/ml/datasets/Statlog+%28German+Credit+Data%29) voor de betekenis van de categorische-functies. De laatste kolom is het label, dat het krediet risico aanduidt en slechts twee mogelijke waarden heeft: hoog credit risico = 2 en laag krediet risico = 1.

## <a name="experiment-summary"></a>Experiment overzicht

We volgen de volgende stappen om het experiment te maken:

1. Sleep de module Duitse credit card ICB data dataset naar het canvas van het experiment.
1. Voeg een module voor het **bewerken van meta gegevens** toe, zodat we betekenis volle namen voor elke kolom kunnen toevoegen.
1. Voeg een **Splits gegevens** module toe om de trainings-en test sets te maken. Stel de Fractie van rijen in de eerste uitvoer gegevensset in op 0,7. Met deze instelling geeft u op dat 70% van de gegevens wordt uitgevoerd naar de linker poort van de module en de rest naar de juiste poort. We gebruiken de gegevensset links voor training en de juiste voor test doeleinden.
1. Voeg een uitmuntende **beslissings structuur** module toe om een gestimuleerde beslissings structuur classificatie te initialiseren.
1. Een **Train model** -module toevoegen. Verbind de classificatie van de vorige stap naar de links invoer poort van het **Train-model**. Voeg de Trainingsset (de linker uitvoer poort van de **gesplitste gegevens**) toe aan de juiste invoer poort van het **Train-model**. Het **Train-model** traint de classificatie.
1. Voeg een module **score model** toe en verbind de module **Train model** . Voeg vervolgens de testset (de juiste poort van de **gesplitste gegevens**) toe aan het **score model**. Het **score model** maakt de voor spellingen. U kunt de uitvoer poort selecteren om de voor spellingen en de positieve klassen kansen te bekijken.
1. Voeg een module **Evaluate model** toe en verbind de gescoorde gegevensset met de linker invoer poort. Als u de evaluatie resultaten wilt weer geven, selecteert u de uitvoer poort van de module **Evaluate model** en selecteert u **visualiseren**.

Hier volgt de volledige experimentele grafiek:

![Grafiek van het experiment](media/ui-sample-classification-predict-credit-risk-basic/overall-graph.png)

## <a name="results"></a>Resultaten

![De resultaten evalueren](media/ui-sample-classification-predict-credit-risk-basic/evaluate-result.png)

In de resultaten van de evaluatie kunt u zien dat de AUC van het model 0,776 is. Bij drempel waarde van 0,5 is de nauw keurigheid 0,621, het intrekken 0,456 en de F1-Score is 0,526.

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [aml-ui-cleanup](../../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Volgende stappen

Bekijk de andere voor beelden die beschikbaar zijn voor de visuele interface:

- [Voor beeld 1-regressie: De prijs van een auto voors pellen](ui-sample-regression-predict-automobile-price-basic.md)
- [Voor beeld 2-regressie: Algoritmen voor het voors pellen van prijzen vergelijken](ui-sample-regression-predict-automobile-price-compare-algorithms.md)
- [Voor beeld 4-classificatie: Voor speld krediet risico (kosten gevoelig)](ui-sample-classification-predict-credit-risk-cost-sensitive.md)
- [Voor beeld 5-classificatie: Verloop voors pellen](ui-sample-classification-predict-churn.md)
- [Voor beeld 6: classificatie: Vlucht vertragingen voors pellen](ui-sample-classification-predict-flight-delay.md)