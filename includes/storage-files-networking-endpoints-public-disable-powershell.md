---
title: bestand opnemen
description: bestand opnemen
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 6/2/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: a0a9bc29c3e20a025fb2c46a71c2f134c37bee04
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "84464991"
---
Met de volgende PowerShell-opdracht wordt al het verkeer geweigerd naar het openbare eindpunt van het opslagaccount. In deze opdracht is de parameter `-Bypass` ingesteld op `AzureServices`. Hierdoor kunnen vertrouwde Microsoft-services, zoals Azure File Sync, via het openbare eindpunt toegang krijgen tot het opslagaccount.

```PowerShell
# This assumes $storageAccount is still defined from the beginning of this of this guide.
$storageAccount | Update-AzStorageAccountNetworkRuleSet `
        -DefaultAction Deny `
        -Bypass AzureServices `
        -WarningAction SilentlyContinue `
        -ErrorAction Stop | `
    Out-Null
```