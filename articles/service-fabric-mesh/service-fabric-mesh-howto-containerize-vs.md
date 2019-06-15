---
title: Een bestaande .NET-app in een container plaatsen voor Service Fabric Mesh | Microsoft Docs
description: Mesh-ondersteuning toevoegen aan een bestaande .NET-app
services: service-fabric-mesh
keywords: service fabric mesh in container plaatsen
author: dkkapur
ms.author: dekapur
ms.date: 11/08/2018
ms.topic: conceptual
ms.service: service-fabric-mesh
manager: chakdan
ms.openlocfilehash: cb4e327e1c8c0a653cb94233f568b4847494c439
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60419431"
---
# <a name="containerize-an-existing-net-app-for-service-fabric-mesh"></a>Een bestaande .NET-app in een container plaatsen voor Service Fabric Mesh

In dit artikel leert u hoe u ondersteuning voor containerindeling van Service Fabric Mesh toevoegt aan een bestaande .NET-app.

In Visual Studio 2017 kunt u ondersteuning voor plaatsing in containers toevoegen aan ASP.NET- en Console-projecten die gebruikmaken van het volledige .NET-framework.

> [!NOTE]
> .NET **Core**-projecten worden momenteel niet ondersteund.

## <a name="prerequisites"></a>Vereisten

* Als u nog geen abonnement op Azure hebt, kunt u een [gratis account maken](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) voordat u begint.

* Zorg er voor dat u [uw ontwikkelomgeving hebt ingesteld](service-fabric-mesh-howto-setup-developer-environment-sdk.md). Dit omvat het installeren van de Service Fabric-runtime, SDK, Docker, Visual Studio 2017 en het maken van een lokaal cluster.

## <a name="open-an-existing-net-app"></a>Een bestaande .NET-app openen

Open de app waaraan u ondersteuning voor containerindeling wilt toevoegen.

Als u een voorbeeld wilt uitproberen, kunt u het codevoorbeeld [eShop](https://github.com/MikkelHegn/ContainersSFLab) gebruiken. In de rest van dit artikel wordt ervan uitgegaan dat dit project wordt gebruikt, hoewel u deze stappen ook op uw eigen project kunt toepassen.

Download een exemplaar van het **eShop**-project:

```git
git clone https://github.com/MikkelHegn/ContainersSFLab.git
```

Als het downloaden is voltooid, opent u **ContainersSFLab\eShopLegacyWebFormsSolution\eShopLegacyWebForms.sln** in Visual Studio 2017.

## <a name="add-container-support"></a>Ondersteuning voor containers toevoegen
 
Voeg als volgt ondersteuning voor containerindeling toe aan een bestaand ASP.NET- of Console-project met de Service Fabric Mesh-hulpprogramma’s:

Klik in de oplossingsverkenner van Visual Studio met de rechtermuisknop op de projectnaam (in het voorbeeld is dat **eShopLegacyWebForms**) en kies **Toevoegen** > **Container Orchestrator Support**.
Het dialoogvenster **Add Container Orchestrator Support** verschijnt.

![Dialoogvenster containerindeling toevoegen in Visual Studio](./media/service-fabric-mesh-howto-containerize-vs/add-container-orchestration-support.png)

Kies **Service Fabric Mesh** in de vervolgkeuzelijst en klik op **OK**.

Er wordt gecontroleerd of Docker is geïnstalleerd; er wordt een Docker-bestand aan het project toegevoegd en er wordt een Docker-installatiekopie voor het project opgehaald.  
Er wordt een Service Fabric Mesh-toepassingsproject aan uw oplossing toegevoegd. Het bevat uw Mesh-publicatieprofielen en -configuratiebestanden. De naam van het project is dezelfde als die van uw project, waarbij er Application aan het eind is toegevoegd, bijvoorbeeld **eShopLegacyWebFormsApplication**. 

In het nieuwe Mesh-project ziet u twee mappen:
- **App-resources**: deze bevat YAML-bestanden waarmee aanvullende Mesh-resources worden beschreven, zoals het netwerk.
- **Serviceresources**: deze bevat het bestand service.yaml, waarmee wordt beschreven hoe uw app moet worden uitgevoerd als deze is geïmplementeerd.

Zodra ondersteuning voor containerindeling aan de app is toegevoegd, kunt u op **F5** drukken om fouten in uw .NET-app op te sporen in uw lokale Service Fabric Mesh-cluster. Hier ziet u de ASP.NET-app van eShop die wordt uitgevoerd op een Service Fabric Mesh-cluster: 

![eShop-app](./media/service-fabric-mesh-howto-containerize-vs/eshop-running.png)

U kunt nu de app publiceren naar Azure Service Fabric Mesh.

## <a name="next-steps"></a>Volgende stappen

Zie hoe u een app publiceert voor Service Fabric Mesh: [Zelfstudie - Implementeren van een Service Fabric Mesh-toepassing](service-fabric-mesh-tutorial-deploy-service-fabric-mesh-app.md)