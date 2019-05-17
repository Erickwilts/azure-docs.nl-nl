---
title: Eigenschappen van een B2B-Gast gebruiker - Azure Active Directory | Microsoft Docs
description: Azure Active Directory B2B-Gast gebruikerseigenschappen en statussen voor en na verzilvering van uitnodiging
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.custom: it-pro, seo-update-azuread-jan, seoapril2019
ms.collection: M365-identity-device-management
ms.openlocfilehash: b1416dacd65024457e713547223f5c35290b3d15
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65768154"
---
# <a name="properties-of-an-azure-active-directory-b2b-collaboration-user"></a>Eigenschappen van de gebruiker van een Azure Active Directory B2B-samenwerking

Dit artikel beschrijft de eigenschappen en de status van het gebruikersobject B2B-gasten in Azure Active Directory (Azure AD) voor en na een uitnodiging inwisselen. Een Azure AD-gebruiker voor samenwerking business-to-business (B2B) is een gebruiker met UserType = Gast. Deze gastgebruiker meestal afkomstig is van een partnerorganisatie en beperkte bevoegdheden in de map van de uitnodigende, heeft standaard.

Afhankelijk van de behoeften van de uitnodigende organisatie, kan de gebruiker van een Azure AD B2B-samenwerking in een van de volgende account statussen zijn:

- Status 1: Homed in een extern exemplaar van Azure AD en weergegeven als een gastgebruiker in de organisatie. In dit geval zich de B2B-gebruiker aanmeldt met een Azure AD-account die deel uitmaakt van de uitgenodigde tenant. Als de andere organisatie geen Azure AD gebruikt, wordt de gastgebruiker in Azure AD nog steeds gemaakt. De vereisten zijn dat ze hun uitnodiging inwisselen en Azure AD verifieert u het e-mailadres. Deze benadering wordt ook een just-in-time (JIT) tenants of een 'viraal' tenants genoemd.

- Status van 2: In een door Microsoft of een andere account homed en weergegeven als een gastgebruiker in de organisatie van de host. In dit geval de gastgebruiker zich aanmeldt met een Microsoft-account of een sociaal account (google.com of vergelijkbaar). De identiteit van de uitgenodigde gebruiker is gemaakt als een Microsoft-account in de adreslijst van de uitnodigende organisatie tijdens de inschrijving van de aanbieding.

- Status van 3: Homed in van de organisatie van de host on-premises Active Directory en gesynchroniseerd met de host-organisatie in Azure AD. U kunt Azure AD Connect gebruiken voor het synchroniseren van de partneraccounts naar de cloud als Azure AD B2B-gebruikers met UserType = Gast. Zie [verlenen partner lokaal beheerde accounts toegang tot cloudbronnen](hybrid-on-premises-to-cloud.md).

- Status van 4: Homed in van de organisatie van de host Azure AD met UserType = Gast en referenties die de organisatie van de host wordt beheerd.

  ![Diagram met de status van de vier gebruiker](media/user-properties/redemption-diagram.png)


Nu gaan we kijken hoe de gebruiker van een Azure AD B2B-samenwerking eruit in Azure AD.

### <a name="before-invitation-redemption"></a>Voordat u verzilvering van uitnodiging

Status 1 en 2 voor status-accounts zijn het resultaat van het uitnodigen van gastgebruikers om samen te werken met behulp van de referenties van de gebruikers zelf Gast. Wanneer de uitnodiging naar de gastgebruiker in eerste instantie verzonden is, wordt een account gemaakt in uw directory. Dit account heeft geen referenties die zijn gekoppeld aan deze omdat verificatie wordt uitgevoerd door de gastgebruiker id-provider. De **bron** eigenschap voor het gastgebruikersaccount in de map is ingesteld op **Invited gebruiker**. 

![Schermafbeelding van de eigenschappen van de gebruiker voordat u inschrijving van de aanbieding](media/user-properties/before-redemption.png)

### <a name="after-invitation-redemption"></a>Na verzilvering van uitnodiging

Nadat de gastgebruiker de uitnodiging accepteert de **bron** eigenschap is bijgewerkt op basis van de gastgebruiker id-provider.

Voor gastgebruikers ook kunnen in de status 1, de **bron** is **externe Azure Active Directory**.

![Status 1 gastgebruiker na inwisseling van de aanbieding](media/user-properties/after-redemption-state1.png)

Gastgebruikers in 2, status, de **bron** is **Microsoft-Account**.

![De gastgebruiker status 2 na inwisseling van de aanbieding](media/user-properties/after-redemption-state2.png)

Voor gastgebruikers ook kunnen in de status 3 en 4, status, de **bron** eigenschap is ingesteld op **Azure Active Directory** of **Windows Server Active Directory**, zoals beschreven in de volgende sectie.

## <a name="key-properties-of-the-azure-ad-b2b-collaboration-user"></a>Eigenschappen van de sleutel van de gebruiker van Azure AD B2B-samenwerking
### <a name="usertype"></a>UserType
Deze eigenschap geeft aan dat de relatie van de gebruiker op de host-tenants. Deze eigenschap kan twee waarden hebben:
- Lid: Deze waarde geeft aan dat een werknemer van de organisatie van de host en een gebruiker in de organisatie salarisadministratie. Deze gebruiker wordt bijvoorbeeld verwacht kunnen toegang hebben tot interne websites. Deze gebruiker wordt niet beschouwd als een externe samenwerker.

- Gast: Deze waarde geeft aan dat een gebruiker die wordt niet beschouwd als intern gebruik binnen het bedrijf, zoals een externe samenwerker, partner of klant. Een gebruiker is niet ontvangen van een CEO interne memo of ontvangen van de voordelen van het bedrijf, bijvoorbeeld verwacht.

  > [!NOTE]
  > Het UserType is niet gekoppeld aan hoe de gebruiker zich aanmeldt, de directory-rol van de gebruiker, enzovoort. Deze eigenschap geeft aan dat de relatie van de gebruiker voor de organisatie van de host gewoon en kan de organisatie om af te dwingen van beleidsregels die afhankelijk van deze eigenschap zijn.

### <a name="source"></a>Source
Deze eigenschap geeft aan hoe de gebruiker zich aanmeldt.

- Uitgenodigde gebruiker: Deze gebruiker is uitgenodigd maar een uitnodiging nog niet is ingewisseld.

- Externe Active Directory: Deze gebruiker is toegewezen in een externe organisatie en verifieert met behulp van een Azure AD-account die deel uitmaakt van de andere organisatie. Dit type aanmelding komt overeen met de status van 1.

- Microsoft-account: Deze gebruiker is toegewezen in een Microsoft-account en verifieert met behulp van een Microsoft-account. Dit type aanmelding komt overeen met de status 2.

- Windows Server Active Directory: Deze gebruiker is aangemeld vanuit on-premises Active Directory die deel uitmaakt van deze organisatie. Dit type aanmelding komt overeen met de status 3.

- Azure Active Directory: Deze gebruiker wordt geverifieerd met behulp van een Azure AD-account die deel uitmaakt van deze organisatie. Dit type aanmelding komt overeen met status 4.
  > [!NOTE]
  > Bron- en UserType zijn onafhankelijke eigenschappen. Een waarde van de bron betekent niet dat voor een bepaalde waarde voor UserType.

## <a name="can-azure-ad-b2b-users-be-added-as-members-instead-of-guests"></a>Kunnen Azure AD B2B-gebruikers worden toegevoegd als leden in plaats van gasten?
Een Azure AD B2B-gebruiker en de gastgebruiker zijn meestal gelijk. Daarom wordt de gebruiker van een Azure AD B2B-samenwerking toegevoegd als een gebruiker met UserType = Gast standaard. In sommige gevallen is de partnerorganisatie echter lid is van een grotere organisatie waarvan de organisatie van de host ook deel uitmaakt. Als dit het geval is, kan de organisatie van de host wilt voor het behandelen van gebruikers in de partnerorganisatie als leden in plaats van gasten. Gebruik de Azure AD B2B uitnodiging Manager API's toe te voegen of een gebruiker van de partnerorganisatie aan de organisatie van de host als een lid uitnodigen.

## <a name="filter-for-guest-users-in-the-directory"></a>Filter voor gastgebruikers ook kunnen in de map

![Schermopname van het filter voor gastgebruikers](media/user-properties/filter-guest-users.png)

## <a name="convert-usertype"></a>UserType converteren
Het is mogelijk converteren naar UserType lid Gast en vice versa met behulp van PowerShell. Echter, vertegenwoordigt het UserType-eigenschap van de gebruiker-relatie voor de organisatie. Daarom moet u deze eigenschap alleen als de relatie van de gebruiker naar de wijzigingen in de organisatie. Als de relatie van de gebruiker wordt gewijzigd, moet de UPN (User Principal Name) dan wijzigen? Moet de gebruiker nog steeds toegang tot dezelfde resources? Een postvak worden toegewezen? Wordt niet aanbevolen het UserType wijzigen met behulp van PowerShell als een atomische activiteit. Ook als deze eigenschap onveranderbare wordt met behulp van PowerShell, wordt niet aanbevolen voor het maken van een afhankelijkheid op deze waarde.

## <a name="remove-guest-user-limitations"></a>Beperkingen van Gast-gebruiker verwijderen
Mogelijk zijn er gevallen waar u wilt uw gastgebruikers hogere rechten te verlenen. U kunt een gastgebruiker toevoegen aan een rol en zelfs de standaardbeperkingen voor de Gast-gebruiker verwijderen in de map waarin een gebruiker dezelfde bevoegdheden als leden.

Het is mogelijk de standaardbeperkingen uitschakelen zodat een gastgebruiker in de directory van het bedrijf dezelfde machtigingen als een gebruiker lid heeft.

![Schermopname van de externe gebruikers optie in de gebruikersinstellingen](media/user-properties/remove-guest-limitations.png)

## <a name="can-i-make-guest-users-visible-in-the-exchange-global-address-list"></a>Kan ik gastgebruikers zichtbaar maken in de algemene Exchange-adreslijst?
Ja. Standaard Gast objecten zijn niet zichtbaar in de globale adreslijst van uw organisatie, maar u kunt Azure Active Directory PowerShell gebruiken om ze zichtbaar. Zie voor meer informatie, **kan ik objecten Gast zichtbaar maken in de globale adreslijst?** in [beheren van toegang voor gasten in Office 365-groepen](https://docs.microsoft.com/office365/admin/create-groups/manage-guest-access-in-groups?redirectSourcePath=%252fen-us%252farticle%252fmanage-guest-access-in-office-365-groups-9de497a9-2f5c-43d6-ae18-767f2e6fe6e0&view=o365-worldwide#faq). 

## <a name="next-steps"></a>Volgende stappen

* [Wat is Azure AD B2B-samenwerking?](what-is-b2b.md)
* [B2B-samenwerking gebruikerstokens](user-token.md)
* [B2B-samenwerking-gebruikersclaims toewijzen](claims-mapping.md)
