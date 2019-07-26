---
title: Azure SQL Database-en SQL Data Warehouse connectiviteits architectuur | Microsoft Docs
description: In dit document wordt de architectuur van Azure SQL-connectiviteit uitgelegd voor database verbindingen vanuit Azure of van buiten Azure.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: rohitnayakmsft
ms.author: rohitna
ms.reviewer: carlrab, vanto
manager: craigg
ms.date: 07/02/2019
ms.openlocfilehash: 9dfc22be45b68ba4ff59d88810435db35bafc8b6
ms.sourcegitcommit: 5604661655840c428045eb837fb8704dca811da0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/25/2019
ms.locfileid: "68494972"
---
# <a name="azure-sql-connectivity-architecture"></a>Architectuur van Azure SQL-connectiviteit

In dit artikel worden de architectuur van Azure SQL Database en SQL Data Warehouse connectiviteit beschreven en wordt uitgelegd hoe de verschillende onderdelen het verkeer omleiden naar uw exemplaar van Azure SQL. Met deze connectiviteits onderdelen wordt het netwerk verkeer omgeleid naar de Azure SQL Database of SQL Data Warehouse met clients die verbinding maken vanuit Azure en met clients die verbinding maken van buiten Azure. In dit artikel vindt u ook script voorbeelden voor het wijzigen van de manier waarop connectiviteit plaatsvindt en de overwegingen met betrekking tot het wijzigen van de standaard connectiviteits instellingen.

## <a name="connectivity-architecture"></a>Connectiviteitsarchitectuur

In het volgende diagram vindt u een overzicht van de architectuur van de Azure SQL Database-connectiviteit.

![architectuur overzicht](./media/sql-database-connectivity-architecture/connectivity-overview.png)

In de volgende stappen wordt beschreven hoe een verbinding tot stand wordt gebracht met een Azure-SQL database:

- Clients maken verbinding met de gateway, die een openbaar IP-adres heeft en luistert op poort 1433.
- De gateway, afhankelijk van het effectief verbindings beleid, leidt het verkeer om naar het juiste database cluster.
- In het database cluster verkeer wordt doorgestuurd naar de juiste Azure-SQL database.

## <a name="connection-policy"></a>Verbindings beleid

Azure SQL Database ondersteunt de volgende drie opties voor de instelling van het verbindings beleid van een SQL Database Server:

- **Omleiden (aanbevolen):** Clients maken rechtstreeks verbinding met het knoop punt dat als host fungeert voor de data base. Om connectiviteit in te scha kelen, moeten de clients uitgaande firewall regels toestaan voor alle Azure IP-adressen in de regio met behulp van netwerk beveiligings groepen (NSG) met [service Tags](../virtual-network/security-overview.md#service-tags)) voor poort 11000-11999, niet alleen de IP-adressen van de Azure SQL database gateway op poort 1433. Omdat pakketten rechtstreeks naar de Data Base gaan, hebben latentie en door Voer betere prestaties.
- **Webtoepassingsproxy** In deze modus worden alle verbindingen via de gateways van de Azure SQL Database geproxyeerd. Om connectiviteit in te scha kelen, moet de client uitgaande firewall regels hebben die alleen de Azure SQL Database gateway-IP-adressen (meestal twee IP-adressen per regio) toestaan. Het kiezen van deze modus kan leiden tot hogere latentie en een lagere door Voer, afhankelijk van de aard van de werk belasting. Het is raadzaam om `Redirect` het verbindings beleid voor `Proxy` de laagste latentie en de hoogste door Voer uit te voeren via het verbindings beleid.
- **Prijs** Dit is het verbindings beleid dat wordt toegepast op alle servers na het maken, tenzij u het verbindings beleid expliciet `Proxy` wijzigt `Redirect`naar ofwel of. Het effectief beleid is afhankelijk van het feit of de verbindingen afkomstig zijn van`Redirect`in azure () of buiten`Proxy`Azure ().

## <a name="connectivity-from-within-azure"></a>Connectiviteit vanuit Azure

Als u verbinding maakt vanuit Azure, hebben uw verbindingen standaard een verbindings beleid `Redirect` van. Een beleid `Redirect` waarbij wordt aangegeven dat nadat de TCP-sessie is ingesteld op de Azure-SQL database, de client sessie wordt omgeleid naar het juiste database cluster met een wijziging in de virtuele doel-IP van die van de Azure SQL database gateway naar die van de cluster. Daarna stroomt alle volgende pakketten rechtstreeks naar het cluster, waarbij de Azure SQL Database gateway wordt omzeild. In het volgende diagram ziet u deze verkeers stroom.

![architectuur overzicht](./media/sql-database-connectivity-architecture/connectivity-azure.png)

## <a name="connectivity-from-outside-of-azure"></a>Connectiviteit van buiten Azure

Als u verbinding maakt vanuit buiten Azure, hebben uw verbindingen standaard een verbindings beleid `Proxy` van. Een beleid voor `Proxy` betekent dat de TCP-sessie tot stand is gebracht via de Azure SQL database gateway en dat alle volgende pakketten via de gateway stromen. In het volgende diagram ziet u deze verkeers stroom.

![architectuur overzicht](./media/sql-database-connectivity-architecture/connectivity-onprem.png)

## <a name="azure-sql-database-gateway-ip-addresses"></a>IP-adressen van Azure SQL Database gateway

In de volgende tabel worden de IP-adressen van gateways per regio weer gegeven. Als u verbinding wilt maken met een Azure SQL Database, moet u netwerk verkeer toestaan & van **alle** gateways voor de regio.

Nu gaan we meer gateways in elke regio toevoegen en de gateways buiten gebruik stellen in de kolom IP-adres uit de buiten gebruik gestelde gateway van de tabel hieronder. Meer informatie over het uit bedrijf nemen van processen vindt u in het volgende artikel: [Azure SQL Database verkeer migratie naar nieuwere gateways](sql-database-gateway-migration.md)


| Naam regio          | IP-adres van de gateway | Buiten gebruik gestelde gateway </br> IP-adres| Opmerkingen over uit bedrijf nemen | 
| --- | --- | --- | --- |
| Australië - centraal    | 20.36.105.0 | | |
| Australië-Central2   | 20.36.113.0 | | |
| Australië - oost       | 13.75.149.87, 40.79.161.1 | | |
| Australië - zuidoost | 191.239.192.109, 13.73.109.251 | | |
| Brazilië - zuid         | 104.41.11.5        |                 | |
| Canada - midden       | 40.85.224.249      |                 | |
| Canada - oost          | 40.86.226.166      |                 | |
| US - centraal           | 13.67.215.62, 52.182.137.15 | 23.99.160.139 | Geen verbindingen na 1 september 2019 |
| China - oost           | 139.219.130.35     |                 | |
| China - oost 2         | 40.73.82.1         |                 | |
| China - noord          | 139.219.15.17      |                 | |
| China - noord 2        | 40.73.50.0         |                 | |
| Azië - oost            | 191.234.2.139, 52.175.33.150 |       | |
| East US              | 40.121.158.30, 40.79.153.12 | 191.238.6.43 | Geen verbindingen na 1 september 2019 |
| US - oost 2            | 40.79.84.180, 52.177.185.181, 52.167.104.0 | 191.239.224.107    | Geen verbindingen na 1 september 2019 |
| Frankrijk - centraal       | 40.79.137.0, 40.79.129.1 |           | |
| Duitsland - centraal      | 51.4.144.100       |                 | |
| Duitsland-noord Oost   | 51.5.144.179       |                 | |
| India - centraal        | 104.211.96.159     |                 | |
| India - zuid          | 104.211.224.146    |                 | |
| India - west           | 104.211.160.80     |                 | |
| Japan - oost           | 13.78.61.196, 40.79.184.8, 13.78.106.224 | 191.237.240.43 | Geen verbindingen na 1 september 2019 |
| Japan - west           | 104.214.148.156, 40.74.100.192 | 191.238.68.11 | Geen verbindingen na 1 september 2019 |
| Korea - centraal        | 52.231.32.42       |                 | |
| Korea - zuid          | 52.231.200.86      |                 | |
| US - noord-centraal     | 23.96.178.199      | 23.98.55.75     | Geen verbindingen na 1 september 2019 |
| Europa - noord         | 40.113.93.91       | 191.235.193.75  | Geen verbindingen na 1 september 2019 |
| Zuid-Afrika - noord   | 102.133.152.0      |                 | |
| Zuid-Afrika - west    | 102.133.24.0       |                 | |
| US - zuid-centraal     | 13.66.62.124       | 23.98.162.75    | Geen verbindingen na 1 september 2019 |
| Azië - zuidoost      | 104.43.15.0        | 23.100.117.95   | Geen verbindingen na 1 september 2019 |
| UAE - centraal          | 20.37.72.64        |                 | |
| UAE - noord            | 65.52.248.0        |                 | |
| Verenigd Koninkrijk Zuid             | 51.140.184.11      |                 | |
| Verenigd Koninkrijk West              | 51.141.8.11        |                 | |
| US - west-centraal      | 13.78.145.25       |                 | |
| Europa -west          | 40.68.37.158       | 191.237.232.75  | Geen verbindingen na 1 september 2019 |
| US - west              | 104.42.238.205     | 23.99.34.75     | Geen verbindingen na 1 september 2019 |
| US - west 2            | 13.66.226.202      |                 | |
|                      |                    |                 | |

## <a name="change-azure-sql-database-connection-policy"></a>Azure SQL Database verbindings beleid wijzigen

Als u het Azure SQL Database verbindings beleid voor een Azure SQL Database Server wilt wijzigen, gebruikt u de opdracht verbindingen [-beleid](https://docs.microsoft.com/cli/azure/sql/server/conn-policy) .

- Als uw verbindings beleid is ingesteld op `Proxy`, worden alle netwerk pakketten via de gateway van de Azure SQL database. Voor deze instelling moet u alleen uitgaand naar het IP-adres van de Azure SQL Database gateway toestaan. Het gebruik van een `Proxy` instelling van heeft meer latentie dan een `Redirect`instelling van.
- Als uw verbindings beleid is ingesteld `Redirect`, stromen alle netwerk pakketten rechtstreeks naar het database cluster. Voor deze instelling moet u uitgaand naar meerdere IP-adressen toestaan.

## <a name="script-to-change-connection-settings-via-powershell"></a>Script voor het wijzigen van de verbindings instellingen via Power shell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> De Power shell-Azure Resource Manager module wordt nog steeds ondersteund door Azure SQL Database, maar alle toekomstige ontwikkeling is voor de module AZ. SQL. Zie [AzureRM. SQL](https://docs.microsoft.com/powershell/module/AzureRM.Sql/)voor deze cmdlets. De argumenten voor de opdrachten in de module AZ en in de AzureRm-modules zijn aanzienlijk identiek. Voor het volgende script is de [Azure PowerShell-module](/powershell/azure/install-az-ps)vereist.

Het volgende Power shell-script laat zien hoe u het verbindings beleid wijzigt.

```powershell
# Get SQL Server ID
$sqlserverid=(Get-AzSqlServer -ServerName sql-server-name -ResourceGroupName sql-server-group).ResourceId

# Set URI
$id="$sqlserverid/connectionPolicies/Default"

# Get current connection policy
(Get-AzResource -ResourceId $id).Properties.connectionType

# Update connection policy
Set-AzResource -ResourceId $id -Properties @{"connectionType" = "Proxy"} -f
```

## <a name="script-to-change-connection-settings-via-azure-cli"></a>Script voor het wijzigen van de verbindings instellingen via Azure CLI

> [!IMPORTANT]
> Voor dit script is de [Azure cli](https://docs.microsoft.com/cli/azure/install-azure-cli)vereist.

### <a name="azure-cli-in-a-bash-shell"></a>Azure CLI in een bash-shell

> [!IMPORTANT]
> Voor dit script is de [Azure cli](https://docs.microsoft.com/cli/azure/install-azure-cli)vereist.

Het volgende CLI-script laat zien hoe u het verbindings beleid wijzigt in een bash-shell.

```azurecli-interactive
# Get SQL Server ID
sqlserverid=$(az sql server show -n sql-server-name -g sql-server-group --query 'id' -o tsv)

# Set URI
ids="$sqlserverid/connectionPolicies/Default"

# Get current connection policy
az resource show --ids $ids

# Update connection policy
az resource update --ids $ids --set properties.connectionType=Proxy
```

### <a name="azure-cli-from-a-windows-command-prompt"></a>Azure CLI vanaf een Windows-opdracht prompt

> [!IMPORTANT]
> Voor dit script is de [Azure cli](https://docs.microsoft.com/cli/azure/install-azure-cli)vereist.

Het volgende CLI-script laat zien hoe u het verbindings beleid wijzigt vanuit een Windows-opdracht prompt (waarbij Azure CLI is geïnstalleerd).

```azurecli
# Get SQL Server ID and set URI
FOR /F "tokens=*" %g IN ('az sql server show --resource-group myResourceGroup-571418053 --name server-538465606 --query "id" -o tsv') do (SET sqlserverid=%g/connectionPolicies/Default)

# Get current connection policy
az resource show --ids %sqlserverid%

# Update connection policy
az resource update --ids %sqlserverid% --set properties.connectionType=Proxy
```

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het wijzigen van het Azure SQL Database verbindings beleid voor een Azure SQL Database-Server verbinding [-beleid](https://docs.microsoft.com/cli/azure/sql/server/conn-policy).
- Voor informatie over Azure SQL Database verbindings gedrag voor clients die gebruikmaken van ADO.NET 4,5 of een latere versie, Zie [poorten na 1433 voor ADO.NET 4,5](sql-database-develop-direct-route-ports-adonet-v12.md).
- Zie [SQL database Application Development Overview (overzicht van toepassings ontwikkeling](sql-database-develop-overview.md)) voor algemene informatie over het ontwikkelen van toepassingen.
