---
title: PowerShell-voorbeeld - Azure SQL-database verplaatsen - elastische pools | Microsoft Docs
description: Azure PowerShell-voorbeeldscript voor het verplaatsen van een SQL-database tussen elastische pools met behulp van PowerShell
services: sql-database
ms.service: sql-database
ms.subservice: elastic-pools
ms.custom: ''
ms.devlang: PowerShell
ms.topic: sample
author: stevestein
ms.reviewer: ''
ms.author: sstein
ms.date: 03/12/2019
ms.openlocfilehash: 3e43a027507a52965dd56bbf6015f18d2b2ebc2c
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68569872"
---
# <a name="use-powershell-to-create-elastic-pools-and-move-databases-between-elastic-pools"></a>PowerShell gebruiken voor het maken van elastische pools en verplaatsen van databases tussen elastische pools

In dit PowerShell-voorbeeldscript worden twee elastische pools gemaakt, waarna een database van de ene elastische pool naar een andere elastische pool wordt verplaatst. Vervolgens wordt de database vanuit de elastische pool verplaatst naar een rekenkracht voor één database.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]
[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om Power shell lokaal te installeren en te gebruiken, hebt u voor deze zelf studie AZ Power shell 1.4.0 of hoger nodig. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-az-ps). Als u PowerShell lokaal uitvoert, moet u ook `Connect-AzAccount` uitvoeren om verbinding te kunnen maken met Azure.

## <a name="sample-script"></a>Voorbeeldscript

[!code-powershell-interactive[main](../../../powershell_scripts/sql-database/move-database-between-pools-and-standalone/move-database-between-pools-and-standalone.ps1?highlight=18-19 "Move database between pools")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie

Gebruik de volgende opdracht om de resource groep en alle bijbehorende resources te verwijderen.

```powershell
Remove-AzResourceGroup -ResourceGroupName $resourcegroupname
```

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Hiermee maakt u een resourcegroep waarin alle resources worden opgeslagen. |
| [New-AzSqlServer](/powershell/module/az.sql/new-azsqlserver) | Hiermee maakt u een SQL Database-server die een enkele database of elastische pool host. |
| [New-AzSqlElasticPool](/powershell/module/az.sql/new-azsqlelasticpool) | Hiermee maakt u een elastische pool. |
| [New-AzSqlDatabase](/powershell/module/az.sql/new-azsqldatabase) | Hiermee maakt u een database op een SQL Database-server als een zelfstandige of een pooldatabase. |
| [Set-AzSqlDatabase](/powershell/module/az.sql/set-azsqldatabase) | Hiermee worden database-eigenschappen bijgewerkt of worden databasegegevens verplaatst naar, uit of tussen elastische pools. |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Hiermee verwijdert u een resourcegroep met inbegrip van alle geneste resources. |
|||

## <a name="next-steps"></a>Volgende stappen

Zie [Documentatie over Azure PowerShell](/powershell/azure/overview) voor meer informatie over Azure PowerShell.

Aanvullende voorbeelden van SQL Database PowerShell-scripts vindt u in [Azure SQL Database PowerShell-scripts](../sql-database-powershell-samples.md).
