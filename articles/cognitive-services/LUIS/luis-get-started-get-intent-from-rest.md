---
title: "Quick Start: profiteren van intenties met REST-Api's-LUIS"
description: Gebruik in deze REST API Snelstartgids een beschik bare open bare LUIS-app om de bedoeling van een gebruiker te bepalen op basis van de conversatie tekst.
ms.topic: quickstart
ms.date: 05/18/2020
ms.custom: tracking-python
zone_pivot_groups: programming-languages-set-one
ms.openlocfilehash: 5d43a1aee9e3f355a3cfcab927d87571798677e7
ms.sourcegitcommit: 1de57529ab349341447d77a0717f6ced5335074e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/09/2020
ms.locfileid: "84608550"
---
# <a name="quickstart-get-intent-with-rest-apis"></a>Quick Start: profiteren van de bedoeling van REST-Api's

In deze Snelstartgids gebruikt u een LUIS-app om de bedoeling van een gebruiker te bepalen op basis van de tekst van de conversatie. Verzend de voor waarde van de gebruiker als tekst naar het HTTP prediction-eind punt van de pizza-app. Op het eind punt past LUIS het model van de pizza-app toe om de tekst in de natuurlijke taal te analyseren, waardoor de algemene intentie kan worden bepaald en gegevens worden geëxtraheerd die relevant zijn voor het onderwerps domein van de app.

Voor dit artikel hebt u een gratis [LUIS](https://www.luis.ai)-account nodig.

<a name="create-luis-subscription-key"></a>

::: zone pivot="programming-language-csharp"
[!INCLUDE [Get intent with C# and REST](./includes/get-started-get-intent-rest-csharp.md)]
::: zone-end

::: zone pivot="programming-language-go"
[!INCLUDE [Get intent with Go and REST](./includes/get-started-get-intent-rest-go.md)]
::: zone-end

::: zone pivot="programming-language-java"
[!INCLUDE [Get intent with Java and REST](./includes/get-started-get-intent-rest-java.md)]
::: zone-end

::: zone pivot="programming-language-javascript"
[!INCLUDE [Get intent with Node.js and REST](./includes/get-started-get-intent-rest-nodejs.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [Get intent with Python and REST](./includes/get-started-get-intent-rest-python.md)]
::: zone-end
