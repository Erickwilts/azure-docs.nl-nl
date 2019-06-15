---
title: Goedkeuren of weigeren van aanvragen voor Azure AD-rollen in PIM - Azure Active Directory | Microsoft Docs
description: Meer informatie over het goedkeuren of weigeren van aanvragen voor Azure AD-rollen in Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 04/09/2019
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 47e9033b33c70a72a1685696f75a442e88eed033
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60289721"
---
# <a name="approve-or-deny-requests-for-azure-ad-roles-in-pim"></a>Goedkeuren of weigeren van aanvragen voor Azure AD-rollen in PIM

Met Azure Active Directory (Azure AD) Privileged Identity Management (PIM), kunt u rollen om te vereisen van goedkeuring voor activering configureren en kies een of meer gebruikers of groepen als gedelegeerde fiatteurs. Gedelegeerde fiatteurs hebt 24 uur verzoeken goed te keuren. Als een aanvraag niet binnen 24 uur is goedgekeurd, moet klikt u vervolgens de in aanmerking komende gebruiker opnieuw een nieuwe aanvraag indienen. Het tijdvenster voor goedkeuring van 24 uur per dag kan niet worden geconfigureerd.

Volg de stappen in dit artikel voor het goedkeuren of weigeren van aanvragen voor Azure AD-rollen.

## <a name="view-pending-requests"></a>Aanvragen in behandeling weergeven

Als een gedelegeerde fiatteur ontvangt u een e-mailmelding wanneer een aanvraag om de rol Azure AD moeten worden goedgekeurd. U kunt deze aanvragen in behandeling weergeven in PIM.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

1. Open **Azure AD Privileged Identity Management**.

1. Klik op **Azure AD-rollen**.

1. Klik op **aanvragen goedkeuren**.

    ![Rollen in PIM-Azure AD - rollen](./media/azure-ad-pim-approval-workflow/pim-directory-roles-approve-requests.png)

    U ziet een lijst met aanvragen moeten worden goedgekeurd.

## <a name="approve-requests"></a>Aanvragen goedkeuren

1. Selecteer de aanvragen die u wilt goedkeuren en klik vervolgens op **goedkeuren** geselecteerde aanvragen deelvenster opent u de goedkeuren.

    ![Lijst met aanvragen PIM goedkeuren](./media/azure-ad-pim-approval-workflow/pim-approve-requests-list.png)

1. In de **reden voor goedkeuring** typt u een reden op.

    ![PIM goedkeuren geselecteerd aanvragen](./media/azure-ad-pim-approval-workflow/pim-approve-selected-requests.png)

1. Klik op **goedkeuren**.

    Het statussymbool wordt bijgewerkt met uw goedkeuring.

    ![PIM goedkeuren geselecteerd aanvragen](./media/azure-ad-pim-approval-workflow/pim-approve-status.png)

## <a name="deny-requests"></a>Aanvragen weigeren

1. Selecteer de aanvragen die u wilt weigeren en klik vervolgens op **weigeren** geselecteerde aanvragen deelvenster opent u de weigeren.

    ![Lijst met aanvragen PIM goedkeuren](./media/azure-ad-pim-approval-workflow/pim-deny-requests-list.png)

1. In de **reden voor weigering** typt u een reden op.

    ![PIM weigeren geselecteerd aanvragen](./media/azure-ad-pim-approval-workflow/pim-deny-selected-requests.png)

1. Klik op **weigeren**.

    Het statussymbool wordt bijgewerkt met uw DOS-aanval.

## <a name="next-steps"></a>Volgende stappen

- [E-mailmeldingen in PIM](pim-email-notifications.md)
- [Goedkeuren of weigeren van aanvragen voor Azure-resource-rollen in PIM](pim-resource-roles-approval-workflow.md)
