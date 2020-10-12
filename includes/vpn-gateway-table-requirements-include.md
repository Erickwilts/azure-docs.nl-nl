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
ms.openlocfilehash: dc018b5d09c9b33c10cd2d54ac6572537e05ed25
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "67175922"
---
De volgende tabel bevat de vereisten voor VPN-gateways op basis van beleid en route. Deze tabel is van toepassing op de Resource Manager en de klassieke implementatiemodellen. Voor het klassieke model zijn op beleid gebaseerde VPN-gateways hetzelfde als statische gateways. Op route gebaseerde gateways zijn hetzelfde als dynamische gateways.

|  | **Op beleid gebaseerde Basic VPN-gateway** | **Op route gebaseerde Basic VPN-gateway** | **Op route gebaseerde Standard VPN-gateway** | **Op route gebaseerde High Performance VPN-gateway** |
| --- | --- | --- | --- | --- |
| **Site-naar-site-connectiviteit (S2S)** |VPN-configuratie op basis van beleid |VPN-configuratie op basis van route |VPN-configuratie op basis van route |VPN-configuratie op basis van route |
| **Punt-naar-site-verbinding (P2S**) |Niet ondersteund |Ondersteund (kan tegelijk bestaan met S2S) |Ondersteund (kan tegelijk bestaan met S2S) |Ondersteund (kan tegelijk bestaan met S2S) |
| **Verificatiemethode** |Vooraf gedeelde sleutel |Vooraf gedeelde sleutel voor S2S-connectiviteit, certificaten voor P2S-connectiviteit |Vooraf gedeelde sleutel voor S2S-connectiviteit, certificaten voor P2S-connectiviteit |Vooraf gedeelde sleutel voor S2S-connectiviteit, certificaten voor P2S-connectiviteit |
| **Maximumaantal S2S-verbindingen** |1 |10 |10 |30 |
| **Maximumaantal P2S-verbindingen** |Niet ondersteund |128 |128 |128 |
| **Ondersteuning voor actieve routering (BGP)** (*) |Niet ondersteund |Niet ondersteund |Ondersteund |Ondersteund |

  (*) BGP wordt niet ondersteund voor het klassieke implementatiemodel.
