---
title: bestand opnemen
description: bestand opnemen
services: functions
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 04/24/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 48bb91b3b2e9a31de63e515edb857bc2a170ea79
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66132261"
---
## <a name="deploy-the-function-app-project-to-azure"></a>Het functie-appproject implementeren in Azure

Nadat de functie-app is gemaakt in Azure, kunt u de [ `func azure functionapp publish` ](../articles/azure-functions/functions-run-local.md#project-file-deployment) Core Tools-opdracht voor het implementeren van de projectcode van uw naar Azure. In de volgende opdracht, vervangt u `<APP_NAME>` met de naam van uw app uit de vorige stap.

```bash
func azure functionapp publish <APP_NAME>
```

Hier ziet u uitvoer die vergelijkbaar is met het volgende voorbeeld, waarin is afgekapt voor de leesbaarheid.

```output
Getting site publishing info...
...

Preparing archive...
Uploading content...
Upload completed successfully.
Deployment completed successfully.
Syncing triggers...
Functions in myfunctionapp:
    HttpTrigger - [httpTrigger]
        Invoke url: https://myfunctionapp.azurewebsites.net/api/httptrigger?code=cCr8sAxfBiow548FBDLS1....
```

Kopieer de aanroepen van URL-waarde voor uw HttpTrigger, waarin u nu kunt uw functie testen in Azure. De URL bevat een `code` query string-waarde die is de sleutel van uw functie. Deze sleutel waardoor het lastig is voor anderen om aan te roepen van het eindpunt van uw HTTP-trigger in Azure.