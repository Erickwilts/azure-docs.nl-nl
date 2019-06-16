---
title: Azure multi-factor Authentication gebruikersstatussen - Azure Active Directory
description: Meer informatie over de status van de gebruiker in Azure multi-factor Authentication.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 01/11/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6cd26f7bdf928dcbd2cf841526857dc44623f133
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67057418"
---
# <a name="how-to-require-two-step-verification-for-a-user"></a>Hoe u verificatie in twee stappen vereist voor een gebruiker

U kunt een van de twee benaderingen voor het vereisen van verificatie in twee stappen, beide met behulp van een globale beheerdersaccount moet nemen. De eerste mogelijkheid is het inschakelen van MFA voor een gebruiker. Wanneer gebruikers afzonderlijk zijn ingeschakeld, worden ze gevraagd zich te authenticeren met behulp van hun tweede stap telkens wanneer ze zich aanmelden (met enkele uitzonderingen, zoals wanneer ze zich vanaf vertrouwde-IP-adressen aanmelden of wanneer de functie _dit apparaat onhouden_ is ingeschakeld). De tweede optie is voor het instellen van een beleid voor voorwaardelijke toegang waarvoor verificatie in twee stappen onder bepaalde omstandigheden.

> [!TIP]
> Kies een van deze methoden voor het vereisen van verificatie in twee stappen, niet beide. Inschakelen van een gebruiker voor de Azure multi-factor Authentication heeft voorrang op eventuele beleidsregels voor voorwaardelijke toegang.

## <a name="choose-how-to-enable"></a>Kiezen hoe u om in te schakelen

**Ingeschakeld door het veranderen van gebruikersstatus** -dit is de traditionele methode voor het vereisen van verificatie in twee stappen en in dit artikel wordt besproken. Het werkt met beide Azure MFA in de cloud en Azure MFA-Server. Met deze methode vereist dat gebruikers om uit te voeren van verificatie in twee stappen **telkens** ze zich aanmelden en vervangt u beleid voor voorwaardelijke toegang. Dit is de methode die wordt gebruikt voor gebruikers met licenties voor Office 365 of Microsoft 365 Business, zoals ze geen functies voor voorwaardelijke toegang omvatten.

Ingeschakeld door het beleid voor voorwaardelijke toegang - is dit de meest flexibele manier om in te schakelen van verificatie in twee stappen voor uw gebruikers. Inschakelen met behulp van alleen beleid voor voorwaardelijke toegang werkt voor Azure MFA in de cloud en is een premium-functie van Azure AD. Meer informatie over deze methode kan worden gevonden [implementeren van cloud-gebaseerde Azure multi-factor Authentication](howto-mfa-getstarted.md).

Ingeschakeld door Azure AD Identity Protection - met deze methode maakt gebruik van de Azure AD Identity Protection-beleid voor gebruikersrisico's om te vereisen van verificatie in twee stappen alleen op basis van aanmeldingsrisico voor alle cloudtoepassingen. Deze methode is een Azure Active Directory P2-licentie vereist. Meer informatie over deze methode kan worden gevonden [Azure Active Directory Identity Protection](../identity-protection/howto-sign-in-risk-policy.md)

> [!Note]
> Meer informatie over licenties en prijzen vindt u op de [Azure AD](https://azure.microsoft.com/pricing/details/active-directory/
) en [multi-Factor Authentication](https://azure.microsoft.com/pricing/details/multi-factor-authentication/) prijspagina's.

## <a name="enable-azure-mfa-by-changing-user-state"></a>Azure MFA inschakelen door het veranderen van de status van gebruiker

Gebruikersaccounts in Azure multi-factor Authentication hebben de volgende drie afzonderlijke statussen:

| Status | Description | Niet-browsertoepassingen beïnvloed | Browser-apps die worden beïnvloed | Moderne verificatie beïnvloed |
|:---:|:---:|:---:|:--:|:--:|
| Uitgeschakeld |De standaardstatus voor een nieuwe gebruiker die niet zijn geregistreerd bij Azure MFA. |Nee |Nee |Nee |
| Enabled |De gebruiker is geregistreerd in Azure MFA, maar is niet geregistreerd. Ze ontvangt een prompt voor het registreren van de volgende keer dat ze zich aanmelden. |Nee.  Ze blijven werken totdat het registratieproces is voltooid. | Ja. Nadat de sessie is verlopen, is registratie bij Azure MFA is vereist.| Ja. Nadat het toegangstoken is verlopen, is registratie bij Azure MFA is vereist. |
| Afgedwongen |De gebruiker is ingeschreven en het registratieproces is voltooid voor Azure MFA. |Ja. Apps in vereist app-wachtwoorden. |Ja. Azure MFA is vereist bij het aanmelden. | Ja. Azure MFA is vereist bij het aanmelden. |

De status van een gebruiker geeft weer of een beheerder hen heeft ingeschreven in Azure MFA en of ze het registratieproces hebben voltooid.

Alle gebruikers beginnen *Uitgeschakeld*. Als u een gebruiker zich inschrijft via Azure MFA, wordt zijn of haar status gewijzigd naar *Ingeschakeld*. Zodra de ingeschakelde gebruiker zich heeft aangemeld en het registratieproces heeft voltooid, wordt de status ingesteld op *Afgedwongen*.  

### <a name="view-the-status-for-a-user"></a>De status van een gebruiker weergeven

Gebruik de volgende stappen om de pagina te openen waar u de status van een gebruiker kunt bekijken en beheren:

1. Meld u als beheerder aan bij [Azure Portal](https://portal.azure.com).
2. Ga naar **Azure Active Directory** > **gebruikers en groepen** > **alle gebruikers**.
3. Selecteer **multi-Factor Authentication**.
   ![Schakel multi-factor Authentication](./media/howto-mfa-userstates/selectmfa.png)
4. Een nieuwe pagina waarop de status van de gebruiker wordt geopend.
   ![status in de multi-factor authentication-gebruiker - schermafbeelding](./media/howto-mfa-userstates/userstate1.png)

### <a name="change-the-status-for-a-user"></a>De status van een gebruiker wijzigen

1. Gebruik de voorgaande stappen om toegang te krijgen tot de Azure MFA **gebruikers** pagina.
2. Zoek de gebruiker die u wilt inschakelen voor Azure MFA. Mogelijk moet u de weergave bovenaan wijzigen.
   ![Selecteer de gebruiker voor de status voor wijzigen van het tabblad gebruikers](./media/howto-mfa-userstates/enable1.png)
3. Schakel het selectievakje naast de naam van de gebruiker in.
4. Aan de rechterkant, onder **snelle stappen**, kiest u **inschakelen** of **uitschakelen**.
   ![Geselecteerde gebruiker inschakelen door te klikken op inschakelen in het menu snelle stappen](./media/howto-mfa-userstates/user1.png)

   > [!TIP]
   > *Ingeschakelde* gebruikers worden automatisch omgezet naar *Afgedwongen* zodra ze zich registreren voor Azure MFA. Wijzig de status van de gebruiker niet handmatig in *Afgedwongen*.

5. Bevestig uw selectie in het pop-upvenster dat wordt geopend.

Nadat u gebruikers hebt ingeschakeld, kunt u gebruikers informeren via e-mail Laat ze weten dat ze gevraagd worden om zich te registreren de volgende keer dat ze zich aanmelden. Als uw organisatie gebruikmaakt van niet-browser-apps die moderne verificatie niet ondersteunen, moeten zij ook app-wachtwoorden maken. U kunt ook een koppeling naar de [Azure MFA eindgebruiker handleiding](../user-help/multi-factor-authentication-end-user.md) meesturen waarmee gebruikers aan de slag kunnen.

### <a name="use-powershell"></a>PowerShell gebruiken

Status van de gebruiker te wijzigen met behulp van [Azure AD PowerShell](/powershell/azure/overview), wijzigen `$st.State`. Er zijn drie mogelijke statussen:

* Enabled
* Afgedwongen
* Uitgeschakeld  

Verplaats gebruikers niet rechtstreeks naar de status *Afgedwongen*. Als u dit doet, werken apps die niet op een browser zijn gebaseerd niet meer, omdat de gebruiker het registratieproces voor Azure MFA niet heeft doorlopen en dus geen [app-wachtwoord](howto-mfa-mfasettings.md#app-passwords) heeft opgehaald.

De Module eerst installeren, met behulp van:

   ```PowerShell
   Install-Module MSOnline
   ```

> [!TIP]
> Vergeet niet om te verbinden via eerst **Connect-MsolService**

In dit voorbeeld PowerShell-script kunt MFA voor een afzonderlijke gebruiker:

   ```PowerShell
   Import-Module MSOnline
   $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
   $st.RelyingParty = "*"
   $st.State = "Enabled"
   $sta = @($st)
   Set-MsolUser -UserPrincipalName bsimon@contoso.com -StrongAuthenticationRequirements $sta
   ```

Met behulp van PowerShell is een goede optie wanneer u moet bulksgewijs gebruikers in staat. Als u bijvoorbeeld het volgende script doorloopt een lijst van gebruikers en, schakelt u MFA op hun accounts:

   ```PowerShell
   $users = "bsimon@contoso.com","jsmith@contoso.com","ljacobson@contoso.com"
   foreach ($user in $users)
   {
       $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
       $st.RelyingParty = "*"
       $st.State = "Enabled"
       $sta = @($st)
       Set-MsolUser -UserPrincipalName $user -StrongAuthenticationRequirements $sta
   }
   ```

Als u wilt uitschakelen MFA, moet u dit script gebruiken:

   ```PowerShell
   Get-MsolUser -UserPrincipalName user@domain.com | Set-MsolUser -StrongAuthenticationMethods @()
   ```

dat kan ook worden ingekort tot:

   ```PowerShell
   Set-MsolUser -UserPrincipalName user@domain.com -StrongAuthenticationRequirements @()
   ```

### <a name="convert-users-from-per-user-mfa-to-conditional-access-based-mfa"></a>Gebruikers voor het converteren van MFA per gebruiker voor voorwaardelijke toegang op basis van MFA

De volgende PowerShell kan u helpen bij het maken van de conversie van voorwaardelijke toegang op basis van Azure multi-factor Authentication.

```PowerShell
# Disable MFA for all users, keeping their MFA methods intact
Get-MsolUser -All | Disable-MFA -KeepMethods

# Enforce MFA for all users
Get-MsolUser -All | Set-MfaState -State Enforced

# Wrapper to disable MFA with the option to keep the MFA
# methods (to avoid having to proof-up again later)
function Disable-Mfa {

    [CmdletBinding()]
    param(
        [Parameter(ValueFromPipeline=$True)]
        $User,
        [switch] $KeepMethods
    )

    Process {

        Write-Verbose ("Disabling MFA for user '{0}'" -f $User.UserPrincipalName)
        $User | Set-MfaState -State Disabled

        if ($KeepMethods) {
            # Restore the MFA methods which got cleared when disabling MFA
            Set-MsolUser -ObjectId $User.ObjectId `
                         -StrongAuthenticationMethods $User.StrongAuthenticationMethods
        }
    }
}

# Sets the MFA requirement state
function Set-MfaState {

    [CmdletBinding()]
    param(
        [Parameter(ValueFromPipelineByPropertyName=$True)]
        $ObjectId,
        [Parameter(ValueFromPipelineByPropertyName=$True)]
        $UserPrincipalName,
        [ValidateSet("Disabled","Enabled","Enforced")]
        $State
    )

    Process {
        Write-Verbose ("Setting MFA state for user '{0}' to '{1}'." -f $ObjectId, $State)
        $Requirements = @()
        if ($State -ne "Disabled") {
            $Requirement =
                [Microsoft.Online.Administration.StrongAuthenticationRequirement]::new()
            $Requirement.RelyingParty = "*"
            $Requirement.State = $State
            $Requirements += $Requirement
        }

        Set-MsolUser -ObjectId $ObjectId -UserPrincipalName $UserPrincipalName `
                     -StrongAuthenticationRequirements $Requirements
    }
}

```

## <a name="next-steps"></a>Volgende stappen

* Waarom is een gebruiker wordt gevraagd of niet wordt gevraagd om uit te voeren van MFA? Zie de sectie [rapport van Azure AD-aanmeldingen in de rapporten in de Azure multi-factor Authentication-document](howto-mfa-reporting.md#azure-ad-sign-ins-report).
* Zie het artikel voor het configureren van extra instellingen zoals goedgekeurde IP-adressen, aangepaste spraakberichten en Fraudewaarschuwingen [instellingen van de Azure multi-factor Authentication configureren](howto-mfa-mfasettings.md)
* Informatie over het beheren van gebruikersinstellingen voor Azure multi-factor Authentication kan worden gevonden in het artikel [gebruikersinstellingen met Azure multi-factor Authentication in de cloud beheren](howto-mfa-userdevicesettings.md)
