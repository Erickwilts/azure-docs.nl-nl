---
title: Een verwijderde toegewezen SQL-groep herstellen
description: Instructies voor het herstellen van een verwijderde SQL-groep in azure Synapse Analytics.
services: synapse-analytics
author: anumjs
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 08/29/2018
ms.author: anjangsh
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 29edf6ebd451bf05fe24249eeacb416a70001d56
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/04/2020
ms.locfileid: "93313566"
---
# <a name="restore-a-deleted-dedicated-sql-pool-in-azure-synapse-analytics"></a>Een verwijderde, toegewezen SQL-groep herstellen in azure Synapse Analytics

In dit artikel leert u hoe u een toegewezen SQL-groep kunt herstellen met behulp van de Azure Portal of Power shell.

## <a name="before-you-begin"></a>Voordat u begint

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

**Controleer de DTU-capaciteit.** Elke toegewezen SQL-groep wordt gehost door een [logische SQL-Server](../../azure-sql/database/logical-servers.md) (bijvoorbeeld MyServer.database.Windows.net) die een standaard DTU-quotum heeft.  Controleer of de server voldoende resterende DTU-quota heeft voor de data base die wordt hersteld. Zie [een wijziging in een DTU-quotum aanvragen](sql-data-warehouse-get-started-create-support-ticket.md)voor meer informatie over het berekenen van de benodigde DTU of om meer DTU aan te vragen.

## <a name="restore-a-deleted-data-warehouse-through-powershell"></a>Een verwijderd Data Warehouse herstellen via Power shell

Als u een verwijderde toegewezen SQL-groep wilt herstellen, gebruikt u de cmdlet [Restore-AzSqlDatabase](/powershell/module/az.sql/restore-azsqldatabase?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) . Als de bijbehorende server ook is verwijderd, kunt u dat data warehouse niet herstellen.

1. Voordat u begint, moet u ervoor zorgen dat u [Azure PowerShell installeert](/powershell/azure/?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json).
2. Open PowerShell.
3. Maak verbinding met uw Azure-account en vermeld alle abonnementen die aan uw account zijn gekoppeld.
4. Selecteer het abonnement dat de verwijderde toegewezen SQL-groep bevat die moet worden hersteld.
5. Het specifieke verwijderde data warehouse ophalen.
6. De verwijderde toegewezen SQL-groep herstellen
    1. Als u de verwijderde toegewezen SQL-groep naar een andere server wilt herstellen, moet u de naam van de andere server opgeven.  Deze server kan zich ook in een andere resource groep en regio bevinden.
    1. Als u wilt herstellen naar een ander abonnement, gebruikt u de knop [verplaatsen](../../azure-resource-manager/management/move-resource-group-and-subscription.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json#use-the-portal) om de server naar een ander abonnement te verplaatsen.
7. Controleer of het herstelde data warehouse online is.
8. Nadat de herstel bewerking is voltooid, kunt u uw herstelde data warehouse configureren door [de data base na het herstel te configureren](../../azure-sql/database/disaster-recovery-guidance.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json#configure-your-database-after-recovery).

```Powershell
$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
#$TargetResourceGroupName="<YourTargetResourceGroupName>" # uncomment to restore to a different server.
#$TargetServerName="<YourtargetServerNameWithoutURLSuffixSeeNote>"
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Connect-AzAccount
Get-AzSubscription
Select-AzSubscription -SubscriptionName $SubscriptionName

# Get the deleted database to restore
$DeletedDatabase = Get-AzSqlDeletedDatabaseBackup -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Restore deleted database
$RestoredDatabase = Restore-AzSqlDatabase –FromDeletedDatabaseBackup –DeletionDate $DeletedDatabase.DeletionDate -ResourceGroupName $DeletedDatabase.ResourceGroupName -ServerName $DeletedDatabase.ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $DeletedDatabase.ResourceID

# Use the following command to restore deleted data warehouse to a different server
#$RestoredDatabase = Restore-AzSqlDatabase –FromDeletedDatabaseBackup –DeletionDate $DeletedDatabase.DeletionDate -ResourceGroupName $TargetResourceGroupName -ServerName $TargetServerName -TargetDatabaseName $NewDatabaseName –ResourceId $DeletedDatabase.ResourceID

# Verify the status of restored database
$RestoredDatabase.status
```

## <a name="restore-a-deleted-database-using-the-azure-portal"></a>Een verwijderde data base herstellen met behulp van de Azure Portal

1. Meld u aan bij de [Azure-portal](https://portal.azure.com/).
2. Ga naar de server waarop uw verwijderde data warehouse werd gehost.
3. Selecteer het pictogram **Verwijderde data bases** in de inhouds opgave.

    ![Verwijderde data bases](./media/sql-data-warehouse-restore-deleted-dw/restoring-deleted-01.png)

4. Selecteer de verwijderde Azure Synapse Analytics die u wilt herstellen.

    ![Verwijderde databases selecteren](./media/sql-data-warehouse-restore-deleted-dw/restoring-deleted-11.png)

5. Geef een nieuwe **database naam** op en klik op **OK**

    ![Database naam opgeven](./media/sql-data-warehouse-restore-deleted-dw/restoring-deleted-21.png)

## <a name="next-steps"></a>Volgende stappen

- [Een bestaande toegewezen SQL-groep herstellen](sql-data-warehouse-restore-active-paused-dw.md)
- [Herstellen vanuit een toegewezen SQL-groep met geo-back-ups](sql-data-warehouse-restore-from-geo-backup.md)
