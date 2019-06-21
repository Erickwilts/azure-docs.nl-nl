---
title: Limieten en quota - Custom Vision Service
titlesuffix: Azure Cognitive Services
description: Meer informatie over de limieten en quota voor de Custom Vision Service.
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 03/25/2019
ms.author: anroth
ms.openlocfilehash: 9cff5fdac39be2338305cd37a4b2328a28a48255
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67269271"
---
# <a name="limits-and-quotas"></a>Limieten en quota

Er zijn twee lagen van sleutels voor de Custom Vision service. U kunt zich registreren voor een F0 (gratis) of een nieuw S0 (standaard)-abonnement via de Azure-portal. Zie het bijbehorende [pagina met prijzen van Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/custom-vision-service/) voor meer informatie over prijzen en transacties.

Het aantal trainingsafbeeldingen per project en tags per project worden naar verwachting verhoogd na verloop van tijd voor S0-projecten.

||**F0**|**S0**|
|-----|-----|-----|
|Projecten|2|100|
|Trainingsafbeeldingen per project |5,000|100.000|
|Voorspellingen / maand|10.000 |Onbeperkt|
|Tags / project|50|500|
|Iteraties |10|10|
|Min gelabelde afbeeldingen per Tag, classificatie (meer dan 50 aanbevolen) |5|5|
|Min gelabelde afbeeldingen per Tag, detectie van Object (meer dan 50 aanbevolen)|15|15|
|Hoe lang voorspelling installatiekopieën worden opgeslagen|30 dagen|30 dagen|
|[Voorspelling](https://go.microsoft.com/fwlink/?linkid=865445) bewerkingen met storage (transacties Per seconde)|2|10|
|[Voorspelling](https://go.microsoft.com/fwlink/?linkid=865445) bewerkingen zonder opslag (transacties Per seconde)|2|20|
|[TrainProject](https://go.microsoft.com/fwlink/?linkid=865446) (API-aanroepen Per seconde)|2|10|
|[Andere API-aanroepen](https://go.microsoft.com/fwlink/?linkid=865446) (transacties Per seconde)|10|10|
|Maximale afbeeldingsgrootte (training installatiekopie uploaden) |6 MB|6 MB|
|Maximale afbeeldingsgrootte (voorspelling)|4 MB|4 MB|
|Maximum aantal regio's per object detectie training afbeelding|200|200|
|Maximum aantal tags per classificatie-afbeelding|30|30|
