---
title: 'Zelf studie: Jive configureren voor het automatisch inrichten van gebruikers met Azure Active Directory | Microsoft Docs'
description: Meer informatie over de stappen die u moet uitvoeren in Jive en Azure AD om gebruikers accounts van Azure AD automatisch in te richten en te deactiveren naar Jive.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: article
ms.date: 01/26/2018
ms.author: jeedes
ms.openlocfilehash: fa45ece76499b50694d366ed2dcdd57cc1e0c7dc
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "91312093"
---
# <a name="tutorial-configure-jive-for-automatic-user-provisioning"></a>Zelf studie: Jive configureren voor automatische gebruikers inrichting

Het doel van deze zelf studie is om u te laten zien welke stappen u moet uitvoeren in Jive en Azure AD om gebruikers accounts van Azure AD automatisch in te richten en te deactiveren naar Jive.

## <a name="prerequisites"></a>Vereisten

In het scenario dat in deze zelfstudie wordt beschreven, wordt ervan uitgegaan dat u al beschikt over de volgende items:

*   Een Azure Active Directory-Tenant.
*   Een Jive-abonnement dat is ingeschakeld voor eenmalige aanmelding.
*   Een gebruikers account in Jive met team beheerders machtigingen.

## <a name="assigning-users-to-jive"></a>Gebruikers toewijzen aan Jive

Azure Active Directory gebruikt een concept met de naam 'toewijzingen' om te bepalen welke gebruikers toegang moeten krijgen tot geselecteerde apps. In de context van het automatisch inrichten van gebruikers accounts worden alleen de gebruikers en groepen die zijn toegewezen aan een toepassing in azure AD gesynchroniseerd.

Voordat u de inrichtings service configureert en inschakelt, moet u bepalen welke gebruikers en/of groepen in azure AD de gebruikers vertegenwoordigen die toegang nodig hebben tot uw Jive-app. Nadat u hebt besloten, kunt u deze gebruikers toewijzen aan uw Jive-app door de volgende instructies te volgen:

[Een gebruiker of groep toewijzen aan een bedrijfs-app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-jive"></a>Belang rijke tips voor het toewijzen van gebruikers aan Jive

*   U wordt aangeraden één Azure AD-gebruiker toe te wijzen aan Jive om de inrichtings configuratie te testen. Extra gebruikers en/of groepen kunnen later worden toegewezen.

*   Wanneer u een gebruiker toewijst aan Jive, moet u een geldige gebruikersrol selecteren. De rol ' standaard toegang ' werkt niet voor het inrichten.

## <a name="enable-user-provisioning"></a>Gebruikers inrichten inschakelen

In deze sectie vindt u instructies voor het verbinden van uw Azure AD-Jive en het configureren van de inrichtings service om toegewezen gebruikers accounts in Jive te maken, bij te werken en uit te scha kelen op basis van de gebruikers-en groeps toewijzing in azure AD.

> [!TIP]
> U kunt er ook voor kiezen om op SAML gebaseerde enkelvoudige Sign-On voor Jive in te scha kelen, gevolgd door de instructies in [Azure Portal](https://portal.azure.com). Eenmalige aanmelding kan onafhankelijk van automatische inrichting worden geconfigureerd, maar deze twee functies gelden voor elkaar.

### <a name="to-configure-user-account-provisioning"></a>Het inrichten van een gebruikers account configureren:

Het doel van deze sectie is het maken van een overzicht van de gebruikers inrichting van Active Directory gebruikers accounts in te stellen op Jive.
Als onderdeel van deze procedure moet u een beveiligings token van de gebruiker opgeven dat u moet aanvragen bij Jive.com.

1. Blader in het [Azure Portal](https://portal.azure.com)naar het gedeelte **Azure Active Directory > Enter prise-apps > alle toepassingen** .

1. Als u Jive al hebt geconfigureerd voor eenmalige aanmelding, zoekt u naar uw instantie van Jive met behulp van het zoek veld. Als dat niet het geval is, selecteert u **toevoegen** en zoeken naar **Jive** in de toepassings galerie. Selecteer Jive in de zoek resultaten en voeg deze toe aan uw lijst met toepassingen.

1. Selecteer uw exemplaar van Jive en selecteer vervolgens het tabblad **inrichten** .

1. Stel de **Inrichtingsmodus** in op **Automatisch**. 

    ![In de scherm afbeelding wordt de Jive-inrichtings pagina weer gegeven, waarbij de inrichtings modus is ingesteld op automatisch en andere waarden die u kunt instellen.](./media/jive-provisioning-tutorial/provisioning.png)

1. Geef onder de sectie **beheerders referenties** de volgende configuratie-instellingen op:
   
    a. Typ in het tekstvak **Jive beheer gebruikers naam** een Jive-account naam waaraan het profiel van de **systeem beheerder** is toegewezen in Jive.com.
   
    b. Typ het wacht woord voor dit account in het tekstvak **Jive beheerders wachtwoord** .
   
    c. Typ in het tekstvak **Jive Tenant-URL** de URL van de Jive-Tenant.
      
      > [!NOTE]
      > De Jive-Tenant-URL is de URL die door uw organisatie wordt gebruikt om u aan te melden bij Jive.  
      > Normaal gesp roken heeft de URL de volgende indeling: **www. \<organization\> . jive.com**.          

1. Klik in het Azure Portal op **verbinding testen** om ervoor te zorgen dat Azure AD verbinding kan maken met uw Jive-app.

1. Voer het e-mail adres in van een persoon of groep die inrichtings fout meldingen moet ontvangen in het veld **e-mail melding** en schakel het selectie vakje hieronder in.

1. Klik op **opslaan.**

1. Selecteer in de sectie toewijzingen de optie **Azure Active Directory gebruikers synchroniseren met Jive.**

1. Controleer in de sectie **kenmerk toewijzingen** de gebruikers kenmerken die zijn gesynchroniseerd vanuit Azure AD naar Jive. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt om te voldoen aan de gebruikers accounts in Jive voor bijwerk bewerkingen. Selecteer de knop Opslaan om eventuele wijzigingen door te voeren.

1. Als u de Azure AD-inrichtings service voor Jive wilt inschakelen, wijzigt u de **inrichtings status** **in in het** gedeelte instellingen

1. Klik op **opslaan.**

Hiermee start u de initiële synchronisatie van gebruikers en/of groepen die zijn toegewezen aan Jive in de sectie gebruikers en groepen. Het duurt langer voordat de initiële synchronisatie is uitgevoerd dan volgende synchronisaties, die ongeveer elke 40 minuten plaatsvinden, zolang de service wordt uitgevoerd. U kunt de sectie **synchronisatie Details** gebruiken om de voortgang te bewaken en koppelingen te volgen voor het inrichtings logboek, waarin alle acties worden beschreven die worden uitgevoerd door de inrichtings service in uw Jive-app.

Zie [Rapportage over automatische inrichting van gebruikersaccounts](../app-provisioning/check-status-user-account-provisioning.md) voor informatie over het lezen van de Azure AD-inrichtingslogboeken.

## <a name="additional-resources"></a>Aanvullende resources

* [Gebruikersaccountinrichting voor zakelijke apps beheren](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)
* [Eenmalige aanmelding configureren](jive-tutorial.md)
