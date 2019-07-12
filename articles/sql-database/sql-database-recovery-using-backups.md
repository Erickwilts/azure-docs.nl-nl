---
title: Een Azure SQL-database herstellen vanuit een back-up | Microsoft Docs
description: Meer informatie over Point-in-Time-Restore, waarmee u terug moet brengen een Azure SQL Database naar een eerder punt in tijd (maximaal 35 dagen).
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
ms.date: 04/30/2019
ms.openlocfilehash: 80d01a360a2f80749bd7fbe7a9aadb9dda1189c6
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67706993"
---
# <a name="recover-an-azure-sql-database-using-automated-database-backups"></a>Een Azure SQL-database met behulp van geautomatiseerde databaseback-ups herstellen

Back-ups van SQL-Database worden standaard opgeslagen in geo-replicatie blob-opslag (RA-GRS). De volgende opties zijn beschikbaar voor het gebruik van de database recovery [automatische databaseback-ups](sql-database-automated-backups.md):

- Maak een nieuwe database op dezelfde SQL-Database-server hersteld naar een opgegeven punt in tijd binnen de bewaarperiode liggen.
- Maak een database op dezelfde SQL-Database-server op het tijdstip van verwijdering van een verwijderde database kunt herstellen.
- Maak een nieuwe database op elke SQL-Database-server in dezelfde regio hersteld tot het tijdstip waarop de meest recente back-ups.
- Maak een nieuwe database op elke SQL-Database-server in een andere regio op het punt van de meest recente gerepliceerde back-ups kunt herstellen.

Als u hebt geconfigureerd [back-up maken met een langetermijnbewaarperiode](sql-database-long-term-retention.md), u kunt ook een nieuwe database maken van alle LTR back-up op elke SQL-Database-server.

> [!IMPORTANT]
> U kunt een bestaande database tijdens het terugzetten niet overschrijven.

Bij het gebruik van Standard of Premium-servicelaag, maakt een herstelde database een kosten extra opslagruimte in de volgende omstandigheden:

- Herstellen van P11-P15 S4-S12 of P1 – P6 als de maximale databasegrootte groter is dan 500 GB.
- Herstellen van P1 – P6 voor S4-S12 als de maximale grootte van de database groter dan 250 GB is.

De extra kosten zijn icurred wanneer de maximale grootte van de herstelde database groter is dan de hoeveelheid opslag die is opgenomen met de doel-database-servicelaag en het prestatieniveau serviceniveau. De extra opslagruimte die boven de inbegrepen hoeveelheid is extra kosten verbonden. Informatie over prijzen van extra opslagruimte, Zie de [pagina met prijzen van SQL-Database](https://azure.microsoft.com/pricing/details/sql-database/). Als de werkelijke hoeveelheid gebruikte ruimte kleiner dan de hoeveelheid opslag die is opgenomen, kunt u voorkomen dat dit extra kosten door de maximale databasegrootte op de hoeveelheid bij inbegrepen.

> [!NOTE]
> [Automatische databaseback-ups](sql-database-automated-backups.md) worden gebruikt bij het maken van een [kopiëren van de database](sql-database-copy.md).

## <a name="recovery-time"></a>Hersteltijd

De hersteltijd om terug te zetten van een database met behulp van geautomatiseerde databaseback-ups wordt beïnvloed door diverse factoren:

- De grootte van de database
- De compute-grootte van de database
- Het aantal betrokken transactielogboeken
- De hoeveelheid activiteit die moet opnieuw worden afgespeeld om te herstellen naar het herstelpunt
- De bandbreedte van het netwerk als het herstellen naar een andere regio
- Het aantal gelijktijdige restore-aanvragen worden verwerkt in de doelregio

Voor een grote en/of zeer actief-database, kan het terugzetten van enkele uren duren. Als er langdurige storing in een regio, is het mogelijk dat er grote aantallen geo-restore-aanvragen worden verwerkt door een andere regio's zijn. Wanneer er veel aanvragen, verhogen de hersteltijd voor databases in deze regio. De meeste worden voltooid in minder dan 12 uur hersteld.

Voor één abonnement zijn er beperkingen met betrekking tot het aantal gelijktijdige terugzetten aanvragen.  Deze beperkingen van toepassing op een willekeurige combinatie van punt in tijd herstelbewerkingen, geo-herstellen en terugzetten van langetermijnbewaarperiode):

| | **Maximum aantal gelijktijdige aanvragen worden verwerkt** | **Maximum aantal gelijktijdige aanvragen worden ingediend** |
| :--- | --: | --: |
|Individuele database (per abonnement)|10|60|
|Elastische pool (per groep)|4|200|
||||

Er is momenteel niet geen ingebouwde methode om de hele server te herstellen. De [Azure SQL Database: Volledige Serverherstelproces](https://gallery.technet.microsoft.com/Azure-SQL-Database-Full-82941666) script is een voorbeeld van hoe u deze taak kunt uitvoeren.

> [!IMPORTANT]
> Als u wilt herstellen met behulp van geautomatiseerde back-ups, moet u lid zijn van de rol Inzender voor SQL Server in het abonnement of raadpleegt u de eigenaar van de abonnement - [RBAC: Ingebouwde rollen](../role-based-access-control/built-in-roles.md). U kunt herstellen met behulp van Azure Portal, PowerShell of de REST-API. U kunt Transact-SQL niet gebruiken.

## <a name="point-in-time-restore"></a>Terugzetten naar eerder tijdstip

U kunt een zelfstandige, gegroepeerd, herstellen of instantie-database naar een eerder tijdstip met behulp van de Azure-portal [PowerShell](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqldatabase), of de [REST-API](https://docs.microsoft.com/rest/api/sql/databases). De aanvraag kunt opgeven van een servicelaag of compute-grootte voor de herstelde database. Zorg ervoor dat er voldoende bronnen op de server waarop u de database wilt herstellen. Als u klaar bent, wordt een nieuwe database op dezelfde server als de oorspronkelijke database worden gemaakt. De herstelde database wordt gefactureerd tegen normale tarieven op basis van de servicelaag en de rekencapaciteit. U doet geen kosten in rekening totdat het herstellen van de database is voltooid.

U kunt in het algemeen een database herstellen naar een eerder tijdstip voor herstel voor. U kunt de herstelde database behandelen als vervanging voor de oorspronkelijke database of bijwerken van de oorspronkelijke database gebruiken als een bron van gegevens.

- **Vervanging van de database**

  Als de herstelde database is bedoeld als vervanging voor de oorspronkelijke database, moet u de oorspronkelijke database compute grootte en de servicelaag. U kunt vervolgens Wijzig de naam van de oorspronkelijke database en de herstelde database geven de oorspronkelijke naam met behulp van de [ALTER DATABASE](/sql/t-sql/statements/alter-database-azure-sql-database) opdracht in T-SQL.

- **Herstel van gegevens**

  Als u van plan bent voor het ophalen van gegevens uit de herstelde database te herstellen van een gebruiker of toepassing fout, moet u om te schrijven en uitvoeren van een script voor het herstel van gegevens die haalt gegevens uit de herstelde database en is van toepassing op de oorspronkelijke database. Hoewel de herstelbewerking lang duren kan om uit te voeren, is het herstellen van de database is zichtbaar in de lijst van de database tijdens het herstelproces. Als u de database tijdens de terugzetbewerking verwijdert, wordt de herstelbewerking geannuleerd en voor de database die is niet voltooid voor het herstellen wordt er geen kosten in rekening gebracht.

Om te herstellen van een enkele, gegroepeerd, of database naar een eerder tijdstip met behulp van de Azure-portal-exemplaar, open de pagina voor uw database en klik op **herstellen** op de werkbalk.

![point-in-time-restore](./media/sql-database-recovery-using-backups/point-in-time-recovery.png)

> [!IMPORTANT]
> Programmatisch database herstellen vanuit een back-up, Zie [via een programma uitvoeren recovery met behulp van geautomatiseerde back-ups](sql-database-recovery-using-backups.md#programmatically-performing-recovery-using-automated-backups)

## <a name="deleted-database-restore"></a>Verwijderde database herstellen

U kunt een verwijderde database herstellen naar het tijdstip van verwijdering of een eerder tijdstip tijdstip op dezelfde SQL-Database-server met behulp van Azure portal, [PowerShell](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqldatabase), of de [REST (createMode = terugzetten)](https://docs.microsoft.com/rest/api/sql/databases/createorupdate). U kunt [verwijderde database herstellen in het beheerde exemplaar met behulp van PowerShell](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../recreate-dropped-database-on-azure-sql-managed-instance). 

> [!TIP]
> Zie voor een voorbeeld van PowerShell-script waarin wordt getoond hoe een verwijderde database herstellen, [herstellen van een SQL-database met behulp van PowerShell](scripts/sql-database-restore-database-powershell.md).
> [!IMPORTANT]
> Als u een Azure SQL Database server-exemplaar verwijdert, worden alle bijbehorende databases worden ook verwijderd en kunnen niet worden hersteld. Er is momenteel geen ondersteuning voor het herstellen van een verwijderde server.

### <a name="deleted-database-restore-using-the-azure-portal"></a>Verwijderde database herstellen met behulp van de Azure portal

Als u wilt herstellen van een verwijderde database met behulp van de Azure portal, open de pagina voor uw server en in de Operations-gebied, klikt u op **verwijderde databases**.

![deleted-database-restore-1](./media/sql-database-recovery-using-backups/deleted-database-restore-1.png)

![deleted-database-restore-2](./media/sql-database-recovery-using-backups/deleted-database-restore-2.png)

> [!IMPORTANT]
> Als u programmatisch een verwijderde database herstellen, Zie [via een programma uitvoeren recovery met behulp van geautomatiseerde back-ups](sql-database-recovery-using-backups.md#programmatically-performing-recovery-using-automated-backups)

## <a name="geo-restore"></a>Geo-herstel

U kunt een SQL-database op elke server in een Azure-regio van de meest recente geo-replicatie back-ups herstellen. Geo-herstel maakt gebruik van een geo-replicatie back-up als de bron. Het kan worden aangevraagd, zelfs als de database of het datacenter niet toegankelijk als gevolg van een storing is.

Geo-restore is de standaardoptie voor herstel wanneer de database niet beschikbaar vanwege een incident in de regio van de host is. U kunt de database herstellen naar een server in een andere regio. Er is een vertraging tussen wanneer een back-up is gemaakt en wanneer het zich geo-replicatie naar een Azure blob in een andere regio. Als gevolg hiervan mag de herstelde database maximaal één uur achter de bestaande database. De volgende afbeelding ziet het herstellen van de database van de laatste beschikbare back-up in een andere regio.

![geo-restore](./media/sql-database-geo-restore/geo-restore-2.png)

> [!TIP]
> Zie voor een voorbeeld van PowerShell-script waarin wordt getoond hoe om uit te voeren van een geo-restore, [herstellen van een SQL-database met behulp van PowerShell](scripts/sql-database-restore-database-powershell.md).

Point-in-time terugzetten op een secundaire geo-server is momenteel niet ondersteund. Point-in-time restore kan alleen op een primaire database worden uitgevoerd. Zie voor gedetailleerde informatie over het gebruik van geo-herstel te herstellen na een storing [herstellen na een storing](sql-database-disaster-recovery.md).

> [!IMPORTANT]
> Geo-restore is de meest eenvoudige oplossing voor noodherstel beschikbaar zijn in SQL Database. Er wordt gebruikgemaakt van back-ups via geo-replicatie automatisch gemaakt met RPO = 1 uur en de geschatte hersteltijd van maximaal 12 uur. Er is geen garantie dat de doelregio capaciteit voor uw database (s) niet herstellen na een regionale ourage omdat een sharp toename van de aanvraag waarschijnlijk zal hebben. Geo-restore is voor kritieke toepassing niet zakelijke die gebruikmaken van relatief kleine databases, oplossing voor een juiste noodherstel. Voor bedrijfskritieke toepassingen die gebruikmaken van grote databases en bedrijfscontinuïteit te waarborgen, moet u [automatische failovergroepen](sql-database-auto-failover-group.md). Het biedt een veel lager RPO en RTO en de capaciteit is altijd gegarandeerd. Zie voor meer informatie over opties voor bedrijfscontinuïteit, [overzicht van bedrijfscontinuïteit](sql-database-business-continuity.md).

### <a name="geo-restore-using-the-azure-portal"></a>Geo-herstellen met behulp van de Azure-portal

Voor geo-restore a database tijdens de [bewaarperiode op basis van DTU-model](sql-database-service-tiers-dtu.md) of [bewaarperiode op vCore gebaseerde model](sql-database-service-tiers-vcore.md) met behulp van de Azure-portal, open de pagina SQL-Databases en klik vervolgens op **toevoegen** . In de **bron selecteren** in het tekstvak, selecteer **back-up**. Geef de back-up van waaruit het herstel wilt uitvoeren in de regio en op de server van uw keuze.

> [!Note]
> Geo-herstellen met behulp van de Azure-portal is niet beschikbaar in het beheerde exemplaar. Gebruik in plaats daarvan PowerShell.

## <a name="programmatically-performing-recovery-using-automated-backups"></a>Herstel met behulp van geautomatiseerde back-ups uitvoeren via een programma

Zoals eerder is besproken, naast de Azure portal, kan databaseherstel worden uitgevoerd via een programma met behulp van Azure PowerShell of de REST-API. De volgende tabellen beschrijven de reeks opdrachten die beschikbaar zijn.

### <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> De PowerShell Azure Resource Manager-module nog steeds wordt ondersteund door Azure SQL Database, maar alle toekomstige ontwikkeling is voor de module Az.Sql. Zie voor deze cmdlets [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). De argumenten voor de opdrachten in de Az-module en de AzureRm-modules zijn vrijwel identiek zijn.

- Als u een zelfstandige of gegroepeerde-database herstellen, Zie [terugzetten AzSqlDatabase](/powershell/module/az.sql/restore-azsqldatabase).

  | Cmdlet | Description |
  | --- | --- |
  | [Get-AzSqlDatabase](/powershell/module/az.sql/get-azsqldatabase) |Hiermee haalt u een of meer databases op. |
  | [Get-AzSqlDeletedDatabaseBackup](/powershell/module/az.sql/get-azsqldeleteddatabasebackup) | Hiermee haalt u een verwijderde database die u kunt herstellen op. |
  | [Get-AzSqlDatabaseGeoBackup](/powershell/module/az.sql/get-azsqldatabasegeobackup) |Hiermee haalt u een geografisch redundante back-up van een database op. |
  | [Restore-AzSqlDatabase](/powershell/module/az.sql/restore-azsqldatabase) |Hiermee herstelt u een SQL-database. |

  > [!TIP]
  > Zie voor een voorbeeld van PowerShell-script waarin wordt getoond hoe om uit te voeren van een point-in-time-terugzetten van een database, [herstellen van een SQL-database met behulp van PowerShell](scripts/sql-database-restore-database-powershell.md).

- Als u een beheerd exemplaar in de database herstellen, Zie [terugzetten AzSqlInstanceDatabase](/powershell/module/az.sql/restore-azsqlinstancedatabase).

  | Cmdlet | Description |
  | --- | --- |
  | [Get-AzSqlInstance](/powershell/module/az.sql/get-azsqlinstance) |Hiermee haalt u een of meer beheerde exemplaren. |
  | [Get-AzSqlInstanceDatabase](/powershell/module/az.sql/get-azsqlinstancedatabase) | Hiermee haalt u een exemplaar van databases. |
  | [Restore-AzSqlInstanceDatabase](/powershell/module/az.sql/restore-azsqlinstancedatabase) |Een exemplaar in de database herstelt. |

### <a name="rest-api"></a>REST-API

Een enkele of gegroepeerde om database te herstellen met behulp van de REST-API:

| API | Description |
| --- | --- |
| [REST (createMode=Recovery)](https://docs.microsoft.com/rest/api/sql/databases) |Hiermee herstelt u een database |
| [Get maken of bijwerken van de Status van Database](https://docs.microsoft.com/rest/api/sql/operations) |De status geretourneerd tijdens een herstelbewerking |

### <a name="azure-cli"></a>Azure-CLI

- Zie voor het herstellen van een enkele of gegroepeerde database met behulp van Azure CLI, [az sql db restore](/cli/azure/sql/db#az-sql-db-restore).
- Zie voor het herstellen van een beheerd exemplaar met behulp van Azure CLI, [az sql DEELB herstellen](/cli/azure/sql/midb#az-sql-midb-restore)

## <a name="summary"></a>Samenvatting

Automatische back-ups beveiligen uw databases van gebruiker en toepassingsfouten, per ongeluk database verwijderen en langdurige uitval. Deze ingebouwde mogelijkheid is beschikbaar voor alle service-lagen en compute-grootten.

## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht voor zakelijke continuïteit en scenario's, [overzicht voor zakelijke continuïteit](sql-database-business-continuity.md).
- Voor meer informatie over Azure SQL Database geautomatiseerde back-ups, Zie [geautomatiseerde back-ups van SQL-Database](sql-database-automated-backups.md).
- Zie voor meer informatie over met een langetermijnbewaarperiode, [langetermijnretentie](sql-database-long-term-retention.md).
- Zie voor meer informatie over opties voor sneller herstel, [actieve geo-replicatie](sql-database-active-geo-replication.md) of [automatische failovergroepen](sql-database-auto-failover-group.md).
