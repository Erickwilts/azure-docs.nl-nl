---
title: 'Train model: module verwijzing'
titleSuffix: Azure Machine Learning
description: Meer informatie over het gebruik van de module **Train model** in azure machine learning voor het trainen van een classificatie of regressie model.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 10/22/2019
ms.openlocfilehash: c9d02490a5627ead502429ead06608b4fb621058
ms.sourcegitcommit: 87781a4207c25c4831421c7309c03fce5fb5793f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2020
ms.locfileid: "76548404"
---
# <a name="train-model-module"></a>Module Train-Model

In dit artikel wordt een module in Azure Machine Learning Designer (preview) beschreven.

Gebruik deze module om een classificatie of regressie model te trainen. De training vindt plaats nadat u een model hebt gedefinieerd en de para meters hebt ingesteld, en dat gelabelde gegevens vereist. U kunt ook het **Train-model** gebruiken om een bestaand model opnieuw te trainen met nieuwe gegevens. 

## <a name="how-the-training-process-works"></a>Hoe het trainings proces werkt

In Azure Machine Learning is het maken en gebruiken van een machine learning model doorgaans een proces dat uit drie stappen bestaat. 

1. U configureert een model door een bepaald type algoritme te kiezen en de para meters of Hyper parameters te definiëren. Kies een van de volgende model typen: 

    + **Classificatie** modellen, gebaseerd op Neural-netwerken, beslissings structuren en beslissings bossen en andere algoritmen.
    + **Regressie** modellen, die standaard lineaire regressie kunnen bevatten, of die gebruikmaken van andere algoritmen, waaronder Neural netwerken en Bayesiaanse regressie.  

2. Geef een gegevensset op die een label heeft en die gegevens bevat die compatibel zijn met het algoritme. Verbind zowel de gegevens als het model om het **model te trainen**.

    Wat de training produceert, is een specifieke binaire indeling, de iLearner, die de statistische patronen inkapselt die zijn geleerd van de gegevens. U kunt deze indeling niet rechtstreeks wijzigen of lezen. andere modules kunnen echter dit getrainde model gebruiken. 
    
    U kunt ook de eigenschappen van het model weer geven. Zie de sectie met resultaten voor meer informatie.

3. Nadat de training is voltooid, kunt u het getrainde model met een van de [Score modules](./score-model.md)gebruiken om voor spellingen te doen op nieuwe gegevens.

## <a name="how-to-use-train-model"></a>**Train model** gebruiken  
  
1.  Configureer in Azure Machine Learning een classificatie model of regressie model.
    
2. Voeg de module **Train model** toe aan de pijp lijn.  U kunt deze module vinden onder de categorie **machine learning** . Vouw **Train**uit en sleep de module **Train model** naar uw pijp lijn.
  
3.  Koppel de niet-getrainde modus aan de linkerkant. Koppel de trainings gegevensset aan de rechter invoer van **Train model**.

    De trainings gegevensset moet een kolom Label bevatten. Alle rijen zonder labels worden genegeerd.
  
4.  Klik voor **kolom Label**op **kolom kiezer starten**en kies één kolom die de resultaten bevat die het model kan gebruiken voor de training.
  
    - Voor classificatie problemen moet de kolom label **categorische** waarden of **discrete** waarden bevatten. Enkele voor beelden zijn een Ja/Nee-classificatie, een ziekte classificatie code of naam of een inkomsten groep.  Als u een noncategorical-kolom selecteert, wordt er tijdens de training een fout geretourneerd door de module.
  
    -   Voor regressie problemen moet de kolom label **numerieke** gegevens bevatten die de variabele antwoord vertegenwoordigt. In het ideale geval duiden de numerieke gegevens op een doorlopende schaal. 
    
    Voor beelden hiervan zijn een score voor een credit risico, de geschatte tijd voor het mislukken van een harde schijf of het verwachte aantal aanroepen naar een Call Center op een bepaalde dag of tijd.  Als u geen numerieke kolom kiest, wordt er mogelijk een fout bericht weer geven.
  
    -   Als u niet opgeeft welke label kolom u wilt gebruiken, wordt Azure Machine Learning afleiden dat de juiste label kolom is door gebruik te maken van de meta gegevens van de gegevensset. Als de verkeerde kolom wordt gekozen, gebruikt u de kolom kiezer om deze te corrigeren.
  
    > [!TIP] 
    > Als u problemen ondervindt met het gebruik van de kolom kiezer, raadpleegt u het artikel [kolommen in gegevensset selecteren](./select-columns-in-dataset.md) voor tips. Hierin worden enkele algemene scenario's en tips beschreven voor het gebruik van de opties **with Rules** en **op naam** .
  
5.  Voer de pijplijn uit. Als u veel gegevens hebt, kan dit enige tijd duren.

## <a name="bkmk_results"></a>Levert

Nadat het model is getraind:


+ Als u het model in andere pijp lijnen wilt gebruiken, selecteert u de module en selecteert u het pictogram **gegevensset registreren** onder het tabblad **uitvoer** in het rechterdeel venster. U kunt opgeslagen modellen openen in het module palet onder **gegevens sets**.

+ Als u het model wilt gebruiken bij het voors pellen van nieuwe waarden, verbindt u het met de module [score model](./score-model.md) , samen met nieuwe invoer gegevens.


## <a name="next-steps"></a>Volgende stappen

Bekijk de [set met modules die beschikbaar zijn](module-reference.md) voor Azure machine learning. 