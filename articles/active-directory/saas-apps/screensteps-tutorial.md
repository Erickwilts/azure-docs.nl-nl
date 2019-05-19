---
title: 'Zelfstudie: Azure Active Directory-integratie met ScreenSteps | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en ScreenSteps.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 4563fe94-a88f-4895-a07f-79df44889cf9
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/07/2019
ms.author: jeedes
ms.openlocfilehash: 570f789d0f399c5ffa7535101136ab65ba58ccd5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "65867651"
---
# <a name="tutorial-azure-active-directory-integration-with-screensteps"></a>Zelfstudie: Azure Active Directory-integratie met ScreenSteps

In deze zelfstudie leert u hoe u ScreenSteps integreren met Azure Active Directory (Azure AD).
ScreenSteps integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot ScreenSteps heeft.
* U kunt uw gebruikers worden automatisch aangemeld ScreenSteps (Single Sign-On) met hun Azure AD-accounts inschakelen.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met ScreenSteps, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* ScreenSteps eenmalige aanmelding ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Biedt ondersteuning voor ScreenSteps **SP** gestart door SSO

## <a name="adding-screensteps-from-the-gallery"></a>ScreenSteps uit de galerie toe te voegen

Voor het configureren van de integratie van ScreenSteps in Azure AD, moet u ScreenSteps uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen ScreenSteps uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **ScreenSteps**, selecteer **ScreenSteps** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![ScreenSteps in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met ScreenSteps op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in ScreenSteps tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met ScreenSteps, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureren van eenmalige aanmelding ScreenSteps](#configure-screensteps-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Maken van de testgebruiker ScreenSteps](#create-screensteps-test-user)**  : als u wilt een equivalent van Britta Simon in ScreenSteps die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met ScreenSteps, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **ScreenSteps** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![ScreenSteps domein en URL's, eenmalige aanmelding informatie](common/sp-signonurl.png)

    In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<tenantname>.ScreenSteps.com`

    > [!NOTE]
    > Deze waarde is niet echt. Deze waarde bijwerken met de werkelijke aanmeldings-URL, die verderop in deze zelfstudie wordt uitgelegd.

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

6. Op de **ScreenSteps instellen** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-screensteps-single-sign-on"></a>ScreenSteps voor eenmalige aanmelding configureren

1. Meld u in een ander browservenster in uw bedrijf ScreenSteps site als beheerder.

1. Klik op **Accountinstellingen**.

    ![Accountbeheer](./media/screensteps-tutorial/ic778523.png "accountbeheer")

1. Klik op **Eenmalige aanmelding**.

    ![Externe verificatie](./media/screensteps-tutorial/ic778524.png "externe verificatie")

1. Klik op **Single Sign-on-eindpunt maken**.

    ![Externe verificatie](./media/screensteps-tutorial/ic778525.png "externe verificatie")

1. In de **één maken Aanmeldingseindpunt** sectie, voert u de volgende stappen uit:

    ![Maken van een verificatie-eindpunt](./media/screensteps-tutorial/ic778526.png "een verificatie-eindpunt maken")

    a. In de **titel** tekstvak, typ een titel.

    b. Uit de **modus** in de lijst met **SAML**.

    c. Klik op **Create**.

1. **Bewerken** het nieuwe eindpunt.

    ![Bewerk eindpunt](./media/screensteps-tutorial/ic778528.png "eindpunt bewerken")

1. In de **bewerken één aanmelding eindpunt** sectie, voert u de volgende stappen uit:

    ![Externe verificatie-eindpunt](./media/screensteps-tutorial/ic778527.png "externe verificatie-eindpunt")

    a. Klik op **nieuwe-SAML certificaatbestand uploaden**, en upload het certificaat dat u hebt gedownload vanuit Azure portal.

    b. Plakken **aanmeldings-URL** waarde die u hebt gekopieerd vanuit de Azure portal in de **externe aanmeldings-URL** tekstvak.

    c. Plakken **afmeldings-URL van** waarde die u hebt gekopieerd vanuit de Azure portal in de **URL voor afmelden** tekstvak.

    d. Selecteer een **groep** gebruikers toewijzen aan wanneer deze zijn ingericht.

    e. Klik op **Bijwerken**.

    f. Kopiëren de **URL voor SAML-consument** bij het Klembord en plak in op de **aanmeldings-URL** -tekstvak in **SAML-basisconfiguratie** sectie in Azure portal.

    g. Ga terug naar de **Single Sign-on Endpoint bewerken**.

    h. Klik op de **standaard voor account** knop met dit eindpunt voor alle gebruikers die zich bij ScreenSteps aanmelden. U kunt ook klikken op de **toevoegen aan de Site** knop met dit eindpunt voor specifieke sites in **ScreenSteps**.

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

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan ScreenSteps.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **ScreenSteps**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **ScreenSteps**.

    ![De koppeling ScreenSteps in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-screensteps-test-user"></a>ScreenSteps testgebruiker maken

In deze sectie maakt u een gebruiker met de naam van Britta Simon in ScreenSteps. Werken met [ScreenSteps Client ondersteuningsteam](https://www.screensteps.com/contact) om toe te voegen de gebruikers in het ScreenSteps-platform. Er moeten gebruikers worden gemaakt en geactiveerd voordat u eenmalige aanmelding kunt gebruiken.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel ScreenSteps in het toegangsvenster, moet u worden automatisch aangemeld bij de ScreenSteps waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)