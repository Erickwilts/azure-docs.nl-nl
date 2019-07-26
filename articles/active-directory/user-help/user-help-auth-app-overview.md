---
title: Overzicht van de Microsoft Authenticator-app - Azure Active Directory | Microsoft Docs
description: Meer informatie over de Microsoft Authenticator-app, met inbegrip van wat deze inhoudt, hoe de app werkt en welke informatie is opgenomen in deze sectie van de inhoud.
services: active-directory
author: eross-msft
manager: daveba
ms.reviewer: sahenry
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: overview
ms.date: 01/24/2019
ms.author: lizross
ms.collection: M365-identity-device-management
ms.openlocfilehash: d3766436851cb2f31fa7bb070402191e03ad9ad1
ms.sourcegitcommit: 04ec7b5fa7a92a4eb72fca6c6cb617be35d30d0c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/22/2019
ms.locfileid: "68382337"
---
# <a name="what-is-the-microsoft-authenticator-app"></a>Wat is de Microsoft Authenticator-app?

>[!Important]
>Deze inhoud is bedoeld voor gebruikers. Als u een beheerder bent, kunt u meer informatie vinden over hoe u uw Azure Active Directory-omgeving (Azure AD) instelt en beheert in de [documentatie voor Azure Active Directory](https://docs.microsoft.com/azure/active-directory).

Met de Microsoft Authenticator-app kunt u zich aanmelden bij uw accounts als u gebruikmaakt van verificatie in twee stappen. Verificatie in twee stappen helpt u veiliger toegang te krijgen tot uw accounts, vooral als u gevoelige informatie bekijkt. Omdat wachtwoorden kunnen worden vergeten, gestolen of gewijzigd, is verificatie in twee stappen een extra beveiligingsstap waarmee u uw account beschermt door het voor anderen lastiger te maken om in te breken.

U kunt de Microsoft Authenticator-app op meerdere manieren gebruiken, bijvoorbeeld:

- Reageer op een vraag om verificatie nadat u zich hebt aangemeld met uw gebruikers naam en wacht woord.

- Meld u aan zonder een wacht woord in te voeren, met uw gebruikers naam, de verificator-app en uw mobiele apparaat met de vinger afdruk, het gezicht of de pincode.

- Als code generator voor andere accounts die ondersteuning bieden voor verificator-apps.

> [!Important]
> De Microsoft Authenticator-app werkt met een account dat gebruikmaakt van twee ledige verificatie en ondersteunt de op tijd gebaseerde mobiele TOTP-standaarden (one-time password).
>
> Mogelijk moet u voor uw organisatie gebruikmaken van een verificator-app om u aan te melden en de gegevens en documenten van uw organisatie te openen. Hoewel uw gebruikersnaam mogelijk in de app wordt weergegeven, wordt het account pas echt ingesteld om te fungeren als een verificatiemethode wanneer u het registratieproces voltooit. Zie [Uw werk- of schoolaccount toevoegen](user-help-auth-app-add-work-school-account.md) voor meer informatie.
> 
> Als u problemen ondervindt met aanmelden bij uw account, gaat u naar [Wanneer u zich niet kunt aanmelden bij uw Microsoft-account](https://support.microsoft.com/help/12429) voor hulp. Bekijk meer informatie over wat u kunt doen wanneer het bericht ['Dat Microsoft-account bestaat niet'](https://support.microsoft.com/help/13811) wordt weergegeven wanneer u zich aanmeldt bij uw Microsoft-account.

## <a name="terminology"></a>Terminologie

| Begrip|Description|
| ----|-----------|
| Verificatie in twee stappen | Een verificatieproces waarvoor u specifiek twee stukjes verificatie-informatie, zoals een wachtwoord en een pincode, moet gebruiken. De Microsoft Authenticator-app biedt ondersteuning voor zowel de standaard verificatie in twee stappen als aanmelding zonder wachtwoord. |
| Multi-factor authentication (MFA) | Alle verificatie in twee stappen bestaat uit meervoudige verificatie, waarvoor u *ten minste* twee stukjes verificatie-informatie moet gebruiken, op basis van de vereisten van uw organisatie. |
| Microsoft-account (ook MSA genoemd) | U maakt uw eigen persoonlijke accounts om toegang te krijgen tot uw consumentgerichte producten en cloudservices van Microsoft, zoals Outlook, OneDrive, Xbox LIVE en Office 365. Uw Microsoft-account wordt gemaakt en opgeslagen in het accountsysteem voor consumentidentiteiten van Microsoft, dat wordt beheerd door Microsoft. |
| Werk- of schoolaccount | Uw organisatie maakt uw werk- of schoolaccount (zoals alain@contoso.com) waarmee u toegang tot interne en mogelijk beperkte resources, zoals Microsoft Azure, Windows Intune en Office 365, kunt krijgen. |
| Verificatiecode | De code van zes cijfers die wordt weergegeven in de Authenticator-app, onder elk toegevoegd account. De verificatiecode verandert elke 30 seconden om te voorkomen dat iemand een code meerdere keren kan gebruiken. Dit wordt ook wel een eenmalige wachtwoordcode of OTP (One-Time Passcode) genoemd. |

## <a name="how-two-factor-verification-works-with-the-app"></a>Hoe verificatie in twee stappen werkt in combinatie met de app

Verificatie in twee stappen werkt met de Microsoft Authenticator-app op de volgende manieren:

- **Melding.** Typ uw gebruikersnaam en wachtwoord op het apparaat waar u zich aanmeldt voor uw werk- of schoolaccount of uw persoonlijke Microsoft-account. De Microsoft Authenticator-app stuurt vervolgens een melding met de vraag of u de **aanmelding goedkeurt**. Kies **Goedkeuren** als u de aanmeldingspoging herkent. Kies anders **Weigeren**. Als u kiest voor **Weigeren**, kunt u de aanvraag ook als frauduleus markeren.

- **Verificatiecode.** Typ uw gebruikersnaam en wachtwoord op het apparaat waar u zich aanmeldt voor uw werk- of schoolaccount of uw persoonlijke Microsoft-account. Kopieer vervolgens de bijbehorende verificatiecode vanuit het scherm **Accounts** van de Microsoft Authenticator-app. De verificatiecode wordt ook wel verificatie met een eenmalige wachtwoordcode of OTP (One-Time Passcode) genoemd.

- **Aanmelden zonder wachtwoord.** Typ uw gebruikersnaam in het apparaat waar u zich aanmeldt voor uw werk- of schoolaccount of uw persoonlijke Microsoft-account en gebruik vervolgens uw mobiele apparaat om te bevestigen dat u het bent door gebruik te maken van uw vingerafdruk, gezicht of pincode. Voor deze methode hoeft u niet uw wachtwoord op te geven.

### <a name="whether-to-use-your-devices-biometric-capabilities"></a>Of de biometrische mogelijkheden van uw apparaat moeten worden gebruikt

Als u een pincode gebruikt om het verificatieproces uit te voeren, kunt u de Microsoft Authenticator-app instellen op gebruik van de (biometrische) mogelijkheden voor vingerafdruk- of gezichtsherkenning van uw apparaat. U kunt dit instellen wanneer u de verificator-app voor het eerst gebruikt om uw account te verifiëren door in plaats van uw pincode de optie voor gebruik van de biometrische mogelijkheden van uw apparaat als identificatie te selecteren.

## <a name="who-decides-if-you-use-this-feature"></a>Wie besluit of u deze functie gebruikt?

Afhankelijk van uw accounttype kan uw organisatie besluiten dat u gebruik moet maken van verificatie in twee stappen, maar het is ook mogelijk dat u dat zelf kunt bepalen.

- **Werk- of schoolaccount.** Als u een werk- of schoolaccount gebruikt (zoals alain@contoso.com), is het aan uw organisatie of u verificatie in twee stappen moet gebruiken, samen met de specifieke verificatiemethoden. Zie [Werk- of schoolaccounts toevoegen](user-help-auth-app-add-work-school-account.md) voor meer informatie over het toevoegen van uw werk- of schoolaccount aan de Microsoft Authenticator-app.

- **Persoonlijk Microsoft-account.** U kunt ervoor kiezen verificatie in twee stappen in te stellen voor uw persoonlijke Microsoft-accounts (zoals alain@outlook.com). Zie [Uw persoonlijke accounts toevoegen](user-help-auth-app-add-personal-ms-account.md) voor meer informatie over het toevoegen van uw persoonlijke Microsoft-accounts.

- **Niet-Microsoft-account.** U kunt ervoor kiezen verificatie in twee stappen in te stellen voor uw niet-Microsoft-accounts (zoals alain@gmail.com). Uw niet-Microsoft-accounts hanteren wellicht niet de term verificatie in twee stappen, maar u zou de functie moeten kunnen vinden in de instellingen voor **beveiliging** of **aanmelding**. De Microsoft Authenticator-app werkt met alle accounts die ondersteuning bieden voor de TOTP-normen. Zie [Uw niet-Microsoft-accounts toevoegen](user-help-auth-app-add-non-ms-account.md) voor meer informatie over het toevoegen van uw niet-Microsoft-accounts.

## <a name="in-this-section"></a>In deze sectie doet u het volgende

| Artikel | Description |
| ------ | ------------ |
| [De app downloaden en installeren](user-help-auth-app-download-install.md) | Beschrijft waar en hoe u de Microsoft Authenticator-app kunt downloaden en installeren voor apparaten met Android en iOS. |
| [Uw werk- of schoolaccounts toevoegen](user-help-auth-app-add-work-school-account.md) | Beschrijft hoe u uw verschillende werk- of schoolaccounts toevoegt aan de Microsoft Authenticator-app. |
| [Uw persoonlijke accounts toevoegen](user-help-auth-app-add-personal-ms-account.md) | Beschrijft hoe u uw persoonlijke Microsoft-accounts toevoegt aan de Microsoft Authenticator-app. |
| [Uw niet-Microsoft-account toevoegen](user-help-auth-app-add-non-ms-account.md) | Beschrijft hoe u uw niet-Microsoft-accounts toevoegt aan de Microsoft Authenticator-app. |
| [Uw accounts handmatig toevoegen](user-help-auth-app-add-account-manual.md) | Beschrijft hoe u handmatig uw accounts toevoegt aan de Microsoft Authenticator-app als u de verstrekte QR-code niet kunt scannen. |
| [Aanmelden met de app](user-help-auth-app-sign-in.md) | Beschrijft hoe u zich aanmeldt bij uw verschillende accounts met behulp van de Microsoft Authenticator-app.|
| [Back-up en herstel van accountreferenties](user-help-auth-app-backup-recovery.md) | Bevat informatie over back-up en herstel van uw accountreferenties met behulp van de Microsoft Authenticator-app. |
| [Veelgestelde vragen over Microsoft Authenticator-app](user-help-auth-app-faq.md) | Geeft antwoord op veelgestelde vragen over de app. |
