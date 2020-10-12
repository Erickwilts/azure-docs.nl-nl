---
title: 'Power shell-script: lijst met uitnodigingen voor Azure-gegevens delen die worden verzonden naar een consument | Microsoft Docs'
description: Meer informatie over hoe dit Power shell-script uitnodigingen voor een consument ontvangt en een voor beeld van het script dat u kunt gebruiken.
services: data-share
author: joannapea
ms.service: data-share
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 07/07/2019
ms.author: joanpo
ms.openlocfilehash: 3f0374ba5b98c145359d18143584e23fc543c552
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "90985616"
---
# <a name="use-powershell-to-get-a-data-share-invitation"></a>Power shell gebruiken voor het ophalen van een uitnodiging voor een gegevens share

Met dit Power shell-script worden uitnodigingen voor een consument verzonden.

## <a name="sample-script"></a>Voorbeeldscript
```powershell
# Set variables with your own values
$invitationId = "<invitationId>"
$location = "<location>"

#List invitations sent to a consumer
Get-AzDataShareInvitation

#Get a specific invitation sent to a consumer
Get-AzDataShareInvitation -location -invitationId 

```

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt: 

| Opdracht | Opmerkingen |
|---|---|
| [Get-AzDataShareInvitation](/powershell/module/az.datashare/get-azdatashareinvitation?view=azps-2.6.0) | Verzonden uitnodigingen voor gegevens delen ophalen en weer geven. |
|||

## <a name="next-steps"></a>Volgende stappen

Zie [Documentatie over Azure PowerShell](https://docs.microsoft.com/powershell/) voor meer informatie over Azure PowerShell.

Aanvullende voor beelden van Power shell-scripts voor Azure data share vindt u in de [Azure data share Power shell](../../samples-powershell.md)-voor beelden.
