---
title: Partitioneren en horizon taal schalen in Azure Cosmos DB
description: Meer informatie over de werking van partitionering in Azure Cosmos DB, het configureren van partitionering en partitie sleutels en het kiezen van de juiste partitie sleutel voor uw toepassing.
ms.author: rimman
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/01/2019
ms.openlocfilehash: 67ad37491f71ac82ff52331d19ea92a646c80a52
ms.sourcegitcommit: a52f17307cc36640426dac20b92136a163c799d0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68716985"
---
# <a name="partitioning-and-horizontal-scaling-in-azure-cosmos-db"></a>Partitioneren en horizon taal schalen in Azure Cosmos DB

In dit artikel worden fysieke en logische partities in Azure Cosmos DB uitgelegd. Ook worden aanbevolen procedures beschreven voor schalen en partitioneren. 

## <a name="logical-partitions"></a>Logische partities

Een logische partitie bestaat uit een set items die dezelfde partitie sleutel hebben. Zo kunt u in een container waarin alle items een `City` eigenschap bevatten, gebruiken `City` als de partitie sleutel voor de container. Groepen van items waarvoor specifieke waarden zijn opgegeven `City`voor, `London`zoals, `Paris`en `NYC`, vormen afzonderlijke logische partities. U hoeft zich geen zorgen te maken over het verwijderen van een partitie wanneer de onderliggende gegevens worden verwijderd.

In Azure Cosmos DB is een container de basis eenheid van schaal baarheid. Gegevens die worden toegevoegd aan de container en de door Voer die u in de container inricht, worden automatisch (horizon taal) gepartitioneerd over een set logische partities. Gegevens en door voer worden gepartitioneerd op basis van de partitie sleutel die u opgeeft voor de Azure Cosmos-container. Zie [een Azure Cosmos-container maken](how-to-create-container.md)voor meer informatie.

Een logische partitie definieert ook het bereik van database transacties. U kunt items binnen een logische partitie bijwerken met behulp van een [trans actie met snap shot-isolatie](database-transactions-optimistic-concurrency.md). Wanneer nieuwe items worden toegevoegd aan een container, worden nieuwe logische partities transparant gemaakt door het systeem.

## <a name="physical-partitions"></a>Fysieke partities

Een Azure Cosmos-container wordt geschaald door gegevens en door voer over een groot aantal logische partities te distribueren. Intern worden een of meer logische partities toegewezen aan een fysieke partitie die bestaat uit een set replica's, ook wel een [*replicaset*](global-dist-under-the-hood.md)genoemd. Elke replicaset host een exemplaar van de Azure Cosmos DB data base-engine. Een replicaset zorgt ervoor dat de gegevens die zijn opgeslagen in de fysieke partitie, duurzaam, Maxi maal beschikbaar en consistent zijn. Een fysieke partitie ondersteunt de maximale hoeveelheid opslag-en aanvraag eenheden (RUs). Elke replica die de fysieke partitie vormt, neemt de opslag limiet van de partitie over. Alle replica's van een fysieke partitie ondersteunen gezamenlijk de door Voer die is toegewezen aan de fysieke partitie. 

In de volgende afbeelding ziet u hoe logische partities worden toegewezen aan fysieke partities die globaal worden gedistribueerd:

![Een afbeelding die Azure Cosmos DB partitionering demonstreert](./media/partition-data/logical-partitions.png)

De door Voer die is ingericht voor een container, wordt gelijkmatig verdeeld over fysieke partities. Een partitie sleutel ontwerp dat de doorvoer aanvragen niet gelijkmatig distribueert, kan leiden tot het maken van ' hot ' partities. Dynamische partities kunnen leiden tot een frequentie beperking en een inefficiënt gebruik van de ingerichte door Voer en hogere kosten.

In tegens telling tot logische partities vormen fysieke partities een interne implementatie van het systeem. U kunt de grootte, plaatsing of het aantal fysieke partities niet bepalen en u kunt de toewijzing tussen logische partities en fysieke partities niet beheren. U kunt echter het aantal logische partities en de distributie van gegevens, werk belasting en door Voer bepalen door [de juiste logische partitie sleutel te kiezen](partitioning-overview.md#choose-partitionkey).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [het kiezen van een partitie sleutel](partitioning-overview.md#choose-partitionkey).
* Meer informatie over [ingerichte door Voer in azure Cosmos DB](request-units.md).
* Meer informatie over [globale distributie in azure Cosmos DB](distribute-data-globally.md).
* Meer informatie over het [inrichten van de door Voer voor een Azure Cosmos-container](how-to-provision-container-throughput.md).
* Meer informatie over het [inrichten van de door Voer voor een Azure Cosmos-data base](how-to-provision-database-throughput.md).
