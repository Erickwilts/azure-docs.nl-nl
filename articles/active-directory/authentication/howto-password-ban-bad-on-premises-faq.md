---
title: On-premises Azure AD-wachtwoord beveiliging FAQ - Azure Active Directory
description: On-premises bescherming van Azure AD-wachtwoord Veelgestelde vragen
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: article
ms.date: 02/01/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jsimmons
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3b4879093ed80a554219b053cc5a2bc895126725
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67702898"
---
# <a name="azure-ad-password-protection-on-premises---frequently-asked-questions"></a>Azure AD wachtwoordbeveiliging on-premises - Veelgestelde vragen over

## <a name="general-questions"></a>Algemene vragen

**V: Welke richtlijnen moeten gebruikers worden gegeven voor het selecteren van een veilig wachtwoord?**

De huidige richtlijnen van Microsoft over dit onderwerp kunt u vinden op de volgende koppeling:

[Wachtwoord voor Microsoft-richtlijnen](https://www.microsoft.com/research/publication/password-guidance)

**V: Kan on-premises bescherming van Azure AD-wachtwoord in niet-openbare clouds worden ondersteund?**

Nee, on-premises Azure AD-wachtwoord beveiliging wordt alleen ondersteund in de openbare cloud. Geen datum is aangekondigd voor beschikbaarheid van niet-openbare cloud.

**V: Hoe kan ik voordelen van Azure AD-wachtwoord beveiliging toepassen op een subset van mijn on-premises gebruikers?**

Wordt niet ondersteund. Eenmaal geïmplementeerd en ingeschakeld, beveiliging van Azure AD-wachtwoord niet onderscheid - ontvangen gelijk beveiligingsvoordelen van alle gebruikers.

**V: Wat is het verschil tussen een wachtwoordwijziging en een wachtwoorden in te stellen (of opnieuw instellen)?**

Een wachtwoordwijziging is wanneer een gebruiker ervoor een nieuw wachtwoord kiest na waaruit blijkt dat zij hebben kennis van het oude wachtwoord. Bijvoorbeeld: dit is wat er gebeurt wanneer een gebruiker meldt zich aan bij Windows en wordt vervolgens gevraagd om een nieuw wachtwoord.

Een wachtwoord instellen (ook wel een wachtwoord opnieuw instellen) is als een beheerder wordt vervangen door het wachtwoord voor een account met een nieuw wachtwoord, bijvoorbeeld met behulp van het hulpprogramma Active Directory: gebruikers en Computers. Voor deze bewerking moet een hoge mate van bevoegdheden (meestal Domain Admin) en de persoon die meestal uitvoeren van de bewerking heeft geen kennis van het oude wachtwoord. Helpdesk scenario's dit vaak doen, bijvoorbeeld wanneer een gebruiker die is het wachtwoord vergeten te assisteren. U ziet ook wachtwoord ingesteld gebeurtenissen wanneer een nieuwe gebruikersaccount wordt gemaakt voor het eerst met een wachtwoord.

Het wachtwoordbeleid voor validatie gedraagt zich hetzelfde, ongeacht of een wachtwoord wijzigen of een set is dat wordt uitgevoerd. De Azure AD-wachtwoord DC Protection Agent-service verschillende gebeurtenissen, zodat u op de hoogte of een wachtwoordwijziging aanmelden of set-bewerking is uitgevoerd.  Zie [Azure AD-wachtwoord beveiliging, controle en logboekregistratie](https://docs.microsoft.com/azure/active-directory/authentication/howto-password-ban-bad-on-premises-monitor).

**V: Waarom zijn dubbele wachtwoord afwijzing gebeurtenissen vastgelegd wanneer u probeert een zwak wachtwoord in te stellen met de Active Directory: gebruikers en Computers management-module?**

De Active Directory: gebruikers en Computers-beheermodule wordt eerst geprobeerd om in te stellen het nieuwe wachtwoord met behulp van het Kerberos-protocol. Als de mislukt maakt de module een tweede probeert in te stellen van het wachtwoord met behulp van een oudere (SAM RPC)-protocol (de specifieke protocollen die worden gebruikt zijn niet belangrijk). Als het nieuwe wachtwoord wordt beschouwd als zwakke door Azure AD-wachtwoord Protection, resulteert dit in twee sets met wachtwoord opnieuw instellen van afwijzing gebeurtenissen worden geregistreerd.

**V: Is dit dan ondersteund voor het installeren van Azure AD-wachtwoord bescherming naast andere producten op basis van wachtwoord-filter?**

Ja. Ondersteuning voor meerdere geregistreerde wachtwoord filter-dll's een functie van de Windows core is en niet specifiek voor Azure AD-wachtwoord beveiliging. Alle geregistreerde wachtwoord filter dll-bestanden moet accepteren voordat een wachtwoord wordt geaccepteerd.

**V: Hoe kan ik implementeren en Azure AD-wachtwoord beveiliging configureren in de Active Directory-omgeving zonder het gebruik van Azure?**

Wordt niet ondersteund. Azure AD-wachtwoord beveiliging is een functie van Azure die ondersteuning biedt voor wordt uitgebreid naar een on-premises Active Directory-omgeving.

**V: Hoe kan ik de inhoud van het beleid op het niveau van de Active Directory wijzigen?**

Wordt niet ondersteund. Het beleid kan alleen worden beheerd met behulp van de Azure AD-beheerportal. Zie ook de vorige vraag.

**V: Waarom is de DFS-replicatie voor sysvol-replicatie vereist?**

FRS (de voorafgaande-technologie DFSR) heeft tal van bekende problemen en wordt volledig ondersteund in nieuwere versies van Windows Server Active Directory. Nul testen van de beveiliging van Azure AD-wachtwoord wordt uitgevoerd op domeinen FRS geconfigureerd.

Zie de volgende artikelen voor meer informatie:

[De aanvraag voor sysvol-replicatie migreren naar DFS-replicatie](https://blogs.technet.microsoft.com/askds/2010/04/22/the-case-for-migrating-sysvol-to-dfsr)

[Het einde is Nigh voor FRS](https://blogs.technet.microsoft.com/filecab/2014/06/25/the-end-is-nigh-for-frs)

**V: Hoeveel schijfruimte vereist de functie op de sysvol-share van het domein?**

Het gebruik van de precieze schijfruimte is nadat deze is afhankelijk van factoren zoals het aantal en de lengte van de uitgesloten tokens in de globale uitgesloten lijst met Microsoft en de aangepaste lijst per tenant plus versleuteling overhead. De inhoud van deze lijsten zijn waarschijnlijk in de toekomst vergroten. Met die in gedachten is een redelijke verwachting is dat de functie ten minste vijf (5) MB aan ruimte op de sysvol-share van het domein moet.

**V: Waarom is opnieuw opstarten vereist om te installeren of upgraden van de DC-agentsoftware?**

Deze vereiste wordt veroorzaakt door core Windows gedrag.

**V: Is er manier om een DC-agent voor het gebruik van een bepaalde proxyserver te configureren?**

Nee. Aangezien de proxyserver stateless is, is het niet belangrijk welke specifieke proxy-server wordt gebruikt.

**V: Is het goed dat het implementeren van de Azure AD-wachtwoord beveiliging-proxyservice naast andere services zoals Azure AD Connect?**

Ja. De Azure AD-wachtwoord beveiliging Proxy-service en Azure AD Connect moet nooit rechtstreeks met elkaar conflicteren.

**V: In welke volgorde moeten de DC-agents en proxy's worden geïnstalleerd en geregistreerd?**

Een volgorde van de installatie van de Proxy-agent, DC-agentinstallatie forest registratie en registratie van Proxy wordt ondersteund.

**V: Ik moet over de prestaties op de domeincontrollers van het implementeren van deze functie is bereikt?**

De Azure AD-wachtwoord DC Protection Agent-service al dan niet mogen aanzienlijk invloed hebben op prestaties van domeincontrollers in een bestaande Active Directory-implementatie in orde.

Voor de meeste implementaties van Active Directory-wachtwoord zijn wijzigingsbewerkingen een klein deel van de algemene workload op een bepaalde domeincontroller. Stel bijvoorbeeld dat Active Directory-domein met 10000 gebruikersaccounts en een MaxPasswordAge beleid ingesteld op 30 dagen. Gemiddeld, ziet dit domein 10000/30 = ~ 333 wachtwoordswijzigingen elke dag een klein aantal bewerkingen voor zelfs één domeincontroller is. Houd rekening met een mogelijke slechtste scenario: Stel deze ~-333 wachtwoordwijzigingen op een enkele DC via één uur zijn uitgevoerd. In dit scenario kan bijvoorbeeld optreden wanneer veel werknemers alle worden geleverd om te werken op een maandagochtend. Zelfs in dat geval we proberen nog steeds ~333/60 minuten = zes wachtwoordwijzigingen per minuut, zodat het is niet een aanzienlijke belasting.

Als uw huidige domeincontrollers zijn al wordt uitgevoerd op het niveau van prestaties beperkt (bijvoorbeeld maximum is overschreden met betrekking tot CPU, schijfruimte, schijf-i/o, enzovoort), het is echter aangeraden toevoegen van extra domeincontrollers of beschikbare schijfruimte, uitvouwen voordat Deze functie wordt geïmplementeerd. Zie ook de vraag hierboven over sysvol schijfruimtegebruik hierboven.

**V: Ik wil Azure AD-wachtwoord beveiliging op een paar DC's in Mijn domein testen. Is het mogelijk om af te dwingen de wachtwoordwijzigingen gebruiker deze specifieke DC's gebruiken?**

Nee. De Windows-clientbesturingssysteem bepaalt welke domeincontroller wordt gebruikt wanneer een gebruiker het wachtwoord wijzigt. De domeincontroller is geselecteerd op basis van factoren, zoals Active Directory-site en elk subnet toewijzingen, omgevingsspecifieke netwerkconfiguratie, enzovoort. Azure AD-wachtwoord Protection heeft geen controle over deze factoren en kan niet van invloed zijn op welke domeincontroller is geselecteerd voor het wijzigen van het wachtwoord van een gebruiker.

Een manier om dit doel gedeeltelijk bereiken zijn Azure AD-wachtwoord beveiliging op alle domeincontrollers in een bepaalde Active Directory-site implementeren. Deze benadering biedt redelijke dekking voor de Windows-clients die zijn toegewezen aan die site, en dus ook voor de gebruikers die zijn aangemeld bij deze clients en hun wachtwoorden wijzigen.

**V: Als ik de Azure AD-wachtwoord DC Protection Agent-service op alleen de primaire domeincontroller (PDC) installeert, worden alle andere domeincontrollers in het domein ook worden beveiligd?**

Nee. Als het wachtwoord van een gebruiker wordt gewijzigd op een domeincontroller voor bepaalde niet-primaire domeincontroller, wordt het wachtwoord niet-versleutelde tekst nooit naar de primaire domeincontroller (dit idee is een algemene profieltoewijzingen perceptie) verzonden. Zodra een nieuw wachtwoord wordt geaccepteerd op een domeincontroller die is opgegeven, wordt die DC dat wachtwoord gebruikt voor het maken van de verschillende verificatie-protocol-specifieke-hashes van wachtwoord en klikt u vervolgens blijft bestaan die hashes in de map. Het wachtwoord niet-versleutelde tekst is niet persistent. De bijgewerkte hashes worden vervolgens gerepliceerd naar de primaire domeincontroller. Wachtwoorden van gebruikers kunnen in sommige gevallen rechtstreeks op de PDC en wordt opnieuw, afhankelijk van verschillende factoren, zoals netwerktopologie en ontwerp voor Active Directory-site worden gewijzigd. (Zie de vorige vraag.)

Kortom, is de implementatie van de Azure AD-wachtwoord DC Protection Agent-service op de PDC tot 100% dekking voor de beveiliging van de functie binnen het domein vereist. Implementeren van de functie op de PDC alleen beschikt niet over de voordelen van Azure AD-wachtwoord beveiliging beveiliging voor alle andere domeincontrollers in het domein.

**V: Een System Center Operations Manager managementpack beschikbaar is voor Azure AD-wachtwoord beveiliging?**

Nee.

**V: Waarom Azure nog steeds weigert zwakke wachtwoorden zelfs als het beleid om te worden in de controlemodus geconfigureerd?**

Controlemodus wordt alleen ondersteund in de on-premises Active Directory-omgeving. Azure is impliciet altijd in de modus 'afdwingen' wanneer wachtwoorden worden geëvalueerd.

## <a name="additional-content"></a>Aanvullende inhoud

De volgende koppelingen kunnen maken geen deel uit van de belangrijkste documentatie voor Azure AD-wachtwoord Protection maar een handige bron van meer informatie over de functie.

[Azure AD wachtwoord Protection is nu algemeen beschikbaar.](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-Password-Protection-is-now-generally-available/ba-p/377487)

[E-Phishing Protection handleiding – deel 15: Implementeren van de beveiligingsservice voor Microsoft Azure AD-wachtwoord (voor On-Premises te!)](https://blogs.technet.microsoft.com/cloudready/2018/10/14/email-phishing-protection-guide-part-15-implement-the-microsoft-azure-ad-password-protection-service-for-on-premises-too/)

[Azure AD-wachtwoordbeveiliging en Smart Lockout bent nu in openbare Preview.](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-Password-Protection-and-Smart-Lockout-are-now-in-Public/ba-p/245423#M529)

## <a name="microsoft-premierunified-support-training-available"></a>Training voor Microsoft Premier\Unified het ondersteuningsteam beschikbaar

Als u geïnteresseerd bent in meer informatie over de beveiliging van Azure AD-wachtwoord en deze in uw omgeving is geïmplementeerd, kunt u profiteren van een proactieve Microsoft service is beschikbaar voor klanten met een contract voor Premier Support- of Unified-ondersteuning. De service is met de naam Azure Active Directory: Wachtwoordbeveiliging. Neem contact op met uw Technical Account Manager voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

Als u een on-premises bescherming van Azure AD-wachtwoord-vraag die hier niet wordt beantwoord hebt, dienen een feedbackitem hieronder - Hartelijk dank!

[Wachtwoordbeveiliging in Azure AD implementeren](howto-password-ban-bad-on-premises-deploy.md)
