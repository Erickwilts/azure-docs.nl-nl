---
title: Bewaking en metrische gegevens-Azure Database for PostgreSQL-flexibele server
description: In dit artikel worden de functies bewaking en metrische gegevens van Azure Database for PostgreSQL-flexibele server beschreven.
author: lfittl-msft
ms.author: lufittl
ms.service: postgresql
ms.topic: conceptual
ms.date: 09/23/2020
ms.openlocfilehash: 1519e0b5cef6055cf8d8b0aded0d8ad323d548a2
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "91707843"
---
# <a name="monitor-metrics-on-azure-database-for-postgresql---flexible-server"></a>Metrische gegevens controleren op Azure Database for PostgreSQL-flexibele server

> [!IMPORTANT]
> Azure Database for PostgreSQL - Flexible Server is als preview-versie beschikbaar

Het bewaken van gegevens over uw servers helpt u bij het oplossen en optimaliseren van uw werk belasting. Azure Database for PostgreSQL biedt verschillende bewakings opties om inzicht te krijgen in het gedrag van uw server.

## <a name="metrics"></a>Metrische gegevens
Azure Database for PostgreSQL biedt diverse metrische gegevens die inzicht geven in het gedrag van de bronnen die de PostgreSQL-Server ondersteunen. Elke metriek wordt verzonden met een frequentie van één minuut en heeft een [geschiedenis van Maxi maal 93 dagen](../../azure-monitor/platform/data-platform-metrics.md#retention-of-metrics). U kunt waarschuwingen configureren voor de metrische gegevens. Andere opties zijn het instellen van geautomatiseerde acties, het uitvoeren van geavanceerde analyses en het archiveren van de geschiedenis. Zie het overzicht van Azure- [metrische](../../azure-monitor/platform/data-platform-metrics.md)gegevens voor meer informatie.

### <a name="list-of-metrics"></a>Lijst met metrische gegevens
De volgende metrische gegevens zijn beschikbaar voor PostgreSQL flexibele server:


|Gegevens|Weergave naam voor metrische gegevens|Eenheid|Beschrijving|
|---|---|---|---|
| active_connections | Actieve verbindingen | Aantal | Het aantal verbindingen met uw server. | 
| backup_storage_used | Gebruikte back-upopslag | Bytes | Hoeveelheid gebruikte back-upopslag. Deze waarde vertegenwoordigt de som van de opslag die wordt gebruikt door alle back-ups van de volledige data base, differentiële back-ups en logboek back-ups die worden bewaard op basis van de Bewaar periode voor back-ups die is ingesteld voor de server. De frequentie van de back-ups is service beheerd. Voor geo-redundante opslag is het gebruik van back-upopslag twee keer zo dat van de lokaal redundante opslag. |
| connections_failed | Mislukte verbindingen | Aantal | Mislukte verbindingen. |
| connections_succeeded | Geslaagde verbindingen | Aantal | Geslaagde verbindingen. |
| cpu_credits_consumed | Verbruikt CPU-tegoed | Aantal | Het aantal tegoeden dat door de flexibele server wordt gebruikt. Van toepassing op de Burstable-laag. |
| cpu_credits_remaining | Resterend CPU-tegoed | Aantal | Aantal beschik bare tegoeden voor burst. Van toepassing op de Burstable-laag. |
| cpu_percent | CPU-percentage | Percentage | Percentage CPU-gebruik. | 
| disk_queue_depth | Schijf wachtrij diepte | Aantal | Aantal openstaande I/O-bewerkingen op de gegevens schijf. |
| IOPS | IOPS | Aantal | Aantal I/O-bewerkingen op de schijf per seconde. |
| maximum_used_transactionIDs | Maximum aantal gebruikte trans actie-Id's | Aantal | De maximale trans actie-ID die wordt gebruikt. |
| memory_percent | Geheugen percentage | Percentage | Het percentage geheugen dat in gebruik is. |
| network_bytes_egress | Netwerk uit | Bytes | Hoeveelheid uitgaand netwerk verkeer. |
| network_bytes_ingress | Netwerk in | Bytes | Hoeveelheid binnenkomend netwerk verkeer. |
| read_iops | IOPS lezen | Aantal | Aantal I/O-lees bewerkingen per seconde van gegevens schijf. |
| read_throughput | Lees doorvoer | Bytes | Bytes gelezen per seconde van de schijf. |
| storage_free | Vrije opslag ruimte | Bytes | De hoeveelheid beschik bare opslag ruimte. |
| storage_percent | Opslag percentage | Percentage | Percentage gebruikte opslag ruimte. De opslag die door de service wordt gebruikt, kan de database bestanden, transactie logboeken en de server logboeken bevatten.|
| storage_used | Gebruikte opslag | Bytes | Percentage gebruikte opslag ruimte. De opslag die door de service wordt gebruikt, kan de database bestanden, transactie logboeken en de server logboeken bevatten. |
| txlogs_storage_used | Gebruikte transactie logboek opslag | Bytes | De hoeveelheid opslag ruimte die wordt gebruikt door de transactie Logboeken. | 
| write_throughput | Schrijf doorvoer | Bytes | Geschreven bytes per seconde naar schijf. |
| write_iops | IOPS schrijven | Aantal | Aantal I/O-schrijf bewerkingen per seconde van de gegevens schijf. |

## <a name="server-logs"></a>Serverlogboeken
Met Azure Database for PostgreSQL kunt u standaard logboeken van post gres configureren en gebruiken. Voor meer informatie over Logboeken gaat u naar het [document logboek registratie concepten](concepts-logging.md).
