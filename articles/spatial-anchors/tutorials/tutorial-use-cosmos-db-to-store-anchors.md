---
title: Zelfstudie - Share Azure ruimtelijke ankers sessies en apparaten met een Azure Cosmos DB back-end | Microsoft Docs
description: In deze zelfstudie leert u hoe u Azure ruimtelijke ankers-id's delen op Android/iOS-apparaten in Unity met een back-end-service en Azure Cosmos DB.
author: ramonarguelles
manager: vicenterivera
services: azure-spatial-anchors
ms.author: rgarcia
ms.date: 02/24/2019
ms.topic: tutorial
ms.service: azure-spatial-anchors
ms.openlocfilehash: fc172b5327d72687fea7d13ddb706ecc7ab630b6
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "67135340"
---
# <a name="tutorial-share-azure-spatial-anchors-across-sessions-and-devices-with-an-azure-cosmos-db-back-end"></a>Zelfstudie: Back-end van share Azure ruimtelijke ankers sessies en apparaten met een Azure Cosmos DB

In deze zelfstudie leert u hoe u [Azure ruimtelijke ankers](../overview.md) ankers maken tijdens één sessie en zoek tijdens een andere sessie op hetzelfde apparaat of een ander account. Bijvoorbeeld, de tweede sessie op een andere dag mogelijk. Deze dezelfde ankers kunnen ook worden gevonden door meerdere apparaten op dezelfde plaats en op hetzelfde moment.

![GIF-bestand ter illustratie object persistentie](./media/persistence.gif)

[Azure ruimtelijke ankers](../overview.md) is een platformoverschrijdende developer-service die u gebruiken kunt om te maken van gemengde realiteit ervaringen met objecten die hun locatie opslaan op apparaten na verloop van tijd. Wanneer u klaar bent, hebt u een app die kan worden geïmplementeerd op twee of meer apparaten. Ruimtelijke ankers die zijn gemaakt door één exemplaar wordt de id deelt met de andere met behulp van Azure Cosmos DB.

U leert het volgende:

> [!div class="checklist"]
> * Een ASP.NET Core web-app in Azure die kan worden gebruikt voor het delen van ankers, opslaat in Azure Cosmos DB implementeren.
> * Configureer de scène AzureSpatialAnchorsLocalSharedDemo in de Unity-voorbeeld van de snelstartgidsen van Azure om te profiteren van de delen ankers Web-App.
> * Een app implementeren op een of meer apparaten en voer deze uit.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [Share Anchors Sample Prerequisites](../../../includes/spatial-anchors-share-sample-prereqs.md)]

Het is vermelden waard dat, hoewel in deze zelfstudie hebt u, worden gebruikt Unity en Azure Cosmos DB, deze alleen bieden u een voorbeeld van het wordt delen van ruimtelijke ankers-id's op apparaten. U kunt andere talen en back-endtechnologieën gebruiken om hetzelfde doel te bereiken. De ASP.NET Core web-app die wordt gebruikt in deze zelfstudie vereist ook, de .NET Core-SDK 2.2. Deze wordt uitgevoerd fijn op Web-Apps voor Windows, maar niet op dit moment uitgevoerd op Web-Apps voor Linux.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="create-a-database-account"></a>Een databaseaccount maken

[!INCLUDE [cosmos-db-create-dbaccount-table](../../../includes/cosmos-db-create-dbaccount-table.md)]

Kopieer de `Connection String` omdat u hebt deze nodig.

## <a name="download-the-unity-sample-project"></a>Download het voorbeeldproject Unity

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

## <a name="deploy-the-sharing-anchors-service"></a>De service voor het delen ankers implementeren

Open Visual Studio en open het project in de `Sharing\SharingServiceSample` map.

### <a name="configure-the-service-to-use-your-azure-cosmos-db-database"></a>De service configureren voor het gebruik van uw Azure Cosmos DB-database

In **Solution Explorer**Open `SharingService\Startup.cs`.

Zoek `#define INMEMORY_DEMO` aan de bovenkant van het bestand en de opmerking die lijn uit. Sla het bestand op.

In **Solution Explorer**Open `SharingService\appsettings.json`.

Zoek de `StorageConnectionString` eigenschap en stel de waarde moet gelijk zijn aan de `Connection String` waarde die u hebt gekopieerd in de [maken van een database-account-stap](#create-a-database-account). Sla het bestand op.

[!INCLUDE [Publish Azure](../../../includes/spatial-anchors-publish-azure.md)]

[!INCLUDE [Run Share Anchors Sample](../../../includes/spatial-anchors-run-share-sample.md)]

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u Azure Cosmos DB gebruikt voor het delen van anker-id's op apparaten. Voor meer informatie over het gebruik van Azure ruimtelijke ankers in een nieuwe HoloLens Unity-app, gaat u naar de volgende zelfstudie.

> [!div class="nextstepaction"]
> [Een nieuwe Android-app starten](./tutorial-new-unity-hololens-app.md)