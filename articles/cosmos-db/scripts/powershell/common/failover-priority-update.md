---
title: PowerShell-script voor het wijzigen van de failover-prioriteit voor een Azure Cosmos-account met één schrijfregio
description: 'Voorbeeld van Azure PowerShell-script: failover-prioriteit wijzigen of failover activeren voor een Azure Cosmos-account met één schrijfregio'
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 03/18/2020
ms.author: mjbrown
ms.openlocfilehash: 72302f5b757bc95488cbf38888cae120611ef9e8
ms.sourcegitcommit: b6f3ccaadf2f7eba4254a402e954adf430a90003
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/20/2020
ms.locfileid: "92280765"
---
# <a name="change-failover-priority-or-trigger-failover-for-an-azure-cosmos-account-with-single-write-region-by-using-powershell"></a>Failover-prioriteit wijzigen of failover activeren voor een Azure Cosmos-account met één schrijfregio met behulp van PowerShell

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>Voorbeeldscript

> [!NOTE]
> Iedere wijziging van een regio met `failoverPriority=0` resulteert in een handmatige failover en kan alleen worden uitgevoerd voor een account dat is geconfigureerd voor handmatige failover. Wijzigingen van alle andere regio's hebben uitsluitend tot gevolg dat de failover-prioriteit voor een Cosmos-account wordt aangepast.
> [!NOTE]
> In dit voorbeeld wordt een SQL (Core) API-account gebruikt. Als u dit voorbeeld voor andere API's wilt gebruiken, kopieert u de gerelateerde eigenschappen en past u deze toe op uw API-specifieke script.

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/common/ps-account-failover-priority-update.ps1 "Update failover priority for an Azure Cosmos account or trigger a manual failover")]

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
| [Get-AzCosmosDBAccount](https://docs.microsoft.com/powershell/module/az.cosmosdb/get-azcosmosdbaccount) | Hiermee geeft u de Cosmos DB-accounts weer of een bepaald Cosmos DB-account. |
| [Update-AzCosmosDBAccountFailoverPriority](https://docs.microsoft.com/powershell/module/az.cosmosdb/update-azcosmosdbaccountfailoverpriority) | De prioriteitsvolgorde wijzigen van de failover-regio's van een Cosmos DB-account. |
|**Azure-resourcegroepen**| |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | Hiermee verwijdert u een resourcegroep met inbegrip van alle geneste resources. |
|||

## <a name="next-steps"></a>Volgende stappen

Zie [Documentatie over Azure PowerShell](https://docs.microsoft.com/powershell/) voor meer informatie over Azure PowerShell.
