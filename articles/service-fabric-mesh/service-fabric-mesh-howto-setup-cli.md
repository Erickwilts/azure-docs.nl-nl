---
title: De Azure Service Fabric Mesh CLI instellen | Microsoft Docs
description: Informatie over hoe u de Azure Service Fabric Mesh CLI instelt.
services: service-fabric-mesh
keywords: ''
author: dkkapur
ms.author: dekapur
ms.date: 11/28/2018
ms.topic: conceptual
ms.service: service-fabric-mesh
manager: timlt
ms.openlocfilehash: c716ae0a2bb30e7e8eb249a1d230097efc0d3795
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "62097790"
---
# <a name="set-up-service-fabric-mesh-cli"></a>Service Fabric Mesh CLI instellen
De Service Fabric Mesh CLI (opdrachtregelinterface) is vereist voor het implementeren en beheren van resources, zowel lokaal als in Azure Service Fabric Mesh. 

Er kunnen drie typen CLI worden gebruikt. Ze staan in de tabel hieronder. 

| CLI Module | Doelomgeving |  Description | 
|---|---|---|
| az mesh | Azure Service Fabric Mesh | De primaire CLI, waarmee u uw toepassingen kunt implementeren en resources beheren in de Azure Service Fabric Mesh-omgeving. 
| sfctl | Lokale clusters | De Service Fabric CLI waarmee u Service Fabric-resources in lokale clusters kunt implementeren en testen.  
| Maven CLI | Lokale clusters en Azure Service Fabric Mesh | Een wrapper rond `az mesh` en `sfctl` waarmee Java-ontwikkelaars een ervaring vertrouwd vanaf de opdrachtregel gebruiken voor lokale en Azure voor ontwikkeling biedt.  

Voor de preview is de Azure Service Fabric-NET CLI geschreven als een uitbreiding voor Azure CLI. U kunt deze installeren in de Azure Cloud Shell of een lokale installatie van Azure CLI. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

## <a name="install-the-azure-service-fabric-mesh-cli"></a>De Azure Service Fabric Mesh CLI installeren
1. Azure CLI versie 2.0.43 of hoger moet geïnstalleerd zijn. Voer `az --version` uit om de versie te bekijken. Raadpleeg [De Azure CLI installeren][azure-cli-install] voor informatie over het installeren van of upgraden naar de nieuwste versie van de CLI.

2. Installeer de Azure Service Fabric Mesh CLI-uitbreidingsmodule met behulp van de volgende opdracht. 

    ```azurecli-interactive
    az extension add --name mesh
    ```

3. Voer de volgende opdracht uit voor het bijwerken van een bestaande Azure Service Fabric Mesh CLI-module.

    ```azurecli-interactive
    az extension update --name mesh
    ```

## <a name="install-the-service-fabric-cli-sfctl"></a>Service Fabric CLI (sfctl) installeren 

Volg de instructies in [Service Fabric CLI instellen](https://docs.microsoft.com/azure/service-fabric/service-fabric-cli). De module **sfctl** kan worden gebruikt om toepassingen op basis van het resourcemodel in Service Fabric-clusters op de lokale computer te implementeren. 

## <a name="install-the-maven-cli"></a>Maven CLI installeren 

Als u de Maven CLI wilt gebruiken, moet het volgende op de computer zijn geïnstalleerd: 

* [Java](https://www.azul.com/downloads/zulu/)
* [Maven](https://maven.apache.org/download.cgi)
* [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
* Azure Mesh CLI (az mesh): voor Azure Service Fabric Mesh 
* SFCTL (sfctl): voor lokale clusters 

De Maven CLI voor Service Fabric is nog in preview. 

Als u de Maven-invoegtoepassing in uw Maven Java-app wilt gebruiken, voegt u het volgende codefragment aan het bestand pom.xml toe:

```XML
<project>
  ...
  <build>
    ...
    <plugins>
      ...
      <plugin>
        <groupId>com.microsoft.azure</groupId>
          <artifactId>azure-sfmesh-maven-plugin</artifactId>
          <version>0.1.0</version>
          <configuration>
            ...
          </configuration>
      </plugin>
    </plugins>
  </build>
</project>
```

Lees het [naslagmateriaal over Maven CLI](service-fabric-mesh-reference-maven.md) voor uitgebreide informatie over het gebruik.

## <a name="next-steps"></a>Volgende stappen

U kunt ook uw [Windows-ontwikkelomgeving](service-fabric-mesh-howto-setup-developer-environment-sdk.md) instellen.

Zoek antwoorden op [veelgestelde vragen en problemen](service-fabric-mesh-faq.md).

[azure-cli-install]: /cli/azure/install-azure-cli
