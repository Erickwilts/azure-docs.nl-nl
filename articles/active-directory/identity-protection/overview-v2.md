---
title: Wat is Azure Active Directory Identity Protection (vernieuwd)? | Microsoft Docs
description: Wat is Azure Active Directory Identity Protection (vernieuwd)?
services: active-directory
keywords: Azure active directory identity protection cloud app discovery, toepassingen, beveiliging, risico's, risiconiveau, beveiligingsproblemen, beveiligingsbeleid beheren
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: mtillman
ms.assetid: e7434eeb-4e98-4b6b-a895-b5598a6cccf1
ms.service: active-directory
ms.subservice: identity-protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/03/2018
ms.author: joflore
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2b4f8caf03aad339cea3c3fcc732fc1af6086ea7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67108889"
---
# <a name="what-is-azure-active-directory-identity-protection-refreshed"></a>Wat is Azure Active Directory Identity Protection (vernieuwd)?

De ervaring Identity Protection is vernieuwd om identiteiten in uw organisatie beter te beschermen. Deze vernieuwde ervaring biedt:

- Opnieuw ontworpen beheerervaring die om het meest relevant is voor you¬ gebruikersrisico en aanmeldingsrisico risico draait

- Krachtige onderzoeken ervaring met ondersteuning voor het filteren, sorteren en slimme downloaden

- Verbeterde risicoberekening gebruiker kunt u uw inspanningen voor gebruikers die zijn ondergebracht worden aangetast prioriteit

- Nieuwe API-ondersteuning om in te schakelen programmatische toegang tot gegevens van de risico 's

- Vereenvoudigd beheer Feedbackproces waarmee u uw gebruikers direct beveiligen

- Nieuwe beheerde machine learning voor het verbeteren van de nauwkeurigheid van de risico's te beoordelen



Beveiliging is vandaag nog een belangrijkste aandachtspunt voor organisaties. Het merendeel van de beveiliging schendingen nemen plaats wanneer de toegang tot een omgeving achter de deur krijgen door het stelen van de identiteit van een gebruiker. In de afgelopen jaren, zijn aanvallers steeds meer in gebruik te maken van inbreuken op de derde partij en het gebruik van geavanceerde phishing-aanvallen van kracht geworden. Als aanvallers toegang nog van gebruiker met laag bevoegde accounts krijgen, is het relatief gemakkelijk voor hen toegang te krijgen tot belangrijke bedrijfsbronnen via een laterale verplaatsing. 

Om te reageren op deze bedreigingen, dankzij u Azure AD Identity Protection: 

- Proactief te voorkomen dat verdachte identiteiten worden misbruikt 

- Automatisch risico wanneer er verdachte activiteit wordt gedetecteerd 

- Riskante gebruikers en aanmeldingen voor potentiële beveiligingsproblemen adres onderzoeken  

- Worden gewaarschuwd wanneer het risico van een gebruiker een opgegeven drempelwaarde bereikt 

 

Azure AD Identity Protection is een functie van Azure Active Directory Premium P2 waarmee u kunt beleid configureren voor het automatisch reageren wanneer de identiteit van een gebruiker ermee is geknoeid of wanneer iemand anders dan de eigenaar van het account probeert aan te melden met hun de identiteit. Deze beleidsregels, naast andere besturingselementen voor voorwaardelijke toegang is geleverd door Azure AD kunnen ofwel automatisch blokkeren van toegang of initiëren risicobeperking acties, zoals wachtwoord opnieuw instellen of multi-factor authentication afdwingen. Identity Protection biedt ook mogelijkheden voor bewaking en rapportage voor beter inzicht krijgen in risico en de mogelijke inbreuk in uw organisatie. 

>[!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RWsS6Q]


## <a name="risk-events"></a>Risicogebeurtenissen

Azure AD Identity Protection detecteert de volgende risicogebeurtenissen: 

 

| Type risicogebeurtenis | Description | Detectietype |
| ---             | ---         | ---            |
| Ongewone reizen | Zich aanmelden vanaf een ongewone locatie op basis van de gebruiker recente aanmeldingen. | Offline |
| Anonieme IP-adres | Zich aanmelden vanaf een anoniem IP-adres (bijvoorbeeld: Tor browser, anonymizer VPN's). | Realtime |
| Onbekende eigenschappen voor aanmelden | Meld u aan met eigenschappen die we niet hebben gezien onlangs voor de opgegeven gebruiker. | Realtime |
| Malware gekoppelde IP-adres | Zich aanmelden vanaf een IP-adres van schadelijke software gekoppeld | Offline |
| Gelekte referenties | Deze risicogebeurtenis geeft aan dat van de gebruiker geldige referenties zijn gelekt | Offline |





## <a name="types-of-risk"></a>Typen van de risico 's 

Identity Protection is gebaseerd op twee soorten risico's:

- Aanmeldingsrisico

- Gebruikersrisico

### <a name="sign-in-risk"></a>Aanmeldingsrisico

Een aanmeldingsrisico vertegenwoordigt de kans dat een bepaalde verificatieaanvraag is niet geautoriseerd door de eigenaar van de identiteit.

Er zijn twee evaluaties van aanmeldingsrisico: 

- **Aanmeldingsrisico (realtime)** -het aanmeldingsrisico (realtime) is gebaseerd op de realtime detecties waarvoor tijdens de verwerking van de aanmelding is geactiveerd.  

- **Aanmeldingsrisico (totaal)** -het risico aanmelden (totaal), is het totale risico van een aanmelding. Dit wordt berekend door een machine learning-model dat rekening gehouden met:

    - Realtime detecties (hierboven beschreven)
    
    - Offline detecties (die worden geactiveerd nadat de aanmelding heeft plaatsgevonden) 
    
    - Alle andere functies van de aanmelding


### <a name="user-risk"></a>Gebruikersrisico

Een gebruikersrisico vertegenwoordigt de kans dat een opgegeven identiteit is geknoeid. 

Het gebruikersrisico wordt berekend op basis van alle risico's die zijn gekoppeld aan de gebruiker:

- Alle riskante aanmeldingen
- Alle risicogebeurtenissen zijn niet gekoppeld aan een aanmelding
- De huidige gebruikersrisico
- Risico's herstel of ontslag acties wordt uitgevoerd op de gebruiker tot heden



## <a name="how-identity-protection-detects-risk"></a>Hoe Identity Protection risico gedetecteerd  

Azure AD gebruikt machine learning voor het detecteren van afwijkingen en verdachte activiteiten, met behulp van beide signalen gedetecteerd in realtime tijdens aanmeldingen ook als niet-realtime signalen die betrekking hebben op gebruikers en hun activiteiten aanmelden. Met deze gegevens, berekent Identity Protection een realtime aanmeldingsrisico telkens wanneer die een gebruiker zich verifieert, evenals een algemene gebruiker risiconiveau voor de gebruiker bereik bepalen. Identity Protection kunt u automatisch actie ondernemen voor deze detecties risico's door configureren Identity Protection gebruikersrisico en Aanmeldingsrisico beleid.  

 

Om te begrijpen hoe Identity Protection risico detecteert, er zijn twee belangrijke concepten: gebruikersrisico en aanmeldingsrisico. Aanmeldingsrisico weerspiegelt de kans dat een bepaalde verificatieaanvraag is niet geautoriseerd door de eigenaar van de identiteit. Er zijn twee soorten aanmelden risico's: realtime en de totale. Realtime aanmeldingsrisico wordt gedetecteerd op het moment van de opgegeven aanmeldingspoging (zoals aanmeldingen vanaf anonieme IP-adressen). Totaal aantal aanmeldingsrisico wordt de statistische functie van gedetecteerde realtime aanmelding risico's ook alle volgende real time-risicogebeurtenissen die zijn gekoppeld aan van de gebruiker aanmeldingen (zoals onmogelijk traject). Gebruikersrisico weerspiegelt de algehele kans dat een actor slecht een opgegeven identiteit is geknoeid. Gebruikersrisico bevat alle activiteiten die de risico's voor een bepaalde gebruiker, met inbegrip van:

- Realtime aanmeldingsrisico
- Volgende aanmeldingsrisico
- Riskante gebruiker detecties.   

 

 
 ![Stroom](./media/overview-v2/01.png)
 

 

De basislijn-stroom voor de risicodetectie Identity Protection en de reactietijd van een bepaalde aanmelding worden samengevat in de bovenstaande afbeelding.  

 

 

 

## <a name="common-scenarios"></a>Algemene scenario's 

Bekijk het voorbeeld van een werknemer van Contoso. 

1. Een werknemer wil zich aanmelden bij Exchange Online in de Tor-browser. Op het moment van aanmelding vaststelt Azure AD dat realtime risicogebeurtenissen. 

2. Azure AD detecteert dat de werknemer vanaf een anoniem IP-adres aanmeldt zich, een risiconiveau voor middelgrote aanmelding wordt geactiveerd. 

3. De werknemer is sprake van een MFA-prompt, omdat de IT-beheerder van Contoso is geconfigureerd de Identity Protection aanmeldingsrisico beleid voor voorwaardelijke toegang. Het beleid vereist MFA voor een aanmeldingsrisico van gemiddeld of hoger. 

4. De werknemer geeft de MFA-prompt en heeft toegang tot Exchange Online en het risiconiveau van de gebruiker is niet gewijzigd. 

Wat is er gebeurd achter de schermen? Een realtime aanmeldingsrisico de aanmeldingspoging vanuit de browser Tor geactiveerd in Azure AD voor anonieme IP-adres. Als de aanvraag wordt verwerkt door Azure AD, wordt het aanmeldingsrisico-beleid geconfigureerd in Identity Protection, omdat het niveau van aanmeldingsrisico van de werknemer is voldaan aan de drempelwaarde (gemiddeld) toegepast. Omdat de werknemer eerder was geregistreerd voor MFA, zijn ze kunnen reageren op en geeft de MFA-controle. De mogelijkheid om door te geven is de MFA-controle een signaal ontvangen naar Azure AD dat ze waarschijnlijk de eigenaar van de geldige identiteit zijn en hun risiconiveau van de gebruiker niet stijgt. 


Maar wat gebeurt er als de werknemer is niet een probeert aan te melden? 

1. Een kwaadwillende actor met referenties van de werknemer wil zich aanmelden bij hun Exchange Online-account vanuit de browser Tor omdat ze willen verbergen van hun IP-adres. 

2. Azure AD detecteert dat de aanmeldingspoging is van een anoniem IP-adres, een realtime aanmeldingsrisico activeren. 

3. De schadelijke actor is sprake van een MFA-prompt, omdat de IT-beheerder van Contoso is geconfigureerd de Identity Protection aanmelden risico beleid voor voorwaardelijke toegang om MFA te vereisen wanneer het aanmeldingsrisico is normaal of hoger. 

4. De schadelijke actor de MFA-controle is mislukt en heeft geen toegang tot Exchange Online-account van de werknemer. 

5. De mislukte MFA prompt geactiveerd risicogebeurtenis moeten worden vastgelegd, verhogen van de gebruiker risico's voor toekomstige aanmeldingen. 

Nu dat een kwaadwillende actor toegang probeert te krijgen van Sarah account, laten we zien wat er gebeurt met de volgende keer dat de werknemer probeert aan te melden. 

1. De werknemer wil zich aanmelden bij Exchange Online via Outlook. Op het moment van aanmelding detecteert Azure AD realtime risicogebeurtenissen, evenals alle voorgaande gebruikersrisico. 

2. Azure AD een realtime aanmeldingsrisico niet wordt gedetecteerd, maar hoog gebruikersrisico vanwege de afgelopen riskante activiteit wordt gedetecteerd in de vorige scenario's.  

3. De werknemer is sprake van een prompt voor wachtwoord opnieuw instellen, omdat Contoso IT-beheerder is geconfigureerd de beleid voor gebruikersrisico's Identity Protection als u wilt wijzigen wachtwoord vereisen wanneer een gebruiker met een hoog risico zich aanmeldt. 

4. Omdat de werknemer is geregistreerd voor SSPR en MFA, ze hun wachtwoord is opnieuw instellen. 

5. Referenties van de werknemer niet meer worden aangetast door hun wachtwoord opnieuw instellen van wachtwoorden en hun identiteit teruggezet naar een veilige status. 

6. Van de werknemer vorige risicogebeurtenissen zijn opgelost en haar risiconiveau van de gebruiker wordt automatisch opnieuw worden ingesteld als reactie op inbreuk op de referenties beperkende. 

## <a name="how-do-i-configure-identity-protection"></a>Hoe configureer ik Identity Protection? 

Als u wilt aan de slag met Identity Protection, moet u eerst een beleid voor gebruikersrisico's en een beleid voor aanmeldingsrisico configureren. Zodra deze beleidsregels zijn geconfigureerd en op een testgroep toegepast, kunt u risicogebeurtenissen voor meer informatie over hoe Identity Protection reageert in uw omgeving te simuleren. De onderstaande snelstartgids handleidingen bieden een overzicht over het instellen van de hiervoor genoemde beleidsregels en testen in uw omgeving. 

 

Identity Protection ondersteunt 3 rollen in Azure AD om gelijk te verdelen van de management-activiteiten rond uw implementatie: 

| Rol | Kan doen | Niet mogelijk is |
| --- | --- | --- |
| Globale beheerder | Volledige toegang tot Identity Protection, onboarding Identity Protection | |
| Beveiligingsbeheerder | Volledige toegang tot Identity Protection | Onboarding Identity Protection, wachtwoorden opnieuw instellen voor een gebruiker |
| Beveiligingslezer | Alleen-lezen toegang tot Identity Protection | Onboarding Identity Protection, gebruikers herstellen, configureren van beleid, wachtwoorden opnieuw instellen| 

Zie voor meer informatie, [beheerdersrollen toewijzen in Azure Active Directory](../users-groups-roles/directory-assign-admin-roles.md)

 
## <a name="licensing"></a>Licentieverlening

>[!NOTE]
> Tijdens de openbare preview van identiteitsbeveiliging (vernieuwd), wordt alleen de Azure AD Premium P2-klanten hebben toegang tot het rapport riskante gebruikers en het rapport riskante aanmeldingen.



| Mogelijkheid | Azure AD Premium P2 | Azure AD Premium P1 | Azure AD Basic/gratis |
| --- | --- | --- | --- |
| Beleid voor gebruikersrisico 's | Ja | Nee | Nee |
| Beleid voor aanmeldingsrisico | Ja | Nee | Nee |
| Rapport riskante gebruikers | Volledige toegang | Beperkte informatie | Beperkte informatie |
| Rapport Riskante aanmeldingen | Volledige toegang | Beperkte informatie | Beperkte informatie |
| MFA-registratiebeleid | Ja | Nee | Nee |







## <a name="next-steps"></a>Volgende stappen 

Als u wilt aan de slag met Identity Protection, Zie [beleid voor aanmeldingsrisico configureren](quickstart-sign-in-risk-policy.md). 






