---
title: Contoso-migratie reeks | Microsoft Docs
description: Koppelingen naar voor beelden van Contoso-migratie scenario's voor migratie naar Azure.
ms.topic: conceptual
ms.date: 04/20/2020
ms.author: raynew
ms.openlocfilehash: f9f7b3b64cf91019ed4e40d5d52b859419b74836
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "86107628"
---
# <a name="contoso-migration-series"></a>Migratieserie Contoso


We hebben een reeks artikelen die laten zien hoe de fictieve organisatie Contoso de on-premises infra structuur migreert naar de [Microsoft Azure](https://azure.microsoft.com/overview/what-is-azure/) Cloud. 

De reeks bevat scenario's waarin wordt uitgelegd hoe u een infrastructuur migratie instelt en hoe u verschillende typen migraties uitvoert. Scenario's groeien in complexiteit naarmate ze worden uitgevoerd. In de artikelen ziet u hoe de organisatie van Contoso de migratie afhandelt, maar algemene instructies en aanwijzers worden in de hele groep verschaft.

## <a name="migration-articles"></a>Migratie artikelen

De artikelen in de reeks worden in de onderstaande tabel samenvatten.  

- Elk migratie scenario wordt aangestuurd door iets andere bedrijfs vereisten, waardoor de migratie strategie wordt bepaald.
- Voor elk implementatie scenario geven we informatie over bedrijfs Stuur Programma's en-doelen, een voorgestelde architectuur, stappen voor het uitvoeren van de migratie en aanbevelingen voor opschonen en volgende stappen nadat de migratie is voltooid.


**Artikel** | **Details** 
--- | --- 
[Artikel 1: overzicht](/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-overview) | Overzicht van de artikel reeks, de migratie strategie van Contoso en de voor beeld-apps die in de reeks worden gebruikt. 
[Artikel 2: Azure-infra structuur implementeren](/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-infrastructure) | Contoso bereidt zijn on-premises infra structuur en Azure-infra structuur voor migratie voor. Dezelfde infra structuur wordt gebruikt voor alle artikelen in de reeks. 
[Artikel 3: on-premises resources evalueren voor migratie naar Azure](/azure/cloud-adoption-framework/migrate/azure-migration-guide/assess?tabs=Tools)  | Contoso voert een evaluatie uit van de on-premises SmartHotel360-app die wordt uitgevoerd op VMware. Contoso evalueert app-Vm's met behulp van de Azure Migrate-service en de app SQL Server-Data Base met behulp van Data Migration Assistant.
[Artikel 4: een app opnieuw hosten op een Azure-VM en een door SQL beheerd exemplaar](/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-vm-sql-managed-instance) | Contoso voert een lift-en-Shift-migratie naar Azure uit voor de on-premises SmartHotel360-app. Contoso migreert de front-end-app-VM met behulp van [Azure migrate](./migrate-services-overview.md). Contoso migreert de app-Data Base naar een door SQL beheerd exemplaar met behulp van de [Azure database Migration service](../dms/dms-overview.md).
[Artikel 5: een app op Azure-Vm's opnieuw hosten](/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-vm) | Contoso migreert de SmartHotel360-app-Vm's naar Azure-Vm's met behulp van de Azure Migrate-service. 
[Artikel 6: een app opnieuw hosten op virtuele machines in Azure en in een SQL Server AlwaysOn-beschikbaarheids groep](/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-vm-sql-ag) | Contoso migreert de SmartHotel360-app. Contoso maakt gebruik van Azure Migrate om de app-Vm's te migreren. De Database Migration Service wordt gebruikt om de app-data base te migreren naar een SQL Server-cluster dat wordt beveiligd door een AlwaysOn-beschikbaarheids groep. 
[Artikel 7: een Linux-app op Azure-Vm's opnieuw hosten](/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-linux-vm) | Contoso voltooit een lift-en-Shift-migratie van de Linux osTicket-app naar Azure-Vm's met behulp van Azure Migrate.
[Artikel 8: een Linux-app opnieuw hosten op Azure Vm's en Azure Database for MySQL](/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-linux-vm-mysql) | Contoso migreert de Linux osTicket-app naar Azure-Vm's met behulp van Azure Migrate. De app-data base wordt gemigreerd naar Azure data base for MySQ, met behulp van Azure Database Migration Service (bevat een alternatieve optie die gebruikmaakt van MySQL Workbench).
[Artikel 9: een app refactoriseren in een Azure-web-app en Azure SQL Database](/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-refactor-web-app-sql) | Contoso migreert de SmartHotel360-app naar een Azure-web-app en migreert de app-Data Base naar Azure SQL Database, met behulp van de Azure Database Migration Service.
[Artikel 10: een Windows-app refactoriseren met Azure-app Services en SQL Managed instance](/azure/cloud-adoption-framework/migrate/azure-best-practices/contoso-migration-refactor-web-app-sql-managed-instance) | Contoso migreert een on-premises Windows-app naar een Azure-web-app en migreert de app-Data Base naar een door Azure SQL beheerd exemplaar met behulp van de Azure Database Migration Service.
[Artikel 11: een Linux-app in een Azure-web-app en Azure Database for MySQL](/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-refactor-linux-app-service-mysql) | Contoso migreert de Linux osTicket-app naar een Azure-web-app in meerdere Azure-regio's, met behulp van Azure Traffic Manager, geïntegreerd met GitHub voor continue levering. Contoso migreert de app-Data Base naar een Azure Database for MySQL-exemplaar. 
[Artikel 12: refactorion Team Foundation Server op Azure DevOps Services](/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-tfs-vsts) | Contoso migreert de on-premises Team Foundation Server implementatie naar Azure DevOps Services in Azure.
[Artikel 13: een app opnieuw samen stellen in azure](/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rebuild) | Contoso bouwt de SmartHotel-app opnieuw op met een scala aan Azure-functies en-services, waaronder Azure App Service, Azure Kubernetes service (AKS), Azure Functions, Azure Cognitive Services en Azure Cosmos DB.
[Artikel 14: een migratie naar Azure schalen](/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-scale) | Na het uitvoeren van de migratie combinaties, bereidt contoso voor om te schalen naar een volledige migratie naar Azure.



## <a name="next-steps"></a>Volgende stappen

- [Meer informatie over](/azure/architecture/cloud-adoption/migrate/) Cloud migratie.
- Meer informatie over migratie strategieën voor andere scenario's (bron/doel paren) vindt u in de [hand leiding voor database migratie](https://datamigration.microsoft.com/).
