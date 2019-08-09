---
title: Web-app die gebruikers aanmeldt (overzicht)-micro soft Identity-platform
description: Meer informatie over het bouwen van een web-app die gebruikers aanmeldt (overzicht)
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40
ms.collection: M365-identity-device-management
ms.openlocfilehash: 95aeeacfd85dd79453bff4e365e5b050039f77b9
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2019
ms.locfileid: "68852470"
---
# <a name="scenario-web-app-that-signs-in-users"></a>Scenario: Web-app waarmee gebruikers worden aangemeld

Meer informatie over wat u nodig hebt om een web-app te bouwen waarmee gebruikers zich aanmelden met het micro soft Identity-platform.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="getting-started"></a>Aan de slag

Als u uw eerste Portable (ASP.NET Core) Web-Apps wilt maken waarmee gebruikers zich aanmelden, volgt u deze Snelstartgids:

> [!div class="nextstepaction"]
> [Snelstart: ASP.NET Core web-app die gebruikers aanmeldt](quickstart-v2-aspnet-core-webapp.md)

Als u liever met ASP.NET wilt blijven, probeert u de volgende zelf studie:

> [!div class="nextstepaction"]
> [Snelstart: ASP.NET-Web-app die gebruikers aanmeldt](quickstart-v2-aspnet-webapp.md)

## <a name="overview"></a>Overzicht

U voegt verificatie toe aan uw web-app zodat gebruikers zich kunnen aanmelden. Door verificatie toe te voegen kan uw web-app toegang krijgen tot beperkte profiel informatie, en de ervaring die u aan de gebruikers biedt, bijvoorbeeld aanpassen. Web-apps verifiëren een gebruiker in een webbrowser. In dit scenario leidt de webtoepassing de browser van de gebruiker om deze te registreren bij Azure AD. Azure AD retourneert een aanmeldings reactie via de browser van de gebruiker, die claims bevat over de gebruiker in een beveiligings token. Gebruikers die zich aanmelden, maken gebruik van het [Open-ID Connect-](./v2-protocols-oidc.md) standaard protocol zelf vereenvoudigd door middleware- [bibliotheken](scenario-web-app-sign-user-app-configuration.md#libraries-used-to-protect-web-apps).

![Web-app-aanmeld gebruikers](./media/scenario-webapp/scenario-webapp-signs-in-users.svg)

Als tweede fase kunt u uw toepassing ook in staat stellen om namens de aangemelde gebruiker Web-Api's aan te roepen. Deze volgende fase is een ander scenario, dat u vindt in [Web-app-aanroepen Web-api's](scenario-web-app-call-api-overview.md)

> [!NOTE]
> Het toevoegen van een aanmelding aan een web-app is het beveiligen van de web-app en het valideren van een gebruikers token. Dit zijn de **middleware** -bibliotheken. Voor dit scenario is de micro soft-verificatie bibliotheken (MSAL) nog niet vereist, die het verkrijgen van een token voor het aanroepen van beveiligde Api's. De verificatie bibliotheken worden alleen geïntroduceerd in het vervolg scenario wanneer de web-app Web-Api's moet aanroepen.

## <a name="specifics"></a>Opsporingsgegevens

- Tijdens de registratie van de toepassing moet u een of meerdere opgeven (als u uw app op verschillende locaties implementeert) antwoord-Uri's. In sommige gevallen (ASP.NET/ASP.NET core) moet u de IDToken inschakelen. Ten slotte wilt u een afmeldings-URI instellen, zodat uw toepassing reageert op gebruikers die zich afmelden.
- In de code voor uw toepassing moet u de instantie opgeven waarvoor u zich aanmeldt voor de web-app. Mogelijk wilt u de token validatie aanpassen (met name in de ISV-scenario's).
- Webtoepassingen ondersteunen elk account type. Zie [ondersteunde account typen](v2-supported-account-types.md)voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Appregistratie](scenario-web-app-sign-user-app-registration.md)
