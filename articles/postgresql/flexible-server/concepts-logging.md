---
title: Logboeken-Azure Database for PostgreSQL flexibele server
description: Beschrijft configuratie van logboek registratie, opslag en analyse in Azure Database for PostgreSQL-flexibele server
author: lfittl-msft
ms.author: lufittl
ms.service: postgresql
ms.topic: how-to
ms.date: 09/22/2020
ms.openlocfilehash: 852bce26e348c817b609d5ff837641741afe4461
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "90936602"
---
# <a name="logs-in-azure-database-for-postgresql---flexible-server"></a>Meldt zich aan Azure Database for PostgreSQL-flexibele server

> [!IMPORTANT]
> Azure Database for PostgreSQL - Flexible Server is als preview-versie beschikbaar

Met Azure Database for PostgreSQL kunt u standaard logboeken van post gres configureren en gebruiken. De logboeken kunnen worden gebruikt om configuratie fouten en suboptimale prestaties te identificeren, op te lossen en te herstellen. Logboek registratie-informatie die u kunt configureren en toegang bevat fouten, query gegevens, autovacuüm records, verbindingen en controle punten. (Toegang tot transactie Logboeken is niet beschikbaar).

Controle logboek registratie wordt beschikbaar gesteld via een post gres-extensie `pgaudit` . Ga naar het artikel over [controle concepten](concepts-audit.md) voor meer informatie.

## <a name="configure-logging"></a>Logboek registratie configureren

U kunt post gres Standard-logboek registratie op uw server configureren met de server parameters voor registratie. Ga voor meer informatie over post gres-logboek parameters naar de secties [Wanneer u zich wilt registreren](https://www.postgresql.org/docs/current/runtime-config-logging.html#RUNTIME-CONFIG-LOGGING-WHEN) en [wat u wilt registreren in](https://www.postgresql.org/docs/current/runtime-config-logging.html#RUNTIME-CONFIG-LOGGING-WHAT) de post gres-documentatie. De meeste, maar niet alle, post gres registratie parameters zijn beschikbaar voor configuratie in Azure Database for PostgreSQL.

Raadpleeg de documentatie van de [Portal](howto-configure-server-parameters-using-portal.md) of de [cli-documentatie](howto-configure-server-parameters-using-cli.md)voor meer informatie over het configureren van para meters in azure database for PostgreSQL.

> [!NOTE]
> Als u een groot aantal logboeken configureert, bijvoorbeeld logboek registratie, kunt u aanzienlijke prestatie overhead toevoegen. 

## <a name="accessing-logs"></a>Logboeken openen

Azure Database for PostgreSQL is geïntegreerd met Azure Monitor Diagnostische instellingen. Met Diagnostische instellingen kunt u uw post gres-Logboeken in JSON-indeling verzenden naar Azure Monitor logboeken voor analyse-en waarschuwings doeleinden, Event Hubs voor streaming en Azure Storage voor archivering. 

### <a name="log-format"></a>Logboek indeling

De volgende tabel beschrijft de velden voor het type **PostgreSQLLogs** . Afhankelijk van het uitvoer eindpunt dat u kiest, worden de opgenomen velden en de volg orde waarin ze worden weer gegeven. 

|**Veld** | **Beschrijving** |
|---|---|
| TenantId | Uw Tenant-ID |
| SourceSystem | `Azure` |
| TimeGenerated [UTC] | Tijds tempel voor het vastleggen van het logboek in UTC |
| Type | Het type van het logboek. Altijd `AzureDiagnostics` |
| SubscriptionId | GUID voor het abonnement waartoe de server behoort |
| ResourceGroup | Naam van de resource groep waartoe de server behoort |
| ResourceProvider | De naam van de resource provider. Altijd `MICROSOFT.DBFORPOSTGRESQL` |
| ResourceType | `Servers` |
| ResourceId | Resource-URI |
| Resource | Naam van de server |
| Categorie | `PostgreSQLLogs` |
| OperationName | `LogEvent` |
| Gelijk | Logboek registratie niveau, voor beeld: logboek, fout, kennisgeving |
| Bericht | Primair logboek bericht | 
| Domain | Server versie, voor beeld: post gres-10 |
| Detail | Secundair logboek bericht (indien van toepassing) |
| ColumnName | De naam van de kolom (indien van toepassing) |
| SchemaName | De naam van het schema (indien van toepassing) |
| Gegevens type | Naam van het gegevens type (indien van toepassing) |
| LogicalServerName | Naam van de server | 
| _ResourceId | Resource-URI |
| Voorvoegsel | Voor voegsel van logboek regel |


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [configureren en openen van Logboeken](howto-configure-and-access-logs.md).
- Meer informatie over [Azure monitor prijzen](https://azure.microsoft.com/pricing/details/monitor/).
- Meer informatie over [audit logboeken](concepts-audit.md)
