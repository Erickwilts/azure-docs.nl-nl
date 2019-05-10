---
title: 'Voorbeeld van Azure CLI: beheerde schijven naar een opslagaccount kopiëren'
description: Azure CLI voorbeeld - uitvoer of een beheerde schijven kopiëren naar een opslagaccount.
services: virtual-machines-linux
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/09/2019
ms.author: ramankum
ms.custom: mvc,seodec18
ms.openlocfilehash: c5055860df007d0f009bd08c2b211ffeeaad711f
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/09/2019
ms.locfileid: "65510879"
---
# <a name="exportcopy-a-managed-disk-to-a-storage-account-using-the-azure-cli"></a>Een beheerde schijf naar een opslagaccount exporteren of kopiëren met behulp van Azure CLI

Met dit script wordt de onderliggende VHD van een beheerde schijf geëxporteerd naar een opslagaccount in dezelfde of een andere regio. Eerst wordt de SAS-URI van de beheerde schijf gegenereerd, waarna deze wordt gebruikt om de VHD naar een opslagaccount te kopiëren. Gebruik dit script om beheerde schijven naar een andere regio te kopiëren voor regionale uitbreiding. Als u publiceren van het VHD-bestand van een beheerde schijf in Azure Marketplace wilt, kunt u dit script naar het VHD-bestand kopiëren naar een opslagaccount en vervolgens een SAS-URI van de gekopieerde VHD publiceren in de Marketplace te genereren.   


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeldscript

[!code-azurecli[main](../../../cli_scripts/virtual-machine/copy-managed-disks-vhd-to-storage-account/copy-managed-disks-vhd-to-storage-account.sh "Copy the VHD of a managed disk")]


## <a name="script-explanation"></a>Uitleg van het script

Dit script gebruikt de volgende opdrachten voor het genereren van de SAS-URI voor een beheerde schijf en kopieert de onderliggende VHD naar een opslagaccount met behulp van de SAS-URI. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [az disk grant-access](https://docs.microsoft.com/cli/azure/disk?view=azure-cli-latest#az-disk-grant-access) | Hiermee genereert u een SAS met het kenmerk alleen-lezen die wordt gebruikt om het onderliggende VHD-bestand te kopiëren naar een opslagaccount of om het bestand on-premises te downloaden  |
| [az storage blob copy start](https://docs.microsoft.com/cli/azure/storage/blob/copy) | Hiermee kopieert u een blob asynchroon vanuit het ene opslagaccount naar het andere |

## <a name="next-steps"></a>Volgende stappen

[Een beheerde schijf maken op basis van een VHD](virtual-machines-linux-cli-sample-create-managed-disk-from-vhd.md?toc=%2fcli%2fmodule%2ftoc.json)

[Een virtuele machine maken op basis van een beheerde schijf](./virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json)

Raadpleeg de [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure) voor meer informatie over de Azure CLI.

Aanvullende CLI-scriptvoorbeelden voor virtuele machines en beheerde schijven vindt u in de [Azure-documentatie voor Linux-VM's](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
