---
title: HC-serie-Azure Virtual Machines
description: Specificaties voor Vm's van de HC-serie.
author: ju-shim
ms.service: virtual-machines
ms.subservice: sizes
ms.topic: conceptual
ms.date: 10/09/2020
ms.author: amverma
ms.reviewer: jushiman
ms.openlocfilehash: 254e8591892fe08e8f05c49ed47a4cb00da16e78
ms.sourcegitcommit: 50802bffd56155f3b01bfb4ed009b70045131750
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "91930183"
---
# <a name="hc-series"></a>HC-serie

Vm's uit de HC-serie zijn geoptimaliseerd voor toepassingen die worden aangedreven door een dichte berekening, zoals impliciete, geeindigd element analyse, moleculaire dynamiek en reken kundige schei kunde. HC Vm's feature 44 Intel Xeon Platinum 8168-processor kernen, 8 GB aan RAM per CPU-kern en geen hyperthreading. Het Intel Xeon Platinum-platform biedt ondersteuning voor het uitgebreide ecosysteem van software hulpprogramma's van Intel, zoals de Intel math-kernelmodus en geavanceerde vector verwerkings mogelijkheden zoals AVX-512.

Virtuele machines met de HC-serie 100 GB/sec Mellanox EDR InfiniBand. Deze Vm's zijn verbonden met een niet-blokkerende Fat-structuur voor geoptimaliseerde en consistente RDMA-prestaties. Deze Vm's ondersteunen adaptieve route ring en het dynamische Connected Trans Port (DCT, in aanvulling op Standard RC en UD Transports). Deze functies verbeteren de prestaties, schaal baarheid en consistentie van toepassingen, en het gebruik ervan wordt aanbevolen.

[ACU](acu.md): 297-315<br>
[Premium Storage](premium-storage-performance.md): ondersteund<br>
[Premium Storage caching](premium-storage-performance.md): ondersteund<br>
[Livemigratie](maintenance-and-updates.md): niet ondersteund<br>
[Updates](maintenance-and-updates.md)voor het behouden van geheugen: niet ondersteund<br>
[Ondersteuning](generation-2.md)voor het genereren van vm's: generatie 1 en 2<br>
<br>

| Grootte | vCPU | Processor | Geheugen (GiB) | Geheugen bandbreedte GB/s | Basis-CPU-frequentie (GHz) | Frequentie van alle kernen (GHz, piek) | Frequentie met één kern geheugen (GHz, piek) | RDMA-prestaties (GB/s) | MPI-ondersteuning | Tijdelijke opslag (GiB) | Max. aantal gegevensschijven | Maximum aantal Ethernet-Vnic's |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_HC44rs | 44 | Intel Xeon Platinum 8168 | 352 | 191 | 2.7 | 3.4 | 3.7 | 100 | Alles | 700 | 4 | 8 |

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="other-sizes"></a>Andere grootten

- [Algemeen doel](sizes-general.md)
- [Geoptimaliseerd geheugen](sizes-memory.md)
- [Geoptimaliseerde opslag](sizes-storage.md)
- [Geoptimaliseerde GPU](sizes-gpu.md)
- [Krachtig rekenvermogen](sizes-hpc.md)
- [Vorige generaties](sizes-previous-gen.md)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [het configureren van uw vm's, het](./workloads/hpc/configure.md) [inschakelen van Infiniband](./workloads/hpc/enable-infiniband.md), het [instellen van MPI](./workloads/hpc/setup-mpi.md)en het optimaliseren van HPC-toepassingen voor Azure bij [HPC-workloads](./workloads/hpc/overview.md).
- Lees over de laatste aankondigingen en enkele HPC-voorbeelden en -resultaten in de [Azure Compute Tech Community-blogs](https://techcommunity.microsoft.com/t5/azure-compute/bg-p/AzureCompute).
- Zie [High Performance Computing (HPC) in azure](/azure/architecture/topics/high-performance-computing/)voor een architectuur weergave op hoog niveau voor het uitvoeren van HPC-workloads.
- Meer informatie over hoe [Azure Compute units (ACU)](acu.md) u kan helpen bij het vergelijken van de reken prestaties in azure-sku's.
