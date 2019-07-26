---
title: Azure Service Fabric-knooppunt typen en virtuele-machine schaal sets | Microsoft Docs
description: Meer informatie over hoe Azure Service Fabric knooppunt typen betrekking hebben op schaal sets voor virtuele machines en hoe u extern verbinding maakt met een exemplaar van een schaalset of cluster knooppunt.
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: chackdan
editor: ''
ms.assetid: 5441e7e0-d842-4398-b060-8c9d34b07c48
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/23/2018
ms.author: chackdan
ms.openlocfilehash: f33b25112b5c4ee77f1f7d2a419ffb8e926a27d9
ms.sourcegitcommit: a0b37e18b8823025e64427c26fae9fb7a3fe355a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/25/2019
ms.locfileid: "68501367"
---
# <a name="azure-service-fabric-node-types-and-virtual-machine-scale-sets"></a>Azure Service Fabric-knooppunt typen en schaal sets voor virtuele machines
[Virtuele-machine schaal sets](/azure/virtual-machine-scale-sets) vormen een Azure Compute-resource. U kunt schaal sets gebruiken voor het implementeren en beheren van een verzameling virtuele machines als een set. Elk knooppunt type dat u in een Azure Service Fabric-cluster definieert, stelt een afzonderlijke schaal in.  De Service Fabric runtime geïnstalleerd op elke virtuele machine in de schaalset door de extensie van de virtuele machine van micro soft. Azure. ServiceFabric. U kunt elk knooppunt type omhoog of omlaag schalen, de SKU van het besturings systeem wijzigen dat op elk cluster knooppunt wordt uitgevoerd, verschillende sets poorten openen en verschillende capaciteits metrieken gebruiken.

In de volgende afbeelding ziet u een cluster met twee knooppunt typen, front-end en back-end. Elk knooppunt type heeft vijf knoop punten.

![Een cluster met twee knooppunt typen][NodeTypes]

## <a name="map-virtual-machine-scale-set-instances-to-nodes"></a>Instanties van schaal sets voor virtuele machines toewijzen aan knoop punten
Zoals in de voor gaande afbeelding wordt weer gegeven, beginnen de schaalset-instanties bij instantie 0 en nemen ze vervolgens toe met 1. De nummering wordt weer gegeven in de namen van knoop punten. Zo is het knoop punt BackEnd_0 instantie 0 van de back-upschaalset. Deze schaalset heeft vijf instanties, met de naam BackEnd_0, BackEnd_1, BackEnd_2, BackEnd_3 en BackEnd_4.

Wanneer u een schaalset opschaalt, wordt er een nieuw exemplaar gemaakt. De nieuwe instantie naam van de schaalset is doorgaans de naam van de schaalset plus het volgende exemplaar nummer. In ons voor beeld is dit BackEnd_5.

## <a name="map-scale-set-load-balancers-to-node-types-and-scale-sets"></a>Load balancers instellen voor de kaart schaal voor knooppunt typen en schaal sets
Als u uw cluster in de Azure Portal hebt geïmplementeerd of de voor beeld-Azure Resource Manager sjabloon hebt gebruikt, worden alle resources onder een resource groep weer gegeven. U kunt de load balancers voor elke schaalset of elk knooppunt type bekijken. De naam van de load balancer gebruikt de volgende indeling: Naam van het **lb-&lt;knooppunt type.&gt;** Een voor beeld is LB-sfcluster4doc-0, zoals wordt weer gegeven in de volgende afbeelding:

![Resources][Resources]

## <a name="service-fabric-virtual-machine-extension"></a>Extensie van de virtuele machine Service Fabric
Service Fabric extensie van de virtuele machine wordt gebruikt om Service Fabric naar Azure Virtual Machines te bootslen en de knooppunt beveiliging te configureren.

Hier volgt een fragment van Service Fabric extensie van de virtuele machine:

```json
"extensions": [
  {
    "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
    "properties": {
      "type": "ServiceFabricLinuxNode",
      "autoUpgradeMinorVersion": true,
      "protectedSettings": {
        "StorageAccountKey1": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('supportLogStorageAccountName')),'2015-05-01-preview').key1]",
       },
       "publisher": "Microsoft.Azure.ServiceFabric",
       "settings": {
         "clusterEndpoint": "[reference(parameters('clusterName')).clusterEndpoint]",
         "nodeTypeRef": "[variables('vmNodeType0Name')]",
         "durabilityLevel": "Silver",
         "enableParallelJobs": true,
         "nicPrefixOverride": "[variables('subnet0Prefix')]",
         "dataPath": "D:\\\\SvcFab",
         "certificate": {
           "commonNames": [
             "[parameters('certificateCommonName')]"
           ],
           "x509StoreName": "[parameters('certificateStoreValue')]"
         }
       },
       "typeHandlerVersion": "1.1"
     }
   },
```

Hier volgen de eigenschaps beschrijvingen:

| **Name** | **Toegestane waarden** | ** --- ** | **Uitleg of korte beschrijving** |
| --- | --- | --- | --- |
| name | string | --- | unieke naam voor extensie |
| type | "ServiceFabricLinuxNode" of "ServiceFabricWindowsNode | --- | Geeft aan dat het besturings systeem Service Fabric Boots trapt |
| autoUpgradeMinorVersion | waar of onwaar | --- | Automatische upgrade van secundaire versies van SF runtime inschakelen |
| publisher | Microsoft.Azure.ServiceFabric | --- | de naam van de uitgever van het Service Fabric gebied |
| clusterEndpont | string | --- | URI: poort voor beheer eindpunt |
| nodeTypeRef | string | --- | naam van nodeType |
| durabilityLevel | Bronze, Silver, Gold, Platinum | --- | tijd die is toegestaan voor het onderbreken van een onveranderlijke Azure-infra structuur |
| enableParallelJobs | waar of onwaar | --- | Schakel Compute ParallelJobs in zoals virtuele machine verwijderen en start VM opnieuw op in dezelfde schaalset als parallel |
| nicPrefixOverride | string | --- | Subnetvoorvoegsel zoals "10.0.0.0/24" |
| commonNames | teken reeks [] | --- | Algemene namen van geïnstalleerde cluster certificaten |
| x509StoreName | string | --- | De naam van het archief waar het geïnstalleerde cluster certificaat zich bevindt |
| typeHandlerVersion | 1.1 | --- | De versie van de extensie. 1,0 de klassieke versie van de uitbrei ding wordt aanbevolen om te upgraden naar 1,1 |
| dataPath | string | --- | Het pad naar het station dat wordt gebruikt om de status voor Service Fabric systeem services en toepassings gegevens op te slaan. 

## <a name="next-steps"></a>Volgende stappen
* Zie het [overzicht van de functie overal implementeren en een vergelijking met door Azure beheerde clusters](service-fabric-deploy-anywhere.md).
* Meer informatie over [cluster beveiliging](service-fabric-cluster-security.md).
* [Externe verbinding maken](service-fabric-cluster-remote-connect-to-azure-cluster-node.md) met een specifiek exemplaar van een schaalset
* [De waarden voor het RDP-poort bereik bijwerken na de](./scripts/service-fabric-powershell-change-rdp-port-range.md) implementatie van cluster-vm's
* [De gebruikers naam en het wacht woord van de beheerder](./scripts/service-fabric-powershell-change-rdp-user-and-pw.md) voor cluster-vm's wijzigen

<!--Image references-->
[NodeTypes]: ./media/service-fabric-cluster-nodetypes/NodeTypes.png
[Resources]: ./media/service-fabric-cluster-nodetypes/Resources.png
[InboundNatPools]: ./media/service-fabric-cluster-nodetypes/InboundNatPools.png
