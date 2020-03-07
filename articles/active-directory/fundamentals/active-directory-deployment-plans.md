---
title: Implementatieplannen voor - Azure Active Directory | Microsoft Docs
description: End-to-end-richtlijnen over het implementeren van de vele mogelijkheden van Azure Active Directory.
services: active-directory
author: msaburnley
manager: daveba
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 08/20/2019
ms.author: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 17e6708225262349d56c6e261895882e9c31677f
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2020
ms.locfileid: "78378223"
---
# <a name="azure-active-directory-deployment-plans"></a>Azure Active Directory-implementatieplannen
Zoekt u end-to-end-richt lijnen voor het implementeren van Azure Active Directory (Azure AD)-mogelijkheden? Met Azure AD-implementatie plannen kunt u de bedrijfs waarde, plannings overwegingen en operationele procedures door lopen die nodig zijn om algemene Azure AD-mogelijkheden te implementeren.

Gebruik vanuit een van de plannings pagina's de afdruk functie van uw browser om een bijgewerkte offline versie van de documentatie te maken.
## <a name="include-the-right-stakeholders"></a>Neem de juiste belanghebbenden op

Wanneer u begint met het plannen van uw implementatie voor een nieuwe functie, is het belang rijk dat u belang rijke belanghebbenden in uw organisatie opneemt. U wordt aangeraden de persoon of personen te identificeren en te documenteren die aan elk van de volgende rollen voldoen en samen te werken om de betrokkenheid van het project te bepalen.  

Rollen kunnen de volgende zijn: 

|Rol |Beschrijving |
|-|-|
|Eind gebruiker|Een representatieve groep gebruikers waarvoor de functionaliteit wordt geïmplementeerd. Bekijkt vaak een voor beeld van de wijzigingen in een test programma.
|IT-ondersteunings Manager|De IT-afdeling ondersteunt organisaties die in het perspectief van een helpdesk medewerker invoer kunnen bieden over de ondersteuning van deze wijziging.  
|Identiteits architect of Azure Global-beheerder|Identity Management-team representatief om te bepalen hoe deze wijziging wordt afgestemd op de infra structuur voor identiteits beheer in uw organisatie.|
|Zakelijke eigenaar van de toepassing |De algemene zakelijke eigenaar van de betrokken toepassing (en), waaronder het beheren van de toegang.  Kan ook invoer geven over de gebruikers ervaring en het nut van deze wijziging ten opzichte van het perspectief van een eind gebruiker.
|Eigenaar van beveiliging|Een vertegenwoordiger van het beveiligings team dat kan worden afgemeld dat het plan voldoet aan de beveiligings vereisten van uw organisatie.|
|Nalevings beheerder|De persoon binnen uw organisatie die verantwoordelijk is voor de naleving van de bedrijfs-, branche-of overheids vereisten.|

**De mate van betrokkenheid kan het volgende omvatten:**

- **R**esponsible voor het implementeren van het project plan en de resultaten 

- **Een**Pproval van het project plan en het resultaat 

- **C**ontributor naar plan en resultaat van project 

- **Ik**nformed van het project plan en de resultaten


## <a name="best-practices-for-a-pilot"></a>Aanbevolen procedures voor een pilot
Met een pilot kunt u testen met een kleine groep voordat u een mogelijkheid voor iedereen inschakelt. Zorg ervoor dat als onderdeel van uw test, elke use-case binnen uw organisatie grondig wordt getest. Het is het beste om een specifieke groep proef gebruikers te richten voordat u deze naar uw organisatie als geheel implementeert.

In uw eerste golf, richt IT, bruikbaarheid en andere geschikte gebruikers die kunnen testen en feedback geven. Deze feedback moet worden gebruikt om de communicatie en instructies die u naar uw gebruikers verzendt verder te ontwikkelen en om inzicht te krijgen in de soorten problemen die uw ondersteunings medewerkers kunnen zien. 

Het uitbreiden van de implementatie naar grotere groepen gebruikers moet worden uitgevoerd door het bereik van de doel groep (en) te verg Roten. Dit kan worden gedaan via een [dynamisch groepslid maatschap](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-dynamic-membership)of door gebruikers hand matig toe te voegen aan de doel groep (en).


## <a name="deploy-authentication"></a>Verificatie implementeren

| Mogelijkheid | Beschrijving|
| -| -|
| [Multi-Factor Authentication](https://aka.ms/deploymentplans/mfa)| Azure Multi-Factor Authentication (MFA) is een Microsoft-oplossing voor verificatie in twee stappen. Met door de beheerder goedgekeurde verificatie methoden helpt Azure MFA u bij het beveiligen van de toegang tot uw gegevens en toepassingen terwijl de vraag naar een eenvoudig aanmeldings proces beantwoordt. |
| [Voorwaardelijke toegang](https://aka.ms/deploymentplans/ca)| Met voorwaardelijke toegang kunt u geautomatiseerde beslissingen over toegangs beheer implementeren voor wie toegang heeft tot uw Cloud-apps, op basis van voor waarden. |
| [Self-service voor wachtwoord opnieuw instellen](https://aka.ms/deploymentplans/sspr)| Met selfservice voor wachtwoord herstel kunnen uw gebruikers hun wacht woord opnieuw instellen zonder tussen komst van de beheerder, wanneer en wanneer dat nodig is. |
| [Geen wacht woord](https://aka.ms/deploymentplans/passwordless) | Implementeer verificatie op basis van een wacht woord met behulp van de Microsoft Authenticator app-of FIDO2-beveiligings sleutels in uw organisatie |

## <a name="deploy-application-management"></a>Toepassings beheer implementeren

| Mogelijkheid | Beschrijving|
| -| - |
| [Eenmalige aanmelding](https://aka.ms/deploymentplans/sso)| Met eenmalige aanmelding kunnen uw gebruikers toegang krijgen tot de apps en bronnen die ze nodig hebben om zaken te doen terwijl ze zich slechts één keer aanmelden. Nadat ze zich hebben aangemeld, kunnen ze van Microsoft Office naar Sales Force naar interne toepassingen gaan zonder dat ze een tweede keer referenties hoeven in te voeren. |
| [Toegangs venster](https://aka.ms/deploymentplans/accesspanel)| Biedt uw gebruikers een eenvoudige hub om te ontdekken en toegang tot al hun toepassingen. Stel ze in staat om productiever te zijn met selfservice mogelijkheden, zoals het aanvragen van toegang tot apps en groepen, of het beheren van de toegang tot bronnen namens anderen. |


## <a name="deploy-hybrid-scenarios"></a>Hybride scenario's implementeren

| Mogelijkheid | Beschrijving|
| -| -|
| [ADFS voor wachtwoordhash-synchronisatie](https://aka.ms/deploymentplans/adfs2phs)| Met wacht woord-hash-synchronisatie worden hashes van gebruikers wachtwoorden gesynchroniseerd van on-premises Active Directory naar Azure AD, zodat gebruikers zonder tussen komst van de on-premises-Active Directory worden geverifieerd met Azure AD. |
| [ADFS voor Pass Through-verificatie](https://aka.ms/deploymentplans/adfs2pta)| Met Azure AD Pass-Through-verificatie kunnen gebruikers zich aanmelden bij zowel lokale als Cloud toepassingen met dezelfde wacht woorden. Deze functie biedt gebruikers een betere ervaring-een minder wacht woord om te onthouden en vermindert de IT-helpdesk kosten, omdat gebruikers minder kans hebben om zich aan te melden. Als iemand zich aanmeldt bij Azure AD, worden met deze functie de wachtwoorden rechtstreeks gecontroleerd tegen de on-premises Active Directory. |
| [Azure AD-toepassingsproxy](https://aka.ms/deploymentplans/appproxy)| De huidige werknemer wil overal, op elke plek en op elk apparaat productief kunnen zijn. Ze moeten toegang hebben tot SaaS-apps in de Cloud en zakelijke apps on-premises. Azure AD-toepassings proxy maakt deze robuuste toegang mogelijk zonder kost bare en complexe virtuele particuliere netwerken (Vpn's) of gedemilitariseerde zones (Dmz's). |
| [Naadloze eenmalige aanmelding](../hybrid/how-to-connect-sso-quick-start.md)| Met Naadloze eenmalige aanmelding van Azure Active Directory (Naadloze SSO van Azure AD) worden gebruikers aangemeld als hun bedrijfsapparaten zijn verbonden met net bedrijfsnetwerk. Met deze functie hoeven gebruikers hun wacht woord niet in te voeren om zich aan te melden bij Azure AD, en hoeven ze hun gebruikers namen meestal niet in te voeren. Deze functie biedt geautoriseerde gebruikers eenvoudig toegang tot uw Cloud toepassingen zonder dat hiervoor extra on-premises onderdelen nodig zijn. |

## <a name="deploy-user-provisioning"></a>Gebruikers inrichten implementeren

| Mogelijkheid | Beschrijving|
| -| -|
| [Inrichten van gebruikers](https://aka.ms/deploymentplans/userprovisioning)| Met Azure AD kunt u het maken, onderhouden en verwijderen van gebruikers-id's in cloud(SaaS)-toepassingen als Dropbox, Salesforce, ServiceNow en andere, automatiseren. |
| [Gebruikers inrichten in de Cloud](https://aka.ms/deploymentplans/cloudhr)| Cloud HR-gebruikers die zich richten op Active Directory, maken een basis voor het aanhouden van identiteits beheer en verbeteren de kwaliteit van bedrijfs processen die afhankelijk zijn van gezaghebbende identiteits gegevens. Met behulp van deze functie in uw Cloud-HR-product, zoals workday of Successfactors, kunt u de identiteits levenscyclus van werk nemers en voorwaardelijke werk nemers naadloos beheren door regels te configureren die zijn toegewezen aan processen voor het door geven van invoeg gebruikers (zoals nieuwe huren, beëindigen, Overdracht) naar IT-inrichtings acties (zoals maken, inschakelen, uitschakelen) |

## <a name="deploy-governance-and-reporting"></a>Governance en rapportage implementeren

| Mogelijkheid | Beschrijving|
| -| -|
| [Privileged Identity Management](https://aka.ms/deploymentplans/pim)| Azure AD Privileged Identity Management (PIM) helpt u bij het beheren van geprivilegieerde beheerders rollen in azure AD, Azure-resources en andere online services van micro soft. PIM biedt oplossingen zoals just-in-time-toegang, werk stromen voor het aanvragen van goed keuring en volledig geïntegreerde toegangs beoordelingen, zodat u schadelijke activiteiten van geprivilegieerde rollen in realtime kunt identificeren, detecteren en voor komen. |
| [Rapportage en bewaking](https://aka.ms/deploymentplans/reporting)| Het ontwerp van uw Azure AD-oplossing voor rapportage en bewaking is afhankelijk van uw wettelijke, beveiligings-en operationele vereisten, evenals uw bestaande omgeving en processen. In dit artikel vindt u de verschillende ontwerp opties en wordt u begeleid bij de juiste implementatie strategie. |
