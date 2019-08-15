---
title: Voor beelden van Azure Service Fabric container Application-manifesten | Microsoft Docs
description: Meer informatie over het configureren van de toepassings-en service manifest instellingen voor een Service Fabric-toepassing.
services: service-fabric
documentationcenter: na
author: peterpogorski
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/11/2018
ms.author: pepogors
ms.openlocfilehash: 10419240f730a6b68f1161f158f7f903a98a9933
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/15/2019
ms.locfileid: "69035610"
---
# <a name="service-fabric-application-and-service-manifest-examples"></a>Voor beelden van Service Fabric-toepassingen en-services
Deze sectie bevat voor beelden van toepassings-en service manifesten. Deze voor beelden zijn niet bedoeld voor het weer geven van belang rijke scenario's, maar voor het weer geven van de verschillende instellingen die beschikbaar zijn en hoe u deze kunt gebruiken. 

Hier volgt een index van de weer gegeven functies en de manifesten waarvan ze deel uitmaken.

|Functie|Manifest|
|---|---|
|[Resourcebeheer](service-fabric-resource-governance.md)|[Toepassings manifest reliable Services](service-fabric-manifest-example-reliable-services-app.md#application-manifest), [container toepassings manifest](service-fabric-manifest-example-container-app.md#application-manifest)|
|[Een service uitvoeren als een lokaal beheerders account](service-fabric-application-runas-security.md)|[Reliable Services toepassings manifest](service-fabric-manifest-example-reliable-services-app.md#application-manifest)|
|[Een standaard beleid Toep assen op alle service code pakketten](service-fabric-application-runas-security.md#apply-a-default-policy-to-all-service-code-packages)|[Reliable Services toepassings manifest](service-fabric-manifest-example-reliable-services-app.md#application-manifest)|
|[Gebruikers-en groeps-principals maken](service-fabric-application-runas-security.md)|[Reliable Services toepassings manifest](service-fabric-manifest-example-reliable-services-app.md#application-manifest)|
|een gegevens pakket delen tussen service-exemplaren|[Reliable Services toepassings manifest](service-fabric-manifest-example-reliable-services-app.md#application-manifest)|
|[Service-eind punten onderdrukken](service-fabric-service-manifest-resources.md#overriding-endpoints-in-servicemanifestxml)|[Reliable Services toepassings manifest](service-fabric-manifest-example-reliable-services-app.md#application-manifest)|
|[Een script uitvoeren bij het starten van de service](service-fabric-run-script-at-service-startup.md)|[VotingWeb-service manifest](service-fabric-manifest-example-reliable-services-app.md#votingweb-service-manifest)|
|[Een HTTPS-eind punt definiëren](service-fabric-tutorial-dotnet-app-enable-https-endpoint.md#define-an-https-endpoint-in-the-service-manifest)|[VotingWeb-service manifest](service-fabric-manifest-example-reliable-services-app.md#votingweb-service-manifest)|
|[Een configuratie pakket declareren](service-fabric-application-and-service-manifests.md)|[VotingData-service manifest](service-fabric-manifest-example-reliable-services-app.md#votingdata-service-manifest)|
|[Een gegevens pakket declareren](service-fabric-application-and-service-manifests.md)|[VotingData-service manifest](service-fabric-manifest-example-reliable-services-app.md#votingdata-service-manifest)|
|[Omgevings variabelen negeren](service-fabric-get-started-containers.md#configure-and-set-environment-variables)|[Manifest van container toepassing](service-fabric-manifest-example-container-app.md#application-manifest)|
|[Poort-naar-host-toewijzing van container configureren](service-fabric-get-started-containers.md#configure-container-port-to-host-port-mapping-and-container-to-container-discovery)| [Manifest van container toepassing](service-fabric-manifest-example-container-app.md#application-manifest)|
|[Verificatie van container register configureren](service-fabric-get-started-containers.md#configure-container-registry-authentication)|[Manifest van container toepassing](service-fabric-manifest-example-container-app.md#application-manifest)|
|[Isolatie modus instellen](service-fabric-get-started-containers.md#configure-isolation-mode)|[Manifest van container toepassing](service-fabric-manifest-example-container-app.md#application-manifest)|
|[Specifieke container installatie kopieën voor het besturings systeem opgeven](service-fabric-get-started-containers.md#specify-os-build-specific-container-images)|[Manifest van container toepassing](service-fabric-manifest-example-container-app.md#application-manifest)|
|[Omgevings variabelen instellen](service-fabric-get-started-containers.md#configure-and-set-environment-variables)|[Container FrontEndService service manifest](service-fabric-manifest-example-container-app.md#frontendservice-service-manifest), [Container BackEndService service manifest](service-fabric-manifest-example-container-app.md#backendservice-service-manifest)|
|[Een eind punt configureren](service-fabric-get-started-containers.md#configure-communication)|[Container FrontEndService service manifest](service-fabric-manifest-example-container-app.md#frontendservice-service-manifest), [Container BackEndService service manifest](service-fabric-manifest-example-container-app.md#backendservice-service-manifest), [VotingData service manifest](service-fabric-manifest-example-reliable-services-app.md#votingdata-service-manifest)|
|opdrachten door geven aan de container|[Container FrontEndService-service manifest](service-fabric-manifest-example-container-app.md#frontendservice-service-manifest)|
|[Een certificaat importeren in een container](service-fabric-securing-containers.md)|[Container FrontEndService-service manifest](service-fabric-manifest-example-container-app.md#frontendservice-service-manifest)|
|[Volume stuur programma configureren](service-fabric-containers-volume-logging-drivers.md)|[Container BackEndService-service manifest](service-fabric-manifest-example-container-app.md#backendservice-service-manifest)|

