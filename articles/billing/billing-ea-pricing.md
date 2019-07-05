---
title: Weergeven en downloaden van uw organisatie Azure-prijzen
description: Informatie over het weergeven en downloaden van de prijzen of schat de kosten met prijzen van uw organisatie.
author: bandersmsft
manager: jureid
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/01/2019
ms.author: banders
ms.custom: seodec18
ms.openlocfilehash: a7f7da197a06dbbb730a7386882e534b8381cf9e
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67491352"
---
# <a name="view-and-download-your-organizations-azure-pricing"></a>Weergeven en downloaden van uw organisatie Azure-prijzen

Azure-klanten met een Azure Enterprise Agreement (EA) of [Microsoft KLANTOVEREENKOMST](#check-your-access-to-a-microsoft-customer-agreement) kunnen weergeven en downloaden van de prijzen in de Azure-portal.

## <a name="ea-pricing"></a>EA-prijzen

Afhankelijk van de voor uw organisatie door de Enterprise-beheerder ingestelde beleid, bieden alleen bepaalde beheerdersrollen toegang tot informatie over uw organisatie EA de prijzen. Zie voor meer informatie, [beheerdersrollen in Azure begrijpen Azure Enterprise overeenkomst](billing-understand-ea-roles.md).

1. Als een Enterprise-beheerder aanmelden bij de [Azure-portal](https://portal.azure.com/).
1. Zoeken naar *kosten Management en facturering*.

   ![Schermafbeelding van zoeken in Azure portal](./media/billing-ea-pricing/portal-cm-billing-search.png)

1. Selecteer onder de factureringsaccount **gebruik en kosten**.

   ![Schermafbeelding van gebruik en de kosten onder facturering](./media/billing-ea-pricing/ea-pricing-usage-charges-nav.png)

1. Selecteer ![schermafdruk van Azure portal search](./media/billing-ea-pricing/download-icon.png) **downloaden** voor de maand.

1. Onder **prijslijst**, selecteer **CSV-bestand downloaden**.

   ![Schermafbeelding van de prijslijst csv knop downloaden](./media/billing-ea-pricing/download-ea-price-sheet.png)

## <a name="microsoft-customer-agreement-pricing"></a>Prijzen voor Microsoft-KLANTOVEREENKOMST

U moet de facturering profieleigenaar, bijdrager, lezer of factuur manager weergeven en downloaden van de prijzen. Zie voor meer informatie over facturering rollen voor Microsoft-klant-overeenkomsten, [facturering profiel rollen en taken](billing-understand-mca-roles.md#billing-profile-roles-and-tasks).

### <a name="download-price-sheets-for-the-current-billing-period"></a>Prijzenoverzichten downloaden voor de huidige factureringsperiode

1. Aanmelden bij de [Azure-portal](https://portal.azure.com).
1. Zoeken naar *kosten Management en facturering*.
1. Selecteer een profiel voor facturering. Afhankelijk van uw toegang moet u mogelijk eerst een factureringsaccount selecteren.
1. In de **overzicht** gebied, vinden de downloadkoppelingen onder de kosten maand tot heden.
1. Selecteer **Azure prijslijst**.
![Schermafbeelding van downloaden uit overzicht](./media/billing-ea-pricing/open-pricing.png)

### <a name="download-price-sheets-for-billed-charges"></a>Prijzenoverzichten voor gefactureerde kosten voor downloaden

1. Aanmelden bij de [Azure-portal](https://portal.azure.com).
1. Zoeken naar *kosten Management en facturering*.
1. Selecteer een profiel voor facturering. Afhankelijk van uw toegang moet u mogelijk eerst een factureringsaccount selecteren.
1. Selecteer **Facturen**.
1. In het raster factuur vindt u de rij van de factuur overeenkomt met de prijslijst die u wilt downloaden.
1. Klik op het weglatingsteken (`...`) aan het einde van de rij.
![Schermafbeelding van het weglatingsteken geselecteerd](./media/billing-ea-pricing/billingprofile-invoicegrid.png)

1. Als u zien van de prijzen voor de services in de geselecteerde factuur wilt, selecteert u **factuur prijslijst**.
1. Als u zien van de prijzen voor alle Azure-services voor de betreffende factureringsperiode wilt, selecteert u **Azure prijslijst**.

![Schermafbeelding van contextmenu met prijzenoverzichten](./media/billing-ea-pricing/contextmenu-pricesheet.png)

## <a name="estimate-costs-with-the-azure-pricing-calculator"></a>Schat de kosten met de prijscalculator van Azure

U kunt ook de prijzen van uw organisatie gebruiken kosten wilt ramen met de prijscalculator van Azure.

1. Ga naar de [prijscalculator van Azure](https://azure.microsoft.com/pricing/calculator).
1. Selecteer in de rechterbovenhoek, **aanmelden**.
1. Onder **programma's en aanbieding** > **Licensing-programma**, selecteer **Enterprise Agreement (EA)** .
1. Onder **programma's en aanbieding** > **geselecteerde overeenkomst**, selecteer **niets geselecteerd**.

    ![Schermafbeelding van de prijslijst csv knop downloaden](./media/billing-ea-pricing/ea-pricing-calculator-estimate.png)

1. Kies de organisatie.
1. Selecteer **Toepassen**.
1. Zoeken en vervolgens producten aan uw offerte toevoegen.
1. Geschatte aangegeven prijzen zijn gebaseerd op de prijzen voor de organisatie die u hebt geselecteerd.

## <a name="check-your-access-to-a-microsoft-customer-agreement"></a>Uw toegang tot een Microsoft-KLANTOVEREENKOMST controleren
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="next-steps"></a>Volgende stappen

Als u een EA-klant bent, ziet:

- [Beheer de toegang tot uw EA-factureringsgegevens voor Azure](billing-manage-access.md)
- [Weergeven en uw Microsoft Azure-factuur downloaden](billing-download-azure-invoice.md)
- [Weergeven en downloaden van uw Microsoft Azure-gebruik en kosten](billing-download-azure-daily-usage.md)
- [Meer informatie over uw factuur voor Enterprise Agreement-klanten](billing-understand-your-bill-ea.md)

Als u een Microsoft-KLANTOVEREENKOMST hebt, Zie:

- [Meer informatie over de voorwaarden in de prijslijst voor een Microsoft-KLANTOVEREENKOMST](billing-mca-understand-pricesheet.md)
- [Weergeven en uw Microsoft Azure-factuur downloaden](billing-download-azure-invoice.md)
- [Weergeven en downloaden van uw Microsoft Azure-gebruik en kosten](billing-download-azure-daily-usage.md)
- [Weergeven en btw-documenten voor uw facturering profiel downloaden](billing-mca-download-tax-document.md)
- [Meer informatie over de kosten op uw profiel facturering factuur](billing-mca-understand-your-bill.md)
