---
title: Verlengen of vernieuwen van Azure-resource-roltoewijzingen in PIM - Azure Active Directory | Microsoft Docs
description: Informatie over het verlengen of vernieuwen van Azure-resource-roltoewijzingen in Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: pim
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: a064fc67bf94ba6aa443e429fe83179d84cada84
ms.sourcegitcommit: 6ea7f0a6e9add35547c77eef26f34d2504796565
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/14/2019
ms.locfileid: "65602647"
---
# <a name="extend-or-renew-azure-resource-role-assignments-in-pim"></a>Verlengen of vernieuwen van Azure-resource-roltoewijzingen in PIM

Azure Active Directory (Azure AD) Privileged Identity Management (PIM) introduceert nieuwe besturingselementen voor het beheren van de levenscyclus van toegang en de toewijzing voor Azure-resources. Beheerders kunnen lidmaatschap met behulp van begin- en datum / tijd-eigenschappen toewijzen. Wanneer het einde van de toewijzing nadert, verzonden PIM e-mailmeldingen naar de betrokken gebruikers of groepen. Het verzendt ook e-mailmeldingen voor beheerders van de resource om ervoor te zorgen dat de juiste toegang wordt bijgehouden. Toewijzingen kunnen worden vernieuwd en blijven zichtbaar is in een verlopen gedurende maximaal 30 dagen, zelfs als de toegang niet is uitgebreid.

## <a name="who-can-extend-and-renew"></a>Wie kunt uitbreiden en vernieuwen?

Alleen beheerders van de resource kunnen verlengen of vernieuwen van roltoewijzingen. Het lid van de betrokken kan aanvragen om uit te breiden van functies die zijn verlopen en de aanvraag voor het vernieuwen van de rollen die al zijn verlopen.

## <a name="when-are-notifications-sent"></a>Wanneer worden meldingen verzonden?

PIM verzendt e-mailmeldingen voor beheerders en betrokken leden van de rollen die zijn verlopen binnen 14 dagen en één dag vóór de vervaldatum. Een extra e-mailbericht stuurt wanneer een toewijzing officieel is verlopen. 

Beheerders ontvangen meldingen wanneer een lid van de rol van een verlopen of verlopen aanvragen om te verlengen of vernieuwen. Wanneer een beheerder van een specifieke oplossing voor de aanvraag, alle andere beheerders zijn op de hoogte gesteld van de beslissing resolutie (goedgekeurd of geweigerd). Vervolgens wordt het lid van de aanvragende geïnformeerd over de beslissing. 

## <a name="extend-role-assignments"></a>Uitbreiden van roltoewijzingen

De volgende stappen beschrijven het proces voor aanvragen, het oplossen van of beheren van een extensie of een verlenging van een roltoewijzing. 

### <a name="member-extend"></a>Lid uitbreiden

Leden van een roltoewijzing kunnen uitbreiden verlopende roltoewijzingen rechtstreeks vanuit de **in aanmerking komende** of **Active** tabblad op de **mijn rollen** pagina van een resource en van het hoogste niveau **Mijn rollen** pagina van de PIM-portal. Leden kunnen aanvragen om uit te breiden in aanmerking komende en actieve (toegewezen) functies die in de volgende 14 dagen verlopen.

![Rollen verlengen](media/pim-resource-roles-renew-extend/aadpim-rbac-extend-ui.png)

Wanneer de toewijzing einddatum en-tijd is binnen 14 dagen, op de knop **uitbreiden** verandert in een actieve koppeling in de gebruikersinterface. In het volgende voorbeeld wordt ervan uitgegaan dat de huidige datum valt 27 maart.

![Knop uitbreiden](media/pim-resource-roles-renew-extend/aadpim-rbac-extend-within-14.png)

Selecteer om aan te vragen een uitbreiding van deze roltoewijzing, **uitbreiden** om het aanvraagformulier te openen.

![Open het aanvraagformulier](media/pim-resource-roles-renew-extend/aadpim-rbac-extend-role-assignment-request.png)

Als u informatie over de oorspronkelijke toewijzing, vouw **toewijzingsdetails**. Geef een reden op voor de extensie-aanvraag, en selecteer vervolgens **uitbreiden**.

>[!Note]
>We raden aan om de details van waarom de extensie is nodig, en hoe lang de extensie moet worden verleend (als u deze informatie hebt).

![Roltoewijzing uitbreiden](media/pim-resource-roles-renew-extend/aadpim-rbac-extend-form-complete.png)

Binnen enkele ogenblikken ontvangt resource beheerders een e-mailbericht melding met het verzoek dat ze de extensie-aanvraag bekijken. Als een aanvraag om uit te breiden al heeft ingediend, wordt een pop-upmelding weergegeven aan de bovenkant van de Azure portal waarin wordt uitgelegd van de fout.

![Melding waarin wordt uitgelegd fout](media/pim-resource-roles-renew-extend/aadpim-rbac-extend-failed-existing-request.png)

Ga naar de **aanvragen in behandeling** tabblad in het linkerdeelvenster om de status van uw aanvraag weer te geven of om deze te annuleren.

![Aanvragen in behandeling](media/pim-resource-roles-renew-extend/aadpim-rbac-extend-cancel-request.png)

### <a name="admin-approve"></a>Beheerder goedkeuren

Wanneer een lid van een aanvraag om uit te breiden een roltoewijzing verzendt, wordt in het resource-beheerders een e-mailbericht de details van de oorspronkelijke toewijzing en de reden voor de aanvraag bevat ontvangen. De melding bevat een directe koppeling naar de aanvraag voor de beheerder goedkeuren of weigeren. 

Naast het gebruik van de koppeling van e-mailbericht te volgen, beheerders kunnen goedkeuren of weigeren van aanvragen door te gaan naar de portal en vervolgens de PIM-beheer **aanvragen goedkeuren** in het linkerdeelvenster.

![Schermopname van foutbericht](media/pim-resource-roles-renew-extend/aadpim-rbac-extend-admin-approve-grid.png)

Wanneer een beheerder selecteert **goedkeuren** of **weigeren**, de details van de aanvraag worden weergegeven, samen met een veld reden voor de controlelogboeken op te geven.

![De aanvraag voor roltoewijzing goedkeuren](media/pim-resource-roles-renew-extend/aadpim-rbac-extend-admin-approve-blade.png)

Wanneer een aanvraag om uit te breiden roltoewijzing wordt goedgekeurd, worden de resource-beheerders kunnen kiezen een nieuwe begindatum, einddatum en toewijzingstype. Wijzigen van dit toewijzingstype kan het nodig zijn als de beheerder wil de toegang beperkt voor het voltooien van een specifieke taak (bijvoorbeeld één dag). In dit voorbeeld kan de beheerder de toewijzing van wijzigen **in aanmerking komende** naar **Active**. Dit betekent dat ze kunnen toegang bieden aan de aanvrager zonder dat ze om te activeren.

### <a name="admin-extend"></a>Beheerdersuitbreiding

Als rollid vergeet of kan niet een verlenging van het lidmaatschap van rol aanvragen, kan een beheerder een toewijzing namens het lid kunt uitbreiden. Administratieve extensies van het lidmaatschap van een rol geen goedkeuring vereist, maar de meldingen worden verzonden naar alle andere beheerders nadat de functie is uitgebreid.

Als u wilt uitbreiden een lidmaatschap van de rol, blader naar de weergave van resources rol of lid in PIM. Het lid dat is vereist dat er vinden. Selecteer vervolgens **uitbreiden** in de actiekolom.

![Uitbreiden van een lidmaatschap van de rol](media/pim-resource-roles-renew-extend/aadpim-rbac-extend-admin-extend.png)

## <a name="renew-role-assignments"></a>Roltoewijzingen vernieuwen

Terwijl qua ontwerp vergelijkbaar met het proces voor het aanvragen van een extensie, wordt het proces om een verlopen roltoewijzing te verlengen verschilt. Met behulp van de volgende stappen uit, kunnen leden en beheerders vernieuwen toegang tot verlopen rollen indien nodig.

### <a name="member-renew"></a>Lid vernieuwen

Leden die niet langer toegang bronnen tot hebben toegang tot maximaal 30 dagen van de geschiedenis van werkitemtoewijzingen verlopen. U doet dit door ze te bladeren **mijn rollen** in het linkerdeelvenster en selecteer vervolgens de **verlopen rollen** tabblad in de sectie Azure-resource-rollen.

![Verlopen rollen tabblad](media/pim-resource-roles-renew-extend/aadpim-rbac-renew-from-myroles.png)

De lijst met rollen weergegeven standaard ingesteld op **in aanmerking komende rollen**. Gebruik de vervolgkeuzelijst om uit te schakelen tussen van in aanmerking komende en actieve rollen toegewezen krijgen.

Voor het aanvragen van verlenging voor het gebruik van de roltoewijzingen in de lijst, selecteer de **vernieuwen** actie. Geef een reden op voor de aanvraag. Het is handig voor een duur naast elke aanvullende context waarmee de beheerder van de resource wilt goedkeuren of weigeren.

![De roltoewijzing te verlengen](media/pim-resource-roles-renew-extend/aadpim-rbac-renew-request-form.png)

Nadat de aanvraag heeft ingediend, resource beheerders zijn op de hoogte gesteld van een aanvraag in behandeling om een roltoewijzing te verlengen.

### <a name="admin-approves"></a>Beheerder goedkeurt

Resource-beheerders hebben toegang tot de vernieuwingsaanvraag via de koppeling in het e-mailbericht of door PIM vanuit de Azure-portal te openen en te selecteren **aanvragen goedkeuren** in het linkerdeelvenster.

![Aanvragen goedkeuren](media/pim-resource-roles-renew-extend/aadpim-rbac-extend-admin-approve-grid.png)

Wanneer een beheerder selecteert **goedkeuren** of **weigeren**, de details van de aanvraag worden weergegeven, samen met een veld reden voor de controlelogboeken op te geven.

![Roltoewijzing goedkeuren](media/pim-resource-roles-renew-extend/aadpim-rbac-extend-admin-approve-blade.png)

Bij het goedkeuren van een aanvraag om roltoewijzing te verlengen, moeten resource beheerders een nieuwe begindatum, einddatum en toewijzingstype invoeren. 

### <a name="admin-renew"></a>Beheerdersverlenging

Resource-beheerders kunnen vernieuwen verlopen roltoewijzingen worden van de **leden** tabblad in het navigatiemenu links van een resource. Ze kunnen ook vernieuwen verlopen roltoewijzingen vanuit de **verlopen** tabblad van de rollen van de resourcerol van een.

Om weer te geven van een lijst van alle roltoewijzingen, is verlopen op de **leden** scherm, selecteer **verlopen rollen**.

![Verlopen rollen](media/pim-resource-roles-renew-extend/aadpim-rbac-renew-from-member-blade.png)

## <a name="next-steps"></a>Volgende stappen

- [Goedkeuren of weigeren van aanvragen voor Azure-resource-rollen in PIM](pim-resource-roles-approval-workflow.md)
- [Azure-resource rolinstellingen in PIM configureren](pim-resource-roles-configure-role-settings.md)
