---
title: Include-bestand
description: Include-bestand
services: azure-monitor
author: rboucher
tags: azure-service-management
ms.topic: include
ms.date: 02/07/2019
ms.author: robb
ms.custom: include file
ms.openlocfilehash: e6b64b5a1a60ba3bbf93e607536eeb0379669c73
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "91641765"
---
**Volume gegevensverzameling en gegevensretentie** 

| Laag | Limiet per dag | Bewaartijd voor gegevens | Opmerking |
|:---|:---|:---|:---|
| Huidig Prijscategorie per GB<br>(geïntroduceerd in april 2018) | Geen limiet | 30 - 730 dagen | Gegevensretentie na meer dan 31 dagen is tegen extra kosten mogelijk. Meer informatie over Azure Monitor-prijzen. |
| Verouderd Gratis lagen<br>(geïntroduceerd in april 2016) | 500 MB | 7 dagen | Als uw werkruimte de limiet van 500 MB per dag bereikt, wordt de gegevensopname gestopt en aan het begin van de volgende dag hervat. Een dag is gebaseerd op UTC. Houd er rekening mee dat de gegevens die worden verzameld door Azure Security Center, niet zijn opgenomen in de limiet van 500 MB per dag en nog steeds worden verzameld als de limiet is overschreden.  |
| Verouderd Zelfstandig Laag per GB<br>(geïntroduceerd in april 2016) | Geen limiet | 30 tot 730 dagen | Gegevensretentie na meer dan 31 dagen is tegen extra kosten mogelijk. Meer informatie over Azure Monitor-prijzen. |
| Verouderd Per knooppunt (OMS)<br>(geïntroduceerd in april 2016) | Geen limiet | 30 tot 730 dagen | Gegevensretentie na meer dan 31 dagen is tegen extra kosten mogelijk. Meer informatie over Azure Monitor-prijzen. |
| Verouderd Standaardlaag | Geen limiet | 30 dagen  | Retentie kan niet worden aangepast |
| Verouderd Premiumlaag | Geen limiet | 365 dagen  | Retentie kan niet worden aangepast |

**Aantal werkruimten per abonnement.**

| Prijscategorie    | Werkruimtelimiet | Opmerkingen
|:---|:---|:---|
| Gratis laag  | 10 | Deze limiet kan niet worden verhoogd. |
| Alle andere servicelagen | Geen limiet | Er gelden limieten voor het aantal resources binnen een resourcegroep en voor het aantal resourcegroepen per abonnement. |

**Azure Portal**

| Categorie | Limiet | Opmerkingen |
|:---|:---|:---|
| Maximum aantal records geretourneerd door logboekquery | 10.000 | Verminder de hoeveelheid resultaten met behulp van het querybereik, het tijdsbereik en de filters in de query. |


**API gegevensverzamelaar**

| Categorie | Limiet | Opmerkingen |
|:---|:---|:---|
| Maximumgrootte voor een enkel bericht | 30 MB | Grotere volumes splitsen in meerdere berichten. |
| Maximumgrootte voor veldwaarden  | 32 kB | Velden die langer zijn dan 32 KB worden afgebroken. |

**Search API**

| Categorie | Limiet | Opmerkingen |
|:---|:---|:---|
| Maximum aantal records dat wordt geretourneerd in één query | 500.000 | |
| Maximale grootte van geretourneerde gegevens | 64.000.000 bytes (~ 61 MiB)| |
| Maximale uitvoeringstijd van query | 10 minuten | Zie [Time-outs](https://dev.loganalytics.io/documentation/Using-the-API/Timeouts) voor meer informatie.  |
| Maximale aanvraagsnelheid | 200 aanvragen per 30 seconden per IP-adres van Azure AD-gebruikers of client | Zie [Snelheidslimieten](https://dev.loganalytics.io/documentation/Using-the-API/Limits) voor meer informatie. |

**Algemene limieten voor werkruimten**

| Categorie | Limiet | Opmerkingen |
|:---|:---|:---|
| Maximum aantal kolommen in een tabel         | 500 | |
| Maximum aantal tekens voor de kolomnaam | 500 | |
| Gegevensexport | Momenteel niet beschikbaar | Gebruik Azure-functie of logische app om gegevens samen te voegen en te exporteren. | 

**<a name="data-ingestion-volume-rate">Gegevensopnamevolume</a>**

Azure Monitor is een grootschalige gegevensservice die elke maand een groeiend aantal terabytes aan gegevens van duizenden klanten verwerkt. De limiet voor het opnamevolume moet de klanten van Azure Monitor isoleren tegen plotselinge opnamepieken in een multitenancy-omgeving. Er is een standaarddrempel van 500 MB (gecomprimeerd) voor het opnamevolume gedefenieerd in werkruimtes. Dit staat gelijk aan een niet-gecomprimeerd volume van ongeveer **6 GB/min**. De werkelijke grootte kan per gegevenstype variëren afhankelijk van de logboeklengte en de compressieratio ervan. Deze volumebeperking geldt voor elke gegevensopname, ongeacht of deze is verzonden vanuit Azure-resources met behulp van [Diagnostische instellingen](../articles/azure-monitor/platform/diagnostic-settings.md), [Gegevensverzamelaar-API](../articles/azure-monitor/platform/data-collector-api.md) of agents.

Als u gegevens naar een werkruimte verzendt met een volumesnelheid die hoger is dan 80 % van de drempel die in uw werkruimte is geconfigureerd, wordt er om de zes uur een gebeurtenis verzonden naar de *bewerkingstabel* in uw werkruimte, zolang de drempel nog steeds wordt overschreden. Als het opnamevolume hoger is dan de drempel, worden sommige gegevens verwijderd en wordt er om de zes uur een gebeurtenis verzonden naar de *bewerkingstabel* in uw werkruimte, zolang de drempel wordt overschreden. Als uw opnamevolume de drempel blijft overschrijden of als u verwacht de drempel binnenkort te bereiken, kunt u een verhoging aanvragen door een ondersteuningsaanvraag te openen. 

Als u een melding wilt ontvangen over een het bereiken van het volumelimiet in uw werkruimte, maakt u een [waarschuwingsregel voor het logboek](../articles/azure-monitor/platform/alerts-log.md) met behulp van de volgende query met waarschuwingslogica op basis van het aantal resultaten hoger dan nul, evaluatieperiode van 5 minuten en frequentie van 5 minuten.

Opnamevolume heeft de drempel overschreden
```Kusto
Operation
| where OperationCategory == "Ingestion"
| where OperationKey == "Ingestion rate limit"
| where OperationStatus == "Error"
```

Opnamevolume heeft 80% van de drempel bereikt
```Kusto
Operation
| where OperationCategory == "Ingestion"
| where OperationKey == "Ingestion rate limit"
| where OperationStatus == "Warning"
```

Opnamevolume heeft 70% van de drempel bereikt
```Kusto
Operation
| where OperationCategory == "Ingestion"
| where OperationKey == "Ingestion rate limit"
| where OperationStatus == "Info"
```

>[!NOTE]
>Afhankelijk van hoe lang u logboekanalyse hebt gebruikt, hebt u mogelijk toegang tot verouderde prijscategorieën. Meer informatie over [verouderde prijscategorieën van logboekanalyse](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#legacy-pricing-tiers). 
