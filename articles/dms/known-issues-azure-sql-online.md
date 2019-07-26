---
title: Artikel over bekende problemen/migratie beperkingen met online migraties naar Azure SQL Database | Microsoft Docs
description: Meer informatie over bekende problemen/migratie beperkingen met online migraties voor Azure SQL Database.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 07/26/2019
ms.openlocfilehash: afafaa86988905329a0e4ff45f29bea9d1d57820
ms.sourcegitcommit: a0b37e18b8823025e64427c26fae9fb7a3fe355a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/25/2019
ms.locfileid: "68501044"
---
# <a name="known-issuesmigration-limitations-with-online-migrations-to-azure-sql-database"></a>Bekende problemen/migratie beperkingen met online migraties naar Azure SQL Database

Bekende problemen en beperkingen die zijn gekoppeld aan online migraties van SQL Server naar Azure SQL Database worden hieronder beschreven.

> [!IMPORTANT]
> Met online migraties van SQL Server naar Azure SQL Database wordt de migratie van SQL_variant-gegevens typen niet ondersteund.

### <a name="migration-of-temporal-tables-not-supported"></a>Migratie van tijdelijke tabellen wordt niet ondersteund

**Symptoom** Als uw bron database uit een of meer tijdelijke tabellen bestaat, mislukt de migratie van de data base tijdens de bewerking ' volledige gegevens laden ' en ziet u mogelijk het volgende bericht:

```
{ "resourceId":"/subscriptions/<subscription id>/resourceGroups/migrateready/providers/Microsoft.DataMigration/services/<DMS Service name>", "errorType":"Database migration error", "errorEvents":"["Capture functionalities could not be set. RetCode: SQL_ERROR SqlState: 42000 NativeError: 13570 Message: [Microsoft][SQL Server Native Client 11.0][SQL Server]The use of replication is not supported with system-versioned temporal table '[Application. Cities]' Line: 1 Column: -1 "]" }
```

 ![Voor beeld van fouten in tijdelijke tabellen](media/known-issues-azure-sql-online/dms-temporal-tables-errors.png)

**Tijdelijke oplossing** Gebruik de volgende stappen.

1. Zoek de tijdelijke tabellen in uw bron schema met behulp van de onderstaande query.

     ``` 
     select name,temporal_type,temporal_type_desc,* from sys.tables where temporal_type <>0
     ```

2. Sluit deze tabellen uit op de Blade **migratie-instellingen configureren** , waarop u tabellen voor migratie opgeeft.

3. Voer de migratie activiteit opnieuw uit.

**Bronnen** Zie het artikel [tijdelijke tabellen](https://docs.microsoft.com/sql/relational-databases/tables/temporal-tables?view=sql-server-2017)voor meer informatie.

### <a name="migration-of-tables-includes-one-or-more-columns-with-the-hierarchyid-data-type"></a>Migratie van tabellen bevat een of meer kolommen met het gegevens type hierarchyid

**Symptoom** Er wordt mogelijk een SQL-uitzonde ring weer gegeven met de suggestie ' ntext is incompatibel met hierarchyid ' tijdens de bewerking ' volledige gegevens laden ':

![voor beeld van hierarchyid-fouten](media/known-issues-azure-sql-online/dms-hierarchyid-errors.png)

**Tijdelijke oplossing** Gebruik de volgende stappen.

1. Zoek de gebruikers tabellen die kolommen bevatten met het gegevens type hierarchyid met behulp van de onderstaande query.

      ``` 
      select object_name(object_id) 'Table name' from sys.columns where system_type_id =240 and object_id in (select object_id from sys.objects where type='U')
      ```

2. Sluit deze tabellen uit op de Blade **migratie-instellingen configureren** , waarop u tabellen voor migratie opgeeft.

3. Voer de migratie activiteit opnieuw uit.

### <a name="migration-failures-with-various-integrity-violations-with-active-triggers-in-the-schema-during-full-data-load-or-incremental-data-sync"></a>Migratie fouten met verschillende integriteits schendingen met actieve triggers in het schema tijdens volledige belasting van gegevens of incrementele gegevens synchronisatie

**Tijdelijke oplossing** Gebruik de volgende stappen.

1. Zoek de triggers die momenteel actief zijn in de bron database met behulp van de volgende query:

     ```
     select * from sys.triggers where is_disabled =0
     ```

2. Schakel de triggers in uw bron database uit met behulp van de stappen in het artikel [Disable trigger (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/disable-trigger-transact-sql?view=sql-server-2017).

3. Voer de migratie activiteit opnieuw uit.

### <a name="support-for-lob-data-types"></a>Ondersteuning voor LOB-gegevens typen

**Symptoom** Als de LOB-kolom (lengte van Large Object) groter is dan 32 KB, worden gegevens mogelijk afgekapt bij het doel. U kunt de lengte van de LOB-kolom controleren met behulp van de volgende query:

``` 
SELECT max(DATALENGTH(ColumnName)) as LEN from TableName
```

**Tijdelijke oplossing** Als u een LOB-kolom hebt die groter is dan 32 KB, neemt u contact op met het technische team om [Azure-database migraties te vragen](mailto:AskAzureDatabaseMigrations@service.microsoft.com).

### <a name="issues-with-timestamp-columns"></a>Problemen met Time Stamp-kolommen

**Symptoom** De time stamp-waarde van Azure Database Migration Service wordt niet gemigreerd. Azure Database Migration Service wordt in plaats daarvan een nieuwe time stamp-waarde in de doel tabel gegenereerd.

**Tijdelijke oplossing**

Als u Azure Database Migration Service nodig hebt om de exacte tijds tempel waarde te migreren die is opgeslagen in de bron tabel, neemt u contact op met het technische team om [Azure-database migraties te vragen](mailto:AskAzureDatabaseMigrations@service.microsoft.com).

### <a name="data-migration-errors-dont-provide-additional-details-on-the-database-detailed-status-blade"></a>Gegevens migratie fouten bieden geen aanvullende details over de Blade gedetailleerde status van data base

**Symptoom** Wanneer u problemen met de migratie ondervinden in de status weergave Details van data bases, is het selecteren van de koppeling **fouten in gegevens migratie** op het bovenste lint mogelijk niet meer specifieke informatie over de migratie fouten.

![gegevens migratie fouten geen voor beeld van Details](media/known-issues-azure-sql-online/dms-data-migration-errors-no-details.png)

**Tijdelijke oplossing** Voer de volgende stappen uit om specifieke fout gegevens op te halen.

1. Sluit de Blade gedetailleerde status van de data base om het scherm migratie activiteit weer te geven.

     ![scherm migratie activiteit](media/known-issues-azure-sql-online/dms-migration-activity-screen.png)

2. Selecteer **Fout Details bekijken** om specifieke fout berichten weer te geven die u helpen bij het oplossen van migratie fouten.

### <a name="geography-datatype-not-supported-in-sqldb-online-migration"></a>Geografie gegevens type wordt niet ondersteund in SQLDB online migratie

**Symptoom** De migratie mislukt met een fout bericht met de volgende tekst:

 ' * * er is een onherstelbare fout opgetreden ', ' errorEvents ':<Table>.<Column> is van het type GEOGRAFIE, wat niet wordt ondersteund door Full load in de ondersteunings modus Full LOB.

**Tijdelijke oplossing** Hoewel Azure Database Migration Service het geografie gegevens type voor offline migraties naar Azure SQL Database ondersteunt, wordt voor online migraties het geografie type geo niet ondersteund. Probeer alternatieve methoden om het gegevens type bij de bron te wijzigen in een ondersteund type voordat u Azure Database Migration Service probeert te gebruiken voor een online migratie van deze data base.

### <a name="supported-editions"></a>Ondersteunde versies

**Symptoom** De migratie mislukt met een fout bericht met de volgende tekst:

 Validatie fout migratie-instellingen: De editie van de server [Business Intelligence Edition (64-bits)] komt niet overeen met de ondersteunde editie (s) [ENTER prise, Standard, Developer].

**Tijdelijke oplossing** Ondersteuning voor online migraties naar Azure SQL Database met behulp van Azure Database Migration Service gaat alleen over de Enter prise-, Standard-en Developer-edities. Zorg ervoor dat u een ondersteunde editie gebruikt voordat u begint met het migratie proces.
