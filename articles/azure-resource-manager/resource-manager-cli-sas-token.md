---
title: Azure-sjabloon met SAS-token en Azure CLI implementeren | Microsoft Docs
description: Azure Resource Manager en Azure CLI gebruiken om resources te implementeren naar Azure met een sjabloon die is beveiligd met SAS-token.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 05/31/2017
ms.author: tomfitz
ms.openlocfilehash: a71698831859e92300706ade8c2284cbfc7ee241
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67205413"
---
# <a name="deploy-private-resource-manager-template-with-sas-token-and-azure-cli"></a>Persoonlijke Resource Manager-sjablonen met SAS-token en Azure CLI implementeren

Wanneer de sjabloon bevindt zich in een storage-account, kunt u beperken van toegang aan de sjabloon en de token van een shared access signature (SAS) opgeven tijdens de implementatie. In dit onderwerp wordt uitgelegd hoe u Azure PowerShell gebruiken met Resource Manager-sjablonen voor het opgeven van een SAS-token tijdens de implementatie. 

## <a name="add-private-template-to-storage-account"></a>Persoonlijke sjabloon met storage-account toevoegen

U kunt uw sjablonen toevoegen aan een storage-account en een koppeling naar deze tijdens de implementatie met een SAS-token.

> [!IMPORTANT]
> De blob met de sjabloon is door de onderstaande stappen te volgen, toegankelijk voor de eigenaar van het account. De blob is echter toegankelijk voor iedereen met deze URI bij het maken van een SAS-token voor de blob. Als een andere gebruiker de URI onderschept, kan die gebruiker toegang tot de sjabloon. Met behulp van een SAS-token is een goede manier van het beperken van toegang tot uw sjablonen, maar u moet geen gevoelige gegevens zoals wachtwoorden rechtstreeks in de sjabloon.
> 
> 

Het volgende voorbeeld stelt u een persoonlijke account van de opslagcontainer en uploadt een sjabloon:
   
```azurecli
az group create --name "ManageGroup" --location "South Central US"
az storage account create \
    --resource-group ManageGroup \
    --location "South Central US" \
    --sku Standard_LRS \
    --kind Storage \
    --name {your-unique-name}
connection=$(az storage account show-connection-string \
    --resource-group ManageGroup \
    --name {your-unique-name} \
    --query connectionString)
az storage container create \
    --name templates \
    --public-access Off \
    --connection-string $connection
az storage blob upload \
    --container-name templates \
    --file vmlinux.json \
    --name vmlinux.json \
    --connection-string $connection
```

### <a name="provide-sas-token-during-deployment"></a>SAS-token opgeven tijdens de implementatie
Een SAS-token genereren voor het implementeren van een persoonlijke sjablonen in een opslagaccount, en deze opnemen in de URI voor de sjabloon. Stel het verlooptijdstip om toe te staan voldoende tijd voor het voltooien van de implementatie.
   
```azurecli
expiretime=$(date -u -d '30 minutes' +%Y-%m-%dT%H:%MZ)
connection=$(az storage account show-connection-string \
    --resource-group ManageGroup \
    --name {your-unique-name} \
    --query connectionString)
token=$(az storage blob generate-sas \
    --container-name templates \
    --name vmlinux.json \
    --expiry $expiretime \
    --permissions r \
    --output tsv \
    --connection-string $connection)
url=$(az storage blob url \
    --container-name templates \
    --name vmlinux.json \
    --output tsv \
    --connection-string $connection)
az group deployment create --resource-group ExampleGroup --template-uri $url?$token
```

Zie voor een voorbeeld van het gebruik van een SAS-token met gekoppelde sjablonen [gekoppelde sjablonen gebruiken met Azure Resource Manager](resource-group-linked-templates.md).

## <a name="next-steps"></a>Volgende stappen
* Zie voor een inleiding tot het implementeren van sjablonen, [resources implementeren met Resource Manager-sjablonen en Azure PowerShell](resource-group-template-deploy-cli.md).
* Zie voor een volledig voorbeeld van een script waarmee een sjabloon wordt geïmplementeerd, [sjabloonscript voor implementatie van Resource Manager-](resource-manager-samples-cli-deploy.md)
* Zie voor het definiëren van parameters in sjabloon, [-sjablonen maken](resource-group-authoring-templates.md#parameters).
