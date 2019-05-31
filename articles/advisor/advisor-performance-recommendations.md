---
title: Prestaties van Azure-toepassingen met Azure Advisor | Microsoft Docs
description: Advisor gebruiken om de prestaties van uw Azure-implementaties te optimaliseren.
services: advisor
documentationcenter: NA
author: kasparks
ms.service: advisor
ms.topic: article
ms.date: 01/29/2019
ms.author: kasparks
ms.openlocfilehash: 8fdae1e12e56dcbcb56941726b0c089ad59b8fc8
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/28/2019
ms.locfileid: "66254656"
---
# <a name="improve-performance-of-azure-applications-with-azure-advisor"></a>Prestaties van Azure-toepassingen met Azure Advisor

Azure Advisor-aanbevelingen voor prestaties te verbeteren en de reactiesnelheid van uw bedrijfskritische toepassingen. U kunt ook aanbevelingen voor prestaties van de Advisor krijgen bij de **prestaties** van de Advisor-dashboard.

## <a name="reduce-dns-time-to-live-on-your-traffic-manager-profile-to-fail-over-to-healthy-endpoints-faster"></a>Time to live van uw Traffic Manager-profiel sneller failover naar de eindpunten in orde van DNS-beperken

[Tijd naar Live (TTL) instellingen](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-performance-considerations) op uw Traffic Manager-profiel kunt u opgeven hoe snel om over te schakelen eindpunten als een bepaald eindpunt niet meer reageert op query's. Vermindering van de TTL-waarden, betekent dat clients worden doorgestuurd naar een functionerende eindpunten sneller.

Azure Advisor identificeert Traffic Manager-profielen met een langer TTL geconfigureerd en aanbevolen configuratie van de TTL-waarde 20 seconden of 60 seconden, afhankelijk van of u het profiel is geconfigureerd voor [Fast Failover](https://azure.microsoft.com/roadmap/fast-failover-and-tcp-probing-in-azure-traffic-manager/).

## <a name="improve-database-performance-with-sql-db-advisor"></a>Verbeter de prestaties van de database met SQL DB Advisor

Advisor biedt u een consistente, geconsolideerde weergave van de aanbevelingen voor al uw Azure-resources. Het is geïntegreerd met SQL Database Advisor voor aanbevelingen voor het verbeteren van de prestaties van uw SQL Azure-database. SQL Database Advisor beoordeelt de prestaties van uw SQL Azure-databases door het analyseren van uw gebruiksgeschiedenis. Vervolgens biedt aanbevelingen die bij uitstek geschikt zijn voor het uitvoeren van de typische werkbelasting van de database.

> [!NOTE]
> Om aanbevelingen te krijgen, moet een database over een week van het gebruik van, en in de afgelopen week moet er activiteit zijn consistent. SQL Database Advisor kunt eenvoudiger voor consistente querypatronen dan voor willekeurige pieken van activiteit optimaliseren.

Zie voor meer informatie over SQL Database Advisor [SQL Database Advisor](https://azure.microsoft.com/documentation/articles/sql-database-advisor/).

## <a name="improve-app-service-performance-and-reliability"></a>App Service-prestaties en betrouwbaarheid verbeteren

Azure Advisor kan worden geïntegreerd met aanbevelingen voor het verbeteren van de ervaring van uw App Services en relevante platformmogelijkheden detecteren. Voorbeelden van aanbevelingen voor de App-Services zijn:
* Detectie van exemplaren waar geheugen of CPU-resources worden uitgeput door app-runtimes met opties voor risicobeperking.
* Detectie van exemplaren waar collocating resources, zoals web-apps en -databases kan verbeteren, prestaties en lagere kosten.

Zie voor meer informatie over aanbevelingen voor de App Services [Best Practices voor Azure App Service](https://azure.microsoft.com/documentation/articles/app-service-best-practices/).

## <a name="use-managed-disks-to-prevent-disk-io-throttling"></a>Managed Disks gebruiken om te voorkomen dat de-i/o-schijfbeperking

Advisor identificeert virtuele machines die deel uitmaken van een storage-account dat de schaalbaarheidsdoel bereikt. Dit probleem kunt u de virtuele machines vatbaar voor i/o-beperking. Advisor wordt aangeraden ze Managed Disks gebruiken om te voorkomen dat de systeemprestaties.

## <a name="improve-the-performance-and-reliability-of-virtual-machine-disks-by-using-premium-storage"></a>De prestaties en betrouwbaarheid van virtuele-machineschijven verbeteren met behulp van Premium Storage

Advisor identificeert virtuele machines met standard-schijven waarvoor een groot aantal transacties in uw storage-account en wordt aanbevolen een upgrade naar premium-schijven. 

Azure Premium Storage voorziet in ondersteuning voor hoge prestaties en lage latentie schijven voor virtuele machines die I/O-intensieve workloads uitvoeren. VM-schijven die gebruikmaken van premium storage-accounts worden gegevens opgeslagen op SSD-schijven (SSD's). Voor de beste prestaties voor uw toepassing, wordt u aangeraden dat u alle schijven van virtuele machines waarvoor hoge IOPS naar premium storage gemigreerd.

## <a name="remove-data-skew-on-your-sql-data-warehouse-table-to-increase-query-performance"></a>Gegevensverschil op uw SQL datawarehouse-tabel te verhogen van de prestaties van query's verwijderen

Gegevensverschil kan leiden tot onnodige gegevens knelpunten in verkeer of resource biedt bij het uitvoeren van uw workload. Advisor detecteert distributiegegevens scheeftrekken groter is dan 15% is en aangeraden dat u uw gegevens distribueren en terugkeren naar uw tabel distributie sleutel selecties. Zie voor meer informatie over het identificeren en verwijderen van scheeftrekken [probleemoplossing scheeftrekken](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-distribute#how-to-tell-if-your-distribution-column-is-a-good-choice).

## <a name="create-or-update-outdated-table-statistics-on-your-sql-data-warehouse-table-to-increase-query-performance"></a>Maken of bijwerken van verouderde tabelstatistieken op uw SQL datawarehouse-tabel voor betere queryprestaties

Advisor identificeert de tabellen die u geen recente hebt [tabelstatistieken](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-statistics) en de gebruiker wordt aanbevolen maken of bijwerken van statistieken tabel. Query's optimaliseren up-to-date statische waarden gebruikt om te schatten van de kardinaliteit of het aantal rijen in het queryresultaat waarmee het queryoptimalisatieprogramma om een goede queryplan voor de snelste prestaties te maken voor de SQL data warehouse.

## <a name="scale-up-to-optimize-cache-utilization-on-your-sql-data-warehouse-tables-to-increase-query-performance"></a>Opschalen naar het Optimaliseer het gebruik van de cache voor uw SQL Data Warehouse-tabellen voor betere queryprestaties

Azure Advisor detecteert als uw SQL Data Warehouse beschikt over hoge cache percentage gebruikt en een lage percentage bereikt. Deze voorwaarde geeft aan dat de verwijdering van hoge cache, die kan invloed hebben op de prestaties van uw SQL Data Warehouse. Advisor kan erop wijzen dat u omhoog schalen van uw SQL Data Warehouse om ervoor te zorgen voldoende capaciteit cache voor uw werkbelasting die u toewijst.

## <a name="convert-sql-data-warehouse-tables-to-replicated-tables-to-increase-query-performance"></a>SQL Data Warehouse-tabellen omzetten in gerepliceerde tabellen voor betere queryprestaties

Advisor identificeert de tabellen die geen gerepliceerde tabellen zijn, maar veel voordeel hebben van het converteren van en stelt deze tabellen te converteren. Aanbevelingen zijn gebaseerd op de grootte van gerepliceerde tabel, het aantal kolommen, tabel Distributietype en het aantal partities van de SQL Data Warehouse-tabel. Aanvullende methodiek kan worden opgegeven in de aanbeveling voor context. Zie voor meer informatie over hoe deze aanbeveling wordt bepaald, [aanbevelingen voor SQL Data Warehouse](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-concept-recommendations#replicate-tables). 

## <a name="migrate-your-storage-account-to-azure-resource-manager-to-get-all-of-the-latest-azure-features"></a>Uw Storage-Account migreren naar Azure Resource Manager om de nieuwste functies van Azure

Migreer uw Storage-Account-implementatiemodel naar Azure Resource Manager (Resource Manager) om te profiteren van sjabloonimplementaties, extra beveiligingsopties en de mogelijkheid om te upgraden naar een GPv2-account voor het gebruik van de nieuwste functies van Azure Storage. Advisor identificeert een zelfstandige storage-accounts die van het klassieke implementatiemodel gebruikmaken en beveelt migreren naar het Resource Manager-implementatiemodel.

> [!NOTE]
> Klassieke waarschuwingen in Azure Monitor zijn gepland voor het buiten gebruik stellen in juni 2019. Het is raadzaam dat u uw klassieke storage-account voor het gebruik van Resource Manager te behouden waarschuwingen functionaliteit met het nieuwe platform upgraden. Zie voor meer informatie, [buiten gebruik stellen met klassieke waarschuwingen](https://azure.microsoft.com/updates/classic-alerting-monitoring-retirement/).

## <a name="design-your-storage-accounts-to-prevent-hitting-the-maximum-subscription-limit"></a>Ontwerp uw storage-accounts om te voorkomen dat te maken met de limiet voor het maximum aantal abonnementen

Een Azure-regio kunt ondersteunt maximaal 250 storage-accounts per abonnement. Zodra de limiet is bereikt, kunt u zich niet voor het maken van meer storage accounts in deze combinatie van regio /-abonnement. Advisor controleert uw abonnementen en surface aanbevelingen voor u kunt ontwerpen voor minder storage-accounts voor die zich dicht bij de maximumlimiet is bereikt.

## <a name="optimize-the-performance-of-your-azure-mysql-azure-postgresql-and-azure-mariadb-servers"></a>Optimaliseer de prestaties van uw Azure MySQL, Azure PostgreSQL en Azure MariaDB-servers 

### <a name="fix-the-cpu-pressure-of-your-azure-mysql-azure-postgresql-and-azure-mariadb-servers-with-cpu-bottlenecks"></a>Druk op de CPU van uw Azure MySQL, Azure PostgreSQL en Azure MariaDB-servers met CPU-knelpunten oplossen
Zeer hoog gebruik van de CPU gedurende een langere periode kan leiden tot trage queryprestaties voor uw workload. Vergroten van de CPU te helpen bij het optimaliseren van de runtime van de database-query's en de algehele prestaties verbeteren. Azure Advisor identificeert servers met een hoog CPU-gebruik die waarschijnlijk CPU beperkte workloads worden uitgevoerd en aan te bevelen Computing schalen.

### <a name="reduce-memory-constraints-on-your-azure-mysql-azure-postgresql-and-azure-mariadb-servers-or-move-to-a-memory-optimized-sku"></a>Geheugen beperkingen met betrekking tot uw Azure MySQL, Azure PostgreSQL en Azure MariaDB-servers beperken of verplaatsen naar een voor geheugen geoptimaliseerde SKU
Een lage cache treffers verhouding kan leiden tot tragere prestaties van query's en meer IOPS. Dit kan zijn vanwege een ongeldige query-abonnement of het uitvoeren van een geheugen intensieve workload. Het queryplan oplossen of [verhogen van het geheugen](https://docs.microsoft.com/azure/postgresql/concepts-pricing-tiers) van de Azure Database for PostgreSQL-databaseserver, Azure MySQL-database-server of Azure MariaDB server ervoor dat de uitvoering van de database-werkbelasting te optimaliseren. Azure Advisor identificeert servers die worden beïnvloed door dit verloop van de pool hoog Buffergebruik en raadt aan om een van beide het verhelpen van het queryplan verplaatsen naar een hogere SKU met meer geheugen en opslag vergroten om meer IOPS.

### <a name="use-a-azure-mysql-or-azure-postgresql-read-replica-to-scale-out-reads-for-read-intensive-workloads"></a>Een Azure MySQL of een Azure PostgreSQL-Leesreplica gebruiken om uit leesbewerkingen voor lees-intensieve workloads te schalen
Azure Advisor maakt gebruik van heuristiek op basis van een werkbelasting, zoals de verhouding van leesbewerkingen en schrijfbewerkingen op de server gedurende de afgelopen zeven dagen voor het identificeren van lees-intensieve werkbelastingen. Uw Azure database voor PostgreSQL-bron of de Azure database voor MySQL-resource met een verhouding van zeer hoge lezen/schrijven kan resulteren in CPU-en/of geheugen contentions leiden tot trage prestaties van query's. Toevoegen van een [replica](https://docs.microsoft.com/azure/postgresql/howto-read-replicas-portal) helpt bij het uitschalen van leesbewerkingen op de replica-server te voorkomen dat CPU-en/of geheugen beperkingen op de primaire server. Advisor worden servers geïdentificeerd met dergelijke hoge Lees-intensieve werkbelastingen, en kunt het beste toevoegen een [replica lezen](https://docs.microsoft.com/azure/postgresql/concepts-read-replicas) voor de offload van enkele van de werkbelastingen voor lezen.


### <a name="scale-your-azure-mysql-azure-postgresql-or-azure-mariadb-server-to-a-higher-sku-to-prevent-connection-constraints"></a>Schalen van uw Azure MySQL en Azure PostgreSQL en Azure MariaDB-server naar een hogere SKU om te voorkomen dat verbindingsbeperkingen
Elke nieuwe verbinding met uw databaseserver geheugen in beslag neemt. Prestaties van de database-server minder als verbindingen met uw server vanwege mislukken een [bovengrens](https://docs.microsoft.com/azure/postgresql/concepts-limits) in het geheugen. Azure Advisor worden geïdentificeerd van servers met veel fouten bij het verbinden met en kunt het beste een upgrade van uw server verbindingen limieten voor meer geheugen aan de server door omhoog schalen van compute of geheugen geoptimaliseerd SKU's, waarvoor meer rekencapaciteit per kern.

## <a name="scale-your-cache-to-a-different-size-or-sku-to-improve-cache-and-application-performance"></a>Schaal uw Cache naar een andere grootte of SKU ter verbetering van de Cache en de prestaties van toepassingen

Cache-exemplaren het beste presteren wanneer niet wordt uitgevoerd onder zware belasting op het hoge geheugen-, hoge serverbelasting of hoge netwerkbandbreedte waardoor deze niet meer reageren, er gegevens verloren gaan of niet beschikbaar. Advisor wordt Cache-exemplaren in deze voorwaarden identificeert en aanbeveelt van de toepassing van aanbevolen procedures voor het verminderen van de geheugendruk, de belasting van de server of de bandbreedte van het netwerk of naar een andere grootte of SKU te schalen met meer capaciteit.

## <a name="add-regions-with-traffic-to-your-azure-cosmos-db-account"></a>Regio's met verkeer toevoegen aan uw Azure Cosmos DB-account

Advisor detecteert Azure Cosmos DB-accounts waarvoor verkeer vanuit een regio die momenteel niet is geconfigureerd en aan toe te voegen die regio. Dit latentie voor aanvragen die afkomstig zijn van deze regio wordt verbeterd en zorgt ervoor dat de beschikbaarheid in het geval van uitval van de regio. [Meer informatie over het distribueren van globale gegevens met Azure Cosmos DB](https://aka.ms/cosmos/globaldistribution)

## <a name="configure-your-azure-cosmos-db-indexing-policy-with-customer-included-or-excluded-paths"></a>Configureren van uw Azure Cosmos DB indexeringsbeleid met klant opgenomen of uitgesloten paden

Azure Advisor identificeert Cosmos DB-containers die gebruikmaakt van de standaardbeleidsregels voor indexering van beleid, maar kunnen profiteren van een aangepast indexeringsbeleid op basis van het workloadpatroon. De standaardbeleidsregels voor indexering voor alle eigenschappen geïndexeerd, maar de RU's en de voor indexering verbruikte opslag met behulp van een aangepast indexeringsbeleid met expliciete opgenomen of uitgesloten paden gebruikt in queryfilters kunt beperken. [Meer informatie over het wijzigen van beleid voor index](https://aka.ms/cosmosdb/modify-index-policy)

## <a name="configure-your-azure-cosmos-db-query-page-size-maxitemcount-to--1"></a>Configureren van uw Azure Cosmos DB-query paginagrootte (MaxItemCount) op-1 

Azure Advisor identificeert Azure Cosmos DB-containers die de paginagrootte voor query van 100 en om de paginagrootte van een 1 te gebruiken voor snellere scans. [Meer informatie over het maximumaantal van Item](https://aka.ms/cosmosdb/sql-api-query-metrics-max-item-count)

## <a name="how-to-access-performance-recommendations-in-advisor"></a>Toegang tot de aanbevelingen voor prestaties in Advisor

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en open vervolgens [Advisor](https://aka.ms/azureadvisordashboard).

2.  Klik op de Advisor-dashboard op de **prestaties** tabblad.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over aanbevelingen van Advisor:

* [Inleiding tot Advisor](advisor-overview.md)
* [Aan de slag met Advisor](advisor-get-started.md)
* [Aanbevelingen van Advisor-kosten](advisor-performance-recommendations.md)
* [Advisor-aanbevelingen voor hoge beschikbaarheid](advisor-high-availability-recommendations.md)
* [Advisor-aanbevelingen voor beveiliging](advisor-security-recommendations.md)
