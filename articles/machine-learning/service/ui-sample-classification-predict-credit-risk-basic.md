---
title: 'Classificatie: Kredietrisico voorspellen'
titleSuffix: Azure Machine Learning service
description: Meer informatie over het bouwen van een machine learning-classificatie zonder te hoeven schrijven van één regel code met behulp van de visuele interface.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: article
author: xiaoharper
ms.author: zhanxia
ms.reviewer: sgilley
ms.date: 05/10/2019
ms.openlocfilehash: f37c945758cfbd03889d79acf764e7f67022267a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65789402"
---
# <a name="sample-3---classification-predict-credit-risk"></a>Voorbeeld 3 - indeling: Kredietrisico voorspellen

Meer informatie over het bouwen van een machine learning-classificatie zonder te hoeven schrijven van één regel code met behulp van de visuele interface. In dit voorbeeld traint een **twee-class boosted-beslisboom** om te voorspellen tegoed risico (hoog of laag) op basis van tegoed toepassingsinformatie zoals kredietgeschiedenis, leeftijd en aantal creditcards te accepteren.

Omdat we willen de vraag worden beantwoord "Waarvoor een?" Dit is een probleem classificatie genoemd. U kunt echter de dezelfde fundamentele procedure voor het aanpakken van elk type machine learning probleem, ongeacht of dit regressie, classificatie, clustering, enzovoort toepassen.

Hier volgt de voltooide grafiek voor dit experiment:

![Grafiek van het experiment](media/ui-sample-classification-predict-credit-risk-basic/overall-graph.png)

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [aml-ui-prereq](../../../includes/aml-ui-prereq.md)]

4. Selecteer de **Open** knop van het experiment voorbeeld 3:

    ![Open het experiment](media/ui-sample-classification-predict-credit-risk-basic/open-sample3.png)

## <a name="related-sample"></a>Gerelateerde voorbeeld

[Voorbeeld 4 - classificatie: Credit Risk Prediction (kosten gevoelige)](ui-sample-classification-predict-credit-risk-cost-sensitive.md) biedt een geavanceerde experiment dat hetzelfde probleem als dit experiment is opgelost. U leert hoe u uitvoeren _kosten gevoelige_ classificatie met behulp van een **Execute Python Script** -module en vergelijk de prestaties van twee binaire classificatie-algoritmen. Verwijzen naar deze als u wilt meer informatie over het bouwen van experimenten voor classificatie.

## <a name="data"></a>Gegevens

We gebruiken de gegevensset Duits creditcard is geregistreerd vanuit de opslagplaats UC Irvine.
De gegevensset bevat 1000 voorbeelden met 20 functies en 1 label. Elk voorbeeld vertegenwoordigt een persoon. De functies behoren functies voor numerieke en categorische. Zie de [UCI website](https://archive.ics.uci.edu/ml/datasets/Statlog+%28German+Credit+Data%29) voor de betekenis van de categorische functies. De laatste kolom wordt het label geeft aan het kredietrisico en heeft slechts twee mogelijke waarden: hoge kredietrisico = 2, en lage kredietrisico's = 1.

## <a name="experiment-summary"></a>Samenvatting van experiment

We stappen deze voor het maken van het experiment uitvoert:

1. Sleep de module van de gegevensset Duits UCI creditcardgegevens in van het experimentcanvas.
1. Voeg een **metagegevens bewerken** module, zodat we betekenisvolle namen voor elke kolom kunt toevoegen.
1. Voeg een **Split Data** module om de sets trainings- en testset te maken. De fractie van rijen in de eerste uitvoergegevensset naar 0,7 instellen. Deze instelling geeft aan dat 70% van de gegevens wordt uitvoer naar de linker-poort van de module en de rest aan de juiste poort. We gebruiken de links gegevensset voor training en de juiste is voor het testen.
1. Voeg een **Two-Class Boosted beslissingsstructuur** module een boosted decision tree classificatie worden geïnitialiseerd.
1. Voeg een **Train Model** module. Verbinding maken met de classificatie van de vorige stap aan de linkerinvoerpoort van de **Train Model**. De trainingsset toevoegen (links-uitvoerpoort van de **Split Data**) aan de rechterinvoerpoort van de **Train Model**. De **Train Model** wordt de classificatie trainen.
1. Voeg een **Score Model** module en maak verbinding de **Train Model** module toe. Voeg de testset (de juiste poort van de **Split Data**) naar de **Score Model**. De **Score Model** wordt de voorspellingen. U kunt de uitvoerpoort om te zien van de voorspellingen en de kansen positieve klasse selecteren.
1. Voeg een **Evaluate Model** module en maak de beoordeelde gegevensset verbinding met de linkerinvoerpoort. Als u wilt zien van de evaluatieresultaten, selecteert u de uitvoerpoort van de **Evaluate Model** -module en selecteer **Visualize**.

Dit is de volledige experimentgrafiek:

![Grafiek van het experiment](media/ui-sample-classification-predict-credit-risk-basic/overall-graph.png)

## <a name="results"></a>Resultaten

![De resultaten evalueren](media/ui-sample-classification-predict-credit-risk-basic/evaluate-result.png)

In de resultaten van de beoordeling ziet u dat de AUC van het model 0.776 is. Met drempelwaarde 0,5, de precisie is 0.621, het intrekken van het 0,456 is en de F1-score 0.526 is.

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [aml-ui-cleanup](../../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Volgende stappen

De voorbeelden beschikbaar zijn voor de visuele interface verkennen:

- [Voorbeeld 1: regressie: De prijs van een auto's voorspellen](ui-sample-regression-predict-automobile-price-basic.md)
- [Voorbeeld 2: regressie: Algoritmen voor auto's voorspellen vergelijken](ui-sample-regression-predict-automobile-price-compare-algorithms.md)
- [Voorbeeld 4 - classificatie: Kredietrisico (kosten gevoelige) voorspellen](ui-sample-classification-predict-credit-risk-cost-sensitive.md)
- [Voorbeeld 5 - indeling: Verloop voorspellen](ui-sample-classification-predict-churn.md)