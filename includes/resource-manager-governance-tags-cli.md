---
title: Include-bestand
description: Include-bestand
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/20/2018
ms.author: tomfitz
ms.custom: include file, devx-track-azurecli
ms.openlocfilehash: 53d4aa70b55577cdf2f6a1b898b496eb368157f5
ms.sourcegitcommit: 8c7f47cc301ca07e7901d95b5fb81f08e6577550
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/27/2020
ms.locfileid: "92755392"
---
Gebruik de opdracht [az group update](/cli/azure/group) om twee tags toe te voegen aan een resourcegroep:

```azurecli-interactive
az group update -n myResourceGroup --set tags.Environment=Test tags.Dept=IT
```

Stel dat u een derde tag wilt toevoegen. Voer de opdracht opnieuw uit met de nieuwe tag. Deze wordt toegevoegd aan de bestaande tags.

```azurecli-interactive
az group update -n myResourceGroup --set tags.Project=Documentation
```

Resources nemen geen tags over van de resourcegroep. Op dit moment heeft uw resourcegroep drie tags, maar hebben de resources geen tags. Gebruik het volgende script als u alle tags van een resourcegroep op de bijbehorende resources wilt toepassen en daarbij ook de bestaande tags voor de resources wilt behouden:

```azurecli-interactive
# Get the tags for the resource group
jsontag=$(az group show -n myResourceGroup --query tags)

# Reformat from JSON to space-delimited and equals sign
t=$(echo $jsontag | tr -d '"{},' | sed 's/: /=/g')

# Get the resource IDs for all resources in the resource group
r=$(az resource list -g myResourceGroup --query [].id --output tsv)

# Loop through each resource ID
for resid in $r
do
  # Get the tags for this resource
  jsonrtag=$(az resource show --id $resid --query tags)
  
  # Reformat from JSON to space-delimited and equals sign
  rt=$(echo $jsonrtag | tr -d '"{},' | sed 's/: /=/g')
  
  # Reapply the updated tags to this resource
  az resource tag --tags $t$rt --id $resid
done
```

U kunt ook tags van de resourcegroep toepassen op de resources zonder de bestaande tags te houden:

```azurecli-interactive
# Get the tags for the resource group
jsontag=$(az group show -n myResourceGroup --query tags)

# Reformat from JSON to space-delimited and equals sign
t=$(echo $jsontag | tr -d '"{},' | sed 's/: /=/g')

# Get the resource IDs for all resources in the resource group
r=$(az resource list -g myResourceGroup --query [].id --output tsv)

# Loop through each resource ID
for resid in $r
do
  # Apply tags from resource group to this resource
  az resource tag --tags $t --id $resid
done
```

Gebruik een JSON-tekenreeks om meerdere waarden met elkaar te combineren in een enkele tag.

```azurecli-interactive
az group update -n myResourceGroup --set tags.CostCenter='{"Dept":"IT","Environment":"Test"}'
```

Als u alle tags van een resourcegroep wilt verwijderen, gebruikt u:

```azurecli-interactive
az group update -n myResourceGroup --remove tags
```
