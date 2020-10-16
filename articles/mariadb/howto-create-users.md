---
title: Gebruikers maken-Azure Database for MariaDB
description: In dit artikel wordt beschreven hoe u nieuwe gebruikers accounts kunt maken om te communiceren met een Azure Database for MariaDB-server.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: how-to
ms.date: 10/1/2020
ms.openlocfilehash: 7a3515850027963dcabbf2cef4efd8bf43802cbe
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "91653057"
---
# <a name="create-users-in-azure-database-for-mariadb"></a>Create users in Azure Database for MariaDB (Gebruikers maken in Azure Database for MariaDB) 
In dit artikel wordt beschreven hoe u gebruikers kunt maken in Azure Database for MariaDB.

> [!NOTE]
> Afwijking-vrije communicatie
>
> Micro soft biedt ondersteuning voor een gevarieerde en inbegrips omgeving. Dit artikel bevat verwijzingen naar het woord _Slave_. De micro soft- [stijl gids voor beschik bare communicatie](https://github.com/MicrosoftDocs/microsoft-style-guide/blob/master/styleguide/bias-free-communication.md) herkent deze als een uitsluitend woord. Het woord wordt in dit artikel gebruikt voor consistentie omdat het momenteel het woord is dat wordt weer gegeven in de software. Wanneer de software is bijgewerkt om het woord te verwijderen, wordt dit artikel zodanig bijgewerkt dat het in uitlijning is.
>

Wanneer u uw Azure Database for MariaDB voor het eerst hebt gemaakt, hebt u de gebruikers naam en het wacht woord van de server beheerder aangemeld. Voor meer informatie kunt u de [Snelstartgids](quickstart-create-mariadb-server-database-using-azure-portal.md)volgen. U kunt de gebruikers naam van de server beheerder aanmelden via de Azure Portal.

De gebruiker van de server beheerder heeft bepaalde bevoegdheden voor uw server als vermeld: selecteren, invoegen, bijwerken, verwijderen, maken, verwijderen, opnieuw laden, verwerken, verwijzingen, INDEX, wijzigen, data BASEs, tijdelijke tabellen maken, tabellen vergren delen, uitvoeren, replicatie slave, replicatie CLIENT, weer gave, gebruiker weer geven, gebeurtenis, TRIGGER

Zodra de Azure Database for MariaDB-server is gemaakt, kunt u het eerste gebruikers account van de server beheerder gebruiken om extra gebruikers te maken en beheerders toegang te verlenen. Het account voor de server beheerder kan ook worden gebruikt om gebruikers met minder bevoegdheden te maken die toegang hebben tot afzonderlijke database schema's.

> [!NOTE]
> De rol SUPER privilege en DBA worden niet ondersteund. Lees de [bevoegdheden](concepts-limits.md#privileges--data-manipulation-support) in het artikel beperkingen om te begrijpen wat er niet wordt ondersteund in de service.<br><br>
> De invoeg toepassingen voor wacht woorden, zoals ' validate_password ' en ' caching_sha2_password ', worden niet ondersteund door de service.

## <a name="create-additional-admin-users"></a>Extra gebruikers met beheerders rechten maken
1. De verbindings gegevens en de gebruikers naam van de beheerder ophalen.
   Voor verbinding met uw databaseserver moet u beschikken over de volledige servernaam en aanmeldingsreferenties van de beheerder. U kunt eenvoudig de server naam en aanmeldings gegevens vinden op de pagina **overzicht** van de server of op de pagina **eigenschappen** in de Azure Portal. 

2. Gebruik het beheerders account en-wacht woord om verbinding te maken met uw database server. Gebruik het client hulpprogramma van uw voor keur, zoals MySQL Workbench, mysql.exe, HeidiSQL of anderen. 
   Als u niet zeker weet hoe u verbinding kunt maken, raadpleegt u [MySQL Workbench gebruiken om verbinding te maken en gegevens op te vragen](./connect-workbench.md)

3. Bewerk de volgende SQL-code en voer deze uit. Vervang de nieuwe gebruikers naam voor de waarde van de tijdelijke aanduiding `new_master_user` . Met deze syntaxis worden de vermelde bevoegdheden voor alle database schema's (*.*) verleend aan de gebruikers naam (new_master_user in dit voor beeld). 

   ```sql
   CREATE USER 'new_master_user'@'%' IDENTIFIED BY 'StrongPassword!';
   
   GRANT SELECT, INSERT, UPDATE, DELETE, CREATE, DROP, RELOAD, PROCESS, REFERENCES, INDEX, ALTER, SHOW DATABASES, CREATE TEMPORARY TABLES, LOCK TABLES, EXECUTE, REPLICATION SLAVE, REPLICATION CLIENT, CREATE VIEW, SHOW VIEW, CREATE ROUTINE, ALTER ROUTINE, CREATE USER, EVENT, TRIGGER ON *.* TO 'new_master_user'@'%' WITH GRANT OPTION; 
   
   FLUSH PRIVILEGES;
   ```

4. De subsidies verifiëren 
   ```sql
   USE sys;
   
   SHOW GRANTS FOR 'new_master_user'@'%';
   ```

## <a name="create-database-users"></a>Databasegebruikers maken

1. De verbindings gegevens en de gebruikers naam van de beheerder ophalen.
   Voor verbinding met uw databaseserver moet u beschikken over de volledige servernaam en aanmeldingsreferenties van de beheerder. U kunt eenvoudig de server naam en aanmeldings gegevens vinden op de pagina **overzicht** van de server of op de pagina **eigenschappen** in de Azure Portal. 

2. Gebruik het beheerders account en-wacht woord om verbinding te maken met uw database server. Gebruik het client hulpprogramma van uw voor keur, zoals MySQL Workbench, mysql.exe, HeidiSQL of anderen. 
   Als u niet zeker weet hoe u verbinding kunt maken, raadpleegt u [MySQL Workbench gebruiken om verbinding te maken en gegevens op te vragen](./connect-workbench.md)

3. Bewerk de volgende SQL-code en voer deze uit. Vervang de waarde van de tijdelijke aanduiding door `db_user` de gewenste nieuwe gebruikers naam en de tijdelijke aanduiding voor de `testdb` naam van uw eigen data base.

   Deze SQL-code syntaxis maakt bijvoorbeeld een nieuwe Data Base met de naam testdb. Vervolgens wordt er een nieuwe gebruiker in de Azure Database for MariaDB-service gemaakt en worden alle bevoegdheden verleend aan het nieuwe database schema (testdb. \* ) voor die gebruiker. 

   ```sql
   CREATE DATABASE testdb;
   
   CREATE USER 'db_user'@'%' IDENTIFIED BY 'StrongPassword!';
   
   GRANT ALL PRIVILEGES ON testdb . * TO 'db_user'@'%';
   
   FLUSH PRIVILEGES;
   ```

4. Controleer de subsidies in de data base.
   ```sql
   USE testdb;
   
   SHOW GRANTS FOR 'db_user'@'%';
   ```

5. Meld u aan bij de server, waarbij u de aangewezen data base opgeeft, met de nieuwe gebruikers naam en wacht woord. In dit voor beeld wordt de MySQL-opdracht regel weer gegeven. Met deze opdracht wordt u gevraagd om het wacht woord voor de gebruikers naam. Vervang uw eigen server naam, database naam en gebruikers naam.

   ```bash
   mysql --host mydemoserver.mariadb.database.azure.com --database testdb --user db_user@mydemoserver -p
   ```
   Zie MariaDB-documentatie voor Gebruikersaccountbeheer, [syntaxis](https://mariadb.com/kb/en/library/grant/)voor [gebruikers](https://mariadb.com/kb/en/library/user-account-management/)en [bevoegdheden](https://mariadb.com/kb/en/library/grant/#privilege-levels)voor meer informatie over het beheer van gebruikers accounts.

## <a name="azure_superuser"></a>azure_superuser

Alle Azure Database for MySQL-servers worden gemaakt met een gebruiker met de naam ' azure_superuser '. Dit is een systeem account dat door micro soft is gemaakt voor het beheren van de server voor het uitvoeren van bewaking, back-ups en andere regel matig onderhoud. Engineers van de oproep kunnen dit account ook gebruiken om toegang te krijgen tot de server tijdens een incident met certificaat verificatie en moeten toegang aanvragen met Just-in-time (JIT)-processen.

## <a name="next-steps"></a>Volgende stappen
Open de firewall voor de IP-adressen van de computers van de nieuwe gebruikers zodat ze verbinding kunnen maken: [Azure database for MariaDB firewall regels maken en beheren met behulp van de Azure Portal](howto-manage-firewall-portal.md)  

<!--or [Azure CLI](howto-manage-firewall-using-cli.md).-->
