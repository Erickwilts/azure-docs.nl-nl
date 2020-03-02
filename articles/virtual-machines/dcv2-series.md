---
title: DC-serie-Azure Virtual Machines
description: Specificaties voor de Vm's van de DC-serie.
services: virtual-machines
author: susaxen
ms.service: virtual-machines
ms.topic: article
ms.date: 02/20/2020
ms.author: lahugh
ms.openlocfilehash: fd8f01f0fc7180d271404ffee4496ff9cbac7222
ms.sourcegitcommit: 1fa2bf6d3d91d9eaff4d083015e2175984c686da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/01/2020
ms.locfileid: "78205922"
---
# <a name="preview-dcv2-series"></a>Voor beeld: DCv2-serie


De DCv2-serie kan u helpen de vertrouwelijkheid en integriteit van uw gegevens en code te beschermen tijdens de verwerking in de open bare Cloud. Deze machines worden ondersteund door de nieuwste generatie Intel XEON E-2288G-processor met SGX-technologie. Met de Intel Turbo Boost-technologie kunnen deze machines tot wel 5.0 GHz gaan. Met DCv2-serie-instanties kunnen klanten veilige, op enclave gebaseerde toepassingen bouwen om hun code en gegevens te beschermen terwijl ze worden gebruikt.

Voor beelden van use cases zijn onder meer het vertrouwelijk delen van gegevens, het opsporen van fraude, het witwassen van anti geld, het block Chain, het gebruik van analyse, de intelligentie en de vertrouwelijke machine learning.

Premium Storage: ondersteund *

Premium Storage caching: ondersteund *

Livemigratie: niet ondersteund

Updates voor het behouden van geheugen: niet ondersteund

\* Met uitzonde ring van Standard_DC8_v2



| Grootte             | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Max. aantal gegevensschijven | Maximale doorvoer voor schijven met caching en tijdelijke opslag: IOPS / MBps (cachegrootte in GiB) | Max. doorvoer voor schijf zonder caching: IOPS/MBps | Maximum aantal Nic's/verwachte netwerk bandbreedte (MBps) |
|------------------|------|-------------|------------------------|----------------|-------------------------------------------------------------------------|-------------------------------------------|----------------------------------------------|
| Standard_DC1s_v2 | 1    | 4           | 50                     | 1              | 2000/16 (21)                                                            | 1600/24                                   | 2                                            |
| Standard_DC2s_v2 | 2    | 8           | 100                    | 2              | 4000/32 (43)                                                            | 3200/48                                   | 2                                            |
| Standard_DC4s_v2 | 4    | 16          | 200                    | 4              | 8000/64 (86)                                                            | 6400/96                                   | 2                                            |
| Standard_DC8_v2  | 8   | 32          | 400                    | 8              | 16000/128 (172)                                                         | 12800/192                                 | 2                                            |

- Vm's uit de DCv2-serie zijn [twee vm's van de tweede generatie](./linux/generation-2.md#creating-a-generation-2-vm) en ondersteunen alleen `Gen2` installatie kopieën.
- Momenteel alleen beschikbaar in UK-zuid.
- Vorige generatie van vertrouwelijk Compute-Vm's: [gelijkstroom reeks](sizes-previous-gen.md)
- DCv2 Vm's maken met behulp van Azure Portal [virtuele machine maken-Portal](./linux/quick-create-portal.md)



## <a name="other-sizes"></a>Andere grootten

- [Algemeen doel](sizes-general.md)
- [Geoptimaliseerd geheugen](sizes-memory.md)
- [Geoptimaliseerde opslag](sizes-storage.md)
- [Geoptimaliseerde GPU](sizes-gpu.md)
- [Krachtig rekenvermogen](sizes-hpc.md)
- [Vorige generaties](sizes-previous-gen.md)

## <a name="next-steps"></a>Volgende stappen

Meer informatie over hoe [Azure Compute units (ACU)](acu.md) u kan helpen bij het vergelijken van de reken prestaties in azure-sku's.
