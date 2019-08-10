---
title: Een aangepaste Azure AD-rol activeren in Privileged Identity Management (PIM) | Microsoft Docs
description: Een aangepaste Azure AD-rol activeren voor toewijzings Privileged Identity Management (PIM)
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.assetid: ''
ms.service: role-based-access-control
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/06/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: e38b0f4463c2188f0b2da6ebb1b57d08af69b41f
ms.sourcegitcommit: 124c3112b94c951535e0be20a751150b79289594
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/10/2019
ms.locfileid: "68947329"
---
# <a name="activate-an-azure-ad-custom-role-in-privileged-identity-management"></a>Een aangepaste Azure AD-rol activeren in Privileged Identity Management

Privileged Identity Management in Azure Active Directory (Azure AD) biedt nu ondersteuning voor Just-in-time-en tijdgebonden toewijzing aan aangepaste rollen die zijn gemaakt voor toepassings beheer in de beheer ervaring voor identiteits-en toegangs beheer. Zie [aangepaste beheerders rollen in azure Active Directory (preview)](../users-groups-roles/roles-custom-overview.md)voor meer informatie over het maken van aangepaste rollen voor het delegeren van toepassings beheer in azure AD.

> [!NOTE]
> Aangepaste Azure AD-rollen worden tijdens de preview-fase niet geïntegreerd met de ingebouwde Directory rollen. Zodra de mogelijkheid algemeen beschikbaar is, wordt het beheer van rollen uitgevoerd in de ingebouwde functie-ervaring.

## <a name="activate-a-role"></a>Een rol activeren

Wanneer u een aangepaste Azure AD-rol moet activeren, kunt u de activering aanvragen door de optie mijn rollen navigatie in PIM te selecteren.

1. Aanmelden bij [de Azure-portal](https://portal.azure.com).
1. Open Azure AD- [privileged Identity Management](https://portal.azure.com/?Microsoft_AAD_IAM_enableCustomRoleManagement=true&Microsoft_AAD_IAM_enableCustomRoleAssignment=true&feature.rbacv2roles=true&feature.rbacv2=true&Microsoft_AAD_RegisteredApps=demo#blade/Microsoft_Azure_PIMCommon/CommonMenuBlade/quickStart).

1. Selecteer **aangepaste Azure AD-rollen** om een lijst weer te geven met de aangepaste roltoewijzingen in azure AD.

   ![Bekijk de lijst met aangepaste roltoewijzingen die in aanmerking komen voor Azure AD](./media/azure-ad-custom-roles-activate/view-preview-roles.png)

1. Zoek op de pagina **aangepaste rollen van Azure AD (preview)** de toewijzing die u nodig hebt.
1. Selecteer **uw rol activeren** om de pagina **activeren** te openen.
1. Als voor uw rol multi-factor Authentication is vereist, selecteert u **uw identiteit verifiëren voordat u doorgaat**. U hoeft slechts één keer per sessie te verifiëren.
1. Selecteer **Mijn identiteit verifiëren** en volg de instructies om een aanvullende beveiligings verificatie te geven.
1. Als u een aangepast toepassings bereik wilt opgeven, selecteert u **bereik** om het deel venster filter te openen. U moet toegang tot een rol aanvragen op het minimale bereik dat nodig is. Als uw toewijzing zich in een toepassings bereik bevindt, kunt u alleen activeren bij dat bereik.

   ![Een Azure AD-resource bereik toewijzen aan de roltoewijzing](./media/azure-ad-custom-roles-activate/assign-scope.png)

1. Geef indien nodig een aangepaste begin tijd voor de activering op. Wanneer u dit gebruikt, wordt het rollidmaatschap geactiveerd op het opgegeven tijdstip.
1. Voer in het vak **reden** de reden voor de activerings aanvraag in. Deze kunnen worden opgegeven, of niet in de instelling van de rol.
1. Selecteer **activeren**.

Als voor de rol geen goed keuring is vereist, wordt deze geactiveerd volgens uw instellingen en wordt deze toegevoegd aan de lijst met actieve rollen. Als u de geactiveerde functie wilt gebruiken, begint u met de stappen in [een aangepaste Azure AD-rol toewijzen in privileged Identity Management](azure-ad-custom-roles-assign.md).

Als voor de rol goed keuring vereist is, ontvangt u een melding van Azure dat de aanvraag goed keuring in behandeling is.

## <a name="next-steps"></a>Volgende stappen

- [Een aangepaste Azure AD-rol toewijzen](azure-ad-custom-roles-assign.md)
- [Een aangepaste gebruikersrol toewijzing van Azure AD verwijderen of bijwerken](azure-ad-custom-roles-update-remove.md)
- [Een aangepaste functie toewijzing voor Azure AD configureren](azure-ad-custom-roles-configure.md)
- [Roldefinities in azure AD](../users-groups-roles/directory-assign-admin-roles.md)
