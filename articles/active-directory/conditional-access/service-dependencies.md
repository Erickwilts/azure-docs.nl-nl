---
title: Wat zijn serviceafhankelijkheden in Azure Active Directory voor voorwaardelijke toegang? | Microsoft Docs
description: Meer informatie over hoe voorwaarden worden gebruikt in Azure Active Directory voor voorwaardelijke toegang voor het activeren van een beleid.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: article
ms.date: 03/18/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: b9aca2e4ea5e107358ff72e83562057830ece2cc
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2019
ms.locfileid: "67509346"
---
# <a name="what-are-service-dependencies-in-azure-active-directory-conditional-access"></a>Wat zijn serviceafhankelijkheden in Azure Active Directory voor voorwaardelijke toegang? 

U kunt toegangsvereisten voor tot websites en services opgeven met beleid voor voorwaardelijke toegang. Bijvoorbeeld: uw toegangsvereisten meervoudige verificatie (MFA) vereisen kunnen opnemen of [beheerde apparaten](require-managed-devices.md). 

Wanneer u rechtstreeks toegang hebben tot een site of service, is de impact van een beleidsregel doorgaans gemakkelijk om vast te stellen. Als u een beleid dat MFA voor SharePoint Online geconfigureerd vereist hebt, kunt u voor MFA, wordt afgedwongen voor elke aanmelding bij de SharePoint-webportal. Het is echter niet altijd eenvoudig om te beoordelen van de impact van een beleid, omdat er cloud-apps met afhankelijkheden van andere cloud-apps. Microsoft Teams kunt bijvoorbeeld toegang tot bronnen in SharePoint Online bieden. Dus als u toegang hebben tot Microsoft Teams in onze huidige scenario, bent u ook onderhevig aan de SharePoint-MFA-beleid.   

## <a name="policy-enforcement"></a>Beleidsafdwinging 

Als u een serviceafhankelijkheid geconfigureerd hebt, kan het beleid worden toegepast met behulp van vroege gebonden of gekoppeld. 

- **Afdwingen van beleid voor vroege gebonden** betekent dat een gebruiker moet voldoen aan het beleid van de afhankelijke service voordat u toegang tot de aanroepende toepassing. Bijvoorbeeld, een gebruiker moet voldoen aan beleid voor SharePoint voordat u zich in MS Teams. 
- **Gekoppeld beleidsafdwinging** vindt plaats nadat de gebruiker zich bij de app aanroepen. Afdwinging is uitgesteld naar bij het aanroepen van app-aanvragen, een token voor de downstream-service. Voorbeelden zijn onder meer MS Teams toegang tot Planner en Office.com toegang tot SharePoint. 

Het volgende diagram ziet u serviceafhankelijkheden MS Teams. Effen de pijlen geven early-bound-afdwinging onderbroken pijl voor Planner gekoppeld afdwingen geeft. 

![Serviceafhankelijkheden MS Teams](./media/service-dependencies/01.png)

Als een best practice, moet u de algemene beleidsregels instellen voor gerelateerde apps en services waar mogelijk. Met een consistente beveiligingspostuur biedt u de beste gebruikerservaring. Bijvoorbeeld, minder instellen van een algemene beleid voor Exchange Online, SharePoint Online, Microsoft Teams en Skype voor bedrijven aanzienlijk onverwachte prompts die kunnen voortvloeien uit verschillende soorten beleid wordt toegepast op de downstream-services. 

De onderstaande tabel bevat aanvullende service-afhankelijkheden, waar de client-apps moeten voldoen aan  

| Client-apps         | Downstream-service                          | Afdwinging |
| :--                 | :--                                         | ---         | 
| Azure Data Lake     | Microsoft Azure-beheer (portal en API) | Vroege gebonden |
| Microsoft Classroom | Exchange                                    | Vroege gebonden |
|                     | SharePoint                                  | Vroege gebonden  |
| Microsoft Teams     | Exchange                                    | Vroege gebonden |
|                     | MS Planner                                  | Gekoppeld  |
|                     | SharePoint                                  | Vroege gebonden |
|                     | Skype voor Bedrijven Online                   | Vroege gebonden |
| Office-Portal       | Exchange                                    | Gekoppeld  |
|                     | SharePoint                                  | Gekoppeld  |
| Outlook-groepen      | Exchange                                    | Vroege gebonden |
|                     | SharePoint                                  | Vroege gebonden |
| PowerApps           | Microsoft Azure-beheer (portal en API) | Vroege gebonden |
|                     | Windows Azure Active Directory              | Vroege gebonden |
| Project             | Dynamics CRM                                | Vroege gebonden |
| Skype voor Bedrijven  | Exchange                                    | Vroege gebonden |
| Visual Studio       | Microsoft Azure-beheer (portal en API) | Vroege gebonden |

## <a name="next-steps"></a>Volgende stappen

Zie voor informatie over het implementeren van voorwaardelijke toegang in uw omgeving, [plannen van uw implementatie van voorwaardelijke toegang in Azure Active Directory](plan-conditional-access.md).
