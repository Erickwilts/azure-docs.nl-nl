---
title: Lijst groepslid maatschap voor bulk down load-Azure Active Directory Portal | Microsoft Docs
description: Voeg gebruikers bulksgewijs toe in het beheer centrum van Azure.
services: active-directory
author: curtand
ms.author: curtand
manager: mtillman
ms.date: 04/16/2020
ms.topic: how-to
ms.service: active-directory
ms.subservice: users-groups-roles
ms.workload: identity
ms.custom: it-pro
ms.reviewer: jeffsta
ms.collection: M365-identity-device-management
ms.openlocfilehash: 65a6a622a0d3fac7b28cc699d860068f12b780fe
ms.sourcegitcommit: c4ad4ba9c9aaed81dfab9ca2cc744930abd91298
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/12/2020
ms.locfileid: "84728602"
---
# <a name="bulk-download-members-of-a-group-in-azure-active-directory"></a>Leden van een groep bulksgewijs downloaden in Azure Active Directory

Met Azure Active Directory-Portal (Azure AD) kunt u de leden van een groep in uw organisatie bulksgewijs downloaden naar een bestand met door komma's gescheiden waarden (CSV).

## <a name="to-bulk-download-group-membership"></a>Groepslid maatschap bulksgewijs downloaden

1. Meld u aan bij [de Azure Portal](https://portal.azure.com) met een Administrator-account van de gebruiker in de organisatie. Groeps eigenaren kunnen ook leden van groepen waarvan ze eigenaar zijn, bulksgewijs downloaden.
1. Selecteer **groepen**  >  **alle groepen**in azure AD.
1. Open de groep waarvan u het lidmaatschap wilt downloaden en selecteer **leden**.
1. Selecteer **leden downloaden** op de pagina **leden** om een CSV-bestand te downloaden met daarin de groeps leden.

   ![De opdracht leden downloaden bevindt zich op de profiel pagina voor de groep](./media/groups-bulk-download-members/download-panel.png)

## <a name="check-download-status"></a>Download status controleren

U kunt de status van al uw bulk aanvragen in behandeling bekijken op de pagina **resultaten van bulk bewerking** .

[![](media/groups-bulk-download-members/bulk-center.png "Check status in the Bulk Operations Results page")](media/groups-bulk-download-members/bulk-center.png#lightbox)

## <a name="bulk-download-service-limits"></a>Service limieten bulksgewijs downloaden

Elke bulk activiteit voor het downloaden van een lijst met groeps leden kan Maxi maal één uur worden uitgevoerd. Zo kunt u een lijst met ten minste 500.000 leden downloaden.

## <a name="next-steps"></a>Volgende stappen

- [Groeps leden bulksgewijs importeren](groups-bulk-import-members.md)
- [Groeps leden bulksgewijs verwijderen](groups-bulk-download-members.md)
