---
title: Snelstartgids - gebruiksrechtovereenkomst moet zijn geaccepteerd voordat u toegang tot cloud-apps die zijn beveiligd met Azure Active Directory voor voorwaardelijke toegang vereisen | Microsoft Docs
description: In deze snelstartgids leert u hoe u kunt vereisen dat de gebruiksrechtovereenkomst worden geaccepteerd voordat de toegang tot de geselecteerde cloud-apps met Azure Active Directory voor voorwaardelijke toegang wordt verleend.
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
ms.openlocfilehash: 2a3523a050a021f3a98c144efe14d692704fba63
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67112218"
---
# <a name="quickstart-require-terms-of-use-to-be-accepted-before-accessing-cloud-apps"></a>Quickstart: Gebruiksrechtovereenkomst moet zijn geaccepteerd voordat u toegang tot cloud-apps vereisen

Voordat u toegang tot bepaalde cloud-apps in uw omgeving, is het raadzaam om op te halen van toestemming van gebruikers in de vorm van het accepteren van de gebruiksrechtovereenkomst (gebruiksvoorwaarden). Voorwaardelijke toegang van Azure Active Directory (Azure AD) beschikt u over:

- Een eenvoudige methode voor het configureren van gebruiksvoorwaarden
- De optie voor het accepteren van de gebruiksrechtovereenkomst via beleid voor voorwaardelijke toegang vereisen  

Deze quickstart laat zien hoe u configureert een [Azure AD voorwaardelijke toegangsbeleid](../active-directory-conditional-access-azure-portal.md) waarvoor een gebruiksrechtovereenkomst kunnen worden geaccepteerd voor een geselecteerde cloud-app in uw omgeving is vereist.

![Beleid maken](./media/require-tou/5555.png)

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

Als u wilt het scenario in deze snelstartgids hebt voltooid, hebt u het volgende nodig:

- **Toegang tot een Azure AD Premium-editie** -Azure AD voor voorwaardelijke toegang is een Azure AD Premium-capaciteit.
- **Een testaccount met de naam Isabella Simonsen** : als u niet hoe ik een testaccount maakt weet, Zie [cloudgebruikers toevoegen](../fundamentals/add-users-azure-active-directory.md#add-a-new-user).

## <a name="test-your-sign-in"></a>Test de aanmelding

Het doel van deze stap is om een indruk van de ervaring aanmelden zonder een beleid voor voorwaardelijke toegang.

**Voor het testen van de aanmelding:**

1. Aanmelden bij uw [Azure-portal](https://portal.azure.com/) als Isabella Simonsen.
1. Meld u af.

## <a name="create-your-terms-of-use"></a>Uw gebruiksvoorwaarden maken

In deze sectie biedt u de stappen voor het maken van een voorbeeld van een gebruiksvoorwaarden. Als u een gebruiksvoorwaarden maakt, selecteert u een waarde voor **afdwingen met sjablonen voor voorwaardelijke toegang voor**. Selecteren **aangepast beleid** opent u het dialoogvenster voor het maken van een nieuw beleid voor voorwaardelijke toegang zodra uw gebruiksvoorwaarden is gemaakt.

**Uw gebruiksvoorwaarden maken:**

1. Maak een nieuw document in Microsoft Word.

1. Type **mijn gebruiksvoorwaarden**, en sla het document op uw computer als **mytou.pdf**.

1. Aanmelden bij uw [Azure-portal](https://portal.azure.com) als hoofdbeheerder, beveiligingsbeheerder of een beheerder van voorwaardelijke toegang.

1. Klik in de Azure-portal op de navigatiebalk links op **Azure Active Directory**.

   ![Azure Active Directory](./media/require-tou/02.png)

1. Op de **Azure Active Directory** pagina, in de **Security** sectie, klikt u op **voorwaardelijke toegang**.

   ![Voorwaardelijke toegang](./media/require-tou/03.png)

1. In de **beheren** sectie, klikt u op **gebruiksvoorwaarden**.

   ![Gebruiksvoorwaarden](./media/require-tou/04.png)

1. Klik in het menu aan de bovenkant op **nieuwe voorwaarden**.

   ![Gebruiksvoorwaarden](./media/require-tou/05.png)

1. Op de **nieuwe gebruiksvoorwaarden** pagina:

   ![Gebruiksvoorwaarden](./media/require-tou/112.png)

   1. In de **naam** tekstvak, type **mijn gebruiksvoorwaarden**.

   1. In de **weergavenaam** tekstvak, type **mijn gebruiksvoorwaarden**.

   1. Upload de voorwaarden van de PDF-bestanden gebruiken.

   1. Als **taal**, selecteer **Engels**.

   1. Als **vereisen dat gebruikers om uit te breiden de gebruiksvoorwaarden**, selecteer **op**.

   1. Als **afdwingen met sjablonen voor voorwaardelijke toegang voor**, selecteer **aangepast beleid**.

   1. Klik op **Create**.

## <a name="create-your-conditional-access-policy"></a>Uw beleid voor voorwaardelijke toegang maken

Deze sectie wordt beschreven hoe u het vereiste beleid voor voorwaardelijke toegang maken. Maakt gebruik van het scenario in deze Quick Start:

- De Azure-portal als tijdelijke aanduiding voor een cloud-app waarvoor uw gebruiksvoorwaarden worden geaccepteerd. 
- De voorbeeldgebruiker in voor het testen van het beleid voor voorwaardelijke toegang.  

Stel in het beleid:

| Instelling | Value |
| --- | --- |
| Gebruikers en groepen | Isabella Simonsen |
| Cloud-apps | Microsoft Azure Management |
| Toegang verlenen | Mijn gebruiksvoorwaarden |

![Beleid maken](./media/require-tou/1234.png)

**Uw beleid voor voorwaardelijke toegang configureren:**

1. Op de **nieuw** pagina, in de **naam** tekstvak, type **vereisen gebruiksvoorwaarden voor Isabella**.

   ![Name](./media/require-tou/71.png)

1. In de **toewijzing** sectie, klikt u op **gebruikers en groepen**.

   ![Gebruikers en groepen](./media/require-tou/06.png)

1. Op de **gebruikers en groepen** pagina:

   ![Gebruikers en groepen](./media/require-tou/24.png)

   1. Klik op **gebruikers en groepen selecteren**, en selecteer vervolgens **gebruikers en groepen**.

   1. Klik op **Selecteren**.

   1. Op de **Selecteer** pagina, selecteert u **Isabella Simonsen**, en klik vervolgens op **Selecteer**.

   1. Op de **gebruikers en groepen** pagina, klikt u op **gedaan**.

1. Klik op **Cloud-apps**.

   ![Cloud-apps](./media/require-tou/08.png)

1. Op de **Cloud-apps** pagina:

   ![Cloud-apps selecteren](./media/require-tou/26.png)

   1. Klik op **apps selecteren**.

   1. Klik op **Selecteren**.

   1. Op de **Selecteer** pagina, selecteert u **Microsoft Azure Management**, en klik vervolgens op **Selecteer**.

   1. Op de **Cloud-apps** pagina, klikt u op **gedaan**.

1. In de **besturingselementen voor toegang** sectie, klikt u op **verlenen**.

   ![Besturingselementen voor toegang](./media/require-tou/10.png)

1. Op de **verlenen** pagina:

   ![Verlenen](./media/require-tou/111.png)

   1. Selecteer **toegang verlenen**.

   1. Selecteer **mijn gebruiksvoorwaarden**.

   1. Klik op **Selecteren**.

1. In de **beleid inschakelen** sectie, klikt u op **op**.

   ![Beleid inschakelen](./media/require-tou/18.png)

1. Klik op **Create**.

## <a name="evaluate-a-simulated-sign-in"></a>Een gesimuleerde aanmelding evalueren

Nu dat u uw beleid voor voorwaardelijke toegang hebt geconfigureerd, wilt u waarschijnlijk weet of deze werkt zoals verwacht. Gebruik als een eerste stap de voorwaardelijke toegang beleid hulpprogramma what-if om te simuleren een aanmelding van uw testgebruiker. De simulatie schat de impact van deze aanmelding op uw beleid in en genereert een simulatierapport.  

Initialiseren van de wat als hulpprogramma voor het evalueren van beleid is ingesteld:

- **Isabella Simonsen** als gebruiker
- **Microsoft Azure Management** als cloud-app

Te klikken op **wat gebeurt er als** maakt u een simulatierapport waarin wordt weergegeven:

- **Gebruiksvoorwaarden vereisen voor Isabella** onder **beleidsregels die worden toegepast**
- **Mijn gebruiksvoorwaarden** als **verlenen besturingselementen**.

![Wat gebeurt er als hulpprogramma voor beleid](./media/require-tou/79.png)

**Uw beleid voor voorwaardelijke toegang evalueren:**

1. Op de [voorwaardelijke toegang - beleid](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/Policies) in het menu bovenaan op de pagina, klikt u op **wat gebeurt er als**.  

   ![Wat als](./media/require-tou/14.png)

1. Klik op **gebruikers**, selecteer **Isabella Simonsen**, en klik vervolgens op **Selecteer**.

   ![Gebruiker](./media/require-tou/15.png)

1. Selecteer een cloud-app:

   ![Cloud-apps](./media/require-tou/16.png)

   1. Klik op **Cloud-apps**.

   1. Op de **pagina voor Cloud-apps**, klikt u op **apps selecteren**.

   1. Klik op **Selecteren**.

   1. Op de **Selecteer** pagina, selecteert u **Microsoft Azure Management**, en klik vervolgens op **Selecteer**.

   1. Klik op de pagina van de cloud-apps op **gedaan**.

1. Klik op **wat gebeurt er als**.

## <a name="test-your-conditional-access-policy"></a>Testen van uw beleid voor voorwaardelijke toegang

In de vorige sectie hebt u geleerd hoe u een gesimuleerde aanmelding evalueren. Naast een simulatie, moet u ook testen van uw beleid voor voorwaardelijke toegang om ervoor te zorgen dat deze naar verwachting werkt.

Als u wilt testen van uw beleid, willen aanmelden bij uw [Azure-portal](https://portal.azure.com) met behulp van uw **Isabella Simonsen** test-account. U ziet een dialoogvenster waarin u moet de gebruiksvoorwaarden accepteren.

![Gebruiksvoorwaarden](./media/require-tou/57.png)

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u niet meer nodig hebt, verwijdert u de testgebruiker en het beleid voor voorwaardelijke toegang:

- Als u niet hoe u een Azure AD-gebruiker verwijdert weet, Zie [gebruikers verwijderen uit Azure AD](../fundamentals/add-users-azure-active-directory.md#delete-a-user).

- Als u wilt verwijderen van uw beleid, selecteert u uw beleid en klik vervolgens op **verwijderen** in de werkbalk Snelle toegang.

    ![Multi-Factor Authentication](./media/require-tou/33.png)

- Als u wilt verwijderen van uw gebruiksvoorwaarden, selecteert u deze en klik vervolgens op **voorwaarden verwijderen** in de werkbalk bovenaan.

    ![Multi-Factor Authentication](./media/require-tou/29.png)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [MFA vereisen voor specifieke apps](app-based-mfa.md)
> [toegang blokkeren als er een risico voor de sessie wordt gedetecteerd](app-sign-in-risk.md)
