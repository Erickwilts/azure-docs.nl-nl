---
title: Azure MFA server-webservice voor mobiele apps-Azure Active Directory
description: Configureer de MFA-server om pushmeldingen te verzenden aan gebruikers met de Microsoft Authenticator App.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 07/11/2018
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 785c4ccf591ce6a7a50a3d144fb1740121c73ce6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2020
ms.locfileid: "80653195"
---
# <a name="enable-mobile-app-authentication-with-azure-multi-factor-authentication-server"></a>Authenticatie met de mobiele app integreren met Azure Multi-Factor Authentication-server

De Microsoft verificator-app biedt een extra optie voor verificatie buiten de band. In plaats van een geautomatiseerde telefoon oproep of SMS-bericht naar de gebruiker tijdens het aanmelden te plaatsen, pusht Azure Multi-Factor Authentication een melding naar de Microsoft Authenticator-app op de smartphone of Tablet van de gebruiker. De gebruiker tikt alleen op **verificatie** (of voert een pincode in en tikt op "verifiëren") in de app om de aanmelding te volt ooien.

Het gebruik van een mobiele app voor verificatie in twee stappen wordt aanbevolen wanneer het telefonische bereik onbetrouwbaar is. Als u de app als een OATH-tokengenerator gebruikt, hebt u geen netwerk- of internetverbinding nodig.

> [!IMPORTANT]
> Met ingang van 1 juli 2019 biedt micro soft geen MFA-server meer voor nieuwe implementaties. Nieuwe klanten die multi-factor Authentication van hun gebruikers willen vereisen, moeten gebruikmaken van Azure Multi-Factor Authentication op basis van de Cloud. Bestaande klanten die MFA-server voorafgaand aan 1 juli hebben geactiveerd, kunnen de nieuwste versie downloaden, toekomstige updates en activerings referenties genereren.

> [!IMPORTANT]
> Als u Microsoft Azure Multi-Factor Authentication-serverv8.x of hoger hebt geïnstalleerd, zijn de meeste van de onderstaande stappen niet vereist. Verificatie voor mobiele apps kan worden ingesteld door de stappen onder [De mobiele app configureren](#configure-the-mobile-app-settings-in-the-azure-multi-factor-authentication-server) te volgen.

## <a name="requirements"></a>Vereisten

Voor het gebruik van de Microsoft Authenticator-app moet u Microsoft Azure Multi-Factor Authentication-server 8.x of hoger gebruiken

## <a name="configure-the-mobile-app-settings-in-the-azure-multi-factor-authentication-server"></a>De instellingen voor de webservice voor mobiele apps op de Azure Multi-Factor Authentication-server configureren

1. Klik in de console van de Multi-Factor Authentication-server op het pictogram Gebruikersportal. Als gebruikers zelf een verificatiemethode mogen kiezen, schakelt u op het tabblad Instellingen onder **Toestaan dat gebruikers de methode selecteren** de optie **Mobiele app** in. Als deze functie niet is ingeschakeld, moeten eindgebruikers contact opnemen met uw helpdesk om de activering van de mobiele app te voltooien.
2. Schakel het selectievakje **Toestaan dat gebruikers de mobiele app activeren** in.
3. Schakel het selectievakje **Registreren van gebruikers toestaan** in.
4. Klik op het pictogram **mobiele app** .
5. Voer in het veld **Accountnaam** de naam van het bedrijf of de organisatie in die moet worden weergegeven in de mobiele toepassing voor dit account.
   ![Configuratie van MFA-server - instellingen voor mobiele app](./media/howto-mfaserver-deploy-mobileapp/mobile.png)

## <a name="next-steps"></a>Volgende stappen

- [Geavanceerde scenario's met Azure multi-factor Authentication en vpn's van](howto-mfaserver-nps-vpn.md)derden.
