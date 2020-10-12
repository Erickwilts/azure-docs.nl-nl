---
title: Een Cloud service container maken met Power shell | Microsoft Docs
description: In dit artikel wordt uitgelegd hoe u een Cloud service container maakt met Power shell. De container fungeert als host voor web-en werk rollen.
services: cloud-services
documentationcenter: .net
author: cawaMS
ms.service: cloud-services
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: na
ms.date: 11/18/2016
ms.author: cawa
ms.openlocfilehash: d40a5b64cc8018f45bf08158ce808b2baae27962
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "87049090"
---
# <a name="use-an-azure-powershell-command-to-create-an-empty-cloud-service-container"></a>Een Azure PowerShell opdracht gebruiken om een lege Cloud service container te maken

In dit artikel wordt uitgelegd hoe u snel een Cloud Services container maakt met behulp van Azure PowerShell-cmdlets. Volg de onderstaande stappen:

1. Installeer de Microsoft Azure PowerShell cmdlet van de pagina [Azure PowerShell down loads](https://aka.ms/webpi-azps) .
2. Open de Power shell-opdracht prompt.
3. Gebruik de [add-AzureAccount](/powershell/module/servicemanagement/azure.service/add-azureaccount?view=azuresmps-4.0.0) om u aan te melden.

   > [!NOTE]
   > Zie [Azure PowerShell installeren en configureren](/powershell/azure/)voor meer instructies voor het installeren van de Azure PowerShell-cmdlet en het maken van een verbinding met uw Azure-abonnement.
   >
   >
4. Gebruik de cmdlet **New-Service** om een lege Azure Cloud service-container te maken.

   ```
   New-AzureService [-ServiceName] <String> [-AffinityGroup] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
   New-AzureService [-ServiceName] <String> [-Location] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
   ```

5. Volg dit voor beeld om de cmdlet aan te roepen:

   ```powershell
   New-AzureService -ServiceName "mytestcloudservice" -Location "Central US" -Label "mytestcloudservice"
   ```

Voer de volgende handelingen uit voor meer informatie over het maken van de Azure-Cloud service:

```powershell
Get-help New-AzureService
```

### <a name="next-steps"></a>Volgende stappen

* Raadpleeg de opdrachten [Get-service](/powershell/module/servicemanagement/azure.service/Get-AzureService?view=azuresmps-4.0.0), [Remove-service](/powershell/module/servicemanagement/azure.service/Remove-AzureService?view=azuresmps-4.0.0)en [set-service](/powershell/module/servicemanagement/azure.service/set-azureservice?view=azuresmps-4.0.0) om de Cloud service-implementatie te beheren. U kunt ook verwijzen naar [Cloud Services configureren](cloud-services-how-to-configure-portal.md) voor meer informatie.
* Als u uw Cloud service project wilt publiceren naar Azure, raadpleegt u het  **PublishCloudService.ps1** code voorbeeld van [gearchiveerde Cloud Services-opslag plaats](https://github.com/MicrosoftDocs/azure-cloud-services-files/tree/master/Scripts/cloud-services-continuous-delivery).