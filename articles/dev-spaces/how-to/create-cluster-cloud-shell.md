---
title: Over het maken van een Kubernetes-cluster ingeschakeld voor Azure Dev ruimten met behulp van Azure Cloud Shell
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
author: zr-msft
ms.author: zarhoads
ms.date: 10/04/2018
ms.topic: conceptual
description: Meer informatie over het snel maken van een Kubernetes-cluster ingeschakeld voor Azure Dev opslagruimten direct vanuit uw browser zonder iets te installeren.
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, containers, Helm, NET service, service mesh-routering, kubectl, k8s
ms.openlocfilehash: c9dabc13e85295b88483f43b26ccf0b15406ad9b
ms.sourcegitcommit: 4c2b9bc9cc704652cc77f33a870c4ec2d0579451
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2019
ms.locfileid: "65861610"
---
# <a name="create-a-kubernetes-cluster-using-azure-cloud-shell"></a>Een Kubernetes-cluster maken met behulp van Azure Cloud Shell

U kunt [Azure Cloud Shell](/azure/cloud-shell) gebruiken om een cluster te maken voor Azure Dev Spaces met behulp van de **uitproberen** knop op deze pagina. Als u niet bent aangemeld, volg de aanwijzingen voor het aanmelden met een Azure-account en vervolgens typt u de opdrachten bij de Azure Cloud Shell-prompt wanneer deze wordt weergegeven.

## <a name="create-the-cluster"></a>Het cluster maken

Maak eerst de resourcegroep in een [regio die ondersteuning biedt voor Azure Dev spaties](https://docs.microsoft.com/azure/dev-spaces/#a-rapid,-iterative-kubernetes-development-experience-for-teams).

```azurecli-interactive
az group create --name MyResourceGroup --location <region>
```

Maak een Kubernetes-cluster met de volgende opdracht:

```azurecli-interactive
az aks create -g MyResourceGroup -n MyAKS --location <region> --disable-rbac --generate-ssh-keys
```

Het duurt een paar minuten om het cluster te maken.  Als het gereed is wordt de uitvoer weergegeven in de JSON-indeling. Zoek naar `provisioningState` en controleer of er `Succeeded` staat.

## <a name="next-steps"></a>Volgende stappen

Zie [Azure Dev spaties](/azure/dev-spaces/) voor koppelingen naar volledige zelfstudies.
