---
title: Basislijn beleid blokkeert verouderde verificatie (preview)-Azure Active Directory
description: Beleid voor voorwaardelijke toegang om verouderde verificatie protocollen te blok keren
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 05/16/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: 413df402d5acc878130a47821efd5fa3a512257d
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2019
ms.locfileid: "68931940"
---
# <a name="baseline-policy-block-legacy-authentication-preview"></a>Basislijn beleid: Verouderde verificatie blok keren (preview-versie)

Om uw gebruikers eenvoudig toegang te geven tot uw Cloud-apps, ondersteunt Azure Active Directory (Azure AD) een breed scala aan verificatie protocollen, waaronder verouderde verificatie. Verouderde verificatie is een term die verwijst naar een verificatie aanvraag die wordt gedaan door:

* Oudere Office-clients die geen gebruik maken van moderne verificatie (bijvoorbeeld Office 2010 client)
* Elke client die gebruikmaakt van verouderde e-mail protocollen zoals IMAP/SMTP/POP3

De meeste pogingen om zich aan te melden, zijn tegenwoordig afkomstig van verouderde verificatie. Verouderde verificatie biedt geen ondersteuning voor multi-factor Authentication (MFA). Zelfs als er een MFA-beleid is ingeschakeld in uw directory, kan een ongeldige actor worden geverifieerd met behulp van een verouderd protocol en MFA overs Laan.

De beste manier om uw account te beschermen tegen kwaad aardige verificatie aanvragen door verouderde protocollen, is om deze pogingen te blok keren. Om het voor u gemakkelijker te maken om uw omgeving te beveiligen, hebben we dit basislijn beleid gemaakt om verouderde verificatie te blok keren.

**Verouderde verificatie blok keren** is een [basislijn beleid](concept-baseline-protection.md) waarmee alle verificatie aanvragen worden geblokkeerd die afkomstig zijn van verouderde protocollen. Moderne authenticatie moet worden gebruikt om u aan te melden voor alle gebruikers. Alle aanvragen die afkomstig zijn van verouderde protocollen, worden in combi natie met de andere basislijn beleidsregels geblokkeerd en alle gebruikers moeten telkens MFA gebruiken wanneer dat nodig is. Met dit beleid wordt Exchange ActiveSync niet geblokkeerd.

## <a name="identify-legacy-authentication-use"></a>Gebruik van verouderde authenticatie identificeren

Voordat u verouderde verificatie in uw Directory kunt blok keren, moet u eerst begrijpen of uw gebruikers apps hebben die gebruikmaken van verouderde verificatie en hoe dit van invloed is op uw algemene Directory. Aanmeld logboeken van Azure AD kunnen worden gebruikt om te begrijpen of u gebruikmaakt van verouderde verificatie.

1. Navigeer naar het **Azure Portal** >  **-** **Azure Active Directory** > aanmeldingen.
1. Voeg de kolom client toepassing toe als deze niet wordt weer gegeven door > te klikken op de**client-app**columns.
1. Filters > toevoegen**client-app** > Selecteer alle opties voor **andere clients** en klik op **Toep assen**.

Bij filteren worden alleen de aanmeldings pogingen weer gegeven die zijn gemaakt door verouderde verificatie protocollen. Als u op elke afzonderlijke aanmeldings poging klikt, wordt er meer informatie weer gegeven. In het veld **client-app** onder het tabblad **basis informatie** wordt aangegeven welk verouderde verificatie protocol is gebruikt.

In deze logboeken wordt aangegeven welke gebruikers nog steeds afhankelijk zijn van verouderde verificatie en welke toepassingen verouderde protocollen gebruiken om verificatie aanvragen uit te voeren. Voor gebruikers die niet in deze logboeken worden weer gegeven en worden bevestigd dat ze geen verouderde verificatie gebruiken, implementeert u een beleid voor voorwaardelijke toegang of schakelt u het **basislijn beleid in: verouderde verificatie blok keren** voor deze gebruikers.

## <a name="moving-away-from-legacy-authentication"></a>Van verouderde authenticatie weggooien

Zodra u een beter idee hebt van wie verouderde verificatie in uw Directory gebruikt en welke toepassingen hiervan afhankelijk zijn, voert de volgende stap uw gebruikers bij om moderne verificatie te gebruiken. Moderne verificatie is een methode van identiteits beheer die veiliger gebruikers verificatie en-autorisatie biedt. Als er een MFA-beleid aanwezig is in uw directory, zorgt moderne verificatie ervoor dat de gebruiker wanneer dat nodig is, wordt gevraagd MFA op te vragen. Het is het veiligere alternatief voor verouderde verificatie protocollen.

Deze sectie bevat een stapsgewijze overzicht van het bijwerken van uw omgeving naar moderne verificatie. Lees de onderstaande stappen voordat u een verouderde verificatie blokkerings beleid inschakelt in uw organisatie.

### <a name="step-1-enable-modern-authentication-in-your-directory"></a>Stap 1: Moderne verificatie inschakelen in uw Directory

De eerste stap bij het inschakelen van moderne verificatie is dat uw Directory ondersteuning biedt voor moderne verificatie. Moderne authenticatie is standaard ingeschakeld voor mappen die zijn gemaakt op of na 1 augustus 2017. Als uw map vóór deze datum is gemaakt, moet u de volgende stappen gebruiken om moderne verificatie voor uw directory hand matig in te scha kelen:

1. Controleer of uw directory al ondersteuning biedt voor moderne verificatie door uit `Get-CsOAuthConfiguration` te voeren vanuit de [Skype voor bedrijven online Power shell-module](https://docs.microsoft.com/office365/enterprise/powershell/manage-skype-for-business-online-with-office-365-powershell).
1. Als uw opdracht een lege `OAuthServers` eigenschap retourneert, wordt moderne verificatie uitgeschakeld. Werk de instelling bij om moderne authenticatie `Set-CsOAuthConfiguration`in te scha kelen met. Als uw `OAuthServers` eigenschap een vermelding bevat, bent u klaar om te gaan.

Zorg ervoor dat u deze stap uitvoert voordat u verdergaat. Het is essentieel dat uw Directory configuraties eerst worden gewijzigd omdat ze bepalen welk protocol door alle Office-clients wordt gebruikt. Zelfs als u Office-clients gebruikt die moderne authenticatie ondersteunen, wordt standaard verouderde protocollen gebruikt als moderne verificatie is uitgeschakeld voor uw Directory.

### <a name="step-2-office-applications"></a>Stap 2: Office-toepassingen

Zodra u moderne verificatie hebt ingeschakeld in uw directory, kunt u beginnen met het bijwerken van toepassingen door moderne verificatie voor Office-clients in te scha kelen. Clients met Office 2016 of hoger ondersteunen standaard moderne verificatie. Er zijn geen extra stappen vereist.

Als u Office 2013 Windows-clients of ouder gebruikt, raden we u aan om een upgrade uit te voeren naar Office 2016 of hoger. Zelfs na het volt ooien van de vorige stap voor het inschakelen van moderne verificatie in uw directory, blijven de oudere Office-toepassingen gebruikmaken van verouderde verificatie protocollen. Als u Office 2013-clients gebruikt en niet onmiddellijk een upgrade kunt uitvoeren naar Office 2016 of hoger, volgt u de stappen in het volgende artikel om [moderne authenticatie voor Office 2013 op Windows-apparaten in te scha kelen](https://docs.microsoft.com/office365/admin/security-and-compliance/enable-modern-authentication). Als u uw account wilt beschermen terwijl u verouderde verificatie gebruikt, raden we u aan om sterke wacht woorden te gebruiken in uw Directory. Bekijk [Azure AD-wachtwoord beveiliging](../authentication/concept-password-ban-bad.md) om zwakke wacht woorden in uw directory te verzwakken.

Office 2010 biedt geen ondersteuning voor moderne verificatie. U moet alle gebruikers met Office 2010 upgraden naar een recentere versie van Office. U wordt aangeraden om een upgrade uit te voeren naar Office 2016 of hoger, omdat deze standaard verouderde verificatie blokkeert.

Als u MacOS gebruikt, raden we u aan om een upgrade uit te voeren naar Office voor Mac 2016 of hoger. Als u de systeem eigen e-mailclient gebruikt, moet MacOS versie 10,14 of hoger op alle apparaten zijn geïnstalleerd.

### <a name="step-3-exchange-and-sharepoint"></a>Stap 3: Exchange en share point

Voor Windows-clients die gebruikmaken van moderne authenticatie, moet Exchange Online ook moderne authenticatie zijn ingeschakeld. Als moderne verificatie is uitgeschakeld voor Exchange Online, worden op Windows gebaseerde Outlook-clients die ondersteuning bieden voor moderne verificatie (Outlook 2013 of hoger) basis verificatie gebruikt om verbinding te maken met Exchange Online-post vakken.

Share point online is ingeschakeld voor de standaard instelling voor moderne verificatie. Voor directory's die na 1 augustus 2017 zijn gemaakt, is moderne verificatie standaard ingeschakeld in Exchange Online. Als u echter eerder moderne authenticatie had uitgeschakeld of als u een map gebruikt die vóór deze datum is gemaakt, volgt u de stappen in het volgende artikel om [moderne verificatie in te scha kelen in Exchange Online](https://docs.microsoft.com/exchange/clients-and-mobile-in-exchange-online/enable-or-disable-modern-authentication-in-exchange-online).

### <a name="step-4-skype-for-business"></a>Stap 4: Skype voor Bedrijven

Als u verouderde verificatie aanvragen van Skype voor bedrijven wilt voor komen, moet u moderne verificatie inschakelen voor Skype voor bedrijven online. Voor mappen die zijn gemaakt na 1 augustus 2017 is moderne verificatie voor Skype voor bedrijven standaard ingeschakeld.

We raden u aan om over te stappen op micro soft-teams. Dit biedt standaard ondersteuning voor moderne verificatie. Als u op dit moment echter niet kunt migreren, moet u moderne verificatie inschakelen voor Skype voor bedrijven online, zodat Skype voor bedrijven-clients moderne verificatie gaan gebruiken. Volg de stappen in dit artikel [Skype voor bedrijven-topologieën die worden ondersteund door moderne verificatie](https://docs.microsoft.com/skypeforbusiness/plan-your-deployment/modern-authentication/topologies-supported), om moderne verificatie mogelijk te maken voor Skype voor bedrijven.

Naast het inschakelen van moderne verificatie voor Skype voor bedrijven online, raden we u aan moderne verificatie voor Exchange Online in te scha kelen bij het inschakelen van moderne verificatie voor Skype voor bedrijven. Met dit proces kunt u de status van moderne verificatie in Exchange Online en Skype voor bedrijven online synchroniseren en voor komen dat er meerdere aanmeldings prompts voor Skype voor bedrijven-clients worden verzonden.

### <a name="step-5-using-mobile-devices"></a>Stap 5: Mobiele apparaten gebruiken

Toepassingen op uw mobiele apparaat moeten ook verouderde verificatie blok keren. U kunt het beste Outlook voor Mobile gebruiken. Outlook voor mobiel biedt standaard ondersteuning voor moderne verificatie en voldoet aan andere MFA-beleids regels voor basis beveiliging.

Als u de systeem eigen iOS-e-mailclient wilt gebruiken, moet u iOS versie 11,0 of hoger uitvoeren om ervoor te zorgen dat de e-mailclient is bijgewerkt om verouderde verificatie te blok keren.

### <a name="step-6-on-premises-clients"></a>Stap 6: On-premises clients

Als u een hybride klant bent die gebruikmaakt van Exchange Server on-premises en Skype voor bedrijven on-premises, moeten beide services worden bijgewerkt om moderne verificatie mogelijk te maken. Wanneer u moderne verificatie gebruikt in een hybride omgeving, verifieert u gebruikers on-premises. Het verhaal dat hun toegang tot resources (bestanden of e-mails) wijzigt.

Voordat u on-premises moderne verificatie kunt gaan inschakelen, moet u ervoor zorgen dat u aan de vereisten voldoet.
U bent nu klaar om on-premises moderne authenticatie in te scha kelen.

De stappen voor het inschakelen van moderne verificatie vindt u in de volgende artikelen:

* [Exchange Server on-premises configureren voor het gebruik van hybride, moderne authenticatie](https://docs.microsoft.com/office365/enterprise/configure-exchange-server-for-hybrid-modern-authentication)
* [Moderne verificatie (ADAL) gebruiken met Skype voor bedrijven](https://docs.microsoft.com/skypeforbusiness/manage/authentication/use-adal)

## <a name="enable-the-baseline-policy"></a>Het basislijn beleid inschakelen

Beleid voor **beleids basislijn: Verouderde Authenticatie blok** keren (preview) wordt vooraf geconfigureerd en wordt bovenaan weer gegeven wanneer u navigeert naar de Blade voorwaardelijke toegang in azure Portal. Deze instelling wordt pas van kracht na een geslaagde aanmelding, zodat gebruikers nog steeds met de optie verouderde verificatie kunnen proberen.

Om dit beleid in te scha kelen en uw organisatie te beschermen:

1. Meld u aan bij de **Azure Portal** als globale beheerder, beveiligings beheerder of beheerder van de voorwaardelijke toegang.
1. Blader naar **Azure Active Directory** > **voorwaardelijke toegang**.
1. Selecteer **basislijn beleid in de lijst met beleids regels: Verouderde verificatie (preview**-versie) blok keren.
1. Stel **beleid inschakelen** om **beleid direct te gebruiken**in.
1. Klik op **Opslaan**.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie:

* [Basis beveiligings beleid voor voorwaardelijke toegang](concept-baseline-protection.md)
* [Vijf stappen voor het beveiligen van uw identiteits infrastructuur](../../security/fundamentals/steps-secure-identity.md)
* [Wat is voorwaardelijke toegang in Azure Active Directory?](overview.md)
