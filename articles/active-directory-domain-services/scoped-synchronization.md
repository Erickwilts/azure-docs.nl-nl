---
title: Scoped Synchronization for Azure AD Domain Services | Microsoft Docs
description: Meer informatie over het gebruik van de Azure Portal voor het configureren van scoped Synchronization from Azure AD to a Azure Active Directory Domain Services Managed Domain
services: active-directory-ds
author: MicrosoftGuyJFlo
manager: daveba
ms.assetid: 9389cf0f-0036-4b17-95da-80838edd2225
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: how-to
ms.date: 07/24/2020
ms.author: joflore
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 514932726d9283af0c3fb404f787a10057ce8842
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/13/2020
ms.locfileid: "91967848"
---
# <a name="configure-scoped-synchronization-from-azure-ad-to-azure-active-directory-domain-services-using-the-azure-portal"></a>Configureer de synchronisatie van scoped vanuit Azure AD naar Azure Active Directory Domain Services met behulp van de Azure Portal

Azure Active Directory Domain Services (Azure AD DS) om verificatie services te bieden, synchroniseert gebruikers en groepen van Azure AD. In een hybride omgeving kunnen gebruikers en groepen van een on-premises Active Directory Domain Services-omgeving (AD DS) eerst worden gesynchroniseerd met Azure AD met behulp van Azure AD Connect en vervolgens worden gesynchroniseerd met een door Azure AD DS beheerd domein.

Alle gebruikers en groepen uit een Azure AD-adres lijst worden standaard gesynchroniseerd met een beheerd domein. Als u specifieke vereisten hebt, kunt u in plaats daarvan ervoor kiezen om alleen een gedefinieerde set gebruikers te synchroniseren.

In dit artikel wordt beschreven hoe u scoped Synchronization configureert en vervolgens de set gebruikers met het Azure Portal wijzigt of uitschakelt. U kunt [deze stappen ook uitvoeren met behulp van Power shell][scoped-sync-powershell].

## <a name="before-you-begin"></a>Voordat u begint

U hebt de volgende resources en bevoegdheden nodig om dit artikel te volt ooien:

* Een actief Azure-abonnement.
    * Als u nog geen Azure-abonnement hebt, [maakt u een account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Een Azure Active Directory-tenant die aan uw abonnement is gekoppeld, gesynchroniseerd met een on-premises map of een cloudmap.
    * [Maak zo nodig een Azure Active Directory-tenant][create-azure-ad-tenant] of [koppel een Azure-abonnement aan uw account][associate-azure-ad-tenant].
* Een door Azure Active Directory Domain Services beheerd domein dat in uw Azure AD-tenant is ingeschakeld en geconfigureerd.
    * Als dat nodig is, voltooit u de zelf studie voor het [maken en configureren van een Azure Active Directory Domain Services beheerd domein][tutorial-create-instance].
* U hebt *globale beheerders* bevoegdheden nodig in uw Azure AD-Tenant om het Azure AD DS-synchronisatie bereik te wijzigen.

## <a name="scoped-synchronization-overview"></a>Overzicht van scoped Synchronization

Alle gebruikers en groepen uit een Azure AD-adres lijst worden standaard gesynchroniseerd met een beheerd domein. Als slechts een paar gebruikers toegang nodig hebben tot het beheerde domein, kunt u alleen die gebruikers accounts synchroniseren. Deze synchronisatie met een bereik is gebaseerd op een groep. Wanneer u synchronisatie op basis van een groeps bereik configureert, worden alleen de gebruikers accounts die deel uitmaken van de groepen die u opgeeft gesynchroniseerd met het beheerde domein. Geneste groepen worden niet gesynchroniseerd, alleen de specifieke groepen die u selecteert.

U kunt het synchronisatie bereik wijzigen wanneer u het beheerde domein maakt of wanneer het is geïmplementeerd. U kunt nu ook het synchronisatie bereik wijzigen voor een bestaand beheerd domein zonder dat u het opnieuw hoeft te maken.

Zie voor meer informatie over het synchronisatie proces [begrijpen synchronisatie in azure AD Domain Services][concepts-sync].

> [!WARNING]
> Het wijzigen van het bereik van de synchronisatie zorgt ervoor dat het beheerde domein alle gegevens opnieuw synchroniseert. De volgende overwegingen zijn van toepassing:
>
>  * Wanneer u het synchronisatie bereik voor een beheerd domein wijzigt, wordt een volledige hersynchronisatie uitgevoerd.
>  * Objecten die niet meer in het beheerde domein zijn vereist, worden verwijderd. Nieuwe objecten worden gemaakt in het beheerde domein.

## <a name="enable-scoped-synchronization"></a>Scoped synchronisatie inschakelen

Voer de volgende stappen uit om scoped synchronisatie in te scha kelen in de Azure Portal:

1. Zoek en selecteer **Azure AD Domain Services** in de Azure-portal. Kies uw beheerde domein, bijvoorbeeld *aaddscontoso.com*.
1. Selecteer **synchronisatie** in het menu aan de linkerkant.
1. Selecteer **scoped**voor het *synchronisatie type*.
1. Kies **groepen selecteren**, zoek naar en selecteer de groepen die u wilt toevoegen.
1. Wanneer alle wijzigingen zijn aangebracht, selecteert u **synchronisatie bereik opslaan**.

Het wijzigen van het bereik van de synchronisatie zorgt ervoor dat het beheerde domein alle gegevens opnieuw synchroniseert. Objecten die niet meer nodig zijn in het beheerde domein, worden verwijderd en het kan enige tijd duren voordat de synchronisatie is voltooid.

## <a name="modify-scoped-synchronization"></a>Scoped synchronisatie wijzigen

Voer de volgende stappen uit om de lijst met groepen te wijzigen waarvan gebruikers moeten worden gesynchroniseerd met het beheerde domein:

1. Zoek en selecteer **Azure AD Domain Services** in de Azure-portal. Kies uw beheerde domein, bijvoorbeeld *aaddscontoso.com*.
1. Selecteer **synchronisatie** in het menu aan de linkerkant.
1. Als u een groep wilt toevoegen, **selecteert u groepen bovenaan selecteren** en kiest u vervolgens de groepen die u wilt toevoegen.
1. Als u een groep uit het synchronisatie bereik wilt verwijderen, selecteert u deze in de lijst met momenteel gesynchroniseerde groepen en kiest u **groepen verwijderen**.
1. Wanneer alle wijzigingen zijn aangebracht, selecteert u **synchronisatie bereik opslaan**.

Het wijzigen van het bereik van de synchronisatie zorgt ervoor dat het beheerde domein alle gegevens opnieuw synchroniseert. Objecten die niet meer nodig zijn in het beheerde domein, worden verwijderd en het kan enige tijd duren voordat de synchronisatie is voltooid.

## <a name="disable-scoped-synchronization"></a>Scoped synchronisatie uitschakelen

Voer de volgende stappen uit als u de synchronisatie op basis van een groep wilt uitschakelen voor een beheerd domein:

1. Zoek en selecteer **Azure AD Domain Services** in de Azure-portal. Kies uw beheerde domein, bijvoorbeeld *aaddscontoso.com*.
1. Selecteer **synchronisatie** in het menu aan de linkerkant.
1. Wijzig het *synchronisatie type* van **scoped** in **all**en selecteer vervolgens **synchronisatie bereik opslaan**.

Het wijzigen van het bereik van de synchronisatie zorgt ervoor dat het beheerde domein alle gegevens opnieuw synchroniseert. Objecten die niet meer nodig zijn in het beheerde domein, worden verwijderd en het kan enige tijd duren voordat de synchronisatie is voltooid.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het synchronisatie proces [begrijpen synchronisatie in azure AD Domain Services][concepts-sync].

<!-- INTERNAL LINKS -->
[scoped-sync-powershell]: powershell-scoped-synchronization.md
[concepts-sync]: synchronization.md
[tutorial-create-instance]: tutorial-create-instance.md
[create-azure-ad-tenant]: ../active-directory/fundamentals/sign-up-organization.md
[associate-azure-ad-tenant]: ../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md
