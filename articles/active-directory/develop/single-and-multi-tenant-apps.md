---
title: Apps voor één of meerdere tenants in Azure Active Directory
description: Meer informatie over de functies en de verschillen tussen één tenant en multitenant-apps in Azure AD.
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2018
ms.author: ryanwi
ms.reviewer: justhu
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8db590c8c79db9bc47edbbef69a11d3b0e4b51f6
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2019
ms.locfileid: "65540352"
---
# <a name="tenancy-in-azure-active-directory"></a>Tenants in Azure Active Directory

Azure Active Directory (Azure AD) organiseert objecten, zoals gebruikers en apps in groepen met de naam *tenants*. Tenants kunnen een beheerder beleid instellen voor gebruikers binnen de organisatie en de apps die de organisatie eigenaar is van om te voldoen aan de beveiligings- en gebruiksbeleid. 

## <a name="who-can-sign-in-to-your-app"></a>Wie kan zich aanmelden bij uw app?

Als het gaat om het ontwikkelen van apps, ontwikkelaars kunnen kiezen hun app configureren voor één tenant of meerdere tenants worden tijdens de appregistratie in de [Azure-portal](https://portal.azure.com).
* Apps met één tenant zijn alleen beschikbaar in de tenant die ze in, ook wel bekend als hun starttenant zijn geregistreerd.
* Apps met meerdere tenants zijn beschikbaar voor gebruikers in hun starttenant en andere tenants.

U kunt uw app om te worden van één tenant of meerdere tenants door in te stellen de doelgroep als volgt configureren in de Azure-portal.

| Doelgroep | Eén/meerdere-tenant | Wie zich kan aanmelden | 
|----------|--------| ---------|
| Accounts in deze map alleen | Eén tenant | Alle gebruikers- en gastaccounts in uw map kunnen gebruikmaken van uw toepassing of API.<br>*Gebruik deze optie als uw doelgroep intern voor uw organisatie is.* |
| Accounts in een Azure AD-map | Multitenant | Alle gebruikers en gasten met een werk- of schoolaccount van Microsoft kunnen u uw toepassing of de API gebruiken. Dit omvat scholen en bedrijven die gebruikmaken van Office 365.<br>*Gebruik deze optie als de doelgroep bestaat uit bedrijf of onderwijsinstelling klanten.* |
| Accounts in een Azure AD-directory en persoonlijke Microsoft-accounts (zoals Skype, Xbox, Outlook.com) | Multitenant | Alle gebruikers met een werk- of school- of persoonlijk Microsoft-account kunnen gebruiken voor uw toepassing of de API. Het bevat scholen en bedrijven die gebruikmaken van Office 365, evenals persoonlijke accounts die worden gebruikt om aan te melden bij services als Xbox en Skype.<br>*Gebruik deze optie om het doel van de uitgebreide set Microsoft-accounts.* | 

## <a name="best-practices-for-multi-tenant-apps"></a>Aanbevolen procedures voor apps met meerdere tenants

Het bouwen van geweldige apps met meerdere tenants kan lastig zijn vanwege het aantal verschillende beleidsregels die IT-beheerders in hun tenants instellen kunnen. Als u ervoor kiest om een multitenant-app te maken, volgt u deze aanbevolen procedures:

* Test uw app in een tenant die is geconfigureerd [beleid voor voorwaardelijke toegang](conditional-access-dev-guide.md).
* Ga als volgt het principe van minimale gebruikerstoegang om ervoor te zorgen dat uw app alleen aanvragen machtigingen die daadwerkelijk nodig. Voorkomen dat de machtigingen die toestemming van een beheerder vereisen als gebruikers mogelijk niet verkrijgen van uw app helemaal in sommige organisaties aanvragen. 
* Bieden de juiste namen en beschrijvingen voor alle machtigingen die u beschikbaar als onderdeel van uw app maakt. Hierdoor kunnen gebruikers en beheerders weten wat ze ermee akkoord dat wanneer ze proberen om het gebruik van uw app-API's. Zie voor meer informatie, de aanbevolen procedures-sectie in de [handleiding machtigingen](v1-permissions-and-consent.md).

## <a name="next-steps"></a>Volgende stappen

* [Het converteren van een app voor meerdere tenants worden](howto-convert-app-to-be-multi-tenant.md)
