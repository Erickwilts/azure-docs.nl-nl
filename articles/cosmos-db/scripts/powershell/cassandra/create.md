---
title: PowerShell-script voor het maken van een keyspace en tabel voor Azure Cosmos DB Cassandra-API
description: Azure PowerShell-script - keyspace en tabel maken voor Azure Cosmos DB Cassandra-API
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: sample
ms.date: 05/13/2020
ms.author: mjbrown
ms.openlocfilehash: 44084b7f37b326ad28dc6298f5393f95325fffae
ms.sourcegitcommit: b6f3ccaadf2f7eba4254a402e954adf430a90003
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/20/2020
ms.locfileid: "92282831"
---
# <a name="create-a-keyspace-and-table-for-azure-cosmos-db---cassandra-api"></a>Een keyspace en tabel maken voor Azure Cosmos DB - Cassandra-API

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>Voorbeeldscript

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/cassandra/ps-cassandra-create.ps1 "Create a keyspace and table for Cassandra API")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie

Na het uitvoeren van het voorbeeldscript kan de volgende opdracht worden gebruikt om de resourcegroep en alle resources die er aan zijn gekoppeld te verwijderen.

```powershell
Remove-AzResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
|**Azure Cosmos DB**| |
| [New-AzCosmosDBAccount](https://docs.microsoft.com/powershell/module/az.cosmosdb/new-azcosmosdbaccount) | Hiermee maakt u een Cosmos DB-account. |
| [New-AzCosmosDBCassandraKeyspace](https://docs.microsoft.com/powershell/module/az.cosmosdb/new-azcosmosdbcassandrakeyspace) | Hiermee maakt u een Cosmos DB Cassandra-API-keyspace. |
| [New-AzCosmosDBCassandraClusterKey](https://docs.microsoft.com/powershell/module/az.cosmosdb/new-azcosmosdbcassandraclusterkey) | Hiermee maakt u een Cosmos DB Cassandra-API-clustersleutel. |
| [New-AzCosmosDBCassandraColumn](https://docs.microsoft.com/powershell/module/az.cosmosdb/new-azcosmosdbcassandracolumn) | Hiermee maakt u een Cosmos DB Cassandra-API-kolom. |
| [New-AzCosmosDBCassandraSchema](https://docs.microsoft.com/powershell/module/az.cosmosdb/new-azcosmosdbcassandraschema) | Hiermee maakt u een Cosmos DB Cassandra-API-schema. |
| [New-AzCosmosDBCassandraTable](https://docs.microsoft.com/powershell/module/az.cosmosdb/new-azcosmosdbcassandratable) | Hiermee maakt u een Cosmos DB Cassandra-API-tabel. |
|**Azure-resourcegroepen**| |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | Hiermee verwijdert u een resourcegroep met inbegrip van alle geneste resources. |
|||

## <a name="next-steps"></a>Volgende stappen

Zie [Documentatie over Azure PowerShell](https://docs.microsoft.com/powershell/) voor meer informatie over Azure PowerShell.
