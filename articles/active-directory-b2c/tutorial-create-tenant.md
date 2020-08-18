---
title: 'Zelfstudie: een Azure Active Directory B2C-tenant maken'
description: Volg deze zelfstudie om te ontdekken hoe u de registratie van uw toepassingen kunt voorbereiden door een Azure Active Directory B2C-tenant te maken met behulp van Azure Portal.
services: B2C
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: tutorial
ms.date: 07/01/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: fbccbcf1ac85b63c5610b9904a84e5e6e3fb6c63
ms.sourcegitcommit: 4f1c7df04a03856a756856a75e033d90757bb635
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/07/2020
ms.locfileid: "87922190"
---
# <a name="tutorial-create-an-azure-active-directory-b2c-tenant"></a>Zelfstudie: Een Azure Active Directory B2C-tenant maken

Voordat uw toepassingen kunnen communiceren met Azure Active Directory B2C (Azure AD B2C), moeten deze worden geregistreerd in een tenant die u beheert.

In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Een Azure AD B2C-tenant maken
> * Uw tenant koppelen aan uw abonnement
> * Overschakelen naar de map met uw Azure AD B2C-tenant
> * De Azure AD B2C-resource toevoegen als **favoriet** in Azure Portal

In de volgende zelfstudie leert u hoe u een toepassing registreert.

## <a name="prerequisites"></a>Vereisten

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="create-an-azure-ad-b2c-tenant"></a>Een Azure AD B2C-tenant maken

1. Meld u aan bij de [Microsoft Azure-portal](https://portal.azure.com/). Meld u aan met een Azure-account waaraan ten minste de rol [Inzender](../role-based-access-control/built-in-roles.md) binnen het abonnement of een resourcegroep binnen het abonnement is toegewezen.

1. Selecteer de map met uw abonnement.

    Selecteer het pictogram **Map + abonnement** in de werkbalk van Azure Portal en selecteer vervolgens de map met uw abonnement. Dit is een andere map dan de map waarin uw Azure AD B2C-tenant komt.

    ![Tenant van het abonnement, filter van map en abonnement waarbij de tenant van het abonnement is geselecteerd](media/tutorial-create-tenant/portal-01-pick-directory.png)

1. Selecteer in het menu van de Azure-portal of op de **startpagina** de optie **Een resource maken**.
1. Zoek naar **Azure Active Directory B2C** en selecteer vervolgens **Maken**.
1. Selecteer **Een nieuwe Azure AD B2C-tenant maken**.

    ![Een nieuwe Azure AD B2C-tenant maken die is geselecteerd in Azure Portal](media/tutorial-create-tenant/portal-02-create-tenant.png)

1. Voer op de pagina **Een map maken** de volgende informatie in:

   - **Organisatienaam**: voer een naam in voor uw Azure AD B2C-tenant.
   - **Initiële domeinnaam**: voer een domeinnaam in voor uw Azure AD B2C-tenant.
   - **Land-of regio**: selecteer uw land of regio in de lijst. Deze selectie kan later niet meer worden gewijzigd.
   - **Abonnement**: selecteer uw abonnement in de lijst.
   - **Resourcegroep**: selecteer een resourcegroep die de tenant zal bevatten. Of selecteer **Nieuwe maken**, geef een **naam** op voor de resourcegroep en selecteer de **locatie van de resourcegroep** en daarna **OK**.

    ![Een tenantformulier maken met voorbeeldwaarden in Azure Portal](media/tutorial-create-tenant/review-and-create-tenant.png)

1. Selecteer **Controleren en maken**.
1. Controleer de instellingen voor uw map. Selecteer vervolgens **Maken**.

U kunt meerdere Azure AD B2C-tenants koppelen aan één Azure-abonnement voor factureringsdoeleinden. Als u een tenant wilt koppelen, moet u een beheerder zijn in de Azure AD B2C-tenant en moet aan u ten minste een rol van Inzender zijn toegewezen binnen het Azure-abonnement. Zie [Een Azure AD B2C-tenant koppelen aan een abonnement](billing.md#link-an-azure-ad-b2c-tenant-to-a-subscription).

## <a name="select-your-b2c-tenant-directory"></a>Selecteer de map van uw B2C-tenant

Als u uw nieuwe Azure AD B2C-tenant wilt gaan gebruiken, moet u overschakelen naar de map die de tenant bevat.

Selecteer het filter **Map + abonnement** in het bovenste menu van Azure Portal en selecteer vervolgens de map die uw Azure AD B2C-tenant bevat.

Als u uw nieuwe Azure B2C-tenant niet in de lijst ziet, vernieuwt u het browservenster en selecteert u vervolgens het filter **Map +-abonnement** opnieuw in het bovenste menu.

![B2C-tenant met de map die is geselecteerd in Azure Portal](media/tutorial-create-tenant/portal-07-select-tenant-directory.png)

## <a name="add-azure-ad-b2c-as-a-favorite-optional"></a>Azure AD B2C als favoriet toevoegen (optioneel)

Met deze optionele stap kunt u uw Azure AD B2C-tenant gemakkelijker selecteren in de alle volgende zelfstudies.

U kunt de resource als favoriet toevoegen in plaats van elke keer wanneer u met uw tenant wilt werken te zoeken naar *Azure AD B2C* in **Alle services**. Vervolgens kunt u deze selecteren in het menu **Favorieten** van de portal om snel naar uw Azure AD B2C-tenant te bladeren.

U hoeft deze bewerking slechts één keer uit te voeren. Voordat u deze stappen uitvoert, moet u ervoor zorgen dat u bent overgeschakeld naar de map met uw Azure AD B2C-tenant, zoals wordt beschreven in de vorige sectie [Selecteer de map van uw B2C-tenant](#select-your-b2c-tenant-directory).

1. Meld u aan bij de [Microsoft Azure-portal](https://portal.azure.com).
1. Selecteer **Alle services** in het menu van Azure Portal.
1. Zoek in het zoekvak **Alle services** op **Azure AD B2C**, beweeg de muisaanwijzer over het zoekresultaat en selecteer vervolgens het sterpictogram in de knopinfo. **Azure AD B2C** wordt nu weergegeven in Azure Portal onder **Favorieten**.
1. Als u de positie van uw nieuwe favoriet wilt wijzigen, gaat u naar het Azure Portal-menu, selecteert u **Azure AD B2C** en sleept u het omhoog of omlaag naar de gewenste positie.

    ![Azure AD B2C, menu Favorieten, Microsoft Azure Portal](media/tutorial-create-tenant/portal-08-b2c-favorite.png)

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u het volgende geleerd:

> [!div class="checklist"]
> * Een Azure AD B2C-tenant maken
> * Uw tenant koppelen aan uw abonnement
> * Overschakelen naar de map met uw Azure AD B2C-tenant
> * De Azure AD B2C-resource toevoegen als **favoriet** in Azure Portal

Hierna leert u hoe u een webtoepassing in uw nieuwe tenant kunt registreren.

> [!div class="nextstepaction"]
> [Uw toepassingen registreren >](tutorial-register-applications.md)
