---
title: Voor beeld van API management-beleid-toegang toestaan met Google OAuth token
titleSuffix: Azure API Management
description: 'Voor beeld van Azure API management-beleid: laat zien hoe u toegang tot uw eind punten als een OAuth-token provider kunt autoriseren.'
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 10/13/2017
ms.author: apimpm
ms.openlocfilehash: 0f6c9fe2146414f78e90d6ade1a00045cdf3a04f
ms.sourcegitcommit: a92fbc09b859941ed64128db6ff72b7a7bcec6ab
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/15/2020
ms.locfileid: "92078014"
---
# <a name="authorize-access-using-google-oauth-token"></a>Toestaan van toegang met behulp van Google-OAuth-token

Dit artikel bevat een voor beeld van een Azure API management-beleid dat laat zien hoe u toegang tot uw eind punten kunt autoriseren als een OAuth-token provider. Volg de stappen die worden beschreven in [een beleid instellen of bewerken](../set-edit-policies.md)om een beleids code in te stellen of te bewerken. Zie voor andere voor beelden [beleids voorbeelden](../policy-reference.md).

## <a name="policy"></a>Beleid

Plak de code in het **binnenkomende** blok.

[!code-xml[Main](../../../api-management-policy-samples/examples/Simple Google OAuth validate-jwt.policy.xml)]

## <a name="next-steps"></a>Volgende stappen

Meer informatie over APIM-beleid:

+ [Transformatiebeleid](../api-management-transformation-policies.md)
+ [Voor beelden van beleid](../policy-reference.md)