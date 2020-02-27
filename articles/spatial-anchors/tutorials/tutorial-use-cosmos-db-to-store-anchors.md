---
title: 'Zelf studie: ankers delen met Azure Cosmos DB'
description: In deze zelf studie leert u hoe u met een back-end-service en Azure Cosmos DB Azure spatiale ankers-id's kunt delen op Android-en iOS-apparaten in eenheid.
author: ramonarguelles
manager: vriveras
services: azure-spatial-anchors
ms.author: rgarcia
ms.date: 02/24/2019
ms.topic: tutorial
ms.service: azure-spatial-anchors
ms.openlocfilehash: 71b3027d86400d6921895f86e257ddff2961f91f
ms.sourcegitcommit: f15f548aaead27b76f64d73224e8f6a1a0fc2262
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2020
ms.locfileid: "77615155"
---
# <a name="tutorial-sharing-azure-spatial-anchors-across-sessions-and-devices-with-an-azure-cosmos-db-back-end"></a>Zelf studie: ruimtelijke Azure-ankers delen in sessies en apparaten met een Azure Cosmos DB back-end

Deze zelf studie is een voortzetting van het [delen van Azure spatiale ankers in sessies en apparaten.](../../../articles/spatial-anchors/tutorials/tutorial-share-anchors-across-devices.md) Dit leidt u door het proces van het toevoegen van een aantal meer mogelijkheden om Azure Cosmos DB te fungeren als de back-end-opslag, terwijl u ruimte vrijmaakt voor Azure spatiale ankers in sessies en apparaten.

![GIF-illustratie van object persistentie](./media/persistence.gif)

Het is belang rijk dat u de eenheid en Azure Cosmos DB in deze zelf studie gebruikt, maar dit is een voor beeld van het delen van ruimtelijke ankers-id's op alle apparaten. U kunt andere talen en technologieën gebruiken om hetzelfde doel te bereiken. De ASP.NET Core web-app die in deze zelf studie wordt gebruikt, vereist ook de .NET Core 2,2-SDK. Het werkt prima op Web Apps voor Windows, maar wordt op dit moment niet uitgevoerd op Web Apps voor Linux.

## <a name="create-a-database-account"></a>Een databaseaccount maken

Voeg een Azure Cosmos-data base toe aan de resource groep die u eerder hebt gemaakt.

[!INCLUDE [cosmos-db-create-dbaccount-table](../../../includes/cosmos-db-create-dbaccount-table.md)]

Kopieer de `Connection String` omdat u deze nodig hebt.

## <a name="make-minor-changes-to-the-sharingservice-files"></a>Kleine wijzigingen aanbrengen in de SharingService-bestanden

Open `SharingService\Startup.cs`in **Solution Explorer**.

Zoek `#define INMEMORY_DEMO` boven aan het bestand en geef een opmerking door. Sla het bestand op.

Open `SharingService\appsettings.json`in **Solution Explorer**.

Zoek de eigenschap `StorageConnectionString` en stel de waarde in op hetzelfde als de `Connection String` waarde die u hebt gekopieerd in de [stap een database account maken](#create-a-database-account). Sla het bestand op.

U kunt de service voor delen opnieuw publiceren en de voor beeld-app uitvoeren.

## <a name="troubleshooting"></a>Problemen oplossen

### <a name="unity-20193"></a>Unity 2019,3

Als gevolg van het verbreken van wijzigingen, wordt unit 2019,3 momenteel niet ondersteund. Gebruik unit 2019,1 of 2019,2.

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u Azure Cosmos DB gebruikt voor het delen van anker-id's op apparaten. Ga verder met de volgende zelf studie voor meer informatie over het gebruik van Azure spatiale ankers in een nieuwe unit HoloLens-app.

> [!div class="nextstepaction"]
> [Een nieuwe unit HoloLens-app starten](./tutorial-new-unity-hololens-app.md)
