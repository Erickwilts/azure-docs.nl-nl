---
title: 'Zelfstudie: Gebruik de Azure Database Migration Service om een online migratie uit te voeren van MySQL naar Azure Database for MySQL | Microsoft Docs'
description: Leer hoe u een online migratie uitvoert van MySQL on-premises naar Azure Database for MySQL met behulp van de Azure Database Migration Service.
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
ms.openlocfilehash: 409fa3a501b80e225fff299980f374f73f0dbf13
ms.sourcegitcommit: 300cd05584101affac1060c2863200f1ebda76b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65415730"
---
# <a name="tutorial-migrate-mysql-to-azure-database-for-mysql-online-using-dms"></a>Zelfstudie: MySQL online migreren naar Azure Database for MySQL met behulp van DMS

U kunt de Azure Database Migration Service gebruiken om de databases met minimale downtime te migreren van een on-premises MySQL-exemplaar naar [Azure Database for MySQL](https://docs.microsoft.com/azure/mysql/). Met andere woorden, de migratie is mogelijk met minimale downtime van de toepassing. In deze studieles migreert u de voorbeelddatabase **Werknemers** van een on-premises exemplaar van MySQL 5.7 naar Azure Database for MySQL met behulp van een online migratieactiviteit in de Azure Database Migration Service.

In deze zelfstudie leert u het volgende:
> [!div class="checklist"]
> * Het voorbeeldschema migreren met behulp van het hulpprogramma mysqldump.
> * De Azure-portal gebruiken om een Azure Database Migration Service-exemplaar te maken.
> * Een migratieproject maken met behulp van de Azure Database Migration Service.
> * De migratie uitvoeren.
> * De migratie controleren.

> [!NOTE]
> Als u Azure Database Migration Service gebruikt om een onlinemigratie uit te voeren, is het vereist dat u een exemplaar maakt op basis van de prijscategorie Premium.

> [!IMPORTANT]
> Voor een optimale migratie-ervaring raadt Microsoft u aan een exemplaar van de Azure Database Migration Service te maken in dezelfde Azure-regio als de doeldatabase. Het verplaatsen van gegevens naar regio's of geografieën kan het migratieproces vertragen en fouten veroorzaken.

## <a name="prerequisites"></a>Vereisten

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Download en installeer [MySQL-communityversie](https://dev.mysql.com/downloads/mysql/) 5.6 of 5.7. De on-premises MySQL-versie moet overeenkomen met de Azure Database for MySQL-versie. MySQL 5.6 kan bijvoorbeeld alleen migreren naar Azure Database for MySQL 5.6 en kan niet worden geüpgraded naar 5.7.
* [Maak een exemplaar in Azure Database for MySQL](https://docs.microsoft.com/azure/mysql/quickstart-create-mysql-server-database-using-azure-portal). Raadpleeg het artikel [MySQL Workbench gebruiken om verbinding te maken en gegevens op te vragen](https://docs.microsoft.com/azure/mysql/connect-workbench) voor informatie over hoe u een database verbindt en maakt met behulp van de Azure-portal.  
* Een Azure Virtual Network (VNet) maken voor de Azure Database Migration Service met behulp van het Azure Resource Manager-implementatiemodel, waarmee u site-naar-site-verbinding met uw on-premises bronservers met behulp van [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) of [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways). Zie voor meer informatie over het maken van een VNet, de [documentatie voor Virtual Network](https://docs.microsoft.com/azure/virtual-network/), en met name de artikelen met vindt u meer details.

    > [!NOTE]
    > Tijdens de installatie van de VNet, als u ExpressRoute gebruikt met het naar Microsoft-netwerkpeering, voeg de volgende service [eindpunten](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview) aan het subnet waarin de service worden ingericht:
    > * Doel-database-eindpunt (bijvoorbeeld SQL-eindpunt, Cosmos DB-eindpunt, enzovoort)
    > * Opslageindpunt
    > * Service bus-eindpunt
    >
    > Deze configuratie is nodig omdat de Azure Database Migration Service beschikt niet over de verbinding met internet.

* Zorg ervoor dat uw VNet netwerkbeveiligingsgroepsregels de volgende poorten voor binnenkomende communicatie naar Azure Database Migration Service niet blokkeren: 443, 53, 9354, 445, 12000. Zie het artikel voor meer informatie over Azure VNet NSG wordt verkeer gefilterd, [netwerkverkeer filteren met netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/virtual-network-vnet-plan-design-arm).
* Configureer uw [Windows Firewall voor toegang tot de database-engine](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
* Stel uw Windows-firewall open voor toegang van de Azure Database Migration Service tot de brondatabase van MySQL Server. Standaard verloopt dit via TCP-poort 3306.
* Wanneer u een firewallapparaat gebruikt voor de brondatabase(s), moet u mogelijk firewallregels toevoegen om voor de Azure Database Migration Service toegang tot de brondatabase(s) voor de migratie toe te staan.
* Maak een [firewallregel](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) op serverniveau voor Azure Database for MySQL om de Azure Database Migration Service toegang te bieden tot de doeldatabases. Geef het subnetbereik van het VNet gebruikt voor de Azure Database Migration Service.
* De brondatabase van MySQL moet op een ondersteunde MySQL-communityversie zijn. Om de versie van het MySQL-exemplaar te bepalen, voert u in het MySQL-hulpprogramma of MySQL Workbench de volgende opdracht uit:

    ```
    SELECT @@version;
    ```

* Azure Database for MySQL ondersteunt alleen InnoDB-tabellen. Raadpleeg het artikel [Tabellen van MyISAM naar InnoDB converteren](https://dev.mysql.com/doc/refman/5.7/en/converting-tables-to-innodb.html) om MyISAM-tabellen te converteren naar InnoDB.

* Schakel binaire logboekregistratie in het bestand my.ini (Windows) of my.cnf (Unix) in de brondatabase in met behulp van de volgende configuratie:

    * **server_id** = 1 of hoger (alleen relevant voor MySQL 5.6)
    * **log-bin** =\<pad > (alleen relevant voor MySQL 5.6)

        Bijvoorbeeld: log-bin = E:\MySQL_logs\BinLog
    * **binlog_format** = row
    * **Expire_logs_days** = 5 (het is raadzaam gebruik niet nul zijn; alleen relevant voor MySQL 5.6)
    * **Binlog_row_image** = full (alleen relevant voor MySQL 5.6)
    * **log_slave_updates** = 1

* De gebruiker moet beschikken over de rol ReplicationAdmin met de volgende bevoegdheden:

    * **REPLICATIECLIENT**: alleen vereist voor taken voor de verwerking van wijzigingen. Met andere woorden, voor taken voor volledig laden is deze bevoegdheid niet vereist.
    * **REPLICATIEREPLICA**: alleen vereist voor taken voor de verwerking van wijzigingen. Met andere woorden, voor taken voor volledig laden is deze bevoegdheid niet vereist.
    * **SUPER**: alleen vereist in eerdere versies dan MySQL 5.6.6.

## <a name="migrate-the-sample-schema"></a>Het voorbeeldschema migreren

Om alle databaseobjecten zoals tabelschema’s, indexen en opgeslagen procedures te voltooien, moeten we het schema uit de brondatabase extraheren en op de database toepassen. Om het schema te extraheren, kunt u mysqldump gebruiken met de parameter `--no-data`.

Ervan uitgaande dat u hebt MySQL **werknemers** voorbeelddatabase in de on-premises systeem, de opdracht voor het schema-migratie met behulp van mysqldump is:

```
mysqldump -h [servername] -u [username] -p[password] --databases [db name] --no-data > [schema file path]
```

Bijvoorbeeld:

```
mysqldump -h 10.10.123.123 -u root -p --databases employees --no-data > d:\employees.sql
```

Voer de volgende opdracht uit om het schema te importeren in de Azure Database for MySQL-doeldatabase:

```
mysql.exe -h [servername] -u [username] -p[password] [database]< [schema file path]
 ```

Bijvoorbeeld:

```
mysql.exe -h shausample.mysql.database.azure.com -u dms@shausample -p employees < d:\employees.sql
 ```

Als u refererende sleutels in uw schema hebt, mislukken de eerste lading en doorlopende synchronisatie van de migratie.  Voer het volgende script in MySQL Workbench voor het extraheren van het script drop refererende sleutel en refererende sleutels script toevoegen.

```
SET group_concat_max_len = 8192;
    SELECT SchemaName, GROUP_CONCAT(DropQuery SEPARATOR ';\n') as DropQuery, GROUP_CONCAT(AddQuery SEPARATOR ';\n') as AddQuery
    FROM
    (SELECT
    KCU.REFERENCED_TABLE_SCHEMA as SchemaName,
    KCU.TABLE_NAME,
    KCU.COLUMN_NAME,
    CONCAT('ALTER TABLE ', KCU.TABLE_NAME, ' DROP FOREIGN KEY ', KCU.CONSTRAINT_NAME) AS DropQuery,
    CONCAT('ALTER TABLE ', KCU.TABLE_NAME, ' ADD CONSTRAINT ', KCU.CONSTRAINT_NAME, ' FOREIGN KEY (`', KCU.COLUMN_NAME, '`) REFERENCES `', KCU.REFERENCED_TABLE_NAME, '` (`', KCU.REFERENCED_COLUMN_NAME, '`) ON UPDATE ',RC.UPDATE_RULE, ' ON DELETE ',RC.DELETE_RULE) AS AddQuery
    FROM INFORMATION_SCHEMA.KEY_COLUMN_USAGE KCU, information_schema.REFERENTIAL_CONSTRAINTS RC
    WHERE
      KCU.CONSTRAINT_NAME = RC.CONSTRAINT_NAME
      AND KCU.REFERENCED_TABLE_SCHEMA = RC.UNIQUE_CONSTRAINT_SCHEMA
  AND KCU.REFERENCED_TABLE_SCHEMA = 'SchemaName') Queries
  GROUP BY SchemaName;
 ```

Voer het script voor verwijdering van refererende sleutels (de tweede kolom) in het queryresultaat uit om de refererende sleutel te verwijderen.

> [!IMPORTANT]
> Als u gegevens met behulp van een back-up importeert, verwijdert u de opdrachten maken OPSTELLER handmatig of met behulp van de--skip-opsteller opdracht bij het uitvoeren van een mysqldump. OPSTELLER vereist super bevoegdheden voor het maken en is beperkt in Azure Database voor MySQL.

Als u een trigger hebt in de gegevens (insert of update trigger), wordt deze de integriteit van gegevens in het doel voor de gerepliceerde gegevens van de bron afdwingen. Het is raadzaam triggers in alle tabellen in de doeldatabase uit te schakelen tijdens de migratie, en de triggers dan weer in te schakelen zodra de migratie is voltooid.

Als u wilt uitschakelen triggers in de doeldatabase, gebruik de volgende opdracht:

```
SELECT Concat('DROP TRIGGER ', Trigger_Name, ';') FROM  information_schema.TRIGGERS WHERE TRIGGER_SCHEMA = 'your_schema';
```

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Registreer de Microsoft.DataMigration-resourceprovider

1. Meld u aan bij de Azure-portal, selecteer **Alle services** en selecteer vervolgens **Abonnementen**.

   ![Portal-abonnementen weergeven](media/tutorial-mysql-to-azure-mysql-online/portal-select-subscriptions.png)

2. Selecteer het abonnement waarin u het Azure Database Migration Service-exemplaar wilt maken en selecteer vervolgens **Resourceproviders**.

    ![Resourceproviders weergeven](media/tutorial-mysql-to-azure-mysql-online/portal-select-resource-provider.png)

3. Zoek naar migratie en selecteer rechts van **Microsoft.DataMigration** de optie **Registreren**.

    ![Resourceprovider registreren](media/tutorial-mysql-to-azure-mysql-online/portal-register-resource-provider.png)

## <a name="create-a-dms-instance"></a>Een DMS-exemplaar maken

1. Selecteer in de Azure-portal **Een resource maken**, zoek naar Azure Database Migration Service, en selecteer vervolgens **Azure Database Migration Service** uit de vervolgkeuzelijst.

    ![Azure Marketplace](media/tutorial-mysql-to-azure-mysql-online/portal-marketplace.png)

2. Selecteer in het scherm **Azure Database Migration Service** **Maken**.

    ![Azure Database Migration Service-exemplaar maken](media/tutorial-mysql-to-azure-mysql-online/dms-create1.png)
  
3. Geef in het scherm **Migratieservice maken** een naam op voor de service, het abonnement en een nieuwe of bestaande resourcegroep.

4. Selecteer een bestaand VNet of maak een nieuwe.

    Het VNet biedt de Azure Database Migration Service toegang tot de bron-SQL Server en de doel-Azure SQL Database-exemplaar.

    Zie het artikel voor meer informatie over het maken van een VNet in Azure portal [een virtueel netwerk maken met de Azure-portal](https://aka.ms/DMSVnet).

5. Selecteer een prijscategorie.

    Zie voor meer informatie over de kosten en prijscategorieën de [Pagina met prijzen](https://aka.ms/dms-pricing).

    ![Instellingen configureren van een Azure Database Migration Service-exemplaar](media/tutorial-mysql-to-azure-mysql-online/dms-settings3.png)

6. Selecteer **Maken** om de dienst te maken.

## <a name="create-a-migration-project"></a>Maak een migratieproject

Nadat de service is gemaakt, zoek deze op in de Azure-portal, open hem en maak vervolgens een nieuw migratieproject.

1. Selecteer in de Azure-portal **Alle diensten**, zoek naar Azure Database Migration Service, en selecteer vervolgens **Azure Database Migration Service**.

      ![Zoek alle exemplaren van de Azure Database Migration Service](media/tutorial-mysql-to-azure-mysql-online/dms-search.png)

2. Zoek in het scherm **Azure Database Migration service** naar de naam van de Azure Database Migration Service-instantie die u hebt gemaakt en selecteer vervolgens het exemplaar.

     ![Zoek uw exemplaar van de Azure Database Migration Service](media/tutorial-mysql-to-azure-mysql-online/dms-instance-search.png)

3. Selecteer + **Nieuw migratieproject**.
4. Geef in het scherm **Nieuw migratieproject** een naam op voor het project, selecteer in het tekstvak **bronservertype** de optie **MySQL** en selecteer in het tekstvak **doelservertype** de optie **AzureDbForMySQL**.
5. Selecteer in de sectie **Het type activiteit kiezen** de optie **Onlinegegevensmigratie**

    ![Azure Database Migration Service-project maken](media/tutorial-mysql-to-azure-mysql-online/dms-create-project4.png)

    > [!NOTE]
    > U kunt ook **alleen maken in project** nu het migratieproject maken en uitvoeren van de migratie later opnieuw.

6. Selecteer **Opslaan**, noteer de vereisten om DMS succesvol te gebruiken om gegevens te migreren, en selecteer vervolgens **Activiteit maken en uitvoeren**.

## <a name="specify-source-details"></a>Geef brondetails op

1. Geef in het scherm **Brondetails toevoegen** de verbindingsgegevens op voor het bronexemplaar van MySQL.

    ![Scherm Brondetails toevoegen](media/tutorial-mysql-to-azure-mysql-online/dms-add-source-details.png)

## <a name="specify-target-details"></a>Doeldetails opgeven

1. Selecteer **Opslaan** en geef vervolgens in het scherm **Doeldetails** de verbindingsgegevens op voor de Azure Database for MySQL-doelserver. Dit is het vooraf ingerichte exemplaar van Azure Database for MySQL waarnaar het **Werknemers**-schema is geïmplementeerd met behulp van mysqldump.

    ![Scherm Doeldetails](media/tutorial-mysql-to-azure-mysql-online/dms-add-target-details.png)

2. Selecteer **Opslaan**, en klik vervolgens in het scherm **Toewijzen aan doeldatabases**, wijs de bron- en de doeldatabase voor de migratie toe.

    Als de doeldatabase de naam van de dezelfde database als de bron-database bevat, wordt in de Azure Database Migration Service de doeldatabase standaard geselecteerd.

    ![Toewijzen aan doeldatabases](media/tutorial-mysql-to-azure-mysql-online/dms-map-target-details.png)

3. Selecteer **Opslaan**, geef in het scherm **Migratieoverzicht** in het tekstvak **Naam activiteit** een naam op voor de migratieactiviteit en controleer het overzicht om ervoor te zorgen dat de bron- en doeldetails overeenkomen met wat u eerder hebt opgegeven.

    ![Migratieoverzicht](media/tutorial-mysql-to-azure-mysql-online/dms-migration-summary.png)

## <a name="run-the-migration"></a>De migratie uitvoeren

* Selecteer **Migratie uitvoeren**.

    Het venster van de migratieactiviteit wordt weergegeven en de **Status** van de activiteit is **Initialiseren**.

## <a name="monitor-the-migration"></a>Bewaak de migratie

1. Selecteer op het scherm van de migratieactiviteit de optie **Vernieuwen** om de weergave bij te werken totdat de **Status** van de migratie als **Voltooid** wordt weergegeven.

     ![Activiteitenstatus: Voltooid](media/tutorial-mysql-to-azure-mysql-online/dms-activity-completed.png)

2. Selecteer onder **Databasenaam** een specifieke database om de migratiestatus voor de bewerkingen **Alle gegevens worden geladen** en **Incrementele gegevenssynchronisatie** te bekijken.

    ‘Alle gegevens worden geladen’ toont de migratiestatus van de eerste lading terwijl ‘Incrementele gegevenssynchronisatie’ de CDC-status (Change Data Capture) toont.

     ![Activiteitenstatus: Volledig geladen](media/tutorial-mysql-to-azure-mysql-online/dms-activity-full-load-completed.png)

     ![Activiteitenstatus: Incrementele gegevenssynchronisatie](media/tutorial-mysql-to-azure-mysql-online/dms-activity-incremental-data-sync.png)

## <a name="perform-migration-cutover"></a>Migratie-cutover uitvoeren

Nadat de eerste volledige lading is voltooid, worden de databases gemarkeerd als **Gereed voor cutover**.

1. Wanneer u klaar bent om de databasemigratie te voltooien, selecteert u **Cutover starten**.

    ![Cutover starten](media/tutorial-mysql-to-azure-mysql-online/dms-start-cutover.png)

2. Zorg dat u alle transacties stopt die bij de brondatabase binnenkomen; wacht totdat de teller van **Wijzigingen in behandeling** op **0** staat.
3. Selecteer **Bevestigen** en selecteer vervolgens **Toepassen**.
4. Wanneer de databasemigratiestatus **Voltooid** toont, verbindt u uw toepassingen met de nieuwe doel-Azure SQL Database.

## <a name="next-steps"></a>Volgende stappen

* Raadpleeg het artikel [Bekende problemen/beperkingen met online migraties naar Azure Database for MySQL](known-issues-azure-mysql-online.md) voor informatie over bekende problemen en beperkingen bij het uitvoeren van online migraties naar Azure Database for MySQL.
* Raadpleeg het artikel [Wat is de Azure Database Migration Service?](https://docs.microsoft.com/azure/dms/dms-overview) voor informatie over de Azure Database Migration Service.
* Raadpleeg het artikel [Wat is Azure Database for MySQL?](https://docs.microsoft.com/azure/mysql/overview) voor informatie over Azure Database for MySQL.
