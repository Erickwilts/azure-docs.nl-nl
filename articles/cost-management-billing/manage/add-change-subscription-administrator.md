---
title: Azure-abonnementsbeheerders toevoegen of wijzigen
description: Hierin wordt beschreven hoe u een Azure-abonnementsbeheerder kunt toevoegen of wijzigen met behulp van op rollen gebaseerd toegangsbeheer van Azure (Azure RBAC).
author: genlin
ms.reviewer: dcscontentpm
tags: billing
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: conceptual
ms.date: 08/20/2020
ms.author: banders
ms.openlocfilehash: 10956953f9ab3a9e32b9da4ab8a3501d38b0e2c3
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/22/2020
ms.locfileid: "92369655"
---
# <a name="add-or-change-azure-subscription-administrators"></a>Azure-abonnementsbeheerders toevoegen of wijzigen


Om de toegang tot Azure-resources te beheren, moet u over de juiste beheerdersrol beschikken. Azure heeft een autorisatiesysteem met de naam [Op rollen gebaseerd toegangsbeheer van Azure (Azure RBAC)](../../role-based-access-control/overview.md) met verschillende ingebouwde rollen waaruit u kunt kiezen. U kunt deze rollen toewijzen aan verschillende bereiken, zoals de beheergroep, het abonnement of de resourcegroep. Standaard kan de persoon die een nieuw Azure-abonnement maakt, andere gebruikers beheerderstoegang tot een abonnement toekennen.

In dit artikel wordt beschreven hoe u de beheerdersrol voor een gebruiker toevoegt of wijzigt met behulp van Azure RBAC in het abonnementsbereik.

Microsoft raadt u aan om de toegang tot resources te beheren met Azure RBAC. Als u echter nog steeds gebruikmaakt van het klassieke implementatiemodel en de klassieke resources beheert met behulp van [de Power shell-module van Azure Service Management](/powershell/module/servicemanagement/azure.service), moet u een klassieke beheerder gebruiken.

> [!TIP]
> Als u de Azure-portal alleen gebruikt voor het beheren van de klassieke resources, hoeft u de klassieke beheerdersrol niet te gebruiken.

Zie [Azure Resource Manager vs. klassieke implementatie](../../azure-resource-manager/management/deployment-models.md) en [Beheerders van het klassieke Azure-abonnement](../../role-based-access-control/classic-administrators.md) voor meer informatie.

<a name="add-an-admin-for-a-subscription"></a>

## <a name="assign-a-subscription-administrator"></a>Abonnementsbeheerder toewijzen

Als u een gebruiker beheerder van een Azure-abonnement wilt maken, wijst een bestaande beheerder aan de gebruiker de rol van [Eigenaar](../../role-based-access-control/built-in-roles.md#owner) (een Azure-rol) toe voor het abonnementsbereik. De rol van eigenaar geeft de gebruiker volledige toegang tot alle resources in het abonnement, waaronder het recht om toegang aan anderen te delegeren. Deze stappen zijn hetzelfde als die voor elke andere functietoewijzing.

Als u niet zeker weet wie de accountbeheerder is voor een abonnement, gebruikt u de volgende stappen om erachter te komen.

1. Open de [pagina Abonnementen in de Azure-portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
1. Selecteer het abonnement dat u wilt controleren en kijk vervolgens bij **Instellingen** .
1. Selecteer **Eigenschappen** . De accountbeheerder van het abonnement wordt weergegeven in het vak **Accountbeheerder** .

### <a name="to-assign-a-user-as-an-administrator"></a>Gebruiker toewijzen als beheerder

1. Meld u bij de Azure-portal aan als de eigenaar van het abonnement en open [Abonnementen](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).

1. Klik op het abonnement waartoe u toegang wilt verlenen.

1. Klik op **Toegangsbeheer (IAM)** .

1. Klik op het tabblad **Roltoewijzingen** om alle roltoewijzingen voor dit abonnement weer te geven.

    ![Schermopname met roltoewijzingen](./media/add-change-subscription-administrator/role-assignments.png)

1. Klik op **Toevoegen** > **Roltoewijzing toevoegen** om het deelvenster **Roltoewijzing toevoegen** te openen.

    Als u niet bent gemachtigd voor het toewijzen van rollen, is de optie uitgeschakeld.

1. Selecteer in de vervolgkeuzelijst **Rol** de rol **Eigenaar** .

1. Selecteer een gebruiker in de lijst **Selecteren** . Als u de gebruiker niet in de lijst ziet staan, kunt u tekst typen in het vak **Selecteren** om in de map te zoeken naar weergavenamen en e-mailadressen.

    ![Schermopname met de rol Eigenaar geselecteerd](./media/add-change-subscription-administrator/add-role.png)

1. Klik op **Opslaan** om de rol toe te wijzen.

    Na enkele ogenblikken wordt de rol van eigenaar op abonnementsniveau toegewezen aan de gebruiker.

## <a name="next-steps"></a>Volgende stappen

* [Wat is Azure RBAC (toegangsbeheer op basis van rollen)?](../../role-based-access-control/overview.md)
* [Inzicht in de verschillende rollen](../../role-based-access-control/rbac-and-directory-admin-roles.md)
* [Een Azure-abonnement aan uw Azure Active Directory-tenant toevoegen of koppelen](../../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md)
* [Machtigingen voor beheerrol in Azure Active Directory](../../active-directory/roles/permissions-reference.md)

## <a name="need-help-contact-support"></a>Hebt u hulp nodig? Contact opnemen met ondersteuning

Als u nog steeds hulp nodig hebt, neemt u [contact op met de ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel op te lossen.
