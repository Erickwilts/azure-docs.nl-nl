---
title: Rendering van Azure-referentiearchitecturen - Azure Batch
description: Architecturen voor het gebruik van Azure Batch en andere Azure-services uit te breiden van een on-premises render farm door te sturen naar de cloud
services: batch
ms.service: batch
author: davefellows
manager: jeconnoc
ms.author: lahugh
ms.date: 02/07/2019
ms.topic: conceptual
ms.custom: seodec18
ms.openlocfilehash: ae4680c948ce8e1efd32207dc37821d61182f2d8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60774160"
---
# <a name="reference-architectures-for-azure-rendering"></a>Referentiemateriaal voor architecturen voor Azure rendering

In dit artikel geeft een architectuur op hoog niveau diagrammen voor scenario's uit te breiden, of 'burst', een on-premises farm renderen naar Azure. De voorbeelden tonen verschillende opties voor Azure compute, netwerk- en -services.

## <a name="hybrid-with-nfs-or-cfs"></a>Hybride met NFS- of CFS

Het volgende diagram toont een hybride scenario met de volgende Azure-services:

* **COMPUTE** -Azure Batch-pool of virtuele-Machineschaalset opgehaald.

* **Netwerk** -On-premises: Azure ExpressRoute or VPN. Azure: Azure VNet.

* **Opslag** - invoer en uitvoer bestanden: NFS- of met behulp van Azure-VM's, gesynchroniseerd met on-premises opslag via Azure File Sync of RSync CFS. U kunt ook: Avere vFXT invoer of uitvoer van de bestanden van on-premises NAS-apparaten met NFS.

  ![Cloudbursting - hybride oplossing met NFS- of CFS](./media/batch-rendering-architectures/hybrid-nfs-cfs-avere.png)

## <a name="hybrid-with-blobfuse"></a>Hybride met Blobfuse

Het volgende diagram toont een hybride scenario met de volgende Azure-services:

* **COMPUTE** -Azure Batch-pool of virtuele-Machineschaalset opgehaald.

* **Netwerk** -On-premises: Azure ExpressRoute or VPN. Azure: Azure VNet.

* **Opslag** - invoer en uitvoer bestanden: BLOB-opslag gekoppeld voor de compute-resources via Azure Blobfuse.

  ![Cloudbursting - hybride oplossing met Blobfuse](./media/batch-rendering-architectures/hybrid-blob-fuse.png)

## <a name="hybrid-compute-and-storage"></a>Hybride berekeningen en opslag

Het volgende diagram ziet u een volledig verbonden hybride scenario voor berekeningen en opslag en bevat de volgende Azure-services:

* **COMPUTE** -Azure Batch-pool of virtuele-Machineschaalset opgehaald.

* **Netwerk** -On-premises: Azure ExpressRoute or VPN. Azure: Azure VNet.

* **Opslag** -Cross-premises: Avere vFXT. Optionele archiveren van on-premises naar Blob storage via Azure Data Box-bestanden, of on-premises Avere FXT voor NAS-versnelling.

  ![Cloudbursting - hybride berekeningen en opslag](./media/batch-rendering-architectures/hybrid-compute-storage-avere.png)


## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het gebruik van [renderen managers](batch-rendering-render-managers.md) met Azure Batch.

* Meer informatie over opties voor [Rendering in Azure](batch-rendering-service.md).