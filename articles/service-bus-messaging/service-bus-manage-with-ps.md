---
title: PowerShell gebruiken voor het beheren van Azure Service Bus-bronnen | Microsoft Docs
description: PowerShell-module gebruiken om te maken en beheren van Service Bus-bronnen
services: service-bus-messaging
documentationcenter: .NET
author: axisc
manager: timlt
editor: spelluru
ms.assetid: ''
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/21/2018
ms.author: aschhab
ms.openlocfilehash: 0d15aa4d7b8a922f7606b7c4d1b357a80b3cbfab
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60311043"
---
# <a name="use-powershell-to-manage-service-bus-resources"></a>PowerShell gebruiken voor het beheren van Service Bus-bronnen

Microsoft Azure PowerShell is een scriptomgeving die u gebruiken kunt om te beheren en automatiseren van de implementatie en het beheer van Azure-services. In dit artikel wordt beschreven hoe u de [Service Bus-Resource Manager PowerShell-module](/powershell/module/az.servicebus) kunt inrichten en beheren van Service Bus-entiteiten (naamruimten, wachtrijen, onderwerpen en abonnementen) met behulp van een lokale Azure PowerShell-console of het script.

U kunt ook Service Bus-entiteiten met behulp van Azure Resource Manager-sjablonen beheren. Zie voor meer informatie het artikel [maken van Service Bus-resources met behulp van Azure Resource Manager-sjablonen](service-bus-resource-manager-overview.md).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Vereisten

Voordat u begint, moet u de volgende vereisten:

* Een Azure-abonnement. Zie voor meer informatie over het verkrijgen van een abonnement [Aankoopopties][purchase options], [aanbiedingen voor leden][member offers], of [gratis account][free account].
* Een computer met Azure PowerShell. Zie voor instructies [aan de slag met Azure PowerShell-cmdlets](/powershell/azure/get-started-azureps).
* Een algemeen begrip van de PowerShell-scripts, NuGet-pakketten en .NET Framework.

## <a name="get-started"></a>Aan de slag

De eerste stap is PowerShell gebruiken om aan te melden bij uw Azure-account en Azure-abonnement. Volg de instructies in [aan de slag met Azure PowerShell-cmdlets](/powershell/azure/get-started-azureps) Meld u aan bij uw Azure-account en ophalen en toegang tot de resources in uw Azure-abonnement.

## <a name="provision-a-service-bus-namespace"></a>Inrichten van een Service Bus-naamruimte

Als u werkt met Service Bus-naamruimten, kunt u de [Get-AzServiceBusNamespace](/powershell/module/az.servicebus/get-azservicebusnamespace), [New-AzServiceBusNamespace](/powershell/module/az.servicebus/new-azservicebusnamespace), [Remove-AzServiceBusNamespace](/powershell/module/az.servicebus/remove-azservicebusnamespace), en [ Set-AzServiceBusNamespace](/powershell/module/az.servicebus/set-azservicebusnamespace) cmdlets.

Dit voorbeeld maakt u een paar lokale variabelen in het script; `$Namespace` en `$Location`.

* `$Namespace` is de naam van de Service Bus-naamruimte die we willen werken.
* `$Location` Hiermee geeft u het datacenter waarin we de naamruimte inrichten.
* `$CurrentNamespace` slaat de referentie-naamruimte die we ophalen (of maak).

In een werkelijke script `$Namespace` en `$Location` kunnen worden doorgegeven als parameters.

Dit deel van het script doet het volgende:

1. Probeert op te halen van een Service Bus-naamruimte met de opgegeven naam.
2. Als de naamruimte wordt gevonden, rapporteert deze wat is gevonden.
3. Als de naamruimte niet wordt gevonden, maakt de naamruimte en vervolgens wordt de zojuist gemaakte naamruimte opgehaald.
   
    ``` powershell
    # Query to see if the namespace currently exists
    $CurrentNamespace = Get-AzServiceBusNamespace -ResourceGroup $ResGrpName -NamespaceName $Namespace
   
    # Check if the namespace already exists or needs to be created
    if ($CurrentNamespace)
    {
        Write-Host "The namespace $Namespace already exists in the $Location region:"
        # Report what was found
        Get-AzServiceBusNamespace -ResourceGroup $ResGrpName -NamespaceName $Namespace
    }
    else
    {
        Write-Host "The $Namespace namespace does not exist."
        Write-Host "Creating the $Namespace namespace in the $Location region..."
        New-AzServiceBusNamespace -ResourceGroup $ResGrpName -NamespaceName $Namespace -Location $Location
        $CurrentNamespace = Get-AzServiceBusNamespace -ResourceGroup $ResGrpName -NamespaceName $Namespace
        Write-Host "The $Namespace namespace in Resource Group $ResGrpName in the $Location region has been successfully created."
                
    }
    ```

### <a name="create-a-namespace-authorization-rule"></a>Een verificatieregel voor een naamruimte maken

Het volgende voorbeeld laat zien hoe voor het beheren van verificatieregels voor een naamruimte met behulp van de [New-AzServiceBusAuthorizationRule](/powershell/module/az.servicebus/new-azservicebusauthorizationrule), [Get-AzServiceBusAuthorizationRule](/powershell/module/az.servicebus/get-azservicebusauthorizationrule), [ Set-AzServiceBusAuthorizationRule](/powershell/module/az.servicebus/set-azservicebusauthorizationrule), en [Remove-AzServiceBusAuthorizationRule](/powershell/module/az.servicebus/remove-azservicebusauthorizationrule) cmdlets.

```powershell
# Query to see if rule exists
$CurrentRule = Get-AzServiceBusAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule

# Check if the rule already exists or needs to be created
if ($CurrentRule)
{
    Write-Host "The $AuthRule rule already exists for the namespace $Namespace."
}
else
{
    Write-Host "The $AuthRule rule does not exist."
    Write-Host "Creating the $AuthRule rule for the $Namespace namespace..."
    New-AzServiceBusAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule -Rights @("Listen","Send")
    $CurrentRule = Get-AzServiceBusAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule
    Write-Host "The $AuthRule rule for the $Namespace namespace has been successfully created."

    Write-Host "Setting rights on the namespace"
    $authRuleObj = Get-AzServiceBusAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule

    Write-Host "Remove Send rights"
    $authRuleObj.Rights.Remove("Send")
    Set-AzServiceBusAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthRuleObj $authRuleObj

    Write-Host "Add Send and Manage rights to the namespace"
    $authRuleObj.Rights.Add("Send")
    Set-AzServiceBusAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthRuleObj $authRuleObj
    $authRuleObj.Rights.Add("Manage")
    Set-AzServiceBusAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthRuleObj $authRuleObj

    Write-Host "Show value of primary key"
    $CurrentKey = Get-AzServiceBusKey -ResourceGroup $ResGrpName -NamespaceName $Namespace -Name $AuthRule
        
    Write-Host "Remove this authorization rule"
    Remove-AzServiceBusAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -Name $AuthRule
}
```

## <a name="create-a-queue"></a>Een wachtrij maken

Voor het maken van een wachtrij of onderwerp, voert u een naamruimte-uit met behulp van het script in de vorige sectie. Maak vervolgens de wachtrij:

```powershell
# Check if queue already exists
$CurrentQ = Get-AzServiceBusQueue -ResourceGroup $ResGrpName -NamespaceName $Namespace -QueueName $QueueName

if($CurrentQ)
{
    Write-Host "The queue $QueueName already exists in the $Location region:"
}
else
{
    Write-Host "The $QueueName queue does not exist."
    Write-Host "Creating the $QueueName queue in the $Location region..."
    New-AzServiceBusQueue -ResourceGroup $ResGrpName -NamespaceName $Namespace -QueueName $QueueName -EnablePartitioning $True
    $CurrentQ = Get-AzServiceBusQueue -ResourceGroup $ResGrpName -NamespaceName $Namespace -QueueName $QueueName
    Write-Host "The $QueueName queue in Resource Group $ResGrpName in the $Location region has been successfully created."
}
```

### <a name="modify-queue-properties"></a>Wachtrijeigenschappen van de wijzigen

Nadat het script is uitgevoerd in de vorige sectie, kunt u de [Set AzServiceBusQueue](/powershell/module/az.servicebus/set-azservicebusqueue) cmdlet voor het bijwerken van de eigenschappen van een wachtrij, zoals in het volgende voorbeeld:

```powershell
$CurrentQ.DeadLetteringOnMessageExpiration = $True
$CurrentQ.MaxDeliveryCount = 7
$CurrentQ.MaxSizeInMegabytes = 2048
$CurrentQ.EnableExpress = $True

Set-AzServiceBusQueue -ResourceGroup $ResGrpName -NamespaceName $Namespace -QueueName $QueueName -QueueObj $CurrentQ
```

## <a name="provisioning-other-service-bus-entities"></a>Inrichting van andere Service Bus-entiteiten

U kunt de [Service Bus-PowerShell-module](/powershell/module/az.servicebus) voor het inrichten van andere entiteiten, zoals onderwerpen en abonnementen. Deze cmdlets zijn syntactisch vergelijkbaar met de wachtrij maken-cmdlets in de vorige sectie wordt gedemonstreerd.

## <a name="next-steps"></a>Volgende stappen

- Zie de volledige documentatie van de Service Bus-Resource Manager PowerShell-module [hier](/powershell/module/az.servicebus). Deze pagina geeft een lijst van alle beschikbare cmdlets.
- Zie het artikel voor informatie over het gebruik van Azure Resource Manager-sjablonen, [maken van Service Bus-resources met behulp van Azure Resource Manager-sjablonen](service-bus-resource-manager-overview.md).
- Informatie over [Service Bus .NET-beheerbibliotheken](service-bus-management-libraries.md).

Er zijn enkele alternatieve methoden voor het beheren van Service Bus-entiteiten, zoals beschreven in de volgende blogberichten:

* [Over het maken van Service Bus-wachtrijen, onderwerpen en abonnementen die gebruikmaken van een PowerShell-script](https://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
* [Over het maken van een Service Bus-Namespace en een Event Hub met behulp van een PowerShell-script](https://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)
* [Service Bus PowerShell-Scripts](https://code.msdn.microsoft.com/Service-Bus-PowerShell-a46b7059)

<!--Anchors-->

[purchase options]: https://azure.microsoft.com/pricing/purchase-options/
[member offers]: https://azure.microsoft.com/pricing/member-offers/
[free account]: https://azure.microsoft.com/pricing/free-trial/
