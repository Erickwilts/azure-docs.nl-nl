---
title: Een Azure Application Gateway - sjablonen maken
description: Dit artikel bevat instructies voor het maken van een Azure-toepassingsgateway met behulp van de Azure Resource Manager-sjabloon
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: conceptual
ms.date: 6/26/2019
ms.author: victorh
ms.openlocfilehash: a762e8c9ed1981173f3729837456ac2cfea081b8
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67449539"
---
# <a name="create-an-application-gateway-using-the-azure-resource-manager-template"></a>Een toepassingsgateway met behulp van de Azure Resource Manager-sjabloon maken

Azure Application Gateway is een load balancer in laag 7. De gateway biedt opties voor failovers en het routeren van HTTP-aanvragen tussen servers (on-premises en in de cloud). Application Gateway bevat veel ADC-functies (Application Delivery Controller), waaronder HTTP-taakverdeling, op cookies gebaseerde sessieaffiniteit, SSL-offload (Secure Sockets Layer), aangepaste statustests en ondersteuning voor meerdere locaties. Ga voor een volledige lijst van ondersteunde functies naar [overzicht van Application Gateway](application-gateway-introduction.md)

In dit artikel wordt u begeleid bij het downloaden en wijzigen van een bestaande [Azure Resource Manager-sjabloon](../azure-resource-manager/resource-group-authoring-templates.md) van GitHub en het implementeren van de sjabloon vanuit GitHub, Azure PowerShell en de Azure CLI.

Als u gewoon de sjabloon rechtstreeks vanuit GitHub zonder wijzigingen aan te implementeert, kunt u overslaan als u wilt een sjabloon implementeren vanuit GitHub.

## <a name="scenario"></a>Scenario

In dit scenario:

* Maak een toepassingsgateway met web application firewall.
* Maakt u een virtueel netwerk met de naam VirtualNetwork1 met een gereserveerd CIDR-blok van 10.0.0.0/16.
* Maakt u een subnet met de naam Appgatewaysubnet dat gebruikmaakt van 10.0.0.0/28 als CIDR-blok.
* Stelt u twee eerder geconfigureerde back-end-IP-adressen in voor de webservers die load balancing moeten verzorgen voor het verkeer. In deze voorbeeldsjabloon worden de back-end-IP-adressen 10.0.1.10 en 10.0.1.11 gebruikt.

> [!NOTE]
> Deze instellingen zijn de parameters voor deze sjabloon. Voor het aanpassen van de sjabloon, kunt u regels, de listener, SSL en andere opties in het bestand azuredeploy.json.

![Scenario](./media/application-gateway-create-gateway-arm-template/scenario.png)

## <a name="download-and-understand-the-azure-resource-manager-template"></a>De Azure Resource Manager-sjabloon downloaden en begrijpen

U kunt de bestaande Azure Resource Manager-sjabloon downloaden om een virtueel netwerk en twee subnets vanuit GitHub te maken. Vervolgens kunt u wijzigingen aanbrengen en de sjabloon opnieuw gebruiken. Volg hiervoor de volgende stappen:

1. Navigeer naar [toepassingsgateway maken met web application firewall is ingeschakeld](https://github.com/Azure/azure-quickstart-templates/tree/master/101-application-gateway-waf).
1. Klik op **azuredeploy.json** en vervolgens op **RAW**.
1. Sla het bestand op in een lokale map op uw computer.
1. Als u bekend met Azure Resource Manager-sjablonen bent, gaat u naar stap 7.
2. Open het bestand dat u hebt opgeslagen en bekijk de inhoud onder **parameters** in regel
3. Azure Resource Manager-sjabloonparameters bieden een tijdelijke aanduiding voor waarden die kunnen worden ingevuld tijdens de implementatie.

   | Parameter | Description |
   | --- | --- |
   | **subnetPrefix** |CIDR-blokkering voor het subnet van de gateway. |
   | **applicationGatewaySize** | Grootte van de toepassingsgateway.  WAF kan alleen middelgrote en grote. |
   | **backendIpaddress1** |IP-adres van de eerste webserver. |
   | **backendIpaddress2** |IP-adres van de tweede webserver. |
   | **wafEnabled** | Als u op of de WAF is ingeschakeld.|
   | **wafMode** | Modus van de web application firewall.  De beschikbare opties zijn **preventie** of **detectie**.|
   | **wafRuleSetType** | RuleSet-type voor WAF.  OWASP is momenteel de enige ondersteunde optie. |
   | **wafRuleSetVersion** |RuleSet-versie. OWASP CRS 2.2.9 en 3.0 zijn momenteel de ondersteunde opties. |

1. Controleer de inhoud onder **resources** en ziet u de volgende eigenschappen:

   * **type**. Het type resource dat door de sjabloon wordt aangemaakt. In dit geval wordt het type is `Microsoft.Network/applicationGateways`, die staat voor een toepassingsgateway.
   * **Naam**. Naam voor de resource. Let op het gebruik van `[parameters('applicationGatewayName')]`, wat betekent dat de naam is opgegeven als invoer door u of door een parameterbestand tijdens de implementatie.
   * **Eigenschappen**. Lijst met eigenschappen voor de resource. Deze sjabloon maakt tijdens het maken van de toepassingsgateway gebruik van het virtuele netwerk en het openbare IP-adres.

1. Ga terug naar [ https://github.com/Azure/azure-quickstart-templates/blob/master/101-application-gateway-waf/ ](https://github.com/Azure/azure-quickstart-templates/blob/master/101-application-gateway-waf).
1. Klik op **azuredeploy-parameters.json**, en klik vervolgens op **RAW**.
1. Sla het bestand op in een lokale map op uw computer.
1. Open het bestand dat u hebt opgeslagen en bewerk de waarden voor de parameters. Gebruik de volgende waarden voor het implementeren van de toepassingsgateway die in ons scenario wordt beschreven.

     ```json
     {
         "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
         "contentVersion": "1.0.0.0",
         "parameters": {
             "addressPrefix": {
             "value": "10.0.0.0/16"
             },
             "subnetPrefix": {
             "value": "10.0.0.0/28"
             },
             "applicationGatewaySize": {
             "value": "WAF_Medium"
             },
             "capacity": {
             "value": 2
             },
             "backendIpAddress1": {
             "value": "10.0.1.10"
             },
             "backendIpAddress2": {
             "value": "10.0.1.11"
             },
             "wafEnabled": {
             "value": true
             },
             "wafMode": {
             "value": "Detection"
             },
             "wafRuleSetType": {
             "value": "OWASP"
             },
             "wafRuleSetVersion": {
             "value": "3.0"
             }
         }
     }
     ```

1. Sla het bestand op. U kunt de JSON-sjabloon en parametersjabloon met online JSON-validatiehulpprogramma's zoals testen [JSlint.com](https://www.jslint.com/).

## <a name="deploy-the-azure-resource-manager-template-using-azure-powershell"></a>Implementeren van de Azure Resource Manager-sjabloon met behulp van Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Als u Azure PowerShell nog nooit hebt gebruikt, Zie: [Hoe u Azure PowerShell installeren en configureren](/powershell/azure/overview) en volg de instructies om te melden bij Azure en selecteer uw abonnement.

1. Verbinding maken met Azure

    ```azurepowershell
    Connect-AzAccount
    ```

1. Controleer de abonnementen voor het account.

    ```azurepowershell
    Get-AzSubscription
    ```

    U wordt gevraagd om te verifiëren met uw referenties.

1. Kies welk Azure-abonnement u wilt gebruiken.

    ```azurepowershell
    Select-AzSubscription -Subscriptionid "GUID of subscription"
    ```

1. Als nodig is, maakt u een resource-groep met de **New-AzureResourceGroup** cmdlet. In het volgende voorbeeld maakt u een resourcegroep met de naam AppgatewayRG op de locatie US - oost.

    ```azurepowershell
    New-AzResourceGroup -Name AppgatewayRG -Location "West US"
    ```

1. Voer de **New-AzResourceGroupDeployment** cmdlet voor het implementeren van het nieuwe virtuele netwerk met behulp van de vorige sjabloon en parameterbestanden bestanden dat u hebt gedownload en gewijzigd.
    
    ```azurepowershell
    New-AzResourceGroupDeployment -Name TestAppgatewayDeployment -ResourceGroupName AppgatewayRG `
    -TemplateFile C:\ARM\azuredeploy.json -TemplateParameterFile C:\ARM\azuredeploy-parameters.json
    ```

## <a name="deploy-the-azure-resource-manager-template-using-the-azure-cli"></a>De Azure Resource Manager-sjabloon met de Azure CLI implementeren

Volg de volgende stappen uit voor het implementeren van de Azure Resource Manager-sjabloon die u hebt gedownload met behulp van Azure CLI:

1. Als u Azure CLI nog nooit hebt gebruikt, raadpleegt u [De Azure CLI installeren en configureren](/cli/azure/install-azure-cli) en volgt u de instructies tot het punt waar u uw Azure-account en -abonnement moet selecteren.

1. Voer indien nodig de `az group create` opdracht voor het maken van een resourcegroep, zoals wordt weergegeven in het volgende codefragment. Hier ziet u de uitvoer van de opdracht. De lijst die na de uitvoer wordt weergegeven, beschrijft de gebruikte parameters. Zie voor meer informatie over resourcegroepen [Overzicht van Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

    ```azurecli
    az group create --location westus --name appgatewayRG
    ```
    
    **-n (of --name)** . Naam voor de nieuwe resourcegroep. In ons scenario is dit *appgatewayRG*.
    
    **-l (of --location)** . De Azure-regio waar de nieuwe resourcegroep wordt gemaakt. In ons scenario heeft *westus*.

1. Voer de `az group deployment create` cmdlet voor het implementeren van het nieuwe virtuele netwerk met behulp van de sjabloon en de parameter-bestanden u hebt gedownload en gewijzigd in de vorige stap. De lijst die na de uitvoer wordt weergegeven, beschrijft de gebruikte parameters.

    ```azurecli
    az group deployment create --resource-group appgatewayRG --name TestAppgatewayDeployment --template-file azuredeploy.json --parameters @azuredeploy-parameters.json
    ```

## <a name="deploy-the-azure-resource-manager-template-using-click-to-deploy"></a>De Azure Resource Manager-sjabloon met behulp van click-to-deploy implementeren

U kunt de Azure Resource Manager-sjablonen ook gebruiken via click-to-deploy. Dit is een eenvoudige manier om sjablonen te gebruiken in de Azure Portal.

1. Ga naar [maken van een toepassingsgateway met web application firewall](https://azure.microsoft.com/documentation/templates/101-application-gateway-waf/).

1. Klik op **Implementeren in Azure**.

    ![Implementeren in Azure](./media/application-gateway-create-gateway-arm-template/deploytoazure.png)
    
1. Vul de parameters voor de implementatiesjabloon in in de portal en klik op **OK**.

    ![Parameters](./media/application-gateway-create-gateway-arm-template/ibiza1.png)
    
1. Selecteer **ik ga akkoord met de voorwaarden en bepalingen bovenstaande** en klikt u op **aankoop**.

1. Klik op de pagina van de implementatie van aangepaste **maken**.

## <a name="providing-certificate-data-to-resource-manager-templates"></a>Certificaatgegevens naar Resource Manager-sjablonen bieden

Als u SSL gebruikt met een sjabloon, wordt het certificaat moet worden opgegeven in een base64-tekenreeks in plaats van dat wordt geüpload. Als u wilt converteren een pfx- of cer naar een tekenreeks met Base 64 gebruikt u een van de volgende opdrachten. De volgende opdrachten wordt het certificaat converteren naar een Base 64-tekenreeks, die kan worden opgegeven in de sjabloon. De verwachte uitvoer is een tekenreeks die kan worden opgeslagen in een variabele en geplakt in de sjabloon.

### <a name="macos"></a>macOS

```bash
cert=$( base64 <certificate path and name>.pfx )
echo $cert
```

### <a name="windows"></a>Windows
```powershell
[System.Convert]::ToBase64String([System.IO.File]::ReadAllBytes("<certificate path and name>.pfx"))
```

## <a name="delete-all-resources"></a>Alle resources verwijderen

Als u wilt verwijderen van alle resources die in dit artikel is gemaakt, voert u een van de volgende stappen uit:

### <a name="azure-powershell"></a>Azure PowerShell

```azurepowershell
Remove-AzResourceGroup -Name appgatewayRG
```

### <a name="azure-cli"></a>Azure-CLI

```azurecli
az group delete --name appgatewayRG
```

## <a name="next-steps"></a>Volgende stappen

Als u configureren van SSL-offload wilt, Zie: [Een toepassingsgateway voor SSL-offload configureren](application-gateway-ssl.md).

Als u een toepassingsgateway wilt voor gebruik met een interne load balancer configureren, Zie: [Een toepassingsgateway maken met een interne load balancer (ILB)](application-gateway-ilb.md).

Als u meer informatie wilt over de algemene opties voor load balancing, raadpleegt u:

* [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)