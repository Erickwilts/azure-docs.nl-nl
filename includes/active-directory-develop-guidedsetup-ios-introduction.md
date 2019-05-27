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
ms.date: 03/20/2019
ms.author: jmprieur
ms.reviwer: brandwe
ms.custom: include file
ms.openlocfilehash: 08849cb44b5a3db3d66dc444d5e84fb3df66ad9a
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/21/2019
ms.locfileid: "65967634"
---
# <a name="call-the-microsoft-graph-api-from-an-ios-application"></a>De Microsoft Graph-API aanroepen vanuit een iOS-toepassing

Deze handleiding wordt beschreven hoe een toepassing systeemeigen iOS (Swift) API's waarvoor toegangstokens van het eindpunt van Microsoft identity-platform kunt aanroepen. De handleiding wordt uitgelegd hoe u voor toegangstokens verkrijgen en ze gebruiken in aanroepen naar de Microsoft Graph API en andere API's.

Nadat u de oefeningen in deze handleiding hebt voltooid, kan uw toepassing een beveiligde API oproept vanuit een bedrijf of organisatie die Azure AD is. Uw toepassing kunt maken van beveiligde API-aanroepen met behulp van persoonlijke accounts, zoals outlook.com, live.com en andere, evenals de werk-of schoolaccount.

## <a name="prerequisites"></a>Vereisten

- XCode-versie 10.x is vereist voor het voorbeeld dat is gemaakt in deze handleiding. U kunt downloaden XCode uit de [iTunes website](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12 "XCode downloaden URL").
- De [Carthage](https://github.com/Carthage/Carthage) afhankelijkheidsbeheerder is vereist voor Pakketbeheer.

## <a name="how-this-guide-works"></a>De werking van deze handleiding

![Laat zien hoe de voorbeeld-app wordt gegenereerd door deze zelfstudie werkt](media/active-directory-develop-guidedsetup-ios-introduction/iosintro.svg)

In deze handleiding kan de voorbeeld-App een iOS-toepassing om op te vragen van de Microsoft Graph API of een web-API die tokens van het eindpunt van de Microsoft identity-platform accepteert. In dit scenario voor een token wordt toegevoegd aan de HTTP-aanvragen met behulp van de **autorisatie** header. Ophalen van tokens en vernieuwing worden afgehandeld door de Microsoft Authentication Library (MSAL).

### <a name="handle-token-acquisition-for-access-to-protected-web-apis"></a>Verwerken van token ophalen voor toegang tot beveiligde web-API 's

Nadat de gebruiker zich verifieert, wordt in de voorbeeld-App een token ontvangt. Het token wordt gebruikt om op te vragen van de Microsoft Graph API of een web-API die wordt beveiligd door het eindpunt van de Microsoft identity-platform.

API's, zoals Microsoft Graph, vereisen een toegangstoken voor toegang tot bepaalde resources. Tokens zijn vereist voor het lezen van het profiel van een gebruiker, toegang tot de agenda van een gebruiker, een e-mail verzenden, enzovoort. Uw toepassing kan een toegangstoken aanvragen met behulp van MSAL en API-bereiken op te geven. Het toegangstoken wordt toegevoegd aan de HTTP **autorisatie** koptekst voor elke aanroep die op basis van de beveiligde bron plaatsvindt.

MSAL kunt opslaan in cache en toegangstokens vernieuwen voor u, zodat uw toepassing niet te hoeft beheren.

## <a name="libraries"></a>Bibliotheken

Deze handleiding worden de volgende bibliotheek gebruikt:

|Tapewisselaar|Description|
|---|---|
|[MSAL.framework](https://github.com/AzureAD/microsoft-authentication-library-for-objc)|Preview van Microsoft Authentication Library voor iOS|
