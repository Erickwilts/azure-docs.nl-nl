---
title: 'Zelfstudie: Integratie van Azure Active Directory met New Relic by Account | Microsoft Docs'
description: Leer hoe u eenmalige aanmelding configureert tussen Azure Active Directory en New Relic by Account.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 04/16/2020
ms.author: jeedes
ms.openlocfilehash: 19a471b9339828250baa190b1de57c86eb22b30c
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2020
ms.locfileid: "88554459"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-new-relic-by-account"></a>Zelfstudie: Integratie van eenmalige aanmelding (SSO) van Azure Active Directory met New Relic by Account

In deze zelfstudie leert u hoe u New Relic by Account integreert met Azure Active Directory (Azure AD). Wanneer u New Relic by Account met Azure AD integreert, kunt u het volgende doen:

* In Azure AD bepalen wie er toegang heeft tot New Relic by Account.
* Instellen dat gebruikers automatisch met hun Azure AD-account worden aangemeld bij New Relic by Account.
* Uw accounts op een centrale locatie beheren: Azure Portal.

Zie [Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) voor meer informatie over de integratie van SaaS-apps met Azure AD.

## <a name="prerequisites"></a>Vereisten

U hebt het volgende nodig om aan de slag te gaan:

* Een Azure AD-abonnement Als u geen abonnement hebt, kunt u zich aanmelden voor een [gratis account](https://azure.microsoft.com/free/).
* Een abonnement op New Relic by Account waarvoor eenmalige aanmelding is ingeschakeld.

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* New Relic by Account ondersteunt door **SP** geïnitieerde eenmalige aanmelding

* Zodra u New Relic by Account hebt geconfigureerd, kunt u sessiebeheer afdwingen, waardoor exfiltratie en infiltratie van gevoelige gegevens van uw organisatie in realtime worden beschermd. Sessiebeheer is een uitbreiding van voorwaardelijke toegang. [Meer informatie over het afdwingen van sessiebeheer met Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).

## <a name="adding-new-relic-by-account-from-the-gallery"></a>New Relic by Account toevoegen vanuit de galerie

Voor het configureren van de integratie van New Relic by Account met Microsoft Azure Active Directory moet u New Relic by Account vanuit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
1. Selecteer in het linkernavigatiedeelvenster de service **Azure Active Directory**.
1. Ga naar **Bedrijfstoepassingen** en selecteer vervolgens **Alle toepassingen**.
1. Selecteer **Nieuwe toepassing** om een nieuwe toepassing toe te voegen.
1. Typ in de sectie **Toevoegen uit de galerie** **New Relic by Account** in het zoekvak.
1. Selecteer **New Relic by Account** in het resultatenvenster en voeg de app vervolgens toe. Wacht enkele seconden tot de app is toegevoegd aan de tenant.

## <a name="configure-and-test-azure-ad-single-sign-on-for-new-relic-by-account"></a>Eenmalige aanmelding van Azure AD configureren en testen voor New Relic by Account

Configureer en test eenmalige aanmelding van Azure AD met New Relic by Account met behulp van een testgebruiker met de naam **B.Simon**. Eenmalige aanmelding werkt alleen als u een koppelingsrelatie tot stand brengt tussen een Azure AD-gebruiker en de bijbehorende gebruiker in New Relic by Account.

Voltooi de volgende bouwstenen om eenmalige aanmelding van Azure AD met New Relic by Account te configureren en te testen:

1. **[Eenmalige aanmelding van Azure AD configureren](#configure-azure-ad-sso)** : zodat uw gebruikers deze functie kunnen gebruiken.
    * **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)** : om eenmalige aanmelding van Azure AD te testen met B.Simon.
    * **[De Azure AD-testgebruiker toewijzen](#assign-the-azure-ad-test-user)** zodat B.Simon eenmalige aanmelding van Azure AD kan gebruiken.
1. **[Eenmalige aanmelding van New Relic by Account configureren](#configure-new-relic-by-account-sso)** : als u de instellingen voor eenmalige aanmelding aan de toepassingszijde wilt configureren.
    * **[Een testgebruiker maken voor New Relic by Account](#create-new-relic-by-account-test-user)** : als u een tegenhanger van B.Simon in New Relic by Account wilt hebben die is gekoppeld aan de Azure AD-weergave van de gebruiker.
1. **[Eenmalige aanmelding testen](#test-sso)** : om te controleren of de configuratie werkt.

## <a name="configure-azure-ad-sso"></a>Eenmalige aanmelding van Azure AD configureren

Volg deze stappen om eenmalige aanmelding van Azure AD in te schakelen in Azure Portal.

1. Zoek in [Azure Portal](https://portal.azure.com/) op de integratiepagina van de toepassing **New Relic by Account** de sectie **Beheren** en selecteer **Eenmalige aanmelding**.
1. Selecteer **SAML** op de pagina **Selecteer een methode voor eenmalige aanmelding**.
1. Op de pagina **Eenmalige aanmelding instellen met SAML** klikt u op het bewerkings-/penpictogram voor **Standaard-SAML-configuratie** om de instellingen te bewerken.

   ![Standaard SAML-configuratie bewerken](common/edit-urls.png)
1. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met het volgende patroon:

    `https://rpm.newrelic.com/accounts/{acc_id}/sso/saml/login`. Vergeet niet om `acc_id` te vervangen door uw eigen account-id van New Relic by Account.

    b. Typ een URL in het vak **Id (Entiteits-id)** : `rpm.newrelic.com`

1. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

1. Kopieer in het gedeelte **New Relic by Account instellen** de juiste URL('s) op basis van uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

In deze sectie gaat u een testgebruiker met de naam B.Simon maken in Azure Portal.

1. Selecteer in het linkerdeelvenster van Azure Portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.
1. Selecteer **Nieuwe gebruiker** boven aan het scherm.
1. Volg de volgende stappen bij de eigenschappen voor **Gebruiker**:
   1. Voer in het veld **Naam**`B.Simon` in.  
   1. Voer username@companydomain.extension in het veld **Gebruikersnaam** in. Bijvoorbeeld `B.Simon@contoso.com`.
   1. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak **Wachtwoord**.
   1. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie geeft u B.Simon toestemming om eenmalige aanmelding van Azure te gebruiken door toegang te verlenen tot New Relic by Account.

1. Selecteer in Azure Portal de optie **Bedrijfstoepassingen** en selecteer vervolgens **Alle toepassingen**.
1. Selecteer **New Relic by Account** in de lijst met toepassingen.
1. Zoek op de overzichtspagina van de app de sectie **Beheren** en selecteer **Gebruikers en groepen**.

   ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

1. Selecteer **Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![De koppeling Gebruiker toevoegen](common/add-assign-user.png)

1. Selecteer in het dialoogvenster **Gebruikers en groepen** de optie **B.Simon** in de lijst Gebruikers. Klik vervolgens op de knop **Selecteren** onderaan het scherm.
1. Als u een waarde voor een rol verwacht in de SAML-assertie, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren. Klik vervolgens op de knop **Selecteren** onderaan het scherm.
1. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

## <a name="configure-new-relic-by-account-sso"></a>Eenmalige aanmelding configureren voor New Relic by Account

1. Meld u in een andere browser als beheerder aan bij de bedrijfssite van **New Relic by Account**.

2. Klik in het menu bovenaan op **Accountinstellingen**.
   
    ![Accountinstellingen](./media/new-relic-tutorial/ic797036.png "Accountinstellingen")

3. Klik op het tabblad **Security and authentication** en klik vervolgens op het tabblad **Single sign on**.
   
    ![Single Sign-On](./media/new-relic-tutorial/ic797037.png "Eenmalige aanmelding")

4. Voer in het dialoogvenster SAML de volgende stappen uit:
   
    ![SAML](./media/new-relic-tutorial/ic797038.png "SAML")
   
    a. Klik op **Choose File** om uw gedownloade Azure Active Directory-certificaat te uploaden.

    b. Plak in het tekstvak **Remote login URL** de waarde van de **aanmeldings-URL** die u hebt gekopieerd uit Azure Portal.
   
    c. Plak in het tekstvak **Logout landing URL** de waarde van de **URL voor afmelden** die u hebt gekopieerd uit Azure Portal.

    d. Klik op **Save my changes**.

### <a name="create-new-relic-by-account-test-user"></a>Nieuwe New Relic by Account-testgebruiker maken

1. Meld u als beheerder aan bij uw bedrijfssite van **New Relic by Account**.

2. Klik in het menu bovenaan op **Accountinstellingen**.
   
    ![Accountinstellingen](./media/new-relic-tutorial/ic797040.png "Accountinstellingen")

3. Klik in het linkernavigatievenster op **Account**, klik op **Summary** en klik vervolgens op **Add user**.
   
    ![Accountinstellingen](./media/new-relic-tutorial/ic797041.png "Accountinstellingen")

4. Voer in het dialoogvenster **Active users** de volgende stappen uit:
   
    ![Actieve gebruikers](./media/new-relic-tutorial/ic797042.png "Actieve gebruikers")
   
    a. Typ in het tekstvak **Email** het e-mailadres van een geldige Azure Active Directory-gebruiker die u wilt inrichten.

    b. Bij **Role** selecteert u **User**.

    c. Klik op **Add this user**.

> [!NOTE]
> U kunt alle hulpprogramma's voor het maken van gebruikersaccounts of API's van New Relic by Account gebruiken om Azure AD-gebruikersaccounts in te richten.

## <a name="test-sso"></a>Eenmalige aanmelding testen 

In deze sectie gaat u uw configuratie van Azure AD-eenmalige aanmelding testen via het toegangsvenster.

Wanneer u in het toegangsvenster op de tegel New Relic by Account klikt, wordt u automatisch aangemeld bij het exemplaar van New Relic by Account waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende bronnen

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [New Relic by Account gebruiken met Azure AD](https://aad.portal.azure.com/)

- [Wat is sessiebeheer in Microsoft Cloud App Security?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)