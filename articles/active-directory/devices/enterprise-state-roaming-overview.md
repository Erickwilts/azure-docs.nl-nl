---
title: Wat is enterprise state roaming in Azure Active Directory? | Microsoft Docs
description: Enterprise State Roaming biedt gebruikers een uniforme ervaring op hun Windows-apparaten en vermindert de tijd die nodig is voor het configureren van een nieuw apparaat.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: overview
ms.date: 06/28/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: na
ms.collection: M365-identity-device-management
ms.openlocfilehash: c5b60970592180a2353860369e637d4b9a9bb8f9
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67481914"
---
# <a name="what-is-enterprise-state-roaming"></a>Wat is Enterprise State Roaming?

Met Windows 10, [Azure Active Directory (Azure AD)](../fundamentals/active-directory-whatis.md) gebruikers krijgen de mogelijkheid hun gebruikersinstellingen en gegevens van de toepassing-instellingen naar de cloud veilig te synchroniseren. Enterprise State Roaming biedt gebruikers een uniforme ervaring op hun Windows-apparaten en vermindert de tijd die nodig is voor het configureren van een nieuw apparaat. Enterprise State Roaming werkt die vergelijkbaar is met de standaard [consumer-instellingen synchroniseren](https://go.microsoft.com/fwlink/?linkid=2015135) die is geïntroduceerd in Windows 8. Daarnaast biedt Enterprise State Roaming:

* **Scheiding van zakelijke en consumentgegevens** : organisaties zijn controle over hun gegevens en er is geen combinatie van zakelijke gegevens in een cloudopslagaccount consument of consumentgegevens in een enterprise cloud-account.
* **Verbeterde beveiliging** : gegevens worden automatisch versleuteld vóór het verlaten van de gebruiker Windows 10-apparaat met behulp van Azure Rights Management (Azure RMS) en gegevens blijven versleuteld in rust in de cloud. Alle inhoud blijft versleuteld in rust in de cloud, met uitzondering van de naamruimten, zoals de namen van de instellingen en Windows-app.  
* **Beheer en controle voor een betere** – controle en inzicht biedt ten opzichte van die worden gesynchroniseerd met de instellingen in uw organisatie en op welke apparaten via de integratie van Azure AD-portal. 

Enterprise State Roaming is beschikbaar in meerdere Azure-regio's. U kunt de bijgewerkte lijst met beschikbare regio's vinden op de [Azure-Services per regio's](https://azure.microsoft.com/regions/#services) pagina onder Azure Active Directory.

| Artikel | Description |
| --- | --- |
| [Enterprise State Roaming in Azure Active Directory inschakelen](enterprise-state-roaming-enable.md) |Enterprise State Roaming is beschikbaar voor elke organisatie met een Premium Azure Active Directory (Azure AD)-abonnement. Zie voor meer informatie over het verkrijgen van een Azure AD-abonnement, de [Azure AD-product](https://azure.microsoft.com/services/active-directory) pagina. |
| [Instellingen en gegevensroaming Veelgestelde vragen](enterprise-state-roaming-faqs.md) |In dit onderwerp vindt u antwoorden op enkele vragen die IT-beheerders mogelijk over de instellingen en app-gegevens synchroniseren. |
| [Groep beleid en de MDM-instellingen voor synchronisatie van instellingen](enterprise-state-roaming-group-policy-settings.md) |Windows 10 biedt Groepsbeleid en beheer van mobiele apparaten (MDM)-beleidsinstellingen om te beperken van synchronisatie van instellingen. |
| [Windows 10 naslaginformatie over roaminginstellingen voor](enterprise-state-roaming-windows-settings-reference.md) |Hier volgt een volledige lijst van alle instellingen die zal worden zwerven en/of back-up in Windows 10. |
| [Problemen oplossen](enterprise-state-roaming-troubleshooting.md) |Dit onderwerp doorloopt enkele eenvoudige stappen voor het oplossen van problemen en bevat een lijst met bekende problemen. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het inschakelen van enterprise state roaming [enterprise state roaming inschakelen](enterprise-state-roaming-enable.md).
