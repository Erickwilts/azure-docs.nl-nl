---
title: 'Azure CLI-script: de schaal van een Azure Database for MariaDB-server aanpassen'
description: Met dit CLI-voorbeeldscript wordt de schaal van een Azure Database for MariaDB-server aangepast naar een ander prestatieniveau na het doorzoeken van de metrische gegevens.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.devlang: azurecli
ms.topic: sample
ms.custom: mvc
ms.date: 08/07/2019
ms.openlocfilehash: 772c4de9c53f618d0c7c5d8b8d6acc2830595f55
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2019
ms.locfileid: "68884148"
---
# <a name="monitor-and-scale-an-azure-database-for-mariadb-server-using-azure-cli"></a>Een Azure Database for MariaDB-server bewaken en de schaal ervan aanpassen met Azure CLI
Met dit CLI-voorbeeld script worden reken-en opslag ruimte voor één Azure Database for MariaDB Server geschaald nadat een query op de metrische gegevens is doorzocht. Compute kan omhoog of omlaag worden geschaald. Opslag kan alleen omhoog worden geschaald.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal uit te voeren, moet u voor dit artikel gebruikmaken van Azure CLI-versie 2.0 of hoger. Controleer de versie door `az --version` uit te voeren. Zie [Azure CLI installeren]( /cli/azure/install-azure-cli) voor het installeren of upgraden van uw versie van Azure CLI. 

## <a name="sample-script"></a>Voorbeeldscript
Werk het script bij met uw abonnements-ID.
[!code-azurecli-interactive[main](../../../cli_scripts/mariadb/scale-mariadb-server/scale-mariadb-server.sh "Create and scale Azure Database for MariaDB.")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie
Gebruik de volgende opdracht om de resourcegroep en alle resources die er aan zijn gekoppeld te verwijderen nadat het script is uitgevoerd. 
[!code-azurecli-interactive[main](../../../cli_scripts/mariadb/scale-mariadb-server/delete-mariadb.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Uitleg van het script
Dit script maakt gebruik van de opdrachten die in de volgende tabel worden weergegeven:

| **Opdracht** | **Opmerkingen** |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Hiermee maakt u een resourcegroep waarin alle resources worden opgeslagen. |
| [az mariadb server create](/cli/azure/mariadb/server#az-mariadb-server-create) | Hiermee wordt een MariaDB-server gemaakt waarop de databases worden gehost. |
| [AZ mariadb Server Update](/cli/azure/mariadb/server#az-mariadb-server-update) | Hiermee worden de eigenschappen van de MariaDB-server bijgewerkt. |
| [az monitor metrics list](/cli/azure/monitor/metrics#az-monitor-metrics-list) | Geeft de metrische waarde weer voor de resources. |
| [az group delete](/cli/azure/group#az-group-delete) | Hiermee verwijdert u een resourcegroep met inbegrip van alle geneste resources. |

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [Azure database for MariaDB Compute en opslag](../concepts-pricing-tiers.md)
- Aanvullende scripts proberen: [Azure CLI-voorbeelden voor Azure Database for MariaDB](../sample-scripts-azure-cli.md)
- Meer informatie over de [Azure cli](/cli/azure)
