---
title: Vereiste para meters verzamelen
services: cognitive-services
author: IEvangelist
manager: nitinme
description: De para meters voor alle Cognitive Services containers
ms.service: cognitive-services
ms.topic: include
ms.date: 7/24/2019
ms.author: dapine
ms.openlocfilehash: 636a41fde345a08db1549e53626522962f9cf74f
ms.sourcegitcommit: bafb70af41ad1326adf3b7f8db50493e20a64926
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/25/2019
ms.locfileid: "68488781"
---
## <a name="gathering-required-parameters"></a>Vereiste para meters verzamelen

Er zijn drie primaire para meters voor alle vereiste containers van Cognitive Services. De gebruiksrecht overeenkomst (**EULA**) moet aanwezig zijn met een waarde van `accept`. Daarnaast zijn zowel een eind punt-URL als een API-sleutel nodig.

### <a name="endpoint-uri-endpointuri"></a>Eind punt-URI`{Endpoint_URI}`

De URI-waarde van het **eind punt** is beschikbaar op de pagina *overzicht* van Azure portal van de bijbehorende cognitieve service resource. Ga naar de *overzichts* pagina, beweeg de muis aanwijzer over het `Copy to clipboard` eind punt en er wordt een <span class="docon docon-edit-copy x-hidden-focus"></span> pictogram weer gegeven. Kopieer en gebruik waar nodig.

![De URI van het eind punt verzamelen voor later gebruik](../media/overview-endpoint-uri.png)

### <a name="keys-apikey"></a>Subknooppuntsleutels`{API_Key}`

Deze sleutel wordt gebruikt om de container te starten en is beschikbaar op de pagina sleutels van de Azure Portal van de bijbehorende cognitieve service resource. Ga naar de pagina *sleutels* en klik op het `Copy to clipboard` <span class="docon docon-edit-copy x-hidden-focus"></span> pictogram.

![Een van de twee sleutels ophalen voor later gebruik](../media/keys-copy-api-key.png)

> [!IMPORTANT]
> Deze abonnements sleutels worden gebruikt om toegang te krijgen tot uw API voor de cognitieve service. Deel uw sleutels niet. Sla ze veilig op, bijvoorbeeld met behulp van Azure Key Vault. We raden u ook aan deze sleutels regel matig opnieuw te genereren. Er is slechts één sleutel nodig om een API-aanroep te maken. Wanneer u de eerste sleutel opnieuw genereert, kunt u de tweede sleutel gebruiken voor verdere toegang tot de service.