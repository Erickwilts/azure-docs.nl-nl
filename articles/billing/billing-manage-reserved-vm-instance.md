---
title: Azure-reserveringen beheren
description: Informatie over het bereik van abonnement wijzigen en toegang voor Azure-reserveringen beheren.
ms.service: billing
author: bandersmsft
manager: yashesvi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/01/2019
ms.author: banders
ms.openlocfilehash: 89279387b3630ea654070eef671f131ec757d55f
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67491186"
---
# <a name="manage-reservations-for-azure-resources"></a>Reserveringen voor Azure-resources beheren

Nadat u een reservering voor Azure koopt, moet u de reservering van toepassing op een ander abonnement, wie kan de reservering beheren of wijzigen van het bereik van de reservering wijzigen. U kunt ook een reservering in twee reserveringen op sommige van de exemplaren die u hebt aangeschaft van toepassing op een ander abonnement splitsen.

Als u Azure Reserved Virtual Machine Instances hebt gekocht, kunt u de instelling optimaliseren voor de reservering. De reserveringskorting kunt toepassen op virtuele machines in dezelfde reeks of kunt u Datacenter-capaciteit voor een specifieke VM-grootte reserveren.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="reservation-order-and-reservation"></a>Reserveringsorder en reservering

Wanneer u van een reservering koopt, worden twee objecten gemaakt: **Reserveringsorder** en **reservering**.

Op het moment van aankoop heeft een Reserveringsorder één reservering daaronder. Acties zoals splitsen, samenvoegen, gedeeltelijke restitutie of exchange maken nieuwe reserveringen onder de **Reserveringsorder**.

Als u wilt een Reserveringsorder weergeven, gaat u naar **reserveringen** > Selecteer de reservering en klik vervolgens op de **Reserveringsorder-ID**.

![Voorbeeld van details van de reservering met de Reserveringsorder-ID ](./media/billing-manage-reserved-vm-instance/reservation-order-details.png)

Een reservering machtigingen worden overgenomen van de reserveringsorder.

## <a name="change-the-reservation-scope"></a>Het reserveringsbereik wijzigen

 De reserveringskorting is van toepassing op virtuele machines, SQL databases, Azure Cosmos DB of andere bronnen die overeenkomen met uw reservering en uitvoeren in het reserveringsbereik. De context van de facturering is afhankelijk van het abonnement dat u gebruikt de reservering kopen.

Het bereik van een reservering bijwerken:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **alle services** > **reserveringen**.
3. Selecteer de reservering.
4. Selecteer **Instellingen** > **Configuratie**.
5. Het bereik wijzigen.

Als u wijzigt van gedeeld op één scope, kunt u alleen abonnementen waar u de eigenaar bent. Alleen abonnementen binnen dezelfde factureringscontext als de reservering kunnen worden geselecteerd.

Het bereik geldt alleen voor afzonderlijke abonnementen met betalen per gebruik-tarieven (aanbiedingen MS-AZR - 0003P of MS-AZR - 0023P), Enterprise-aanbieding MS-AZR - 0017P of MS-AZR - 0148P of typen voor CSP-abonnementen.

## <a name="add-or-change-users-who-can-manage-a-reservation"></a>Gebruikers toevoegen of wijzigen die een reservering kunnen beheren

U kunt reservering beheer delegeren door personen toe te voegen aan de rollen op de reserveringsorder of de reservering. De persoon die de reserveringsorder en de accountbeheerder wordt geplaatst hebben standaard de rol van eigenaar van de reserveringsorder en de reservering.

U kunt toegang tot de reserveringen orders en reserveringen onafhankelijk van de abonnementen die aan de reserveringskorting beheren. Wanneer u iemand machtigingen voor het beheren van een reserveringsorder of de reservering geeft, geeft geen deze machtiging voor het beheren van het abonnement. Op dezelfde manier als u iemand machtigingen voor het beheren van een abonnement binnen het bereik van de reservering geeft, geeft geen deze rechten voor het beheren van de reserveringsorder of de reservering.

Als u een exchange- of restitutie, moet de gebruiker toegang hebben tot de reserveringsorder. Wanneer iemand machtigingen verlenen, is het raadzaam te machtigen om de volgorde van de reservering, niet de reservering.


Om te delegeren van beheer van toegang voor een reservering:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **alle Services** > **reservering** aan lijst reserveringen waartoe u toegang hebt.
3. Selecteer de reservering die u wilt toegang tot andere gebruikers delegeren.
4. Selecteer **toegangsbeheer (IAM)** .
5. Selecteer **roltoewijzing toevoegen** > **rol** > **eigenaar**. Als u beperkte toegang wilt geven, selecteert u een andere rol.
6. Typ het e-mailadres van de gebruiker die u wilt toevoegen als eigenaar.
7. Selecteer de gebruiker en selecteer vervolgens **Opslaan**.

## <a name="split-a-single-reservation-into-two-reservations"></a>Een enkele reservering in twee reserveringen splitsen

 Nadat u meer dan één exemplaar van de resource binnen een reservering koopt, kunt u exemplaren binnen deze reservering toewijzen aan verschillende abonnementen. Standaard worden alle exemplaren één scope - u één abonnement hebt of gedeeld. Bijvoorbeeld, u hebt gekocht 10 exemplaren van de reservering en het bereik voor het abonnement A. worden opgegeven U kunt nu het bereik voor 7 reserveringen wijzigen in abonnement A en de resterende 3 B. splitsen van een reservering abonnement kunt u exemplaren voor gedetailleerde scopebeheer distribueren. U kunt de toewijzing aan abonnementen vereenvoudigen door te kiezen gedeeld bereik. Maar voor cost management of budgettering doeleinden, kunt u hoeveelheden voor specifieke abonnementen toewijzen.

 U kunt splitsen een reservering in twee reserveringen via PowerShell, CLI, of via de API.

### <a name="split-a-reservation-by-using-powershell"></a>Een reservering splitsen met behulp van PowerShell

1. De reserveringsorder-ID ophalen door het uitvoeren van de volgende opdracht uit:

    ```powershell
    # Get the reservation orders you have access to
    Get-AzReservationOrder
    ```

2. De details van een reservering ophalen:

    ```powershell
    Get-AzReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId b8be062a-fb0a-46c1-808a-5a844714965a
    ```

3. De reservering in twee splitsen en distribueren van de exemplaren:

    ```powershell
    # Split the reservation. The sum of the reservations, the quantity, must equal the total number of instances in the reservation that you're splitting.
    Split-AzReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId b8be062a-fb0a-46c1-808a-5a844714965a -Quantity 3,2
    ```
4. U kunt het bereik bijwerken door de volgende opdracht uit:

    ```powershell
    Update-AzReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId 5257501b-d3e8-449d-a1ab-4879b1863aca -AppliedScopeType Single -AppliedScope /subscriptions/15bb3be0-76d5-491c-8078-61fe3468d414
    ```

## <a name="cancellations-and-exchanges"></a>Annuleringen en uitwisselingen

Afhankelijk van het reserveringstype, is het mogelijk om te annuleren of een reservering wisselen. Zie de annulering en uitwisselingen secties in de volgende onderwerpen voor meer informatie:

- [Vooruitbetalen voor Virtual Machines met Azure Reserved VM Instances](..//virtual-machines/windows/prepay-reserved-vm-instances.md#cancellations-and-exchanges)
- [Vooruitbetalen voor SUSE-software-abonnementen vanuit Azure Reservations](../virtual-machines/linux/prepay-suse-software-charges.md#cancellation-and-exchanges-not-allowed)
- [Vooruitbetalen voor compute-resources van SQL Database met gereserveerde capaciteit voor Azure SQL Database](../sql-database/sql-database-reserved-capacity.md#cancellations-and-exchanges)

## <a name="change-optimize-setting-for-reserved-vm-instances"></a>Wijziging instelling voor gereserveerde VM-instanties optimaliseren

 Wanneer u een gereserveerde VM-instantie koopt, kiest u de grootte van instantieflexibiliteit of capaciteitsprioriteit. Grootte-instantieflexibiliteit past de reserveringskorting op andere virtuele machines in dezelfde [groep voor VM-grootte](https://aka.ms/RIVMGroups). Capaciteitsprioriteit bepaalt de volgorde van Datacenter-capaciteit voor uw implementaties. Deze optie biedt extra vertrouwen ontstaat in uw vermogen om te starten van de VM-exemplaren, wanneer u ze nodig hebt.

Wanneer het bereik van de reservering wordt gedeeld, is de flexibiliteit van de grootte van exemplaar op. De capaciteit van het datacenter wordt niet prioriteit voor VM-implementaties.

U kunt de reservering voor capaciteitsprioriteit in plaats van de VM-instantieflexibiliteit grootte optimaliseren voor reserveringen waar het bereik één is.

De instelling optimaliseren voor de reservering bijwerken:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **alle Services** > **reserveringen**.
3. Selecteer de reservering.
4. Selecteer **Instellingen** > **Configuratie**.
5. Wijzig de **optimaliseren voor** instelling.

## <a name="need-help-contact-us"></a>Hulp nodig? Neem contact met ons op.

Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Azure-reserveringen, de volgende artikelen:

- [Wat zijn de reserveringen voor Azure?](billing-save-compute-costs-reservations.md)

Een service-abonnement kopen:
- [Vooruitbetalen voor Virtual Machines met Azure Reserved VM Instances](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Vooruitbetalen voor compute-resources van SQL Database met gereserveerde capaciteit voor Azure SQL Database](../sql-database/sql-database-reserved-capacity.md)
- [Betaal vooruit voor Azure Cosmos DB-resources met Azure Cosmos DB gereserveerde capaciteit](../cosmos-db/cosmos-db-reserved-capacity.md)

Koop een softwareabonnement:
- [Betaal vooruit voor Red Hat software plannen uit de Azure-reserveringen](../virtual-machines/linux/prepay-rhel-software-charges.md)
- [Vooruitbetalen voor SUSE-software-abonnementen vanuit Azure Reservations](../virtual-machines/linux/prepay-suse-software-charges.md)

Korting en het gebruik van begrijpen:
- [Begrijpen hoe de VM-reserveringskorting wordt toegepast](billing-understand-vm-reservation-charges.md)
- [Begrijpen hoe de korting voor Red Hat Enterprise Linux-software-plan wordt toegepast](../billing/billing-understand-rhel-reservation-charges.md)
- [Begrijpen hoe de korting voor SUSE Linux Enterprise software plan wordt toegepast](../billing/billing-understand-suse-reservation-charges.md)
- [Begrijpen hoe andere reservering kortingen zijn toegepast](billing-understand-reservation-charges.md)
- [Gebruik van de reservering voor uw abonnement op gebruiksbasis begrijpen](billing-understand-reserved-instance-usage.md)
- [Inzicht in gebruik van de reservering voor uw Enterprise-inschrijving](billing-understand-reserved-instance-usage-ea.md)
- [Kosten van de Windows-software is niet opgenomen in de reserveringen](billing-reserved-instance-windows-software-costs.md)
