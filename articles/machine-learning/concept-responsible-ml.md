---
title: Wat is er verantwoordelijk machine learning (preview-versie)
titleSuffix: Azure Machine Learning
description: Meer informatie over de verantwoordelijke machine learning is en hoe u deze kunt gebruiken in Azure Machine Learning
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: luquinta
author: luisquintanilla
ms.date: 08/05/2020
ms.openlocfilehash: 689b90fc1f45faad72640f47e5eebe936d2dc8b7
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "87829387"
---
# <a name="what-is-responsible-machine-learning-preview"></a>Wat is er verantwoordelijk machine learning? (preview)

In dit artikel leert u wat verantwoordelijk machine learning (ML) is en hoe u deze in de praktijk kunt plaatsen met Azure Machine Learning.

## <a name="responsible-machine-learning-principles"></a>Verantwoordelijke machine learning principes

Tijdens de ontwikkeling en het gebruik van AI-systemen moet de vertrouwens relatie de kern zijn. Vertrouwen in het platform, het proces en de modellen. Bij micro soft heeft de verantwoordelijke machine learning de volgende waarden en principes:

- machine learning modellen begrijpen
  - Model gedrag interpreteren en uitleggen
  - Model oneerlijkheid beoordelen en beperken
- Personen en hun gegevens beveiligen
  - Voor komen dat gegevens bloot vallen met differentiële privacy
  - Werken met versleutelde gegevens met behulp van Homomorphic-versleuteling
- Het end-to-end-machine learning proces beheren
  - De levens cyclus van machine learning documenteren met gegevens bladen

:::image type="content" source="media/concept-responsible-ml/responsible-ml-pillars.png" alt-text="Verantwoordelijk ML van pijlers: interpreteer bare, differentiële privacy, Homomorphic versleuteling, audittrail Azure Machine Learning":::

Aangezien kunst matige intelligentie en autonome systemen meer kunnen worden geïntegreerd in de infra structuur van de samenleving, is het belang rijk om proactief een inspanning te maken en de onbedoelde gevolgen van deze technologieën te verhelpen.

## <a name="interpret-and-explain-model-behavior"></a>Model gedrag interpreteren en uitleggen

Moeilijk te verklaren of ondoorzichtige systemen kunnen problemen veroorzaken omdat het moeilijk is voor belanghebbenden zoals systeem ontwikkelaars, regelgevende instanties, gebruikers en besluit vormers om te begrijpen waarom systemen bepaalde beslissingen nemen. Sommige AI-systemen zijn beter verklaren dan andere en er is soms een afweging tussen een systeem met nauw keurigheid en een meer uitleg.

Gebruik [InterpretML](https://github.com/interpretml/interpret), een open-source pakket dat is gebouwd door micro soft, voor het bouwen van interpretatieve AI-systemen. [InterpretML kan worden gebruikt in azure machine learning](how-to-machine-learning-interpretability.md) voor [het interpreteren en uitleggen van uw machine learning modellen](how-to-machine-learning-interpretability-aml.md), inclusief [geautomatiseerde machine learning modellen](how-to-machine-learning-interpretability-automl.md).

## <a name="mitigate-fairness-in-machine-learning-models"></a>Verdeling in machine learning modellen beperken

Naarmate AI-systemen meer betrokken zijn bij de alledaagse besluit vorming van de samenleving, is het uiterst belang rijk dat deze systemen goed werken voor het leveren van eerlijke resultaten voor iedereen.

Oneerlijkheid in AI-systemen kan de volgende onbedoelde gevolgen hebben:

- Het inhouden van verkoop kansen, resources of informatie van personen.
- Versterking van BIASS en stereo typen.

Veel aspecten van verdeling kunnen niet worden vastgelegd of vertegenwoordigd door metrische gegevens. Er zijn hulpprogram ma's en procedures waarmee verdeling in het ontwerp en de ontwikkeling van AI-systemen kunnen worden verbeterd.

Er zijn twee belang rijke stappen om de oneerlijkheid in AI-systemen te verminderen. We raden [FairLearn](https://github.com/fairlearn/fairlearn)aan, een open-source pakket waarmee de potentiële oneerlijkheid van AI-systemen kan worden beoordeeld en verholpen. Zie het [artikel verdeling in ml](./concept-fairness-ml.md)voor meer informatie over verdeling en het FairLearn-pakket.

## <a name="prevent-data-exposure-with-differential-privacy"></a>Voor komen dat gegevens bloot vallen met differentiële privacy

Wanneer gegevens worden gebruikt voor analyse, is het belang rijk dat de gegevens persoonlijk en vertrouwelijk blijven in het gebruik. Differentiële privacy is een verzameling systemen en procedures die u helpen de gegevens van personen veilig en privé te houden.

In traditionele scenario's worden onbewerkte gegevens opgeslagen in bestanden en data bases. Wanneer gebruikers gegevens analyseren, gebruiken ze doorgaans de onbewerkte gegevens. Dit is een probleem omdat dit kan leiden tot inbreuk op de privacy van een persoon. Differentiële privacy probeert dit probleem te verhelpen door "Noise" of wille keurigheid toe te voegen aan de gegevens, zodat gebruikers geen afzonderlijke gegevens punten kunnen identificeren.

Het implementeren van differentiatie van een differentieel particulier systeem is lastig. [WhiteNoise](https://github.com/opendifferentialprivacy/whitenoise-core) is een open-source project dat verschillende onderdelen bevat voor het bouwen van wereld wijde differentiatie systemen. Voor meer informatie over de differentiële privacy en het WhiteNoise-project raadpleegt u het artikel privacy [beschermen met behulp van differentiële privacy en WhiteNoise](./concept-differential-privacy.md) .

> [!NOTE]
> De naam van de Toolkit wordt gewijzigd en de nieuwe naam wordt in de komende weken geïntroduceerd. 

## <a name="work-on-encrypted-data-with-homomorphic-encryption"></a>Werken met versleutelde gegevens met Homomorphic-versleuteling

In traditionele Cloud opslag-en berekenings oplossingen moet de Cloud niet-versleutelde toegang hebben tot klant gegevens om deze te berekenen. Met deze toegang worden de gegevens in de Cloud operators weer gegeven. Data privacy is afhankelijk van het toegangs beheer beleid dat door de Cloud wordt geïmplementeerd en wordt vertrouwd door de klant.

Met Homomorphic versleuteling kunnen berekeningen worden uitgevoerd op versleutelde gegevens zonder dat daarvoor toegang nodig is tot een geheime sleutel (ontsleuteling). De resultaten van de berekeningen zijn versleuteld en kunnen alleen worden onthuld door de eigenaar van de geheime sleutel. Met Homomorphic-versleuteling hebben Cloud operators nooit niet-versleutelde toegang tot de gegevens die ze opslaan en verwerken. Berekeningen worden direct uitgevoerd op versleutelde gegevens. Data privacy is afhankelijk van geavanceerde crypto grafie en de gegevens eigenaar beheert alle informatie releases. Zie [micro soft Research](https://www.microsoft.com/research/project/homomorphic-encryption/)voor meer informatie over Homomorphic-versleuteling bij micro soft.

Om aan de slag te gaan met Homomorphic-versleuteling in Azure Machine Learning gebruikt u de [versleutelde python-](https://pypi.org/project/encrypted-inference/) bindingen voor het afdwingen van [micro soft-zegels](https://github.com/microsoft/SEAL). Micro soft afdichting is een open-source Homomorphic-versleutelings bibliotheek waarmee toevoegingen en vermenigvuldigingen kunnen worden uitgevoerd op versleutelde gehele getallen of echte getallen. Zie de pagina [Azure Architecture Center](https://docs.microsoft.com/azure/architecture/solution-ideas/articles/homomorphic-encryption-seal) of [micro soft Research project](https://www.microsoft.com/research/project/microsoft-seal/)(Engelstalig) voor meer informatie over micro soft-zegels.

Raadpleeg het volgende voor beeld voor meer informatie [over het implementeren van een versleutelde webservice in azure machine learning](how-to-homomorphic-encryption-seal.md).

## <a name="document-the-machine-learning-lifecycle-with-datasheets"></a>De levens cyclus van machine learning documenteren met gegevens bladen

Het documenteren van de juiste informatie in het machine learning proces is essentieel voor het maken van verantwoordelijke beslissingen in elke fase. Gegevens bladen zijn een manier om machine learning activa te documenteren die worden gebruikt en gemaakt als onderdeel van de levens cyclus van machine learning.

Modellen worden meestal gezien als ' dekkende dozen ' en vaak is er weinig informatie over deze systemen. Omdat machine learning systemen steeds betrouwbaarder worden en worden gebruikt voor het nemen van beslissingen, is het gebruik van gegevens bladen een stap voor het ontwikkelen van meer verantwoordelijke machine learning systemen.

Sommige model gegevens die u mogelijk wilt documenteren als onderdeel van een gegevens blad:

- Beoogd gebruik
- Model architectuur
- Gebruikte trainings gegevens
- Gebruikte evaluatie gegevens
- Metrische prestatie gegevens van trainings model
- Verdeling-gegevens.

Raadpleeg het volgende voor beeld om te leren hoe u de Azure Machine Learning SDK kunt gebruiken om [gegevens bladen voor modellen](https://github.com/microsoft/MLOps/blob/master/pytorch_with_datasheet/model_with_datasheet.ipynb)te implementeren.

## <a name="additional-resources"></a>Aanvullende bronnen

- Raadpleeg voor meer informatie de [verantwoordelijke vernieuwings Toolkit](https://docs.microsoft.com/azure/architecture/guide/responsible-innovation/) voor informatie over best practices.
- Meer informatie over de set met richt lijnen [over milliliters](https://www.partnershiponai.org/about-ml/) voor machine learning systeem documentatie.
