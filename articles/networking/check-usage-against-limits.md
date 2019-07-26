---
title: Azure-resource gebruik controleren op limieten | Microsoft Docs
description: Meer informatie over het controleren van uw Azure-resource gebruik op Azure-abonnements limieten.
services: networking
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/05/2018
ms.author: jeconnoc
ms.openlocfilehash: b8bcbabe4b355e4e9cd617e9003902885b8edd88
ms.sourcegitcommit: de47a27defce58b10ef998e8991a2294175d2098
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/15/2019
ms.locfileid: "67872439"
---
# <a name="check-resource-usage-against-limits"></a>Gebruik van resources controleren op basis van limieten

In dit artikel leert u hoe u het nummer kunt zien van elk netwerk bron type dat u in uw abonnement hebt geïmplementeerd en wat uw [abonnements limieten](../azure-subscription-service-limits.md?toc=%2fazure%2fnetworking%2ftoc.json#networking-limits) zijn. De mogelijkheid om het gebruik van resources op basis van limieten te bekijken is handig voor het bijhouden van het huidige gebruik en het plannen van toekomstig gebruik. U kunt de [Azure-Portal](#azure-portal), [Power shell](#powershell)of de [Azure cli](#azure-cli) gebruiken om het gebruik bij te houden.

## <a name="azure-portal"></a>Azure-portal

1. Meld u aan bij Azure [Portal](https://portal.azure.com).
2. Selecteer boven aan de linkerbovenhoek van de Azure Portal **alle services**.
3. Voer *abonnementen* in het vak **filter** in. Wanneer **abonnementen** worden weer gegeven in de zoek resultaten, selecteert u deze.
4. Selecteer de naam van het abonnement waarvoor u gebruiks gegevens wilt bekijken.
5. Selecteer onder **instellingen**de optie **gebruik en quotum**.
6. U kunt de volgende opties selecteren:
   - **Resource typen**: U kunt alle resource typen selecteren of de specifieke typen resources selecteren die u wilt weer geven.
   - **Providers**: U kunt alle resource providers selecteren of COMPUTE , **netwerk**of **Storage**selecteren.
   - **Locaties**: U kunt alle Azure-locaties selecteren of specifieke locaties selecteren.
   - U kunt selecteren om alle resources weer te geven, of alleen de resources waarvoor ten minste één is geïmplementeerd.

     In het voor beeld in de volgende afbeelding ziet u alle netwerk resources met ten minste één resource die is geïmplementeerd in het VS-Oost:

       ![Gebruiks gegevens weer geven](./media/check-usage-against-limits/view-usage.png)

     U kunt de kolommen sorteren door de kolomkop te selecteren. De limieten die worden weer gegeven, zijn de limieten voor uw abonnement. Als u een standaard limiet wilt verhogen, selecteert u **aanvraag verg Roten**, voltooit u de ondersteunings aanvraag en verzendt u deze. Voor alle resources geldt een limiet van Maxi maal [Azure-](../azure-subscription-service-limits.md?toc=%2fazure%2fnetworking%2ftoc.json#networking-limits)limieten. Als uw huidige limiet al het maximum aantal is, kan de limiet niet worden verhoogd.

## <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

U kunt de opdrachten uitvoeren die volgen in de [Azure Cloud shell](https://shell.azure.com/powershell), of door Power shell uit te voeren vanaf uw computer. De Azure Cloud Shell is een gratis interactieve shell. In deze shell zijn algemene Azure-hulpprogramma's vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account. Als u Power shell vanaf uw computer uitvoert, hebt u de Azure PowerShell module versie 1.0.0 of hoger nodig. Voer `Get-Module -ListAvailable Az` uit op uw computer om de geïnstalleerde versie te vinden. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-az-ps). Als u Power shell lokaal uitvoert, moet u ook uitvoeren `Login-AzAccount` om u aan te melden bij Azure.

Bekijk uw gebruik op basis van limieten met [Get-AzNetworkUsage](https://docs.microsoft.com/powershell/module/az.network/get-aznetworkusage). In het volgende voor beeld wordt het gebruik opgehaald voor resources waarbij ten minste één resource wordt geïmplementeerd op de locatie VS-Oost:

```azurepowershell-interactive
Get-AzNetworkUsage `
  -Location eastus `
  | Where-Object {$_.CurrentValue -gt 0} `
  | Format-Table ResourceType, CurrentValue, Limit
```

U ontvangt uitvoer met dezelfde indeling als in de volgende voorbeeld uitvoer:

```powershell
ResourceType            CurrentValue Limit
------------            ------------ -----
Virtual Networks                   1    50
Network Security Groups            2   100
Public IP Addresses                1    60
Network Interfaces                 1 24000
Network Watchers                   1     1
```

## <a name="azure-cli"></a>Azure-CLI

Als u Azure-opdracht regel interface opdrachten gebruikt om taken in dit artikel te volt ooien, moet u de opdrachten uitvoeren in de [Azure Cloud shell](https://shell.azure.com/bash)of door de CLI vanaf uw computer uit te voeren. Voor dit artikel is de Azure CLI-versie 2.0.32 of hoger vereist. Voer `az --version` uit om te kijken welke versie is geïnstalleerd. Als u uw CLI wilt installeren of upgraden, raadpleegt u [De Azure CLI installeren](/cli/azure/install-azure-cli). Als u de Azure cli lokaal uitvoert, moet u ook uitvoeren `az login` om u aan te melden bij Azure.

Uw gebruik weer geven op basis van limieten met [AZ Network List-usages](/cli/azure/network?view=azure-cli-latest#az-network-list-usages). In het volgende voor beeld wordt het gebruik opgehaald voor resources op de locatie VS-Oost:

```azurecli-interactive
az network list-usages \
  --location eastus \
  --out table
```

U ontvangt uitvoer met dezelfde indeling als in de volgende voorbeeld uitvoer:

```azurecli
Name                    CurrentValue Limit
------------            ------------ -----
Virtual Networks                   1    50
Network Security Groups            2   100
Public IP Addresses                1    60
Network Interfaces                 1 24000
Network Watchers                   1     1
Load Balancers                     0   100
Application Gateways               0    50
```
