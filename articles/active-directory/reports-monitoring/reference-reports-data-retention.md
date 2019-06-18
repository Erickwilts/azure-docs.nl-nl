---
title: Hoe lang slaat Azure AD reporting gegevens? | Microsoft Docs
description: Meer informatie over hoe lang de verschillende typen gegevens worden opgeslagen in Azure.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 183e53b0-0647-42e7-8abe-3e9ff424de12
ms.service: active-directory
ms.devlang: ''
ms.topic: reference
ms.tgt_pltfrm: ''
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 41fa12c9d79d14a6602d995ed93b5d1a23be8a4d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65781048"
---
# <a name="how-long-does-azure-ad-store-reporting-data"></a>Hoe lang slaat Azure AD reporting gegevens?

In dit artikel hebt u meer informatie over het bewaarbeleid voor gegevens voor de verschillende aanmeldactiviteitenrapporten in Azure Active Directory. 

### <a name="when-does-azure-ad-start-collecting-data"></a>Wanneer begint Azure AD het verzamelen van gegevens?

| Azure AD Edition | Verzameling starten |
| :--              | :--   |
| Azure AD Premium P1 <br /> Azure AD Premium P2 | Wanneer u zich aanmeldt voor een abonnement |
| Azure AD Free <br /> Azure AD Basic | De eerste keer dat u opent de [Azure Active Directory-blade](https://ms.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) of gebruik de [reporting API's](https://aka.ms/aadreports)  |

---

### <a name="when-is-the-activity-data-available-in-the-azure-portal"></a>Wanneer is de activiteitsgegevens beschikbaar zijn in Azure portal?

- **Onmiddellijk** : als u al met rapporten in Azure portal werkt.
- **Binnen 2 uur** : als u dit nog niet hebt ingeschakeld op rapportage in Azure portal.

---

### <a name="how-soon-can-i-see-activities-data-after-getting-a-premium-license"></a>Hoe snel kan ik activiteiten gegevens zien nadat u een premium-licentie?

Als u al gegevens van activiteiten met uw gratis licentie hebt, kunt klikt u vervolgens u zien deze onmiddellijk bij een upgrade. Als u geen gegevens, duurt het een of twee dagen voor de gegevens worden weergegeven in de rapporten na de upgrade naar een premium-licentie.

---

### <a name="can-i-see-last-months-data-after-getting-an-azure-ad-premium-license"></a>Kan ik van afgelopen maand gegevens zien nadat u een Azure AD premium-licentie?

Als u onlangs hebt overgeschakeld naar een premiumversie (met inbegrip van een evaluatieversie), ziet u gegevens van 7 dagen in eerste instantie. Wanneer gegevens worden bij elkaar opgeteld, ziet u gegevens voor de afgelopen 30 dagen.

---

### <a name="when-does-azure-ad-start-collecting-security-signal-data"></a>Wanneer begint Azure AD signaal beveiligingsgegevens verzamelen?  

Voor beveiliging signalen, de verzamelingsproces wordt gestart wanneer u zich aanmelden voor gebruik van de **Identity Protection Center**. 

---

### <a name="how-long-does-azure-ad-store-the-data"></a>Hoe lang de gegevens opslaan in Azure AD?

**Activiteitsrapporten**    

| Rapport                 | Azure AD Free | Azure AD Basic | Azure AD Premium P1 | Azure AD Premium P2 |
| :--                    | :--           | :--            | :--                 | :--                 |
| Auditlogboeken             | 7 dagen        |  7 dagen        | 30 dagen             | 30 dagen             |
| Aanmeldingen               | N/A           |  N/A           | 30 dagen             | 30 dagen             |
| Gebruik van Azure MFA        | 30 dagen       |  30 dagen       | 30 dagen             | 30 dagen             |

U kunt de gegevens van de activiteit controleren en meld u langer dan de bewaartermijn die hierboven worden beschreven door het routering naar Azure storage-account met behulp van Azure Monitor behouden. Zie voor meer informatie, [archief Azure AD vastgelegd in een Azure storage-account](quickstart-azure-monitor-route-logs-to-storage-account.md).

**Beveiliging signalen**

| Rapport         | Azure AD Free | Azure AD Basic | Azure AD Premium P1 | Azure AD Premium P2 |
| :--            | :--           | :--            | :--                 | :--                 |
| Gebruikers die risico lopen  | 7 dagen        | 7 dagen         | 30 dagen             | 90 dagen             |
| Riskante aanmeldingen | 7 dagen        | 7 dagen         |  30 dagen            | 90 dagen             |

---
