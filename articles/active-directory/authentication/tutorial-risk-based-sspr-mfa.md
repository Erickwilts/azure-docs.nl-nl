---
title: Op risicogebeurtenissen gebaseerde MFA en SSPR met Azure Identity Protection
description: In deze zelfstudie gaat u integraties met Azure Identity Protection inschakelen voor Multi-Factor Authentication en de selfservice voor het opnieuw instellen van wachtwoorden, om de kansen op risicovol gedrag te verkleinen.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: tutorial
ms.date: 01/31/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: 17cea353df0337b062b89cd440f79f7869450f8d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67113207"
---
# <a name="tutorial-use-risk-events-to-trigger-multi-factor-authentication-and-password-changes"></a>Zelfstudie: Risicogebeurtenissen gebruiken als trigger voor Multi-Factor Authentication en het wijzigen van wachtwoorden

In deze zelfstudie gaat u functies van Azure Active Directory (Azure AD) Identity Protection inschakelen. Dit is een Azure AD Premium P2-functie en is meer dan alleen een controle- en rapportagehulpmiddel. Ter bescherming van identiteiten binnen uw organisatie, kunt u op risicogebeurtenissen gebaseerde beleidsregels configureren die automatisch op risicovol gedrag reageren. Deze beleidsregels kunnen automatisch herstelstappen blokkeren of initiëren, zoals onder andere het verplicht wijzigen van wachtwoorden en het afdwingen van Multi-Factor Authentication.

Azure AD Identity Protection-beleid kunnen worden gebruikt naast de bestaande beleidsregels voor voorwaardelijke toegang als een extra beveiligingslaag. Misschien dat het nooit voorkomt dat gebruikers risicovol gedrag activeren waarvoor een van deze beleidsregels is vereist, maar u als beheerder weet dat ze zijn beveiligd.

Er zijn enkele gebeurtenissen die een risicogebeurtenis kunnen activeren, zoals onder andere:

* Gebruikers van wie de referenties zijn gelekt
* Aanmeldingen vanaf anonieme IP-adressen
* Onmogelijke reis naar ongewone locaties
* Aanmeldingen vanaf geïnfecteerde apparaten
* Aanmeldingen van IP-adressen met verdachte activiteit
* Aanmeldingen vanaf onbekende locaties

Meer informatie over Azure AD Identity Protection vindt u in het artikel [Wat is Azure AD Identity Protection](../active-directory-identityprotection.md)

> [!div class="checklist"]
> * Registratie bij Azure MFA inschakelen
> * Op risicogebeurtenissen gebaseerde wachtwoordwijzigingen inschakelen
> * Op risicogebeurtenissen gebaseerde Multi-Factor Authentication inschakelen

## <a name="prerequisites"></a>Vereisten

* Toegang tot een werkende Azure AD-tenant waaraan ten minste een proefversie van een Azure AD Premium P2-licentie is toegewezen.
* Een account met de bevoegdheden van een globale beheerder op de Azure AD-tenant.
* U hebt de voorgaande zelfstudies over de selfservice voor wachtwoordherstel (SSPR) en Multi-Factor Authentication (MFA) voltooid.

## <a name="enable-risk-based-policies-for-sspr-and-mfa"></a>Op risicogebeurtenissen gebaseerde beleidsregels voor SSPR en MFA inschakelen

Het inschakelen van het beleid op basis van risicogebeurtenissen is een eenvoudig proces. De onderstaande stappen leiden u door een voorbeeldconfiguratie.

### <a name="enable-users-to-register-for-multi-factor-authentication"></a>Gebruikers in staat stellen zich voor Multi-Factor Authentication te registeren.

Azure AD Identity Protection bevat een standaardbeleid dat u kan helpen bij het registreren van uw gebruikers voor Multi-Factor Authentication en om hun huidige registratiestatus vast te stellen. Door dit beleid in te schakelen, worden gebruikers niet meteen verplicht om Multi-Factor Authentication uit te voeren, maar wordt hen gevraagd zich vooraf te registreren.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Klik op **Alle services** en blader vervolgens naar **Azure AD Identity Protection**.
1. Klik op **Registratie bij MFA**.
1. Stel Beleid afdwingen in op **Aan**.
   1. Door beleid in te stellen, moeten alle gebruikers methoden registreren ter voorbereiding van het gebruik van Multi-Factor Authentication.
1. Klik op **Opslaan**.

   ![Vereisen dat gebruikers zich registreren voor MFA bij het aanmelden](./media/tutorial-risk-based-sspr-mfa/risk-based-require-mfa-registration.png)

### <a name="enable-risk-based-password-changes"></a>Op risicogebeurtenissen gebaseerde wachtwoordwijzigingen inschakelen

Microsoft werkt samen met onderzoekers, justitie en politie, diverse beveiligingsteams bij Microsoft en andere betrouwbare bronnen om naar paren van gebruikersnamen en wachtwoorden te zoeken. Wanneer een van deze paren overeenkomt met een account in uw omgeving, kan een op risicogebeurtenissen gebaseerde wachtwoordwijziging worden geactiveerd met behulp van het volgende beleid.

1. Klik op Beleid voor gebruikersrisico's.
1. Selecteer bij **Voorwaarden** de optie **Gebruikersrisico** en kies vervolgens **Gemiddeld en hoger**.
1. Klik op Selecteren en vervolgens op Voltooid.
1. Bij **Toegang** kiest u **Toegang toestaan** en selecteert u vervolgens **Wachtwoordwijziging vereisen**.
1. Klik op Selecteren.
1. Stel Beleid afdwingen in op **Aan**.
1. Klik op **Opslaan**

### <a name="enable-risk-based-multi-factor-authentication"></a>Op risicogebeurtenissen gebaseerde Multi-Factor Authentication inschakelen

De meeste gebruikers vertonen normaal gedrag dat kan worden getraceerd. Wanneer ze buiten de norm hiervoor vallen, zou het riskant kunnen zijn om hen toe te staan zich zomaar aan te melden. Mogelijk wilt u die gebruiker blokkeren of misschien alleen maar vragen een Multi-Factor Authentication uit te voeren om te bewijzen dat ze echt zijn wie ze beweren te zijn. Als u een beleid wilt inschakelen waarbij MFA is vereist als er een risicovolle aanmelding wordt gedetecteerd, schakelt u het volgende beleid in.

1. Klik op Beleid voor aanmeldingsrisico's instellen.
1. Selecteer bij **Voorwaarden** de optie **Gebruikersrisico** en kies vervolgens **Gemiddeld en hoger**.
1. Klik op Selecteren en vervolgens op Voltooid.
1. Bij **Toegang** kiest u **Toegang toestaan** en selecteert u vervolgens **Multi-Factor Authentication vereisen**.
1. Klik op Selecteren.
1. Stel Beleid afdwingen in op **Aan**.
1. Klik op **Opslaan**

## <a name="clean-up-resources"></a>Resources opschonen

Als u klaar bent met testen en er geen behoefte meer aan hebt dat het op risicogebeurtenissen gebaseerd beleid is ingeschakeld, gaat u terug naar elk beleid dat u wilt uitschakelen en stelt u **Beleid afdwingen** in op **Uit**.
