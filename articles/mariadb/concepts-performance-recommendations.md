---
title: Aanbevelingen voor prestaties in Azure Database for MariaDB
description: In dit artikel wordt de functie aanbevolen prestatie aanbeveling in Azure Database for MariaDB
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 06/27/2019
ms.openlocfilehash: b363a994024b4a53703b6107ef4190129e900547
ms.sourcegitcommit: 78ebf29ee6be84b415c558f43d34cbe1bcc0b38a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/12/2019
ms.locfileid: "68950651"
---
# <a name="performance-recommendations-in-azure-database-for-mariadb"></a>Aanbevelingen voor prestaties in Azure Database for MariaDB

**Van toepassing op:** Azure Database for MariaDB 10,2

> [!IMPORTANT]
> Aanbevelingen voor prestaties zijn een preview-versie.

Met de functie voor prestatie verbeteringen worden uw data bases geanalyseerd om aangepaste suggesties te maken voor betere prestaties. Voor het produceren van de aanbevelingen kijken de analyse naar verschillende database kenmerken, waaronder schema. Schakel het [query archief](concepts-query-store.md) op de server in om de functie aanbevelingen voor prestaties volledig te benutten. Als het prestatie schema is uitgeschakeld, kunnen performance_schema en een subset van de prestatie schema-instrumenten die vereist zijn voor de functie, worden ingeschakeld in query Store. Nadat u een aanbevolen prestatie aanbeveling hebt geïmplementeerd, moet u de prestaties testen om de impact van deze wijzigingen te evalueren.

## <a name="permissions"></a>Machtigingen

De machtigingen van **Eigenaar** of **Inzender** zijn vereist voor het uitvoeren van analyses met de functie Prestatieaanbevelingen.

## <a name="performance-recommendations"></a>Prestatieaanbevelingen

De functie [Prestatieaanbevelingen](concepts-performance-recommendations.md) analyseert workloads op de server om indexen te analyseren met de mogelijkheid om prestaties te verbeteren.

Open **prestatie aanbevelingen** van het gedeelte **intelligente prestaties** van de menu balk op de pagina Azure portal voor uw MariaDB-server.

![Landingspagina van Prestatieaanbevelingen](./media/concepts-performance-recommendations/performance-recommendations-page.png)

Selecteer **analyseren** en kies een Data Base, waarmee de analyse wordt gestart. Afhankelijk van uw werk belasting kan het enkele minuten duren voordat de analyse is voltooid. Wanneer de analyse is voltooid, verschijnt er een melding in de portal. Analyse voert een grondige controle uit van uw data base. We raden u aan om analyses uit te voeren tijdens rustige Peri Oden.

In het venster **aanbevelingen** wordt een lijst met aanbevelingen weer gegeven als deze zijn gevonden en de gerelateerde query-id die deze aanbeveling heeft gegenereerd. Met de query-ID kunt u de [mysql. query_store](concepts-query-store.md#mysqlquery_store) -weer gave gebruiken om meer te weten te komen over de query.

![Nieuwe pagina prestatie aanbevelingen](./media/concepts-performance-recommendations/performance-recommendations-result.png)

Aanbevelingen worden niet automatisch toegepast. Als u de aanbeveling wilt Toep assen, kopieert u de query tekst en voert u deze uit vanaf de client van uw keuze. Vergeet niet om te testen en te controleren om de aanbeveling te evalueren.

## <a name="recommendation-types"></a>Aanbevelings typen

Momenteel worden alleen aanbevelingen voor het *maken van indexen* ondersteund.

### <a name="create-index-recommendations"></a>Aanbevelingen voor index maken

Bij het *maken van index* aanbevelingen worden nieuwe indexen voorgesteld om de meest frequente uitvoeringen of tijdrovende query's in de werk belasting te versnellen. Voor dit aanbevelings type moet het [query archief](concepts-query-store.md) worden ingeschakeld. In query Store worden query gegevens verzameld en worden de gedetailleerde query runtime en de frequentie statistieken beschreven die de analyse gebruikt om de aanbeveling te doen.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [bewaken en afstemmen](concepts-monitoring.md) van Azure database for MariaDB.