---
title: bestand opnemen
description: bestand opnemen
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 11/02/2020
ms.openlocfilehash: 269242e61b1f20221ddb3ff3d251bf9cd5c7108a
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/04/2020
ms.locfileid: "93322274"
---
Het rekendoel dat u gebruikt als host voor uw model, is van invloed op de kosten en beschikbaarheid van het geïmplementeerde eindpunt. Gebruik deze tabel om een geschikt rekendoel te kiezen.

| Rekendoel | Gebruikt voor | GPU-ondersteuning | FPGA-ondersteuning | Beschrijving |
| ----- | ----- | ----- | ----- | ----- |
| [Lokale&nbsp;web&nbsp;service](../articles/machine-learning/how-to-deploy-local-container-notebook-vm.md) | Testen/fouten opsporen | &nbsp; | &nbsp; | Gebruiken voor testen en problemen oplossen. Hardwareversnelling is afhankelijk van het gebruik van bibliotheken in het lokale systeem.
| [Azure Kubernetes Service (AKS)](../articles/machine-learning/how-to-deploy-azure-kubernetes-service.md) | Realtime deductie |  [Ja](../articles/machine-learning/how-to-deploy-inferencing-gpus.md) (webservice-implementatie) | [Ja](../articles/machine-learning/how-to-deploy-fpga-web-service.md)   |Gebruiken voor grootschalige productie-implementaties. Biedt een snelle reactietijd en automatische schaalaanpassing van de geïmplementeerde service. Automatische schaalaanpassing van clusters wordt niet ondersteund via de Azure Machine Learning SDK. Als u de knooppunten in het AKS-cluster wilt wijzigen, gebruikt u de gebruikersinterface voor uw AKS-cluster in de Azure-portal. AKS is de enige optie die beschikbaar is voor de ontwerpfunctie. |
| [Azure Container Instances](../articles/machine-learning/how-to-deploy-azure-container-instance.md) | Testen of ontwikkelen | &nbsp;  | &nbsp; | Gebruiken voor lage CPU-werkbelastingen waarvoor minder dan 48 GB RAM-geheugen nodig is. |
| [Azure Machine Learning-rekenclusters](../articles/machine-learning/how-to-use-parallel-run-step.md) | Batch&nbsp;deductie | [Ja](../articles/machine-learning/how-to-use-parallel-run-step.md) (machine learning-pijplijn) | &nbsp;  | Batchscoreberekening uitvoeren op serverloze berekening. Ondersteunt VM's met normale en lage prioriteit. |

> [!NOTE]
> Hoewel GPU voor trainings- en testdoeleinden wordt ondersteund voor rekendoelen als lokale Azure Machine Learning-rekeninstanties en Azure Machine Learning-rekenclusters, wordt het gebruik van GPU voor deductie _bij implementatie als een webservice_ alleen ondersteund in AKS.
>
> Het gebruik van een GPU voor deductie _bij scoreberekening met een machine learning-pijplijn_ wordt alleen ondersteund in Azure Machine Learning Compute.

> [!NOTE]
> * Containerinstanties zijn alleen geschikt voor kleine modellen met een grootte van minder dan 1 GB.
> * Gebruik AKS clusters met één knooppunt voor het ontwikkelen en testen van grotere modellen.