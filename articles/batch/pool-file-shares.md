---
title: Azure-bestands share voor Azure Batch Pools
description: Een Azure Files-share koppelen vanuit reken knooppunten in een Linux-of Windows-groep in Azure Batch.
ms.topic: how-to
ms.date: 05/24/2018
ms.openlocfilehash: 6bbaba20512a17de563e74ba095057c5c3f71f6b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "87986420"
---
# <a name="use-an-azure-file-share-with-a-batch-pool"></a>Een Azure-bestands share gebruiken met een batch-pool

[Azure files](../storage/files/storage-files-introduction.md) biedt volledig beheerde bestands shares in de cloud die toegankelijk zijn via het SMB-protocol (Server Message Block). Dit artikel bevat informatie en code voorbeelden voor het koppelen en gebruiken van een Azure-bestands share op groeps berekenings knooppunten.

## <a name="considerations-for-use-with-batch"></a>Overwegingen voor gebruik met batch

* Overweeg het gebruik van een Azure-bestands share wanneer u Pools hebt met een relatief laag aantal parallelle taken als u gebruikmaakt van niet-Premium Azure Files. Bekijk de [prestaties en schaal doelen](../storage/files/storage-files-scale-targets.md) om te bepalen of Azure files (die gebruikmaakt van een Azure Storage-account) moet worden gebruikt, op basis van de verwachte grootte van de groep en het aantal Asset-bestanden. 

* Azure-bestands shares zijn [rendabel](https://azure.microsoft.com/pricing/details/storage/files/) en kunnen worden geconfigureerd met gegevens replicatie naar een andere regio, waardoor het wereld wijd redundant is. 

* U kunt een Azure-bestands share gelijktijdig koppelen van een on-premises computer. Zorg er echter voor dat u de [gelijktijdigheids implicaties](../storage/common/storage-concurrency.md) begrijpt met name bij het gebruik van rest api's.

* Zie ook de algemene [overwegingen](../storage/files/storage-files-planning.md) bij de planning voor Azure-bestands shares.


## <a name="create-a-file-share"></a>Een bestandsshare maken

[Maak een bestands share](../storage/files/storage-how-to-create-file-share.md) in een opslag account dat is gekoppeld aan uw batch-account, of in een afzonderlijk opslag account.

## <a name="mount-an-azure-file-share-on-a-batch-pool"></a>Een Azure-bestands share koppelen aan een batch-pool

Raadpleeg de documentatie over het [koppelen van een virtueel bestands systeem aan een batch-pool](virtual-file-mount.md).

## <a name="next-steps"></a>Volgende stappen

* Zie voor andere opties voor het lezen en schrijven van gegevens in batch [persistente taak-en taak uitvoer](batch-task-output.md).
* Zie ook de [batch Shipyard](https://github.com/Azure/batch-shipyard) Toolkit, die [Shipyard recepten](https://github.com/Azure/batch-shipyard/tree/master/recipes) bevat voor het implementeren van bestands systemen voor werk belastingen voor batch-containers.
