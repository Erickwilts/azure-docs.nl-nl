---
title: Ondersteuning voor containers
titleSuffix: Azure Cognitive Services
description: Meer informatie over het maken van een Azure container instance-resource.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 11/21/2019
ms.author: dapine
ms.openlocfilehash: 18f4edf5cc63a448779423cc1b302130b4b80724
ms.sourcegitcommit: 2f8ff235b1456ccfd527e07d55149e0c0f0647cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/07/2020
ms.locfileid: "75692518"
---
## <a name="create-an-azure-container-instance-resource"></a>Een Azure container instance-resource maken

1. Ga naar de pagina [maken](https://ms.portal.azure.com/#create/Microsoft.ContainerInstances) voor container instances.

2. Voer op het tabblad **basis beginselen** de volgende gegevens in:

    |Instelling|Waarde|
    |--|--|
    |Abonnement|Selecteer uw abonnement.|
    |Resourcegroep|Selecteer de beschik bare resource groep of maak een nieuwe, zoals `cognitive-services`.|
    |Containernaam|Voer een naam in, zoals `cognitive-container-instance`. De naam moet in kleine letters worden gereduceerd.|
    |Locatie|Selecteer een regio voor de implementatie.|
    |Type installatiekopie|Als uw container installatie kopie is opgeslagen in een container register waarvoor geen referenties zijn vereist, kiest u `Public`. Als u toegang hebt tot de container installatie kopie, moet u `Private`kiezen. Raadpleeg [container opslagplaatsen en installatie kopieën](../../cognitive-services-container-support.md#container-repositories-and-images) voor meer informatie over of de container installatie kopie is `Public` of `Private` (open bare preview). |
    |Naam van installatiekopie|Voer de locatie van de Cognitive Services-container in. De locatie is wat wordt gebruikt als argument voor de opdracht `docker pull`. Raadpleeg de [container opslagplaatsen en installatie kopieën](../../cognitive-services-container-support.md#container-repositories-and-images) voor de beschik bare afbeeldings namen en de bijbehorende opslag plaats.<br><br>De naam van de installatie kopie moet volledig gekwalificeerd zijn om drie delen op te geven. Ten eerste, het container register, vervolgens de naam van de installatie kopie: `<container-registry>/<repository>/<image-name>`.<br><br>Hier volgt een voor beeld `mcr.microsoft.com/azure-cognitive-services/keyphrase` de Sleuteltermextractie installatie kopie in de micro soft Container Registry onder de Azure Cognitive Services-opslag plaats. Een ander voor beeld is `containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text` dat de tekst afbeelding speech weergeeft in de micro soft-opslag plaats van het container voorbeeld container register. |
    |Type besturingssysteem|`Linux`|
    |Grootte|Wijzig grootte in de voorgestelde aanbevelingen voor uw specifieke cognitieve service-container:<br>twee CPU-kernen<br>4 GB

3. Voer op het tabblad **netwerken** de volgende gegevens in:

    |Instelling|Waarde|
    |--|--|
    |Poorten|Stel de TCP-poort in op `5000`. Beschrijft de container op poort 5000.|

4. Voer op het tabblad **Geavanceerd** de vereiste **omgevings variabelen** in voor de instellingen van de container facturering van de Azure container Instance-bron:

    | Sleutel | Waarde |
    |--|--|
    |`apikey`|Wordt gekopieerd van de pagina **sleutels** van de resource. Het is een teken reeks van 32 alfanumerieke tekens zonder spaties of streepjes `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`.|
    |`billing`|Wordt gekopieerd van de pagina **overzicht** van de resource.|
    |`eula`|`accept`|

5. Klik op **controleren en maken**
6. Nadat de validatie is geslaagd, klikt u op **maken** om het aanmaak proces te volt ooien
7. Wanneer de resource is geïmplementeerd, is deze gereed
