---
title: 'Zelfstudie: Integratie van Azure Active Directory met Picturepark | Microsoft Docs'
description: Informatie over hoe u eenmalige aanmelding configureert tussen Azure Active Directory en Picturepark.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 04/18/2019
ms.author: jeedes
ms.openlocfilehash: 8a00cf11edfea2e732a18a392d465525b38ea45f
ms.sourcegitcommit: 59f506857abb1ed3328fda34d37800b55159c91d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2020
ms.locfileid: "92520841"
---
# <a name="tutorial-azure-active-directory-integration-with-picturepark"></a>Zelfstudie: Integratie van Azure Active Directory met Picturepark

In deze zelfstudie leert u hoe u Picturepark kunt integreren met Azure Active Directory (Azure AD).
De integratie van Picturepark met Azure AD biedt de volgende voordelen:

* U kunt in Azure AD bepalen wie toegang tot Picturepark heeft.
* U kunt instellen dat gebruikers automatisch met hun Azure AD-account worden aangemeld bij Picturepark (eenmalige aanmelding).
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Picturepark hebt u het volgende nodig:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u een [gratis account](https://azure.microsoft.com/free/) krijgen
* Een abonnement op Picturepark waarvoor eenmalige aanmelding is ingeschakeld

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Picturepark biedt ondersteuning voor door **SP** geïnitieerde eenmalige aanmelding

## <a name="adding-picturepark-from-the-gallery"></a>Picturepark toevoegen vanuit de galerie

Om de integratie van Picturepark te configureren in Azure AD, moet u Picturepark vanuit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Voer de volgende stappen uit om Picturepark toe te voegen vanuit de galerie:**

1. Klik in het linkernavigatievenster in de **[Azure-portal](https://portal.azure.com)** op het **Azure Active Directory** -pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen** .

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ **Picturepark** in het zoekvak, selecteer **Picturepark** in het resultatenvenster en klik op de knop **Toevoegen** om de toepassing toe te voegen.

     ![Picturepark in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In dit gedeelte configureert en test u eenmalige aanmelding van Azure AD met Picturepark op basis van een testgebruiker met de naam **Britta Simon** .
Eenmalige aanmelding werkt alleen als er een koppelingsrelatie tussen een Azure AD-gebruiker en de daaraan gerelateerde gebruiker in Picturepark tot stand is gebracht.

Om eenmalige aanmelding van Azure AD met Picturepark te configureren en testen, moet u de volgende procedures voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)** : als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Eenmalige aanmelding van Picturepark configureren](#configure-picturepark-single-sign-on)** : de instellingen voor eenmalige aanmelding aan de toepassingszijde configureren.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)** : als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)** : als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Een testgebruiker voor Picturepark maken](#create-picturepark-test-user)** : als u een tegenhanger van Britta Simon in Picturepark wilt hebben die is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)** : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voer de volgende stappen uit om eenmalige aanmelding van Azure AD te configureren met Picturepark:

1. Ga in de [Azure-portal](https://portal.azure.com/) naar de pagina van de integratie van **Picturepark** en selecteer **Eenmalige aanmelding** .

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![Domein- en URL-gegevens voor eenmalige aanmelding bij Picturepark](common/sp-identifier.png)

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<companyname>.picturepark.com`

    b. In het tekstvak **Id (Entiteits-id)** typt u een URL met het volgende patroon:

    ```http
        https://<companyname>.current-picturepark.com
        https://<companyname>.picturepark.com
        https://<companyname>.next-picturepark.com
    ```

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke aanmeldings-URL en id. Neem contact op met het [ondersteuningsteam van Picturepark](https://picturepark.com/company/picturepark-customer-support) om deze waarden op te vragen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

5. Klik in de sectie **SAML-handtekeningcertificaat** op de knop **Bewerken** om het dialoogvenster **SAML-handtekeningcertificaat** te openen.

    ![SAML-handtekeningcertificaat bewerken](common/edit-certificate.png)

6. Kopieer in de sectie **SAML-handtekeningcertificaat** de waarde voor **VINGERAFDRUK** en sla deze op de computer op.

    ![Waarde van vingerafdruk kopiëren](common/copy-thumbprint.png)

7. Kopieer in de sectie **Picturepark instellen** de juiste URL('s) op basis van uw behoeften. Gebruik voor de **aanmeldings-URL** de waarde met het volgende patroon: `https://login.microsoftonline.com/_my_directory_id_/wsfed`

    > [!Note]
    > _my_directory_id_ is de tenant-id van het Azure AD-abonnement.

    ![Configuratie-URL's kopiëren](./media/picturepark-tutorial/configurls.png)

    a. Azure AD-id

    b. Afmeldings-URL

### <a name="configure-picturepark-single-sign-on"></a>Eenmalige aanmelding voor Picturepark configureren

1. Meld u in een andere browser als beheerder aan bij uw bedrijfssite in Picturepark.

2. Klik in de werkbalk bovenaan op **Administrative tools** en klik vervolgens op **Management Console** .
   
    ![Management Console](./media/picturepark-tutorial/ic795062.png "Beheerconsole")

3. Klik op **Authentication** en vervolgens op **Identity providers** .
   
    ![Verificatie](./media/picturepark-tutorial/ic795063.png "Verificatie")

4. Voer op de configuratiepagina **Identity provider configuration** de volgende stappen uit:
   
    ![Identity provider configuration](./media/picturepark-tutorial/ic795064.png "Configuratie van identiteitsprovider")
   
    a. Klik op **Add** .
  
    b. Typ een naam voor uw configuratie.
   
    c. Selecteer **Set as default** .
   
    d. Plak in het tekstvak **Issuer URI** de waarde van de **aanmeldings-URL** die u hebt gekopieerd uit de Azure-portal.
   
    e. Plak in het tekstvak **Trusted Issuer Thumb Print** de waarde van de **vingerafdruk** die u hebt gekopieerd uit de sectie **SAML-handtekeningcertificaat** . 

5. Klik op **JoinDefaultUsersGroup** .

6. Als u het kenmerk **EmailAddress** in het tekstvak **Claim** wilt instellen, typt u `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` en klikt u op **Save** .

      ![Configuratie](./media/picturepark-tutorial/ic795065.png "Configuratie")

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken 

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory** , selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers** .

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon** in.
  
    b. In het veld **Gebruikersnaam** typt u `brittasimon@yourcompanydomain.extension`. Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create** .

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In dit gedeelte gaat u Britta Simon toestemming geven voor gebruik van eenmalige aanmelding met Azure door haar toegang te geven tot Picturepark.

1. Selecteer **Bedrijfstoepassingen** in de Azure-portal, selecteer **Alle toepassingen** en selecteer vervolgens **Picturepark** .

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer **Picturepark** in de lijst met toepassingen.

    ![De Picturepark-koppeling in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen** .

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop **Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen** .

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen** .

### <a name="create-picturepark-test-user"></a>Picturepark-testgebruiker maken

Als u wilt dat Azure AD-gebruikers zich kunnen aanmelden bij Picturepark, moeten ze worden ingericht bij Picturepark. In het geval van Picturepark is inrichten een handmatige taak.

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**

1. Meld u aan bij uw **Picturepark** -tenant.

1. Klik in de werkbalk bovenaan op **Administrative tools** en klik vervolgens op **Users** .
   
    ![Gebruikers](./media/picturepark-tutorial/ic795067.png "Gebruikers")

1. Klik op het tabblad **Users overview** op **New** .
   
    ![Gebruikersbeheer](./media/picturepark-tutorial/ic795068.png "Gebruikersbeheer")

1. Voer in het dialoogvenster **Create User** de volgende stappen uit voor een geldige Azure Active Directory-gebruiker die u wilt inrichten:
   
    ![Create User](./media/picturepark-tutorial/ic795069.png "Gebruiker maken")
   
    a. Typ in het tekstvak **Email Address** het **e-mailadres** van de gebruiker `BrittaSimon@contoso.com`.  
   
    b. Typ in de tekstvakken **Password** en **Confirm Password** het **wachtwoord** van Britta Simon. 
   
    c. Typ in het tekstvak **First Name** de **voornaam** van de gebruiker **Britta** . 
   
    d. Typ in het tekstvak **Last Name** de **achternaam** van de gebruiker **Simon** .
   
    e. Typ in het tekstvak **Company** de **bedrijfsnaam** van de gebruiker. 
   
    f. Selecteer in het tekstvak **Country** **het land/de regio** van de gebruiker.
  
    g. Typ in het tekstvak **ZIP** de **postcode** van de plaats.
   
    h. Typ in het tekstvak **City** de **plaatsnaam** van de gebruiker.

    i. Selecteer een **taal** .
   
    j. Klik op **Create** .

>[!NOTE]
>U kunt ook alle andere hulpprogramma's voor het maken van gebruikersaccounts of API's van Picturepark gebruiken om Microsoft Azure Active Directory-gebruikersaccounts in te richten.
>

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie gaat u uw configuratie van Azure AD-eenmalige aanmelding testen via het toegangsvenster.

Wanneer u in het toegangsvenster op de tegel Picturepark klikt, wordt u automatisch aangemeld bij de instantie van Picturepark waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](../user-help/my-apps-portal-end-user-access.md) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](./tutorial-list.md)

- [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](../conditional-access/overview.md)