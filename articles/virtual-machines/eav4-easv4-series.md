---
title: Eav4-serie en Easv4-serie
description: Specificaties voor de virtuele machines uit de Eav4-en Easv4-serie.
author: migerdes
ms.service: virtual-machines
ms.subservice: sizes
ms.topic: conceptual
ms.date: 02/03/2020
ms.author: jushiman
ms.openlocfilehash: 775b296185f9f0f0e967ada2688e24158f36360e
ms.sourcegitcommit: 271601d3eeeb9422e36353d32d57bd6e331f4d7b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/20/2020
ms.locfileid: "88648357"
---
# <a name="eav4-and-easv4-series"></a>Eav4- en Easv4-serie

De Eav4-serie en de Easv4-serie maken gebruik van AMD 2.35 GHz EPYC<sup>TM</sup> 7452-processor in een configuratie met meerdere threads met Maxi maal 256 MB L3-cache, waarmee u de meeste opties voor het uitvoeren van de meeste voor geheugen geoptimaliseerde werk belastingen kunt verhogen. De Eav4-serie en de Easv4-serie hebben dezelfde geheugen-en schijf configuraties als de Ev3 & Esv3-serie.

## <a name="eav4-series"></a>Eav4-serie

ACU: 230-260

Premium Storage: niet ondersteund

Premium Storage caching: niet ondersteund

Livemigratie: ondersteund

Updates voor geheugen behoud: ondersteund

De grootte van de Eav4-serie is gebaseerd op de 2.35 GHz AMD EPYC<sup>TM</sup> 7452-processor die een hogere maximum frequentie van 3.35 GHz kan bereiken en Premium SSD kan gebruiken. De grootten van de Eav4-serie zijn ideaal voor geheugenintensieve bedrijfs toepassingen. Gegevensschijfopslag wordt apart van virtuele machines in rekening gebracht. Als u Premium SSD wilt gebruiken, gebruikt u de grootte van de Easv4-serie. De prijs-en facturerings meters voor Easv4-grootten zijn gelijk aan die van de Eav3-serie.

| Grootte | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Max. aantal gegevensschijven | Maximale tijdelijke opslagdoorvoer: IOPS / MBps lezen / MBps schrijven | Max. aantal NIC's | Verwachte netwerk bandbreedte (Mbps) |
| -----|-----|-----|-----|-----|-----|-----|-----|
| Standard \_ E2a \_ v4|2|16|50|4|3000 / 46 / 23|2 | 1000 |
| Standard \_ E4a \_ v4|4|32|100|8|6000 / 93 / 46|2 | 2000 |
| Standard \_ E8a \_ v4|8|64|200|16|12.000 / 187 / 93|4 | 4000 |
| Standard \_ E16a \_ v4|16|128|400|32|24.000 / 375 / 187|8 | 8000 |
| Standard \_ E20a \_ v4|20|160|500|32|30000/468/234|8 | 10.000 |
| Standard \_ E32a \_ v4|32|256|800|32|48.000 / 750 / 375|8 | 16000 |
| Standard \_ E48a \_ v4|48|384|1200|32|96000/1000 (500)|8 | 24000 |
| Standard \_ E64a \_ v4|64|512|1600|32|96000/1000 (500)|8 | 30.000 |
| Standard \_ E96a \_ v4|96|672|2400|32|96000/1000 (500)|8 | 30.000 |

## <a name="easv4-series"></a>Easv4-serie

ACU: 230-260

Premium Storage: ondersteund

Premium Storage caching: ondersteund

Livemigratie: ondersteund

Updates voor geheugen behoud: ondersteund

De grootte van de Easv4-serie is gebaseerd op de 2.35 GHz AMD EPYC<sup>TM</sup> 7452-processor die een hogere maximum frequentie van 3.35 GHz kan bereiken en Premium SSD kan gebruiken. De grootten van de Easv4-serie zijn ideaal voor geheugenintensieve bedrijfs toepassingen.

| Grootte | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Max. aantal gegevensschijven | Maximale doorvoer voor schijven met caching en tijdelijke opslag: IOPS / MBps (cachegrootte in GiB) | Max. doorvoer voor schijf zonder caching: IOPS/MBps | Max. aantal NIC's | Verwachte netwerk bandbreedte (MBps) |
|-----|-----|-----|-----|-----|-----|-----|-----|-----|
| Standard_E2as_v4|2|16|32|4|4000/32 (50)|3200/48|2 | 1000 |
| Standard_E4as_v4|4|32|64|8|8000/64 (100)|6400/96|2 | 2000 |
| Standard_E8as_v4|8|64|128|16|16000/128 (200)|12800/192|4 | 4000 |
| Standard_E16as_v4|16|128|256|32|32000/255 (400)|25600/384|8 | 8000 |
| Standard_E20as_v4|20|160|320|32|40000/320 (500)|32000/480|8 | 10.000 |
| Standard_E32as_v4|32|256|512|32|64000/510 (800)|51200/768|8 | 16000 |
| Standard_E48as_v4|48|384|768|32|96000/1020 (1200)|76800/1148|8 | 24000 |
| Standard_E64as_v4|64|512|1024|32|128000/1020 (1600)|80000/1200|8 | 30.000 |
| Standard_E96as_v4|96|672|1344|32|192000/1020 (2400)|80000/1200|8 | 30.000 |

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="other-sizes-and-information"></a>Andere grootten en informatie

- [Algemeen doel](sizes-general.md)
- [Geoptimaliseerd geheugen](sizes-memory.md)
- [Geoptimaliseerde opslag](sizes-storage.md)
- [Geoptimaliseerde GPU](sizes-gpu.md)
- [Krachtig rekenvermogen](sizes-hpc.md)
- [Vorige generaties](sizes-previous-gen.md)

Prijs calculator: [prijs calculator](https://azure.microsoft.com/pricing/calculator/)

Meer informatie over schijven typen: [schijf typen](./disks-types.md#ultra-disk)

## <a name="next-steps"></a>Volgende stappen

Meer informatie over hoe [Azure Compute units (ACU)](acu.md) u kan helpen bij het vergelijken van de reken prestaties in azure-sku's.
