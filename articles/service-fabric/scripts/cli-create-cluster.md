---
title: Implementatievoorbeeld van een Azure CLI-script
description: Maak met behulp van Azure CLI een beveiligd Service Fabric Linux-cluster in Azure.
services: service-fabric
documentationcenter: ''
author: athinanthny
manager: chackdan
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 01/18/2018
ms.author: atsenthi
ms.custom: mvc
ms.openlocfilehash: 397ae7ee3b5accbbac1bc17020e6c32397c105a2
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2019
ms.locfileid: "68592385"
---
# <a name="create-a-secure-service-fabric-linux-cluster-in-azure"></a>Een beveiligd Service Fabric Linux-cluster maken in Azure

Met deze opdracht wordt een zelfondertekend certificaat gemaakt, wordt dit toegevoegd aan een sleutelkluis en wordt het certificaat lokaal gedownload.  Het nieuwe certificaat wordt gebruikt om het cluster te beveiligen wanneer het wordt geïmplementeerd.  U kunt ook een bestaand certificaat gebruiken in plaats van een nieuw te maken.  In beide gevallen moet de onderwerpnaam van het certificaat overeenkomen met het domein dat u gebruikt om toegang te krijgen tot het Service Fabric-cluster. Deze overeenkomst is vereist om een SSL te kunnen verstrekken voor de HTTPS-beheereindpunten van het cluster en Service Fabric Explorer. U kunt geen SSL-certificaat verkrijgen van een certificeringsinstantie voor het domein `.cloudapp.azure.com`. U hebt voor uw cluster een aangepaste domeinnaam nodig. Wanneer u een certificaat van een CA aanvraagt, moet de onderwerpnaam van het certificaat overeenkomen met de aangepaste domeinnaam die u voor uw cluster gebruikt.

Installeer zo nodig [Azure CLI](/en-us/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="sample-script"></a>Voorbeeldscript

[!code-sh[main](../../../cli_scripts/service-fabric/create-cluster/create-cluster.sh "Deploy an application to a cluster")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie

Nadat het voorbeeldscript is uitgevoerd, kan de volgende opdracht worden gebruikt om de resourcegroep, het cluster en alle gerelateerde resources te verwijderen.

```azurecli
ResourceGroupName = "aztestclustergroup"
az group delete --name $ResourceGroupName
```

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [az sf cluster create](https://docs.microsoft.com/cli/azure/sf/cluster?view=azure-cli-latest) | Hiermee wordt een nieuw Service Fabric-cluster gemaakt.  |

## <a name="next-steps"></a>Volgende stappen

Meer Service Fabric CLI-voorbeelden voor Azure Service Fabric zijn te vinden in de [Voorbeelden van Azure Service Fabric CLI](../samples-cli.md).
