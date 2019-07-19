---
title: Opdrachten uitvoeren in actieve containers in Azure Container Instances
description: Meer informatie over het uitvoeren van een opdracht in een container die momenteel wordt uitgevoerd in Azure Container Instances
services: container-instances
author: dlepow
manager: gwallace
ms.service: container-instances
ms.topic: article
ms.date: 03/30/2018
ms.author: danlep
ms.openlocfilehash: a8583cf605891631a2bce6914b24525aebd59ea0
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2019
ms.locfileid: "68325984"
---
# <a name="execute-a-command-in-a-running-azure-container-instance"></a>Een opdracht uitvoeren in een actief Azure-container exemplaar

Azure Container Instances ondersteunt het uitvoeren van een opdracht in een actieve container. Het uitvoeren van een opdracht in een container die u al hebt gestart, is met name handig tijdens het ontwikkelen van toepassingen en het oplossen van problemen. Het meest voorkomende gebruik van deze functie is het starten van een interactieve shell, zodat u problemen kunt opsporen in een actieve container.

## <a name="run-a-command-with-azure-cli"></a>Een opdracht uitvoeren met Azure CLI

Voer een opdracht uit in een actieve container met [AZ container exec][az-container-exec] in the [Azure CLI][azure-cli]:

```azurecli
az container exec --resource-group <group-name> --name <container-group-name> --exec-command "<command>"
```

Als u bijvoorbeeld een bash-shell in een nginx-container wilt starten, gaat u als volgt te werk:

```azurecli
az container exec --resource-group myResourceGroup --name mynginx --exec-command "/bin/bash"
```

In het onderstaande voor beeld wordt de bash-shell gestart in een actieve Linux-container, waardoor er een Terminal `ls` is waarin wordt uitgevoerd:

```console
$ az container exec --resource-group myResourceGroup --name mynginx --exec-command "/bin/bash"
root@caas-83e6c883014b427f9b277a2bba3b7b5f-708716530-2qv47:/# ls
bin   dev  home  lib64  mnt  proc  run   srv  tmp  var
boot  etc  lib   media  opt  root  sbin  sys  usr
root@caas-83e6c883014b427f9b277a2bba3b7b5f-708716530-2qv47:/# exit
exit
Bye.
```

In dit voor beeld wordt de opdracht prompt gestart in een actieve nano server-container:

```console
$ az container exec --resource-group myResourceGroup --name myiis --exec-command "cmd.exe"
Microsoft Windows [Version 10.0.14393]
(c) 2016 Microsoft Corporation. All rights reserved.

C:\>dir
 Volume in drive C has no label.
 Volume Serial Number is 76E0-C852

 Directory of C:\

03/23/2018  09:13 PM    <DIR>          inetpub
11/20/2016  11:32 AM             1,894 License.txt
03/23/2018  09:13 PM    <DIR>          Program Files
07/16/2016  12:09 PM    <DIR>          Program Files (x86)
03/13/2018  08:50 PM           171,616 ServiceMonitor.exe
03/23/2018  09:13 PM    <DIR>          Users
03/23/2018  09:12 PM    <DIR>          var
03/23/2018  09:22 PM    <DIR>          Windows
               2 File(s)        173,510 bytes
               6 Dir(s)  21,171,609,600 bytes free

C:\>exit
Bye.
```

## <a name="multi-container-groups"></a>Groepen met meerdere containers

Als uw [container groep](container-instances-container-groups.md) meerdere containers heeft, zoals een toepassings container en een logboek registratie, geeft u de naam op van de container waarin de opdracht `--container-name`moet worden uitgevoerd.

In de container groep *mynginx* zijn bijvoorbeeld twee containers, *nginx-app* en *logboek registratie*. Een shell op de *nginx-app-* container starten:

```azurecli
az container exec --resource-group myResourceGroup --name mynginx --container-name nginx-app --exec-command "/bin/bash"
```

## <a name="restrictions"></a>Beperkingen

Azure Container Instances ondersteunt momenteel het starten van één proces met [AZ container exec][az-container-exec]en u kunt geen opdracht argumenten door geven. Het is bijvoorbeeld niet mogelijk om opdrachten als in `sh -c "echo FOO && echo BAR"`te koppelen of `echo FOO`uit te voeren.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over andere hulpprogram ma's voor probleem oplossing en veelvoorkomende implementatie problemen in het oplossen van problemen [met containers en implementaties in azure container instances](container-instances-troubleshooting.md).

<!-- LINKS - internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-exec]: /cli/azure/container#az-container-exec
[azure-cli]: /cli/azure