---
title: Toegang tot wijzigings feed in Azure Cosmos DB Azure Cosmos DB
description: In dit artikel worden verschillende opties beschreven die beschikbaar zijn voor lees-en toegangs wijzigings invoer in Azure Cosmos DB Azure Cosmos DB.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/23/2019
ms.author: rimman
ms.openlocfilehash: 3d52ba1abc22aae6121ea6a36f943851dfcca7a0
ms.sourcegitcommit: c72ddb56b5657b2adeb3c4608c3d4c56e3421f2c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68467671"
---
# <a name="reading-azure-cosmos-db-change-feed"></a>Azure Cosmos DB wijzigings feed lezen

U kunt met behulp van de volgende opties met de Azure Cosmos DB wijzigings feed werken:

* Azure Functions gebruiken
* De processor bibliotheek van de wijzigings feed gebruiken
* De Azure Cosmos DB SQL API SDK gebruiken

## <a name="using-azure-functions"></a>Azure Functions gebruiken

Azure Functions is de eenvoudigste en aanbevolen optie. Wanneer u een Azure Functions trigger voor Cosmos DB maakt, kunt u de container selecteren om verbinding te maken en de Azure-functie wordt geactiveerd wanneer er een wijziging is in de container. Triggers kunnen worden gemaakt met behulp van de Azure Functions Portal, de Azure Cosmos DB portal of programmatisch met Sdk's. Visual Studio en VS code bieden ondersteuning voor het schrijven van Azure Functions, en u kunt zelfs de Azure Functions CLI gebruiken voor het ontwikkelen van meerdere platforms. U kunt de code op uw bureau blad schrijven en fouten opsporen en de functie vervolgens met één klik implementeren. Zie [Data Base computing zonder server met behulp van Azure functions](serverless-computing-database.md) en [gebruik Azure functions](change-feed-functions.md)) artikelen voor meer informatie.

## <a name="using-the-change-feed-processor-library"></a>De processor bibliotheek van de wijzigings feed gebruiken

De processor bibliotheek van de wijzigings feed verbergt de complexiteit en biedt nog steeds een volledige controle over de wijzigings feed. De bibliotheek volgt het onderhouds patroon, waarbij uw verwerkings functie wordt aangeroepen door de bibliotheek. Als u een wijzigings feed voor hoge door Voer hebt, kunt u meerdere clients instantiëren om de wijzigings feed te lezen. Omdat u de bibliotheek van de feed-processor wijzigt, wordt de belasting van de verschillende clients automatisch verdeeld zonder dat u deze logica hoeft te implementeren. De complexiteit wordt verwerkt door de bibliotheek. Als u uw eigen Load Balancer wilt hebben, kunt u implementeren `IPartitionLoadBalancingStrategy` voor een aangepaste partitie strategie voor het verwerken van wijzigingen in feeds. Zie voor meer informatie gebruiken de bibliotheek voor het [wijzigen van de feed-processor](change-feed-processor.md).

## <a name="using-the-azure-cosmos-db-sql-api-sdk"></a>De Azure Cosmos DB SQL API SDK gebruiken

Met de SDK krijgt u een controle op laag niveau van de wijzigings feed. U kunt het controle punt beheren, toegang krijgen tot een bepaalde logische partitie sleutel, enzovoort. Als u meerdere lezers hebt, kunt u gebruiken `ChangeFeedOptions` om Lees belasting te distribueren naar verschillende threads of verschillende clients. 

## <a name="next-steps"></a>Volgende stappen

U kunt nu doorgaan naar meer informatie over de wijzigingenfeed in de volgende artikelen:

* [Overzicht van wijzigings feed](change-feed.md)
* [Met behulp van de change feed met Azure Functions](change-feed-functions.md)
* [Met behulp van de change feed processor-bibliotheek](change-feed-processor.md)
