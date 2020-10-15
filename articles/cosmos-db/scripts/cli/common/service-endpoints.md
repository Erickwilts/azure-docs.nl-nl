---
title: Een Azure Cosmos-account maken met service-eindpunten voor virtueel netwerk
description: Een Azure Cosmos-account maken met service-eindpunten voor virtueel netwerk
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: sample
ms.date: 07/29/2020
ms.openlocfilehash: 4d1a56c80cab58e98121ae35c98a086d16dfe02b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "87432243"
---
# <a name="create-an-azure-cosmos-account-with-virtual-network-service-endpoints-using-azure-cli"></a>Een Azure Cosmos-account maken met service-eindpunten voor virtueel netwerk met behulp van Azure CLI

[!INCLUDE [cloud-shell-try-it.md](../../../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor deze opdracht Azure CLI versie 2.9.1 of hoger gebruiken. Voer `az --version` uit om de versie te bekijken. Zie [Azure CLI installeren](/cli/azure/install-azure-cli) als u de CLI wilt installeren of een upgrade wilt uitvoeren.

## <a name="sample-script"></a>Voorbeeldscript

In dit voorbeeld wordt een nieuw virtueel netwerk gemaakt met een subnet in de front-end en back-end, en worden er service-eindpunten ingeschakeld voor `Microsoft.AzureCosmosDB`. Vervolgens wordt de resource-id voor dit subnet opgehaald en toegepast op het Azure Cosmos-account, en worden er service-eindpunten voor het account ingeschakeld.

> [!NOTE]
> Dit voorbeeld laat zien hoe u een Core (SQL) API-account gebruikt. Als u dit voorbeeld voor andere API's wilt gebruiken, moet u de parameters `enable-virtual-network` en `virtual-network-rules` in het onderstaande script toepassen op uw API-specifieke script.

[!code-azurecli-interactive[main](../../../../../cli_scripts/cosmosdb/common/service-endpoints.sh "Create an Azure Cosmos account with service endpoints.")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie

Na het uitvoeren van het voorbeeldscript kan de volgende opdracht worden gebruikt om de resourcegroep en alle resources die er aan zijn gekoppeld te verwijderen.

```azurecli-interactive
az group delete --name $resourceGroupName
```

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Hiermee maakt u een resourcegroep waarin alle resources worden opgeslagen. |
| [az network vnet create](/cli/azure/network/vnet#az-network-vnet-create) | Hiermee maakt u een virtueel Azure-netwerk. |
| [az network vnet subnet create](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-create) | Hiermee maakt u een subnet voor een virtueel Azure-netwerk. |
| [az network vnet subnet show](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-show) | Hiermee retourneert u een subnet voor een virtueel Azure-netwerk. |
| [az cosmosdb create](/cli/azure/cosmosdb#az-cosmosdb-create) | Hiermee wordt een Azure Cosmos DB-account gemaakt. |
| [az group delete](/cli/azure/resource#az-resource-delete) | Hiermee verwijdert u een resourcegroep met inbegrip van alle geneste resources. |

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [documentatie van Azure Cosmos DB CLI](/cli/azure/cosmosdb) voor meer informatie over de Azure Cosmos DB CLI.

Alle Azure Cosmos DB CLI-voorbeeldscripts vindt u in de [documentatie van Azure Cosmos DB CLI GitHub-opslagplaats](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb).
