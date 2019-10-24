---
title: Activeer mijn Azure AD-rollen in PIM-Azure Active Directory | Microsoft Docs
description: Meer informatie over het activeren van Azure AD-rollen in Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 06/28/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6007762c897337170dec69c3486302aa62723480
ms.sourcegitcommit: 8074f482fcd1f61442b3b8101f153adb52cf35c9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/22/2019
ms.locfileid: "72756291"
---
# <a name="activate-my-azure-ad-roles-in-pim"></a>Activeer mijn Azure AD-rollen in PIM

Azure Active Directory (Azure AD) Privileged Identity Management (PIM) vereenvoudigt de manier waarop bedrijven bevoorrechte toegang tot resources in azure AD en andere micro soft-onlineservices, zoals Office 365 of Microsoft Intune, kunnen beheren.  

Als u in aanmerking komt voor een administratieve rol, betekent dit dat u die rol kunt activeren wanneer u geprivilegieerde acties moet uitvoeren. Als u bijvoorbeeld af en toe Office 365-functies beheert, is het mogelijk dat beheerders van de bevoegde rol van uw organisatie u geen permanente globale beheerder maken, omdat die rol ook van invloed is op andere services. In plaats daarvan komt u in aanmerking voor Azure AD-rollen zoals Exchange Online-beheerder. U kunt aanvragen om die rol te activeren als u de juiste bevoegdheden nodig hebt, en u hebt beheerders controle voor een vooraf bepaalde periode.

Dit artikel is voor beheerders die hun Azure AD-rol moeten activeren in Privileged Identity Management.

## <a name="activate-a-role"></a>Een rol activeren

Wanneer u een Azure AD-rol nodig hebt, kunt u activering aanvragen met behulp van de navigatie optie **mijn rollen** in privileged Identity Management.

1. Meld u aan bij de [Azure-portal](https://portal.azure.com/).

1. Open **Azure AD privileged Identity Management**. Zie voor meer informatie over het toevoegen van de Privileged Identity Management tegel aan uw dash board [beginnen met privileged Identity Management](pim-getting-started.md).

1. Klik op **Azure AD-rollen**.

1. Klik op **mijn rollen** om een lijst weer te geven met de Azure AD-rollen die u in aanmerking komt.

    ![Azure AD-rollen: mijn rollen met een lijst met in aanmerking komende of actieve rollen](./media/pim-how-to-activate-role/directory-roles-my-roles.png)

1. Zoek een rol die u wilt activeren.

    ![Azure AD-rollen: mijn lijst met in aanmerking komende rollen met de koppeling activeren](./media/pim-how-to-activate-role/directory-roles-my-roles-activate.png)

1. Klik op **activeren** om het deel venster activerings Details voor rollen te openen.

1. Als voor uw rol multi-factor Authentication (MFA) is vereist, klikt **u op uw identiteit verifiëren voordat u doorgaat**. U hoeft slechts één keer per sessie te authenticeren.

    ![Mijn identiteits venster met MFA controleren voordat de functie wordt geactiveerd](./media/pim-how-to-activate-role/directory-roles-my-roles-mfa.png)

1. Klik op **Mijn identiteit verifiëren** en volg de instructies voor aanvullende beveiligings verificatie.

    ![Pagina aanvullende beveiligings verificatie waarin wordt gevraagd hoe u contact met u kunt opnemen](./media/pim-how-to-activate-role/additional-security-verification.png)

1. Klik op **activeren** om het deel venster activering te openen.

    ![Activerings deel venster om begin tijd, duur, ticket en reden op te geven](./media/pim-how-to-activate-role/directory-roles-activate.png)

1. Geef indien nodig een aangepaste begin tijd voor de activering op.

1. Geef de activerings duur op.

1. Voer in het vak **activerings reden** de reden in voor de activerings aanvraag. Voor sommige rollen moet u een fout ticket nummer opgeven.

    ![Het deel venster activering is voltooid met een aangepaste start tijd, duur, ticket en reden](./media/pim-how-to-activate-role/directory-roles-activation-pane.png)

1. Klik op **Activeren**.

    Als voor de rol geen goed keuring is vereist, wordt een **activerings status** venster weer gegeven waarin de status van de activering wordt weer gegeven.

    ![Pagina activerings status met de drie fasen van activering](./media/pim-how-to-activate-role/activation-status.png)

    Zodra alle fasen zijn voltooid, klikt u op de koppeling **Afmelden** om u af te melden bij de Azure Portal. Wanneer u zich opnieuw bij de portal aanmeldt, kunt u nu de rol gebruiken.

    Als voor de [rol goed keuring moet](./azure-ad-pim-approval-workflow.md) worden geactiveerd, wordt in de rechter bovenhoek van uw browser gemeld dat de aanvraag goed keuring in behandeling is.

    ![Goedkeurings melding in afwachting van activerings aanvraag](./media/pim-how-to-activate-role/directory-roles-activate-notification.png)

## <a name="view-the-status-of-your-requests"></a>De status van uw aanvragen weer geven

U kunt de status van de in behandeling zijnde aanvragen weer geven om te activeren.

1. Open Azure AD Privileged Identity Management.

1. Klik op **Azure AD-rollen**.

1. Klik op **mijn aanvragen** om een lijst met uw aanvragen weer te geven.

    ![Azure AD-rollen: mijn aanvragen lijst](./media/pim-how-to-activate-role/directory-roles-my-requests.png)

## <a name="deactivate-a-role"></a>Een rol deactiveren

Zodra een rol is geactiveerd, wordt deze automatisch gedeactiveerd wanneer de tijds limiet (in aanmerking komende duur) is bereikt.

Als u de beheerders taken vroegtijdig voltooit, kunt u een rol ook hand matig deactiveren in Azure AD Privileged Identity Management.

1. Open Azure AD Privileged Identity Management.

1. Klik op **Azure AD-rollen**.

1. Klik op **mijn rollen**.

1. Klik op **actieve rollen** om uw lijst met actieve rollen weer te geven.

1. Zoek de rol die u wilt gebruiken en klik vervolgens op **deactiveren**.

## <a name="cancel-a-pending-request"></a>Een aanvraag in behandeling annuleren

Als u geen activering van een rol vereist waarvoor goed keuring is vereist, kunt u een aanvraag in behandeling op elk gewenst moment annuleren.

1. Open Azure AD Privileged Identity Management.

1. Klik op **Azure AD-rollen**.

1. Klik op **mijn aanvragen**.

1. Klik op de knop **Annuleren** voor de functie die u wilt annuleren.

    Wanneer u op Annuleren klikt, wordt de aanvraag geannuleerd. Als u de functie opnieuw wilt activeren, moet u een nieuwe aanvraag indienen voor activering.

   ![De lijst met mijn aanvragen met de knop Annuleren gemarkeerd](./media/pim-how-to-activate-role/directory-role-cancel.png)

## <a name="troubleshoot"></a>Problemen oplossen

### <a name="permissions-are-not-granted-after-activating-a-role"></a>Machtigingen worden niet toegekend na het activeren van een rol

Wanneer u een rol in Privileged Identity Management activeert, wordt de activering mogelijk niet onmiddellijk door gegeven aan alle portals waarvoor de rol met privileges is vereist. Zelfs als de wijziging is doorgevoerd, kan het soms gebeuren dat webcaching in een portal tot gevolg heeft dat de wijziging niet onmiddellijk van kracht gaat. Als de activering is vertraagd, is dit wat u moet doen.

1. Meld u af bij de Azure-portal en meld u weer aan.

    Wanneer u een Azure AD-rol activeert, ziet u de fasen van uw activering. Wanneer alle fasen zijn voltooid, ziet u de koppeling **Afmelden**. U kunt deze koppeling gebruiken om u af te melden. Hiermee worden de meeste gevallen voor de activerings vertraging opgelost.

1. Controleer in Privileged Identity Management of u wordt vermeld als lid van de rol.

## <a name="next-steps"></a>Volgende stappen

- [Mijn Azure-resource rollen in Privileged Identity Management activeren](pim-resource-roles-activate-your-roles.md)
