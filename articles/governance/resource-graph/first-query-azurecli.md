---
title: Uw eerste query uitvoeren met Azure CLI
description: Dit artikel helpt u bij de stappen om de Resource Graph-extensie voor Azure CLI in te schakelen en uw eerste query uit te voeren.
author: DCtheGeek
ms.author: dacoulte
ms.date: 07/26/2019
ms.topic: quickstart
ms.service: resource-graph
manager: carmonm
ms.openlocfilehash: afe32c197b5eab24ba298395bb2c9e83f847d6c5
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68562577"
---
# <a name="quickstart-run-your-first-resource-graph-query-using-azure-cli"></a>Quickstart: Uw eerste Resource Graph-query uitvoeren met Azure CLI

De eerste stap voor het gebruik van Azure Resource Graph bestaat uit het controleren of de extensie voor [Azure CLI](/cli/azure/) is geïnstalleerd. In deze snelstartgids doorloopt u het proces voor het toevoegen van de extensie aan uw Azure CLI-installatie. U kunt de extensie gebruiken met een lokale installatie van Azure CLI of via de [Azure Cloud Shell](https://shell.azure.com).

Aan het einde van dit proces hebt u de extensie toegevoegd aan de Azure CLI-installatie van uw keuze en hebt u uw eerste Resource Graph-query uitgevoerd.

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="add-the-resource-graph-extension"></a>De Resource Graph-extensie toevoegen

De extensie moet worden toegevoegd opdat Azure CLI query's kan uitvoeren voor Azure Resource Graph. Deze extensie werkt overal waar Azure CLI kan worden gebruikt, met inbegrip van [bash in Windows 10](/windows/wsl/install-win10), [Cloud Shell](https://shell.azure.com) (zowel zelfstandige als in de portal), de [Azure CLI Docker-installatiekopie](https://hub.docker.com/r/microsoft/azure-cli/), of lokaal geïnstalleerd.

1. Controleer of de meest recente versie van Azure CLI is geïnstalleerd (minimaal **2.0.45**). Als deze nog niet is geïnstalleerd, volgt u [deze instructies](/cli/azure/install-azure-cli-windows?view=azure-cli-latest) op.

1. In de gewenste Azure CLI-omgeving kunt u deze importeren met de volgende opdracht:

   ```azurecli-interactive
   # Add the Resource Graph extension to the Azure CLI environment
   az extension add --name resource-graph
   ```

1. Controleer of de uitbrei ding is geïnstalleerd en de verwachte versie is (ten minste **0.1.11**):

   ```azurecli-interactive
   # Check the extension list (note that you may have other extensions installed)
   az extension list

   # Run help for graph query options
   az graph query -h
   ```

## <a name="run-your-first-resource-graph-query"></a>Uw eerste Resource Graph-query uitvoeren

Nu de Azure CLI-extensie is toegevoegd aan uw gewenste omgeving, kunt u een eenvoudige Resource Graph-query uitvoeren. De query retourneert de eerste vijf Azure-resources met de **naam** en het **resourcetype** van elke resource.

1. Voer als volgt uw eerste Azure Resource Graph-query uit met de extensie `graph` en de opdracht `query`:

   ```azurecli-interactive
   # Login first with az login if not using Cloud Shell

   # Run Azure Resource Graph query
   az graph query -q 'project name, type | limit 5'
   ```

   > [!NOTE]
   > Omdat deze voorbeeldquery geen sorteermodificator geeft, bijvoorbeeld `order by`, zal deze query waarschijnlijk per aanvraag een andere set resources opleveren als de query meerdere keren wordt uitgevoerd.

1. Werk de query als volgt bij om de eigenschap **naam** te `order by`:

   ```azurecli-interactive
   # Run Azure Resource Graph query with 'order by'
   az graph query -q 'project name, type | limit 5 | order by name asc'
   ```

   > [!NOTE]
   > Net als bij de eerste query zal deze query waarschijnlijk per aanvraag een andere set resources opleveren als de query meerdere keren wordt uitgevoerd. De volgorde van de queryopdrachten is belangrijk. In dit voorbeeld komt `order by` na `limit`. Hiermee worden de queryresultaten eerst beperkt en daarna geordend.

1. Werk de query als volgt bij om eerst te `order by` op de eigenschap **naam** en daarna de resultaten van de top vijf te `limit`:

   ```azurecli-interactive
   # Run Azure Resource Graph query with `order by` first, then with `limit`
   az graph query -q 'project name, type | order by name asc | limit 5'
   ```

Wanneer de laatste query meerdere keren wordt uitgevoerd, ervan uitgaande dat niets in uw omgeving verandert, zijn de geretourneerde resultaten consistent en zoals verwacht. Ze zijn gesorteerd op de eigenschap **naam**, maar nog steeds beperkt tot de top 5-resultaten.

## <a name="clean-up-resources"></a>Resources opschonen

Als u de Resource Graph-extensie uit uw Azure CLI-omgeving wilt verwijderen, kunt u dit doen met de volgende opdracht:

```azurecli-interactive
# Remove the Resource Graph extension from the Azure CLI environment
az extension remove -n resource-graph
```

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de [querytaal](./concepts/query-language.md)
- [Resources verkennen](./concepts/explore-resources.md)
- Uw eerste query uitvoeren met [Azure PowerShell](first-query-powershell.md)
- Voorbeelden uit [Starter query's](./samples/starter.md) bekijken
- Voorbeelden uit [Geavanceerde query's](./samples/advanced.md) bekijken
- Feedback geven op [UserVoice](https://feedback.azure.com/forums/915958-azure-governance)