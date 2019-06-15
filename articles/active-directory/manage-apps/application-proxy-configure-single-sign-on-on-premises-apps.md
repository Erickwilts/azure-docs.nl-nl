---
title: SAML eenmalige aanmelding voor on-premises toepassingen met Azure Active Directory Application Proxy (Preview) | Microsoft Docs
description: Informatie over het bieden van eenmalige aanmelding voor on-premises toepassingen worden gepubliceerd via toepassingsproxy die zijn beveiligd met SAML-verificatie.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: mimart
ms.reviewer: japere
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 907cb598d708bfa26f53d2e43fef5456258c21b1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66393036"
---
# <a name="saml-single-sign-on-for-on-premises-applications-with-application-proxy-preview"></a>SAML eenmalige aanmelding voor on-premises toepassingen met Application Proxy (Preview)

U kunt eenmalige aanmelding (SSO) opgeven voor on-premises toepassingen die zijn beveiligd met SAML-verificatie en externe toegang tot deze toepassingen via Application Proxy. Met SAML eenmalige aanmelding verifieert Azure Active Directory (Azure AD) met behulp van Azure AD-account van de gebruiker naar de toepassing. Azure AD communiceert de informatie aanmelding voor de toepassing via een verbindingsprotocol. U kunt gebruikers ook toewijzen aan de rollen van de specifieke toepassing is op basis van regels die u in de SAML-claims definieert. Doordat de toepassingsproxy naast de SAML SSO hebben uw gebruikers externe toegang tot de toepassing en een naadloze ervaring voor eenmalige aanmelding.

De toepassingen worden gebruikt, gebruiken de SAML-tokens die zijn uitgegeven door **Azure Active Directory**. Deze configuratie is niet van toepassing op toepassingen die gebruikmaken van een on-premises id-provider. Voor deze scenario's, het beste controleren [Resources voor het migreren van toepassingen naar Azure AD](migration-resources.md).

SAML SSO met Application Proxy werkt ook met de functie voor SAML-tokenversleuteling. Zie voor meer informatie, [configureren van Azure AD-SAML-tokenversleuteling](howto-saml-token-encryption.md).

## <a name="publish-the-on-premises-application-with-application-proxy"></a>De on-premises toepassing met Application Proxy publiceren

Voordat u eenmalige aanmelding voor on-premises toepassingen bieden kunt, zorg ervoor dat u toepassingsproxy hebt ingeschakeld en u hebt een connector is geïnstalleerd. Zie [toevoegen van een on-premises toepassing voor externe toegang via Application Proxy in Azure AD](application-proxy-add-on-premises-application.md) voor meer informatie over hoe.

Houd rekening met het volgende wanneer u de zelfstudie gaan:

* Publiceer uw toepassing op basis van de instructies in de zelfstudie. Zorg ervoor dat u selecteert **Azure Active Directory** als de **verificatie vooraf** methode voor uw toepassing (stap 4 in [een on-premises app toevoegen aan Azure AD](application-proxy-add-on-premises-application.md#add-an-on-premises-app-to-azure-ad
)).
* Kopieer de **externe URL** voor de toepassing.
* Als een best practice, gebruik van aangepaste domeinen waar mogelijk voor een optimale gebruikerservaring. Meer informatie over [werken met aangepaste domeinen in Azure AD-toepassingsproxy](application-proxy-configure-custom-domain.md).
* Ten minste één gebruiker toevoegen aan de toepassing en zorg ervoor dat de testaccount toegang heeft tot de on-premises toepassing. Met behulp van de test test-account als u de toepassing door naar de pagina bereiken kunt de **externe URL** valideren Application Proxy correct is ingesteld. Zie voor informatie over probleemoplossing, [Application Proxy oplossen van problemen en foutberichten](application-proxy-troubleshoot.md).

## <a name="set-up-saml-sso"></a>Een SAML SSO instellen

1. Selecteer in de Azure portal, **Azure Active Directory > bedrijfstoepassingen** en selecteer de toepassing in de lijst.
1. Uit van de app **overzicht** weergeeft, schakelt **eenmalige aanmelding**.
1. Selecteer **SAML** als de methode voor eenmalige aanmelding.
1. In de **instellen van eenmalige aanmelding met SAML** pagina, bewerken de **SAML-basisconfiguratie** gegevens, en volg de stappen in [Enter basisconfiguratie SAML](configure-single-sign-on-non-gallery-applications.md#saml-based-single-sign-on) configureren op basis van SAML verificatie voor de toepassing.

   * Zorg ervoor dat de **antwoord-URL** komt overeen met de **externe URL** voor de on-premises toepassing die u hebt gepubliceerd via toepassingsproxy of een pad is onder de **externe URL**.
   * Voor een IDP gestart door flow waar uw toepassing een andere vereist **antwoord-URL** voor de SAML-configuratie toevoegen als een **extra** URL in de lijst en het selectievakje is ingeschakeld om aan te geven als de primaire **antwoord-URL**.
   * Zorg ervoor dat de back endtoepassing in de juiste wordt opgegeven voor een Serviceprovider geïnitieerde flow **antwoord-URL** of URL van de Bevestigingsconsumerservice te gebruiken voor het ontvangen van het verificatietoken.

     ![Voer basisgegevens voor SAML-configuratie](./media/application-proxy-configure-single-sign-on-on-premises-apps/basic-saml-configuration.png)

    > [!NOTE]
    > Als de back-endtoepassing wordt verwacht dat de **antwoord-URL** als de interne URL, moet u een van beide gebruikt [aangepaste domeinen](application-proxy-configure-custom-domain.md) hebben die overeenkomt met de interne en externe URL's of de Apps in mijn beveiligde aanmelding-extensie installeren op apparaten van gebruikers. Deze extensie worden automatisch omgeleid naar de juiste Application Proxy-Service. Zie voor het installeren van de extensie [mijn Apps van beveiligde aanmelding extensie](../user-help/my-apps-portal-end-user-access.md#download-and-install-the-my-apps-secure-sign-in-extension).

## <a name="test-your-app"></a>Uw app testen

Als u al deze stappen hebt voltooid, moet uw app te starten. Voor het testen van de app:

1. Open een browser en navigeer naar de externe URL die u hebt gemaakt toen u de app hebt gepubliceerd. 
1. Meld u aan met de testaccount dat u hebt toegewezen aan de app. U moet mogelijk de toepassing laadt en eenmalige aanmelding in de toepassing.

## <a name="next-steps"></a>Volgende stappen

- [Hoe biedt Azure AD-toepassingsproxy eenmalige aanmelding?](application-proxy-single-sign-on.md)
- [Application Proxy oplossen](application-proxy-troubleshoot.md)
