---
title: bestand opnemen
description: bestand opnemen
services: active-directory
documentationcenter: dev-center-name
author: danieldobalian
manager: CelesteDG
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: ios
ms.workload: identity
ms.date: 09/19/2018
ms.author: jmprieur
ms.reviwer: brandwe
ms.custom: include file
ms.openlocfilehash: 4a898aab9b1eab24b60337428128083255c32b7a
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/21/2019
ms.locfileid: "65967606"
---
## <a name="test-querying-the-microsoft-graph-api-from-your-ios-application"></a>De Microsoft Graph-API vanaf uw iOS-toepassing uitvoeren van query's testen

Voor het uitvoeren van de code in de simulator drukt u op **opdracht** + **R**.

![Uw toepassing testen in de emulator](media/active-directory-develop-guidedsetup-ios-test/iostestscreenshot.png)

Wanneer u klaar om te testen bent, selecteert u **Microsoft Graph-API aanroepen**. Wanneer u hierom wordt gevraagd, typt u uw gebruikersnaam en wachtwoord.

### <a name="provide-consent-for-application-access"></a>Geef toestemming voor toegang tot toepassingen

De eerste keer dat u zich aanmelden bij uw toepassing, bent u gevraagd te geven van toestemming van de toepassing om toegang tot uw profiel en u zich aanmeldt:

![Geef uw toestemming voor toegang tot toepassingen](media/active-directory-develop-guidedsetup-ios-test/iosconsentscreen.png)

### <a name="view-application-results"></a>Resultaten van de toepassing weergeven

Nadat u zich hebt aangemeld, ziet u de gegevens van uw gebruikersprofiel geretourneerd door de Microsoft Graph API-aanroep in de **logboekregistratie** sectie.

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Meer informatie over bereiken en gedelegeerde machtigingen

De Microsoft Graph API vereist de **user.read** scope om te lezen van het gebruikersprofiel. Dit bereik wordt automatisch toegevoegd in elke toepassing die geregistreerd op de portal voor wachtwoordregistratie standaard. Andere voor Microsoft Graph-API's, evenals de aangepaste API's voor uw back-endserver, mogelijk extra scopes. De Microsoft Graph API vereist de **Calendars.Read** bereik om agenda's van de gebruiker weer te geven.

Voor toegang tot agenda's van de gebruiker in de context van een toepassing moet toevoegen de **Calendars.Read** overgedragen machtigingen voor de registratiegegevens van de toepassing. Voeg vervolgens de **Calendars.Read** bereik instellen op de **acquireTokenSilent** aanroepen.

>[!NOTE]
>De gebruiker mogelijk voor extra toestemmingen gevraagd als u het aantal bereiken verhogen.

<!--end-collapse-->

[!INCLUDE [Help and support](./active-directory-develop-help-support-include.md)]
