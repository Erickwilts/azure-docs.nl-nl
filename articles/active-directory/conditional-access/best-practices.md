---
title: Aanbevolen procedures voor voorwaardelijke toegang in Azure Active Directory | Microsoft Docs
description: Meer informatie over de dingen die u moet weten en wat het is om te voor komen bij het configureren van beleid voor voorwaardelijke toegang.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: article
ms.date: 01/25/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3b7265f8d5ec4b7336253787e9cb881900a52b79
ms.sourcegitcommit: 5d6c8231eba03b78277328619b027d6852d57520
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/13/2019
ms.locfileid: "68963453"
---
# <a name="best-practices-for-conditional-access-in-azure-active-directory"></a>Aanbevolen procedures voor voorwaardelijke toegang in Azure Active Directory

Met [Azure Active Directory (Azure AD) voorwaardelijke toegang](../active-directory-conditional-access-azure-portal.md)kunt u bepalen hoe geautoriseerde gebruikers toegang hebben tot uw Cloud-apps. In dit artikel vindt u informatie over:

- Dingen die u moet weten 
- Wat u moet doen bij het configureren van beleid voor voorwaardelijke toegang. 

In dit artikel wordt ervan uitgegaan dat u bekend bent met de concepten en de terminologie die wordt beschreven in [Wat is voorwaardelijke toegang in azure Active Directory?](../active-directory-conditional-access-azure-portal.md)

## <a name="whats-required-to-make-a-policy-work"></a>Wat is er nodig om een beleid te kunnen gebruiken?

Wanneer u een nieuw beleid maakt, zijn er geen gebruikers, groepen, apps of toegangs elementen geselecteerd.

![Cloud-apps](./media/best-practices/02.png)

Als u uw beleid wilt laten werken, moet u het volgende configureren:

| Wat           | Werking                                  | Waarom |
| :--            | :--                                  | :-- |
| **Cloud-apps** |Selecteer een of meer apps.  | Het doel van een beleid voor voorwaardelijke toegang is om u te laten bepalen hoe geautoriseerde gebruikers toegang hebben tot Cloud-apps.|
| **Gebruikers en groepen** | Selecteer ten minste één gebruiker of groep die is gemachtigd om toegang te krijgen tot uw geselecteerde Cloud-apps. | Een beleid voor voorwaardelijke toegang waaraan geen gebruikers en groepen zijn toegewezen, wordt nooit geactiveerd. |
| **Toegangs beheer** | Selecteer ten minste één toegangs beheer. | Als aan uw voor waarden wordt voldaan, moet uw beleids processor weten wat er moet gebeuren. |

## <a name="what-you-should-know"></a>Wat u moet weten

### <a name="how-are-conditional-access-policies-applied"></a>Hoe wordt beleid voor voorwaardelijke toegang toegepast?

Er kunnen meer dan één beleid voor voorwaardelijke toegang van toepassing zijn wanneer u een Cloud-app opent. In dit geval moet aan alle beleids regels worden voldaan. Als voor het ene beleid bijvoorbeeld MFA is vereist en de tweede een compatibel apparaat vereist, moet u MFA door lopen en een compatibel apparaat gebruiken. 

Alle beleids regels worden in twee fasen afgedwongen:

- In de **eerste** fase worden alle beleids regels geëvalueerd en worden alle toegangs controles die niet worden voldaan, verzameld. 

- In de **tweede** fase wordt u gevraagd om te voldoen aan de vereisten die u niet hebt vervuld. Als een van de beleids regels toegang blokkeert, wordt u geblokkeerd en wordt niet gevraagd om te voldoen aan andere beleids elementen. Als geen van de beleids regels u blokkeert, wordt u gevraagd om aan andere beleids besturings elementen te voldoen in de volgende volg orde:

   ![Bestellen](./media/best-practices/06.png)
    
   Externe MFA-providers en gebruiks voorwaarden komen nu voor.

### <a name="how-are-assignments-evaluated"></a>Hoe worden toewijzingen geëvalueerd?

Alle toewijzingen zijn logisch **ANDed**. Als u meer dan één toewijzing hebt geconfigureerd, moeten aan alle toewijzingen worden voldaan om een beleid te activeren.  

Als u een locatie voorwaarde wilt configureren die van toepassing is op alle verbindingen die buiten het netwerk van uw organisatie zijn gemaakt:

- **Alle locaties** toevoegen
- **Alle vertrouwde IP-adressen** uitsluiten

### <a name="what-to-do-if-you-are-locked-out-of-the-azure-ad-admin-portal"></a>Wat moet ik doen als ik de Azure AD-beheer Portal vergrendeld?

Als u geen toegang meer hebt tot de Azure AD-Portal vanwege een onjuiste instelling in een beleid voor voorwaardelijke toegang:

- Controleren of er andere beheerders in uw organisatie zijn die nog niet zijn geblokkeerd. Een beheerder met toegang tot de Azure Portal kan het beleid dat van invloed is op uw aanmelding, uitschakelen. 
- Als geen van de beheerders in uw organisatie het beleid kan bijwerken, moet u een ondersteunings aanvraag indienen. Micro soft ondersteuning kan beleids regels voor voorwaardelijke toegang controleren en bijwerken die de toegang verhinderen.

### <a name="what-happens-if-you-have-policies-in-the-azure-classic-portal-and-azure-portal-configured"></a>Wat gebeurt er als u beleid hebt in de klassieke Azure-Portal en Azure Portal geconfigureerd?  

Beide beleids regels worden afgedwongen door Azure Active Directory en de gebruiker krijgt alleen toegang wanneer aan alle vereisten wordt voldaan.

### <a name="what-happens-if-you-have-policies-in-the-intune-silverlight-portal-and-the-azure-portal"></a>Wat gebeurt er als u beleid hebt in de intune Silverlight-Portal en de Azure Portal?

Beide beleids regels worden afgedwongen door Azure Active Directory en de gebruiker krijgt alleen toegang wanneer aan alle vereisten wordt voldaan.

### <a name="what-happens-if-i-have-multiple-policies-for-the-same-user-configured"></a>Wat gebeurt er als er meerdere beleids regels zijn voor dezelfde gebruiker die is geconfigureerd?  

Voor elke aanmelding evalueert Azure Active Directory alle beleids regels en zorgt ervoor dat aan alle vereisten wordt voldaan voordat toegang wordt verleend aan de gebruiker. Toegangs troefs voor alle andere configuratie-instellingen blok keren. 

### <a name="does-conditional-access-work-with-exchange-activesync"></a>Werkt voorwaardelijke toegang met Exchange ActiveSync?

Ja, u kunt Exchange ActiveSync in een beleid voor voorwaardelijke toegang gebruiken met enkele [beperkingen](block-legacy-authentication.md). 

### <a name="how-should-you-configure-conditional-access-with-office-365-apps"></a>Hoe moet u voorwaardelijke toegang configureren met Office 365-apps?

Omdat Office 365-apps onderling zijn verbonden, raden we u aan om veelgebruikte apps samen toe te wijzen bij het maken van beleid.

Veelgebruikte, onderling verbonden toepassingen zijn onder andere Microsoft Flow, micro soft planner, micro soft teams, Office 365 Exchange Online, Office 365 share point online en Office 365 Yammer.

Het is belang rijk voor beleids regels waarvoor gebruikers interacties nodig zijn, zoals multi-factor Authentication, wanneer de toegang aan het begin van een sessie of taak wordt geregeld. Als u dat niet doet, kunnen gebruikers sommige taken in een app niet volt ooien. Als u bijvoorbeeld multi-factor Authentication op onbeheerde apparaten nodig hebt om toegang te krijgen tot share point, maar niet via e-mail, kunnen gebruikers in hun e-mail share Point-bestanden niet aan een bericht toevoegen. Meer informatie vindt u in het artikel, [Wat zijn service afhankelijkheden in azure Active Directory voorwaardelijke toegang?](service-dependencies.md).

## <a name="what-you-should-avoid-doing"></a>Wat u moet vermijden

Het Framework voor voorwaardelijke toegang biedt een uitstekende configuratie flexibiliteit. Een fantastische flexibiliteit betekent echter ook dat u elk configuratie beleid zorgvuldig moet door nemen voordat het wordt vrijgegeven om ongewenste resultaten te voor komen. In deze context moet u speciale aandacht schenken aan toewijzingen die van invloed zijn op volledige sets, zoals **alle gebruikers/groepen/Cloud-apps**.

In uw omgeving moet u de volgende configuraties voor komen:

**Voor alle gebruikers alle Cloud-apps:**

- **Toegang blok keren** : deze configuratie blokkeert uw hele organisatie. Dit is niet een goed idee.
- **Compatibel apparaat vereisen** : voor gebruikers die hun apparaten nog niet hebben Inge schreven, blokkeert dit beleid alle toegang, inclusief toegang tot de intune-Portal. Als u een beheerder bent zonder een Inge schreven apparaat, blokkeert dit beleid u om weer toegang te krijgen tot de Azure Portal om het beleid te wijzigen.
- **Lid worden van een domein vereisen** : dit beleid blok keren toegang biedt ook de mogelijkheid om toegang te blok keren voor alle gebruikers in uw organisatie als u nog geen apparaat hebt toegevoegd aan een domein.
- **Beleid voor app-beveiliging vereisen** : dit beleid blokkeert heeft ook de mogelijkheid om toegang te blok keren voor alle gebruikers in uw organisatie als u niet beschikt over een intune-beleid. Als u een beheerder bent zonder een client toepassing die een intune-beleid voor app-beveiliging heeft, blokkeert dit beleid u om weer toegang te krijgen tot portals zoals intune en Azure.

**Voor alle gebruikers alle Cloud-apps, alle platformen:**

- **Toegang blok keren** : deze configuratie blokkeert uw hele organisatie. Dit is niet een goed idee.

## <a name="how-should-you-deploy-a-new-policy"></a>Hoe moet u een nieuw beleid implementeren?

Als eerste stap moet u uw beleid evalueren met het [hulp programma What if](what-if-tool.md).

Wanneer nieuwe beleids regels gereed zijn voor uw omgeving, implementeert u deze in fasen:

1. Pas een beleid toe op een kleine groep gebruikers en controleer of het werkt zoals verwacht. 
1. Wanneer u een beleid uitbreidt om meer gebruikers toe te voegen. Ga door met het uitsluiten van alle beheerders uit het beleid om er zeker van te zijn dat ze nog steeds toegang hebben en een beleid kunnen bijwerken als er een wijziging is vereist.
1. Pas een beleid alleen op alle gebruikers toe als dat nodig is. 

Maak als best practice een gebruikers account met de volgende opties:

- Toegewezen aan beleids beheer 
- Uitgesloten van al uw beleids regels

## <a name="policy-migration"></a>Migratie van beleid

U kunt de beleids regels die u niet hebt gemaakt, migreren in de Azure Portal omdat:

- U kunt nu scenario's aanpakken die u niet eerder kon afhandelen.
- U kunt het aantal beleids regels beperken dat u moet beheren door ze te consolideren.   
- U kunt al uw beleids regels voor voorwaardelijke toegang beheren op één centrale locatie.
- De klassieke Azure-Portal is buiten gebruik gesteld.   

Zie [klassiek beleid migreren in de Azure Portal](policy-migration.md)voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

Als u wilt weten:

- Hoe u een beleid voor voorwaardelijke toegang configureert, Zie [MFA vereisen voor specifieke apps met Azure Active Directory voorwaardelijke toegang](app-based-mfa.md).
- Hoe u uw beleid voor voorwaardelijke toegang plant, Zie [de implementatie van voorwaardelijke toegang plannen in azure Active Directory](plan-conditional-access.md).
