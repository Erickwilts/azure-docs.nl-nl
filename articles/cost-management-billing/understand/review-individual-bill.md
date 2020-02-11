---
title: Inzicht in uw Azure-factuur
description: Lees hier hoe uw factuur en resourcegebruik zijn opgebouwd en hoe u de kosten voor uw afzonderlijke Azure-abonnement controleert.
author: bandersmsft
manager: dougeby
tags: billing
ms.service: cost-management-billing
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/20/2019
ms.author: banders
ms.openlocfilehash: 1249758e5dd5b7e823c11890e57549195da22529
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2020
ms.locfileid: "75987346"
---
# <a name="tutorial-review-your-individual-azure-bill"></a>Zelfstudie: Inzicht in uw Azure-factuur

Dit artikel helpt u om uw Azure-factuur te begrijpen en controleren. Voor elke factureringsperiode ontvangt u normaal gesproken een factuur per e-mail. De factuur is een afspiegeling van de kosten die u hebt gemaakt. De kostengegevens op de factuur zijn ook beschikbaar in de Azure-portal. In deze zelfstudie vergelijkt u uw factuur met het bestand met gedetailleerde dagelijkse gebruiksgegevens en met de kostenanalyse in de Azure-portal.

Deze zelfstudie is alleen van toepassing op Azure-klanten met een individueel abonnement. Bekende voorbeelden van individuele abonnementen zijn abonnementen van het type betalen per gebruik die rechtstreeks zijn gekocht via de Azure-website.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Gefactureerde kosten vergelijken met het gebruiksbestand
> * Kosten en gebruik vergelijken in Kostenanalyse

## <a name="prerequisites"></a>Vereisten

U moet een betaalde factureringsaccount voor het *Microsoft Online Services-programma* hebben. Het account wordt gemaakt wanneer u zich aanmeldt voor Azure via de Azure-website, bijvoorbeeld als u een [account hebt met betalen naar gebruik-tarieven](https://azure.microsoft.com/offers/ms-azr-0003p/) of als u een [Visual Studio-abonnee](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/) bent.

Facturen voor [gratis Azure-accounts](https://azure.microsoft.com/offers/ms-azr-0044p/) worden alleen gemaakt wanneer het maandelijkse tegoed is overschreden.

Het moet meer dan 30 dagen geleden zijn dat u zich geabonneerd hebt op Azure. Azure factureert u aan het einde van uw factureringsperiode.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

- Meld u aan bij Azure Portal op https://portal.azure.com.

## <a name="compare-invoiced-charges-with-usage-file"></a>Gefactureerde kosten vergelijken met het gebruiksbestand

<a name="charges"></a>

De eerste stap voor het vergelijken van gebruik en kosten is het downloaden van uw factuur- en gebruiksbestanden. In het uitgebreide CSV-gebruiksbestand worden uw kosten weergegeven per factureringsperiode en dagelijks gebruik. Het bevat geen belastinginformatie. Als u de bestanden wilt downloaden, moet u een accountbeheerder zijn of de rol van eigenaar hebben.

Typ in de Azure-portal *abonnementen* in het zoekvak en klik vervolgens op [Abonnementen](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).

![Naar Abonnementen navigeren](./media/review-individual-bill/navigate-subscriptions.png)

Klik in de lijst met abonnementen op het abonnement.

Klik onder **Facturering** op **Facturen**.

Zoek in de lijst met facturen naar de factuur die u wilt downloaden en klik vervolgens op het downloadsymbool. Mogelijk moet u de periode wijzigen om oudere facturen weer te geven. Het kan enkele minuten duren voordat het bestand met gebruiksgegevens en de factuur zijn gegenereerd.

![Schermopname van de factureringsperioden, de downloadoptie en de totale kosten voor elke factureringsperiode](./media/review-individual-bill/download-invoice.png)

Klik in het venster Gebruik + kosten downloaden op **CSV-bestand downloaden** en **Factuur downloaden**.

![Schermopname met de pagina voor factuur en gebruik downloaden](./media/review-individual-bill/usageandinvoice.png)

Als de melding **Niet beschikbaar** verschijnt, zijn er verschillende redenen waarom u geen gebruiksgegevens of factuur ziet:

- Het is minder dan 30 dagen geleden dat u zich geabonneerd hebt op Azure.
- Er is geen gebruik voor de factureringsperiode.
- Er is nog geen factuur gegenereerd. Wacht tot het einde van de factureringsperiode.
- U bent niet gemachtigd om facturen te bekijken. U ziet mogelijk geen oude facturen tenzij u de accountbeheerder bent. Zie [Toegang tot factureringsgegevens voor Azure beheren](../manage/manage-billing-access.md) voor meer informatie over het verkrijgen van toegang tot factureringsgegevens.
- Als u een gratis proefversie gebruikt of een maandelijks tegoed met uw abonnement dat u niet overschrijdt, ontvangt u geen factuur tenzij u een Microsoft-klantovereenkomst hebt.

Vervolgens controleert u de kosten. De factuur bevat waarden voor belastingen en uw gebruikskosten.

![Voorbeeld van Azure-factuur](./media/review-individual-bill/invoice-usage-charge.png)

Open het CSV-bestand met gebruiksgegevens dat u hebt gedownload. Tel aan het einde van het bestand de waarde op voor alle items in de kolom *Cost*.

![Voorbeeld van gebruiksbestand met opgetelde kosten](./media/review-individual-bill/usage-file-usage-charges.png)

 De opgetelde waarde voor *Cost* moet exact overeenkomen met het bedrag bij *Gebruikskosten* op uw factuur.

Uw gebruikskosten worden weergegeven op meterniveau. De volgende termen betekenen hetzelfde in de factuur en het gedetailleerde gebruiksbestand. De factureringscyclus op de factuur is bijvoorbeeld hetzelfde als de factureringsperiode die wordt weergegeven in het gedetailleerde gebruiksbestand.

| Factuur (PDF) | Gedetailleerd gebruik (CSV)|
| --- | --- |
|Factureringscyclus | BillingPeriodStartDate BillingPeriodEndDate |
|Naam |De categorie van de meter |
|Type |Subcategorie van de meter |
|Resource |MeterName |
|Regio |MeterRegion |
|Verbruikt | Aantal |
|Inbegrepen |Inbegrepen hoeveelheid |
|Factureerbaar |Overschreden hoeveelheid |
|Tarief | EffectivePrice|
| Waarde | Kosten |

In het gedeelte **Gebruikskosten** van uw factuur ziet u de totale waarde (kosten) voor elke meter die tijdens uw factureringsperiode is verbruikt. In de volgende afbeelding ziet u bijvoorbeeld gebruikskosten voor de Azure Storage-service voor de resource *P10 Disks*.

![Factuur gebruikskosten](./media/review-individual-bill/invoice-usage-charges.png)

Filter in uw CSV-gebruiksbestand op *MeterName* voor de bijbehorende resource die op uw factuur wordt weergegeven. Tel vervolgens de waarde van *Cost* op voor items in die kolom. Hier volgt een voorbeeld.

![Gebruiksbestand met opgetelde waarde voor MeterName](./media/review-individual-bill/usage-file-usage-charge-resource.png)

De opgetelde waarde voor *Cost* moet exact overeenkomen met het bedrag bij *Gebruikskosten* voor de betreffende resource op uw factuur.

Zie [De voorwaarden op uw Microsoft Azure-factuur begrijpen](understand-invoice.md) en [Meer informatie over de gebruiksvoorwaarden in uw bestand voor Azure-gebruik en -kosten](understand-usage.md) voor meer informatie.

## <a name="compare-charges-and-usage-in-cost-analysis"></a>Kosten en gebruik vergelijken in Kostenanalyse

Een kostenanalyse in de Azure-portal kan ook helpen om uw kosten te controleren. Als u een beknopt overzicht wilt van het gefactureerde gebruik en de bijbehorende kosten, selecteert u uw abonnement op de pagina [Abonnementen](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) in de Azure-portal. Klik vervolgens op **Kostenanalyse** en klik daarna in de lijst met weergaven op **Factuurgegevens**.

![Voorbeeld met selectie van Factuurgegevens](./media/review-individual-bill/cost-analysis-select-invoice-details.png)

Selecteer vervolgens in de lijst met het datumbereik een periode voor de factuur. Voeg een filter voor het factuurnummer toe selecteer vervolgens het InvoiceNumber dat overeenkomt met het nummer op uw factuur. In Kostenanalyse worden de kostengegevens voor de gefactureerde items weergegeven.

![Voorbeeld van gegevens van gefactureerde in Kostenanalyse](./media/review-individual-bill/cost-analysis-service-usage-charges.png)

De kosten die worden weergegeven in Kostenanalyse moeten exact overeenkomen met het bedrag bij *Gebruikskosten* voor de betreffende resource op uw factuur.

![Factuur gebruikskosten](./media/review-individual-bill/invoice-usage-charges.png)

## <a name="external"></a>Externe services worden afzonderlijk gefactureerd

Externe services of Marketplace-kosten zijn voor resources die zijn gemaakt door externe softwareleveranciers. Deze resources zijn beschikbaar voor gebruik via Azure Marketplace. Een Barracuda-firewall is bijvoorbeeld een Azure Marketplace-resource die wordt aangeboden door een externe partij. Alle kosten voor de firewall en de bijbehorende meters worden als externe servicekosten weergegeven.

Externe servicekosten worden afzonderlijk in rekening gebracht. Deze kosten worden niet weergegeven op uw Azure-factuur. Raadpleeg voor meer informatie [Inzicht in Azure-facturering voor externe servicekosten](understand-azure-marketplace-charges.md).

### <a name="resources-billed-by-usage-meters"></a>Resources die worden gefactureerd op basis van gebruiksmeters

Azure factureert niet rechtstreeks op basis van de resourcekosten. Kosten voor een resource worden berekend met behulp van een of meer meters. Meters worden gebruikt om het gebruik van een resource bij te houden gedurende de levensduur. Deze meters worden vervolgens gebruikt om de factuur te berekenen.

Wanneer u één Azure-resource maakt, zoals een virtuele machine, worden er een of meer meterexemplaren gemaakt. Meters worden gebruikt om het gebruik van de resource gedurende een periode bij te houden. Elke meter verzendt gebruiksrecords die door Azure worden gebruikt om de factuur te berekenen.

Een virtuele machine (VM) die in Azure is gemaakt, kan bijvoorbeeld de volgende meters hebben om het gebruik bij te houden:

- Rekenuren
- Uren IP-adres
- Inkomende gegevensoverdracht
- Uitgaande gegevensoverdracht
- Standard - Beheerde schijf
- Standard - Beheerde schijfbewerkingen
- Standard - IO - Schijf
- Standard - IO - Lezen blok-blob
- Standard - IO - Schrijven blok-blob
- Standard - IO - Verwijderen blok-blob

Wanneer de virtuele machine wordt gemaakt, begint elke meter gebruiksrecords te verzenden. Dit gebruik en de metertarieven worden bijgehouden in het Azure-metersysteem.

In het CSV-gebruiksbestand kunt u de meters zien die zijn gebruikt voor het berekenen van uw factuur.

## <a name="payment"></a>Uw factuur betalen

Als u een creditcard hebt ingesteld als uw betalingswijze, wordt het bedrag automatisch binnen 10 dagen na het einde van de factureringsperiode in rekening gebracht. Op uw creditcardoverzicht wordt het regelitem weergegeven als **MSFT Azure**.

Zie [Een creditcard toevoegen, bijwerken of verwijderen voor Azure](../manage/change-credit-card.md) als u de creditcard die wordt gebruikt, wilt wijzigen.

Als u [betaalt per factuur](../manage/pay-by-invoice.md), verstuurt u uw betaling met de gegevens die onder aan uw factuur worden weergegeven.

Als u de status van uw betaling wilt controleren, [maakt u een ondersteuningsticket](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Gefactureerde kosten vergelijken met het gebruiksbestand
> * Kosten en gebruik vergelijken in Kostenanalyse

Voltooi de quickstart om aan de slag te gaan met Kostenanalyse.

> [!div class="nextstepaction"]
> [Beginnen met kosten analyseren](../costs/quick-acm-cost-analysis.md)
