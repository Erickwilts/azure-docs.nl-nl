---
title: Toegestane CA voor het inschakelen van aangepaste HTTPS op Azure front-deur
description: Als u uw eigen certificaat gebruikt om HTTPS in te scha kelen op een aangepast Azure front-deur domein, moet u een toegestane certificerings instantie (CA) gebruiken om deze te maken.
services: frontdoor
documentationcenter: ''
author: duongau
ms.assetid: ''
ms.service: frontdoor
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/30/2020
ms.author: duau
ms.openlocfilehash: 20c5d611272ee2159ce8ddcc2865797a225a7ebb
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "91613676"
---
# <a name="allowed-certificate-authorities-for-enabling-custom-https-on-azure-front-door"></a>Toegestane certificerings instanties voor het inschakelen van aangepaste HTTPS op Azure front-deur

Wanneer u [de https-functie inschakelt met uw eigen certificaat](front-door-custom-domain-https.md?tabs=option-2-enable-https-with-your-own-certificate) voor een aangepast Azure front deur-domein. U hebt een toegestane certificerings instantie (CA) nodig om uw TLS/SSL-certificaat te maken. Anders wordt de aanvraag geweigerd als u een niet-toegestane CA of een zelfondertekend certificaat gebruikt.

[!INCLUDE [cdn-front-door-allowed-ca](../../includes/cdn-front-door-allowed-ca.md)]
