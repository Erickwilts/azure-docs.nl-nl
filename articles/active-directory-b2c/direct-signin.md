---
title: Direct aanmelden instellen met behulp van Azure Active Directory B2C | Microsoft Docs
description: Meer informatie over het vooraf invullen van de aanmeldings naam of het direct omleiden naar een sociale ID-provider.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 06/18/2018
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 9a02ad3ea43ae9d91489417bc314e3c23d54a958
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "78188763"
---
# <a name="set-up-direct-sign-in-using-azure-active-directory-b2c"></a>Direct aanmelden instellen met behulp van Azure Active Directory B2C

Wanneer u aanmelden voor uw toepassing instelt met behulp van Azure Active Directory (AD) B2C, kunt u de aanmeldings naam of de directe aanmelding vooraf invullen bij een specifieke ID-provider voor sociale netwerken, zoals Facebook, LinkedIn of een Microsoft-account.

## <a name="prepopulate-the-sign-in-name"></a>Vul de aanmeldings naam vooraf in

Tijdens een traject voor aanmeldings gebruikers kan een Relying Party toepassing een specifieke gebruiker of domein naam aanrichten. Bij het instellen van een gebruiker kan een toepassing, in de autorisatie aanvraag, de `login_hint` query parameter met de aanmeldings naam van de gebruiker opgeven. Azure AD B2C vult de aanmeldings naam automatisch in, terwijl de gebruiker alleen het wacht woord hoeft op te geven.

![Aanmeldings pagina voor aanmelding met login_hint query parameter gemarkeerd in URL](./media/direct-signin/login-hint.png)

De gebruiker kan de waarde in het tekstvak voor aanmelden wijzigen.

Als u een aangepast beleid gebruikt, moet u het `SelfAsserted-LocalAccountSignin-Email` technische profiel overschrijven. Stel in `<InputClaims>` de sectie de DefaultValue van de signInName-claim in `{OIDC:LoginHint}`op. De `{OIDC:LoginHint}` variabele bevat de waarde van de `login_hint` para meter. Azure AD B2C leest de waarde van de claim signInName en vult het tekstvak signInName vooraf in.

```xml
<ClaimsProvider>
  <DisplayName>Local Account</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="SelfAsserted-LocalAccountSignin-Email">
      <InputClaims>
        <!-- Add the login hint value to the sign-in names claim type -->
        <InputClaim ClaimTypeReferenceId="signInName" DefaultValue="{OIDC:LoginHint}" />
      </InputClaims>
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="redirect-sign-in-to-a-social-provider"></a>Aanmelding door sturen naar een sociale provider

Als u de aanmeldings traject hebt geconfigureerd zodat uw toepassing sociale accounts kan bevatten, zoals Facebook, LinkedIn of Google, kunt u de `domain_hint` para meter opgeven. Deze query parameter biedt een hint voor het Azure AD B2C over de ID-provider voor sociale netwerken die moet worden gebruikt voor aanmelding. Als de toepassing bijvoorbeeld is opgegeven `domain_hint=facebook.com`, wordt de aanmelding direct naar de aanmeldings pagina van Facebook verzonden.

![Aanmeldings pagina voor aanmelding met domain_hint query parameter gemarkeerd in URL](./media/direct-signin/domain-hint.png)

Als u een aangepast beleid gebruikt, kunt u de domein naam configureren met behulp `<Domain>domain name</Domain>` van het XML- `<ClaimsProvider>`element van elke.

```xml
<ClaimsProvider>
    <!-- Add the domain hint value to the claims provider -->
    <Domain>facebook.com</Domain>
    <DisplayName>Facebook</DisplayName>
    <TechnicalProfiles>
    ...
```


