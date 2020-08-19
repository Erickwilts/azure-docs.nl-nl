---
author: IngridAtMicrosoft
ms.service: media-services
ms.topic: include
ms.date: 08/17/2020
ms.author: inhenkel
ms.custom: CLI
ms.openlocfilehash: aa76f7b85302651f6874747610c3355f0572a7ee
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2020
ms.locfileid: "88556292"
---
<!-- List and set subscriptions -->

1. U kunt een lijst met uw abonnementen opvragen met de opdracht [az account list](/cli/azure/account#az-account-list):

    ```
    az account list --output table
    ```

2. Gebruik `az account set` met de abonnements-ID of -naam waarnaar u wilt overschakelen.

    ```
    az account set --subscription "My Demos"
    ```
