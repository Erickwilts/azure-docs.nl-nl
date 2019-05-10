---
title: Toegang verlenen tot Azure SQL Database en SQL Data Warehouse | Microsoft Docs
description: Meer informatie over het verlenen van toegang met Microsoft Azure SQL Database en SQL Data Warehouse.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: sql-data-warehouse
ms.devlang: ''
ms.topic: conceptual
author: VanMSFT
ms.author: vanto
ms.reviewer: carlrab
manager: craigg
ms.date: 05/08/2019
ms.openlocfilehash: 783a8f0bc25717f1c2bf78a9c0d40b209a07939b
ms.sourcegitcommit: 399db0671f58c879c1a729230254f12bc4ebff59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/09/2019
ms.locfileid: "65473345"
---
# <a name="azure-sql-database-and-sql-data-warehouse-access-control"></a>Toegangsbeheer voor Azure SQL Database en SQL Data Warehouse

Voor de beveiliging, Azure [SQL-Database](sql-database-technical-overview.md) en [SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) toegangsbeheer met firewallregels connectiviteit beperken door IP-adres, verificatiemechanismen die vereisen dat gebruikers om te bewijzen dat hun identiteits- en autorisatiemechanismen worden gebruikers beperkt tot bepaalde gegevens en acties. 

> [!IMPORTANT]
> Zie [SQL security overview](sql-database-security-overview.md) (SQL-beveiligingsoverzicht) voor een overzicht van de beveiligingsfuncties van SQL Database. Zie voor een zelfstudie [beveiligen van uw Azure SQL Database](sql-database-security-tutorial.md). Zie voor een overzicht van beveiligingsfuncties van SQL Data Warehouse, [SQL Data Warehouse-beveiligingsoverzicht](../sql-data-warehouse/sql-data-warehouse-overview-manage-security.md)

## <a name="firewall-and-firewall-rules"></a>Firewall en firewallregels

Microsoft Azure SQL Database levert een relationele-databaseservice voor Azure en andere op internet gebaseerde toepassingen. Om uw gegevens te beschermen, verhinderen firewalls alle toegang tot de databaseserver totdat u opgeeft welke computers zijn gemachtigd. De firewall verleent toegang tot databases op basis van het IP-adres waar de aanvraag vandaan komt. Zie [Overzicht van de firewallregels voor SQL Database](sql-database-firewall-configure.md) voor meer informatie

De Azure SQL Database-service is alleen beschikbaar via TCP-poort 1433. Zorg voor toegang tot een SQL Database vanaf uw computer ervoor dat de firewall van uw clientcomputer uitgaande TCP-communicatie op TCP-poort 1433 toestaat. Blokkeer binnenkomende verbindingen op TCP-poort 1433 als u deze niet nodig hebt voor andere toepassingen. 

Als onderdeel van het verbindingsproces worden verbindingen van virtuele Azure-machines omgeleid naar een ander IP-adres en poort, die uniek zijn voor elke werkrol. Het poortnummer ligt in het bereik van 11000 tot 11999. Zie voor meer informatie over TCP-poorten, [poorten boven 1433 voor ADO.NET 4.5 en SQL Database2](sql-database-develop-direct-route-ports-adonet-v12.md).

## <a name="authentication"></a>Verificatie

SQL Database ondersteunt twee typen verificatie:

- **SQL-verificatie**:

  Deze verificatiemethode maakt gebruik van een gebruikersnaam en wachtwoord. Wanneer u de SQL Database-server voor uw database hebt gemaakt, kunt u een aanmelding 'serverbeheerder' opgegeven met een gebruikersnaam en wachtwoord. Met deze aanmeldingsgegevens kunt u zich bij elke database op die server als de database-eigenaar of 'dbo' verifiëren. 
- **Azure Active Directory-verificatie**:

  Deze verificatiemethode maakt gebruik van identiteiten die worden beheerd door Azure Active Directory en wordt ondersteund voor beheerde en geïntegreerde domeinen. Als u Azure Active Directory-verificatie wilt gebruiken, moet u een andere serverbeheerder maken, de 'Azure AD-beheerder' genaamd, die Azure AD-gebruikers en -groepen kan beheren. Deze beheerder kan ook alle bewerkingen uitvoeren die reguliere serverbeheerders kunnen uitvoeren. Zie [Verbinding maken met SQL Database met behulp van Azure Active Directory-verificatie](sql-database-aad-authentication.md) voor een overzicht van het maken van een Azure AD-beheerder om Azure Active Directory-verificatie in te schakelen.

De Database-engine sluit verbindingen die gedurende meer dan 30 minuten inactief zijn. De verbinding moet zich opnieuw aanmelden voordat deze kan worden gebruikt. Continu actieve verbindingen met SQL Database moeten ten minste elke 10 uur opnieuw worden geautoriseerd (uitgevoerd door de database-engine). De database-engine probeert opnieuw te autoriseren met het oorspronkelijk opgegeven wachtwoord en er is geen gebruikersinvoer vereist. Voor betere prestaties wanneer een wachtwoord opnieuw wordt ingesteld in SQL-Database, is de verbinding niet geverifieerd, zelfs als de verbinding wordt hersteld vanwege een Groepsgewijze verbinding. Dit wijkt af van het gedrag van een lokale SQL Server. Als het wachtwoord is gewijzigd sinds de verbinding de eerste keer is geautoriseerd, moet de verbinding worden beëindigd en wordt er een nieuwe verbinding gemaakt met het nieuwe wachtwoord. Een gebruiker met de `KILL DATABASE CONNECTION`-machtiging kan een verbinding met SQL Database expliciet afsluiten met behulp van de opdracht [KILL](https://docs.microsoft.com/sql/t-sql/language-elements/kill-transact-sql).

Gebruikersaccounts kunnen worden gemaakt in de hoofddatabase en kunnen machtigingen krijgen toegewezen in alle databases op de server, of ze kunnen worden gemaakt in de database zelf (dit worden ingesloten gebruikers genoemd). Zie [Aanmeldingen beheren](sql-database-manage-logins.md) voor informatie over het maken en beheren van aanmeldingen. Gebruik ingesloten databases draagbaarheid en schaalbaarheid te verbeteren. Zie [Contained Database Users - Making Your Database Portable](https://docs.microsoft.com/sql/relational-databases/security/contained-database-users-making-your-database-portable) (Ingesloten databasegebruikers: een draagbare database maken), [CREATE USER (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/create-user-transact-sql) (GEBRUIKER MAKEN (Transact-SQL)) en [Contained Databases](https://docs.microsoft.com/sql/relational-databases/databases/contained-databases) (Ingesloten databases) voor meer informatie.

Als best practice moet uw toepassing een specifiek account gebruiken om te verifiëren. Op deze manier kunt u de machtigingen die aan de toepassing worden verleend, beperken en het risico op schadelijke activiteiten verminderen in het geval uw toepassingscode kwetsbaar is voor SQL-injectieaanvallen. U wordt aanbevolen om een [ingesloten databasegebruiker](https://docs.microsoft.com/sql/relational-databases/security/contained-database-users-making-your-database-portable) te maken, zodat uw app zich rechtstreeks bij de database kan verifiëren. 

## <a name="authorization"></a>Autorisatie

Autorisatie verwijst naar wat een gebruiker kan doen binnen een Azure SQL Database. Dit wordt bepaald door de [rollidmaatschappen](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/database-level-roles) en [objectmachtigingen](https://docs.microsoft.com/sql/relational-databases/security/permissions-database-engine) voor de database van uw gebruikersaccount. Het wordt aanbevolen om gebruikers de minimaal benodigde bevoegdheden te verlenen. Het serverbeheerdersaccount waarmee u verbinding maakt is lid van db_owner, die geautoriseerd is om alle bewerkingen binnen de database uit te voeren. Gebruik dit account voor het implementeren van schema-updates en andere beheerbewerkingen. Gebruik het 'ApplicationUser'-account met beperktere machtigingen om vanuit van uw toepassing verbinding te maken met de database met de minste bevoegdheden die nodig zijn voor uw toepassing. Zie [Aanmeldingen beheren](sql-database-manage-logins.md) voor meer informatie.

Normaal gesproken hebben alleen beheerders toegang tot de `master`database nodig. Routinematige toegang tot elke gebruikersdatabase moet verlopen via ingesloten databasegebruikers die geen beheerder zijn die in elke database worden gemaakt. Wanneer u ingesloten databasegebruikers gebruikt, hoeft u geen aanmeldingen te maken in de `master`database. Zie [Ingesloten databasegebruikers: een draagbare database maken](https://docs.microsoft.com/sql/relational-databases/security/contained-database-users-making-your-database-portable) voor meer informatie.

Zorg ervoor dat u de volgende functies kunt gebruiken voor het beperken of het verhogen van machtigingen:

- [Imitatie](https://docs.microsoft.com/dotnet/framework/data/adonet/sql/customizing-permissions-with-impersonation-in-sql-server) en [module-ondertekening](https://docs.microsoft.com/dotnet/framework/data/adonet/sql/signing-stored-procedures-in-sql-server) kunnen worden gebruikt om machtigingen tijdelijk veilig te verhogen.
- [Beveiliging op rijniveau](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) kan worden gebruikt om de rijen waartoe een gebruiker toegang heeft te beperken.
- [Gegevensmaskering](sql-database-dynamic-data-masking-get-started.md) kan worden gebruikt om de weergave van gevoelige gegevens te beperken.
- [Opgeslagen procedures](https://docs.microsoft.com/sql/relational-databases/stored-procedures/stored-procedures-database-engine) kunnen worden gebruikt om de acties die op de database kunnen worden uitgevoerd te beperken.

## <a name="next-steps"></a>Volgende stappen

- Zie [SQL security overview](sql-database-security-overview.md) (SQL-beveiligingsoverzicht) voor een overzicht van de beveiligingsfuncties van SQL Database.
- Zie voor meer informatie over firewall-regels, [Firewall-regels](sql-database-firewall-configure.md).
- Zie [Aanmeldingen beheren](sql-database-manage-logins.md) voor meer informatie over gebruikers en aanmeldingen. 
- Zie voor een discussie over proactieve controle [Databasecontrole](sql-database-auditing.md) en [SQL Database threat detection](sql-database-threat-detection.md).
- Zie voor een zelfstudie [beveiligen van uw Azure SQL Database](sql-database-security-tutorial.md).
