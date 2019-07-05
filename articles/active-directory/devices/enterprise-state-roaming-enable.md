---
title: Enterprise State Roaming in Azure Active Directory inschakelen | Microsoft Docs
description: Veelgestelde vragen over Enterprise State Roaming-instellingen in het Windows-apparaten.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: na
ms.collection: M365-identity-device-management
ms.openlocfilehash: 45c1fc6340df6a5400864b2e1222a2c65e586232
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67482031"
---
# <a name="enable-enterprise-state-roaming-in-azure-active-directory"></a>Enterprise state roaming inschakelen in Azure Active Directory
Enterprise State Roaming is beschikbaar voor elke organisatie met een Azure AD Premium of Enterprise Mobility + Security (EMS)-licentie. Zie voor meer informatie over het verkrijgen van een Azure AD-abonnement, de [productpagina van Azure AD](https://azure.microsoft.com/services/active-directory).

Wanneer u de Enterprise State Roaming inschakelt, wordt uw organisatie een gratis, beperkt gebruik van een licentie voor Azure Rights Management-beveiliging automatisch verleend van Azure Information Protection. Dit gratis abonnement is beperkt tot het versleutelen en ontsleutelen van de instellingen van de onderneming en toepassingsgegevens gesynchroniseerd door Enterprise State Roaming. U moet hebben [een betaald abonnement](https://azure.microsoft.com/pricing/details/information-protection/) gebruik van de volledige functionaliteit van de Azure Rights Management-service.

## <a name="to-enable-enterprise-state-roaming"></a>Enterprise State Roaming inschakelen

1. Aanmelden bij [Azure AD-beheercentrum](https://aad.portal.azure.com/).
1. Selecteer **Azure Active Directory** &gt; **apparaten** &gt; **Enterprise State Roaming**.
1. Selecteer **gebruikers kunnen instellingen en app-gegevens synchroniseren via apparaten**. Zie voor meer informatie, [apparaatinstellingen configureren](https://docs.microsoft.com/azure/active-directory/device-management-azure-portal).
  
   ![afbeelding van de instelling van de apparaten met het label gebruikers kan instellingen en app-gegevens synchroniseren via apparaten](./media/enterprise-state-roaming-enable/device-settings.png)
  
Het apparaat moet verifiëren met behulp van een Azure AD-identiteit voor een Windows 10-apparaat om de service Enterprise State Roaming te gebruiken. Voor apparaten die zijn gekoppeld aan Azure AD, is identiteit van de gebruiker primaire aanmelding hun Azure AD-identiteit, zodat er geen aanvullende configuratie vereist. Voor apparaten die gebruikmaken van on-premises Active Directory, de IT-beheerder moet [configureren hybride Azure Active Directory gekoppelde apparaten](https://docs.microsoft.com/azure/active-directory/devices/hybrid-azuread-join-manual-steps). 

## <a name="data-storage"></a>Gegevensopslag
Enterprise State Roaming gegevens worden gehost in een of meer [Azure-regio's](https://azure.microsoft.com/regions/) dat beste zijn afgestemd op de waarde land/regio in de Azure Active Directory-exemplaar. Enterprise State Roaming gegevens zijn gepartitioneerd op basis van drie belangrijke geografische regio's: Noord-Amerika, EMEA en APAC. Enterprise State Roaming gegevens voor de tenant is lokaal bevindt als de geografische regio, en wordt niet gerepliceerd tussen regio's.  Bijvoorbeeld:

| Land/regio waarde | heeft de gegevens die worden gehost in |
| -------------------- | ------------------------ |
| Een EMEA-land/regio, zoals Frankrijk of Zambia | Een of meer van de Azure-regio's in Europa |
| Een Noord-Amerikaanse land/regio, zoals de Verenigde Staten of Canada | een of meer van de Azure-regio's binnen de VS |
| Een APAC land/regio, zoals Australië en Nieuw-Zeeland | een of meer van de Azure-regio's in Azië |
| Regio's Zuid-Amerika en Antarctica | een of meer Azure-regio's binnen de VS |

De waarde van het land/regio is ingesteld als onderdeel van het proces voor het maken van Azure AD directory en vervolgens kan niet worden gewijzigd. Als u meer informatie op uw locatie voor de gegevensopslag, een ticket indienen bij [ondersteuning van Azure](https://azure.microsoft.com/support/options/).

## <a name="view-per-user-device-sync-status"></a>De apparaatstatus synchronisatie per gebruiker weergeven
Volg deze stappen om een statusrapport voor synchronisatie van per gebruiker-apparaat weer te geven.

1. Aanmelden bij [Azure AD-beheercentrum](https://aad.portal.azure.com/).
1. Selecteer **Azure Active Directory** &gt; **gebruikers** &gt; **alle gebruikers**.
1. Selecteer de gebruiker en selecteer vervolgens **apparaten**.
1. Onder **weergeven**, selecteer **synchronisatie-instellingen en app-gegevens voor apparaten** om weer te geven van de synchronisatiestatus.
  
   ![afbeelding van de gegevens van de Apparaatinstelling synchroniseren](./media/enterprise-state-roaming-enable/sync-status.png)
  
1. Als er apparaten synchroniseren voor deze gebruiker, ziet u de apparaten, zoals hier wordt weergegeven.
  
   ![afbeelding van het apparaat synchroniseren kolomgegevens](./media/enterprise-state-roaming-enable/device-status-row.png)

## <a name="data-retention"></a>Bewaartijd van gegevens
Gegevens die zijn gesynchroniseerd met de Microsoft-cloud met behulp van Enterprise State Roaming worden bewaard totdat deze handmatig wordt verwijderd of de desbetreffende gegevens blijkt dat het verouderd. 

### <a name="explicit-deletion"></a>Expliciet verwijderen
Expliciet verwijderen is als een Azure-beheerder Hiermee verwijdert u een gebruiker of een map of anders expliciet aanvragen dat gegevens worden verwijderd.

* **Gebruiker verwijderen**: Wanneer een gebruiker wordt verwijderd in Azure AD, wordt het gebruikersaccount roaminggegevens na 90 tot 180 dagen verwijderd. 
* **Map verwijderen**: Het verwijderen van een volledige map in Azure AD is een directe bewerking. Alle instellingsgegevens die zijn gekoppeld aan die directory na 90 tot 180 dagen wordt verwijderd. 
* **Op aanvraag verwijderen**: Als de Azure AD-beheerder wil handmatig verwijderen van de gegevens of instellingsgegevens van een specifieke gebruiker, de beheerder kan een ticket indienen bij [ondersteuning van Azure](https://azure.microsoft.com/support/). 

### <a name="stale-data-deletion"></a>Verouderde gegevens verwijderen
Gegevens die niet is geopend voor één jaar ("de bewaarperiode') wordt beschouwd als verouderd en kan worden verwijderd uit de Microsoft-cloud. De bewaarperiode kan worden gewijzigd, maar is niet minder dan 90 dagen. De verouderde gegevens mogelijk een specifieke set Windows/toepassings- of alle instellingen voor een gebruiker. Bijvoorbeeld:

* Als apparaten geen toegang een verzameling bepaalde instellingen tot (bijvoorbeeld een toepassing van het apparaat is verwijderd of een instellingengroep zoals 'Thema' is uitgeschakeld voor alle apparaten van een gebruiker), en vervolgens deze verzameling verlopen na de bewaarperiode is en kan worden verwijderd . 
* Als een gebruiker is uitgeschakeld in instellingen synchroniseren op al hun apparaten, klikt u vervolgens geen van de instellingsgegevens worden geopend en de van instellingsgegevens voor die gebruiker wordt verlopen en na de bewaarperiode kan worden verwijderd. 
* Als de Azure AD-directory-beheerder uitgeschakeld Enterprise State Roaming voor de hele map, klikt u vervolgens alle gebruikers wordt in die map wordt stoppen van de synchronisatie van instellingen en alle instellingsgegevens in de voor alle gebruikers zullen verlopen en na de bewaarperiode kan worden verwijderd. 

### <a name="deleted-data-recovery"></a>Verwijderde gegevens herstellen
Het bewaarbeleid voor gegevens kan niet worden geconfigureerd. Zodra de gegevens worden definitief verwijderd, kan het niet worden hersteld. Echter de instellingsgegevens alleen verwijderd uit de Microsoft-cloud, niet van de apparaten van eindgebruikers. Als een apparaat opnieuw verbinding later opnieuw met de service Enterprise State Roaming maakt, worden de instellingen opnieuw gesynchroniseerd en opgeslagen in de Microsoft cloud.

## <a name="next-steps"></a>Volgende stappen

* [Overzicht van Enterprise State Roaming](enterprise-state-roaming-overview.md)
* [Instellingen en gegevensroaming Veelgestelde vragen](enterprise-state-roaming-faqs.md)
* [Instellingen voor beleid en MDM voor synchronisatie van instellingen](enterprise-state-roaming-group-policy-settings.md)
* [Windows 10 naslaginformatie over roaminginstellingen voor](enterprise-state-roaming-windows-settings-reference.md)
* [Problemen oplossen](enterprise-state-roaming-troubleshooting.md)
