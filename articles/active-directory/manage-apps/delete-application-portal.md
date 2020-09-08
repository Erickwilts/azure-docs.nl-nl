---
title: 'Quickstart: Een toepassing verwijderen uit uw Azure Active Directory-tenant (Azure AD)'
description: In deze quickstart wordt Azure Portal gebruikt om een toepassing te verwijderen uit uw Azure AD-tenant (Azure Active Directory).
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: quickstart
ms.workload: identity
ms.date: 07/01/2020
ms.author: kenwith
ms.openlocfilehash: 659d136695943d846fe57986d4b64a37f0d8f30e
ms.sourcegitcommit: 58d3b3314df4ba3cabd4d4a6016b22fa5264f05a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/02/2020
ms.locfileid: "89300116"
---
# <a name="quickstart-delete-an-application-from-your-azure-active-directory-azure-ad-tenant"></a>Quickstart: Een toepassing verwijderen uit uw Azure Active Directory-tenant (Azure AD)

In deze quickstart wordt Azure Portal gebruikt om een toepassing die was toegevoegd aan uw Azure AD-tenant (Azure Active Directory) te verwijderen.

## <a name="prerequisites"></a>Vereisten

Als u een toepassing wilt verwijderen uit uw Azure AD-tenant, hebt u het volgende nodig:

- Een Azure-account met een actief abonnement. [Gratis een account maken](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- Een van de volgende rollen: Globale beheerder, Cloudtoepassingsbeheerder, Toepassingsbeheerder of eigenaar van de service-principal.
- Optioneel: Voltooiing van [Uw apps weergeven](view-applications-portal.md).
- Optioneel: Voltooiing van [Een app toevoegen](add-application-portal.md).
- Optioneel: Voltooiing van [Een app configureren](add-application-portal-configure.md).
- Optioneel: Voltooiing van [Gebruikers toewijzen aan een app](add-application-portal-assign-users.md).
- Optioneel: Voltooiing van [Eenmalige aanmelding instellen](add-application-portal-setup-sso.md).

>[!IMPORTANT]
>Gebruik een niet-productieomgeving om de stappen in deze quickstart te testen.

## <a name="delete-an-application-from-your-azure-ad-tenant"></a>Een toepassing verwijderen uit uw Azure AD-tenant

Een toepassing verwijderen uit uw Azure AD-tenant:

1. Selecteer in het Azure AD-portal **Bedrijfstoepassingen**. Zoek en selecteer vervolgens de toepassing die u wilt verwijderen. In dit geval hebben we de toepassing **GitHub_test** verwijderd die we in de vorige quickstart hebben toegevoegd.
1. Selecteer **Eigenschappen** in de sectie **Beheren** van het linkervenster.
1. Selecteer **Verwijderen** en selecteer vervolgens **Ja** om te bevestigen dat u de app uit uw Azure AD-tenant wilt verwijderen.

> [!TIP]
> U kunt het beheer van apps automatiseren met behulp van de Graph API. Zie [App-beheer automatiseren met de Microsoft Graph API](https://docs.microsoft.com/graph/application-saml-sso-configure-api).

## <a name="clean-up-resources"></a>Resources opschonen

Als u de quickstart-reeks hebt voltooid, kunt u de app verwijderen om uw testtenant op te schonen. Hoe u de app verwijdert, is behandeld in deze quickstart.

## <a name="next-steps"></a>Volgende stappen

U hebt de quickstart-reeks voltooid! Lees als volgende stap meer over best practices in app-beheer.
> [!div class="nextstepaction"]
> [Best practices voor toepassingsbeheer](application-management-fundamentals.md)
