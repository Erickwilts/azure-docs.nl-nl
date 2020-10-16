---
title: Versie van pagina-indeling
titleSuffix: Azure AD B2C
description: Versie geschiedenis van de pagina-indeling voor UI-aanpassing in aangepast beleid.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 08/24/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 868d99a82009dc8545fc24ad1cfa1da3959da131
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "88852074"
---
# <a name="page-layout-versions"></a>Versie van pagina-indeling

Pagina-indelings pakketten worden regel matig bijgewerkt met oplossingen en verbeteringen in hun pagina-elementen. In het volgende wijzigingslog bestand worden de wijzigingen aangegeven die in elke versie zijn geïntroduceerd.

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

## <a name="self-asserted-page-selfasserted"></a>Zelfbevestigende pagina (selfasserted)

**2.1.0**

- Lokalisatie-en toegankelijkheids oplossingen.

**2.0.0**

- Er is ondersteuning toegevoegd voor [besturings elementen voor weer gave](display-controls.md) in aangepast beleid.

**1.2.0**

- De velden gebruikers naam/e-mail adres en wacht woord gebruiken nu het `form` HTML-element zodat Edge en Internet Explorer (IE) deze gegevens op de juiste manier kunnen opslaan.
- Er is een Configureer bare validatie vertraging voor gebruikers invoer toegevoegd voor verbeterde gebruikers ervaring.
- Toegankelijkheids oplossingen
- U kunt nu het `data-preload="true"` kenmerk toevoegen aan [uw HTML-tags](custom-policy-ui-customization.md#guidelines-for-using-custom-page-content) om de laad volgorde voor CSS en Java script te bepalen.
  - Laad gekoppelde CSS-bestanden op hetzelfde moment als uw HTML-sjabloon zodat er geen Flik kering is tussen het laden van de bestanden.
  - De volg orde bepalen waarin uw `script` Tags worden opgehaald en uitgevoerd voordat de pagina wordt geladen.
- Het veld e-mail is nu `type=email` en mobiele toetsen borden bieden de juiste suggesties
- Ondersteuning voor Chrome-vertaling

**1.1.0**

- Waarschuwing bij annuleren verwijderd
- CSS-klasse voor fout elementen
- Fout logica weer geven/verbergen is verbeterd
- Standaard-CSS verwijderd

**1.0.0**

- Eerste release

## <a name="unified-sign-in-sign-up-page-with-password-reset-link-unifiedssp"></a>Aanmeldings pagina voor Unified Sign-in met de koppeling voor het opnieuw instellen van een wacht woord (unifiedssp)

**2.1.0**

- Er is ondersteuning toegevoegd voor meerdere registratie koppelingen.
- Er is ondersteuning toegevoegd voor gebruikers invoer validatie volgens de predicaat regels die in het beleid zijn gedefinieerd.

**1.2.0**

- De velden gebruikers naam/e-mail adres en wacht woord gebruiken nu het `form` HTML-element zodat Edge en Internet Explorer (IE) deze gegevens op de juiste manier kunnen opslaan.
- Toegankelijkheids oplossingen
- U kunt nu het `data-preload="true"` kenmerk toevoegen aan [uw HTML-tags](custom-policy-ui-customization.md#guidelines-for-using-custom-page-content) om de laad volgorde voor CSS en Java script te bepalen.
  - Laad gekoppelde CSS-bestanden op hetzelfde moment als uw HTML-sjabloon zodat er geen Flik kering is tussen het laden van de bestanden.
  - De volg orde bepalen waarin uw `script` Tags worden opgehaald en uitgevoerd voordat de pagina wordt geladen.
- Het veld e-mail is nu `type=email` en mobiele toetsen borden bieden de juiste suggesties
- Ondersteuning voor Chrome-vertaling

**1.1.0**

- Besturings element voor behoud van aangemeld (KMSI) toegevoegd

**1.0.0**

- Eerste release

## <a name="mfa-page-multifactor"></a>MFA-pagina (multi-factor)

**1.2.1**

- Toegankelijkheids oplossingen op standaard sjablonen

**1.2.0**

- Toegankelijkheids oplossingen
- U kunt nu het `data-preload="true"` kenmerk toevoegen aan [uw HTML-tags](custom-policy-ui-customization.md#guidelines-for-using-custom-page-content) om de laad volgorde voor CSS en Java script te bepalen.
  - Laad gekoppelde CSS-bestanden op hetzelfde moment als uw HTML-sjabloon zodat er geen Flik kering is tussen het laden van de bestanden.
  - De volg orde bepalen waarin uw `script` Tags worden opgehaald en uitgevoerd voordat de pagina wordt geladen.
- Het veld e-mail is nu `type=email` en mobiele toetsen borden bieden de juiste suggesties
- Ondersteuning voor Chrome-vertaling

**1.1.0**

- De knop voor het bevestigen van de code is verwijderd
- Het invoer veld voor de code heeft nu alleen invoer van Maxi maal zes (6) tekens
- Op de pagina wordt automatisch geprobeerd om te controleren of de code die u hebt ingevoerd wanneer een 6-cijferige code wordt ingevoerd, zonder dat u hoeft te klikken op een knop
- Als de code onjuist is, wordt het invoer veld automatisch gewist
- Na drie (3) pogingen met een onjuiste code stuurt B2C een fout terug naar de Relying Party
- Toegankelijkheids oplossingen
- Standaard-CSS verwijderd

**1.0.0**

- Eerste release

## <a name="exception-page-globalexception"></a>Uitzonderings pagina (globalexception)

**1.2.0**

- Toegankelijkheids oplossingen
- U kunt nu het `data-preload="true"` kenmerk toevoegen aan [uw HTML-tags](custom-policy-ui-customization.md#guidelines-for-using-custom-page-content) om de laad volgorde voor CSS en Java script te bepalen.
  - Laad gekoppelde CSS-bestanden op hetzelfde moment als uw HTML-sjabloon zodat er geen Flik kering is tussen het laden van de bestanden.
  - De volg orde bepalen waarin uw `script` Tags worden opgehaald en uitgevoerd voordat de pagina wordt geladen.
- Het veld e-mail is nu `type=email` en mobiele toetsen borden bieden de juiste suggesties
- Ondersteuning voor Chrome-vertaling

**1.1.0**

- Toegankelijkheids oplossing
- Het standaard bericht is verwijderd wanneer er geen contact is van het beleid
- Standaard-CSS verwijderd

**1.0.0**

- Eerste release

## <a name="other-pages-providerselection-claimsconsent-unifiedssd"></a>Andere pagina's (ProviderSelection, ClaimsConsent, UnifiedSSD)

**1.2.0**

- Toegankelijkheids oplossingen
- U kunt nu het `data-preload="true"` kenmerk toevoegen aan [uw HTML-tags](custom-policy-ui-customization.md#guidelines-for-using-custom-page-content) om de laad volgorde voor CSS en Java script te bepalen.
  - Laad gekoppelde CSS-bestanden op hetzelfde moment als uw HTML-sjabloon zodat er geen Flik kering is tussen het laden van de bestanden.
  - De volg orde bepalen waarin uw `script` Tags worden opgehaald en uitgevoerd voordat de pagina wordt geladen.
- Het veld e-mail is nu `type=email` en mobiele toetsen borden bieden de juiste suggesties
- Ondersteuning voor Chrome-vertaling

**1.0.0**

- Eerste release

## <a name="next-steps"></a>Volgende stappen

Zie [de gebruikers interface van uw toepassing aanpassen met behulp van een aangepast beleid](custom-policy-ui-customization.md)voor meer informatie over het aanpassen van de gebruikers interface van uw toepassingen in een aangepast beleid.
