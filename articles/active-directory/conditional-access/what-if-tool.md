---
title: Wat is de wat als hulpprogramma Azure Active Directory voor de voorwaardelijke toegang?
description: Meer informatie over hoe u begrijp de gevolgen van beleid voor voorwaardelijke toegang in uw omgeving.
services: active-directory
keywords: Voorwaardelijke toegang tot apps, voorwaardelijke toegang in Azure AD, beveiligde toegang tot bedrijfsresources, beleid voor voorwaardelijke toegang
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.subservice: conditional-access
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/20/2018
ms.author: joflore
ms.reviewer: nigu
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6a0f1fa0630a58054a138b730141b982af427475
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67111922"
---
# <a name="what-is-the-what-if-tool-in-azure-active-directory-conditional-access"></a>Wat is de wat als hulpprogramma Azure Active Directory voor de voorwaardelijke toegang?

[Voorwaardelijke toegang](../active-directory-conditional-access-azure-portal.md) is een functie van Azure Active Directory (Azure AD) waarmee u om te bepalen hoe gemachtigde gebruikers toegang tot uw cloud-apps. Hoe u weet wat u kunt verwachten formulier beleid voor voorwaardelijke toegang in uw omgeving? Als u wilt deze vraag te beantwoorden, kunt u de **voorwaardelijke toegang hulpprogramma what-if**.

In dit artikel wordt uitgelegd hoe u dit hulpprogramma kunt gebruiken voor het testen van beleid voor voorwaardelijke toegang.

## <a name="what-it-is"></a>Wat is het?

De **voor voorwaardelijke toegang in wat als beleid hulpprogramma** kunt u begrijp de gevolgen van het beleid voor voorwaardelijke toegang in uw omgeving. In plaats van uw beleid te testen door handmatig meerdere aanmeldingen uit te voeren, kunt u met dit hulpprogramma een gesimuleerde gebruikersaanmelding evalueren. De simulatie schat de impact van deze aanmelding op uw beleid in en genereert een simulatierapport. Het rapport de toegepaste beleidsregels voor voorwaardelijke toegang niet alleen weergeven, maar ook [klassieke beleidsregels](policy-migration.md#classic-policies) als deze bestaan.    

Wat bepalen als hulpprogramma's biedt ook een manier om snel de beleidsregels die betrekking hebben op een specifieke gebruiker. U kunt de informatie, bijvoorbeeld gebruiken als u nodig hebt om een probleem te verhelpen.  

## <a name="how-it-works"></a>Hoe werkt het?

In de **voorwaardelijke toegang hulpprogramma what-if**, moet u eerst het configureren van instellingen van het scenario aanmelding dat u wilt simuleren. Deze instellingen omvatten:

- De gebruiker die u wilt testen 

- De gebruiker probeert toegang tot de cloud-apps

- De voorwaarden waarmee de toegang tot de cloud configureert apps wordt uitgevoerd
     
Als de volgende stap kunt u een simulatie uitvoeren die wordt geëvalueerd als de instellingen te starten. Alleen de beleidsregels die zijn ingeschakeld, maken deel uit van een evaluatie uitgevoerd.


Wanneer de evaluatie is voltooid, genereert het hulpprogramma een rapport van het betreffende beleid.



## <a name="running-the-tool"></a>Het programma wordt uitgevoerd

U vindt de **wat gebeurt er als** hulpprogramma op de **[voorwaardelijke toegang - beleid](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/Policies)** pagina in de Azure portal.

Voor het starten van het hulpprogramma in de werkbalk boven op de lijst met beleidsregels, klikt u op **wat gebeurt er als**.

![Wat als](./media/what-if-tool/01.png)

Voordat u een evaluatie uitvoeren kunt, moet u de instellingen configureren.

## <a name="settings"></a>Instellingen

In deze sectie vindt u informatie over de instellingen van de simulatie uitvoeren.

![Wat als](./media/what-if-tool/02.png)


### <a name="user"></a>Gebruiker

U kunt slechts één gebruiker selecteren. Dit is de enige vereiste veld.

### <a name="cloud-apps"></a>Cloud-apps

De standaardwaarde voor deze instelling is **alle cloud-apps**. De standaardinstelling voert een evaluatie van alle beschikbare beleidsregels in uw omgeving. U kunt het bereik aan beleidsregels die betrekking hebben op specifieke cloud-apps beperken.


### <a name="ip-address"></a>IP-adres

Het IP-adres is één IPv4-adres om na te bootsen de [locatievoorwaarde](location-condition.md). Het adres vertegenwoordigt Internetgericht adres van het apparaat dat wordt gebruikt door de gebruiker zich aanmeldt. U kunt controleren of het IP-adres van een apparaat, bijvoorbeeld te navigeren naar [wat is Mijn IP-adres](https://whatismyipaddress.com).    

### <a name="device-platforms"></a>Apparaatplatformen

Deze instelling imiteert de [apparaat platforms voorwaarde](conditions.md#device-platforms) en Hiermee geeft u het equivalent van **alle platformen (inclusief niet-ondersteunde)** . 
### <a name="client-apps"></a>Client-apps

Deze instelling imiteert de [client-apps voorwaarde](conditions.md#client-apps).
Standaard deze instelling zorgt ervoor dat een evaluatie van alle beleidsregels die **Browser** of **mobiele apps en bureaubladclients** ofwel afzonderlijk of beide geselecteerd. Ook wordt gedetecteerd voor die afdwingen **Exchange ActiveSync (EAS)** . U kunt deze instelling verfijnen door te selecteren:

- **Browser** om te evalueren van alle beleidsregels met ten minste **Browser** geselecteerde. 

- **Mobiele apps en bureaubladclients** om te evalueren van alle beleidsregels met ten minste **mobiele apps en bureaubladclients** geselecteerde. 


### <a name="sign-in-risk"></a>Aanmeldingsrisico

Deze instelling imiteert de [voorwaarde voor aanmeldingsrisico](conditions.md#sign-in-risk).   


## <a name="evaluation"></a>Evaluatie 

U start een evaluatie door te klikken op **wat gebeurt er als**. Het resultaat van evaluatie van biedt u een rapport dat bestaat uit: 

![Wat als](./media/what-if-tool/03.png)

- Een indicator of klassieke beleidsregels bestaan in uw omgeving
- Beleidsregels die betrekking hebben op de gebruiker
- Beleidsregels die niet van toepassing op de gebruiker


Als [klassieke beleidsregels](policy-migration.md#classic-policies) bestaat voor de geselecteerde cloud-apps, een indicator is aan u gepresenteerd. Door te klikken op de indicator, wordt u omgeleid naar de pagina klassieke beleidsregels. U kunt op de pagina klassieke beleidsregels migreren van een klassiek beleid of alleen uitschakelen. U kunt terugkeren naar het resultaat van evaluatie van deze pagina sluiten.

In de lijst met beleidsregels die betrekking hebben op de geselecteerde gebruiker, kunt u ook een lijst met vinden [verlenen besturingselementen](controls.md#grant-controls) en [sessie](controls.md#session-controls) besturingselementen moet voldoen aan de gebruiker.

In de lijst met beleidsregels die niet van toepassing op de gebruiker, u kunt en vindt ook de redenen waarom dit beleid niet van toepassing. Voor elke vermelde beleid staat de reden voor de eerste voorwaarde waaraan is niet voldaan aan. Een mogelijke reden voor een beleid dat niet is toegepast is een uitgeschakeld beleid, omdat ze niet meer worden beoordeeld.   



## <a name="next-steps"></a>Volgende stappen

- Als u weten hoe u een beleid voor voorwaardelijke toegang configureren wilt, Zie [MFA vereisen voor specifieke apps met Azure Active Directory voor voorwaardelijke toegang](app-based-mfa.md).

- Als u klaar om te configureren van beleid voor voorwaardelijke toegang voor uw omgeving bent, raadpleegt u de [aanbevolen procedures voor voorwaardelijke toegang in Azure Active Directory](best-practices.md). 

- Als u migreren van klassiek beleid wilt, raadpleegt u [klassiek beleid migreren in Azure portal](policy-migration.md)  
