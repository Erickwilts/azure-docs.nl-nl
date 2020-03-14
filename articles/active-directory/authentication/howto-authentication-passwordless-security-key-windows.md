---
title: Aanmeldings wachtwoord zonder wacht woord voor Windows-Azure Active Directory
description: Meer informatie over het inschakelen van aanmelden met een wacht woord voor een beveiligde sleutel Azure Active Directory met behulp van FIDO2-beveiligings sleutels (preview-versie)
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 01/30/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: librown, aakapo
ms.collection: M365-identity-device-management
ms.openlocfilehash: ca6ef244a887e75a0d8b9bb663d5325a33cd1e89
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/13/2020
ms.locfileid: "79263905"
---
# <a name="enable-passwordless-security-key-sign-in-to-windows-10-devices-with-azure-active-directory-preview"></a>Aanmeldings wachtwoord zonder wacht woord inschakelen voor Windows 10-apparaten met Azure Active Directory (preview-versie)

Dit document is gericht op het inschakelen van FIDO2-verificatie op basis van wacht woorden met Windows 10-apparaten. Aan het einde van dit artikel kunt u zich aanmelden bij uw Azure AD-en hybride Azure AD Windows 10-apparaten met uw Azure AD-account met behulp van een FIDO2-beveiligings sleutel.

|     |
| --- |
| FIDO2-beveiligings sleutels zijn een open bare preview-functie van Azure Active Directory. Zie [aanvullende gebruiks voorwaarden voor Microsoft Azure-previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie over Previews|
|     |

## <a name="requirements"></a>Vereisten

| Apparaattype | Toegevoegd aan Azure AD | Hybride Azure AD-join |
| --- | --- | --- |
| [Azure Multi-Factor Authentication](howto-mfa-getstarted.md) | X | X |
| [Preview van registratie van gecombineerde beveiligings gegevens](concept-registration-mfa-sspr-combined.md) | X | X |
| Compatibele [FIDO2-beveiligings sleutels](concept-authentication-passwordless.md#fido2-security-keys) | X | X |
| Webauthn vereist Windows 10 versie 1809 of hoger | X | X |
| Voor apparaten die lid zijn van [Azure AD](../devices/concept-azure-ad-join.md) is Windows 10 versie 1903 of hoger vereist | X |   |
| Voor [hybride Azure AD gekoppelde apparaten](../devices/concept-azure-ad-join-hybrid.md) is Windows 10 Insider build 18945 of hoger vereist |   | X |
| Volledige patches voor Windows Server 2016/2019-domein controllers. |   | X |
| [Azure AD Connect](../hybrid/how-to-connect-install-roadmap.md#install-azure-ad-connect) versie 1.4.32.0 of hoger |   | X |
| [Microsoft intune](https://docs.microsoft.com/intune/fundamentals/what-is-intune) (optioneel) | X | X |
| Inrichtings pakket (optioneel) | X | X |
| Groepsbeleid (optioneel) |   | X |

### <a name="unsupported-scenarios"></a>Niet-ondersteunde scenario's

De volgende scenario's worden niet ondersteund:

- Windows Server-Active Directory Domain Services (AD DS) de implementatie van een domein (alleen on-premises apparaten).
- RDP-, VDI-en Citrix-scenario's met behulp van een beveiligings sleutel.
- S/MIME met behulp van een beveiligings sleutel.
- ' Uitvoeren als ' met een beveiligings sleutel.
- Meld u aan bij een server met behulp van een beveiligings sleutel.
- Als u uw beveiligings sleutel niet hebt gebruikt om u online aan te melden bij uw apparaat, kunt u dit niet gebruiken om u aan te melden of om offline te ontgrendelen.
- Een Windows 10-apparaat aanmelden of ontgrendelen met een beveiligings sleutel met meerdere Azure AD-accounts. Dit scenario maakt gebruik van het laatste account dat aan de beveiligings sleutel is toegevoegd. Met webauthx kunnen gebruikers het account kiezen dat ze willen gebruiken.
- Een apparaat met Windows 10 versie 1809 ontgrendelen. Gebruik Windows 10 versie 1903 of hoger voor de beste ervaring.

## <a name="prepare-devices-for-preview"></a>Apparaten voorbereiden voor de preview-versie

Aan Azure AD gekoppelde apparaten die u tijdens de preview-versie van de functie test, moeten Windows 10-versies 1809 of hoger uitvoeren. De beste ervaring is met Windows 10 versie 1903 of hoger.

Voor hybride Azure AD gekoppelde apparaten moet Windows 10 Insider build 18945 of hoger worden uitgevoerd.

## <a name="enable-security-keys-for-windows-sign-in"></a>Beveiligings sleutels voor Windows-aanmelding inschakelen

Organisaties kunnen ervoor kiezen om een of meer van de volgende methoden te gebruiken om het gebruik van beveiligings sleutels voor Windows-aanmelding in te scha kelen op basis van de vereisten van de organisatie:

- [Inschakelen met intune](#enable-with-intune)
- [Doel gerichte intune-implementatie](#targeted-intune-deployment)
- [Inschakelen met een inrichtings pakket](#enable-with-a-provisioning-package)
- [Inschakelen met groepsbeleid (alleen voor hybride Azure AD gekoppelde apparaten)](#enable-with-group-policy)

> [!IMPORTANT]
> Organisaties met **hybride Azure AD gekoppelde apparaten** moeten **ook** de stappen in het artikel uitvoeren, [FIDO2-verificatie inschakelen voor on-premises resources](howto-authentication-passwordless-security-key-on-premises.md) voordat de verificatie van de beveiligings sleutel voor Windows 10 FIDO2 werkt.
>
> Organisaties met **apparaten die lid zijn van Azure AD** moeten dit doen voordat hun apparaten kunnen worden geverifieerd bij on-premises bronnen met FIDO2-beveiligings sleutels.

### <a name="enable-with-intune"></a>Inschakelen met intune

Voer de volgende stappen uit om het gebruik van beveiligings sleutels met intune in te scha kelen:

1. Meld u aan bij de [Azure-portal](https://portal.azure.com).
1. Blader naar **Microsoft Intune** > **apparaatregistratie** > **Windows-inschrijving** > **Windows hello voor bedrijven** > - **Eigenschappen**.
1. Stel onder **instellingen** **beveiligings sleutels gebruiken voor aanmelden in** op **ingeschakeld**.

Configuratie van beveiligings sleutels voor aanmelden is niet afhankelijk van het configureren van Windows hello voor bedrijven.

### <a name="targeted-intune-deployment"></a>Doel gerichte intune-implementatie

Als u specifieke apparaatgroepen wilt instellen om de referentie provider in te scha kelen, gebruikt u de volgende aangepaste instellingen via intune:

1. Meld u aan bij de [Azure-portal](https://portal.azure.com).
1. Blader naar **Microsoft Intune** > **apparaatconfiguratie** > **profielen** > **profiel maken**.
1. Configureer het nieuwe profiel met de volgende instellingen:
   - Naam: beveiligings sleutels voor Windows-aanmelding
   - Beschrijving: Hiermee kunt u FIDO-beveiligings sleutels gebruiken tijdens het aanmelden bij Windows
   - Platform: Windows 10 en hoger
   - Profiel type: aangepast
   - Aangepaste OMA-URI-instellingen:
      - Naam: Schakel FIDO-beveiligings sleutels voor Windows-aanmelding in
      - OMA-URI: ./Device/Vendor/MSFT/PassportForWork/SecurityKey/UseSecurityKeyForSignin
      - Gegevens type: geheel getal
      - Waarde: 1
1. Dit beleid kan worden toegewezen aan specifieke gebruikers, apparaten of groepen. Zie [profielen voor gebruikers en apparaten toewijzen in Microsoft intune](https://docs.microsoft.com/intune/device-profile-assign)voor meer informatie.

![Beleid voor aangepaste configuratie van intune-apparaten maken](./media/howto-authentication-passwordless-security-key/intune-custom-profile.png)

### <a name="enable-with-a-provisioning-package"></a>Inschakelen met een inrichtings pakket

Voor apparaten die niet worden beheerd door intune, kan een inrichtings pakket worden geïnstalleerd om de functionaliteit in te scha kelen. De app Windows Configuration Designer kan worden geïnstalleerd vanuit de [Microsoft Store](https://www.microsoft.com/p/windows-configuration-designer/9nblggh4tx22). Voer de volgende stappen uit om een inrichtings pakket te maken:

1. Start de ontwerp functie voor Windows-configuratie.
1. Selecteer **bestand** > **Nieuw project**.
1. Geef uw project een naam en noteer het pad waar het project is gemaakt en selecteer vervolgens **volgende**.
1. Verlaat het *inrichtings pakket* dat is geselecteerd als de **geselecteerde project werk stroom** en selecteer **volgende**.
1. Selecteer *alle Windows Desktop-edities* onder **Kies welke instellingen u wilt weer geven en configureren en**Selecteer vervolgens **volgende**.
1. Selecteer **Voltooien**.
1. Blader in het zojuist gemaakte project naar **runtime-instellingen** > **WindowsHelloForBusiness** > **SecurityKeys** > **UseSecurityKeyForSignIn**.
1. Stel **UseSecurityKeyForSignIn** in op *ingeschakeld*.
1.  > **inrichtings pakket** voor **export** selecteren
1. Accepteer de standaard waarden in het venster **Build** onder **Beschrijving van het inrichtings pakket**en selecteer vervolgens **volgende**.
1. Accepteer de standaard waarden in het venster **bouwen** onder **Selecteer beveiligings Details voor het inrichtings pakket** en selecteer **volgende**.
1. Noteer of wijzig het pad in de **Build** -Vensters onder **selecteren waar het inrichtings pakket moet worden opgeslagen** en selecteer **volgende**.
1. Selecteer **samen stellen** op de pagina **het inrichtings pakket bouwen** .
1. Sla de twee gemaakte bestanden (*ppkg* en *Cat*) op een locatie op waar u ze op een later tijdstip op de computers kunt Toep assen.
1. Als u het inrichtings pakket wilt Toep assen dat u hebt gemaakt, raadpleegt u [een inrichtings pakket Toep assen](https://docs.microsoft.com/windows/configuration/provisioning-packages/provisioning-apply-package).

> [!NOTE]
> Op apparaten met Windows 10 versie 1809 moet ook de modus gedeelde PC (*EnableSharedPCMode*) worden ingeschakeld. Zie [een gedeelde of gast computer instellen met Windows 10](https://docs.microsoft.com/windows/configuration/set-up-shared-or-guest-pc)voor meer informatie over het inschakelen van deze functionaliteit.

### <a name="enable-with-group-policy"></a>Inschakelen met groepsbeleid

Organisaties kunnen met **hybride Azure AD gekoppelde apparaten**de volgende Groepsbeleid configureren om aanmelden met Fido-beveiligings sleutel in te scha kelen. U kunt de instelling vinden onder **computer configuratie** > **Beheersjablonen** > **systeem** > **Aanmelden** > aanmelden met **beveiligings sleutel inschakelen**:

- Als u dit beleid instelt op **ingeschakeld** , kunnen gebruikers zich aanmelden met beveiligings sleutels.
- Als u dit beleid instelt op **uitgeschakeld** of **niet geconfigureerd** , kunnen gebruikers zich niet meer aanmelden met beveiligings sleutels.

Voor deze groepsbeleid instelling is een bijgewerkte versie van de `credentialprovider.admx` groepsbeleid sjabloon vereist. Deze nieuwe sjabloon is beschikbaar in de volgende versie van Windows Server en met Windows 10 20H1. Deze instelling kan worden beheerd met een apparaat waarop een van deze nieuwere versies van Windows of een andere versie wordt uitgevoerd door de richt lijnen in het ondersteunings onderwerp te volgen, [het centrale archief voor Groepsbeleid-Beheersjablonen in Windows te maken en te beheren](https://support.microsoft.com/help/3087759/how-to-create-and-manage-the-central-store-for-group-policy-administra).

## <a name="sign-in-with-fido2-security-key"></a>Aanmelden met FIDO2-beveiligings sleutel

In het onderstaande voor beeld heeft een gebruiker met de naam Bala Sandhu al hun FIDO2-beveiligings sleutel ingericht met behulp van de stappen in het vorige artikel, [wacht woordloze beveiligings sleutel aanmelden inschakelen](howto-authentication-passwordless-security-key.md#user-registration-and-management-of-fido2-security-keys). Zorg ervoor dat u ook voor hybride Azure AD gekoppelde apparaten een [wacht woord hebt ingeschakeld voor de on-premises resources](howto-authentication-passwordless-security-key-on-premises.md). Bala kan de referentie provider voor de beveiligings sleutel kiezen in het vergrendelings scherm van Windows 10 en de beveiligings sleutel voor aanmelding bij Windows invoegen.

![De beveiligings sleutel meldt zich aan bij het vergrendelings scherm van Windows 10](./media/howto-authentication-passwordless-security-key/fido2-windows-10-1903-sign-in-lock-screen.png)

### <a name="manage-security-key-biometric-pin-or-reset-security-key"></a>Beveiligings sleutel biometrisch, pincode of opnieuw instellen beveiligings sleutel beheren

* Windows 10 versie 1903 of hoger
   * Gebruikers kunnen **Windows-instellingen** op hun apparaat > **accounts** > **beveiligings sleutel** openen
   * Gebruikers kunnen hun pincode wijzigen, biometrie bijwerken of hun beveiligings sleutel opnieuw instellen

## <a name="troubleshooting-and-feedback"></a>Problemen oplossen en feedback

Als u feedback wilt delen of problemen ondervindt tijdens het vooraf bekijken van deze functie, kunt u met de volgende stappen delen via de Windows feedback hub-app:

1. Start de **feedback-hub** en zorg ervoor dat u bent aangemeld.
1. Feedback verzenden in de volgende categorisatie:
   - Categorie: beveiliging en privacy
   - Subcategorie: FIDO
1. Als u logboeken wilt vastleggen, gebruikt u de optie om **mijn probleem opnieuw te maken**

## <a name="next-steps"></a>Volgende stappen

[Toegang tot on-premises resources inschakelen voor Azure AD en hybride Azure AD gekoppelde apparaten](howto-authentication-passwordless-security-key-on-premises.md)

[Meer informatie over apparaatregistratie](../devices/overview.md)

[Meer informatie over Azure Multi-Factor Authentication](../authentication/howto-mfa-getstarted.md)
