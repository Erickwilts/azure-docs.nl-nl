---
title: Azure SQL Database geavanceerde beveiligingsoverzicht | Microsoft Docs
description: Dit onderwerp beschrijft een geavanceerde beveiliging van gegevens van Azure SQL Database en wordt uitgelegd hoe het werkt en wat is het verschil met een enkele of gegroepeerde database in Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: bonova
ms.author: bonova
ms.reviewer: sstein, carlrab, vanto
manager: craigg
ms.date: 06/26/2019
ms.openlocfilehash: b03f546b992bd9de6092dc0da8ef72aa69aa1da2
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67447793"
---
# <a name="use-sql-database-advanced-data-security-with-virtual-networks-and-near-100-compatibility"></a>Geavanceerde beveiliging van gegevens met virtuele netwerken en in de buurt van 100% compatibiliteit van SQL-Database gebruiken

Beheerd exemplaar is een nieuwe optie voor implementatie van Azure SQL Database, bijna 100% compatibiliteit met de nieuwste SQL Server on-premises Database-Engine (Enterprise Edition) bieden, biedt een systeemeigen [virtueel netwerk (VNet)](../virtual-network/virtual-networks-overview.md) implementatie die algemene beveiligingsproblemen, en een [bedrijfsmodel](https://azure.microsoft.com/pricing/details/sql-database/) gunstig voor on-premises SQL Server-klanten. Het beheerde exemplaar implementatiemodel kan bestaande SQL Server klanten lift- and -shift van hun on-premises toepassingen naar de cloud met minimale wijzigingen van toepassing en -database. Op hetzelfde moment, behoudt de Implementatieoptie voor beheerd exemplaar alle PaaS-mogelijkheden (automatische patching en versie-updates, [geautomatiseerde back-ups](sql-database-automated-backups.md), [hoge beschikbaarheid](sql-database-high-availability.md) ), die aanzienlijk minder beperkt beheer-overhead en totale Eigendomskosten.

> [!IMPORTANT]
> Zie voor een lijst met regio's waarin het beheerde exemplaar Implementatieoptie op dit moment beschikbaar is, [ondersteunde regio's](sql-database-managed-instance-resource-limits.md#supported-regions).

Het volgende diagram geeft een overzicht van de belangrijkste functies van beheerde exemplaren:

![belangrijke functies](./media/sql-database-managed-instance/key-features.png)

Het implementatiemodel van het beheerde exemplaar is ontworpen voor klanten die willen migreren van een groot aantal apps van on-premises of IaaS, zelf is ingebouwd, of ISV-omgeving volledig beheerde PaaS-cloudomgeving, met als lage migratie inspanning mogelijk worden opgegeven. Met behulp van de volledig geautomatiseerde [Data Migration Service (DMS)](../dms/tutorial-sql-server-to-managed-instance.md#create-an-azure-database-migration-service-instance) in Azure, klanten kunnen lift- en shift van hun on-premises SQL Server naar een beheerd exemplaar biedt compatibiliteit met SQL Server on-premises en volledig isoleren de exemplaren van de klant met ondersteuning voor systeemeigen VNet.  Met Software Assurance, kunt u exchange-hun bestaande licenties voor kortingstarieven op een beheerd exemplaar met de [Azure Hybrid Benefit voor SQL Server](https://azure.microsoft.com/pricing/hybrid-benefit/).  Een beheerd exemplaar is de beste Migratiebestemming in de cloud voor SQL Server-exemplaren die hoge beveiliging en een uiterst programmeerbare oppervlak vereisen.

Het beheerde exemplaar implementatie optie doelstellingen biedt dicht bij surface area van 100% compatibiliteit met de nieuwste versie van on-premises SQL Server via een gefaseerde release-plan.

Om te bepalen tussen de implementatieopties voor Azure SQL Database: Zie individuele database, gepoolde database en het beheerd exemplaar, en SQL Server die wordt gehost in virtuele machine, [u bij het kiezen van de juiste versie van SQL Server in Azure](sql-database-paas-vs-sql-server-iaas.md).

## <a name="key-features-and-capabilities"></a>Belangrijke functies en mogelijkheden

Beheerd exemplaar combineert het beste functies die beschikbaar zijn in Azure SQL Database en SQL Server Database Engine.

> [!IMPORTANT]
> Een beheerd exemplaar wordt uitgevoerd met alle van de functies van de meest recente versie van SQL Server, met inbegrip van online bewerkingen, automatische plan correcties en andere prestatieverbeteringen enterprise. Een vergelijking van de functies die beschikbaar wordt uitgelegd in [vergelijking van functies: Azure SQL Database versus SQL Server](sql-database-features.md).

| **Voordelen van PaaS** | **Bedrijfscontinuïteit** |
| --- | --- |
|Er is geen aanschaffen van hardware en het beheer <br>Er is geen management overhead voor het beheren van de onderliggende infrastructuur <br>Snel inrichten en schalen van service <br>Automatische patching en versie-upgrade <br>Integratie met andere PaaS-services voor gegevens |uptime van 99,99% SLA  <br>Ingebouwde [hoge beschikbaarheid](sql-database-high-availability.md) <br>Gegevens die worden beveiligd met [geautomatiseerde back-ups](sql-database-automated-backups.md) <br>Klanten kunnen worden geconfigureerd back-up bewaarperiode <br>De gebruiker geïnitieerde [back-ups](https://docs.microsoft.com/sql/t-sql/statements/backup-transact-sql?view=azuresqldb-mi-current) <br>[Punt in tijd database terugzetten](sql-database-recovery-using-backups.md#point-in-time-restore) mogelijkheid |
|**Beveiliging en naleving** | **Management**|
|Geïsoleerde omgeving ([VNet-integratie](sql-database-managed-instance-connectivity-architecture.md), één service, speciale berekenings- en tenant) <br>[TDE (Transparent Data Encryption)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql)<br>[Azure AD-verificatie](sql-database-aad-authentication.md), eenmalige aanmelding <br> <a href="/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current">Azure AD server-principals (aanmeldingen)</a> (**preview-versie**) <br>Voldoet aan de standaarden voor compliance hetzelfde als Azure SQL-database <br>[SQL-controle](sql-database-managed-instance-auditing.md) <br>[Detectie van bedreigingen](sql-database-managed-instance-threat-detection.md) |Azure Resource Manager-API voor het automatiseren van service inrichten en schalen <br>Functionaliteit voor handmatige service inrichten en schalen van Azure portal <br>Data migratieservice

> [!IMPORTANT]
> Azure SQL-Database (alle implementatieopties voor), is gecertificeerd volgens diverse nalevingsstandaarden. Zie voor meer informatie de [Microsoft Azure Trust Center](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942) waar u de meest recente lijst met SQL Database-nalevingscertificeringen kunt vinden.

De belangrijkste functies van beheerde exemplaren worden weergegeven in de volgende tabel:

|Functie | Description|
|---|---|
| SQL Server-versie / build | SQL Server Database Engine (meest recente stabiel) |
| Automatische back-ups beheren | Ja |
| Ingebouwd exemplaar en database controleren en metrische gegevens | Ja |
| Automatische softwarepatches | Ja |
| De nieuwste functies van de Database-Engine | Ja |
| Het aantal gegevensbestanden (rijen) per de database | Meerdere |
| Aantal logboekbestanden (logboek) per database | 1 |
| VNet - implementatie van Azure Resource Manager | Ja |
| VNet - klassieke implementatiemodel | Nee |
| Portal-ondersteuning | Ja|
| Ingebouwde integratie-Service (SSIS) | Niet - SSIS is een onderdeel van [PaaS van Azure Data Factory](https://docs.microsoft.com/azure/data-factory/tutorial-deploy-ssis-packages-azure) |
| Ingebouwde Analysis Services (SSAS) | Niet - SSAS is gescheiden [PaaS](https://docs.microsoft.com/azure/analysis-services/analysis-services-overview) |
| Ingebouwde Reporting Service (SSRS) | Nee, Power BI of SSRS IaaS gebruiken |
|||

## <a name="vcore-based-purchasing-model"></a>Aankoopmodel op basis van vCore

De [vCore gebaseerde aankoopmodel](sql-database-service-tiers-vcore.md) voor beheerde exemplaren biedt u flexibiliteit, controle, transparantie, en een eenvoudige manier te vertalen on-premises vereisten workloads naar de cloud. Dit model kunt u rekenkracht, geheugen en opslag op basis van uw workloadbehoefte wijzigen. Het vCore-model is ook in aanmerking komen voor van 30 procent besparen met de [Azure Hybrid Benefit voor SQL Server](https://azure.microsoft.com/pricing/hybrid-benefit/).

In het vCore-model, kunt u kiezen tussen verschillende hardwaregeneraties.

- **Gen4** logische CPU's zijn gebaseerd op Intel E5-2673 v3 (Haswell)-processors van 2,4 GHz, gekoppelde SSD, fysieke kernen, 7 GB RAM-geheugen per kern en compute-grootten tussen 8 en maximaal 24 vCores.
- **Gen5** logische CPU's zijn gebaseerd op Intel E5-2673 v4-processors 2,3 GHz (Broadwell) NVMe SSD, hyper-threaded logische core, een snelle en compute-grootten tussen 4 en 80 kernen.

Meer informatie over het verschil tussen hardwaregeneraties in [beheerd exemplaar resourcelimieten](sql-database-managed-instance-resource-limits.md#hardware-generation-characteristics).

> [!IMPORTANT]
> Nieuwe Gen4 databases worden niet meer ondersteund in de regio AustraliaEast.

## <a name="managed-instance-service-tiers"></a>Beheerd exemplaar van service-lagen

Beheerd exemplaar is beschikbaar in twee Servicelagen:

- **Algemeen gebruik**: Ontworpen voor toepassingen met normale prestaties en i/o-latentie is vereist.
- **Bedrijfskritiek**: Ontworpen voor toepassingen met laag i/o-latentie is vereist en minimale gevolgen van het onderliggende Onderhoudsbewerkingen op de werkbelasting.

Beide Servicelagen garanderen een beschikbaarheid van 99,99% en kunnen u onafhankelijk opslaggrootte selecteren en de rekencapaciteit. Zie voor meer informatie over de architectuur voor hoge beschikbaarheid van Azure SQL Database, [hoge beschikbaarheid en Azure SQL Database](sql-database-high-availability.md).

### <a name="general-purpose-service-tier"></a>Categorie voor algemeen gebruik-service

De volgende lijst beschrijft de belangrijkste kenmerken van de categorie Algemeen gebruik-service:

- Ontwerpen voor de meeste zakelijke toepassingen met normale prestatie-eisen
- Hoogwaardige Azure Blob-opslag (8 TB)
- Ingebouwde [hoge beschikbaarheid](sql-database-high-availability.md#basic-standard-and-general-purpose-service-tier-availability) op basis van betrouwbare Azure Blob-opslag en [Azure Service Fabric](../service-fabric/service-fabric-overview.md)

Zie voor meer informatie, [opslag laag in het algemeen doel laag](https://medium.com/azure-sqldb-managed-instance/file-layout-in-general-purpose-azure-sql-managed-instance-cf21fff9c76c) en [storage aanbevolen procedures voor prestaties en overwegingen voor het beheerde exemplaren (Algemeen)](https://blogs.msdn.microsoft.com/sqlcat/2018/07/20/storage-performance-best-practices-and-considerations-for-azure-sql-db-managed-instance-general-purpose/).

Meer informatie over het verschil tussen service-lagen in [beheerd exemplaar resourcelimieten](sql-database-managed-instance-resource-limits.md#service-tier-characteristics).

### <a name="business-critical-service-tier"></a>Kritieke-bedrijfslaag

Kritieke-bedrijfslaag is gebouwd voor toepassingen met hoge i/o-vereisten. Het biedt de hoogste herstelmogelijkheden bij fouten met behulp van verschillende geïsoleerde replica's.

De volgende lijst geeft een overzicht van de belangrijkste kenmerken van de laag bedrijfskritiek service:

- Ontworpen voor zakelijke toepassingen met de hoogste prestaties en HA-vereisten
- Wordt geleverd met zeer snelle, lokale SSD-opslag (maximaal 1 TB op Gen4 en tot 4 TB voor Gen5)
- Ingebouwde [hoge beschikbaarheid](sql-database-high-availability.md#premium-and-business-critical-service-tier-availability) op basis van [Always On Availability Groups](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server) en [Azure Service Fabric](../service-fabric/service-fabric-overview.md).
- Ingebouwde extra [alleen-lezen databasereplica](sql-database-read-scale-out.md) die kan worden gebruikt voor rapportage- en andere werkbelastingen alleen-lezen
- [In-Memory OLTP](sql-database-in-memory.md) die kunnen worden gebruikt voor de werkbelasting aan de vereisten voor hoge prestaties  

Meer informatie over het verschil tussen service-lagen in [beheerd exemplaar resourcelimieten](sql-database-managed-instance-resource-limits.md#service-tier-characteristics).

## <a name="advanced-security-and-compliance"></a>Geavanceerde beveiliging en naleving van voorschriften

De Implementatieoptie voor beheerd exemplaar combineert geavanceerde beveiligingsfuncties die worden geleverd door Azure-cloud- en SQL Server Database Engine.

### <a name="managed-instance-security-isolation"></a>Isolatie beheerd exemplaar

Een beheerd exemplaar biedt extra beveiligingsisolatie van andere tenants in de Azure-cloud. Isolatie bevat:

- [Implementatie van systeemeigen virtuele netwerk](sql-database-managed-instance-connectivity-architecture.md) en verbinding met uw on-premises-omgeving met behulp van Azure Express Route- of VPN-Gateway.
- In een standaardimplementatie, wordt SQL-eindpunt alleen weergegeven via een privé IP-adres, zodat u veilige connectiviteit van Azure privé of hybride netwerken.
- Één tenant met toegewezen onderliggende infrastructuur (compute, storage).

Het volgende diagram geeft een overzicht van de verschillende connectiviteitsopties voor uw toepassingen:

![hoge beschikbaarheid](./media/sql-database-managed-instance/application-deployment-topologies.png)  

Zie voor meer informatie over VNet-integratie en toegang wordt afgedwongen op het subnetniveau meer [VNet-architectuur voor beheerde exemplaren](sql-database-managed-instance-connectivity-architecture.md) en [verbinding maken met uw toepassing naar een beheerd exemplaar](sql-database-managed-instance-connect-app.md).

> [!IMPORTANT]
> Plaats meerdere beheerd exemplaar in hetzelfde subnet bevinden, waar die is toegestaan door uw beveiligingsvereisten, omdat u extra voordelen opent Hiermee. Collocating-exemplaren in hetzelfde subnet aanzienlijk netwerken onderhoud aan de infrastructuur vereenvoudigen en exemplaar inrichtingstijd, omdat de lange duur van de inrichting is gekoppeld aan de kosten van de implementatie van de eerste beheerde instantie in een subnet beperken.

### <a name="azure-sql-database-security-features"></a>Beveiligingsfuncties van Azure SQL Database

Azure SQL Database biedt een set geavanceerde beveiligingsfuncties die kunnen worden gebruikt om uw gegevens te beveiligen.

- [Beheerd exemplaar controle](sql-database-managed-instance-auditing.md) databasegebeurtenissen en schrijft deze naar een controlelogboekbestand in uw Azure storage-account geplaatst. Controles kunt naleving van regelgeving, begrijpen databaseactiviteiten en inzicht krijgen in discrepanties en afwijkingen die kunnen wijzen op problemen voor het bedrijf of vermoedelijke beveiligingsschendingen.
- Versleuteling van gegevens in beweging - een beheerd exemplaar van uw gegevens worden beveiligd door te geven van de versleuteling van data-in-motion met behulp van Transport Layer Security. Naast transport layer security, de Implementatieoptie voor beheerd exemplaar biedt bescherming van gevoelige gegevens in beweging, inactieve en tijdens queryverwerking met [Always Encrypted](/sql/relational-databases/security/encryption/always-encrypted-database-engine). Het unieke Always Encrypted biedt ongeëvenaarde beveiliging tegen diefstal van kritieke gegevens. Bijvoorbeeld, met Always Encrypted, creditcardnummers, worden versleuteld opgeslagen in de database altijd, zelfs tijdens query verwerken, waardoor ontsleuteling bij gebruik door gemachtigde medewerkers of toepassingen die deze gegevens moeten verwerken.
- [Detectie van bedreigingen](sql-database-managed-instance-threat-detection.md) vormt een aanvulling op [controle](sql-database-managed-instance-auditing.md) door te geven van een extra beveiligingslaag die is ingebouwd in de service die ongebruikelijke en potentieel schadelijke detecteert intelligence probeert te openen of misbruik te maken van databases. U wordt gewaarschuwd bij verdachte activiteiten, potentiële kwetsbaarheden, en SQL-injectie aanvallen, en afwijkende patronen voor databasetoegang. Waarschuwingen van Threat detection kunnen worden bekeken via [Azure Security Center](https://azure.microsoft.com/services/security-center/) en Geef details op van verdachte activiteiten en geven aanbevelingen voor het onderzoeken en tegenhouden.  
- [Dynamische gegevensmaskering](/sql/relational-databases/security/dynamic-data-masking) blootstelling van gevoelige gegevens door deze te maskeren voor niet-gemachtigde gebruikers. Dynamische gegevensmaskering helpt onbevoegde toegang tot gevoelige gegevens voorkomen doordat u aangeven hoeveel van de gevoelige gegevens worden vrijgegeven, met minimale impact op de toepassingslaag. Dit is een beveiligingsfunctie op basis van beleid. De gevoelige gegevens in de resultatenset van een query die is uitgevoerd op toegewezen databasevelden worden verborgen, terwijl de gegevens in de database niet worden gewijzigd.
- [Beveiliging op rijniveau](/sql/relational-databases/security/row-level-security) kunt u om te bepalen de toegang tot rijen in een database-tabel op basis van de kenmerken van de gebruiker die een query uitvoert (bijvoorbeeld door het groepslidmaatschap of uitvoeringscontext context groep). Beveiliging op rijniveau (RLS) vereenvoudigt het ontwerp en de code van de beveiliging in uw toepassing. Met RLS kunt u beperkingen instellen voor de toegang tot gegevens in rijen. Bijvoorbeeld, ervoor te zorgen dat werknemers alleen de rijen met gegevens die relevant voor hun afdeling zijn, of een gegevenstoegang beperken tot alleen de relevante gegevens.
- [Transparante gegevensversleuteling (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) beheerd exemplaar van de gegevensbestanden, bekend als het versleutelen van gegevens in rust worden versleuteld. TDE voert realtime i/o-versleuteling en ontsleuteling van de gegevens en logboekbestanden. De versleuteling gebruikt een databaseversleutelingssleutel (DEK), dat is opgeslagen in de database-bootrecord voor beschikbaarheid tijdens het herstel. U kunt alle databases in een beheerd exemplaar met transparante gegevensversleuteling beveiligen. TDE is van de SQL Server beproefde versleuteling-at-rest-technologie die is volgens veel nalevingsstandaarden vereist voor bescherming tegen diefstal van opslagmedia.

Migratie van een versleutelde database naar een beheerd exemplaar wordt via de Azure Database Migration Service (DMS) of systeemeigen terugzetten ondersteund. Als u van plan bent voor het migreren van een versleutelde database met behulp van systeemeigen herstellen, is de migratie van de bestaande TDE certificaat van de on-premises SQL Server of SQL Server in een virtuele machine naar een beheerd exemplaar van een vereiste stap. Zie voor meer informatie over opties voor de migratie, [migratie van SQL Server-exemplaar naar beheerd exemplaar](sql-database-managed-instance-migrate.md).

## <a name="azure-active-directory-integration"></a>Integratie van Azure Active Directory

De Implementatieoptie voor beheerd exemplaar biedt ondersteuning voor traditionele SQL server Database engine-aanmeldingen en aanmeldingen die zijn geïntegreerd met Azure Active Directory (AAD). Azure AD server-principals (aanmeldingen) (**preview-versie**) zijn Azure-cloud-versie van on-premises Databaseaanmeldingen die u in uw on-premises omgeving gebruikt. Azure AD server-principals (aanmeldingen) kunt u om op te geven van gebruikers en groepen uit uw Azure Active Directory ' True ' op binnen het bereik van exemplaar-principals, geschikt voor het uitvoeren van een bewerking op exemplaarniveau, met inbegrip van query's tussen meerdere databases binnen dezelfde tenant beheerd exemplaar.

Een nieuwe syntaxis is geïntroduceerd voor het maken van Azure AD server-principals (aanmeldingen) (**openbare preview**), **van externe PROVIDER**. Zie voor meer informatie over de syntaxis van de <a href="/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current">CREATE LOGIN</a>, en bekijk de [inrichten van een Azure Active Directory-beheerder voor uw beheerde exemplaar](sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-managed-instance) artikel.

### <a name="azure-active-directory-integration-and-multi-factor-authentication"></a>Azure Active Directory-integratie en meervoudige verificatie

Het beheerde exemplaar implementatie-optie kunt u centraal beheren van identiteiten van databasegebruikers en andere Microsoft-services met [Azure Active Directory-integratie](sql-database-aad-authentication.md). Deze mogelijkheid vereenvoudigt het beheer van machtigingen en verbetert de beveiliging. Azure Active Directory ondersteunt [Multi-Factor Authentication](sql-database-ssms-mfa-authentication-configure.md) (MFA) voor betere beveiliging van gegevens en toepassingen, en ondersteunt ook een proces voor eenmalige aanmelding (SSO).

### <a name="authentication"></a>Verificatie

Beheerd exemplaar verificatie verwijst naar hoe gebruikers hun identiteit bewijst bij het verbinden met de database. SQL Database ondersteunt twee typen verificatie:  

- **SQL-verificatie**:

  Deze verificatiemethode maakt gebruik van een gebruikersnaam en wachtwoord.
- **Azure Active Directory-verificatie**:

  Deze verificatiemethode maakt gebruik van identiteiten die worden beheerd door Azure Active Directory en wordt ondersteund voor beheerde en geïntegreerde domeinen. Gebruik [waar mogelijk](https://docs.microsoft.com/sql/relational-databases/security/choose-an-authentication-mode) Active Directory-verificatie (geïntegreerde beveiliging).

### <a name="authorization"></a>Autorisatie

Autorisatie verwijst naar wat een gebruiker kan doen binnen een Azure SQL Database en wordt beheerd door de databaserollidmaatschappen en objectmachtigingen van uw gebruikersaccount. Een beheerd exemplaar heeft dezelfde autorisatiemogelijkheden bedoeld als SQL Server 2017.

## <a name="database-migration"></a>Databasemigratie

Beheerd exemplaar implementatie optie doelen gebruikersscenario's met grote databasemigratie van on-premises of IaaS-implementaties database. Beheerd exemplaar biedt ondersteuning voor verschillende opties voor de migratie van database:

### <a name="back-up-and-restore"></a>Back-up en herstel  

De migratie maakt gebruik van SQL-back-ups naar Azure Blob storage. Back-ups die zijn opgeslagen in Azure storage-blob kunnen rechtstreeks worden hersteld naar een beheerd exemplaar met de [T-SQL terugzetten opdracht](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-transact-sql?view=azuresqldb-mi-current).

- Zie voor een snelstart voor het herstellen van de Wide World Importers - back-upbestand van Standard-database, het [herstellen van een back-upbestand naar een beheerd exemplaar](sql-database-managed-instance-get-started-restore.md). In deze quickstart ziet dat u moet een back-upbestand uploaden naar Azure BLOB-opslag en beveiligd met behulp van een Shared access signature (SAS)-sleutel.
- Zie voor meer informatie over het herstellen van URL [systeemeigen terugzetten vanuit URL](sql-database-managed-instance-migrate.md#native-restore-from-url).

> [!IMPORTANT]
> Back-ups van een beheerd exemplaar kunnen alleen worden hersteld naar een ander beheerd exemplaar. Ze kunnen niet worden hersteld naar een on-premises SQL-Server of naar een enkele database/elastische pool.

### <a name="data-migration-service"></a>Data migratieservice

Azure Database Migration Service is een volledig beheerde service die is ontworpen om in te schakelen naadloze migratie van meerdere databasebronnen naar Azure Data platforms met minimale downtime. Deze service stroomlijnt de vereiste taken voor het verplaatsen van bestaande van derden en SQL Server-databases naar Azure SQL Database (individuele databases, gepoolde databases in elastische pools en databases in een beheerd exemplaar van exemplaar) en SQL Server in virtuele Azure-machine. Zie [uw on-premises database migreren naar beheerd exemplaar met behulp van DMS](https://aka.ms/migratetoMIusingDMS).

## <a name="sql-features-supported"></a>SQL-functies die worden ondersteund

De Implementatieoptie voor het beheerde exemplaar is erop gericht om te leveren dicht bij surface area van 100% compatibiliteit met on-premises SQL Server die afkomstig zijn in verschillende fasen totdat de algemene beschikbaarheid van de service. Voor een functies en van de vergelijkingslijst, Zie [vergelijking van de SQL-Database](sql-database-features.md), en voor een lijst van T-SQL-verschillen in beheerde exemplaren ten opzichte van SQL Server, Zie [beheerd exemplaar T-SQL-verschillen van SQL Server](sql-database-managed-instance-transact-sql-information.md).

De Implementatieoptie voor beheerd exemplaar biedt ondersteuning voor achterwaartse compatibiliteit met SQL 2008-databases. Directe migratie van SQL 2005-database-servers wordt ondersteund, het compatibiliteitsniveau voor gemigreerde SQL 2005-databases worden bijgewerkt naar SQL 2008.
  
Het volgende diagram geeft een overzicht van surface area compatibiliteit in het beheerde exemplaar:  

![Migratie](./media/sql-database-managed-instance/migration.png)

### <a name="key-differences-between-sql-server-on-premises-and-in-a-managed-instance"></a>Belangrijke verschillen tussen SQL Server on-premises en in een beheerd exemplaar

De voordelen van beheerd exemplaar implementatie optie wordt altijd-up-to-date in de cloud, betekent dat sommige functies in on-premises SQL Server kunnen een verouderd, buiten gebruik gesteld of alternatieven zijn. Er zijn specifieke gevallen wanneer hulpprogramma's nodig hebt voor het herkennen van dat een bepaalde functie in een iets andere manier werkt of service wordt niet uitgevoerd in een omgeving die u volledig geen zeggenschap:

- Hoge beschikbaarheid is ingebouwd in en vooraf geconfigureerd met behulp van technologie die vergelijkbaar is met [Always On Availability Groups](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server).
- Automatische back-ups en punt in tijd herstel. Klanten kan initiëren `copy-only` back-ups die niet leiden tot met automatische back-keten problemen.
- Beheerd exemplaar is niet toegestaan voor het volledige fysieke paden op te geven, zodat alle bijbehorende scenario's moeten anders worden ondersteund: DB herstellen biedt geen ondersteuning voor het verplaatsen met, DB maken kunnen geen fysieke paden, BULK INSERT werkt met Azure-Blobs alleen, enzovoort.
- Beheerd exemplaar ondersteunt [Azure AD-verificatie](sql-database-aad-authentication.md) als cloud-alternatief voor het Windows-verificatie.
- Beheerd exemplaar beheert automatisch de XTP-bestandsgroep en bestanden voor databases met In-Memory OLTP-objecten
- Beheerd exemplaar biedt ondersteuning voor SQL Server Integration Services (SSIS) en host SSIS-catalogus (SSISDB) waarin de SSIS-pakketten kunt, maar ze worden uitgevoerd op een beheerde Azure-SSIS Integration Runtime (IR) in Azure Data Factory (ADF), Zie [maken Azure-SSIS IR in ADF](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime). Als u wilt vergelijken van de SSIS-functies in SQL-Database, Zie [vergelijken Azure SQL Database single databases/elastische pools en beheerd exemplaar](../data-factory/create-azure-ssis-integration-runtime.md#compare-sql-database-single-databaseelastic-pool-and-sql-database-managed-instance).

### <a name="managed-instance-administration-features"></a>Beheerd exemplaar beheerfuncties

De Implementatieoptie voor beheerd exemplaar kunt systeembeheerder besteed minder tijd aan administratieve taken omdat de service SQL Database, kunnen voor u uitvoert of deze taken vereenvoudigt. Bijvoorbeeld, [OS / RDBMS-installatie en patching uit handen](sql-database-high-availability.md), [dynamische exemplaar vergroten of verkleinen en de configuratie](sql-database-single-database-scale.md), [back-ups](sql-database-automated-backups.md), [databasereplicatie](replication-with-sql-database-managed-instance.md)(met inbegrip van systeemdatabases), [configuratie voor hoge beschikbaarheid](sql-database-high-availability.md), en de configuratie van de gezondheid en [prestatiebewaking](../azure-monitor/insights/azure-sql.md) gegevensstromen.

> [!IMPORTANT]
> Zie voor een lijst met ondersteunde, gedeeltelijk ondersteunde en niet-ondersteunde functies [SQL Database-functies](sql-database-features.md). Zie voor een lijst van T-SQL-verschillen in beheerde exemplaren ten opzichte van SQL Server, [beheerd exemplaar T-SQL-verschillen van SQL Server](sql-database-managed-instance-transact-sql-information.md)

### <a name="how-to-programmatically-identify-a-managed-instance"></a>Een beheerd exemplaar via een programma te identificeren

De volgende tabel toont enkele eigenschappen, toegankelijk zijn via Transact-SQL, dat u kunt gebruiken voor het detecteren van uw toepassing werkt met beheerd exemplaar en belangrijke eigenschappen ophalen.

|Eigenschap|Value|Opmerking|
|---|---|---|
|`@@VERSION`|Microsoft SQL Azure (RTM) - 12.0.2000.8 2018-03-07 Copyright (C) 2018 Microsoft Corporation.|Deze waarde is gelijk aan die in SQL-Database.|
|`SERVERPROPERTY ('Edition')`|SQL Azure|Deze waarde is gelijk aan die in SQL-Database.|
|`SERVERPROPERTY('EngineEdition')`|8|Deze waarde identificatie unieke van een beheerd exemplaar.|
|`@@SERVERNAME`, `SERVERPROPERTY ('ServerName')`|Volledige DNS-exemplaarnaam in de volgende indeling:`<instanceName>`.`<dnsPrefix>`.database.Windows.NET, waar `<instanceName>` is geleverd door de klant, terwijl `<dnsPrefix>` automatisch gegenereerde deel uitmaakt van de naam van de globale DNS-naam uniekheid garanderen ('wcus17662feb9ce98', bijvoorbeeld)|Voorbeeld: Mijn-managed-instance.wcus17662feb9ce98.database.windows.net|

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het maken van uw eerste beheerd exemplaar, [snelstartgids](sql-database-managed-instance-get-started.md).
- Voor een functies en van de vergelijkingslijst, Zie [algemene SQL-functies](sql-database-features.md).
- Zie voor meer informatie over VNet-configuratie, [managed instance VNet configuration](sql-database-managed-instance-connectivity-architecture.md).
- Zie voor een snelstartgids die een beheerd exemplaar maakt en wordt een database teruggezet vanuit een back-upbestand, [een beheerd exemplaar maken](sql-database-managed-instance-get-started.md).
- Zie voor een zelfstudie met behulp van de Azure Database Migration Service (DMS) voor de migratie, [beheerd exemplaar migreren met behulp van DMS](../dms/tutorial-sql-server-to-managed-instance.md).
- Voor geavanceerde bewaking van prestaties van de database beheerde instanties met ingebouwde intelligentie voor het oplossen van problemen, Zie [Monitor Azure SQL Database met behulp van Azure SQL Analytics](../azure-monitor/insights/azure-sql.md).
- Zie voor informatie over de prijzen [SQL-Database beheerd exemplaar prijzen](https://azure.microsoft.com/pricing/details/sql-database/managed/).
