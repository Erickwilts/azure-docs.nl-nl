---
title: Integreer uw aanbieding voor micro soft Commercial Marketplace met Azure Active Directory
description: Gebruik Azure Active Directory om uw Microsoft AppSource en Azure Marketplace-aanbiedingen te verifiëren.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
author: keferna
ms.author: keferna
ms.date: 07/24/2020
ms.openlocfilehash: a6e304e5ffeab8f0a44cbdfe1566465f2b9bf34a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "88607409"
---
# <a name="integrate-your-commercial-marketplace-listing-with-azure-active-directory"></a>Integreer uw commerciële Marketplace-vermelding met Azure Active Directory

 In dit artikel worden de vereisten beschreven voor het integreren van een commerciële Marketplace-vermelding of aanbieding met Azure Active Directory (Azure AD). Azure AD is een Cloud-identiteits service die gebruikmaakt van de industrie standaard frameworks om verificatie met een Microsoft-account in te scha kelen. Meer [informatie over Azure Active Directory](https://azure.microsoft.com/services/active-directory).

## <a name="azure-ad-benefits"></a>Voor delen van Azure AD

Microsoft AppSource-en Azure Marketplace-klanten gebruiken in-product ervaringen om catalogussen voor online winkels te doorzoeken. Voor deze acties moeten klanten zich aanmelden bij het product. Azure AD-integratie biedt de volgende voor delen:

- Snellere betrokkenheid en een geoptimaliseerde klant ervaring
- Eenmalige aanmelding (SSO) voor miljoenen zakelijke gebruikers
- Consistente, aanmeldings ervaring voor toepassingen die worden gepubliceerd door verschillende partners
- Schaal bare, platformoverschrijdende verificatie voor mobiele en Cloud-apps

## <a name="offers-that-require-azure-ad"></a>Aanbiedingen waarvoor Azure AD is vereist

De verschillende [aanbiedings opties en aanbiedings typen](determine-your-listing-type.md) voor commerciële Marketplace hebben verschillende vereisten voor de implementatie van Azure AD. Raadpleeg de volgende tabel voor meer informatie.

| Type aanbieding    | Is Azure AD SSO vereist om contact met mij op te nemen?  | Is Azure AD SSO vereist voor de proef versie? | Is Azure AD SSO vereist voor het test station?  | Azure AD SSO vereist voor Transact |
| :------------------- | :-------------------|:-------------------|:-------------------|:-------------------|
| Virtuele machine | N.v.t. | Nee | Nee | Nee |
| Azure-apps (oplossings sjabloon)  | N.v.t. | N.v.t. | N.v.t. | N.v.t. |
| Beheerde apps  | N.v.t. | N.v.t. | N.v.t. | Nee |
| SaaS  | Nee | Ja | Ja | Ja |
| Containers  | N.v.t. | N.v.t. | N.v.t. | Nee |
| Adviesservices  | Nee | N.v.t. | N.v.t. | N.v.t. |

Zie voor meer informatie over SaaS technische vereisten [Azure AD en transactable SaaS-aanbiedingen in de commerciële Marketplace](./azure-ad-saas.md).

## <a name="azure-ad-integration"></a>Azure AD-integratie

- Zie voor meer informatie over het integreren van Azure AD voor uw Program ma's voor het afhandelen van SaaS-aanbiedingen [Azure AD en transactable SaaS-aanbiedingen in de commerciële Marketplace](./azure-ad-saas.md).
- Zie [Azure Active Directory voor ontwikkel aars](../active-directory/develop/index.yml)voor meer informatie over het inschakelen van eenmalige aanmelding door Azure ad te integreren in uw vermelding.
- Zie [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../active-directory/manage-apps/what-is-single-sign-on.md)voor meer informatie over eenmalige aanmelding voor Azure AD.

## <a name="enable-a-trial-listing"></a>Een proef lijst inschakelen

Door automatische klant instellingen kan de kans op conversie worden verhoogd. Wanneer uw klant uw proef lijst selecteert en wordt omgeleid naar uw proef omgeving, kunt u de klant rechtstreeks instellen zonder aanvullende aanmeldings stappen.

Tijdens de verificatie verzendt Azure AD een token naar uw app of aanbieding. Met de gebruikers gegevens die door het token worden verstrekt, wordt het maken van een gebruikers account in uw app of aanbieding ingeschakeld. Zie [voorbeeld tokens](../active-directory/develop/id-tokens.md)voor meer informatie.

Wanneer u Azure AD gebruikt om verificatie met één klik in te scha kelen in uw app of proef versie, kunt u het volgende doen:

- Stroom lijn de klant ervaring van de commerciële Marketplace naar uw proef versie.
- Houd het gevoel van een in-product ervaring, zelfs wanneer de gebruiker wordt omgeleid van de commerciële Marketplace naar uw domein of test omgeving.
- Verminder de kans op onbreken wanneer gebruikers worden omgeleid omdat er geen aanvullende aanmeldings stappen zijn.
- Verminder implementatie barrières voor de grote populatie van Azure AD-gebruikers.

## <a name="verify-azure-ad-integration"></a>Azure AD-integratie verifiëren

### <a name="multitenant-solutions"></a>Multi tenant-oplossingen

Gebruik Azure AD om de volgende acties te ondersteunen:

- Registreer uw app in een van de online winkels voor commerciële Marketplace. Bekijk de [app-registratie](../active-directory/develop/quickstart-register-app.md) of AppSource- [certificering](../active-directory/azuread-dev/howto-get-appsource-certified.md) voor meer informatie.
- Schakel de functie ondersteuning voor multitenancy in azure AD in om een proef ervaring met één klik te krijgen.

Voer de volgende stappen uit als u geen ervaring hebt met het gebruik van Azure AD federatieve eenmalige aanmelding:

1. Registreer uw app in de commerciële Marketplace.
1. Ontwikkel SSO met Azure AD met behulp van [OAuth 2,0](../active-directory/azuread-dev/v1-protocols-oauth-code.md) of [OpenID Connect Connect](../active-directory/azuread-dev/v1-protocols-openid-connect-code.md).
1. Schakel de functie ondersteuning voor multitenancy in azure AD in om een proef ervaring met één klik te bieden.

### <a name="single-tenant-solutions"></a>Oplossingen met één Tenant

Gebruik Azure AD om een van de volgende acties te ondersteunen:

- Gast gebruikers toevoegen aan uw directory met behulp van [Azure AD B2B](../active-directory/b2b/what-is-b2b.md).
- U kunt voor klanten hand matig proef versies instellen met behulp van de publicatie optie **contact opnemen** .
- Ontwikkel een test drive per klant.
- Bouw een multi tenant-voorbeeld demo-app die gebruikmaakt van SSO.

## <a name="next-steps"></a>Volgende stappen

Als u dit nog niet hebt gedaan, 

- [Meer informatie](https://azuremarketplace.microsoft.com/sell) over de commerciële Marketplace.

Als u zich wilt registreren in het partner centrum, begint u met het maken van een nieuwe aanbieding of het werken met een bestaand abonnement:

- [Meld u aan bij Partner Center](https://partner.microsoft.com/dashboard/account/v3/enrollment/introduction/partnership) om uw aanbieding te maken of te volt ooien.
