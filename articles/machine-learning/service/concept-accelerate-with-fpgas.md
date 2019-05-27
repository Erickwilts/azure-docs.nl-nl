---
title: What are field-programmable gate arrays (FPGA)
titleSuffix: Azure Machine Learning service
description: Informatie over het versnellen van modellen en deep neural networks met FPGA's op Azure. Dit artikel bevat een inleiding tot veld-programmable gate arrays (FPGA) en hoe de Azure Machine Learning-service biedt realtime kunstmatige intelligentie (AI) wanneer u uw model naar een Azure-FPGA implementeert.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: tedway
author: tedway
ms.reviewer: jmartens
ms.date: 04/24/2019
ms.custom: seodec18
ms.openlocfilehash: 1a690ea350ea98589e9134cd6f401c6ac3c58083
ms.sourcegitcommit: 16cb78a0766f9b3efbaf12426519ddab2774b815
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2019
ms.locfileid: "65851028"
---
# <a name="what-are-field-programmable-gate-arrays-fpga"></a>What are field-programmable gate arrays (FPGA)

Dit artikel bevat een inleiding tot veld-programmable gate arrays (FPGA) en hoe de Azure Machine Learning-service biedt realtime kunstmatige intelligentie (AI) wanneer u uw model implementeert naar een Azure-FPGA.

FPGA's bevatten een matrix van programmeerbare logische blokken, en een hiërarchie van herconfigureerbare koppelt. De verbindingen toestaan deze blokken op verschillende manieren worden geconfigureerd nadat de productie. In vergelijking met andere chips, bieden FPGA's een combinatie van programmeren en prestaties.

## <a name="fpgas-vs-cpu-gpu-and-asic"></a>FPGA's vs. CPU, GPU en ASIC

Het volgende diagram en tabel laten u zien hoe FPGA's zich verhouden tot andere processors.

![Diagram van FPGA vergelijking van Azure Machine Learning-service](./media/concept-accelerate-with-fpgas/azure-machine-learning-fpga-comparison.png)

|Processor||Description|
|---|:-------:|------|
|Toepassingsspecifieke geïntegreerde circuits|ASICs|Aangepaste circuits, zoals Google TensorFlow Processor eenheden (TPU), biedt het hoogste rendement. Ze kunnen niet opnieuw worden geconfigureerd wanneer uw behoeften veranderen.|
|Veld-programmable gate arrays|FPGA 's|FPGA's, zoals die beschikbaar zijn op Azure, levert prestaties dicht bij ASICs. Ze zijn ook flexibele en herconfigureerbare gedurende een periode, om nieuwe logica te implementeren.|
|Grafische verwerkingseenheden|GPU 's|Een populaire optie voor AI-berekeningen. GPU's bieden mogelijkheden voor parallelle verwerking, waardoor het sneller op beeldweergave dan CPU's.|
|Central processing units|CPU's|Voor algemene doeleinden processors, de prestaties van die niet ideaal voor grafische afbeeldingen en video verwerking is.|

FPGA's in Azure zijn gebaseerd op Intel FPGA-apparaten, welke data scientists en ontwikkelaars met realtime AI-berekeningen te versnellen. Deze architectuur FPGA-functionaliteit biedt prestaties, flexibiliteit en de schaal en is beschikbaar op Azure.

FPGA's maken het mogelijk is om aanvragen voor lage latentie voor realtime Deductie (of het model scoren). Asynchrone aanvragen (batchverwerking) niet meer nodig hebt. Batchverwerking kan de latentie, veroorzaken, omdat er meer gegevens moet worden verwerkt. Implementaties van neurale verwerkingseenheden vereisen batchverwerking; de latentie kan dus minder vaak, vergeleken met de CPU en GPU-processors.

### <a name="reconfigurable-power"></a>Herconfigureerbare power
U kunt FPGA's voor verschillende soorten machine learning-modellen opnieuw configureren. Deze flexibiliteit maakt het gemakkelijker om te versnellen van de toepassingen op basis van de meest optimale numerieke precisie en model van het geheugen wordt gebruikt. Omdat FPGA's herconfigureerbare, u kunt op de hoogte blijven met de vereisten van snel veranderende AI-algoritmen.

### <a name="whats-supported-on-azure"></a>Wat wordt ondersteund op Azure
Microsoft Azure is's werelds grootste investeringen in de cloud in FPGA's. FPGA's op Azure ondersteunt:

+ Afbeelding classificatie en herkenning van scenario 's
+ TensorFlow-implementatie
+ Dnn's: ResNet 50, ResNet 152 VGG-16, SSD-VGG en DenseNet 121
+ Intel FPGA-hardware 

Met deze hardwarearchitectuur FPGA-functionaliteit, getrainde neurale netwerken uitvoeren snel en met een lagere latentie. Azure kan vooraf getrainde (DNN) in de deep neural networks parallel op FPGA's uit de service te schalen. De dnn's kunnen worden vooraf getrainde, als een grondige featurizer voor overdracht leren of op uw wensen afgestemd met bijgewerkte waarden.

### <a name="scenarios-and-applications"></a>Scenario's en toepassingen

Azure FPGA's zijn geïntegreerd met Azure Machine Learning. Gebruikt door Microsoft FPGA's voor DNN evaluatie, Bing-zoekresultaten en software gedefinieerde netwerken (SDN)-versnelling om te verlagen, latentie, terwijl het vrijmaken van CPU's voor andere taken.

De volgende scenario's gebruiken FPGA's:
+ [Geautomatiseerd systeem voor netwerkinspectie optische](https://blogs.microsoft.com/ai/build-2018-project-brainwave/)

+ [Land cover toewijzing](https://blogs.technet.microsoft.com/machinelearning/2018/05/29/how-to-use-fpgas-for-deep-learning-inference-to-perform-land-cover-mapping-on-terabytes-of-aerial-images/)

## <a name="deploy-to-fpgas-on-azure"></a>Implementeren op FPGA's op Azure

Voor het maken van een image recognition-service in Azure, kunt u ondersteunde dnn's als een featurizer voor implementatie op Azure FPGA's:

1. Gebruik de [Azure Machine Learning-SDK voor Python](https://aka.ms/aml-sdk) om de servicedefinitie van een te maken. De servicedefinitie van een is een bestand met een beschrijving van een pijplijn van grafieken (invoer, featurizer en classificatie) op basis van TensorFlow. De implementatieopdracht automatisch de definitie en grafieken comprimeert in een ZIP-bestand, en wordt het ZIP-bestand geüpload naar Azure Blob-opslag. De DNN is al geïmplementeerd om uit te voeren op FPGA.

1. Registreer het model met behulp van de SDK met het ZIP-bestand in Azure Blob-opslag.

1. De service met het geregistreerde model implementeren met behulp van de SDK.

Om te beginnen getrainde DNN modellen implementeren op FPGA's in de Azure-cloud, Zie [implementeert een model als een webservice op een FPGA](how-to-deploy-fpga-web-service.md).


## <a name="next-steps"></a>Volgende stappen

Bekijk deze video's en blogs:

+ [Zeer grootschalige hardware: ML op schaal boven op Azure + FPGA: Build 2018 (video)](https://www.youtube.com/watch?v=BMgQAHIx2eY)

+ [In de Microsoft FPGA gebaseerde instelbare cloud (video)](https://channel9.msdn.com/Events/Build/2017/B8063)

+ [Project Brainwave voor realtime AI: project-startpagina](https://www.microsoft.com/research/project/project-brainwave/)
