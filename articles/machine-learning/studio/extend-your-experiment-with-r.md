---
title: Uw experiment uitbreiden met R
titleSuffix: Azure Machine Learning Studio
description: Klik hier voor meer informatie over het uitbreiden van de functionaliteit van Azure Machine Learning Studio via de R-taal met behulp van de module R-Script uitvoeren.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: previous-author=heatherbshapiro, previous-ms.author=hshapiro
ms.date: 03/20/2017
ms.openlocfilehash: 8c1292d0d36874892a286d91b1e367c7336b99aa
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60811421"
---
# <a name="azure-machine-learning-studio-extend-your-experiment-with-r"></a>Azure Machine Learning Studio: Uw experiment uitbreiden met R 
U kunt de functionaliteit van Azure Machine Learning Studio via de R-taal uitbreiden met behulp van de [R-Script uitvoeren] [ execute-r-script] module.

Deze module accepteert meerdere gegevenssets voor invoer en resulteert in een enkele gegevensset als uitvoer. Typt u een R-script in de **R-Script** parameter van de [R-Script uitvoeren] [ execute-r-script] module.

U toegang tot elke invoerpoort van de module met behulp van code die vergelijkbaar is met het volgende:

    dataset1 <- maml.mapInputPort(1)

## <a name="listing-all-currently-installed-packages"></a>Lijst van alle geïnstalleerde pakketten
De lijst met geïnstalleerde pakketten kunt wijzigen. Een lijst met geïnstalleerde pakketten te vinden in [R-pakketten ondersteund door Azure Machine Learning Studio](https://msdn.microsoft.com/library/azure/mt741980.aspx).

U kunt de volledige, actuele lijst met geïnstalleerde pakketten ook ophalen door in te voeren van de volgende code in de [R-Script uitvoeren] [ execute-r-script] module:

    out <- data.frame(installed.packages(,,,fields="Description"))
    maml.mapOutputPort("out")

Hierdoor wordt de lijst met pakketten verzonden naar de uitvoerpoort van de [R-Script uitvoeren] [ execute-r-script] module.
Als u wilt weergeven van de pakketlijst, verbinding maken met een conversie-module, zoals [converteren naar CSV] [ convert-to-csv] aan de linkerkant uitvoer van de [R-Script uitvoeren] [ execute-r-script] module Voer het experiment uit, klik op de uitvoer van de conversie-module en selecteer **downloaden**. 

![Uitvoer van "Converteren naar CSV"-module downloaden](./media/extend-your-experiment-with-r/download-package-list.png)


<!--
For convenience, here is the [current full list with version numbers in Excel format](https://az754797.vo.msecnd.net/docs/RPackages.xlsx).
-->

## <a name="importing-packages"></a>Pakketten importeren
U kunt importeren pakketten die nog niet zijn geïnstalleerd met behulp van de volgende opdrachten in de [R-Script uitvoeren] [ execute-r-script] module:

    install.packages("src/my_favorite_package.zip", lib = ".", repos = NULL, verbose = TRUE)
    success <- library("my_favorite_package", lib.loc = ".", logical.return = TRUE, verbose = TRUE)

waar de `my_favorite_package.zip` -bestand bevat uw pakket.




<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[convert-to-csv]: https://msdn.microsoft.com/library/azure/faa6ba63-383c-4086-ba58-7abf26b85814/
