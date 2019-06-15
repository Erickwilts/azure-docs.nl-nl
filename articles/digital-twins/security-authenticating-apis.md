---
title: Informatie over Azure digitale dubbels API-verificatie | Microsoft Docs
description: Gebruikt Azure digitale dubbels om verbinding te verifiëren naar API 's
author: lyrana
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 11/13/2018
ms.author: lyrana
ms.openlocfilehash: 4ea4479d77e06940bed50859341952ffbcbbda46
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60533823"
---
# <a name="connect-and-authenticate-to-apis"></a>Verbinding maken en te verifiëren voor API 's

Azure van digitale dubbels maakt gebruik van Azure Active Directory (Azure AD) om te verifiëren van gebruikers en toepassingen te beschermen. Azure AD ondersteunt verificatie voor een verscheidenheid aan moderne architecturen. Ze allemaal zijn gebaseerd op de standaardprotocollen OAuth 2.0 of OpenID Connect. Ontwikkelaars kunnen bovendien Azure AD gebruiken om één tenant en line-of-business (LOB)-toepassingen te bouwen. Ontwikkelaars kunnen ook Azure AD gebruiken voor het ontwikkelen van toepassingen voor meerdere tenants.

Voor een overzicht van Azure AD, gaat u naar de [fundamentals pagina](https://docs.microsoft.com/azure/active-directory/fundamentals/index) voor stapsgewijze instructies, concepten en snelstartgidsen.

Om een toepassing of service met Azure Active Directory te integreren, moet een ontwikkelaar de toepassing eerst bij Azure Active Directory registreren. Zie voor gedetailleerde instructies en schermafbeeldingen [in deze Quick Start](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v1-add-azure-ad-app).

[Toepassingsscenario's vijf primaire](https://docs.microsoft.com/azure/active-directory/develop/v2-app-types) worden ondersteund door Azure AD:

* Toepassing met één pagina (SPA): Er moet een gebruiker zich aanmeldt bij een toepassing met één pagina die wordt beveiligd door Azure AD.
* De webbrowser voor web-App: Er moet een gebruiker zich aanmeldt bij een webtoepassing die wordt beveiligd door Azure AD.
* Systeemeigen toepassing voor de web-API: Een systeemeigen toepassing die wordt uitgevoerd op een telefoon, tablet of PC moet een gebruiker voor resources van een web-API die wordt beveiligd door Azure AD verifiëren.
* Web-App aan web-API: Een web-App nodig heeft om op te halen van resources van een web-API is beveiligd door Azure AD.
* Daemon of servertoepassing naar web-API: Een daemontoepassing of een servertoepassing met geen web-UI moet resources ophalen uit een web-API die is beveiligd door Azure AD.

De Windows Azure-Verificatiebibliotheek biedt verschillende manieren voor het verkrijgen van Active Directory-tokens. Zie voor meer informatie over de bibliotheek en codevoorbeelden, [in dit artikel](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki).

## <a name="call-digital-twins-from-a-middle-tier-web-api"></a>Digitale dubbels aanroepen vanuit een middelste laag web-API

Wanneer ontwikkelaars ontwerpen van oplossingen voor digitale Twins, maken ze doorgaans een middelste laag-toepassing of de API. De app of API vervolgens roept de digitale dubbels API downstream. Ter ondersteuning van de oplossingsarchitectuur van deze standaard web, zorg ervoor dat gebruikers eerste:

1. Verifiëren met de middelste laag-toepassing

1. Een token van OAuth 2.0 namens is verkregen tijdens de verificatie

1. Het verkregen token wordt vervolgens gebruikt om te verifiëren met of aanroepen van API's die de stroom op-andere gebruikers-Of verdere downstream gebruikt

Zie voor instructies over het organiseren van de stroom op-andere gebruikers-of [OAuth 2.0 namens-stroom](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow). Ook vindt u voorbeelden van code in [een downstream web-API aanroept](https://azure.microsoft.com/resources/samples/active-directory-dotnet-webapi-onbehalfof/).

## <a name="next-steps"></a>Volgende stappen

Als u wilt configureren en testen van digitale dubbels van Azure met behulp van de stroom voor OAuth 2.0-impliciete goedkeuring voor, Lees [Postman configureren](./how-to-configure-postman.md).

Lees meer over de beveiliging van Azure digitale dubbels [maken en beheren van roltoewijzingen](./security-create-manage-role-assignments.md).