---
title: 'Zelfstudie: Azure Active Directory-integratie met BlueJeans | Microsoft Docs'
description: Leer hoe u eenmalige aanmelding kunt configureren tussen Azure Active Directory en BlueJeans.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: dfc634fd-1b55-4ba8-94a8-b8288429b6a9
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/16/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2830e95f042d5c83d91d6c026d2fa2e70eb3e3b2
ms.sourcegitcommit: 6ea7f0a6e9add35547c77eef26f34d2504796565
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/14/2019
ms.locfileid: "65601639"
---
# <a name="tutorial-azure-active-directory-integration-with-bluejeans"></a>Zelfstudie: Azure Active Directory-integratie met BlueJeans

In deze zelfstudie leert u hoe u BlueJeans kunt integreren met Azure Active Directory (Azure AD).
De integratie van BlueJeans met Azure AD heeft de volgende voordelen:

* U kunt in Azure AD bepalen wie toegang heeft tot BlueJeans.
* U kunt uw gebruikers zich automatisch laten aanmelden bij BlueJeans (eenmalige aanmelding) met hun Azure AD-account.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met BlueJeans hebt u het volgende nodig:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/)
* Een abonnement op BlueJeans waarvoor eenmalige aanmelding is ingeschakeld

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* BlueJeans ondersteunt door **SP** geïnitieerde eenmalige aanmelding

* BlueJeans ondersteunt het [**geautomatiseerd** inrichten van gebruikers](bluejeans-provisioning-tutorial.md)

## <a name="adding-bluejeans-from-the-gallery"></a>BlueJeans toevoegen vanuit de galerie

Voor het configureren van de integratie van BlueJeans in Azure AD moet u BlueJeans uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Voer de volgende stappen uit om BlueJeans toe te voegen vanuit de galerie:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ **BlueJeans** in het zoekvak, selecteer **BlueJeans** in het deelvenster met resultaten en klik op de knop **Toevoegen** om de toepassing toe te voegen.

     ![BlueJeans in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In dit gedeelte gaat u Azure AD-eenmalige aanmelding met BlueJeans configureren en testen met behulp van een testgebruiker met de naam **Britta Simon**.
Eenmalige aanmelding werkt alleen als er een koppelingsrelatie tussen een Azure AD-gebruiker en de daaraan gerelateerde gebruiker in BlueJeans tot stand is gebracht.

Als u Azure AD-eenmalige aanmelding met BlueJeans wilt configureren en testen, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Eenmalige aanmelding voor BlueJeans configureren](#configure-bluejeans-single-sign-on)**: als u de instellingen voor eenmalige aanmelding aan de clientzijde wil configureren.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Een testgebruiker maken in BlueJeans](#create-bluejeans-test-user)**: om in BlueJeans een tegenhanger van Britta Simon te hebben die is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voer de volgende stappen uit om Azure AD-eenmalige aanmelding bij BlueJeans te configureren:

1. In de [Azure-portal](https://portal.azure.com/) selecteert u **Eenmalige aanmelding** op de pagina voor integratie van toepassingen met **BlueJeans**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](media/bluejeans-tutorial/edit-urls-bluejeans.png)

4. In de **SAML-basisconfiguratie** dialoogvenster, voer de volgende waarden:

    ![Domein- en URL-gegevens voor eenmalige aanmelding bij BlueJeans](media/bluejeans-tutorial/tutorial_bluejeans-basic-configuration.png)

   - In het tekstvak **Id** typt u het volgende: `http://samlsp.bluejeans.com`
    
   - In de **aanmeldings-URL** tekstvak typt u de URL van de landingspagina die u van BlueJeans (als u deze waarde, kunt u contact met de [BlueJeans Client ondersteuningsteam](https://support.bluejeans.com/contact)): `https://<companyname>.bluejeans.com`
    
   - Klik op **Opslaan**.

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

6. In het gedeelte **BlueJeans instellen** kopieert u de juiste URL('s) op basis van uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. URL voor afmelden

### <a name="configure-bluejeans-single-sign-on"></a>Eenmalige aanmelding voor BlueJeans configureren

1. In een ander browservenster aanmelden bij uw **BlueJeans** bedrijf site als beheerder.

2. Ga naar **ADMIN \> GROEPSINSTELLINGEN \> BEVEILIGING**.

    ![Admin](./media/bluejeans-tutorial/ic785868.png "Admin")

3. Voer in het gedeelte **BEVEILIGING** de volgende stappen uit:

    ![SAML-eenmalige aanmelding](./media/bluejeans-tutorial/ic785869.png "SAML-eenmalige aanmelding")

    a. Selecteer **SAML-eenmalige aanmelding**.

    b. Selecteer **Automatische inrichting inschakelen**.

4. Ga verder met de volgende stappen:

    ![Certificaatpad](./media/bluejeans-tutorial/ic785870.png "Certificaatpad")

    a. Klik op **Bestand kiezen** om het base-64 gecodeerde certificaat dat u hebt gedownload vanuit de Azure-portal te uploaden.

    b. Plak in het tekstvak **Aanmeldings-URL** de waarde van **Aanmeldings-URL** die u hebt gekopieerd uit de Azure-portal.

    c. Plak in het tekstvak **Wachtwoord-URL wijzigen** de waarde van **Wachtwoord-URL wijzigen** die u hebt gekopieerd uit de Azure-portal.

    d. Plak in het tekstvak **Afmeldings-URL** de waarde van **Afmeldings-URL** die u hebt gekopieerd uit de Azure-portal.

5. Ga verder met de volgende stappen:

    ![Wijzigingen opslaan](./media/bluejeans-tutorial/ic785874.png "Wijzigingen opslaan")

    a. In de **gebruikers-Id** tekstvak, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.

    b. Typ `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name` in het tekstvak **E-mailadres**.

    c. Klik op **WIJZIGINGEN OPSLAAN**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. In de **gebruikersnaam** veldtype `brittasimon\@yourcompanydomain.extension`. Bijvoorbeeld BrittaSimon@contoso.com.

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In dit gedeelte gaat u Britta Simon toestemming geven voor gebruik van eenmalige aanmelding met Azure door haar toegang te geven tot BlueJeans.

1. Selecteer in de Azure-portal **Bedrijfstoepassingen**, selecteer **Alle toepassingen** en selecteer vervolgens **BlueJeans**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer **BlueJeans** in de lijst met toepassingen.

    ![De koppeling BlueJeans in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-bluejeans-test-user"></a>Een testgebruiker maken in BlueJeans

Het doel van dit gedeelte is het maken van een gebruiker met de naam Britta Simon in BlueJeans. BlueJeans ondersteunt het automatisch inrichten van gebruikers, wat standaard is ingeschakeld. U kunt [hier](bluejeans-provisioning-tutorial.md) meer informatie vinden over het configureren van het automatisch inrichten van gebruikers.

**Als u de gebruiker handmatig moet maken, voert u de volgende stappen uit:**

1. Aanmelden bij uw **BlueJeans** bedrijf site als beheerder.

2. Ga naar **ADMIN \> GEBRUIKERS BEHEREN \> GEBRUIKER TOEVOEGEN**.

    ![Admin](./media/bluejeans-tutorial/ic785877.png "Admin")

    > [!IMPORTANT]
    > Het tabblad **GEBRUIKER TOEVOEGEN** is alleen beschikbaar als op het tabblad **BEVEILIGING** **Automatische inrichting inschakelen** niet is ingeschakeld.

3. Voer in het gedeelte **GEBRUIKER TOEVOEGEN** de volgende stappen uit:

    ![Gebruiker toevoegen](./media/bluejeans-tutorial/ic785886.png "Gebruiker toevoegen")

    a. Voer in het tekstvak **Voornaam** de voornaam van de gebruiker in, zoals **Britta**.

    b. Typ in het tekstvak **Achternaam** de achternaam van de gebruiker, zoals **Simon**.

    c. Voer in het tekstvak **Pick a BlueJeans Username** de gebruikersnaam van de gebruiker in, bijvoorbeeld **Brittasimon**

    d. Voer in het tekstvak **Create a Password** uw wachtwoord in.

    e. Voer in het tekstvak **Company** de naam van uw bedrijf in.

    f. In **e-mailadres** tekst vak, voer het e-mailadres van gebruiker, zoals `brittasimon\@contoso.com`.

    g. Geef uw vergadering-id op in het tekstvak **Create a BlueJeans Meeting I.D**.

    h. Geef uw wachtwoordcode op in het tekstvak **Pick a Moderator Passcode**.

    i. Klik op **CONTINUE**.

    ![Gebruiker toevoegen](./media/bluejeans-tutorial/ic785887.png "Gebruiker toevoegen")

    J. Klik op **ADD USER**.

> [!NOTE]
> U kunt ook alle andere hulpprogramma's voor het creëren van BlueJeans-gebruikersaccounts of API's van BlueJeans gebruiken om Azure AD-gebruikersaccounts in te richten.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Bl in het toegangsvenster klikt, zou u automatisch moeten worden aangemeld bij de instantie van BlueJeans waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
