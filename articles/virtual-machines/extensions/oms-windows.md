---
title: Azure Monitor-virtuele machine-extensie voor Windows | Microsoft Docs
description: De Log Analytics-agent op Windows-machine met behulp van de extensie van een virtuele machine implementeren.
services: virtual-machines-windows
documentationcenter: ''
author: roiyz-msft
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: feae6176-2373-4034-b5d9-a32c6b4e1f10
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 04/29/2019
ms.author: roiyz
ms.openlocfilehash: fb931d5ce72b21cb17abbcd11095dbc8d611f0c9
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67064431"
---
# <a name="azure-monitor-virtual-machine-extension-for-windows"></a>Azure Monitor-virtuele machine-extensie voor Windows

Logboeken in Azure Monitor biedt mogelijkheden voor bewaking voor cloud en on-premises assets. De extensie van Log Analytics-agent virtuele machine voor Windows is gepubliceerd en ondersteund door Microsoft. De extensie voor de Log Analytics-agent geïnstalleerd op virtuele Azure-machines en virtuele machines voor een bestaande Log Analytics-werkruimte worden ingeschreven. In dit document worden de ondersteunde platforms, configuraties en implementatie-opties voor de extensie van de Azure Monitor-virtuele machine voor Windows.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites"></a>Vereisten

### <a name="operating-system"></a>Besturingssysteem

De Log Analytics-agent-extensie voor Windows ondersteunt de volgende versies van het Windows-besturingssysteem:

- Windows Server 2019
- Windows Server 2008 R2, 2012, 2012 R2, 2016, versie 1709 en 1803

### <a name="agent-and-vm-extension-version"></a>Versie agent en VM-extensie
De volgende tabel bevat een toewijzing van de versie van de Windows Azure Monitor VM-extensie en bundel van Log Analytics-agent voor elke versie. 

| Log Analytics-Windows-agent bundelversie krijgt | Versie van Azure Monitor Windows VM-uitbreiding | Releasedatum | Releaseopmerkingen |
|--------------------------------|--------------------------|--------------------------|--------------------------|
| 10.20.18001 | 1.0.18001 | Juni 2019 | <ul><li> Kleine correcties en stabilization-verbeteringen </li><li> De mogelijkheid om uit te schakelen van standaardreferenties bij het maken van een proxyverbinding (ondersteuning voor WINHTTP_AUTOLOGON_SECURITY_LEVEL_HIGH) is toegevoegd </li></ul>|
| 10.19.13515 | 1.0.13515 | Maart 2019 | <ul><li>Secundaire stabilization-oplossingen </li></ul> |
| 10.19.10006 | N.v.t. | December 2018 | <ul><li> Secundaire stabilization-oplossingen </li></ul> | 
| 8.0.11136 | N.v.t. | September 2018 |  <ul><li> Er is ondersteuning toegevoegd voor het detecteren van resource-ID wijzigen op de virtuele machine verplaatsen </li><li> Er is ondersteuning toegevoegd voor het melden van resource-ID bij het gebruik van niet-extensie installeren </li></ul>| 
| 8.0.11103 | N.v.t. |  April 2018 | |
| 8.0.11081 | 1.0.11081 | Nov 2017 | | 
| 8.0.11072 | 1.0.11072 | September 2017 | |
| 8.0.11049 | 1.0.11049 | Februari 2017 | |

### <a name="azure-security-center"></a>Azure Security Center

Azure Security Center wordt automatisch bepalingen van de Log Analytics-agent en verbindt u deze met de standaard Log Analytics-werkruimte van de Azure-abonnement. Als u van Azure Security Center gebruikmaakt, niet uitgevoerd door de stappen in dit document. In dat geval wordt de geconfigureerde werkruimte en het einde van de verbinding met Azure Security Center overschreven.

### <a name="internet-connectivity"></a>Internetconnectiviteit
De Log Analytics-agent-extensie voor Windows is vereist dat de virtuele doelmachine is verbonden met internet. 

## <a name="extension-schema"></a>Extensieschema

De volgende JSON ziet u het schema voor de Log Analytics-agent-extensie. De extensie is vereist voor de werkruimte-ID en werkruimtesleutel van de doel-Log Analytics-werkruimte. Deze kunnen worden gevonden in de instellingen voor de werkruimte in de Azure-portal. Omdat de sleutel van de werkruimte moet worden behandeld als gevoelige gegevens, moet deze worden opgeslagen in de instellingsconfiguratie van een beveiligde. Azure-VM-extensie beveiligde instellingsgegevens versleuteld en alleen op de virtuele doelmachine worden ontsleuteld. Houd er rekening mee dat **workspaceId** en **workspaceKey** zijn hoofdlettergevoelig.

```json
{
    "type": "extensions",
    "name": "OMSExtension",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.EnterpriseCloud.Monitoring",
        "type": "MicrosoftMonitoringAgent",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "workspaceId": "myWorkSpaceId"
        },
        "protectedSettings": {
            "workspaceKey": "myWorkspaceKey"
        }
    }
}
```
### <a name="property-values"></a>Waarden van eigenschappen

| Name | Waarde / voorbeeld |
| ---- | ---- |
| apiVersion | 2015-06-15 |
| publisher | Microsoft.EnterpriseCloud.Monitoring |
| type | MicrosoftMonitoringAgent |
| typeHandlerVersion | 1.0 |
| workspaceId (bijvoorbeeld)* | 6f680a37-00c6-41c7-a93f-1437e3462574 |
| workspaceKey (bijvoorbeeld) | z4bU3p1/GrnWpQkky4gdabWXAhbWSTz70hm4m2Xt92XI+rSRgE8qVvRhsGo9TXffbrTahyrwv35W0pOqQAU7uQ== |

\* De werkruimte-id heet de consumerId in de Log Analytics-API.

## <a name="template-deployment"></a>Sjabloonimplementatie

Azure VM-extensies kunnen worden geïmplementeerd met Azure Resource Manager-sjablonen. De JSON-schema in de vorige sectie beschreven kan worden gebruikt in een Azure Resource Manager-sjabloon om uit te voeren van de Log Analytics-agent-extensie tijdens de sjabloonimplementatie van een Azure Resource Manager. Een voorbeeldsjabloon met de Log Analytics-agent VM-extensie kunt u vinden op de [Azure Quick Start-galerie](https://github.com/Azure/azure-quickstart-templates/tree/master/201-oms-extension-windows-vm). 

>[!NOTE]
>De sjabloon biedt geen ondersteuning voor het opgeven van meer dan één werkruimte-ID en werkruimtesleutel als u wilt de agent configureren om te rapporteren aan meerdere werkruimten. Zie configureren van de agent om te rapporteren aan meerdere werkruimten [toevoegen of verwijderen van een werkruimte](../../azure-monitor/platform/agent-manage.md#adding-or-removing-a-workspace).  

De JSON voor de extensie van een virtuele machine kan worden genest in de bron van de virtuele machine of geplaatst op de hoofdmap of het hoogste niveau van een Resource Manager JSON-sjabloon. De plaatsing van de JSON is van invloed op de waarde van de resourcenaam en het type. Zie voor meer informatie, [naam en type voor de onderliggende resources instellen](../../azure-resource-manager/resource-group-authoring-templates.md#child-resources). 

Het volgende voorbeeld wordt ervan uitgegaan dat de extensie Azure Monitor is genest in de bron van de virtuele machine. Wanneer het nesten van de extensie-resource, de JSON wordt geplaatst in de `"resources": []` object van de virtuele machine.


```json
{
    "type": "extensions",
    "name": "OMSExtension",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.EnterpriseCloud.Monitoring",
        "type": "MicrosoftMonitoringAgent",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "workspaceId": "myWorkSpaceId"
        },
        "protectedSettings": {
            "workspaceKey": "myWorkspaceKey"
        }
    }
}
```

Bij het plaatsen van de JSON-extensie in de hoofdmap van de sjabloon, naam van de resource bevat een verwijzing naar de bovenliggende virtuele machine en het type weerspiegelt de geneste configuratie. 

```json
{
    "type": "Microsoft.Compute/virtualMachines/extensions",
    "name": "<parentVmResource>/OMSExtension",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.EnterpriseCloud.Monitoring",
        "type": "MicrosoftMonitoringAgent",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "workspaceId": "myWorkSpaceId"
        },
        "protectedSettings": {
            "workspaceKey": "myWorkspaceKey"
        }
    }
}
```

## <a name="powershell-deployment"></a>PowerShell-implementatie

De `Set-AzVMExtension` opdracht kan worden gebruikt voor het implementeren van de virtuele machine-extensie van Log Analytics-agent op een bestaande virtuele machine. Voordat u de opdracht uitvoert, moeten de openbare en privé-configuraties worden opgeslagen in een PowerShell-hash-tabel. 

```powershell
$PublicSettings = @{"workspaceId" = "myWorkspaceId"}
$ProtectedSettings = @{"workspaceKey" = "myWorkspaceKey"}

Set-AzVMExtension -ExtensionName "Microsoft.EnterpriseCloud.Monitoring" `
    -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" `
    -Publisher "Microsoft.EnterpriseCloud.Monitoring" `
    -ExtensionType "MicrosoftMonitoringAgent" `
    -TypeHandlerVersion 1.0 `
    -Settings $PublicSettings `
    -ProtectedSettings $ProtectedSettings `
    -Location WestUS 
```

## <a name="troubleshoot-and-support"></a>Problemen oplossen en ondersteuning

### <a name="troubleshoot"></a>Problemen oplossen

Gegevens over de status van extensie-implementaties kunnen worden opgehaald uit de Azure-portal en met behulp van de Azure PowerShell-module. Als wilt zien de implementatiestatus van extensies voor een bepaalde virtuele machine, voert u de volgende opdracht uit met behulp van de Azure PowerShell-module.

```powershell
Get-AzVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

Extensie uitvoering uitvoer wordt vastgelegd op bestanden die zijn gevonden in de volgende map:

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\
```

### <a name="support"></a>Ondersteuning

Als u hulp nodig hebt op elk gewenst moment in dit artikel, u kunt contact opnemen met de Azure-experts op het [forums voor Azure MSDN en Stack Overflow](https://azure.microsoft.com/support/forums/). U kunt ook een Azure-ondersteuning-incident indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer Get-ondersteuning. Voor meer informatie over het gebruik van ondersteuning voor Azure, de [Veelgestelde vragen over Microsoft Azure-ondersteuning](https://azure.microsoft.com/support/faq/).
