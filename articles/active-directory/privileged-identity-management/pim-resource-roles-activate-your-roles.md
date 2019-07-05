---
title: Mijn Azure-resource-rollen in PIM - Azure Active Directory activeren | Microsoft Docs
description: Informatie over het activeren van uw Azure-resource-rollen in Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 06/28/2019
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: bdce060006f65f2b0fb08023066ee504578bc552
ms.sourcegitcommit: 837dfd2c84a810c75b009d5813ecb67237aaf6b8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2019
ms.locfileid: "67501661"
---
# <a name="activate-my-azure-resource-roles-in-pim"></a>Mijn Azure-resource-rollen in PIM activeren

Met behulp van Azure Active Directory (Azure AD) Privileged Identity Management (PIM), kunnen in aanmerking komende rolleden voor de Azure-resources plannen dat activering voor een toekomstige datum en tijd. Ze kunnen ook een specifieke activeringsduur binnen de maximale (geconfigureerd door beheerders) selecteren.

In dit artikel geldt voor leden die willen hun Azure-resource-rollen in PIM activeren.

## <a name="activate-a-role"></a>Een rol activeren

Wanneer u nodig hebt om de rol van een Azure-resource te, kunt u activering aanvragen met behulp van de **mijn rollen** navigatieoptie in PIM.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

1. Open **Azure AD Privileged Identity Management**. Zie voor meer informatie over het toevoegen van de PIM-tegel aan uw dashboard [beginnen met PIM](pim-getting-started.md).

1. Klik op **mijn rollen**.

    ![Mijn rollen-pagina met functies die u kunt activeren](./media/pim-resource-roles-activate-your-roles/resources-my-roles.png)

1. Klik op **Azure-resourcerollen** voor een overzicht van uw in aanmerking komende Azure-resource-rollen.

   ![Mijn rollen - pagina voor Azure-resource-rollen](./media/pim-resource-roles-activate-your-roles/resources-my-roles-azure-resources.png) 

1. In de **Azure-resourcerollen** lijst, het vinden van de rol die u wilt activeren.

    ![Azure-resourcerollen - mijn lijst met in aanmerking komende rollen](./media/pim-resource-roles-activate-your-roles/resources-my-roles-activate.png)

1. Klik op **activeren** om het deelvenster activeren te openen.

1. Als uw rol is vereist voor multi-factor authentication (MFA), klikt u op **Verifieer uw identiteit voordat u doorgaat**. U moet slechts één keer per sessie geverifieerd.

    ![Mijn identiteit verifiëren met MFA voor rolactivering](./media/pim-resource-roles-activate-your-roles/resources-my-roles-mfa.png)

1. Klik op **Mijn identiteit verifiëren** en volg de instructies voor aanvullende beveiligingsverificatie.

    ![Scherm voor instellingen voor beveiligingsverificatie, zoals een PIN-code](./media/pim-resource-roles-activate-your-roles/resources-mfa-enter-code.png)

1. Als u een verminderde bereik opgeven wilt, klikt u op **bereik** het filterdeelvenster Resource te openen.

    Het is beste alleen aanvragen van toegang tot de bronnen die u nodig. U kunt de resourcegroepen of resources die u toegang tot hebt opgeven op het filterdeelvenster Resource.

    ![Activeren - Resource filterdeelvenster bereik opgeven](./media/pim-resource-roles-activate-your-roles/resources-my-roles-resource-filter.png)

1. Geef zo nodig een begintijd aangepaste activering. Het lid wordt geactiveerd na de geselecteerde periode.

1. In de **reden** voert u de reden voor de aanvraag voor activering.

    ![Voltooide activeren deelvenster met bereik, de begintijd, de duur en de reden](./media/pim-resource-roles-activate-your-roles/resources-my-roles-activate-done.png)

1. Klik op **Activeren**.

    Als de rol geen goedkeuring is vereist, wordt deze geactiveerd en toegevoegd aan de lijst met actieve rollen. Als u de rol gebruiken wilt, volgt u de stappen in de volgende sectie.

    Als de [rol is goedkeuring vereist](pim-resource-roles-approval-workflow.md) wilt activeren, een melding wordt weergegeven in de rechterbovenhoek van uw browser waarin de aanvraag is in afwachting van goedkeuring.

    ![De activeringsaanvraag is in afwachting van goedkeuringsmelding](./media/pim-resource-roles-activate-your-roles/resources-my-roles-activate-notification.png)

## <a name="use-a-role-immediately-after-activation"></a>Gebruik een rol onmiddellijk na de activering

In het geval van eventuele vertraging na de activering door deze stappen uit te voeren nadat u voor het gebruik van uw Azure-resourcerollen onmiddellijk geactiveerd.

1. Open Azure AD Privileged Identity Management.

1. Klik op **mijn rollen** voor een overzicht van uw in aanmerking komende Azure AD-rollen en functies van Azure-resource.

1. Klik op **Azure-resourcerollen**.

1. Klik op de **actieve rollen** tabblad.

1. Zodra de functie actief is, afmelden bij de portal en meld u opnieuw aan.

    Nu moet de rol worden gebruikt.

## <a name="view-the-status-of-your-requests"></a>De status van de aanvragen bekijken

U ziet de status van de in behandeling zijnde aanvragen om te activeren.

1. Open Azure AD Privileged Identity Management.

1. Klik op **mijn aanvragen** aanvragen voor een overzicht van uw Azure AD-rol en de rol van Azure-resource.

    ![Mijn aanvragen - pagina van Azure-resource met de aanvragen in behandeling](./media/pim-resource-roles-activate-your-roles/resources-my-requests.png)

1. Schuif naar rechts om weer te geven de **de Status van wijzigingsaanvragen** kolom.

## <a name="cancel-a-pending-request"></a>Een aanvraag in behandeling annuleren

Als u geen activering van een rol waarvoor goedkeuring wordt vereist, kunt u een aanvraag in behandeling op elk gewenst moment annuleren.

1. Open Azure AD Privileged Identity Management.

1. Klik op **mijn aanvragen**.

1. Voor de rol die u wilt annuleren, klikt u op de **annuleren** koppeling.

    Wanneer u op Annuleren klikt, wordt de aanvraag geannuleerd. Voor het activeren van de rol opnieuw, moet u een nieuwe aanvraag voor activering te verzenden.

   ![De lijst met mijn aanvragen met de actie voor annuleren van gemarkeerd](./media/pim-resource-roles-activate-your-roles/resources-my-requests-cancel.png)

## <a name="troubleshoot"></a>Problemen oplossen

### <a name="permissions-are-not-granted-after-activating-a-role"></a>Machtigingen zijn niet verleend na het activeren van een rol

Wanneer u een rol in PIM kunt activeren, kan de activering niet direct doorgegeven aan alle portals waarvoor de bevoorrechte rol. Zelfs als de wijziging is doorgevoerd, kan opslaan in webcache in een portal soms resulteren in de wijziging niet onmiddellijk te lopen. Als de activering is vertraagd, is dit wat u moet doen.

1. Afmelden bij de Azure-portal en weer aanmelden.

    Wanneer u een Azure-resource-rol kunt activeren, ziet u de fasen van de activering. Wanneer alle fasen voltooid zijn, ziet u een **Afmelden** koppeling. Gebruik deze koppeling kunt u zich afmelden. Dit wordt meestal voor vertraging op te lossen.

1. Controleer in PIM, of dat u wordt vermeld als het lid van de rol.

## <a name="next-steps"></a>Volgende stappen

- [Azure-resource-rollen in PIM verlengen of vernieuwen](pim-resource-roles-renew-extend.md)
- [Mijn Azure AD-rollen in PIM activeren](pim-how-to-activate-role.md)
