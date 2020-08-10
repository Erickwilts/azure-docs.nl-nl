---
title: 'Voorbeeld van Azure CLI-script: werken met sleutel-waardeparen in een App Configuration-archief'
titleSuffix: Azure App Configuration
description: Een Azure CLI-script gebruiken om sleutel-waardeparen te maken, weergeven, bijwerken en verwijderen uit App Configuration-archief
services: azure-app-configuration
author: lisaguthrie
ms.service: azure-app-configuration
ms.devlang: azurecli
ms.topic: sample
ms.date: 02/19/2020
ms.author: lcozzens
ms.custom: devx-track-azurecli
ms.openlocfilehash: e3407a1da469bf4f31e5d54d83ee837bddbafffc
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2020
ms.locfileid: "87494754"
---
# <a name="work-with-key-values-in-an-azure-app-configuration-store"></a>Werken met sleutelwaarden in een Azure-app-configuratiearchief

Dit voorbeeldscript laat zien hoe u het volgende kunt doen:
* Een nieuw sleutel-waardepaar maken
* Alle bestaande sleutel-waardeparen weergeven
* De waarde van een nieuwe sleutel bijwerken
* Het nieuwe sleutel-waardepaar verwijderen

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Voor dit artikel is Azure CLI versie 2.0 of hoger vereist. Voer `az --version` uit om de versie te bekijken. Als u uw CLI wilt installeren of upgraden, raadpleegt u [De Azure CLI installeren](/cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Voorbeeldscript

```azurecli-interactive
#!/bin/bash

appConfigName=myTestAppConfigStore
newKey="TestKey"
refKey="KeyVaultReferenceTestKey"
uri="[URL to value stored in Key Vault]"
uri2="[URL to another value stored in Key Vault]"

# Create a new key-value 
az appconfig kv set --name $appConfigName --key $newKey --value "Value 1"

# List current key-values
az appconfig kv list --name $appConfigName

# Update new key's value
az appconfig kv set --name $appConfigName --key $newKey --value "Value 2"

# List current key-values
az appconfig kv list --name $appConfigName

# Create a new key-value referencing a value stored in Azure Key Vault
az appconfig kv set --name $appConfigName --key $refKey --content-type "application/vnd.microsoft.appconfig.keyvaultref+json;charset=utf-8" --value "{\"uri\":\"$uri\"}"

# List current key-values
az appconfig kv list --name $appConfigName

# Update Key Vault reference
az appconfig kv set --name $appConfigName --key $refKey --value "{\"uri\":\"$uri2\"}"

# List current key-values
az appconfig kv list --name $appConfigName

# Delete new key
az appconfig kv delete  --name $appConfigName --key $newKey

# Delete Key Vault reference
az appconfig kv delete --name $appConfigName --key $refKey

# List current key-values
az appconfig kv list --name $appConfigName
```

[!INCLUDE [cli-script-cleanup](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Uitleg van het script

Deze tabel bevat de opdrachten die worden gebruikt in het voorbeeldscript. 

| Opdracht | Opmerkingen |
|---|---|
| [az appconfig kv set](/cli/azure/appconfig/kv#az-appconfig-kv-set) | Een sleutel-waardepaar maken of bijwerken. |
| [az appconfig kv list](/cli/azure/appconfig/kv#az-appconfig-kv-list) | De sleutel-waardeparen weergeven die zijn opgenomen in een App Configuration-archief. |
| [az appconfig kv delete](/cli/azure/appconfig/kv#az-appconfig-kv-delete) | Een sleutel-waardepaar verwijderen. |

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [documentatie van Azure CLI](/cli/azure) voor meer informatie over de Azure CLI.

Meer voorbeelden van CLI-scripts voor een App Configuration-archief vindt u in de [Azure App Configuration CLI-voorbeelden](../cli-samples.md).
