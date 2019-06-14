---
title: 'Zelfstudie: Azure Active Directory-integratie met RunMyProcess | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en RunMyProcess.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: d31f7395-048b-4a61-9505-5acf9fc68d9b
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: dfef1371b7ac61712c0f70efd48c0e791c4c729d
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60517879"
---
# <a name="tutorial-azure-active-directory-integration-with-runmyprocess"></a>Zelfstudie: Azure Active Directory-integratie met RunMyProcess

In deze zelfstudie leert u hoe u RunMyProcess integreren met Azure Active Directory (Azure AD).

RunMyProcess integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot RunMyProcess heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij RunMyProcess (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met RunMyProcess, moet u de volgende items:

- Een Azure AD-abonnement
- Een RunMyProcess eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u een proefversie van één maand hier downloaden:[proefversie aanbieding](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. RunMyProcess uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-runmyprocess-from-the-gallery"></a>RunMyProcess uit de galerie toe te voegen
Voor het configureren van de integratie van RunMyProcess in Azure AD, moet u RunMyProcess uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen RunMyProcess uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)** , klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Toepassingen][3]

1. Typ in het zoekvak **RunMyProcess**.

    ![Het maken van een Azure AD-testgebruiker](./media/runmyprocess-tutorial/tutorial_runmyprocess_search.png)

1. Selecteer in het deelvenster resultaten **RunMyProcess**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/runmyprocess-tutorial/tutorial_runmyprocess_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met RunMyProcess op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in RunMyProcess is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in RunMyProcess tot stand worden gebracht.

In RunMyProcess, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met RunMyProcess, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker RunMyProcess](#creating-a-runmyprocess-test-user)**  : als u wilt een equivalent van Britta Simon in RunMyProcess die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing RunMyProcess.

**Voor het configureren van Azure AD eenmalige aanmelding met RunMyProcess, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **RunMyProcess** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/runmyprocess-tutorial/tutorial_runmyprocess_samlbase.png)

1. Op de **RunMyProcess domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/runmyprocess-tutorial/tutorial_runmyprocess_url.png)

    In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://live.runmyprocess.com/live/<tenant id>`

    > [!NOTE] 
    > De waarde is niet echt. Werk de waarde bij met de werkelijke aanmeldings-URL. Neem contact op met [RunMyProcess Client ondersteuningsteam](mailto:support@runmyprocess.com) om de waarde. 

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/runmyprocess-tutorial/tutorial_runmyprocess_certificate.png) 

1. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/runmyprocess-tutorial/tutorial_general_400.png)

1. Op de **RunMyProcess configuratie** sectie, klikt u op **configureren RunMyProcess** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/runmyprocess-tutorial/tutorial_runmyprocess_configure.png) 

1. In een ander browservenster aanmelden voor uw tenant RunMyProcess als beheerder.

1. Klik in het linkernavigatievenster, **Account** en selecteer **configuratie**.
   
    ![Eenmalige aanmelding aan app-zijde configureren](./media/runmyprocess-tutorial/tutorial_runmyprocess_001.png)

1. Ga naar **verificatiemethode** sectie en de onderstaande stappen uitvoeren:
   
    ![Eenmalige aanmelding aan app-zijde configureren](./media/runmyprocess-tutorial/tutorial_runmyprocess_002.png)

    a. Als **methode**, selecteer **eenmalige aanmelding met Samlv2**. 

    b. In de **SSO omleiden** tekstvak, plak de waarde van **Single Sign-On Service URL voor SAML**, die u hebt gekopieerd vanuit Azure portal.

    c. In de **afmelden omleiden** tekstvak, plak de waarde van **afmelding URL**, die u hebt gekopieerd vanuit Azure portal.

    d. In de **indeling van de Id** tekstvak typt u de waarde van **indeling van de id** als **urn: oasis: namen: tc: SAML:1.1:nameid-indeling: emailAddress**.

    e. Kopieer de inhoud van het gedownloade certificaat-bestand en plak deze in de **certificaat** tekstvak. 
 
    f. Klik op **opslaan** pictogram.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/runmyprocess-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/runmyprocess-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/runmyprocess-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/runmyprocess-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-runmyprocess-test-user"></a>Het maken van een testgebruiker RunMyProcess

Als u wilt dat Azure AD-gebruikers zich aanmelden bij RunMyProcess, moeten ze worden ingericht voor RunMyProcess. In het geval van RunMyProcess is inrichten een handmatige taak.

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**

1. Meld u aan bij uw bedrijf RunMyProcess site aan als beheerder.

1. Klik op **Account** en selecteer **gebruikers** in het linkernavigatievenster, en klik op **nieuwe gebruiker**.
   
    ![New User](./media/runmyprocess-tutorial/tutorial_runmyprocess_003.png "New User")

1. In de **gebruikersinstellingen** sectie, voert u de volgende stappen uit:
   
    ![Profile](./media/runmyprocess-tutorial/tutorial_runmyprocess_004.png "Profile") 
  
    a. Type de **naam** en **e** van een geldige Azure AD-account die u inrichten in de bijbehorende tekstvakken wilt. 

    b. Selecteer een **IDE taal**, **taal**, en **profiel**. 

    c. Selecteer **account maken e-mailbericht verzenden naar mij**. 

    d. Klik op **Opslaan**.
   
    >[!NOTE]
    >U kunt elke andere RunMyProcess gebruiker account hulpmiddelen voor het maken of API's geleverd door RunMyProcess voor het inrichten van Azure Active Directory-gebruikersaccounts. 
    > 

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan RunMyProcess.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan RunMyProcess toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **RunMyProcess**.

    ![Eenmalige aanmelding configureren](./media/runmyprocess-tutorial/tutorial_runmyprocess_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

Het doel van deze sectie is het testen van de configuratie van uw Azure AD-eenmalige aanmelding via het toegangsvenster.

Wanneer u op de tegel RunMyProcess in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing RunMyProcess.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/runmyprocess-tutorial/tutorial_general_01.png
[2]: ./media/runmyprocess-tutorial/tutorial_general_02.png
[3]: ./media/runmyprocess-tutorial/tutorial_general_03.png
[4]: ./media/runmyprocess-tutorial/tutorial_general_04.png

[100]: ./media/runmyprocess-tutorial/tutorial_general_100.png

[200]: ./media/runmyprocess-tutorial/tutorial_general_200.png
[201]: ./media/runmyprocess-tutorial/tutorial_general_201.png
[202]: ./media/runmyprocess-tutorial/tutorial_general_202.png
[203]: ./media/runmyprocess-tutorial/tutorial_general_203.png

