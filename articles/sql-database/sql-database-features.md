---
title: Vergelijking van Azure SQL Database-functies | Microsoft Docs
description: In dit artikel worden de functies vergeleken van SQL Server die beschikbaar zijn in verschillende soorten Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: bonova, sstein
manager: craigg
ms.date: 05/10/2019
ms.openlocfilehash: 5bdbd9bebfb819ae18de884a014c574e12c53ebf
ms.sourcegitcommit: 83a89c45253b0d432ce8dcd70084c18e9930b1fd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/22/2019
ms.locfileid: "68371704"
---
# <a name="feature-comparison-azure-sql-database-versus-sql-server"></a>Vergelijking van functies: Azure SQL Database versus SQL Server

Azure SQL Database deelt een algemene code basis met SQL Server. De functies van SQL Server die door Azure SQL Database worden ondersteund, zijn afhankelijk van het type Azure-SQL database dat u maakt. Met Azure SQL Database kunt u een Data Base maken als onderdeel van een [beheerd exemplaar](sql-database-managed-instance.md), als één data base of als onderdeel van een elastische pool.

Micro soft blijft functies toevoegen aan Azure SQL Database. Ga naar de webpagina service-updates voor Azure voor de nieuwste updates met behulp van de volgende filters:

- Gefilterd op de [SQL Database-service](https://azure.microsoft.com/updates/?service=sql-database).
- Gefilterd op [aankondigingen](https://azure.microsoft.com/updates/?service=sql-database&update-type=general-availability) over de algemene beschikbaarheid (GA) van SQL Database-functies.

Als u meer informatie over de verschillen nodig hebt, kunt u deze vinden op de afzonderlijke pagina's voor [één data base en elastische Pools](sql-database-transact-sql-information.md) of een [beheerd exemplaar](sql-database-managed-instance-transact-sql-information.md).

## <a name="sql-features"></a>SQL-functies

De volgende tabel bevat de belangrijkste functies van SQL Server en geeft informatie over of de functie gedeeltelijk of volledig wordt ondersteund in een beheerd exemplaar of Individuele database en elastische Pools, met een koppeling naar meer informatie over de functie.

| **SQL-functie** | **Afzonderlijke data bases en elastische Pools** | **Beheerde instanties** |
| --- | --- | --- |
| [Altijd versleuteld](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine) | Ja, Zie [certificaat archief](sql-database-always-encrypted.md) en [sleutel kluis](sql-database-always-encrypted-azure-key-vault.md) | Ja, Zie [certificaat archief](sql-database-always-encrypted.md) en [sleutel kluis](sql-database-always-encrypted-azure-key-vault.md) |
| [AlwaysOn-beschikbaarheids groepen](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server) | [Hoge Beschik baarheid](sql-database-high-availability.md) is opgenomen in elke Data Base. Herstel na nood gevallen wordt beschreven in [overzicht van bedrijfs continuïteit met Azure SQL database](sql-database-business-continuity.md) | [Hoge Beschik baarheid](sql-database-high-availability.md) is opgenomen in elke Data Base en [kan niet worden beheerd door de gebruiker](sql-database-managed-instance-transact-sql-information.md#always-on-availability). Herstel na nood gevallen wordt beschreven in [overzicht van bedrijfs continuïteit met Azure SQL database](sql-database-business-continuity.md) |
| [Een Data Base koppelen](https://docs.microsoft.com/sql/relational-databases/databases/attach-a-database) | Nee | Nee |
| [Toepassingsrollen](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/application-roles) | Ja | Ja |
| [Controle](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine) | [Ja](sql-database-auditing.md)| [Ja](sql-database-managed-instance-auditing.md), met enkele [verschillen](sql-database-managed-instance-transact-sql-information.md#auditing) |
| [Automatische back-ups](sql-database-automated-backups.md) | Ja. Volledige back-ups worden elke zeven dagen, differentieel 12 uur en logboek back-ups om de 5-10 minuten uitgevoerd. | Ja. Volledige back-ups worden elke zeven dagen, differentieel 12 uur en logboek back-ups om de 5-10 minuten uitgevoerd. |
| [Automatisch afstemmen (afdwingen van plan)](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning)| [Ja](sql-database-automatic-tuning.md)| [Ja](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning) |
| [Automatisch afstemmen (indexen)](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning)| [Ja](sql-database-automatic-tuning.md)| Nee |
| [Opdracht BACKUP](https://docs.microsoft.com/sql/t-sql/statements/backup-transact-sql) | Nee, alleen door het systeem geïnitieerde automatische back-ups: Zie [automatische back-ups](sql-database-automated-backups.md) | Ja, door de gebruiker gestarte back-ups naar Azure Blob Storage (automatische systeem back-ups kunnen niet worden gestart door de gebruiker). Zie [back-upverschillen](sql-database-managed-instance-transact-sql-information.md#backup) |
| [Ingebouwde functies](https://docs.microsoft.com/sql/t-sql/functions/functions) | De meeste-Zie afzonderlijke functies | Ja, Zie [opgeslagen procedures, functies, triggers-verschillen](sql-database-managed-instance-transact-sql-information.md#stored-procedures-functions-and-triggers) | 
| [BULK INSERT-instructie](https://docs.microsoft.com/sql/relational-databases/import-export/import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server) | Ja, maar alleen van Azure Blob-opslag als een bron. | Ja, maar alleen van Azure Blob Storage als bron: Zie [verschillen](sql-database-managed-instance-transact-sql-information.md#bulk-insert--openrowset). |
| [Certificaten en asymmetrische sleutels](https://docs.microsoft.com/sql/relational-databases/security/sql-server-certificates-and-asymmetric-keys) | Ja, zonder toegang tot bestands systeem voor `BACKUP` en `CREATE` bewerkingen. | Ja, zonder toegang tot bestands systeem voor `BACKUP` en `CREATE` bewerkingen-Zie [certificaat verschillen](sql-database-managed-instance-transact-sql-information.md#certificates). | 
| [Change Data Capture-CDC](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-data-capture-sql-server) | Nee | Ja |
| [Wijzigingen bijhouden](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-tracking-sql-server) | Ja |Ja |
| [Sortering-data base](https://docs.microsoft.com/sql/relational-databases/collations/set-or-change-the-database-collation) | Ja | Ja |
| [Sortering-Server/exemplaar](https://docs.microsoft.com/sql/relational-databases/collations/set-or-change-the-server-collation) | Nee, standaard sortering `SQL_Latin1_General_CP1_CI_AS` van logische servers wordt altijd gebruikt. | Ja, kan worden ingesteld wanneer het [exemplaar wordt gemaakt](scripts/sql-managed-instance-create-powershell-azure-resource-manager-template.md) en kan later niet worden bijgewerkt. |
| [Column Store-indexen](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-overview) | Yes- [Premium-laag, Standard-laag-S3 en hoger, algemeen laag en bedrijfskritiek lagen](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-overview) |Ja |
| [Common language runtime-CLR](https://docs.microsoft.com/sql/relational-databases/clr-integration/common-language-runtime-clr-integration-programming-concepts) | Nee | Ja, maar zonder toegang tot bestands systeem in `CREATE ASSEMBLY` instructie-Zie [CLR-verschillen](sql-database-managed-instance-transact-sql-information.md#clr) |
| [Inge sloten data bases](https://docs.microsoft.com/sql/relational-databases/databases/contained-databases) | Ja | Momenteel niet [als gevolg van een defect in Restore, inclusief herstel naar](sql-database-managed-instance-transact-sql-information.md#cant-restore-contained-database)een bepaald tijdstip. Dit is een defect dat binnenkort wordt opgelost. |
| [Opgenomen gebruikers](https://docs.microsoft.com/sql/relational-databases/security/contained-database-users-making-your-database-portable) | Ja | Ja |
| [Controle van tref woorden voor de stroom taal](https://docs.microsoft.com/sql/t-sql/language-elements/control-of-flow) | Ja | Ja |
| [Referenties](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/credentials-database-engine) | Ja, maar alleen [Data Base-bereik referenties](https://docs.microsoft.com/sql/t-sql/statements/create-database-scoped-credential-transact-sql). | Ja, maar alleen **Azure Key Vault** en `SHARED ACCESS SIGNATURE` worden ondersteund Zie [Details](sql-database-managed-instance-transact-sql-information.md#credential) |
| [Query's met namen van meerdere data bases/drie delen](https://docs.microsoft.com/sql/relational-databases/linked-servers/linked-servers-database-engine) | Nee-Zie [elastische query's](sql-database-elastic-query-overview.md) | Ja, plus [elastische query's](sql-database-elastic-query-overview.md) |
| [Transacties tussen databases](https://docs.microsoft.com/sql/relational-databases/linked-servers/linked-servers-database-engine) | Nee | Ja, binnen het exemplaar. Zie [verschillen tussen gekoppelde servers](sql-database-managed-instance-transact-sql-information.md#linked-servers) voor query's die betrekking hebben op meerdere instanties. |
| [Cursors](https://docs.microsoft.com/sql/t-sql/language-elements/cursors-transact-sql) | Ja |Ja |
| [Gegevens compressie](https://docs.microsoft.com/sql/relational-databases/data-compression/data-compression) | Ja |Ja |
| [Data base-DbMail](https://docs.microsoft.com/sql/relational-databases/database-mail/database-mail) | Nee | Ja |
| [Database spiegeling](https://docs.microsoft.com/sql/database-engine/database-mirroring/database-mirroring-sql-server) | Nee | [Nee](sql-database-managed-instance-transact-sql-information.md#database-mirroring) |
| [Configuratie-instellingen van de data base](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) | Ja | Ja |
| [Database momentopnamen](https://docs.microsoft.com/sql/relational-databases/databases/database-snapshots-sql-server) | Nee | Nee |
| [Gegevenstypen](https://docs.microsoft.com/sql/t-sql/data-types/data-types-transact-sql) | Ja |Ja |
| [DBCC-instructies](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-transact-sql) | De meeste-Zie afzonderlijke instructies | Ja, Zie [DBCC-verschillen](sql-database-managed-instance-transact-sql-information.md#dbcc) |
| [DDL-instructies](https://docs.microsoft.com/sql/t-sql/statements/statements) | De meeste-Zie afzonderlijke instructies | Ja, Zie [T-SQL-verschillen](sql-database-managed-instance-transact-sql-information.md) |
| [DDL-triggers](https://docs.microsoft.com/sql/relational-databases/triggers/ddl-triggers) | Alleen data base |  Ja |
| [Gedistribueerde partitie weergaven](https://docs.microsoft.com/sql/t-sql/statements/create-view-transact-sql#partitioned-views) | Nee | Ja |
| [Gedistribueerde trans acties-MS DTC](https://docs.microsoft.com/sql/relational-databases/native-client-ole-db-transactions/supporting-distributed-transactions) | Nee-Zie [elastische trans acties](sql-database-elastic-transactions-overview.md) |  Geen-Zie [verschillen met gekoppelde servers](sql-database-managed-instance-transact-sql-information.md#linked-servers) |
| [DML-instructies](https://docs.microsoft.com/sql/t-sql/queries/queries) | Ja | Ja |
| [DML-triggers](https://docs.microsoft.com/sql/relational-databases/triggers/create-dml-triggers) | De meeste-Zie afzonderlijke instructies |  Ja |
| [DMV's](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/system-dynamic-management-views) | De meeste-Zie afzonderlijke Dmv's |  Ja, Zie [T-SQL-verschillen](sql-database-managed-instance-transact-sql-information.md) |
| [Dynamische gegevensmaskering](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking)|[Ja](sql-database-dynamic-data-masking-get-started.md)| [Ja](sql-database-dynamic-data-masking-get-started.md) |
| [Gebeurtenis meldingen](https://docs.microsoft.com/sql/relational-databases/service-broker/event-notifications) | Geen-Zie [waarschuwingen](sql-database-insights-alerts-portal.md) | Nee |
| [Expressies](https://docs.microsoft.com/sql/t-sql/language-elements/expressions-transact-sql) |Ja | Ja |
| [Uitgebreide gebeurtenissen (XEvent)](https://docs.microsoft.com/sql/relational-databases/extended-events/extended-events) | Sommige-Zie [uitgebreide gebeurtenissen in SQL database](sql-database-xevent-db-diff-from-svr.md) | Ja, zie de [verschillen in uitgebreide gebeurtenissen](sql-database-managed-instance-transact-sql-information.md#extended-events) |
| [Uitgebreide opgeslagen procedures](https://docs.microsoft.com/sql/relational-databases/extended-stored-procedures-programming/creating-extended-stored-procedures) | Nee | Nee |
| [Bestanden en bestands groepen](https://docs.microsoft.com/sql/relational-databases/databases/database-files-and-filegroups) | Alleen primaire bestands groep | Ja. Bestands paden worden automatisch toegewezen en de bestands locatie kan niet worden opgegeven `ALTER DATABASE ADD FILE` in de [instructie](sql-database-managed-instance-transact-sql-information.md#alter-database-statement).  |
| [Filestream](https://docs.microsoft.com/sql/relational-databases/blob/filestream-sql-server) | Nee | [Nee](sql-database-managed-instance-transact-sql-information.md#filestream-and-filetable) |
| [Zoek opdracht in volledige tekst (FT'S)](https://docs.microsoft.com/sql/relational-databases/search/full-text-search) |  Ja, maar woord afbreekers van derden worden niet ondersteund | Ja, maar [woord afbreekers van derden worden niet ondersteund](sql-database-managed-instance-transact-sql-information.md#full-text-semantic-search) |
| [Functies](https://docs.microsoft.com/sql/t-sql/functions/functions) | De meeste-Zie afzonderlijke functies | Ja, Zie [opgeslagen procedures, functies, triggers-verschillen](sql-database-managed-instance-transact-sql-information.md#stored-procedures-functions-and-triggers) |
| [Grafiek verwerking](https://docs.microsoft.com/sql/relational-databases/graphs/sql-graph-overview) | Ja | Ja |
| [Optimalisatie in het geheugen](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization) | Alleen de [lagen ja-Premium en bedrijfskritiek](sql-database-in-memory.md) | Ja, [alleen bedrijfskritiek laag](sql-database-managed-instance.md) |
| [Ondersteuning voor JSON-gegevens](https://docs.microsoft.com/sql/relational-databases/json/json-data-sql-server) | [Ja](sql-database-json-features.md) | [Ja](sql-database-json-features.md) |
| [Taal elementen](https://docs.microsoft.com/sql/t-sql/language-elements/language-elements-transact-sql) | De meeste-Zie afzonderlijke elementen |  Ja, Zie [T-SQL-verschillen](sql-database-managed-instance-transact-sql-information.md) |
| [Gekoppelde servers](https://docs.microsoft.com/sql/relational-databases/linked-servers/linked-servers-database-engine) | Nee, Zie [elastische query's](sql-database-elastic-query-horizontal-partitioning.md) | Ja. Alleen voor [SQL Server en SQL database](sql-database-managed-instance-transact-sql-information.md#linked-servers) zonder gedistribueerde trans acties. |
| [Logboek verzending](https://docs.microsoft.com/sql/database-engine/log-shipping/about-log-shipping-sql-server) | [Hoge Beschik baarheid](sql-database-high-availability.md) is opgenomen in elke Data Base. Herstel na nood gevallen wordt beschreven in [overzicht van bedrijfs continuïteit met Azure SQL database](sql-database-business-continuity.md) | Systeem eigen gemaakt als onderdeel van het DMS-migratie proces. Niet beschikbaar als oplossing met hoge Beschik baarheid, omdat andere methoden voor [hoge Beschik baarheid](sql-database-high-availability.md) zijn opgenomen in elke Data Base en het wordt afgeraden om gebruik te maken van log-shipping als ha-alternatief. Herstel na nood gevallen wordt beschreven in [overzicht van bedrijfs continuïteit met Azure SQL database](sql-database-business-continuity.md). Niet beschikbaar als replicatie mechanisme tussen data bases: Gebruik secundaire replica's op [bedrijfskritiek laag](sql-database-service-tier-business-critical.md), [groepen met automatische failover](sql-database-auto-failover-group.md)of transactionele [replicatie](sql-database-managed-instance-transactional-replication.md) als alternatief. |
| [Aanmeldingen en gebruikers](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/principals-database-engine) | Ja, maar `CREATE` en `ALTER` aanmeldings instructies bieden niet alle opties (geen Azure Active Directory-aanmeldingen op Windows en server niveau). `EXECUTE AS LOGIN`wordt niet ondersteund. Gebruik `EXECUTE AS USER` in plaats daarvan.  | Ja, met enkele [verschillen](sql-database-managed-instance-transact-sql-information.md#logins-and-users). Windows-aanmeldingen worden niet ondersteund en moeten worden vervangen door Azure Active Directory aanmeldingen. |
| [Minimale logboek registratie in bulk import](https://docs.microsoft.com/sql/relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import) | Nee | Nee |
| [Systeem gegevens wijzigen](https://docs.microsoft.com/sql/relational-databases/databases/system-databases) | Nee | Ja |
| [OLE-automatisering](https://docs.microsoft.com/sql/database-engine/configure-windows/ole-automation-procedures-server-configuration-option) | Nee | Nee |
| [Online-index bewerkingen](https://docs.microsoft.com/sql/relational-databases/indexes/perform-index-operations-online) | Ja | Ja |
| [OPENDATASOURCE](https://docs.microsoft.com/sql/t-sql/functions/opendatasource-transact-sql)|Nee|Ja, alleen voor andere Azure SQL-data bases en SQL-servers. Zie [T-SQL-verschillen](sql-database-managed-instance-transact-sql-information.md)|
| [OPENJSON](https://docs.microsoft.com/sql/t-sql/functions/openjson-transact-sql)|Ja|Ja|
| [OPEN QUERY](https://docs.microsoft.com/sql/t-sql/functions/openquery-transact-sql)|Nee|Ja, alleen voor andere Azure SQL-data bases en SQL-servers. Zie [T-SQL-verschillen](sql-database-managed-instance-transact-sql-information.md)|
| [OPENROWSET](https://docs.microsoft.com/sql/t-sql/functions/openrowset-transact-sql)|Ja, alleen importeren uit Azure Blob-opslag. |Ja, alleen voor andere Azure SQL-data bases en SQL-servers en voor het importeren vanuit Azure Blob-opslag. Zie [T-SQL-verschillen](sql-database-managed-instance-transact-sql-information.md)|
| [OPENXML](https://docs.microsoft.com/sql/t-sql/functions/openxml-transact-sql)|Ja|Ja|
| [Operators](https://docs.microsoft.com/sql/t-sql/language-elements/operators-transact-sql) | De meeste-Zie afzonderlijke Opera tors |Ja, Zie [T-SQL-verschillen](sql-database-managed-instance-transact-sql-information.md) |
| [Partitioneren](https://docs.microsoft.com/sql/relational-databases/partitions/partitioned-tables-and-indexes) | Ja | Ja |
| Openbaar IP-adres | Ja. De toegang kan worden beperkt met behulp van firewall-of service-eind punten.  | Ja. Moet expliciet zijn ingeschakeld en poort 3342 moet zijn ingeschakeld in NSG-regels. Open bare IP kan indien nodig worden uitgeschakeld. Zie [openbaar eind punt](sql-database-managed-instance-public-endpoint-securely.md) voor meer informatie. | 
| [Herstel naar een bepaald tijdstip data base](https://docs.microsoft.com/sql/relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model) | Ja, alle service lagen met uitzonde ring van grootschalige-Zie [SQL database Recovery](sql-database-recovery-using-backups.md#point-in-time-restore) | Ja, Zie [SQL database herstellen](sql-database-recovery-using-backups.md#point-in-time-restore) |
| [Polybase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) | Nee. U kunt een query uitvoeren op gegevens in de bestanden die in `OPENROWSET` Azure Blob Storage worden geplaatst met behulp van de functie. | Nee. U kunt een query uitvoeren op gegevens in de bestanden die in `OPENROWSET` Azure Blob Storage worden geplaatst met behulp van de functie. |
| [Predicaten](https://docs.microsoft.com/sql/t-sql/queries/predicates) | Ja | Ja |
| [Query meldingen](https://docs.microsoft.com/sql/relational-databases/native-client/features/working-with-query-notifications) | Nee | Ja |
| [R Services](https://docs.microsoft.com/sql/advanced-analytics/r-services/sql-server-r-services) | Ja, in [open bare preview](https://docs.microsoft.com/sql/advanced-analytics/what-s-new-in-sql-server-machine-learning-services)  | Nee |
| [Resource regeling](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor) | Nee | Ja |
| [Instructies voor herstellen](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-for-restoring-recovering-and-managing-backups-transact-sql) | Nee | Ja, met verplichte `FROM URL` opties voor de back-upbestanden die in Azure Blob Storage worden geplaatst. Zie [verschillen herstellen](sql-database-managed-instance-transact-sql-information.md#restore-statement) |
| [Data base terugzetten vanuit een back-up](https://docs.microsoft.com/sql/relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases#restore-data-backups) | Alleen uit automatische back-ups: Zie [SQL database herstel](sql-database-recovery-using-backups.md) | Van automatische back-ups: Zie [SQL database herstel](sql-database-recovery-using-backups.md) en de volledige back-ups die zijn geplaatst op Azure Blob Storage-Zie [back-upverschillen](sql-database-managed-instance-transact-sql-information.md#backup) |
| [Data base terugzetten naar SQL Server](https://docs.microsoft.com/sql/relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases#restore-data-backups) | Nee. Gebruik BACPAC of BCP in plaats van systeem eigen herstel. | Nee, omdat SQL Server data base-engine die in het beheerde exemplaar wordt gebruikt, een hogere versie heeft dan een RTM-versie van SQL Server die on-premises wordt gebruikt. Gebruik in plaats daarvan BACPAC, BCP of transactionele replicatie. |
| [Beveiliging op rijniveau](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) | Ja | Ja |
| [Semantisch zoeken](https://docs.microsoft.com/sql/relational-databases/search/semantic-search-sql-server) | Nee | Nee |
| [Volg nummers](https://docs.microsoft.com/sql/relational-databases/sequence-numbers/sequence-numbers) | Ja | Ja |
| [Service Broker](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-service-broker) | Nee | Ja, maar alleen binnen het exemplaar. [Service Broker verschillen](sql-database-managed-instance-transact-sql-information.md#service-broker) bekijken |
| [Server configuratie-instellingen](https://docs.microsoft.com/sql/database-engine/configure-windows/server-configuration-options-sql-server) | Nee | Ja, Zie [T-SQL-verschillen](sql-database-managed-instance-transact-sql-information.md) |
| [Set-instructies](https://docs.microsoft.com/sql/t-sql/statements/set-statements-transact-sql) | De meeste-Zie afzonderlijke instructies | Ja, Zie [T-SQL-verschillen](sql-database-managed-instance-transact-sql-information.md)|
| [Ruimtelijk](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server) | Ja | Ja |
| [SQL Server Agent](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent) | Nee, Zie [elastische taken](elastic-jobs-overview.md) | Ja, [SQL Server Agent verschillen](sql-database-managed-instance-transact-sql-information.md#sql-server-agent) bekijken |
| [SQL Server controle](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine) | Geen Zie [SQL database controle](sql-database-auditing.md) | Ja, Zie [controle verschillen](sql-database-managed-instance-transact-sql-information.md#auditing) |
| [Opgeslagen procedures](https://docs.microsoft.com/sql/relational-databases/stored-procedures/stored-procedures-database-engine) | Ja | Ja |
| [Opgeslagen systeem functies](https://docs.microsoft.com/sql/relational-databases/system-functions/system-functions-for-transact-sql) | De meeste-Zie afzonderlijke functies | Ja, Zie [opgeslagen procedures, functies, triggers-verschillen](sql-database-managed-instance-transact-sql-information.md#stored-procedures-functions-and-triggers) |
| [Opgeslagen systeem procedures](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/system-stored-procedures-transact-sql) | Sommige-Zie afzonderlijke opgeslagen procedures | Ja, Zie [opgeslagen procedures, functies, triggers-verschillen](sql-database-managed-instance-transact-sql-information.md#stored-procedures-functions-and-triggers) |
| [Systeem tabellen](https://docs.microsoft.com/sql/relational-databases/system-tables/system-tables-transact-sql) | Sommige-Zie afzonderlijke tabellen | Ja, Zie [T-SQL-verschillen](sql-database-managed-instance-transact-sql-information.md) |
| [Systeem catalogus weergaven](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/catalog-views-transact-sql) | Sommige-Zie afzonderlijke weer gaven | Ja, Zie [T-SQL-verschillen](sql-database-managed-instance-transact-sql-information.md) |
| [TempDB](https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database) | Ja. Maxi [maal 32 GB per kern voor elke Data Base](sql-database-vcore-resource-limits-single-databases.md). | Ja. [24 GB grootte per vCore voor de hele GP-laag en beperkt door de exemplaar grootte van de BC-laag](sql-database-managed-instance-resource-limits.md#service-tier-characteristics)  |
| [Tijdelijke tabellen](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql#database-scoped-global-temporary-tables-azure-sql-database) | Lokale en data base-scoped globale tijdelijke tabellen | Lokale tijdelijke tabellen van het bereik Local en instance-Scope |
| [Tijdelijke tabellen](https://docs.microsoft.com/sql/relational-databases/tables/temporal-tables) | [Ja](sql-database-temporal-tables.md) | [Ja](sql-database-temporal-tables.md) |
| Tijd zone-optie | Nee | [Ja](sql-database-managed-instance-timezone.md), en moet worden geconfigureerd wanneer het beheerde exemplaar wordt gemaakt. |
| Detectie van bedreigingen|  [Ja](sql-database-threat-detection.md)|[Ja](sql-database-managed-instance-threat-detection.md)|
| [Tracerings vlaggen](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql) | Nee | Nee |
| [Transactionele replicatie](sql-database-managed-instance-transactional-replication.md) | Alleen Ja, transactionele [en momentopname replicatie abonnee](sql-database-single-database-migrate.md) | Ja, in [open bare preview](https://docs.microsoft.com/sql/relational-databases/replication/replication-with-sql-database-managed-instance). Bekijk [hier](sql-database-managed-instance-transact-sql-information.md#replication)de beperkingen. |
| [Variabelen](https://docs.microsoft.com/sql/t-sql/language-elements/variables-transact-sql) | Ja | Ja |
| [TDE (Transparent Data Encryption)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-tde) | Alleen ja-Algemeen en Bedrijfskritiek service lagen| [Ja](transparent-data-encryption-azure-sql.md) |
| [Windows Server Failover Clustering](https://docs.microsoft.com/sql/sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server) | Nee. Andere technieken die [hoge Beschik baarheid](sql-database-high-availability.md) bieden, worden opgenomen in elke Data Base. Herstel na nood gevallen wordt beschreven in [overzicht van bedrijfs continuïteit met Azure SQL database](sql-database-business-continuity.md) | Nee. Andere technieken die [hoge Beschik baarheid](sql-database-high-availability.md) bieden, worden opgenomen in elke Data Base. Herstel na nood gevallen wordt beschreven in [overzicht van bedrijfs continuïteit met Azure SQL database](sql-database-business-continuity.md) |
| [XML-indexen](https://docs.microsoft.com/sql/t-sql/statements/create-xml-index-transact-sql) | Ja | Ja |

## <a name="platform-capabilities"></a>Mogelijkheden van het platform

Azure-platform biedt een aantal PaaS-mogelijkheden die als extra waarde worden toegevoegd aan de standaard database functies. Er zijn een aantal externe services die kunnen worden gebruikt met Azure SQL Database Service. 

| **Platform functie** | **Afzonderlijke data bases en elastische Pools** | **Beheerde instanties** |
| --- | --- | --- |
| [Actieve Geo-replicatie](sql-database-active-geo-replication.md) | Ja, alle service lagen dan grootschalige | Nee, Zie [automatische failover-groepen (preview-versie)](sql-database-auto-failover-group.md) als alternatief |
| [Groepen voor automatische failover](sql-database-auto-failover-group.md) | Ja, alle service lagen dan grootschalige | Ja, in [open bare preview](sql-database-auto-failover-group.md)|
| [Azure Resource Health](/azure/service-health/resource-health-overview) | Ja | Nee |
| [Gegevens migratie service (DMS)](https://docs.microsoft.com/sql/dma/dma-overview) | Ja | Ja |
| [Geo-restore](sql-database-recovery-using-backups.md#geo-restore) | Ja, alle service lagen dan grootschalige | Ja, met behulp van [Azure PowerShell](https://medium.com/azure-sqldb-managed-instance/geo-restore-your-databases-on-azure-sql-instances-1451480e90fa). |
| [Grootschalige-architectuur](sql-database-service-tier-hyperscale.md) | Ja | Nee |
| [Lange termijn retentie van back-ups-LTR](sql-database-long-term-retention.md) | Ja, behoud automatisch tot tien jaar back-ups. | Nog niet. Gebruik `COPY_ONLY` [hand matige back-ups](sql-database-managed-instance-transact-sql-information.md#backup) als tijdelijke oplossing. |
| [Op beleid gebaseerd beheer](https://docs.microsoft.com/sql/relational-databases/policy-based-management/administer-servers-by-using-policy-based-management) | Nee | Nee |
| Resource groepen | Ja, als [elastische Pools](sql-database-elastic-pool.md) | Ingebouwde één beheerd exemplaar kan meerdere data bases hebben die dezelfde groep resources delen |
| Omhoog of omlaag schalen (online) | Ja, u kunt DTU of gereserveerde vCores of maximale opslag ruimte wijzigen met de minimale downtime. | Ja, u kunt gereserveerde vCores of maximale opslag ruimte wijzigen met de minimale downtime. | 
| Automatisch schalen | Ja, in [serverloze model](sql-database-serverless.md) | Nee, u moet gereserveerde Compute en opslag kiezen. |
| Onderbreken/hervatten | Ja, in [serverloze model](sql-database-serverless.md) | Nee | 
| [SMO](https://docs.microsoft.com/sql/relational-databases/server-management-objects-smo/sql-server-management-objects-smo-programming-guide) | [Ja](https://www.nuget.org/packages/Microsoft.SqlServer.SqlManagementObjects) | Ja, [versie 150](https://www.nuget.org/packages/Microsoft.SqlServer.SqlManagementObjects) |
| [SQL Analytics](https://docs.microsoft.com/azure/azure-monitor/insights/azure-sql) | Ja | Ja |
| [SQL Data Sync](sql-database-get-started-sql-data-sync.md) | Ja | Nee |
| [SQL Server PowerShell](https://docs.microsoft.com/sql/relational-databases/scripting/sql-server-powershell) | Ja | Ja |
| [SQL Server Analysis Services (SSAS)](https://docs.microsoft.com/sql/analysis-services/analysis-services) | Nee, [Azure Analysis Services](https://azure.microsoft.com/services/analysis-services/) is een afzonderlijke Azure-Cloud service. | Nee, [Azure Analysis Services](https://azure.microsoft.com/services/analysis-services/) is een afzonderlijke Azure-Cloud service. |
| [SQL Server Integration Services (SSIS)](https://docs.microsoft.com/sql/integration-services/sql-server-integration-services) | Ja, met een beheerde SSIS in Azure Data Factory-omgeving (ADF), waarbij pakketten worden opgeslagen in SSISDB die worden gehost door Azure SQL Database en worden uitgevoerd op Azure SSIS Integration Runtime (IR), raadpleegt u [Azure-SSIS IR maken in ADF](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime). <br/><br/>Zie [Azure SQL database afzonderlijke data bases/elastische Pools en beheerde exemplaren vergelijken](../data-factory/create-azure-ssis-integration-runtime.md#compare-sql-database-single-databaseelastic-pool-and-sql-database-managed-instance)om de SSIS-functies in SQL database server en het beheerde exemplaar te vergelijken. | Ja, met een beheerde SSIS in Azure Data Factory-omgeving (ADF), waarbij pakketten worden opgeslagen in SSISDB die worden gehost door een beheerd exemplaar en worden uitgevoerd op Azure SSIS Integration Runtime (IR), raadpleegt u [Azure-SSIS IR maken in ADF](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime). <br/><br/>Zie [Azure SQL database afzonderlijke data bases/elastische Pools en beheerde exemplaren vergelijken](../data-factory/create-azure-ssis-integration-runtime.md#compare-sql-database-single-databaseelastic-pool-and-sql-database-managed-instance)om de SSIS-functies in SQL database en het beheerde exemplaar te vergelijken. |
| [SQL Server Reporting Services (SSRS)](https://docs.microsoft.com/sql/reporting-services/create-deploy-and-manage-mobile-and-paginated-reports) | Nee- [zie Power bi](https://docs.microsoft.com/power-bi/) | Nee- [zie Power bi](https://docs.microsoft.com/power-bi/) |
| [Query prestaties inzichten (QPI)](sql-database-query-performance.md) | Ja | Nee. Gebruik ingebouwde rapporten in SQL Server Management Studio en Azure Data Studio. |
| [VNet](../virtual-network/virtual-networks-overview.md) | Gedeeltelijk, het maakt beperkte toegang mogelijk met behulp van [VNet-eind punten](sql-database-vnet-service-endpoint-rule-overview.md) | Ja, beheerd exemplaar wordt geïnjecteerd in het VNet van de klant. Zie [subnet](sql-database-managed-instance-transact-sql-information.md#subnet) en [VNet](sql-database-managed-instance-transact-sql-information.md#vnet) |

## <a name="tools"></a>Hulpprogramma's
Azure SQL database ondersteunt diverse hulp middelen voor gegevens die uou kunnen helpen bij het beheren van uw gegevens.

| **SQL-hulp programma** | **Afzonderlijke data bases en elastische Pools** | **Beheerde instanties** |
| --- | --- | --- |
| [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/what-is) | Ja | Ja |
| [BACPAC-bestand (exporteren)](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application) | Ja, Zie [SQL database exporteren](sql-database-export.md) | Ja, Zie [SQL database exporteren](sql-database-export.md) |
| [BACPAC-bestand (importeren)](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/import-a-bacpac-file-to-create-a-new-user-database) | Ja, Zie [SQL database importeren](sql-database-import.md) | Ja, Zie [SQL database importeren](sql-database-import.md) |
| [Data Quality Services (DQS)](https://docs.microsoft.com/sql/data-quality-services/data-quality-services) | Nee | Nee |
| [Master Data Services (MDS)](https://docs.microsoft.com/sql/master-data-services/master-data-services-overview-mds) | Nee | Nee |
| [SQL Server Data Tools (SSDT)](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt) | Ja | Ja |
| [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) | Ja | Ja [, versie 18,0 en hoger](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) |
| [SQL Server Profiler](https://docs.microsoft.com/sql/tools/sql-server-profiler/sql-server-profiler) | Geen-Zie [uitgebreide gebeurtenissen](sql-database-xevent-db-diff-from-svr.md) | Ja |
| [System Center Operations Manager-SCOM](https://docs.microsoft.com/system-center/scom/welcome) | [Ja](https://www.microsoft.com/download/details.aspx?id=38829) | Nee |

## <a name="next-steps"></a>Volgende stappen

- Zie [Wat is SQL Database?](sql-database-technical-overview.md) voor informatie over de service Azure SQL Database
- Zie [Wat is een beheerd exemplaar?](sql-database-managed-instance.md)voor meer informatie over een beheerd exemplaar.
