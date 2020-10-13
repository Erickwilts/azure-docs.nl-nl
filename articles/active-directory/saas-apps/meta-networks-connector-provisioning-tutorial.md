---
title: 'Zelf studie: een connector voor meta netwerken configureren voor het automatisch inrichten van gebruikers met Azure Active Directory | Microsoft Docs'
description: Meer informatie over het configureren van Azure Active Directory voor het automatisch inrichten en ongedaan maken van de inrichting van gebruikers accounts op de connector van het meta netwerk.
services: active-directory
author: zchia
writer: zchia
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: article
ms.date: 10/01/2019
ms.author: Zhchia
ms.openlocfilehash: c43624b57f5a46473e8640200a7284a9fa737790
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "91849228"
---
# <a name="tutorial-configure-meta-networks-connector-for-automatic-user-provisioning"></a>Zelf studie: connector voor meta netwerken configureren voor automatische gebruikers inrichting

Het doel van deze zelf studie is het demonstreren van de stappen die moeten worden uitgevoerd in de connector voor meta netwerken en Azure Active Directory (Azure AD) om Azure AD te configureren voor het automatisch inrichten en ongedaan maken van de inrichting van gebruikers en/of groepen voor de connector van het meta netwerk.

> [!NOTE]
> In deze zelfstudie wordt een connector beschreven die is gebaseerd op de Azure AD-service voor het inrichten van gebruikers. Zie voor belangrijke details over wat deze service doet, hoe het werkt en veelgestelde vragen [Inrichting en ongedaan maken van inrichting van gebruikers automatiseren naar SaaS-toepassingen met Azure Active Directory](../app-provisioning/user-provisioning.md).
>
> Deze connector bevindt zich momenteel in de open bare preview. Zie voor meer informatie over de algemene Microsoft Azure-gebruiksvoorwaarden voor preview-functies [Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Vereisten

In het scenario dat in deze zelfstudie wordt beschreven, wordt ervan uitgegaan dat u al beschikt over de volgende vereisten:

* Een Azure AD-Tenant
* [Een Tenant voor meta netwerken connector](https://www.metanetworks.com/)
* Een gebruikers account in de meta netwerken-connector met beheerders machtigingen.

## <a name="assigning-users-to-meta-networks-connector"></a>Gebruikers toewijzen aan connector voor meta netwerken

Azure Active Directory gebruikt een concept met de naam *toewijzingen* om te bepalen welke gebruikers toegang moeten krijgen tot geselecteerde apps. In de context van het automatisch inrichten van gebruikers worden alleen de gebruikers en/of groepen die zijn toegewezen aan een toepassing in azure AD gesynchroniseerd.

Voordat u automatische gebruikers inrichting configureert en inschakelt, moet u beslissen welke gebruikers en/of groepen in azure AD toegang nodig hebben tot de connector van het meta netwerk. Nadat u hebt besloten, kunt u deze gebruikers en/of groepen toewijzen aan de connector voor de meta netwerken door de volgende instructies te volgen:
* [Een gebruiker of groep toewijzen aan een bedrijfs-app](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-meta-networks-connector"></a>Belang rijke tips voor het toewijzen van gebruikers aan de connector voor de meta netwerken

* U wordt aangeraden één Azure AD-gebruiker toe te wijzen aan de connector voor meta netwerken om de configuratie van automatische gebruikers inrichting te testen. Extra gebruikers en/of groepen kunnen later worden toegewezen.

* Wanneer u een gebruiker toewijst aan de connector van de meta netwerken, moet u een geldige toepassingsspecifieke rol (indien beschikbaar) selecteren in het dialoog venster toewijzing. Gebruikers met de rol **Standaardtoegang** worden uitgesloten van het inrichten.

## <a name="setup-meta-networks-connector-for-provisioning"></a>Connector voor meta netwerken instellen voor inrichting

1. Meld u aan bij de [beheer console van uw meta netwerken connector](https://login.metanetworks.com/login/) met de naam van uw organisatie. Ga naar **beheer > API-sleutels**.

    ![Beheer console van de connector voor meta netwerken](media/meta-networks-connector-provisioning-tutorial/apikey.png)

2.  Klik op het plus teken in de rechter bovenhoek van het scherm om een nieuwe API- **sleutel**te maken.

    ![Pictogram meta netwerken-connector plus](media/meta-networks-connector-provisioning-tutorial/plusicon.png)

3.  Stel de **API-sleutel naam** en de beschrijving van de **API-sleutel**in.

    :::image type="content" source="media/meta-networks-connector-provisioning-tutorial/keyname.png" alt-text="Scherm afbeelding van de beheer console van de meta netwerken-connector met gemarkeerde P I sleutel naam en een P i Key-beschrijvings waarde van Azure A D en een P I-sleutel." border="false":::

4.  **Schrijf** bevoegdheden voor **groepen** en **gebruikers**inschakelen.

    ![Connector bevoegdheden voor meta netwerken](media/meta-networks-connector-provisioning-tutorial/privileges.png)

5.  Klik op **Toevoegen**. Kopieer het **geheim** en sla het op omdat dit de enige keer is dat u het kunt bekijken. Deze waarde wordt ingevoerd in het veld geheime token op het tabblad inrichting van uw meta netwerken connector-toepassing in de Azure Portal.

    :::image type="content" source="media/meta-networks-connector-provisioning-tutorial/token.png" alt-text="Scherm afbeelding van de beheer console van de meta netwerken-connector met gemarkeerde P I sleutel naam en een P i Key-beschrijvings waarde van Azure A D en een P I-sleutel." border="false":::

6.  Voeg een IdP toe door te navigeren naar **beheer > instellingen > IdP > nieuwe maken**.

    ![Meta netwerken connector IdP toevoegen](media/meta-networks-connector-provisioning-tutorial/newidp.png)

7.  Op de pagina **IDP-configuratie** kunt u een **naam** opgeven voor de IDP-configuratie en een **pictogram**kiezen.

    ![IdP naam van meta netwerken connector](media/meta-networks-connector-provisioning-tutorial/idpname.png)

    ![IdP pictogram van de meta netwerken connector](media/meta-networks-connector-provisioning-tutorial/icon.png)

8.  Onder **scim configureren** selecteert u de naam van de API-sleutel die u in de vorige stappen hebt gemaakt. Klik op **Opslaan**.

    ![Meta netwerken connector SCIM configureren](media/meta-networks-connector-provisioning-tutorial/configure.png)

9.  Ga naar **beheer > instellingen > tabblad IDP**. Klik op de naam van de IdP-configuratie die in de vorige stappen is gemaakt om de **IDP-id**weer te geven. Deze **id** wordt toegevoegd aan het einde van de **Tenant-URL** en voert de waarde in het veld Tenant- **URL** in op het tabblad inrichting van uw meta netwerken connector-toepassing in de Azure Portal.

    ![IdP-ID van meta netwerken connector](media/meta-networks-connector-provisioning-tutorial/idpid.png)

## <a name="add-meta-networks-connector-from-the-gallery"></a>Connector voor meta netwerken toevoegen vanuit de galerie

Voordat u de connector voor meta netwerken configureert voor het automatisch inrichten van gebruikers met Azure AD, moet u de connector voor meta netwerken vanuit de Azure AD-toepassings galerie toevoegen aan uw lijst met beheerde SaaS-toepassingen.

**Voer de volgende stappen uit om de connector voor meta netwerken toe te voegen vanuit de Azure AD-toepassings galerie:**

1. Selecteer in de **[Azure Portal](https://portal.azure.com)** in het navigatie venster links **Azure Active Directory**.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Ga naar **Bedrijfstoepassingen** en selecteer vervolgens **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u een nieuwe toepassing wilt toevoegen, selecteert u de knop **nieuwe toepassing** boven aan het deel venster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Voer in het zoekvak **META netwerken connector**in, selecteer **META netwerken connector** in het paneel resultaten en klik vervolgens op de knop **toevoegen** om de toepassing toe te voegen.

    ![Meta Networks Connector in de resultatenlijst](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-meta-networks-connector"></a>Automatische gebruikers inrichting configureren voor de connector voor meta netwerken 

In deze sectie wordt u begeleid bij de stappen voor het configureren van de Azure AD-inrichtings service om gebruikers en/of groepen te maken, bij te werken en uit te scha kelen in de connector voor meta netwerken op basis van gebruikers-en/of groeps toewijzingen in azure AD.

> [!TIP]
> U kunt er ook voor kiezen om op SAML gebaseerde eenmalige aanmelding voor de connector voor meta netwerken in te scha kelen, gevolgd door de instructies in de [zelf studie meta-netwerk connector voor eenmalige aanmelding](https://docs.microsoft.com/azure/active-directory/saas-apps/metanetworksconnector-tutorial). Eenmalige aanmelding kan onafhankelijk van automatische gebruikers inrichting worden geconfigureerd, hoewel deze twee functies elkaar in de compliment

### <a name="to-configure-automatic-user-provisioning-for-meta-networks-connector-in-azure-ad"></a>Automatische gebruikers inrichting configureren voor meta netwerken connector in azure AD:

1. Meld u aan bij de [Azure-portal](https://portal.azure.com). Selecteer **Bedrijfstoepassingen** en vervolgens **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer **Meta Networks Connector** in de lijst met toepassingen.

    ![De koppeling Meta Networks Connector in de lijst met toepassingen](common/all-applications.png)

3. Selecteer het tabblad **Inrichten**.

    ![Scherm opname van de opties voor beheer met de inrichtings optie.](common/provisioning.png)

4. Stel de **Inrichtingsmodus** in op **Automatisch**.

    ![Scherm afbeelding van de vervolg keuzelijst voor de inrichtings modus met de automatische optie aangeroepen.](common/provisioning-automatic.png)

5. Selecteer in de sectie **beheerders referenties** de invoer `https://api.metanetworks.com/v1/scim/<IdP ID>` in de Tenant- **URL**. Voer de waarde voor het **scim-verificatie token** in die eerder is opgehaald in het **geheime token**. Klik op **verbinding testen** om te controleren of Azure AD verbinding kan maken met de connector van de meta netwerken. Als de verbinding mislukt, zorg er dan voor dat uw meta netwerken Connector-account beheerders machtigingen heeft en probeer het opnieuw.

    ![Tenant-URL + token](common/provisioning-testconnection-tenanturltoken.png)

6. Voer in het veld **E-mailadres voor meldingen** het e-mailadres in van een persoon of groep die de inrichtingsfoutmeldingen zou moeten ontvangen en vink het vakje **Een e-mailmelding verzenden als een fout optreedt** aan.

    ![E-mailadres voor meldingen](common/provisioning-notification-email.png)

7. Klik op **Opslaan**.

8. Selecteer in de sectie **toewijzingen** de optie **Azure Active Directory gebruikers synchroniseren met meta netwerken connector**.

    ![Gebruikers toewijzingen van de meta-netwerk connector](media/meta-networks-connector-provisioning-tutorial/usermappings.png)

9. Controleer de gebruikers kenmerken die zijn gesynchroniseerd van Azure AD naar de meta netwerken-connector in de sectie **kenmerk toewijzing** . De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt om te voldoen aan de gebruikers accounts in de meta netwerken connector voor bijwerk bewerkingen. Selecteer de knop **Opslaan** om eventuele wijzigingen door te voeren.

    ![Gebruikers kenmerken van connector voor meta netwerken](media/meta-networks-connector-provisioning-tutorial/userattributes.png)

10. Selecteer in de sectie **toewijzingen** de optie **Azure Active Directory groepen synchroniseren met meta netwerken connector**.

    ![Groeps toewijzingen van meta netwerken connector](media/meta-networks-connector-provisioning-tutorial/groupmappings.png)

11. Controleer de groeps kenmerken die zijn gesynchroniseerd vanuit Azure AD naar de meta netwerken-connector in de sectie **kenmerk toewijzing** . De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt om de groepen in de meta netwerken connector te vergelijken voor bijwerk bewerkingen. Selecteer de knop **Opslaan** om eventuele wijzigingen door te voeren.

    ![Groeps kenmerken van meta netwerken connector](media/meta-networks-connector-provisioning-tutorial/groupattributes.png)

12. Als u bereikfilters wilt configureren, raadpleegt u de volgende instructies in de [zelfstudie Bereikfilter](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

13. Als u de Azure AD-inrichtings service voor meta netwerken **connector wilt inschakelen, wijzigt u de** **inrichtings status** in in het gedeelte **instellingen** .

    ![Inrichtingsstatus ingeschakeld](common/provisioning-toggle-on.png)

14. Definieer de gebruikers en/of groepen die u wilt inrichten voor de connector van de meta netwerken door de gewenste waarden in het **bereik** in het gedeelte **instellingen** te kiezen.

    ![Inrichtingsbereik](common/provisioning-scope.png)

15. Wanneer u klaar bent om in te richten, klikt u op **Opslaan**.

    ![Inrichtingsconfiguratie opslaan](common/provisioning-configuration-save.png)

Met deze bewerking wordt de eerste synchronisatie gestart van alle gebruikers en/of groepen die zijn gedefinieerd onder **Bereik** in de sectie **Instellingen**. De initiële synchronisatie duurt langer dan volgende synchronisaties, die ongeveer om de 40 minuten plaatsvinden zolang de Azure AD-inrichtingsservice wordt uitgevoerd. U kunt de sectie **synchronisatie Details** gebruiken om de voortgang te bewaken en koppelingen naar het rapport inrichtings activiteiten te volgen, waarin alle acties worden beschreven die worden uitgevoerd door de Azure AD Provisioning-Service op de meta netwerken connector.

Zie [Rapportage over automatische inrichting van gebruikersaccounts](../app-provisioning/check-status-user-account-provisioning.md) voor informatie over het lezen van de Azure AD-inrichtingslogboeken.

## <a name="additional-resources"></a>Aanvullende resources

* [Gebruikersaccountinrichting voor zakelijke apps beheren](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over het controleren van logboeken en het ophalen van rapporten over de inrichtingsactiviteit](../app-provisioning/check-status-user-account-provisioning.md)

