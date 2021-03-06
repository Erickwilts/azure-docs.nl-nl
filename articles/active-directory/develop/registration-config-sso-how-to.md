---
title: Eenmalige aanmelding voor de toepassing configureren
description: Eenmalige aanmelding configureren voor een aangepaste toepassing die u ontwikkelt en registreert met Azure AD.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.custom: aaddev
ms.workload: identity
ms.topic: conceptual
ms.date: 07/15/2019
ms.author: ryanwi
ROBOTS: NOINDEX
ms.openlocfilehash: 62f4f629e44d317d36e182adb48f8f00b9f1c2b3
ms.sourcegitcommit: 2488894b8ece49d493399d2ed7c98d29b53a5599
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2021
ms.locfileid: "98063056"
---
# <a name="how-to-configure-single-sign-on-for-an-application"></a>Eenmalige aanmelding voor een toepassing configureren

Het inschakelen van federatieve eenmalige aanmelding (SSO) in uw app wordt automatisch ingeschakeld wanneer federeren via Azure AD voor OpenID Connect Connect, SAML 2,0 of WS-inschakelt. Als uw eind gebruikers zich moeten aanmelden ondanks al een bestaande sessie met Azure AD, is het waarschijnlijk dat uw app onjuist is geconfigureerd.

* Als u ADAL/MSAL gebruikt, zorg er dan voor dat u **PromptBehavior** hebt ingesteld op **auto** in plaats van **altijd**.

* Als u een mobiele app bouwt, hebt u mogelijk aanvullende configuraties nodig om brokered of niet-brokered SSO in te scha kelen.

Zie [SSO van meerdere apps inschakelen in Android](../azuread-dev/howto-v1-enable-sso-android.md)voor Android.<br>

Zie [SSO van meerdere apps inschakelen in Ios](../azuread-dev/howto-v1-enable-sso-ios.md)voor IOS.

## <a name="next-steps"></a>Volgende stappen

[Azure AD SSO](../manage-apps/what-is-single-sign-on.md)<br>

[SSO van meerdere apps inschakelen in Android](../azuread-dev/howto-v1-enable-sso-android.md)<br>

[SSO van meerdere apps inschakelen in iOS](../azuread-dev/howto-v1-enable-sso-ios.md)<br>

[Apps integreren in AzureAD](./quickstart-register-app.md)<br>

[Machtigingen en toestemming in het eindpunt van het Microsoft-identiteitsplatform](./v2-permissions-and-consent.md)<br>

[AzureAD stack overflow](https://stackoverflow.com/questions/tagged/azure-active-directory)