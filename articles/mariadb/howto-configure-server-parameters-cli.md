---
title: Server parameters configureren-Azure CLI-Azure Database for MariaDB
description: In dit artikel wordt beschreven hoe u de service parameters in Azure Database for MariaDB kunt configureren met behulp van het Azure CLI-opdracht regel programma.
author: savjani
ms.author: pariks
ms.service: mariadb
ms.devlang: azurecli
ms.topic: how-to
ms.date: 10/1/2020
ms.custom: devx-track-azurecli
ms.openlocfilehash: 4009d8047dae7bf8d9ba66566ff8797fa09a8878
ms.sourcegitcommit: 6ab718e1be2767db2605eeebe974ee9e2c07022b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/12/2020
ms.locfileid: "94538135"
---
# <a name="configure-server-parameters-in-azure-database-for-mariadb-using-the-azure-cli"></a>Server parameters configureren in Azure Database for MariaDB met behulp van de Azure CLI
U kunt configuratie parameters voor een Azure Database for MariaDB server weer geven, tonen en bijwerken met behulp van Azure CLI, het opdracht regel programma van Azure. Een subset van de engine configuraties wordt weer gegeven op server niveau en kan worden gewijzigd.

>[!Note]
> Serverparameters kunnen globaal worden bijgewerkt op serverniveau via de [Azure CLI](./howto-configure-server-parameters-cli.md), [PowerShell](./howto-configure-server-parameters-using-powershell.md) of [Azure Portal](./howto-server-parameters.md).

## <a name="prerequisites"></a>Vereisten
Als u deze hand leiding wilt door lopen, hebt u het volgende nodig:
- [Een Azure Database for MariaDB server](quickstart-create-mariadb-server-database-using-azure-cli.md)
- [Azure cli](/cli/azure/install-azure-cli) -opdracht regel programma of gebruik de Azure Cloud shell in de browser.

## <a name="list-server-configuration-parameters-for-azure-database-for-mariadb-server"></a>Server configuratie parameters voor Azure Database for MariaDB server weer geven
Voer de opdracht [AZ mariadb Server Configuration List](/cli/azure/mariadb/server/configuration#az-mariadb-server-configuration-list) uit om alle para meters die kunnen worden gewijzigd op een server en hun waarden weer te geven.

U kunt de server configuratie parameters weer geven voor de server **mydemoserver.mariadb.database.Azure.com** onder resource groep **myresourcegroup**.
```azurecli-interactive
az mariadb server configuration list --resource-group myresourcegroup --server mydemoserver
```

Zie de sectie MariaDB Reference op [Server systeem variabelen](https://mariadb.com/kb/en/library/server-system-variables/)voor de definitie van elk van de vermelde para meters.

## <a name="show-server-configuration-parameter-details"></a>Details van server configuratie parameters weer geven
Voer de opdracht [AZ mariadb Server Configuration show](/cli/azure/mariadb/server/configuration#az-mariadb-server-configuration-show) uit om details over een bepaalde configuratie parameter voor een server weer te geven.

In dit voor beeld worden details weer gegeven van de configuratie parameter server van het **langzame \_ query \_ logboek** voor server **mydemoserver.mariadb.database.Azure.com** onder resource groep **myresourcegroup.**
```azurecli-interactive
az mariadb server configuration show --name slow_query_log --resource-group myresourcegroup --server mydemoserver
```

## <a name="modify-a-server-configuration-parameter-value"></a>Een waarde voor de para meter server configuratie wijzigen
U kunt ook de waarde van een bepaalde server configuratie parameter wijzigen, waarmee de onderliggende configuratie waarde wordt bijgewerkt voor de MariaDB-server engine. Als u de configuratie wilt bijwerken, gebruikt u de opdracht [AZ mariadb server configuration set](/cli/azure/mariadb/server/configuration#az-mariadb-server-configuration-set) . 

Voor het bijwerken van de configuratie parameter van de **langzame \_ query \_ logboek** server van server **mydemoserver.mariadb.database.Azure.com** onder resource groep **myresourcegroup.**
```azurecli-interactive
az mariadb server configuration set --name slow_query_log --resource-group myresourcegroup --server mydemoserver --value ON
```

Als u de waarde van een configuratie parameter opnieuw wilt instellen, laat u de optionele `--value` para meter weg en de service wordt de standaard waarde. In het bovenstaande voor beeld zou er als volgt uitzien:
```azurecli-interactive
az mariadb server configuration set --name slow_query_log --resource-group myresourcegroup --server mydemoserver
```

Met deze code wordt de langzame configuratie van het **\_ query \_ logboek** opnieuw ingesteld op de **standaard waarde.** 

## <a name="setting-parameters-not-listed"></a>Ingestelde para meters niet vermeld
Als de server parameter die u wilt bijwerken niet wordt weer gegeven in de Azure Portal, kunt u eventueel de para meter instellen op het verbindings niveau met `init_connect` . Hiermee stelt u de server parameters in voor elke client die verbinding maakt met de server. 

Werk de configuratie parameter **init \_ Connect** server van server **mydemoserver.mariadb.database.Azure.com** onder resource groep **myresourcegroup** bij om waarden in te stellen, zoals een tekenset.
```azurecli-interactive
az mariadb server configuration set --name init_connect --resource-group myresourcegroup --server mydemoserver --value "SET character_set_client=utf8;SET character_set_database=utf8mb4;SET character_set_connection=latin1;SET character_set_results=latin1;"
```

## <a name="working-with-the-time-zone-parameter"></a>Werken met de para meter tijd zone

### <a name="populating-the-time-zone-tables"></a>De tijd zone tabellen worden gevuld

De tijdzone tabellen op uw server kunnen worden gevuld door de opgeslagen procedure aan te roepen `mysql.az_load_timezone` vanuit een hulp programma zoals de MariaDB-opdracht regel of MariaDB Workbench.

> [!NOTE]
> Als u de opdracht uitvoert `mysql.az_load_timezone` vanuit MariaDB Workbench, moet u de veilige update modus mogelijk eerst uitschakelen met `SET SQL_SAFE_UPDATES=0;` .

```sql
CALL mysql.az_load_timezone();
```

> [!IMPORTANT]
> U moet de server opnieuw opstarten om te controleren of de tabellen van de tijd zone juist zijn ingevuld. Gebruik de [Azure Portal](howto-restart-server-portal.md) of [cli](howto-restart-server-cli.md)om de server opnieuw op te starten.

Voer de volgende opdracht uit om de beschik bare tijd zone waarden weer te geven:

```sql
SELECT name FROM mysql.time_zone_name;
```

### <a name="setting-the-global-level-time-zone"></a>De tijd zone van het globale niveau instellen

De tijd zone globaal niveau kan worden ingesteld met behulp van de opdracht [AZ mariadb server configuration set](/cli/azure/mariadb/server/configuration#az-mariadb-server-configuration-set) .

Met de volgende opdracht wordt de configuratie parameter van de **tijd \_ zone** server van server **mydemoserver.mariadb.database.Azure.com** onder resource groep **myresourcegroup** naar **VS/Pacific** bijgewerkt.

```azurecli-interactive
az mariadb server configuration set --name time_zone --resource-group myresourcegroup --server mydemoserver --value "US/Pacific"
```

### <a name="setting-the-session-level-time-zone"></a>De tijd zone op sessie niveau instellen

De tijd zone op sessie niveau kan worden ingesteld door de `SET time_zone` opdracht uit te voeren vanuit een hulp programma zoals de MariaDB-opdracht regel of MariaDB Workbench. In het volgende voor beeld wordt de tijd zone ingesteld op de **Amerikaanse/Pacific-** tijd zone.  

```sql
SET time_zone = 'US/Pacific';
```

Raadpleeg de MariaDB-documentatie voor [datum-en tijd functies](https://mariadb.com/kb/en/library/date-time-functions/).

## <a name="next-steps"></a>Volgende stappen

- [Server parameters configureren in azure Portal](howto-server-parameters.md)
