---
title: 'Zelf studie: globale relay-identiteits synchronisatie configureren voor het automatisch inrichten van gebruikers met Azure Active Directory | Microsoft Docs'
description: Meer informatie over het automatisch inrichten en ongedaan maken van de inrichting van gebruikers accounts vanuit Azure AD naar de globale relay-identiteits synchronisatie.
services: active-directory
documentationcenter: ''
author: Zhchia
writer: Zhchia
manager: beatrizd
ms.assetid: 0c4a3bf0-d0a6-4eab-909b-6cf9f9234e4c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/22/2020
ms.author: Zhchia
ms.openlocfilehash: 400e82780abd08e0db4f49d72b352e290ea1f212
ms.sourcegitcommit: 693df7d78dfd5393a28bf1508e3e7487e2132293
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/28/2020
ms.locfileid: "92900300"
---
# <a name="tutorial-configure-global-relay-identity-sync-for-automatic-user-provisioning"></a>Zelf studie: globale relay-identiteits synchronisatie configureren voor automatische gebruikers inrichting

In deze zelf studie worden de stappen beschreven die u moet uitvoeren in zowel de globale relay-identiteits synchronisatie als Azure Active Directory (Azure AD) voor het configureren van automatische gebruikers inrichting. Wanneer deze is geconfigureerd, worden gebruikers en groepen automatisch door Azure AD ingericht en ongedaan gemaakt voor globale relay-identiteits synchronisatie met behulp van de Azure AD-inrichtings service. Zie voor belangrijke details over wat deze service doet, hoe het werkt en veelgestelde vragen [Inrichting en ongedaan maken van inrichting van gebruikers automatiseren naar SaaS-toepassingen met Azure Active Directory](../manage-apps/user-provisioning.md). 


## <a name="capabilities-supported"></a>Ondersteunde mogelijkheden
> [!div class="checklist"]
> * Gebruikers maken in globale relay-identiteits synchronisatie
> * Gebruikers in globale relay-identiteits synchronisatie verwijderen wanneer ze niet meer toegang nodig hebben
> * Gebruikers kenmerken gesynchroniseerd laten tussen Azure AD en globale relay-identiteits synchronisatie
> * Inrichtings groepen en groepslid maatschappen in globale relay-identiteits synchronisatie

## <a name="prerequisites"></a>Vereisten

In het scenario dat in deze zelfstudie wordt beschreven, wordt ervan uitgegaan dat u al beschikt over de volgende vereisten:

* [Een Azure AD-Tenant](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) 
* Een gebruikers account in azure AD met [toestemming](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) voor het configureren van inrichting (bijvoorbeeld toepassings beheerder, Cloud toepassings beheerder, eigenaar van de toepassing of globale beheerder).

## <a name="step-1-plan-your-provisioning-deployment"></a>Stap 1. Implementatie van de inrichting plannen
1. Lees [hoe de inrichtingsservice werkt](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).
2. Bepaal wie u wilt opnemen in het [bereik voor inrichting](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts).
3. Bepaal welke gegevens moeten worden [toegewezen tussen Azure AD en globale relay-identiteits synchronisatie](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes). 

## <a name="step-2-configure-global-relay-identity-sync-to-support-provisioning-with-azure-ad"></a>Stap 2. Globale relay-identiteits synchronisatie configureren ter ondersteuning van inrichting met Azure AD

Neem contact op met de vertegenwoordiger van de globale relay-identiteit om de Tenant-URL te ontvangen. Deze waarde wordt ingevoerd in het veld **Tenant-URL** op het tabblad inrichting van uw globale synchronisatie toepassing voor relay-identiteit in de Azure Portal.

## <a name="step-3-add-global-relay-identity-sync-from-the-azure-ad-application-gallery"></a>Stap 3. Globale relay-identiteits synchronisatie toevoegen vanuit de Azure AD-toepassings galerie

Voeg de globale relay-identiteits synchronisatie toe vanuit de Azure AD-toepassings galerie om het beheer van de inrichting van de globale relay-identiteits synchronisatie te starten. Meer informatie over het toevoegen van een toepassing uit [de galerie.](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app) 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>Stap 4. Definiëren wie u wilt opnemen in het bereik voor inrichting 

Met de Azure AD-inrichtingsservice kunt u bepalen wie worden ingericht op basis van toewijzing aan de toepassing en/of op basis van kenmerken van de gebruiker/groep. Als u ervoor kiest om te bepalen wie wordt ingericht voor uw app op basis van toewijzing, kunt u de volgende [stappen](../manage-apps/assign-user-or-group-access-portal.md) gebruiken om gebruikers en groepen aan de toepassing toe te wijzen. Als u ervoor kiest om uitsluitend te bepalen wie wordt ingericht op basis van kenmerken van de gebruiker of groep, kunt u een bereikfilter gebruiken zoals [hier](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts) wordt beschreven. 

* Begin klein. Test de toepassing met een kleine set gebruikers en groepen voordat u de toepassing naar iedereen uitrolt. Wanneer het bereik voor inrichting is ingesteld op toegewezen gebruikers en groepen, kunt u dit beheren door een of twee gebruikers of groepen aan de app toe te wijzen. Wanneer het bereik is ingesteld op alle gebruikers en groepen, kunt u een [bereikfilter op basis van kenmerken](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts) opgeven. 


## <a name="step-5-configure-automatic-user-provisioning-to-global-relay-identity-sync"></a>Stap 5. Automatische gebruikers inrichting configureren voor synchronisatie van globale relay-identiteit 

In deze sectie wordt u begeleid bij de stappen voor het configureren van de Azure AD-inrichtings service om gebruikers en/of groepen te maken, bij te werken en uit te scha kelen in globale relay-identiteits synchronisatie-app op basis van gebruikers-en/of groeps toewijzingen in azure AD.

### <a name="to-configure-automatic-user-provisioning-for-global-relay-identity-sync-in-azure-ad"></a>Automatische gebruikers inrichting configureren voor de synchronisatie van globale relay-identiteiten in azure AD:

1. Meld u aan bij de [Azure-portal](https://portal.azure.com). Selecteer **Bedrijfstoepassingen** en vervolgens **Alle toepassingen** .

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst toepassingen de optie **globale relay-identiteits synchronisatie** .

    ![De koppeling globale relay-identiteits synchronisatie in de lijst met toepassingen](common/all-applications.png)

3. Selecteer het tabblad **Inrichten** .

    ![Tabblad Inrichting](common/provisioning.png)

4. Stel de **Inrichtingsmodus** in op **Automatisch** .

    ![Tabblad inrichten automatisch](common/provisioning-automatic.png)

5. Geef in het gedeelte **beheerders referenties** de URL op van de globale relay-identiteits synchronisatie **Tenant** . Klik op **verbinding testen** om te controleren of Azure AD verbinding kan maken met de globale relay-identiteits synchronisatie. Als de verbinding mislukt, zorgt u ervoor dat uw globale relay-identiteits synchronisatie account beheerders machtigingen heeft en neemt u contact op met uw globale relay-vertegenwoordiger om het probleem op te lossen.

    ![Autorisatie knop](media/global-relay-identity-sync-provisioning-tutorial/authorization.png)

6. Voer in het veld **E-mailadres voor meldingen** het e-mailadres in van een persoon of groep die de inrichtingsfoutmeldingen zou moeten ontvangen en schakel het selectievakje **Een e-mailmelding verzenden als een fout optreedt** in.

    ![E-mailadres voor meldingen](common/provisioning-notification-email.png)

7. Selecteer **Opslaan** .

8. Selecteer in de sectie **toewijzingen** de optie **Azure Active Directory gebruikers synchroniseren met globale relay-identiteits synchronisatie** .

9. Controleer de gebruikers kenmerken die zijn gesynchroniseerd vanuit Azure AD naar globale relay-identiteits synchronisatie in de sectie **kenmerk toewijzing** . De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt om te voldoen aan de gebruikers accounts in de globale relay-identiteits synchronisatie voor bijwerk bewerkingen. Als u ervoor kiest om het [overeenkomende doel kenmerk](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)te wijzigen, moet u ervoor zorgen dat de API voor synchronisatie van globale relay-identiteiten het filteren van gebruikers op basis van dat kenmerk ondersteunt. Selecteer de knop **Opslaan** om eventuele wijzigingen door te voeren.

   |Kenmerk|Type|
   |---|---|
   |userName|Tekenreeks|
   |actief|Booleaans|
   |displayName|Tekenreeks|
   |title|Tekenreeks|
   |preferredLanguage|Tekenreeks|
   |name.givenName|Tekenreeks|
   |name.familyName|Tekenreeks|
   |naam. opgemaakt|Tekenreeks|
   |adressen [type EQ "werk]. opgemaakt|Tekenreeks|
   |addresses[type eq "work"].streetAddress|Tekenreeks|
   |emails[type eq "work"].value|Tekenreeks|
   |adressen [type EQ "werk]. locatie|Tekenreeks|
   |adressen [type EQ "werk]. regio|Tekenreeks|
   |addresses[type eq "work"].postalCode|Tekenreeks|
   |adressen [type EQ "werk]. land|Tekenreeks|
   |adressen [type EQ "other"]. Format|Tekenreeks|
   |phoneNumbers[type eq "work"].value|Tekenreeks|
   |phoneNumbers[type eq "mobile"].value|Tekenreeks|
   |phoneNumbers[type eq "fax"].value|Tekenreeks|
   |externalId|Tekenreeks|
   |name.honorificPrefix|Tekenreeks|
   |naam. honorificSuffix|Tekenreeks|
   |nickName|Tekenreeks|
   |userType|Tekenreeks|
   |landinstelling|Tekenreeks|
   |timezone|Tekenreeks|
   |e-mail berichten [type EQ "Home"]. waarde|Tekenreeks|
   |e-mail berichten [type EQ "Overig"]. waarde|Tekenreeks|
   |phoneNumbers [type EQ "Home"]. waarde|Tekenreeks|
   |phoneNumbers [type EQ "other"]. waarde|Tekenreeks|
   |phoneNumbers [type EQ "semafoon"]. waarde|Tekenreeks|
   |adressen [type EQ "Home"]. streetAddress|Tekenreeks|
   |adressen [type EQ "Home"]. locatie|Tekenreeks|
   |adressen [type EQ "Home"]. Region|Tekenreeks|
   |adressen [type EQ "Home"]. post code|Tekenreeks|
   |adressen [type EQ "Home"]. Country|Tekenreeks|
   |adressen [type EQ "Home"]. geformatteerd|Tekenreeks|
   |adressen [type EQ "other"]. streetAddress|Tekenreeks|
   |adressen [type EQ "other"]. locatie|Tekenreeks|
   |adressen [type EQ "other"]. regio|Tekenreeks|
   |adressen [type EQ "other"]. post code|Tekenreeks|
   |adressen [type EQ "other"]. Country|Tekenreeks|
   |rollen [Primary EQ "True"]. display|Tekenreeks|
   |rollen [Primary EQ "True"]. type|Tekenreeks|
   |roles[primary eq "True"].value|Tekenreeks|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:employeeNumber|Tekenreeks|
   |urn: IETF: params: scim: schemas: extensie: Enter prise: 2.0: gebruiker: costCenter|Tekenreeks|
   |urn: IETF: params: scim: schemas: extensie: Enter prise: 2.0: gebruiker: organisatie|Tekenreeks|
   |urn: IETF: params: scim: schemas: extensie: Enter prise: 2.0: gebruiker: deling|Tekenreeks|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:department|Tekenreeks|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:manager|Referentie|
   |urn: IETF: params: scim: schemas: extensie: GlobalRelay: 2.0: gebruiker: proxyAddresses|Tekenreeks|
   |urn: IETF: params: scim: schemas: extensie: GlobalRelay: 2.0: gebruiker: extensionAttribute1|Tekenreeks|
   |urn: IETF: params: scim: schemas: extensie: GlobalRelay: 2.0: gebruiker: extensionAttribute2|Tekenreeks|
   |urn: IETF: params: scim: schemas: extensie: GlobalRelay: 2.0: gebruiker: extensionAttribute3|Tekenreeks|
   |urn: IETF: params: scim: schemas: extensie: GlobalRelay: 2.0: gebruiker: extensionAttribute4|Tekenreeks|
   |urn: IETF: params: scim: schemas: extensie: GlobalRelay: 2.0: gebruiker: extensionAttribute5|Tekenreeks|
   |urn: IETF: params: scim: schemas: extensie: GlobalRelay: 2.0: gebruiker: extensionAttribute6|Tekenreeks|
   |urn: IETF: params: scim: schemas: extensie: GlobalRelay: 2.0: gebruiker: extensionAttribute7|Tekenreeks|
   |urn: IETF: params: scim: schemas: extensie: GlobalRelay: 2.0: gebruiker: extensionAttribute8|Tekenreeks|
   |urn: IETF: params: scim: schemas: extensie: GlobalRelay: 2.0: gebruiker: extensionAttribute9|Tekenreeks|
   |urn: IETF: params: scim: schemas: extensie: GlobalRelay: 2.0: gebruiker: extensionAttribute10|Tekenreeks|
   |urn: IETF: params: scim: schemas: extensie: GlobalRelay: 2.0: gebruiker: extensionAttribute11|Tekenreeks|
   |urn: IETF: params: scim: schemas: extensie: GlobalRelay: 2.0: gebruiker: extensionAttribute12|Tekenreeks|
   |urn: IETF: params: scim: schemas: extensie: GlobalRelay: 2.0: gebruiker: extensionAttribute13|Tekenreeks|
   |urn: IETF: params: scim: schemas: extensie: GlobalRelay: 2.0: gebruiker: extensionAttribute14|Tekenreeks|
   |urn: IETF: params: scim: schemas: extensie: GlobalRelay: 2.0: gebruiker: extensionAttribute15|Tekenreeks|



10. Selecteer in de sectie **toewijzingen** de optie **Azure Active Directory groepen synchroniseren met globale relay-identiteits synchronisatie** .

11. Controleer de groeps kenmerken die zijn gesynchroniseerd vanuit Azure AD naar Global relay-identiteits synchronisatie in de sectie **kenmerk toewijzing** . De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen, worden gebruikt voor het vergelijken van de groepen in de globale relay-identiteits synchronisatie voor bijwerk bewerkingen. Selecteer de knop **Opslaan** om eventuele wijzigingen door te voeren.

      |Kenmerk|Type|
      |---|---|
      |displayName|Tekenreeks|
      |leden|Naslaginformatie|

12. Als u bereikfilters wilt configureren, raadpleegt u de volgende instructies in de [zelfstudie Bereikfilter](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

13. Als u de Azure AD-inrichtings service voor globale relay-identiteits synchronisatie wilt inschakelen, **wijzigt u de** **inrichtings status** in in het gedeelte **instellingen** .

    ![Inrichtingsstatus ingeschakeld](common/provisioning-toggle-on.png)

14. Definieer de gebruikers en/of groepen die u wilt inrichten voor globale relay-identiteits synchronisatie door de gewenste waarden in het **bereik** in de sectie **instellingen** te kiezen.

    ![Inrichtingsbereik](common/provisioning-scope.png)

15. Wanneer u klaar bent om in te richten, klikt u op **Opslaan** .

    ![Inrichtingsconfiguratie opslaan](common/provisioning-configuration-save.png)

Met deze bewerking wordt de eerste synchronisatiecyclus gestart van alle gebruikers en groepen die zijn gedefinieerd onder **Bereik** in de sectie **Instellingen** . De initiële cyclus duurt langer dan volgende cycli, die ongeveer om de 40 minuten plaatsvinden zolang de Azure AD-inrichtingsservice wordt uitgevoerd. 

## <a name="step-6-monitor-your-deployment"></a>Stap 6. Uw implementatie bewaken
Nadat u het inrichten hebt geconfigureerd, gebruikt u de volgende resources om uw implementatie te bewaken:

1. Gebruik de [inrichtingslogboeken](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs) om te bepalen welke gebruikers al dan niet met succes zijn ingericht
2. Controleer de [voortgangsbalk](https://docs.microsoft.com/azure/active-directory/app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user) om de status van de inrichtingscyclus weer te geven en te zien of deze al bijna is voltooid
3. Als het configureren van de inrichting een foutieve status lijkt te hebben, wordt de toepassing in quarantaine geplaatst. [Klik hier](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status) voor meer informatie over quarantainestatussen.  

## <a name="additional-resources"></a>Aanvullende resources

* [Gebruikersaccountinrichting voor zakelijke apps beheren](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over het controleren van logboeken en het ophalen van rapporten over de inrichtingsactiviteit](../manage-apps/check-status-user-account-provisioning.md)
