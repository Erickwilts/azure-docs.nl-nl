---
title: Verbeter de prestaties van Azure-apps met Advisor
description: Gebruik aanbevelingen voor prestaties in Azure Advisor om de snelheid en reactie tijd van uw bedrijfskritische toepassingen te verbeteren.
ms.topic: article
ms.date: 01/29/2019
ms.openlocfilehash: bdca8cd39427fb0d25f8b3308eaf2be24e0eb81a
ms.sourcegitcommit: ef055468d1cb0de4433e1403d6617fede7f5d00e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2020
ms.locfileid: "88257455"
---
# <a name="improve-the-performance-of-azure-applications-by-using-azure-advisor"></a>Verbeter de prestaties van Azure-toepassingen met behulp van Azure Advisor

De aanbevelingen voor prestaties in Azure Advisor kunnen helpen de snelheid en reactie tijd van uw bedrijfskritische toepassingen te verbeteren. U kunt de prestatie aanbevelingen van Advisor op het tabblad **prestaties** van het Advisor-dash board ontvangen.

## <a name="reduce-dns-time-to-live-on-your-traffic-manager-profile-to-fail-over-to-healthy-endpoints-faster"></a>Verminder DNS-time-to-Live op uw Traffic Manager profiel om sneller een failover uit te kunnen geven naar gezonde eind punten

U kunt [TTL-instellingen (time-to-Live)](../traffic-manager/traffic-manager-performance-considerations.md) gebruiken in uw Azure Traffic Manager-profiel om op te geven hoe snel eind punten moeten worden geswitcheerd als een bepaald eind punt niet meer reageert op query's. Als u de TTL-waarden reduceert, worden clients sneller gerouteerd naar werkende eind punten.

Azure Advisor identificeert Traffic Manager profielen met een langere TTL-configuratie. U kunt het beste de TTL-waarde instellen op 20 seconden of 60 seconden, afhankelijk van het feit of het profiel is geconfigureerd voor [snelle failover](https://azure.microsoft.com/roadmap/fast-failover-and-tcp-probing-in-azure-traffic-manager/).

## <a name="improve-database-performance-by-using-sql-database-advisor-temporarily-disabled"></a>Prestaties van de data base verbeteren met behulp van SQL Database Advisor (tijdelijk uitgeschakeld)

Azure Advisor biedt een consistente, geconsolideerde weer gave van aanbevelingen voor al uw Azure-resources. Het integreert met SQL Database Advisor om u aanbevelingen te doen voor het verbeteren van de prestaties van uw data bases.SQL Database Advisor evalueert de prestaties van uw data bases door de gebruiks geschiedenis te analyseren. Vervolgens worden aanbevelingen geboden die het meest geschikt zijn voor het uitvoeren van de normale werk belasting van de data base.

> [!NOTE]
> Voordat u aanbevelingen kunt krijgen, moet uw data base gedurende een week worden gebruikt en moet er binnen die week een consistente activiteit zijn. SQL Database Advisor kan gemakkelijker worden geoptimaliseerd voor consistente query patronen dan voor wille keurige bursts van de activiteit.

Zie [SQL database Advisor](../azure-sql/database/database-advisor-implement-performance-recommendations.md)voor meer informatie.

## <a name="upgrade-your-storage-client-library-to-the-latest-version-for-better-reliability-and-performance"></a>Upgrade uw Storage-client bibliotheek naar de nieuwste versie voor betere betrouw baarheid en prestaties

De nieuwste versie van de SDK voor Storage-client bibliotheek bevat oplossingen voor problemen die door klanten worden gemeld en proactief worden geïdentificeerd via onze QA-procedure. De nieuwste versie bevat ook optimalisatie van betrouw baarheid en prestaties samen met nieuwe functies waarmee u uw algemene ervaring kunt verbeteren met behulp van Azure Storage. Advisor biedt aanbevelingen en stappen die nodig zijn om een upgrade uit te voeren naar de nieuwste versie van de SDK als u een verouderde versie gebruikt. De aanbevelingen zijn voor ondersteunde talen: C++ en .NET.

## <a name="improve-app-service-performance-and-reliability"></a>De prestaties en betrouw baarheid van App Service verbeteren

Azure Advisor integreert aanbevelingen voor het verbeteren van uw App Service ervaring en het detecteren van relevante platform mogelijkheden. Voor beelden van App Service aanbevelingen zijn:
* Detectie van exemplaren waarbij geheugen-of CPU-bronnen worden uitgeput door app-runtime, met opties voor risico beperking.
* Detectie van exemplaren waarbij het door co-zoeken van bronnen, zoals web-apps en data bases, de prestaties kan verbeteren en de kosten kan verlagen.

Zie [Aanbevolen procedures voor Azure app service](../app-service/app-service-best-practices.md)voor meer informatie.

## <a name="use-managed-disks-to-prevent-disk-io-throttling"></a>Managed disks gebruiken om schijf-I/O-beperking te voor komen

Advisor identificeert virtuele machines die deel uitmaken van een opslag account dat het schaalbaarheids doel bereikt. Deze voor waarde zorgt ervoor dat de Vm's vatbaar zijn voor I/O-beperking. Advisor wordt aangeraden Managed disks te gebruiken om te voor komen dat de prestaties verslechteren.

## <a name="improve-the-performance-and-reliability-of-virtual-machine-disks-by-using-premium-storage"></a>Verbeter de prestaties en betrouw baarheid van virtuele-machine schijven met behulp van Premium Storage

Advisor identificeert virtuele machines met standaard schijven met een groot aantal trans acties op uw opslag account. Het wordt aanbevolen om te upgraden naar Premium-schijven. 

Azure Premium Storage biedt schijf ondersteuning met hoge prestaties en lage latentie voor virtuele machines die I/O-intensieve workloads uitvoeren. Virtuele-machine schijven die gebruikmaken van Premium Storage accounts slaan gegevens op Ssd's (Solid-state drives) op. Voor de beste prestaties van uw toepassing wordt u aangeraden alle schijven voor virtuele machines te migreren die hoge IOPS vereisen om te Premium Storage.

## <a name="remove-data-skew-on-your-sql-data-warehouse-tables-to-increase-query-performance"></a>Verscheefing van gegevens in uw SQL Data Warehouse tabellen verwijderen om de query prestaties te verbeteren

Gegevens scheefheid kan leiden tot onnodige gegevens verplaatsing of bron knelpunten wanneer u uw workload uitvoert. Advisor detecteert distributie gegevens scheefheid van meer dan 15%. U wordt aangeraden uw gegevens opnieuw te distribueren en uw tabel distributie sleutel selecties opnieuw te bezoeken. Zie [problemen met scheef trekken oplossen](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-tables-distribute.md#how-to-tell-if-your-distribution-column-is-a-good-choice)voor meer informatie over het identificeren en verwijderen van scheefheid.

## <a name="create-or-update-outdated-table-statistics-in-your-sql-data-warehouse-tables-to-increase-query-performance"></a>Verouderde tabel statistieken in uw SQL Data Warehouse tabellen maken of bijwerken om de query prestaties te verbeteren

Advisor identificeert tabellen die geen actuele [tabel statistieken](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-tables-statistics.md) hebben en raadt u aan om de statistieken te maken of bij te werken. De query Optimizer in Azure SQL Data Warehouse maakt gebruik van actuele statistieken om de kardinaliteit of het aantal rijen in query resultaten te schatten. Met deze schattingen kan het query optimalisatie programma een query plan maken om de snelste prestaties te bieden.

## <a name="improve-mysql-connection-management"></a>MySQL-verbindingsbeheer verbeteren

Advisor-analyse kan erop wijzen dat de toepassing die verbinding maakt met een MySQL-server mogelijk niet efficiënt beheert. Dit probleem kan leiden tot onnodig Resource verbruik en een totale latentie van een hogere toepassing. We raden u aan om het aantal kortdurende verbindingen te beperken en onnodige niet-actieve verbindingen te elimineren om verbindingsbeheer te verbeteren. U kunt deze verbeteringen aanbrengen door een verbindings groep aan de server zijde te configureren, zoals ProxySQL.


## <a name="scale-up-to-optimize-cache-utilization-on-your-sql-data-warehouse-tables-to-increase-query-performance"></a>Schaal omhoog om het cache gebruik op uw SQL Data Warehouse tabellen te optimaliseren om de query prestaties te verbeteren

Azure Advisor detecteert of uw SQL Data Warehouse tabellen een hoog percentage voor cache gebruik en een lage treffer percentage hebben. Deze voor waarde duidt op hoge cache verwijdering, wat van invloed kan zijn op de prestaties van uw SQL Data Warehouse-exemplaar. Advisor raadt u aan om uw SQL Data Warehouse-exemplaar te schalen, zodat u voldoende cache capaciteit toewijst voor uw werk belasting.

## <a name="convert-sql-data-warehouse-tables-to-replicated-tables-to-increase-query-performance"></a>SQL Data Warehouse tabellen converteren naar gerepliceerde tabellen om de query prestaties te verbeteren

Advisor identificeert tabellen die geen gerepliceerde tabellen zijn, maar die zouden kunnen worden geconverteerd. U wordt aangeraden deze tabellen te converteren. Aanbevelingen zijn gebaseerd op:
- De grootte van de gerepliceerde tabel. 
- Het aantal kolommen. 
- Het distributie type van de tabel. 
- Het aantal partities op de SQL Data Warehouse tabel. 

Extra heuristiek kan worden gegeven in de aanbeveling voor context. Zie [SQL Data Warehouse aanbevelingen](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-concept-recommendations.md#replicate-tables)voor meer informatie over hoe deze aanbeveling wordt bepaald. 

## <a name="migrate-your-storage-account-to-azure-resource-manager-to-get-the-latest-azure-features"></a>Uw opslag account migreren naar Azure Resource Manager om de nieuwste Azure-functies op te halen

Migreer het implementatie model van uw opslag account naar Azure Resource Manager om te profiteren van:
- Sjabloon implementaties.
- Aanvullende beveiligings opties.
- De mogelijkheid om te upgraden naar een GPv2-account zodat u de nieuwste Azure Storage-functies kunt gebruiken. 

Advisor identificeert zelfstandige opslag accounts die gebruikmaken van het klassieke implementatie model en raadt aan de migratie naar het Resource Manager-implementatie model te migreren.

> [!NOTE]
> Klassieke waarschuwingen in Azure Monitor zijn in augustus 2019 buiten gebruik gesteld. We raden u aan uw klassieke opslag account bij te werken om Resource Manager te gebruiken om de functionaliteit van waarschuwingen met het nieuwe platform te behouden. Zie [klassieke waarschuwingen buiten](../azure-monitor/platform/monitoring-classic-retirement.md#retirement-of-classic-monitoring-and-alerting-platform)gebruik stellen voor meer informatie.

## <a name="design-your-storage-accounts-to-prevent-reaching-the-maximum-subscription-limit"></a>Uw opslag accounts ontwerpen om te voor komen dat de maximale abonnements limiet wordt bereikt

Een Azure-regio ondersteunt Maxi maal 250 opslag accounts per abonnement. Wanneer deze limiet is bereikt, kunt u geen opslag accounts meer maken in die regio/abonnements combinatie. Advisor controleert uw abonnementen en biedt aanbevelingen die u kunt ontwerpen voor minder opslag accounts voor elk abonnement/regio dat bijna de maximale limiet bereikt.

## <a name="consider-increasing-the-size-of-your-vpn-gateway-sku-to-address-high-p2s-use"></a>Overweeg de omvang van uw VPN Gateway-SKU te verg Roten om een hoog P2S gebruik te aanpakken

Elke Azure VPN Gateway SKU kan slechts een opgegeven aantal gelijktijdige P2S-verbindingen ondersteunen. Als de verbinding tot stand is gebracht, kunnen er extra Verbindings pogingen mislukken. Als u de grootte van uw gateway verg root, kunt u meer gelijktijdige P2S-gebruikers ondersteunen. Advisor biedt aanbevelingen en instructies voor het verg Roten van de grootte van uw gateway.

## <a name="consider-increasing-the-size-of-your-vpn-gateway-sku-to-address-high-cpu"></a>Overweeg de omvang van uw VPN Gateway SKU te verg Roten voor een hoge CPU

Bij een hoge belasting van het verkeer kan uw VPN-gateway pakketten verwijderen vanwege een hoge CPU. Overweeg om uw VPN Gateway SKU bij te werken. Door de grootte van uw VPN-gateway te verg Roten, zorgt u ervoor dat de verbindingen niet worden verbroken vanwege een hoge CPU. Advisor biedt aanbevelingen om dit probleem proactief op te lossen. 

## <a name="increase-batch-size-when-loading-to-maximize-load-throughput-data-compression-and-query-performance"></a>Batchgrootte vergroten tijdens het laden om de laaddoorvoer, gegevenscompressie en queryprestaties te maximaliseren

Advisor detecteert of u de belasting prestaties en door Voer kunt verhogen door de Batch grootte te verg Roten tijdens het laden in uw data base. U kunt overwegen de instructie COPY te gebruiken. Als u de instructie COPY niet kunt gebruiken, kunt u overwegen de Batch grootte te verg Roten wanneer u hulpprogram ma's voor laden gebruikt, zoals de SQLBulkCopy-API of BCP. Een goede algemene regel is het gebruik van een batch grootte tussen 100.000 en 1.000.000 rijen. Door de Batch grootte te verg Roten, wordt de belasting door Voer, gegevens compressie en query prestaties verbeterd.

## <a name="co-locate-the-storage-account-in-the-same-region-to-minimize-latency-when-loading"></a>Het opslag account in dezelfde regio samen zoeken om de latentie bij het laden te minimaliseren

Advisor detecteert of u laadt vanuit een regio die afwijkt van de SQL-groep. Overweeg het laden van een opslag account in dezelfde regio als uw SQL-groep om de latentie te minimaliseren bij het laden van gegevens. Deze wijziging helpt de latentie te minimaliseren en de belasting prestaties te verg Roten.

## <a name="use-a-supported-kubernetes-version"></a>Een ondersteunde Kubernetes-versie gebruiken

Advisor detecteert niet-ondersteunde versies van Kubernetes.

## <a name="optimize-the-performance-of-your-azure-database-for-mysql-azure-database-for-postgresql-and-azure-database-for-mariadb-servers"></a>Optimaliseer de prestaties van uw Azure Database for MySQL, Azure Database for PostgreSQL en Azure Database for MariaDB servers

### <a name="fix-the-cpu-pressure-of-your-azure-database-for-mysql-azure-database-for-postgresql-and-azure-database-for-mariadb-servers-with-cpu-bottlenecks"></a>De CPU-druk van uw Azure Database for MySQL, Azure Database for PostgreSQL en Azure Database for MariaDB servers met CPU-knel punten oplossen
Intensief gebruik van de CPU gedurende een langere periode kan de prestaties van uw werk belasting vertragen. Het verg Roten van de CPU-grootte helpt de runtime van de database query's te optimaliseren en de algehele prestaties te verbeteren. Advisor identificeert servers met een hoog CPU-gebruik waarvoor waarschijnlijk CPU-beperkte werk belastingen worden uitgevoerd en raadt u aan om uw reken kracht te schalen.

### <a name="reduce-memory-constraints-on-your-azure-database-for-mysql-azure-database-for-postgresql-and-azure-database-for-mariadb-servers-or-move-to-a-memory-optimized-sku"></a>Verminder de geheugen beperkingen op uw Azure Database for MySQL-, Azure Database for PostgreSQL-en Azure Database for MariaDB-servers of ga naar een voor geheugen geoptimaliseerde SKU
Een lage cache verhouding kan leiden tot tragere query prestaties en meer IOPS. Dit probleem kan worden veroorzaakt door een beschadigd query plan of een geheugenintensieve werk belasting. Het herstellen van het query plan of het [verg Roten](../postgresql/concepts-pricing-tiers.md) van het geheugen van de Azure Database for PostgreSQL, Azure Database for MySQL of Azure database for MariaDB server helpt de uitvoering van de werk belasting van de data base te optimaliseren. Azure Advisor identificeert servers die worden beïnvloed door deze hoge buffer van groeps verloop. U wordt aangeraden een van de volgende acties uit te voeren: 
- Het query plan herstellen
- Ga naar een SKU met meer geheugen 
- Verg root de opslag ruimte om meer IOPS te verkrijgen.

### <a name="use-an-azure-database-for-mysql-or-azure-database-for-postgresql-read-replica-to-scale-out-reads-for-read-intensive-workloads"></a>Een Azure Database for MySQL of Azure Database for PostgreSQL een replica lezen om Lees bewerkingen voor lees-intensieve workloads uit te schalen
Advisor maakt gebruik van methodieken op basis van werk belastingen, zoals de verhouding van Lees bewerkingen op de server gedurende de afgelopen zeven dagen om Lees-intensieve workloads te identificeren. Een Azure Database for PostgreSQL-of Azure Database for MySQL resource met een hoge lees-en schrijf ratio kan leiden tot CPU-of geheugen conflicten en leiden tot trage query prestaties. Het toevoegen van een [replica](../postgresql/howto-read-replicas-portal.md) helpt bij het uitschalen van Lees bewerkingen naar de replica server en voor komt CPU-of geheugen beperkingen op de primaire server. Advisor identificeert servers met lees-intensieve workloads en raadt u aan een [Lees replica](../postgresql/concepts-read-replicas.md)toe te voegen   voor het offloaden van sommige Lees workloads.


### <a name="scale-your-azure-database-for-mysql-azure-database-for-postgresql-or-azure-database-for-mariadb-server-to-a-higher-sku-to-prevent-connection-constraints"></a>Uw Azure Database for MySQL, Azure Database for PostgreSQL of Azure Database for MariaDB server schalen naar een hogere SKU om verbindings beperkingen te voor komen
Elke nieuwe verbinding met uw database server neemt geheugen in beslag. De prestaties van de database server verslechteren als verbindingen met uw server mislukken vanwege een [bovenlimiet](../postgresql/concepts-limits.md) in het geheugen. Azure Advisor identificeert servers met veel verbindings fouten. Het wordt aanbevolen om de verbindings limieten van uw server te upgraden om meer geheugen beschikbaar te maken voor uw server door een van de volgende acties uit te voeren:
- De reken kracht omhoog schalen. 
- Gebruik Sku's die zijn geoptimaliseerd voor geheugen en meer reken kracht per kern.

## <a name="scale-your-cache-to-a-different-size-or-sku-to-improve-cache-and-application-performance"></a>Schaal uw cache op een andere grootte of SKU om de cache-en toepassings prestaties te verbeteren

Cache-instanties worden het beste uitgevoerd wanneer ze niet worden uitgevoerd onder hoge geheugen belasting, hoge server belasting of hoge netwerk bandbreedte. Deze omstandigheden kunnen ervoor zorgen dat ze niet meer reageren, verlies van gegevens ondervinden of niet meer beschikbaar zijn. Advisor identificeert cache-exemplaren in deze omstandigheden. U wordt aangeraden een van de volgende acties uit te voeren:
- Pas aanbevolen procedures toe om de geheugen belasting, server belasting of netwerk bandbreedte te verlagen.
- Schaal naar een andere grootte of SKU met meer capaciteit.

## <a name="add-regions-with-traffic-to-your-azure-cosmos-db-account"></a>Regio's met verkeer toevoegen aan uw Azure Cosmos DB-account

Advisor detecteert Azure Cosmos DB accounts die verkeer hebben van een regio die momenteel niet is geconfigureerd. Het wordt aanbevolen om die regio toe te voegen. Hierdoor wordt de latentie verbeterd van aanvragen die afkomstig zijn uit die regio en wordt de beschik baarheid in het geval van regionale storingen gegarandeerd. [Meer informatie over de distributie van globale gegevens met Azure Cosmos DB.](https://aka.ms/cosmos/globaldistribution)

## <a name="configure-your-azure-cosmos-db-indexing-policy-by-using-custom-included-or-excluded-paths"></a>Uw Azure Cosmos DB-indexerings beleid configureren met behulp van aangepaste, opgenomen of uitgesloten paden

Advisor identificeert Azure Cosmos DB containers die gebruikmaken van het standaard indexerings beleid, maar kunnen wel profiteren van een aangepast indexerings beleid. Deze bepaling is gebaseerd op het werkbelasting patroon. Met het standaard indexerings beleid worden alle eigenschappen geïndexeerd. Een aangepast indexerings beleid met expliciet opgenomen of uitgesloten paden die in query filters worden gebruikt, kan het RUs-en opslag verbruik voor indexering verminderen. [Meer informatie over het wijzigen van index beleid.](https://aka.ms/cosmosdb/modify-index-policy)

## <a name="set-your-azure-cosmos-db-query-page-size-maxitemcount-to--1"></a>Stel het pagina formaat van uw Azure Cosmos DB-query (MaxItemCount) in op-1 

Azure Advisor identificeert Azure Cosmos DB containers die gebruikmaken van een query pagina grootte van 100. Het wordt aanbevolen een pagina grootte van-1 te gebruiken voor snellere scans. [Meer informatie over MaxItemCount.](https://aka.ms/cosmosdb/sql-api-query-metrics-max-item-count)

## <a name="consider-using-accelerated-writes-feature-in-your-hbase-cluster-to-improve-cluster-performance"></a>U kunt de functie versneld schrijven gebruiken in uw HBase-cluster om de prestaties van het cluster te verbeteren
Azure Advisor analyseert de systeem Logboeken in de afgelopen 7 dagen en identificeert of uw cluster de volgende scenario's heeft aangetroffen:
1. Hoge latentie voor WAL-synchronisatietijd 
2. Hoog aantal schrijfaanvragen (minstens drie periodes van één uur met meer dan 1000 avg_write_requests/second/node)

Deze kenmerken geven aan dat uw cluster een hoge latentie voor schrijven heeft. Dit kan worden veroorzaakt door een zware werk belasting die op uw cluster wordt uitgevoerd. Als u de prestaties van uw cluster wilt verbeteren, kunt u overwegen om gebruik te maken van de functie voor versneld schrijven van Azure HDInsight HBase. Met de functie Versnelde schrijfbewerkingen voor HDInsight Apache HBase-clusters worden Premium SSD-beheerde schijven gekoppeld aan elke RegionServer (werkknooppunt) in plaats van dat er cloudopslag wordt gebruikt. Het gevolg hiervan is een lage latentie voor schrijven en een hogere tolerantie voor uw toepassingen. Meer informatie over deze functie vindt u [hier](https://docs.microsoft.com/azure/hdinsight/hbase/apache-hbase-accelerated-writes#how-to-enable-accelerated-writes-for-hbase-in-hdinsight)

## <a name="review-azure-data-explorer-table-cache-period-policy-for-better-performance-preview"></a>De cache-periode (beleid) van de Azure Data Explorer-tabel controleren voor betere prestaties (preview-versie)
Deze aanbeveling heeft betrekking op Azure Data Explorer-tabellen die een groot aantal query's hebben die verder dan de geconfigureerde cacheperiode (beleid) zoeken (u ziet de top 10 van tabellen op querypercentage dat toegang heeft tot de gegevens buiten de cache). De aanbevolen actie voor het verbeteren van de prestaties van het cluster: Beperk query's voor deze tabel tot het minimale tijdsbereik dat nodig is (binnen het gedefinieerde beleid). Als er gegevens uit het hele tijdsbereik vereist zijn, verhoogt u de cacheperiode tot de aanbevolen waarde.

## <a name="improve-performance-by-optimizing-mysql-temporary-table-sizing"></a>Prestaties verbeteren door grootteaanpassing van tijdelijke tabel van MySQL te optimaliseren
Advisor-analyse geeft aan dat uw MySQL-server mogelijk niet langer I/O-overhead mag zijn als gevolg van instellingen met een lage tijdelijke-tabel parameter. Dit kan leiden tot onnodige schijftransacties en verminderde prestaties. We raden u aan de waarden voor de parameter 'tmp_table_size' en 'max_heap_table_size' te verhogen om het aantal schijftransacties te verminderen. [Meer informatie](https://aka.ms/azure_mysql_tmp_table)

## <a name="distribute-data-in-server-group-to-distribute-workload-among-nodes"></a>Gegevens in Server groep distribueren om de werk belasting tussen knoop punten te verdelen
Advisor identificeert de Server groepen waar de gegevens niet zijn gedistribueerd, maar blijven op de coördinator. Op basis hiervan adviseert Advisor dat voor volledige grootschalige-voor delen (Citus) gegevens worden gedistribueerd op worker-knoop punten voor uw server groepen. Hierdoor worden de prestaties van de query verbeterd door gebruik te maken van resource van elk knoop punt in de Server groep. [Meer informatie](https://go.microsoft.com/fwlink/?linkid=2135201) 

## <a name="how-to-access-performance-recommendations-in-advisor"></a>Aanbevelingen voor toegang tot prestatie verbeteringen in Advisor

1. Meld u aan bij de [Azure Portal](https://portal.azure.com)en open vervolgens [Advisor](https://aka.ms/azureadvisordashboard).

2.  Op het Advisor-dash board selecteert u het tabblad **prestaties** .

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Advisor-aanbevelingen:

* [Inleiding tot Advisor](advisor-overview.md)
* [Aan de slag met Advisor](advisor-get-started.md)
* [Aanbevelingen van Advisor met betrekking tot kosten](advisor-cost-recommendations.md)
* [Aanbevelingen voor de Advisor-betrouw baarheid](advisor-high-availability-recommendations.md)
* [Aanbevelingen voor de beveiliging van Advisor](advisor-security-recommendations.md)
* [Aanbevelingen voor operationele uitmuntendheid van Advisor](advisor-operational-excellence-recommendations.md)
* [Advisor REST API](/rest/api/advisor/)
