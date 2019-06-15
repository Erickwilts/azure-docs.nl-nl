---
title: Azure Active Directory-risicogebeurtenissen | Microsoft Docs
description: Deze artice biedt u een gedetailleerd overzicht van wat risicogebeurtenissen zijn.
services: active-directory
keywords: Azure active directory identity protection, beveiliging, risico's, risiconiveau, beveiligingsproblemen, beveiligingsbeleid
author: MarkusVi
manager: daveba
ms.assetid: fa2c8b51-d43d-4349-8308-97e87665400b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: e50cc4ca9c98cb6a8e0f19cfcf6c1f86f1949beb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67107684"
---
# <a name="azure-active-directory-risk-events"></a>Azure Active Directory-risicogebeurtenissen

De meeste van de beveiliging terugdringen plaatsvinden als aanvallers toegang krijgen tot een omgeving door het stelen van de identiteit van een gebruiker. Verdachte identiteiten detecteren, is geen eenvoudige taak. Azure Active Directory maakt gebruik van geavanceerde machine learning-algoritmen en methodieken voor het detecteren van verdachte activiteit die is gekoppeld aan uw gebruikersaccounts. Elke gedetecteerde verdachte actie wordt opgeslagen in een record met de naam een **risicogebeurtenis**.

Er zijn twee plaatsen waar u de gemelde risico bekijken:

 - **Azure AD-rapportage** -risicogebeurtenissen zijn onderdeel van Azure AD-beveiligingsgroep rapporten. Zie voor meer informatie de [gebruikers lopen risico beveiligingsrapport](concept-user-at-risk.md) en de [riskante aanmeldingen beveiligingsrapport](concept-risky-sign-ins.md).

 - **Azure AD Identity Protection** -risicogebeurtenissen zijn ook deel uit van de rapportagemogelijkheden van [Azure Active Directory Identity Protection](../active-directory-identityprotection.md).

Bovendien kunt u de [risicogebeurtenissen Identity Protection API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/identityriskevent) programmatische toegang tot beveiligingsdetecties met behulp van Microsoft Graph. Zie voor meer informatie, [aan de slag met Azure Active Directory Identity Protection en Microsoft Graph](../identity-protection/graph-get-started.md). 

Azure Active Directory detecteert momenteel zes typen risicogebeurtenissen:

- [Gebruikers met de referenties zijn gelekt](#leaked-credentials) 
- [Aanmeldingen vanaf anonieme IP-adressen](#sign-ins-from-anonymous-ip-addresses) 
- [Onmogelijke reis naar ongewone locaties](#impossible-travel-to-atypical-locations) 
- [Aanmeldingen vanaf geïnfecteerde apparaten](#sign-ins-from-infected-devices) 
- [Aanmeldingen vanaf IP-adressen met verdachte activiteiten](#sign-ins-from-ip-addresses-with-suspicious-activity) 
- [Aanmeldingen vanaf onbekende locaties](#sign-in-from-unfamiliar-locations) 

![Risicogebeurtenis](./media/concept-risk-events/91.png)

> [!IMPORTANT]
> Soms wellicht vindt u een risicogebeurtenis zonder een bijbehorende aanmelding vermelding in de [aanmeldingenrapport](concept-sign-ins.md). Dit komt doordat Identity Protection risico's voor zowel evalueert **interactieve** en **niet-interactieve** aanmeldingen, terwijl de aanmeldingen-lijst alleen de interactieve aanmeldingen bevat.

Het inzicht u voor een gedetecteerde risicogebeurtenis krijgt is gekoppeld aan uw Azure AD-abonnement. 

* Met de **Azure AD Premium P2-editie**, krijgt u de meest gedetailleerde informatie over alle onderliggende detecties. 
* Met de **editie van Azure AD Premium P1**, geavanceerde detectie (zoals onbekende aanmelden eigenschappen) zijn niet inbegrepen bij uw licentie en wordt weergegeven onder de naam **aanmelden met extra risico gedetecteerd** . Bovendien worden de risico's detailvelden en risiconiveau verborgen.

Tijdens de detectie van risicogebeurtenissen al een belangrijk aspect vertegenwoordigt van de beveiliging van uw identiteiten, hebt u ook de optie voor het handmatig ermee of automatische antwoorden implementeren door het beleid voor voorwaardelijke toegang configureren. Zie voor meer informatie, [Azure Active Directory Identity Protection](../active-directory-identityprotection.md).

## <a name="risk-event-types"></a>Risicogebeurtenistypen

De **risico gebeurtenistype** eigenschap is een id voor de verdachte actie voor een record van de gebeurtenis risico is gemaakt.

Continue van Microsoft-investeringen in het detectieproces leiden tot:

- Verbeteringen in de nauwkeurigheid van de detectie van bestaande risicogebeurtenissen 
- Nieuwe typen risicogebeurtenissen die in de toekomst worden toegevoegd

### <a name="leaked-credentials"></a>Gelekte referenties

Bij cybercriminelen geldige wachtwoorden van legitieme gebruikers binnendringen, delen ze vaak deze referenties. Dit wordt meestal gedaan door ze te posten openbaar op de donkere web of plakt u sites of door trading of de referenties op de zwarte markt verkopen. De Microsoft gelekte referenties service verkrijgt gebruikersnaam / wachtwoord paren van openbare en donkere websites bewaken en door te werken met:

- Onderzoekers
- Justitie en politie
- Beveiligingsteams bij Microsoft
- Andere vertrouwde bronnen 

Wanneer de service verkrijgt gebruikersnaam / wachtwoord paren, worden ze gecontroleerd op basis van de huidige geldige referenties AAD-gebruikers. Wanneer een overeenkomst wordt gevonden, betekent dit dat het wachtwoord van een gebruiker is aangetast, en een **gelekte referenties risicogebeurtenis** wordt gemaakt.

### <a name="sign-ins-from-anonymous-ip-addresses"></a>Aanmeldingen vanaf anonieme IP-adressen

Dit type risicogebeurtenis identificeert gebruikers die zich heeft aangemeld vanaf een IP-adres dat is geïdentificeerd als een anonieme proxy IP-adres. Deze proxy's worden gebruikt door mensen die u wilt verbergen, IP-adres van hun apparaat en kunnen voor kwade bedoelingen worden gebruikt.

### <a name="impossible-travel-to-atypical-locations"></a>Onmogelijke reis naar ongewone locaties

Dit type risicogebeurtenis identificeert twee aanmeldingen die afkomstig zijn van geografisch verafgelegen locaties, waarbij ten minste een van de locaties mogelijk ook ongewone voor de gebruiker, krijgt het verleden gedrag. Tussen verschillende andere factoren rekening dit algoritme voor machine learning gehouden met de tijd tussen de twee aanmeldingen en de tijd die het voor de gebruiker voor een van de eerste locatie reis naar de tweede, waarmee wordt aangegeven dat een andere gebruiker is met behulp van dezelfde zou hebben geduurd de referenties.

Het algoritme wordt genegeerd voor de hand liggende 'valse positieven' bijdragen aan de voorwaarden onmogelijk traject, zoals VPN-verbindingen en de locaties die regelmatig worden gebruikt door andere gebruikers in de organisatie. Het systeem heeft een eerste leerperiode van 14 dagen gedurende welke het aanmelden gedrag van een nieuwe gebruiker hoort. 

### <a name="sign-in-from-unfamiliar-locations"></a>Aanmelding vanaf onbekende locaties

Dit type risicogebeurtenis rekening gehouden met na aanmelding locaties (IP, breedtegraad / lengtegraad en ASN) om te bepalen van de nieuwe / onbekende locaties. Het systeem wordt informatie opgeslagen over de voorgaande locaties die worden gebruikt door een gebruiker en deze 'vertrouwde' locaties overweegt. De risicogebeurtenis wordt geactiveerd wanneer de aanmelding plaatsvindt vanaf een locatie die nog niet in de lijst met vertrouwde sites. Het systeem heeft een eerste leerperiode van 30 dagen, gedurende welke biedt deze nieuwe locaties als onbekende locaties niet markeren. Aanmeldingen vanaf bekende apparaten en de locaties die geografisch dicht bij een vertrouwde locatie wordt ook negeert door het systeem. 

Identity Protection detecteert aanmeldingen vanaf onbekende locaties ook voor basisverificatie / verouderde protocollen. Omdat deze protocollen geen moderne vertrouwde functies zoals client-id hebt, is er niet voldoende telemetrie naar de fout-positieven. Als u wilt verkleinen het aantal gedetecteerde risico, moet u overstappen op moderne verificatie.   

### <a name="sign-ins-from-infected-devices"></a>Aanmeldingen vanaf geïnfecteerde apparaten

Dit type risicogebeurtenis identificeert aanmeldingen vanaf apparaten geïnfecteerd met malware die bekend zijn bij het actief communiceren met een bot-server. Dit wordt bepaald door de IP-adressen van het apparaat van de gebruiker op basis van IP-adressen die verbonden met een bot-server zijn. 

### <a name="sign-ins-from-ip-addresses-with-suspicious-activity"></a>Aanmeldingen van IP-adressen met verdachte activiteit
Dit type risicogebeurtenis identificeert IP-adressen van waaruit een groot aantal mislukte aanmeldpogingen zijn waargenomen, bij verschillende gebruikersaccounts, gedurende een korte periode. Dit komt overeen met verkeerspatronen van IP-adressen die door aanvallers worden gebruikt, en een sterke indicator dat accounts al zijn of zijn op het punt te worden aangetast. Dit is een machine learning-algoritme dat wordt genegeerd voor de hand liggende fout-positieven, zoals IP-adressen die regelmatig worden gebruikt door andere gebruikers in de organisatie.  Het systeem heeft een eerste leerperiode van 14 dagen waar deze het gedrag aanmelding van een nieuwe gebruiker en een nieuwe tenant hoort.

## <a name="detection-type"></a>Detectietype

De eigenschap van het type detectie is een indicator (**realtime** of **Offline**) voor de periode voor detectie van een risicogebeurtenis. Op dit moment zijn de meeste risicogebeurtenissen gedetecteerd offline in een bewerking voor na verwerking nadat de risicogebeurtenis is opgetreden.

De volgende tabel bevat de hoeveelheid tijd die nodig is voor een detectietype worden weergegeven in het bijbehorende rapport:

| Detectietype | Wachttijd bij rapportage |
| --- | --- |
| Realtime | 5 tot 10 minuten |
| Offline | 2 tot 4 uur |


Voor de typen risicogebeurtenissen gedetecteerd voor Azure Active Directory, zijn de detectietypen:

| Type risicogebeurtenis | Detectietype |
| :-- | --- | 
| [Gebruikers met de referenties zijn gelekt](#leaked-credentials) | Offline |
| [Aanmeldingen vanaf anonieme IP-adressen](#sign-ins-from-anonymous-ip-addresses) | Realtime |
| [Onmogelijke reis naar ongewone locaties](#impossible-travel-to-atypical-locations) | Offline |
| [Aanmeldingen vanaf onbekende locaties](#sign-in-from-unfamiliar-locations) | Realtime |
| [Aanmeldingen vanaf geïnfecteerde apparaten](#sign-ins-from-infected-devices) | Offline |
| [Aanmeldingen vanaf IP-adressen met verdachte activiteiten](#sign-ins-from-ip-addresses-with-suspicious-activity) | Offline|


## <a name="risk-level"></a>Risiconiveau

De eigenschap risico van een risicogebeurtenis is een indicator (**hoge**, **gemiddeld**, of **laag**) voor de ernst en het vertrouwen van een risicogebeurtenis. Deze eigenschap kunt u de prioriteit van de acties die u moet uitvoeren. 

De ernst van de risicogebeurtenis vertegenwoordigt de kracht van het signaal identiteitsgevaren te voorspellen. Het vertrouwen is een indicator van de mogelijkheid van fout-positieven. 

Bijvoorbeeld: 

* **Hoge**: Hoge betrouwbaarheid en de risicogebeurtenis hoge urgentie. Deze gebeurtenissen zijn sterk indicatoren die de identiteit van de gebruiker is aangetast, en eventuele betrokken gebruikersaccounts onmiddellijk moeten worden hersteld.

* **Gemiddeld**: Urgentie hoog, maar lagere vertrouwen risicogebeurtenis, of vice versa. Deze gebeurtenissen zijn potentieel risicovolle en eventuele betrokken gebruikersaccounts moeten worden hersteld.

* **Lage**: Lage vertrouwen en de risicogebeurtenis met lage urgentie. Deze gebeurtenis mogelijk een directe actie is vereist, maar in combinatie met andere risicogebeurtenissen kan bieden een sterke indicatie dat de identiteit is geknoeid.

![Risiconiveau](./media/concept-risk-events/01.png)

### <a name="leaked-credentials"></a>Gelekte referenties

Gelekte referenties risicogebeurtenissen zijn geclassificeerd als een **hoge**, omdat ze voorzien in een duidelijke aanwijzing is dat de gebruikersnaam en wachtwoord beschikbaar voor een aanvaller zijn.

### <a name="sign-ins-from-anonymous-ip-addresses"></a>Aanmeldingen vanaf anonieme IP-adressen

Het risiconiveau voor dit type risicogebeurtenis is **gemiddeld** omdat een anoniem IP-adres niet een sterke indicatie van de inbreuk op een account is. Het is raadzaam dat u direct contact opnemen met de gebruiker om te controleren of als ze anonieme IP-adressen gebruikten.


### <a name="impossible-travel-to-atypical-locations"></a>Onmogelijke reis naar ongewone locaties

Onmogelijk traject is meestal een goede indicatie die een hacker kan aanmelden. Fout-positieven kunnen echter optreden wanneer een gebruiker op reis gaat met behulp van een nieuw apparaat of een VPN-verbinding die doorgaans niet wordt gebruikt door andere gebruikers in de organisatie. Een andere bron van fout-positieven is voor toepassingen die onjuist server IP-adressen als client-IP-adressen, zodat het uiterlijk van aanmeldingen plaatsvinden in het datacenter waar die toepassing van back-end wordt gehost (dit zijn vaak Microsoft-datacenters die kan het uiterlijk geven van aanmeldingen plaatsvinden van Microsoft die eigendom zijn IP-adressen). Als gevolg van deze fout-positieven, het risiconiveau voor deze risicogebeurtenis is **gemiddeld**.

> [!TIP]
> U kunt het bedrag van de gerapporteerde fout-positieven voor dit type risicogebeurtenis beperken door het configureren van [benoemde locaties](../active-directory-named-locations.md). 

### <a name="sign-in-from-unfamiliar-locations"></a>Aanmelding vanaf onbekende locaties

Onbekende locaties kunnen bieden een sterke aanwijzing dat een aanvaller in staat het gebruik van de identiteit van een gestolen is. Fout-positieven kunnen optreden wanneer een gebruiker wordt verzonden, wordt geprobeerd om een nieuw apparaat of met behulp van een nieuwe VPN-verbinding. Als gevolg van deze fout-positieven het risiconiveau voor dit gebeurtenistype is **gemiddeld**.

### <a name="sign-ins-from-infected-devices"></a>Aanmeldingen vanaf geïnfecteerde apparaten

Deze risicogebeurtenis identificeert IP-adressen, niet de apparaten van de gebruiker. Als meerdere apparaten zich achter een enkel IP-adres, en alleen bepaalde worden bepaald door een bot-netwerk, aanmeldingen vanaf andere apparaten mijn trigger deze gebeurtenis onnodig, daarom deze risicogebeurtenis is geclassificeerd als **laag**.  

U wordt aangeraden dat u contact opnemen met de gebruiker en scannen op apparaten van de gebruiker. Het is ook mogelijk dat de persoonlijke apparaat van een gebruiker is geïnfecteerd of dat iemand anders een geïnfecteerd apparaat uit hetzelfde IP-adres als de gebruiker heeft gebruikt. Geïnfecteerde apparaten zijn vaak geïnfecteerd met malware die nog niet zijn geïdentificeerd door de antivirussoftware, en kan ook duiden op een ongeldige gebruikers-gewoonten dat het apparaat worden geïnfecteerd hebben veroorzaakt.

Zie voor meer informatie over het adres malware-infecties de [Malware Protection Center](https://www.microsoft.com/en-us/security/portal/definitions/adl.aspx/).

### <a name="sign-ins-from-ip-addresses-with-suspicious-activity"></a>Aanmeldingen van IP-adressen met verdachte activiteit

Het is raadzaam dat u contact opnemen met de gebruiker om te controleren of als ze daadwerkelijk aangemeld vanaf een IP-adres dat is gemarkeerd als verdacht. Het risiconiveau voor dit gebeurtenistype is '**gemiddeld**' omdat verschillende apparaten mogelijk iets achter op hetzelfde IP-adres, terwijl alleen sommige zijn mogelijk niet volledig verantwoordelijk voor de verdachte activiteit. 


## <a name="next-steps"></a>Volgende stappen

* [Gebruikers die lopen risico beveiligingsrapport](concept-user-at-risk.md)
* [Rapport riskante aanmeldingen](concept-risky-sign-ins.md)
* [Azure AD Identity Protection](../active-directory-identityprotection.md).
