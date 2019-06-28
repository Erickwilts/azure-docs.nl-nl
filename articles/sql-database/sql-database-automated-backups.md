---
title: Azure SQL-Database automatische, geografisch redundante back-ups | Microsoft Docs
description: SQL-Database maakt een back-up van de lokale database om de paar minuten en maakt gebruik van Azure geografisch redundante opslag met leestoegang voor geo-redundantie automatisch.
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
ms.date: 05/20/2019
ms.openlocfilehash: 3e56244f074e31672cf77bc74998096e215a4db7
ms.sourcegitcommit: a7ea412ca4411fc28431cbe7d2cc399900267585
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2019
ms.locfileid: "67357336"
---
# <a name="automated-backups"></a>Automatische back-ups

SQL-Database automatisch wordt gemaakt van de databaseback-ups die tussen 7 en 35 dagen worden bewaard en maakt gebruik van Azure [geo-redundante opslag met leestoegang (RA-GRS)](../storage/common/storage-redundancy-grs.md#read-access-geo-redundant-storage) om ervoor te zorgen dat ze behouden blijft, zelfs als het datacenter niet beschikbaar is. Deze back-ups worden gemaakt automatisch en zonder extra kosten. U hoeft te doen zodat ze zich voordoen. Databaseback-ups zijn een essentieel onderdeel van een strategie voor zakelijke continuïteit en noodherstel omdat ze uw gegevens tegen per ongeluk beschadigd of verwijderd beschermen. Als uw beveiligingsregels is vereist dat uw back-ups beschikbaar voor een lange periode (maximaal 10 jaar zijn), kunt u een [langetermijnretentie](sql-database-long-term-retention.md) op Singleton-databases en elastische pools.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="what-is-a-sql-database-backup"></a>Wat is er een back-up van SQL Database

SQL-Database maakt gebruik van SQL Server-technologie maken [volledige back-ups](https://docs.microsoft.com/sql/relational-databases/backup-restore/full-database-backups-sql-server) elke week [differentiële back-ups](https://docs.microsoft.com/sql/relational-databases/backup-restore/differential-backups-sql-server) elke 12 uur en [transactielogboekback-ups](https://docs.microsoft.com/sql/relational-databases/backup-restore/transaction-log-backups-sql-server) elke 5-10 minuten. De back-ups worden opgeslagen in [RA-GRS-opslag-blobs](../storage/common/storage-redundancy-grs.md#read-access-geo-redundant-storage) die worden gerepliceerd naar een [gekoppeld Datacenter](../best-practices-availability-paired-regions.md) voor bescherming tegen een storing in het datacenter. Wanneer u een database herstelt, wordt de service zoekt uit welke volledige, differentiële en transactie-logboek back-ups moeten worden teruggezet.

U kunt deze back-ups te gebruiken:

- **Een bestaande database herstellen naar een punt-in-time in het verleden** binnen de bewaarperiode liggen die met behulp van de Azure portal, Azure PowerShell, Azure CLI of REST-API. Met deze bewerking wordt een nieuwe database op dezelfde server als de oorspronkelijke database maken in individuele databases en elastische pools. In het beheerde exemplaar kunt met deze bewerking maken van een kopie van de database of dezelfde of verschillende Managed Instance onder hetzelfde abonnement.
  - **[Wijzigen van de back-up bewaarperiode](#how-to-change-the-pitr-backup-retention-period)**  tussen 7 tot 35 dagen voor het configureren van uw back-upbeleid.
  - **Beleid met een langetermijnbewaarperiode gewijzigd van 10 jaar** op individuele databases en elastische Pools met behulp van [de Azure-portal](sql-database-long-term-backup-retention-configure.md#configure-long-term-retention-policies) of [Azure PowerShell](sql-database-long-term-backup-retention-configure.md#use-powershell-to-configure-long-term-retention-policies-and-restore-backups).
- **Een verwijderde database herstellen tot het moment waarop het werd verwijderd** of op elk gewenst moment binnen de bewaarperiode liggen. De verwijderde database kan alleen worden hersteld in de dezelfde logische server of een beheerd exemplaar waar de oorspronkelijke database is gemaakt.
- **Een database herstellen naar een andere geografische regio**. Geo-restore kunt u herstellen na een noodgeval geografische wanneer u geen toegang uw server en database tot. Het maakt een nieuwe database in een bestaande server overal ter wereld.
- **Een database herstellen vanuit een specifieke langetermijnback-up** op één Database of elastische Pool als de database is geconfigureerd met een beleid met een langetermijnbewaarperiode (LTR). LTR kunt u een oude versie van het gebruik van de database terugzetten [de Azure-portal](sql-database-long-term-backup-retention-configure.md#view-backups-and-restore-from-a-backup-using-azure-portal) of [Azure PowerShell](sql-database-long-term-backup-retention-configure.md#use-powershell-to-configure-long-term-retention-policies-and-restore-backups) om te voldoen aan een aanvraag voor naleving of uit te voeren van een oude versie van de toepassing. Zie [Langetermijnretentie](sql-database-long-term-retention.md) voor meer informatie.
- Als u wilt een herstelbewerking uitvoert, Zie [-database herstellen vanuit back-ups](sql-database-recovery-using-backups.md).

> [!NOTE]
> In de Azure-opslag, de term *replicatie* verwijst naar het kopiëren van bestanden van de ene locatie naar een andere. De SQL *databasereplicatie* verwijst naar meerdere secundaire databases met een primaire database gesynchroniseerd te houden.

U kunt sommige van deze bewerkingen met behulp van de volgende voorbeelden:

| | Azure Portal | Azure PowerShell |
|---|---|---|
| Wijzigen van de back-upretentie | [Individuele Database](sql-database-automated-backups.md#change-pitr-backup-retention-period-using-the-azure-portal) <br/> [Beheerd exemplaar](sql-database-automated-backups.md#change-pitr-for-a-managed-instance) | [Individuele Database](sql-database-automated-backups.md#change-pitr-backup-retention-period-using-powershell) <br/>[Beheerd exemplaar](https://docs.microsoft.com/powershell/module/az.sql/set-azsqlinstancedatabasebackupshorttermretentionpolicy) |
| Langetermijnretentie van back-ups wijzigen | [Individuele database](sql-database-long-term-backup-retention-configure.md#configure-long-term-retention-policies)<br/>Managed Instance - N.V.T.  | [Individuele Database](sql-database-long-term-backup-retention-configure.md#use-powershell-to-configure-long-term-retention-policies-and-restore-backups)<br/>Managed Instance - N.V.T.  |
| Database van een point-in-time herstellen | [Individuele database](sql-database-recovery-using-backups.md#point-in-time-restore) | [Individuele database](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqldatabase) <br/> [Beheerd exemplaar](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqlinstancedatabase) |
| Verwijderde database herstellen | [Individuele database](sql-database-recovery-using-backups.md#deleted-database-restore-using-the-azure-portal) | [Individuele database](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldeleteddatabasebackup) <br/> [Beheerd exemplaar](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldeletedinstancedatabasebackup)|
| Database herstellen van Azure Blob-opslag | Individuele database - N.V.T. <br/>Managed Instance - N.V.T.  | Individuele database - N.V.T. <br/>[Beheerd exemplaar](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-get-started-restore) |

## <a name="how-long-are-backups-kept"></a>Hoe lang worden back-ups opgeslagen

Elke SQL-Database heeft een back-up bewaartermijn tussen 7 en 35 dagen die afhankelijk zijn van de aankopen model en de servicelaag. U kunt de bewaarperiode voor back-up voor een database op SQL Database-server bijwerken. Zie voor meer informatie, [bewaarperiode voor back-up wijzigen](#how-to-change-the-pitr-backup-retention-period).

Als u een database verwijdert, blijven SQL-Database de back-ups op dezelfde manier als zou voor een online-database. Als u een Basic-database met een bewaarperiode van zeven dagen verwijdert, wordt bijvoorbeeld een back-up die vier dagen oud is drie dagen opgeslagen.

Als u de back-ups langer dan de maximale bewaarperiode houden wilt, kunt u de back-eigenschappen voor het toevoegen van een of meer langdurige bewaarperioden met uw database wijzigen. Zie [Langetermijnretentie](sql-database-long-term-retention.md) voor meer informatie.

> [!IMPORTANT]
> Als u de Azure SQL-server die als host fungeert voor SQL-databases verwijdert, worden alle elastische pools en databases die deel uitmaken van de server worden ook verwijderd en kunnen niet worden hersteld. U kunt een server verwijderd niet herstellen. Maar als u met een langetermijnbewaarperiode hebt geconfigureerd, wordt de back-ups voor de databases met LTR worden niet verwijderd en wordt deze databases kunnen worden hersteld.

### <a name="default-backup-retention-period"></a>Bewaartermijn voor back-up

#### <a name="dtu-based-purchasing-model"></a>DTU gebaseerde aankoopmodel

De bewaartermijn voor een database gemaakt met behulp van het op DTU gebaseerde aankoopmodel, is afhankelijk van de service tier:

- Basis-servicelaag is **één** week.
- Standard-servicelaag **vijf** weken.
- Premium-servicelaag **vijf** weken.

#### <a name="vcore-based-purchasing-model"></a>Aankoopmodel op basis van vCore

Als u de [vCore gebaseerde aankoopmodel](sql-database-service-tiers-vcore.md), is de standaardperiode voor back-upretentie **zeven** dagen (voor één, gegroepeerd en databases-instantie). Voor alle Azure SQL-databases (één, gegroepeerd, en het exemplaar van databases, kunt u [wijzigen bewaarperiode voor back-tot 35 dagen](#how-to-change-the-pitr-backup-retention-period).

> [!WARNING]
> Als u de huidige bewaarperiode verkorten, zijn alle bestaande back-ups ouder is dan de nieuwe bewaarperiode niet meer beschikbaar. Als u de huidige retentietermijn verhoogt, wordt SQL Database de bestaande back-ups behouden totdat de langere bewaarperiode is bereikt.

## <a name="how-often-do-backups-happen"></a>Hoe vaak gebeurt er back-ups

### <a name="backups-for-point-in-time-restore"></a>Back-ups voor point-in-time restore

SQL Database biedt ondersteuning voor self-service voor point-in-time restore (PITR) door het automatisch maken van volledige back-up, differentiële back-ups en transactielogboekback-ups. Volledige databaseback-ups wekelijks worden gemaakt, differentiële databaseback-ups worden in het algemeen gemaakt voor elke 12 uur en transactielogboekback-ups worden meestal elke 5-10 minuten, gemaakt met de frequentie op basis van de rekencapaciteit en hoeveelheid activiteit in een database. De eerste volledige back-up is gepland, onmiddellijk nadat een database wordt gemaakt. Deze meestal binnen 30 minuten is voltooid, maar het kan langer duren als de database van een aanzienlijke grootte is. Bijvoorbeeld, de eerste back-up kan dit langer duren voor een herstelde database of een databasekopie. Na de eerste volledige back-up, worden alle verdere back-ups automatisch gepland en beheerd op de achtergrond op de achtergrond. De precieze timing van alle databaseback-ups wordt bepaald door de service SQL Database, zoals het geeft een balans de werkbelasting van het algehele systeem tussen. U kan wijzigen of uitschakelen van de back-uptaken. 

De PITR back-ups zijn geografisch redundante en beveiligd door [Azure Storage-overschrijdend-replicatie](../storage/common/storage-redundancy-grs.md#read-access-geo-redundant-storage)

Zie voor meer informatie, [Point-in-time restore](sql-database-recovery-using-backups.md#point-in-time-restore)

### <a name="backups-for-long-term-retention"></a>Back-ups voor langetermijnretentie

Één en gepoolde databases bieden de mogelijkheid van het configureren van (LTR) met een langetermijnbewaarperiode van volledige back-ups voor maximaal tien jaar in Azure Blob-opslag. Als LTR-beleid is ingeschakeld, worden de wekelijkse volledige back-ups automatisch gekopieerd naar een andere container voor RA-GRS-opslag. Om te voldoen aan verschillende nalevingsvereiste, kunt u verschillende bewaartermijnen voor wekelijkse, maandelijkse en/of jaarlijkse back-ups. Het opslagverbruik, is afhankelijk van de geselecteerde frequentie van back-ups en de retentie-periode. U kunt de [LTR-prijscalculator](https://azure.microsoft.com/pricing/calculator/?service=sql-database) om in te schatten van de kosten voor LTR-opslag.

Zoals PITR, zijn de LTR back-ups geografisch redundante en beveiligd door [Azure Storage-overschrijdend-replicatie](../storage/common/storage-redundancy-grs.md#read-access-geo-redundant-storage).

Zie voor meer informatie, [langetermijnretentie](sql-database-long-term-retention.md).

## <a name="storage-costs"></a>Opslagkosten
Zeven dagen aan geautomatiseerde back-ups van uw databases worden standaard naar RA-GRS-standaardblobopslag gekopieerd. De opslag wordt gebruikt voor wekelijkse volledige back-ups, dagelijkse differentiële back-ups en back-ups van transactielogboeken die om de vijf minuten worden gekopieerd. De grootte van het transactielogboek is afhankelijk van de wijzigingssnelheid van de database. U kunt gratis gebruik maken van opslagruimte ter waarde van 100% van de databasegrootte. Voor aanvullend verbruik van back-upopslag worden GB/maand berekend.

Zie voor meer informatie over prijzen voor gegevensopslag, de [prijzen](https://azure.microsoft.com/pricing/details/sql-database/single/) pagina. 

## <a name="are-backups-encrypted"></a>Back-ups versleuteld

Als uw database is versleuteld met TDE, wordt de back-ups worden automatisch versleuteld in rust, met inbegrip van LTR-back-ups. Als TDE is ingeschakeld voor een Azure SQL database, worden back-ups zijn eveneens versleuteld. Alle nieuwe Azure SQL-databases zijn geconfigureerd met TDE standaard ingeschakeld. Zie voor meer informatie over TDE [Transparent Data Encryption met Azure SQL Database](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql).

## <a name="how-does-microsoft-ensure-backup-integrity"></a>Hoe garandeert Microsoft dat de integriteit van de back-up

Regelmatig test het technische team van Azure SQL Database automatisch het terugzetten van geautomatiseerde databaseback-ups van databases in de service. Bij het herstellen, databases, ontvangen ook integriteitscontroles met DBCC CHECKDB. Problemen met gevonden tijdens de integriteitscontrole resulteert in een waarschuwing aan het technische team. Zie voor meer informatie over de integriteit van gegevens in Azure SQL Database, [integriteit van gegevens in Azure SQL Database](https://azure.microsoft.com/blog/data-integrity-in-azure-sql-database/).

## <a name="how-do-automated-backups-impact-compliance"></a>Hoe geautomatiseerde back-ups van invloed zijn op naleving

Wanneer u uw database van een servicelaag op basis van DTU, met de standaard PITR bewaarperiode van 35 dagen, naar een laag vCore-gebaseerde service migreert, wordt de bewaarperiode PITR behouden om ervoor te zorgen dat beleid voor gegevensherstel van uw toepassing niet worden gecompromitteerd. Als de bewaarperiode standaard niet voldoet aan uw vereisten voor naleving, kunt u de bewaarperiode voor PITR met PowerShell of REST-API wijzigen. Zie voor meer informatie, [bewaarperiode voor back-up wijzigen](#how-to-change-the-pitr-backup-retention-period).

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="how-to-change-the-pitr-backup-retention-period"></a>Het wijzigen van de periode van de back-upretentie PITR

U kunt de PITR back-up standaardbewaartermijn met behulp van de Azure-portal, PowerShell of de REST-API wijzigen. De ondersteunde waarden zijn: 7, 14, 21, 28 of 35 dagen. De volgende voorbeelden laten zien hoe u PITR retentie wijzigen op 28 dagen.

> [!NOTE]
> Deze API's is alleen van invloed op de bewaarperiode PITR. Als u LTR voor uw database hebt geconfigureerd, zullen niet worden beïnvloed. Zie voor meer informatie over het wijzigen van de periode van de bewaarperiode LTR [langetermijnretentie](sql-database-long-term-retention.md).

### <a name="change-pitr-backup-retention-period-using-the-azure-portal"></a>Back-up bewaarperiode PITR met behulp van de Azure-portal wijzigen

Gebaseerd op welke server-object u wijzigt de periode van PITR back-upretentie met behulp van de Azure-portal wijzigen, gaat u naar het object met de server waarvan u wilt wijzigen in de portal en selecteer vervolgens de gewenste optie bewaarperiode.

#### <a name="change-pitr-for-a-sql-database-server"></a>Het wijzigen van PITR voor een SQL Database-server

![Wijziging PITR Azure portal](./media/sql-database-automated-backup/configure-backup-retention-sqldb.png)

#### <a name="change-pitr-for-a-managed-instance"></a>Het wijzigen van PITR voor een beheerd exemplaar

![Wijziging PITR Azure portal](./media/sql-database-automated-backup/configure-backup-retention-sqlmi.png)

### <a name="change-pitr-backup-retention-period-using-powershell"></a>Wijzigen van de back-up bewaarperiode PITR met behulp van PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> De PowerShell Azure Resource Manager-module nog steeds wordt ondersteund door Azure SQL Database, maar alle toekomstige ontwikkeling is voor de module Az.Sql. Zie voor deze cmdlets [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). De argumenten voor de opdrachten in de Az-module en de AzureRm-modules zijn vrijwel identiek zijn.

```powershell
Set-AzSqlDatabaseBackupShortTermRetentionPolicy -ResourceGroupName resourceGroup -ServerName testserver -DatabaseName testDatabase -RetentionDays 28
```

### <a name="change-pitr-retention-period-using-rest-api"></a>Wijzigen van de bewaarperiode PITR met behulp van REST-API

#### <a name="sample-request"></a>Voorbeeldaanvraag

```http
PUT https://management.azure.com/subscriptions/00000000-1111-2222-3333-444444444444/resourceGroups/resourceGroup/providers/Microsoft.Sql/servers/testserver/databases/testDatabase/backupShortTermRetentionPolicies/default?api-version=2017-10-01-preview
```

#### <a name="request-body"></a>Aanvraagtekst

```json
{
  "properties":{
    "retentionDays":28
  }
}
```

#### <a name="sample-response"></a>Voorbeeldantwoord

Statuscode: 200

```json
{
  "id": "/subscriptions/00000000-1111-2222-3333-444444444444/providers/Microsoft.Sql/resourceGroups/resourceGroup/servers/testserver/databases/testDatabase/backupShortTermRetentionPolicies/default",
  "name": "default",
  "type": "Microsoft.Sql/resourceGroups/servers/databases/backupShortTermRetentionPolicies",
  "properties": {
    "retentionDays": 28
  }
}
```

Zie voor meer informatie, [back-up bewaren REST-API](https://docs.microsoft.com/rest/api/sql/backupshorttermretentionpolicies).

## <a name="next-steps"></a>Volgende stappen

- Databaseback-ups zijn een essentieel onderdeel van een strategie voor zakelijke continuïteit en noodherstel omdat ze uw gegevens tegen per ongeluk beschadigd of verwijderd beschermen. Zie voor meer informatie over de andere Azure SQL Database oplossingen voor bedrijfscontinuïteit, [overzicht voor zakelijke continuïteit](sql-database-business-continuity.md).
- Als u wilt herstellen naar een eerder tijdstip met behulp van de Azure-portal, Zie [database herstellen naar een eerder tijdstip met behulp van de Azure-portal](sql-database-recovery-using-backups.md).
- Als u wilt herstellen naar een eerder tijdstip met behulp van PowerShell, Zie [database herstellen naar een eerder tijdstip met behulp van PowerShell](scripts/sql-database-restore-database-powershell.md).
- Om te configureren, beheren en herstellen vanuit een langetermijnretentie van automatische back-ups in Azure Blob-opslag met behulp van de Azure portal, Zie [beheren met behulp van de Azure-portal langetermijnretentie](sql-database-long-term-backup-retention-configure.md).
- Om te configureren, beheren en herstellen vanuit een langetermijnretentie van automatische back-ups in Azure Blob-opslag met behulp van PowerShell, raadpleegt u [beheren met behulp van PowerShell langetermijnretentie](sql-database-long-term-backup-retention-configure.md).
