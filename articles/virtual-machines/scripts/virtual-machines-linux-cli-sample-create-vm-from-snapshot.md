---
title: Een VM maken op basis van een momentopname - CLI-voorbeeld
description: Voorbeeld van Azure CLI-script - Een virtuele machine maken van een momentopname
services: virtual-machines-linux
documentationcenter: virtual-machines
author: ramankumarlive
manager: kavithag
editor: ramankum
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/10/2017
ms.author: ramankum
ms.custom: mvc
ms.openlocfilehash: a6110ba2787cb99e20c099eb466e2dbd0c3df28e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "87085296"
---
# <a name="create-a-virtual-machine-from-a-snapshot-with-cli"></a>Een virtuele machine maken van een momentopname met CLI

Met dit script maakt u een virtuele machine van een momentopname van een besturingssysteemschijf.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeldscript

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-from-snapshot/create-vm-from-snapshot.sh "Create VM from snapshot")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Gebruik de volgende opdracht om de resourcegroep, VM, en alle gerelateerde resources te verwijderen.

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt voor het maken van een beheerde schijf, een virtuele machine en alle gerelateerde resources. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [az snapshot show](/cli/azure/snapshot) | Hiermee haalt u een momentopname op door de naam van de momentopname en de naam van de resourcegroep op te geven. De id van het geretourneerde object wordt gebruikt voor het maken van een beheerde schijf.  |
| [az disk create](/cli/azure/disk) | Hiermee maakt u beheerde schijven vanuit een momentopname met behulp van de id van een momentopname, de schijfnaam, het opslagtype en de grootte.  |
| [az vm create](/cli/azure/vm) | Hiermee maakt u een virtuele machine die gebruikmaakt van een beheerde besturingssysteemschijf. |

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [documentatie van Azure CLI](/cli/azure) voor meer informatie over de Azure CLI.

U kunt extra CLI-scriptvoorbeelden voor virtuele machines vinden in de [Azure-documentatie voor Linux-VM's](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
