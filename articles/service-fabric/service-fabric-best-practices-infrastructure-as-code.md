---
title: Azure Service Fabric-infra structuur als best practices voor code | Microsoft Docs
description: Aanbevolen procedures voor het beheren van Service Fabric als een infra structuur als code.
services: service-fabric
documentationcenter: .net
author: peterpogorski
manager: chackdan
editor: ''
ms.assetid: 19ca51e8-69b9-4952-b4b5-4bf04cded217
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/23/2019
ms.author: pepogors
ms.openlocfilehash: ae1cd0912733116dce1b550dd937cc9fc5f8737b
ms.sourcegitcommit: 4b647be06d677151eb9db7dccc2bd7a8379e5871
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2019
ms.locfileid: "68359767"
---
# <a name="infrastructure-as-code"></a>Infrastructure als code

Maak in een productie scenario Azure Service Fabric-clusters met behulp van Resource Manager-sjablonen. Resource Manager-sjablonen bieden meer controle over de bron eigenschappen en zorgen ervoor dat u een consistent resource model hebt.

Voorbeeld sjablonen voor Resource Manager zijn beschikbaar voor Windows en Linux in de [Azure-voor beelden op github](https://github.com/Azure-Samples/service-fabric-cluster-templates). Deze sjablonen kunnen worden gebruikt als uitgangs punt voor uw cluster sjabloon. Down `azuredeploy.json` load `azuredeploy.parameters.json` en bewerk ze om te voldoen aan uw aangepaste vereisten.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Gebruik de volgende `azuredeploy.json` Azure `azuredeploy.parameters.json` cli-opdrachten om de en sjablonen te implementeren die u hierboven hebt gedownload:

```azurecli
ResourceGroupName="sfclustergroup"
Location="westus"

az group create --name $ResourceGroupName --location $Location 
az group deployment create --name $ResourceGroupName  --template-file azuredeploy.json --parameters @azuredeploy.parameters.json
```

Een resource maken met behulp van Power shell

```powershell
$ResourceGroupName="sfclustergroup"
$Location="westus"
$Template="azuredeploy.json"
$Parameters="azuredeploy.parameters.json"

New-AzResourceGroup -Name $ResourceGroupName -Location $Location
New-AzResourceGroupDeployment -Name $ResourceGroupName -TemplateFile $Template -TemplateParameterFile $Parameters
```

## <a name="azure-service-fabric-resources"></a>Azure Service Fabric-resources

U kunt toepassingen en services implementeren in uw Service Fabric-cluster via Azure Resource Manager. Zie [toepassingen en services beheren als Azure Resource Manager resources](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-arm-resource) voor meer informatie. Hieronder vindt u best practice Service Fabric toepassingsspecifieke resources die moeten worden meegenomen in de resources van de Resource Manager-sjabloon.

```json
{
    "apiVersion": "2017-07-01-preview",
    "type": "Microsoft.ServiceFabric/clusters/applicationTypes",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationTypeName'))]",
    "location": "[variables('clusterLocation')]",
},
{
    "apiVersion": "2017-07-01-preview",
    "type": "Microsoft.ServiceFabric/clusters/applicationTypes/versions",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationTypeName'), '/', parameters('applicationTypeVersion'))]",
    "location": "[variables('clusterLocation')]",
},
{
    "apiVersion": "2017-07-01-preview",
    "type": "Microsoft.ServiceFabric/clusters/applications",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'))]",
    "location": "[variables('clusterLocation')]",
},
{
    "apiVersion": "2017-07-01-preview",
    "type": "Microsoft.ServiceFabric/clusters/applications/services",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'), '/', parameters('serviceName'))]",
    "location": "[variables('clusterLocation')]"
}
```

Als u uw toepassing wilt implementeren met behulp van Azure Resource Manager, moet u eerst [een sfpkg](https://docs.microsoft.com/azure/service-fabric/service-fabric-package-apps#create-an-sfpkg) service Fabric toepassings pakket maken. Het volgende python-script is een voor beeld van het maken van een sfpkg:

```python
# Create SFPKG that needs to be uploaded to Azure Storage Blob Container
microservices_sfpkg = zipfile.ZipFile(
    self.microservices_app_package_name, 'w', zipfile.ZIP_DEFLATED)
package_length = len(self.microservices_app_package_path)

for root, dirs, files in os.walk(self.microservices_app_package_path):
    root_folder = root[package_length:]
    for file in files:
        microservices_sfpkg.write(os.path.join(
            root, file), os.path.join(root_folder, file))

microservices_sfpkg.close()
```

## <a name="azure-virtual-machine-operating-system-automatic-upgrade-configuration"></a>Automatische upgrade configuratie van het besturings systeem van de virtuele machine van Azure 
Het upgraden van uw virtuele machines is een door de gebruiker geïnitieerde bewerking. het wordt aanbevolen dat u de Schaalset voor de virtuele machine gebruikt voor het [automatisch bijwerken van het besturings systeem](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-automatic-upgrade) voor Azure service Fabric clusters host patch management; Patch Orchestration Application is een alternatieve oplossing die is bedoeld voor wanneer deze buiten Azure wordt gehost, maar POA kan worden gebruikt in azure, waarbij de overhead van het hosten van POA in azure een gemeen schappelijke reden is om een automatische upgrade van het besturings systeem van de virtuele machine te maken over POA. Hieronder vindt u de eigenschappen van de Resource Manager-sjabloon voor de berekening van de virtuele machine Scale set om automatische upgrades van besturings systemen in te scha kelen:

```json
"upgradePolicy": {
   "mode": "Automatic",
   "automaticOSUpgradePolicy": {
        "enableAutomaticOSUpgrade": true,
        "disableAutomaticRollback": false
    }
},
```
Wanneer u automatische OS-upgrades gebruikt met Service Fabric, wordt de nieuwe installatie kopie van het besturings systeem samen met één update domein per keer uitgedraaid om hoge Beschik baarheid van de services die in Service Fabric worden uitgevoerd, te onderhouden. Als u automatische besturingssysteem upgrades wilt gebruiken in Service Fabric moet uw cluster zijn geconfigureerd voor het gebruik van de Silver duurzaamheid-laag of hoger.

Zorg ervoor dat de volgende register sleutel is ingesteld op false om te voor komen dat uw Windows-hostcomputers niet-gecoördineerde updates initiëren: HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU.

Hieronder vindt u de eigenschappen van de Resource Manager-sjabloon voor het berekenen van de naam van de compute virtual machine set om de register sleutel WindowsUpdate in te stellen op False
```json
"osProfile": {
        "computerNamePrefix": "{vmss-name}",
        "adminUsername": "{your-username}",
        "secrets": [],
        "windowsConfiguration": {
          "provisionVMAgent": true,
          "enableAutomaticUpdates": false
        }
      },
```

## <a name="azure-service-fabric-cluster-upgrade-configuration"></a>Upgrade configuratie van Azure Service Fabric cluster
Hier volgt de Service Fabric eigenschap cluster resource manager-sjabloon voor het inschakelen van automatische upgrade:
```json
"upgradeMode": "Automatic",
```
Als u het cluster hand matig wilt upgraden, downloadt u de cab/deb-distributie naar een virtuele cluster machine en roept u de volgende Power shell aan:
```powershell
Copy-ServiceFabricClusterPackage -Code -CodePackagePath <"local_VM_path_to_msi"> -CodePackagePathInImageStore ServiceFabric.msi -ImageStoreConnectionString "fabric:ImageStore"
Register-ServiceFabricClusterPackage -Code -CodePackagePath "ServiceFabric.msi"
Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <"msi_code_version">
```

## <a name="next-steps"></a>Volgende stappen

* Een cluster maken op Vm's of computers met Windows Server: [Service Fabric cluster maken voor Windows Server](service-fabric-tutorial-create-vnet-and-windows-cluster.md)
* Een cluster maken op Vm's of op computers met Linux: [Een Linux-cluster maken](service-fabric-tutorial-create-vnet-and-linux-cluster.md)
* Meer informatie over [ondersteuningsopties voor Service Fabric](service-fabric-support.md)
