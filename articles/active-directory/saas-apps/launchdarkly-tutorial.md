---
title: 'Zelfstudie: Azure Active Directory-integratie met LaunchDarkly | Microsoft Docs'
description: Leer hoe u eenmalige aanmelding kunt configureren tussen Azure Active Directory en LaunchDarkly.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/02/2019
ms.author: jeedes
ms.openlocfilehash: 1ffa5d359e689220bd8cdbc7b9f6e305f451269a
ms.sourcegitcommit: 9b8425300745ffe8d9b7fbe3c04199550d30e003
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/23/2020
ms.locfileid: "92458747"
---
# <a name="tutorial-azure-active-directory-integration-with-launchdarkly"></a>Zelfstudie: Azure Active Directory-integratie met LaunchDarkly

In deze zelfstudie leert u hoe u LaunchDarkly kunt integreren met Azure Active Directory (Azure AD).
De integratie van LaunchDarkly met Azure AD heeft de volgende voordelen:

* U kunt in Azure AD bepalen wie toegang heeft tot LaunchDarkly.
* U kunt uw gebruikers zich automatisch laten aanmelden bij LaunchDarkly (eenmalige aanmelding) met hun Azure AD-account.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met LaunchDarkly hebt u het volgende nodig:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* Een abonnement op LaunchDarkly waarvoor eenmalige aanmelding is ingeschakeld

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* LaunchDarkly ondersteunt door **SP en IDP** geïnitieerde eenmalige aanmelding
* LaunchDarkly ondersteunt het **Just In Time** inrichten van gebruikers

## <a name="adding-launchdarkly-from-the-gallery"></a>LaunchDarkly toevoegen vanuit de galerie

Voor het configureren van de integratie van LaunchDarkly in Azure AD moet u LaunchDarkly uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Voer de volgende stappen uit om LaunchDarkly toe te voegen vanuit de galerie:**

1. Klik in het linkernavigatievenster in de **[Azure-portal](https://portal.azure.com)** op het **Azure Active Directory** -pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ **LaunchDarkly** in het zoekvak, selecteer **LaunchDarkly** in het deelvenster met resultaten en klik op de knop **Toevoegen** om de toepassing toe te voegen.

     ![LaunchDarkly in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie gaat u Azure AD-eenmalige aanmelding met [toepassingsnaam] configureren en testen met behulp van een testgebruiker met de naam **Britta Simon**.
Eenmalige aanmelding werkt alleen als er een koppelingsrelatie tussen een Azure AD-gebruiker en de daaraan gerelateerd gebruiker in [toepassingsnaam] tot stand is gebracht.

Als u Azure AD-eenmalige aanmelding met [toepassingsnaam] wilt configureren en testen, moet u de volgende stappen uitvoeren:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)** : als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Eenmalige aanmelding voor LaunchDarkly configureren](#configure-launchdarkly-single-sign-on)** : om de instellingen voor eenmalige aanmelding aan de clientzijde te configureren.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)** : als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)** : als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Een testgebruiker maken in LaunchDarkly](#create-launchdarkly-test-user)** : om in LaunchDarkly een tegenhanger van Britta Simon te hebben die is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)** : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD-eenmalige aanmelding met [toepassingsnaam], moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/) selecteert u **Eenmalige aanmelding** op de pagina voor integratie van toepassingen met **LaunchDarkly**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit als u de toepassing in de door **IDP** geïnitieerde modus wilt configureren:

    ![Schermopname die de Standaard SAML-configuratie toont, waar u de id en URL kunt invoeren en vervolgens Opslaan selecteert.](common/idp-intiated.png)

    a. In het tekstvak **Id** typt u een URL: `app.launchdarkly.com`

    b. In het tekstvak **Antwoord-URL** typt u een URL met de volgende notatie: `https://app.launchdarkly.com/trust/saml2/acs/<customers-unique-id>`

    > [!NOTE]
    > De waarde van de antwoord-URL is niet de echte waarde. U werkt de waarde bij met de werkelijke antwoord-URL, zoals later in de zelfstudie wordt uitgelegd. Als u de toepassing wilt gebruiken in **IDP** -modus, moet u het veld **Aanmeldings-URL** leeg laten, anders kunt u het aanmelding niet initiëren vanuit de **IDP**. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

5. Klik op **Extra URL's instellen** en voer de volgende stap uit als u de toepassing in de door **SP** geïnitieerde modus wilt configureren:

    In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://app.launchdarkly.com`

    ![Schermopname die Extra URL's instellen toont, waar u een aanmeldings-URL kunt invoeren.](common/metadata-upload-additional-signon.png)

6. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

7. In het gedeelte **LaunchDarkly instellen** kopieert u de juiste URL('s) op basis van uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-launchdarkly-single-sign-on"></a>Eenmalige aanmelding voor LaunchDarkly configureren

1. Meld u in een ander webbrowservenster bij uw LaunchDarkly-bedrijfssite aan als beheerder.

2. Selecteer **Accountinstellingen** in het linkernavigatievenster.

    ![Schermopname met het item Accountinstellingen geselecteerd in Productie.](./media/launchdarkly-tutorial/configure1.png)

3. Klik op het tabblad **Beveiliging**.

    ![Schermopname van het tabblad Beveiliging van Accountinstellingen.](./media/launchdarkly-tutorial/configure2.png)

4. Klik op **EENMALIGE AANMELDING INSCHAKELEN** en vervolgens **SAML-CONFIGURATIE BEWERKEN**.

    ![Schermopname van de pagina voor eenmalige aanmelding waar u S S O kunt inschakelen en de SAML-configuratie kunt bewerken.](./media/launchdarkly-tutorial/configure3.png)

5. In het gedeelte **Uw SAML-configuratie bewerken** voert u de volgende stappen uit:

    ![Schermopname van de sectie Uw SAML-configuratie bewerken, waar u de hier beschreven wijzigingen kunt aanbrengen.](./media/launchdarkly-tutorial/configure4.png)

    a. Kopieer de **SAML-consumptieservice-URL** voor uw instantie en plak deze in het tekstvak Antwoord-URL in het gedeelte **Domein en URL's voor LaunchDarkly** van Azure Portal.

    b. Plak in het tekstvak voor de **URL voor eenmalige aanmelding** de waarde van de **aanmeldings-URL** die u uit de Azure-portal hebt gekopieerd.

    c. Open het van de Azure-portal gedownloade certificaat in Kladblok, kopieer de inhoud en plak deze in het vak **X.509-certificaat**. U kunt het certificaat ook rechtstreeks uploaden door op **een uploaden** te klikken.

    d. Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory** , selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon** in.
  
    b. In het veld **Gebruikersnaam** typt u **brittasimon\@yourcompanydomain.extension**  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In dit gedeelte gaat u Britta Simon toestemming geven voor gebruik van eenmalige aanmelding met Azure door haar toegang te geven tot LaunchDarkly.

1. Selecteer in de Azure-portal **Bedrijfstoepassingen** , selecteer **Alle toepassingen** en selecteer vervolgens **LaunchDarkly**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer **LaunchDarkly** in de lijst met toepassingen.

    ![De koppeling LaunchDarkly in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop **Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-launchdarkly-test-user"></a>Een testgebruiker maken in LaunchDarkly

Het doel van dit gedeelte is het maken van een gebruiker met de naam Britta Simon in LaunchDarkly. LaunchDarkly ondersteunt just-in-time-inrichting, wat standaard is ingeschakeld. Er is geen actie-item voor u in deze sectie. Bij een poging LaunchDarkly te gebruiken wordt er een nieuwe gebruiker gemaakt als deze nog niet bestaat.

> [!Note]
> Neem contact op met het [klantondersteuningsteam van LaunchDarkly](mailto:support@launchdarkly.com) als u handmatig een gebruiker moet maken.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie gaat u uw configuratie van Azure AD-eenmalige aanmelding testen via het toegangsvenster.

Wanneer u op de tegel LaunchDarkly in het toegangsvenster klikt, zou u automatisch moeten worden aangemeld bij de instantie van LaunchDarkly waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](../user-help/my-apps-portal-end-user-access.md) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](./tutorial-list.md)

- [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](../conditional-access/overview.md)