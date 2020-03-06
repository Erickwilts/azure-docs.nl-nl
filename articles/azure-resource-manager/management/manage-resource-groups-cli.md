---
title: Resource groepen beheren-Azure CLI
description: Gebruik Azure CLI voor het beheren van uw resource groepen via Azure Resource Manager. Laat zien hoe u resource groepen kunt maken, weer geven en verwijderen.
author: mumian
ms.topic: conceptual
ms.date: 02/11/2019
ms.author: jgao
ms.openlocfilehash: 7face572f545153ea92efbdb345bbaabda5dd126
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2020
ms.locfileid: "78384973"
---
# <a name="manage-azure-resource-manager-resource-groups-by-using-azure-cli"></a>Azure Resource Manager-resource groepen beheren met behulp van Azure CLI

Meer informatie over het gebruik van Azure CLI met [Azure Resource Manager](overview.md) voor het beheren van uw Azure-resource groepen. Zie [Azure-resources beheren met Azure cli](manage-resources-cli.md)voor meer informatie over het beheren van Azure-resources.

Andere artikelen over het beheren van resource groepen:

- [Azure-resource groepen beheren met behulp van de Azure Portal](manage-resources-portal.md)
- [Azure-resource groepen beheren met behulp van Azure PowerShell](manage-resources-powershell.md)

## <a name="what-is-a-resource-group"></a>Wat is een resource groep?

Een resourcegroep is een container met gerelateerde resources voor een Azure-oplossing. De resourcegroep kan alle resources voor de oplossing bevatten of enkel de resources die u als groep wilt beheren. U bepaalt hoe resources worden toegewezen aan resourcegroepen op basis van wat voor uw organisatie het meest zinvol is. Over het algemeen voegt u resources die dezelfde levens cyclus delen, toe aan dezelfde resource groep, zodat u deze eenvoudig kunt implementeren, bijwerken en verwijderen als groep.

De resourcegroep slaat metagegevens op over de resources. Dat is de reden waarom u moet aangeven waar die metagegevens moeten worden opgeslagen als u een locatie voor de resourcegroep opgeeft. In verband met nalevingsvereisten moet u er mogelijk voor zorgen dat uw gegevens worden opgeslagen in een bepaalde regio.

De resourcegroep slaat metagegevens op over de resources. Wanneer u een locatie voor de resource groep opgeeft, geeft u op waar de meta gegevens worden opgeslagen.

## <a name="create-resource-groups"></a>Resource groepen maken

Met het volgende CLI-script maakt u een resource groep en wordt vervolgens de resource groep weer gegeven.

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
echo "Enter the location (i.e. centralus):" &&
read location &&
az group create --name $resourceGroupName --location $location
```

## <a name="list-resource-groups"></a>Resource groepen weer geven

Het volgende CLI-script bevat de resource groepen onder uw abonnement.

```azurecli-interactive
az group list
```

Een resource groep ophalen:

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group show --name $resourceGroupName
```

## <a name="delete-resource-groups"></a>Resourcegroepen verwijderen

Met het volgende CLI-script wordt een resource groep verwijderd:

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group delete --name $resourceGroupName
```

Zie [Azure Resource Manager resource groep verwijderen](delete-resource-group.md)voor meer informatie over de manier waarop Azure Resource Manager het verwijderen van resources ordent.

## <a name="deploy-resources-to-an-existing-resource-group"></a>Resources implementeren voor een bestaande resource groep

Zie [resources implementeren voor een bestaande resource groep](manage-resources-cli.md#deploy-resources-to-an-existing-resource-group).

## <a name="deploy-a-resource-group-and-resources"></a>Een resource groep en-resources implementeren

U kunt een resource groep maken en resources voor de groep implementeren met behulp van een resource manager-sjabloon. Zie [resource groep maken en resources implementeren](../templates/deploy-to-subscription.md#resource-group-and-resources)voor meer informatie.

## <a name="redeploy-when-deployment-fails"></a>Opnieuw implementeren wanneer de implementatie mislukt

Deze functie wordt ook wel bekend als *Rollback bij fout*. Zie opnieuw [implementeren wanneer de implementatie mislukt](../templates/rollback-on-error.md)voor meer informatie.

## <a name="move-to-another-resource-group-or-subscription"></a>Verplaatsen naar een andere resource groep of een ander abonnement

U kunt de resources in de groep verplaatsen naar een andere resource groep. Zie [resources verplaatsen](manage-resources-cli.md#move-resources)voor meer informatie.

## <a name="lock-resource-groups"></a>Resource groepen vergren delen

Vergren delen voor komt dat andere gebruikers in uw organisatie per ongeluk essentiële resources verwijderen of wijzigen, zoals een Azure-abonnement, resource groep of resource. 

Met het volgende script wordt een resource groep vergrendeld zodat de resource groep niet kan worden verwijderd.

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az lock create --name LockGroup --lock-type CanNotDelete --resource-group $resourceGroupName  
```

Met het volgende script worden alle vergren delingen voor een resource groep opgehaald:

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az lock list --resource-group $resourceGroupName  
```

Met het volgende script wordt een vergren deling verwijderd:

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
echo "Enter the lock name:" &&
read lockName &&
az lock delete --name $lockName --resource-group $resourceGroupName
```

Zie voor meer informatie [Resources vergrendelen met Azure Resource Manager](lock-resources.md).

## <a name="tag-resource-groups"></a>Label resource groepen

U kunt Tags Toep assen op resource groepen en resources om uw assets logisch te organiseren. Zie [Tags gebruiken om uw Azure-resources te organiseren](tag-resources.md#azure-cli)voor meer informatie.

## <a name="export-resource-groups-to-templates"></a>Resource groepen exporteren naar sjablonen

Nadat u de resource groep hebt ingesteld, kunt u de Resource Manager-sjabloon voor de resource groep weer geven. Het exporteren van de sjabloon biedt twee voor delen:

- Automatiseer toekomstige implementaties van de oplossing omdat de sjabloon alle volledige infra structuur bevat.
- De syntaxis van de sjabloon leren door te kijken naar de JavaScript Object Notation (JSON) die uw oplossing vertegenwoordigt.

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group export --name $resourceGroupName  
```

Met het script wordt de sjabloon op de-console weer gegeven.  Kopieer de JSON en sla deze op als een bestand.

De functie sjabloon exporteren biedt geen ondersteuning voor het exporteren van Azure Data Factory-resources. Zie [een Data Factory in azure Data Factory kopiëren of klonen](https://aka.ms/exportTemplateViaAdf)voor meer informatie over het exporteren van Data Factory-resources.

Als u resources wilt exporteren die zijn gemaakt via het klassieke implementatie model, moet u [deze migreren naar het Resource Manager-implementatie model](https://aka.ms/migrateclassicresourcetoarm).

Zie voor meer informatie [het exporteren van één en meerdere resources naar sjabloon in azure Portal](../templates/export-template-portal.md).

## <a name="manage-access-to-resource-groups"></a>Toegang tot resource groepen beheren

[Op rollen gebaseerde toegangsbeheer (RBAC)](../../role-based-access-control/overview.md) is de manier waarop u de toegang tot resources in Azure beheert. Zie [toegang beheren met RBAC en Azure cli](../../role-based-access-control/role-assignments-cli.md)voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

- Zie [overzicht van Azure Resource Manager](overview.md)voor meer informatie Azure Resource Manager.
- Zie [inzicht krijgen in de structuur en de syntaxis van Azure Resource Manager sjablonen](../templates/template-syntax.md)voor meer informatie over de syntaxis van de Resource Manager-sjabloon.
- Zie [Stapsgewijze zelf studies](/azure/azure-resource-manager/)voor meer informatie over het ontwikkelen van sjablonen.
- Zie [sjabloon verwijzing](/azure/templates/)voor het weer geven van de Azure Resource Manager sjabloon schema's.