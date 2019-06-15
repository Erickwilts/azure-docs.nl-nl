---
title: Gedelegeerde toegang in Windows Virtual Desktop Preview - Azure
description: Klik hier voor meer informatie over het overdragen van beheerdersbevoegdheden op de implementatie van een Windows virtuele bureaublad Preview, inclusief voorbeelden.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: helohr
ms.openlocfilehash: 250aea52de63a6397ce00e9cadcadf3a8ba39858
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60870503"
---
# <a name="delegated-access-in-windows-virtual-desktop-preview"></a>Gedelegeerde toegang in Windows Virtual Desktop Preview

Windows virtuele bureaublad Preview is een model voor gedelegeerde toegang waarmee u bij het definiëren van de mate van toegang dat een bepaalde gebruiker is toegestaan dat ze een rol toe te wijzen. Een roltoewijzing bestaat uit drie onderdelen: beveiligings-principal, rol en bereik. Het model van de gedelegeerde toegang virtueel bureaublad van Windows is gebaseerd op het Azure RBAC-model. Zie voor meer informatie over specifieke roltoewijzingen en de bijbehorende onderdelen, [het overzicht van Azure, op rollen gebaseerde toegang beheer](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles).

Virtuele Windows-bureaublad gedelegeerde toegang ondersteunt de volgende waarden voor elk element van de toewijzing van rollen:

* Beveiligings-principal
    * Gebruikers
    * Service-principals
* Roldefinitie
    * Ingebouwde rollen
* Scope
    * Tenant-groepen
    * Tenants
    * Host-pools
    * App-groepen

## <a name="built-in-roles"></a>Ingebouwde rollen

Gedelegeerde toegang in Windows virtueel bureaublad heeft diverse ingebouwde roldefinities die u aan gebruikers en service-principals toewijzen kunt.

* De eigenaar van een extern bureaublad-services kunnen alles beheren, inclusief toegang tot bronnen.
* Een extern bureaublad-services-Inzenders kunnen alles maar toegang tot resources beheren.
* De lezer van een extern bureaublad-services kunnen alles weergeven, maar geen wijzigingen kunt aanbrengen.
* Een Operator RDS kunt diagnostische activiteiten bekijken.

## <a name="powershell-cmdlets-for-role-assignments"></a>PowerShell-cmdlets voor roltoewijzingen

U kunt de volgende cmdlets voor het maken, weergeven en bewerken van roltoewijzingen uitvoeren:

* **Get-RdsRoleAssignment** geeft een lijst van roltoewijzingen.
* **Nieuwe RdsRoleAssignment** maakt u een nieuwe roltoewijzing.
* **Set-RdsRoleAssignment** roltoewijzingen bewerkingen.

### <a name="accepted-parameters"></a>Geaccepteerde parameters

U kunt de basic drie cmdlets met de volgende parameters:

* **AadTenantId**: Hiermee geeft u de Azure Active Directory-tenant-ID van waaruit de service-principal een lid is.
* **AppGroupName**: naam van de extern bureaublad-app-groep.
* **Diagnostische gegevens**: geeft het bereik van diagnostische gegevens. (Moet worden gekoppeld met ofwel de **infrastructuur** of **Tenant** parameters.)
* **HostPoolName**: naam van de groep met extern bureaublad-host.
* **Infrastructuur**: geeft het bereik van de infrastructuur.
* **RoleDefinitionName**: naam van de rol extern bureaublad-Services toegang op rollen gebaseerd beheer toegewezen aan de gebruiker, groep of app. (Bijvoorbeeld extern bureaublad-Services eigenaar, lezer van extern bureaublad-Services, enzovoort.)
* **ServerPrincipleName**: naam van de Azure Active Directory-toepassing.
* **SignInName**: e-mailadres van de gebruiker of user principal name.
* **Tenantnaam**: naam van de extern bureaublad-tenant.

## <a name="next-steps"></a>Volgende stappen

Zie voor een volledig overzicht van PowerShell-cmdlets met elke rol gebruiken kunt, de [PowerShell-referentie](/powershell/windows-virtual-desktop/overview).

Zie voor richtlijnen voor het instellen van een omgeving met virtuele Windows-bureaublad [Windows Virtual Desktop Preview-omgeving](environment-setup.md).
