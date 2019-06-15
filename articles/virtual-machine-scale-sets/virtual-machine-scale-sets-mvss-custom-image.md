---
title: Verwijzen naar een aangepaste installatiekopie in een sjabloon van de Azure-schaal | Microsoft Docs
description: Leer hoe u een aangepaste installatiekopie toevoegen aan een bestaande sjabloon voor Azure virtuele-Machineschaalset opgehaald
services: virtual-machine-scale-sets
documentationcenter: ''
author: mayanknayar
manager: drewm
editor: ''
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2018
ms.author: manayar
ms.openlocfilehash: 2415d0dc2b9a2c4229d9910b42eb8ec9309ac7a7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64869099"
---
# <a name="add-a-custom-image-to-an-azure-scale-set-template"></a>Een aangepaste installatiekopie toevoegen aan een Azure-schaalsetsjabloon

Dit artikel wordt beschreven hoe u wijzigt de [eenvoudige sjabloon voor schaalsets](virtual-machine-scale-sets-mvss-start.md) implementeren vanuit een aangepaste installatiekopie.

## <a name="change-the-template-definition"></a>De sjabloondefinitie van de wijzigen
In een [vorige artikel](virtual-machine-scale-sets-mvss-start.md) we een eenvoudige schaalsetsjabloon hebt gemaakt. We nu deze oudere sjabloon gebruiken en wijzigen voor het maken van een sjabloon die een schaalset op basis van een aangepaste installatiekopie implementeert.  

### <a name="creating-a-managed-disk-image"></a>Installatiekopie van een beheerde schijf maken

Als u al een aangepaste beheerde installatiekopie (een resource van het type `Microsoft.Compute/images`), en vervolgens kunt u deze sectie overslaan.

Voeg eerst toe een `sourceImageVhdUri` parameter, die de URI naar de gegeneraliseerde blob in Azure Storage met de aangepaste installatiekopie om vanuit te implementeren.


```diff
     },
     "adminPassword": {
       "type": "securestring"
+    },
+    "sourceImageVhdUri": {
+      "type": "string",
+      "metadata": {
+        "description": "The source of the generalized blob containing the custom image"
+      }
     }
   },
   "variables": {},
```

Vervolgens voegt u toe een resource van het type `Microsoft.Compute/images`, die de installatiekopie van de beheerde schijf op basis van de gegeneraliseerde blob zich bevindt in de URI is `sourceImageVhdUri`. Deze installatiekopie moet zich in dezelfde regio als de schaalset die wordt gebruikt. Geef in de eigenschappen van de installatiekopie, het type besturingssysteem, de locatie van de blob (van de `sourceImageVhdUri` parameter), en het opslagaccounttype:

```diff
   "resources": [
     {
+      "type": "Microsoft.Compute/images",
+      "apiVersion": "2019-03-01",
+      "name": "myCustomImage",
+      "location": "[resourceGroup().location]",
+      "properties": {
+        "storageProfile": {
+          "osDisk": {
+            "osType": "Linux",
+            "osState": "Generalized",
+            "blobUri": "[parameters('sourceImageVhdUri')]",
+            "storageAccountType": "Standard_LRS"
+          }
+        }
+      }
+    },
+    {
       "type": "Microsoft.Network/virtualNetworks",
       "name": "myVnet",
       "location": "[resourceGroup().location]",

```

In de schaalset ingesteld resource, Voeg een `dependsOn` component die verwijzen naar de aangepaste installatiekopie om te controleren of de installatiekopie wordt gemaakt voordat de schaalset probeert te implementeren vanuit die installatiekopie:

```diff
       "location": "[resourceGroup().location]",
       "apiVersion": "2019-03-01-preview",
       "dependsOn": [
-        "Microsoft.Network/virtualNetworks/myVnet"
+        "Microsoft.Network/virtualNetworks/myVnet",
+        "Microsoft.Compute/images/myCustomImage"
       ],
       "sku": {
         "name": "Standard_A1",

```

### <a name="changing-scale-set-properties-to-use-the-managed-disk-image"></a>Het wijzigen van schaal instellen eigenschappen om te gebruiken van de installatiekopie van de beheerde schijf

In de `imageReference` instellen van de schaal `storageProfile`, in plaats van op te geven de uitgever, aanbieding, sku, en versie van een platforminstallatiekopie, geef de `id` van de `Microsoft.Compute/images` resource:

```diff
         "virtualMachineProfile": {
           "storageProfile": {
             "imageReference": {
-              "publisher": "Canonical",
-              "offer": "UbuntuServer",
-              "sku": "16.04-LTS",
-              "version": "latest"
+              "id": "[resourceId('Microsoft.Compute/images', 'myCustomImage')]"
             }
           },
           "osProfile": {
```

In dit voorbeeld gebruikt de `resourceId` functie voor het ophalen van de resource-ID van de installatiekopie hebt gemaakt in dezelfde sjabloon. Als u de installatiekopie van de beheerde schijf vooraf hebt gemaakt, moet u in plaats daarvan de ID van die installatiekopie opgeven. Deze ID moet van het formulier: `/subscriptions/<subscription-id>resourceGroups/<resource-group-name>/providers/Microsoft.Compute/images/<image-name>`.


## <a name="next-steps"></a>Volgende stappen

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
