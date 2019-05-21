---
title: 'Zelfstudie: Azure Active Directory-integratie met SilkRoad leven Suite | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en SilkRoad leven Suite.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 3cd92319-7964-41eb-8712-444f5c8b4d15
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/16/2019
ms.author: jeedes
ms.openlocfilehash: bcad6232de7fa257b58fe6d84f2c2ff794b64589
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2019
ms.locfileid: "65902275"
---
# <a name="tutorial-azure-active-directory-integration-with-silkroad-life-suite"></a>Zelfstudie: Azure Active Directory-integratie met SilkRoad leven Suite

In deze zelfstudie leert u hoe u SilkRoad leven Suite integreren met Azure Active Directory (Azure AD).
SilkRoad leven Suite integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot SilkRoad leven Suite heeft.
* U kunt uw gebruikers worden automatisch aangemeld bij SilkRoad leven Suite (Single Sign-On) inschakelen met hun Azure AD-accounts.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met SilkRoad leven Suite, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/)
* Eenmalige aanmelding SilkRoad leven Suite ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Biedt ondersteuning voor SilkRoad leven Suite **SP** gestart door SSO

## <a name="adding-silkroad-life-suite-from-the-gallery"></a>SilkRoad leven Suite uit de galerie toe te voegen

Voor het configureren van de integratie van SilkRoad leven Suite in Azure AD, moet u SilkRoad leven Suite uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen SilkRoad leven Suite uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **SilkRoad leven Suite**, selecteer **SilkRoad leven Suite** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![SilkRoad leven Suite in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met SilkRoad leven Suite op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in SilkRoad leven Suite tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met SilkRoad leven Suite, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureer SilkRoad leven Suite Single Sign-On](#configure-silkroad-life-suite-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Maak SilkRoad leven Suite testgebruiker](#create-silkroad-life-suite-test-user)**  : als u wilt een equivalent van Britta Simon in SilkRoad leven Suite die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met SilkRoad leven Suite, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **SilkRoad leven Suite** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. Voer in de sectie **Standaard SAML-configuratie** de volgende stappen uit als u beschikt over een **bestand met metagegevens van de serviceprovider**:

    > [!NOTE]
    > U krijgt de **Service Provider-bestand met metagegevens** verderop in deze zelfstudie wordt uitgelegd.

    a. Klik op **Metagegevensbestand uploaden**.

    ![image](common/upload-metadata.png)

    b. Klik op **map logo** voor het selecteren van het bestand met metagegevens en klikt u op **uploaden**.

    ![image](common/browse-upload-metadata.png)

    c. Zodra het bestand met metagegevens is geüpload, de **id** en **antwoord-URL** waarden ophalen automatisch ingevuld in de sectie SAML-basisconfiguratie:

    ![image](common/sp-identifier-reply.png)

    > [!Note]
    > Als de waarden voor **Id** en **Antwoord-URL** niet automatisch worden ingevuld, kunt u de waarden zelf invullen afhankelijk van uw behoeften.

    d. In het tekstvak **Aanmeldings-URL** typt u een URL met het volgende patroon: `https://<subdomain>.silkroad-eng.com/Authentication/`

5. Op de **SAML-basisconfiguratie** sectie, als u nog geen **Service Provider-bestand met metagegevens**, voer de volgende stappen uit:

    ![SilkRoad leven Suite domein en URL's eenmalige aanmelding informatie](common/sp-identifier-reply.png)

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met het volgende patroon: `https://<subdomain>.silkroad-eng.com/Authentication/`

    b. Typ in het vak **Id** een URL met het volgende patroon:

    | |
    |--|
    | `https://<subdomain>.silkroad-eng.com/Authentication/SP`|
    | `https://<subdomain>.silkroad.com/Authentication/SP`|

    c. In het tekstvak **Antwoord-URL** typt u een URL met het volgende patroon:

    | |
    |--|
    | `https://<subdomain>.silkroad-eng.com/Authentication/`|
    | `https://<subdomain>.silkroad.com/Authentication/`|

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke aanmeldings-URL, id en antwoord-URL. Neem contact op met [SilkRoad leven Suite Client ondersteuningsteam](https://www.silkroad.com/locations/) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

6. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **XML-bestand met federatieve metagegevens**  te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/metadataxml.png)

7. Op de **SilkRoad leven Suite instellen** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-silkroad-life-suite-single-sign-on"></a>Levensduur van SilkRoad Suite eenmalige aanmelding configureren

1. Meld u aan uw bedrijf SilkRoad site als beheerder.

    > [!NOTE]
    > Neem contact op met ondersteuning voor SilkRoad of met uw vertegenwoordiger SilkRoad Services voor het verkrijgen van toegang tot de toepassing SilkRoad leven Suite verificatie voor federatie configureren met Microsoft Azure AD.

1. Ga naar **serviceprovider**, en klik vervolgens op **Federation Details**.

    ![Azure AD voor eenmalige aanmelding](./media/silkroad-life-suite-tutorial/tutorial_silkroad_06.png)

1. Klik op **Federatiemetagegevens downloaden**, en sla het bestand met metagegevens op uw computer. Gebruik van Federatiemetagegevens gedownload als een **Service Provider-bestand met metagegevens** in de **SAML-basisconfiguratie** sectie in Azure portal.

    ![Azure AD voor eenmalige aanmelding](./media/silkroad-life-suite-tutorial/tutorial_silkroad_07.png)

1. In uw **SilkRoad** toepassing, klikt u op **verificatie bronnen**.

    ![Azure AD voor eenmalige aanmelding](./media/silkroad-life-suite-tutorial/tutorial_silkroad_08.png) 

1. Klik op **verificatiebron toevoegen**.

    ![Azure AD voor eenmalige aanmelding](./media/silkroad-life-suite-tutorial/tutorial_silkroad_09.png)

1. In de **verificatiebron toevoegen** sectie, voert u de volgende stappen uit:

    ![Azure AD voor eenmalige aanmelding](./media/silkroad-life-suite-tutorial/tutorial_silkroad_10.png)
  
    a. Onder **optie 2 - bestand met metagegevens**, klikt u op **Bladeren** voor het uploaden van het gedownloade metagegevensbestand vanuit Azure portal.
  
    b. Klik op **maken id-Provider met behulp van gegevens uit een bestand**.

1. In de **verificatie bronnen** sectie, klikt u op **bewerken**.

    ![Azure AD voor eenmalige aanmelding](./media/silkroad-life-suite-tutorial/tutorial_silkroad_11.png)

1. Op de **verificatiebron bewerken** dialoogvenster, voer de volgende stappen uit:

    ![Azure AD voor eenmalige aanmelding](./media/silkroad-life-suite-tutorial/tutorial_silkroad_12.png)

    a. Als **ingeschakeld**, selecteer **Ja**.

    b. In de **EntityId** tekstvak, plak de waarde van **Azure AD-id** die u hebt gekopieerd vanuit Azure portal.

    c. In de **IdP beschrijving** tekstvak, typ een beschrijving voor uw configuratie (bijvoorbeeld: *Azure AD SSO*).

    d. In de **metagegevensbestand** tekstvak, Upload het **metagegevens** -bestand dat u hebt gedownload vanuit Azure portal.
  
    e. In de **IdP naam** tekstvak, typ een naam die specifiek is voor uw configuratie (bijvoorbeeld: *Azure SP*).
  
    f. In de **afmeldings-URL van Service** tekstvak, plak de waarde van **afmeldings-URL van** die u hebt gekopieerd vanuit Azure portal.

    g. In de **aanmeldings-URL van service** tekstvak, plak de waarde van **aanmeldings-URL** die u hebt gekopieerd vanuit Azure portal.

    h. Klik op **Opslaan**.

1. Alle andere verificatie-bronnen uitschakelen.

    ![Azure AD voor eenmalige aanmelding](./media/silkroad-life-suite-tutorial/tutorial_silkroad_13.png)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. In de **gebruikersnaam** veldtype `brittasimon@yourcompanydomain.extension`  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen tot SilkRoad leven Suite.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **SilkRoad leven Suite**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **SilkRoad leven Suite**.

    ![De koppeling SilkRoad leven Suite in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-silkroad-life-suite-test-user"></a>SilkRoad leven Suite testgebruiker maken

In deze sectie maakt u een gebruiker met de naam van Britta Simon in SilkRoad leven Suite. Werken met [SilkRoad leven Suite Client ondersteuningsteam](https://www.silkroad.com/locations/) om toe te voegen de gebruikers in het platform SilkRoad leven Suite. Er moeten gebruikers worden gemaakt en geactiveerd voordat u eenmalige aanmelding kunt gebruiken.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel SilkRoad leven Suite in het toegangsvenster, moet u worden automatisch aangemeld bij de SilkRoad leven Suite waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
