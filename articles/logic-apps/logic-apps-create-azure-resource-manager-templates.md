---
title: Sjablonen voor logische apps maken voor implementatie-Azure Logic Apps
description: Meer informatie over het maken van Azure Resource Manager sjablonen voor het automatiseren van de implementatie in Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 07/26/2019
ms.openlocfilehash: 7d216a3706c13a5fff312850e244a521ab22ae9e
ms.sourcegitcommit: 04ec7b5fa7a92a4eb72fca6c6cb617be35d30d0c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/22/2019
ms.locfileid: "68386536"
---
# <a name="create-azure-resource-manager-templates-to-automate-deployment-for-azure-logic-apps"></a>Azure Resource Manager sjablonen maken voor het automatiseren van de implementatie voor Azure Logic Apps

In dit artikel worden de manieren beschreven waarop u een [Azure Resource Manager sjabloon](../azure-resource-manager/resource-group-overview.md) voor uw logische app kunt maken om het maken en implementeren van uw logische app te automatiseren. Zie [overzicht: voor een overzicht van de structuur en syntaxis voor een sjabloon die uw werk stroom definitie bevat en andere resources die nodig zijn voor implementatie. Implementatie voor Logic apps automatiseren met Azure Resource Manager sjablonen](logic-apps-azure-resource-manager-templates-overview.md).

Azure Logic Apps biedt een [vooraf gemaakte Logic apps Azure Resource Manager sjabloon](https://github.com/Azure/azure-quickstart-templates/blob/master/101-logic-app-create/azuredeploy.json) die u opnieuw kunt gebruiken, niet alleen voor het maken van logische apps, maar ook voor het definiëren van de resources en para meters die moeten worden gebruikt voor de implementatie. U kunt deze sjabloon gebruiken voor uw eigen bedrijfs scenario's of de sjabloon aanpassen aan uw vereisten. Zie de volgende onderwerpen voor meer informatie over Azure Resource Manager sjablonen:

* [Structuur en syntaxis van Azure Resource Manager sjabloon](../azure-resource-manager/resource-group-authoring-templates.md)
* [Azure Resource Manager sjablonen ontwerpen](../azure-resource-manager/resource-group-authoring-templates.md)
* [Azure Resource Manager-sjablonen voor consistentie van de cloud ontwikkelen](../azure-resource-manager/templates-cloud-consistency.md)

> [!IMPORTANT]
> Verbindingen in uw sjabloon moeten dezelfde Azure-resource groep en-locatie gebruiken als uw logische app.

<a name="visual-studio"></a>

## <a name="create-templates-with-visual-studio"></a>Sjablonen maken met Visual Studio

Voor de eenvoudigste manier om geldige sjablonen voor logische apps met para meters te maken die in de meeste gevallen gereed zijn voor implementatie, gebruikt u Visual Studio (gratis Community Edition of hoger) en de Azure Logic Apps-Hulpprogram Ma's voor Visual Studio. U kunt vervolgens [uw logische app maken in Visual Studio](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md) of [een bestaande logische app zoeken en downloaden van de Azure Portal naar Visual Studio](../logic-apps/manage-logic-apps-with-visual-studio.md).

Door uw logische app te downloaden, krijgt u een sjabloon die de definities voor uw logische app en andere resources, zoals verbindingen, bevat. De sjabloon bevat ook *parameterizes*of definieert para meters voor de waarden die worden gebruikt voor het implementeren van uw logische app en andere resources. U kunt de waarden voor deze para meters opgeven in een afzonderlijk parameter bestand. Op die manier kunt u deze waarden gemakkelijker wijzigen op basis van uw implementatie behoeften. Zie de volgende onderwerpen voor meer informatie:

* [Logische apps maken met Visual Studio](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md)
* [Logische apps beheren met Visual Studio](../logic-apps/manage-logic-apps-with-visual-studio.md)

<a name="azure-powershell"></a>

## <a name="create-templates-with-azure-powershell"></a>Sjablonen maken met Azure PowerShell

U kunt Resource Manager-sjablonen maken met behulp van Azure PowerShell met de [module LogicAppTemplate](https://github.com/jeffhollan/LogicAppTemplateCreator). Deze open-source module evalueert eerst uw logische app en alle verbindingen die de logische app gebruikt. De module genereert vervolgens sjabloon resources met de vereiste para meters voor implementatie.

Stel bijvoorbeeld dat u een logische app hebt die een bericht ontvangt van een Azure Service Bus wachtrij en gegevens uploadt naar een Azure-SQL database. De module behoudt alle Orchestration Logic en parameterizes de verbindings reeksen SQL en Service Bus, zodat u deze waarden kunt opgeven en wijzigen op basis van uw implementatie behoeften.

### <a name="install-powershell-modules"></a>Power shell-modules installeren

1. Als u dat nog niet hebt gedaan, installeert u [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps).

1. Voor de eenvoudigste manier om de LogicAppTemplate-module te installeren vanaf de [PowerShell Gallery](https://www.powershellgallery.com/packages/LogicAppTemplate), voert u deze opdracht uit:

   ```text
   PS> Install-Module -Name LogicAppTemplate
   ```

   Als u wilt bijwerken naar de nieuwste versie, voert u deze opdracht uit:

   ```text
   PS> Update-Module -Name LogicAppTemplate
   ```

Als u hand matig wilt installeren, volgt u de stappen in GitHub voor [Logic app-sjabloon Maker](https://github.com/jeffhollan/LogicAppTemplateCreator).

### <a name="install-azure-resource-manager-client"></a>Azure Resource Manager-client installeren

Als u de LogicAppTemplate-module wilt gebruiken met een toegangs token voor Azure-tenants en-abonnementen, installeert u het [Azure Resource Manager-client hulpprogramma](https://github.com/projectkudu/ARMClient), een eenvoudig opdracht regel programma dat de Azure Resource Manager-API aanroept.

Wanneer u de `Get-LogicAppTemplate` opdracht uitvoert met dit hulp programma, wordt met de opdracht eerst een toegangs token opgehaald via het ARMClient-hulp programma, wordt het token door sluizen naar het Power shell-script en wordt de sjabloon als een JSON-bestand gemaakt. Zie dit [artikel over het Azure Resource Manager-client hulpprogramma](https://blog.davidebbo.com/2015/01/azure-resource-manager-client.html)voor meer informatie over het hulp programma.

### <a name="generate-template-with-powershell"></a>Sjabloon genereren met Power shell

Als u uw sjabloon wilt genereren na de installatie van de LogicAppTemplate-module, voert u deze Power shell-opdracht uit:

```text
PS> Get-LogicAppTemplate
```

Als u de aanbeveling voor pijpleidingen in een token van het [Azure Resource Manager-client hulpprogramma](https://github.com/projectkudu/ARMClient)wilt volgen, voert `$SubscriptionId` u deze opdracht uit in plaats van uw Azure-abonnements-id:

```text
PS> armclient token $SubscriptionId | Get-LogicAppTemplate -LogicApp <logic-app-name> -ResourceGroup <Azure-resource-group-name> -SubscriptionId $SubscriptionId -Verbose | Out-File C:\template.json
```

Na extractie kunt u een parameter bestand maken op basis van uw sjabloon door de volgende opdracht uit te voeren:

```text
PS> Get-ParameterTemplate -TemplateFile $filename | Out-File '<parameters-file-name>.json'
```

Voer de volgende opdracht uit voor extractie met Azure Key Vault referenties (alleen statisch):

```text
PS> Get-ParameterTemplate -TemplateFile $filename -KeyVault Static | Out-File $fileNameParameter
```

| Parameters | Vereist | Description |
|------------|----------|-------------|
| TemplateFile | Ja | Het bestandspad naar het sjabloon bestand |
| KeyVault | Nee | Een Enum die beschrijft hoe mogelijke sleutel kluis waarden moeten worden afgehandeld. De standaardwaarde is `None`. |
||||

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Sjablonen voor logische Apps implementeren](../logic-apps/logic-apps-deploy-azure-resource-manager-templates.md)
