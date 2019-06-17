---
title: Toegang tot Azure AD-rollen in PIM - Azure Active Directory controleren | Microsoft Docs
description: Meer informatie over het bekijken van de toegang van Azure AD-rollen in Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 06/21/2018
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3850af026ea13e1920aea65e18358ebb04ef0d25
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65140970"
---
# <a name="review-access-to-azure-ad-roles-in-pim"></a>Toegang tot Azure AD-rollen in PIM bekijken

Azure Active Directory (AD) Privileged Identity Management (PIM) vereenvoudigt de manier waarop ondernemingen bevoegde toegang tot resources in Azure AD en andere Microsoft online services zoals Office 365 en Microsoft Intune beheren.  

Als u aan een beheerdersrol toegewezen bent, kan beheerder met bevoorrechte rol van uw organisatie vragen u om regelmatig te bevestigen dat u die rol nog voor de taak moet. U kunt een e-mailbericht een koppeling bevat krijgen of gaat u rechtstreeks naar de [Azure-portal](https://portal.azure.com). Volg de stappen in dit artikel om uit te voeren met een zelf bekijken van de toegewezen rollen.

Als u een beheerder met bevoorrechte rol of globale beheerder geïnteresseerd in toegangsbeoordelingen bent, krijgt u meer informatie op [een toegangsbeoordeling starten](pim-how-to-start-security-review.md).

## <a name="add-the-privileged-identity-management-application"></a>De Privileged Identity Management-toepassing toevoegen
U kunt de Azure AD Privileged Identity Management (PIM)-toepassing in de [Azure-portal](https://portal.azure.com/) om uit te voeren van uw beoordeling.  Als u niet de Azure AD Privileged Identity Management-toepassing in de portal hebt, volgt u deze stappen om te beginnen.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Selecteer uw gebruikersnaam in de rechterbovenhoek van de Azure-portal en selecteer de map waar u u wilt worden uitgevoerd.
3. Selecteer **Alle services** en gebruik het tekstvak Filteren om te zoeken naar **Azure AD Privileged Identity Management**.
4. Schakel **Vastmaken aan dashboard** in en klik op de knop **Maken**. De Privileged Identity Management-toepassing wordt geopend.

## <a name="approve-or-deny-access"></a>Goedkeuren of weigeren van toegang
Wanneer u goedkeuren of weigeren van toegang, u bent slechts zorgt ervoor dat de revisor of u nog steeds gebruik van deze rol of niet. Kies **goedkeuren** als u wilt blijven in de rol of **weigeren** als u de toegang niet meer nodig. De status van uw worden niet meteen gewijzigd totdat de revisor van de toepassing de resultaten.
Volg deze stappen om te zoeken en de toegangsbeoordeling voltooien:

1. Selecteer in de toepassing PIM **revisie bevoegde toegang**. Als u alle in behandeling zijnde toegangsbeoordelingen hebt, worden ze weergegeven in de Azure AD Access beoordelingen-blade.
2. Selecteer de beoordeling die u wilt uitvoeren.
3. Tenzij u de controle hebt gemaakt, wordt u weergegeven als de enige gebruiker in de evaluatie. Klik op het vinkje naast de naam van uw.
4. Kies een **goedkeuren** of **weigeren**. U moet mogelijk om op te nemen van een reden op voor uw beslissing in de **Geef een reden** in het tekstvak.  
5. Sluit de **Controleer Azure AD-rollen** blade.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen

- [Perform an access review of my Azure AD resource roles in PIM](pim-resource-roles-perform-access-review.md) (Een toegangsbeoordeling uitvoeren van mijn Azure AD-resourcerollen in PIM)
