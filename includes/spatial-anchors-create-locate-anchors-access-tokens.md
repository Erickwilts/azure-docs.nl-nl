---
author: ramonarguelles
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 02/21/2019
ms.author: rgarcia
ms.openlocfilehash: 63725d55e2b2935ec6a899789249259b096865c3
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66110630"
---
### <a name="access-tokens"></a>Toegangstokens

Toegangstokens zijn een meer robuuste methode voor verificatie met Azure ruimtelijke ankers. Met name bij het voorbereiden van uw toepassing voor een productie-implementatie. De samenvatting van deze benadering is voor het instellen van een back-end-service die uw clienttoepassing kan veilig worden geverifieerd met. Uw back-end-service-interfaces met AAD tijdens runtime en de Azure ruimtelijke ankers Secure Token Service om aan te vragen een toegangstoken. Dit token wordt vervolgens geleverd aan de clienttoepassing en in de SDK gebruikt voor verificatie met Azure ruimtelijke ankers.
