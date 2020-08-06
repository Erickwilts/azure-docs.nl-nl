---
title: 'Zelfstudie: een lokaal Azure Resource Manager-sjabloon implementeren'
description: Ontdek hoe u een Azure Resource Manager-sjabloon kunt implementeren vanaf uw lokale computer
ms.date: 05/20/2020
ms.topic: tutorial
ms.author: jgao
ms.custom: devx-track-azurecli
ms.openlocfilehash: ef52d20b20446e3b8f3b761949bf255b99d358cc
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2020
ms.locfileid: "87499582"
---
# <a name="tutorial-deploy-a-local-azure-resource-manager-template"></a>Zelfstudie: een lokaal Azure Resource Manager-sjabloon implementeren

Ontdek hoe u een Azure Resource Manager-sjabloon kunt implementeren vanaf uw lokale computer. Dit neemt ongeveer **8 minuten** in beslag.

Deze zelfstudie is de eerste van een reeks. In deze reeks deelt u het sjabloon op in modules door een gekoppelde sjabloon te maken. U slaat het gekoppelde sjabloon op in een opslagaccount en beveiligt dit met een SAS-token. U leert ook hoe u een DevOps-pijplijn maakt om sjablonen te implementeren. Deze reeks gaat over de implementatie van sjablonen.  Als u wilt leren hoe u een sjabloon kunt implementeren, bekijk dan de [zelfstudies voor beginners](./template-tutorial-create-first-template.md).

## <a name="get-tools"></a>Hulpprogramma's installeren

Laten we er allereerst voor zorgen dat u de nodige hulpprogramma's heeft om sjablonen te implementeren.

### <a name="command-line-deployment"></a>Implementatie via opdrachtregels

U heeft Azure PowerShell of Azure CLI nodig om het sjabloon te implementeren. Zie voor installatie-instructies:

- [Azure PowerShell installeren](/powershell/azure/install-az-ps)
- [Azure CLI installeren in Windows](/cli/azure/install-azure-cli-windows)
- [Azure CLI installeren in Linux](/cli/azure/install-azure-cli-linux)

Nadat u Azure PowerShell of Azure CLI hebt geïnstalleerd moet u zich voor de eerste keer aanmelden. Bekijk [Aanmelden - PowerShell](/powershell/azure/install-az-ps#sign-in) of [Aanmelden - Azure CLI](/cli/azure/get-started-with-azure-cli#sign-in) voor ondersteuning.

### <a name="editor-optional"></a>Editor (optioneel)

Sjablonen zijn JSON-bestanden. Om sjablonen te controleren/bewerken heeft u een goede JSON-editor nodig. We raden Visual Studio Code met de extensie Resource Manager Tools aan. Als u deze hulpprogramma's wilt installeren, raadpleegt u [Quickstart: Azure Resource Manager-sjablonen maken met Visual Studio Code](quickstart-create-templates-use-visual-studio-code.md).

## <a name="review-template"></a>Sjabloon controleren

Het sjabloon implementeert een opslagaccount, App Service-plan en webtoepassing. Als u het sjabloon wilt aanmaken, doorloop dan de [zelfstudie over Snelstartsjablonen](template-tutorial-quickstart-template.md). Dat is echter geen vereiste om deze zelfstudie te voltooien.

:::code language="json" source="~/resourcemanager-templates/get-started-deployment/local-template/azuredeploy.json":::

> [!IMPORTANT]
> Opslagaccountnamen moeten tussen 3 en 24 tekens lang zijn en mogen alleen getallen en kleine letters bevatten. De naam moet uniek zijn. In het sjabloon is de opslagaccountnaam de projectnaam met 'store' daaraan toegevoegd. De projectnaam moet tussen 3 en 11 tekens lang zijn. De projectnaam moet dus voldoen aan de vereisten voor de opslagaccountnaam en minder dan 11 tekens lang zijn.

Sla een kopie van de sjabloon op uw lokale computer op met de extensie .json, bijvoorbeeld azuredeploy.json. Verderop in deze sjabloon implementeert u deze sjabloon.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan met uw Azure-referenties om aan de slag te gaan met Azure PowerShell/Azure CLI en een sjabloon te implementeren.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
Connect-AzAccount
```

# <a name="azure-cli"></a>[Azure-CLI](#tab/azure-cli)

```azurecli
az login
```

---

Als u meerdere Azure-abonnementen hebt, selecteert u het abonnement dat u wilt gebruiken:

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
Set-AzContext [SubscriptionID/SubscriptionName]
```

# <a name="azure-cli"></a>[Azure-CLI](#tab/azure-cli)

```azurecli
az account set --subscription [SubscriptionID/SubscriptionName]
```

---

## <a name="create-resource-group"></a>Een resourcegroep maken

Wanneer u een sjabloon implementeert, geeft u een resourcegroep op die de resources zal bevatten. Voordat u de opdracht voor de implementatie uitvoert, maakt u de resourcegroep met behulp van Azure CLI of Azure PowerShell. Selecteer de tabbladen in de volgende codesectie om te kiezen tussen Azure PowerShell en Azure CLI. De CLI-voorbeelden in dit artikel zijn geschreven voor de Bash-shell.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
$projectName = Read-Host -Prompt "Enter a project name that is used to generate resource and resource group names"
$resourceGroupName = "${projectName}rg"

New-AzResourceGroup `
  -Name $resourceGroupName `
  -Location "Central US"
```

# <a name="azure-cli"></a>[Azure-CLI](#tab/azure-cli)

```azurecli
echo "Enter a project name that is used to generate resource and resource group names:"
read projectName
resourceGroupName="${projectName}rg"

az group create \
  --name $resourceGroupName \
  --location "Central US"
```

---

## <a name="deploy-template"></a>Sjabloon implementeren

Gebruik een of beide implementatie-opties om de sjabloon te implementeren.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
$projectName = Read-Host -Prompt "Enter the same project name"
$templateFile = Read-Host -Prompt "Enter the template file path and file name"
$resourceGroupName = "${projectName}rg"

New-AzResourceGroupDeployment `
  -Name DeployLocalTemplate `
  -ResourceGroupName $resourceGroupName `
  -TemplateFile $templateFile `
  -projectName $projectName `
  -verbose
```

Zie [Resources implementeren met Resource Manager-sjablonen en Azure PowerShell](./deploy-powershell.md) voor meer informatie over de implementatie van sjablonen met Azure PowerShell.

# <a name="azure-cli"></a>[Azure-CLI](#tab/azure-cli)

```azurecli
echo "Enter the same project name:"
read projectName
echo "Enter the template file path and file name:"
read templateFile
resourceGroupName="${projectName}rg"

az deployment group create \
  --name DeployLocalTemplate \
  --resource-group $resourceGroupName \
  --template-file $templateFile \
  --parameters projectName=$projectName \
  --verbose
```

Zie [Resources implementeren met Resource Manager-sjablonen en Azure CLI](./deploy-cli.md) voor meer informatie over de implementatie van sjablonen met Azure CLI.

---

## <a name="clean-up-resources"></a>Resources opschonen

Schoon de geïmplementeerd resources op door de resourcegroep te verwijderen.

1. Selecteer **Resourcegroep** in het linkermenu van Azure Portal.
2. Voer de naam van de resourcegroep in het veld **Filter by name** in.
3. Selecteer de naam van de resourcegroep.
4. Selecteer **Resourcegroep verwijderen** in het bovenste menu.

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u een lokale sjabloon kunt implementeren. In de volgende zelfstudie splitst u de sjabloon op in een hoofdsjabloon en een gekoppelde sjabloon en leert u hoe u de gekoppelde sjabloon kunt opslaan en beveiligen.

> [!div class="nextstepaction"]
> [Een gekoppelde sjabloon implementeren](./deployment-tutorial-linked-template.md)
