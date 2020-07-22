---
title: Voorbeeld van Azure CLI-script - Een virtuele Windows-machine versleutelen
description: Voorbeeld van Azure CLI-script - Een virtuele Windows-machine versleutelen
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 12/15/2017
ms.author: cynthn
ms.openlocfilehash: 8703e62aa7880fdddc347fa96edc213d9d54eab2
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2020
ms.locfileid: "86501140"
---
# <a name="encrypt-a-windows-virtual-machine-in-azure"></a>Virtuele Windows-machine versleutelen in Azure

Met dit script maakt u een beveiligde Azure-sleutelkluis, versleutelingssleutels, een service-principal voor Azure Active Directory en een virtuele Windows-machine (VM). De virtuele machine wordt vervolgens versleuteld met behulp van de versleutelingssleutel uit Key Vault en de referenties van de service-principal.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeldscript

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/encrypt-disks/encrypt_windows_vm.sh "Encrypt VM disks")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Gebruik de volgende opdracht om de resourcegroep, VM, en alle gerelateerde resources te verwijderen.

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt voor het maken van een resourcegroep, Azure-sleutelkluis, service-principal en alle gerelateerde resources. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [az group create](/cli/azure/group) | Hiermee maakt u een resourcegroep waarin alle resources worden opgeslagen. |
| [az keyvault create](/cli/azure/keyvault) | Hiermee maakt u een Azure-sleutelkluis voor het opslaan van beveiligde gegevens, zoals versleutelingssleutels. |
| [az keyvault key create](/cli/azure/keyvault/key) | Hiermee maakt u een versleutelingssleutel in Key Vault. |
| [az ad sp create-for-rbac](/cli/azure/ad/sp) | Hiermee maakt u een service-principal voor Azure Active Directory om de toegang tot versleutelingssleutels veilig te verifiëren en beheren. |
| [az keyvault set-policy](/cli/azure/keyvault) | Hiermee stelt u de machtigingen in voor de sleutelkluis om de service-principal toegang te bieden tot versleutelingssleutels. |
| [az vm create](/cli/azure/vm) | Hiermee maakt u de virtuele machine en verbindt u deze met de netwerkkaart, het virtuele netwerk, het subnet en de netwerkbeveiligingsgroep. Met deze opdracht geeft u ook de installatiekopie van de virtuele machine op die moet worden gebruikt, samen met beheerdersreferenties.  |
| [az vm encryption enable](/cli/azure/vm/encryption) | Hiermee schakelt u versleuteling in op een virtuele machine met behulp van de referenties voor de service-principal en de versleutelingssleutel. |
| [az vm encryption show](/cli/azure/vm/encryption) | Hiermee vraagt u de status op van het versleutelingsproces voor de virtuele machine. |
| [az group delete](/cli/azure/vm/extension) | Hiermee verwijdert u een resourcegroep met inbegrip van alle geneste resources. |

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [documentatie van Azure CLI](/cli/azure) voor meer informatie over de Azure CLI.

Aanvullende CLI-scriptvoorbeelden voor virtuele machines vindt u in de [Azure-documentatie voor Windows-VM's](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%windows%2ftoc.json).
