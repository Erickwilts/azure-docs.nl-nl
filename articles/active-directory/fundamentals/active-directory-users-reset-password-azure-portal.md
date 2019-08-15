---
title: Opnieuw instellen van het wachtwoord van een gebruiker - Azure Active Directory | Microsoft Docs
description: Instructies over het wachtwoord opnieuw instellen van een gebruiker met Azure Active Directory.
services: active-directory
author: msaburnley
manager: daveba
ms.assetid: fad5624b-2f13-4abc-b3d4-b347903a8f16
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 09/05/2018
ms.author: ajburnle
ms.reviewer: jeffsta
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: b4fdbbd4d71a9c97259678413cd9e59ee8aeae6b
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/15/2019
ms.locfileid: "69032673"
---
# <a name="reset-a-users-password-using-azure-active-directory"></a>Wachtwoord opnieuw instellen van een gebruiker met Azure Active Directory

U kunt als beheerder, het wachtwoord van een gebruiker opnieuw als het wachtwoord is vergeten, als de gebruiker toegang tot een apparaat wordt geblokkeerd, of als de gebruiker een wachtwoord nooit hebt ontvangen.

>[!Note]
>Als uw Azure AD-tenant de basismap van een gebruiker is, kunt u zich niet hun wachtwoord opnieuw instellen. Dit betekent dat als de gebruiker bij uw organisatie met een account van een andere organisatie, een Microsoft-account of een Google-account aanmeldt zich, kunt u zich niet op hun wachtwoord opnieuw instellen.<br><br>Als de gebruiker een bron van dienst als Windows Server Active Directory heeft, kunt u zult alleen het wachtwoord opnieuw instellen als u wachtwoord terugschrijven hebt ingeschakeld.<br><br>Als de gebruiker een bron van instantie als externe Azure AD heeft, kunt u zich niet aan het wachtwoord opnieuw instellen. Alleen de gebruiker of een beheerder in de externe Azure AD, kan het wachtwoord opnieuw instellen.

>[!Note]
>Als u niet een beheerder bent en zoekt in plaats daarvan voor instructies over hoe u uw eigen werk- of schoolaccount wachtwoord opnieuw instellen, Zie [uw werk- of schoolaccount wachtwoord opnieuw instellen](../user-help/active-directory-passwords-update-your-own-password.md).

## <a name="to-reset-a-password"></a>Een wachtwoord opnieuw instellen

1. Meld u aan bij de [Azure Portal](https://portal.azure.com/) als gebruikers beheerder of wachtwoord beheerder. Zie voor meer informatie over de beschikbare rollen [beheerdersrollen toewijzen in Azure Active Directory](../users-groups-roles/directory-assign-admin-roles.md#available-roles)

2. Selecteer **Azure Active Directory**, selecteer **gebruikers**, zoekt en selecteert u de gebruiker die het opnieuw instellen moet en selecteer vervolgens **wachtwoord opnieuw instellen**.

    De **Alain Charon - profiel** pagina wordt weergegeven met de **wachtwoord opnieuw instellen** optie.

    ![De profielpagina van gebruiker met de optie voor wachtwoord opnieuw instellen is gemarkeerd](media/active-directory-users-reset-password-azure-portal/user-profile-reset-password-link.png)

3. In de **wachtwoord opnieuw instellen** weergeeft, schakelt **wachtwoord opnieuw instellen**.

    > [!Note]
    > Wanneer u Azure Active Directory gebruikt, wordt er automatisch een tijdelijk wacht woord voor de gebruiker gegenereerd. Wanneer u Active Directory on-premises gebruikt, maakt u het wacht woord voor de gebruiker.

4. Kopiëren van het wachtwoord en geeft u het aan de gebruiker. De gebruiker moet het wachtwoord wijzigen tijdens de volgende aanmelding.

    >[!Note]
    >Het tijdelijke wachtwoord verloopt nooit. De volgende keer dat de gebruiker zich aanmeldt, wordt het wachtwoord nog steeds werken, ongeacht hoeveel tijd is verstreken sinds het tijdelijke wachtwoord is gegenereerd.

## <a name="next-steps"></a>Volgende stappen

Nadat u het wachtwoord van de gebruiker opnieuw instelt hebt, kunt u de volgende basis-processen uitvoeren:

- [Toevoegen of verwijderen van gebruikers](add-users-azure-active-directory.md)

- [Rollen toewijzen aan gebruikers](active-directory-users-assign-role-azure-portal.md)

- [Toevoegen of wijzigen van de profielgegevens](active-directory-users-profile-azure-portal.md)

- [Een basisgroep maken en leden toevoegen](active-directory-groups-create-azure-portal.md)

Of u complexere gebruiker-scenario's, zoals het toewijzen van gemachtigden, met behulp van beleid en het delen van gebruikersaccounts kan uitvoeren. Zie voor meer informatie over andere beschikbare acties [Azure Active Directory management gebruikersdocumentatie](../users-groups-roles/index.yml).
