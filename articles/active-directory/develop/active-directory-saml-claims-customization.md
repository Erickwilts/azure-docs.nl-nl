---
title: SAML-token claims aanpassen voor zakelijke apps in azure AD
titleSuffix: Microsoft identity platform
description: Meer informatie over het aanpassen van de claims die zijn uitgegeven in het SAML-token voor zakelijke toepassingen in azure AD.
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
ms.assetid: f1daad62-ac8a-44cd-ac76-e97455e47803
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/22/2019
ms.author: ryanwi
ms.reviewer: luleon, paulgarn, jeedes
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4307c9036db45145a7c0e95cb5e55a667c6851eb
ms.sourcegitcommit: 5acd8f33a5adce3f5ded20dff2a7a48a07be8672
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2019
ms.locfileid: "72893401"
---
# <a name="how-to-customize-claims-issued-in-the-saml-token-for-enterprise-applications"></a>Procedure: claims aanpassen die zijn uitgegeven in het SAML-token voor zakelijke toepassingen

Vandaag, Azure Active Directory (Azure AD), ondersteunt eenmalige aanmelding (SSO) met de meeste zakelijke toepassingen, waaronder toepassingen die vooraf zijn geïntegreerd in de Azure AD-App-galerie en aangepaste toepassingen. Wanneer een gebruiker met behulp van het SAML 2,0-protocol via Azure AD met een toepassing wordt geverifieerd, verzendt Azure AD een token naar de toepassing (via een HTTP-bericht). Vervolgens valideert en gebruikt de toepassing het token voor het registreren van de gebruiker in plaats van een gebruikers naam en wacht woord op te vragen. Deze SAML-tokens bevatten stukjes informatie over de gebruiker die *claims*wordt genoemd.

Een *claim* is informatie die een id-provider staat voor een gebruiker binnen het token dat wordt uitgegeven voor die gebruiker. In het [SAML-token](https://en.wikipedia.org/wiki/SAML_2.0)bevinden deze gegevens zich doorgaans in de instructie SAML Attribute. De unieke ID van de gebruiker wordt gewoonlijk weer gegeven in het SAML-onderwerp, ook wel naam-id genoemd.

Azure AD geeft standaard een SAML-token aan uw toepassing met een `NameIdentifier`-claim met een waarde van de gebruikers naam van de gebruiker (ook wel bekend als de user principal name) in azure AD, waarmee de gebruiker uniek kan worden geïdentificeerd. Het SAML-token bevat ook aanvullende claims met het e-mail adres, de voor naam en de achternaam van de gebruiker.

Als u de claims die in het SAML-token aan de toepassing zijn uitgegeven, wilt weer geven of bewerken, opent u de toepassing in Azure Portal. Open vervolgens de sectie **gebruikers kenmerken & claims** .

![Open de sectie gebruikers kenmerken & claims in de Azure Portal](./media/active-directory-saml-claims-customization/sso-saml-user-attributes-claims.png)

Er zijn twee mogelijke redenen waarom u de claims die in het SAML-token zijn uitgegeven, mogelijk moet bewerken:

* De toepassing vereist dat de claim `NameIdentifier` of NameID iets anders is dan de gebruikers naam (of user principal name) die is opgeslagen in azure AD.
* De toepassing is geschreven om een andere set claim-Uri's of claim waarden te vereisen.

## <a name="editing-nameid"></a>NameID bewerken

De NameID (naam-id-waarde) bewerken:

1. Open de pagina **naam identificatie waarde** .
1. Selecteer het kenmerk of de trans formatie die u wilt Toep assen op het kenmerk. U kunt desgewenst de indeling opgeven waarvan de NameID-claim moet beschikken.

   ![De NameID-waarde (naam-id) bewerken](./media/active-directory-saml-claims-customization/saml-sso-manage-user-claims.png)

### <a name="nameid-format"></a>NameID-indeling

Als de SAML-aanvraag het element NameIDPolicy met een specifieke indeling bevat, zal Azure AD voldoen aan de indeling in de aanvraag.

Als de SAML-aanvraag geen element voor NameIDPolicy bevat, wordt het NameID door Azure AD uitgegeven met de indeling die u opgeeft. Als er geen indeling is opgegeven, gebruikt Azure AD de standaard bron indeling die is gekoppeld aan de geselecteerde claim bron.

U kunt een van de volgende opties selecteren in de vervolg keuzelijst **Kies naam-id-indeling** .

| NameID-indeling | Beschrijving |
|---------------|-------------|
| **Standaard** | Azure AD gebruikt de standaard indeling voor de bron. |
| **Behouden** | Azure AD gebruikt persistent als de NameID-indeling. |
| **EmailAddress** | Azure AD maakt gebruik van EmailAddress als NameID-indeling. |
| **Opgegeven** | Azure AD gebruikt niet opgegeven als NameID-indeling. |

Tijdelijke NameID wordt ook ondersteund, maar is niet beschikbaar in de vervolg keuzelijst en kan niet worden geconfigureerd op de kant van Azure. Zie het [SAML-protocol voor eenmalige aanmelding](single-sign-on-saml-protocol.md)voor meer informatie over het NameIDPolicy-kenmerk.

### <a name="attributes"></a>Kenmerken

Selecteer de gewenste bron voor de claim `NameIdentifier` (of NameID). U kunt kiezen uit de volgende opties.

| Naam | Beschrijving |
|------|-------------|
| E-mail | E-mail adres van de gebruiker |
| userprincipalName | UPN (User Principal Name) van de gebruiker |
| onpremisessamaccount | SAM-account naam die is gesynchroniseerd vanuit on-premises Azure AD |
| id | objectid van de gebruiker in azure AD |
| employeeid | Werk nemer-ID van de gebruiker |
| Uitbreidingen van de directory | Directory-extensies [die zijn gesynchroniseerd vanuit on-premises Active Directory met behulp van Azure AD Connect-synchronisatie](../hybrid/how-to-connect-sync-feature-directory-extensions.md) |
| Extensie kenmerken 1-15 | Kenmerken van on-premises extensies die worden gebruikt om het Azure AD-schema uit te breiden |

Zie voor meer informatie [tabel 3: geldige ID-waarden per bron](active-directory-claims-mapping.md#table-3-valid-id-values-per-source).

U kunt ook een constante (statische) waarde toewijzen aan claims die u in azure AD definieert. Volg de onderstaande stappen om een constante waarde toe te wijzen:

1. Klik in het [Azure Portal](https://portal.azure.com/), in het gedeelte **gebruikers kenmerken & claims** , op het pictogram **bewerken** om de claims te bewerken.

1. Klik op de vereiste claim die u wilt wijzigen.

1. Voer de constante waarde zonder aanhalings tekens in het **bron kenmerk** in volgens uw organisatie en klik op **Opslaan**.

    ![Open de sectie gebruikers kenmerken & claims in de Azure Portal](./media/active-directory-saml-claims-customization/organization-attribute.png)

1. De constante waarde wordt hieronder weer gegeven.

    ![Open de sectie gebruikers kenmerken & claims in de Azure Portal](./media/active-directory-saml-claims-customization/edit-attributes-claims.png)

### <a name="special-claims---transformations"></a>Speciale claim transformaties

U kunt ook de functies voor het transformeren van claims gebruiken.

| Functie | Beschrijving |
|----------|-------------|
| **ExtractMailPrefix()** | Hiermee verwijdert u het domein achtervoegsel uit het e-mail adres of de user principal name. Hiermee wordt alleen het eerste deel van de gebruikers naam geëxtraheerd dat wordt door gegeven (bijvoorbeeld ' joe_smith ' in plaats van joe_smith@contoso.com). |
| **Samen voegen ()** | Voegt een kenmerk toe aan een geverifieerd domein. Als de geselecteerde gebruikers-id een domein heeft, wordt de gebruikers naam geëxtraheerd om het geselecteerde geverifieerde domein toe te voegen. Als u bijvoorbeeld het e-mail adres (joe_smith@contoso.com) selecteert als waarde voor de gebruikers-id en contoso.onmicrosoft.com selecteert als het geverifieerde domein, resulteert dit in joe_smith@contoso.onmicrosoft.com. |
| **ToLower()** | Hiermee worden de tekens van het geselecteerde kenmerk geconverteerd naar kleine letters. |
| **ToUpper()** | Hiermee worden de tekens van het geselecteerde kenmerk geconverteerd naar hoofd letters. |

## <a name="adding-application-specific-claims"></a>Toepassingsspecifieke claims toevoegen

Toepassingsspecifieke claims toevoegen:

1. Selecteer in **gebruikers kenmerken & claims**de optie **nieuwe claim toevoegen** om de pagina **gebruikers claims beheren** te openen.
1. Voer de **naam** van de claims in. De waarde hoeft niet strikt een URI-patroon te volgen, conform de SAML-specificatie. Als u een URI-patroon nodig hebt, kunt u dat in het veld **naam ruimte** plaatsen.
1. Selecteer de **bron** waar de claim de waarde gaat ophalen. U kunt een gebruikers kenmerk selecteren in de vervolg keuzelijst bron kenmerk of een trans formatie Toep assen op het kenmerk gebruiker voordat u het als een claim verzendt.

### <a name="claim-transformations"></a>Claim transformaties

Een trans formatie Toep assen op een gebruikers kenmerk:

1. Selecteer in **claim beheren**de optie *trans formatie* als claim bron om de pagina **trans formatie beheren** te openen.
2. Selecteer de functie in de vervolg keuzelijst transformeren. Afhankelijk van de functie die u hebt geselecteerd, moet u para meters en een constante waarde opgeven om in de trans formatie te evalueren. Raadpleeg de onderstaande tabel voor meer informatie over de beschik bare functies.
3. Als u meerdere trans formatie wilt Toep assen, klikt u op **trans formatie toevoegen**. U kunt Maxi maal twee trans formatie op een claim Toep assen. U kunt bijvoorbeeld eerst het e-mail voorvoegsel van de `user.mail`uitpakken. Vervolgens maakt u de reeks hoofd letters.

   ![De NameID-waarde (naam-id) bewerken](./media/active-directory-saml-claims-customization/sso-saml-multiple-claims-transformation.png)

U kunt de volgende functies gebruiken om claims te transformeren.

| Functie | Beschrijving |
|----------|-------------|
| **ExtractMailPrefix()** | Hiermee verwijdert u het domein achtervoegsel uit het e-mail adres of de user principal name. Hiermee wordt alleen het eerste deel van de gebruikers naam geëxtraheerd dat wordt door gegeven (bijvoorbeeld ' joe_smith ' in plaats van joe_smith@contoso.com). |
| **Samen voegen ()** | Hiermee maakt u een nieuwe waarde door twee kenmerken samen te voegen. U kunt desgewenst een scheidings teken tussen de twee kenmerken gebruiken. Voor NameID-claim transformatie is de koppeling beperkt tot een geverifieerd domein. Als de geselecteerde gebruikers-id een domein heeft, wordt de gebruikers naam geëxtraheerd om het geselecteerde geverifieerde domein toe te voegen. Als u bijvoorbeeld het e-mail adres (joe_smith@contoso.com) selecteert als waarde voor de gebruikers-id en contoso.onmicrosoft.com selecteert als het geverifieerde domein, resulteert dit in joe_smith@contoso.onmicrosoft.com. |
| **ToLower()** | Hiermee worden de tekens van het geselecteerde kenmerk geconverteerd naar kleine letters. |
| **ToUpper()** | Hiermee worden de tekens van het geselecteerde kenmerk geconverteerd naar hoofd letters. |
| **Contains ()** | Voert een kenmerk of constante uit als de invoer overeenkomt met de opgegeven waarde. Als dat niet het geval is, kunt u een andere uitvoer opgeven.<br/>Als u bijvoorbeeld een claim wilt verzenden waarbij de waarde het e-mail adres van de gebruiker is als deze het domein ' @contoso.com ' bevat, anders wilt u de user principal name uitvoeren. Hiervoor moet u de volgende waarden configureren:<br/>*Para meter 1 (invoer)* : gebruiker. e-mail adres<br/>*Waarde*: ' @contoso.com '<br/>Para meter 2 (uitvoer): gebruiker. e-mail adres<br/>Para meter 3 (uitvoer als er geen overeenkomst is): User. userPrincipalName |
| **EndWith()** | Voert een kenmerk of constante uit als de invoer eindigt met de opgegeven waarde. Als dat niet het geval is, kunt u een andere uitvoer opgeven.<br/>Als u bijvoorbeeld een claim wilt verzenden waarvan de waarde de werk nemer-ID van de gebruiker is als de werk nemer-ID eindigt op ' 000 ', moet u anders een extensie kenmerk uitvoeren. Hiervoor moet u de volgende waarden configureren:<br/>*Para meter 1 (invoer)* : User. EmployeeID<br/>*Waarde*: 000<br/>Para meter 2 (uitvoer): User. EmployeeID<br/>Para meter 3 (uitvoer als er geen overeenkomst is): User. extensionAttribute1 |
| **StartWith()** | Voert een kenmerk of constante uit als de invoer begint met de opgegeven waarde. Als dat niet het geval is, kunt u een andere uitvoer opgeven.<br/>Als u bijvoorbeeld een claim wilt verzenden waarvan de waarde de werk nemers-ID van de gebruiker is als het land/de regio begint met ' VS ', moet u anders een extensie kenmerk uitvoeren. Hiervoor moet u de volgende waarden configureren:<br/>*Para meter 1 (invoer)* : gebruiker. land<br/>*Waarde*: "US"<br/>Para meter 2 (uitvoer): User. EmployeeID<br/>Para meter 3 (uitvoer als er geen overeenkomst is): User. extensionAttribute1 |
| **Extra heren ()-na overeenkomende** | Retourneert de subtekenreeks nadat deze overeenkomt met de opgegeven waarde.<br/>Als de waarde van de invoer bijvoorbeeld "Finance_BSimon" is, is de overeenkomende waarde "Finance_", en de uitvoer van de claim is "BSimon". |
| **Extra heren ()-vóór overeenkomst** | Retourneert de subtekenreeks totdat deze overeenkomt met de opgegeven waarde.<br/>Als de waarde van de invoer bijvoorbeeld "BSimon_US" is, is de overeenkomende waarde "_US", en de uitvoer van de claim is "BSimon". |
| **Uitpakken ()-tussen overeenkomende** | Retourneert de subtekenreeks totdat deze overeenkomt met de opgegeven waarde.<br/>Als de waarde van de invoer bijvoorbeeld ' Finance_BSimon_US ' is, is de eerste overeenkomende waarde ' Finance_ ', de tweede overeenkomende waarde ' _US ', en de uitvoer van de claim is ' BSimon '. |
| **ExtractAlpha ()-voor voegsel** | Retourneert het voor voegsel alfabetisch deel van de teken reeks.<br/>Als de waarde van de invoer bijvoorbeeld ' BSimon_123 ' is, retourneert deze ' BSimon '. |
| **ExtractAlpha ()-achtervoegsel** | Retourneert het achtervoegsel alfabetisch deel van de teken reeks.<br/>Als de waarde van de invoer bijvoorbeeld ' 123_Simon ' is, wordt "Simon" geretourneerd. |
| **ExtractNumeric ()-voor voegsel** | Retourneert het numerieke gedeelte van de teken reeks.<br/>Als de waarde van de invoer bijvoorbeeld ' 123_BSimon ' is, retourneert deze ' 123 '. |
| **ExtractNumeric ()-achtervoegsel** | Retourneert het numerieke deel van het achtervoegsel van de teken reeks.<br/>Als de waarde van de invoer bijvoorbeeld ' BSimon_123 ' is, retourneert deze ' 123 '. |
| **IfEmpty()** | Voert een kenmerk of constante uit als de invoer null of leeg is.<br/>Als u bijvoorbeeld een kenmerk wilt uitvoeren dat in een extensionAttribute is opgeslagen als de werk nemer-ID voor een bepaalde gebruiker leeg is. Hiervoor moet u de volgende waarden configureren:<br/>Para meter 1 (invoer): User. EmployeeID<br/>Para meter 2 (uitvoer): User. extensionAttribute1<br/>Para meter 3 (uitvoer als er geen overeenkomst is): User. EmployeeID |
| **IfNotEmpty()** | Voert een kenmerk of constante uit als de invoer niet null of leeg is.<br/>Als u bijvoorbeeld een kenmerk wilt uitvoeren dat in een extensionAttribute is opgeslagen als de werk nemer-ID voor een bepaalde gebruiker niet leeg is. Hiervoor moet u de volgende waarden configureren:<br/>Para meter 1 (invoer): User. EmployeeID<br/>Para meter 2 (uitvoer): User. extensionAttribute1 |

Als u aanvullende trans formaties nodig hebt, verzendt u uw idee in het [Feedback forum in azure AD](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=160599) in de categorie *SaaS-toepassing* .

## <a name="emitting-claims-based-on-conditions"></a>Claims verzenden op basis van voor waarden

U kunt de bron van een claim opgeven op basis van het gebruikers type en de groep waartoe de gebruiker behoort. 

Het gebruikers type kan zijn:
- **Alle**: alle gebruikers hebben toegang tot de toepassing.
- **Leden**: native lid van de Tenant
- **Alle gasten**: de gebruiker wordt overgezet van een externe organisatie met of zonder Azure AD.
- **Aad-gasten**: een gast gebruiker maakt deel uit van een andere organisatie die gebruikmaakt van Azure AD.
- **Externe gasten**: gast gebruikers behoren tot een externe organisatie die geen Azure AD heeft.


Een scenario waarbij dit nuttig is wanneer de bron van een claim afwijkt van een gast en een werk nemer die toegang heeft tot een toepassing. U kunt opgeven dat als de gebruiker een werk nemer is, het NameID-bericht afkomstig is van de gebruiker. e-mail, maar als de gebruiker een gast is, wordt het NameID gebrond vanuit User. extensionAttribute1.

Een claim voorwaarde toevoegen:

1. Vouw in **claim beheren**de claim voorwaarden uit.
2. Selecteer het gebruikers type.
3. Selecteer de groep (en) waarvan de gebruiker deel moet uitmaken. U kunt Maxi maal 10 unieke groepen selecteren voor alle claims voor een bepaalde toepassing. 
4. Selecteer de **bron** waar de claim de waarde gaat ophalen. U kunt een gebruikers kenmerk selecteren in de vervolg keuzelijst bron kenmerk of een trans formatie Toep assen op het kenmerk gebruiker voordat u het als een claim verzendt.

De volg orde waarin u de voor waarden toevoegt, is belang rijk. Azure AD evalueert de voor waarden van boven naar beneden om te beslissen welke waarde moet worden opgegeven in de claim. 

Zo is Brita Simon een gast gebruiker in de contoso-Tenant. Ze maakt deel uit van een andere organisatie die ook gebruikmaakt van Azure AD. Op basis van de onderstaande configuratie voor de fabrikam-toepassing, wanneer Brita zich probeert aan te melden bij Fabrikam, worden de voor waarden als volgt geëvalueerd door Azure AD.

Eerst controleert Azure AD of het gebruikers type van Brita `All guests`is. Aangezien dit het geval is, wijst Azure AD de bron voor de claim toe aan `user.extensionattribute1`. Ten tweede verifieert Azure AD of het gebruikers type van Brita is `AAD guests`, omdat dit ook zo is, dan wijst Azure AD de bron voor de claim toe aan `user.mail`. Ten slotte wordt de claim verzonden met de waarde `user.email` voor Brita.

![Voorwaardelijke configuratie van claims](./media/active-directory-saml-claims-customization/sso-saml-user-conditional-claims.png)

## <a name="next-steps"></a>Volgende stappen

* [Toepassings beheer in azure AD](../manage-apps/what-is-application-management.md)
* [Eenmalige aanmelding configureren voor toepassingen die zich niet in de Azure AD-toepassings galerie bevinden](../manage-apps/configure-federated-single-sign-on-non-gallery-applications.md)
* [Problemen met eenmalige aanmelding op basis van SAML oplossen](howto-v1-debug-saml-sso-issues.md)
