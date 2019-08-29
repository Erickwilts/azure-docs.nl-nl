---
title: Community tools-klassieke resources verplaatsen naar Azure Resource Manager | Microsoft Docs
description: In dit artikel worden de hulpprogram ma's die door de community zijn verschaft, gecatalogiseerd voor het migreren van IaaS-resources van klassiek naar het implementatie model van Azure Resource Manager.
services: virtual-machines-windows
documentationcenter: ''
author: singhkays
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: 228b697b-3950-49f5-84bb-283bb56621b1
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 03/30/2017
ms.author: kasing
ms.openlocfilehash: 2b5d218812878db6fb110c043afffd2f9a2d344d
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2019
ms.locfileid: "70102663"
---
# <a name="community-tools-to-migrate-iaas-resources-from-classic-to-azure-resource-manager"></a>Community-programma’s voor de migratie van IaaS-resources van klassiek naar Azure Resource Manager
In dit artikel worden de hulpprogram ma's die door de community zijn verschaft, gecatalogiseerd om hulp te bieden bij de migratie van IaaS-resources van klassiek naar het Azure Resource Manager-implementatie model.

> [!NOTE]
> Deze hulpprogram ma's worden niet officieel ondersteund door Microsoft Ondersteuning. Daarom worden ze op GitHub geopend en zijn we blij met het accepteren van pull voor fixes of extra scenario's. Als u een probleem wilt melden, gebruikt u de functie GitHub issues.
> 
> Door te migreren met deze hulpprogram ma's wordt uitval tijd voor uw klassieke virtuele machine veroorzaakt. Als u op zoek bent naar platform ondersteunde migratie, gaat u naar 
> 
>   * [Door het platform ondersteunde migratie van IaaS-resources van klassiek naar Azure Resource Manager stack](migration-classic-resource-manager-overview.md)
>   * [Technisch diep gaande kennis van de migratie van klassiek naar Azure Resource Manager](migration-classic-resource-manager-deep-dive.md)
>   * [Migratie van IaaS-resources van klassiek naar Azure Resource Manager met behulp van Azure PowerShell](migration-classic-resource-manager-ps.md)
> 
> 

## <a name="asmmetadataparser"></a>AsmMetadataParser
Dit is een verzameling hulpprogram ma's die zijn gemaakt als onderdeel van ondernemings migraties van Azure service management tot Azure Resource Manager. Met dit hulp programma kunt u uw infra structuur repliceren naar een ander abonnement dat kan worden gebruikt voor het testen van de migratie en eventuele problemen verhelpen voordat u de migratie op uw productie abonnement uitvoert.

[Koppeling naar de documentatie van het hulp programma](https://github.com/Azure/classic-iaas-resourcemanager-migration/tree/master/AsmToArmMigrationApiToolset)

## <a name="migaz"></a>migAz
migAz is een extra optie voor het migreren van een volledige set klassieke IaaS-resources naar Azure Resource Manager IaaS-resources. De migratie kan plaatsvinden binnen hetzelfde abonnement of tussen verschillende abonnementen en abonnements typen (bijvoorbeeld: CSP-abonnementen).

[Koppeling naar de documentatie van het hulp programma](https://github.com/Azure/migAz)

## <a name="next-steps"></a>Volgende stappen

* [Overzicht van door het platform ondersteunde migratie van IaaS-resources van klassiek naar Azure Resource Manager](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Technische details over door platforms ondersteunde migratie van klassiek naar Azure Resource Manager](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Planning voor de migratie van IaaS-resources van het klassieke implementatiemodel naar Azure Resource Manager](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Power shell gebruiken voor het migreren van IaaS-resources van klassiek naar Azure Resource Manager](migration-classic-resource-manager-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [CLI gebruiken voor het migreren van IaaS-resources van klassiek naar Azure Resource Manager](../linux/migration-classic-resource-manager-cli.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Bekijk de meest voorkomende migratiefouten](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Bekijk de veelgestelde vragen over het migreren van IaaS-resources van klassiek naar Azure Resource Manager](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

