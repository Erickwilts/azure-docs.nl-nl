---
title: Query Performance Insight in Azure Database for MySQL
description: In dit artikel wordt de Query Performance Insight functie in Azure Database for MySQL beschreven
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 06/27/2019
ms.openlocfilehash: f6c74582760d77ae4318739c74f77d0bcd0501fe
ms.sourcegitcommit: 78ebf29ee6be84b415c558f43d34cbe1bcc0b38a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/12/2019
ms.locfileid: "68950365"
---
# <a name="query-performance-insight-in-azure-database-for-mysql"></a>Query Performance Insight in Azure Database for MySQL

**Van toepassing op:** Azure Database for MySQL 5,7

> [!IMPORTANT]
> Query Performance Insight is in preview.

Query Performance Insight helpt u snel te identificeren wat uw langste query's zijn, hoe ze in de loop van de tijd veranderen en welke wacht tijden van invloed zijn op deze.

## <a name="common-scenarios"></a>Algemene scenario's

### <a name="long-running-queries"></a>Query's waarvoor de uitvoering lang duurt

- Langst uitgevoerde query's in de afgelopen X uur identificeren
- Eerste N query's identificeren die op resources wachten
 
### <a name="wait-statistics"></a>Wacht statistieken

- Wat is de wacht aard van een query?
- Uitleg over trends voor de resource wacht en waar de bron conflicten bestaan

## <a name="permissions"></a>Machtigingen

De machtigingen **Eigenaar** of **Inzender** zijn vereist om de tekst van de query's weer te geven in Query Performance Insight. Met de machtiging **Lezer** kunt u grafieken en tabellen weergeven maar geen tekst opvragen.

## <a name="prerequisites"></a>Vereisten

Query Performance Insight werkt alleen als de gegevens in het [query archief](concepts-query-store.md)aanwezig zijn.

## <a name="viewing-performance-insights"></a>Prestatie inzichten weer geven

De weergave [Query Performance Insight](concepts-query-performance-insight.md) in de Azure Portal toont visualisaties van belangrijke informatie uit de Query Store.

Selecteer op de pagina Portal van uw Azure Database for MySQL-server **query Performance Insight** onder het gedeelte **intelligente prestaties** van de menu balk.

### <a name="long-running-queries"></a>Query's waarvoor de uitvoering lang duurt

Op het tabblad **langlopende query's** worden de top 5 query's weer gegeven op gemiddelde duur per uitvoering, samengevoegd in intervallen van 15 minuten. U kunt meer query's weer geven door te selecteren in de vervolg keuzelijst **aantal query's** . Het is mogelijk dat de grafiekkleuren voor een specifieke query-id verschillen wanneer u dit doet.

U kunt in de grafiek klikken en slepen om de tijdspanne te beperken tot een specifiek tijdvenster. U kunt ook de pictogrammen in-en uitzoomen gebruiken om respectievelijk een kleinere of grotere tijds periode weer te geven.

![Langlopende query's Query Performance Insight](./media/concepts-query-performance-insight/query-performance-insight-landing-page.png) 

### <a name="wait-statistics"></a>Wacht statistieken

> [!NOTE]
> Wacht statistieken zijn bedoeld voor het oplossen van problemen met de prestaties van query's. U wordt aangeraden alleen in te scha kelen voor het oplossen van problemen.

Wacht statistieken bieden een weer gave van de wacht gebeurtenissen die optreden tijdens het uitvoeren van een specifieke query. Meer informatie over de gebeurtenis typen wacht in de [documentatie](https://go.microsoft.com/fwlink/?linkid=2098206)van de MySQL-engine.

Selecteer het tabblad **Wachtstatistieken** om de bijbehorende visualisaties voor wachttijden in de server weer te geven.

Query's die worden weer gegeven in de weer gave wachten statistieken, worden gegroepeerd op de query's die de grootste wacht tijden tijdens het opgegeven tijds interval vertonen.

![Query Performance Insight wacht op statistieken](./media/concepts-query-performance-insight/query-performance-insight-wait-statistics.png)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [bewaken en afstemmen](concepts-monitoring.md) van Azure database for MySQL.