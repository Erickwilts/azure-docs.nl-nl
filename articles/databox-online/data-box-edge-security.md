---
title: Azure Data Box-Edge-beveiliging | Microsoft Docs
description: Beschrijving van de beveiliging en privacy-functies die bescherming van uw Azure Data Box-Edge-apparaat, service en gegevens on-premises en in de cloud.
services: Data Box Edge
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 04/15/2019
ms.author: alkohli
ms.openlocfilehash: 8823aebe17a5446b3c507878833c2525c338dde1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64718004"
---
# <a name="azure-data-box-edge-security-and-data-protection"></a>Azure Data Box-Edge-beveiliging en gegevensbescherming

Beveiliging is van groot belang bij het u bij het kiezen van een nieuwe technologie, met name als de technologie wordt gebruikt met vertrouwelijke of geheime gegevens. Azure Data Box Edge kunt u ervoor zorgen dat alleen entiteiten gemachtigde kunnen weergeven, wijzigen of verwijderen van uw gegevens.

Dit artikel beschrijft de gegevens in het Edge-beveiligingsfuncties die helpen beschermen van elk van de onderdelen van de oplossing en de gegevens die erin zijn opgeslagen.

Azure Data Box Edge bestaat uit vier hoofdonderdelen die met elkaar communiceren:

- **Gegevens in het Edge-service, die wordt gehost in Azure**. De management-resource die u gebruiken voor het maken van de volgorde van de apparaten, het apparaat configureren en de volgorde te volgen tot voltooiing.
- **Gegevens in het Edge-apparaat**. De overdrachtsapparaat dat aan u wordt geleverd, zodat u uw on-premises gegevens in Azure importeren kunt.
- **Clients /-hosts die zijn verbonden met het apparaat**. De clients in uw infrastructuur die verbinding maken met de gegevens in het Edge-apparaat en die gegevens bevatten die moet worden beveiligd.
- **Cloudopslag**. De locatie van het Azure-cloud-platform waar gegevens worden opgeslagen. Deze locatie is meestal het opslagaccount dat is gekoppeld aan de gegevens in het Edge-resource die u maakt.

## <a name="data-box-edge-service-protection"></a>De beveiliging van gegevens in het Edge-service

De gegevens in het Edge-service is een management-service die wordt gehost in Azure. De service wordt gebruikt voor het configureren en beheren van het apparaat.

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-service-protection.md)]

## <a name="data-box-edge-device-protection"></a>Bescherming van gegevens in het Edge-apparaat

De gegevens in het Edge-apparaat is een on-premises-apparaat waarmee u uw gegevens transformeren kunt met lokale verwerking en vervolgens te verzenden naar Azure. Uw apparaat:

- Moet een activeringscode voor toegang tot de gegevens in het Edge-service.
- Wordt beveiligd op altijd door het wachtwoord van een apparaat.
- Een apparaat vergrendeld is. Het apparaat BMC en BIOS zijn beveiligd met een wachtwoord. Het BIOS wordt beveiligd door de gebruikerstoegang is beperkt.
- Beveiligd opstarten is ingeschakeld.
- Wordt uitgevoerd Windows Defender Device Guard. Device Guard kunt u alleen vertrouwde toepassingen die u in uw code-integriteitsbeleidsregels definieert uitvoeren.

### <a name="protect-the-device-via-activation-key"></a>Het apparaat via de activeringscode beveiligen

Alleen een geautoriseerde gegevens in het Edge-apparaat is toegestaan om toe te voegen van de gegevens in het Edge-service die u in uw Azure-abonnement maken. Als u wilt toestaan dat een apparaat, moet u een activeringscode gebruiken om te activeren van het apparaat met de gegevens in het Edge-service.

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-activation-key.md)]

Zie voor meer informatie, [ophalen van een activeringscode](data-box-edge-deploy-prep.md#get-the-activation-key).

### <a name="protect-the-device-via-password"></a>Het apparaat via wachtwoord beveiligen

Wachtwoorden ervoor te zorgen dat alleen geautoriseerde gebruikers toegang uw gegevens tot kunnen. Gegevens in het Edge-apparaten opstart in een vergrendelde status.

U kunt:

- Verbinding maken met de lokale web-UI van het apparaat via een browser en geef vervolgens een wachtwoord voor aanmelding bij het apparaat.
- Op afstand verbinding maken met de PowerShell-interface van het apparaat via HTTP. Extern beheer is standaard ingeschakeld. Vervolgens kunt u het wachtwoord van het apparaat te melden bij het apparaat opgeven. Zie voor meer informatie, [verbinding maken met uw gegevens in het Edge-apparaat op afstand](data-box-edge-connect-powershell-interface.md#connect-to-the-powershell-interface).

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-password-best-practices.md)]
- Gebruik de lokale web-UI te [het wachtwoord wijzigen](data-box-edge-manage-access-power-connectivity-mode.md#manage-device-access). Als u het wachtwoord wijzigt, zorg er dan voor dat alle externe toegang-gebruikers een melding ontvangen, zodat ze niet beschikken over problemen met aanmelden.

## <a name="protect-your-data"></a>Beveilig uw gegevens

Deze sectie beschrijft de gegevens in het Edge-beveiligingsfuncties die in-transit en opgeslagen gegevens te beschermen.

### <a name="protect-data-at-rest"></a>Gegevens in rust beveiligen

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-data-rest.md)]

### <a name="protect-data-in-flight"></a>Beveiligen van gegevens tijdens de overdracht

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-data-flight.md)]

### <a name="protect-data-via-storage-accounts"></a>Bescherm gegevens via de storage-accounts

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-protect-data-storage-accounts.md)]
- Draaien en vervolgens [uw opslagaccountsleutels synchroniseren](data-box-edge-manage-shares.md#sync-storage-keys) regelmatig om uw storage-account beschermen tegen onbevoegde gebruikers.

## <a name="manage-personal-information"></a>Persoonlijke gegevens beheren

De gegevens in het Edge-service worden verzameld van persoonlijke gegevens in de volgende scenario's:

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-manage-personal-data.md)]

Volg de stappen in de lijst van gebruikers die toegang tot of verwijderen van een share wilt weergeven, [beheren van bestandsshares aan de rand van gegevens in het](data-box-edge-manage-shares.md).

Voor meer informatie, raadpleegt u het privacybeleid van Microsoft op de [Trust Center](https://www.microsoft.com/trustcenter).

## <a name="next-steps"></a>Volgende stappen

[Uw gegevens in het Edge-apparaat implementeren](data-box-edge-deploy-prep.md)
