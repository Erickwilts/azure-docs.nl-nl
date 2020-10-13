---
title: Vereisten voor Azure Database Migration Service
description: Meer informatie over een overzicht van de vereisten voor het gebruik van de Azure Database Migration Service voor het uitvoeren van database migraties.
services: database-migration
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: seo-lt-2019
ms.topic: conceptual
ms.date: 02/25/2020
ms.openlocfilehash: e6002bb7995be1cfd1b2812b765835ff7af924e7
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "91308534"
---
# <a name="overview-of-prerequisites-for-using-the-azure-database-migration-service"></a>Overzicht van vereisten voor het gebruik van de Azure Database Migration Service

Er zijn verschillende vereisten vereist om ervoor te zorgen dat Azure Database Migration Service probleemloos wordt uitgevoerd bij het uitvoeren van database migraties. Sommige van de vereisten gelden voor alle scenario's (bron-doel paren) die door de service worden ondersteund, terwijl andere vereisten uniek zijn voor een specifiek scenario.

De vereisten voor het gebruik van de Azure Database Migration Service worden weer gegeven in de volgende secties.

## <a name="prerequisites-common-across-migration-scenarios"></a>Vereisten die gebruikelijk zijn in migratie scenario's

Azure Database Migration Service vereisten die gemeen schappelijk zijn voor alle ondersteunde migratie scenario's zijn onder andere het volgende:

* Maak een Microsoft Azure Virtual Network voor Azure Database Migration Service met behulp van het Azure Resource Manager-implementatiemodel. Dit geeft site-naar-site-verbinding met uw on-premises bronservers met behulp van [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) of [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).
* Zorg ervoor dat de regels voor de netwerk beveiligings groep (NSG) van uw virtuele netwerk niet de volgende communicatie poorten 443, 53, 9354, 445, 12000 blok keren. Zie het artikel [Netwerkverkeer filteren met netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) voor meer informatie over verkeer filteren van verkeer via de netwerkbeveiligingsgroep voor virtuele netwerken.
* Wanneer u een firewallapparaat gebruikt voor de brondatabase(s), moet u mogelijk firewallregels toevoegen om voor Azure Database Migration Service toegang tot de brondatabase(s) voor de migratie toe te staan.
* Configureer uw [Windows Firewall voor toegang tot de database-engine](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
* Schakel het TCP/IP-protocol in, dat standaard is uitgeschakeld tijdens de installatie van SQL Server Express, door de instructies in het artikel [In- of uitschakelen van een Server Network Protocol](https://docs.microsoft.com/sql/database-engine/configure-windows/enable-or-disable-a-server-network-protocol#SSMSProcedure) te volgen.

    > [!IMPORTANT]
    > Het maken van een instantie van Azure Database Migration Service vereist toegang tot de instellingen van het virtuele netwerk die normaal gesp roken niet binnen dezelfde resource groep vallen. Als gevolg hiervan moet de gebruiker die een exemplaar van DMS maakt, toestemming hebben op abonnements niveau. Voer het volgende script uit om de vereiste rollen te maken, die u indien nodig kunt toewijzen:
    >
    > ```
    >
    > $readerActions = `
    > "Microsoft.Network/networkInterfaces/ipConfigurations/read", `
    > "Microsoft.DataMigration/*/read", `
    > "Microsoft.Resources/subscriptions/resourceGroups/read"
    >
    > $writerActions = `
    > "Microsoft.DataMigration/services/*/write", `
    > "Microsoft.DataMigration/services/*/delete", `
    > "Microsoft.DataMigration/services/*/action", `
    > "Microsoft.Network/virtualNetworks/subnets/join/action", `
    > "Microsoft.Network/virtualNetworks/write", `
    > "Microsoft.Network/virtualNetworks/read", `
    > "Microsoft.Resources/deployments/validate/action", `
    > "Microsoft.Resources/deployments/*/read", `
    > "Microsoft.Resources/deployments/*/write"
    >
    > $writerActions += $readerActions
    >
    > # TODO: replace with actual subscription IDs
    > $subScopes = ,"/subscriptions/00000000-0000-0000-0000-000000000000/","/subscriptions/11111111-1111-1111-1111-111111111111/"
    >
    > function New-DmsReaderRole() {
    > $aRole = [Microsoft.Azure.Commands.Resources.Models.Authorization.PSRoleDefinition]::new()
    > $aRole.Name = "Azure Database Migration Reader"
    > $aRole.Description = "Lets you perform read only actions on DMS service/project/tasks."
    > $aRole.IsCustom = $true
    > $aRole.Actions = $readerActions
    > $aRole.NotActions = @()
    >
    > $aRole.AssignableScopes = $subScopes
    > #Create the role
    > New-AzRoleDefinition -Role $aRole
    > }
    >
    > function New-DmsContributorRole() {
    > $aRole = [Microsoft.Azure.Commands.Resources.Models.Authorization.PSRoleDefinition]::new()
    > $aRole.Name = "Azure Database Migration Contributor"
    > $aRole.Description = "Lets you perform CRUD actions on DMS service/project/tasks."
    > $aRole.IsCustom = $true
    > $aRole.Actions = $writerActions
    > $aRole.NotActions = @()
    >
    >   $aRole.AssignableScopes = $subScopes
    > #Create the role
    > New-AzRoleDefinition -Role $aRole
    > }
    > 
    > function Update-DmsReaderRole() {
    > $aRole = Get-AzRoleDefinition "Azure Database Migration Reader"
    > $aRole.Actions = $readerActions
    > $aRole.NotActions = @()
    > Set-AzRoleDefinition -Role $aRole
    > }
    >
    > function Update-DmsConributorRole() {
    > $aRole = Get-AzRoleDefinition "Azure Database Migration Contributor"
    > $aRole.Actions = $writerActions
    > $aRole.NotActions = @()
    > Set-AzRoleDefinition -Role $aRole
    > }
    >
    > # Invoke above functions
    > New-DmsReaderRole
    > New-DmsContributorRole
    > Update-DmsReaderRole
    > Update-DmsConributorRole
    > ```

## <a name="prerequisites-for-migrating-sql-server-to-azure-sql-database"></a>Vereisten voor het migreren van SQL Server naar Azure SQL Database

Naast Azure Database Migration Service vereisten die gemeen schappelijk zijn voor alle migratie scenario's, zijn er ook vereisten die specifiek van toepassing zijn op één scenario of een andere.

Wanneer u de Azure Database Migration Service gebruikt voor het uitvoeren van SQL Server naar Azure SQL Database migraties, naast de vereisten die gemeen schappelijk zijn voor alle migratie scenario's, moet u rekening houden met de volgende aanvullende vereisten:

* Maak een instantie van Azure SQL Database instantie die u doet door de details in het artikel [een Data Base maken in Azure SQL database in de Azure Portal](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal)te volgen.
* Download en installeer de [Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595) v3.3 of hoger.
* Stel uw Windows-firewall open voor toegang van de Azure Database Migration Service tot de brondatabase van SQL Server. Standaard verloopt dit via TCP-poort 1433.
* Als u meerdere benoemde SQL Server-exemplaren met behulp van dynamische poorten uitvoert, kunt u desgewenst de SQL Browser Service inschakelen en toegang tot de UDP-poort 1434 via uw firewalls toestaan, zodat de Azure Database Migration Service verbinding kan maken met een benoemd exemplaar op uw bronserver.
* Maak een [firewall regel](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) op server niveau voor SQL database om de Azure database Migration service toegang tot de doel databases toe te staan. Geef het subnetbereik van het virtuele netwerk op dat wordt gebruikt voor Azure Database Migration Service.
* Zorg ervoor dat de referenties waarmee verbinding wordt gemaakt met het SQL Server-bronexemplaar [CONTROL SERVER](https://docs.microsoft.com/sql/t-sql/statements/grant-server-permissions-transact-sql)-machtigingen hebben.
* Zorg ervoor dat de referenties die worden gebruikt om verbinding te maken met de doel database, de machtiging beheer data base hebben voor de doel database.

   > [!NOTE]
   > Voor een volledig overzicht van de vereisten die nodig zijn om de Azure Database Migration Service te gebruiken om migraties uit te voeren van SQL Server naar Azure SQL Database, raadpleegt u de zelf studie [SQL Server migreren naar Azure SQL database](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-azure-sql).
   >

## <a name="prerequisites-for-migrating-sql-server-to-azure-sql-managed-instance"></a>Vereisten voor het migreren van SQL Server naar Azure SQL Managed instance

* Maak een door SQL beheerd exemplaar door de details in het artikel [een door Azure SQL beheerd exemplaar maken in de Azure Portal](https://aka.ms/sqldbmi)te volgen.
* Open uw firewalls om SMB-verkeer op poort 445 toe te staan voor het Azure Database Migration Service IP-adres of het subnet-bereik.
* Stel uw Windows-firewall open voor toegang van de Azure Database Migration Service tot de brondatabase van SQL Server. Standaard verloopt dit via TCP-poort 1433.
* Als u meerdere benoemde SQL Server-exemplaren met behulp van dynamische poorten uitvoert, kunt u desgewenst de SQL Browser Service inschakelen en toegang tot de UDP-poort 1434 via uw firewalls toestaan, zodat de Azure Database Migration Service verbinding kan maken met een benoemd exemplaar op uw bronserver.
* Zorg ervoor dat de aanmeldingsreferenties die worden gebruikt voor verbinding met het bronexemplaar van SQL Server en het doelexemplaar van Managed Instance lid zijn van de serverrol sysadmin.
* Maak een netwerkshare die de Azure Database Migration Service kan gebruiken om een back-up te maken van de brondatabase.
* Zorg ervoor dat het serviceaccount van het bronexemplaar van SQL Server schrijfbevoegdheid heeft voor de netwerkshare die u hebt gemaakt en dat het computeraccount voor de bronserver lees-/schrijftoegang heeft tot de share.
* Maak een notitie van een Windows-gebruiker (en wachtwoord) die volledig beheer heeft over de netwerkshare die u eerder hebt gemaakt. De Azure Database Migration Service imiteert de gebruikers referenties voor het uploaden van de back-upbestanden naar Azure Storage container voor de herstel bewerking.
* Maak een BLOB-container en haal de SAS-URI op met behulp van de stappen in het artikel [Azure Blob Storage-resources beheren met Storage Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container). Zorg ervoor dat u alle machtigingen (lezen, schrijven, verwijderen, lijst) in het venster beleid selecteert tijdens het maken van de SAS-URI.

   > [!NOTE]
   > Zie voor een volledige lijst van de vereisten die nodig zijn om de Azure Database Migration Service te gebruiken voor het uitvoeren van migraties van SQL Server naar SQL Managed instance de zelf studie [migreren SQL Server naar SQL Managed instance](https://aka.ms/migratetomiusingdms).

## <a name="next-steps"></a>Volgende stappen

Zie het artikel [Wat is de Azure database Migration service](dms-overview.md)voor een overzicht van de Azure database Migration service en regionale Beschik baarheid.
