---
title: Rollen in PIM - Azure Active Directory kunt u niet beheren | Microsoft Docs
description: Beschrijving van de rollen die u niet in Azure AD Privileged Identity Management (PIM beheren).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 01/18/2019
ms.author: rolyon
ms.custom: pim ; H1Hack27Feb2017;oldportal;it-pro;
ms.collection: M365-identity-device-management
ms.openlocfilehash: aa5fb632ee5fd9c18bde7443e81fe2ef6e5335e4
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60437270"
---
# <a name="roles-you-cannot-manage-in-pim"></a>Rollen die in PIM kunt u niet beheren

Azure Active Directory (Azure AD) Privileged Identity Management (PIM) kunt u voor het beheren van alle [Azure AD-rollen](../users-groups-roles/directory-assign-admin-roles.md) en alle [Azure-resourcerollen](../../role-based-access-control/built-in-roles.md). Deze rollen bevatten ook uw aangepaste rollen die zijn gekoppeld aan uw beheergroepen, abonnementen, resourcegroepen en resources. Er zijn echter enkele functies die u niet kunt beheren. Dit artikel beschrijft de rollen die in PIM kunt u niet beheren.

## <a name="classic-subscription-administrator-roles"></a>Klassieke abonnementsbeheerdersrollen

U kunt de volgende klassiek abonnement beheerdersrollen in PIM niet beheren:

- Accountbeheerder
- Servicebeheerder
- Medebeheerder

Zie voor meer informatie over de beheerdersrollen klassiek abonnement [klassiek abonnement beheerder functies, Azure RBAC-rollen en Azure AD-beheerdersrollen](../../role-based-access-control/rbac-and-directory-admin-roles.md).

## <a name="what-about-office-365-admin-roles"></a>Hoe zit het met Office 365-beheerdersrollen?

Rollen in Exchange Online of SharePoint Online, met uitzondering van de beheerder van Exchange en SharePoint-beheerder, worden niet weergegeven in Azure AD en dus kunnen niet worden beheerd in PIM. Zie voor meer informatie over deze Office 365-services, [Office 365-beheerdersrollen](https://docs.microsoft.com/office365/admin/add-users/about-admin-roles).

> [!NOTE]
> SharePoint-beheerder heeft beheerderstoegang tot SharePoint Online via de SharePoint Online-beheercentrum, en bijna alle taken kunt uitvoeren in SharePoint Online. In aanmerking komende gebruikers kunnen met behulp van deze rol in SharePoint na het activeren van in PIM vertragingen optreden.

## <a name="next-steps"></a>Volgende stappen

- [Azure AD-rollen in PIM toewijzen](pim-how-to-add-role-to-user.md)
- [Azure-resource-rollen in PIM toewijzen](pim-resource-roles-assign-roles.md)
