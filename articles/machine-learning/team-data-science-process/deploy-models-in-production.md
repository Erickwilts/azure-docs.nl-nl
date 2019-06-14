---
title: Implementeren van modellen in productie - Team Data Science Process
description: Klik hier voor meer informatie over het implementeren van modellen voor productie, zodat ze een actieve rol bij het maken van zakelijke beslissingen te nemen.
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/30/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 1a75c842989cfbaf7bb1880831fda2bc6994622b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60238348"
---
# <a name="deploy-models-to-production-to-play-an-active-role-in-making-business-decisions"></a>Modellen implementeren naar productie om af te spelen een actieve rol bij het maken van zakelijke beslissingen te nemen

Productie-implementatie kunt een model voor het een actieve rol spelen in een bedrijf. Voorspellingen op basis van een geïmplementeerd model kunnen worden gebruikt voor zakelijke beslissingen te nemen.

## <a name="production-platforms"></a>Productieplatforms

Er zijn verschillende benaderingen en platforms modellen in productie te plaatsen. Hier zijn enkele opties:

- [Waar u wilt implementeren van modellen met Azure Machine Learning-service](../service/how-to-deploy-and-where.md)
- [Implementatie van een model in SQL-server](https://docs.microsoft.com/sql/advanced-analytics/tutorials/sqldev-py6-operationalize-the-model)
- [Microsoft Machine Learning Server](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone)

>[!NOTE]
>Voorafgaand aan de implementatie heeft een zorgen dat de latentie van het model scoren laag genoeg is voor gebruik in productie is.
>

>[!NOTE]
>Zie voor een implementatie met behulp van Azure Machine Learning Studio, [een Azure Machine Learning-webservice implementeren](../studio/publish-a-machine-learning-web-service.md).
>

## <a name="ab-testing"></a>A / B-tests

Als meerdere modellen in productie, kan het nuttig zijn om uit te voeren [A / B-tests](https://en.wikipedia.org/wiki/A/B_testing) kunnen de prestaties van de modellen. 
 
## <a name="next-steps"></a>Volgende stappen

Scenario's die laten zien van alle de stappen in het proces voor het **specifieke scenario's** worden ook gegeven. Ze worden weergegeven en die is gekoppeld met miniaturen beschrijvingen in de [voorbeeld walkthroughs](walkthroughs.md) artikel. Ze laten zien hoe u naar de cloud, on-premises hulpprogramma's en services combineren in een werkstroom of een pijplijn te maken van een intelligente toepassingen. 
