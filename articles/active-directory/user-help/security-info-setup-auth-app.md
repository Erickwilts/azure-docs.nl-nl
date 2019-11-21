---
title: Set up the Microsoft Authenticator app as your authentication method - Azure AD
description: How to set up your Security info to verify your identity using the Microsoft Authenticator app as your authentication method.
services: active-directory
author: eross-msft
manager: daveba
ms.reviewer: sahenry
ms.service: active-directory
ms.workload: identity
ms.subservice: user-help
ms.topic: conceptual
ms.date: 02/13/2019
ms.author: lizross
ms.collection: M365-identity-device-management
ms.openlocfilehash: 761c9d3ed4cb49a7dc95ea98ce6f884930185643
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2019
ms.locfileid: "74233200"
---
# <a name="set-up-the-microsoft-authenticator-app-as-your-authentication-method-from-the-security-info-preview-page"></a>Set up the Microsoft Authenticator app as your authentication method from the Security info (preview) page

You can follow these steps to add your two-factor verification and password reset methods. After you've set this up the first time, you can return to the **Security info** page to add, update, or delete your security information.

If you're prompted to set this up immediately after you sign in to your work or school account, see the detailed steps in the [Set up your security info from the sign-in page prompt](security-info-setup-signin.md) article.

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-preview-notice-security-info.md)]

>[!Note]
>If you don't see an authenticator app option, it's possible that your organization doesn't allow you to use an authentication app for verification. In this case, you'll need to choose another method or contact your administrator for more help.

## <a name="set-up-the-microsoft-authenticator-app-from-the-security-info-page"></a>Set up the Microsoft Authenticator app from the Security info page

Depending on your organization’s settings, you might be able to use an authentication app as one of your security info methods. You aren't required to use the Microsoft Authenticator app, and you can choose a different app during the set up process. However, this article uses the Microsoft Authenticator app.

### <a name="to-set-up-the-microsoft-authenticator-app"></a>To set up the Microsoft Authenticator app

1. Sign in to your work or school account and then go to your https://myprofile.microsoft.com/ page.

    ![My Profile page, showing highlighted Security info links](media/security-info/securityinfo-myprofile-page.png)

2. Select **Security info** from the left navigation pane or from the link in the **Security info** block, and then select **Add method** from the **Security info** page.

    ![Security info page with highlighted Add method option](media/security-info/securityinfo-myprofile-addmethod-page.png)

3. On the **Add a method** page, select **Authenticator app** from the drop-down list, and then select **Add**.

    ![Add method box, with Authenticator app selected](media/security-info/securityinfo-myprofile-addauthapp.png)

4. On the **Start by getting the app** page, select **Download now** to download and install the Microsoft Authenticator app on your mobile device, and then select **Next**.

    Zie voor meer informatie over het downloaden en installeren van de app, [De Microsoft Authenticator-app downloaden en installeren](user-help-auth-app-download-install.md).

    ![Start by getting the app page](media/security-info/securityinfo-myprofile-getauthapp.png)

   > [!Note]
   > Als u een andere verificator-app dan de Microsoft Authenticator-app gebruiken wilt, selecteert u de koppeling **Ik wil een andere verificator-app gebruiken**.
   >
   > Als u van uw organisatie u een andere methode dan de authenticator-app kiezen mag, kunt u de **Ik wil andere methode instellen-koppeling** selecteren.

5. Blijven op de **Uw account instellen**-pagina tijdens het instellen van de Microsoft Authenticator-app op uw mobiele apparaat.

    ![Set up the authenticator app page](media/security-info/securityinfo-myprofile-setupauthapp.png)

6. Open de Microsoft Authenticator-app, selecteer dat u meldingen toestaat (als u hierom wordt gevraagd), selecteer **Account toevoegen** uit het pictogram **Aanpassen en controle** in de rechterbovenhoek en selecteer vervolgens **Werk- of schoolaccount**.

    >[!Note]
    >If this is the first time you're setting up the Microsoft Authenticator app, you might receive a prompt asking whether to allow the app to access your camera (iOS) or to allow the app to take pictures and record video (Android). You must select **Allow** so the authenticator app can access your camera to take a picture of the QR code in the next step. If you don't allow the camera, you can still set up the authenticator app, but you'll need to add the code information manually. For information about how to add the code manually, see see [Manually add an account to the app](user-help-auth-app-add-account-manual.md).

7. Ga terug naar de pagina **Uw account instellen** op uw computer en selecteer vervolgens **Volgende**.

    De pagina **QR-code scannen** wordt weergegeven.

    ![De QR-code scannen met behulp van de Authenticator-app](media/security-info/securityinfo-myprofile-qrcodeauthapp.png)

8. Scan the provided code with the Microsoft Authenticator app QR code reader, which appeared on your mobile device after you created your work or school account in Step 6.

    De verificator-app moet nu uw werk of schoolaccount toevoegen zonder aanvullende informatie van u. Als de QR-codelezer de code echter niet lezen kan, kunt u de **Koppeling kan QR-code niet scannen** selecteren en handmatig de code en URL in de Microsoft Authenticator-app invoeren. Zie voor meer informatie over het handmatig toevoegen van een code [Handmatig een account toevoegen aan de app](user-help-auth-app-add-account-manual.md).

9. Selecteer **Volgende** op de pagina **QR-code scannen** op uw computer.

    Een melding wordt verzonden naar de Microsoft Authenticator-app op uw mobiele apparaat voor het testen van uw account.

    ![Uw account met de verificator-app testen](media/security-info/securityinfo-myprofile-tryitauthapp.png)

10. Keur de melding in de Microsoft Authenticator-app goed en selecteer vervolgens **Volgende**.

     ![Melding van geslaagd, met verbinding van de app en uw account](media/security-info/securityinfo-myprofile-successauthapp.png)

     Uw beveiligingsgegevens zijn bijgewerkt voor het standaard gebruik van de Microsoft Authenticator-app om uw identiteit te verifiëren bij het gebruik van verificatie in twee stappen of opnieuw instellen van het wachtwoord.

## <a name="delete-your-authenticator-app-from-your-security-info-methods"></a>Delete your authenticator app from your security info methods

If you no longer want to use your authenticator app as a security info method, you can remove it from the **Security info** page. This works for all authenticator apps, not just the Microsoft Authenticator app. After you delete the app, you'll have to go into the authenticator app on your mobile device and delete the account.

>[!Important]
>If you delete the authenticator app by mistake, there's no way to undo it. You'll have to add the authenticator app again, following the steps in the [Set up the authenticator app](#set-up-the-microsoft-authenticator-app-from-the-security-info-page) section of this article.

### <a name="to-delete-the-authenticator-app"></a>To delete the authenticator app

1. On the **Security info** page, select the **Delete** link next to the Authenticator app.

    ![Link to delete the authenticator app from security info](media/security-info/securityinfo-myprofile-deleteauthapp.png)

2. Select **Yes** from the confirmation box to delete the authenticator app. After the authenticator app is deleted, it's removed from your security info and it disappears from the **Security info** page. If the authenticator app is your default method, the default will change to another available method.

3. Open the authenticator app on your mobile device, select **Edit accounts**, and then delete your work or school account from the authenticator app.

    Your account is completely removed from the authenticator app for two-factor verification and password reset requests.

## <a name="change-your-default-security-info-method"></a>Change your default security info method

If you want the authenticator app to be the default method used when you sign-in to your work or school account using two-factor verification or for password reset requests, you can set it from the Security **info** page.

### <a name="to-change-your-default-security-info-method"></a>To change your default security info method

1. On the **Security info** page, select the **Change** link next to the **Default sign-in method** information.

    ![Change link for default sign-in method](media/security-info/securityinfo-myprofile-changedefaultauthapp.png)

2. Choose **Microsoft Authenticator - notification** from the drop-down list of available methods. If you're not using the Microsoft Authenticator app, select the **Authenticator app or hardware token** option.

    ![Choose method for default sign-in](media/security-info/securityinfo-myprofile-defaultauthapp.png)

3. Select **Confirm**.

    The default method used for sign-in changes to the Microsoft Authenticator app.

## <a name="additional-security-info-methods"></a>Additional security info methods

You have additional options for how your organization contacts you to verify your identity, based on what's you're trying to do. De opties zijn:

- **Mobile device text.** Enter your mobile device number and get a text a code you'll use for two-step verification or password reset. For step-by-step instructions about how to verify your identity with a text message (SMS), see [Set up security info to use text messaging (SMS)](security-info-setup-text-msg.md).

- **Mobile device or work phone call.** Enter your mobile device number and get a phone call for two-step verification or password reset. For step-by-step instructions about how to verify your identity with a phone number, see [Set up security info to use phone calls](security-info-setup-phone-number.md).

- **Security key.** Register your Microsoft-compatible security key and use it along with a PIN for two-step verification or password reset. For step-by-step instructions about how to verify your identity with a security key, see [Set up security info to use a security key](security-info-setup-security-key.md).

- **Email address.** Enter your work or school email address to get an email for password reset. This option isn't available for two-step verification. For step-by-step instructions about how to set up your email, see [Set up security info to use email](security-info-setup-email.md).

- **Security questions.** Answer some security questions created by your administrator for your organization. This option is only available for password reset and not for two-step verification. For step-by-step instructions about how to set up your security questions, see the [Set up security info to use security questions](security-info-setup-questions.md) article.

    >[!Note]
    >If some of these options are missing, it's most likely because your organization doesn't allow those methods. If this is the case, you'll need to choose an available method or contact your administrator for more help.

## <a name="next-steps"></a>Volgende stappen

- Sign-in using the Microsoft Authenticator app, following steps in the [Sign in using two-step verification or security info](security-info-setup-signin.md) article.

- Stel uw wachtwoord opnieuw in als u dit hebt verloren of bent vergeten, uit het [Portal voor wachtwoordherstel](https://passwordreset.microsoftonline.com/) of volg de stappen in het artikel [Uw werk- of schoolaccount wachtwoord opnieuw instellen](active-directory-passwords-update-your-own-password.md).

- Krijg tips voor het oplossen van problemen en hulp met problemen met aanmelden in het artikel [U kunt niet aanmelden bij uw Microsoft-account](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant).
