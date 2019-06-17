---
title: Postman configureren voor Azure Media Services REST API-aanroepen
description: Informatie over het configureren van Postman voor Media Services REST API-aanroepen.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/01/2019
ms.author: juliako
ms.openlocfilehash: 5d0b5a57f3fe587a06a102c958b17dbf2a73225c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "61466675"
---
# <a name="configure-postman-for-media-services-rest-api-calls"></a>Postman configureren voor Media Services REST API-aanroepen  

> [!NOTE]
> Er worden geen nieuwe functies of functionaliteit meer aan Media Services v2. toegevoegd. <br/>Maak kennis met de nieuwste versie, [Media Services v3](https://docs.microsoft.com/azure/media-services/latest/). Zie ook [hulp bij de migratie van v2 naar v3](../latest/migrate-from-v2-to-v3.md)

Deze zelfstudie leert u hoe het configureren van **Postman** zodat deze kan worden gebruikt voor het aanroepen van Azure Media Services (AMS) REST-API's. De zelfstudie laat zien hoe u de omgeving en verzameling bestanden importeren in **Postman**. De verzameling bevat gegroepeerde definities van HTTP-aanvragen die Azure Media Services (AMS) REST-API's aanroepen. Het omgevingsbestand bevat variabelen die worden gebruikt door de verzameling.

Deze omgeving en de verzameling wordt gebruikt in artikelen die laten zien hoe u verschillende taken met Azure Media Services REST API's te bereiken.

## <a name="prerequisites"></a>Vereisten

- Installeer de [Postman](https://www.getpostman.com/) REST-client als u de REST-API's wilt uitvoeren die in een aantal AMS REST-zelfstudies worden weergegeven. 

    We gebruiken **Postman** maar elk ander REST-hulpprogramma is hiervoor geschikt. Enkele andere alternatieven: **Visual Studio Code** met de REST-invoegtoepassing of **Telerik Fiddler**. 

## <a name="configure-the-environment"></a>De omgeving configureren 

1. Maak een .json-bestand waarin de omgevingsvariabelen die worden gebruikt in de AMS-zelfstudies. Noem het bestand (bijvoorbeeld **AzureMediaServices.postman_environment.json**). Open het bestand en plak de code die de Postman-omgeving van bepaalt [deze vermelding van de code](postman-environment.md). 
2. Open de **Postman**.
3. Selecteer rechts van het scherm de optie **Manage environment**.

    ![Bestand uploaden](./media/media-services-rest-upload-files/postman-create-env.png)
4. Klik in het dialoogvenster **Manage environment** op **Import**.
5. Blader en selecteer de **AzureMediaServices.postman_environment.json** bestand.
6. De **Media** omgeving wordt toegevoegd.
7. Sluit het dialoogvenster.
8. Selecteer de **Media** omgeving.

    ![Bestand uploaden](./media/media-services-rest-upload-files/postman-choose-env.png)

## <a name="configure-the-collection"></a>De collectie configureren

1. Maak een .json-bestand met de **Postman** verzameling met alle bewerkingen die nodig zijn voor het uploaden van een bestand met Media Services. Noem het bestand (bijvoorbeeld **AzureMediaServicesOperations.postman_collection.json**). Open het bestand en plak de code die definieert de **Postman** verzamelen van [deze vermelding van de code](postman-collection.md).
2. Klik op **Import** om het verzamelingbestand te importeren.
3. Kies de **AzureMediaServicesOperations.postman_collection.json** bestand.

    ![Bestand uploaden](./media/media-services-rest-upload-files/postman-import-collection.png)

## <a name="next-steps"></a>Volgende stappen

Bekijk de [upload items](media-services-rest-upload-files.md) artikel.  
