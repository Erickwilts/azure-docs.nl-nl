---
title: Achter een firewall - API van Translator tekst vertalen
titlesuffix: Azure Cognitive Services
description: Vertalen achter een firewall voor IP-met de Translator Text-API.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: swmachan
ms.openlocfilehash: d0ebce2dd41b170a18057ca76dd3ae4cf3ce0be2
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67435985"
---
# <a name="how-to-translate-behind-ip-firewalls-with-the-translator-text-api"></a>Hoe u achter een firewall voor IP-met de API van Translator tekst vertalen

Translator Text-API kunt vertalen achter firewalls met behulp van-naam of IP-filters. Domeinnaam filtering is de aanbevolen methode. We **wordt niet aanbevolen** firewall uitvoeren van Microsoft Translator via een IP-adres worden gefilterd. De installatie is het waarschijnlijk om in de toekomst zonder voorafgaande kennisgeving te verbreken.

## <a name="translator-ip-addresses"></a>NAT IP-adressen
De IP-adressen voor api.cognitive.microsofttranslator.com - Microsoft Translator Text-API vanaf 20 November 2018:

* **Azië en Stille Oceaan:** 40.90.139.163, 104.44.89.44
* **Europa:** 40.90.138.4, 40.90.141.99
* **Noord-Amerika:** 40.90.139.36, 40.90.139.2


## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Vertalen achter firewalls in uw Translator-API-aanroep IP](reference/v3-0-translate.md)
