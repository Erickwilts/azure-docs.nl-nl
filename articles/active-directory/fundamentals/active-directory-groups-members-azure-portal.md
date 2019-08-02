---
title: Groepsleden - Azure Active Directory toevoegen of verwijderen | Microsoft Docs
description: Instructies over het toevoegen of verwijderen van leden uit een groep met behulp van Azure Active Directory.
services: active-directory
author: msaburnley
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 08/23/2018
ms.author: ajburnle
ms.custom: it-pro, seodec18
ms.reviewer: krbain
ms.collection: M365-identity-device-management
ms.openlocfilehash: f1c83c57be63ae9e2a4d4113accaefe8a2c2b525
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68561959"
---
# <a name="add-or-remove-group-members-using-azure-active-directory"></a>Toevoegen of verwijderen van leden van beveiligingsgroep met Azure Active Directory
Met Azure Active Directory, kunt u blijven toevoegen en verwijderen van leden van de beveiligingsgroep.

## <a name="to-add-group-members"></a>Groepsleden toevoegen

1. Meld u aan bij de [Azure Portal](https://portal.azure.com) met het account van een globale administrator voor de map.

2. Selecteer **Azure Active Directory**, en selecteer vervolgens **groepen**.

3. Uit de **groepen - alle groepen** pagina, zoek en selecteer de groep die u wilt toevoegen van het lid. In dit geval gebruikt u de eerder gemaakte groep **MDM-beleid - West**.

    ![Pagina groepen-alle groepen in de naam van de groep is gemarkeerd](media/active-directory-groups-members-azure-portal/group-all-groups-screen.png)

4. Selecteer op de pagina **Overzicht van MDM-beleid - West** de optie **Leden** onder **Beheren**.

    ![Beleid voor MDM - West overzichtspagina met leden-optie is gemarkeerd](media/active-directory-groups-members-azure-portal/group-overview-blade.png)

5. Selecteer **leden toevoegen**, en vervolgens zoekt en selecteert u elk van de leden die u wilt toevoegen aan de groep en kies vervolgens **Selecteer**.

    U krijgt een bericht weergegeven dat de leden zijn toegevoegd.

    ![Ledenpagina toevoegen met gezocht lid wordt weergegeven](media/active-directory-groups-members-azure-portal/update-members.png)

6. Vernieuwen van het scherm om alle van de namen van leden toegevoegd aan de groep te zien.

## <a name="to-remove-group-members"></a>Groepsleden verwijderen

1. Uit de **groepen - alle groepen** pagina, zoek en selecteer de groep die u wilt voor het lid in verwijderen. Het opnieuw gebruiken we, **MDM-beleid - West**.

2. Selecteer **leden** uit de **beheren** gebied, zoekt en selecteert u de naam van het lid wilt verwijderen en selecteer vervolgens **verwijderen**.

    ![De pagina van het lid info, met de optie verwijderen](media/active-directory-groups-members-azure-portal/remove-members-from-group.png)

## <a name="next-steps"></a>Volgende stappen

- [Groepen en leden weergeven](active-directory-groups-view-azure-portal.md)

- [Instellingen van uw groep bewerken](active-directory-groups-settings-azure-portal.md)

- [Toegang tot resources met behulp van groepen beheren](active-directory-manage-groups.md)

- [Dynamische regels voor gebruikers in een groep beheren](../users-groups-roles/groups-create-rule.md)

- [Een Azure-abonnement aan Azure Active Directory koppelen of toevoegen](active-directory-how-subscriptions-associated-directory.md)
