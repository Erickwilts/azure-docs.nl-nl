---
title: Een interne Load Balancer maken - Azure-sjabloon
titlesuffix: Azure Load Balancer
description: Meer informatie over hoe u met een sjabloon een interne load balancer maakt in Resource Manager
services: load-balancer
documentationcenter: na
author: KumudD
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: 339183dabf67abb43d6fc99bbe5258a39f12b14a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66122280"
---
# <a name="create-an-internal-load-balancer-using-a-template"></a>Een interne load balancer maken met behulp van een sjabloon

> [!div class="op_single_selector"]
> * [Azure Portal](../load-balancer/load-balancer-get-started-ilb-arm-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-ilb-arm-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-ilb-arm-cli.md)
> * [Sjabloon](../load-balancer/load-balancer-get-started-ilb-arm-template.md)

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="deploy-the-template-by-using-click-to-deploy"></a>De sjabloon implementeren met Klik om te implementeren

De voorbeeldsjabloon in de openbare opslagplaats maakt gebruik van een parameterbestand dat de standaardwaarden bevat voor het genereren van het hierboven beschreven scenario. Als u deze sjabloon wilt implementeren met behulp van Klik om te implementeren, volgt u [deze koppeling](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-internal-load-balancer). Klik op **Distribueren naar Azure**, vervang indien nodig de standaardparameterwaarden en volg de instructies in de portal.

## <a name="deploy-the-template-by-using-powershell"></a>De sjabloon implementeren met PowerShell

Volg onderstaande stappen als u de sjabloon die u hebt gedownload, wilt implementeren met PowerShell.

1. Als u Azure PowerShell nog niet eerder hebt gebruikt, kunt u [Azure PowerShell installeren en configureren](/powershell/azure/overview) raadplegen en de instructies helemaal tot aan het einde volgen om u aan te melden bij Azure en uw abonnement te selecteren.
2. Download het parameterbestand naar de lokale schijf.
3. Bewerk het bestand en sla het op.
4. Voer de **New-AzResourceGroupDeployment** cmdlet om een resourcegroep met behulp van de sjabloon te maken.

    ```azurepowershell-interactive
    New-AzResourceGroupDeployment -Name TestRG -Location westus `
        -TemplateFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-internal-load-balancer/azuredeploy.json' `
        -TemplateParameterFile 'C:\temp\azuredeploy.parameters.json'
    ```

## <a name="deploy-the-template-by-using-the-azure-cli"></a>De sjabloon implementeren met de Azure CLI

Volg onderstaande stappen als u de sjabloon wilt implementeren met de Azure CLI.

1. Als u Azure CLI nog nooit hebt gebruikt, raadpleegt u [De Azure CLI installeren en configureren](../cli-install-nodejs.md) en volgt u de instructies tot het punt waar u uw Azure-account en -abonnement moet selecteren.
2. Voer de opdracht **azure config mode** uit om over te schakelen naar de modus Resource Manager, zoals hieronder weergegeven.

    ```azurecli-interactive
    azure config mode arm
    ```

    Dit is de verwachte uitvoer voor de bovenstaande opdracht:

        info:    New mode is arm

3. Open het parameterbestand, selecteer de inhoud en sla deze op in een bestand op uw computer. Hier is het parameterbestand opgeslagen als *parameters.json*.
4. Voer de opdracht **azure group deployment create** uit om de nieuwe interne load balancer te implementeren met behulp van de sjabloon en de parameterbestanden die u hebt gedownload en hierboven hebt gewijzigd. De lijst die na de uitvoer wordt weergegeven, beschrijft de gebruikte parameters.

    ```azurecli
    azure group create --name TestRG --location westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-internal-load-balancer/azuredeploy.json --parameters-file parameters.json
    ```

## <a name="next-steps"></a>Volgende stappen

[Een distributiemodus voor de load balancer configureren met bron-IP-affiniteit](load-balancer-distribution-mode.md)

[TCP-time-outinstellingen voor inactiviteit voor de load balancer configureren](load-balancer-tcp-idle-timeout.md)

Zie voor de JSON-syntaxis en de eigenschappen van een load balancer in een sjabloon, [Microsoft.Network/loadBalancers](/azure/templates/microsoft.network/loadbalancers).