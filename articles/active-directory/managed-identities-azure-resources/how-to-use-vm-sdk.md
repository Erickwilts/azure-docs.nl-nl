---
title: Over het gebruik van beheerde identiteiten voor Azure-resources op een Azure-VM met Azure-SDK 's
description: Codevoorbeelden voor het gebruik van Azure-SDK's met een Azure-VM die is identiteiten voor een Azure-resources worden beheerd.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/01/2017
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 00c86562e0fdb4e6d62d44088b7aba08e45e22a4
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60293231"
---
# <a name="how-to-use-managed-identities-for-azure-resources-on-an-azure-vm-with-azure-sdks"></a>Over het gebruik van beheerde identiteiten voor Azure-resources op een Azure-VM met Azure-SDK 's 

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]  
In dit artikel bevat een lijst van de SDK-voorbeelden die laten gebruik van hun respectieve Azure SDK ondersteuning voor beheerde identiteiten voor Azure-resources zien.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

> [!IMPORTANT]
> - Alle code/voorbeeldscript in dit artikel wordt ervan uitgegaan dat de client wordt uitgevoerd op een virtuele machine met beheerde identiteiten voor Azure-resources ingeschakeld. Gebruik de virtuele machine 'Connect'-functie in Azure portal, op afstand verbinding maken met uw virtuele machine. Zie voor meer informatie over het inschakelen van beheerde identiteiten voor Azure-resources op een virtuele machine [configureren beheerde identiteiten voor een Azure-resources op een virtuele machine met behulp van de Azure-portal](qs-configure-portal-windows-vm.md), of een van de variant (met behulp van PowerShell, CLI, een sjabloon of een Azure-artikelen SDK). 

## <a name="sdk-code-samples"></a>SDK-codevoorbeelden

| SDK             | Codevoorbeeld |
| --------------- | ----------- |
| .NET            | [Een Azure Resource Manager-sjabloon van een Windows-VM met behulp van beheerde identiteiten voor Azure-resources implementeren](https://github.com/Azure-Samples/windowsvm-msi-arm-dotnet) |
| .NET Core       | [Azure-services aanroepen vanuit een Linux-VM met behulp van beheerde identiteiten voor Azure-resources](https://github.com/Azure-Samples/linuxvm-msi-keyvault-arm-dotnet/) |
| Node.js         | [Resources met behulp van beheerde identiteiten voor Azure-resources beheren](https://azure.microsoft.com/resources/samples/resources-node-manage-resources-with-msi/) |
| Python          | [Beheerde identiteiten voor Azure-resources gebruiken om te verifiëren gewoon uit binnen een virtuele machine](https://azure.microsoft.com/resources/samples/resource-manager-python-manage-resources-with-msi/) |
| Ruby            | [Resources beheren vanuit een VM met beheerde identiteiten voor Azure-resources ingeschakeld](https://azure.microsoft.com/resources/samples/resources-ruby-manage-resources-with-msi/) |

## <a name="next-steps"></a>Volgende stappen

- Zie [Azure-SDK's](https://azure.microsoft.com/downloads/) voor de volledige lijst van de Azure SDK-bronnen, met inbegrip van bibliotheek downloads, documentatie en meer.
- Zie voor het inschakelen van beheerde identiteiten voor Azure-resources op een Azure VM [configureren beheerde identiteiten voor een Azure-resources op een virtuele machine met behulp van de Azure-portal](qs-configure-portal-windows-vm.md).








