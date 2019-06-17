---
title: Beheren van Azure SQL Database-langetermijnretentie | Microsoft Docs
description: Meer informatie over het opslaan van geautomatiseerde back-ups in de SQL Azure-opslag en deze herstellen
services: sql-database
ms.service: sql-database
ms.subservice: backup-restore
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab
manager: craigg
ms.date: 04/17/2019
ms.openlocfilehash: 255f118d6dc6873364c2f8d4569e23c3e54ea83e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66164374"
---
# <a name="manage-azure-sql-database-long-term-backup-retention"></a>Langetermijnretentie voor Azure SQL Database beheren

In Azure SQL Database, kunt u één of een gegroepeerde database met een [langetermijnretentie](sql-database-long-term-retention.md) beleid (LTR) automatisch back-ups in Azure Blob-opslag maximaal 10 jaar bewaren. U kunt vervolgens deze back-ups met behulp van de Azure portal of PowerShell met een database herstellen.

> [!IMPORTANT]
> [Azure SQL Database Managed Instance](sql-database-managed-instance.md) ondersteunt geen langetermijnretentie van back-up.

## <a name="use-the-azure-portal-to-configure-long-term-retention-policies-and-restore-backups"></a>De Azure portal gebruiken om te configureren op de lange termijn voor het bewaren en herstellen van back-ups

De volgende secties laten zien hoe u de Azure portal gebruiken voor het configureren van de langetermijnretentie, back-ups langetermijnretentie weergeven en back-up herstellen vanuit een langetermijnretentie.

### <a name="configure-long-term-retention-policies"></a>Op de lange termijn bewaarbeleid configureren

U kunt SQL-Database te configureren [automatische back-ups](sql-database-long-term-retention.md) voor een periode die langer is dan de retentietermijn van uw servicecategorie. 

1. In de Azure-portal, selecteert u uw SQL-server en klik vervolgens op **back-ups beheren**. Op de **-beleid configureren** tabblad *schakelt u het selectievakje voor de database op die u wilt instellen of wijzigen op de lange termijn back-up bewaarbeleid*. Als het selectievakje naast de database niet is ingeschakeld, de wijzigingen voor het beleid niet van toepassing op die database.  

   ![koppeling van de back-ups beheren](./media/sql-database-long-term-retention/ltr-configure-ltr.png)

2. In de **-beleid configureren** deelvenster, selecteren als wilt behouden wekelijks, maandelijks of jaarlijks back-ups en geeft u de bewaarperiode voor elk. 

   ![configureren van beleid](./media/sql-database-long-term-retention/ltr-configure-policies.png)

3. Als u klaar bent, klikt u op **toepassen**.

> [!IMPORTANT]
> Wanneer u een beleid met een langetermijnbewaarperiode back-up inschakelt, is het duurt maximaal zeven dagen voor de eerste back-up zichtbaar en beschikbaar zijn om terug te zetten. Zie voor meer informatie van de LTR-back-up cadance [langetermijnretentie](sql-database-long-term-retention.md).

### <a name="view-backups-and-restore-from-a-backup-using-azure-portal"></a>Weergeven van back-ups en herstellen vanuit een back-up met behulp van Azure portal

Bekijk de back-ups die worden bewaard voor een specifieke database met een LTR-beleid en herstel van deze back-ups. 

1. In de Azure-portal, selecteert u uw SQL-server en klik vervolgens op **back-ups beheren**. Op de **beschikbare back-ups** tabblad, selecteert u de database waarvoor u wilt zien van beschikbare back-ups.

   ![database selecteren](./media/sql-database-long-term-retention/ltr-available-backups-select-database.png)

3. In de **beschikbare back-ups** deelvenster, bekijk de beschikbare back-ups. 

   ![back-ups weergeven](./media/sql-database-long-term-retention/ltr-available-backups.png)

4. Selecteer de back-up van waaruit u wilt herstellen en geef vervolgens de naam van de nieuwe database.

   ![De pagina Restore](./media/sql-database-long-term-retention/ltr-restore.png)

5. Klik op **OK** de database te herstellen vanuit de back-up in Azure SQL-opslag naar de nieuwe database.

6. Klik op de werkbalk op het meldingspictogram om de status van de hersteltaak weer te geven.

   ![voortgang hersteltaak](./media/sql-database-get-started-backup-recovery/restore-job-progress-long-term.png)

5. Als de hersteltaak is voltooid, opent u de **SQL-databases** pagina om de herstelde database weer te geven.

> [!NOTE]
> Hier kunt u verbinding maken met de herstelde database met behulp van SQL Server Management Studio om noodzakelijke taken uit te voeren, zoals [een deel van de gegevens uit de herstelde database extraheren om naar de bestaande database te kopiëren, of de bestaande database verwijderen en de naam van de herstelde database wijzigen in de naam van de bestaande database](sql-database-recovery-using-backups.md#point-in-time-restore).
>

## <a name="use-powershell-to-configure-long-term-retention-policies-and-restore-backups"></a>PowerShell gebruiken om te configureren op de lange termijn voor het bewaren en herstellen van back-ups

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> De PowerShell Azure Resource Manager-module nog steeds wordt ondersteund door Azure SQL Database, maar alle toekomstige ontwikkeling is voor de module Az.Sql. Zie voor deze cmdlets [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). De argumenten voor de opdrachten in de Az-module en de AzureRm-modules zijn vrijwel identiek zijn.

De volgende secties laten zien hoe u PowerShell gebruiken voor het configureren van de langetermijnretentie van back-ups, back-ups weergeven in Azure SQL-opslag en herstel van een back-up in Azure SQL-opslag.


### <a name="rbac-roles-to-manage-long-term-retention"></a>RBAC-rollen beheren met een langetermijnbewaarperiode

Als u wilt LTR-back-ups beheren, moet u zijn 
- De eigenaar van het abonnement of
- Inzender voor SQL Server-rol in **abonnement** bereik of
- De rol Inzender voor SQL-Database in **abonnement** bereik

Als u meer gedetailleerde controle vereist is, kunt u aangepaste RBAC-rollen maken en toewijzen in **abonnement** bereik. 

Voor **Get-AzSqlDatabaseLongTermRetentionBackup** en **terugzetten AzSqlDatabase** de rol moet de volgende bevoegdheden hebben:

Microsoft.Sql/locations/longTermRetentionBackups/read Microsoft.Sql/locations/longTermRetentionServers/longTermRetentionBackups/read Microsoft.Sql/locations/longTermRetentionServers/longTermRetentionDatabases/ longTermRetentionBackups leestijd
 
Voor **Remove-AzSqlDatabaseLongTermRetentionBackup** moet de rol van de volgende bevoegdheden hebben:

Microsoft.Sql/locations/longTermRetentionServers/longTermRetentionDatabases/longTermRetentionBackups/delete


### <a name="create-an-ltr-policy"></a>Een LTR-beleid maken

```powershell
# Get the SQL server 
# $subId = “{subscription-id}”
# $serverName = “{server-name}”
# $resourceGroup = “{resource-group-name}” 
# $dbName = ”{database-name}”

Connect-AzAccount
Select-AzSubscription -SubscriptionId $subId

# get the server
$server = Get-AzSqlServer -ServerName $serverName -ResourceGroupName $resourceGroup

# create LTR policy with WeeklyRetention = 12 weeks. MonthlyRetention and YearlyRetention = 0 by default.
Set-AzSqlDatabaseBackupLongTermRetentionPolicy -ServerName $serverName -DatabaseName $dbName -ResourceGroupName $resourceGroup -WeeklyRetention P12W 

# create LTR policy with WeeklyRetention = 12 weeks, YearlyRetention = 5 years and WeekOfYear = 16 (week of April 15). MonthlyRetention = 0 by default.
Set-AzSqlDatabaseBackupLongTermRetentionPolicy -ServerName $serverName -DatabaseName $dbName -ResourceGroupName $resourceGroup -WeeklyRetention P12W -YearlyRetention P5Y -WeekOfYear 16
```

### <a name="view-ltr-policies"></a>LTR-beleid weergeven
In dit voorbeeld ziet u hoe u het LTR-beleid in een server

```powershell
# Get all LTR policies within a server
$ltrPolicies = Get-AzSqlDatabase -ResourceGroupName Default-SQL-WestCentralUS -ServerName trgrie-ltr-server | Get-AzSqlDatabaseLongTermRetentionPolicy -Current 

# Get the LTR policy of a specific database 
$ltrPolicies = Get-AzSqlDatabaseBackupLongTermRetentionPolicy -ServerName $serverName -DatabaseName $dbName  -ResourceGroupName $resourceGroup -Current
```
### <a name="clear-an-ltr-policy"></a>Schakel een LTR-beleid
In dit voorbeeld laat zien hoe een LTR-beleid op basis van een database wissen

```powershell
Set-AzSqlDatabaseBackupLongTermRetentionPolicy -ServerName $serverName -DatabaseName $dbName -ResourceGroupName $resourceGroup -RemovePolicy
```

### <a name="view-ltr-backups"></a>LTR back-ups weergeven

Dit voorbeeld laat zien hoe u de LTR-back-ups binnen een server. 

```powershell
# Get the list of all LTR backups in a specific Azure region 
# The backups are grouped by the logical database id.
# Within each group they are ordered by the timestamp, the earliest
# backup first.  
$ltrBackups = Get-AzSqlDatabaseLongTermRetentionBackup -Location $server.Location 

# Get the list of LTR backups from the Azure region under 
# the named server. 
$ltrBackups = Get-AzSqlDatabaseLongTermRetentionBackup -Location $server.Location -ServerName $serverName

# Get the LTR backups for a specific database from the Azure region under the named server 
$ltrBackups = Get-AzSqlDatabaseLongTermRetentionBackup -Location $server.Location -ServerName $serverName -DatabaseName $dbName

# List LTR backups only from live databases (you have option to choose All/Live/Deleted)
$ltrBackups = Get-AzSqlDatabaseLongTermRetentionBackup -Location $server.Location -DatabaseState Live

# Only list the latest LTR backup for each database 
$ltrBackups = Get-AzSqlDatabaseLongTermRetentionBackup -Location $server.Location -ServerName $serverName -OnlyLatestPerDatabase
```

### <a name="delete-ltr-backups"></a>LTR-back-ups verwijderen

In dit voorbeeld wordt weergegeven hoe u een LTR verwijderen uit de lijst met back-ups.

```powershell
# remove the earliest backup 
$ltrBackup = $ltrBackups[0]
Remove-AzSqlDatabaseLongTermRetentionBackup -ResourceId $ltrBackup.ResourceId
```
> [!IMPORTANT]
> Back-up is niet-omkeerbare LTR te verwijderen. U kunt meldingen instellen over elke verwijderen in Azure Monitor door te filteren op bewerking 'Hiermee verwijdert u een back-up bewaren lange termijn'. Het activiteitenlogboek bevat informatie over de identiteit en wanneer de aanvraag heeft ingediend. Zie [waarschuwingen voor activiteitenlogboek maken](../azure-monitor/platform/alerts-activity-log.md) voor gedetailleerde instructies.
>

### <a name="restore-from-ltr-backups"></a>Herstellen vanuit back-ups van links naar rechts
In dit voorbeeld laat zien hoe om te herstellen vanaf een back-up van links naar rechts. Opmerking: deze interface niet hebt gewijzigd, maar de resource-id-parameter is het nu de LTR-back-resource-id vereist. 

```powershell
# Restore LTR backup as an S3 database
Restore-AzSqlDatabase -FromLongTermRetentionBackup -ResourceId $ltrBackup.ResourceId -ServerName $serverName -ResourceGroupName $resourceGroup -TargetDatabaseName $dbName -ServiceObjectiveName S3
```

> [!NOTE]
> Hier kunt u verbinding kunt maken met de herstelde database met behulp van SQL Server Management Studio om uit te voeren van de noodzakelijke taken, zoals het ophalen van een deel van de gegevens uit de herstelde database te kopiëren naar de bestaande database of te verwijderen van de bestaande database en wijzig de naam van de herstelde de database de naam van de bestaande database. Zie [punt in tijd herstel](sql-database-recovery-using-backups.md#point-in-time-restore).

## <a name="next-steps"></a>Volgende stappen

- Zie [Automatic backups](sql-database-automated-backups.md) (Automatische back-ups) voor meer informatie over door de service gegenereerde automatische back-ups.
- Zie [Langetermijnretentie](sql-database-long-term-retention.md) voor meer informatie over back-ups met langetermijnretentie.
