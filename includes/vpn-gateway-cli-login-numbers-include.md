---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 21dbec52dded5a195cc764741ab9e8d79737c549
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66121204"
---
1. Meld u aan bij uw Azure-abonnement met de opdracht [az login](/cli/azure/) en volg de instructies op het scherm. Zie [Aan de slag met Azure CLI](/cli/azure/get-started-with-azure-cli) voor meer informatie over aanmelden.

   ```azurecli
   az login
   ```
2. Als u meer dan één Azure-abonnement hebt, worden alle abonnementen voor het account weergegeven.

   ```azurecli
   az account list --all
   ```
3. Geef het abonnement op dat u wilt gebruiken.

   ```azurecli
   az account set --subscription <replace_with_your_subscription_id>
   ```
