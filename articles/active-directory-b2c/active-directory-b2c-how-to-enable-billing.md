---
title: 'Het koppelen van een Azure-abonnement: Azure Active Directory B2C | Microsoft Docs'
description: Stapsgewijze handleiding voor het inschakelen van de facturering voor Azure AD B2C-tenant in een Azure-abonnement.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.date: 01/24/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: d2440b638a9c8eb27505b747d26917de8032e6cd
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66510130"
---
# <a name="link-an-azure-subscription-to-an-azure-active-directory-b2c-tenant"></a>Een Azure-abonnement koppelen aan een Azure Active Directory B2C-tenant

> [!IMPORTANT]
> Zie voor de meest recente informatie over het gebruik van facturering en prijzen voor Azure Active Directory (Azure AD) B2C, [prijzen van Azure AD B2C](https://azure.microsoft.com/pricing/details/active-directory-b2c/).

Kosten voor het gebruik van Azure AD B2C worden gefactureerd met een Azure-abonnement. Wanneer een Azure AD B2C-tenant is gemaakt, wordt de tenantbeheerder moet expliciet de Azure AD B2C-tenant koppelen aan een Azure-abonnement. In dit artikel ziet u hoe u.

> [!NOTE]
> Een abonnement dat is gekoppeld aan een Azure AD B2C-tenant kan worden gebruikt voor de facturering van Azure AD B2C-gebruik of andere Azure-resources met inbegrip van aanvullende resources voor Azure AD B2C.  Het kan niet worden gebruikt voor het toevoegen van andere Azure-licentie op basis van services of Office 365-licenties in de Azure AD B2C-tenant.

De abonnementskoppeling wordt bereikt door het maken van een Azure AD B2C 'resource' in de doel-Azure-abonnement. Veel Azure AD B2C 'resources' kunnen worden gemaakt binnen een enkel Azure-abonnement, samen met andere Azure-resources (bijvoorbeeld virtuele machines, gegevensopslag, LogicApps). Alle resources binnen het abonnement kunt u bekijken door te gaan naar de Azure AD-tenant waaraan het abonnement is gekoppeld.

Azure Cloud Solution Providers (CSP)-abonnementen worden ondersteund in Azure AD B2C. De functionaliteit is beschikbaar voor Azure AD B2C en voor alle Azure-resources met behulp van de API's of de Azure-portal. Beheerders van CSP-abonnement kunnen koppelen, verplaatsen en verwijderen van relaties met Azure AD B2C op dezelfde manier die is uitgevoerd voor alle Azure-resources. Het beheer van Azure AD B2C met behulp van op rollen gebaseerd toegangsbeheer wordt niet beïnvloed door de koppeling tussen de Azure AD B2C-tenant en een Azure CSP-abonnement. Op rollen gebaseerd toegangsbeheer wordt bereikt door middel van de tenant-base-rollen, geen abonnement op basis van rollen.

Er is een geldig Azure-abonnement nodig om door te gaan.

## <a name="create-an-azure-ad-b2c-tenant"></a>Een Azure AD B2C-tenant maken

U moet eerst [maken van een Azure AD B2C-tenant](active-directory-b2c-get-started.md) dat u wilt een abonnement aan koppelen. Deze stap overslaan als u een Azure AD B2C-tenant al hebt gemaakt.

## <a name="open-azure-portal-in-the-azure-ad-tenant-that-shows-your-azure-subscription"></a>Open Azure portal in de Azure AD-tenant die laat zien van uw Azure-abonnement

Navigeer naar de Azure AD-tenant die laat zien van uw Azure-abonnement. Open de [Azure-portal](https://portal.azure.com), en schakel over naar de Azure AD-tenant waarin de Azure-abonnement u wilt gebruiken.

![Overschakelen naar uw Azure AD-tenant](./media/active-directory-b2c-how-to-enable-billing/SelectAzureADTenant.png)

## <a name="find-azure-ad-b2c-in-the-azure-marketplace"></a>Azure AD B2C niet vinden in de Azure Marketplace

Klik op de **een resource maken** knop. Voer in het veld **Marketplace doorzoeken** in: `B2C`.

![Knop toevoegen gemarkeerd en de tekst Azure AD B2C in het veld marketplace doorzoeken](../../includes/media/active-directory-b2c-create-tenant/find-azure-ad-b2c.png)

Selecteer in de lijst met resultaten **Azure AD B2C**.

![Azure AD B2C geselecteerd in de lijst met resultaten](../../includes/media/active-directory-b2c-create-tenant/find-azure-ad-b2c-result.png)

Meer informatie over Azure AD B2C worden weergegeven. Klik op de knop **Maken** om de nieuwe Azure Active Directory B2C-tenant te configureren.

Selecteer in het scherm voor het maken van resource **koppeling een bestaande Azure AD B2C-Tenant aan mijn Azure-abonnement**.

## <a name="create-an-azure-ad-b2c-resource-within-the-azure-subscription"></a>Maak een Azure AD B2C-resource binnen het Azure-abonnement

Selecteer een Azure AD B2C-tenant in de vervolgkeuzelijst in het dialoogvenster voor het maken van resources. U ziet alle van de tenants dat u de globale beheerder van en die niet al zijn gekoppeld aan een abonnement.

De naam van de Azure AD B2C-resource wordt vooraf geselecteerd zodat deze overeenkomt met de domeinnaam van de Azure AD B2C-tenant.

Voor het abonnement, selecteert u een actief Azure-abonnement dat u de beheerder van zijn.

Selecteer een resourcegroep en locatie van de resourcegroep. De selectie die u hier heeft geen invloed op uw locatie van de Azure AD B2C-tenant, de prestaties of de status van de facturering.

![B2C-Resource maken](./media/active-directory-b2c-how-to-enable-billing/createresourceb2c.png)

## <a name="manage-your-azure-ad-b2c-tenant-resources"></a>Uw Azure AD B2C-tenantresources beheren

Als een Azure AD B2C-resource is gemaakt binnen de Azure-abonnement, ziet u een nieuwe resource van het type 'B2C-tenant' toegevoegd naast uw andere Azure-resources.

U kunt deze resource om te gebruiken:

- Ga naar het abonnement om te controleren van factureringsgegevens.
- Ga naar uw Azure AD B2C-tenant
- Een ondersteuningsaanvraag indienen
- Uw Azure AD B2C-tenant-resource verplaatsen naar een andere Azure-abonnement of naar een andere resourcegroep.

![B2C-Resource-instellingen](./media/active-directory-b2c-how-to-enable-billing/b2cresourcesettings.png)

## <a name="known-issues"></a>Bekende problemen

### <a name="self-imposed-restrictions"></a>Zelf opgelegde beperkingen

Een gebruiker kan een regionale beperkingen voor het maken van Azure-resource hebt ingesteld. Deze beperking, waardoor het maken van Azure AD B2C-resource. Als u wilt beperken, verwijzen wij u versoepelen deze beperking.

## <a name="next-steps"></a>Volgende stappen

Zodra deze stappen voltooid voor elk van uw Azure AD B2C-tenants zijn, wordt uw Azure-abonnement gefactureerd in overeenstemming met de details van uw Azure Direct of Enterprise Agreement.

U kunt het gebruik en factureringsgegevens bekijken binnen de geselecteerde Azure-abonnement. U kunt ook bekijken gedetailleerd gebruik van de dag rapporten met behulp van de [gebruik rapportage-API](active-directory-b2c-reference-usage-reporting-api.md).
