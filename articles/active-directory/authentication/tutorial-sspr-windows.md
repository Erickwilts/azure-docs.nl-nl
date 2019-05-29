---
title: Azure AD SSPR vanuit het aanmeldingsscherm van Windows 10
description: In deze zelfstudie gaat u in het aanmeldingsscherm van Windows 10 het opnieuw instellen van wachtwoorden inschakelen om zo het aantal telefonische hulpaanvragen naar de helpdesk te verminderen.
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: tutorial
ms.date: 02/01/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: ea65120a2a735477d048b9012e160e0cdafe8835
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/28/2019
ms.locfileid: "66253069"
---
# <a name="tutorial-azure-ad-password-reset-from-the-login-screen"></a>Zelfstudie: Azure AD-wachtwoord opnieuw instellen vanuit het aanmeldingsscherm

In deze zelfstudie gaat u gebruikers in staat stellen om hun wachtwoorden op het aanmeldingsscherm van Windows 10 opnieuw in te stellen. Met de nieuwe Windows-update van 10 april 2018 kunnen gebruikers met apparaten die zijn toegevoegd aan **Azure Active Directory** of **hybride Azure Active Directory**, de koppeling 'Wachtwoord opnieuw instellen' op hun aanmeldingsscherm gebruiken. Wanneer gebruikers op deze koppeling klikken, worden ze omgeleid naar dezelfde selfservice voor wachtwoordherstel (SSPR) als waarmee ze al bekend zijn. Als een gebruiker is vergrendeld is dit proces niet ontgrendelen van accounts in on-premises Active Directory.

> [!div class="checklist"]
> * De koppeling Wachtwoord opnieuw instellen configureren met Intune
> * Indien gewenst configureren met behulp van het Windows-register
> * Weten wat gebruikers te zien krijgen

## <a name="prerequisites"></a>Vereisten

* U moet actief ten minste Windows 10, versie April 2018 Update (v1803) en de apparaten moeten zijn:
   * [aan Azure AD zijn gekoppeld](../device-management-azure-portal.md) of
   * [aan een hybride Azure AD zijn gekoppeld](../device-management-hybrid-azuread-joined-devices-setup.md), met netwerkconnectiviteit aan een domeincontroller.
* U moet self-service voor wachtwoordherstel voor Azure AD inschakelen.
* Als uw Windows 10-apparaten zich achter een proxyserver of firewall, moet u de URL's toevoegen `passwordreset.microsoftonline.com` en `ajax.aspnetcdn.com` aan de HTTPS-verkeer (poort 443) toegestane lijst met URL's.
* Self-service voor Wachtwoordherstel voor Windows 10 wordt alleen ondersteund met proxy's op computerniveau
* Bekijk de beperkingen hieronder voordat u deze functie in uw omgeving gebruikt.
* Als u een afbeelding, voordat sysprep ervoor te zorgen dat de webcache is uitgeschakeld voor de ingebouwde beheerder voordat u de stap CopyProfile uitvoert. Meer informatie hierover vindt u in het ondersteuningsartikel [slechte prestaties bij het gebruik van aangepaste standaardgebruikersprofiel](https://support.microsoft.com/help/4056823/performance-issue-with-custom-default-user-profile).

## <a name="configure-reset-password-link-using-intune"></a>De koppeling Wachtwoord opnieuw instellen configureren met Intune

Intune gebruiken om de configuratie te wijzigen zodat gebruikers het wachtwoord opnieuw kunnen instellen vanuit het aanmeldingsscherm, is de meest flexibele methode. Met Intune kunt u een wijziging van de configuratie implementeren voor een specifieke groep machines die u hebt gedefinieerd. Voor deze methode is een Intune-registratie van het apparaat vereist.

### <a name="create-a-device-configuration-policy-in-intune"></a>Een beleid voor apparaatconfiguratie maken in Intune

1. Meld u aan bij [Azure Portal](https://portal.azure.com) en klik op **Intune**.
2. Maak een nieuw apparaatconfiguratieprofiel door te gaan naar **Apparaatconfiguratie** > **Profielen** > **Profiel maken**
   * Geef een beschrijvende naam op voor het profiel
   * Geef eventueel een duidelijke beschrijving van het profiel op
   * Platform **Windows 10 en hoger**
   * Profieltype **Aangepast**

3. **Instellingen** configureren
   * **Voeg de volgende OMA-URI-instelling toe** om de koppeling Wachtwoord opnieuw instellen in te schakelen
      * Geef een beschrijvende naam op om uit te leggen wat de instelling doet
      * Geef eventueel een duidelijke beschrijving van de instelling
      * **OMA-URI** ingesteld op `./Vendor/MSFT/Policy/Config/Authentication/AllowAadPasswordReset`
      * **Gegevenstype** ingesteld op **Geheel getal**
      * **Waarde** ingesteld op **1**
      * Klik op **OK**
   * Klik op **OK**
4. Klik op **Maken**

### <a name="assign-a-device-configuration-policy-in-intune"></a>Een beleid voor apparaatconfiguratie toewijzen in Intune

#### <a name="create-a-group-to-apply-device-configuration-policy-to"></a>Een groep maken waarop het beleid voor apparaatconfiguratie moet worden toegepast

1. Meld u aan bij [Azure Portal](https://portal.azure.com) en klik op **Azure Active Directory**.
2. Blader naar **Gebruikers en groepen** > **Alle groepen** > **Nieuwe groep**
3. Geef een naam op voor de groep en kies onder **Type lidmaatschap** de optie **Toegewezen**
   * Kies onder **Leden** de aan Azure AD toegevoegde Windows 10-apparaten waarop u het beleid wilt toepassen.
   * Klik op **Selecteren**.
4. Klik op **Maken**

Meer informatie over het maken van groepen vindt u in het artikel [Toegang tot resources beheren met Azure Active Directory-groepen](../fundamentals/active-directory-manage-groups.md).

#### <a name="assign-device-configuration-policy-to-device-group"></a>Beleid voor apparaatconfiguratie toewijzen aan een apparaatgroep

1. Meld u aan bij [Azure Portal](https://portal.azure.com) en klik op **Intune**.
2. Zoek het apparaatconfiguratieprofiel dat u eerder hebt gemaakt door te gaan naar **Apparaatconfiguratie** > **Profielen** en te klikken op het profiel
3. Het profiel toewijzen aan een groep apparaten 
   * Klik op **Toewijzingen** > onder **Opnemen** > **Groepen selecteren die moeten worden opgenomen**
   * Selecteer de groep die u eerder hebt gemaakt en klik op **Selecteren**
   * Klik op **Opslaan**

   ![Toewijzing][Assignment]

U hebt nu een beleid voor apparaatconfiguratie gemaakt en toegewezen om de koppeling Wachtwoord opnieuw instellen op het aanmeldingsscherm in te schakelen met Intune.

## <a name="configure-reset-password-link-using-the-registry"></a>De koppeling Wachtwoord opnieuw instellen configureren met het register

1. Meld u met administratorreferenties aan op de Windows-pc
2. Voer **regedit** uit als administrator
3. Stel de volgende registersleutel in
   * `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\AzureADAccount`
      * `"AllowPasswordReset"=dword:00000001`

## <a name="what-do-users-see"></a>Wat gebruikers zien

Welke wijzigingen zien de gebruikers nadat het beleid is geconfigureerd en toegewezen? Hoe weten ze dat ze hun wachtwoord opnieuw kunnen instellen via het aanmeldingsscherm?

![Aanmeldingsscherm][LoginScreen]

Wanneer gebruikers zich proberen aan te melden, zien ze nu de koppeling Wachtwoord opnieuw instellen die toegang biedt tot de selfservice voor wachtwoordherstel vanuit het aanmeldingsscherm. Deze functionaliteit maakt het mogelijk dat gebruikers hun wachtwoord opnieuw instellen zonder dat ze een ander apparaat moeten gebruiken om toegang te krijgen tot een webbrowser.

Uw gebruikers vinden hulp voor het gebruik van deze functie in [Uw wachtwoord voor werk of school opnieuw instellen](../user-help/active-directory-passwords-update-your-own-password.md#reset-password-at-sign-in)

Het auditlogboek van Azure AD bevat informatie over het IP-adres en het ClientType waarvoor het wachtwoord opnieuw is ingesteld.

![Voorbeeld van opnieuw instellen van wachtwoord via het aanmeldingsscherm in het auditlogboek van Azure AD](media/tutorial-sspr-windows/windows-sspr-azure-ad-audit-log.png)

Als gebruikers een wachtwoord opnieuw instellen vanaf het aanmeldingsscherm van een Windows 10-apparaat, wordt er een tijdelijk account met lage bevoegdheid gemaakt met de naam defaultuser1. Dit account wordt gebruikt om het proces voor het opnieuw instellen van het wachtwoord te beveiligen. Het account zelf kent een willekeurig gegenereerd wachtwoord, wordt niet getoond bij het aanmelden op een apparaat en wordt automatisch verwijderd nadat de gebruiker het wachtwoord opnieuw heeft ingesteld. Er kunnen meerdere defaultuser''-profielen naast elkaar bestaan, maar deze kunnen worden genegeerd.

## <a name="limitations"></a>Beperkingen

Wanneer u deze functie test met Hyper-V, wordt de koppeling 'Wachtwoord opnieuw instellen' niet weergegeven.

* Ga naar de VM die u voor de test gebruikt, klik op **Weergave** en schakel **Uitgebreide sessie** uit.

Wanneer u deze functionaliteit test met Extern bureaublad of een Verbeterde VM-sessie, wordt de koppeling Wachtwoord opnieuw instellen niet weergegeven.

* Wachtwoordherstel wordt momenteel niet ondersteund vanaf een extern bureaublad.

Als u Ctrl + Alt + Del is vereist door het beleid in versies van Windows 10 voordat u v1809, **wachtwoord opnieuw instellen** werkt niet.

Als meldingen voor het vergrendelen van het scherm zijn uitgeschakeld, werkt **Wachtwoord opnieuw instellen** niet.

Van de volgende beleidsinstellingen is bekend dat ze de mogelijkheid om wachtwoorden opnieuw in te stellen verstoren

   * HideFastUserSwitching, indien ingesteld op ingeschakeld of 1
   * DontDisplayLastUserName, indien ingesteld op ingeschakeld of 1
   * NoLockScreen, indien ingesteld op ingeschakeld of 1
   * EnableLostMode, indien ingesteld op het apparaat
   * Explorer.exe is vervangen door een aangepaste shell

Deze functie werkt niet voor netwerken waarvoor 802.1x-netwerkverificatie is geïmplementeerd en de optie Onmiddellijk uitvoeren voor gebruiker zich aanmeldt. Voor netwerken waarvoor 802.1x-netwerkverificatie is geïmplementeerd, wordt het aanbevolen computerverificatie te gebruiken om deze functie in te schakelen.

Voor scenario's waarbij wordt deelgenomen aan een hybride domein, wordt de SSPR-werkstroom voltooid zonder dat er een Active Directory-domeincontroller is vereist. Als een gebruiker het wachtwoord opnieuw instelt terwijl er geen communicatie met een Active Directory-domeincontroller mogelijk is (bijvoorbeeld als er extern wordt gewerkt), kan de gebruiker zich pas aanmelden bij het apparaat als het apparaat kan communiceren met een domeincontroller en de in de cache opgeslagen referentie kan bijwerken. **Connectiviteit met een domeincontroller is vereist om het nieuwe wachtwoord de eerste keer te gebruiken**.

## <a name="clean-up-resources"></a>Resources opschonen

Als u besluit dat u de functionaliteit die u hebt geconfigureerd als onderdeel van deze zelfstudie niet meer wilt gebruiken, verwijdert u het Intune-apparaatconfiguratieprofiel dat u hebt gemaakt of de registersleutel.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u gebruikers in staat gesteld om hun wachtwoorden op het aanmeldingsscherm van Windows 10 opnieuw in te stellen. Ga verder met de volgende zelfstudie om te zien hoe Azure Identity Protection kan worden geïntegreerd in de selfservice voor het opnieuw instellen van wachtwoorden en Multi-Factor Authentication-ervaringen.

> [!div class="nextstepaction"]
> [Risico's beoordelen bij het aanmelden](tutorial-risk-based-sspr-mfa.md)

[Assignment]: ./media/tutorial-sspr-windows/profile-assignment.png "Intune-beleid voor apparaatconfiguratie toewijzen aan een groep Windows 10-apparaten"
[LoginScreen]: ./media/tutorial-sspr-windows/logon-reset-password.png "De koppeling Wachtwoord opnieuw instellen op het aanmeldingsscherm van Windows 10"
