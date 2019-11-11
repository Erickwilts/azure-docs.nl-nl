---
title: 'Snelstartgids: inrichten van apparaten instellen met behulp van een Azure Resource Manager sjabloon'
description: Azure-snelstart - Azure IoT Hub Device Provisioning-service instellen met een sjabloon
author: wesmc7777
ms.author: wesmc
ms.date: 11/08/2019
ms.topic: quickstart
ms.service: iot-dps
services: iot-dps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: fdc75424c5c99e80c13ac086229da93411e3ce83
ms.sourcegitcommit: bc193bc4df4b85d3f05538b5e7274df2138a4574
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/10/2019
ms.locfileid: "73903369"
---
# <a name="quickstart-set-up-the-iot-hub-device-provisioning-service-with-an-azure-resource-manager-template"></a>Snelstartgids: de IoT Hub Device Provisioning Service instellen met een Azure Resource Manager sjabloon

U kunt [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) gebruiken voor het programmatisch instellen van de benodigde Azure-cloudresources voor het inrichten van uw apparaten. Deze stappen laten zien hoe u een IoT-hub en een nieuwe IoT Hub Device Provisioning-service maakt en deze twee services aan elkaar koppelt met een Azure Resource Manager-sjabloon. Deze snelstart maakt gebruik van [Azure CLI](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy-cli) voor het uitvoeren van de benodigde programmatische stappen voor het maken van een resourcegroep en het implementeren van de sjabloon, maar u kunt ook gewoon [Azure Portal](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy-portal), [PowerShell ](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy), .NET, ruby of andere programmeertalen gebruiken om deze stappen uit te voeren en de sjabloon te implementeren. 


## <a name="prerequisites"></a>Vereisten

- Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.
- Voor deze snelstart moet u Azure CLI lokaal uitvoeren. Azure CLI versie 2.0 of hoger moet geïnstalleerd zijn. Voer `az --version` uit om de versie te bekijken. Als u de CLI wilt installeren of upgraden, raadpleegt u [Azure CLI installeren](https://docs.microsoft.com/cli/azure/install-azure-cli).


## <a name="sign-in-to-azure-and-create-a-resource-group"></a>Aanmelden bij Azure en een resourcegroep maken

Meld u aan bij uw Azure-account en selecteer uw abonnement.

1. Voer de volgende [opdracht][lnk-login-command]uit vanaf de opdracht prompt:
    
    ```azurecli
    az login
    ```

    Volg de instructies om te verifiëren met de code en meld u aan bij uw Azure-account via een webbrowser.

2. Als u meerdere Azure-abonnementen hebt en u zich aanmeldt bij Azure, hebt u toegang tot alle Azure accounts die zijn gekoppeld aan uw referenties. Gebruik de volgende [opdracht om de Azure-accounts weer te geven][lnk-az-account-command] die u kunt gebruiken:
    
    ```azurecli
    az account list 
    ```

    Gebruik de volgende opdracht om het abonnement te selecteren dat u wilt gebruiken voor het uitvoeren van de opdrachten voor het maken van uw IoT-hub. U kunt de naam van het abonnement of de id van de uitvoer van de vorige opdracht gebruiken:

    ```azurecli
    az account set --subscription {your subscription name or id}
    ```

3. Wanneer u Azure-cloudresources zoals IoT-hubs maakt en services inricht, groepeert u deze in een resourcegroep. Gebruik een bestaande resource groep of voer de volgende opdracht uit [om een resource groep te maken][lnk-az-resource-command]:
    
    ```azurecli
     az group create --name {your resource group name} --location westus
    ```

    > [!TIP]
    > In het voorbeeld wordt de resourcegroep gemaakt in de locatie US - west. U kunt een lijst met beschikbare locaties weergeven met de opdracht `az account list-locations -o table`.
    >
    >

## <a name="create-a-resource-manager-template"></a>Een Resource Manager-sjabloon maken

Gebruik een JSON-sjabloon voor het maken van een inrichtingsservice en een gekoppelde IoT-hub in de resourcegroep. U kunt ook een Azure Resource Manager-sjabloon gebruiken om een bestaande inrichtingsservice of IoT-hub te wijzigen.

1. Gebruik een teksteditor voor het maken van een Azure Resource Manager-sjabloon met de naam **template.json** met de volgende minimale inhoud. 

   ```json
   {
       "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "1.0.0.0",
       "parameters": {},
       "variables": {},
       "resources": []
   }
   ```

2. Vervang de sectie **parameters** door de volgende inhoud. De sectie parameters bevat de parameters die kunnen worden doorgegeven vanuit een ander bestand. In deze sectie wordt de naam van de IoT-hub en de inrichtingsservice die u wilt maken opgegeven. Ook de locatie voor de IoT-hub en de locatie voor de inrichtingsservice worden hierin opgegeven. De waarden zijn beperkt tot Azure-regio's die ondersteuning bieden voor IoT-hubs en inrichtingsservices. Voer de opdracht `az provider show --namespace Microsoft.Devices --query "resourceTypes[?resourceType=='ProvisioningServices'].locations | [0]" --out table` uit voor een lijst van ondersteunde locaties voor Device Provisioning Service, of ga naar de pagina [Azure Status](https://azure.microsoft.com/status/) en zoek naar 'Device Provisioning Service'.

   ```json
    "parameters": {
        "iotHubName": {
            "type": "string"
        },
        "provisioningServiceName": {
            "type": "string"
        },
        "hubLocation": {
            "type": "string",
            "allowedValues": [
                "eastus",
                "westus",
                "westeurope",
                "northeurope",
                "southeastasia",
                "eastasia"
            ]
        }
    },

   ```

3. Vervang de sectie **variables** (variabelen) door de volgende inhoud. In deze sectie definieert u waarden die later worden gebruikt voor het opbouwen van de verbindingsreeks voor de IoT-hub, die nodig is om de inrichtingsservice aan de IoT-hub te koppelen. 
 
   ```json
    "variables": {        
        "iotHubResourceId": "[resourceId('Microsoft.Devices/Iothubs', parameters('iotHubName'))]",
        "iotHubKeyName": "iothubowner",
        "iotHubKeyResource": "[resourceId('Microsoft.Devices/Iothubs/Iothubkeys', parameters('iotHubName'), variables('iotHubKeyName'))]"
    },

   ```

4. Als u een IoT-hub wilt maken, voegt u de volgende regels toe aan de verzameling **resources**. De JSON bevat de minimale vereiste eigenschappen voor het maken van een IoT-hub. De eigenschappen **name** (naam) en **location** (locatie) worden als parameters doorgegeven. Zie [Microsoft.Devices/IotHubs template reference](https://docs.microsoft.com/azure/templates/microsoft.devices/iothubs) voor meer informatie over de eigenschappen die u voor een IoT-hub kunt opgeven in een sjabloon.

   ```json
        {
            "apiVersion": "2017-07-01",
            "type": "Microsoft.Devices/IotHubs",
            "name": "[parameters('iotHubName')]",
            "location": "[parameters('hubLocation')]",
            "sku": {
                "name": "S1",
                "capacity": 1
            },
            "tags": {
            },
            "properties": {
            }            
        },

   ``` 

5. Voor het maken van de inrichtingsservice voegt u de volgende regels toe na de specificatie van de IoT-hub in de verzameling **resources**. De **naam** en **locatie** van de inrichtingsservice worden doorgegeven in parameters. Geef de IoT-hubs waaraan u de inrichtingsservice wilt koppelen op in de verzameling **iotHubs**. U moet ten minste de eigenschappen **connectionString** en **location** opgeven voor elke gekoppelde IoT-hub. U kunt ook eigenschappen zoals **allocationWeight** en **applyAllocationPolicy** instellen voor elke IoT-hub, evenals de eigenschappen **allocationPolicy** en  **authorizationPolicies** voor de inrichtingsservice zelf. Zie [Microsoft.Devices/provisioningServices template reference](https://docs.microsoft.com/azure/templates/microsoft.devices/provisioningservices) voor meer informatie.

   De eigenschap **dependsOn** wordt gebruikt om ervoor te zorgen dat Resource Manager de IoT-hub vóór de inrichtingsservice maakt. Voor de sjabloon is de verbindingsreeks van de IoT-hub vereist voordat de koppeling met de inrichtingsservice kan worden opgegeven, zodat de hub en de sleutels eerst moeten worden gemaakt. De sjabloon maakt gebruik van functies zoals **concat** en **listKeys** voor het maken van de verbindingsreeks. Zie [Functies van Azure Resource Manager-sjablonen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-functions) voor meer informatie.

   ```json
        {
            "type": "Microsoft.Devices/provisioningServices",
            "sku": {
                "name": "S1",
                "capacity": 1
            },
            "name": "[parameters('provisioningServiceName')]",
            "apiVersion": "2017-11-15",
            "location": "[parameters('hubLocation')]",
            "tags": {},
            "properties": {
                "iotHubs": [
                    {
                        "connectionString": "[concat('HostName=', reference(variables('iotHubResourceId')).hostName, ';SharedAccessKeyName=', variables('iotHubKeyName'), ';SharedAccessKey=', listkeys(variables('iotHubKeyResource'), '2017-07-01').primaryKey)]",
                        "location": "[parameters('hubLocation')]",
                        "name": "[concat(parameters('iotHubName'),'.azure-devices.net')]"
                    }
                ]
            },
            "dependsOn": ["[parameters('iotHubName')]"]
        }

   ```

6. Sla het sjabloonbestand op. De voltooide sjabloon moet er als volgt uitzien:

   ```json
   {
       "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "1.0.0.0",
       "parameters": {
           "iotHubName": {
               "type": "string"
           },
           "provisioningServiceName": {
               "type": "string"
           },
           "hubLocation": {
               "type": "string",
               "allowedValues": [
                   "eastus",
                   "westus",
                   "westeurope",
                   "northeurope",
                   "southeastasia",
                   "eastasia"
               ]
           }
       },
       "variables": {        
           "iotHubResourceId": "[resourceId('Microsoft.Devices/Iothubs', parameters('iotHubName'))]",
           "iotHubKeyName": "iothubowner",
           "iotHubKeyResource": "[resourceId('Microsoft.Devices/Iothubs/Iothubkeys', parameters('iotHubName'), variables('iotHubKeyName'))]"
       },
       "resources": [
           {
               "apiVersion": "2017-07-01",
               "type": "Microsoft.Devices/IotHubs",
               "name": "[parameters('iotHubName')]",
               "location": "[parameters('hubLocation')]",
               "sku": {
                   "name": "S1",
                   "capacity": 1
               },
               "tags": {
               },
               "properties": {
               }            
           },
           {
               "type": "Microsoft.Devices/provisioningServices",
               "sku": {
                   "name": "S1",
                   "capacity": 1
               },
               "name": "[parameters('provisioningServiceName')]",
               "apiVersion": "2017-11-15",
               "location": "[parameters('hubLocation')]",
               "tags": {},
               "properties": {
                   "iotHubs": [
                       {
                           "connectionString": "[concat('HostName=', reference(variables('iotHubResourceId')).hostName, ';SharedAccessKeyName=', variables('iotHubKeyName'), ';SharedAccessKey=', listkeys(variables('iotHubKeyResource'), '2017-07-01').primaryKey)]",
                           "location": "[parameters('hubLocation')]",
                           "name": "[concat(parameters('iotHubName'),'.azure-devices.net')]"
                       }
                   ]
               },
               "dependsOn": ["[parameters('iotHubName')]"]
           }
       ]
   }
   ```

## <a name="create-a-resource-manager-parameter-file"></a>Een Resource Manager-parameterbestand maken

De sjabloon die u hebt gedefinieerd in de laatste stap maakt gebruik van parameters voor de naam van de IoT-hub, de naam van de inrichtingsservice en de locatie (Azure-regio) om ze te maken. U geeft deze parameters door in een afzonderlijk bestand. Zo kunt u dezelfde sjabloon voor meerdere implementaties gebruiken. Volg deze stappen voor het maken van het parameterbestand:

1. Gebruik een teksteditor voor het maken van een Azure Resource Manager-parameterbestand met de naam **parameters.json** met de volgende minimale inhoud: 

   ```json
   {
       "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
       "contentVersion": "1.0.0.0",
       "parameters": {}
       }
   }
   ```

2. Voeg de waarde voor **iotHubName** toe aan de sectie parameters. Als u de naam wijzigt, moet u de juiste naamconventies voor een IoT-hub volgen. De naam moet 3 tot 50 tekens lang zijn en mag alleen bestaan uit hoofdletters, kleine letters, alfanumerieke tekens en afbreekstreepjes ('-'). 

   ```json
    "parameters": {
        "iotHubName": {
            "value": "my-sample-iot-hub"
        },
    }
   
   ```

3. Voeg de waarde voor **provisioningServiceName** toe aan de sectie parameters. Als u de naam wijzigt, moet u de juiste naamconventies voor een IoT Hub Device Provisioning-service volgen. De naam moet 3 tot 64 tekens lang zijn en mag alleen bestaan uit hoofdletters, kleine letters, alfanumerieke tekens en afbreekstreepjes ('-').

   ```json
    "parameters": {
        "iotHubName": {
            "value": "my-sample-iot-hub"
        },
        "provisioningServiceName": {
            "value": "my-sample-provisioning-service"
        },
    }

   ```

4. Voeg de waarde voor **hubLocation** toe aan de sectie parameters. Hiermee wordt zowel de locatie voor de IoT-hub als de locatie voor de inrichtingsservice opgegeven. De waarde moet overeenkomen met een van de opgegeven locaties in de verzameling **allowedValues** in de parameterdefinitie in het sjabloonbestand. Deze verzameling beperkt de waarden tot Azure-locaties die ondersteuning bieden voor zowel IoT-hubs als inrichtingsservices. Voer de opdracht `az provider show --namespace Microsoft.Devices --query "resourceTypes[?resourceType=='ProvisioningServices'].locations | [0]" --out table` uit voor een lijst van ondersteunde locaties voor Device Provisioning Service, of ga naar de pagina [Azure Status](https://azure.microsoft.com/status/) en zoek naar 'Device Provisioning Service'.

   ```json
    "parameters": {
        "iotHubName": {
            "value": "my-sample-iot-hub"
        },
        "provisioningServiceName": {
            "value": "my-sample-provisioning-service"
        },
        "hubLocation": {
            "value": "westus"
        }
    }

   ```

5. Sla het bestand op. 


> [!IMPORTANT]
> De IoT-hub en de inrichtingsservice kunnen openbaar worden gedetecteerd als DNS-eindpunten. Zorg er dus voor dat wordt voorkomen dat gevoelige informatie in de namen wordt vermeld.
>

## <a name="deploy-the-template"></a>De sjabloon implementeren

Met de volgende Azure CLI-opdrachten kunt u de sjablonen implementeren en de implementatie controleren.

1. Voer de volgende [opdracht voor het starten van een implementatie](https://docs.microsoft.com/cli/azure/group/deployment?view=azure-cli-latest#az-group-deployment-create) uit om uw sjabloon te implementeren:
    
    ```azurecli
     az group deployment create -g {your resource group name} --template-file template.json --parameters @parameters.json
    ```

   Zoek de eigenschap **provisioningState** die is ingesteld op Succeeded (Geslaagd) in de uitvoer. 

   ![Uitvoer van inrichting](./media/quick-setup-auto-provision-rm/output.png) 


2. Voer de volgende [opdracht voor het weergeven van resources](https://docs.microsoft.com/cli/azure/resource?view=azure-cli-latest#az-resource-list) uit en zoek de nieuwe inrichtingsservice en IoT-hub in de uitvoer om de implementatie te controleren:

    ```azurecli
     az resource list -g {your resource group name}
    ```


## <a name="clean-up-resources"></a>Resources opschonen

Andere Quick Starts in deze verzameling zijn op deze Quick Start gebaseerd. Als u van plan bent om door te gaan met andere snelstartgidsen of met de zelfstudies, verwijdert u de resources die u in deze snelstartgids hebt gemaakt niet. Als u niet wilt door gaan, kunt u de Azure CLI gebruiken om [een afzonderlijke resource te verwijderen][lnk-az-resource-command], zoals een IOT-hub of een inrichtings service, of om een resource groep en alle bijbehorende resources te verwijderen.

Voer de volgende opdracht uit voor het verwijderen van een inrichtingsservice:

```azurecli
az iot hub delete --name {your provisioning service name} --resource-group {your resource group name}
```
Voer de volgende opdracht uit voor het verwijderen van een IoT-hub:

```azurecli
az iot hub delete --name {your iot hub name} --resource-group {your resource group name}
```

Voer de volgende opdracht uit voor het verwijderen van een resourcegroep en alle bijbehorende resources:

```azurecli
az group delete --name {your resource group name}
```

U kunt ook resourcegroepen en afzonderlijke resources verwijderen met behulp van Azure Portal, PowerShell of REST-API's of ondersteunde platform-SDK's die zijn gepubliceerd voor Azure Resource Manager of de IoT Hub Device Provisioning-service.

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u een IoT-hub en een Device Provisioning Service-exemplaar geïmplementeerd en de twee resources gekoppeld. Als u wilt weten hoe u deze instellingen gebruikt voor het inrichten van een gesimuleerd apparaat, gaat u verder met de rest van de snelstartgids voor het maken van een gesimuleerd apparaat.

> [!div class="nextstepaction"]
> [Snelstartgids voor het maken van een gesimuleerd apparaat](./quick-create-simulated-device.md)


<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-CLI-install]: https://docs.microsoft.com/cli/azure/install-az-cli2
[lnk-login-command]: https://docs.microsoft.com/cli/azure/get-started-with-az-cli2
[lnk-az-account-command]: https://docs.microsoft.com/cli/azure/account
[lnk-az-register-command]: https://docs.microsoft.com/cli/azure/provider
[lnk-az-addcomponent-command]: https://docs.microsoft.com/cli/azure/component
[lnk-az-resource-command]: https://docs.microsoft.com/cli/azure/resource
[lnk-az-iot-command]: https://docs.microsoft.com/cli/azure/iot
[lnk-iot-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-devguide]: iot-hub-devguide.md
[lnk-portal]: iot-hub-create-through-portal.md 
