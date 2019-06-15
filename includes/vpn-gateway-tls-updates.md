---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 07/30/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: e4d20cd39d2a843ee1ab57a412ac668b3495fdb1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66147065"
---
>[!NOTE]
>Vanaf 1 juli 2018 is ondersteuning voor TLS 1.0 en 1.1 uit Azure VPN Gateway verwijderd. VPN Gateway ondersteunt alleen TLS 1.2. Om ondersteuning te behouden, Zie de [updates voor het inschakelen van ondersteuning voor TLS1.2](#tls1).

Bovendien worden de volgende oudere algoritmen ook afgeschaft voor TLS vanaf 1 juli 2018:

* RC4 (Rivest Cipher 4)
* DES (Data Encryption Algorithm)
* 3DES (Triple Data Encryption Algorithm)
* MD5 (Message Digest 5)

### <a name="tls1"></a>Hoe schakel ik ondersteuning voor TLS 1.2 in Windows 7 en Windows 8.1?

[!INCLUDE [tls 1.2](vpn-gateway-tls-include.md)]
