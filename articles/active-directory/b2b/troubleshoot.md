---
title: 'B2B-samenwerking: Azure Active Directory oplossen | Microsoft Docs'
description: Oplossingen voor veelvoorkomende problemen met Azure Active Directory B2B-samenwerking
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 05/25/2017
ms.author: mimart
author: v-miegge
manager: celestedg
ms.reviewer: mal
ms.custom: it-pro, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4185d29ff1770ed9549b4b63a2e5da579bcf054f
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65767162"
---
# <a name="troubleshooting-azure-active-directory-b2b-collaboration"></a>Oplossen van problemen met Azure Active Directory B2B-samenwerking

Hier volgen enkele oplossingen voor veelvoorkomende problemen met Azure Active Directory (Azure AD) B2B-samenwerking.

## <a name="ive-added-an-external-user-but-do-not-see-them-in-my-global-address-book-or-in-the-people-picker"></a>Kan ik een externe gebruiker hebt toegevoegd, maar ze niet zien in mijn algemene adresboek of in de personen selecteren

In gevallen waarbij externe gebruikers niet worden ingevuld in de lijst, kan het object repliceren in een paar minuten duren.

## <a name="a-b2b-guest-user-is-not-showing-up-in-sharepoint-onlineonedrive-people-picker"></a>Een gastgebruiker B2B wordt niet weergegeven in SharePoint Online/OneDrive personen selecteren

De mogelijkheid om te zoeken naar bestaande gastgebruikers ook kunnen in de SharePoint Online (SPO) personen selecteren is uitgeschakeld zodat deze overeenkomen met de verouderde gedrag standaard.

U kunt deze functie inschakelen met behulp van de instelling 'ShowPeoplePickerSuggestionsForGuestUsers' op het niveau van de verzameling tenant en de site. U kunt de functie met de cmdlets Set-SPOTenant en Set-SPOSite waarmee leden om te zoeken naar alle bestaande gastgebruikers ook kunnen in de map instellen. Wijzigingen in het tenantbereik is niet van invloed op al ingerichte SPO-sites.

## <a name="invitations-have-been-disabled-for-directory"></a>Uitnodigingen zijn uitgeschakeld voor de directory

Als u een bericht dat u bent niet gemachtigd om uit te nodigen van gebruikers, moet u controleren dat uw gebruikersaccount is gemachtigd om uit te nodigen externe gebruikers onder gebruikersinstellingen:

![Schermopname van de instellingen voor externe gebruikers](media/troubleshoot/external-user-settings.png)

Als u deze instellingen onlangs hebt gewijzigd of de rol Gastuitnodiging toegewezen aan een gebruiker, is er mogelijk een vertraging van 15 tot 60 minuut voordat de wijzigingen van kracht.

## <a name="the-user-that-i-invited-is-receiving-an-error-during-redemption"></a>De gebruiker die ik uitgenodigd is een fout ontvangen tijdens de inschrijving

Veelvoorkomende fouten zijn onder andere:

### <a name="invitees-admin-has-disallowed-emailverified-users-from-being-created-in-their-tenant"></a>Beheerder van genodigde is EmailVerified gebruikers worden gemaakt in hun tenant toegestaan

Wanneer gebruikers uitnodigen waarvan organisatie gebruikmaakt van Azure Active Directory, maar waarbij de specifieke gebruikersaccount niet bestaat (bijvoorbeeld, de gebruiker bestaat niet in Azure AD-contoso.com). De beheerder van contoso.com kan beschikken over een beleid te voorkomen dat gebruikers worden gemaakt. De gebruiker moet contact op met de beheerder om te bepalen als externe gebruikers zijn toegestaan. De beheerder van de externe gebruiker wilt toestaan dat gebruikers e-mailadres geverifieerd in een domein (Zie dit [artikel](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0) op zodat gebruikers een e-mailadres geverifieerd).

![Fout met vermelding van dat de tenant is niet toegestaan voor e-mailadres geverifieerde gebruikers](media/troubleshoot/allow-email-verified-users.png)

### <a name="external-user-does-not-exist-already-in-a-federated-domain"></a>Externe gebruiker bestaat niet al in een federatief domein

Als u van federatieve verificatie gebruikmaakt en de gebruiker nog niet in Azure Active Directory bestaat, kan de gebruiker kan niet worden uitgenodigd.

U lost dit probleem, moet de beheerder van de externe gebruiker van het gebruikersaccount met Azure Active Directory synchroniseren.

## <a name="how-does--which-is-not-normally-a-valid-character-sync-with-azure-ad"></a>Hoe wordt de\#', die is normaal gesproken geen geldig teken, synchroniseren met Azure AD?

"\#' is een gereserveerd teken in de UPN's voor Azure AD B2B-samenwerking of externe gebruikers, omdat het uitgenodigde account user@contoso.com user_contoso.com#EXT# wordt@fabrikam.onmicrosoft.com. Daarom \# in die afkomstig zijn van on-premises UPN's zijn niet toegestaan aan te melden bij Azure portal. 

## <a name="i-receive-an-error-when-adding-external-users-to-a-synchronized-group"></a>Ik ontvang een foutbericht wanneer u externe gebruikers toevoegt aan een groep gesynchroniseerd

Externe gebruikers kunnen worden toegevoegd, uitsluitend voor "toegewezen" of "-" beveiligingsgroepen en niet naar groepen die zich Gemastered on-premises.

## <a name="my-external-user-did-not-receive-an-email-to-redeem"></a>De externe gebruiker heeft geen ontvangen een e-mailbericht om in te wisselen

De genodigde neemt contact op met de Internet-provider of spam filter om ervoor te zorgen dat het volgende adres is toegestaan: Invites@microsoft.com

## <a name="i-notice-that-the-custom-message-does-not-get-included-with-invitation-messages-at-times"></a>Ik zie dat het aangepaste bericht geen opgenomen in de Uitnodigingsberichten soms krijgt

Om te voldoen aan de privacywetgeving, onze API's omvatten geen aangepaste berichten in de e-mailuitnodiging als:

- De afzender van de uitnodiging geen een e-mailadres in de uitnodigende tenant
- Wanneer een App service-principal voor de uitnodiging verzendt

Als dit scenario voor u belangrijk is, kunt u onze API uitnodigingse-mail onderdrukken en verzenden via het mechanisme voor e-mailadres van uw keuze. Raadpleeg uw juridische afdeling om te controleren of alle e-mails verzenden dat op deze manier ook voldoet aan de privacywetgeving.

## <a name="you-receive-an-aadsts65005-error-when-you-try-to-log-in-to-an-azure-resource"></a>U ontvangt een foutbericht "AADSTS65005" wanneer u probeert te melden bij een Azure-resource

Een gebruiker met een Gast-account kan worden aangemeld en ontvangt de volgende strekking weergegeven:

    AADSTS65005: Using application 'AppName' is currently not supported for your organization contoso.com because it is in an unmanaged state. An administrator needs to claim ownership of the company by DNS validation of contoso.com before the application AppName can be provisioned.

De gebruiker heeft een Azure-account en een virale tenant die is afgebroken of niet-beheerd. Bovendien zijn er geen globale of beheerders in de tenant van de bedrijfsportal.

U lost dit probleem, moet u de verlaten tenant overnemen. Raadpleeg [een niet-beheerde directory overnemen als in Azure Active Directory-beheerder](https://docs.microsoft.com/azure/active-directory/users-groups-roles/domains-admin-takeover). U moet ook toegang tot de DNS-internetgerichte server voor het betreffende domeinachtervoegsel om direct bewijzen dat u de controle van de naamruimte hebt. Nadat de tenant wordt geretourneerd naar de status van een beheerde, neem bespreken met de klant of de gebruikers verlaten en geverifieerde domeinnaam is de beste optie voor hun organisatie.

## <a name="a-guest-user-with-a-just-in-time-or-viral-tenant-is-unable-to-reset-their-password"></a>Een gastgebruiker met een just-in-time- of 'viraal'-tenant is kan niet hun wachtwoord opnieuw instellen

Als de tenant-identiteit een just-in-time (JIT) of een virale tenant is (wat betekent dat het een afzonderlijke, niet-beheerde Azure-tenant), alleen de gastgebruiker kan hun wachtwoord opnieuw instellen. Soms een organisatie wordt [neemt het beheer van virale tenants](https://docs.microsoft.com/azure/active-directory/users-groups-roles/domains-admin-takeover) die worden gemaakt wanneer werknemers hun werk-e-mailadressen gebruiken om u te registreren voor services. Nadat de organisatie ten opzichte van een virale tenant heeft, kunt alleen een beheerder in die organisatie van de gebruiker-wachtwoord opnieuw instellen of SSPR inschakelen. Indien nodig, als de organisatie, kunt u het gastgebruikersaccount verwijderen uit uw directory en een uitnodiging opnieuw verzenden.

## <a name="next-steps"></a>Volgende stappen

[Ondersteuning krijgen voor B2B-samenwerking](get-support.md)
