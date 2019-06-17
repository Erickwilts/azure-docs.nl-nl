---
title: Een Snelzoekgids voor in de tegels Designer bekijken in Azure Monitor | Microsoft Docs
description: Met behulp van Designer bekijken in Azure Monitor, kunt u aangepaste weergaven die worden weergegeven in de Azure-portal en bevatten een aantal visualisaties op de gegevens in de Log Analytics-werkruimte maken. In dit artikel is een referentie-handleiding voor de instellingen voor de tegels die beschikbaar in uw aangepaste weergaven zijn.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: 41787c8f-6c13-4520-b0d3-5d3d84fcf142
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/17/2018
ms.author: bwren
ms.openlocfilehash: 9c0283081bd7245b1b886ed82ba03130a7a3bf2c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "61341740"
---
# <a name="reference-guide-to-view-designer-tiles-in-azure-monitor"></a>Naslaggids voor Weergaveontwerper tegels in Azure Monitor
Met behulp van Designer bekijken in Azure Monitor, kunt u tal van aangepaste weergaven maken in Azure portal waarmee u gegevens visualiseren in uw Log Analytics-werkruimte kunt. In dit artikel is een referentie-handleiding voor de instellingen voor de tegels die beschikbaar in uw aangepaste weergaven zijn.

Zie voor meer informatie over Designer bekijken:

* [Designer weergeven](view-designer.md): Biedt een overzicht van de Weergaveontwerper en procedures voor het maken en bewerken van aangepaste weergaven.
* [Naslaginformatie voor visualisatieonderdelen](view-designer-parts.md): Biedt een Naslaggids voor de instellingen voor de visualisatie-onderdelen die beschikbaar in uw aangepaste weergaven zijn.


De beschikbare Weergaveontwerper-tegels worden in de volgende tabel beschreven:  

| tegel | Description |
|:--- |:--- |
| [Number](#number-tile) |Het aantal records uit een query. |
| [Twee getallen](#two-numbers-tile) |Het aantal records uit twee verschillende query's. |
| [ringdiagram](#donut-tile) | Een grafiek die gebaseerd op een query, met een totale waarde in het midden. |
| Lijndiagram en bijschrift | Een lijndiagram dat gebaseerd op een query en een toelichting bij een totale waarde. |
| [Lijndiagram](#line-chart-tile) |Een lijndiagram dat gebaseerd op een query. |
| [Twee tijdlijnen](#two-timelines-tile) | Een kolomdiagram met twee reeksen, elk op basis van een afzonderlijke query te maken. |

De volgende secties beschrijven de tegeltypen en de bijbehorende eigenschappen in detail.

> [!NOTE]
> Tegels in de weergaven zijn gebaseerd op [query's bijgehouden](../log-query/log-query-overview.md) in uw Log Analytics-werkruimte. Op dit moment niet wordt ondersteund [cross-query's voor resource](../log-query/cross-workspace-query.md) voor het ophalen van gegevens uit Application Insights.

## <a name="number-tile"></a>Een tegel getal
De **getal** tegel zowel het aantal records uit een logbestand-query en een label wordt weergegeven.

![Een tegel getal](media/view-designer-tiles/tile-number.png)

| Instelling | Description |
|:--- |:--- |
| Name |De tekst die wordt weergegeven aan de bovenkant van de tegel. |
| Description |De tekst die wordt weergegeven onder de naam van de tegel. |
| **tegel** | |
| Legenda |De tekst die wordt weergegeven onder de waarde. |
| Query’s uitvoeren |De query die wordt uitgevoerd. Het aantal van de records die worden geretourneerd door de query wordt weergegeven. |
| **Geavanceerde** |**> Gegevensstroom niet verifiëren** |
| Enabled |Selecteer deze koppeling als gegevensstroom niet verifiëren voor de tegel moet worden ingeschakeld. Deze benadering biedt een alternatieve bericht als gegevens niet beschikbaar is. U normaal gesproken benadering gebruiken voor een bericht in de tijdelijke periode wanneer de weergave is geïnstalleerd en de gegevens beschikbaar. |
| Query’s uitvoeren |De query die wordt uitgevoerd om te bepalen of gegevens beschikbaar voor de weergave zijn. Als de query geen resultaten oplevert, wordt een bericht weergegeven in plaats van de waarde van de hoofdquery. |
| Message |Het bericht dat wordt weergegeven als de gegevens voor de gegevensstroom verificatie-query geen gegevens retourneert. Als u geen bericht, bieden een *evaluatie uitvoeren* statusbericht wordt weergegeven. |


## <a name="two-numbers-tile"></a>Twee getallen tegel
Deze tegel toont het aantal records uit twee verschillende logboeken-query's en een label voor elke.

![Twee getallen tegel](media/view-designer-tiles/tile-two-numbers.png)

| Instelling | Description |
|:--- |:--- |
| Name |De tekst die wordt weergegeven aan de bovenkant van de tegel. |
| Description |De tekst die wordt weergegeven onder de naam van de tegel. |
| **Eerste tegel** | |
| Legenda |De tekst die wordt weergegeven onder de waarde. |
| Query’s uitvoeren |De query die wordt uitgevoerd. Het aantal van de records die worden geretourneerd door de query wordt weergegeven. |
| **Tweede tegel** | |
| Legenda |De tekst die wordt weergegeven onder de waarde. |
| Query’s uitvoeren |De query die wordt uitgevoerd. Het aantal van de records die worden geretourneerd door de query wordt weergegeven. |
| **Geavanceerde** |**> Gegevensstroom niet verifiëren** |
| Enabled |Selecteer deze koppeling als gegevensstroom niet verifiëren voor de tegel moet worden ingeschakeld. Deze benadering biedt een alternatieve bericht als gegevens niet beschikbaar is. U normaal gesproken benadering gebruiken voor een bericht in de tijdelijke periode wanneer de weergave is geïnstalleerd en de gegevens beschikbaar. |
| Query’s uitvoeren |De query die wordt uitgevoerd om te bepalen of gegevens beschikbaar voor de weergave zijn. Als de query geen resultaten oplevert, wordt een bericht weergegeven in plaats van de waarde van de hoofdquery. |
| Message |Het bericht dat wordt weergegeven als de gegevens voor de gegevensstroom verificatie-query geen gegevens retourneert. Als u geen bericht, bieden een *evaluatie uitvoeren* statusbericht wordt weergegeven. |


## <a name="donut-tile"></a>Ring-tegel
De **ring** tegel geeft één getal met een overzicht van de kolom van een waarde in een logboekquery. De Ring worden de resultaten van de top drie records grafisch weergegeven.

![Ring-tegel](media/view-designer-tiles/tile-donut.png)

| Instelling | Description |
|:--- |:--- |
| Name |De tekst die wordt weergegeven aan de bovenkant van de tegel. |
| Description |De tekst die wordt weergegeven onder de naam van de tegel. |
| **ringdiagram** | |
| Query’s uitvoeren |De query die wordt uitgevoerd voor de ring. De eerste eigenschap een tekstwaarde is en de tweede eigenschap is een numerieke waarde. Deze query gebruikt normaal gesproken de *meting* trefwoord om samen te vatten resultaten. |
| **ringdiagram** |**> Center** |
| Text |De tekst die wordt weergegeven onder de waarde binnen de ring. |
| Bewerking |De bewerking die wordt uitgevoerd op de eigenschap value om samen te vatten het als één waarde.<ul><li>Som: De waarden van alle records met de waarde van eigenschap toevoegen.</li><li>Percentage: Percentage van de opgetelde waarden van records met de waarde van de eigenschap in vergelijking met de opgetelde waarden van alle records.</li></ul> |
| Resultaatwaarden gebruikt in middenbewerking |(Optioneel) Selecteer het plusteken (+) om toe te voegen een of meer waarden. De resultaten van de query zijn beperkt tot records met de eigenschapswaarden die u opgeeft. Als er geen waarden worden toegevoegd, worden alle records worden opgenomen in de query. |
| **ringdiagram** |**> Aanvullende opties** |
| Kleuren |De kleur die wordt weergegeven voor elk van de drie belangrijkste eigenschappen. Gebruik het opgeven van alternatieve kleuren voor specifieke eigenschapswaarden *geavanceerde kleur toewijzing*. |
| Geavanceerde kleurtoewijzing |Geeft een kleur die specifieke eigenschapswaarden vertegenwoordigt. Als de waarde die u opgeeft in de rechterbovenhoek drie is, wordt de alternatieve kleur weergegeven in plaats van de standaard kleur. Als de eigenschap niet in de rechterbovenhoek drie is, wordt de kleur niet weergegeven. |
| **Geavanceerde** |**> Gegevensstroom niet verifiëren** |
| Enabled |Selecteer deze koppeling als gegevensstroom niet verifiëren voor de tegel moet worden ingeschakeld. Deze benadering biedt een alternatieve bericht als gegevens niet beschikbaar is. U normaal gesproken benadering gebruiken voor een bericht in de tijdelijke periode wanneer de weergave is geïnstalleerd en de gegevens beschikbaar. |
| Query’s uitvoeren |De query die wordt uitgevoerd om te bepalen of gegevens beschikbaar voor de weergave zijn. Als de query geen resultaten oplevert, wordt een bericht weergegeven in plaats van de waarde van de hoofdquery. |
| Message |Het bericht dat wordt weergegeven als de gegevens voor de gegevensstroom verificatie-query geen gegevens retourneert. Als u geen bericht, bieden een *evaluatie uitvoeren* statusbericht wordt weergegeven. |


## <a name="line-chart-tile"></a>Regel grafiek tegel
Deze tegel is een lijndiagram met meerdere reeksen uit een logboekquery na verloop van tijd. 

![Regel grafiek en een bijschrift tegel](media/view-designer-tiles/tile-line-chart.png)

| Instelling | Description |
|:--- |:--- |
| Name |De tekst die wordt weergegeven aan de bovenkant van de tegel. |
| Description |De tekst die wordt weergegeven onder de naam van de tegel. |
| **Lijndiagram** | |
| Query’s uitvoeren |De query die wordt uitgevoerd voor het lijndiagram. De eerste eigenschap een tekstwaarde is en de tweede eigenschap is een numerieke waarde. Deze query gebruikt normaal gesproken de *meting* trefwoord om samen te vatten resultaten. Als de query gebruikt de *interval* de x-as-trefwoord wordt gebruikt voor dit tijdsinterval. Als de query geen gebruik maakt van de *interval* #trefwoord, de x-as wordt per uur intervallen. |
| **Lijndiagram** |**> Y-as** |
| Logaritmische schaal gebruiken |Selecteer deze koppeling naar een logaritmische schaal gebruiken voor de y-as. |
| Eenheden |Geef de eenheden voor de waarden die zijn geretourneerd door de query. Deze informatie wordt gebruikt om de labels worden weergegeven in de grafiek die aangeeft welke waarde en eventueel om de waarden te converteren. De **eenheidstype** Hiermee geeft u de categorie van de eenheid en definieert de **huidige eenheidstype** waarden die beschikbaar zijn. Als u een waarde in **converteren naar** en vervolgens de numerieke waarden worden geconverteerd van de **huidige eenheid** type de **converteren naar** type. |
| Aangepast label |De tekst die wordt weergegeven voor de y-as naast het label voor de *eenheid* type. Als er geen label is opgegeven, alleen de *eenheid* type weergegeven. |
| **Geavanceerde** |**> Gegevensstroom niet verifiëren** |
| Enabled |Selecteer deze koppeling als gegevensstroom niet verifiëren voor de tegel moet worden ingeschakeld. Deze benadering biedt een alternatieve bericht als gegevens niet beschikbaar is. U normaal gesproken benadering gebruiken voor een bericht in de tijdelijke periode wanneer de weergave is geïnstalleerd en de gegevens beschikbaar. |
| Query’s uitvoeren |De query die wordt uitgevoerd om te bepalen of gegevens beschikbaar voor de weergave zijn. Als de query geen resultaten oplevert, wordt een bericht weergegeven in plaats van de waarde van de hoofdquery. |
| Message |Het bericht dat wordt weergegeven als de gegevens voor de gegevensstroom verificatie-query geen gegevens retourneert. Als u geen bericht, bieden een *evaluatie uitvoeren* statusbericht wordt weergegeven. |


## <a name="line-chart-and-callout-tile"></a>Regel grafiek en een bijschrift tegel
Deze tegel is zowel een regel die meerdere reeksen uit een logbestand query's uitvoeren voor tijd en een toelichting met de samengevatte waarde weergegeven grafiek. 

![Regel grafiek en een bijschrift tegel](media/view-designer-tiles/tile-line-chart-callout.png)

| Instelling | Description |
|:--- |:--- |
| Name |De tekst die wordt weergegeven aan de bovenkant van de tegel. |
| Description |De tekst die wordt weergegeven onder de naam van de tegel. |
| **Lijndiagram** | |
| Query’s uitvoeren |De query die wordt uitgevoerd voor het lijndiagram. De eerste eigenschap een tekstwaarde is en de tweede eigenschap is een numerieke waarde. Deze query gebruikt normaal gesproken de *meting* trefwoord om samen te vatten resultaten. Als de query gebruikt de *interval* de x-as-trefwoord wordt gebruikt voor dit tijdsinterval. Als de query geen gebruik maakt van de *interval* #trefwoord, de x-as wordt per uur intervallen. |
| **Lijndiagram** |**> Bijschrift** |
| Titel van bijschrift | De tekst die wordt weergegeven boven de waarde van bijschrift. |
| Reeksnaam |De eigenschapswaarde voor de reeks moet worden gebruikt als de waarde van bijschrift. Als er geen reeks is opgegeven, worden alle records van de query worden gebruikt. |
| Bewerking |De bewerking die wordt uitgevoerd op de eigenschap value om samen te vatten het als één waarde voor de toelichting.<ul><li>Gemiddelde: Het gemiddelde van de waarden van alle records.</li><li>Aantal: De telling van alle records die worden geretourneerd door de query.</li><li>Laatste voorbeeld: De waarde van het afgelopen interval dat opgenomen in de grafiek.</li><li>Max.: De maximale waarde van de tijden die zijn opgenomen in de grafiek.</li><li>Min: De minimumwaarde van de tijden die zijn opgenomen in de grafiek.</li><li>Som: De som van de waarden van alle records.</li></ul> |
| **Lijndiagram** |**> Y-as** |
| Logaritmische schaal gebruiken |Selecteer deze koppeling naar een logaritmische schaal gebruiken voor de y-as. |
| Eenheden |Geef de eenheden voor de waarden moeten worden geretourneerd door de query. Deze informatie wordt gebruikt op gegevenslabels weergegeven die wijzen op de typen en, optioneel, om te converteren van de waarden. De *eenheid* type Hiermee geeft u de categorie van de eenheid en definieert de beschikbare *huidige eenheid* waarden. Als u een waarde in *converteren naar*, de numerieke waarden worden geconverteerd van de *huidige eenheid* type de *converteren naar* type. |
| Aangepast label |De tekst die wordt weergegeven voor de y-as naast het label voor de *eenheid* type. Als er geen label is opgegeven, alleen de *eenheid* type weergegeven. |
| **Geavanceerde** |**> Gegevensstroom niet verifiëren** |
| Enabled |Selecteer deze koppeling als gegevensstroom niet verifiëren voor de tegel moet worden ingeschakeld. Deze benadering biedt een alternatieve bericht als gegevens niet beschikbaar is. U normaal gesproken benadering gebruiken voor een bericht in de tijdelijke periode wanneer de weergave is geïnstalleerd en de gegevens beschikbaar. |
| Query’s uitvoeren |De query die wordt uitgevoerd om te bepalen of gegevens beschikbaar voor de weergave zijn. Als de query geen resultaten oplevert, wordt een bericht weergegeven in plaats van de waarde van de hoofdquery. |
| Message |Het bericht dat wordt weergegeven als de gegevens voor de gegevensstroom verificatie-query geen gegevens retourneert. Als u geen bericht, bieden een *evaluatie uitvoeren* statusbericht wordt weergegeven. |


## <a name="two-timelines-tile"></a>Twee tijdlijnen tegel
De **twee tijdlijnen** tegel toont de resultaten van de twee logboeken-query's na verloop van tijd als kolomdiagrammen. Een bijschrift wordt weergegeven voor elke reeks. 

![Twee tijdlijnen tegel](media/view-designer-tiles/tile-two-timelines.png)

| Instelling | Description |
|:--- |:--- |
| Name |De tekst die wordt weergegeven aan de bovenkant van de tegel. |
| Description |De tekst die wordt weergegeven onder de naam van de tegel. |
| Eerste grafiek | |
| Legenda |De tekst die wordt weergegeven onder het bijschrift voor de eerste reeks. |
| Kleur |De kleur die wordt gebruikt voor de kolommen in de eerste reeks. |
| Grafiekquery |De query die wordt uitgevoerd voor de eerste reeks. Het aantal records via elk tijdsinterval wordt vertegenwoordigd door de grafiekkolommen. |
| Bewerking |De bewerking die wordt uitgevoerd op de eigenschap value om samen te vatten het als één waarde voor de toelichting.<ul><li>Gemiddelde: Het gemiddelde van de waarden van alle records.</li><li>Aantal: De telling van alle records die worden geretourneerd door de query.</li><li>Laatste voorbeeld: De waarde van het afgelopen interval dat opgenomen in de grafiek.</li><li>Max.: De maximale waarde van de tijden die zijn opgenomen in de grafiek.</li></ul> |
| **Tweede grafiek** | |
| Legenda |De tekst die wordt weergegeven onder het bijschrift voor de tweede reeks. |
| Kleur |De kleur die wordt gebruikt voor de kolommen in de tweede reeks. |
| Grafiekquery |De query die wordt uitgevoerd voor de tweede reeks. Het aantal records via elk tijdsinterval wordt vertegenwoordigd door de grafiekkolommen. |
| Bewerking |De bewerking die wordt uitgevoerd op de eigenschap value om samen te vatten het als één waarde voor de toelichting.<ul><li>Gemiddelde: Het gemiddelde van de waarden van alle records.</li><li>Aantal: De telling van alle records die worden geretourneerd door de query.</li><li>Laatste voorbeeld: De waarde van het afgelopen interval dat opgenomen in de grafiek.</li><li>Max.: De maximale waarde van de tijden die zijn opgenomen in de grafiek. |
| **Geavanceerde** |**> Gegevensstroom niet verifiëren** |
| Enabled |Selecteer deze koppeling als gegevensstroom niet verifiëren voor de tegel moet worden ingeschakeld. Deze benadering biedt een alternatieve bericht als gegevens niet beschikbaar is. U normaal gesproken benadering gebruiken voor een bericht in de tijdelijke periode wanneer de weergave is geïnstalleerd en de gegevens beschikbaar. |
| Query’s uitvoeren |De query die wordt uitgevoerd om te bepalen of gegevens beschikbaar voor de weergave zijn. Als de query geen resultaten oplevert, wordt een bericht weergegeven in plaats van de waarde van de hoofdquery. |
| Message |Het bericht dat wordt weergegeven als de gegevens voor de gegevensstroom verificatie-query geen gegevens retourneert. Als u geen bericht, bieden een *evaluatie uitvoeren* statusbericht wordt weergegeven. |


## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [query's bijgehouden](../log-query/log-query-overview.md) ter ondersteuning van de query's in tegels.
* Voeg [visualisatie delen](view-designer-parts.md) aan uw aangepaste weergave.
