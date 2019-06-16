---
title: Serverlogboeken in Azure Database for PostgreSQL - één Server
description: Dit artikel wordt beschreven hoe Azure Database for PostgreSQL - Server één genereert query en foutenlogboeken en hoe bewaarduur logboek is geconfigureerd.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 4d1cf2c59e324cedd9b747b1ac65d6edcb9deb45
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65067395"
---
# <a name="server-logs-in-azure-database-for-postgresql---single-server"></a>Serverlogboeken in Azure Database for PostgreSQL - één Server
Azure Database voor PostgreSQL-query en de fout genereert logboeken. Query's en fout-logboeken kunnen worden gebruikt om te bepalen, oplossen en herstellen van fouten in de configuratie en optimale prestaties. (Toegang tot transactielogboeken is niet opgenomen). 

## <a name="configure-logging"></a>Logboekregistratie configureren 
U kunt de logboekregistratie configureren op de server met behulp van de serverparameters voor logboekregistratie. Op elke nieuwe server **log_checkpoints** en **log_connections** zijn standaard ingeschakeld. Er zijn extra parameters die u kunt aanpassen aan uw behoeften logboekregistratie: 

![Azure Database voor PostgreSQL - parameters voor logboekregistratie](./media/concepts-server-logs/log-parameters.png)

Zie voor meer informatie over deze parameters van de PostgreSQL [foutrapportage en de logboekregistratie](https://www.postgresql.org/docs/current/static/runtime-config-logging.html) documentatie. Zie voor meer informatie over het configureren van Azure Database voor PostgreSQL-parameters, de [portaldocumentatie](howto-configure-server-parameters-using-portal.md) of de [CLI-documentatie](howto-configure-server-parameters-using-cli.md).

## <a name="access-server-logs-through-portal-or-cli"></a>Serverlogboeken openen via de portal of de CLI
Als u de logboeken hebt ingeschakeld, kunt u deze kunt openen vanaf de Azure Database for PostgreSQL log-opslag met behulp de [Azure-portal](howto-configure-server-logs-in-portal.md), [Azure CLI](howto-configure-server-logs-using-cli.md), en Azure REST API's. De logboekbestanden van elke 1 uur of de grootte van 100MB draaien, afhankelijk van wat het eerste komt. U kunt instellen dat de bewaarperiode voor deze log opslag met behulp van de **log\_retentie\_periode** parameter die is gekoppeld aan uw server. De standaardwaarde is 3 dagen; de maximumwaarde is 7 dagen. De server moet voldoende opslagruimte voor het opslaan van de logboekbestanden toegewezen. (Deze retentie-parameter heeft geen betrekking op diagnostische logboeken van Azure.)


## <a name="diagnostic-logs"></a>Diagnostische logboeken
Azure Database voor PostgreSQL is geïntegreerd met Azure Monitor diagnostische logboeken. Als u Logboeken hebt ingeschakeld op uw PostgreSQL-server, kunt u deze verzonden naar [logboeken van Azure Monitor](../azure-monitor/log-query/log-query-overview.md), Event Hubs of Azure Storage. Voor meer informatie over het inschakelen van diagnostische logboeken, Zie de sectie van de procedures van de [diagnostische logboeken documentatie](../azure-monitor/platform/diagnostic-logs-overview.md). 

> [!IMPORTANT]
> Deze diagnostische functies voor server-Logboeken is alleen beschikbaar in het algemeen gebruik en geoptimaliseerd voor geheugen [Prijscategorieën](concepts-pricing-tiers.md).

De volgende tabel wordt beschreven wat er in elk logboek. Afhankelijk van het uitvoereindpunt dat u kiest, de velden die zijn opgenomen en de volgorde waarin ze worden weergegeven kunnen variëren. 

|**Veld** | **Beschrijving** |
|---|---|
| TenantId | Uw tenant-ID |
| SourceSystem | `Azure` |
| TimeGenerated [UTC] | Tijdstempel wanneer het logboek is vastgelegd in UTC |
| Type | Het type van het logboek. Altijd `AzureDiagnostics` |
| SubscriptionId | GUID voor het abonnement waartoe de server behoort |
| ResourceGroup | Naam van de resourcegroep die de server behoort |
| ResourceProvider | De naam van de resourceprovider. Altijd `MICROSOFT.DBFORPOSTGRESQL` |
| ResourceType | `Servers` |
| ResourceId | Resource-URI |
| Resource | Naam van de server |
| Category | `PostgreSQLLogs` |
| OperationName | `LogEvent` |
| errorLevel | Logboekregistratie op, bijvoorbeeld: U ZIET DAT LOGBOEK, FOUT: |
| Message | Primaire logboekbericht | 
| Domein | Server-versie, voorbeeld: postgres-10 |
| Details | Secundaire logboekbericht (indien van toepassing) |
| Kolomnaam | Naam van de kolom (indien van toepassing) |
| SchemaName | Naam van het schema (indien van toepassing) |
| DatatypeName | Naam van het gegevenstype (indien van toepassing) |
| LogicalServerName | Naam van de server | 
| _ResourceId | Resource-URI |

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over het verkrijgen van toegang tot logboeken van de [Azure-portal](howto-configure-server-logs-in-portal.md) of [Azure CLI](howto-configure-server-logs-using-cli.md).
- Meer informatie over [prijzen van Azure Monitor](https://azure.microsoft.com/pricing/details/monitor/).
