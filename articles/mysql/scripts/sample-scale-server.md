---
title: 'Azure CLI-script: schaal van een Azure Database for MySQL-server aanpassen'
description: Met dit CLI-voorbeeldscript wordt de schaal van een Azure Database for MySQL-server aangepast naar een ander prestatieniveau na het uitvoeren van query's op de metrische gegevens.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.devlang: azurecli
ms.topic: sample
ms.custom: mvc
ms.date: 08/07/2019
ms.openlocfilehash: f54b3f6fa8bb37f57479d6a9e7bc05340e411a48
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2019
ms.locfileid: "68882931"
---
# <a name="monitor-and-scale-an-azure-database-for-mysql-server-using-azure-cli"></a>Een Azure Database for MySQL-server bewaken en de schaal ervan aanpassen met Azure CLI
Met dit CLI-voorbeeld script worden reken-en opslag ruimte voor één Azure Database for MySQL Server geschaald nadat een query op de metrische gegevens is doorzocht. Compute kan omhoog of omlaag worden geschaald. Opslag kan alleen omhoog worden geschaald.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal uit te voeren, moet u voor dit artikel gebruikmaken van Azure CLI-versie 2.0 of hoger. Controleer de versie door `az --version` uit te voeren. Zie [Azure CLI installeren]( /cli/azure/install-azure-cli) voor het installeren of upgraden van uw versie van Azure CLI. 

## <a name="sample-script"></a>Voorbeeldscript
Werk het script bij met uw abonnements-ID.
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/scale-mysql-server/scale-mysql-server.sh "Create and scale Azure Database for MySQL.")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie
Gebruik de volgende opdracht om de resourcegroep en alle resources die er aan zijn gekoppeld te verwijderen nadat het script is uitgevoerd. 
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/scale-mysql-server/delete-mysql.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Uitleg van het script
Dit script maakt gebruik van de opdrachten die in de volgende tabel worden weergegeven:

| **Opdracht** | **Opmerkingen** |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Hiermee maakt u een resourcegroep waarin alle resources worden opgeslagen. |
| [az mysql server create](/cli/azure/mysql/server#az-mysql-server-create) | Hiermee wordt een MySQL-server gemaakt waar de databases worden gehost. |
| [Update AZ mysql server](/cli/azure/mysql/server#az-mysql-server-update) | Hiermee worden de eigenschappen van de MySQL-server bijgewerkt. |
| [az monitor metrics list](/cli/azure/monitor/metrics#az-monitor-metrics-list) | Geeft de metrische waarde weer voor de resources. |
| [az group delete](/cli/azure/group#az-group-delete) | Hiermee verwijdert u een resourcegroep met inbegrip van alle geneste resources. |

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [Azure database for MySQL Compute en opslag](../concepts-pricing-tiers.md)
- Aanvullende scripts proberen: [Azure CLI-voorbeelden voor Azure Database for MySQL](../sample-scripts-azure-cli.md)
- Meer informatie over de [Azure cli](/cli/azure)