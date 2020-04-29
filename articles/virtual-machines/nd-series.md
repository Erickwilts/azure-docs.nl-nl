---
title: ND-serie-Azure Virtual Machines
description: Specificaties voor de Vm's uit de ND-serie.
services: virtual-machines
author: vikancha
ms.service: virtual-machines
ms.topic: article
ms.date: 02/03/2020
ms.author: lahugh
ms.openlocfilehash: 163dc0d6ecd4f91273a82b04ce186f7a8df868af
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "79465053"
---
# <a name="nd-series"></a>ND-serie

De virtuele machines uit de ND-serie vormen een nieuwe toevoeging aan de GPU-familie die is ontworpen voor AI en diep gaande werk belastingen. Ze bieden uitstekende prestaties voor training en demijnen. ND-instanties worden aangedreven door [Nvidia Tesla P40](https://images.nvidia.com/content/pdf/tesla/184427-Tesla-P40-Datasheet-NV-Final-Letter-Web.pdf) -Gpu's en Intel Xeon E5-2690 v4-Cpu's (Broadwell). Deze instanties bieden uitstekende prestaties voor drijvende-komma bewerkingen met één precisie, voor AI-workloads die gebruikmaken van Microsoft Cognitive Toolkit, tensor flow, Caffe en andere frameworks. De ND-serie biedt ook een veel grotere GPU-geheugen grootte (24 GB), waardoor u veel grotere Neural net-modellen kunt aanpassen. Net als de NC-serie biedt de ND-serie een configuratie met een secundaire netwerk met lage latentie en een hoge door Voer via RDMA en InfiniBand-connectiviteit, zodat u grootschalige trainings taken kunt uitvoeren die veel Gpu's bespannen.

Premium Storage: ondersteund

Premium Storage caching: ondersteund

Livemigratie: niet ondersteund

Updates voor het behouden van geheugen: niet ondersteund

> [!IMPORTANT]
> Voor deze VM-serie wordt het vCPU (kern quotum) per regio in uw abonnement in eerste instantie ingesteld op 0. [Vraag een toename van een vCPU-quotum](../azure-supportability/resource-manager-core-quotas-request.md) aan voor deze reeks in een [beschik bare regio](https://azure.microsoft.com/regions/services/).
>
| Grootte | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | GPU | GPU-geheugen: GiB | Max. aantal gegevensschijven | Maxi maal aantal niet-opgeslagen schijf doorvoer: IOPS/MBps | Max. aantal NIC's |
|---|---|---|---|---|---|---|---|---|
| Standard_ND6s    | 6  | 112 | 736  | 1 | 24 | 12 | 20000/200 | 4 |
| Standard_ND12s   | 12 | 224 | 1474 | 2 | 48 | 24 | 40000/400 | 8 |
| Standard_ND24s   | 24 | 448 | 2948 | 4 | 96 | 32 | 80000/800 | 8 |
| Standard_ND24rs * | 24 | 448 | 2948 | 4 | 96 | 32 | 80000/800 | 8 |

1 GPU = One P40-kaart.

*RDMA-compatibel

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="supported-operating-systems-and-drivers"></a>Ondersteunde besturingssystemen en stuurprogramma’s

Om te profiteren van de GPU-mogelijkheden van Vm's uit de Azure N-serie, moeten de NVIDIA GPU-Stuur Programma's zijn geïnstalleerd.

Met de [uitbrei ding NVIDIA GPU-stuur programma](./extensions/hpccompute-gpu-windows.md) worden de juiste NVIDIA-CUDA of raster Stuur Programma's geïnstalleerd op een virtuele machine uit de N-serie. De uitbrei ding installeren of beheren met de Azure Portal of hulpprogram ma's, zoals Azure PowerShell of Azure Resource Manager sjablonen. Zie de [documentatie over NVIDIA GPU-Stuur Programma's](./extensions/hpccompute-gpu-windows.md) voor ondersteunde besturings systemen en implementaties tappen. Zie [extensies en functies van virtuele Azure-machines](./extensions/overview.md)voor algemene informatie over VM-extensies.

Als u ervoor kiest om de NVIDIA GPU-Stuur Programma's hand matig te installeren, raadpleegt u [het stuur programma voor](./windows/n-series-driver-setup.md) de installatie van de Windows-of [n-Series GPU-](./linux/n-series-driver-setup.md) stuur programma voor Linux voor ondersteunde besturings systemen, stuur Programma's, installatie en verificaties tappen.

## <a name="other-sizes"></a>Andere grootten

- [Algemeen doel](sizes-general.md)
- [Geoptimaliseerd geheugen](sizes-memory.md)
- [Geoptimaliseerde opslag](sizes-storage.md)
- [Geoptimaliseerde GPU](sizes-gpu.md)
- [Krachtig rekenvermogen](sizes-hpc.md)
- [Vorige generaties](sizes-previous-gen.md)

## <a name="next-steps"></a>Volgende stappen

Meer informatie over hoe [Azure Compute units (ACU)](acu.md) u kan helpen bij het vergelijken van de reken prestaties in azure-sku's.
