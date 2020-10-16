---
title: Dash board orders van de partner centrum in de commerciële Marketplace Analytics, Microsoft AppSource en Azure Marketplace
description: Meer informatie over het openen van analytische rapporten over uw commerciële Marketplace-bestellingen in een grafische en download bare indeling.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
ms.date: 07/22/2020
author: mingshen-ms
ms.author: mingshen
ms.openlocfilehash: d5adc1bfe19de48568d0e77bb488bea0e5a02818
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "87327375"
---
# <a name="orders-dashboard-in-commercial-marketplace-analytics"></a>Dashboard Bestellingen in Commerciële marketplace-analyses

Dit artikel bevat informatie over het **dash board orders** in partner centrum. In dit dash board wordt informatie over uw orders weer gegeven in een grafische en download bare indeling.

Als u toegang wilt krijgen tot het **dash board orders** in de hulpprogram ma's van de Partner Center-analyse, opent u het **[dash board analyseren](https://partner.microsoft.com/dashboard/commercial-marketplace/analytics/summary)** onder commerciële Marketplace.

>[!NOTE]
> Zie [Veelgestelde vragen en terminologie voor de analyse van commerciële Marketplace](./faq-terminology.md)voor gedetailleerde definities van analyse terminologie.

## <a name="orders-dashboard"></a>Dashboard voor orders

In het **dash board orders** van het menu **analyseren** worden de huidige orders voor al uw SaaS-aanbiedingen weer gegeven. U kunt grafische weer gaven van de volgende items bekijken:

- [Order overzicht](#order-summary)
- [Orders op geografie](#orders-by-geography)
- [Bestellingen per aanbiedingen](#orders-by-offers)
- [Trend van orders per site versus per seat](#orders-trend-per-site-versus-per-seat)
- [Orders per plan](#orders-by-plans)
- [Trend van orders en stoelen](#orders-and-seats-trend)
- [Tabel Order Details](#order-details-table)

De maximale latentie tussen het maken en rapporteren van orders in het partner centrum is 48 uur.

## <a name="order-dashboard-details"></a>Details van bestel dashboard

In deze sectie worden de analyse rapporten uitvoeriger beschreven.

### <a name="order-summary"></a>Order overzicht

In de sectie Overzicht van de order wordt een telling weer gegeven van alle aangeschafte orders (met uitzonde ring van geannuleerde orders), geannuleerde orders en seats.

De percentage waarde naast totaal aantal orders vertegenwoordigt de hoeveelheid groei van het geselecteerde datum bereik.

![Samen vatting van de bestelling analyseren in Partner Center](./media/order-summary.png)

- Een groene en opwaartse drie hoek duidt op een positieve groei trend.
- Een rode drie hoek die omlaag wijst wijst op een negatieve groei trend ten opzichte van de vorige maand.
- Groei trends worden weer gegeven in micro bar-grafieken. U kunt de waarde voor elke maand weer geven door de muis aanwijzer boven de kolommen in de grafiek te plaatsen.
- Geannuleerde orders zijn het aantal orders dat eerder is aangeschaft en die vervolgens zijn geannuleerd tijdens het geselecteerde datum bereik.
- Stoelen zijn een aantal seats die zijn gemaakt tijdens het geselecteerde datum bereik.

### <a name="orders-by-geography"></a>Orders op geografie

Op basis van het heatmap voor **Orders per regio** wordt een telling weer gegeven van uw orders op een wereld kaart en worden de seats weer gegeven die zijn toegewezen in het land of de regio van de klant. Deze heatmap werkt op dezelfde locatie als de **[klant op geografie heatmap](./customer-dashboard.md#customer-by-geography)**.

![Partner centrum orders per geografie analyseren](./media/orders-by-geography.png)

### <a name="orders-by-offers"></a>Bestellingen per aanbiedingen

Met de **opdrachten orders per biedt** ring grafiek worden de orders (inclusief geannuleerde orders) geordend op basis van de namen van de aanbiedingen.

- De beste aanbiedingen worden weer gegeven in de grafiek en de rest van de aanbiedingen worden als ' rest all ' gegroepeerd.
- U kunt specifieke aanbiedingen in de legenda selecteren om alleen die aanbiedingen in de grafiek weer te geven.
- Als u de muis aanwijzer boven een segment in de grafiek houdt, wordt het aantal orders en percentage van die aanbieding vergeleken met het totale aantal orders in alle aanbiedingen.
- De **trend orders per aanbiedingen** worden weer gegeven per maand trends in groei. De kolom maand vertegenwoordigt het aantal orders per aanbiedings naam. In het lijn diagram wordt de trend van het groei percentage weer gegeven die op een z-as is getekend.
- U kunt de schuif regelaar aan de bovenkant van de grafiek gebruiken om naar rechts en naar links op de x-as te schuiven en te focussen op specifieke gegevens punten.
- U kunt het trendanalyse diagram weer geven door een specifiek item in de legenda te selecteren.
- U kunt er ook voor kiezen om trends en gegevens weer te geven voor **geannuleerde orders**. De grafiek werkt op dezelfde manier als de grafiek **Orders per aanbieding** .

### <a name="orders-trend-per-site-versus-per-seat"></a>Trend van orders per site versus per seat

De grafiek **per locatie versus per seat-** ring vertegenwoordigt de uitsplitsing van de SaaS-orders per locatie en per seat-SaaS die door klanten zijn aangeschaft (deze grafiek bevat geannuleerde orders). Het kolom diagram vertegenwoordigt de trend van per site SaaS en per seat SaaS-orders die door klanten zijn aangeschaft (deze grafiek bevat geannuleerde orders).

### <a name="orders-by-plans"></a>Orders per plan

Het diagram **Orders per abonnementen** vertegenwoordigt de trend van orders op het plan niveau voor al uw aanbiedingen (dit omvat geannuleerde orders). De ring grafiek vertegenwoordigt de uitsplitsing van de bovenste vijf plan orders en het kolom diagram vertegenwoordigt de trend van de orders voor de vijf meest voorkomende plannen.

### <a name="orders-and-seats-trend"></a>Trend van orders en stoelen

In het **trend diagram orders en stoelen** worden de belangrijkste 50 aanbiedingen met het hoogste aantal orders weer gegeven. Deze worden weer gegeven op een opvul bord en worden gesorteerd op het hoogste aantal orders en het percentage van de bestelling.

- **Orders per plan** : Selecteer een aanbieding om de uitsplitsing van het aantal orders voor de vijf beste plannen in de grafiek weer te geven.
- **Stoelen per plan** : de maandelijkse trend van seats voor de vijf tien plannen. Als het aanbod dat u selecteert, geen aanbieding per seat is, worden er geen gegevens weer gegeven.

### <a name="canceled-orders-by-offers"></a>Geannuleerde orders per aanbiedingen

In het cirkel diagram **geannuleerde orders per bieden** worden al uw geannuleerde orders geordend op basis van de namen van de aanbiedingen. De beste aanbiedingen worden weer gegeven in de grafiek en de rest van de aanbiedingen worden gegroepeerd als ' rest all '. U kunt specifieke aanbiedingen in de legenda selecteren om weer te geven in de grafiek.

- Als u de muis aanwijzer boven een segment in de grafiek houdt, wordt het aantal orders en percentage van de geselecteerde aanbieding weer gegeven vergeleken met het totale aantal orders in alle aanbiedingen.
- In het kolom diagram worden maand-per-maand trends weer gegeven. De kolommen vertegenwoordigen het aantal geannuleerde orders per aanbiedings naam. U kunt de schuif regelaar boven op het diagram gebruiken om naar rechts en naar links op de x-as te schuiven en te focussen op specifieke gegevens punten. U kunt het trendanalyse diagram weer geven door een specifiek item in de legenda te selecteren.

### <a name="order-details-table"></a>Tabel Order Details

In de tabel Order Details wordt een genummerde lijst met de 1000 belangrijkste orders gesorteerd op datum van aanschaf.

- Elke kolom in het raster is sorteerbaar.
- De gegevens kunnen worden geëxtraheerd naar een TSV-bestand als het aantal records kleiner is dan 1000.
- Als records een getal van meer dan 1000, worden geëxporteerde gegevens asynchroon in een download pagina voor de volgende 30 dagen geplaatst.
- Filters toep assen op de **tabel Order Details** om alleen de gegevens weer te geven waarin u bent geïnteresseerd. Filteren op land/regio, type Azure-licentie, licentie type voor commerciële Marketplace, type aanbieding, Bestel status, gratis sporen, abonnements-ID van de commerciële Marketplace, klant-ID en bedrijfs naam.
- Omdat SaaS-aanbiedingen zijn gekocht via Azure Marketplace of AppSource, hebt u geen Azure-abonnement nodig. de Marketplace-abonnements-ID wordt weer gegeven als 00000000-0000-0000-0000-000000000000 in de sectie **gedetailleerde order gegevens** .

#### <a name="orders-page-filters"></a>Pagina filters voor orders

Deze filters worden toegepast op pagina niveau.

U kunt meerdere filters selecteren om de grafiek te genereren voor de criteria die u wilt weer geven en de gegevens die u wilt weer gegeven in het raster van **gedetailleerde order gegevens** /exporteren. Filters worden toegepast op de gegevens die zijn geëxtraheerd voor het gegevens bereik dat u hebt geselecteerd in de rechter bovenhoek van de pagina Orders.

- Aanbiedings typen en aanbiedings namen worden alleen weer gegeven voor aanbiedingen waarvoor u orders hebt tijdens het geselecteerde datum bereik. Namen van aanbiedingen in de lijst worden weer gegeven voor aanbiedingen die u hebt geselecteerd in de lijst.
- Toegepaste filters tonen de totale metrieken binnen elke selectie (s) voor elk geselecteerd filter. Toegepaste filters worden niet weer gegeven wanneer de standaard selectie wordt gekozen.
- Als **Alles** is geselecteerd voor een van de vervolg keuzelijsten, worden alle metrische gegevens in de geselecteerde pagina geaggregeerd. Bijvoorbeeld: ' all ' in de optie voor het filter type aanbieding betekent dat alle aanbiedings typen zijn geselecteerd. Dit is de standaard selectie voor de vervolg keuzelijst. In toegepaste filters wordt niets weer gegeven wanneer **Alles** is geselecteerd.
- **Selectie van meerdere waarden**: alle metrische gegevens op de pagina worden geaggregeerd voor alle selecties die zijn gemaakt in de vervolg keuzelijst. Als er meerdere selecties worden gemaakt, wordt in het toegepaste filter het aantal gemaakte selecties weer gegeven. Zie de onderstaande afbeelding voor naslag informatie.

    ![Partner centrum Analyseer de volg orde waarin meerdere waarden zijn toegepast op het filter](./media/filters-applied.png)

- **Selectie van één waarde**: als u een waarde hebt geselecteerd, wordt in het toegepaste filter het aantal weer gegeven van het filter dat u hebt geselecteerd. Zie de onderstaande afbeelding voor naslag informatie.

     ![Partner centrum-order analyseren met één waarde toegepast op filter](./media/filters-applied-single.png)

## <a name="next-steps"></a>Volgende stappen

- Voor een overzicht van analyse rapporten die beschikbaar zijn in de commerciële Marketplace van partner Center raadpleegt u [Analytics voor de commerciële Marketplace in Partner Center](./analytics.md).
- Zie [Summary dash board in Commercial Marketplace Analytics](./summary-dashboard.md)voor grafieken, trends en waarden van statistische gegevens die de Marketplace-activiteiten voor uw aanbieding samenvatten.
- Voor virtuele machine (VM) zijn metrische gegevens over gebruik en gefactureerde facturering, Zie [gebruiks dashboard in commerciële Marketplace-analyses](./usage-dashboard.md).
- Zie [klanten dashboard in Commercial Marketplace Analytics](./customer-dashboard.md)voor gedetailleerde informatie over uw klanten, waaronder groei trends.
- Zie [dash board downloaden in Commercial Marketplace Analytics](./downloads-dashboard.md)voor een lijst met Download aanvragen voor de afgelopen 30 dagen.
- Voor een geconsolideerde weer gave van feedback van klanten voor aanbiedingen op Azure Marketplace en AppSource raadpleegt u het [dash board beoordelingen en beoordelingen in Commercial Marketplace Analytics](./ratings-reviews.md).
- Zie [Veelgestelde vragen en terminologie voor de analyse van commerciële Marketplace](./faq-terminology.md)voor veelgestelde vragen over commerciële Marketplace-analyses en voor een uitgebreid woorden boek met gegevens termen.
