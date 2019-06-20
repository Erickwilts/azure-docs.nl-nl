---
title: De parameters van de service in Azure Database voor PostgreSQL - één-Server configureren
description: In dit artikel wordt beschreven hoe u de parameters van de service configureren in Azure Database voor PostgreSQL - één Server met behulp van de Azure CLI-opdrachtregel.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.devlang: azurecli
ms.topic: conceptual
ms.date: 06/19/2019
ms.openlocfilehash: f276247076438a03973148b5cf65ddbeb409b024
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67274774"
---
# <a name="customize-server-configuration-parameters-for-azure-database-for-postgresql---single-server-using-azure-cli"></a>Parameters voor serverconfiguratie aanpassen voor Azure Database voor PostgreSQL - één Server met behulp van Azure CLI
U kunt weergeven, weergeven en bijwerken van parameters voor de configuratie voor een Azure PostgreSQL-server met behulp van de opdrachtregelinterface (Azure CLI). Een subset van de engine configuraties op niveau van de server wordt weergegeven en kan worden gewijzigd. 

## <a name="prerequisites"></a>Vereisten
Als u wilt in deze gebruiksaanwijzing kunt doorlopen, hebt u het volgende nodig:
- Een Azure Database for PostgreSQL-server en database maken door de volgende [een Azure Database for PostgreSQL maken](quickstart-create-server-database-azure-cli.md)
- Installeer [Azure CLI](/cli/azure/install-azure-cli) opdrachtregelinterface op uw computer of gebruik de [Azure Cloud Shell](../cloud-shell/overview.md) in Azure portal met uw browser.

## <a name="list-server-configuration-parameters-for-azure-database-for-postgresql-server"></a>Lijst met parameters voor serverconfiguratie voor Azure Database for PostgreSQL-server
Als u alle parameters van kan worden gewijzigd in een server en de bijbehorende waarden, voer de [az postgres server configuration list](/cli/azure/postgres/server/configuration) opdracht.

U kunt de parameters voor serverconfiguratie voor de server een lijst **mydemoserver.postgres.database.azure.com** onder de resourcegroep **myresourcegroup**.
```azurecli-interactive
az postgres server configuration list --resource-group myresourcegroup --server mydemoserver
```
## <a name="show-server-configuration-parameter-details"></a>Details van de parameter voor de serverconfiguratie van de weergeven
Uitvoeren als u wilt weergeven van details over een bepaalde configuratieparameter voor een server, de [az postgres server configuration show](/cli/azure/postgres/server/configuration) opdracht.

In dit voorbeeld worden details van de **log\_min\_berichten** configuratieparameter van de server voor server **mydemoserver.postgres.database.azure.com** onder de resourcegroep **myresourcegroup.**
```azurecli-interactive
az postgres server configuration show --name log_min_messages --resource-group myresourcegroup --server mydemoserver
```
## <a name="modify-server-configuration-parameter-value"></a>Parameterwaarde voor server-configuratie wijzigen
U kunt ook de waarde van een bepaalde server configuratieparameter, die de onderliggende configuratiewaarde voor de PostgreSQL-server-engine-updates wijzigen. Voor het bijwerken van de configuratie, gebruikt u de [az postgres server configuratieset](/cli/azure/postgres/server/configuration) opdracht. 

Om bij te werken de **log\_min\_berichten** server configuratieparameter van server **mydemoserver.postgres.database.azure.com** onder de resourcegroep  **myresourcegroup.**
```azurecli-interactive
az postgres server configuration set --name log_min_messages --resource-group myresourcegroup --server mydemoserver --value INFO
```
Als u instellen van de waarde van een configuratieparameter wilt, kiest u gewoon laat u uit de optionele `--value` parameter, de service is van toepassing op de standaardwaarde. In bovenstaande voorbeeld wordt eruitziet het, zoals:
```azurecli-interactive
az postgres server configuration set --name log_min_messages --resource-group myresourcegroup --server mydemoserver
```
Hiermee stelt u deze opdracht de **log\_min\_berichten** configuratie op de standaardwaarde **waarschuwing**. Zie voor meer informatie over de serverconfiguratie en de toegestane waarden voor PostgreSQL-documentatie op [serverconfiguratie](https://www.postgresql.org/docs/9.6/static/runtime-config.html).

## <a name="next-steps"></a>Volgende stappen
- [Leer hoe u een server opnieuw starten](howto-restart-server-cli.md)
- Als u wilt configureren en om toegang tot serverlogboeken, Zie [Server-logboeken in Azure Database for PostgreSQL](concepts-server-logs.md)
