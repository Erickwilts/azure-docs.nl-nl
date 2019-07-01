---
title: Upgrade uw QnA Maker-service - QnA Maker
titleSuffix: Azure Cognitive Services
description: Delen of upgrade van uw services QnA Maker om de resources beter beheren.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 03/25/2019
ms.author: diberry
ms.openlocfilehash: 2fdbb245f838d92e84d1247faa610a2f1a66c532
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67439752"
---
# <a name="share-or-upgrade-your-qna-maker-service"></a>Delen of upgrade uw QnA Maker-service
Delen of upgrade van uw services QnA Maker om de resources beter beheren. 

U kunt kiezen om bij te werken van afzonderlijke onderdelen van de QnA Maker-stack nadat de initiële is gemaakt. Bekijk de details van de afhankelijke onderdelen en de SKU-selectie [hier](https://aka.ms/qnamaker-docs-capacity).

## <a name="share-existing-services-with-qna-maker"></a>Bestaande services delen met QnA Maker

QnA Maker maakt verschillende Azure-resources. Als u wilt beperken management en profiteren van de kosten delen, gebruik de volgende tabel om te begrijpen wat u wel en niet delen:

|Service|Delen|
|--|--|
|Cognitive Services|X|
|App service-plan|✔|
|App Service|X|
|Application Insights|✔|
|Zoekservice|✔|

## <a name="upgrade-qna-maker-management-sku"></a>QnA Maker Management SKU upgraden

Als u wilt hebben van meer vragen en antwoorden in uw knowledge base, buiten de huidige laag upgrade uw prijscategorie QnA Maker-service. 

Upgrade de QnA Maker management SKU:

1. Ga naar uw QnA Maker-resource in Azure portal en selecteer **prijscategorie**.

    ![QnA Maker-resource](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-resource.png)

2. Kies de juiste SKU en druk op **Selecteer**.

    ![Prijzen voor QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-pricing-page.png)

## <a name="upgrade-app-service"></a>Appservice bijwerken

 Wanneer uw knowledge base fungeren meer aanvragen van uw client-app moet, moet u uw app-service prijscategorie bijwerken.

U kunt [omhoog schalen](https://docs.microsoft.com/azure/app-service/web-sites-scale) of verkleinen van de appservice.

1. Ga naar de resource voor de App-service in Azure portal en selecteer **omhoog schalen** of **omlaag schalen** opties zoals vereist.

    ![QnA Maker app service schalen](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-scale.png)

## <a name="upgrade-azure-search-service"></a>Azure Search-service upgraden

Wanneer u van plan bent om veel knowledge bases, upgrade van uw Azure Search-service prijscategorie. 

Het is momenteel niet mogelijk om uit te voeren van een in-place upgrade van de Azure SKU zoeken. U kunt echter een nieuwe resource van Azure search maken met de gewenste SKU, de gegevens te herstellen naar de nieuwe resource en vervolgens koppelen aan de QnA Maker-stack.

1. Maak een nieuwe Azure search-resource in Azure portal en kies de gewenste SKU.

    ![QnA Maker Azure search-resource](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-azuresearch-new.png)

2. De indexen van uw oorspronkelijke Azure search-resource op de nieuwe terugzetten. Bekijk de voorbeeldcode van de back-up terugzetten [hier](https://github.com/pchoudhari/QnAMakerBackupRestore).

3. Nadat de gegevens is hersteld, gaat u naar uw nieuwe Azure search-resource, worden Selecteer **sleutels**, en noteer de **naam** en de **administratorsleutel**.

    ![QnA Maker Azure search-sleutels](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-azuresearch-keys.png)

4. Als u wilt de nieuwe Azure search-resource een koppeling naar de QnA Maker-stack, gaat u naar de QnA Maker App service.

    ![QnA Maker appservice](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-resource-list-appservice.png)

5. Selecteer **toepassingsinstellingen** en vervang de **AzureSearchName** en **AzureSearchAdminKey** velden uit stap 3.

    ![QnA Maker Azure App service-instelling](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-settings.png)

6. Start de App-service.

    ![QnA Maker appservice opnieuw opstarten](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-restart.png)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Gebruik QnA Maker-API](../Quickstarts/csharp.md)
