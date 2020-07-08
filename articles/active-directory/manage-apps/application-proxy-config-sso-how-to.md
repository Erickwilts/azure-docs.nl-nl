---
title: Eenmalige aanmelding configureren voor een toepassings proxy-app
description: Hoe u eenmalige aanmelding voor uw toepassings proxy toepassing snel kunt configureren
services: active-directory
documentationcenter: ''
author: kenwith
manager: celestedg
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 03/12/2019
ms.author: kenwith
ms.reviewer: japere, asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: bb3d1d921be858531372130264e777f8c04a9328
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2020
ms.locfileid: "84764941"
---
# <a name="how-to-configure-single-sign-on-to-an-application-proxy-application"></a>Eenmalige aanmelding configureren voor een toepassings proxy toepassing

Met eenmalige aanmelding (SSO) kunnen uw gebruikers toegang krijgen tot een toepassing zonder dat er meerdere keren worden geverifieerd. Hiermee kan de ene verificatie worden uitgevoerd in de Cloud, op basis van Azure Active Directory, en kan de service of connector de gebruiker imiteren om aanvullende verificatie uitdagingen van de toepassing te volt ooien.

## <a name="how-to-configure-single-sign-on"></a>Eenmalige aanmelding configureren
Als u SSO wilt configureren, moet u eerst controleren of uw toepassing is geconfigureerd voor verificatie vooraf via Azure Active Directory. Als u deze configuratie wilt uitvoeren, gaat u naar **Azure Active Directory**  - &gt; **Enter prise-toepassingen**  - &gt; **alle toepassingen** van  - &gt; de toepassings ** - &gt; proxy**van uw toepassing. Op deze pagina ziet u het veld pre-verificatie en zorgt u ervoor dat is ingesteld op Azure Active Directory. 

Zie stap 4 van het [app-publicatie document](application-proxy-add-on-premises-application.md)voor meer informatie over de pre-verificatie methoden.

   ![Methode voor verificatie vooraf in Azure Portal](./media/application-proxy-config-sso-how-to/app-proxy.png)

## <a name="configuring-single-sign-on-modes-for-application-proxy-applications"></a>Modus voor eenmalige aanmelding configureren voor toepassings proxy toepassingen
Configureer het specifieke type eenmalige aanmelding. De aanmeldings methoden worden geclassificeerd op basis van het type verificatie dat door de back-end-toepassing wordt gebruikt. App-proxy toepassingen ondersteunen drie typen aanmelding:

-   **Aanmelden op basis van wacht woorden**: aanmelden op basis van wacht woorden kan worden gebruikt voor elke toepassing die gebruikmaakt van gebruikers naam en wacht woord om zich aan te melden. Configuratie stappen zijn [een eenmalige aanmelding met een wacht woord configureren voor een toepassing in de Azure AD-galerie](configure-password-single-sign-on-non-gallery-applications.md).

-   **Geïntegreerde Windows-verificatie**: voor toepassingen die gebruikmaken van geïntegreerde Windows-authenticatie (IWA), wordt eenmalige aanmelding ingeschakeld via beperkte Kerberos-delegering (KCD). Deze methode biedt toepassings proxy connectors machtiging in Active Directory om gebruikers te imiteren en om namens hen tokens te verzenden en ontvangen. Meer informatie over het configureren van KCD vindt u in de [eenmalige aanmelding met KCD-documentatie](application-proxy-configure-single-sign-on-with-kcd.md).

-   **Op headers gebaseerde aanmelding**: aanmelden op basis van een header is ingeschakeld via een partnerschap en vereist een aantal aanvullende configuratie. Zie de [PingAccess voor Azure AD-documentatie](application-proxy-configure-single-sign-on-with-ping-access.md)voor meer informatie over de samen werking en stapsgewijze instructies voor het configureren van eenmalige aanmelding voor een toepassing die gebruikmaakt van headers voor authenticatie.

-   **Eenmalige aanmelding via SAML**: met eenmalige aanmelding via SAML verifieert Azure AD bij de toepassing met behulp van het Azure ad-account van de gebruiker. Azure AD communiceert de aanmeldings gegevens met de toepassing via een verbindings protocol. Met eenmalige aanmelding op basis van SAML kunt u gebruikers toewijzen aan specifieke toepassings rollen op basis van de regels die u in uw SAML-claims definieert. Zie voor meer informatie over het instellen van eenmalige SAML-aanmelding [SAML voor eenmalige aanmelding met toepassings proxy](application-proxy-configure-single-sign-on-on-premises-apps.md).

U kunt elk van deze opties vinden door naar uw toepassing te gaan in ' bedrijfs toepassingen ' en de pagina voor **eenmalige aanmelding** te openen in het menu links. Als uw toepassing is gemaakt in de oude Portal, ziet u mogelijk niet al deze opties.

Op deze pagina ziet u ook een aanvullende aanmeldings optie: gekoppelde aanmelding. Deze optie wordt ook ondersteund door de toepassings proxy. Met deze optie wordt echter niet eenmalige aanmelding toegevoegd aan de toepassing. Die zei dat de toepassing mogelijk al een eenmalige aanmelding heeft geïmplementeerd met een andere service, zoals Active Directory Federation Services. 

Met deze optie kan een beheerder een koppeling maken naar een toepassing waar gebruikers zich voor het eerst bevinden bij het openen van de toepassing. Als er bijvoorbeeld een toepassing is die is geconfigureerd om gebruikers te verifiëren met behulp van Active Directory Federation Services 2,0, kan een beheerder de optie ' gekoppelde aanmelding ' gebruiken om een koppeling te maken naar het toegangs venster.

## <a name="next-steps"></a>Volgende stappen
- [Wachtwoord kluis voor eenmalige aanmelding met toepassings proxy](application-proxy-configure-single-sign-on-password-vaulting.md)
- [Beperkte Kerberos-overdracht voor eenmalige aanmelding met toepassings proxy](application-proxy-configure-single-sign-on-with-kcd.md)
- [Verificatie op basis van een header voor eenmalige aanmelding met toepassings proxy](application-proxy-configure-single-sign-on-with-ping-access.md) 
- [SAML voor eenmalige aanmelding met toepassings proxy](application-proxy-configure-single-sign-on-on-premises-apps.md).
