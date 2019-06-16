---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines-windows, virtual-machines-linux
author: cynthn
ms.service: multiple
ms.topic: include
ms.date: 06/11/2019
ms.author: cynthn;azcspmt;jonbeck
ms.custom: include file
ms.openlocfilehash: e8e11ffd4260c2956c6bb4740973eb77abfdc7b9
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67055215"
---
GPU-geoptimaliseerde VM-grootten zijn gespecialiseerde virtuele machines die beschikbaar zijn met één of meerdere NVIDIA GPU's. Deze grootten zijn ontworpen voor intensieve compute- en grafisch intensieve visualisatie werkbelastingen. In dit artikel bevat informatie over het aantal en type van GPU's, vcpu's, gegevensschijven en NIC's. De doorvoer en netwerkbandbreedte Storage zijn ook opgenomen voor elke grootte in deze groepering.

* **NC, de NCv2, NCv3** grootten zijn geoptimaliseerd voor rekenintensieve en netwerkintensieve toepassingen en algoritmen. Enkele voorbeelden zijn CUDA - en opencl-toepassingen en simulaties, AI en Deep Learning. De NCv3-serie is gericht op high-performance computing-workloads met NVIDIA Tesla V100 GPU. De NC-serie maakt gebruik van de Intel Xeon E5-2690 v3 2,60 GHz v3 (Haswell)-processor, en de NCv2-serie en de NCv3-serie VM's gebruikmaken van de Intel Xeon E5-2690 v4 (Broadwell)-processor.

* **ND- en NDv2** de ND-serie is gericht op scenario's voor training en Deductie voor deep learning. Maakt gebruik van NVIDIA Tesla P40 GPU- en de Intel Xeon E5-2690 v4 (Broadwell)-processor. De NDv2-serie maakt gebruik van de Intel Xeon Platinum 8168 (Skylake)-processor.

* **NV en NVv3** grootten zijn geoptimaliseerd en ontworpen voor externe visualisatie, streaming, games, codering, en VDI-scenario's met behulp van frameworks als OpenGL en DirectX.  Deze VM's worden ondersteund door NVIDIA Tesla M60 GPU.

## <a name="nc-series"></a>NC-serie

Premium-opslag:  Niet ondersteund

Premium Storage opslaan in cache:  Niet ondersteund

NC-serie VM's worden aangestuurd door de [NVIDIA Tesla R80](https://www.nvidia.com/content/dam/en-zz/Solutions/Data-Center/tesla-product-literature/Tesla-K80-BoardSpec-07317-001-v05.pdf) kaart en de Intel Xeon E5-2690 v3 (Haswell)-processor. Gebruikers kunnen Verwerk gegevens sneller door gebruik te maken van CUDA voor energie-exploratietoepassingen, vastlopen simulaties, ray getraceerde rendering, deep learning en meer. De NC24r-configuratie biedt een lage latentie en hoge doorvoer network interface die is geoptimaliseerd voor nauw gekoppelde werkbelastingen voor parallelle berekeningen.

| Grootte | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | GPU | GPU-geheugen: GiB | Max. aantal gegevensschijven | Max. aantal NIC's |
| --- | --- | --- | --- | --- | --- | --- | ---- |
| Standard_NC6 |6 |56 | 340 | 1 | 12 | 24 | 1 |
| Standard_NC12 |12 |112 | 680 | 2 | 24 | 48 | 2 |
| Standard_NC24 |24 |224 | 1440 | 4 | 48 | 64 | 4 |
| Standard_NC24r* |24 |224 | 1440 | 4 | 48 | 64 | 4 |

1 GPU = halve K80-kaart.

*RDMA-compatibel

## <a name="ncv2-series"></a>NCv2-serie

Premium-opslag:  Ondersteund

Premium Storage opslaan in cache:  Ondersteund

NCv2-serie VM's worden aangestuurd door [NVIDIA Tesla P100](https://www.nvidia.com/en-us/data-center/tesla-p100/) GPU's. Deze GPU's bieden meer dan 2 x de verwerkingsprestaties van de NC-serie. Klanten kunnen profiteren van deze bijgewerkte GPU's traditionele HPC-workloads, zoals de modellering van reservoirmodellering, DNA sequentiëren, eiwitanalyse, Monte Carlo-simulaties en anderen. Naast de GPU's, ook de NCv2-serie VM's worden aangestuurd door de Intel Xeon E5-2690 v4 (Broadwell)-processors.

De NC24rs versie 2-configuratie biedt een lage latentie en hoge doorvoer network interface die is geoptimaliseerd voor nauw gekoppelde werkbelastingen voor parallelle berekeningen.

> [!IMPORTANT]
> Voor deze serie grootte van is de vCPU (kern) quotum in uw abonnement in eerste instantie ingesteld op 0 in elke regio. [Een vCPU-quotum verhoging](../articles/azure-supportability/resource-manager-core-quotas-request.md) voor deze serie in een [beschikbare regio](https://azure.microsoft.com/regions/services/).
>

| Grootte | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | GPU | GPU-geheugen: GiB | Max. aantal gegevensschijven | Maximale doorvoer voor schijf zonder caching: IOPS / MBps | Max. aantal NIC's |
| --- | --- | --- | --- | --- | --- | ---  | ---| --- |
| Standard_NC6s_v2 | 6 |112 | 736 | 1 | 16 | 12 | 20000/ 200 | 4 |
| Standard_NC12s_v2 | 12 |224 | 1474 | 2 | 32 | 24 | 40000 / 400 | 8 |
| Standard_NC24s_v2 | 24 |448 | 2948 | 4 | 64 | 32 | 80000 / 800 | 8 |
| Standard_NC24rs_v2* | 24 |448 | 2948 | 4 | 64 | 32 | 80000 / 800 | 8 |

1 GPU = één P100-kaart.

*RDMA-compatibel

## <a name="ncv3-series"></a>NCv3-serie

Premium-opslag:  Ondersteund

Premium Storage opslaan in cache:  Ondersteund

NCv3-serie VM's worden aangestuurd door [NVIDIA Tesla V100](https://www.nvidia.com/en-us/data-center/tesla-v100/) GPU's. Deze GPU's leveren tot 1,5 keer de verwerkingsprestaties van de NCv2-serie. Klanten kunnen profiteren van deze bijgewerkte GPU's traditionele HPC-workloads, zoals de modellering van reservoirmodellering, DNA sequentiëren, eiwitanalyse, Monte Carlo-simulaties en anderen. De NC24rs v3-configuratie biedt een lage latentie en hoge doorvoer network interface die is geoptimaliseerd voor nauw gekoppelde werkbelastingen voor parallelle berekeningen. Naast de GPU's, ook de NCv3-serie VM's worden aangestuurd door de Intel Xeon E5-2690 v4 (Broadwell)-processors.

> [!IMPORTANT]
> Voor deze serie grootte van is de vCPU (kern) quotum in uw abonnement in eerste instantie ingesteld op 0 in elke regio. [Een vCPU-quotum verhoging](../articles/azure-supportability/resource-manager-core-quotas-request.md) voor deze serie in een [beschikbare regio](https://azure.microsoft.com/regions/services/).
>

| Grootte | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | GPU | GPU-geheugen: GiB | Max. aantal gegevensschijven | Maximale doorvoer voor schijf zonder caching: IOPS / MBps | Max. aantal NIC's |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_NC6s_v3 | 6 |112 | 736 | 1 | 16 | 12 | 20000 / 200 | 4 |
| Standard_NC12s_v3 | 12 |224 | 1474 | 2 | 32 | 24 | 40000 / 400 | 8 |
| Standard_NC24s_v3 | 24 |448 | 2948 | 4 | 64 | 32 | 80000 / 800 | 8 | 
| Standard_NC24rs_v3* |24 |448 | 2948 | 4 | 64 | 32 | 80000 / 800 | 8 |

1 GPU = één V100-kaart.

*RDMA-compatibel

## <a name="ndv2-series-preview"></a>NDv2-serie (Preview)

Premium-opslag:  Ondersteund

Premium Storage opslaan in cache:  Ondersteund

Infiniband: Niet ondersteund

NDv2-serie virtuele machine is een nieuwe toevoeging aan de GPU die is ontworpen voor de behoeften van de HPC-, AI- en machine learning-werkbelastingen. Het wordt aangestuurd door 8 NVLINK van NVIDIA Tesla V100 GPU's en 40 Intel Xeon Platinum 8168 (Skylake)-kernen en 672 GiB van het systeemgeheugen met elkaar verbonden. NDv2-exemplaren bieden uitstekende FP32- en FP64-prestaties voor HPC- en AI-workloads met behulp van Cuda, TensorFlow, Pytorch, Caffe en andere frameworks.

[Meld u aan en krijg toegang tot deze machines tijdens de preview](https://aka.ms/ndv2signup).
<br>

| Grootte | vCPU | GPU | Geheugen | NIC's (max.) | Met maximaal Schijfgrootte | Met maximaal Gegevensschijven | Maximale doorvoer voor schijf zonder caching: IOPS / MBps | Maximale netwerkbandbreedte | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_ND40s_v2 | 40 | 8 V100 (NVLink) | 672 GiB | 8 | Tijdelijke 1344 / 2948XIO | 32 | 80000 / 800 | 24000 Mbps |

## <a name="nd-series"></a>ND-serie

Premium-opslag:  Ondersteund

Premium Storage opslaan in cache:  Ondersteund

De ND-serie virtuele machines zijn een nieuwe toevoeging aan de GPU-serie die is ontworpen voor AI- en Deep Learning werkbelastingen. Ze bieden uitstekende prestaties voor training en Deductie. ND-exemplaren worden aangestuurd door [NVIDIA Tesla P40](http://images.nvidia.com/content/pdf/tesla/184427-Tesla-P40-Datasheet-NV-Final-Letter-Web.pdf) GPU's en Intel Xeon E5-2690 v4 (Broadwell)-processors. Deze instanties bieden uitstekende prestaties voor enkele precisie zwevende drijvende-kommaberekeningen, voor AI-workloads met Microsoft Cognitive Toolkit, TensorFlow, Caffe en andere frameworks. De ND-serie biedt daarnaast een veel groter GPU-geheugen (24 GB), waardoor het mogelijk is om veel grotere modellen met een neuraal netwerk in te zetten. Net als de NC-serie, de ND-serie biedt een configuratie met een secundair netwerk met lage latentie en hoge doorvoersnelheid via RDMA en InfiniBand-connectiviteit, zodat u grootschalige trainingstaken waarvoor veel GPU's kunt uitvoeren.

> [!IMPORTANT]
> Voor deze serie grootte van is de vCPU (kern) quotum per regio in uw abonnement in eerste instantie ingesteld op 0. [Een vCPU-quotum verhoging](../articles/azure-supportability/resource-manager-core-quotas-request.md) voor deze serie in een [beschikbare regio](https://azure.microsoft.com/regions/services/).
>

| Grootte | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | GPU | GPU-geheugen: GiB | Max. aantal gegevensschijven | Maximale doorvoer voor schijf zonder caching: IOPS / MBps | Max. aantal NIC's |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_ND6s | 6 |112 | 736 | 1 | 24 | 12 | 20000 / 200 | 4 |
| Standard_ND12s | 12 |224 | 1474 | 2 | 48 | 24 | 40000 / 400 | 8 | 
| Standard_ND24s | 24 |448 | 2948 | 4 | 96 | 32 | 80000 / 800 | 8 |
| Standard_ND24rs* | 24 |448 | 2948 | 4 | 96 | 32 | 80000 / 800 | 8 |

1 GPU = één P40-kaart.

*RDMA-compatibel

## <a name="nv-series"></a>NV-serie

Premium-opslag:  Niet ondersteund

Premium Storage opslaan in cache:  Niet ondersteund

De NV-serie virtuele machines worden aangestuurd door [NVIDIA Tesla M60](http://images.nvidia.com/content/tesla/pdf/188417-Tesla-M60-DS-A4-fnl-Web.pdf) GPU's en NVIDIA GRID technologie voor desktop versneld en virtuele bureaubladen waarop klanten hun gegevens of simulatie kunnen visualiseren zijn. Gebruikers kunnen hun grafisch intensieve werkstromen op de NV-exemplaren grafische mogelijkheden en daarnaast enkelvoudige precisieworkloads, zoals codering en rendering uitvoeren visualiseren. NV-serie VM's worden ook aangestuurd door de Intel Xeon E5-2690 v3 (Haswell)-processors.

Elke GPU in NV-exemplaren wordt geleverd met een licentie RASTER. Deze licentie geeft u de flexibiliteit om te gebruiken een NV-exemplaar als een virtuele werkstation voor één gebruiker of 25 gelijktijdige gebruikers verbinding kunnen maken met de virtuele machine voor een virtuele toepassing-scenario.

| Grootte | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | GPU | GPU-geheugen: GiB | Max. aantal gegevensschijven | Max. aantal NIC's | Virtuele werkstations | Virtuele toepassingen |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_NV6 |6 |56 |340 | 1 | 8 | 24 | 1 | 1 | 25 |
| Standard_NV12 |12 |112 |680 | 2 | 16 | 48 | 2 | 2 | 50 |
| Standard_NV24 |24 |224 |1440 | 4 | 32 | 64 | 4 | 4 | 100 |

1 GPU = halve M60-kaart.

## <a name="nvv3-series-preview-sup1sup"></a>NVv3-serie (Preview) <sup>1</sup>

Premium-opslag:  Ondersteund

Premium Storage opslaan in cache:  Ondersteund

Virtuele machines uit de NVv3-serie worden aangedreven door [NVIDIA Tesla M60](http://images.nvidia.com/content/tesla/pdf/188417-Tesla-M60-DS-A4-fnl-Web.pdf) GPU's en NVIDIA GRID technologie van Intel E5-2690 v4 (Broadwell)-processors. Deze virtuele machines zijn bedoeld voor GPU grafische toepassingen versnelde en virtuele bureaubladen waar klanten willen hun gegevens visualiseren, resultaten wilt weergeven, werken met CAD- of render en stream-inhoud te simuleren. Daarnaast kunnen deze virtuele machines enkelvoudige, nauwkeurige workloads uitvoeren zoals encoding en renderen. NVv3 virtuele machines ondersteunt Premiumopslag en worden geleverd met twee keer het systeemgeheugen (RAM) in vergelijking met diens voorganger NV-serie.  

Elke GPU in NVv3 exemplaren wordt geleverd met een licentie RASTER. Deze licentie geeft u de flexibiliteit om te gebruiken een NV-exemplaar als een virtuele werkstation voor één gebruiker of 25 gelijktijdige gebruikers verbinding kunnen maken met de virtuele machine voor een virtuele toepassing-scenario.

| Grootte | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | GPU | GPU-geheugen: GiB | Max. aantal gegevensschijven | Maximale doorvoer voor schijf zonder caching: IOPS / MBps | Max. aantal NIC's | Virtuele werkstations | Virtuele toepassingen | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_NV12s_v3 |12 |112 |320 | 1 | 8 | 12 | 20000 / 200 | 4 | 1 | 25 |
| Standard_NV24s_v3 |24 |224 |640 | 2 | 16 | 24 | 40000 / 400 | 8 | 2 | 50 |
| Standard_NV48s_v3 |48 |448 |1280 | 4 | 32 | 32 | 80000 / 800 | 8 | 4 | 100 |

1 GPU = halve M60-kaart.

<sup>1</sup> NVv3-serie VM's zijn uitgerust met Intel Hyper-Threading-technologie
