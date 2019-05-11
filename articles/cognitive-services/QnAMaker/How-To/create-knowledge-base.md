---
title: Een Knowledge Base maken
titleSuffix: QnA Maker API - Azure Cognitive Services
description: Gebruik de QnA Maker API-serviceportal, om toe te voegen een kennisdatabase met chit chat maken. Hierdoor wordt uw app deelnemen. Toevoegen van een vooraf gevulde set van de bovenste chit-chat in uw KB als uitgangspunt voor van uw bot chit-chat en bespaart u tijd en kosten van het schrijven van deze helemaal.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 05/10/2019
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: 6b64315d8639cf8a7204ee809598567ec76fd188
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2019
ms.locfileid: "65541800"
---
# <a name="quickstart-create-a-knowledge-base-using-the-qna-maker-api-service-portal"></a>Quickstart: Een kennisdatabase met behulp van de portal QnA Maker API maken

De portal QnA Maker API kunt u eenvoudig uw bestaande gegevensbronnen voegen bij het maken van een kennisdatabase. U kunt een nieuwe QnA Maker-kennisdatabase maken van de volgende documenttypen:

<!-- added for scanability -->
* Veelgestelde vragen over pagina 's
* Producten handleidingen
* Gestructureerde documenten

Een persoonlijkheid chit chat als u uw kennis meer bezighouden met uw gebruikers wilt opnemen.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint. 

## <a name="create-a-new-knowledge-base"></a>Een knowledge base maken

1. Meld u aan bij naar de [QnA Maker portal](https://qnamaker.ai) met uw Azure-referenties in en selecteer **maken van een kennisdatabase**.

1. Als u nog geen een QnA Maker-service hebt gemaakt, selecteert u **maken van een service QnA**. 

1. Selecteer uw Azure-tenant, de naam van de Azure-abonnement en de naam van de Azure-resource die is gekoppeld aan de QnA Maker-service in de lijsten in **stap 2** in de portal voor QnA Maker. Selecteer de Azure QnA Maker-service die als voor de Knowledge Base host.

    ![QnA-service instellen](../media/qnamaker-how-to-create-kb/setup-qna-resource.png)

1. Voer de naam van uw knowledge base en de gegevensbronnen voor de nieuwe knowledge base.

    ![Set-gegevensbronnen](../media/qnamaker-how-to-create-kb/set-data-sources.png)

    - Geef de service een **naam.** Dubbele namen en speciale tekens worden ondersteund.
    - URL's voor gegevens die u uitgepakte wilt toevoegen. Zie voor meer informatie over de verschillende typen gegevensbronnen ondersteund [hier](../Concepts/data-sources-supported.md).
    - Uploaden van bestanden voor gegevens die u opgehaald wilt. Zie de [informatie over de prijzen](https://aka.ms/qnamaker-pricing) om te zien hoeveel documenten kunnen toevoegen.
    - Als u vragen en antwoorden supereenvoudig handmatig toevoegen wilt, kunt u overslaan **stap 4** weergegeven in de vorige afbeelding.

1. Voeg **Chit chat** aan uw KB. Kies chit chat-ondersteuning voor uw bot toevoegen door te kiezen uit een van de wensen. 

    ![Chit chat toevoegen aan KB](../media/qnamaker-how-to-create-kb/create-kb-chit-chat.png)

1. Selecteer **maken uw KB**.

    ![KB maken](../media/qnamaker-how-to-create-kb/create-kb.png)

1. Het duurt enkele minuten duren voordat gegevens worden geëxtraheerd.

    ![Extractie](../media/qnamaker-how-to-create-kb/hang-tight-extraction.png)

1. Wanneer uw Knowledge Base is gemaakt, wordt u omgeleid naar de **Knowledge base** pagina.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u klaar bent met de knowledge base, kunt u deze in de QnA Maker portal verwijderen.

## <a name="next-steps"></a>Volgende stappen

Voor kosten besparen metingen, kunt u [delen](upgrade-qnamaker-service.md?#share-existing-services-with-qna-maker) sommige, maar niet alle Azure-resources die zijn gemaakt voor QnA Maker.

> [!div class="nextstepaction"]
> [Chit chat persoonlijke toevoegen](./chit-chat-knowledge-base.md)
