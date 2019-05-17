---
title: Resource en de sleutel-management - QnA Maker
titleSuffix: Azure Cognitive Services
description: Uw service QnA Maker omgaat met twee soorten sleutels, abonnementssleutels en eindpunt sleutels.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 03/04/2019
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: 458d587c7ac73f7c8dacdceae3c9f923263533b3
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65792547"
---
# <a name="how-to-manage-keys-in-qna-maker"></a>Over het beheren van sleutels in QnA Maker

Uw service QnA Maker omgaat met twee soorten sleutels, **abonnementssleutels** en **eindpunt sleutels**.

![Sleutelbeheer](../media/qnamaker-how-to-key-management/key-management.png)

1. **Abonnementssleutels**: Deze sleutels worden gebruikt voor toegang tot de [QnA Maker management-service API's](https://go.microsoft.com/fwlink/?linkid=2092179). Deze API's kunt u uitvoeren bewerken van uw knowledge base.  

2. **Eindpunt sleutels**: Deze sleutels worden gebruikt voor toegang tot het eindpunt van de knowledge base voor een antwoord voor de vraag van een gebruiker. Doorgaans gebruikt u dit eindpunt in uw bot chatten of code van de client-toepassing die de QnA Maker-service worden verbruikt.
 
## <a name="subscription-keys"></a>Abonnementssleutels
U kunt weergeven en opnieuw instellen van uw abonnementssleutels vanuit de Azure-portal waar u de QnA Maker-resource hebt gemaakt. 
1. Ga naar de QnA Maker-resource in Azure portal.

    ![Lijst met resources voor QnA Maker](../media/qnamaker-how-to-key-management/qnamaker-resource-list.png)

2. Ga naar **sleutels**.

    ![Abonnementssleutel](../media/qnamaker-how-to-key-management/subscription-key.PNG)

## <a name="endpoint-keys"></a>Eindpunt-sleutels

Eindpunt-sleutels kunnen worden beheerd vanuit de [QnA Maker portal](https://qnamaker.ai).

1. Meld u aan bij de [QnA Maker portal](https://qnamaker.ai), gaat u naar uw profiel en klik vervolgens op **Service-instellingen**.

    ![Eindpuntsleutel](../media/qnamaker-how-to-key-management/Endpoint-keys.png)

2. Weergeven of opnieuw instellen van uw sleutels.

    ![belangrijkste endpoint-beheerder](../media/qnamaker-how-to-key-management/Endpoint-keys1.png)

    >[!NOTE]
    >Als u denkt dat ze zijn aangetast, vernieuwt u uw sleutels. Dit is mogelijk bijbehorende wijzigingen in uw clienttoepassing of -bot-code.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een kennisdatabase maken in een andere taal](./language-knowledge-base.md)
