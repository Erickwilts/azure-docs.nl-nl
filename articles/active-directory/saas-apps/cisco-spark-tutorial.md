---
title: 'Zelfstudie: Eenmalige aanmelding van Azure Active Directory integreren met Cisco Webex | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Cisco Webex.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/31/2020
ms.author: jeedes
ms.openlocfilehash: b6da84c1be38dde72b663de1db3756e77f4db915
ms.sourcegitcommit: 6a4687b86b7aabaeb6aacdfa6c2a1229073254de
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/06/2020
ms.locfileid: "91759773"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-cisco-webex"></a>Zelfstudie: Eenmalige aanmelding van Azure Active Directory integreren met Cisco Webex

In deze zelfstudie leert u hoe u Cisco Webex kunt integreren met Azure AD (Active Directory). Wanneer u Cisco Webex integreert met Azure AD, kunt u het volgende doen:

* In Azure AD beheren wie toegang heeft tot Cisco Webex.
* U kunt instellen dat gebruikers automatisch met hun Azure AD-account worden aangemeld bij Cisco Webex.
* Uw accounts op een centrale locatie beheren: Azure Portal.

Zie [Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) voor meer informatie over de integratie van SaaS-apps met Azure AD.

## <a name="prerequisites"></a>Vereisten

U hebt het volgende nodig om aan de slag te gaan:

* Een Azure AD-abonnement Als u geen abonnement hebt, kunt u zich aanmelden voor een [gratis account](https://azure.microsoft.com/free/).
* Een Cisco Webex-abonnement waarvoor eenmalige aanmelding is ingeschakeld.

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Cisco Webex biedt ondersteuning voor met **SP** geïnitieerde SSO.
* Cisco Webex biedt ondersteuning voor het **automatisch** inrichten van gebruikers.
* Nadat u Cisco Webex hebt geconfigureerd, kunt u sessiebeheer afdwingen, waardoor exfiltratie en infiltratie van gevoelige gegevens van uw organisatie in realtime worden beschermd. Sessiebeheer is een uitbreiding van voorwaardelijke toegang. [Meer informatie over het afdwingen van sessiebeheer met Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-deployment-aad)

## <a name="adding-cisco-webex-from-the-gallery"></a>Cisco Webex toevoegen vanuit de galerie

Voor het configureren van de integratie van Cisco Webex in Azure AD moet u Cisco Webex vanuit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
1. Selecteer in het linkernavigatiedeelvenster de service **Azure Active Directory**.
1. Ga naar **Bedrijfstoepassingen** en selecteer vervolgens **Alle toepassingen**.
1. Selecteer **Nieuwe toepassing** om een nieuwe toepassing toe te voegen.
1. Typ in de sectie **Toevoegen vanuit de galerie** in het zoekvak: **Cisco Webex**.
1. Selecteer **Cisco WebEx** in het resultatenpaneel en voeg de app vervolgens toe. Wacht enkele seconden tot de app is toegevoegd aan de tenant.

## <a name="configure-and-test-azure-ad-single-sign-on-for-cisco-webex"></a>Eenmalige aanmelding van Azure AD configureren en testen voor Cisco Webex

Configureer en test eenmalige aanmelding van Azure AD met Cisco Webex met behulp van een testgebruiker met de naam **B.Simon**. Eenmalige aanmelding werkt alleen als u een koppelingsrelatie tot stand brengt tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Cisco Webex.

Als u eenmalige aanmelding van Azure AD met Cisco Webex wilt configureren en testen, moet u de volgende bouwstenen voltooien:

1. **[Eenmalige aanmelding van Azure AD configureren](#configure-azure-ad-sso)** zodat uw gebruikers deze functie kunnen gebruiken.
    1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)** om eenmalige aanmelding van Azure AD te testen met B.Simon.
    1. **[De Azure AD-testgebruiker toewijzen](#assign-the-azure-ad-test-user)** zodat B.Simon eenmalige aanmelding van Azure AD kan gebruiken.
2. **[Cisco Webex configureren](#configure-cisco-webex)** : als u de instellingen voor eenmalige aanmelding aan de toepassingszijde wilt configureren.
    1. **[Een testgebruiker voor Cisco Webex maken](#create-cisco-webex-test-user)** : als u een tegenhanger van B.Simon in Cisco Webex wilt hebben die is gekoppeld aan de Azure AD-weergave van de gebruiker.
3. **[Eenmalige aanmelding testen](#test-sso)** om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-sso"></a>Eenmalige aanmelding van Azure AD configureren

Volg deze stappen om eenmalige aanmelding van Azure AD in te schakelen in Azure Portal.

1. Ga in de [Azure-portal](https://portal.azure.com/) op de integratiepagina van de toepassing **Cisco Webex** naar de sectie **Beheren**, en selecteer **Eenmalige aanmelding**.
1. Selecteer **SAML** op de pagina **Selecteer een methode voor eenmalige aanmelding**.
1. Op de pagina **Eenmalige aanmelding instellen met SAML** klikt u op het bewerkings-/penpictogram voor **Standaard-SAML-configuratie** om de instellingen te bewerken.

   ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** uploadt u het bestand met metagegevens dat u hebt gedownload van de **serviceprovider** en configureert u de toepassing door de volgende stappen uit te voeren:

    >[!Note]
    >U ontvangt het metagegevensbestand van de serviceprovider in de sectie **Cisco Webex configureren**, die verderop in de zelfstudie wordt beschreven. 

    a. Klik op **Metagegevensbestand uploaden**.

    b. Klik op het **mappictogram** om het metagegevensbestand te selecteren en klik op **Uploaden**.

    c. Als het uploaden van het metagegevensbestand van de serviceprovider is geslaagd, worden de waarden voor de **id** en **Antwoord-URL** automatisch ingevuld in de sectie **Standaard SAML-configuratie**:

    Plak in het tekstvak **Aanmeldings-URL** de waarde van **Antwoord-URL**, die automatisch wordt ingevuld tijdens het uploaden van het SP-metagegevensbestand.

1. In de Cisco Webex-toepassing worden de SAML-asserties in een specifieke indeling verwacht. Hiervoor moet u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van uw SAML-tokenkenmerken. In de volgende schermafbeelding wordt de lijst met standaardkenmerken weergegeven.

    ![image](common/default-attributes.png)

1. Bovendien worden in de Cisco Webex-toepassing nog enkele kenmerken verwacht die als SAML-antwoord moeten worden doorgestuurd. Deze worden hieronder weergegeven. Deze kenmerken worden ook vooraf ingevuld, maar u kunt ze herzien volgens uw vereisten.
  
    | Naam |  Bronkenmerk|
    | ---------------|--------- |
    | uid | user.userprincipalname |

1. Ga op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** naar **XML-bestand met federatieve metagegevens** en selecteer **Downloaden** om het certificaat te downloaden. Sla dit vervolgens op de computer op.

   ![De link om het certificaat te downloaden](common/metadataxml.png)

1. Kopieer in de sectie **Cisco Webex instellen** de juiste URL('s) op basis van uw behoeften.

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

In deze sectie stelt u B.Simon in staat gebruik te maken van eenmalige aanmelding van Azure door haar toegang te verlenen tot Cisco Webex.

1. Selecteer in Azure Portal de optie **Bedrijfstoepassingen** en selecteer vervolgens **Alle toepassingen**.
1. Typ en selecteer **Cisco Webex** in de lijst met toepassingen.
1. Zoek op de overzichtspagina van de app de sectie **Beheren** en selecteer **Gebruikers en groepen**.

   ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

1. Selecteer **Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![De koppeling Gebruiker toevoegen](common/add-assign-user.png)

1. Selecteer in het dialoogvenster **Gebruikers en groepen** de optie **B.Simon** in de lijst Gebruikers. Klik vervolgens op de knop **Selecteren** onderaan het scherm.
1. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

## <a name="configure-cisco-webex"></a>Cisco Webex configureren

1. Als u de configuratie in Cisco Webex wilt automatiseren, moet u **My Apps-browserextensie voor veilig aanmelden** installeren door op **De extensie installeren** te klikken.

    ![Uitbreiding van Mijn apps](common/install-myappssecure-extension.png)

2. Als u op **Cisco Webex instellen** klikt nadat u de extensie hebt toegevoegd aan de browser, wordt u doorgestuurd naar de Cisco Webex-toepassing. Geef hier de beheerdersreferenties op om u aan te melden bij Cisco Webex. In de browserextensie wordt de toepassing automatisch voor u geconfigureerd en worden stappen 3 t/m 8 geautomatiseerd.

    ![Instelling configureren](common/setup-sso.png)

3. Als u Cisco Webex handmatig wilt instellen, meldt u zich aan bij [Cisco Cloud Collaboration Management](https://admin.ciscospark.com/) met uw volledige beheerdersreferenties.

4. Selecteer **Settings** en klik in de sectie **Authentication** op **Modify**.

    ![Schermopname met Settings waar u Modify kunt selecteren.](./media/cisco-spark-tutorial/tutorial-cisco-spark-10.png)
  
5. Selecteer **Integrate a 3rd-party identity provider. (Advanced)** en ga naar het volgende scherm.

6. Sleep het bestand met de Azure AD-metagegevens naar de pagina **Import Idp Metadata** of gebruik de optie 'file browser' om het bestand te zoeken en te uploaden. Selecteer vervolgens **Require certificate signed by a certificate authority in Metadata (more secure)** en klik op **Next**.

    ![Schermopname met de pagina I d P Metadata.](./media/cisco-spark-tutorial/tutorial-cisco-spark-11.png)

7. Selecteer **Test SSO Connection**. Wanneer er een nieuw browsertabblad wordt geopend, moet u zich verifiëren bij Azure AD door u aan te melden.

8. Ga terug naar het browsertabblad **Cisco Cloud Collaboration Management**. Als de test is geslaagd, selecteert u **This test was successful.** Schakel de optie voor eenmalige aanmelding in en klik op **Next**.

### <a name="create-cisco-webex-test-user"></a>Testgebruiker voor Cisco Webex maken

In deze sectie maakt u een gebruiker met de naam B.Simon in Cisco Webex. In deze sectie maakt u een gebruiker met de naam B.Simon in Cisco Webex.

1. Meld u aan bij [Cisco Cloud Collaboration Management](https://admin.ciscospark.com/) met uw volledige beheerdersreferenties.

2. Klik op **Users** en vervolgens op **Manage Users**.
   
    ![Schermopname met de pagina Users waar u gebruikers kunt beheren.](./media/cisco-spark-tutorial/tutorial-cisco-spark-12.png) 

3. Selecteer **Manually Add or Modify Users** in het venster **Manage User** en klik vervolgens op **Next**.

4. Selecteer **Names and Email address**. Vul vervolgens de tekstvakken als volgt in:

    ![Schermopname met het dialoogvenster Manage Users waar u handmatig gebruikers kunt toevoegen of wijzigen.](./media/cisco-spark-tutorial/tutorial-cisco-spark-13.png) 

    a. Typ in het tekstvak **Voornaam** de voornaam van de gebruiker, bijvoorbeeld **B**.

    b. Typ in het tekstvak **Last Name** de achternaam van de gebruiker, bijvoorbeeld **Simon**.

    c. Typ in het tekstvak **Email address** het e-mailadres van de gebruiker, bijvoorbeeld b.simon@contoso.com.

5. Klik op het plusteken om B.Simon toe te voegen. Klik op **Volgende**.

6. Klik in het venster **Add Services for Users** op **Save** en daarna op **Finish**.

## <a name="test-sso"></a>Eenmalige aanmelding testen

Wanneer u in het toegangsvenster op de tegel Cisco Webex klikt, wordt u automatisch aangemeld bij de instantie van Cisco Webex waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Cisco Webex uitproberen met Azure AD](https://aad.portal.azure.com)

- [Wat is sessiebeheer in Microsoft Cloud App Security?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Cisco Webex beveiligen met geavanceerde zichtbaarheid en besturingselementen](https://docs.microsoft.com/cloud-app-security/protect-webex)