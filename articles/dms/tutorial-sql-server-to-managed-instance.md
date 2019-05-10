---
title: 'Zelfstudie: DMS gebruiken om te migreren naar een beheerd exemplaar voor Azure SQL Database | Microsoft Docs'
description: Meer informatie over het migreren van on-premises SQL Server naar een beheerd exemplaar voor Azure SQL Database met behulp van de Azure Database Migration Service.
services: dms
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: article
ms.date: 05/08/2019
ms.openlocfilehash: c768b7548b9759e85ebfb050f0ead2dfd3c1a6a6
ms.sourcegitcommit: 300cd05584101affac1060c2863200f1ebda76b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65415108"
---
# <a name="tutorial-migrate-sql-server-to-an-azure-sql-database-managed-instance-offline-using-dms"></a>Zelfstudie: SQL Server migreren naar een beheerd exemplaar voor Azure SQL Database offline met behulp van DMS

U kunt de Azure Database Migration Service gebruiken om databases te migreren van een on-premises SQL Server-exemplaar naar een [beheerd Azure SQL Database-exemplaar](../sql-database/sql-database-managed-instance.md). In het artikel [Een SQL Server-exemplaar migreren naar een beheerd Azure SQL Database-exemplaar](../sql-database/sql-database-managed-instance-migrate.md) vindt u aanvullende methoden, waarvoor enkele handmatige stappen nodig kunnen zijn.

In deze zelfstudie migreert u de database **Adventureworks2012** van een on-premises exemplaar van SQL Server naar een beheerd Azure SQL Database-exemplaar. Dit doet u met behulp van de Azure Database Migration Service.

In deze zelfstudie leert u het volgende:
> [!div class="checklist"]
> - De Azure-portal gebruiken om een Azure Database Migration Service-exemplaar te maken.
> - Een migratieproject maken met behulp van de Azure Database Migration Service.
> - De migratie uitvoeren.
> - De migratie controleren.
> - Een migratierapport downloaden.

[!INCLUDE [online-offline](../../includes/database-migration-service-offline-online.md)]

Dit artikel beschrijft een offline migratie van SQL Server naar een beheerd exemplaar voor Azure SQL Database. Zie voor een online migratie [SQL-Server migreren naar een Azure SQL-Database beheerd exemplaar online met behulp van DMS](tutorial-sql-server-managed-instance-online.md).

## <a name="prerequisites"></a>Vereisten

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

- Een Azure Virtual Network (VNet) maken voor de Azure Database Migration Service met behulp van het Azure Resource Manager-implementatiemodel, waarmee u site-naar-site-verbinding met uw on-premises bronservers met behulp van [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) of [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways). [Lees hier meer over netwerktopologieën voor migraties van beheerde Azure SQL Database-exemplaren met behulp van de Azure Database Migration Service](https://aka.ms/dmsnetworkformi). Zie voor meer informatie over het maken van een VNet, de [documentatie voor Virtual Network](https://docs.microsoft.com/azure/virtual-network/), en met name de artikelen met vindt u meer details.

    > [!NOTE]
    > Tijdens de installatie van de VNet, als u ExpressRoute gebruikt met het naar Microsoft-netwerkpeering, voeg de volgende service [eindpunten](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview) aan het subnet waarin de service worden ingericht:
    > - Doel-database-eindpunt (bijvoorbeeld SQL-eindpunt, Cosmos DB-eindpunt, enzovoort)
    > - Opslageindpunt
    > - Service bus-eindpunt
    >
    > Deze configuratie is nodig omdat de Azure Database Migration Service beschikt niet over de verbinding met internet.

- Zorg ervoor dat uw VNet netwerkbeveiligingsgroepsregels de volgende poorten voor binnenkomende communicatie naar Azure Database Migration Service niet blokkeren: 443, 53, 9354, 445, 12000. Zie het artikel voor meer informatie over Azure VNet NSG wordt verkeer gefilterd, [netwerkverkeer filteren met netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg).
- Configureer uw [Windows-firewall voor toegang tot de engine van de brondatabase](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
- Stel uw Windows-firewall open voor toegang van de Azure Database Migration Service tot de brondatabase van SQL Server. Standaard verloopt dit via TCP-poort 1433.
- Als u meerdere benoemde SQL Server-exemplaren uitvoert met behulp van dynamische poorten, kunt u desgewenst de SQL Browser Service inschakelen en toegang tot de UDP-poort 1434 via uw firewalls toestaan, zodat de Azure Database Migration Service verbinding kan maken met een benoemd exemplaar op uw bronserver.
- Als u een firewallapparaat gebruikt vóór de brondatabases, moet u mogelijk firewallregels toevoegen om de Azure Database Migration Service toegang te geven tot de brondatabase(s) voor migratie. Hetzelfde geldt voor bestanden via SMB-poort 445.
- Maak een beheerd Azure SQL Database-exemplaar aan de hand van de instructies in het artikel [Een beheerd exemplaar voor Azure SQL Database maken in de Azure-portal](https://aka.ms/sqldbmi).
- Zorg ervoor dat de aanmeldingsreferenties die worden gebruikt voor verbinding met het bronexemplaar van SQL Server en het doelexemplaar van het beheerde exemplaar lid zijn van de serverrol sysadmin.
- Maak een netwerkshare die de Azure Database Migration Service kan gebruiken om een back-up te maken van de brondatabase.
- Zorg ervoor dat het serviceaccount van het bronexemplaar van SQL Server schrijfbevoegdheid heeft voor de netwerkshare die u hebt gemaakt en dat het computeraccount voor de bronserver lees-/schrijftoegang heeft tot de share.
- Maak een notitie van een Windows-gebruiker (en wachtwoord) die volledig beheer heeft over de netwerkshare die u eerder hebt gemaakt. De Azure Database Migration Service imiteert de gebruikersreferenties voor het uploaden van de back-upbestanden naar een Azure-opslagcontainer voor herstelbewerkingen.
- Maak een blob-container en haal de SAS-URI van container op met behulp van de stappen in het artikel [Azure Blob Storage-resources beheren met Storage Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container). Vergeet niet om alle machtigingen (Lezen, Schrijven, Verwijderen, Lijst) te selecteren in het beleidsvenster terwijl u de SAS-URI maakt. De Azure Database Migration Service heeft zo toegang tot de container van het opslagaccount voor het uploaden van de back-upbestanden die worden gebruikt voor het migreren van databases naar het beheerde Azure SQL Database-exemplaar.

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Registreer de Microsoft.DataMigration-resourceprovider

1. Meld u aan bij de Azure-portal, selecteer **Alle services** en selecteer vervolgens **Abonnementen**.

    ![Portal-abonnementen weergeven](media/tutorial-sql-server-to-managed-instance/portal-select-subscriptions.png)        

2. Selecteer het abonnement waarin u het Azure Database Migration Service-exemplaar wilt maken en selecteer vervolgens **Resourceproviders**.

    ![Resourceproviders weergeven](media/tutorial-sql-server-to-managed-instance/portal-select-resource-provider.png)

3. Zoek naar migratie en selecteer rechts van **Microsoft.DataMigration** de optie **Registreren**.

    ![Resourceprovider registreren](media/tutorial-sql-server-to-managed-instance/portal-register-resource-provider.png)

## <a name="create-an-azure-database-migration-service-instance"></a>Een Azure Database Migration Service-exemplaar maken

1. Selecteer in de Azure-portal + **Een resource maken**, zoek naar **Azure Database Migration Service** en selecteer vervolgens **Azure Database Migration Service** in de vervolgkeuzelijst.

     ![Azure Marketplace](media/tutorial-sql-server-to-managed-instance/portal-marketplace.png)

2. Selecteer in het scherm **Azure Database Migration Service** **Maken**.

    ![Azure Database Migration Service-exemplaar maken](media/tutorial-sql-server-to-managed-instance/dms-create1.png)

3. Geef in het scherm **Migratieservice maken** een naam op voor de service, het abonnement en een nieuwe of bestaande resourcegroep.

4. Selecteer de locatie waarop u het exemplaar van DMS wilt maken.

5. Selecteer een bestaand VNet of maak een.

    Het VNet biedt de Azure Database Migration Service met toegang tot de bron-SQL Server- en doel-Azure SQL-Database beheerd exemplaar.

    Zie het artikel voor meer informatie over het maken van een VNet in Azure portal [een virtueel netwerk maken met de Azure-portal](https://aka.ms/DMSVnet).

    Zie het artikel [Netwerktopologieën voor migraties van Azure SQL Database Managed Instance met behulp van de Azure Database Migration Service](https://aka.ms/dmsnetworkformi) voor aanvullende informatie.

6. Selecteer een prijscategorie.

    Zie voor meer informatie over de kosten en prijscategorieën de [Pagina met prijzen](https://aka.ms/dms-pricing).

    ![DMS-service starten](media/tutorial-sql-server-to-managed-instance/dms-create-service2.png)

7. Selecteer **Maken** om de dienst te maken.

## <a name="create-a-migration-project"></a>Maak een migratieproject

Nadat er een exemplaar van de service is gemaakt, zoekt u het exemplaar in de Azure-portal, opent u het en maakt u vervolgens een nieuw migratieproject.

1. Selecteer in de Azure-portal **Alle diensten**, zoek naar Azure Database Migration Service, en selecteer vervolgens **Azure Database Migration Service**.

    ![Zoek alle exemplaren van de Azure Database Migration Service](media/tutorial-sql-server-to-managed-instance/dms-search.png)

2. Zoek in het scherm **Azure Database Migration Service** naar de naam van het exemplaar dat u hebt gemaakt en selecteer vervolgens het exemplaar.

3. Selecteer + **Nieuw migratieproject**.

4. Geef in het scherm **Nieuw migratieproject** een naam op voor het project in het tekstvak **Bronservertype**, selecteer **SQL Server**, selecteer in het tekstvak **Type doelserver** **Azure SQL Database Managed Instance** en selecteer vervolgens **Offlinegegevensmigratie** voor **Type activiteit kiezen**.

   ![DMS-project maken](media/tutorial-sql-server-to-managed-instance/dms-create-project2.png)

5. Selecteer **Maken** om het project te maken.

## <a name="specify-source-details"></a>Geef brondetails op

1. Geef in het scherm **Brongegevens** de verbindingsgegevens op voor de brondatabase van SQL Server.

2. Als u geen vertrouwd certificaat hebt geïnstalleerd op de server, schakelt u het selectievakje **Servercertificaat vertrouwen** in.

    Wanneer er geen vertrouwd certificaat is geïnstalleerd, genereert SQL Server een zelfondertekend certificaat wanneer het exemplaar wordt gestart. Dit certificaat wordt gebruikt voor het versleutelen van de referenties voor clientverbindingen.

    > [!CAUTION]
    > SSL-verbindingen die worden versleuteld met behulp van een zelfondertekend certificaat bieden geen sterke beveiliging. Ze zijn vatbaar voor man-in-the-middle-aanvallen. U moet niet vertrouwen op SSL met behulp van zelfondertekende certificaten in een productieomgeving of op servers die zijn verbonden met internet.

   ![Brondetails](media/tutorial-sql-server-to-managed-instance/dms-source-details1.png)

3. Selecteer **Opslaan**.

4. Selecteer in het scherm **Brondatabases selecteren** de database **Adventureworks2012** voor migratie.

   ![Brondatabases selecteren](media/tutorial-sql-server-to-managed-instance/dms-source-database1.png)

    > [!IMPORTANT]
    > Als u SSIS (SQL Server Integration Services) gebruikt, biedt DMS momenteel geen ondersteuning voor het migreren van de catalogusdatabase voor uw SSIS-projecten/pakketten (SSISDB) van SQL Server naar een beheerd Azure SQL Database-exemplaar. U kunt SSIS echter in Azure Data Factory (ADF) inrichten en uw SSIS-projecten/-pakketten opnieuw implementeren naar de bestemmings-SSISDB die wordt gehost door het beheerde Azure SQL Database-exemplaar. Zie het artikel [Pakketten van SQL Server Integration Services migreren naar Azure](https://docs.microsoft.com/azure/dms/how-to-migrate-ssis-packages) voor meer informatie over het migreren van SSIS-pakketten.

5. Selecteer **Opslaan**.

## <a name="specify-target-details"></a>Doeldetails opgeven

1. Geef in het scherm **Details migratiedoel** de verbindingsgegevens op voor het doel, dat bestaat uit het vooraf ingerichte beheerde Azure SQL Database-exemplaar waarnaar u de database **AdventureWorks2012** migreert.

    Als u het beheerde Azure SQL Database-exemplaar nog niet hebt ingericht, selecteert u de [koppeling](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-get-started) voor instructies voor het inrichten van het exemplaar. U kunt nog steeds doorgaan met het maken van het project en vervolgens, wanneer het beheerde exemplaar van Azure SQL Database gereed is, terug naar dit specifieke project voor het uitvoeren van de migratie.

     ![Doel selecteren](media/tutorial-sql-server-to-managed-instance/dms-target-details2.png)

2. Selecteer **Opslaan**.

## <a name="select-source-databases"></a>De brondatabases selecteren

1. Selecteer in het scherm **Brondatabases selecteren** de brondatabase die u wilt migreren.

    ![De brondatabases selecteren](media/tutorial-sql-server-to-managed-instance/select-source-databases.png)

2. Selecteer **Opslaan**.

## <a name="select-logins"></a>Aanmeldingen selecteren

1. Selecteer in het scherm **Aanmeldingen selecteren** de aanmeldingen die u wilt migreren.

    >[!NOTE]
    >Deze versie biedt alleen ondersteuning voor het migreren van de SQL-aanmeldingen.

    ![Aanmeldingen selecteren](media/tutorial-sql-server-to-managed-instance/select-logins.png)

2. Selecteer **Opslaan**.

## <a name="configure-migration-settings"></a>Migratie-instellingen configureren

1. Geef in het scherm **Migratie-instellingen configureren** de volgende instellingen op:

    | | |
    |--------|---------|
    |**De back-upoptie voor de bron kiezen** | Kies de optie **Ik lever de recentste back-upbestanden** wanneer u al beschikt over een volledige set back-upbestanden die DMS kan gebruiken voor migratie van de database. Kies de optie **Ik laat Azure Database Migration Service back-upbestanden maken** als u wilt dat DMS de volledige back up van de brondatabase gebruikt voor migratie. |
    |**De netwerksharelocatie waarheen back-ups van databases kunnen worden verplaatst door Azure Database Migration Service** | De lokale SMB-netwerkshare waarop de Azure Database Migration Service back-ups van de brondatabase kan opslaan. Het serviceaccount waarmee het SQL Server-bronexemplaar wordt uitgevoerd, moet schrijfbevoegdheid op deze netwerkshare hebben. Geef een FQDN-naam of IP-adressen op van de server in de netwerkshare, bijvoorbeeld '\\\servernaam.domeinnaam.com\back-upmap' of '\\\IP-adres\back-upmap'.|
    |**Gebruikersnaam** | Zorg ervoor dat de Windows-gebruiker volledig beheer heeft over de netwerkshare die u hierboven hebt opgegeven. De Azure Database Migration Service imiteert de gebruikersreferenties voor het uploaden van de back-upbestanden naar een Azure-opslagcontainer voor herstelbewerkingen. Als TDE-compatibele databases zijn geselecteerd voor migratie, moet de bovenstaande Windows-gebruiker het ingebouwde administratoraccount zijn en moet [Gebruikersaccountbeheer](https://docs.microsoft.com/windows/security/identity-protection/user-account-control/user-account-control-overview) zijn uitgeschakeld om Azure Database Migration Service in staat te stellen de certificaatbestanden te uploaden en verwijderen. |
    |**Wachtwoord** | Het wachtwoord voor de gebruiker. |
    |**Opslagaccountinstellingen** | De SAS-URI die Azure Database Migration Service toegang biedt tot de container van het opslagaccount waarnaar de service de back-upbestanden uploadt en die wordt gebruikt voor het migreren van databases naar het beheerde Azure SQL Database-exemplaar. [Lees hier meer over het verkrijgen van de SAS-URI voor de blob-container](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container).|
    |**TDE-instellingen** | Als de brondatabases u met transparante gegevensversleuteling (TDE migreert waarnaar) ingeschakeld, moet u schrijfrechten hebben op het beheerde exemplaar van doel-Azure SQL Database.  Selecteer in de vervolgkeuzelijst het abonnement waarin het beheerde Azure SQL Database-exemplaar is ingericht.  Selecteer het **beheerde doelexemplaar voor Azure SQL Database** in de vervolgkeuzelijst. |

    ![Migratie-instellingen configureren](media/tutorial-sql-server-to-managed-instance/dms-configure-migration-settings3.png)

2. Selecteer **Opslaan**.

## <a name="review-the-migration-summary"></a>Migratieoverzicht controleren

1. Geef in het tekstvak **Naam activiteit** van het scherm **Migratieoverzicht** een naam op voor de migratieactiviteit.

2. Vouw de sectie **Validatieoptie** uit om het scherm **Validatieoptie kiezen** weer te geven en geef op of de gemigreerde databases moeten worden gevalideerd op juistheid van de query's. Selecteer vervolgens **Opslaan**.

3. Bekijk en controleer de gegevens van het migratieproject.

    ![Overzicht van migratieproject](media/tutorial-sql-server-to-managed-instance/dms-project-summary2.png)

4. Selecteer **Opslaan**.

## <a name="run-the-migration"></a>De migratie uitvoeren

- Selecteer **Migratie uitvoeren**.

  Het venster van de migratieactiviteit wordt weergegeven en de Status van de activiteit is **In behandeling**.

## <a name="monitor-the-migration"></a>Bewaak de migratie

1. Selecteer in het scherm van de migratieactiviteit **Vernieuwen** om de weergave bij te werken.

   ![Migratieactiviteit wordt uitgevoerd](media/tutorial-sql-server-to-managed-instance/dms-monitor-migration1.png)

    U kunt de databases en aanmeldingscategorieën verder uitvouwen om de migratiestatus van de respectieve serverobjecten te controleren.

   ![Migratieactiviteit wordt uitgevoerd](media/tutorial-sql-server-to-managed-instance/dms-monitor-migration-extend.png)

2. Nadat de migratie is voltooid, selecteert u **Rapport downloaden** om een rapport op te halen met gegevens die zijn gekoppeld aan het migratieproces.

3. Controleer of de doeldatabase bestaat in de omgeving van de doeldatabase van het beheerde Azure SQL Database-exemplaar.

## <a name="next-steps"></a>Volgende stappen

- Zie [Een back-up van de database herstellen voor een beheerd Azure SQL Database-exemplaar](../sql-database/sql-database-managed-instance-restore-from-backup-tutorial.md) voor een zelfstudie waarin wordt uitgelegd hoe u een database migreert naar een beheerd exemplaar met de opdracht T-SQL RESTORE.
- Zie [Wat is een beheerd exemplaar?](../sql-database/sql-database-managed-instance.md) voor meer informatie over beheerde exemplaren.
- Zie [Verbinding maken met toepassingen](../sql-database/sql-database-managed-instance-connect-app.md) voor meer informatie over het verbinden van apps met een beheerd exemplaar.
