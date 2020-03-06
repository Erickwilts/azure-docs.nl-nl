---
title: Problemen met B2B-samen werking oplossen-Azure Active Directory | Microsoft Docs
description: Oplossingen voor veelvoorkomende problemen met Azure Active Directory B2B-samen werking
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: troubleshooting
ms.date: 11/12/2019
tags: active-directory
ms.author: mimart
author: v-miegge
manager: dcscontentpm
ms.reviewer: mal
ms.custom:
- it-pro
- seo-update-azuread-jan"
ms.collection: M365-identity-device-management
ms.openlocfilehash: c7c0a4567da11b10b9a0571656103ef2f17c7da4
ms.sourcegitcommit: 05b36f7e0e4ba1a821bacce53a1e3df7e510c53a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78399047"
---
# <a name="troubleshooting-azure-active-directory-b2b-collaboration"></a>Problemen oplossen Azure Active Directory B2B-samen werking

Hier volgen enkele oplossingen voor veelvoorkomende problemen met Azure Active Directory (Azure AD) B2B-samen werking.

## <a name="ive-added-an-external-user-but-do-not-see-them-in-my-global-address-book-or-in-the-people-picker"></a>Ik heb een externe gebruiker toegevoegd, maar deze wordt niet weer geven in het algemene adres boek of in de kiezer personen

In gevallen waarin externe gebruikers niet zijn ingevuld in de lijst, kan het enkele minuten duren voordat het object wordt gerepliceerd.

## <a name="a-b2b-guest-user-is-not-showing-up-in-sharepoint-onlineonedrive-people-picker"></a>Een B2B-gast gebruiker wordt niet weer gegeven in share point online/personen selecteren in OneDrive

De mogelijkheid om te zoeken naar bestaande gast gebruikers in de share point online (SPO) persoons kiezer is standaard uitgeschakeld om te voldoen aan verouderd gedrag.

U kunt deze functie inschakelen met behulp van de instelling ' ShowPeoplePickerSuggestionsForGuestUsers ' op het niveau van de Tenant en site verzameling. U kunt de functie instellen met behulp van de cmdlets set-SPOTenant en set-SPOSite, waarmee leden alle bestaande gast gebruikers in de Directory kunnen doorzoeken. Wijzigingen in het Tenant bereik hebben geen invloed op reeds ingerichte SPO-sites.

## <a name="invitations-have-been-disabled-for-directory"></a>Uitnodigingen zijn uitgeschakeld voor de map

Als u een melding krijgt dat u geen machtigingen hebt om gebruikers uit te nodigen, controleert u of uw gebruikers account gemachtigd is om externe gebruikers uit te nodigen onder Azure Active Directory > gebruikers instellingen > externe gebruikers > externe samenwerkings instellingen beheren:

![Scherm opname met de instellingen van externe gebruikers](media/troubleshoot/external-user-settings.png)

Als u deze instellingen onlangs hebt gewijzigd of als u de rol van de gast-uitnodiging hebt toegewezen aan een gebruiker, is er mogelijk een vertraging van 15-60 minuten om de wijzigingen van kracht te laten worden.

## <a name="the-user-that-i-invited-is-receiving-an-error-during-redemption"></a>De gebruiker die ik heb uitgenodigd, ontvangt een fout tijdens de inwisseling

Veelvoorkomende fouten zijn onder andere:

### <a name="invitees-admin-has-disallowed-emailverified-users-from-being-created-in-their-tenant"></a>De beheerder van de uitnodiging heeft niet toegestaan dat EmailVerified-gebruikers worden gemaakt in hun Tenant

Wanneer u gebruikers uitnodigt waarvan de organisatie gebruikmaakt van Azure Active Directory, maar waarbij het account van de specifieke gebruiker niet bestaat (bijvoorbeeld omdat de gebruiker niet bestaat in azure AD contoso.com). De beheerder van contoso.com kan een beleid hebben om te voor komen dat gebruikers worden gemaakt. De gebruiker moet contact met hun beheerder controleren om te bepalen of externe gebruikers zijn toegestaan. De beheerder van de externe gebruiker moet mogelijk geverifieerde gebruikers in hun domein toestaan (zie dit [artikel](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0) over het toestaan van geverifieerde e-mail gebruikers).

![Fout met de mede deling dat de Tenant gebruikers geen geverifieerde e-mail berichten toestaan](media/troubleshoot/allow-email-verified-users.png)

### <a name="external-user-does-not-exist-already-in-a-federated-domain"></a>De externe gebruiker bestaat al in een federatief domein

Als u Federatie verificatie gebruikt en de gebruiker nog niet bestaat in Azure Active Directory, kan de gebruiker niet worden uitgenodigd.

Om dit probleem op te lossen, moet de beheerder van de externe gebruiker het account van de gebruiker synchroniseren met Azure Active Directory.

## <a name="how-does--which-is-not-normally-a-valid-character-sync-with-azure-ad"></a>Hoe werkt '\#', maar dit is niet normaal gesp roken een geldig teken, synchroniseer ik met Azure AD?

"\#" is een gereserveerd teken in Upn's voor Azure AD B2B-samen werking of externe gebruikers, omdat de uitgenodigde account user@contoso.com user_contoso. com # EXT #@fabrikam.onmicrosoft.comwordt. Daarom kunnen \# in Upn's die afkomstig zijn van on-premises, zich niet aanmelden bij de Azure Portal. 

## <a name="i-receive-an-error-when-adding-external-users-to-a-synchronized-group"></a>Er wordt een fout bericht weer gegeven bij het toevoegen van externe gebruikers aan een gesynchroniseerde groep

Externe gebruikers kunnen alleen worden toegevoegd aan groepen ' toegewezen ' of ' Beveiliging ' en niet aan groepen die on-premises zijn gemastereerd.

## <a name="my-external-user-did-not-receive-an-email-to-redeem"></a>Mijn externe gebruiker heeft geen e-mail bericht ontvangen om in te wisselen

De uitnodiging moet worden gecontroleerd met hun ISP-of spam filter om ervoor te zorgen dat het volgende adres is toegestaan: Invites@microsoft.com

## <a name="i-notice-that-the-custom-message-does-not-get-included-with-invitation-messages-at-times"></a>Ik zie dat het aangepaste bericht niet op momenten wordt opgenomen in uitnodigings berichten

Om te voldoen aan de privacy-wetten, bevatten onze Api's geen aangepaste berichten in de uitnodiging voor het e-mail bericht wanneer:

- De uitnodiging heeft geen e-mail adres in de uitnodigende Tenant
- Wanneer een appservice-principal de uitnodiging verzendt

Als dit scenario belang rijk voor u is, kunt u onze API-uitnodiging e-mail onderdrukken en verzenden via het e-mail mechanisme van uw keuze. Raadpleeg de juridische afdeling van uw organisatie om ervoor te zorgen dat alle e-mail berichten die u op deze manier verzendt, ook voldoen aan de wetten van de privacy.

## <a name="you-receive-an-aadsts65005-error-when-you-try-to-log-in-to-an-azure-resource"></a>Er wordt een fout bericht ' AADSTS65005 ' weer gegeven wanneer u zich probeert aan te melden bij een Azure-resource

Een gebruiker met een gast account kan zich niet aanmelden en ontvangt het volgende fout bericht:

    AADSTS65005: Using application 'AppName' is currently not supported for your organization contoso.com because it is in an unmanaged state. An administrator needs to claim ownership of the company by DNS validation of contoso.com before the application AppName can be provisioned.

De gebruiker heeft een Azure-gebruikers account en is een virale Tenant die is verlaten of onbeheerd. Daarnaast zijn er geen globale of bedrijfs beheerders in de Tenant.

Om dit probleem op te lossen, moet u de afgebroken Tenant overnemen. Raadpleeg een niet- [beheerde Directory als beheerder in azure Active Directory](https://docs.microsoft.com/azure/active-directory/users-groups-roles/domains-admin-takeover). U moet ook toegang krijgen tot de Internet gerichte DNS voor het betreffende domein achtervoegsel om direct bewijs te geven dat u de controle hebt over de naam ruimte. Nadat de Tenant naar een beheerde status is geretourneerd, kunt u met de klant bespreken of de gebruikers-en geverifieerde domein naam de beste optie is voor hun organisatie.

## <a name="a-guest-user-with-a-just-in-time-or-viral-tenant-is-unable-to-reset-their-password"></a>Een gast gebruiker met een just-in-time-of virus-Tenant kan het wacht woord niet opnieuw instellen

Als de identiteits Tenant een just-in-time (JIT) of virale Tenant is (wat betekent dat het een afzonderlijke, niet-beheerde Azure-Tenant is), kan alleen de gast gebruiker het wacht woord opnieuw instellen. Soms neemt een organisatie het [beheer van virale tenants over](https://docs.microsoft.com/azure/active-directory/users-groups-roles/domains-admin-takeover) die worden gemaakt wanneer werk nemers hun werk-e-mail adressen gebruiken om zich aan te melden voor services. Nadat de organisatie een virale Tenant heeft overgenomen, kan alleen een beheerder in die organisatie het wacht woord van de gebruiker opnieuw instellen of SSPR inschakelen. Als dat nodig is, kunt u, als de uitnodigende organisatie, het gast gebruikers account uit uw Directory verwijderen en een uitnodiging opnieuw verzenden.

## <a name="a-guest-user-is-unable-to-use-the-azuread-powershell-v1-module"></a>Een gast gebruiker kan de AzureAD Power shell v1-module niet gebruiken

Met ingang van 18 november 2019 kunnen gast gebruikers in uw directory (gedefinieerd als gebruikers accounts waarvan de eigenschap **User type** gelijk is aan **gast**) worden geblokkeerd voor het gebruik van de AzureAD Power shell v1-module. Als een gebruiker verdergaat, moet deze lid zijn van een gebruiker (waarbij **User type** gelijk is aan **lid**) of gebruikmaken van de AzureAD Power shell V2-module.

## <a name="in-an-azure-us-government-tenant-i-cant-invite-a-b2b-collaboration-guest-user"></a>In een Azure US Government-Tenant kan ik geen gast gebruiker voor B2B-samen werking uitnodigen

Binnen de Azure-Cloud voor de Amerikaanse overheid wordt B2B-samen werking momenteel alleen ondersteund tussen tenants die zich in de cloud van Azure Amerikaanse overheid bevinden en die allebei ondersteuning bieden voor B2B-samen werking. Als u een gebruiker uitnodigt in een Tenant die geen deel uitmaakt van de Azure-Cloud voor de Amerikaanse overheid of die geen ondersteuning biedt voor B2B-samen werking, krijgt u een fout melding. Zie [Azure Active Directory Premium P1 en P2-variaties](https://docs.microsoft.com/azure/azure-government/documentation-government-services-securityandidentity#azure-active-directory-premium-p1-and-p2)voor meer informatie en beperkingen.

## <a name="next-steps"></a>Volgende stappen

[Ondersteuning voor B2B-samen werking verkrijgen](get-support.md)
