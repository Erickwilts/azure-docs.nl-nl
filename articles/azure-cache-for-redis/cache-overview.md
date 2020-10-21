---
title: Wat is Azure Cache voor Redis?
description: Kom erachter hoe u met Azure Cache voor Redis cache-aside, cacheopslag van inhoud, gebruikerssessie opslaan in cache, wachtrij met taken en berichten, en gedistribueerde transacties kunt inschakelen.
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.topic: overview
ms.date: 05/12/2020
ms.openlocfilehash: 8ec2a302226e3dc44701209a8cbb47b7814a5a2c
ms.sourcegitcommit: fbb620e0c47f49a8cf0a568ba704edefd0e30f81
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "91874846"
---
# <a name="azure-cache-for-redis"></a>Azure Cache voor Redis
Azure Cache voor Redis biedt gegevensopslag in het geheugen op basis van de opensourcesoftware [Redis](https://redis.io/). Redis verbetert de prestaties en schaalbaarheid van toepassingen die gebruik maken van back-end-gegevensarchieven. Het kan grote hoeveelheden toepassingsaanvragen verwerken door vaak gebruikte gegevens in het servergeheugen te bewaren zodat deze snel kunnen worden geschreven en gelezen. Redis is een essentiële oplossing voor gegevensopslag met lage latentie en hoge doorvoer voor moderne toepassingen.

Azure Cache voor Redis biedt Redis aan als een beheerde service. Het biedt veilige en toegewezen Redis-serverinstanties en volledige Redis API-compatibiliteit. De service wordt beheerd door Microsoft, gehost op Azure en is toegankelijk voor alle toepassingen van Azure en daarbuiten.

Azure Cache voor Redis kan worden gebruikt als gedistribueerde gegevens- of inhoudscache, sessieopslag, berichtenbroker en meer. Het kan zelfstandig worden geïmplementeerd of naast andere databaseservice van Azure, zoals Azure SQL of Cosmos DB.

## <a name="key-scenarios"></a>Belangrijke scenario's
Azure Cache voor Redis verbetert de prestaties van toepassingen door veelgebruikte architectuurpatronen van toepassingen te ondersteunen. Sommige van de meest voorkomende zijn de volgende:

| Patroon      | Beschrijving                                        |
| ------------ | -------------------------------------------------- |
| [Gegevenscache](cache-web-app-cache-aside-leaderboard.md) | Databases zijn vaak te groot om rechtstreeks in een cache te laden. Het is gebruikelijk om het [cache-aside](https://docs.microsoft.com/azure/architecture/patterns/cache-aside)-patroon te gebruiken om alleen de benodigde gegevens in de cache te laden. Als het systeem wijzigingen aanbrengt in de gegevens, kan het ook de cache bijwerken, die vervolgens wordt gedistribueerd naar andere clients. Het systeem kan bovendien een vervaldatum instellen voor gegevens of een verwijderingsbeleid gebruiken om gegevensupdates in de cache te activeren.|
| [Inhoudscache](cache-aspnet-output-cache-provider.md) | Veel webpagina's worden gegenereerd op basis van sjablonen die gebruikmaken van statische inhoud, zoals kopteksten, voetteksten en banners. Deze statische items worden meestal niet vaak bijgewerkt. Cache in het geheugen biedt snelle toegang tot statische inhoud vergeleken met back-endgegevensarchieven. Dit patroon vermindert de verwerkingstijd en serverbelasting, waardoor webservers sneller kunnen reageren. Zo hebt u minder servers nodig om belasting te verwerken. Azure Cache voor Redis biedt de Redis Output Cache Provider om dit patroon te ondersteunen met ASP.NET.|
| [Sessieopslag](cache-aspnet-session-state-provider.md) | Dit patroon wordt vaak gebruikt met winkelwagens en andere gegevens van de gebruikersgeschiedenis die een webtoepassing mogelijk wil koppelen aan gebruikerscookies. Te veel informatie opslaan in een cookie kan negatieve gevolgen hebben voor de prestaties als de cookie groter wordt en bij elke aanvraag wordt doorgegeven en gevalideerd. Een gangbare oplossing is om de cookie als sleutel te gebruiken voor het opvragen van gegevens in een database. Het gebruik van een cache in het geheugen zoals Azure Cache voor Redis om gegevens te koppelen aan een gebruiker, is veel sneller dan interactie met een volledige relationele database. |
| Wachtrij met taken en berichten | Toepassingen voegen taken vaak toe aan een wachtrij als er tijd nodig is om de bewerkingen van een bepaalde aanvraag uit te voeren. Langdurige bewerkingen worden in de wachtrij gezet en op volgorde verwerkt, vaak door een andere server.  Deze methode van werk uitstellen heet taken in de wachtrij plaatsen. Azure Cache voor Redis biedt een gedistribueerde wachtrij om dit patroon in te schakelen in uw toepassing.|
| Gedistribueerde transacties | Toepassingen vereisen soms een reeks opdrachten voor een back-endgegevensopslag die moeten worden uitgevoerd als één atomische bewerking. Alle opdrachten moeten slagen of alle moet worden teruggezet naar de beginstatus. Azure Cache voor Redis ondersteunt het uitvoeren van een batch met opdrachten als één [transactie](https://redis.io/topics/transactions). |

## <a name="redis-versions"></a>Redis-versies

Azure Cache voor Redis ondersteunt Redis versie 4.x en, als preview, 6.0. We hebben besloten Redis 5.0 over te slaan om u te voorzien van de nieuwste versie. Voorheen werd in Azure Cache voor Redis slechts één Redis-versie bijhouden. Het biedt een nieuwere, grote release-upgrade en ten minste één oudere, stabiele versie die langer wordt ondersteund. U kunt [kiezen welke versie](cache-how-to-version.md) het beste werkt voor uw toepassing.

> [!NOTE]
> Redis 6.0 is momenteel beschikbaar als preview: [neem contact met ons op](mailto:azurecache@microsoft.com) als u geïnteresseerd bent. Deze preview wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.
>

## <a name="service-tiers"></a>Servicelagen
Azure Cache voor Redis is beschikbaar in de volgende lagen:

| Laag | Beschrijving |
|---|---|
| Basic | Een cache met één knooppunt. Deze laag ondersteunt meerdere geheugenformaten (250 MB - 53 GB) en is ideaal voor ontwikkel-/test- of niet-kritieke workloads. De Basic-laag heeft geen SLA (Service Level Agreement). |
| Standard | Een gerepliceerde cache in een configuratie met twee knooppunten (primair/replica) die door Azure wordt beheerd, met een [SLA](https://azure.microsoft.com/support/legal/sla/cache/v1_0/) met hoge beschikbaarheid. |
| Premium | De Premium-laag is geschikt voor zakelijk gebruik. Caches op de Premium-laag ondersteunen meer functies en hebben een hogere doorvoersnelheid met een lagere latentie. Caches op de Premium-laag worden geïmplementeerd op krachtigere hardware en bieden daarom betere prestaties in vergelijking met de Basic- of Standard-laag. Dit voordeel betekent dat de doorvoer voor een cache van dezelfde grootte sneller gaat bij Premium dan bij Standard. |

### <a name="feature-comparison"></a>Vergelijking van functies
[Prijzen van Azure Cache voor Redis](https://azure.microsoft.com/pricing/details/cache/) bevat een gedetailleerde vergelijking van elke laag. In de volgende tabel worden de functies beschreven die door elke laag worden ondersteund:

| Omschrijving | Premium | Standard | Basic |
| ------------------- | :-----: | :------: | :---: |
| [Service Level Agreement (SLA)](https://azure.microsoft.com/support/legal/sla/cache/v1_0/) |✔|✔|-|
| [Redis-gegevenspersistentie](cache-how-to-premium-persistence.md) |✔|-|-|
| [Redis-cluster](cache-how-to-premium-clustering.md) |✔|-|-|
| [Beveiliging via de firewallregels](cache-configure.md#firewall) |✔|✔|✔|
| Versleuteling tijdens overdracht |✔|✔|✔|
| [Verbeterde veiligheid en isolatie met VNet](cache-how-to-premium-vnet.md) |✔|-|-|
| [Import/export](cache-how-to-import-export-data.md) |✔|-|-|
| [Geplande updates](cache-administration.md#schedule-updates) |✔|✔|✔|
| [Geo-replicatie](cache-how-to-geo-replication.md) |✔|-|-|
| [Opnieuw opstarten](cache-administration.md#reboot) |✔|✔|✔|

### <a name="choosing-the-right-tier"></a>De juiste laag kiezen
Houd rekening met het volgende bij het kiezen van een laag voor Azure Cache voor Redis.

* **Geheugen**: De lagen Basic en Standard bieden 250 MB – 53 GB. De laag Premium biedt tot 1,2 TB (als een cluster) of 120 GB (niet-geclusterd). Zie [Prijzen van Azure Cache voor Redis](https://azure.microsoft.com/pricing/details/cache/) voor meer informatie.
* **Netwerkprestaties**: Als u een werkbelasting hebt waarvoor hoge doorvoer is vereist, biedt Premium meer bandbreedte dan Standard of Basic. Daarnaast hebben grotere caches in elke laag meer bandbreedte vanwege de onderliggende virtuele machine die als host fungeert voor de cache. Zie [Prestaties van Azure Cache voor Redis](cache-planning-faq.md#azure-cache-for-redis-performance) voor meer informatie.
* **Doorvoer**: De laag Premium biedt de maximaal beschikbare doorvoer. Als de cacheserver of client de bandbreedtelimiet bereikt, krijgt u mogelijk time-outs aan de clientzijde. Zie de volgende tabel voor meer informatie.
* **Hoge beschikbaarheid**: Azure Cache voor Redis biedt meerdere opties voor [hoge beschikbaarheid](cache-high-availability.md). Het garandeert dat een Standard/Premium-cache beschikbaar is volgens onze [SLA](https://azure.microsoft.com/support/legal/sla/cache/v1_0/). De SLA heeft alleen betrekking op connectiviteit met de cache-eindpunten. De SLA heeft geen betrekking op beveiliging tegen gegevensverlies. We raden u aan de Redis-functie voor gegevenspersistentie in de laag Premium te gebruiken om de tolerantie voor gegevensverlies te vergroten.
* **Redis-gegevenspersistentie**: Met Premium kunt u de cachegegevens in een Azure Storage-account bewaren. In een Basic- of Standard-cache worden gegevens alleen in het geheugen opgeslagen. Onderliggende problemen met de infrastructuur kunnen leiden tot mogelijk gegevensverlies. We raden u aan de Redis-functie voor gegevenspersistentie in de laag Premium te gebruiken om de tolerantie voor gegevensverlies te vergroten. Azure Cache voor Redis biedt RDB- en AOF-opties (preview-versie) voor Redis-persistentie. Zie [Persistentie configureren voor een Premium Azure Cache voor Redis](cache-how-to-premium-persistence.md) voor meer informatie.
* **Redis-cluster**: Als u caches wilt maken die groter zijn dan 120 GB of als u gegevens wilt verdelen over meerdere redis-knooppunten, kunt u de functie Redis clustering gebruiken, die beschikbaar is in Premium. Elk knooppunt bestaat uit een primair/replica cachepaar voor hoge beschikbaarheid. Zie [Clustering voor een Premium Azure Cache voor Redis configureren](cache-how-to-premium-clustering.md) voor meer informatie.
* **Verbeterde beveiliging en netwerkisolatie**: De implementatie van Azure Virtual Network verbetert de beveiliging en isolatie voor uw Azure Cache voor Redis, evenals subnetten, beleid voor toegangsbeheer en andere functies om de toegang verder te beperken. Zie [Virtual Network-ondersteuning voor een Premium Azure Cache voor Redis configureren](cache-how-to-premium-vnet.md) voor meer informatie.
* **Redis configureren**: In zowel de lagen Standard als Premium kunt u Redis configureren voor Keyspace-meldingen.
* **Maximumaantal clientverbindingen**: De laag Premium biedt het maximumaantal clients dat verbinding kan maken met Redis, met een groter aantal verbindingen voor caches met een grotere omvang. Bij clustering wordt het aantal beschikbare verbindingen voor een geclusterde cache niet verhoogd. Zie [Prijzen van Azure Cache voor Redis](https://azure.microsoft.com/pricing/details/cache/) voor meer informatie.
* **Toegewezen kerngeheugen voor Redis-server**: In de laag Premium hebben alle cachegrootten een toegewezen kerngeheugen voor Redis. In de lagen Basic en Standard hebben de C1-grootte en daarboven een toegewezen kerngeheugen voor de Redis-server.
* **Verwerking met één thread**: Redis maakt standaard gebruik van slechts één thread voor de verwerking van opdrachten. Azure Cache voor Redis gebruikt ook extra kerngeheugens voor I/O-verwerking. Het gebruik van verschillende kerngeheugens verbetert de doorvoerprestaties, ook al maakt het geen lineaire schaling mogelijk. Daarnaast worden grotere VM-grootten meestal met hogere bandbreedtelimieten geleverd dan kleinere. Zo kunt u oververzadiging van het netwerk voorkomen, waardoor er time-outs optreden in uw toepassing.
* **Prestatieverbeteringen**: Premium caches worden geïmplementeerd op hardware met snellere processors en bieden daarom betere prestaties in vergelijking met Basic of Standard. Premium caches hebben een hogere doorvoer en een lagere latentie. Zie [Prestaties van Azure Cache voor Redis](cache-planning-faq.md#azure-cache-for-redis-performance) voor meer informatie

U kunt de cache opschalen naar een hogere laag nadat u de cache hebt gemaakt. Omlaag schalen naar een lagere laag wordt niet ondersteund. Zie voor stapsgewijze instructies [De schaal aanpassen van Azure Cache voor Redis](cache-how-to-scale.md) en [Een schaalbewerking automatiseren](cache-how-to-scale.md#how-to-automate-a-scaling-operation).

## <a name="next-steps"></a>Volgende stappen
* [Snelstart ASP.NET-webapp](cache-web-app-howto.md) Maak een eenvoudige ASP.NET-webapp die gebruikmaakt van een Azure Cache voor Redis.
* [Snelstart .NET](cache-dotnet-how-to-use-azure-redis-cache.md) Maak een .NET-app die gebruikmaakt van een Azure Cache voor Redis.
* [Snelstart .NET Core](cache-dotnet-core-quickstart.md) Maak een .NET Core-app die gebruikmaakt van een Azure Cache voor Redis.
* [Snelstart Node.js](cache-nodejs-get-started.md) Maak een eenvoudige Node.js-app die gebruikmaakt van een Azure Cache voor Redis.
* [Snelstart Java](cache-java-get-started.md) Maak een eenvoudige Java-app die gebruikmaakt van een Azure Cache voor Redis.
* [Snelstart Python](cache-python-get-started.md) Maak een Python-app die gebruikmaakt van een Azure Cache voor Redis.
