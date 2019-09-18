---
title: Verplaats Azure open bare IP naar een andere Azure-regio met behulp van Azure PowerShell
description: Gebruik Azure Resource Manager sjabloon om het open bare Azure-IP-adres van een Azure-regio naar een andere te verplaatsen met behulp van Azure PowerShell.
author: asudbring
ms.service: virtual-network
ms.topic: article
ms.date: 08/29/2019
ms.author: allensu
ms.openlocfilehash: d18dfa7ebed3aefbf6fdb3ffdb6fdd2cf2160cb4
ms.sourcegitcommit: 0fab4c4f2940e4c7b2ac5a93fcc52d2d5f7ff367
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/17/2019
ms.locfileid: "71038925"
---
# <a name="move-azure-public-ip-to-another-region-using-azure-powershell"></a>De open bare Azure-IP naar een andere regio verplaatsen met behulp van Azure PowerShell

Er zijn verschillende scenario's waarin u uw bestaande open bare Azure-Ip's wilt verplaatsen van de ene regio naar een andere. U kunt bijvoorbeeld een openbaar IP-adres maken met dezelfde configuratie en SKU voor testen. Het is ook mogelijk dat u een openbaar IP-adres naar een andere regio wilt verplaatsen als onderdeel van de planning voor nood herstel.

Open bare Azure-Ip's zijn regio specifiek en kunnen niet worden verplaatst van de ene regio naar de andere. U kunt echter een Azure Resource Manager sjabloon gebruiken om de bestaande configuratie van een openbaar IP-adres te exporteren.  U kunt de resource vervolgens in een andere regio zetten door het open bare IP-adres naar een sjabloon te exporteren, de para meters te wijzigen zodat deze overeenkomen met de doel regio en vervolgens de sjabloon te implementeren in de nieuwe regio.  Zie [resource groepen exporteren naar sjablonen](https://docs.microsoft.com/azure/azure-resource-manager/manage-resource-groups-powershell#export-resource-groups-to-templates) voor meer informatie over Resource Manager en sjablonen


## <a name="prerequisites"></a>Vereisten

- Zorg ervoor dat het open bare IP-adres van Azure zich bevindt in de Azure-regio van waaruit u wilt verplaatsen.

- Open bare Azure-Ip's kunnen niet tussen regio's worden verplaatst.  U moet het nieuwe open bare IP-adres koppelen aan resources in de doel regio.

- Als u een open bare IP-configuratie wilt exporteren en een sjabloon wilt implementeren voor het maken van een openbaar IP-adres in een andere regio, hebt u de rol netwerk bijdrager of hoger nodig.
   
- Identificeer de bron netwerk indeling en alle resources die u momenteel gebruikt. Deze indeling bevat, maar is niet beperkt tot load balancers, netwerk beveiligings groepen (Nsg's) en virtuele netwerken.

- Controleer of u met uw Azure-abonnement open bare Ip's kunt maken in de doel regio die wordt gebruikt. Neem contact op met ondersteuning voor het inschakelen van het vereiste quotum.

- Zorg ervoor dat uw abonnement voldoende bronnen heeft ter ondersteuning van het toevoegen van open bare IP-adressen voor dit proces.  Raadpleeg [Azure-abonnement en -servicelimieten, quotums en beperkingen](https://docs.microsoft.com/azure/azure-subscription-service-limits#networking-limits).


## <a name="prepare-and-move"></a>Voorbereiden en verplaatsen
De volgende stappen laten zien hoe u de open bare IP voorbereidt voor de configuratie verplaatsing met behulp van een resource manager-sjabloon en de open bare IP-configuratie verplaatst naar de doel regio met behulp van Azure PowerShell.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

### <a name="export-the-template-and-deploy-from-a-script"></a>De sjabloon exporteren en implementeren vanuit een script

1. Meld u aan bij uw Azure-abonnement met de opdracht [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-2.5.0) en volg de instructies op het scherm:
    
    ```azurepowershell-interactive
    Connect-AzAccount
    ```

2. Haal de resource-ID op van het open bare IP-adres dat u wilt verplaatsen naar de doel regio en plaats deze in een variabele met [Get-AzPublicIPAddress](https://docs.microsoft.com/powershell/module/az.network/get-azpublicipaddress?view=azps-2.6.0):

    ```azurepowershell-interactive
    $sourcePubIPID = (Get-AzPublicIPaddress -Name <source-public-ip-name> -ResourceGroupName <source-resource-group-name>).Id

    ```
3. Exporteer het virtuele bron netwerk naar een. JSON-bestand naar de map waar u de opdracht [export-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/export-azresourcegroup?view=azps-2.6.0)uitvoert:
   
   ```azurepowershell-interactive
   Export-AzResourceGroup -ResourceGroupName <source-resource-group-name> -Resource $sourceVNETID -IncludeParameterDefaultValue
   ```

4. Het bestand dat u hebt gedownload krijgt de naam van de resource groep waaruit de resource is geëxporteerd.  Zoek het bestand dat is geëxporteerd uit de opdracht  **\<met de naam resource-group-name >. json** en open het in een editor naar keuze:
   
   ```azurepowershell
   notepad <source-resource-group-name>.json
   ```

5. Als u de para meter van de naam van het open bare IP-adres wilt bewerken, wijzigt u de eigenschap **DefaultValue** van de bron-open bare IP-naam in de naam van het open bare IP-adres. Controleer of de naam tussen aanhalings tekens is:
    
    ```json
        {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "publicIPAddresses_myVM1pubIP_name": {
        "defaultValue": "<target-publicip-name>",
        "type": "String"
        }
    }

    ```

6. Als u de doel regio wilt bewerken waar het open bare IP-adres wordt verplaatst, wijzigt u de eigenschap **Location** onder resources:

    ```json
            "resources": [
            {
            "type": "Microsoft.Network/publicIPAddresses",
            "apiVersion": "2019-06-01",
            "name": "[parameters('publicIPAddresses_myPubIP_name')]",
            "location": "<target-region>",
            "sku": {
                "name": "Basic",
                "tier": "Regional"
            },
            "properties": {
                "provisioningState": "Succeeded",
                "resourceGuid": "7549a8f1-80c2-481a-a073-018f5b0b69be",
                "ipAddress": "52.177.6.204",
                "publicIPAddressVersion": "IPv4",
                "publicIPAllocationMethod": "Dynamic",
                "idleTimeoutInMinutes": 4,
                "ipTags": []
               }
               }
             ]             
    ```
  
7. Als u regio codes wilt ophalen, kunt u de Azure PowerShell cmdlet [Get-AzLocation](https://docs.microsoft.com/powershell/module/az.resources/get-azlocation?view=azps-1.8.0) gebruiken door de volgende opdracht uit te voeren:

    ```azurepowershell-interactive

    Get-AzLocation | format-table
    
    ```
8. U kunt ook andere para meters in de sjabloon wijzigen als u ervoor kiest en zijn optioneel, afhankelijk van uw vereisten:

    * **SKU** : u kunt de SKU van het open bare IP-adres in de configuratie wijzigen van standaard in Basic of Basic naar Standard door de eigenschap **SKU** > **name** te wijzigen in het  **\<bestand resource-group-name >. json** :

         ```json
            "resources": [
                   {
                    "type": "Microsoft.Network/publicIPAddresses",
                    "apiVersion": "2019-06-01",
                    "name": "[parameters('publicIPAddresses_myPubIP_name')]",
                    "location": "<target-region>",
                    "sku": {
                        "name": "Basic",
                        "tier": "Regional"
                    },
         ```

         Zie [een openbaar IP-adres maken, wijzigen of verwijderen](https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address)voor meer informatie over de verschillen tussen open bare ip's van de Basic-en Standard-SKU.

    * **Toewijzings methode voor openbaar IP-adres** en **time-out voor inactiviteit** : u kunt beide opties in de sjabloon wijzigen door de eigenschap **publicIPAllocationMethod** van **dynamisch** naar **statisch** of **statisch** naar dynamisch te veranderen. U kunt de time-out voor inactiviteit wijzigen door de eigenschap **idleTimeoutInMinutes** te wijzigen in de gewenste hoeveelheid.  De standaard waarde is **4**:

         ```json
         "resources": [
                {
                "type": "Microsoft.Network/publicIPAddresses",
                "apiVersion": "2019-06-01",
                "name": "[parameters('publicIPAddresses_myPubIP_name')]",
                "location": "<target-region>",
                  "sku": {
                  "name": "Basic",
                  "tier": "Regional"
                 },
                "properties": {
                "provisioningState": "Succeeded",
                "resourceGuid": "7549a8f1-80c2-481a-a073-018f5b0b69be",
                "ipAddress": "52.177.6.204",
                "publicIPAddressVersion": "IPv4",
                "publicIPAllocationMethod": "Dynamic",
                "idleTimeoutInMinutes": 4,
                "ipTags": []
                   }
                }            
         ```

        Zie [een openbaar IP-adres maken, wijzigen of verwijderen](https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address)voor meer informatie over de toewijzings methoden en de time-outwaarden voor inactiviteit.


9. Sla het bestand  **\<met de resource-group-name >. json** op.

10. Maak een resource groep in de doel regio voor het open bare doel-IP-adres dat moet worden geïmplementeerd met [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup?view=azps-2.6.0).
    
    ```azurepowershell-interactive
    New-AzResourceGroup -Name <target-resource-group-name> -location <target-region>
    ```
11. Implementeer het bewerkte  **\<resource-group-name >. json-** bestand in de resource groep die u in de vorige stap hebt gemaakt met behulp van [New-AzResourceGroupDeployment](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroupdeployment?view=azps-2.6.0):

    ```azurepowershell-interactive

    New-AzResourceGroupDeployment -ResourceGroupName <target-resource-group-name> -TemplateFile <source-resource-group-name>.json
    
    ```

12. Als u wilt controleren of de resources zijn gemaakt in de doel regio, gebruikt u [Get-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/get-azresourcegroup?view=azps-2.6.0) en [Get-AzPublicIPAddress](https://docs.microsoft.com/powershell/module/az.network/get-azpublicipaddress?view=azps-2.6.0):
    
    ```azurepowershell-interactive

    Get-AzResourceGroup -Name <target-resource-group-name>

    ```

    ```azurepowershell-interactive

    Get-AzPublicIPAddress -Name <target-publicip-name> -ResourceGroupName <target-resource-group-name>

    ```
## <a name="discard"></a>Verwijderen 

Als u na de implementatie wilt beginnen of het open bare IP-adres wilt negeren in het doel, verwijdert u de resource groep die is gemaakt in het doel en wordt het verplaatste open bare IP-adres verwijderd.  Gebruik [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup?view=azps-2.6.0)om de resource groep te verwijderen:

```azurepowershell-interactive

Remove-AzResourceGroup -Name <target-resource-group-name>

```

## <a name="clean-up"></a>Opruimen

Als u de wijzigingen wilt door voeren en de verplaatsing van het virtuele netwerk wilt volt ooien, verwijdert u het virtuele bron netwerk of de resource groep, gebruikt u [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup?view=azps-2.6.0) of [Remove-AzPublicIPAddress](https://docs.microsoft.com/powershell/module/az.network/remove-azpublicipaddress?view=azps-2.6.0):

```azurepowershell-interactive

Remove-AzResourceGroup -Name <source-resource-group-name>

```

``` azurepowershell-interactive

Remove-AzPublicIpAddress -Name <source-publicip-name> -ResourceGroupName <resource-group-name>

```

## <a name="next-steps"></a>Volgende stappen

In deze zelf studie hebt u een open bare Azure-IP van de ene regio naar de andere verplaatst en worden de bron bronnen opgeruimd.  Raadpleeg voor meer informatie over het verplaatsen van resources tussen regio's en herstel na nood gevallen in Azure:


- [Resources verplaatsen naar een nieuwe resourcegroep of een nieuw abonnement](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources)
- [Virtuele Azure-machines verplaatsen naar een andere regio](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-migrate)
