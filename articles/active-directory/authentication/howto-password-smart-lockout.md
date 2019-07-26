---
title: Beveiligings aanvallen met Azure AD Smart-vergren deling voor komen Azure Active Directory
description: Azure Active Directory slim vergren delen helpt uw organisatie te beschermen tegen aanvallen waarbij wordt geprobeerd om wacht woorden te raden
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/25/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: bd03e2b98b1fd1a2a45b5feecc963bcfc7bfe83c
ms.sourcegitcommit: a0b37e18b8823025e64427c26fae9fb7a3fe355a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/25/2019
ms.locfileid: "68499881"
---
# <a name="azure-active-directory-smart-lockout"></a>Azure Active Directory slim vergren delen

Slimme vergren deling helpt bij het vergren delen van ongeldige Actors die de wacht woorden van uw gebruikers proberen te raden of methoden voor het afwijzen van brute kracht gebruiken om aan de slag te gaan. Het kan aanmeldingen herkennen die afkomstig zijn van geldige gebruikers en ze anders behandelen dan gebruikers van aanvallers en andere onbekende bronnen. Slimme vergren delingen blokkeert de aanvallers, terwijl uw gebruikers toegang krijgen tot hun accounts en productief kunnen zijn.

Standaard vergrendelt Smart Lock het account van aanmeldings pogingen voor één minuut na 10 mislukte pogingen. Het account vergrendelt na elke volgende mislukte aanmeldings poging, gedurende één minuut om de eerste keer en langer bij de volgende pogingen.

Slimme vergren deling houdt de laatste drie ongeldige hashes met een onjuist wacht woord bij om te voor komen dat de vergrendelings teller voor hetzelfde wacht woord wordt verhoogd. Als iemand hetzelfde onjuiste wacht woord meermaals opgeeft, wordt het account niet vergrendeld.

 > [!NOTE]
 > De functie voor het bijhouden van hashes is niet beschikbaar voor klanten waarbij Pass-Through-verificatie is ingeschakeld omdat verificatie on-premises plaatsvindt, niet in de Cloud.

Federatieve implementaties met AD FS 2016 en AF FS 2019 kunnen vergelijk bare voor delen bieden met behulp van [AD FS extranet en extranet Smart-vergren deling](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-ad-fs-extranet-smart-lockout-protection).

Slimme vergren deling is altijd ingeschakeld voor alle klanten van Azure AD met deze standaard instellingen die de juiste combi natie van beveiliging en bruikbaarheid bieden. Aanpassing van de instellingen voor slim vergren delen, met waarden die specifiek zijn voor uw organisatie, vereist Azure AD Basic of hogere licenties voor uw gebruikers.

Het gebruik van slimme vergren deling garandeert niet dat een authentiekte gebruiker nooit wordt vergrendeld. Wanneer Slim vergren delen een gebruikers account vergrendelt, proberen we ons het beste om de legitieme gebruiker te vergren delen. De vergrendelings service probeert ervoor te zorgen dat ongeldige actors geen toegang krijgen tot een authentiek gebruikers account.  

* Elk Azure Active Directory Data Center volgt de vergren deling onafhankelijk. Een gebruiker heeft het aantal pogingen (threshold_limit * datacenter_count), als de gebruiker aan elk Data Center komt.
* Slimme vergren deling maakt gebruik van de bekende locatie versus onbekende locatie om onderscheid te maken tussen een ongeldige actor en de legitieme gebruiker. Onbekende en bekende locaties hebben beide afzonderlijke vergrendelings tellers.

Slimme vergren deling kan worden geïntegreerd met hybride implementaties, met behulp van hash-synchronisatie met wacht woord of Pass Through-verificatie om on-premises Active Directory accounts te beveiligen tegen uitsluiting door aanvallers. Door slim vergrendelings beleid in azure AD op de juiste wijze in te stellen, kunnen aanvallen worden gefilterd voordat ze on-premises Active Directory bereiken.

Wanneer [Pass-Through-verificatie](../hybrid/how-to-connect-pta.md)wordt gebruikt, moet u het volgende controleren:

* De drempel waarde voor vergren delingen van Azure AD is **lager** dan de drempel waarde voor het vergren delen van Active Directory Stel de waarden zo in dat de drempel waarde voor het vergren delen van Active Directory-account ten minste twee of drie keer langer duurt dan de drempel waarde voor vergren deling van Azure AD. 
* De Azure AD-vergrendelings duur moet langer worden ingesteld dan de Active Directory account vergrendelings teller opnieuw instellen na de duur. Houd er rekening mee dat de duur van Azure AD wordt ingesteld in seconden, terwijl de duur van de advertentie binnen enkele minuten is ingesteld. 

Als u bijvoorbeeld wilt dat uw Azure AD-teller hoger is dan AD, zou Azure AD 120 seconden (2 minuten) zijn, terwijl uw on-premises AD is ingesteld op 1 minuut (60 seconden).

> [!IMPORTANT]
> Het is momenteel niet mogelijk om de Cloud accounts van gebruikers door de beheerder te ontgrendelen als ze zijn vergrendeld door de mogelijkheden van slim vergren delen. De beheerder moet wachten tot de vergrendelings duur is verlopen.

## <a name="verify-on-premises-account-lockout-policy"></a>On-premises account vergrendelings beleid controleren

Gebruik de volgende instructies om uw on-premises Active Directory account vergrendelings beleid te controleren:

1. Open het groepsbeleid-beheer programma.
2. Bewerk het groeps beleid dat het account vergrendelings beleid van uw organisatie bevat, bijvoorbeeld het **standaard domein beleid**.
3. Bladeren naar **computer configuratie** > **beleid** > **Windows-instellingen** > **beveiligings instellingen** > **account**beleidaccount > **vergren delen Beleid**.
4. Controleer de **drempel waarde voor account vergrendeling** en **Stel de teller voor account vergrendeling na waarden opnieuw** in.

![Het vergrendelings beleid voor on-premises Active Directory-account wijzigen](./media/howto-password-smart-lockout/active-directory-on-premises-account-lockout-policy.png)

## <a name="manage-azure-ad-smart-lockout-values"></a>Slimme vergrendelings waarden voor Azure AD beheren

Op basis van de vereisten van uw organisatie moeten slimme vergrendelings waarden mogelijk worden aangepast. Aanpassing van de instellingen voor slim vergren delen, met waarden die specifiek zijn voor uw organisatie, vereist Azure AD Basic of hogere licenties voor uw gebruikers.

Voer de volgende stappen uit om de slimme vergrendelings waarden voor uw organisatie te controleren of te wijzigen:

1. Meld u aan bij de [Azure Portal](https://portal.azure.com)en klik op **Azure Active Directory**en vervolgens op **verificatie methoden**.
1. Stel de **drempel waarde voor vergren deling**in op basis van het aantal mislukte aanmeldingen dat is toegestaan voor een account voordat het voor het eerst wordt vergrendeld. De standaard waarde is 10.
1. Stel de **vergrendelings duur in seconden in**, tot de lengte in seconden van elke vergren deling. De standaard waarde is 60 seconden (een minuut).

> [!NOTE]
> Als de eerste aanmelding na een vergren deling ook mislukt, wordt het account opnieuw vergrendeld. Als een account herhaaldelijk wordt vergrendeld, neemt de vergrendelings duur toe.

![Pas het beleid voor slimme vergren deling van Azure AD aan in het Azure Portal](./media/howto-password-smart-lockout/azure-active-directory-custom-smart-lockout-policy.png)

## <a name="how-to-determine-if-the-smart-lockout-feature-is-working-or-not"></a>Bepalen of de functie voor slim vergren delen werkt

Wanneer de drempel waarde voor slim vergren delen wordt geactiveerd, wordt het volgende bericht weer gegeven wanneer het account is vergrendeld:

**Uw account is tijdelijk vergrendeld om onbevoegd gebruik te voor komen. Probeer het later opnieuw. Neem contact op met de beheerder als u nog steeds problemen ondervindt.**

## <a name="next-steps"></a>Volgende stappen

* [Ontdek hoe u in uw organisatie een onjuist wacht woord kunt gebruiken met Azure AD.](howto-password-ban-bad.md)
* [Configureer selfservice voor wachtwoord herstel zodat gebruikers hun eigen accounts kunnen ontgrendelen.](quickstart-sspr.md)
