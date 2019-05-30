---
title: Overzicht van Jenkins en Azure
description: Host de Jenkins-build, implementeer een automatiseringsserver in Azure en gebruik reken- en opslagresources van Azure om de (CI-/CD-)pijplijnen voor continue levering en implementatie uit te breiden.
ms.service: jenkins
keywords: jenkins, azure, devops, overzicht
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.topic: overview
ms.date: 07/25/2018
ms.openlocfilehash: 86d32726280cce12888f125c65254a7b02166704
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "60641242"
---
# <a name="azure-and-jenkins"></a>Azure en Jenkins

[Jenkins](https://jenkins.io/) is een populaire open source-automatiseringsserver waarmee u continue integratie en levering kunt instellen voor softwareprojecten. U kunt de Jenkins-implementatie hosten in Azure of een bestaande Jenkins-configuratie uitbreiden met behulp van Azure-resources. Jenkins-invoegtoepassingen kunnen ook worden gebruikt om CI/CD van toepassingen in Azure te vereenvoudigen.

Dit artikel is een inleiding tot het gebruik van Azure met Jenkins en biedt details over de kernfuncties die beschikbaar zijn voor Jenkins-gebruikers. Zie [Een Jenkins-server maken in Azure](install-jenkins-solution-template.md) voor meer informatie over het aan de slag gaan met uw eigen Jenkins-server in Azure.

## <a name="host-your-jenkins-servers-in-azure"></a>Jenkins-servers hosten in Azure

Host Jenkins in Azure om de automatisering van uw build te centraliseren en de schaal van de implementatie aan te passen zodra de behoeften van softwareprojecten groeien. U kunt Jenkins implementeren in Azure met behulp van:
 
- [De Jenkins-oplossingssjabloon](install-jenkins-solution-template.md) in Azure Marketplace.
- [Virtuele machines van Azure](/azure/virtual-machines/linux/overview). Zie onze [zelfstudie](/azure/virtual-machines/linux/tutorial-jenkins-github-docker-cicd) om een Jenkins-exemplaar te maken op een VM.
- Raadpleeg de [instructies](/azure/container-service/kubernetes/container-service-kubernetes-walkthrough) voor het uitvoeren van een Kubernetes-cluster in [Azure Container Service](/azure/container-service/kubernetes/container-service-kubernetes-jenkins).

Controleren en beheren van uw Azure-Jenkins implementeren met [logboeken van Azure Monitor](/azure/log-analytics/log-analytics-overview) en de [Azure CLI](/cli/azure).

## <a name="scale-your-build-automation-on-demand"></a>Automatisering van uw build schalen op aanvraag

Voeg buildagents toe aan een bestaande Jenkins-implementatie om de capaciteit van uw Jenkins-build te schalen als het aantal builds groeit en de complexiteit van taken en pijplijnen toeneemt. U kunt deze buildagents uitvoeren op virtuele machines van Azure door gebruik te maken van de [invoegtoepassing Azure VM-agents](jenkins-azure-vm-agents.md). Zie onze [zelfstudie](/azure/jenkins/jenkins-azure-vm-agents) voor meer details.

Zodra Jenkins-taken en -pijplijnen zijn geconfigureerd voor gebruik met een [Azure-service-principal](/azure/azure-resource-manager/resource-group-overview), kunnen deze referenties worden gebruikt om:

- Buildartefacten veilig op te slaan en te archiveren in [Azure Storage](/azure/storage/common/storage-introduction) met behulp van de [invoegtoepassing Azure Storage](https://plugins.jenkins.io/windows-azure-storage). Raadpleeg de [Instructies voor Jenkins-opslag](/azure/storage/common/storage-java-jenkins-continuous-integration-solution) voor meer informatie.
- Beheer en configureer Azure-resources met [Azure CLI](/azure/jenkins/execute-cli-jenkins-pipeline).

## <a name="deploy-your-code-into-azure-services"></a>Uw code implementeren in Azure-services

Gebruik Jenkins-invoegtoepassingen om toepassingen te implementeren in Azure als onderdeel van CI/CD-pijplijnen van Jenkins. Door te implementeren in [Azure App Service](/azure/app-service/) en [Azure Container Service](/azure/container-service/kubernetes/) kunt u updates faseren, testen en vrijgeven voor uw toepassingen zonder de onderliggende infrastructuur te beheren.

 Invoegtoepassingen kunnen worden geïmplementeerd in de volgende services en omgevingen:

- [Azure App Service op Linux](/azure/app-service/containers/app-service-linux-intro). Bekijk de [zelfstudie](java-deploy-webapp-tutorial.md) om aan de slag te gaan.
- [Azure App Service](/azure/app-service/overview). Bekijk de [instructies](deploy-Jenkins-app-service-plugin.md) om aan de slag te gaan.