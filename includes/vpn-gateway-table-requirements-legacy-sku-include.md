---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 5b9e036816aa532d32b1b4305ef6ae646ae05bae
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "67175924"
---
De volgende tabel bevat de vereisten voor PolicyBased-en RouteBased-VPN-gateways. Deze tabel is van toepassing op de Resource Manager en de klassieke implementatiemodellen. Voor het klassieke model zijn PolicyBased VPN-gateways hetzelfde als statische gateways en gateways op basis van routes zijn hetzelfde als dynamische gateways.

|  | **PolicyBased Basic VPN Gateway** | **RouteBased Basic VPN Gateway** | **RouteBased standaard VPN Gateway** | **RouteBased High Performance VPN Gateway** |
| --- | --- | --- | --- | --- |
| **Site-naar-site-connectiviteit (S2S)** |VPN-configuratie PolicyBased |VPN-configuratie RouteBased |VPN-configuratie RouteBased |VPN-configuratie RouteBased |
| **Punt-naar-site-verbinding (P2S**) |Niet ondersteund |Ondersteund (kan tegelijk bestaan met S2S) |Ondersteund (kan tegelijk bestaan met S2S) |Ondersteund (kan tegelijk bestaan met S2S) |
| **Verificatiemethode** |Vooraf gedeelde sleutel |Vooraf gedeelde sleutel voor S2S-connectiviteit, certificaten voor P2S-connectiviteit |Vooraf gedeelde sleutel voor S2S-connectiviteit, certificaten voor P2S-connectiviteit |Vooraf gedeelde sleutel voor S2S-connectiviteit, certificaten voor P2S-connectiviteit |
| **Maximumaantal S2S-verbindingen** |1 |10 |10 |30 |
| **Maximumaantal P2S-verbindingen** |Niet ondersteund |128 |128 |128 |
| **Ondersteuning voor actieve routering (BGP)** |Niet ondersteund |Niet ondersteund |Ondersteund |Ondersteund |
