---
title: 'Zelf studie: Symantec Web Security service (WSS) configureren voor het automatisch inrichten van gebruikers met Azure Active Directory | Microsoft Docs'
description: Meer informatie over het configureren van Azure Active Directory voor het automatisch inrichten en ongedaan maken van de inrichting van gebruikers accounts in Symantec Web Security service (WSS).
services: active-directory
author: zchia
writer: zchia
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: article
ms.date: 07/23/2019
ms.author: Zhchia
ms.openlocfilehash: 7b4a5e76ec8dad66bf300cc3e1d55471383f83ff
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "91285793"
---
# <a name="tutorial-configure-symantec-web-security-service-wss-for-automatic-user-provisioning"></a>Zelf studie: Symantec Web Security service (WSS) configureren voor automatische gebruikers inrichting

Het doel van deze zelf studie is het demonstreren van de stappen die moeten worden uitgevoerd in Symantec Web Security service (WSS) en Azure Active Directory (Azure AD) om Azure AD te configureren voor het automatisch inrichten en ongedaan maken van de inrichting van gebruikers en/of groepen in Symantec Web Security service (WSS).

> [!NOTE]
> In deze zelfstudie wordt een connector beschreven die is gebaseerd op de Azure AD-service voor het inrichten van gebruikers. Zie voor belangrijke details over wat deze service doet, hoe het werkt en veelgestelde vragen [Inrichting en ongedaan maken van inrichting van gebruikers automatiseren naar SaaS-toepassingen met Azure Active Directory](../app-provisioning/user-provisioning.md).
>
> Deze connector bevindt zich momenteel in de open bare preview. Zie voor meer informatie over de algemene Microsoft Azure-gebruiksvoorwaarden voor preview-functies [Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Vereisten

In het scenario dat in deze zelfstudie wordt beschreven, wordt ervan uitgegaan dat u al beschikt over de volgende vereisten:

* Een Azure AD-Tenant
* [Een Symantec-Tenant (Web Security Service)](https://www.websecurity.symantec.com/buy-renew?inid=brmenu_nav_brhome)
* Een gebruikers account in Symantec Web Security service (WSS) met beheerders machtigingen.

## <a name="assigning-users-to-symantec-web-security-service-wss"></a>Gebruikers toewijzen aan Symantec Web Security service (WSS)

Azure Active Directory gebruikt een concept met de naam *toewijzingen* om te bepalen welke gebruikers toegang moeten krijgen tot geselecteerde apps. In de context van het automatisch inrichten van gebruikers worden alleen de gebruikers en/of groepen die zijn toegewezen aan een toepassing in azure AD gesynchroniseerd.

Voordat u het automatisch inrichten van gebruikers configureert en inschakelt, moet u beslissen welke gebruikers en/of groepen in azure AD toegang nodig hebben tot Symantec Web Security service (WSS). Nadat u hebt besloten, kunt u deze gebruikers en/of groepen toewijzen aan Symantec Web Security service (WSS) door de volgende instructies te volgen:
* [Een gebruiker of groep toewijzen aan een bedrijfs-app](../manage-apps/assign-user-or-group-access-portal.md)

##  <a name="important-tips-for-assigning-users-to-symantec-web-security-service-wss"></a>Belang rijke tips voor het toewijzen van gebruikers aan Symantec Web Security service (WSS)

* Het is raadzaam dat één Azure AD-gebruiker wordt toegewezen aan Symantec Web Security service (WSS) om de configuratie van automatische gebruikers inrichting te testen. Extra gebruikers en/of groepen kunnen later worden toegewezen.

* Wanneer u een gebruiker toewijst aan Symantec Web Security service (WSS), moet u een geldige toepassingsspecifieke rol (indien beschikbaar) selecteren in het dialoog venster toewijzing. Gebruikers met de rol **Standaardtoegang** worden uitgesloten van het inrichten.

## <a name="setup-symantec-web-security-service-wss-for-provisioning"></a>Symantec Web Security service (WSS) instellen voor het inrichten

Voordat u Symantec Web Security service (WSS) configureert voor het automatisch inrichten van gebruikers met Azure AD, moet u SCIM inrichten inschakelen op Symantec Web Security service (WSS).

1. Meld u aan bij de [beheer console van de Symantec Web Security-service](https://portal.threatpulse.com/login.jsp). Navigeer naar **Solutions**  >  **service**.

    ![Symantec Web Security-service (WSS)](media/symantec-web-security-service/service.png)

2. Navigeer naar **account onderhoud**  >  **integratie**  >  **nieuwe integratie**.

    ![Symantec Web Security Service (WSS)](media/symantec-web-security-service/acount.png)

3.  Selecteer **gebruikers van derden & groepen synchroniseren**. 

    ![Scherm afbeelding van de synchronisatie optie & groepen van derden.](media/symantec-web-security-service/third-party-users.png)

4.  Kopieer de **scim-URL** en het **token**. Deze waarden worden ingevoerd in het veld **Tenant-URL** en **geheim token** op het tabblad inrichten van uw Symantec Web Security service (WSS)-toepassing in de Azure Portal.

    ![Scherm afbeelding van het dialoog venster nieuwe integratie met de tekst vakken S C I M U R L en token.](media/symantec-web-security-service/scim.png)

## <a name="add-symantec-web-security-service-wss-from-the-gallery"></a>Symantec Web Security-service (WSS) toevoegen vanuit de galerie

Als u Symantec Web Security service (WSS) wilt configureren voor het automatisch inrichten van gebruikers met Azure AD, moet u Symantec Web Security service (WSS) toevoegen vanuit de Azure AD-toepassings galerie aan uw lijst met beheerde SaaS-toepassingen.

**Als u Symantec Web Security service (WSS) wilt toevoegen vanuit de Azure AD-toepassings galerie, voert u de volgende stappen uit:**

1. Selecteer in de **[Azure Portal](https://portal.azure.com)** in het navigatie venster links **Azure Active Directory**.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Ga naar **Bedrijfstoepassingen** en selecteer vervolgens **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u een nieuwe toepassing wilt toevoegen, selecteert u de knop **nieuwe toepassing** boven aan het deel venster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Voer in het zoekvak **Symantec Web Security service**in, selecteer **Symantec Web Security service** in het deel venster resultaten en klik vervolgens op de knop **toevoegen** om de toepassing toe te voegen.

    ![Symantec Web Security Service (WSS) toevoegen vanuit de galerie](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-symantec-web-security-service-wss"></a>Automatische gebruikers inrichting configureren voor Symantec Web Security service (WSS)

In deze sectie wordt u begeleid bij de stappen voor het configureren van de Azure AD-inrichtings service om gebruikers en/of groepen in Symantec Web Security service (WSS) te maken, bij te werken en uit te scha kelen op basis van gebruikers-en/of groeps toewijzingen in azure AD.

> [!TIP]
> U kunt er ook voor kiezen om op SAML gebaseerde eenmalige aanmelding in te scha kelen voor Symantec Web Security service (WSS), gevolgd door de instructies in de [zelf studie voor de eenmalige aanmelding van de Symantec Web Security service (WSS)](symantec-tutorial.md). Eenmalige aanmelding kan onafhankelijk van automatische gebruikers inrichting worden geconfigureerd, hoewel deze twee functies elkaar aanvullen.

### <a name="to-configure-automatic-user-provisioning-for-symantec-web-security-service-wss-in-azure-ad"></a>Automatische gebruikers inrichting configureren voor Symantec Web Security service (WSS) in azure AD:

1. Meld u aan bij de [Azure-portal](https://portal.azure.com). Selecteer **Bedrijfstoepassingen** en vervolgens **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer **Symantec Web Security-service**in de lijst toepassingen.

    ![De koppeling Symantec Web Security Service (WSS) in de lijst met toepassingen](common/all-applications.png)

3. Selecteer het tabblad **Inrichten**.

    ![Scherm opname van de opties voor beheer met de inrichtings optie.](common/provisioning.png)

4. Stel de **Inrichtingsmodus** in op **Automatisch**.

    ![Scherm afbeelding van de vervolg keuzelijst voor de inrichtings modus met de automatische optie aangeroepen.](common/provisioning-automatic.png)

5. In het gedeelte beheerders referenties voert u de **scim-URL** en **token** waarden in die respectievelijk eerder zijn opgehaald in de **Tenant-URL** en het **geheime token** . Klik op **verbinding testen** om te controleren of Azure AD verbinding kan maken met de Symantec Web Security-service. Als de verbinding mislukt, zorg er dan voor dat uw Symantec Web Security-Service account (WSS) beheerders machtigingen heeft en probeer het opnieuw.

    ![Tenant-URL + token](common/provisioning-testconnection-tenanturltoken.png)

6. Voer in het veld **E-mailadres voor meldingen** het e-mailadres in van een persoon of groep die de inrichtingsfoutmeldingen zou moeten ontvangen en vink het vakje **Een e-mailmelding verzenden als een fout optreedt** aan.

    ![E-mailadres voor meldingen](common/provisioning-notification-email.png)

7. Klik op **Opslaan**.

8. Selecteer in de sectie **toewijzingen** de optie **Azure Active Directory gebruikers synchroniseren met Symantec Web Security service (WSS)**.

    ![Scherm afbeelding van de sectie toewijzingen met de optie Azure Active Directory gebruikers synchroniseren met Symantec Web Security Service W S S aangeroepen.](media/symantec-web-security-service/usermapping.png)

9. Controleer de gebruikers kenmerken die zijn gesynchroniseerd vanuit Azure AD naar Symantec Web Security service (WSS) in de sectie **kenmerk toewijzing** . De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt om te voldoen aan de gebruikers accounts in Symantec Web Security service (WSS) voor bijwerk bewerkingen. Selecteer de knop **Opslaan** om eventuele wijzigingen door te voeren.

    ![Scherm afbeelding van het gedeelte toewijzing toewijzen waarin 16 overeenkomende eigenschappen worden weer gegeven.](media/symantec-web-security-service/userattribute.png)

10. Selecteer in de sectie **toewijzingen** de optie **Azure Active Directory groepen synchroniseren met de Symantec Web Security-service**.

    ![Scherm afbeelding van de sectie toewijzingen met de optie Azure Active Directory groepen synchroniseren met de Symantec Web Security-Service W S S.](media/symantec-web-security-service/groupmapping.png)

11. Controleer de groeps kenmerken die zijn gesynchroniseerd vanuit Azure AD naar Symantec Web Security service (WSS) in de sectie **kenmerk toewijzing** . De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt om de groepen in Symantec Web Security service (WSS) te vergelijken voor bijwerk bewerkingen. Selecteer de knop **Opslaan** om eventuele wijzigingen door te voeren.

    ![Scherm afbeelding van het gedeelte toewijzing toewijzen dat drie overeenkomende eigenschappen weergeeft.](media/symantec-web-security-service/groupattribute.png)

12. Als u bereikfilters wilt configureren, raadpleegt u de volgende instructies in de [zelfstudie Bereikfilter](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

13. Als u de Azure AD-inrichtings service voor Symantec Web Security service wilt inschakelen, **wijzigt u de** **inrichtings status** in in het gedeelte **instellingen** .

    ![Inrichtingsstatus ingeschakeld](common/provisioning-toggle-on.png)

14. Definieer de gebruikers en/of groepen die u wilt inrichten voor Symantec Web Security service (WSS) door de gewenste waarden in het **bereik** te kiezen in de sectie **instellingen** .

    ![Inrichtingsbereik](common/provisioning-scope.png)

15. Wanneer u klaar bent om in te richten, klikt u op **Opslaan**.

    ![Inrichtingsconfiguratie opslaan](common/provisioning-configuration-save.png)

Met deze bewerking wordt de eerste synchronisatie gestart van alle gebruikers en/of groepen die zijn gedefinieerd onder **Bereik** in de sectie **Instellingen**. Het duurt langer voordat de initiële synchronisatie is uitgevoerd dan bij de volgende synchronisaties. Voor meer informatie over hoe lang het duurt voor het inrichten van gebruikers en/of groepen, raadpleegt u [hoe lang het duurt om gebruikers](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md#how-long-will-it-take-to-provision-users)in te richten.

U kunt de **huidige status** sectie gebruiken om de voortgang te controleren en koppelingen naar uw inrichtings activiteiten rapport te volgen, waarin alle acties worden beschreven die worden uitgevoerd door de Azure AD Provisioning-Service in Symantec Web Security service (WSS). Zie [de status van gebruikers inrichten controleren](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md)voor meer informatie. Zie [rapportage over het automatisch inrichten van gebruikers accounts](../app-provisioning/check-status-user-account-provisioning.md)voor informatie over het vastleggen van Azure AD-inrichtings Logboeken.

## <a name="additional-resources"></a>Aanvullende resources

* [Gebruikersaccountinrichting voor zakelijke apps beheren](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over het controleren van logboeken en het ophalen van rapporten over de inrichtingsactiviteit](../app-provisioning/check-status-user-account-provisioning.md)
