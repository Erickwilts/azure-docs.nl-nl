---
title: Ondersteunde clusterversies in Azure Service Fabric
description: Meer informatie over clusterversies in Azure Service Fabric, inclusief een koppeling naar de nieuwste releases van het teamblog Service Fabric.
ms.topic: troubleshooting
ms.date: 03/02/2020
ms.openlocfilehash: 1158266857f0864ed2f442edb100032c70063719
ms.sourcegitcommit: e040ab443f10e975954d41def759b1e9d96cdade
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2020
ms.locfileid: "80385041"
---
# <a name="supported-service-fabric-versions"></a>Ondersteunde Service Fabric-versies

Zorg ervoor dat op uw cluster altijd een ondersteunde Azure Service Fabric-versie wordt uitgevoerd. Een minimum van 60 dagen nadat we de release van een nieuwe versie van Service Fabric hebben aangekondigd, eindigt de ondersteuning voor de vorige versie. U vindt aankondigingen van nieuwe releases op de [Service Fabric team blog](https://azure.microsoft.com/updates/?product=service-fabric).

Raadpleeg de volgende documenten voor meer informatie over hoe u uw cluster een ondersteunde Service Fabric-versie laten uitvoeren:

- [Een Azure Service Fabric-cluster upgraden](service-fabric-cluster-upgrade.md)
- [De Service Fabric-versie upgraden die wordt uitgevoerd op uw zelfstandige Windows Server-cluster](service-fabric-cluster-upgrade-windows-server.md)

## <a name="supported-versions"></a>Ondersteunde versies

In de volgende tabel worden de versies van Service Fabric en de einddatums van de ondersteuning weergegeven.

| Runtime van servicefabric in het cluster | Kan rechtstreeks upgraden vanuit de clusterversie |Compatibele SDK- of NuGet-pakketversie | Einde van de ondersteuning |
| --- | --- |--- | --- |
| Alle clusterversies vóór 5.3.121 | 5.1.158.* |Minder dan of gelijk aan versie 2.3 |20 januari 2017 |
| 5.3.* | 5.1.158.* |Minder dan of gelijk aan versie 2.3 |24 februari 2017 |
| 5.4.* | 5.1.158.* |Minder dan of gelijk aan versie 2.4 |10 mei 2017       |
| 5.5.* | 5.4.164.* |Minder dan of gelijk aan versie 2.5 |10 augustus 2017    |
| 5.6.* | 5.4.164.* |Minder dan of gelijk aan versie 2.6 |13 oktober 2017   |
| 5.7.* | 5.4.164.* |Minder dan of gelijk aan versie 2.7 |15 december 2017  |
| 6.0.* | 5.6.205.* |Minder dan of gelijk aan versie 2.8 |30 maart 2018     |
| 6.1.* | 5.7.221.* |Minder dan of gelijk aan versie 3.0 |15 juli 2018      |
| 6.2.* | 6.0.232.* |Minder dan of gelijk aan versie 3.1 |26 oktober 2018   |
| 6.3.* | 6.1.480.* |Minder dan of gelijk aan versie 3.2 |31 maart 2019  |
| 6.4.* | 6.2.301.* |Minder dan of gelijk aan versie 3.3 |15 september 2019 |
| 6.5.* | 6.4.617.* |Minder dan of gelijk aan versie 3.4 |1 augustus 2020 |
| 7.0.466.* | 6.4.664.* |Minder dan of gelijk aan versie 4.0|Huidige versie, dus geen einddatum |
| 7.0.466.* | 6.5.* |Minder dan of gelijk aan versie 4.0|Huidige versie, dus geen einddatum |
| 7.0.470.* | 7.0.466.* |Minder dan of gelijk aan versie 4.0 |Huidige versie, dus geen einddatum |

## <a name="supported-operating-systems"></a>Ondersteunde besturingssystemen

In de volgende tabel worden de ondersteunde besturingssystemen voor de ondersteunde Service Fabric-versies weergegeven.

| Besturingssysteem | Vroegst ondersteunde Service Fabric-versie |
| --- | --- |
| Windows Server 2012 R2 | Alle versies |
| Windows Server 2016 | Alle versies |
| Windows Server 1709 | 6.0 |
| Windows Server 1803 | 6.4 |
| Windows Server 1809 | 6.4.654.9590 |
| Windows Server 2019 | 6.4.654.9590 |
| Linux Ubuntu 16.04 | 6.0 |

## <a name="supported-version-names"></a>Ondersteunde versienamen

In de volgende tabel worden de versienamen van Service Fabric en de bijbehorende versienummers weergegeven.

| Versienaam | Windows-versienummer | Linux-versienummer |
| --- | --- | --- |
| 5.3 RTO | 5.3.121.9494 | N.v.t. |
| 5.3 CU1 | 5.3.204.9494 | N.v.t. |
| 5.3 CU2 | 5.3.301.9590 | N.v.t. |
| 5.3 CU3 | 5.3.311.9590 | N.v.t. |
| 5.4 CU2 | 5.4.164.9494 | N.v.t. |
| 5.5 CU1 | 5.5.216.0    | N.v.t. |
| 5.5 CU2 | 5.5.219.0    | N.v.t. |
| 5.5 CU3 | 5.5.227.0    | N.v.t. |
| 5.5 CU4 | 5.5.232.0    | N.v.t. |
| 5.6 RTO | 5.6.204.9494 | N.v.t. |
| 5.6 CU2 | 5.6.210.9494 | N.v.t. |
| 5.6 CU3 | 5.6.220.9494 | N.v.t. |
| 5.7 RTO | 5.7.198.9494 | N.v.t. |
| 5.7 CU4 | 5.7.221.9494 | N.v.t. |
| 6.0 RTO | 6.0.211.9494 | 6.0.120.1 |
| 6.0 CU1 | 6.0.219.9494 | 6.0.127.1 |
| 6.0 CU2 | 6.0.232.9494 | 6.0.133.1 |
| 6.1 CU1 | 6.1.456.9494 | 6.1.183.1 |
| 6.1 CU2 | 6.1.467.9494 | 6.1.185.1 |
| 6.1 CU3 | 6.1.472.9494 | N.v.t. |
| 6.1 CU4 | 6.1.480.9494 | 6.1.187.1 |
| 6.2 RTO | 6.2.269.9494 | 6.2.184.1 | 
| 6.2 CU1 | 6.2.274.9494 | 6.2.191.1 |
| 6.2 CU2 | 6.2.283.9494 | 6.2.194.1 |
| 6.2 CU3 | 6.2.301.9494 | 6.2.199.1 |
| 6.3 RTO | 6.3.162.9494 | 6.3.119.1 |
| 6.3 CU1 | 6.3.176.9494 | 6.3.124.1 |
| 6.3 CU1 | 6.3.187.9494 | 6.3.129.1 |
| 6.4 RTO | 6.4.617.9590 | 6.4.625.1 |
| 6.4 CU2 | 6.4.622.9590 | N.v.t. |
| 6.4 CU3 | 6.4.637.9590 | 6.4.634.1 |
| 6.4 CU4 | 6.4.644.9590 | 6.4.639.1 |
| 6.4 CU5 | 6.4.654.9590 | 6.4.649.1 |
| 6.4 CU6 | 6.4.658.9590 | N.v.t. |
| 6.4 CU7 | 6.4.664.9590 | 6.4.661.1 |
| 6.4 CU8 | 6.4.670.9590 | N.v.t. |
| 6.5 RTO | 6.5.639.9590 | 6.5.435.1 |
| 6.5 CU1 | 6.5.641.9590 | 6.5.454.1 |
| 6.5 CU2 | 6.5.658.9590 | 6.5.460.1 |
| 6.5 CU3 | 6.5.664.9590 | 6.5.466.1 |
| 6.5 CU5 | 6.5.676.9590 | 6.5.467.1 |
| 7.0 RTO | 7.0.457.9590 | 7.0.457.1 |
| 7.0 CU2 | 7.0.464.9590 | 7.0.464.1 |
| 7.0 CU3 | 7.0.466.9590 | 7.0.465.1 |
| 7.0 CU4 | 7.0.470.9590 | 7.0.469.1 |
