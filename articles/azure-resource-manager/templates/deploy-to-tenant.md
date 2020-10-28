---
title: Resources implementeren voor Tenant
description: Hierin wordt beschreven hoe u resources implementeert in het Tenant bereik in een Azure Resource Manager sjabloon.
ms.topic: conceptual
ms.date: 10/22/2020
ms.openlocfilehash: 854ccbd43509b6c0b5a04357844c78c32b7e6396
ms.sourcegitcommit: 4cb89d880be26a2a4531fedcc59317471fe729cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/27/2020
ms.locfileid: "92668693"
---
# <a name="tenant-deployments-with-arm-templates"></a>Tenant implementaties met ARM-sjablonen

Als uw organisatie is verouderd, moet u mogelijk [beleid](../../governance/policy/overview.md) of Azure [RBAC (op rollen gebaseerd toegangs beheer)](../../role-based-access-control/overview.md) voor uw Azure AD-Tenant definiëren en toewijzen. Met sjablonen op Tenant niveau kunt u declaratief beleid Toep assen en rollen toewijzen op globaal niveau.

## <a name="supported-resources"></a>Ondersteunde resources

Niet alle resource typen kunnen op het Tenant niveau worden geïmplementeerd. In deze sectie wordt een overzicht gegeven van de resource typen die worden ondersteund.

Gebruik voor Azure-beleid:

* [policyAssignments](/azure/templates/microsoft.authorization/policyassignments)
* [policyDefinitions](/azure/templates/microsoft.authorization/policydefinitions)
* [policySetDefinitions](/azure/templates/microsoft.authorization/policysetdefinitions)

Gebruik voor op rollen gebaseerd toegangs beheer voor Azure (Azure RBAC):

* [roleAssignments](/azure/templates/microsoft.authorization/roleassignments)

Voor geneste sjablonen die worden geïmplementeerd op beheer groepen,-abonnementen of-resource groepen, gebruikt u:

* [implementaties](/azure/templates/microsoft.resources/deployments)

Voor het maken van beheer groepen gebruikt u:

* [managementGroups](/azure/templates/microsoft.management/managementgroups)

Voor het beheren van kosten gebruikt u:

* [billingProfiles](/azure/templates/microsoft.billing/billingaccounts/billingprofiles)
* [Schriften](/azure/templates/microsoft.billing/billingaccounts/billingprofiles/instructions)
* [invoiceSections](/azure/templates/microsoft.billing/billingaccounts/billingprofiles/invoicesections)

## <a name="schema"></a>Schema

Het schema dat u voor Tenant implementaties gebruikt, wijkt af van het schema voor implementaties van resource groepen.

Voor sjablonen gebruikt u:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-08-01/tenantDeploymentTemplate.json#",
    ...
}
```

Het schema voor een parameter bestand is hetzelfde voor alle implementatie bereiken. Gebruik voor parameter bestanden:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
    ...
}
```

## <a name="required-access"></a>Vereiste toegang

De principal-implementatie van de sjabloon moet machtigingen hebben om resources te maken in het Tenant bereik. De principal moet gemachtigd zijn om de implementatie acties ( `Microsoft.Resources/deployments/*` ) uit te voeren en om de resources te maken die in de sjabloon zijn gedefinieerd. Als u bijvoorbeeld een beheer groep wilt maken, moet de principal de machtiging Inzender hebben op het Tenant bereik. Als u roltoewijzingen wilt maken, moet de principal eigenaar toestemming hebben.

De globale beheerder voor de Azure Active Directory is niet automatisch gemachtigd om rollen toe te wijzen. De globale beheerder moet de volgende stappen uitvoeren om sjabloon implementaties in te scha kelen in het Tenant bereik:

1. De toegang tot het account verhogen zodat de globale beheerder rollen kan toewijzen. Zie voor meer informatie [Toegang verhogen om alle Azure-abonnementen en beheergroepen te beheren](../../role-based-access-control/elevate-access-global-admin.md).

1. Wijs eigenaar of bijdrager toe aan de principal die de sjablonen moet implementeren.

   ```azurepowershell-interactive
   New-AzRoleAssignment -SignInName "[userId]" -Scope "/" -RoleDefinitionName "Owner"
   ```

   ```azurecli-interactive
   az role assignment create --assignee "[userId]" --scope "/" --role "Owner"
   ```

De principal heeft nu de vereiste machtigingen voor het implementeren van de sjabloon.

## <a name="deployment-commands"></a>Implementatie opdrachten

De opdrachten voor Tenant implementaties wijken af van de opdrachten voor het implementeren van resource groepen.

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Gebruik [AZ Deployment Tenant Create](/cli/azure/deployment/tenant#az-deployment-tenant-create)voor Azure cli:

```azurecli-interactive
az deployment tenant create \
  --name demoTenantDeployment \
  --location WestUS \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/tenant-deployments/new-mg/azuredeploy.json"
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Voor Azure PowerShell gebruikt u [New-AzTenantDeployment](/powershell/module/az.resources/new-aztenantdeployment).

```azurepowershell-interactive
New-AzTenantDeployment `
  -Name demoTenantDeployment `
  -Location "West US" `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/tenant-deployments/new-mg/azuredeploy.json"
```

---

Zie voor meer gedetailleerde informatie over implementatie opdrachten en opties voor het implementeren van ARM-sjablonen:

* [Resources implementeren met ARM-sjablonen en Azure Portal](deploy-portal.md)
* [Resources implementeren met ARM-sjablonen en Azure CLI](deploy-cli.md)
* [Resources implementeren met ARM-sjablonen en Azure PowerShell](deploy-powershell.md)
* [Resources implementeren met ARM-sjablonen en Azure Resource Manager REST API](deploy-rest.md)
* [Een implementatie knop gebruiken voor het implementeren van sjablonen uit de GitHub-opslag plaats](deploy-to-azure-button.md)
* [ARM-sjablonen implementeren vanuit Cloud Shell](deploy-cloud-shell.md)

## <a name="deployment-scopes"></a>Implementatie bereiken

Wanneer u implementeert in een beheer groep, kunt u resources implementeren voor het volgende:

* de Tenant
* beheer groepen binnen de Tenant
* geabonneerd
* resource groepen (via twee geneste implementaties)
* [uitbreidings bronnen](scope-extension-resources.md) kunnen worden toegepast op resources

De gebruiker die de sjabloon implementeert, moet toegang hebben tot het opgegeven bereik.

In deze sectie wordt beschreven hoe u verschillende bereiken kunt opgeven. U kunt deze verschillende bereiken combi neren in één sjabloon.

### <a name="scope-to-tenant"></a>Bereik naar Tenant

Resources die zijn gedefinieerd in de sectie resources van de sjabloon worden toegepast op de Tenant.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/scope/default-tenant.json" highlight="5":::

### <a name="scope-to-management-group"></a>Bereik voor beheer groep

Als u een beheer groep in de Tenant wilt richten, voegt u een geneste implementatie toe en geeft u de `scope` eigenschap op.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/scope/tenant-to-mg.json" highlight="10,17,22":::

### <a name="scope-to-subscription"></a>Bereik voor abonnement

U kunt ook abonnementen richten binnen de Tenant. De gebruiker die de sjabloon implementeert, moet toegang hebben tot het opgegeven bereik.

Als u een abonnement in de Tenant wilt instellen, gebruikt u een geneste implementatie en de `subscriptionId` eigenschap.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/scope/tenant-to-subscription.json" highlight="10,18":::

## <a name="deployment-location-and-name"></a>Locatie en naam van de implementatie

Voor implementaties op Tenant niveau moet u een locatie opgeven voor de implementatie. De locatie van de implementatie is gescheiden van de locatie van de resources die u implementeert. De implementatie locatie geeft aan waar de implementatie gegevens moeten worden opgeslagen.

U kunt een naam opgeven voor de implementatie of de naam van de standaard implementatie gebruiken. De standaard naam is de naam van het sjabloon bestand. Als u bijvoorbeeld een sjabloon met de naam **azuredeploy.jsop** implementeert, maakt de standaard implementatie naam **azuredeploy** .

Voor elke implementatie naam is de locatie onveranderbaar. U kunt geen implementatie op één locatie maken wanneer er een bestaande implementatie met dezelfde naam op een andere locatie is. Als u de fout code krijgt `InvalidDeploymentLocation` , moet u een andere naam of dezelfde locatie gebruiken als de vorige implementatie voor die naam.

## <a name="create-management-group"></a>Beheergroep maken

Met de [volgende sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/tenant-deployments/new-mg) maakt u een beheer groep.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-08-01/tenantDeploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "mgName": {
      "type": "string",
      "defaultValue": "[concat('mg-', uniqueString(newGuid()))]"
    }
  },
  "resources": [
    {
      "type": "Microsoft.Management/managementGroups",
      "apiVersion": "2019-11-01",
      "name": "[parameters('mgName')]",
      "properties": {
      }
    }
  ]
}
```

## <a name="assign-role"></a>Rol toewijzen

De [volgende sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/tenant-deployments/tenant-role-assignment) wijst een rol toe aan het Tenant bereik.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-08-01/tenantDeploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "principalId": {
      "type": "string",
      "metadata": {
        "description": "principalId if the user that will be given contributor access to the resourceGroup"
      }
    },
    "roleDefinitionId": {
      "type": "string",
      "defaultValue": "8e3af657-a8ff-443c-a75c-2fe8c4bcb635",
      "metadata": {
        "description": "roleDefinition for the assignment - default is owner"
      }
    }
  },
  "variables": {
    // This creates an idempotent guid for the role assignment
    "roleAssignmentName": "[guid('/', parameters('principalId'), parameters('roleDefinitionId'))]"
  },
  "resources": [
    {
      "name": "[variables('roleAssignmentName')]",
      "type": "Microsoft.Authorization/roleAssignments",
      "apiVersion": "2019-04-01-preview",
      "properties": {
        "roleDefinitionId": "[tenantResourceId('Microsoft.Authorization/roleDefinitions', parameters('roleDefinitionId'))]",
        "principalId": "[parameters('principalId')]",
        "scope": "/"
      }
    }
  ]
}
```

## <a name="next-steps"></a>Volgende stappen

* Zie [Azure-roltoewijzingen toevoegen met Azure Resource Manager sjablonen](../../role-based-access-control/role-assignments-template.md)voor meer informatie over het toewijzen van rollen.
* U kunt ook sjablonen implementeren op [abonnements niveau](deploy-to-subscription.md) of op het niveau van de [beheer groep](deploy-to-management-group.md).
