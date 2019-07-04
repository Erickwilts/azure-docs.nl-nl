---
title: Een basisgroep maken en leden toevoegen - Azure Active Directory | Microsoft Docs
description: Instructies over het maken van een basisgroep met behulp van Azure Active Directory.
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: quickstart
ms.date: 03/01/2019
ms.author: lizross
ms.reviewer: krbain
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: d47c742e4f6d2ba8a96e9897f43231e509e8aa63
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2019
ms.locfileid: "67476070"
---
# <a name="create-a-basic-group-and-add-members-using-azure-active-directory"></a>Een basisgroep maken en leden toevoegen met Azure Active Directory
U kunt een basisgroep maken met behulp van de portal Azure Active Directory (Azure AD). Voor de toepassing van dit artikel wordt een basisgroep toegevoegd aan één resource door de resource-eigenaar (administrator) en deze bevat specifieke leden (werknemers) die toegang nodig hebben tot die resource. Voor complexere scenario's, met inbegrip van dynamische lidmaatschappen en regels maken, zie de [Azure Active Directory gebruiker beheerdocumentatie](../users-groups-roles/index.yml).

## <a name="create-a-basic-group-and-add-members"></a>Een basisgroep maken en leden toevoegen
U kunt op hetzelfde moment een basisgroep maken en leden toevoegen.

### <a name="to-create-a-basic-group-and-add-members"></a>Een basisgroep maken en leden toevoegen
1. Meld u aan bij de [Azure Portal](https://portal.azure.com) met het account van een globale administrator voor de map.

2. Selecteer **Azure Active Directory**, **Groepen** en vervolgens **Nieuwe groep**.

    ![Azure AD-pagina, met groepen weergeven](media/active-directory-groups-create-azure-portal/group-full-screen.png)

3. In de pagina **Groep**, vult u de vereiste gegevens in.

    ![Pagina van de nieuwe groep ingevuld met de voorbeeld-info](media/active-directory-groups-create-azure-portal/new-group-blade.png)

   - **Groepstype (vereist).** Selecteer een vooraf gedefinieerd groepstype. Dit omvat:
        
       - **Beveiliging**. Gebruikt voor het beheren van leden en toegang tot computers voor gedeelde resources voor een groep gebruikers. U kunt bijvoorbeeld een beveiligingsgroep voor een specifiek beveiligingsbeleid maken. Op deze manier kunt u in één keer een reeks machtigingen geven aan alle leden in plaats van machtigingen individueel voor elk lid toe te voegen. Zie voor meer informatie over het beheren van toegang tot resources [Toegang tot resources beheren met Azure Active Directory-groepen](active-directory-manage-groups.md).
        
       - **Office 365**. Biedt mogelijkheden voor samenwerking door leden toegang te geven tot een gedeeld postvak, agenda, bestanden, SharePoint-site en meer. Deze optie geeft u ook de mogelijkheid om mensen buiten uw organisatie toegang te geven tot de groep. Zie voor meer informatie over Office 365-groepen [Informatie over Office 365-groepen](https://support.office.com/article/learn-about-office-365-groups-b565caa1-5c40-40ef-9915-60fdb2d97fa2).

   - **Groepsnaam (vereist).** Voeg een naam voor de groep toe, iets wat u moet onthouden en dat klinkt logisch. Een controle uitgevoerd om te bepalen of de naam al wordt gebruikt voor een andere groep. Als de naam al gebruikt wordt, om te voorkomen dat dubbele geven u gevraagd om te wijzigen van de naam van uw groep.

   - **Groepsbeschrijving.** Voeg een optionele beschrijving toe aan uw groep.

   - **Lidmaatschapstype (vereist).** Selecteer een vooraf gedefinieerd lidmaatschapstype. Dit omvat:

     - **Toegewezen.** Hiermee kunt u specifieke gebruikers lid maken van deze groep en hen unieke machtigingen geven. Voor de doeleinden van dit artikel gebruiken we deze optie.

     - **Dynamische gebruiker.** Kunt u dynamisch-lidmaatschapregels gebruiken om automatisch toevoegen en verwijderen van leden. Als de kenmerken van een lid wijzigen, controleert het systeem de dynamische groepsregels voor de map om te zien of het lid aan de regelvereisten voldoet (wordt toegevoegd) of niet langer voldoet aan de regelvereisten (wordt verwijderd).

     - **Dynamisch apparaat.** Hiermee kunt u dynamische groepsregels gebruiken voor automatisch toevoegen en verwijderen van apparaten. Als de kenmerken van een apparaat wijzigen, controleert het systeem de dynamische groepsregels voor de map om te zien of het apparaat aan de regelvereisten voldoet (wordt toegevoegd) of niet langer voldoet aan de regelvereisten (wordt verwijderd).

       >[!Important]
       >U kunt een dynamische groep voor apparaten of gebruikers, maar niet voor beide maken. Het is evenmin mogelijk om een apparaatgroep te maken op basis van kenmerken van de apparaateigenaren. Regels voor apparaatlidmaatschap kunnen alleen verwijzen naar apparaatkenmerken. Zie [Een dynamische groep maken en de status controleren](../users-groups-roles/groups-create-rule.md) voor meer informatie over het maken van een dynamische groep voor gebruikers en apparaten.

4. Selecteer **Maken**.

    Uw groep is gemaakt en gereed om leden toe te voegen.

5. Selecteer het gebied **Leden** van de pagina **Groep** en begin vervolgens te zoeken naar de leden om ze toe te voegen aan uw groep van de pagina **Leden selecteren**.

    ![Leden voor uw groep selecteren tijdens het proces voor het maken van groep](media/active-directory-groups-create-azure-portal/select-members-create-group.png)

6. Wanneer u klaar bent met het toevoegen van leden, kiest u **Selecteer**.

    De pagina **Groepsoverzicht** wordt bijgewerkt om het aantal leden dat nu is toegevoegd aan de groep weer te geven.

    ![Pagina Groepsoverzicht met aantal leden gemarkeerd](media/active-directory-groups-create-azure-portal/group-overview-blade-number-highlight.png)

## <a name="turn-on-or-off-welcome-email"></a>Schakel in of uit de verwelkomingse-mail

Wanneer een nieuwe Office 365-groep is gemaakt, of met dynamisch of statisch lidmaatschap heeft, wordt een Welkom melding verzonden naar alle gebruikers die zijn toegevoegd aan de groep. Wanneer kenmerken van een gebruiker of apparaat wijzigt, worden alle dynamische groepsregels in de organisatie worden verwerkt voor mogelijke wijzigingen in het lidmaatschap. Gebruikers die zijn toegevoegd vervolgens ook de Welkom melding ontvangen. U kunt dit gedrag uitschakelen [Exchange PowerShell](https://docs.microsoft.com/powershell/module/exchange/users-and-groups/Set-UnifiedGroup?view=exchange-ps). 

## <a name="next-steps"></a>Volgende stappen
Nu dat u een groep en ten minste één gebruiker hebt toegevoegd, kunt u het volgende doen:

- [Groepen en leden weergeven](active-directory-groups-view-azure-portal.md)

- [Groepslidmaatschap beheren](active-directory-groups-membership-azure-portal.md)

- [Dynamische regels voor gebruikers in een groep beheren](../users-groups-roles/groups-create-rule.md)

- [Instellingen van uw groep bewerken](active-directory-groups-settings-azure-portal.md)

- [Toegang tot resources met behulp van groepen beheren](active-directory-manage-groups.md)

- [Toegang tot SaaS-apps met behulp van groepen beheren](../users-groups-roles/groups-saasapps.md)

- [Groepen met behulp van PowerShell-opdrachten beheren](../users-groups-roles/groups-settings-v2-cmdlets.md)

- [Een Azure-abonnement aan Azure Active Directory koppelen of toevoegen](active-directory-how-subscriptions-associated-directory.md)
