---
title: 'Snelstart: multi-factor authentication (MFA) vereist voor specifieke apps met Azure Active Directory voor voorwaardelijke toegang | Microsoft Docs'
description: In deze snelstartgids leert u hoe u uw verificatievereisten voor het type gebruikte cloud-app met behulp van voorwaardelijke toegang van Azure Active Directory (Azure AD) kunt verbinden.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: quickstart
ms.date: 01/30/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: db191587f02fa8fa8934cac7a001ea31c233cbdb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67112755"
---
# <a name="quickstart-require-mfa-for-specific-apps-with-azure-active-directory-conditional-access"></a>Quickstart: MFA vereisen voor specifieke apps met Azure Active Directory voor voorwaardelijke toegang

Ter vereenvoudiging van de aanmeldingservaring van uw gebruikers, is het raadzaam om toe te staan dat ze zich aanmelden bij uw cloud-apps met behulp van een gebruikersnaam en wachtwoord. Veel omgevingen hebben echter ten minste een aantal apps waarvoor u wordt aangeraden een sterkere vorm van verificatie-account, zoals multi-factor authentication (MFA) vereist. Dit kan zijn, voor de voorbeeld-waar, voor toegang tot e-mailsysteem van uw organisatie of uw HR-apps. In Azure Active Directory (Azure AD), kunt u dit doel met beleid voor voorwaardelijke toegang uitvoeren.

Deze quickstart laat zien hoe u configureert een [Azure AD voorwaardelijke toegangsbeleid](../active-directory-conditional-access-azure-portal.md) waarvoor multi-factor Authentication-verificatie is vereist voor een geselecteerde cloud-app in uw omgeving.

![Voorbeeld van beleid voor voorwaardelijke toegang in Azure portal](./media/app-based-mfa/32.png)

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

Als u wilt het scenario in deze snelstartgids hebt voltooid, hebt u het volgende nodig:

- **Toegang tot een Azure AD Premium-editie** -Azure AD voor voorwaardelijke toegang is een Azure AD Premium-capaciteit.

- **Een testaccount met de naam Isabella Simonsen** : als u niet hoe ik een testaccount maakt weet, Zie [cloudgebruikers toevoegen](../fundamentals/add-users-azure-active-directory.md#add-a-new-user).

Het scenario in deze quickstart vereist dat per gebruiker MFA niet is ingeschakeld voor uw testaccount. Zie voor meer informatie, [hoe u verificatie in twee stappen vereist voor een gebruiker](../authentication/howto-mfa-userstates.md).

## <a name="test-your-sign-in"></a>Test de aanmelding

Het doel van deze stap is om een indruk van de ervaring aanmelden zonder een beleid voor voorwaardelijke toegang.

**Uw omgeving initialiseren:**

1. Meld u aan uw Azure-portal als Isabella Simonsen.
1. Meld u af.

## <a name="create-your-conditional-access-policy"></a>Uw beleid voor voorwaardelijke toegang maken

Deze sectie wordt beschreven hoe u het vereiste beleid voor voorwaardelijke toegang maken. Maakt gebruik van het scenario in deze Quick Start:

- De Azure-portal als tijdelijke aanduiding voor een cloud-app die MFA vereist. 
- De voorbeeldgebruiker in voor het testen van het beleid voor voorwaardelijke toegang.  

Stel in het beleid:

| Instelling | Value |
| --- | --- |
| Gebruikers en groepen | Isabella Simonsen |
| Cloud-apps | Microsoft Azure Management |
| Toegang verlenen | Meervoudige verificatie vereisen |

![Uitgebreide beleid voor voorwaardelijke toegang](./media/app-based-mfa/31.png)

**Uw beleid voor voorwaardelijke toegang configureren:**

1. Aanmelden bij uw [Azure-portal](https://portal.azure.com) als hoofdbeheerder, beveiligingsbeheerder of een beheerder van voorwaardelijke toegang.

1. Klik in de Azure-portal op de navigatiebalk links op **Azure Active Directory**.

   ![Azure Active Directory](./media/app-based-mfa/02.png)

1. Op de **Azure Active Directory** pagina, in de **Security** sectie, klikt u op **voorwaardelijke toegang**.

   ![Voorwaardelijke toegang](./media/app-based-mfa/03.png)

1. Op de **voorwaardelijke toegang** in de werkbalk bovenaan op de pagina, klikt u op **nieuw beleid**.

   ![Toevoegen](./media/app-based-mfa/04.png)

1. Op de **nieuw** pagina, in de **naam** tekstvak, type **MFA vereisen voor toegang tot Azure portal**.

   ![Name](./media/app-based-mfa/05.png)

1. In de **toewijzing** sectie, klikt u op **gebruikers en groepen**.

   ![Gebruikers en groepen](./media/app-based-mfa/06.png)

1. Op de **gebruikers en groepen** pagina, voert u de volgende stappen uit:

   ![Gebruikers en groepen](./media/app-based-mfa/24.png)

   1. Klik op **gebruikers en groepen selecteren**, en selecteer vervolgens **gebruikers en groepen**.

   1. Klik op **Selecteren**.

   1. Op de **Selecteer** pagina, selecteert u **Isabella Simonsen**, en klik vervolgens op **Selecteer**.

   1. Op de **gebruikers en groepen** pagina, klikt u op **gedaan**.

1. Klik op **Cloud-apps**.

   ![Cloud-apps](./media/app-based-mfa/08.png)

1. Op de **Cloud-apps** pagina, voert u de volgende stappen uit:

   ![Cloud-apps selecteren](./media/app-based-mfa/26.png)

   1. Klik op **apps selecteren**.

   1. Klik op **Selecteren**.

   1. Op de **Selecteer** pagina, selecteert u **Microsoft Azure Management**, en klik vervolgens op **Selecteer**.

   1. Op de **Cloud-apps** pagina, klikt u op **gedaan**.

1. In de **besturingselementen voor toegang** sectie, klikt u op **verlenen**.

   ![Besturingselementen voor toegang](./media/app-based-mfa/10.png)

1. Op de **verlenen** pagina, voert u de volgende stappen uit:

   ![Verlenen](./media/app-based-mfa/11.png)

   1. Selecteer **toegang verlenen**.

   1. Selecteer **meervoudige verificatie vereisen**.

   1. Klik op **Selecteren**.

1. In de **beleid inschakelen** sectie, klikt u op **op**.

   ![Beleid inschakelen](./media/app-based-mfa/18.png)

1. Klik op **Create**.

## <a name="evaluate-a-simulated-sign-in"></a>Een gesimuleerde aanmelding evalueren

Nu dat u uw beleid voor voorwaardelijke toegang hebt geconfigureerd, wilt u waarschijnlijk weet of deze werkt zoals verwacht. Gebruik als een eerste stap de voorwaardelijke toegang beleid hulpprogramma what-if om te simuleren een aanmelding van uw testgebruiker. De simulatie schat de impact van deze aanmelding op uw beleid in en genereert een simulatierapport.  

Initialiseren van de wat als hulpprogramma voor het evalueren van beleid is ingesteld:

- **Isabella Simonsen** als gebruiker
- **Microsoft Azure Management** als cloud-app

Te klikken op **wat gebeurt er als** maakt u een simulatierapport waarin wordt weergegeven:

- **MFA vereisen voor toegang tot Azure portal** onder **beleidsregels die worden toegepast**
- **Meervoudige verificatie vereisen** als **besturingselementen verlenen**.

![Wat gebeurt er als hulpprogramma voor beleid](./media/app-based-mfa/23.png)

**Uw beleid voor voorwaardelijke toegang evalueren:**

1. Op de [voorwaardelijke toegang - beleid](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/Policies) in het menu bovenaan op de pagina, klikt u op **wat gebeurt er als**.  

   ![Wat als](./media/app-based-mfa/14.png)

1. Klik op **gebruikers**, selecteer **Isabella Simonsen**, en klik vervolgens op **Selecteer**.

   ![Gebruiker](./media/app-based-mfa/15.png)

1. Selecteer een cloud-app, moet u de volgende stappen uitvoeren:

   ![Cloud-apps](./media/app-based-mfa/16.png)

   1. Klik op **Cloud-apps**.

   1. Op de **pagina voor Cloud-apps**, klikt u op **apps selecteren**.

   1. Klik op **Selecteren**.

   1. Op de **Selecteer** pagina, selecteert u **Microsoft Azure Management**, en klik vervolgens op **Selecteer**.

   1. Klik op de pagina van de cloud-apps op **gedaan**.

1. Klik op **wat gebeurt er als**.

## <a name="test-your-conditional-access-policy"></a>Testen van uw beleid voor voorwaardelijke toegang

In de vorige sectie hebt u geleerd hoe u een gesimuleerde aanmelding evalueren. Naast een simulatie, moet u ook testen van uw beleid voor voorwaardelijke toegang om ervoor te zorgen dat deze naar verwachting werkt.

Als u wilt testen van uw beleid, willen aanmelden bij uw [Azure-portal](https://portal.azure.com) met behulp van uw **Isabella Simonsen** test-account. U ziet een dialoogvenster waarin u uw account voor aanvullende beveiligingsverificatie instellen is vereist.

![Multi-Factor Authentication](./media/app-based-mfa/22.png)

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u niet meer nodig hebt, verwijdert u de testgebruiker en het beleid voor voorwaardelijke toegang:

- Als u niet hoe u een Azure AD-gebruiker verwijdert weet, Zie [gebruikers verwijderen uit Azure AD](../fundamentals/add-users-azure-active-directory.md#delete-a-user).
- Als u wilt verwijderen van uw beleid, selecteert u uw beleid en klik vervolgens op **verwijderen** in de werkbalk Snelle toegang.

    ![Multi-Factor Authentication](./media/app-based-mfa/33.png)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Vereisen dat de gebruiksrechtovereenkomst moet zijn geaccepteerd](require-tou.md)
> [toegang blokkeren als er een risico voor de sessie wordt gedetecteerd](app-sign-in-risk.md)
