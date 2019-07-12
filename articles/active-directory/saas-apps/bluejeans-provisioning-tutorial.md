---
title: 'Zelfstudie: BlueJeans configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en inrichting ongedaan maken-gebruikersaccounts met BlueJeans.
services: active-directory
documentationcenter: ''
author: zhchia
writer: zhchia
manager: beatrizd-msft
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 84870ef03e07cdabe994da7c04a103bead48d34f
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67673000"
---
# <a name="tutorial-configure-bluejeans-for-automatic-user-provisioning"></a>Zelfstudie: BlueJeans configureren voor het automatisch inrichten van gebruikers

Het doel van deze zelfstudie is ter illustratie van de stappen om te worden uitgevoerd in de BlueJeans en Azure Active Directory (Azure AD) naar Azure AD configureren voor automatisch inrichten en verwijdering van gebruikers en/of groepen aan BlueJeans.

> [!NOTE]
> Deze zelfstudie beschrijft een connector die is gebaseerd op de Provisioning-Service van Azure AD-gebruiker. Zie voor belangrijke informatie over wat deze service biedt, hoe het werkt en veelgestelde vragen [automatiseren van gebruikersinrichting en -opheffing in SaaS-toepassingen met Azure Active Directory](../manage-apps/user-provisioning.md).

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al het volgende hebt:

* Een Azure AD-tenant
* Een tenant BlueJeans met de [mijn bedrijf](https://www.BlueJeans.com/pricing) plannen of beter ingeschakeld
* Een gebruikersaccount in BlueJeans met beheerdersmachtigingen

> [!NOTE]
> De integratie wordt ingericht op Azure AD is afhankelijk van de [BlueJeans API](https://BlueJeans.github.io/developer), die beschikbaar zijn voor teams BlueJeans Standard-abonnement of hoger is.

## <a name="adding-bluejeans-from-the-gallery"></a>BlueJeans toevoegen vanuit de galerie

Voordat u BlueJeans configureert voor het automatisch gebruikers inrichten met Azure AD, moet u BlueJeans uit de galerie met Azure AD toevoegen aan uw lijst met beheerde SaaS-toepassingen.

**Als u wilt toevoegen BlueJeans uit de galerie met Azure AD, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)** , selecteer in het navigatievenster aan de linkerkant **Azure Active Directory**.

    ![De Azure Active Directory-knop](common/select-azuread.png)

2. Ga naar **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.

    ![De blade Enterprise-toepassingen](common/enterprise-applications.png)

3. Als u wilt een nieuwe toepassing toevoegen, selecteert u de **nieuwe toepassing** knop aan de bovenkant van het deelvenster.

    ![De knop nieuwe toepassing](common/add-new-app.png)

4. Voer in het zoekvak **BlueJeans**, selecteer **BlueJeans** in het deelvenster met resultaten en selecteer vervolgens de **toevoegen** om toe te voegen van de toepassing.

    ![BlueJeans in de lijst met resultaten](common/search-new-app.png)

## <a name="assigning-users-to-bluejeans"></a>Gebruikers toewijzen aan BlueJeans

Azure Active Directory maakt gebruik van een concept genaamd "toewijzingen" om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het automatisch inrichten van gebruikers, worden alleen de gebruikers en/of groepen die '' aan een toepassing in Azure AD toegewezen zijn gesynchroniseerd.

Voordat u configureren en inschakelen van automatische inrichten van gebruikers, moet u bepalen welke gebruikers en/of groepen in Azure AD toegang hebben tot BlueJeans moeten. Wanneer u beslist, kunt u deze gebruikers en/of groepen toewijzen aan BlueJeans door de instructies hier:

* [Een gebruiker of groep toewijzen aan een enterprise-app](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-bluejeans"></a>Belangrijke tips voor het toewijzen van gebruikers aan BlueJeans

* Het wordt aanbevolen dat één Azure AD-gebruiker is toegewezen aan BlueJeans voor het testen van de configuratie van de automatische gebruikersinrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.

* Wanneer een gebruiker aan BlueJeans toewijzen, moet u alle geldige toepassingsspecifieke rollen (indien beschikbaar) selecteren in het dialoogvenster toewijzing. Gebruikers met de **standaardtoegang** rol worden uitgesloten van het inrichten.

## <a name="configuring-automatic-user-provisioning-to-bluejeans"></a>Automatisch gebruikers inrichten voor BlueJeans configureren

Deze sectie helpt u bij de stappen voor het configureren van de Azure AD-inrichtingsservice als u wilt maken, bijwerken en uitschakelen van gebruikers en/of groepen in BlueJeans op basis van gebruiker en/of groep toewijzingen in Azure AD.

> [!TIP]
> U kunt er ook voor kiezen om in te schakelen op SAML gebaseerde eenmalige aanmelding voor BlueJeans, vindt u de instructies te volgen in de [één BlueJeans aanmeldings-zelfstudie](bluejeans-tutorial.md). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatisch gebruikers inrichten, hoewel deze twee functies een fraaie aanvulling in elkaar.

### <a name="to-configure-automatic-user-provisioning-for-bluejeans-in-azure-ad"></a>Het configureren van automatisch gebruikers inrichten voor BlueJeans in Azure AD:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) en selecteer **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **BlueJeans**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer **BlueJeans** in de lijst met toepassingen.

    ![De koppeling BlueJeans in de lijst met toepassingen](common/all-applications.png)

3. Selecteer de **Provisioning** tabblad.

    ![BlueJeans Provisioning](./media/bluejeans-provisioning-tutorial/BluejeansProvisioningTab.png)

4. Stel de **Inrichtingsmodus** naar **automatische**.

    ![BlueJeans Provisioning](./media/bluejeans-provisioning-tutorial/Bluejeans1.png)

5. Onder de **beheerdersreferenties** sectie, voer de **Admin Username**, en **beheerderswachtwoord** van uw account BlueJeans. Voorbeelden van deze waarden zijn:

   * In de **Admin Username** veld, de gebruikersnaam van het beheerdersaccount dat op uw tenant BlueJeans vullen. Voorbeeld: admin@contoso.com.

   * In de **beheerderswachtwoord** veld, het wachtwoord dat overeenkomt met de gebruikersnaam van beheerder vullen.

6. Bij het invullen van de velden die in stap 5 wordt weergegeven, klikt u op **testverbinding** om te controleren of Azure AD kunt verbinden met BlueJeans. Als de verbinding is mislukt, zorg ervoor dat uw account BlueJeans beheerdersmachtigingen heeft en probeer het opnieuw.

    ![BlueJeans Provisioning](./media/bluejeans-provisioning-tutorial/BluejeansTestConnection.png)

7. In de **e-mailmelding** en voer het e-mailadres van een persoon of groep die u moet de inrichting fout ontvangen en schakel het selectievakje in - **een e-mailmelding verzenden wanneer een foutoptreedt**.

    ![BlueJeans Provisioning](./media/bluejeans-provisioning-tutorial/BluejeansNotificationEmail.png)

8. Klik op **Opslaan**.

9. Onder de **toewijzingen** sectie, selecteer **synchroniseren Azure Active Directory: gebruikers aan BlueJeans**.

    ![BlueJeans Provisioning](./media/bluejeans-provisioning-tutorial/BluejeansMapping.png)

10. Controleer de kenmerken van de gebruiker die van Azure AD worden gesynchroniseerd naar BlueJeans in de **kenmerk toewijzing** sectie. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikersaccounts in BlueJeans voor update-bewerkingen. Selecteer de **opslaan** knop wijzigingen doorvoeren.

    ![BlueJeans Provisioning](./media/bluejeans-provisioning-tutorial/BluejeansUserMappingAtrributes.png)

11. Als u wilt configureren bereikfilters, raadpleegt u de volgende instructies in de [Scoping filter zelfstudie](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

12. Wijzigen zodat de Azure AD-inrichtingsservice voor BlueJeans de **Inrichtingsstatus** naar **op** in de **instellingen** sectie.

    ![BlueJeans Provisioning](./media/bluejeans-provisioning-tutorial/BluejeansProvisioningStatus.png)

13. De gebruikers en/of groepen die u wilt definiëren voor het inrichten van BlueJeans door het kiezen van de gewenste waarden in **bereik** in de **instellingen** sectie.

    ![BlueJeans Provisioning](./media/bluejeans-provisioning-tutorial/UserGroupSelection.png)

14. Wanneer u klaar om in te richten bent, klikt u op **opslaan**.

    ![BlueJeans Provisioning](./media/bluejeans-provisioning-tutorial/SaveProvisioning.png)

Met deze bewerking wordt gestart voor de initiële synchronisatie van alle gebruikers en/of groepen die zijn gedefinieerd **bereik** in de **instellingen** sectie. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden als de Azure AD-inrichtingsservice wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en koppelingen volgen voor het inrichten van rapport van de activiteit, die alle acties die worden uitgevoerd door de Azure AD-inrichtingsservice op BlueJeans beschrijft.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over het inrichten van automatische gebruikersaccounts](../manage-apps/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Connector-beperkingen

* Bluejeans is niet toegestaan voor gebruikersnamen die meer dan 30 tekens bevatten.

## <a name="additional-resources"></a>Aanvullende resources

* [Het inrichten van gebruikersaccounts voor bedrijfs-Apps beheren](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over het controleren van Logboeken en rapporten over het inrichten van activiteit ophalen](../manage-apps/check-status-user-account-provisioning.md)

<!--Image references-->

[1]: ./media/bluejeans-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/bluejeans-tutorial/tutorial_general_02.png
[3]: ./media/bluejeans-tutorial/tutorial_general_03.png
