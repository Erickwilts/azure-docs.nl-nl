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
ms.openlocfilehash: 52084b065ef65a69a6691b6646d1e199f011910d
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67175870"
---
### <a name="gwipnoconnection"></a> Wijzigen van het lokale netwerk gateway IP-adres - geen gatewayverbinding

Gebruik het voorbeeld om een lokale netwerkgateway die geen gatewayverbinding heeft te wijzigen. Wanneer u deze waarde wijzigt, kunt u tegelijkertijd ook de adresvoorvoegsels wijzigen.

1. Op de lokale netwerkgatewayresource in de **instellingen** sectie, klikt u op **configuratie**.
2. In de **IP-adres** wijzigt u het IP-adres.
3. Klik op **Opslaan** om de instellingen op te slaan.

### <a name="gwipwithconnection"></a>Op het lokale netwerk gateway IP-adres wijzigen - bestaande gatewayverbinding

Als u wilt wijzigen in een lokale netwerkgateway die verbonden is, moet u eerst de verbinding verwijderen. Nadat de verbinding is verwijderd, kunt u het IP-adres van de gateway wijzigen en een nieuwe verbinding maken. U kunt tegelijkertijd ook de adresvoorvoegsels wijzigen. Dit veroorzaakt enige downtime in uw VPN-verbinding. Als u het IP-adres van de gateway wijzigt, hoeft u de VPN-gateway niet te verwijderen. U hoeft alleen de verbinding te verwijderen.
 
#### <a name="1-remove-the-connection"></a>1. Verwijder de verbinding.

1. Op de lokale netwerkgatewayresource in de **instellingen** sectie, klikt u op **verbindingen**.
2. Klik op de **...**  op de regel voor de verbinding en klik vervolgens op **verwijderen**.
3. Klik op **opslaan** uw instellingen op te slaan.

#### <a name="2-modify-the-ip-address"></a>2. Wijzig het IP-adres.

U kunt tegelijkertijd ook de adresvoorvoegsels wijzigen.

1. In de **IP-adres** wijzigt u het IP-adres.
2. Klik op **Opslaan** om de instellingen op te slaan.

#### <a name="3-recreate-the-connection"></a>3. Maak opnieuw verbinding.

1. Navigeer naar de virtuele netwerkgateway voor uw VNet. (Niet de lokale netwerkgateway.)
2. Op de Gateway van het virtuele netwerk, in de **instellingen** sectie, klikt u op **verbindingen**.
3. Klik op de **+ toevoegen** openen de **verbinding toevoegen** blade.
4. Uw verbinding opnieuw maakt.
5. Klik op **OK** om de verbinding te maken.