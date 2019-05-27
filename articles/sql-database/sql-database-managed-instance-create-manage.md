---
title: Beheer-API-naslaginformatie voor Azure SQL Database Managed Instance | Microsoft Docs
description: Meer informatie over het maken en beheren van Azure SQL Database beheerde instanties.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
manager: craigg
ms.date: 03/12/2019
ms.openlocfilehash: 6362084c11ce7aa9078823758700239694162765
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "66150773"
---
# <a name="managed-api-reference-for-azure-sql-database-managed-instances"></a>Beheerde API-naslaginformatie voor Azure SQL Database beheerde instanties

U kunt maken en beheren van Azure SQL Database Managed Instances met behulp van de Azure portal, PowerShell, Azure CLI, REST-API en Transact-SQL. In dit artikel vindt u een overzicht van functies en -API die u gebruiken kunt om te maken en configureren van Managed Instance.

## <a name="azure-portal-create-a-managed-instance"></a>Azure Portal: Een beheerd exemplaar maken

Zie voor een snelstart u voor het maken van een Azure SQL Database Managed Instance, [Quick Start: Maken van een beheerd exemplaar voor Azure SQL Database](sql-database-managed-instance-get-started.md).

## <a name="powershell-create-and-manage-managed-instances"></a>PowerShell: Maken en beheren van beheerde exemplaren

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> De PowerShell Azure Resource Manager-module nog steeds wordt ondersteund door Azure SQL Database, maar alle toekomstige ontwikkeling is voor de module Az.Sql. Zie voor deze cmdlets [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). De argumenten voor de opdrachten in de Az-module en de AzureRm-modules zijn vrijwel identiek zijn.

Als u wilt maken en beheren van beheerde exemplaren met Azure PowerShell, gebruikt u de volgende PowerShell-cmdlets. Als u wilt installeren of upgraden van PowerShell, Zie [Azure PowerShell-module installeren](/powershell/azure/install-az-ps).

> [!TIP]
> Zie voor PowerShell-voorbeeldscripts, [Quick start-script: Maken van Azure SQL Managed Instance met behulp van PowerShell-bibliotheek](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../quick-start-script-create-azure-sql-managed-instance-using-powershell/).

| Cmdlet | Description |
| --- | --- |
|[New-AzSqlInstance](https://docs.microsoft.com/powershell/module/az.sql/new-azsqlinstance)|Hiermee maakt u een beheerd exemplaar voor Azure SQL Database |
|[Get-AzSqlInstance](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlinstance)|Retourneert informatie over Azure SQL Managed Instance|
|[Set-AzSqlInstance](https://docs.microsoft.com/powershell/module/az.sql/set-azsqlinstance)|Hiermee stelt u eigenschappen voor een Azure SQL Database Managed Instance|
|[Remove-AzSqlInstance](https://docs.microsoft.com/powershell/module/az.sql/remove-azsqlinstance)|Hiermee verwijdert u een beheerde Database-exemplaar voor Azure SQL|
|[New-AzSqlInstanceDatabase](https://docs.microsoft.com/powershell/module/az.sql/new-azsqlinstancedatabase)|Hiermee maakt u een Azure SQL Database Managed Instance-database|
|[Get-AzSqlInstanceDatabase](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlinstancedatabase)|Retourneert informatie over beheerd exemplaar voor Azure SQL database|
|[Remove-AzSqlInstanceDatabase](https://docs.microsoft.com/powershell/module/az.sql/remove-azsqlinstancedatabase)|Hiermee verwijdert u een exemplaar van beheerde Database van Azure SQL-database|
|[Restore-AzSqlInstanceDatabase](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqlinstancedatabase)|Hiermee herstelt u een beheerd Database exemplaar van Azure SQL-database|

## <a name="azure-cli-create-and-manage-managed-instances"></a>Azure CLI: Maken en beheren van beheerde exemplaren

Maken en beheren van beheerde exemplaren met [Azure CLI](/cli/azure), gebruikt u de volgende [Azure CLI SQL Managed Instance](/cli/azure/sql/mi) opdrachten. Gebruik de [Cloud Shell](/azure/cloud-shell/overview) om de CLI in uw browser uit te voeren of [installeer](/cli/azure/install-azure-cli) de CLI op macOS, Linux of Windows.

> [!TIP]
> Zie voor een Azure CLI-Quick Start [werkt met SQL beheerd exemplaar met behulp van Azure CLI met](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44).

| Cmdlet | Description |
| --- | --- |
|[AZ sql mi maken](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-create) |Hiermee maakt u een beheerd exemplaar|
|[lijst met AZ sql mi](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-list)|Een lijst met beschikbare beheerde exemplaren|
|[AZ sql mi weergeven](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-show)|De details ophalen voor een beheerd exemplaar|
|[AZ sql mi-update](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-update)|Updates van een beheerd exemplaar|
|[AZ sql mi verwijderen](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-delete)|Hiermee verwijdert u een beheerd exemplaar|
|[AZ sql DEELB maken](https://docs.microsoft.com/cli/azure/sql/midb#az-sql-midb-create) |Hiermee maakt u een beheerde database|
|[lijst met AZ sql DEELB](https://docs.microsoft.com/cli/azure/sql/midb#az-sql-midb-list)|Een lijst met beschikbare beheerde databases|
|[AZ sql DEELB herstellen](https://docs.microsoft.com/cli/azure/sql/midb#az-sql-midb-restore)|Een beheerde database herstellen|
|[AZ sql DEELB verwijderen](https://docs.microsoft.com/cli/azure/sql/midb#az-sql-midb-delete)|Hiermee verwijdert u een beheerde database|

## <a name="transact-sql-create-and-manage-instance-databases"></a>Transact-SQL: Exemplaar-databases maken en beheren

Als u wilt maken en beheren van exemplaar in de database nadat het beheerde exemplaar is gemaakt, moet u de volgende T-SQL-opdrachten gebruiken. U kunt deze opdrachten met de Azure-portal, de opdracht [SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio), [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/what-is). [Visual Studio Code](https://code.visualstudio.com/docs), of andere programma's die kan verbinding maken met een Azure SQL Database-server en Transact-SQL-opdrachten doorgeven.

> [!TIP]
> Voor Zie snelstartgidsen waarin u moet configureren en verbinding maken met een beheerd exemplaar van SQL Server Management Studio op Microsoft Windows, [Quick Start: Azure VM configureren om verbinding te maken met een Azure SQL Database Managed Instance](sql-database-managed-instance-configure-vm.md) en [quickstart: Een punt-naar-site-verbinding configureren naar een beheerd exemplaar voor Azure SQL Database van on-premises](sql-database-managed-instance-configure-p2s.md).
> [!IMPORTANT]
> U kunt maken of verwijderen van een beheerd exemplaar met behulp van Transact-SQL.

| Opdracht | Description |
| --- | --- |
|[CREATE DATABASE](https://docs.microsoft.com/sql/t-sql/statements/create-database-transact-sql?view=azuresqldb-mi-current)|Hiermee maakt u een nieuwe database in de Managed Instance. U moet zijn verbonden met de hoofddatabase om een nieuwe database te maken.|
| [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-mi-current) |Hiermee wijzigt u een beheerd exemplaar voor Azure SQL-database.|

## <a name="rest-api-create-and-manage-managed-instances"></a>REST-API: Maken en beheren van beheerde exemplaren

Als u wilt maken en beheren van beheerde instanties, gebruikt u deze REST-API-aanvragen.

| Opdracht | Description |
| --- | --- |
|[Beheerde exemplaren - maken of bijwerken](https://docs.microsoft.com/rest/api/sql/managedinstances/createorupdate)|Hiermee of een beheerd exemplaar bijwerken.|
|[Beheerde exemplaren - verwijderen](https://docs.microsoft.com/rest/api/sql/managedinstances/delete)|Hiermee verwijdert u een beheerd exemplaar.|
|[Beheerde exemplaren - Get](https://docs.microsoft.com/rest/api/sql/managedinstances/get)|Hiermee haalt u een beheerd exemplaar.|
|[Beheerde exemplaren - lijst](https://docs.microsoft.com/rest/api/sql/managedinstances/list)|Retourneert een lijst met beheerde instanties in een abonnement.|
|[Beheerde exemplaren - lijst per groep](https://docs.microsoft.com/rest/api/sql/managedinstances/listbyresourcegroup)|Retourneert een lijst met beheerde instanties in een resourcegroep.|
|[Beheerde exemplaren - Update](https://docs.microsoft.com/rest/api/sql/managedinstances/update)|Een beheerd exemplaar van updates.|

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het migreren van een SQL Server-database naar Azure, [migreren naar Azure SQL Database](sql-database-single-database-migrate.md).
- Zie [Functies](sql-database-features.md) voor meer informatie over ondersteunde functies.
