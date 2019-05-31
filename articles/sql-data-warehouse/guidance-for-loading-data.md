---
title: Aanbevolen procedures voor het laden van gegevens - Azure SQL Data Warehouse | Microsoft Docs
description: Aanbevelingen en prestatieoptimalisatie voor het laden van gegevens in Azure SQL Data Warehouse.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: load data
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.custom: seoapril2019
ms.openlocfilehash: f1bfd6a9f7b5d6b7622b3fc79848b986172ff746
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66240846"
---
# <a name="best-practices-for-loading-data-into-azure-sql-data-warehouse"></a>Aanbevolen procedures voor het laden van gegevens in Azure SQL Data Warehouse

Aanbevelingen en prestatieoptimalisatie voor het laden van gegevens in Azure SQL Data Warehouse.

## <a name="preparing-data-in-azure-storage"></a>Gegevens voorbereiden in Azure Storage

Bepaal uw opslaglaag en uw datawarehouse om de latentie te minimaliseren.

Bij het exporteren van gegevens in een ORC-bestandsindeling kunnen er Java-geheugenfouten optreden wanneer er grote tekstkolommen zijn. U kunt deze beperking omzeilen door slechts een subset van de kolommen te exporteren.

PolyBase kan rijen met meer dan 1.000.000 bytes aan gegevens niet laden. Wanneer u gegevens in de tekstbestanden in Azure-blob-opslag of Azure Data Lake Store zet, moeten deze minder dan 1.000.000 bytes aan gegevens bevatten. Deze bytebeperking geldt ongeacht het tabelschema.

Alle bestandsindelingen hebben verschillende prestatiekenmerken. Gebruik voor het snelste laadproces gecomprimeerde tekstbestanden. Het verschil tussen UTF-8- en UTF-16-prestaties is minimaal.

Splits grote gecomprimeerde bestanden in kleinere gecomprimeerde bestanden.

## <a name="running-loads-with-enough-compute"></a>Laadtaken uitvoeren met voldoende rekenkracht

Voer voor de hoogste laadsnelheid slechts één taak tegelijk uit. Voer een zo klein mogelijk aantal laadtaken tegelijk uit als dit niet haalbaar is. Als u een grote laadtaak verwacht, kunt u uw datawarehouse opschalen vóór de laadtaak.

Als u loads wilt uitvoeren met geschikte rekenresources, maakt u gebruikers voor het laadproces die zijn aangewezen voor het uitvoeren van loads. Wijs elke gebruiker voor het laadproces toe aan een specifieke resourceklasse. Om uit te voeren een belasting, zich aanmelden als een van de gebruikers voor het laadproces en voert u de belasting. De load wordt uitgevoerd met de resourceklasse van de gebruiker.  Deze methode is eenvoudiger dan de resourceklasse van een gebruiker aanpassen om te voldoen aan de huidige benodigde resourceklasse.

### <a name="example-of-creating-a-loading-user"></a>Voorbeeld van het maken van een gebruiker voor het laadproces

In dit voorbeeld wordt een gebruiker voor het laadproces gemaakt voor de resourceklasse staticrc20. De eerste stap is **verbinding maken met de master** en een aanmelding maken.

```sql
   -- Connect to master
   CREATE LOGIN LoaderRC20 WITH PASSWORD = 'a123STRONGpassword!';
```

Maak verbinding met het datawarehouse en maak een gebruiker. In de volgende code wordt ervan uitgegaan dat u verbonden bent met de database mySampleDataWarehouse. U ziet hoe u een gebruiker maakt met de naam LoaderRC20 en hoe u die gebruiker machtiging voor het beheer van een database geeft. Vervolgens wordt de gebruiker toegevoegd als lid van de databaserol staticrc20.  

```sql
   -- Connect to the database
   CREATE USER LoaderRC20 FOR LOGIN LoaderRC20;
   GRANT CONTROL ON DATABASE::[mySampleDataWarehouse] to LoaderRC20;
   EXEC sp_addrolemember 'staticrc20', 'LoaderRC20';
```

Om door te voeren met resources voor de Statirc20-resourceklassen, meld u aan als LoaderRC20 en voer de belasting.

Voer loads bij voorkeur uit onder statische en niet onder dynamische resourceklassen. Met behulp van de statische resourceklassen worden dezelfde resources ongeacht gegarandeerd uw [datawarehouse-eenheden](what-is-a-data-warehouse-unit-dwu-cdwu.md). Als u een dynamische resourceklasse gebruikt, variëren de resources afhankelijk van uw serviceniveau. Voor dynamische klassen betekent een lager serviceniveau dat u waarschijnlijk een grotere resourceklasse moet gebruiken voor uw gebruiker van het laadproces.

## <a name="allowing-multiple-users-to-load"></a>Meerdere gebruikers toestaan te laden

Vaak is het nodig dat meerdere gebruikers gegevens kunnen laden in een datawarehouse. Laden met de [CREATE TABLE AS SELECT (Transact-SQL)](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) vereist machtigingen voor beheer van de database.  De CONTROL-machtiging biedt beheertoegang tot alle schema's. Mogelijk wilt u niet alle gebruikers die laadtaken uitvoeren, beheertoegang tot alle schema's verlenen. Als u machtigingen wilt beperken, kunt u de instructie DENY CONTROL gebruiken.

Denk bijvoorbeeld aan databaseschema's, schema_A voor afdeling A, en schema_B voor afdeling B. Laat databasegebruikers gebruiker_A en gebruiker_B gebruikers zijn voor PolyBase die respectievelijk laden in afdeling A en B. Beide zijn voorzien van databasemachtigingen voor CONTROL. De makers van schema A en B vergrendelen nu hun schema's met DENY:

```sql
   DENY CONTROL ON SCHEMA :: schema_A TO user_B;
   DENY CONTROL ON SCHEMA :: schema_B TO user_A;
```

Gebruiker_A en gebruiker_B worden nu geblokkeerd door het schema van de andere afdeling.

## <a name="loading-to-a-staging-table"></a>Laden naar een faseringstabel

De hoogste laadsnelheid voor het verplaatsen van gegevens naar een datawarehousetabel kunt u verkrijgen door gegevens te laden naar een tijdelijke tabel.  Definieer de faseringstabel als een heap en gebruik round-robin voor de distributieoptie. 

Bedenk dat laden meestal een proces met twee stappen is waarin u eerst naar een tijdelijke tabel laadt en de gegevens vervolgens in een productiedatawarehousetabel invoegt. Als de productietabel een hash-distributiepunt gebruikt, is de totale tijd voor het laden en invoegen mogelijk sneller als u de faseringstabel met de hash-distributie definieert. Het laden naar de faseringstabel duurt langer, maar de tweede stap van het invoegen van de rijen in de productietabel leidt niet tot de verplaatsing van gegevens in de distributies.

## <a name="loading-to-a-columnstore-index"></a>Laden naar een columnstore-index

Columnstore-indexen vereisen grote hoeveelheden geheugen voor het comprimeren van gegevens tot hoogwaardige rijgroepen. Voor de beste compressie en efficiëntie van de index moet de columnstore-index maximaal 1.048.576 rijen in elke rijgroep comprimeren. Bij geheugenbelasting kan het zijn dat de columnstore-index de maximale compressiesnelheden niet kan halen. Dit kan van invloed op de queryresultaten. Zie voor gedetailleerde informatie [Columnstore geheugenoptimalisaties](sql-data-warehouse-memory-optimizations-for-columnstore-compression.md).

- Zorg dat de gebruiker van het laadproces voldoende geheugen heeft om maximale compressiesnelheden te bereiken. Gebruik hiervoor gebruikers voor het laadproces die lid zijn van een middelgrote of grote resourceklasse. 
- Laad genoeg rijen om nieuwe rijgroepen volledig te vullen. Tijdens een bulksgewijze laadtaak worden elke 1.048.576 rijen rechtstreeks in de columnstore gecomprimeerd als een volledige rijgroep. Laadtaken met minder dan 102.400 rijen verzenden de rijen naar de deltastore waarin rijen zijn ondergebracht in een b-tree-index. Als u te weinig rijen laadt, gaan deze mogelijk allemaal naar de deltastore en worden ze niet direct naar columnstore-indeling gecomprimeerd.

## <a name="handling-loading-failures"></a>Afhandeling van fouten bij het laden

Een load met behulp van een externe tabel kan mislukken met de fout *Query afgebroken--de maximale weigeringsdrempelwaarde is bereikt tijdens het lezen vanuit een externe bron*. Dit bericht geeft aan dat uw externe gegevens vervuilde records bevatten. Een gegevensrecord wordt als 'vervuild' beschouwd als de gegevenstypen en het aantal kolommen niet overeenkomen met de kolomdefinities van de externe tabel of als de gegevens niet overeenkomen met de externe bestandsindeling. 

U kunt vervuilde records voorkomen door ervoor te zorgen dat uw externe tabel- en bestandindelingsdefinities correct zijn en uw externe gegevens overeenstemmen met deze definities. Als een subset van externe gegevensrecords ongeldig is, kunt u ervoor kiezen deze records voor uw query's te weigeren door gebruik te maken van de weigeringsopties in CREATE EXTERNAL TABLE.

## <a name="inserting-data-into-a-production-table"></a>Gegevens in een productietabel invoegen

Een eenmalige laadtaak naar een kleine tabel met een [INSERT-instructie](/sql/t-sql/statements/insert-transact-sql) of zelfs een periodieke herlaadtaak kan een acceptabel resultaat geven met een instructie zoals `INSERT INTO MyLookup VALUES (1, 'Type 1')`.  Het invoegen van singletons is echter niet zo efficiënt als bulksgewijs laden. 

Als u de hele dag door duizenden of meerdere enkele gegevens wilt invoeren, voeg de gegevens dan samen tot een batch zodat deze bulksgewijs kunt laden.  Ontwikkel uw processen om de afzonderlijke gegevens aan een bestand toe te voegen en maak vervolgens een ander proces dat het bestand periodiek laadt.

## <a name="creating-statistics-after-the-load"></a>Statistieken maken na het laden

Voor optimale resultaten van uw query's is het belangrijk dat u statistieken maakt voor alle kolommen van alle tabellen nadat de gegevens voor het eerst zijn geladen of wanneer de gegevens substantieel zijn gewijzigd.  Zie [statistieken](sql-data-warehouse-tables-statistics.md) voor gedetailleerde uitleg van statistieken. In het volgende voorbeeld worden statistieken in vijf kolommen van de tabel Customer_Speed aangemaakt.

```sql
create statistics [SensorKey] on [Customer_Speed] ([SensorKey]);
create statistics [CustomerKey] on [Customer_Speed] ([CustomerKey]);
create statistics [GeographyKey] on [Customer_Speed] ([GeographyKey]);
create statistics [Speed] on [Customer_Speed] ([Speed]);
create statistics [YearMeasured] on [Customer_Speed] ([YearMeasured]);
```

## <a name="rotate-storage-keys"></a>Opslagsleutels draaien

Het is verstandig uit veiligheidsoverwegingen de toegangssleutel in de blob-opslag regelmatig te wijzigen. Er zijn twee opslagsleutels voor uw blob-opslagaccount, waarmee u de sleutels kunt wijzigen.

Sleutels van het Microsoft Azure Storage-account draaien:

Voor elk opslagaccount waarvan de sleutel is gewijzigd, moet u [ALTER DATABASE SCOPED CREDENTIAL](/sql/t-sql/statements/alter-database-scoped-credential-transact-sql) uitvoeren.

Voorbeeld:

De oorspronkelijke sleutel wordt gemaakt

```sql
CREATE DATABASE SCOPED CREDENTIAL my_credential WITH IDENTITY = 'my_identity', SECRET = 'key1'
``` 

Rotate key from key 1 to key 2

```sql
ALTER DATABASE SCOPED CREDENTIAL my_credential WITH IDENTITY = 'my_identity', SECRET = 'key2' 
```

Er hoeven geen andere wijzigingen te worden aangebracht aan onderliggende externe gegevensbronnen.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over PolyBase en het ontwerpen van een Extract, Load en Transform (ELT)-proces [ELT ontwerpen voor SQL Data Warehouse](design-elt-data-loading.md).
- Gebruik voor het laden van een zelfstudie [PolyBase om gegevens te laden uit Azure blob-opslag naar Azure SQL Data Warehouse](load-data-from-azure-blob-storage-using-polybase.md).
- Zie [Uw workload controleren met DMV's](sql-data-warehouse-manage-monitor.md) voor het controleren van het laden van gegevens.



