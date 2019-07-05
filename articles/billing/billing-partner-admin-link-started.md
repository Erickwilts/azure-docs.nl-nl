---
title: Een Azure-account aan een partner-ID koppelen | Microsoft Docs
description: Bewaken met Azure-klanten door een partner-ID koppelen aan het gebruikersaccount dat u gebruikt voor het beheren van resources van de klant bijhouden.
services: billing
author: dhirajgandhi
manager: dhgandhi
ms.author: banders
ms.date: 03/12/2019
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.openlocfilehash: 97866a1f1299c028cdc8f86245308ae4a8b5db88
ms.sourcegitcommit: 837dfd2c84a810c75b009d5813ecb67237aaf6b8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2019
ms.locfileid: "67502677"
---
# <a name="link-a-partner-id-to-your-azure-accounts"></a>Een partner-ID koppelen aan uw Azure-accounts

Microsoft-partners bieden services waarmee klanten helpen bedrijfs- en essentiële doelstellingen met behulp van Microsoft-producten. Als u werkt namens de klant beheren, moet configureren en ondersteunen van Azure-services, de gebruikers van partnerorganisaties toegang tot de omgeving van de klant. Via Partner-Admin-koppeling, kunnen partners met de referenties gebruikt voor het leveren van de service de partner network-ID koppelen.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="get-access-from-your-customer"></a>Toegang van uw klant ontvangt

Voordat u uw partner-ID koppelen, moet uw klant u toegang geven tot hun Azure-resources met behulp van een van de volgende opties:

- **Gastgebruiker**: Uw klant kunt u als een gastgebruiker toevoegen en toegang op rollen gebaseerd beheer (RBAC) rollen toewijzen. Zie voor meer informatie, [gastgebruikers toevoegen van een andere directory](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b).

- **Directory-account**: Uw klant kan een gebruikersaccount maken voor u in hun eigen map en alle RBAC-rol toe te wijzen.

- **Service-principal**: Uw klant kunt een app of script toevoegen van uw organisatie in de map en alle RBAC-rol toe te wijzen. De identiteit van de app of script staat bekend als een service-principal.

## <a name="link-to-a-partner-id"></a>Koppeling naar een partner-ID

Als u toegang tot resources van de klant hebt, gebruikt u de Azure-portal, PowerShell of Azure CLI aan uw Microsoft Partner Network-ID (MPN-ID) koppelen aan uw gebruikers-ID of de service-principal. Koppel de partner-ID in de tenant van elke klant.

### <a name="use-the-azure-portal-to-link-to-a-new-partner-id"></a>Azure portal gebruiken om te koppelen aan een nieuwe partner-ID

1. Ga naar [koppeling naar een partner-ID](https://portal.azure.com/#blade/Microsoft_Azure_Billing/managementpartnerblade) in Azure portal.

2. Meld u aan bij Azure Portal.

3. De Microsoft-partner-id invoeren. De partner-ID is de [Microsoft Partner Network](https://partner.microsoft.com/) -ID voor uw organisatie.

   ![Schermafbeelding van koppeling naar een partner-ID](./media/billing-link-partner-id/link-partner-ID.PNG)

4. Als u wilt koppelen een partner-ID voor een andere klant, schakelen de map. Onder **schakelen tussen mappen**, selecteer uw directory.

   ![Schermafbeelding van schakelen tussen mappen](./media/billing-link-partner-id/directory-switcher.png)

### <a name="use-powershell-to-link-to-a-new-partner-id"></a>PowerShell gebruiken om te koppelen aan een nieuwe partner-ID

1. Installeer de [AzureRM.ManagementPartner](https://www.powershellgallery.com/packages/AzureRM.ManagementPartner) PowerShell-module.

2. Aanmelden bij de klanttenant met het gebruikersaccount of de service-principal. Zie voor meer informatie, [Meld u aan met PowerShell](https://docs.microsoft.com/powershell/azure/authenticate-azureps).

   ```azurepowershell-interactive
    C:\> Connect-AzAccount -TenantId XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX
   ```

3. Koppeling naar de nieuwe partner-ID. De partner-ID is de [Microsoft Partner Network](https://partner.microsoft.com/) -ID voor uw organisatie.

    ```azurepowershell-interactive
    C:\> new-AzManagementPartner -PartnerId 12345
    ```

#### <a name="get-the-linked-partner-id"></a>De gekoppelde partner-ID ophalen
```azurepowershell-interactive
C:\> get-AzManagementPartner
```

#### <a name="update-the-linked-partner-id"></a>Bijwerken van de gekoppelde partner-ID
```azurepowershell-interactive
C:\> Update-AzManagementPartner -PartnerId 12345
```
#### <a name="delete-the-linked-partner-id"></a>Verwijder de gekoppelde partner-ID
```azurepowershell-interactive
C:\> remove-AzManagementPartner -PartnerId 12345
```

### <a name="use-the-azure-cli-to-link-to-a-new-partner-id"></a>De Azure CLI gebruiken om te koppelen aan een nieuwe partner-ID
1. Installeer de Azure CLI-extensie.

    ```azurecli-interactive
    C:\ az extension add --name managementpartner
    ```

2. Aanmelden bij de klanttenant met het gebruikersaccount of de service-principal. Zie voor meer informatie, [Meld u aan met de Azure CLI](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest).

    ```azurecli-interactive
    C:\ az login --tenant <tenant>
    ```

3. Koppeling naar de nieuwe partner-ID. De partner-ID is de [Microsoft Partner Network](https://partner.microsoft.com/) -ID voor uw organisatie.

     ```azurecli-interactive
     C:\ az managementpartner create --partner-id 12345
      ```  

#### <a name="get-the-linked-partner-id"></a>De gekoppelde partner-ID ophalen
```azurecli-interactive
C:\ az managementpartner show
```

#### <a name="update-the-linked-partner-id"></a>Bijwerken van de gekoppelde partner-ID
```azurecli-interactive
C:\ az managementpartner update --partner-id 12345
```

#### <a name="delete-the-linked-partner-id"></a>Verwijder de gekoppelde partner-ID
```azurecli-interactive
C:\ az managementpartner delete --partner-id 12345
```

## <a name="next-steps"></a>Volgende stappen

Deelnemen aan de discussie de [Microsoft-Partner-Community](https://aka.ms/PALdiscussion) ontvangen van updates of feedback verzenden.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

**Wie kan de partner-ID koppelen?**

Gebruikers van de partnerorganisatie die een klant Azure-resources beheert, kunt de partner-ID koppelen aan het account.

**Kan een partner-ID worden gewijzigd nadat deze gekoppeld?**

Ja. Een gekoppelde partner-ID kan worden gewijzigd, toegevoegd of verwijderd.

**Wat gebeurt er als een gebruiker een account heeft in meer dan één klanttenant?**

De koppeling tussen de partner-ID en het account wordt gedaan voor elke klanttenant. Koppel de partner-ID in de tenant van elke klant.

**Kunnen andere partners en klanten bewerken of verwijderen van de koppeling naar de partner-ID?**

De koppeling is gekoppeld op het niveau van de gebruiker. U kunt alleen bewerken of verwijderen van de koppeling naar de partner-ID. De klant en andere partners kunnen de koppeling wijzigen in de partner-ID.


**Welke MPN-ID moet ik gebruiken als mijn bedrijf meerdere heeft?**

Locatie-Partneraccounts en bijbehorende MPN-id's mogen worden gebruikt voor het koppelen van de partner-ID.  Meer informatie over [Partneraccounts](https://docs.microsoft.com/partner-center/account-structure)

**Waar vind ik beïnvloed omzet rapportage voor gekoppelde partner-ID**

Cloud productprestaties rapportage is beschikbaar voor partners in de Partner Center op [dashboard voor Beveiligingsinzichten van mijn](https://partner.microsoft.com/membership/reports/myinsights). U moet Partner Admin koppeling selecteren als het koppelingstype partner.

**Waarom zie ik mijn klanten in de rapporten niet?**

U kunt de klant in de rapporten vanwege de volgende redenen niet zien

1. De gekoppelde gebruikersaccount niet beschikt over [rollen gebaseerd toegangsbeheer](https://docs.microsoft.com/azure/role-based-access-control/overview) op elke klant Azure-abonnement of de resource.

2. Het Azure-abonnement waar de gebruiker heeft [rollen gebaseerd toegangsbeheer](https://docs.microsoft.com/azure/role-based-access-control/overview) toegang beschikt niet over het gebruik.

**Partner-ID met Azure Stack werkt is een koppeling?**

Ja, koppelt u uw partner-ID voor Azure Stack.
