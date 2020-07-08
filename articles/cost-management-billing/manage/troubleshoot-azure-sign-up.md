---
title: Problemen oplossen wanneer u een nieuw account registreert in de Azure-portal of in het Azure-accountcentrum
description: Een probleem oplossen bij aanmelding voor een nieuw account in het Microsoft Azure Portal-accountcentrum.
services: cost-management-billing
author: v-miegge
manager: dcscontentpm
tags: billing
ms.service: cost-management-billing
ms.topic: troubleshooting
ms.date: 06/05/2020
ms.author: v-miegge
ms.openlocfilehash: 0cf6f1c0fcc9a2da0bbfacc9e51076add107ed26
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/08/2020
ms.locfileid: "86102698"
---
# <a name="troubleshoot-issues-when-you-sign-up-for-a-new-account-in-azure-portal-or-azure-account-center"></a>Problemen oplossen wanneer u een nieuw account registreert in de Azure-portal of in het Azure-accountcentrum

Er kan een probleem optreden wanneer u zich in Microsoft Azure Portal of het Azure-accountcentrum probeert te registreren voor een nieuw account. In deze korte handleiding doorloopt u het registratieproces en worden enkele veelvoorkomende problemen bij elke stap besproken.

> [!NOTE]
> Als u al een bestaand account hebt en hulp nodig hebt bij het oplossen van aanmeldingsproblemen, raadpleegt u [Aanmeldingsproblemen voor Azure-abonnementen oplossen](https://docs.microsoft.com/azure/cost-management-billing/manage/troubleshoot-sign-in-issue).

## <a name="before-you-begin"></a>Voordat u begint

Controleer het volgende voordat u begint met het registreren:

- De gegevens van uw Azure-accountprofiel (zoals het e-mailadres, adres en telefoonnummer van de contactpersoon), zijn correct.
- De creditcardgegevens zijn correct.
- U hebt geen ander Microsoft-account met dezelfde gegevens.

## <a name="guided-walkthrough-of-azure-sign-up"></a>Overzicht met begeleiding voor registratie bij Azure

Bij het registreren bij Azure doorloopt u vier secties:

- Over u
- Identiteitverificatie via telefoon
- Identiteitverificatie via creditcard
- Overeenkomst

In dit overzicht vindt u voorbeelden van de gegevens die nodig zijn om u voor een Azure-account te registreren. Elke sectie bevat ook enkele veelvoorkomende problemen en informatie over hoe u deze kunt oplossen.

## <a name="about-you"></a>Over u

![Over u](./media/troubleshoot-azure-sign-up/1.png)
 
### <a name="common-issues-and-solutions"></a>Bekende problemen en oplossingen

#### <a name="you-see-the-message-we-cannot-proceed-with-sign-up-due-to-an-issue-with-your-account-please-contact-billing-support"></a>U ziet het bericht 'De aanmelding kan niet worden voltooid vanwege een probleem met uw account. Neem contact op met de ondersteuning voor facturering.'

Volg deze stappen om deze fout te verhelpen:

1.  Meld u aan bij het [Azure-accountcentrum](https://account.azure.com/Profile) met de accountbeheerdersreferenties.
1.  Selecteer **Details bewerken**.
1.  Controleer of alle adresvelden zijn ingevuld en geldige waarden bevatten.
1.  Wanneer u zich registreert voor het Azure-abonnement, moet u ervoor zorgen dat het factuuradres voor de creditcardregistratie overeenkomt met uw bankgegevens.

Als het bericht nog steeds wordt weergegeven, probeert u zich te registreren in een andere browser.

Wat dacht u van InPrivate-browsing?

#### <a name="free-trial-is-not-available"></a>Er is geen gratis proefversie beschikbaar

Hebt u in het verleden een Azure-abonnement gebruikt? De gebruiksvoorwaarden van Azure beperken de activering van de gratis proefversie alleen voor een gebruiker die nieuw is in Azure. Als u eerder een ander type Azure-abonnement hebt gehad, is het niet mogelijk om een gratis proefversie te activeren. Overweeg u aan te melden voor een [abonnement voor Betalen per gebruik](https://azure.microsoft.com/offers/ms-azr-0003p/).

#### <a name="you-see-the-message-you-are-not-eligible-for-an-azure-subscription"></a>U ziet het bericht 'U komt niet in aanmerking voor een Azure-abonnement'

U kunt dit probleem oplossen door het volgende te controleren:

- De gegevens die u hebt opgegeven voor uw Azure-accountprofiel (zoals het e-mailadres, adres en telefoonnummer van de contactpersoon), zijn correct.
- De creditcardgegevens zijn correct.
- U hebt geen ander Microsoft-account waarvoor dezelfde gegevens worden gebruikt.

#### <a name="you-see-the-message-your-current-account-type-is-not-supported"></a>U ziet het bericht 'Uw huidige accounttype wordt niet ondersteund'

Dit probleem kan zich voordoen als het account is geregistreerd in een [niet-beheerde Azure AD-directory](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-self-service-signup) en niet is opgenomen in de Azure AD-directory van uw organisatie.
U kunt dit probleem oplossen door het Azure-account te registreren met behulp van een ander account of door de niet-beheerde AD-directory over te nemen. Zie [Een niet-beheerde directory overnemen als beheerder in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/users-groups-roles/domains-admin-takeover) voor meer informatie.

## <a name="identity-verification-by-phone"></a>Identiteitverificatie via telefoon

![Identiteitverificatie via telefoon](./media/troubleshoot-azure-sign-up/2.png)
 
Wanneer u het sms-bericht of de oproep ontvangt, voert u in het tekstvak die code in die u hebt ontvangen.

### <a name="common-issues-and-solutions"></a>Bekende problemen en oplossingen

#### <a name="no-verification-text-message-or-phone-call"></a>Geen verificatie-sms of -telefoontje

Hoewel het verificatieproces bij de registratie meestal snel gaat, kan het tot vier minuten duren voordat er een verificatiecode wordt verstrekt.

Hier volgen enkele aanvullende tips:

- U kunt elk telefoonnummer gebruiken voor verificatie, mits het aan de vereisten voldoet. Het telefoonnummer dat u voor verificatie invoert, wordt niet opgeslagen als contactnummer voor het account.
  - Een VoiP-telefoonnummer (Voice-over-IP) kan niet worden gebruikt voor het telefoonverificatieproces.
  - Controleer of uw telefoon oproepen of sms-berichten kan ontvangen van een telefoonnummer uit de Verenigde Staten.
- Controleer het telefoonnummer dat u invoert, inclusief het landnummer dat u in de vervolgkeuzelijst selecteert.
- Als u geen tekstberichten (sms) op uw telefoon ontvangt, probeert u de optie **Mij bellen**.

## <a name="identity-verification-by-card"></a>Identiteitverificatie via creditcard

![Identiteitverificatie via creditcard](./media/troubleshoot-azure-sign-up/3.png)
 
### <a name="common-issues-and-solutions"></a>Bekende problemen en oplossingen

#### <a name="credit-card-declined-or-not-accepted"></a>De creditcard wordt afgewezen of niet geaccepteerd

Virtuele of prepaidcreditcards of -debitcards worden niet geaccepteerd als betaalmethode voor Azure-abonnementen. Zie [Problemen met een geweigerde kaart bij Azure-aanmelding oplossen](https://docs.microsoft.com/azure/cost-management-billing/manage/troubleshoot-declined-card) om te ontdekken waardoor uw kaart nog meer kan zijn afgewezen.

#### <a name="credit-card-form-doesnt-support-my-billing-address"></a>Het creditcardformulier biedt geen ondersteuning voor mijn factuuradres

Uw factuuradres moet zich bevinden in het land dat u selecteert in de sectie **Over u**. Controleer of u het juiste land hebt geselecteerd.

#### <a name="progress-bar-hangs-in-identity-verification-by-card-section"></a>De voortgangsbalk loopt vast in de sectie Identiteitverificatie via creditcard

U moet in uw browser cookies van derden toestaan om de identiteitverificatie via creditcard te voltooien.

Voer de volgende stappen uit om de cookie-instellingen van uw browser bij te werken.

1. Werk de cookie-instellingen bij.
   - Als u **Chrome** gebruikt:
     - Selecteer **Instellingen** > **Geavanceerde instellingen weergeven** > **Privacy** > **Inhoudsinstellingen**. Wis **Cookies en sitegegevens van derden blokkeren**.

   - Als u **Microsoft Edge** gebruikt:
     - Selecteer **Instellingen** > **Geavanceerde instellingen weergeven** > **Cookies** > **Geen cookies blokkeren**.

1. Vernieuw de Azure-registratiepagina en controleer of het probleem is opgelost.
1. Als het probleem niet is opgelost, sluit u de browser en start u deze opnieuw op. Probeer het vervolgens opnieuw.

### <a name="i-saw-a-charge-on-my-free-trial-account"></a>Ik heb kosten gezien voor mijn gratis proefaccount

U kunt na registratie een kleine, tijdelijke verificatiepost voor uw creditcardaccount aantreffen. Deze wordt binnen drie tot vijf dagen verwijderd. Lees meer over [Onverwachte kosten voorkomen](https://docs.microsoft.com/azure/cost-management-billing/manage/getting-started) als u zich zorgen maakt over het kostenbeheer.

## <a name="agreement"></a>Overeenkomst

Ga akkoord met de overeenkomst.

## <a name="other-issues"></a>Overige problemen

### <a name="cant-activate-azure-benefit-plan-like-msdn-bizspark-bizsparkplus-or-mpn"></a>Kan het Azure-vergoedingsplan, bijvoorbeeld MSDN, BizSpark, BizSparkPlus of MPN, niet activeren

Controleer of u de juiste aanmeldingsreferenties gebruikt. Controleer vervolgens het voordeelprogramma om na te gaan of u wel in aanmerking komt.
- MSDN
  - Controleer uw geschiktheidsstatus op uw [MSDN-accountpagina](https://msdn.microsoft.com/subscriptions/manage/default.aspx).
  - Als u uw status niet kunt controleren, neemt u contact op met de [klantenservicecenters voor MSDN-abonnementen](https://msdn.microsoft.com/library/aa493452.aspx).
- Microsoft for Startups
  - Meld u aan bij de [Microsoft for Startups-portal](https://startups.microsoft.com/#start-two) om uw geschiktheidsstatus voor Microsoft te verifiëren voor opstarten.
  - Als u uw status niet kunt verifiëren, kunt u ondersteuning krijgen via de [Microsoft for Startups-forums](https://www.microsoftpartnercommunity.com/t5/Microsoft-for-Startups/ct-p/Microsoft_Startups).
- MPN
  - Meld u aan bij de [MPN-portal](https://mspartner.microsoft.com/Pages/Locale.aspx) om uw geschiktheidsstatus te controleren. Als u over de juiste [competenties voor het cloudplatform](https://mspartner.microsoft.com/pages/membership/cloud-platform-competency.aspx) beschikt, komt u mogelijk in aanmerking voor extra voordelen.
  - Als u uw status niet kunt verifiëren, neemt u contact op met de [MPN-ondersteuning](https://mspartner.microsoft.com/Pages/Support/Premium/contact-support.aspx).


### <a name="cant-activate-new-azure-in-open-subscription"></a>Kan geen nieuw Azure In Open-abonnement activeren

Als u een Azure In Open-abonnement wilt maken, moet u een geldige OSA-sleutel (Online Service Activation) hebben waaraan ten minste één Azure In Open-token is gekoppeld. Als u geen OSA-sleutel hebt, neemt u contact op met een van de Microsoft-partners die worden vermeld in [Microsoft Pinpoint](https://pinpoint.microsoft.com/).

## <a name="additional-help-resources"></a>Aanvullende ondersteuningsresources

Andere artikelen met probleemoplossingen voor Azure Billing en abonnementen

- [Geweigerde kaart](https://docs.microsoft.com/azure/cost-management-billing/manage/troubleshoot-declined-card)
- [Aanmeldingsproblemen voor abonnementen](https://docs.microsoft.com/azure/cost-management-billing/manage/troubleshoot-sign-in-issue)
- [Geen abonnementen gevonden](https://docs.microsoft.com/azure/cost-management-billing/manage/no-subscriptions-found)
- [Weergave voor bedrijfskosten is uitgeschakeld](https://docs.microsoft.com/azure/cost-management-billing/manage/enterprise-mgmt-grp-troubleshoot-cost-view)

## <a name="contact-us-for-help"></a>Contact met ons opnemen voor ondersteuning

Als u vragen hebt of hulp nodig hebt, [kunt u een ondersteuningsaanvraag maken](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="find-out-more-about-azure-cost-management"></a>Meer informatie over Azure Cost Management

- [Documentatie voor Azure Cost Management en facturering](https://docs.microsoft.com/azure/cost-management-billing)
