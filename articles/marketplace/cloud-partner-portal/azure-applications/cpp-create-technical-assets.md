---
title: Technische activa van Azure-toepassing maken | Azure Marketplace
description: De technische activa voor de aanbieding van een Azure-toepassing maken.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: article
ms.date: 12/13/2018
ms.author: pabutler
ms.openlocfilehash: cbe1b8c8f1159d90fbf97eeae272c1c50ec9b9bb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64942989"
---
# <a name="prepare-your-azure-application-technical-assets"></a>Voorbereiden van uw Azure-toepassing technische activa

Dit artikel beschrijft de resources voor het voorbereiden van de technische activa voor uw Azure-toepassing-aanbod.

## <a name="before-you-begin"></a>Voordat u begint

De volgende video bekijken [gebouw Oplossingssjablonen en beheerde toepassingen voor Azure Marketplace](https://channel9.msdn.com/Events/Build/2018/BRK3603), een overzicht over het ontwerpen van een Azure Resource Manager-sjabloon voor het definiëren van een Azure-toepassing-oplossing en klik vervolgens hoe u vervolgens de app-aanbieding publiceren op Azure Marketplace.

>[!VIDEO https://channel9.msdn.com/Events/Build/2018/BRK3603/player]


Raadpleeg de volgende Azure-toepassing-documentatie, waarmee u snelstartgidsen, zelfstudies en voorbeelden.

- [Informatie over Azure Resource Manager-sjablonen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates)
- Quickstarts:

  - [Azure Quickstart-sjablonen](https://azure.microsoft.com/documentation/templates/)
  - [Azure-snelstartsjablonen van GitHub](https://github.com/azure/azure-quickstart-templates)
  - [De toepassingsdefinitie van de publiceren](https://docs.microsoft.com/azure/managed-applications/publish-managed-app-definition-quickstart)
  - [Catalogus-app service implementeren](https://docs.microsoft.com/azure/managed-applications/deploy-service-catalog-quickstart)

  
- Zelfstudies:

  - [Definitiebestanden maken](https://docs.microsoft.com/azure/managed-applications/publish-service-catalog-app)
  - [Een Marketplace-toepassing publiceren](https://docs.microsoft.com/azure/managed-applications/publish-marketplace-app)

  - Voorbeelden:

    - [Azure-CLI](https://docs.microsoft.com/azure/managed-applications/cli-samples)
    - [Azure PowerShell](https://docs.microsoft.com/azure/managed-applications/powershell-samples)
    - [Beheerde toepassingsoplossingen](https://docs.microsoft.com/azure/managed-applications/sample-projects)

## <a name="fundamental-technical-knowledge"></a>Fundamentele technische kennis

Het ontwerpen, bouwen en testen van deze elementen tijd in beslag nemen en technische kennis van het Azure-platform en de technologieën die wordt gebruikt voor het bouwen van de aanbieding is vereist.

Uw IT-team moet beschikken over kennis op over de volgende Microsoft-technologieën:

- Basiskennis van [Azure Services](https://azure.microsoft.com/services/)
- Hoe u [ontwerpen en bouwen van Azure-toepassingen](https://azure.microsoft.com/solutions/architecture/)
- Praktische kennis van [Azure Virtual Machines](https://azure.microsoft.com/services/virtual-machines/), [Azure Storage](https://azure.microsoft.com/services/?filter=storage), en [Azure Networking](https://azure.microsoft.com/services/?filter=networking)
- Praktische kennis van [Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/)
- Praktische kennis van [JSON](https://www.json.org/)

## <a name="suggested-tools"></a>Voorgesteld hulpprogramma 's

Kies een of beide van de volgende scripts omgevingen voor het beheer van uw Azure-toepassing:

- [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)
- [Azure-CLI](https://docs.microsoft.com/cli/azure)

U wordt aangeraden de volgende hulpprogramma's toe te voegen aan uw ontwikkelomgeving:

- [Azure-opslagverkenner](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer)
- [Visual Studio Code](https://code.visualstudio.com/) met de volgende extensies:

  - Extensie: [Azure Resource Manager Tools](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)
  - Extensie: [Beautify](https://marketplace.visualstudio.com/items?itemName=HookyQR.beautify)
  - Extensie: [Prettify JSON](https://marketplace.visualstudio.com/items?itemName=mohsen1.prettify-json)

We raden ook controleren van de beschikbare hulpprogramma's in de [Azure-ontwikkelhulpprogramma's](https://azure.microsoft.com/tools/) pagina en, als u Visual Studio, de [Visual Studio Marketplace](https://marketplace.visualstudio.com/).

## <a name="next-steps"></a>Volgende stappen

[Aanbieding voor Azure-toepassing maken](./cpp-create-offer.md)

