---
title: 'Voorwaardelijke toegang: MFA vereisen voor alle gebruikers-Azure Active Directory'
description: Een aangepast beleid voor voorwaardelijke toegang maken om te vereisen dat alle gebruikers multi-factor Authentication uitvoeren
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 04/02/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: 60d0ad0a1c0a1b4d13ce4d386df22406a8ab8e51
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "81617625"
---
# <a name="conditional-access-require-mfa-for-all-users"></a>Voorwaardelijke toegang: MFA vereisen voor alle gebruikers

Als Alex Weinert, de directory met identiteits beveiliging bij micro soft, vermeld in zijn blog post [uw PA $ $Word niet van belang](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Your-Pa-word-doesn-t-matter/ba-p/731984)is:

> Uw wacht woord is niet van belang, maar MFA doet het niet. Op basis van onze studies is uw account meer dan 99,9% minder kans op schade als u MFA gebruikt.

De richt lijnen in dit artikel helpen uw organisatie bij het maken van een evenwichtige MFA-beleid voor uw omgeving.

## <a name="user-exclusions"></a>Gebruikers uitsluitingen

Beleids regels voor voorwaardelijke toegang zijn krachtige hulp middelen. u wordt aangeraden de volgende accounts uit te sluiten van het beleid:

* **Nood toegang** of **afbreek glazen** om te voor komen dat accounts voor tenants worden vergrendeld. In het onwaarschijnlijke scenario zijn alle beheerders vergrendeld van uw Tenant. u kunt uw beheer account voor nood toegang gebruiken om u aan te melden bij de Tenant stappen nemen om de toegang te herstellen.
   * Meer informatie vindt u in het artikel [manage accounts voor nood toegang in azure AD](../users-groups-roles/directory-emergency-access.md).
* **Service accounts** en **service-principals**, zoals het Azure AD Connect Sync-account. Service accounts zijn niet-interactieve accounts die niet zijn gekoppeld aan een bepaalde gebruiker. Ze worden normaal gesp roken gebruikt door back-end-services die programmatische toegang tot toepassingen toestaan, maar worden ook gebruikt om u aan te melden bij systemen voor administratieve doel einden. Service accounts zoals deze moeten worden uitgesloten omdat MFA niet programmatisch kan worden voltooid. Aanroepen van service-principals worden niet geblokkeerd door voorwaardelijke toegang.
   * Als uw organisatie deze accounts in gebruik heeft in scripts of code, kunt u overwegen deze te vervangen door [beheerde identiteiten](../managed-identities-azure-resources/overview.md). Als tijdelijke oplossing kunt u deze specifieke accounts uitsluiten van het basislijn beleid.

## <a name="application-exclusions"></a>Toepassings uitsluitingen

Organisaties kunnen veel Cloud toepassingen in gebruik hebben. Niet al deze toepassingen kunnen een gelijke beveiliging vereisen. De salaris-en aanwezigheids toepassingen kunnen bijvoorbeeld MFA vereisen, maar de cafeteria is waarschijnlijk niet. Beheerders kunnen ervoor kiezen om specifieke toepassingen uit te sluiten van hun beleid.

## <a name="create-a-conditional-access-policy"></a>Beleid voor voorwaardelijke toegang maken

De volgende stappen helpen u bij het maken van beleid voor voorwaardelijke toegang, zodat alle gebruikers multi-factor Authentication kunnen uitvoeren.

1. Meld u aan bij de **Azure Portal** als globale beheerder, beveiligings beheerder of beheerder van de voorwaardelijke toegang.
1. Blader naar **Azure Active Directory** > **beveiligings** > **voorwaardelijke toegang**.
1. Selecteer **Nieuw beleid**.
1. Geef uw beleid een naam. Het is raadzaam dat organisaties een zinvolle norm maken voor de namen van hun beleid.
1. Onder **toewijzingen**selecteert u **gebruikers en groepen**
   1. Onder **insluiten**selecteert u **alle gebruikers**
   1. Onder **uitsluiten**selecteert u **gebruikers en groepen** en kiest u de accounts voor nood toegang of het afbreek glas van uw organisatie. 
   1. Selecteer **Done**.
1. Onder **Cloud-apps of acties** > **, selecteert**u **alle Cloud-apps**.
   1. Onder **uitsluiten**selecteert u toepassingen waarvoor multi-factor Authentication niet is vereist.
1. Onder **voor waarden** > **client-apps (preview)** stelt u **configureren** op **Ja**in en selecteert u **gereed**.
1. Onder **toegangs beheer** > **toekennen**selecteert u **toegang verlenen**, **multi-factor Authentication vereisen**en selecteert u **selecteren**.
1. Bevestig de instellingen en stel **beleid inschakelen** in **op aan**.
1. Selecteer **maken** om uw beleid in te stellen.

### <a name="named-locations"></a>Benoemde locaties

Organisaties kunnen ervoor kiezen om bekende netwerk locaties die bekend zijn als **benoemde locaties** , op te nemen in hun beleid voor voorwaardelijke toegang. Deze benoemde locaties kunnen vertrouwde IPv4-netwerken bevatten, zoals die voor een hoofd kantoor. Zie voor meer informatie over het configureren van benoemde locaties het artikel [Wat is de voor waarde voor de locatie in azure Active Directory voorwaardelijke toegang?](location-condition.md)

In het bovenstaande voor beeld-beleid kan een organisatie ervoor kiezen om geen multi-factor Authentication te vereisen als ze toegang hebben tot een Cloud-app vanuit hun bedrijfs netwerk. In dit geval kan de volgende configuratie aan het beleid worden toegevoegd:

1. Selecteer onder **toewijzingen**locaties **voor voor waarden** > **Locations**.
   1. Configureer **Ja**.
   1. **Een wille keurige locatie**bevatten.
   1. **Alle vertrouwde locaties**uitsluiten.
   1. Selecteer **Done**.
1. Selecteer **Done**.
1. **Sla** de wijzigingen in het beleid op.

## <a name="next-steps"></a>Volgende stappen

[Algemeen beleid voor voorwaardelijke toegang](concept-conditional-access-policy-common.md)

[Effect bepalen met de modus alleen rapport-alleen voor voorwaardelijke toegang](howto-conditional-access-report-only.md)

[Aanmeld gedrag simuleren met het What If hulp programma voor voorwaardelijke toegang](troubleshoot-conditional-access-what-if.md)
