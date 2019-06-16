---
title: Opmerkingen bij de release - Face-API-Service
titleSuffix: Azure Cognitive Services
description: Opmerkingen bij de release voor de Face-API-Service bevatten een geschiedenis van wijzigingen in de release voor verschillende versies.
services: cognitive-services
author: yluiu
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 06/06/2019
ms.author: yluiu
ms.openlocfilehash: a7667f94d3f4dea2901c4b4b0e2b2c893b9f535e
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67074099"
---
# <a name="face-api-release-notes"></a>Opmerkingen bij de Release van Face-API

In dit artikel geldt voor de Face-API-Service, versie 1.0.

### <a name="release-changes-in-june-2019"></a>Wijzigingen van de release in juni 2019

* Een nieuw model voor face met betere nauwkeurigheid op kleine, side-weergave, occluded en fuzzy gezichten toegevoegd. Gebruikt u deze via [geconfronteerd - detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236), [FaceList - Face toevoegen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250), [LargeFaceList - Face toevoegen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158c10d2de3616c086f2d3), [PersonGroup persoon - Face toevoegen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) en [LargePersonGroup persoon - Face toevoegen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adf2a3a7b9412a4d53f42) door te geven van de nieuwe modelnaam voor face detection `detection_02` in `detectionModel` parameter. Meer informatie in [opgeven van een model voor](Face-API-How-to-Topics/specify-detection-model.md).

### <a name="release-changes-in-april-2019"></a>Wijzigingen van de release in April 2019

* Verbeterde algehele nauwkeurigheid van de `age` en `headPose` kenmerken. De `headPose` kenmerk is tevens bijgewerkt met de `pitch` waarde is nu ingeschakeld. Deze kenmerken gebruiken door op te geven in de `returnFaceAttributes` parameter van [geconfronteerd - detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) `returnFaceAttributes` parameter. 

* Verbeterde snelheid van [geconfronteerd - detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236), [FaceList - Face toevoegen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250), [LargeFaceList - Face toevoegen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158c10d2de3616c086f2d3), [PersonGroup persoon - Face toevoegen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) en [ LargePersonGroup persoon - toevoegen Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adf2a3a7b9412a4d53f42).

### <a name="release-changes-in-march-2019"></a>Release-wijzigingen in maart 2019

* Een nieuw model voor de face-opname met een betere nauwkeurigheid toegevoegd. Gebruik deze via [geconfronteerd - detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236), [FaceList - maken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524b), [LargeFaceList - maken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc), [PersonGroup - maken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244) en [ LargePersonGroup - maken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d) door te geven van de nieuwe modelnaam voor face erkenning `recognition_02` in `recognitionModel` parameter. Meer informatie in [opgeven van een model erkenning](Face-API-How-to-Topics/specify-recognition-model.md).

### <a name="release-changes-in-january-2019"></a>Release-wijzigingen in januari 2019

* Momentopname-functie toegevoegd ter ondersteuning van de gegevensmigratie voor abonnementen: [Momentopname](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/snapshot-get). Meer informatie in [hoe u uw face om gegevens te migreren naar een ander abonnement met Face](Face-API-How-to-Topics/how-to-migrate-face-data.md).

### <a name="release-changes-in-october-2018"></a>Release-wijzigingen in oktober 2018

* Beschrijving voor verfijnd `status`, `createdDateTime`, `lastActionDateTime`, en `lastSuccessfulTrainingDateTime` in [PersonGroup - Training-Status ophalen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395247), [LargePersonGroup - Training-Status ophalen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599ae32c6ac60f11b48b5aa5), en [ LargeFaceList - Get-trainingsstatus](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a1582f8d2de3616c086f2cf).

### <a name="release-changes-in-may-2018"></a>Wijzigingen van de release in mei 2018

* Verbeterde `gender` kenmerk aanzienlijk en ook verbeterd `age`, `glasses`, `facialHair`, `hair`, `makeup` kenmerken. Gebruik via [geconfronteerd - detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) `returnFaceAttributes` parameter. 

* Verbeterde afbeelding maximale bestandsgrootte van 4 MB tot 6 MB in [geconfronteerd - detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236), [FaceList - Face toevoegen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250), [LargeFaceList - Face toevoegen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158c10d2de3616c086f2d3), [PersonGroup persoon - toevoegen Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) en [LargePersonGroup persoon - toevoegen Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adf2a3a7b9412a4d53f42).

### <a name="release-changes-in-march-2018"></a>Release-wijzigingen in maart 2018

* Toegevoegde miljoen schaal Container: [LargeFaceList](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc) en [LargePersonGroup](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d). Meer informatie in [over het gebruik van de functie voor grootschalige](Face-API-How-to-Topics/how-to-use-large-scale.md).

* Verhoogd [geconfronteerd - identificeren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) `maxNumOfCandidatesReturned` parameter uit [1, 5] naar [1, 100] en standaard 10.

### <a name="release-changes-in-may-2017"></a>Wijzigingen van de release in mei 2017

* Toegevoegd `hair`, `makeup`, `accessory`, `occlusion`, `blur`, `exposure`, en `noise` kenmerken in [geconfronteerd - detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) `returnFaceAttributes` parameter.

* 10K personen worden ondersteund in een PersonGroup en [geconfronteerd - identificeren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239).

* Paginering in ondersteund [PersonGroup persoon - lijst](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395241) met optionele parameters: `start` en `top`.

* Ondersteunde gelijktijdigheid van taken bij het toevoegen/verwijderen van gezichten op basis van verschillende FaceLists en andere personen in PersonGroup.

### <a name="release-changes-in-march-2017"></a>Release-wijzigingen in maart 2017
* Toegevoegd `emotion` kenmerk in [geconfronteerd - detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) `returnFaceAttributes` parameter.

* Vaste het gezicht kan niet opnieuw worden gedetecteerd met rechthoek geretourneerd door [geconfronteerd - detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) als `targetFace` in [FaceList - Face toevoegen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250) en [PersonGroup persoon - Face toevoegen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b).

* De grootte van de face-detecteerbare om te controleren of dat het is uitsluitend tussen 36 x 36 naar 4096 x 4096 pixels opgelost.

### <a name="release-changes-in-november-2016"></a>Release-wijzigingen in November 2016
* Face Storage Standard-abonnement extra permanente gezichten opslaan wanneer u toegevoegd [PersonGroup persoon - Face toevoegen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) of [FaceList - Face toevoegen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250) voor identificatie of gelijkenis die overeenkomt met. De opgeslagen afbeeldingen worden als volgt in rekening gebracht: $0,5 per 1000 gezichten. Dit tarief is per dag pro rato. De laag gratis abonnementen nog steeds beperkt tot 1000 totale personen.

### <a name="release-changes-in-october-2016"></a>Wijzigingen van de release oktober 2016
* Het foutbericht van meer dan één gezicht in de targetFace van 'Zijn meer dan één gezicht in de afbeelding' naar 'Is er meer dan één gezicht in de afbeelding' gewijzigd in [FaceList - Face toevoegen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250) en [PersonGroup persoon - toevoegen Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b).

### <a name="release-changes-in-july-2016"></a>Wijzigingen van de release in juli 2016
* Face ondersteund tot persoon object verificatie in [geconfronteerd: controleren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a).

* Optionele toegevoegd `mode` parameter selectie van de twee modi van de werkende inschakelen: `matchPerson` en `matchFace` in [Face - Zoek vergelijkbare](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) en de standaardwaarde is `matchPerson`.

* Optionele toegevoegd `confidenceThreshold` parameter voor de gebruiker om in te stellen de drempelwaarde van een gezicht is of een object persoon in [geconfronteerd - identificeren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239).

* Optionele toegevoegd `start` en `top` parameters in [PersonGroup - lijst](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395248) zodat de gebruiker het beginpunt en het totale aantal PersonGroups aan lijst op te geven.

### <a name="v10-changes-from-v0"></a>Wijzigingen van V0 V1.0
* Root-service-eindpunt van bijgewerkt ```https://westus.api.cognitive.microsoft.com/face/v0/``` naar ```https://westus.api.cognitive.microsoft.com/face/v1.0/```. Wijzigingen zijn toegepast op: [Face - detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236), [geconfronteerd - identificeren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239), [geconfronteerd - Zoek vergelijkbare](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) en [geconfronteerd - groep](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238).

* De grootte van de minimale detecteerbare face bijgewerkt naar 36 x 36 pixels. Gezichten die kleiner zijn dan 36 x 36 pixels niet gedetecteerd.

* De gegevens PersonGroup en persoon in Face V0 afgeschaft. Deze gegevens kan niet worden geopend met de Face V1.0-service.

* Het eindpunt V0 van Face-API op 30 juni 2016 afgeschaft.
