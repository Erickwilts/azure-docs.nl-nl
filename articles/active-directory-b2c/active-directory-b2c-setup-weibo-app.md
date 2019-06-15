---
title: Instellen van zich kunnen registreren en aanmelden met een Weibo-account met behulp van Azure Active Directory B2C | Microsoft Docs
description: Meld u aan en meld u bieden aan klanten met een Weibo account in uw toepassingen met behulp van Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 808d4bc8521917b89a7265be6dfab60757baf910
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66508051"
---
# <a name="set-up-sign-up-and-sign-in-with-a-weibo-account-using-azure-active-directory-b2c"></a>Instellen van zich kunnen registreren en aanmelden met een Weibo-account met behulp van Azure Active Directory B2C

> [!NOTE]
> Deze functie is beschikbaar als preview-versie.
> 

## <a name="create-a-weibo-application"></a>Een Weibo-toepassing maken

Als u wilt een Weibo-account gebruiken als een id-provider in Azure Active Directory (Azure AD) B2C, moet u een toepassing maken in de tenant die aangeeft. Als u nog een Weibo-account hebt, kunt u krijgen via [ https://weibo.com/signup/signup.php?lang=en-us ](https://weibo.com/signup/signup.php?lang=en-us).

1. Aanmelden bij de [Weibo ontwikkelaarsportal](https://open.weibo.com/) met de referenties van uw Weibo-account.
2. Na de aanmelding, selecteert u uw weergavenaam in de rechterbovenhoek.
3. Selecteer in de vervolgkeuzelijst**编辑开发者信息**(informatie voor ontwikkelaars bewerken).
4. Voer de vereiste gegevens in en selecteer**提交**(verzenden).
5. Voltooi de verificatieprocedure van de e-mailbericht.
6. Ga naar de [identiteitverificatie pagina](https://open.weibo.com/developers/identity/edit).
7. Voer de vereiste gegevens in en selecteer**提交**(verzenden).

### <a name="register-a-weibo-application"></a>Een toepassing Weibo registreren

1. Ga naar de [nieuwe pagina voor de app registratie Weibo](https://open.weibo.com/apps/new).
2. Voer informatie over de benodigde toepassing.
3. Selecteer**创建**(maken).
4. Kopieer de waarden van **App-sleutel** en **Appgeheim**. U moet deze de id-provider toevoegen aan uw tenant.
5. De vereiste foto's uploaden en voer de benodigde gegevens.
6. Selecteer**保存以上信息**(opslaan).
7. Selecteer**高级信息**(geavanceerde informatie over).
8. Selecteer**编辑**(bewerken) naast het veld voor OAuth 2.0**授权设置**(Omleidings-URL).
9. Voer `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` voor OAuth 2.0**授权设置**(Omleidings-URL). Bijvoorbeeld, als de tenantnaam van uw contoso is, de URL die moet worden ingesteld `https://contoso.b2clogin.com/contoso.onmicrosoft.com/oauth2/authresp`.
10. Selecteer**提交**(verzenden).  

## <a name="configure-a-weibo-account-as-an-identity-provider"></a>Een account Weibo configureren als een id-provider

1. Meld u als globale beheerder van de Azure AD B2C-tenant aan bij [Azure Portal](https://portal.azure.com/).
2. Zorg ervoor dat u de map met uw Azure AD B2C-tenant door te klikken op de **map- en abonnementsfilter** in het bovenste menu en de map waarin uw tenant te kiezen.
3. Kies **Alle services** linksboven in de Azure Portal, zoek **Azure AD B2C** en selecteer deze.
4. Selecteer **id-providers**, en selecteer vervolgens **toevoegen**.
5. Geef een **naam**. Voer bijvoorbeeld *Weibo*.
6. Selecteer **type id-provider**, selecteer **Weibo (Preview)** , en klikt u op **OK**.
7. Selecteer **instellen van deze id-provider** en voer de App-sleutel die u eerder hebt genoteerd als de **Client-ID** en voert u de App-geheim die u hebt genoteerd als de **clientgeheim** van de Weibo-toepassing die u eerder hebt gemaakt.
8. Klik op **OK** en klik vervolgens op **maken** aan uw Weibo-configuratie op te slaan.
