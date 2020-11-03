---
title: 'Zelfstudie: Azure Active Directory-integratie met Splunk Enterprise en Splunk Cloud | Microsoft Docs'
description: Ontdek hoe u eenmalige aanmelding configureert tussen Azure Active Directory en Splunk Enterprise en Splunk Cloud.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 12/24/2018
ms.author: jeedes
ms.openlocfilehash: b989d6191180cefabf4754786e1b93b38b3ae141
ms.sourcegitcommit: 59f506857abb1ed3328fda34d37800b55159c91d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2020
ms.locfileid: "92517892"
---
# <a name="tutorial-azure-active-directory-integration-with-splunk-enterprise-and-splunk-cloud"></a>Zelfstudie: Azure Active Directory-integratie met Splunk Enterprise en Splunk Cloud

In deze zelfstudie leert u hoe u Splunk Enterprise en Splunk Cloud integreert in Azure Active Directory (Azure AD).
De integratie van Splunk Enterprise en Splunk Cloud met Azure AD heeft de volgende voordelen:

* U kunt in Azure AD bepalen wie toegang krijgt tot Splunk Enterprise en Splunk Cloud.
* U kunt inschakelen dat gebruikers automatisch met hun Azure AD-account worden aangemeld bij Splunk Enterprise en Splunk Cloud (eenmalige aanmelding).
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Als u de Azure AD-integratie met Splunk Enterprise en Splunk Cloud wilt configureren, hebt u de volgende zaken nodig:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* Abonnement voor Splunk Enterprise en Splunk Cloud waarvoor eenmalige aanmelding is ingeschakeld

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Splunk Enterprise en Splunk Cloud bieden ondersteuning voor door **SP** geïnitieerde eenmalige aanmelding

## <a name="adding-splunk-enterprise-and-splunk-cloud-from-the-gallery"></a>Splunk Enterprise en Splunk Cloud toevoegen vanuit de galerie

Als u de integratie van Splunk Enterprise en Splunk Cloud in Azure AD wilt configureren, moet u Splunk Enterprise en Splunk Cloud vanuit de galerie toevoegen aan uw lijst beheerde SaaS-apps.

**Voer de volgende stappen uit om Splunk Enterprise en Splunk Cloud vanuit de galerie toe te voegen:**

1. Klik in het linkernavigatievenster in de **[Azure-portal](https://portal.azure.com)** op het **Azure Active Directory** -pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ **Splunk Enterprise en Splunk Cloud** in het zoekvak, selecteer **Splunk Enterprise en Splunk Cloud** in het resultatendeelvenster en klik dan op **Toevoegen** om de toepassing toe te voegen.

     ![Splunk Enterprise en Splunk Cloud in de lijst resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In dit gedeelte configureert en test u eenmalige aanmelding van Azure AD met Splunk Enterprise en Splunk Cloud op basis van een testgebruiker met de naam **Britta Simon**.
Eenmalige aanmelding werkt alleen als er een koppelingsrelatie tussen een Azure AD-gebruiker en de daaraan gerelateerde gebruiker in Splunk Enterprise en Splunk Cloud tot stand is gebracht.

Voltooi de volgende bouwstenen om eenmalige aanmelding van Azure AD voor Splunk Enterprise en Splunk Cloud te configureren en testen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)** : als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Eenmalige aanmelding bij Splunk Enterprise en Splunk Cloud configureren](#configure-splunk-enterprise-and-splunk-cloud-single-sign-on)** : de instellingen voor eenmalige aanmelding aan de toepassingszijde configureren.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)** : als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)** : als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Een Splunk Enterprise- en Splunk Cloud-testgebruiker maken](#create-splunk-enterprise-and-splunk-cloud-test-user)** : als u een equivalent van Britta Simon wilt maken in Splunk Enterprise en Splunk Cloud dat is gekoppeld aan de Azure AD-versie van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)** : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Als u de eenmalige aanmelding van Azure AD bij Splunk Enterprise en Splunk Cloud wilt configureren, voert u de volgende stappen uit:

1. In [Azure Portal](https://portal.azure.com/) selecteert u op de toepassingsintegratiepagina van **Splunk Enterprise en Splunk Cloud** de optie **Eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![Informatie over Splunk Enterprise- en Splunk Cloud-domeinen en URL's voor eenmalige aanmelding](common/sp-identifier-reply.png)

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met het volgende patroon: `https://<splunkserverUrl>/en-US/app/launcher/home`

    b. In het tekstvak **Id** typt u een URL met het volgende patroon: `<splunkserverUrl>`

    c. In het tekstvak **Antwoord-URL** typt u een URL met de volgende notatie: `https://<splunkserver>/saml/acs`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke id, de antwoord-URL en de aanmeldings-URL. Neem contact op met het [Splunk Enterprise- en Splunk Cloud Client-ondersteuningsteam](https://www.splunk.com/en_us/about-splunk/contact-us.html) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **XML-bestand met federatieve metagegevens**  te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/metadataxml.png)

### <a name="configure-splunk-enterprise-and-splunk-cloud-single-sign-on"></a>Eenmalige aanmelding configureren voor Splunk Enterprise en Splunk Cloud

Als u eenmalige aanmelding wilt configureren aan de **Splunk Enterprise- en Splunk Cloud** -zijde, moet u het gedownloade **XML-bestand met federatieve metagegevens** en de juiste gekopieerde URL's uit Azure Portal verzenden naar het [Splunk Enterprise- en Splunk Cloud-ondersteuningsteam](https://www.splunk.com/en_us/about-splunk/contact-us.html). Het team stelt de instellingen zo in dat de verbinding tussen SAML en eenmalige aanmelding aan beide zijden goed is ingesteld.

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

In dit gedeelte hebt u Britta Simon in staat gesteld gebruik te maken van eenmalige aanmelding van Azure door haar toegang te geven tot Splunk Enterprise en Splunk Cloud.

1. Selecteer in Azure Portal **Bedrijfstoepassingen** , selecteer **Alle toepassingen** en selecteer vervolgens **Splunk Enterprise en Splunk Cloud**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Typ in de lijst met toepassingen **Splunk Enterprise en Splunk Cloud** en selecteer deze optie.

    ![De koppeling naar Splunk Enterprise en Splunk Cloud in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop **Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-splunk-enterprise-and-splunk-cloud-test-user"></a>Een testgebruiker maken voor Splunk Enterprise en Splunk Cloud

In dit gedeelte maakt u de gebruiker Britta Simon in Splunk Enterprise en Splunk Cloud. Neem contact op met het [ondersteuningsteam van Splunk Enterprise en Splunk Cloud](https://www.splunk.com/en_us/about-splunk/contact-us.html) om de gebruikers toe te voegen in het Splunk Enterprise- en Splunk Cloud-platform. Er moeten gebruikers worden gemaakt en geactiveerd voordat u eenmalige aanmelding kunt gebruiken.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie gaat u uw configuratie van Azure AD-eenmalige aanmelding testen via het toegangsvenster.

Wanneer u op de Splunk Enterprise- en Splunk Cloud-tegel in het toegangsvenster klikt, wordt u automatisch aangemeld bij de Splunk Enterprise- en Splunk Cloud-versie waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](../user-help/my-apps-portal-end-user-access.md) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](./tutorial-list.md)

- [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](../conditional-access/overview.md)