---
title: 'Zelf studie: Configure SAP SuccessFactors write-back in Azure Active Directory | Microsoft Docs'
description: Meer informatie over het configureren van een kenmerk write-back naar SAP SuccessFactors vanuit Azure AD
services: active-directory
author: cmmdesai
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.topic: article
ms.workload: identity
ms.date: 10/14/2020
ms.author: chmutali
ms.openlocfilehash: bbd274f6b039ef4492068d939c755ab279c2830a
ms.sourcegitcommit: a92fbc09b859941ed64128db6ff72b7a7bcec6ab
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/15/2020
ms.locfileid: "92069978"
---
# <a name="tutorial-configure-attribute-write-back-from-azure-ad-to-sap-successfactors"></a>Zelf studie: kenmerk write-back van Azure AD naar SAP SuccessFactors configureren
Het doel van deze zelf studie is het weer geven van de stappen voor het terugschrijven van kenmerken van Azure AD naar SAP SuccessFactors Employee Central. 

## <a name="overview"></a>Overzicht

U kunt de SAP SuccessFactors write-app configureren voor het schrijven van specifieke kenmerken van Azure Active Directory naar SAP SuccessFactors Employee Central. De SuccessFactors write inrichtings-app ondersteunt het toewijzen van waarden aan de volgende centrale kenmerken van werk nemers:

* E-mail werk
* Gebruikersnaam
* Zakelijk telefoon nummer (inclusief land nummer, netnummer, cijfer en uitbrei ding)
* Zakelijke telefoon nummer primaire vlag
* Mobiele telefoon nummer (inclusief land nummer, netnummer, getal)
* Primaire vlag voor mobiele telefoon 
* Gebruikers-custom01-custom15-kenmerken
* loginMethod-kenmerk

> [!NOTE]
> Deze app heeft geen afhankelijkheid van de SuccessFactors inkomende integratie-apps voor het inrichten van gebruikers rechten. U kunt deze onafhankelijk van [SuccessFactors configureren voor on-premises AD-](sap-successfactors-inbound-provisioning-tutorial.md) inrichtings-app of [SUCCESSFACTORS naar Azure AD](sap-successfactors-inbound-provisioning-cloud-only-tutorial.md) -inrichtings toepassing.

### <a name="who-is-this-user-provisioning-solution-best-suited-for"></a>Wie is deze oplossing voor het inrichten van de gebruiker geschikt voor?

Deze SuccessFactors write-inrichtings oplossing voor gebruikers is ideaal voor:

* Organisaties die Microsoft 365 gebruiken om gezaghebbende kenmerken die door IT worden beheerd (zoals e-mail adres, telefoon, gebruikers naam), terug te schrijven naar SuccessFactors werk nemers centraal.

## <a name="configuring-successfactors-for-the-integration"></a>SuccessFactors configureren voor de integratie

Alle SuccessFactors-inrichtings connectors vereisen referenties van een SuccessFactors-account met de juiste machtigingen om de Centraal OData-Api's van de werk nemers aan te roepen. In deze sectie worden de stappen beschreven voor het maken van het service account in SuccessFactors en het verlenen van de juiste machtigingen. 

* [API-gebruikers account maken/identificeren in SuccessFactors](#createidentify-api-user-account-in-successfactors)
* [Een functie voor API-machtigingen maken](#create-an-api-permissions-role)
* [Een machtigings groep maken voor de API-gebruiker](#create-a-permission-group-for-the-api-user)
* [Machtigings functie verlenen aan de machtigings groep](#grant-permission-role-to-the-permission-group)

### <a name="createidentify-api-user-account-in-successfactors"></a>API-gebruikers account maken/identificeren in SuccessFactors
Werk samen met uw SuccessFactors-beheer team of implementatie partner om een gebruikers account in SuccessFactors te maken of te identificeren dat wordt gebruikt om de OData-Api's aan te roepen. De referenties van de gebruikers naam en het wacht woord van dit account zijn vereist bij het configureren van de inrichtings-apps in azure AD. 

### <a name="create-an-api-permissions-role"></a>Een functie voor API-machtigingen maken

1. Meld u aan bij SAP SuccessFactors met een gebruikers account dat toegang heeft tot het beheer centrum.
1. Zoek naar *machtigings rollen beheren*en selecteer vervolgens **machtigings rollen beheren** in de zoek resultaten.

   ![Machtigings rollen beheren](./media/sap-successfactors-inbound-provisioning/manage-permission-roles.png)

1. Klik in de lijst machtigings rollen op **nieuwe maken**.

   > [!div class="mx-imgBorder"]
   > ![Nieuwe machtigings functie maken](./media/sap-successfactors-inbound-provisioning/create-new-permission-role-1.png)

1. Voeg een **rolnaam** en een **Beschrijving** voor de nieuwe machtigings functie toe. De naam en beschrijving moeten aangeven dat de rol is voor API-gebruiks machtigingen.

   > [!div class="mx-imgBorder"]
   > ![Details van machtigings rol](./media/sap-successfactors-inbound-provisioning/permission-role-detail.png)

1. Klik onder machtigings instellingen op **machtiging...**, Schuif omlaag in de lijst met machtigingen en klik op **integratie hulpprogramma's beheren**. Schakel het selectie vakje in om de beheerder toe te **staan om toegang te krijgen tot ODATA API via basis verificatie**.

   > [!div class="mx-imgBorder"]
   > ![Integratie hulpprogramma's beheren](./media/sap-successfactors-inbound-provisioning/manage-integration-tools.png)

1. Schuif omlaag in hetzelfde vak en selecteer de **centrale API voor werk nemers**. Voeg machtigingen toe, zoals hieronder wordt weer gegeven, voor meer informatie over het gebruik van ODATA API en Edit met de ODATA-API. Selecteer de optie bewerken als u hetzelfde account wilt gebruiken voor het SuccessFactors-scenario write-back naar. 

   > [!div class="mx-imgBorder"]
   > ![Lees machtigingen voor schrijven](./media/sap-successfactors-inbound-provisioning/odata-read-write-perm.png)

1. Klik op **gereed**. Klik op **Wijzigingen opslaan**.

### <a name="create-a-permission-group-for-the-api-user"></a>Een machtigings groep maken voor de API-gebruiker

1. Zoek in het SuccessFactors-beheer centrum naar *machtigings groepen beheren*en selecteer vervolgens **machtigings groepen beheren** in de zoek resultaten.

   > [!div class="mx-imgBorder"]
   > ![Machtigings groepen beheren](./media/sap-successfactors-inbound-provisioning/manage-permission-groups.png)

1. Klik in het venster machtigings groepen beheren op **nieuwe maken**.

   > [!div class="mx-imgBorder"]
   > ![Nieuwe groep toevoegen](./media/sap-successfactors-inbound-provisioning/create-new-group.png)

1. Voeg een groeps naam voor de nieuwe groep toe. De groeps naam moet aangeven dat de groep voor API-gebruikers is.

   > [!div class="mx-imgBorder"]
   > ![Naam van de machtigings groep](./media/sap-successfactors-inbound-provisioning/permission-group-name.png)

1. Leden toevoegen aan de groep. U kunt bijvoorbeeld **gebruikers naam** selecteren in de vervolg keuzelijst personen groep en vervolgens de gebruikers naam invoeren van het API-account dat wordt gebruikt voor de integratie. 

   > [!div class="mx-imgBorder"]
   > ![Groepsleden toevoegen](./media/sap-successfactors-inbound-provisioning/add-group-members.png)

1. Klik op **gereed** om het maken van de machtigings groep te volt ooien.

### <a name="grant-permission-role-to-the-permission-group"></a>Machtigings functie verlenen aan de machtigings groep

1. Zoek in SuccessFactors-beheer centrum naar *machtigings rollen beheren*en selecteer vervolgens **machtigings rollen beheren** in de zoek resultaten.
1. Selecteer in de **lijst machtigings rollen**de rol die u hebt gemaakt voor de machtigingen voor API-gebruik.
1. Onder **deze rol toekennen aan..**. klikt u op de knop **toevoegen..** ..
1. Selecteer de **machtigings groep..** . in de vervolg keuzelijst en klik vervolgens op **selecteren...** om het venster groepen te openen, te zoeken en de eerder gemaakte groep te selecteren. 

   > [!div class="mx-imgBorder"]
   > ![Machtigings groep toevoegen](./media/sap-successfactors-inbound-provisioning/add-permission-group.png)

1. Controleer de machtigings rol verlenen aan de machtigings groep. 
   > [!div class="mx-imgBorder"]
   > ![Rol en groeps Details van machtiging](./media/sap-successfactors-inbound-provisioning/permission-role-group.png)

1. Klik op **Wijzigingen opslaan**.

## <a name="preparing-for-successfactors-writeback"></a>Write-back van SuccessFactors voorbereiden

De SuccessFactors write inrichtings-app gebruikt bepaalde *code* waarden voor het instellen van e-mail en telefoon nummers in werk nemers centraal. Deze *code* waarden worden ingesteld als constante waarden in de kenmerk toewijzings tabel en zijn verschillend voor elk SuccessFactors-exemplaar. Deze sectie bevat stappen voor het vastleggen van deze *code* waarden.

   > [!NOTE]
   > Zorg dat uw SuccessFactors-beheerder de stappen in deze sectie heeft voltooid. 

### <a name="identify-email-and-phone-number-picklist-names"></a>Namen van selectie lijsten voor E-mail en telefoon nummers identificeren 

In SAP SuccessFactors is een *selectie lijst* een Configureer bare set opties waaruit een gebruiker een selectie kan maken. De verschillende soorten e-mail adres en telefoon nummer (bijvoorbeeld zakelijk, persoonlijk, Overig) worden weer gegeven met een selectie lijst. In deze stap bepalen we de selectie lijsten die in de SuccessFactors-Tenant zijn geconfigureerd voor het opslaan van de waarden voor e-mail en telefoon nummer. 
 
1. Zoek in SuccessFactors-beheer centrum naar *bedrijfs configuratie beheren*. 

   > [!div class="mx-imgBorder"]
   > ![Bedrijfs configuratie beheren](./media/sap-successfactors-inbound-provisioning/manage-business-config.png)

1. Selecteer **onder HRIS**-elementen **emailInfo** en klik op de *Details* van het veld **e-type** .

   > [!div class="mx-imgBorder"]
   > ![E-mail gegevens ophalen](./media/sap-successfactors-inbound-provisioning/get-email-info.png)

1. Noteer op de pagina Details van het **e-mail bericht** de naam van de selectie lijst die is gekoppeld aan dit veld. Standaard is dit **ecEmailType**. Het kan echter ook verschillen in uw Tenant. 

   > [!div class="mx-imgBorder"]
   > ![Selectie lijst voor e-mail identificeren](./media/sap-successfactors-inbound-provisioning/identify-email-picklist.png)

1. Selecteer **onder HRIS**-elementen **phoneInfo** en klik op de *Details* van het veld **type telefoon** .

   > [!div class="mx-imgBorder"]
   > ![Telefoon gegevens ophalen](./media/sap-successfactors-inbound-provisioning/get-phone-info.png)

1. Op de pagina Details van de **telefoon** geeft u de naam van de selectie lijst aan die is gekoppeld aan dit veld. Standaard is dit **ecPhoneType**. Het kan echter ook verschillen in uw Tenant. 

   > [!div class="mx-imgBorder"]
   > ![Telefoon selectie identificeren](./media/sap-successfactors-inbound-provisioning/identify-phone-picklist.png)

### <a name="retrieve-constant-value-for-emailtype"></a>Constante waarde ophalen voor emailType

1. Zoek en open in het SuccessFactors-beheer centrum op *selectielijst centrum*. 
1. Gebruik de naam van de in de vorige sectie vastgelegde e-mail keuze lijst (bijvoorbeeld ecEmailType) om de e-mail selectie lijst te vinden. 

   > [!div class="mx-imgBorder"]
   > ![Selectie lijst voor e-mail type zoeken](./media/sap-successfactors-inbound-provisioning/find-email-type-picklist.png)

1. Open de selectie lijst voor actieve e-mail. 

   > [!div class="mx-imgBorder"]
   > ![Selectie lijst actieve e-mail type openen](./media/sap-successfactors-inbound-provisioning/open-active-email-type-picklist.png)

1. Selecteer op de pagina type selectie van e-mail het type *zakelijk* e-mail adres.

   > [!div class="mx-imgBorder"]
   > ![Type zakelijk e-mail selecteren](./media/sap-successfactors-inbound-provisioning/select-business-email-type.png)

1. Noteer de **optie-id** die is gekoppeld aan het *zakelijke* e-mail adres. Dit is de code die we gaan gebruiken met *emailType* in de kenmerk toewijzings tabel.

   > [!div class="mx-imgBorder"]
   > ![E-mail type code ophalen](./media/sap-successfactors-inbound-provisioning/get-email-type-code.png)

   > [!NOTE]
   > Verwijder het komma teken wanneer u de waarde kopieert. Voor beeld: als de waarde van de **optie-ID** *8.448*is, stelt u de *emailType* in azure AD in op het constante nummer *8448* (zonder het komma teken). 

### <a name="retrieve-constant-value-for-phonetype"></a>Constante waarde ophalen voor phoneType

1. Zoek en open in het SuccessFactors-beheer centrum op *selectielijst centrum*. 
1. Gebruik de naam van de telefoon selectie die is vastgelegd in de vorige sectie om de telefoon keuze lijst te vinden. 

   > [!div class="mx-imgBorder"]
   > ![Selectie lijst voor telefoon type zoeken](./media/sap-successfactors-inbound-provisioning/find-phone-type-picklist.png)

1. Open de selectie lijst met actieve telefoons. 

   > [!div class="mx-imgBorder"]
   > ![Selectie lijst actieve telefoon type openen](./media/sap-successfactors-inbound-provisioning/open-active-phone-type-picklist.png)

1. Bekijk op de pagina selectie lijst voor telefoon type de verschillende typen telefoons onder **selectie lijst waarden**.

   > [!div class="mx-imgBorder"]
   > ![Telefoon typen controleren](./media/sap-successfactors-inbound-provisioning/review-phone-types.png)

1. Noteer de **optie-id** die is gekoppeld aan de *zakelijke* telefoon. Dit is de code die we gaan gebruiken met *businessPhoneType* in de kenmerk toewijzings tabel.

   > [!div class="mx-imgBorder"]
   > ![Zakelijke telefoon code ophalen](./media/sap-successfactors-inbound-provisioning/get-business-phone-code.png)

1. Noteer de **optie-id** die is gekoppeld aan de *mobiele* telefoon. Dit is de code die we gaan gebruiken met *cellPhoneType* in de kenmerk toewijzings tabel.

   > [!div class="mx-imgBorder"]
   > ![Mobiele telefoon code ophalen](./media/sap-successfactors-inbound-provisioning/get-cell-phone-code.png)

   > [!NOTE]
   > Verwijder het komma teken wanneer u de waarde kopieert. Voor beeld: als de waarde van de **optie-ID** *10.606*is, stelt u de *cellPhoneType* in azure AD in op het constante nummer *10606* (zonder het komma teken). 


## <a name="configuring-successfactors-writeback-app"></a>SuccessFactors write-app configureren

Deze sectie bevat stappen voor 

* [De inrichtings connector-app toevoegen en connectiviteit configureren voor SuccessFactors](#part-1-add-the-provisioning-connector-app-and-configure-connectivity-to-successfactors)
* [Kenmerk toewijzingen configureren](#part-2-configure-attribute-mappings)
* [Gebruikers inrichting inschakelen en starten](#enable-and-launch-user-provisioning)

### <a name="part-1-add-the-provisioning-connector-app-and-configure-connectivity-to-successfactors"></a>Deel 1: de app voor de inrichtings connector toevoegen en connectiviteit met SuccessFactors configureren

**SuccessFactors write-back configureren:**

1. Ga naar <https://portal.azure.com>

2. Selecteer in de linker navigatie balk de optie **Azure Active Directory**

3. Selecteer **bedrijfs toepassingen**en vervolgens **alle toepassingen**.

4. Selecteer **een toepassing toevoegen**en selecteer de categorie **alle** .

5. Zoek naar **SuccessFactors write**en voeg die app toe vanuit de galerie.

6. Nadat de app is toegevoegd en het scherm met details van de app wordt weer gegeven, selecteert u **inrichting** maken

7. De **inrichtings** **modus** wijzigen in **automatisch**

8. Voer de sectie **beheerders referenties** als volgt uit:

   * **Gebruikers naam beheerder** : Voer de gebruikers naam in van het gebruikers account van de SUCCESSFACTORS-API, waarbij de bedrijfs-id is toegevoegd. Het heeft de volgende indeling: **gebruikers naam \@ companyID**

   * **Beheerders wachtwoord –** Voer het wacht woord van het gebruikers account van de SuccessFactors-API in. 

   * **Tenant-URL:** Voer de naam van het SuccessFactors OData API services-eind punt in. Voer alleen de hostnaam van de server in zonder http of https. Deze waarde moet er als volgt uitzien: `api4.successfactors.com` .

   * **E-mail melding-** Voer uw e-mail adres in en schakel het selectie vakje e-mail verzenden als er een fout is opgetreden in.
    > [!NOTE]
    > De Azure AD-inrichtings service verzendt een e-mail melding als de inrichtings taak een [quarantaine](/azure/active-directory/manage-apps/application-provisioning-quarantine-status) status heeft.

   * Klik op de knop **verbinding testen** . Als de verbindings test is geslaagd, klikt u bovenaan op de knop **Opslaan** . Als dit mislukt, controleert u of de SuccessFactors-referenties en-URL geldig zijn.
    >[!div class="mx-imgBorder"]
    >![Azure-portal](./media/sap-successfactors-inbound-provisioning/sfwb-provisioning-creds.png)

   * Zodra de referenties zijn opgeslagen, wordt de standaard toewijzing weer gegeven in de sectie **toewijzingen** . Vernieuw de pagina als de kenmerk toewijzingen niet zichtbaar zijn.  

### <a name="part-2-configure-attribute-mappings"></a>Deel 2: kenmerk toewijzingen configureren

In deze sectie configureert u hoe gebruikers gegevens stromen van SuccessFactors naar Active Directory.

1. Klik op het tabblad inrichting onder **toewijzingen**op **Azure Active Directory gebruikers inrichten**.

1. In het veld **bereik van bron object** kunt u selecteren welke groepen gebruikers in azure AD moeten worden beschouwd als write-back, door een set op kenmerken gebaseerde filters te definiëren. Het standaard bereik is alle gebruikers in azure AD. 
   > [!TIP]
   > Wanneer u de inrichtings-app voor de eerste keer configureert, moet u de kenmerk toewijzingen en expressies testen en controleren om ervoor te zorgen dat het gewenste resultaat wordt weer geven. Micro soft raadt aan om de bereik filters onder het bereik van de **bron object** te gebruiken om uw toewijzingen te testen met een aantal test gebruikers van Azure AD. Wanneer u hebt gecontroleerd of de toewijzingen werken, kunt u het filter verwijderen of het bestand geleidelijk uitbreiden om meer gebruikers op te geven.

1. Het veld **acties doel object** ondersteunt alleen de bewerking **Update** .

1. In de toewijzings tabel in de sectie **kenmerk toewijzingen** kunt u de volgende Azure Active Directory kenmerken toewijzen aan SuccessFactors. De volgende tabel bevat richt lijnen voor het toewijzen van de kenmerken voor write-back. 

   | \# | Azure AD-kenmerk | SuccessFactors-kenmerk | Opmerkingen |
   |--|--|--|--|
   | 1 | employeeId | personIdExternal | Dit kenmerk is standaard de overeenkomende id. In plaats van werk nemers kunt u elk ander Azure AD-kenmerk gebruiken dat de waarde kan opslaan die gelijk is aan personIdExternal in SuccessFactors.    |
   | 2 | mail | e-mail | Bron van e-mail kenmerk toewijzen. Voor test doeleinden kunt u userPrincipalName toewijzen aan e-mail. |
   | 3 | 8448 | emailType | Deze constante waarde is de SuccessFactors ID-waarde die aan zakelijke e-mail is gekoppeld. Werk deze waarde bij zodat deze overeenkomt met uw SuccessFactors-omgeving. Zie de sectie [constante waarde ophalen voor emailType](#retrieve-constant-value-for-emailtype) voor de stappen om deze waarde in te stellen. |
   | 4 | true | emailIsPrimary | Gebruik dit kenmerk om zakelijke e-mail in te stellen als primair in SuccessFactors. Als zakelijk e-mail adres niet primair is, stelt u deze vlag in op ONWAAR. |
   | 5 | userPrincipalName | [custom01 – custom15] | Met een **nieuwe toewijzing toevoegen**kunt u optioneel userPrincipalName of een Azure AD-kenmerk schrijven naar een aangepast kenmerk dat beschikbaar is in het SuccessFactors-gebruikers object.  |
   | 6 | on-premises-samAccountName | gebruikersnaam | Met een **nieuwe toewijzing toevoegen**kunt u optioneel een on-premises sAMAccountName toewijzen aan het kenmerk SuccessFactors username. |
   | 7 | Eenmalige aanmelding | loginMethod | Als SuccessFactors-Tenant is ingesteld voor [gedeeltelijke SSO](https://apps.support.sap.com/sap/support/knowledge/en/2320766)en vervolgens nieuwe toewijzing toevoegen gebruikt, kunt u eventueel loginMethod instellen op een constante waarde van ' SSO ' of ' pwd '. |
   | 8 | telephoneNumber | businessPhoneNumber | Gebruik deze toewijzing om *telephoneNumber* uit te stromen van Azure AD naar SuccessFactors Business/Work-telefoon nummer. |
   | 9 | 10605 | businessPhoneType | Deze constante waarde is de SuccessFactors ID-waarde gekoppeld aan de zakelijke telefoon. Werk deze waarde bij zodat deze overeenkomt met uw SuccessFactors-omgeving. Zie de sectie [constante waarde ophalen voor phoneType](#retrieve-constant-value-for-phonetype) voor de stappen om deze waarde in te stellen. |
   | 10 | true | businessPhoneIsPrimary | Gebruik dit kenmerk om de primaire vlag voor het zakelijke telefoon nummer in te stellen. Geldige waarden zijn True of false. |
   | 11 | mobiel | cellPhoneNumber | Gebruik deze toewijzing om *telephoneNumber* uit te stromen van Azure AD naar SuccessFactors Business/Work-telefoon nummer. |
   | 12 | 10606 | cellPhoneType | Deze constante waarde is de SuccessFactors ID-waarde die is gekoppeld aan de mobiele telefoon. Werk deze waarde bij zodat deze overeenkomt met uw SuccessFactors-omgeving. Zie de sectie [constante waarde ophalen voor phoneType](#retrieve-constant-value-for-phonetype) voor de stappen om deze waarde in te stellen. |
   | 13 | onjuist | cellPhoneIsPrimary | Gebruik dit kenmerk om de primaire vlag voor het mobiele telefoon nummer in te stellen. Geldige waarden zijn True of false. |
 
1. Valideer en controleer uw kenmerk toewijzingen. 
 
    >[!div class="mx-imgBorder"]
    >![Toewijzing van write-kenmerk](./media/sap-successfactors-inbound-provisioning/writeback-attribute-mapping.png)

1. Klik op **Opslaan** om de toewijzingen op te slaan. Vervolgens worden de API-expressies voor het JSON-pad bijgewerkt om de phoneType-codes in uw SuccessFactors-exemplaar te gebruiken. 
1. Selecteer **Geavanceerde opties weergeven**. 

    >[!div class="mx-imgBorder"]
    >![Geavanceerde opties weergeven](./media/sap-successfactors-inbound-provisioning/show-advanced-options.png)

1. Klik op **kenmerk lijst bewerken voor SuccessFactors**. 

   > [!NOTE] 
   > Als de optie **kenmerk lijst bewerken voor SuccessFactors** niet wordt weer gegeven in de Azure Portal, gebruikt u de URL *https://portal.azure.com/?Microsoft_AAD_IAM_forceSchemaEditorEnabled=true* voor toegang tot de pagina. 

1. In de **expressie kolom API** in deze weer gave worden de JSON Path-expressies weer gegeven die worden gebruikt door de connector. 
1. Werk de JSON Path-expressies voor zakelijke telefoon en mobiele telefoon bij om de ID-waarde (*businessPhoneType* en *cellPhoneType*) te gebruiken die overeenkomt met uw omgeving. 

    >[!div class="mx-imgBorder"]
    >![Wijziging in het JSON-pad voor het telefoon nummer](./media/sap-successfactors-inbound-provisioning/phone-json-path-change.png)

1. Klik op **Opslaan** om de toewijzingen op te slaan.

## <a name="enable-and-launch-user-provisioning"></a>Gebruikers inrichting inschakelen en starten

Zodra de configuratie van de SuccessFactors-inrichting is voltooid, kunt u de inrichtings service inschakelen in de Azure Portal.

> [!TIP]
> Wanneer u de inrichtings service inschakelt, worden er standaard inrichtings bewerkingen gestart voor alle gebruikers binnen het bereik. Als er fouten zijn opgetreden in de toewijzings-of gegevens problemen, kan de inrichtings taak mislukken en gaat u naar de status van de quarantaine. Om dit te voor komen best practice, raden we u aan om het bereik filter voor **bron objecten** te configureren en uw kenmerk toewijzingen te testen met enkele test gebruikers voordat u de volledige synchronisatie voor alle gebruikers start. Wanneer u hebt gecontroleerd of de toewijzingen werken en u de gewenste resultaten krijgt, kunt u het filter verwijderen of het bestand geleidelijk uitbreiden om meer gebruikers op te geven.

1. Stel op het tabblad **inrichten** de **inrichtings status** in **op aan**.

1. Selecteer **bereik**. U kunt een van de volgende opties selecteren: 
   * **Alle gebruikers en groepen synchroniseren**: Selecteer deze optie als u van plan bent toegewezen kenmerken van alle gebruikers te schrijven van Azure AD naar SuccessFactors, afhankelijk van de bereik regels die zijn gedefinieerd onder **toewijzing**van  ->  **bron object bereik**. 
   * **Alleen toegewezen gebruikers en groepen synchroniseren**: Selecteer deze optie als u van plan bent toegewezen kenmerken te schrijven van alleen gebruikers die u hebt toegewezen aan **deze toepassing in de**  ->  **Manage**  ->  menu optie**gebruikers en groepen** beheren. Deze gebruikers zijn ook onderhevig aan de bereik regels die zijn gedefinieerd onder **toewijzing**van  ->  **bron object bereik**.

   > [!div class="mx-imgBorder"]
   > ![Terugschrijf bereik selecteren](./media/sap-successfactors-inbound-provisioning/select-writeback-scope.png)

   > [!NOTE]
   > De SuccessFactors write Provisioning-app biedt geen ondersteuning voor "groeps toewijzing". Alleen ' gebruikers toewijzing ' wordt ondersteund. 

1. Klik op **Opslaan**.

1. Met deze bewerking wordt de eerste synchronisatie gestart. Dit kan een variabel aantal uren duren, afhankelijk van het aantal gebruikers in de Azure AD-Tenant en het bereik dat is gedefinieerd voor de bewerking. U kunt de voortgangs balk controleren om de voortgang van de synchronisatie cyclus bij te houden. 

1. Controleer op elk gewenst moment het tabblad **inrichtings logboeken** in de Azure Portal om te zien welke acties de inrichtings service heeft uitgevoerd. In de inrichtings logboeken worden alle afzonderlijke synchronisatie gebeurtenissen vermeld die worden uitgevoerd door de inrichtings service. 

1. Zodra de initiële synchronisatie is voltooid, wordt een overzichts rapport van de controle op het tabblad **inrichten** geschreven, zoals hieronder wordt weer gegeven.

   > [!div class="mx-imgBorder"]
   > ![Voortgangs balk inrichten](./media/sap-successfactors-inbound-provisioning/prov-progress-bar-stats.png)

## <a name="supported-scenarios-known-issues-and-limitations"></a>Ondersteunde scenario's, bekende problemen en beperkingen

Raadpleeg de [sectie write-scenario's](../app-provisioning/sap-successfactors-integration-reference.md#writeback-scenarios) in de naslag gids voor SAP SuccessFactors-integratie. 

## <a name="next-steps"></a>Volgende stappen

* [Uitgebreide informatie over Azure AD en SAP SuccessFactors-integratie](../app-provisioning/sap-successfactors-integration-reference.md)
* [Meer informatie over het controleren van logboeken en het ophalen van rapporten over de inrichtingsactiviteit](../app-provisioning/check-status-user-account-provisioning.md)
* [Meer informatie over het configureren van eenmalige aanmelding tussen SuccessFactors en Azure Active Directory](successfactors-tutorial.md)
* [Meer informatie over het integreren van andere SaaS-toepassingen met Azure Active Directory](tutorial-list.md)
* [Meer informatie over het exporteren en importeren van uw inrichtings configuraties](../app-provisioning/export-import-provisioning-configuration.md)

