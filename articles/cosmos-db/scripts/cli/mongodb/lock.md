---
title: Een resourcevergrendeling voor een database en verzameling maken voor MongoDB API voor Azure Cosmos DB
description: Een resourcevergrendeling voor een database en verzameling maken voor MongoDB API voor Azure Cosmos DB
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: sample
ms.date: 07/29/2020
ms.openlocfilehash: aa3d14be3a7a9392ca721fa3de52b8f92dbe986e
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2020
ms.locfileid: "93098141"
---
# <a name="create-a-resource-lock-for-azure-cosmos-dbs-api-for-mongodb-using-azure-cli"></a>Een resourcevergrendeling voor de API van Azure Cosmos DB maken voor MongoDB met behulp van Azure CLI
[!INCLUDE[appliesto-mongodb-api](../../../includes/appliesto-mongodb-api.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit onderwerp gebruikmaken van Azure CLI versie 2.9.1 of hoger. Voer `az --version` uit om de versie te bekijken. Zie [Azure CLI installeren](/cli/azure/install-azure-cli) als u de CLI wilt installeren of een upgrade wilt uitvoeren.

> [!IMPORTANT]
> Resourcevergrendeling werkt niet voor wijzigingen die zijn aangebracht door gebruikers die verbinding maken met behulp van een MongoDB-SDK, Mongoshell, hulpprogramma's of Azure Portal, tenzij het Cosmos DB-account eerst is vergrendeld met de eigenschap `disableKeyBasedMetadataWriteAccess` ingeschakeld. Zie [Wijzigingen aan SDK's voorkomen](../../../role-based-access-control.md#prevent-sdk-changes) voor meer informatie over het inschakelen van deze eigenschap.

## <a name="sample-script"></a>Voorbeeldscript

[!code-azurecli-interactive[main](../../../../../cli_scripts/cosmosdb/mongodb/lock.sh "Create a resource lock for an Azure Cosmos DB MongoDB API database and collection.")]

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [az lock create](/cli/azure/lock#az-lock-create) | Hiermee wordt een vergrendeling gemaakt. |
| [az lock list](/cli/azure/lock#az-lock-list) | Hiermee worden vergrendelingsgegevens weergegeven. |
| [az lock show](/cli/azure/lock#az-lock-show) | Hiermee worden eigenschappen van een vergrendeling weergegeven. |
| [az lock delete](/cli/azure/lock#az-lock-delete) | Hiermee wordt een vergrendeling verwijderd. |

## <a name="next-steps"></a>Volgende stappen

-[Resources vergrendelen om onverwachte wijzigingen te voorkomen](../../../../azure-resource-manager/management/lock-resources.md)

-[Documentatie voor Azure Cosmos DB CLI](/cli/azure/cosmosdb).

-[Azure Cosmos DB CLI GitHub-opslagplaats](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb).
