---
title: Externe B2B-samenwerkings instellingen inschakelen-Azure AD
description: Meer informatie over het inschakelen van Active Directory B2B externe samen werking en het beheren van wie gast gebruikers kunnen uitnodigen. Gebruik de rol gast uitnodiging voor het delegeren van uitnodigingen.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: how-to
ms.date: 08/20/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.collection: M365-identity-device-management
ms.openlocfilehash: ae8bb66141e4cc4e67f1502b208cf519d37c0374
ms.sourcegitcommit: e0785ea4f2926f944ff4d65a96cee05b6dcdb792
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/21/2020
ms.locfileid: "88705984"
---
# <a name="enable-b2b-external-collaboration-and-manage-who-can-invite-guests"></a>Externe B2B-samenwerking inschakelen en beheren wie gasten kan uitnodigen

In dit artikel wordt beschreven hoe u de samen werking van Azure Active Directory (Azure AD) inschakelt, aanwijst wie gasten kan uitnodigen en welke machtigingen gast gebruikers hebben in uw Azure AD. 

Standaard kunnen alle gebruikers en gasten in uw directory gasten uitnodigen, zelfs als ze niet zijn toegewezen aan een beheerdersrol. Met instellingen voor externe samen werking kunt u uitnodigingen voor gasten in-of uitschakelen voor verschillende soorten gebruikers in uw organisatie. U kunt uitnodigingen ook overdragen aan afzonderlijke gebruikers door rollen toe te wijzen waarmee gasten kunnen worden uitgenodigd.

Met Azure AD kunt u bepalen welke externe gast gebruikers in uw Azure AD-Directory kunnen worden weer geven. Gast gebruikers zijn standaard ingesteld op een beperkt machtigings niveau waarmee ze worden geblokkeerd voor het inventariseren van gebruikers, groepen of andere Directory bronnen, maar ze kunnen het lidmaatschap van niet-verborgen groepen zien. Met een nieuwe preview-instelling kunt u de toegang tot gasten nog verder beperken, zodat gasten alleen hun eigen profiel gegevens kunnen weer geven. 

## <a name="configure-b2b-external-collaboration-settings"></a>Externe B2B-samenwerkings instellingen configureren

Met Azure AD B2B Collaboration kan een Tenant beheerder het volgende uitnodigings beleid instellen:

- Uitnodigingen uitschakelen
- Alleen beheerders en gebruikers in de rol van de gast-uitnodiging kunnen uitnodigen
- Beheerders, de rol van de gast-uitnodiging en leden kunnen uitnodigen
- Alle gebruikers, inclusief gasten, kunnen uitnodigen

Standaard kunnen alle gebruikers, inclusief gasten, gast gebruikers uitnodigen.

### <a name="to-configure-external-collaboration-settings"></a>Externe samenwerkings instellingen configureren:

1. Meld u aan bij de [Azure Portal](https://portal.azure.com) als een Tenant beheerder.
2. Selecteer **Azure Active Directory**.
3. Externe **identiteiten**  >  **externe instellingen voor samen werking**selecteren.

4. Kies onder **toegangs beperkingen voor gast gebruikers (preview)** het toegangs niveau dat u wilt dat gast gebruikers hebben:

   > [!IMPORTANT]
   > Voor een korte periode zijn deze nieuwe portal besturings elementen voor gast gebruikers machtigingen alleen zichtbaar via de URL [https://aka.ms/AADRestrictedGuestAccess](https://aka.ms/AADRestrictedGuestAccess) . Zie [machtigingen voor gast toegang beperken (preview)](https://aka.ms/exid-users-restrict-guest-permissions)voor meer informatie.

   - **Gast gebruikers hebben dezelfde toegang als leden (ten opzichte van de meeste)**: met deze optie krijgen gasten dezelfde toegang tot Azure AD-resources en Directory gegevens als gebruikers van een lid.

   - **Gast gebruikers hebben beperkte toegang tot de eigenschappen en lidmaatschappen van Directory-objecten**: (standaard) met deze instelling worden gasten geblokkeerd voor bepaalde directory taken, zoals het inventariseren van gebruikers, groepen of andere Directory bronnen. Gasten kunnen lidmaatschap van alle niet-verborgen groepen zien.

   - **Toegang voor gast gebruikers is beperkt tot eigenschappen en lidmaatschappen van hun eigen Directory-objecten (het meest beperkend)**: met deze instelling hebben gasten alleen toegang tot hun eigen profielen. Gasten mogen geen profielen, groepen of groepslid maatschappen van andere gebruikers zien.
  
    ![Instellingen voor toegangs beperkingen van gast gebruikers](./media/delegate-invitations/guest-user-access.png)

5. Kies onder **instellingen voor uitnodiging voor gast**de gewenste instellingen:

   - **Beheerders en gebruikers in de rol van de gast-uitnodiging kunnen uitnodigen**: als u beheerders en gebruikers in de rol ' gast uitnodiging ' wilt toestaan om gasten uit te nodigen, stelt u dit beleid in op **Ja**.

   - **Leden kunnen uitnodigen**: Stel dit beleid in op **Ja**als u niet-beheerders leden van uw directory toestemming wilt geven gasten uit te nodigen.

   - **Gasten kunnen uitnodigen**: als u gasten wilt toestaan andere gasten uit te nodigen, stelt u dit beleid in op **Ja**.

   - **Eenmalige e-mail wachtwoord voor gasten inschakelen (preview-versie)**: Zie [eenmalige verificatie per e-mail (preview)](one-time-passcode.md)voor meer informatie over de functie voor eenmalige wachtwoord code.

   - **Aanmelden via self-service voor gasten inschakelen via de gebruikers stromen (preview)**: Zie [een self-service voor het registreren van een selfservice gebruiker toevoegen aan een app (preview)](self-service-sign-up-user-flow.md)voor meer informatie over deze instelling.

   > [!NOTE]
   > Als **leden kunnen worden uitgenodigd** is ingesteld op **Nee** en **beheerders en gebruikers in de rol gast uitnodigingen kunnen worden uitgenodigd** is ingesteld op **Ja**, kunnen gebruikers in de rol **gast uitnodiging** nog steeds gasten uitnodigen.

    ![Instellingen voor uitnodiging voor gast](./media/delegate-invitations/guest-invite-settings.png)

6. Kies onder **samenwerkings beperkingen**of u uitnodigingen wilt toestaan of weigeren aan de domeinen die u opgeeft. Zie [uitnodigingen voor B2B-gebruikers van specifieke organisaties toestaan of blok keren](allow-deny-list.md)voor meer informatie.

## <a name="assign-the-guest-inviter-role-to-a-user"></a>De rol van de gast-uitnodiging toewijzen aan een gebruiker

Met de rol gast uitnodiging kunt u afzonderlijke gebruikers de mogelijkheid geven om gasten uit te nodigen zonder hen een globale beheerder of andere beheerdersrol toe te wijzen. Wijs de rol van de gast-uitnodiging toe aan personen. Zorg er vervolgens voor dat u **beheerders en gebruikers instelt in de rol gast uitnodiging. Dit kan worden uitgenodigd** voor **Ja**.

Hier volgt een voor beeld waarin wordt uitgelegd hoe u Power shell gebruikt om een gebruiker toe te voegen aan de rol van de gast-uitnodiging:

```
Add-MsolRoleMember -RoleObjectId 95e79109-95c0-4d8e-aee3-d01accf2d47b -RoleMemberEmailAddress <RoleMemberEmailAddress>
```

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de volgende artikelen over Azure AD B2B-samen werking:

- [Wat is Azure AD B2B-samenwerking?](what-is-b2b.md)
- [Gast gebruikers voor B2B-samen werking toevoegen zonder uitnodiging](add-user-without-invite.md)
- [Een B2B-samenwerkings gebruiker toevoegen aan een rol](add-guest-to-role.md)

