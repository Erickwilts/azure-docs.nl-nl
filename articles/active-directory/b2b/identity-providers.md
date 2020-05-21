---
title: Id-providers voor externe identiteiten-Azure AD
description: Azure Active Directory B2B-samen werking ondersteunt multi-factor Authentication (MFA) voor selectieve toegang tot uw bedrijfs toepassingen
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 05/19/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: elisolMS
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2284d015b451872753dd0855cac42e6f1926545c
ms.sourcegitcommit: 958f086136f10903c44c92463845b9f3a6a5275f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2020
ms.locfileid: "83712159"
---
# <a name="identity-providers-for-external-identities"></a>Id-providers voor externe identiteiten

Een *id-provider* maakt, onderhoudt en beheert id-gegevens en biedt tegelijkertijd verificatieservices voor toepassingen. Wanneer u uw apps en resources deelt met externe gebruikers, is Azure AD de standaard id-provider voor delen. Dit betekent dat wanneer u externe gebruikers uitnodigt die al over een Azure AD of Microsoft-account beschikken, ze automatisch kunnen aanmelden zonder verdere configuratie van uw onderdeel.

U kunt gebruikers echter in staat stellen zich aan te melden met verschillende id-providers.

- **Google**: met Google Federation kunnen externe gebruikers uitnodigingen van u inwisselen door zich aan te melden bij uw apps met hun eigen Gmail-accounts. Google Federation kan ook worden gebruikt in uw Self-service registratie gebruikers stromen.
   > [!NOTE]
   > Als een gebruikers stroom is gekoppeld aan een app en u een uitnodiging voor een gebruiker naar deze app stuurt, kan de gebruiker in het huidige selfservice-aanmeldings voorbeeld geen Gmail-account gebruiken om de uitnodiging te inwisselen. Als tijdelijke oplossing kan de gebruiker het aanmeldings proces voor de self-service door lopen. Of ze kunnen de uitnodiging inwisselen door toegang te krijgen tot een andere app of door gebruik te maken van de portal mijn apps op https://myapps.microsoft.com .

- **Facebook**: wanneer u een app bouwt, kunt u self-service registratie configureren en Facebook-Federatie inschakelen zodat gebruikers zich kunnen aanmelden voor uw app met hun eigen Facebook-accounts. Facebook kan alleen worden gebruikt voor Self-service-aanmeld gebruikers stromen en is niet beschikbaar als aanmeldings optie wanneer gebruikers uitnodigingen van u inwisselen.

- **Directe Federatie**: u kunt ook directe Federatie instellen met een externe ID-provider die ondersteuning biedt voor de SAML-of WS-voeder-protocollen. Met directe Federatie kunnen externe gebruikers uitnodigingen van u inwisselen door zich aan te melden bij uw apps met hun bestaande sociale of zakelijke accounts. 
   > [!NOTE]
   > Directe Federatie-id-providers kunnen niet worden gebruikt in uw Self-service registratie gebruikers stromen.


## <a name="how-it-works"></a>Hoe het werkt

Met de functie voor zelf registratie van Azure AD externe identiteiten kunnen gebruikers zich aanmelden met hun Azure AD-, Google-of Facebook-account. Als u sociale id-providers wilt instellen in uw Azure AD-Tenant, maakt u een toepassing bij elke id-provider en configureert u de referenties. U ontvangt een client-of App-ID en een client-of app-geheim, dat u vervolgens kunt toevoegen aan uw Azure AD-Tenant.

Zodra u een id-provider aan uw Azure AD-Tenant hebt toegevoegd:

- Wanneer u een externe gebruiker uitnodigt voor apps of resources in uw organisatie, kan de externe gebruiker zich aanmelden met een eigen account bij die id-provider.
- Wanneer u [Aanmelden via selfservice](self-service-sign-up-overview.md) voor uw apps inschakelt, kunnen externe gebruikers zich aanmelden voor uw apps met hun eigen accounts met de id-providers die u hebt toegevoegd.

> [!NOTE]
> Azure AD is standaard ingeschakeld voor Self-service registratie, zodat gebruikers altijd de mogelijkheid hebben om zich aan te melden met een Azure AD-account.

Wanneer u uw uitnodiging ingewisselt of zich aanmeldt voor uw app, heeft de externe gebruiker de mogelijkheid zich aan te melden en te verifiëren met de ID-provider voor sociale netwerken:

![Scherm afbeelding van het aanmeldings scherm met Google-en Facebook-opties](media/identity-providers/sign-in-with-social-identity.png)

Voor een optimale aanmeldings ervaring kunt u waar mogelijk met id-providers communiceren, zodat u uw uitgenodigde gasten een naadloze aanmeldings ervaring krijgt wanneer ze toegang hebben tot uw apps.  

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de volgende artikelen voor meer informatie over het toevoegen van id-providers voor aanmelding bij uw toepassingen:

- [Google toevoegen](google-federation.md) aan uw lijst met sociale id-providers
- [Facebook toevoegen](facebook-federation.md) aan uw lijst met sociale id-providers
- [Stel directe Federatie](direct-federation.md) in met een wille keurige organisatie waarvan de ID-provider ondersteuning biedt voor het SAML 2,0-of WS-voeder protocol. Houd er rekening mee dat directe Federatie geen optie is voor Self-service registratie gebruikers stromen.
