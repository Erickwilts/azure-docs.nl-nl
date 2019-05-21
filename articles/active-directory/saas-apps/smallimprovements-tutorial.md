---
title: 'Zelfstudie: Azure Active Directory-integratie met kleine verbeteringen | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory- en kleine verbeteringen.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 59c8a112-41e1-4337-9ef3-3d7029780d61
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/07/2019
ms.author: jeedes
ms.openlocfilehash: 19d9624c5bb60f47ef4bfa1b0629327780c2a9c7
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2019
ms.locfileid: "65901840"
---
# <a name="tutorial-azure-active-directory-integration-with-small-improvements"></a>Zelfstudie: Azure Active Directory-integratie met kleine verbeteringen

In deze zelfstudie leert u hoe u kleine verbeteringen integreren met Azure Active Directory (Azure AD).
Kleine verbeteringen integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot kleine verbeteringen heeft.
* U kunt uw gebruikers worden automatisch aangemeld kleine verbeteringen (Single Sign-On) inschakelen met hun Azure AD-accounts.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met kleine verbeteringen, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* Kleine verbeteringen eenmalige aanmelding ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Kleine verbeteringen ondersteunt **SP** gestart door SSO

## <a name="adding-small-improvements-from-the-gallery"></a>Kleine verbeteringen in de galerie toevoegen

Voor het configureren van de integratie van kleine verbeteringen in Azure AD, moet u kleine verbeteringen in de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen kleine verbeteringen in de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **kleine verbeteringen**, selecteer **kleine verbeteringen** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![Kleine verbeteringen in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met kleine verbeteringen op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in kleine verbeteringen tot stand worden gebracht.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met kleine verbeteringen, u nodig hebt voor de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureren van kleine verbeteringen Single Sign-On](#configure-small-improvements-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Kleine verbeteringen testgebruiker maken](#create-small-improvements-test-user)**  : als u wilt een equivalent van Britta Simon in kleine verbeteringen die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met kleine verbeteringen, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **kleine verbeteringen** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![Kleine verbeteringen domein en URL's, eenmalige aanmelding informatie](common/sp-identifier.png)

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<subdomain>.small-improvements.com`

    b. In het tekstvak **Id (Entiteits-id)** typt u een URL met de volgende notatie: `https://<subdomain>.small-improvements.com`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke aanmeldings-URL en -id. Neem contact op met [kleine verbeteringen Client ondersteuningsteam](mailto:support@small-improvements.com) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

6. Op de **instellen van kleine verbeteringen** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-small-improvements-single-sign-on"></a>Kleine verbeteringen Single Sign-On configureren

1. In een ander browservenster, meld u aan bij uw bedrijf kleine verbeteringen site als beheerder.

1. Klik op de pagina hoofddashboard **beheer** knop aan de linkerkant.

    ![Eenmalige aanmelding configureren](./media/smallimprovements-tutorial/tutorial_smallimprovements_06.png) 

1. Klik op de **SAML SSO** knop van **integraties** sectie.

    ![Eenmalige aanmelding configureren](./media/smallimprovements-tutorial/tutorial_smallimprovements_07.png) 

1. Voer de volgende stappen uit op de pagina instellingen voor eenmalige aanmelding:

    ![Eenmalige aanmelding configureren](./media/smallimprovements-tutorial/tutorial_smallimprovements_08.png)  

    a. In de **HTTP-eindpunt** tekstvak, plak de waarde van **aanmeldings-URL**, die u hebt gekopieerd vanuit Azure portal.

    b. Open het gedownloade certificaat in Kladblok, Kopieer de inhoud en plak deze in de **x509 certificaat** tekstvak. 

    c. Als u dat eenmalige aanmelding en meld u aan formulier optie voor verificatie beschikbaar voor gebruikers wilt, controleert u de **toegang via aanmelding en wachtwoord te inschakelen** optie.  

    d. Voer de juiste waarde als naam van de knop Aanmelden voor eenmalige aanmelding in de **SAML vragen** tekstvak.  

    e. Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. In het veld **Gebruikersnaam** typt u **brittasimon@yourcompanydomain.extension**.  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding toegang verlenen tot kleine verbeteringen.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **kleine verbeteringen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **kleine verbeteringen**.

    ![De koppeling kleine verbeteringen in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-small-improvements-test-user"></a>Kleine verbeteringen testgebruiker maken

Als u wilt dat Azure AD-gebruikers zich aanmelden bij kleine verbeteringen, moeten ze worden ingericht voor kleine verbeteringen. In het geval van kleine verbeteringen is inrichting een handmatige taak.

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**

1. Aanmelding bij uw bedrijf kleine verbeteringen site als beheerder.

1. Vanaf de startpagina, gaat u naar het menu aan de linkerkant, klik op **beheer**.

1. Klik op de **gebruikerslijst** knop in de sectie beheer van gebruikers.

    ![Het maken van een Azure AD-testgebruiker](./media/smallimprovements-tutorial/tutorial_smallimprovements_10.png) 

1. Klik op **gebruikers toevoegen**.

    ![Het maken van een Azure AD-testgebruiker](./media/smallimprovements-tutorial/tutorial_smallimprovements_11.png) 

1. Op de **gebruikers toevoegen** dialoogvenster, voer de volgende stappen uit: 

    ![Het maken van een Azure AD-testgebruiker](./media/smallimprovements-tutorial/tutorial_smallimprovements_12.png)

    a. Voer de **voornaam** van gebruiker, zoals **Julia**.

    b. Voer de **achternaam** van gebruiker, zoals **Simon**.

    c. Voer de **e** van gebruiker, zoals **brittasimon@contoso.com**.

    d. U kunt er ook voor kiezen om in te voeren van het persoonlijke bericht in de **e-mailmelding verzenden** vak. Als u niet verzenden van de melding wilt, schakel dit selectievakje in.

    e. Klik op **maken gebruikers**.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel kleine verbeteringen in het toegangsvenster, moet u worden automatisch aangemeld bij de kleine verbeteringen waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)