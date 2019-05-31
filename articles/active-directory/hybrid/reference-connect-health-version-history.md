---
title: Versiegeschiedenis van Azure AD Connect Health
description: Dit document beschrijft de versies voor Azure AD Connect Health en wat is opgenomen in deze versies.
services: active-directory
documentationcenter: ''
author: zhiweiwangmsft
manager: daveba
editor: curtand
ms.assetid: 8dd4e998-747b-4c52-b8d3-3900fe77d88f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 03/20/2019
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 58de8de9a9fab67d743d4560ccda037d8f41301b
ms.sourcegitcommit: 8c49df11910a8ed8259f377217a9ffcd892ae0ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/29/2019
ms.locfileid: "66298990"
---
# <a name="azure-ad-connect-health-version-release-history"></a>Azure AD Connect Health: Releasegeschiedenis van versie
Azure AD Connect Health het team van Azure Active Directory regelmatig bijgewerkt met nieuwe functies en functionaliteit. In dit artikel bevat de versies en functies die zijn uitgebracht.  

> [!NOTE]
> Connect Health-agents automatisch bijgewerkt worden wanneer nieuwe versie wordt uitgebracht. Controleer of de instellingen voor automatische clientupdate is ingeschakeld in Azure portal. 
>

Azure AD Connect Health for Sync is geïntegreerd met Azure AD Connect-installatie. Meer informatie over [releasegeschiedenis van Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-version-history) voor feedback van de functie, de stemmen op [verbinding maken met de gezondheid van User Voice-kanaal](https://feedback.azure.com/forums/169401-azure-active-directory/filters/new?category_id=165591)


## <a name="may-2019"></a>Mei 2019
**Agent bijwerken:** 
* Azure AD Connect Health-agent voor AD FS (versie 3.1.51.0) 
   1. Opgelost probleem onderscheid maken tussen meerdere aanmeldingen die dezelfde client-request-id delen.
   2. Opgelost probleem onjuiste gebruikersnaam en wachtwoord op taal fouten parseren gelokaliseerd servers.   

## <a name="april-2019"></a>April 2019
**Agent bijwerken:** 
* Azure AD Connect Health-agent voor AD FS (versie 3.1.46.0) 
   1. Waarschuwingen controleren op dubbele SPN-proces voor AD FS oplossen

## <a name="march-2019"></a>Maart 2019
**Agent bijwerken:** 
* Azure AD Connect Health-agent voor AD DS (versie 3.1.41.0)  
   1. Verzameling van .NET-versie
   2. Verbetering van het verzamelen van prestatiemeteritems wanneer bepaalde categorieën ontbreekt
   3. Opgelost probleem op te voorkomen dat bij het maken van meerdere exemplaren van de Monitoring Agent

* Azure AD Connect Health-agent voor AD FS (versie 3.1.41.0) 
   1. Integreren en upgrade uitvoeren van scripts voor AD FS testen met behulp van ADFSToolBox
   2. .NET versie verzameling implementeren
   3. Verbetering van het verzamelen van prestatiemeteritems wanneer bepaalde categorieën ontbreekt
   4. Opgelost probleem op te voorkomen dat bij het maken van meerdere exemplaren van de Monitoring Agent


## <a name="november-2018"></a>November 2018
**Nieuwe functies voor algemene beschikbaarheid:** 
* Azure AD Connect Health for Sync - vaststellen en herstellen van de synchronisatiefouten dubbel kenmerk uit de portal

**Agent bijwerken:** 
* Azure AD Connect Health-agent voor AD DS (versie 3.1.24.0) 
   1. Transport Layer Security (TLS)-protocol versie 1.2 naleving en gedwongen uitvoering
   2. Globale catalogus waarschuwingsruis te verminderen
   3. Health-agent registratie oplossingen voor problemen

* Azure AD Connect Health-agent voor AD FS (versie 3.1.24.0)  
   1. Transport Layer Security (TLS)-protocol versie 1.2 naleving en gedwongen uitvoering
   2. Ondersteuning van Test-ADFSRequestToken voor gelokaliseerd besturingssysteem
   3. Diagnostische agent EventHandler vergrendelingsprobleem opgelost
   4. Health-agent registratie oplossingen voor problemen

## <a name="august-2018"></a>Augustus 2018 
*  Azure AD Connect Health-agent voor synchronisatie (versie 3.1.7.0) die zijn uitgebracht met Azure AD Connect versie 1.1.880.0    
   1. Hotfix voor [releases van hoge CPU-probleem van bewakingsagent met .NET Framework KB](https://support.microsoft.com/help/4346822/high-cpu-issue-in-azure-active-directory-connect-health-for-sync)

## <a name="june-2018"></a>Juni 2018 
**Nieuwe preview-functies:** 
* Azure AD Connect Health for Sync - vaststellen en herstellen van de synchronisatiefouten dubbel kenmerk uit de portal 

**Agent bijwerken:** 
* Azure AD Connect Health-agent voor AD DS (versie 3.1.7.0)    
  1. Hotfix voor [releases van hoge CPU-probleem van bewakingsagent met .NET Framework KB](https://support.microsoft.com/help/4346822/high-cpu-issue-in-azure-active-directory-connect-health-for-sync)
   
* Azure AD Connect Health-agent voor AD FS (versie 3.1.7.0)  
  1. Hotfix voor [releases van hoge CPU-probleem van bewakingsagent met .NET Framework KB](https://support.microsoft.com/help/4346822/high-cpu-issue-in-azure-active-directory-connect-health-for-sync)
  2. Testresultaten van oplossingen op de secundaire server voor AD FS-Server 2016
   
* Azure AD Connect Health-agent voor AD FS (versie 3.1.2.0)  
  1. Hotfix voor agentbeheer van geheugen en gerelateerde waarschuwingen specifiek is voor versie 3.0.244.0


## <a name="may-2018"></a>Mei 2018
**Agent bijwerken:**
* Azure AD Connect Health-agent voor AD DS (versie 3.0.244.0)
  1. Verbetering van de agent-privacy  
  2. Algemene verbeteringen en oplossingen voor problemen

* Azure AD Connect Health-agent voor AD FS (versie 3.0.244.0)
  1. Diagnostics-Agent-Service en gerelateerde PowerShell-module verbeteringen
  2. Verbetering van de agent-privacy  
  3. Algemene verbeteringen en oplossingen voor problemen

* Azure AD Connect Health-agent voor synchronisatie (versie 3.0.164.0) die zijn uitgebracht met Azure AD Connect versie 1.1.819.0 
  1. Verbetering van de agent-privacy  
  2. Algemene verbeteringen en oplossingen voor problemen


## <a name="march-2018"></a>Maart 2018
**Nieuwe preview-functies:**
* Azure AD Connect Health voor AD FS - rapport riskant IP-adres en de waarschuwing.

**Agent bijwerken:**

* Azure AD Connect Health-agent voor AD DS (versie 3.0.176.0)
  1. Verbeteringen van de agent-beschikbaarheid 
  2. Algemene verbeteringen en oplossingen voor problemen
* Azure AD Connect Health-agent voor AD FS (versie 3.0.176.0)
  1. Verbeteringen van de agent-beschikbaarheid 
  2. Algemene verbeteringen en oplossingen voor problemen
* Azure AD Connect Health-agent voor synchronisatie (versie 3.0.129.0) die zijn uitgebracht met Azure AD Connect versie 1.1.750.0  
  1. Verbeteringen van de agent-beschikbaarheid 
  2. Algemene verbeteringen en oplossingen voor problemen

## <a name="december-2017"></a>December 2017
**Agent bijwerken:**

* Azure AD Connect Health-agent voor AD DS (versie 3.0.145.0)
  1. Verbeteringen van de agent-beschikbaarheid 
  2. Nieuwe agent probleemoplossing opdrachten toegevoegd
  3. Algemene verbeteringen en oplossingen voor problemen
* Azure AD Connect Health-agent voor AD FS (versie 3.0.145.0)
  1. Nieuwe agent probleemoplossing opdrachten toegevoegd
  2. Verbeteringen van de agent-beschikbaarheid 
  3. Algemene verbeteringen en oplossingen voor problemen
  
## <a name="october-2017"></a>Oktober 2017
**Agent bijwerken:**

 * Azure AD Connect Health-agent voor synchronisatie (versie 3.0.129.0) die zijn uitgebracht met Azure AD Connect versie 1.1.649.0
<br></br> Er is een probleem met de versie compatibiliteit tussen Azure AD Connect en Azure AD Connect Health-Agent voor synchronisatie opgelost. Dit probleem is van invloed op klanten die Azure AD Connect in-place upgrade naar versie 1.1.647.0 uitvoert, maar heeft momenteel de Health-Agent versie 3.0.127.0. Na de upgrade kan de Health-Agent niet meer statusgegevens over Azure AD Connect-synchronisatieservice verzenden naar Azure AD Health-Service. Met deze oplossing is tijdens in-place upgrade van Azure AD Connect Health-Agent versie 3.0.129.0 geïnstalleerd. Health-Agent versie 3.0.129.0 heeft geen compatibiliteitsproblemen met Azure AD Connect versie 1.1.649.0.

## <a name="july-2017"></a>Juli 2017
**Agent bijwerken:**

* Azure AD Connect Health-agent voor AD DS (versie 3.0.68.0)
  1. Algemene verbeteringen en oplossingen voor problemen
  2. Ondersteuning voor onafhankelijke Clouds
* Azure AD Connect Health-agent voor AD FS (versie 3.0.68.0)
  1. Algemene verbeteringen en oplossingen voor problemen
  2. Ondersteuning voor onafhankelijke Clouds
* Azure AD Connect Health-agent voor synchronisatie (versie 3.0.68.0) die zijn uitgebracht met Azure AD Connect versie 1.1.614.0
  1. Ondersteuning voor Microsoft Azure Government-Cloud- en Microsoft Cloud Duitsland

## <a name="april-2017"></a>April 2017      
**Agent bijwerken:**

* Azure AD Connect Health-agent voor AD FS (versie 3.0.12.0)
  1. Algemene verbeteringen en oplossingen voor problemen
* Azure AD Connect Health-agent voor AD DS (versie 3.0.12.0)
  1. Prestatiemeteritems uploaden verbeteringen
  2. Algemene verbeteringen en oplossingen voor problemen

## <a name="october-2016"></a>Oktober 2016
**Agent bijwerken:**

* Azure AD Connect Health-agent voor AD FS (versie 2.6.408.0)
* Verbeteringen bij het detecteren van client-IP-adressen in verificatieaanvragen
* Oplossingen voor problemen met betrekking tot waarschuwingen
* Azure AD Connect Health-agent voor AD DS (versie 2.6.408.0)
* Oplossingen voor problemen met betrekking tot waarschuwingen.
* Azure AD Connect Health-agent voor synchronisatie (versie 2.6.353.0) die zijn uitgebracht met Azure AD Connect versie 1.1.281.0
* Geef de vereiste gegevens voor de synchronisatie-foutrapporten
* Oplossingen voor problemen met betrekking tot waarschuwingen

**Nieuwe preview-functies:**

* Synchronisatie-foutrapporten voor Azure AD Connect

**Nieuwe functies**

* Azure AD Connect Health voor AD FS - veld IP-adres is beschikbaar in het rapport over top 50 van gebruikers met een ongeldige gebruikersnaam/wachtwoord.

## <a name="july-2016"></a>Juli 2016
**Nieuwe preview-functies:**

* [Azure AD Connect Health voor AD DS](how-to-connect-health-adds.md).

## <a name="january-2016"></a>Januari 2016
**Agent bijwerken:**

* Azure AD Connect Health-agent voor AD FS (versie 2.6.91.1512)

**Nieuwe functies**

* [Hulpprogramma voor test-connectiviteit voor Azure AD Connect Health-Agents](how-to-connect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service)

## <a name="november-2015"></a>November 2015
**Nieuwe functies**

* Ondersteuning voor [op rollen gebaseerd toegangsbeheer](how-to-connect-health-operations.md#manage-access-with-role-based-access-control)

**Nieuwe preview-functies:**

* [Azure AD Connect Health voor synchroniseren](how-to-connect-health-sync.md).

**Opgeloste problemen:**

* Oplossingen voor fouten gezien tijdens agent-registraties.

## <a name="september-2015"></a>September 2015
**Nieuwe functies**

* Onjuiste gebruikersnaam wachtwoord rapport voor AD FS
* Ondersteuning voor niet-geverifieerde HTTP-Proxy configureren
* Ondersteuning voor het configureren van de agent op de Server core
* Verbeteringen in waarschuwingen voor AD FS
* Verbeteringen in Azure AD Connect Health-Agent voor AD FS voor de verbinding en gegevens uploaden.

**Opgeloste problemen:**

* Oplossingen voor problemen in inzicht in gebruik voor AD FS-fouttypen.

## <a name="june-2015"></a>Juni 2015
**Initiële versie van Azure AD Connect Health voor AD FS en AD FS-Proxy.**

**Nieuwe functies**

* Waarschuwingen voor het controleren van AD FS en AD FS-Proxy-servers met e-mailmeldingen.
* Eenvoudige toegang tot AD FS-topologie en patronen in AD FS-prestatiemeteritems.
* Trend in geslaagde token aanvragen op AD FS-servers die zijn gegroepeerd op toepassingen, verificatiemethoden, aanvragen netwerk locatie enzovoort.
* Trends in mislukte aanvragen op AD FS-servers die zijn gegroepeerd op toepassingen, fout typen enzovoort.
* Eenvoudigere implementatie van Agent met behulp van Azure AD-hoofdbeheerder referenties.  

## <a name="next-steps"></a>Volgende stappen
Meer informatie over [uw on-premises identity infrastructure en Synchronisatieservices controleren in de cloud](whatis-hybrid-identity-health.md).

