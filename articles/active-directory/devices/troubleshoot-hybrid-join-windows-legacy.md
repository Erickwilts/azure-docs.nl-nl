---
title: Het oplossen van hybride Azure Active Directory gekoppelde downlevel-apparaten | Microsoft Docs
description: Het oplossen van hybride Azure Active Directory gekoppelde downlevel-apparaten.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: troubleshooting
ms.date: 06/28/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jairoc
ms.collection: M365-identity-device-management
ms.openlocfilehash: c7f02937555f7637a6d2f81be717aaad83bab74f
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67481459"
---
# <a name="troubleshooting-hybrid-azure-active-directory-joined-down-level-devices"></a>Het oplossen van hybride Azure Active Directory gekoppelde downlevel-apparaten 

In dit artikel is alleen van toepassing op de volgende apparaten: 

- Windows 7 
- Windows 8.1 
- Windows Server 2008 R2 
- Windows Server 2012 
- Windows Server 2012 R2 

Zie voor Windows 10 of Windows Server 2016, [probleemoplossing hybride Azure Active Directory gekoppelde Windows 10 en Windows Server 2016-apparaten](troubleshoot-hybrid-join-windows-current.md).

In dit artikel wordt ervan uitgegaan dat u hebt [geconfigureerde hybride Azure Active Directory gekoppelde apparaten](hybrid-azuread-join-plan.md) ter ondersteuning van de volgende scenario's:

- Voorwaardelijke toegang op basis van apparaat

In dit artikel biedt hulp bij probleemoplossing voor het oplossen van problemen.  

**Wat u moet weten:** 

- Lid worden van hybride Azure AD voor downlevel Windows-apparaten werkt net iets anders dan dat het wordt in Windows 10. Veel klanten doen niet realiseren dat ze nodig hebben AD FS (voor federatieve domeinen) of naadloze eenmalige aanmelding (voor beheerde domeinen) geconfigureerd.
- Voor klanten met federatieve domeinen, als de Service Connection Point (SCP) is zodanig geconfigureerd dat deze verwijst naar de naam van het beheerde domein (bijvoorbeeld contoso.onmicrosoft.com, in plaats van contoso.com), wordt klikt u vervolgens hybride Azure AD Join voor downlevel Windows-apparaten werkt niet.
- Het maximum aantal apparaten per gebruiker momenteel geldt ook voor downlevel-apparaten met hybride Azure AD-domein. 
- Hetzelfde fysieke apparaat wordt meerdere keren in Azure AD wanneer meerdere gebruikers van een domein aanmelden downlevel hybride Azure AD gekoppelde apparaten weergegeven.  Bijvoorbeeld, als *jdoe* en *jharnett* aanmelden bij een apparaat, een afzonderlijke inschrijving (DeviceID) wordt gemaakt voor elk van deze in de **gebruiker** tabblad info. 
- U kunt ook meerdere vermeldingen voor een apparaat op het tabblad van de gebruiker gegevens ophalen vanwege een nieuwe installatie van het besturingssysteem of handmatig opnieuw registreren.
- De registratie / join van apparaten is geconfigureerd voor het uitvoeren van een poging tot aanmelden of vergrendelen / ontgrendelen. Het is mogelijk dat er 5 minuten vertraging geactiveerd door een scheduler-taak. 
- Zorg ervoor dat [KB4284842](https://support.microsoft.com/help/4284842) is geïnstalleerd, in het geval van Windows 7 SP1 of Windows Server 2008 R2 SP1. Deze update voorkomt toekomstige authenticatiefouten vanwege verlies van de klant toegang tot beveiligde sleutels na het wijzigen van wachtwoord.

## <a name="step-1-retrieve-the-registration-status"></a>Stap 1: De registratiestatus ophalen 

**Om te controleren of de registratiestatus:**  

1. Aanmelden met het gebruikersaccount dat een hybride Azure AD join is uitgevoerd.
1. Open de opdrachtprompt 
1. Type `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe" /i`

Met deze opdracht wordt het dialoogvenster waarmee u meer informatie over de join-status weergegeven.

![Workplace Join voor Windows](./media/troubleshoot-hybrid-join-windows-legacy/01.png)

## <a name="step-2-evaluate-the-hybrid-azure-ad-join-status"></a>Stap 2: De hybride Azure AD join-status evalueren 

Als het apparaat niet toegevoegd aan hybrid Azure AD is, kunt u proberen te doen van hybride Azure AD join door te klikken op de knop 'Join'. Als de poging tot het uitvoeren van hybride Azure AD join is mislukt, worden de details over de fout weergegeven.

**De meest voorkomende problemen zijn:**

- Een onjuist geconfigureerd voor AD FS of Azure AD of netwerk problemen

    ![Workplace Join voor Windows](./media/troubleshoot-hybrid-join-windows-legacy/02.png)
    
   - Autoworkplace.exe is niet op de achtergrond verifiëren met Azure AD of AD FS. Dit wordt mogelijk veroorzaakt door ontbrekende of verkeerd geconfigureerd AD FS (voor federatieve domeinen) of ontbreekt of is onjuist geconfigureerde Azure AD naadloze eenmalige aanmelding (voor beheerde domeinen) of netwerkproblemen. 
   - Het kan zijn dat multi-factor authentication (MFA) ingeschakeld/geconfigureerd voor de gebruiker is en WIAORMULTIAUTHN niet is geconfigureerd op de AD FS-server. 
   - Een andere mogelijkheid is dat thuisdomein detectie (HRD)-pagina wordt gewacht tot tussenkomst van de gebruiker, waardoor **autoworkplace.exe** uit op de achtergrond aanvragen van een token.
   - Kan het zijn dat AD FS en Azure AD-URL's in de intranetzone van Internet Explorer op de client ontbreken.
   - Problemen met de netwerkverbinding kunnen verhinderen **autoworkplace.exe** van AD FS of de Azure AD-URL's wordt bereikt. 
   - **Autoworkplace.exe** vereist dat de client hebt directe verbinding van de client naar de organisatie on-premises AD-domeincontroller, wat betekent dat hybrid Azure AD join slaagt alleen wanneer de client is verbonden met het intranet van de organisatie.
   - Uw organisatie gebruikt Azure AD naadloze eenmalige aanmelding, `https://autologon.microsoftazuread-sso.com` of `https://aadg.windows.net.nsatc.net` bevinden zich niet op van het apparaat-instellingen voor het intranet van Internet Explorer, en **toestaan van updates op de statusbalk via script** is niet ingeschakeld voor de intranetzone.
- U bent niet aangemeld op als een domeingebruiker

   ![Workplace Join voor Windows](./media/troubleshoot-hybrid-join-windows-legacy/03.png)

   Er zijn enkele verschillende redenen waarom dit kan gebeuren:

   - De aangemelde gebruiker is niet een domeingebruiker (bijvoorbeeld een lokale gebruiker). Hybride Azure AD join op downlevel-apparaten wordt alleen ondersteund voor gebruikers van een domein.
   - De client kan geen verbinding maken met een domeincontroller.    
- Een quotum is bereikt

    ![Workplace Join voor Windows](./media/troubleshoot-hybrid-join-windows-legacy/04.png)

- De service reageert niet 

    ![Workplace Join voor Windows](./media/troubleshoot-hybrid-join-windows-legacy/05.png)

U kunt de statusinformatie ook vinden in het gebeurtenislogboek onder: **Toepassingen en Services Log\Microsoft-werkplek koppelen**
  
**De meest voorkomende oorzaken voor een mislukte hybride Azure AD join zijn:** 

- Uw computer niet is verbonden met het interne netwerk van uw organisatie of met een VPN via een verbinding met uw on-premises AD-domeincontroller.
- U bent aangemeld met een lokaal computeraccount op de computer. 
- Problemen met de configuratie van service: 
   - De AD FS-server niet is geconfigureerd ter ondersteuning van **WIAORMULTIAUTHN**. 
   - De forest van uw computer heeft geen Service Connection Point-object dat naar de naam van uw geverifieerde domeinnaam in Azure AD verwijst 
   - Of als uw domein wordt beheerd, klikt u naadloze eenmalige aanmelding is niet geconfigureerd of werkt.
   - Een gebruiker heeft het maximale aantal apparaten bereikt. 

## <a name="next-steps"></a>Volgende stappen

Voor vragen, Zie de [Apparaatbeheer Veelgestelde vragen](faq.md)  
