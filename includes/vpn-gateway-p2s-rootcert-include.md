---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 12/11/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 4c8e7e5272f180c482ca7fdd44302f49eb888b25
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66157300"
---
Gebruik een basiscertificaat dat is gegenereerd met een commerciële oplossing (aanbevolen) of genereer een zelfondertekend certificaat. Nadat u het basiscertificaat hebt gemaakt, exporteert u de gegevens van het openbare certificaat (niet de persoonlijke sleutel) als een X.509 CER-bestand dat is gecodeerd met Base64. Vervolgens uploadt u de gegevens van het openbare certificaat naar de Azure-server.

* **Commercieel certificaat** Als u een bedrijfsoplossing gebruikt, kunt u de bestaande certificaatketen gebruiken. Haal het CER-bestand op dat u wilt gebruiken voor het basiscertificaat.
* **Zelfondertekend basiscertificaat**: Als u geen commerciële certificeringsoplossing gebruikt, maakt u een zelfondertekend basiscertificaat. Anders zijn de certificaten die u maakt niet compatibel met uw P2S-verbindingen en treedt er voor clients een verbindingsfout op tijdens het verbinden. U kunt Azure PowerShell, MakeCert of OpenSSL gebruiken. In de stappen in de volgende artikelen wordt beschreven hoe u een compatibel, zelfondertekend basiscertificaat genereert:

  * [Instructies voor Windows 10 PowerShell](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md): Bij deze instructies zijn Windows 10 en PowerShell vereist voor het genereren van certificaten. Clientcertificaten die worden gegenereerd op basis van het basiscertificaat kunnen op alle ondersteunde P2S-clients worden ondersteund.
  * [Instructies voor MakeCert](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site-makecert.md): Gebruik MakeCert als u geen toegang hebt tot een Windows 10-computer om certificaten te genereren. MakeCert is inmiddels afgeschaft, maar u kunt het nog wel gebruiken om certificaten te genereren. Clientcertificaten die u genereert op basis van het basiscertificaat kunnen worden geïnstalleerd op alle ondersteunde P2S-clients.
  * [Instructies voor Linux](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site-linux.md)
