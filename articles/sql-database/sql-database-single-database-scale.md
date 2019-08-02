---
title: Eén database Resource schalen-Azure SQL Database | Microsoft Docs
description: In dit artikel wordt beschreven hoe u de berekenings-en opslag resources schaalt die beschikbaar zijn voor één data base in Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
ms.date: 04/26/2019
ms.openlocfilehash: e03c68854d9150c25019fe198fe855a011750844
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68566559"
---
# <a name="scale-single-database-resources-in-azure-sql-database"></a>Enkele database resources in Azure SQL Database schalen

In dit artikel wordt beschreven hoe u de berekenings-en opslag resources schaalt die beschikbaar zijn voor één data base in de ingerichte Compute-laag. Het is ook mogelijk dat de COMPUTE- [laag zonder server (preview)](sql-database-serverless.md) automatische schaal aanpassing en facturen per seconde biedt voor gebruikte reken kracht.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> De Power shell-Azure Resource Manager module wordt nog steeds ondersteund door Azure SQL Database, maar alle toekomstige ontwikkeling is voor de module AZ. SQL. Zie [AzureRM. SQL](https://docs.microsoft.com/powershell/module/AzureRM.Sql/)voor deze cmdlets. De argumenten voor de opdrachten in de module AZ en in de AzureRm-modules zijn aanzienlijk identiek.

## <a name="change-compute-size-vcores-or-dtus"></a>Berekenings grootte wijzigen (vCores of Dtu's)

Nadat u het aantal vCores of Dtu's hebt gekozen, kunt u een enkele data base dynamisch omhoog of omlaag schalen op basis van de werkelijke ervaring met behulp van de [Azure Portal](sql-database-single-databases-manage.md#manage-an-existing-sql-database-server), [Transact-SQL](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current#examples-1), [Power shell](/powershell/module/az.sql/set-azsqldatabase), de [Azure cli](/cli/azure/sql/db#az-sql-db-update)of de [rest API ](https://docs.microsoft.com/rest/api/sql/databases/update).

De volgende video toont het dynamisch wijzigen van de servicelaag en de berekenings grootte om beschik bare Dtu's voor één Data Base te verg Roten.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-dynamically-scale-up-or-scale-down/player]
>

> [!IMPORTANT]
> In sommige gevallen is het wellicht voor het verkleinen van een database voor het vrijmaken van ongebruikte ruimte. Zie voor meer informatie, [bestandsruimte in Azure SQL Database beheren](sql-database-file-space-management.md).

### <a name="impact-of-changing-service-tier-or-rescaling-compute-size"></a>Gevolgen van het wijzigen van de servicelaag of het opnieuw schalen van de reken grootte

Het wijzigen van de servicelaag of de reken grootte van een enkele data base omvat voornamelijk de service die de volgende stappen uitvoert:

1. Een nieuw reken exemplaar maken voor de data base  

    Er wordt een nieuw reken exemplaar voor de data base gemaakt met de aangevraagde servicelaag en de berekenings grootte. Voor sommige combi Naties van de service tier en de grootte van de berekening moet er een replica van de Data Base worden gemaakt in het nieuwe reken exemplaar, waarbij het kopiëren van gegevens is vereist en de algehele latentie kan beïnvloeden. De data base blijft tijdens deze stap online en er worden verbindingen met de data base in het oorspronkelijke Compute-exemplaar in rekening gebracht.

2. Route ring van verbindingen naar een nieuw reken exemplaar overschakelen

    Bestaande verbindingen met de data base in het oorspronkelijke Compute-exemplaar worden verwijderd. Nieuwe verbindingen worden tot stand gebracht in de data base in het nieuwe reken exemplaar. Voor sommige combi Naties van service tier-en Compute-wijzigingen worden database bestanden losgekoppeld en opnieuw gekoppeld tijdens de switch.  De switch kan echter een korte onderbreking van de service tot gevolg hebben wanneer de data base minder dan 30 seconden lang niet beschikbaar is, en vaak slechts enkele seconden. Als er langlopende trans acties worden uitgevoerd wanneer de verbindingen worden verbroken, kan de duur van deze stap langer duren om afgebroken trans acties te herstellen. [Versneld database herstel](sql-database-accelerated-database-recovery.md) kan het effect verminderen van het afbreken van langlopende trans acties.

> [!IMPORTANT]
> Er gaan geen gegevens verloren tijdens elke stap in de werk stroom.

### <a name="latency-of-changing-service-tier-or-rescaling-compute-size"></a>Latentie van het wijzigen van de servicelaag of het opnieuw schalen van de reken grootte

De geschatte latentie voor het wijzigen van de servicelaag of het opnieuw schalen van de reken grootte van één data base of elastische pool is als volgt:

|Servicelaag|Basis, afzonderlijke Data Base,</br>Standaard (S0-S1)|Algemene elastische pool,</br>Standard (S2-S12), </br>Grootschalige </br>Algemeen afzonderlijke data base of elastische pool|Premium of Bedrijfskritiek afzonderlijke data base of elastische pool|
|:---|:---|:---|:---|
|**Basis enkele data base</br> , standaard (S0-S1)**|&bull;&nbsp;Constante tijd latentie, onafhankelijk van gebruikte ruimte</br>&bull;&nbsp;Normaal gesp roken minder dan 5 minuten|&bull;&nbsp;Latentie in verhouding tot de database ruimte die wordt gebruikt door het kopiëren van gegevens</br>&bull;&nbsp;Normaal gesp roken, minder dan 1 minuut per GB gebruikte ruimte|&bull;&nbsp;Latentie in verhouding tot de database ruimte die wordt gebruikt door het kopiëren van gegevens</br>&bull;&nbsp;Normaal gesp roken, minder dan 1 minuut per GB gebruikte ruimte|
|**Basic elastische pool </br>, standaard (S2-S12) </br>, grootschalige </br>, algemeen afzonderlijke data base of elastische pool**|&bull;&nbsp;Latentie in verhouding tot de database ruimte die wordt gebruikt door het kopiëren van gegevens</br>&bull;&nbsp;Normaal gesp roken, minder dan 1 minuut per GB gebruikte ruimte|&bull;&nbsp;Constante tijd latentie, onafhankelijk van gebruikte ruimte</br>&bull;&nbsp;Normaal gesp roken minder dan 5 minuten|&bull;&nbsp;Latentie in verhouding tot de database ruimte die wordt gebruikt door het kopiëren van gegevens</br>&bull;&nbsp;Normaal gesp roken, minder dan 1 minuut per GB gebruikte ruimte|
|**Premium of Bedrijfskritiek afzonderlijke data base of elastische pool**|&bull;&nbsp;Latentie in verhouding tot de database ruimte die wordt gebruikt door het kopiëren van gegevens</br>&bull;&nbsp;Normaal gesp roken, minder dan 1 minuut per GB gebruikte ruimte|&bull;&nbsp;Latentie in verhouding tot de database ruimte die wordt gebruikt door het kopiëren van gegevens</br>&bull;&nbsp;Normaal gesp roken, minder dan 1 minuut per GB gebruikte ruimte|&bull;&nbsp;Latentie in verhouding tot de database ruimte die wordt gebruikt door het kopiëren van gegevens</br>&bull;&nbsp;Normaal gesp roken, minder dan 1 minuut per GB gebruikte ruimte|

> [!TIP]
> Zie voor het bewaken van bewerkingen in uitvoering: [Beheer bewerkingen met behulp van de SQL rest API](https://docs.microsoft.com/rest/api/sql/operations/list), [beheer bewerkingen met CLI](/cli/azure/sql/db/op), Bewaak [bewerkingen met behulp van T-SQL](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) en deze twee Power shell-opdrachten: [Get-AzSqlDatabaseActivity](/powershell/module/az.sql/get-azsqldatabaseactivity) en [Stop-AzSqlDatabaseActivity](/powershell/module/az.sql/stop-azsqldatabaseactivity).

### <a name="cancelling-service-tier-changes-or-compute-rescaling-operations"></a>Wijzigingen in de servicelaag of het opnieuw schalen van berekeningen annuleren

Een wijzigings-of COMPUTE-bewerking voor het opnieuw schalen van een service tier kan worden geannuleerd.

#### <a name="azure-portal"></a>Azure Portal

Ga op de Blade overzicht van Data Base naar **meldingen** en klik op de tegel die aangeeft dat er een actieve bewerking is:

![Doorlopende bewerking](media/sql-database-single-database-scale/ongoing-operations.png)

Klik vervolgens op de knop met het label **annulering van deze bewerking**.

![Doorlopende bewerking annuleren](media/sql-database-single-database-scale/cancel-ongoing-operation.png)

#### <a name="powershell"></a>PowerShell

Stel vanuit een Power shell-opdracht prompt `$ResourceGroupName`de `$ServerName`, en `$DatabaseName`in en voer vervolgens de volgende opdracht uit:

```PowerShell
$OperationName = (az sql db op list --resource-group $ResourceGroupName --server $ServerName --database $DatabaseName --query "[?state=='InProgress'].name" --out tsv)
if(-not [string]::IsNullOrEmpty($OperationName))
    {
        (az sql db op cancel --resource-group $ResourceGroupName --server $ServerName --database $DatabaseName --name $OperationName)
        "Operation " + $OperationName + " has been canceled"
    }
    else
    {
        "No service tier change or compute rescaling operation found"
    }
```

### <a name="additional-considerations-when-changing-service-tier-or-rescaling-compute-size"></a>Aanvullende overwegingen bij het wijzigen van de servicelaag of het opnieuw schalen van de reken grootte

- Als u een upgrade uitvoert naar een hogere servicelaag of reken grootte, wordt de maximale grootte van de data base niet verhoogd, tenzij u expliciet een grotere grootte (maxSize) opgeeft.
- Om een Data Base te downgradeen, moet de gebruikte data base kleiner zijn dan de Maxi maal toegestane grootte van de doellaag en de grootte van de berekening.
- Bij downgrade van **Premium** naar de laag **standaard** geldt een extra opslag kosten als beide (1) de maximale grootte van de Data Base worden ondersteund in de doel berekenings grootte, en (2) de maximale grootte de inbegrepen opslag hoeveelheid van de doel berekenings grootte overschrijdt. Als een P1-data base met een maximale grootte van 500 GB bijvoorbeeld verkleind is aan S3, geldt er een extra opslag kosten, aangezien S3 een maximum grootte van 1 TB ondersteunt en de inbegrepen opslag hoeveelheid alleen 250 GB is. De extra opslag ruimte is dus 500 GB – 250 GB = 250 GB. Zie voor prijzen voor extra opslag [SQL database prijzen](https://azure.microsoft.com/pricing/details/sql-database/). Als de daad werkelijke hoeveelheid gebruikte ruimte kleiner is dan het totale aantal opslag, kunnen deze extra kosten worden vermeden door de maximale grootte van de data base te verlagen tot het opgenomen bedrag.
- Bij het bijwerken van een Data Base waarvoor [geo-replicatie](sql-database-geo-replication-portal.md) is ingeschakeld, werkt u de secundaire data bases bij naar de gewenste servicelaag en de berekenings grootte vóór de upgrade van de primaire data base (algemene richt lijnen voor de beste prestaties). Wanneer u een upgrade naar een andere uitvoert, moet u eerst een upgrade uitvoeren van de secundaire data base.
- Bij het downgradeen van een Data Base met [geo-replicatie](sql-database-geo-replication-portal.md) is ingeschakeld, downgradet u de primaire data bases naar de gewenste servicelaag en reken grootte voordat u de secundaire data base downgradet (algemene richt lijnen voor de beste prestaties). Wanneer u een downgrade naar een andere editie uitvoert, is de primaire data base het eerst te downgradeen vereist.
- De mogelijkheden om de service te herstellen verschillen voor de verschillende servicelagen. Als u downgradet naar de laag **basis** , is er een lagere Bewaar periode voor back-ups. Zie [Azure SQL database back-ups](sql-database-automated-backups.md).
- De nieuwe eigenschappen voor de database worden pas toegepast nadat de wijzigingen zijn voltooid.

### <a name="billing-during-compute-rescaling"></a>Facturering tijdens het berekenen van de schaal

Er worden kosten in rekening gebracht voor elk uur dat een data base bestaat met de hoogste service laag + reken grootte die tijdens dat uur is toegepast, ongeacht het gebruik of het feit dat de data base minder dan een uur actief was. Als u bijvoorbeeld een enkele data base maakt en deze vijf minuten later verwijdert, wordt de factuur voor één data base uur weer gegeven.

## <a name="change-storage-size"></a>Opslag grootte wijzigen

### <a name="vcore-based-purchasing-model"></a>Aankoopmodel op basis van vCore

- Opslag kan worden ingericht tot de maximale grootte, met een veelvoud van 1 GB. De mini maal Configureer bare gegevens opslag is 5 GB
- Opslag voor één data base kan worden ingericht door de maximale grootte te verhogen of te verlagen met behulp van de [Azure Portal](https://portal.azure.com), [Transact-SQL](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current#examples-1), [Power shell](/powershell/module/az.sql/set-azsqldatabase), de [Azure cli](/cli/azure/sql/db#az-sql-db-update)of de [rest API](https://docs.microsoft.com/rest/api/sql/databases/update).
- SQL Database wijst automatisch 30% extra opslag ruimte toe voor de logboek bestanden en 32 GB per vCore voor TempDB, maar niet meer dan 384GB. TempDB bevindt zich op een gekoppelde SSD in alle service lagen.
- De prijs van opslag voor één data base is de som van de hoeveelheid gegevens opslag en de opslag van Logboeken, vermenigvuldigd met de prijs van de opslag eenheid van de servicelaag. De kosten voor TempDB zijn opgenomen in de prijs van de vCore. Zie [SQL database prijzen](https://azure.microsoft.com/pricing/details/sql-database/)voor meer informatie over de prijs van extra opslag.

> [!IMPORTANT]
> In sommige gevallen is het wellicht voor het verkleinen van een database voor het vrijmaken van ongebruikte ruimte. Zie voor meer informatie, [bestandsruimte in Azure SQL Database beheren](sql-database-file-space-management.md).

### <a name="dtu-based-purchasing-model"></a>Op DTU gebaseerd inkoop model

- De DTU-prijs voor één Data Base bevat een bepaalde hoeveelheid opslag zonder extra kosten. Extra opslag buiten de inbegrepen hoeveelheid kan worden ingericht voor extra kosten tot de maximale grootte in stappen van 250 GB tot 1 TB en vervolgens in stappen van 256 GB tot meer dan 1 TB. Zie [Single Data Base voor inbegrepen opslag bedragen en maximale grootte limieten: Opslag grootten en berekenings grootten](sql-database-dtu-resource-limits-single-databases.md#single-database-storage-sizes-and-compute-sizes).
- Extra opslag ruimte voor één data base kan worden ingericht door de maximale grootte te verhogen met behulp van de Azure Portal, [Transact-SQL](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current#examples-1), [Power shell](/powershell/module/az.sql/set-azsqldatabase), de [Azure cli](/cli/azure/sql/db#az-sql-db-update)of de [rest API](https://docs.microsoft.com/rest/api/sql/databases/update).
- De prijs van extra opslag voor één data base is de extra opslag hoeveelheid vermenigvuldigd met de extra eenheids prijs voor opslag van de servicelaag. Zie [SQL database prijzen](https://azure.microsoft.com/pricing/details/sql-database/)voor meer informatie over de prijs van extra opslag.

> [!IMPORTANT]
> In sommige gevallen is het wellicht voor het verkleinen van een database voor het vrijmaken van ongebruikte ruimte. Zie voor meer informatie, [bestandsruimte in Azure SQL Database beheren](sql-database-file-space-management.md).

## <a name="p11-and-p15-constraints-when-max-size-greater-than-1-tb"></a>P11-en P15-beperkingen bij een maximale grootte van meer dan 1 TB

Voor de Premium-laag is er meer dan 1 TB aan opslagruimte beschikbaar in alle regio's, met uitzondering van: China - oost, China - noord, Duitsland - centraal, Duitsland - noordoost, US - west-centraal, US - DoD-regio's en US Government - centraal. In deze regio’s is de maximale opslagruimte in de Premium-laag beperkt tot 1 TB. De volgende overwegingen en beperkingen zijn van toepassing op P11-en P15-data bases met een maximale grootte van meer dan 1 TB:

- Als de maximale grootte voor een P11-of P15-data base ooit is ingesteld op een waarde die groter is dan 1 TB, dan kan deze alleen worden teruggezet of gekopieerd naar een P11-of P15-data base.  Vervolgens kan de data base opnieuw worden geschaald naar een andere reken grootte, op voor waarde dat de hoeveelheid toegewezen ruimte op het moment van de bewerking voor opnieuw schalen niet groter is dan de maximale grootte limieten van de nieuwe reken grootte.
- Voor scenario's met actieve geo-replicatie:
  - Een geo-replicatie relatie instellen: Als de primaire data base P11 of P15 is, moeten de secundaire (s) ook P11 of P15 zijn. de lagere Compute-grootte wordt afgewezen als secundaire, omdat deze niet meer dan 1 TB kunnen ondersteunen.
  - De primaire data base bijwerken in een geo-replicatie relatie: Als de maximale grootte wordt gewijzigd in meer dan 1 TB voor een primaire data base, wordt dezelfde wijziging geactiveerd voor de secundaire data base. Beide upgrades moeten worden uitgevoerd om de wijziging van kracht te laten worden. Er gelden beperkingen voor regio's voor de optie meer dan 1 TB. Als het secundaire deel uitmaakt van een regio die niet meer dan 1 TB ondersteunt, wordt de primaire niet geüpgraded.
- Het is niet mogelijk om de import/export-service te gebruiken voor het laden van P11/P15-data bases met meer dan 1 TB. Gebruik SqlPackage. exe om gegevens te [importeren](sql-database-import.md) en te [exporteren](sql-database-export.md) .

## <a name="next-steps"></a>Volgende stappen

Zie [SQL database resource limieten op basis van vCore-afzonderlijke data bases](sql-database-vcore-resource-limits-single-databases.md) en [SQL database DTU-gebaseerde resource limieten-elastische Pools](sql-database-dtu-resource-limits-single-databases.md)voor algemene resource limieten.
