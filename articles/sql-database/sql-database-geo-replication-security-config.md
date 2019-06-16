---
title: Configureren van Azure SQL Database-beveiliging voor herstel na noodgevallen | Microsoft Docs
description: Meer informatie over de beveiligingsoverwegingen voor het configureren en beheren van de beveiliging na het terugzetten van een database of een failover naar een secundaire server.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab
manager: craigg
ms.date: 12/18/2018
ms.openlocfilehash: 8a2a2fffa9ed3a4dae3c0768291b7585be4bfc6d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64690838"
---
# <a name="configure-and-manage-azure-sql-database-security-for-geo-restore-or-failover"></a>Configureren en beheren van Azure SQL Database-beveiliging voor geografisch herstel en failover

In dit artikel beschrijft de verificatievereisten voor het configureren en beheren van [actieve geo-replicatie](sql-database-active-geo-replication.md) en [automatische failovergroepen](sql-database-auto-failover-group.md). Het biedt ook de stappen die nodig zijn voor het instellen van de gebruikerstoegang tot de secundaire database. Ten slotte wordt ook beschreven hoe u nadat u hebt gebruikt voor toegang tot de herstelde database [geo-herstel](sql-database-recovery-using-backups.md#geo-restore). Zie voor meer informatie over opties voor Systeemherstel [overzicht voor zakelijke continuïteit](sql-database-business-continuity.md).

## <a name="disaster-recovery-with-contained-users"></a>Herstel na noodgeval met ingesloten gebruikers

In tegenstelling tot traditionele gebruikers, die moeten worden toegewezen aan aanmeldingen in de database master, een contained-gebruiker volledig beheerd door de database zelf. Dit heeft twee voordelen. De gebruikers kunnen blijven verbinding maken met de nieuwe primaire database of de database hersteld met behulp van geo-herstel zonder extra configuratie, omdat de database de gebruikers beheert in de noodherstelscenario. Er zijn ook potentiële schaalbaarheids- en prestatievoordelen van deze configuratie vanuit het oogpunt van de aanmelding. Zie [Ingesloten databasegebruikers: een draagbare database maken](https://msdn.microsoft.com/library/ff929188.aspx) voor meer informatie.

De belangrijkste afweging is dat het proces van het herstel na noodgevallen op schaal beheren moeilijker. Wanneer er meerdere databases met dezelfde aanmeldingsnaam gebruiken, onderhouden van de referenties in meerdere databases met behulp van ingesloten gebruikers mogelijk negatief maken de voordelen van ingesloten gebruikers. Bijvoorbeeld: het wachtwoordbeleid van de rotatie heeft wijzigingen consequent worden aangebracht in meerdere databases in plaats van het wijzigen van het wachtwoord voor de aanmelding eenmaal in de database master. Om deze reden, hebt u meerdere databases die gebruikmaken van dezelfde gebruikersnaam en wachtwoord, wordt met behulp van ingesloten gebruikers niet aanbevolen.

## <a name="how-to-configure-logins-and-users"></a>Het configureren van aanmeldingen en gebruikers

Als u aanmeldingen en gebruikers (in plaats van ingesloten gebruikers), moet u extra stappen om ervoor te zorgen dat de dezelfde aanmeldingen aanwezig zijn in de database master uitvoeren. De volgende secties beschrijven de stappen die betrokken zijn en aanvullende overwegingen.

  >[!NOTE]
  > Het is ook mogelijk met gebruik van Azure Active Directory (AAD)-aanmeldingen voor het beheren van uw databases. Zie voor meer informatie, [Azure SQL-aanmeldingen en gebruikers](https://docs.microsoft.com/azure/sql-database/sql-database-manage-logins).

### <a name="set-up-user-access-to-a-secondary-or-recovered-database"></a>Gebruikerstoegang tot een secundaire of herstelde database instellen

In de volgorde voor de secundaire database kan worden gebruikt als een secundaire database alleen-lezen en om te controleren of de juiste toegang tot de nieuwe primaire database of de database hersteld met behulp van geo-restore, moet de database master van de doelserver de juiste beveiliging hebben. configuratie aanwezig zijn vóór het herstel.

De specifieke machtigingen voor elke stap worden verderop in dit onderwerp beschreven.

Voorbereiden van de gebruikerstoegang tot een secundaire geo-replicatie moet worden uitgevoerd als onderdeel configureert van geo-replicatie. Voorbereiden van de gebruikerstoegang tot de databases geografisch herstellen moet worden uitgevoerd op elk gewenst moment wanneer de oorspronkelijke server online is (bijvoorbeeld als onderdeel van de DR-oefening).

> [!NOTE]
> Als u een failover of geo-herstel naar een server waarop geen correct geconfigureerde aanmeldingen, toegang tot deze beperkt tot het serverbeheerdersaccount worden.

Instellen van aanmeldingen op de doelserver bestaat uit drie stappen worden hieronder beschreven:

#### <a name="1-determine-logins-with-access-to-the-primary-database"></a>1. Aanmeldingen met toegang tot de primaire database te bepalen

De eerste stap van het proces is om te bepalen welke aanmeldingen op de doelserver moeten worden gedupliceerd. Dit wordt bereikt met een combinatie van een SELECT-instructies, één in de logische hoofddatabase op de bronserver en één in de primaire database zelf.

Alleen de serverbeheerder of een lid van de **LoginManager** serverfunctie kunt bepalen de aanmeldingen op de bronserver met de volgende SELECT-instructie.

    SELECT [name], [sid]
    FROM [sys].[sql_logins]
    WHERE [type_desc] = 'SQL_Login'

Alleen een lid van de databaserol db_owner, dbo-gebruiker of serverbeheerder, kan alle van de database-gebruiker-principals in de primaire database bepalen.

    SELECT [name], [sid]
    FROM [sys].[database_principals]
    WHERE [type_desc] = 'SQL_USER'

#### <a name="2-find-the-sid-for-the-logins-identified-in-step-1"></a>2. De beveiligings-id vinden voor de aanmeldingen in stap 1 hebt geïdentificeerd

Door het vergelijken van de uitvoer van de query's uit de vorige sectie en die overeenkomt met de SID's worden gemaakt, kunt u de aanmelding op serverniveau toewijzen aan een gebruiker van database. Aanmeldingen die een databasegebruiker met een overeenkomende SID hebben hebben gebruikerstoegang tot deze database als die databasegebruiker principal.

De volgende query kan worden gebruikt om alle van de gebruikersprincipals en hun SID's in een database te bekijken. Alleen een lid van de rol of de server-beheerder voor de database voor db_owner kan deze query kunt uitvoeren.

    SELECT [name], [sid]
    FROM [sys].[database_principals]
    WHERE [type_desc] = 'SQL_USER'

> [!NOTE]
> De **INFORMATION_SCHEMA** en **sys** gebruikers hebben *NULL* SID's, en de **Gast** SID is **0x00**. De **dbo** SID kan beginnen met *0x01060000000001648000000000048454*, als de maker van de database de serverbeheerder in plaats van een lid van is **DbManager**.

#### <a name="3-create-the-logins-on-the-target-server"></a>3. De aanmeldingen maken op de doelserver

De laatste stap is het gaat u naar de doelserver of servers en de aanmeldingen met de juiste beveiligings-id's te genereren. De syntaxis van de basic is als volgt.

    CREATE LOGIN [<login name>]
    WITH PASSWORD = <login password>,
    SID = <desired login SID>

> [!NOTE]
> Als u wilt dat gebruikerstoegang verlenen tot de secundaire server, maar niet naar de primaire, u dat doen kunt door het wijzigen van de gebruikersaanmelding op de primaire server met behulp van de volgende syntaxis.
>
> ```sql
> ALTER LOGIN <login name> DISABLE
> ```
>
> SCHAKEL verandert niet het wachtwoord, zodat u deze altijd inschakelen kunt indien nodig.

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over het beheren van toegang tot databases en aanmeldingen [SQL Database-beveiliging: Beheren van toegang en meld u aan de databasebeveiliging](sql-database-manage-logins.md).
* Zie voor meer informatie over ingesloten databasegebruikers [ingesloten databasegebruikers - waardoor Your Database Portable](https://msdn.microsoft.com/library/ff929188.aspx).
* Zie voor meer informatie over actieve geo-replicatie, [actieve geo-replicatie](sql-database-active-geo-replication.md).
* Zie voor meer informatie over automatische failover-groepen, [automatische failovergroepen](sql-database-auto-failover-group.md).
* Zie voor meer informatie over het gebruik van geo-herstel [geo-herstel](sql-database-recovery-using-backups.md#geo-restore)
