---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 08/14/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 93f6bc8533218af7f0e6dcd1c5f7be6fe8c00e29
ms.sourcegitcommit: 0e59368513a495af0a93a5b8855fd65ef1c44aac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/15/2019
ms.locfileid: "69520830"
---
[!INCLUDE [P2S FAQ All](vpn-gateway-faq-p2s-all-include.md)]

### <a name="can-i-use-my-own-internal-pki-root-ca-to-generate-certificates-for-point-to-site-connectivity"></a>Kan ik mijn eigen interne PKI-basis certificerings instantie gebruiken voor het genereren van certificaten voor punt-naar-site-connectiviteit?

Ja. Voorheen konen alleen zelfondertekende basiscertificaten worden gebruikt. U kunt nog steeds 20 basiscertificaten uploaden.

### <a name="can-i-use-certificates-from-azure-key-vault"></a>Kan ik certificaten van Azure Key Vault gebruiken?

Nee.

### <a name="what-tools-can-i-use-to-create-certificates"></a>Welke hulpprogramma's kan ik gebruiken om certificaten te maken?

U kunt uw Enterprise PKI-oplossing (uw interne PKI), Azure PowerShell, MakeCert en OpenSSL gebruiken.

### <a name="certsettings"></a>Zijn er instructies voor het instellen van het certificaat en de parameters?

* **Interne PKI/Enter prise PKI-oplossing:** Zie de stappen voor het [genereren van certificaten](../articles/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md#generatecert).

* **Azure PowerShell:** Raadpleeg het [Azure PowerShell](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md) -artikel voor stappen.

* **MakeCert:** Zie het artikel [makecert](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site-makecert.md) voor stappen.

* **OpenSSL:** 

    * Bij het exporteren van certificaten, moet u het basiscertificaat naar Base64 converteren.

    * Voor het clientcertificaat:

      * Bij het maken van de persoonlijke sleutel, moet u de lengte 4096 opgeven.
      * Bij het maken van het certificaat moet u voor de paramter *-extensies* de waarde *usr_cert* opgeven.
