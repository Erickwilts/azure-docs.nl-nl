---
title: 'Zelfstudie: Eenmalige aanmelding van Azure Active Directory integreren met OpenAthens | Microsoft Docs'
description: Leer hoe u eenmalige aanmelding tussen Azure Active Directory en OpenAthens configureert.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 10/24/2019
ms.author: jeedes
ms.openlocfilehash: da8ae35ce85ca9ffb031511e81270afd8529681d
ms.sourcegitcommit: 83610f637914f09d2a87b98ae7a6ae92122a02f1
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/13/2020
ms.locfileid: "91994190"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-openathens"></a>Zelfstudie: Eenmalige aanmelding van Azure Active Directory integreren met OpenAthens

In deze zelfstudie leert u hoe u OpenAthens kunt integreren met Azure Active Directory (Azure AD). Wanneer u OpenAthens integreert met Azure AD, kunt u het volgende:

* In Azure AD bepalen wie er toegang heeft tot OpenAthens.
* Ervoor zorgen dat gebruikers zich automatisch met hun Azure AD-account kunnen aanmelden bij OpenAthens.
* Uw accounts op een centrale locatie beheren: Azure Portal.

Zie [Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) voor meer informatie over de integratie van SaaS-apps met Azure AD.

## <a name="prerequisites"></a>Vereisten

U hebt het volgende nodig om aan de slag te gaan:

* Een Azure AD-abonnement Als u geen abonnement hebt, kunt u zich aanmelden voor een [gratis account](https://azure.microsoft.com/free/).
* Een abonnement op OpenAthens waarvoor eenmalige aanmelding is ingeschakeld.

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* OpenAthens ondersteunt door **IDP** geïnitieerde eenmalige aanmelding
* OpenAthens biedt ondersteuning voor het **Just-In-Time** inrichten van gebruikers

## <a name="adding-openathens-from-the-gallery"></a>OpenAthens toevoegen vanuit de galerie

Om de integratie van OpenAthens te configureren in Azure AD, moet u OpenAthens vanuit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
1. Selecteer in het linkernavigatiedeelvenster de service **Azure Active Directory**.
1. Ga naar **Bedrijfstoepassingen** en selecteer vervolgens **Alle toepassingen**.
1. Selecteer **Nieuwe toepassing** om een nieuwe toepassing toe te voegen.
1. Typ in het gedeelte **Toevoegen uit de galerie** in het zoekvak: **OpenAthens**.
1. Selecteer **OpenAthens** in het resultatenvenster en voeg de app toe. Wacht enkele seconden tot de app is toegevoegd aan de tenant.

## <a name="configure-and-test-azure-ad-single-sign-on-for-openathens"></a>Eenmalige aanmelding van Azure AD configureren en testen voor OpenAthens

Configureer en test eenmalige aanmelding van Azure AD met OpenAthens met behulp van een testgebruiker met de naam **B.Simon**. Eenmalige aanmelding werkt alleen als u een koppelingsrelatie tot stand brengt tussen een Azure AD-gebruiker en de bijbehorende gebruiker in OpenAthens.

Voltooi de volgende stappen om eenmalige aanmelding van Azure AD met OpenAthens te configureren en te testen:

1. **[Eenmalige aanmelding van Azure AD configureren](#configure-azure-ad-sso)** : zodat uw gebruikers deze functie kunnen gebruiken.
    * **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)** : om eenmalige aanmelding van Azure AD te testen met B.Simon.
    * **[De Azure AD-testgebruiker toewijzen](#assign-the-azure-ad-test-user)** zodat B.Simon eenmalige aanmelding van Azure AD kan gebruiken.
1. **[Eenmalige aanmelding bij OpenAthens configureren](#configure-openathens-sso)** : als u de instellingen voor eenmalige aanmelding aan de toepassingszijde wilt configureren.
    * **[Een testgebruiker voor OpenAthens maken](#create-openathens-test-user)** : als u een tegenhanger van B.Simon in OpenAthens wilt hebben die is gekoppeld aan de Azure AD-weergave van de gebruiker.
1. **[Eenmalige aanmelding testen](#test-sso)** : om te controleren of de configuratie werkt.

## <a name="configure-azure-ad-sso"></a>Eenmalige aanmelding van Azure AD configureren

Volg deze stappen om eenmalige aanmelding van Azure AD in te schakelen in Azure Portal.

1. Ga in de [Azure-portal](https://portal.azure.com/) op de integratiepagina van de toepassing **OpenAthens** naar het gedeelte **Beheren** en selecteer **Eenmalige aanmelding**.
1. Selecteer **SAML** op de pagina **Selecteer een methode voor eenmalige aanmelding**.
1. Op de pagina **Eenmalige aanmelding instellen met SAML** klikt u op het bewerkings-/penpictogram voor **Standaard-SAML-configuratie** om de instellingen te bewerken.

   ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

1. Upload in de sectie **Standaard SAML-configuratie** het **bestand met metagegevens van de serviceprovider**. De instructies hiervoor vindt u verderop in deze zelfstudie.

    a. Klik op **Metagegevensbestand uploaden**.

    ![Metagegevens uploaden voor OpenAthens](common/upload-metadata.png)

    b. Klik op het **mappictogram** om het metagegevensbestand te selecteren en klik op **Uploaden**.

    ![Bestand met metagegevens selecteren en uploaden voor OpenAthens](common/browse-upload-metadata.png)

    c. Zodra het bestand met metagegevens is geüpload, wordt de waarde voor **Id** automatisch ingevuld in de sectie **Standaard SAML-configuratie**:

    ![Gegevens van domein en URL's voor eenmalige aanmelding van OpenAthens](common/idp-identifier.png)

1. Ga op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** naar **XML-bestand met federatieve metagegevens** en selecteer **Downloaden** om het certificaat te downloaden. Sla dit vervolgens op de computer op.

    ![De link om het certificaat te downloaden](common/metadataxml.png)

1. In het gedeelte **OpenAthens instellen** kopieert u de juiste URL('s) op basis van uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

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

In dit gedeelte geeft u B.Simon toestemming om eenmalige aanmelding van Azure te gebruiken door toegang te verlenen tot OpenAthens.

1. Selecteer in Azure Portal de optie **Bedrijfstoepassingen** en selecteer vervolgens **Alle toepassingen**.
1. Selecteer **OpenAthens** in de lijst met toepassingen.
1. Zoek op de overzichtspagina van de app de sectie **Beheren** en selecteer **Gebruikers en groepen**.

   ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

1. Selecteer **Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![De koppeling Gebruiker toevoegen](common/add-assign-user.png)

1. Selecteer in het dialoogvenster **Gebruikers en groepen** de optie **B.Simon** in de lijst Gebruikers. Klik vervolgens op de knop **Selecteren** onderaan het scherm.
1. Als u een waarde voor een rol verwacht in de SAML-assertie, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren. Klik vervolgens op de knop **Selecteren** onderaan het scherm.
1. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

## <a name="configure-openathens-sso"></a>Eenmalige aanmelding configureren voor OpenAthens

1. Meld u in een ander browservenster als beheerder aan bij de bedrijfssite van OpenAthens.

1. Selecteer **Connections** in de lijst onder het tabblad **Management**.

    ![Schermopname van de pagina OpenAthens van de bedrijfssite met Verbindingen geselecteerd op het tabblad Beheer.](./media/openathens-tutorial/tutorial_openathens_application1.png)

1. Selecteer **SAML 1.1/2.0** en selecteer vervolgens de knop **Configure**.

    ![Schermopname met Type lokaal verificatiesysteem selecteren. dialoogvenster met S A M L 1.1/2.0 en de knop Configureren geselecteerd.](./media/openathens-tutorial/tutorial_openathens_application2.png)

1. Als u de configuratie wilt toevoegen, selecteert u de knop **Browse** om het XML-bestand met metagegevens te uploaden dat u hebt gedownload uit de Azure-portal en selecteert u vervolgens **Add**.

    ![Schermopname met S A M L-verificatiesysteem toevoegen. dialoogvenster met de actie Bladeren en de knop Toevoegen geselecteerd.](./media/openathens-tutorial/tutorial_openathens_application3.png)

1. Voer de volgende stappen uit op het tabblad **Details**.

    ![Eenmalige aanmelding configureren](./media/openathens-tutorial/tutorial_openathens_application4.png)

    a. Selecteer **Use attribute** bij **Display name mapping**.

    b. Typ de waarde `http://schema.microsoft.com/identity/claims/displayname` in het tekstvak **Display name attribute**.

    c. Selecteer **Use attribute** bij **Unique user mapping**.

    d. Typ de waarde `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name` in het tekstvak **Unique user attribute**.

    e. SchakeI bij **Status** alle drie de selectievakjes in.

    f. Selecteer **automatically** bij **Create local accounts**.

    g. Selecteer **Save changes**.

    h. Ga naar het tabblad **</> Relying Party**, kopieer de waarde van **Metadata URL** en open deze URL in de browser om het **XML-bestand met metagegevens van de SP** te downloaden. Upload dit bestand met metagegevensbestand van de serviceprovider naar de sectie **Standaard SAML-configuratie** in Azure AD.

    ![Schermopname van het tabblad Relying Party geselecteerd en de Metagegevens-U R L gemarkeerd.](./media/openathens-tutorial/tutorial_openathens_application5.png)

### <a name="create-openathens-test-user"></a>Een testgebruiker maken voor OpenAthens

In dit gedeelte wordt een gebruiker met de naam Britta Simon gemaakt in OpenAthens. OpenAthens biedt ondersteuning voor **Just-In-Time-inrichting van gebruikers**. Deze functie is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Als er nog geen gebruiker in OpenAthens bestaat, wordt er een nieuwe gemaakt nadat deze is geverifieerd.

## <a name="test-sso"></a>Eenmalige aanmelding testen

In deze sectie gaat u uw configuratie van Azure AD-eenmalige aanmelding testen via het toegangsvenster.

Wanneer u in het toegangsvenster op de tegel OpenAthens klikt, wordt u automatisch aangemeld bij de instantie van OpenAthens waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende bronnen

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Probeer OpenAthens met Azure AD](https://aad.portal.azure.com/)