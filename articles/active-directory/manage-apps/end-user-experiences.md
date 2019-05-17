---
title: Ervaring van de eindgebruikers voor toepassingen - Azure Active Directory | Microsoft Docs
description: Azure Active Directory (Azure AD) biedt aanpasbare manieren voor het implementeren van toepassingen voor eindgebruikers in uw organisatie.
services: active-directory
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 05/03/2019
ms.author: celested
ms.reviewer: arvindh
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3880a62d58b15ef07e524d69c38ba723ea56178f
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65781066"
---
# <a name="end-user-experiences-for-applications-in-azure-active-directory"></a>Ervaringen van eindgebruikers voor toepassingen in Azure Active Directory
Azure Active Directory (Azure AD) biedt aanpasbare manieren voor het implementeren van toepassingen voor eindgebruikers in uw organisatie:

* Azure AD-Toegangsvenster
* Startprogramma voor Office 365-toepassingen
* Directe aanmelding bij federatieve apps
* Dieptekoppelingen naar federatieve apps, op basis van wachtwoorden, of bestaande apps

Welke methode(n) die u wilt implementeren in uw organisatie is's naar eigen inzicht.

## <a name="azure-ad-access-panel"></a>Azure AD-Toegangsvenster
Het deelvenster toegang https://myapps.microsoft.com is een webportal waarmee een eindgebruiker met een organisatie-account in Azure Active Directory om weer te geven en starten cloud-gebaseerde toepassingen waaraan ze zijn toegang verleend door de Azure AD-beheerder. Als u een eindgebruiker met [Azure Active Directory Premium](https://azure.microsoft.com/pricing/details/active-directory/), u kunt ook gebruikmaken van mogelijkheden voor groepsbeheer met Self-service via het toegangsvenster.

![Azure AD-Toegangsvenster](media/what-is-single-sign-on/azure-ad-access-panel.png)

Het toegangsvenster is gescheiden van de Azure-portal en hoeft geen gebruikers hebben een Azure-abonnement of een Office 365-abonnement.

Zie voor meer informatie over de Azure AD-toegangspaneel, de [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="office-365-application-launcher"></a>Startprogramma voor Office 365-toepassingen
Voor organisaties die Office 365 hebt geïmplementeerd, toepassingen die zijn toegewezen aan gebruikers via Azure AD ook worden weergegeven in de Office 365-portal op [ https://portal.office.com/myapps ](https://portal.office.com/myapps). Dit maakt het gemakkelijk en handig voor gebruikers in een organisatie hun apps starten zonder gebruik te maken van een tweede portal en is de aanbevolen app starten oplossing voor organisaties met behulp van Office 365.

![Office 365-portal](./media/end-user-experiences/microsoft-365-portal-office-com.png)

Zie voor meer informatie over het startprogramma voor Office 365-toepassingen, [uw App worden weergegeven in het startprogramma voor Office 365](https://msdn.microsoft.com/office/office365/howto/connect-your-app-to-o365-app-launcher).

## <a name="direct-sign-on-to-federated-apps"></a>Directe aanmelding bij federatieve apps
Meest federatieve toepassingen die ondersteuning bieden voor SAML 2.0, WS-Federation en OpenID connect ook ondersteuning voor de mogelijkheid voor gebruikers om te beginnen bij de toepassing en vervolgens u aangemeld via Azure AD door automatische omleiding of door te klikken op een koppeling aan te melden bij. Dit staat bekend als serviceprovider-aanmelding wordt gestart en meest federatieve toepassingen in de Azure AD-toepassingsgalerie ondersteuning bieden voor deze (Zie de documentatie gekoppeld aan de app configuratie voor eenmalige aanmelding wizard in de Azure-portal voor meer informatie).

![](./media/end-user-experiences/workdaymobile.png)

## <a name="direct-sign-on-links"></a>Directe aanmelding koppelingen
Azure AD biedt ook ondersteuning voor directe eenmalige aanmelding in koppelingen naar afzonderlijke toepassingen die ondersteuning op basis van wachtwoorden eenmalige aanmelding, gekoppelde eenmalige aanmelding en een vorm van federatieve eenmalige aanmelding bieden.

Deze koppelingen zijn speciaal ontworpen URL's die een gebruiker door het proces voor aanmelding bij Azure AD voor een bepaalde toepassing verzenden zonder de gebruiker start ze vanuit de Azure AD toegang tot deelvenster of Office 365. Deze **gebruiker toegang krijgen tot URL's** kunt u vinden onder de eigenschappen van de beschikbare zakelijke toepassingen. Selecteer in de Azure portal, **Azure Active Directory** > **bedrijfstoepassingen**. Selecteer de toepassing en selecteer vervolgens **eigenschappen**.

![Voorbeeld van de URL van gebruikerstoegang in de eigenschappen van Twitter](media/end-user-experiences/direct-sign-on-link.png)

Deze koppelingen kan worden gekopieerd en geplakt waar die u wilt een koppeling aanmelding aan de geselecteerde toepassing wilt opgeven. Dit wordt mogelijk in een e-mailbericht, of in een aangepaste web-portal op basis van die u hebt ingesteld voor toegang tot de toepassing van gebruiker. Hier volgt een voorbeeld van een Azure AD direct één aanmeldings-URL voor Twitter:

`https://myapps.microsoft.com/signin/Twitter/230848d52c8745d4b05a60d29a40fced`

Net als bij de organisatie-specifieke URL's voor het toegangsvenster, u kunt verder aanpassen deze URL door een van de actieve of geverifieerde domeinen voor uw directory nadat de myapps.microsoft.com domein toe te voegen. Dit zorgt ervoor dat eventuele huisstijl van de organisatie wordt geladen onmiddellijk op de pagina aanmelden zonder dat de gebruiker hoeft in te voeren hun gebruikers-ID eerst:

`https://myapps.microsoft.com/contosobuild.com/signin/Twitter/230848d52c8745d4b05a60d29a40fced`

Wanneer een geautoriseerde gebruiker op een van deze koppelingen toepassingsspecifieke klikt, ze eerst zien van hun organisatie-aanmeldingspagina (ervan uitgaande dat ze nog niet bent aangemeld) en na het aanmelden worden omgeleid naar de app zonder eerst op het toegangsvenster stoppen. Als de gebruiker vereisten ontbreken er voor toegang tot de toepassing, zoals de Browseruitbreiding van eenmalige aanmelding op basis van wachtwoorden, wordt klikt u vervolgens de koppeling de gebruiker gevraagd de ontbrekende extensie installeren. URL van de koppeling blijft ook constante als de configuratie voor eenmalige aanmelding voor de toepassing wordt gewijzigd.

Deze koppelingen de mechanismen voor hetzelfde besturingselement gebruiken als het toegangsvenster en de Office 365, en alleen deze gebruikers of groepen die zijn toegewezen aan de toepassing in Azure portal is mogelijk om te verifiëren. Elke gebruiker die niet gemachtigd is zien echter een bericht waarin wordt uitgelegd dat ze geen toegang is verleend en een koppeling naar het laden van het toegangsvenster als u wilt weergeven van beschikbare toepassingen waarvoor ze toegang hebben, krijgen.

## <a name="next-steps"></a>Volgende stappen
Zie voor de implementatieplannen, [Azure Active Directory-implementatieplannen](../fundamentals/active-directory-deployment-plans.md)
