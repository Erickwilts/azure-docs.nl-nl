---
title: Het configureren van eenmalige aanmelding tot een toepassing toepassingsproxy | Microsoft Docs
description: Het configureren van eenmalige aanmelding voor uw toepassing proxytoepassing snel
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/12/2019
ms.author: mimart
ms.reviewer: japere, asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: f26b28b34a569673b397fa4700c5332c3550500f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65825871"
---
# <a name="how-to-configure-single-sign-on-to-an-application-proxy-application"></a>Single sign-on bij een toepassing Application Proxy configureren

Eenmalige aanmelding (SSO) kunt uw gebruikers toegang krijgen tot een toepassing zonder verificatie van meerdere keren. Dit kunt de één verificatie kan worden uitgevoerd in de cloud, op basis van Azure Active Directory, en de service of de Connector om de gebruiker voor het voltooien van elke aanvullende verificatie uitdagingen van de toepassing te imiteren.

## <a name="how-to-configure-single-sign-on"></a>Het configureren van eenmalige aanmelding op
Zorg ervoor dat uw toepassing is geconfigureerd voor pre-authenticatie via Azure Active Directory voor het configureren van eenmalige aanmelding. Als u wilt doen deze configuratie, gaat u naar **Azure Active Directory**  - &gt; **bedrijfstoepassingen**  - &gt; **alle toepassingen**   - &gt; Uw toepassing  **- &gt; Application Proxy**. Op deze pagina kunt u zien van het veld 'Vooraf-verificatie' en zorg ervoor dat is ingesteld op 'Azure Active Directory. 

Zie voor meer informatie over de methoden voor verificatie vooraf, stap 4 van de [app publishing document](application-proxy-add-on-premises-application.md).

   ![Methode voor verificatie vooraf in Azure portal](./media/application-proxy-config-sso-how-to/app-proxy.png)

## <a name="configuring-single-sign-on-modes-for-application-proxy-applications"></a>Modi voor eenmalige aanmelding configureren voor Application Proxy-toepassingen
Het specifieke type van eenmalige aanmelding configureren. De aanmeldings-methoden zijn ingedeeld op basis van de back-endtoepassing maakt gebruik van welk type verificatie. App-Proxy toepassingen ondersteuning bieden voor drie typen van aanmelding:

-   **Op basis van wachtwoorden Sign-On**: Wachtwoord gebaseerde aanmelding kan worden gebruikt voor elke toepassing die gebruikmaakt van velden gebruikersnaam en wachtwoord aan te melden. Configuratiestappen zijn [wachtwoord eenmalige aanmelding configureren voor een toepassing in Azure AD-galerie](configure-password-single-sign-on-gallery-applications.md).

-   **Geïntegreerde Windows-authenticatie**: Voor toepassingen met behulp van geïntegreerde Windows-verificatie (IWA), is eenmalige aanmelding ingeschakeld via Kerberos-beperkte delegatie (KCD). Deze methode biedt machtiging Toepassingsproxyconnectors in Active Directory te imiteren van gebruikers, en voor het verzenden en ontvangen van tokens in hun naam. Informatie over het configureren van KCD kunnen u vinden in de [eenmalige aanmelding met KCD documentatie](application-proxy-configure-single-sign-on-with-kcd.md).

-   **Headers gebaseerde aanmelding**: Headers gebaseerde aanmelding wordt ingeschakeld via een partnerschap en enige aanvullende configuratie vereist. Zie voor meer informatie over de samenwerking en stapsgewijze instructies voor het configureren van eenmalige aanmelding tot een toepassing die gebruikmaakt van headers voor verificatie, de [PingAccess voor Azure AD-documentatie](application-proxy-configure-single-sign-on-with-ping-access.md).

-   **Eenmalige aanmelding SAML**: Met SAML eenmalige aanmelding verifieert Azure AD met behulp van Azure AD-account van de gebruiker naar de toepassing. Azure AD communiceert de informatie aanmelding voor de toepassing via een verbindingsprotocol. Met SAML gebaseerde eenmalige aanmelding, kunt u gebruikers toewijzen aan de rollen van de specifieke toepassing is op basis van regels die u in de SAML-claims definieert. Zie voor meer informatie over het instellen van eenmalige aanmelding SAML [SAML voor eenmalige aanmelding met Application Proxy](application-proxy-configure-single-sign-on-on-premises-apps.md).

Elk van deze opties kunt u vinden door te gaan naar uw toepassing in 'Enterprise Application' en openen van de **Single Sign-On** pagina in het menu links. Houd er rekening mee dat als uw toepassing in de oude portal is gemaakt, u niet al deze opties ziet mogelijk.

Op deze pagina ook ziet u een extra optie voor eenmalige aanmelding: Gekoppelde aanmelding. Deze optie wordt ook ondersteund door de toepassingsproxy. Deze optie wordt echter niet toegevoegd eenmalige aanmelding voor de toepassing. Dat gezegd dat de toepassing mogelijk al eenmalige aanmelding geïmplementeerd met behulp van een andere service, zoals Active Directory Federation Services. 

Deze optie kunt een beheerder een koppeling maken naar een toepassing die gebruikers eerste land op bij het openen van de toepassing. Bijvoorbeeld, als er een toepassing die is geconfigureerd voor het verifiëren van gebruikers met behulp van Active Directory Federation Services 2.0, kunt een beheerder de optie ' gekoppelde aanmelding ' gebruiken om te maken van een koppeling naar het in het toegangsvenster.

## <a name="next-steps"></a>Volgende stappen
- [Wachtwoord voor eenmalige aanmelding met Application Proxy vaulting](application-proxy-configure-single-sign-on-password-vaulting.md)
- [Beperkte Kerberos-delegering voor eenmalige aanmelding met Application Proxy](application-proxy-configure-single-sign-on-with-kcd.md)
- [Koptekst gebaseerde verificatie voor eenmalige aanmelding met Application Proxy](application-proxy-configure-single-sign-on-with-ping-access.md) 
- [SAML voor eenmalige aanmelding met Application Proxy](application-proxy-configure-single-sign-on-on-premises-apps.md).
