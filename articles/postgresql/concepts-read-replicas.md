---
title: Replica's lezen-Azure Database for PostgreSQL-één server
description: In dit artikel wordt de functie voor het lezen van replica's in Azure Database for PostgreSQL-één server beschreven.
author: sr-msft
ms.author: srranga
ms.service: postgresql
ms.topic: conceptual
ms.date: 10/15/2020
ms.openlocfilehash: 7f81e6182209e29e41a21abadbaf05518844d201
ms.sourcegitcommit: 3bcce2e26935f523226ea269f034e0d75aa6693a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/23/2020
ms.locfileid: "92490166"
---
# <a name="read-replicas-in-azure-database-for-postgresql---single-server"></a>Replica's lezen in Azure Database for PostgreSQL-één server

Met de functie replica lezen kunt u gegevens van een Azure Database for PostgreSQL server repliceren naar een alleen-lezen server. U kunt van de primaire server naar Maxi maal vijf replica's repliceren. Replica's worden asynchroon bijgewerkt met de systeemeigen replicatietechnologie van de PostgreSQL-engine.

Replica's zijn nieuwe servers die u op een soortgelijke manier beheert als gewone Azure Database for PostgreSQL-servers. Voor elke Lees replica wordt u gefactureerd voor de ingerichte Compute in vCores en Storage in GB/maand.

Meer informatie over het [maken en beheren van replica's](howto-read-replicas-portal.md).

## <a name="when-to-use-a-read-replica"></a>Wanneer moet u een lees replica gebruiken?
De functie voor het lezen van replica's helpt bij het verbeteren van de prestaties en schaal baarheid van Lees bare werk belastingen. Lees werkbelastingen kunnen worden geïsoleerd voor de replica's, terwijl schrijf werkbelastingen kunnen worden omgeleid naar de primaire.

Een veelvoorkomend scenario is om BI-en analytische werk belastingen de Lees replica te laten gebruiken als gegevens bron voor rapportage.

Omdat replica's alleen-lezen zijn, beperken ze niet rechtstreeks de overhead van de schrijf capaciteit op de primaire. Deze functie is niet gericht op schrijfintensieve werkbelastingen.

De functie voor het lezen van replica's maakt gebruik van asynchrone PostgreSQL-replicatie. De functie is niet bedoeld voor synchrone replicatie scenario's. Er is een meet bare vertraging tussen de primaire en de replica. De gegevens op de replica worden uiteindelijk consistent met de gegevens op de primaire. Gebruik deze functie voor werk belastingen die deze vertraging kunnen bevatten.

## <a name="cross-region-replication"></a>Replicatie in meerdere regio's
U kunt een lees replica maken in een andere regio dan de primaire server. Replicatie tussen regio's kan handig zijn voor scenario's zoals het plannen van herstel na nood gevallen of gegevens dichter bij uw gebruikers te brengen.

>[!NOTE]
> Basic-laag servers ondersteunen alleen replicatie met dezelfde regio.

U kunt een primaire server in een [Azure database for PostgreSQL regio](https://azure.microsoft.com/global-infrastructure/services/?products=postgresql)hebben. Een primaire server kan een replica hebben in het gekoppelde gebied of in de universele replica regio's. In de onderstaande afbeelding ziet u welke replica regio's er beschikbaar zijn, afhankelijk van de primaire regio.

[:::image type="content" source="media/concepts-read-replica/read-replica-regions.png" alt-text="Replica regio's lezen":::](media/concepts-read-replica/read-replica-regions.png#lightbox)

### <a name="universal-replica-regions"></a>Universele replica regio's
U kunt altijd een lees replica maken in een van de volgende regio's, ongeacht waar de primaire server zich bevindt. Dit zijn de universele replica regio's:

Australië-oost, Australië-zuidoost, Brazilië-zuid, Canada-centraal, Canada-oost, VS, Azië-oost, VS-Oost, VS-Oost 2, Japan-Oost, Japan-West, Korea-centraal, Korea-zuid, Noord-Centraal VS, Europa-noord, Zuid-Centraal VS, Zuidoost-Azië, UK-zuid, UK-west, Europa-west, VS-West, VS-West-Centraal vs.

### <a name="paired-regions"></a>Gekoppelde regio's
Naast de universele replica regio's, kunt u een lees replica maken in het gekoppelde Azure-gebied van de primaire server. Als u het paar van uw regio niet weet, kunt u meer informatie vinden in het [artikel gekoppelde regio's in azure](../best-practices-availability-paired-regions.md).

Als u verschillende regio's replica's gebruikt voor het plannen van herstel na nood gevallen, raden we u aan om de replica in het gekoppelde gebied te maken in plaats van een van de andere regio's. Gekoppelde regio's vermijden gelijktijdige updates en geven geen prioriteiten voor fysieke isolatie en gegevens locatie.  

U moet rekening houden met de volgende beperkingen: 

* Regionale Beschik baarheid: Azure Database for PostgreSQL is beschikbaar in Frankrijk-centraal, UAE-noord en Duitsland-centraal. De gekoppelde regio's zijn echter niet beschikbaar.
    
* Uni-directionele paren: sommige Azure-regio's zijn in slechts één richting gekoppeld. Deze regio's omvatten West-India, Brazilië-zuid. 
   Dit betekent dat een primaire server in West-India een replica kan maken in India-zuid. Een primaire server in India-zuid kan echter geen replica maken in West-India. Dit komt doordat de secundaire regio van West-India India-zuid is, India-zuid maar de secundaire regio van het westen is niet West-India.


## <a name="create-a-replica"></a>Replica's maken
Wanneer u de werk stroom voor het maken van de replica start, wordt er een lege Azure Database for PostgreSQL-server gemaakt. De nieuwe server wordt gevuld met de gegevens die zich op de primaire server bevonden. De aanmaak tijd is afhankelijk van de hoeveelheid gegevens op de primaire en de tijd sinds de laatste wekelijkse volledige back-up. De tijd kan variëren van een paar minuten tot enkele uren.

Elke replica is ingeschakeld voor [automatische groei](concepts-pricing-tiers.md#storage-auto-grow)van opslag. Met de functie voor automatisch uitbreiden kan de replica de gegevens repliceren, en wordt voor komen dat de replicatie wordt onderbroken vanwege onvoldoende opslag fouten.

De functie voor het lezen van replica's maakt gebruik van fysieke PostgreSQL-replicatie, geen logische replicatie. Het streamen van replicatie via replicatie sleuven is de standaard bewerkings modus. Als dat nodig is, wordt de back-upfunctie voor logboek registratie gebruikt voor het bijwerken.

Meer informatie over [het maken van een lees replica in de Azure Portal](howto-read-replicas-portal.md).

## <a name="connect-to-a-replica"></a>Verbinding maken met een replica
Wanneer u een replica maakt, neemt deze de firewall regels of het VNet-service-eind punt van de primaire server niet over. Deze regels moeten onafhankelijk worden ingesteld voor de replica.

De replica neemt het beheerders account over van de primaire server. Alle gebruikers accounts op de primaire server worden gerepliceerd naar de replica's die worden gelezen. U kunt alleen verbinding maken met een lees replica met behulp van de gebruikers accounts die beschikbaar zijn op de primaire server.

U kunt verbinding maken met de replica door de hostnaam en een geldig gebruikers account te gebruiken, net zoals bij een gewone Azure Database for PostgreSQL-server. Voor een server met de naam **mijn replica** met de gebruikers naam **myadmin**, kunt u verbinding maken met de replica met behulp van psql:

```
psql -h myreplica.postgres.database.azure.com -U myadmin@myreplica -d postgres
```

Voer bij de prompt het wacht woord voor het gebruikers account in.

## <a name="monitor-replication"></a>Replicatie controleren
Azure Database for PostgreSQL biedt twee metrische gegevens voor het controleren van replicatie. De twee meet waarden zijn de **maximale vertraging voor replica's** en **replica vertraging**. Zie voor meer informatie over het weer geven van deze metrische gegevens het gedeelte **een replica bewaken** in het [artikel Lees-en replica-instructies](howto-read-replicas-portal.md).

De **maximale vertraging** voor de metrische gegevens van replica's toont de vertraging in bytes tussen de primaire en de meest bewaarde replica. Deze metriek is alleen beschikbaar op de primaire server en is alleen beschikbaar als ten minste één van de Lees replica's is verbonden met de primaire.

De metriek van de **replica vertraging** toont de tijd sinds de laatste geplayte trans actie. Als er geen trans acties plaatsvinden op de primaire server, weerspiegelt de metriek deze tijds periode. Deze metriek is alleen beschikbaar voor replica servers. Replica vertraging wordt berekend op basis van de `pg_stat_wal_receiver` weer gave:

```SQL
EXTRACT (EPOCH FROM now() - pg_last_xact_replay_timestamp());
```

Stel een waarschuwing in om u te informeren wanneer de replica vertraging een waarde bereikt die niet geschikt is voor uw werk belasting. 

Voor meer inzicht moet u rechtstreeks een query uitvoeren op de primaire server om de replicatie vertraging in bytes op alle replica's te verkrijgen.

In PostgreSQL versie 10:

```SQL
select pg_wal_lsn_diff(pg_current_wal_lsn(), replay_lsn) 
AS total_log_delay_in_bytes from pg_stat_replication;
```

In PostgreSQL versie 9,6 en lager:

```SQL
select pg_xlog_location_diff(pg_current_xlog_location(), replay_location) 
AS total_log_delay_in_bytes from pg_stat_replication;
```

> [!NOTE]
> Als een primaire server of een lees bewerking van de replica opnieuw wordt gestart, wordt de tijd die nodig is om opnieuw op te starten en te worden weer gegeven in de metrische gegevens van de replica vertraging.

## <a name="stop-replication"></a>Replicatie stoppen
U kunt de replicatie tussen een primaire en een replica stoppen. De actie stoppen zorgt ervoor dat de replica opnieuw wordt opgestart en de replicatie-instellingen worden verwijderd. Nadat de replicatie tussen een primaire server en een lees replica is gestopt, wordt de replica een zelfstandige server. De gegevens op de zelfstandige server zijn de gegevens die beschikbaar zijn op de replica op het moment dat de opdracht stop-replicatie werd gestart. De zelfstandige server is niet actief op de primaire server.

> [!IMPORTANT]
> De zelfstandige server kan niet opnieuw in een replica worden gemaakt.
> Voordat u de replicatie op een lees replica stopt, moet u ervoor zorgen dat de replica over alle gegevens beschikt die u nodig hebt.

Wanneer u de replicatie stopt, verliest de replica alle koppelingen met de vorige primaire en andere replica's.

Meer informatie over het [stoppen van replicatie naar een replica](howto-read-replicas-portal.md).

## <a name="failover"></a>Failover
Er is geen automatische failover tussen de primaire servers en de replica server. 

Omdat de replicatie asynchroon is, is er sprake van een vertraging tussen de primaire en de replica. De hoeveelheid vertraging kan worden beïnvloed door een aantal factoren, zoals hoe zwaar de werk belasting die wordt uitgevoerd op de primaire server en de latentie tussen data centers. In de meeste gevallen variëren de replica vertraging tussen enkele seconden en een paar minuten. In gevallen waarin de primaire uitvoering zeer zware werk belastingen uitvoert en de replica niet snel genoeg kan opvangen, kan de vertraging hoger zijn. U kunt uw werkelijke replicatie vertraging bijhouden met behulp van de metrische *replica vertraging*, die beschikbaar is voor elke replica. Met deze metriek wordt de tijd weer gegeven sinds de laatste geplayte trans actie. U wordt aangeraden om te bepalen wat uw gemiddelde vertraging is door uw replica vertraging te bestuderen gedurende een bepaalde periode. U kunt een waarschuwing instellen voor replica vertraging, zodat u actie kunt ondernemen als deze buiten het verwachte bereik komt.

> [!Tip]
> Als u een failover naar de replica doorzoekt, geeft de vertraging op het moment dat u de replica loskoppelt, aan hoeveel gegevens er verloren zijn gegaan.

Zodra u hebt vastgesteld dat u een failover naar een replica wilt uitvoeren, 

1. Replicatie naar de replica stoppen<br/>
   Deze stap is nodig om de replica-server in staat te stellen schrijf bewerkingen te accepteren. Als onderdeel van dit proces wordt de replica server opnieuw opgestart en ontkoppeld van de primaire. Zodra u stopt met de replicatie, duurt het back-end doorgaans ongeveer twee minuten om te volt ooien. Zie de sectie [Replicatie stoppen](#stop-replication) in dit artikel voor meer informatie over de implicaties van deze actie.
    
2. Uw toepassing naar de (voormalige) replica laten wijzen<br/>
   Elke server heeft een unieke connection string. Werk uw toepassing bij zodat deze verwijst naar de (voormalige) replica in plaats van de primaire.
    
Zodra uw toepassing Lees-en schrijf bewerkingen heeft verwerkt, hebt u de failover voltooid. De uitval tijd van uw toepassings ervaring is afhankelijk van wanneer u een probleem detecteert en de stappen 1 en 2 hierboven uitvoert.

### <a name="disaster-recovery"></a>Herstel na noodgeval

Wanneer er sprake is van een belang rijke nood geval, zoals een zone-niveau of regionale storingen op beschikbaarheids gebied, kunt u herstel na nood gevallen uitvoeren door uw Lees replica te promo veren. U kunt vanuit de gebruikers interface-Portal naar de server voor het lezen van replica's navigeren. Klik vervolgens op het tabblad Replicatie en u kunt de replica stoppen om deze te promo veren tot een onafhankelijke server. U kunt ook de [Azure cli](/cli/azure/postgres/server/replica#az_postgres_server_replica_stop) gebruiken om de replica-server te stoppen en te promo veren.

## <a name="considerations"></a>Overwegingen

In deze sectie vindt u een overzicht van de overwegingen voor de functie replica lezen.

### <a name="prerequisites"></a>Vereisten
Het lezen van replica's en [logische decodering](concepts-logical.md) is beide afhankelijk van het post gres-logboek (write-Ahead) (Wal) voor informatie. Deze twee functies hebben verschillende niveaus van logboek registratie nodig van post gres. Voor logische decodering is een hoger niveau van logboek registratie vereist dan bij het lezen van replica's.

Als u het juiste niveau van logboek registratie wilt configureren, gebruikt u de Azure Replication support-para meter. Ondersteuning voor Azure-replicatie heeft drie instellings opties:

* **Uit** : Hiermee wordt de minste informatie in de wal geplaatst. Deze instelling is niet beschikbaar op de meeste Azure Database for PostgreSQL-servers.  
* **Replica** : uitgebreidere dan **uit**. Dit is het minimale registratie niveau dat nodig is voor het werken met [replica's](concepts-read-replicas.md) . Deze instelling is de standaard waarde op de meeste servers.
* **Logisch** -uitgebreidere dan de **replica**. Dit is het minimale registratie niveau voor het werken met logische code ring. Het lezen van replica's werkt ook bij deze instelling.

De server moet opnieuw worden opgestart na het wijzigen van deze para meter. Intern worden met deze para meter de post gres-para meters `wal_level` , `max_replication_slots` en ingesteld `max_wal_senders` .

### <a name="new-replicas"></a>Nieuwe replica's
Er wordt een lees replica gemaakt als een nieuwe Azure Database for PostgreSQL-server. Een bestaande server kan niet worden gemaakt in een replica. Het is niet mogelijk om een replica van een andere Lees replica te maken.

### <a name="replica-configuration"></a>Replica configuratie
Een replica wordt gemaakt met behulp van dezelfde reken-en opslag instellingen als de primaire. Nadat een replica is gemaakt, kunnen verschillende instellingen worden gewijzigd, waaronder opslag en back-up van de Bewaar periode.

Firewall regels, regels voor virtuele netwerken en parameter instellingen worden niet overgenomen van de primaire server naar de replica wanneer de replica wordt gemaakt of daarna.

### <a name="scaling"></a>Schalen
VCores schalen of tussen Algemeen en geoptimaliseerd voor geheugen:
* PostgreSQL vereist `max_connections` dat de instelling op een secundaire server [groter is dan of gelijk is aan de instelling op de primaire](https://www.postgresql.org/docs/current/hot-standby.html), anders kan het secundaire niet worden gestart.
* In Azure Database for PostgreSQL wordt het Maxi maal toegestane aantal verbindingen voor elke server vastgesteld op de reken-SKU sinds de verbindingen van het geheugen bezet zijn. U vindt meer informatie over de [toewijzing tussen max_connections en Compute-sku's](concepts-limits.md).
* **Omhoog schalen**: u kunt de berekening van een replica eerst opschalen en vervolgens omhoog schalen. Deze volg orde zorgt ervoor dat fouten de vereiste niet schenden `max_connections` .
* **Omlaag schalen**: schaal eerst de primaire Compute en schaal vervolgens omlaag in de replica. Als u de replica lager wilt schalen dan de primaire, treedt er een fout op omdat dit de vereiste schendt `max_connections` .

Opslag ruimte schalen:
* Voor alle replica's is opslag automatisch verg Roten ingeschakeld om replicatie problemen vanuit een opslag-volledige replica te voor komen. Deze instelling kan niet worden uitgeschakeld.
* U kunt de opslag ook hand matig schalen, op dezelfde manier als op een andere server


### <a name="basic-tier"></a>De servicelaag Basic
Basic-laag servers ondersteunen alleen replicatie met dezelfde regio.

### <a name="max_prepared_transactions"></a>max_prepared_transactions
[Postgresql vereist](https://www.postgresql.org/docs/current/runtime-config-resource.html#GUC-MAX-PREPARED-TRANSACTIONS) dat de waarde van de `max_prepared_transactions` para meter op de Lees replica groter dan of gelijk aan de primaire waarde is. anders wordt de replica niet gestart. Als u wilt wijzigen `max_prepared_transactions` op de primaire, wijzigt u deze eerst op de replica's.

### <a name="stopped-replicas"></a>Gestopte replica's
Als u de replicatie tussen een primaire server en een lees replica stopt, wordt de replica opnieuw gestart om de wijziging toe te passen. De gestopte replica wordt een zelfstandige server die zowel lees-als schrijf bewerkingen accepteert. De zelfstandige server kan niet opnieuw in een replica worden gemaakt.

### <a name="deleted-primary-and-standalone-servers"></a>De primaire en zelfstandige servers zijn verwijderd
Wanneer een primaire server wordt verwijderd, worden alle bijbehorende Lees replica's zelfstandige servers. De replica's worden opnieuw gestart om deze wijziging weer te geven.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [het maken en beheren van Lees replica's in de Azure Portal](howto-read-replicas-portal.md).
* Meer informatie over het [maken en beheren van Lees replica's in azure CLI en rest API](howto-read-replicas-cli.md).