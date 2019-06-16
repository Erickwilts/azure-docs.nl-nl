---
title: Snelstartgids - toegang blokkeren wanneer het risico van een sessie wordt gedetecteerd met Azure Active Directory voor voorwaardelijke toegang | Microsoft Docs
description: In deze snelstartgids leert u hoe u een beleid voor voorwaardelijke toegang van Azure Active Directory (Azure AD) voor het blokkeren van aanmeldingen op basis van de sessie risico's kunt configureren.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: quickstart
ms.date: 12/14/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: f8de4e785bbe2496ca38b33512da1c85f9ff76f3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67112777"
---
# <a name="quickstart-block-access-when-a-session-risk-is-detected-with-azure-active-directory-conditional-access"></a>Quickstart: Toegang blokkeren als er een risico voor de sessie wordt gedetecteerd met Azure Active Directory voor voorwaardelijke toegang  

Om te voorkomen dat uw omgeving beveiligd, is het raadzaam om verdachte gebruikers van de aanmelding te blokkeren. [Azure Active Directory (Azure AD) Identity Protection](../active-directory-identityprotection.md) analyseert elke aanmelding en berekent de kans dat een aanmelding bij poging is niet uitgevoerd door de rechtmatige eigenaar van een gebruikersaccount. De kans (laag, Gemiddeld, hoog) wordt aangegeven in de vorm van een berekende waarde met de naam [aanmelden risiconiveaus](conditions.md#sign-in-risk). Door in te stellen de voorwaarde voor aanmeldingsrisico, kunt u een beleid voor voorwaardelijke toegang om te reageren op specifieke aanmeldingsrisico niveaus configureren.

Deze quickstart laat zien hoe u configureert een [beleid voor voorwaardelijke toegang](../active-directory-conditional-access-azure-portal.md) die een aanmelding wordt geblokkeerd als het niveau van een geconfigureerde aanmeldingsrisico is gedetecteerd.

![Beleid maken](./media/app-sign-in-risk/1000.png)

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het voltooien van het scenario in deze zelfstudie hebt u het volgende nodig:

- **Toegang tot een Azure AD Premium P2-editie** -terwijl voorwaardelijke toegang is een Azure AD Premium P1-mogelijkheid, moet u een P2-editie omdat het scenario in deze snelstartgids Identity Protection is vereist.

- **Identity Protection** -Identity Protection wordt ingeschakeld door het scenario in deze Quick Start is vereist. Als u niet hoe u Identity Protection inschakelen weet, raadpleegt u [inschakelen van Azure Active Directory Identity Protection](../identity-protection/enable.md).

- **Tor Browser** : de [Tor Browser](https://www.torproject.org/projects/torbrowser.html.en) is ontworpen om u te helpen u uw privacy online beschermen. Identity Protection detecteert een aanmelding vanuit een Tor Browser als **aanmeldingen vanaf anonieme IP-adressen**, die is voorzien van een gemiddeld risico-niveau. Zie [Risicogebeurtenissen in Azure Active Directory](../reports-monitoring/concept-risk-events.md) voor meer informatie.  

- **Een testaccount met de naam Alain Charon** : als u niet hoe ik een testaccount maakt weet, Zie [cloudgebruikers toevoegen](../fundamentals/add-users-azure-active-directory.md#add-a-new-user).

## <a name="test-your-sign-in"></a>Test de aanmelding

Het doel van deze stap is om ervoor te zorgen dat uw testaccount toegang heeft tot uw tenant met behulp van de Tor-Browser.

**Voor het testen van de aanmelding:**

1. Aanmelden bij uw [Azure-portal](https://portal.azure.com) als **Alain Charon**.
1. Meld u af.

## <a name="create-your-conditional-access-policy"></a>Uw beleid voor voorwaardelijke toegang maken

Het scenario in deze snelstartgids wordt een aanmelding vanuit een Tor-Browser gebruikt voor het genereren van een gedetecteerde **aanmeldingen vanaf anonieme IP-adressen** risicogebeurtenis. Het risiconiveau van deze risicogebeurtenis is normaal. Om te reageren op deze risicogebeurtenis, kunt u de voorwaarde voor aanmeldingsrisico instellen op Gemiddeld. In een productieomgeving, moet u de voorwaarde voor aanmeldingsrisico instellen op hoge of op Gemiddeld en hoog.

Deze sectie wordt beschreven hoe u het vereiste beleid voor voorwaardelijke toegang maken. Stel in het beleid:

| Instelling | Value |
| --- | --- |
| Gebruikers en groepen | Alain Charon  |
| Cloud-apps | Alle cloud-apps |
| Aanmeldingsrisico | Gemiddeld |
| Verlenen | Toegang blokkeren |

![Beleid maken](./media/app-sign-in-risk/130.png)

**Uw beleid voor voorwaardelijke toegang configureren:**

1. Aanmelden bij uw [Azure-portal](https://portal.azure.com) als hoofdbeheerder, beveiligingsbeheerder of een beheerder van voorwaardelijke toegang.

1. Klik in de Azure-portal op de navigatiebalk links op **Azure Active Directory**.

   ![Azure Active Directory](./media/app-sign-in-risk/02.png)

1. Op de **Azure Active Directory** pagina, in de **Security** sectie, klikt u op **voorwaardelijke toegang**.

   ![Voorwaardelijke toegang](./media/app-sign-in-risk/03.png)

1. Op de **voorwaardelijke toegang** in de werkbalk bovenaan op de pagina, klikt u op **toevoegen**.

   ![Name](./media/app-sign-in-risk/108.png)

1. Op de **nieuw** pagina, in de **naam** tekstvak, type **toegang blokkeren voor middelgrote risiconiveau**.

   ![Name](./media/app-sign-in-risk/104.png)

1. In de **toewijzing** sectie, klikt u op **gebruikers en groepen**.

   ![Gebruikers en groepen](./media/app-sign-in-risk/06.png)

1. Op de **gebruikers en groepen** pagina:

   ![Voorwaardelijke toegang](./media/app-sign-in-risk/107.png)

   1. Klik op **gebruikers en groepen selecteren**, en selecteer vervolgens **gebruikers en groepen**.

   1. Klik op **Selecteren**.

   1. Op de **Selecteer** pagina, selecteert u **Alain Charon**, en klik vervolgens op **Selecteer**.

   1. Op de **gebruikers en groepen** pagina, klikt u op **gedaan**.

1. Klik op **Cloud-apps**.

   ![Cloud-apps](./media/app-sign-in-risk/08.png)

1. Op de **Cloud-apps** pagina:

   ![Voorwaardelijke toegang](./media/app-sign-in-risk/109.png)

   1. Klik op **alle cloud-apps**.

   1. Klik op **Gereed**.

1. Klik op **voorwaarden**.

   ![Besturingselementen voor toegang](./media/app-sign-in-risk/19.png)

1. Op de **voorwaarden** pagina:

   ![Het niveau van aanmeldingsrisico](./media/app-sign-in-risk/21.png)

   1. Klik op **aanmeldingsrisico**.

   1. Als **configureren**, klikt u op **Ja**.

   1. Niveau van aanmeldingsrisico selecteren **gemiddeld**.

   1. Klik op **Selecteren**.

   1. Op de **voorwaarden** pagina, klikt u op **gedaan**.

1. In de **besturingselementen voor toegang** sectie, klikt u op **verlenen**.

   ![Besturingselementen voor toegang](./media/app-sign-in-risk/10.png)

1. Op de **verlenen** pagina:

   ![Voorwaardelijke toegang](./media/app-sign-in-risk/105.png)

   1. Selecteer **toegang blokkeren**.

   1. Klik op **Selecteren**.

1. In de **beleid inschakelen** sectie, klikt u op **op**.

   ![Beleid inschakelen](./media/app-sign-in-risk/18.png)

1. Klik op **Create**.

## <a name="evaluate-a-simulated-sign-in"></a>Een gesimuleerde aanmelding evalueren

Nu dat u uw beleid voor voorwaardelijke toegang hebt geconfigureerd, wilt u waarschijnlijk weet of deze werkt zoals verwacht. Als een eerste stap gebruikt u de voorwaardelijke toegang **wat gebeurt er als beleid hulpprogramma** voor het simuleren van een aanmelding van uw testgebruiker. De simulatie schat de impact van deze aanmelding op uw beleid in en genereert een simulatierapport.  

Bij het uitvoeren van de **wat gebeurt er als beleid hulpprogramma** voor dit scenario, de **toegang blokkeren voor middelgrote risiconiveau** moet worden weergegeven onder **beleidsregels die worden toegepast**.

![Gebruiker](./media/app-sign-in-risk/117.png)

**Uw beleid voor voorwaardelijke toegang evalueren:**

1. Op de [voorwaardelijke toegang - beleid](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/Policies) in het menu bovenaan op de pagina, klikt u op **wat gebeurt er als**.  

   ![Wat als](./media/app-sign-in-risk/14.png)

1. Klik op **gebruiker**, selecteer **Alan Charon** op de **gebruikers** pagina en klik vervolgens op **Selecteer**.

   ![Gebruiker](./media/app-sign-in-risk/116.png)

1. Als **aanmeldingsrisico**, selecteer **gemiddeld**.

   ![Gebruiker](./media/app-sign-in-risk/119.png)

1. Klik op **wat gebeurt er als**.

## <a name="test-your-conditional-access-policy"></a>Testen van uw beleid voor voorwaardelijke toegang

In de vorige sectie hebt u geleerd hoe u een gesimuleerde aanmelding evalueren. Naast een simulatie, moet u ook testen van uw beleid voor voorwaardelijke toegang om ervoor te zorgen dat deze naar verwachting werkt.

Als u wilt testen van uw beleid, willen aanmelden bij uw [Azure-portal](https://portal.azure.com) als **Alan Charon** met behulp van de Tor-Browser. Uw aanmeldingspoging moet worden geblokkeerd door uw beleid voor voorwaardelijke toegang.

![Multi-Factor Authentication](./media/app-sign-in-risk/118.png)

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u niet meer nodig hebt, verwijdert u de testgebruiker, de Tor-Browser en het beleid voor voorwaardelijke toegang:

- Als u niet hoe u een Azure AD-gebruiker verwijdert weet, Zie [gebruikers verwijderen uit Azure AD](../fundamentals/add-users-azure-active-directory.md#delete-a-user).

- Als u wilt verwijderen van uw beleid, selecteert u uw beleid en klik vervolgens op **verwijderen** in de werkbalk Snelle toegang.

   ![Multi-Factor Authentication](./media/app-sign-in-risk/33.png)

- Zie voor instructies voor het verwijderen van de Tor Browser, [verwijderen](https://tb-manual.torproject.org/uninstalling/).

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Vereisen dat de gebruiksrechtovereenkomst moet zijn geaccepteerd](require-tou.md)
> [MFA vereisen voor specifieke apps](app-based-mfa.md)
