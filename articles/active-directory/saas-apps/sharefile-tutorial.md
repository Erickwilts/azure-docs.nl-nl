---
title: 'Zelfstudie: Azure Active Directory-integratie met Citrix ShareFile | Microsoft Docs'
description: Leer hoe u eenmalige aanmelding tussen Azure Active Directory en Citrix ShareFile configureert.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 08/18/2020
ms.author: jeedes
ms.openlocfilehash: 59f8148cf6e1d705094e8a6c4ee267b6c77ab7da
ms.sourcegitcommit: 59f506857abb1ed3328fda34d37800b55159c91d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2020
ms.locfileid: "92518412"
---
# <a name="tutorial-azure-active-directory-integration-with-citrix-sharefile"></a>Zelfstudie: Azure Active Directory-integratie met Citrix ShareFile

In deze zelfstudie leert u hoe u Citrix ShareFile integreert met Azure Active Directory (Azure AD).
De integratie van Citrix ShareFile met Azure AD heeft de volgende voordelen:

* U kunt in Azure AD bepalen wie er toegang heeft tot Citrix ShareFile.
* U kunt instellen dat gebruikers automatisch met hun Azure AD-account worden aangemeld bij Citrix ShareFile (eenmalige aanmelding).
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

U hebt het volgende nodig om Azure AD-integratie te configureren met Citrix ShareFile:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* Abonnement op Citrix ShareFile met eenmalige aanmelding (SSO) ingeschakeld

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Citrix ShareFile ondersteunt door **SP** geïnitieerde SSO
* Zodra u Citrix ShareFile hebt geconfigureerd, kunt u sessiebeheer afdwingen, waardoor exfiltratie en infiltratie van gevoelige gegevens van uw organisatie in realtime worden beschermd. Sessiebeheer is een uitbreiding van voorwaardelijke toegang. [Meer informatie over het afdwingen van sessiebeheer met Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-any-app).

## <a name="adding-citrix-sharefile-from-the-gallery"></a>Citrix ShareFile toevoegen vanuit de galerie

Om de integratie van Citrix ShareFile in Azure AD te configureren, moet u Citrix ShareFile vanuit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
1. Selecteer in het linkernavigatiedeelvenster de service **Azure Active Directory** .
1. Ga naar **Bedrijfstoepassingen** en selecteer vervolgens **Alle toepassingen** .
1. Selecteer **Nieuwe toepassing** om een nieuwe toepassing toe te voegen.
1. Typ in de sectie **Toevoegen uit de galerie** de naam **Citrix ShareFile** in het zoekvak.
1. Selecteer **Citrix ShareFile** in het resultatenvenster en voeg vervolgens de app toe. Wacht enkele seconden tot de app is toegevoegd aan de tenant.

## <a name="configure-and-test-azure-ad-sso"></a>Eenmalige aanmelding van Azure AD configureren en testen

In dit gedeelte gaat u eenmalige aanmelding van Azure AD met Citrix ShareFile configureren en testen op basis van een testgebruiker met de naam **Britta Simon** .
Eenmalige aanmelding werkt alleen als er een koppelingsrelatie tussen een Azure AD-gebruiker en de daaraan gerelateerde gebruiker in Citrix ShareFile tot stand is gebracht.

Als u eenmalige aanmelding van Azure AD wilt configureren en testen met Citrix ShareFile, moet u de volgende procedures uitvoeren:

1. **[Eenmalige aanmelding van Azure AD configureren](#configure-azure-ad-sso)** : zodat uw gebruikers deze functie kunnen gebruiken.
    
    * **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)** : als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
    * **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)** : als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
2. **[Eenmalige aanmelding voor Citrix ShareFile configureren](#configure-citrix-sharefile-sso)** : de instellingen voor eenmalige aanmelding aan de toepassingszijde configureren.
    * **[Testgebruiker voor Citrix ShareFile maken](#create-citrix-sharefile-test-user)** : een tegenhanger voor Britta Simon maken in Citrix ShareFile die wordt gekoppeld aan de Azure AD-voorstelling van de gebruiker.
3. **[Eenmalige aanmelding testen](#test-sso)** : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-sso"></a>Eenmalige aanmelding van Azure AD configureren

Volg deze stappen om eenmalige aanmelding van Azure AD in te schakelen in Azure Portal.

1. Zoek in [Azure Portal](https://portal.azure.com/), op de integratiepagina van de toepassing **Citrix ShareFile** , de sectie **Beheren** en selecteer **Eenmalige aanmelding** .
1. Selecteer **SAML** op de pagina **Selecteer een methode voor eenmalige aanmelding** .
1. Op de pagina **Eenmalige aanmelding instellen met SAML** klikt u op het bewerkings-/penpictogram voor **Standaard-SAML-configuratie** om de instellingen te bewerken.

   ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

1. In de sectie **Standaard-SAML-configuratie** voert u de waarden in voor de volgende velden:

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met het volgende patroon: `https://<tenant-name>.sharefile.com/saml/login`

    b. In het tekstvak **Id (Entiteits-id)** typt u een URL met het volgende patroon:

    - `https://<tenant-name>.sharefile.com`
    - `https://<tenant-name>.sharefile.com/saml/info`
    - `https://<tenant-name>.sharefile1.com/saml/info`
    - `https://<tenant-name>.sharefile1.eu/saml/info`
    - `https://<tenant-name>.sharefile.eu/saml/info`

    c. In het tekstvak **Antwoord-URL** typt u een URL met de volgende notatie:
    
    - `https://<tenant-name>.sharefile.com/saml/acs`
    - `https://<tenant-name>.sharefile.eu/saml/<URL path>`
    - `https://<tenant-name>.sharefile.com/saml/<URL path>`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke id, de antwoord-URL en de aanmeldings-URL. Neem contact op met het [ondersteuningsteam van Citrix ShareFile](https://www.citrix.co.in/products/citrix-content-collaboration/support.html) om deze waarden op te vragen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

4. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

6. In het gedeelte **Citrix ShareFile instellen** kopieert u de juiste URL('s) op basis van uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken 

In deze sectie gaat u een testgebruiker met de naam B.Simon maken in Azure Portal.

1. Selecteer in het linkerdeelvenster van Azure Portal de optie **Azure Active Directory** , selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers** .
1. Selecteer **Nieuwe gebruiker** boven aan het scherm.
1. Volg de volgende stappen bij de eigenschappen voor **Gebruiker** :
   1. Voer in het veld **Naam**`B.Simon` in.  
   1. Voer username@companydomain.extension in het veld **Gebruikersnaam** in. Bijvoorbeeld `B.Simon@contoso.com`.
   1. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak **Wachtwoord** .
   1. Klik op **Create** .

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In dit gedeelte gaat u B. Simon toestemming geven voor gebruik van eenmalige aanmelding met Azure door haar toegang te geven tot Citrix ShareFile.

1. Selecteer in Azure Portal de optie **Bedrijfstoepassingen** en selecteer vervolgens **Alle toepassingen** .
1. Selecteer **Citrix ShareFile** in de lijst met toepassingen.
1. Zoek op de overzichtspagina van de app de sectie **Beheren** en selecteer **Gebruikers en groepen** .

   ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

1. Selecteer **Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen** .

    ![De koppeling Gebruiker toevoegen](common/add-assign-user.png)

1. Selecteer in het dialoogvenster **Gebruikers en groepen** de optie **B.Simon** in de lijst Gebruikers. Klik vervolgens op de knop **Selecteren** onderaan het scherm.
1. Als u een waarde voor een rol verwacht in de SAML-assertie, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren. Klik vervolgens op de knop **Selecteren** onderaan het scherm.
1. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen** .

## <a name="configure-citrix-sharefile-sso"></a>Eenmalige aanmelding voor Citrix ShareFile configureren

1. Meld u in een ander browservenster als beheerder aan bij de bedrijfssite van **Citrix ShareFile** .

1. Klik in het menu aan de linkerkant op het **instellingenpictogram** -> **Security** -> **Login & Security Policy** .
   
    ![Accountbeheer](./media/sharefile-tutorial/settings-security.png "Accountbeheer")

1. Voer in het dialoogvenster **Single sign-on / SAML 2.0 Configuration** de volgende stappen uit onder **Basic Settings** :
   
    ![Eenmalige aanmelding](./media/sharefile-tutorial/saml-configuration.png "Eenmalige aanmelding")
   
    a. Selecteer **JA** in **Enable SAML** .

    b. Kopieer de waarde voor **ShareFile Issuer/ Entity ID** en plak deze in het dialoogvenster **Standaard SAML-configuratie** in Azure Portal in het vak **Id-URL** .
    
    c. Plak in het tekstvak **Your IDP Issuer/ Entity ID** de waarde van **Azure AD-id** die u hebt gekopieerd uit de Azure-portal.

    d. Klik op **Change** naast het veld **X.509 Certificate** en upload het certificaat dat u hebt gedownload uit de Azure-portal.
    
    e. Plak in het tekstvak **Login URL** de waarde van **Aanmeldings-URL** die u hebt gekopieerd uit de Azure-portal.
    
    f. Plak in het tekstvak **Logout URL** de waarde van **Afmeldings-URL** die u hebt gekopieerd uit de Azure-portal.

5. Klik op **Save** in de beheerportal van Citrix ShareFile.

## <a name="create-citrix-sharefile-test-user"></a>Testgebruiker voor Citrix ShareFile maken

1. Meld u aan bij uw **Citrix ShareFile** -tenant.

2. Klik op **People** -> **Manage Users Home** -> **Create New Users** -> **Create Employee** .
   
    ![Werknemer maken](./media/sharefile-tutorial/create-user.png "Werknemer maken")

3. Voer in het gedeelte **Basic Information** onderstaande stappen uit:
   
    ![Algemene informatie](./media/sharefile-tutorial/user-form.png "Algemene informatie")
   
    a. Typ in het tekstvak **First Name** de **voornaam** van de gebruiker, in dit geval **Britta** .
   
    b.  Typ in het tekstvak **Last Name** de **achternaam** van de gebruiker, in dit geval **Simon** .
   
    c. Typ in het tekstvak **E-mailadres** het e-mailadres van Britta Simon als **brittasimon\@contoso.com** .

4. Klik op **Add User** .
  
    >[!NOTE]
    >De houder van het Azure AD-account krijgt een e-mailbericht met een koppeling om het account te bevestigen voordat dit actief wordt. U kunt ieder hulpprogramma voor het maken van Citrix ShareFile-gebruikersaccounts of API's van Citrix ShareFile gebruiken voor het inrichten van Azure AD-gebruikersaccounts.

## <a name="test-sso"></a>Eenmalige aanmelding testen 

In deze sectie gaat u uw configuratie van Azure AD-eenmalige aanmelding testen via het toegangsvenster.

Wanneer u in het toegangsvenster op de tegel Citrix ShareFile klikt, zou u automatisch moeten worden aangemeld bij het exemplaar van in het toegangsvenster waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](../user-help/my-apps-portal-end-user-access.md) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](./tutorial-list.md)

- [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](../conditional-access/overview.md)