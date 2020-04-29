---
title: Problemen met het gebruik van VM-extensies in Python 3-Linux Azure Virtual Machines-systemen
description: Meer informatie over het gebruik van VM-extensies in Python 3-Linux-systemen
services: virtual-machines-windows
documentationcenter: ''
author: v-miegge
ms.author: jparrel
manager: dcscontentpm
editor: ''
tags: top-support-issue,azure-resource-manager
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 04/22/2020
ms.assetid: 3cd520fd-eaf7-4ef9-b4d3-4827057e5028
ms.openlocfilehash: 944abc62f25473ea52836af7dc1fdcd1e16d9269
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "82120780"
---
# <a name="issues-using-vm-extensions-in-python-3-enabled-linux-azure-virtual-machines-systems"></a>Problemen met het gebruik van VM-extensies in Python 3-Linux Azure Virtual Machines-systemen

> [!NOTE]
> Micro soft raadt gebruikers aan om **python 3. x** in hun systemen aan te nemen, tenzij voor uw werk belasting **python 2. x** -ondersteuning vereist is. Voor beelden van deze vereiste zijn verouderde beheer scripts of uitbrei dingen zoals **Azure Disk Encryption** en **Azure monitor**.
>
> Voordat u **python 2. x** in productie installeert, moet u rekening houden met de lange termijn ondersteuning van python 2. x, met name de mogelijkheid om beveiligings updates te ontvangen. Als producten, met inbegrip van een deel van de vermelde uitbrei ding, werkt u bij met **python 3,8** -ondersteuning, moet u het gebruik van python 2. x blijven gebruiken.

Sommige Linux-distributies zijn overgegaan naar python 3,8 en het oude `/usr/bin/python` ingangs punt voor python zijn verwijderd. Deze overgang is van invloed op de out-of-the-box, geautomatiseerde implementatie van bepaalde VM-uitbrei dingen (virtuele machine) met de volgende voor waarden:

- Uitbrei dingen die nog steeds overschakelen naar python 3. x-ondersteuning
- Uitbrei dingen die gebruikmaken `/usr/bin/python` van het verouderde ingangs punt

Linux-distributie gebruikers die zijn overgestapt op **python 3. x** , moeten ervoor zorgen `/usr/bin/python` dat het verouderde ingangs punt bestaat voordat deze uitbrei dingen worden geïmplementeerd op hun vm's. Anders kan de implementatie van de uitbrei ding mislukken. 

- De onderschreven Linux-distributies die worden beïnvloed, zijn **Ubuntu Server 20,04 LTS** en **Ubuntu Pro 20,04 LTS**.

- Betrokken VM-extensies bevatten **Azure Disk Encryption**, **log Analytics**, **VM-toegang** (gebruikt voor het opnieuw instellen van wacht woorden) en **Diagnostische gegevens** van de gast (gebruikt voor aanvullende prestatie meter items).

In-place Upgrades, zoals het upgraden van **Ubuntu 18,04 LTS** naar **Ubuntu 20,04 LTS**, moeten de `/usr/bin/python` symlink behouden en blijven ongewijzigd.

## <a name="resolution"></a>Oplossing

Houd rekening met de volgende algemene aanbevelingen voordat u extensies implementeert in de scenario's met bekende problemen die eerder in de samen vatting zijn beschreven:

1.  Voordat u de uitbrei ding implementeert, moet u de `/usr/bin/python` symlink opnieuw invoeren met behulp van de door de leverancier van Linux-distributie.

    - Gebruik bijvoorbeeld voor **Python 2,7**:`sudo apt update && sudo apt install python-is-python2`

2.  Als u al een exemplaar hebt geïmplementeerd dat dit probleem vertoont, gebruikt u de **opdracht uitvoeren** op de **Blade van de virtuele machine** om de opdrachten uit te voeren die hierboven worden genoemd. De opdracht extensie uitvoeren zelf wordt niet beïnvloed door de overgang naar python 3,8.

3.  Als u een nieuwe instantie implementeert en een uitbrei ding moet instellen tijdens de inrichting, gebruikt u **Cloud-init-** gebruikers gegevens om de hierboven vermelde pakketten te installeren.

    Bijvoorbeeld voor python 2,7:

    ```
    # create cloud-init config
    cat > cloudinitConfig.json <<EOF
    #cloud-config
    package_update: true
    
    runcmd:
    - sudo apt update
    - sudo apt install python-is-python2 
    EOF
    
    # create VM
    az vm create \
        --resource-group <resourceGroupName> \
        --name <vmName> \
        --image <Ubuntu 20.04 Image URN> \
        --admin-username azadmin \
        --ssh-key-value "<sshPubKey>" \
        --custom-data ./cloudinitConfig.json
    ```

4.  Als de beleids beheerder van uw organisatie bepaalt dat uitbrei dingen niet mogen worden geïmplementeerd in Vm's, kunt u de ondersteuning voor de extensie uitschakelen op het tijdstip van inrichting:

    - REST-API

      Als u uitbrei dingen wilt uitschakelen en inschakelen wanneer u een virtuele machine met deze eigenschap kunt implementeren:

      ```
        "osProfile": {
          "allowExtensionOperations": false
        },
      ```

## <a name="next-steps"></a>Volgende stappen

Raadpleeg [andere basis systeem wijzigingen sinds 18,04 LTS-python 3 standaard](https://wiki.ubuntu.com/FocalFossa/ReleaseNotes#Python3_by_default) voor aanvullende informatie.
