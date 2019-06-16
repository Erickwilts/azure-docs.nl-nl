---
title: 'Zelfstudie: Azure Active Directory-integratie met Elium | Microsoft Docs'
description: Ontdek hoe u eenmalige aanmelding configureert tussen Azure Active Directory en Elium.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: fae344b3-5bd9-40e2-9a1d-448dcd58155f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/05/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: b714983f1401ba60357a8642e0a56422d01be74e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67103428"
---
# <a name="tutorial-azure-active-directory-integration-with-elium"></a>Zelfstudie: Azure Active Directory-integratie met Elium

In deze zelfstudie leert u hoe u Elium kunt integreren met Azure Active Directory (Azure AD).
Het integreren van Elium met Azure AD biedt u de volgende voordelen:

* U kunt in Azure AD beheren wie toegang heeft tot Elium.
* U kunt uw gebruikers zich automatisch laten aanmelden bij Elium (eenmalige aanmelding) met hun Azure AD-account.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Elium hebt u het volgende nodig:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* Een abonnement op Elium waarvoor eenmalige aanmelding is ingeschakeld

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Elium ondersteunt door **SP** en **IDP** geïnitieerde eenmalige aanmelding (SSO)

* Elium ondersteunt het **Just-In-Time** inrichten van gebruikers

## <a name="adding-elium-from-the-gallery"></a>Elium toevoegen vanuit de galerie

Om de integratie van Elium in Azure AD te configureren, moet u Elium uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Ga als volgt te werk om Elium toe te voegen vanuit de galerie:**

1. In de **[Azure-portal](https://portal.azure.com)** , klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **Elium**, selecteer **Elium** in het resultaatvenster en klik op de knop **Toevoegen** om de toepassing toe te voegen.

     ![Elium in de resultatenlijst](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In dit gedeelte configureert en test u eenmalige aanmelding van Azure AD met Elium op basis van een testgebruiker met de naam **Britta Simon**.
Eenmalige aanmelding werkt alleen als er een koppelingsrelatie tussen een Azure AD-gebruiker en de daaraan gerelateerde gebruiker in Elium tot stand is gebracht.

Voor het configureren en testen van Azure AD-eenmalige aanmelding met Elium moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)** : als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Eenmalige aanmelding voor Elium configureren](#configure-elium-single-sign-on)** : als u de instellingen voor eenmalige aanmelding aan de clientzijde wil configureren.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)** : als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)** : als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Testgebruiker voor Elium maken](#create-elium-test-user)** : als u een tegenhanger van Britta Simon in Elium wilt hebben die is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)** : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Ga als volgt te werk om Azure AD-eenmalige aanmelding met Elium te configureren:

1. In de [Azure-portal](https://portal.azure.com/) selecteert u **Eenmalige aanmelding** op de integratiepagina van de toepassing **Elium**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit als u de toepassing in de door **IDP** geïnitieerde modus wilt configureren:

    ![Domein- en URL-gegevens voor eenmalige aanmelding bij Elium](common/idp-intiated.png)

    a. In het tekstvak **Id** typt u een URL met het volgende patroon: `https://<platform-domain>.elium.com/login/saml2/metadata`

    b. In het tekstvak **Antwoord-URL** typt u een URL met de volgende notatie: `https://<platform-domain>.elium.com/login/saml2/acs`

5. Klik op **Extra URL's instellen** en voer de volgende stap uit als u de toepassing in de door **SP** geïnitieerde modus wilt configureren:

    ![Domein- en URL-gegevens voor eenmalige aanmelding bij Elium](common/metadata-upload-additional-signon.png)

    In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<platform-domain>.elium.com/login/saml2/login`

    > [!NOTE] 
    > Dit zijn geen echte waarden. U verkrijgt deze waarden uit het **SP-metagegevensbestand** dat u kunt downloaden van `https://<platform-domain>.elium.com/login/saml2/metadata`, zoals later in deze zelfstudie wordt uitgelegd.

6. De Elium-toepassing verwacht de SAML-asserties in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit de sectie **Gebruikerskenmerken** op de integratiepagina van de toepassing-beheren. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op de knop **Bewerken** om het dialoogvenster **Gebruikerskenmerken** te openen.

    ![image](common/edit-attribute.png)

7. Bewerk in het gedeelte **Gebruikersclaims** in het dialoogvenster **Gebruikerskenmerken** de claims met het **pictogram Bewerken** of voeg de claims toe door met **Nieuwe claim toevoegen** het kenmerk van het SAML-token te configureren, zoals wordt weergegeven in de bovenstaande afbeelding. Hierna voert u de volgende stappen uit: 

    | Name | Bronkenmerk|
    | ---------------| ----------------|
    | email   |user.mail |
    | first_name| user.givenname |
    | last_name| user.surname|
    | job_title| user.jobtitle|
    | company| user.companyname|

    > [!NOTE]
    > Dit zijn de standaardclaims. **Alleen de email-claim is vereist**. Voor JIT-inrichting is ook alleen de email-claim verplicht. Andere aangepaste claims kunnen variëren per klantenplatform.

    a. Klik op **Nieuwe claim toevoegen** om het dialoogvenster **Gebruikersclaims beheren** te openen.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.

    c. Laat **Naamruimte** leeg.

    d. Selecteer Bron bij **Kenmerk**.

    e. Typ de kenmerkwaarde voor die rij in de lijst met **bronkenmerken**.

    f. Klik op **OK**.

    g. Klik op **Opslaan**.

8. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **XML-bestand met federatieve metagegevens**  te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/metadataxml.png)

9. In de sectie **Elium instellen** kopieert u de juiste URL('s) op basis van uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-elium-single-sign-on"></a>Elium voor eenmalige aanmelding configureren

1. Meld u in een andere browser als beheerder aan bij uw bedrijfssite in Elium.

2. Klik op het **User profile** in de rechterbovenhoek en selecteer **Administration**.

    ![Eenmalige aanmelding configureren](./media/elium-tutorial/user1.png)

3. Selecteer het tabblad **Security**.

    ![Eenmalige aanmelding configureren](./media/elium-tutorial/user2.png)

4. Schuif omlaag naar de sectie **Single sign-on (SSO)** en voer de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/elium-tutorial/user3.png)

    a. Kopieer de waarde van **Verify that SAML2 authentication works for your account** en plak deze in het tekstvak **Aanmeldings-URL** in de sectie **Standaard SAML-configuratie** van de Azure-portal.

    > [!NOTE]
    > Nadat u SSO hebt geconfigureerd, hebt u altijd toegang tot de standaardpagina voor extern aanmelden op de volgende URL: `https://<platform_domain>/login/regular/login` 

    b. Selecteer het selectievakje **Enable SAML2 federation**.

    c. Selecteer het selectievakje **JIT Provisioning**.

    d. Open de **SP Metadata** door op de knop **Download** te klikken.

    e. Zoek naar de **entityID** in het **SP Metadata**-bestand, kopieer de waarde van **entityID** en plak deze in het tekstvak **ID** van de sectie **Standaard SAML-configuratie** van de Azure-portal. 

    ![Eenmalige aanmelding configureren](./media/elium-tutorial/user4.png)

    f. Zoek naar de **AssertionConsumerService** in het **SP Metadata**-bestand, kopieer de waarde van **Location** en plak deze in het tekstvak **Antwoord-URL** van de sectie **Standaard SAML-configuratie** van de Azure-portal.

    ![Eenmalige aanmelding configureren](./media/elium-tutorial/user5.png)

    g. Open het uit de Azure-portal gedownloade metagegevensbestand in Kladblok, kopieer de inhoud en plak deze in het tekstvak **IdP Metadata**.

    h. Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken 

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon** in.
  
    b. In de **gebruikersnaam** veld, typt u **brittasimon\@yourcompanydomain.extension**  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In dit gedeelte gaat u Britta Simon toestemming geven voor gebruik van eenmalige aanmelding met Azure door haar toegang te geven tot Elium.

1. Selecteer **Bedrijfstoepassingen** in de Azure-portal, selecteer **Alle toepassingen** en selecteer vervolgens **Elium**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer **Elium** in de lijst met toepassingen.

    ![Elium in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst Gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-assertie, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onderaan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-elium-test-user"></a>Elium-testgebruiker maken

In dit gedeelte maakt u in Elium een gebruiker met de naam Britta Simon. Elium ondersteunt **Just-In-Time-inrichting**; deze optie is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Als er nog geen gebruiker in Elium bestaat, wordt er een nieuwe gemaakt wanneer u Elium probeert te openen.

>[!Note]
>Als u handmatig een gebruiker wilt maken, neem dan contact op met het [ondersteuningsteam van Elium](mailto:support@elium.com).

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Elium in het toegangsvenster klikt, wordt u automatisch aangemeld bij de instantie van Elium waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

