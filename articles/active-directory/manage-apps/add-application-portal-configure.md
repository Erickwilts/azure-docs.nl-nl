---
title: 'Quickstart: Eigenschappen configureren voor een toepassing in uw Azure Active Directory-tenant (Azure AD)'
description: In deze quickstart wordt Azure Portal gebruikt om een toepassing te configureren die is geregistreerd bij uw Azure AD-tenant (Azure Active Directory).
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: quickstart
ms.workload: identity
ms.date: 10/29/2019
ms.author: kenwith
ms.openlocfilehash: d2a538cac1e8bcf9c67c88b206cacd928bfb73c8
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2020
ms.locfileid: "91448240"
---
# <a name="quickstart-configure-properties-for-an-application-in-your-azure-active-directory-azure-ad-tenant"></a>Quickstart: Eigenschappen configureren voor een toepassing in uw Azure Active Directory-tenant (Azure AD)

In de vorige quickstart hebt u een toepassing toegevoegd aan uw Azure Active Directory-tenant (Azure AD). Wanneer u een toepassing toevoegt, laat u uw Azure AD-tenant weten dat deze de ID-provider voor de app is. Nu gaat u een aantal eigenschappen voor de app configureren.
 
## <a name="prerequisites"></a>Vereisten

Als u de eigenschappen van een toepassing in uw Azure AD-Tenant wilt configureren, hebt u het volgende nodig:

- Een Azure-account met een actief abonnement. [Gratis een account maken](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- Een van de volgende rollen: Globale beheerder, Cloudtoepassingsbeheerder, Toepassingsbeheerder of eigenaar van de service-principal.
- Optioneel: Voltooiing van [Uw apps weergeven](view-applications-portal.md).
- Optioneel: Voltooiing van [Een app toevoegen](add-application-portal.md).

>[!IMPORTANT]
>Gebruik een niet-productieomgeving om de stappen in deze quickstart te testen.

## <a name="configure-app-properties"></a>App-eigenschappen configureren

Wanneer u klaar bent met het toevoegen van een toepassing aan uw Azure AD-tenant, verschijnt de overzichtspagina. Als u een toepassing wilt configureren die al is toegevoegd, bekijk dan de eerste quickstart. U krijgt een overzicht te zien van de toepassingen die aan uw tenant zijn toegevoegd. 

Ga als volgt te werk om de eigenschappen van de toepassing bewerken:

1. Selecteer in het Azure AD-portal **Bedrijfstoepassingen**. Zoek en selecteer vervolgens de toepassing die u wilt configureren.
2. Selecteer **Eigenschappen** in de sectie **Beheren** om het deelvenster **Eigenschappen** te openen voor bewerken.

    ![Schermopname van het scherm Eigenschappen waarop de eigenschappen van de bewerkbare app te zien zijn.](media/add-application-portal/edit-properties.png)

3. Neem even de tijd om de opties die beschikbaar zijn voor configuratie te bestuderen:
    - Met **Ingeschakeld voor gebruikers voor aanmelden?** wordt bepaald of gebruikers die zijn toegewezen aan de toepassing, zich kunnen aanmelden.
    - Met **Gebruikerstoewijzing vereist?** wordt bepaald of gebruikers die niet zijn toegewezen aan de toepassing, zich kunnen aanmelden.
    - Met **Zichtbaar voor gebruikers?** wordt bepaald of gebruikers die zijn toegewezen aan een app, deze kunnen zien in [Mijn apps](https://myapps.microsoft.com) en het startprogramma voor de Microsoft 365-app. (Bekijk het menu in de linkerbovenhoek van een Microsoft 365-website.)
    
    > [!TIP]
    > U kunt gebruikers toewijzen in de sectie **Gebruikers en groepen** in het navigatiegedeelte.

    U kunt onafhankelijk schakelen tussen de drie opties; het resulterende gedrag is niet altijd even duidelijk. Deze tabel is wellicht nuttig:
    
    | Ingeschakeld voor gebruikers voor aanmelden? | Gebruikerstoewijzing vereist? | Zichtbaar voor gebruiker? | Gedrag voor gebruikers die al dan niet aan de app zijn toegewezen. |
    |---|---|---|---|
    | Ja | Ja | Ja | Toegewezen gebruikers kunnen de app zien en zich aanmelden.<br>Niet-toegewezen gebruikers kunnen de app niet zien en zich niet aanmelden. |
    | Ja | Ja | Nee  | Toegewezen gebruikers kunnen de app niet zien maar zich wel aanmelden.<br>Niet-toegewezen gebruikers kunnen de app niet zien en zich niet aanmelden. |
    | Ja | Nee  | Ja | Toegewezen gebruikers kunnen de app zien en zich aanmelden.<br>Niet-toegewezen gebruikers kunnen de app niet zien maar zich wel aanmelden. |
    | Ja | Nee  | Nee  | Toegewezen gebruikers kunnen de app niet zien maar zich wel aanmelden.<br>Niet-toegewezen gebruikers kunnen de app niet zien maar zich wel aanmelden. |
    | Nee  | Ja | Ja | Toegewezen gebruikers kunnen de app niet zien en zich niet aanmelden.<br>Niet-toegewezen gebruikers kunnen de app niet zien en zich niet aanmelden. |
    | Nee  | Ja | Nee  | Toegewezen gebruikers kunnen de app niet zien en zich niet aanmelden.<br>Niet-toegewezen gebruikers kunnen de app niet zien en zich niet aanmelden. |
    | Nee  | Nee  | Ja | Toegewezen gebruikers kunnen de app niet zien en zich niet aanmelden.<br>Niet-toegewezen gebruikers kunnen de app niet zien en zich niet aanmelden. |
    | Nee  | Nee  | Nee  | Toegewezen gebruikers kunnen de app niet zien en zich niet aanmelden.<br>Niet-toegewezen gebruikers kunnen de app niet zien en zich niet aanmelden. |

4. Selecteer **Opslaan** wanneer u klaar bent.

## <a name="use-a-custom-logo"></a>Een aangepast logo gebruiken

Ga als volgt te werk om een aangepast logo te gebruiken:

1. Maak een logo van 215 x 215 pixels en sla dit op in de .png-indeling.
2. Selecteer in het Azure AD-portal **Bedrijfstoepassingen**. Zoek en selecteer vervolgens de toepassing die u wilt configureren.
3. Selecteer **Eigenschappen** in de sectie **Beheren** om het deelvenster **Eigenschappen** te openen voor bewerken. 
4. Selecteer het pictogram om het logo te uploaden.
5. Selecteer **Opslaan** wanneer u klaar bent.

    ![Schermopname van het scherm Eigenschappen waarop te zien is hoe u het logo kunt wijzigen.](media/add-application-portal/change-logo.png)

   > [!NOTE]
   > De miniatuur die in het deelvenster **Eigenschappen** wordt weergegeven, wordt niet meteen bijgewerkt. U kunt het venster **Eigenschappen** sluiten en opnieuw openen om het bijgewerkte pictogram weer te geven.


> [!TIP]
> U kunt het beheer van apps automatiseren met behulp van de Graph API. Zie [App-beheer automatiseren met de Microsoft Graph API](https://docs.microsoft.com/graph/application-saml-sso-configure-api).

## <a name="add-notes"></a>Notities toevoegen

U kunt het veld Notities gebruiken om informatie toe te voegen die relevant is voor het beheer van de toepassing in Azure AD. Notities is een vrije-tekstveld met een maximale grootte van 1024 tekens.

1. Selecteer in het Azure AD-portal **Bedrijfstoepassingen**. Zoek en selecteer vervolgens de toepassing die u wilt configureren.
2. Selecteer **Eigenschappen** in de sectie **Beheren** om het deelvenster **Eigenschappen** te openen voor bewerken.
3. Werk het veld Notities bij en selecteer **Opslaan**.

    ![Schermopname van het scherm Eigenschappen waarop te zien is hoe u de notities kunt wijzigen.](media/add-application-portal/notes-application.png)

    
## <a name="clean-up-resources"></a>Resources opschonen

Als u niet verder gaat met de quickstart-reeks, kunt u de app verwijderen om uw testtenant op te schonen. Zie [Een app verwijderen](delete-application-portal.md) in de laatste quickstart in deze reeks voor meer informatie over het verwijderen van een app.

## <a name="next-steps"></a>Volgende stappen

Ga naar het volgende artikel om te lezen hoe u gebruikers toewijst aan een app.
> [!div class="nextstepaction"]
> [Gebruikers toewijzen aan een app](add-application-portal-assign-users.md)
