---
title: Azure VMware-oplossing op CloudSimple-service
description: Meer informatie over de CloudSimple-service met zijn overzicht. Door de service te maken, kunt u knoop punten aanschaffen, knoop punten reserveren en persoonlijke clouds maken.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: ec77209c0995bf5aec0957c8d4d2269f27d743bc
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "88142125"
---
# <a name="cloudsimple-service-overview"></a>Overzicht van de CloudSimple-service

Met de CloudSimple-service kunt u Azure VMware-oplossing gebruiken door CloudSimple.  Door de service te maken, kunt u knoop punten aanschaffen, knoop punten reserveren en persoonlijke clouds maken.  U maakt de CloudSimple-service in elke Azure-regio waar de CloudSimple-service beschikbaar is. De service definieert het Edge-netwerk van de Azure VMware-oplossing door CloudSimple. Het Edge-netwerk ondersteunt services die VPN, ExpressRoute en Internet connectiviteit met uw persoonlijke Clouds bevatten.

## <a name="gateway-subnet"></a>Gatewaysubnet

Een gateway-subnet is vereist per CloudSimple-service en is uniek voor de regio waarin het is gemaakt. Het gateway-subnet wordt gebruikt bij het maken van het Edge-netwerk en vereist een/28 CIDR-blok.  De adres ruimte van het gateway-subnet moet uniek zijn. Het mag niet overlappen met een netwerk dat communiceert met de CloudSimple-omgeving. De netwerken die communiceren met CloudSimple zijn onder andere on-premises netwerken en een virtueel Azure-netwerk.  Een gateway-subnet kan niet worden verwijderd nadat het is gemaakt.  Het gateway-subnet wordt verwijderd wanneer de service wordt verwijderd.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het [maken van een CloudSimple-service in azure](quickstart-create-cloudsimple-service.md).
