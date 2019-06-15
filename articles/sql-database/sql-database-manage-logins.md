---
title: Azure SQL-aanmeldingen en -gebruikers | Microsoft Docs
description: Meer informatie over SQL-Database en SQL Data Warehouse-beveiligingsbeheer, informatie over het beheer van de database en de aanmeldingsbeveiliging beveiliging via het hoofdaccount op serverniveau.
keywords: sql-databasebeveiliging,beheer databasebeveiliging,aanmeldingsbeveiliging,databasebeveiliging,databasetoegang
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: VanMSFT
ms.author: vanto
ms.reviewer: carlrab
manager: craigg
ms.date: 03/26/2019
ms.openlocfilehash: af6cec22ae455e6a6ead4c45fead2d7ff5b708d2
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67070512"
---
# <a name="controlling-and-granting-database-access-to-sql-database-and-sql-data-warehouse"></a>Beheren en het verlenen van toegang tot de database met SQL Database en SQL Data Warehouse

Na de configuratie van firewall-regels, kunt u verbinding met Azure [SQL-Database](sql-database-technical-overview.md) en [SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) als een van de beheerdersaccounts, als de database-eigenaar of als databasegebruiker in de database.  

> [!NOTE]  
> In dit onderwerp is van toepassing op Azure SQL-server en SQL-Database en SQL Data Warehouse-databases op de Azure SQL-server gemaakt. Voor het gemak wordt de term 'SQL Database' gebruikt wanneer er wordt verwezen naar zowel SQL Database als SQL Data Warehouse. 
> [!TIP]
> Zie voor een zelfstudie [beveiligen van uw Azure SQL Database](sql-database-security-tutorial.md). In deze zelfstudie geldt niet voor **Azure SQL Database Managed Instance**.

## <a name="unrestricted-administrative-accounts"></a>Onbeperkte beheerdersaccounts

Er zijn twee beheerdersaccounts (**serverbeheerder** en **Active Directory-beheerder**) die als beheerder fungeren. Voor het identificeren van deze beheerdersaccounts voor uw SQL-server, opent u Azure portal en navigeer naar het tabblad Eigenschappen van uw SQL server of SQL-Database.

![SQL Server-beheerders](media/sql-database-manage-logins/sql-admins.png)

- **Serverbeheerder**

  Wanneer u een Azure SQL-server maakt, moet u de **aanmeldgegevens van de serverbeheerder** opgeven. De SQL-server maakt het account vervolgens als een aanmelding in de hoofddatabase. Dit account maakt verbinding met behulp van SQL Server-verificatie (gebruikersnaam en wachtwoord). Er kan slechts één van deze accounts bestaan.

  > [!NOTE]
  > Als u het wachtwoord voor de serverbeheerder herstellen, gaat u naar de [Azure-portal](https://portal.azure.com), klikt u op **SQL-Servers**, selecteert u de server in de lijst en klik vervolgens op **wachtwoord opnieuw instellen**.

- **Azure Active Directory-beheerder**

  Ook één Azure Active Directory-account (een afzonderlijk account of het account van een beveiligingsgroep) kan als beheerder worden geconfigureerd. Dit is optioneel het configureren van een Azure AD-beheerder echter een Azure AD-beheerder **moet** worden geconfigureerd als u wilt gebruiken van Azure AD-accounts verbinding maken met SQL-Database. Voor meer informatie over het configureren van toegang tot Azure Active Directory raadpleegt u [Verbinding maken met SQL Database of SQL Data Warehouse met behulp van Azure Active Directory-verificatie](sql-database-aad-authentication.md) en [SSMS-ondersteuning voor Azure AD MFA met SQL Database en SQL Data Warehouse](sql-database-ssms-mfa-authentication.md).

De accounts van de **serverbeheerder** en de **Azure AD-beheerder** hebben de volgende kenmerken:

- Zijn de enige accounts die automatisch verbinding met elke SQL-Database op de server maken kunnen. (Andere accounts die verbinding willen maken met een gebruikersdatabase, moeten eigenaar van de database zijn of een gebruikersaccount in de database hebben.)
- Deze accounts worden in gebruikersdatabases beschouwd als de `dbo`-gebruiker en beschikken over alle machtigingen. (De eigenaar van een gebruikersdatabase wordt in de database ook als `dbo`-gebruiker beschouwd.) 
- Voer niet de `master` database als de `dbo` gebruiker, en hebben beperkte machtigingen in master. 
- Zijn **niet** leden van de standaard SQL Server `sysadmin` vaste serverrol die niet beschikbaar in SQL-database.  
- Kan maken, wijzigen en verwijderen van databases, aanmeldingen en gebruikers in de hoofddatabase, en op serverniveau IP-firewallregels.
- Kunnen toevoegen en verwijderen van leden aan de `dbmanager` en `loginmanager` rollen.
- Vindt de `sys.sql_logins` systeemtabel voorkomt.

### <a name="configuring-the-firewall"></a>De firewall configureren

Als de firewall op serverniveau is geconfigureerd voor een afzonderlijk IP-adres of -bereik, kunnen de **SQL-serverbeheerder** en de **Azure Active Directory-beheerder** verbinding maken met de hoofddatabase en alle gebruikersdatabases. De eerste firewall op serverniveau kan worden geconfigureerd via [Azure Portal](sql-database-single-database-get-started.md) met behulp van [PowerShell](sql-database-powershell-samples.md) of de [REST-API](https://msdn.microsoft.com/library/azure/dn505712.aspx). Zodra een verbinding wordt gemaakt, extra op serverniveau IP-firewall-regels kunnen worden geconfigureerd met behulp [Transact-SQL](sql-database-configure-firewall-settings.md).

### <a name="administrator-access-path"></a>Toegangspad beheerder

Als de firewall op serverniveau correct is geconfigureerd, kunnen de **SQL-serverbeheerder** en de **Azure Active Directory-beheerder** verbinding maken met clienthulpprogramma's zoals SQL Server Management Studio en SQL Server Data Tools. Alleen de nieuwste hulpprogramma's bieden alle functies en mogelijkheden. Het volgende diagram toont een standaardconfiguratie voor de twee beheerdersaccounts.

![configuratie van de twee beheer-accounts](./media/sql-database-manage-logins/1sql-db-administrator-access.png)

Wanneer u een open poort in de firewall op serverniveau gebruikt, kunnen beheerders verbinding maken met elke SQL-database.

### <a name="connecting-to-a-database-by-using-sql-server-management-studio"></a>Verbinding maken met een database via SQL Server Management Studio

Zie voor stapsgewijze instructies voor het maken van een server, een database, de IP-firewallregels op serverniveau en het gebruik van SQL Server Management Studio om op te vragen van een database, [aan de slag met Azure SQL Database-servers, databases en firewallregels met behulp van Azure portal en SQL Server Management Studio](sql-database-single-database-get-started.md).

> [!IMPORTANT]
> Het wordt aanbevolen om altijd de nieuwste versie van Management Studio te gebruiken, zodat uw versie gesynchroniseerd blijft met updates voor Microsoft Azure en SQL Database. [SQL Server Management Studio bijwerken](https://msdn.microsoft.com/library/mt238290.aspx).

## <a name="additional-server-level-administrative-roles"></a>Aanvullende beheerdersrollen op serverniveau

>[!IMPORTANT]
>In deze sectie geldt niet voor **Azure SQL Database Managed Instance** als deze rollen zijn specifiek voor **Azure SQL Database**.

Naast de beheerdersrollen op serverniveau die eerder zijn besproken, biedt SQL Database ook twee beperkte beheerdersrollen in de hoofddatabase waaraan gebruikersaccounts kunnen worden toegevoegd. Deze twee beheerdersrollen verlenen machtigingen voor het maken van databases of voor het beheren van aanmeldingen.

### <a name="database-creators"></a>Databasemakers

Een van deze beheerdersrollen is de rol **dbmanager**. Leden van deze rol kunnen nieuwe databases maken. Voor het gebruik van deze rol maakt u een gebruiker in de `master`-database en voegt u deze gebruiker vervolgens toe aan de databaserol **dbmanager**. Voor het maken van een database, moet de gebruiker een gebruiker op basis van een SQL Server-aanmelding in de `master` database of een ingesloten databasegebruiker op basis van een Azure Active Directory-gebruiker.

1. Verbinding met een administrator-account maken met de `master` database.
2. Maak een aanmelding SQL Server-verificatie met behulp van de [CREATE LOGIN](https://msdn.microsoft.com/library/ms189751.aspx) instructie. Voorbeeldinstructie:

   ```sql
   CREATE LOGIN Mary WITH PASSWORD = '<strong_password>';
   ```

   > [!NOTE]
   > Gebruik een sterk wachtwoord bij het maken van aanmeldgegevens of een ingesloten databasegebruiker. Zie [Sterke wachtwoorden](https://msdn.microsoft.com/library/ms161962.aspx) voor meer informatie.

   Voor betere prestaties worden aanmeldingen (principals op serverniveau) tijdelijk in het cachegeheugen op databaseniveau opgeslagen. Zie [DBCC FLUSHAUTHCACHE](https://msdn.microsoft.com/library/mt627793.aspx) als u de verificatiecache wilt vernieuwen.

3. In de `master` database, het maken van een gebruiker met behulp van de [CREATE USER](https://msdn.microsoft.com/library/ms173463.aspx) instructie. De gebruiker kan een ingesloten databasegebruiker op basis van Azure Active Directory-verificatie zijn (als u uw omgeving hebt geconfigureerd voor Azure AD-verificatie), maar ook een ingesloten databasegebruiker op basis van SQL Server-verificatie of een gebruiker op basis van SQL Server-verificatie met aanmelding voor SQL Server-verificatie (gemaakt in de vorige stap). Voorbeeldinstructies:

   ```sql
   CREATE USER [mike@contoso.com] FROM EXTERNAL PROVIDER; -- To create a user with Azure Active Directory
   CREATE USER Ann WITH PASSWORD = '<strong_password>'; -- To create a SQL Database contained database user
   CREATE USER Mary FROM LOGIN Mary;  -- To create a SQL Server user based on a SQL Server authentication login
   ```

4. De nieuwe gebruiker toevoegen de **dbmanager** databaserol in `master` met behulp van de [ALTER ROLE](https://msdn.microsoft.com/library/ms189775.aspx) instructie. Voorbeeldinstructies:

   ```sql
   ALTER ROLE dbmanager ADD MEMBER Mary; 
   ALTER ROLE dbmanager ADD MEMBER [mike@contoso.com];
   ```

   > [!NOTE]
   > Dbmanager is een databaserol in de hoofddatabase, zodat u alleen een databasegebruiker aan de rol dbmanager kunt toevoegen. U kunt geen aanmelding op serverniveau toevoegen aan een rol op databaseniveau .

5. U kunt zo nodig een firewallregel configureren, zodat de nieuwe gebruiker verbinding kan maken. (De nieuwe gebruiker kan worden gedekt door een bestaande firewallregel.)

Nu de gebruiker verbinding met maken kan de `master` database en kunnen nieuwe databases maken. Het account dat de database maakt, wordt eigenaar van de database.

### <a name="login-managers"></a>Aanmelding managers

De andere beheerdersrol is de rol voor aanmeldingsbeheerder. Leden van deze rol kunnen nieuwe aanmeldingen maken in de hoofddatabase. Desgewenst kunt u dezelfde stappen doorlopen (een aanmelding maken, een gebruiker maken en een gebruiker toevoegen aan de rol **loginmanager**), zodat een gebruiker nieuwe aanmeldingen kan maken in de hoofddatabase. Gewoonlijk zijn aanmeldingen niet nodig, omdat Microsoft het gebruik aanbeveelt van gebruikers van ingesloten databases. Hiervoor wordt verificatie op databaseniveau gebruikt, in plaats van gebruik te maken van gebruikers op basis van aanmelding. Zie [Ingesloten databasegebruikers: een draagbare database maken](https://msdn.microsoft.com/library/ff929188.aspx) voor meer informatie.

## <a name="non-administrator-users"></a>Niet-beheerders

Niet-beheerdersaccounts hebben doorgaans geen toegang nodig tot de hoofddatabase. Maak ingesloten databasegebruikers op databaseniveau met de instructie [CREATE USER (Transact-SQL)](https://msdn.microsoft.com/library/ms173463.aspx). De gebruiker kan een ingesloten databasegebruiker op basis van Azure Active Directory-verificatie zijn (als u uw omgeving hebt geconfigureerd voor Azure AD-verificatie), maar ook een ingesloten databasegebruiker op basis van SQL Server-verificatie of een gebruiker op basis van SQL Server-verificatie met aanmelding voor SQL Server-verificatie (gemaakt in de vorige stap). Zie [Ingesloten databasegebruikers: een draagbare database maken](https://msdn.microsoft.com/library/ff929188.aspx) voor meer informatie. 

Als u gebruikers wilt maken, maakt u verbinding met de database en voert u de instructies uit die in de volgende voorbeelden worden getoond:

```sql
CREATE USER Mary FROM LOGIN Mary; 
CREATE USER [mike@contoso.com] FROM EXTERNAL PROVIDER;
```

In eerste instantie kan slechts een van de beheerders of de eigenaar van de database gebruikers maken. Om extra gebruikers te machtigen voor het maken van nieuwe gebruikers, verleent u de geselecteerde gebruikers de `ALTER ANY USER`-toestemming met behulp van een instructie zoals:

```sql
GRANT ALTER ANY USER TO Mary;
```

Als u wilt geven extra gebruikers volledig beheer van de database, zodat ze lid zijn van de **db_owner** vaste databaserol.

In Azure SQL Database met de `ALTER ROLE` instructie.

```sql
ALTER ROLE db_owner ADD MEMBER Mary;
```

In Azure SQL Data Warehouse gebruik [EXEC sp_addrolemember](/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql).
```sql
EXEC sp_addrolemember 'db_owner', 'Mary';
```


> [!NOTE]
> Een veelvoorkomende reden waarom het maken van een databasegebruiker op basis van een SQL Database server-aanmelding is voor gebruikers die toegang tot meerdere databases nodig hebben. Omdat ingesloten databasegebruikers zijn individuele entiteiten, elke database houdt een eigen gebruikersnaam en het eigen wachtwoord. Hierdoor kan overhead als de gebruiker moet vervolgens elk wachtwoord voor elke database op en het untenable worden kan bij een meerdere wachtwoorden voor veel databases te wijzigen. Echter, wanneer u SQL Server-aanmeldingen en hoge beschikbaarheid (actieve geo-replicatie en failover-groepen), de SQL Server-aanmeldingen moeten worden ingesteld handmatig op elke server. Anders wordt worden de gebruiker van de database niet meer toegewezen aan de aanmelding op serverniveau na een failover optreedt, en pas weer toegang tot de database na failover. Zie voor meer informatie over het configureren van aanmeldingen voor geo-replicatie [configureren en beheren van Azure SQL Database-beveiliging voor geo-herstel en failovers](sql-database-geo-replication-security-config.md).

### <a name="configuring-the-database-level-firewall"></a>De firewall op databaseniveau configureren

U doet er verstandig aan niet-beheerders alleen via de firewall toegang te verlenen tot de databases die ze gebruiken. In plaats van het machtigen van hun IP-adressen via de firewall op serverniveau en hun toegang te verlenen tot alle databases, kunt u de instructie [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) gebruiken om de firewall op databaseniveau te configureren. De firewall op databaseniveau kan niet worden geconfigureerd met behulp van de portal.

### <a name="non-administrator-access-path"></a>Toegangspad niet-beheerder

Wanneer de firewall op databaseniveau correct is geconfigureerd, kunnen databasegebruikers verbinding maken met de hulp van clienthulpprogramma's zoals SQL Server Management Studio of SQL Server Data Tools. Alleen de nieuwste hulpprogramma's bieden alle functies en mogelijkheden. Het volgende diagram toont een standaardtoegangspad voor niet-beheerders.

![Toegangspad niet-beheerder](./media/sql-database-manage-logins/2sql-db-nonadmin-access.png)

## <a name="groups-and-roles"></a>Groepen en rollen

Voor efficiënt toegangsbeheer gebruikt u machtigingen die zijn toegewezen aan groepen en rollen in plaats van aan individuele gebruikers. 

- Als u Azure Active Directory-verificatie gebruikt, plaatst u Azure Active Directory-gebruikers in een Azure Active Directory-groep. Maak voor de groep een ingesloten databasegebruiker. Plaats een of meer databasegebruikers in een [databaserol](https://msdn.microsoft.com/library/ms189121) en wijs vervolgens [machtigingen](https://msdn.microsoft.com/library/ms191291.aspx) toe aan de databaserol.

- Als u SQL Server-verificatie gebruikt, maakt u gebruikers van ingesloten databases in de database. Plaats een of meer databasegebruikers in een [databaserol](https://msdn.microsoft.com/library/ms189121) en wijs vervolgens [machtigingen](https://msdn.microsoft.com/library/ms191291.aspx) toe aan de databaserol.

Bij de databaserollen kan het gaan om de ingebouwde rollen als **db_owner**, **db_ddladmin**, **db_datawriter**, **db_datareader**, **db_denydatawriter** en **db_denydatareader**. **db_owner** wordt doorgaans gebruikt voor het verlenen van volledige machtigingen aan slechts enkele gebruikers. De andere vaste databaserollen zijn handig voor het snel verkrijgen van een eenvoudige database voor ontwikkeldoeleinden, maar worden niet aanbevolen voor de meeste productiedatabases. De vaste databaserol **db_datareader** verleent bijvoorbeeld leestoegang tot alle tabellen in de database, wat doorgaans meer is dan strikt noodzakelijk. Het is veel beter de instructie [CREATE ROLE](https://msdn.microsoft.com/library/ms187936.aspx) te gebruiken om uw eigen gebruikergedefinieerde databaserollen te maken en zorgvuldig elke rol de minimale machtigingen te verlenen die nodig zijn voor de gerelateerde zakelijke behoeften. Als een gebruiker lid is van meerdere rollen, worden de machtigingen van alle rollen samengevoegd.

## <a name="permissions"></a>Machtigingen

Er zijn meer dan 100 machtigingen die afzonderlijk kunnen worden verleend of geweigerd in SQL Database. Veel van deze machtigingen zijn genest. De machtiging `UPDATE` voor een schema bevat bijvoorbeeld de machtiging `UPDATE` voor elke tabel binnen dat schema. Net als bij de meeste machtigingssystemen gaat de weigering van een machtiging vóór toestemming. Vanwege de geneste aard en het aantal machtigingen kan een nauwkeurig onderzoek nodig zijn om een geschikt machtigingssysteem te ontwerpen voor een goede bescherming van uw database. Start met de lijst van machtigingen in [Machtigingen (Database-engine)](https://docs.microsoft.com/sql/relational-databases/security/permissions-database-engine) en controleer de [afbeelding op postergrootte](https://docs.microsoft.com/sql/relational-databases/security/media/database-engine-permissions.png) van de machtigingen.


### <a name="considerations-and-restrictions"></a>Overwegingen en beperkingen

Bij het beheren van aanmeldingen en gebruikers in SQL Database, moet u het volgende overwegen:

- U moet zijn verbonden met de **hoofd**database bij het uitvoeren van de `CREATE/ALTER/DROP DATABASE`-instructies.   
- De databasegebruiker die overeenkomt met de aanmelding van de **Serverbeheerder**, kan niet worden gewijzigd of verwijderd. 
- Amerikaans Engels is de standaardtaal van de aanmelding van de **serverbeheerder**.
- Alleen de beheerders (aanmelding van **serverbeheerder** of Azure AD-beheerder) en de leden van de databaserol **dbmanager** in de **hoofddatabase** zijn gemachtigd om de instructies `CREATE DATABASE` en `DROP DATABASE` uit te voeren.
- U moet zijn verbonden met de hoofddatabase bij het uitvoeren van de `CREATE/ALTER/DROP LOGIN`-instructies. Het gebruik van aanmeldingen wordt echter afgeraden. Gebruik in plaats daarvan ingesloten databasegebruikers.
- Om verbinding te maken met een gebruikersdatabase, moet u de naam van de database in de verbindingsreeks opgeven.
- Alleen de principal-aanmelding op serverniveau en de leden van de databaserol **loginmanager** in de **hoofd**database zijn gemachtigd om `CREATE LOGIN`-, `ALTER LOGIN`- en `DROP LOGIN`-instructies uit te voeren.
- Bij het uitvoeren van de `CREATE/ALTER/DROP LOGIN`- en `CREATE/ALTER/DROP DATABASE`-instructies in een ADO.NET-toepassing is het gebruik van geparametriseerde opdrachten is niet toegestaan. Zie [Opdrachten en parameters](https://msdn.microsoft.com/library/ms254953.aspx) voor meer informatie.
- Bij het uitvoeren van de `CREATE/ALTER/DROP DATABASE`- en `CREATE/ALTER/DROP LOGIN`-instructies moet elk van deze instructies de enige instructie in een Transact-SQL-batch zijn. Als deze niet overeenkomen, treedt er een fout op. De volgende Transact-SQL controleert bijvoorbeeld of de database bestaat. Als deze bestaat, wordt een `DROP DATABASE`-instructie aangeroepen om de database te verwijderen. Omdat de `DROP DATABASE`-instructie niet de enige instructie in de batch is, leidt het uitvoeren van de volgende Transact-SQL-instructie tot een fout.

  ```sql
  IF EXISTS (SELECT [name]
           FROM   [sys].[databases]
           WHERE  [name] = N'database_name')
  DROP DATABASE [database_name];
  GO
  ```
  
  Gebruik in plaats daarvan de volgende Transact-SQL-instructie:
  
  ```sql
  DROP DATABASE IF EXISTS [database_name]
  ```

- Bij het uitvoeren van de `CREATE USER`-instructie met de optie `FOR/FROM LOGIN` moet deze de enige instructie in een Transact-SQL-batch zijn.
- Bij het uitvoeren van de `ALTER USER`-instructie met de optie `WITH LOGIN` moet deze de enige instructie in een Transact-SQL-batch zijn.
- Voor `CREATE/ALTER/DROP` heeft een gebruiker de `ALTER ANY USER`-machtiging voor de database nodig.
- Wanneer de eigenaar van een databaserol probeert toevoegen of verwijderen van een andere databasegebruiker toe uit die databaserol, kan de volgende fout optreden: **Gebruiker of rol 'Naam' bestaat niet in deze database.** Deze fout treedt op omdat de gebruiker niet zichtbaar is voor de eigenaar. Om dit probleem op te lossen, verleent u de roleigenaar de `VIEW DEFINITION`-machtiging voor de gebruiker. 


## <a name="next-steps"></a>Volgende stappen

- Zie [Azure SQL Database-firewall](sql-database-firewall-configure.md) voor meer informatie over firewallregels.
- Zie [SQL security overview](sql-database-security-overview.md) (SQL-beveiligingsoverzicht) voor een overzicht van alle beveiligingsfuncties van SQL Database.
- Zie voor een zelfstudie [beveiligen van uw Azure SQL Database](sql-database-security-tutorial.md).
- Zie [Weergaven en opgeslagen procedures maken](https://msdn.microsoft.com/library/ms365311.aspx) voor meer informatie over weergaven en opgeslagen procedures.
- Zie [Toegang verlenen tot een databaseobject](https://msdn.microsoft.com/library/ms365327.aspx) voor meer informatie over het verlenen van toegang tot een databaseobject.
