---
title: Variabelen toewijzen met Synapse SQL
description: In dit artikel vindt u tips voor het toewijzen van T-SQL-variabelen met Synapse SQL.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick
ms.openlocfilehash: abc0a9f6fa6baefc7cc4b29c84ff179f0851dc30
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "90028913"
---
# <a name="assign-variables-with-synapse-sql"></a>Variabelen toewijzen met Synapse SQL

In dit artikel vindt u tips voor het toewijzen van T-SQL-variabelen met Synapse SQL.

## <a name="set-variables-with-declare"></a>Variabelen instellen met DECLAReren

Variabelen in Synapse SQL worden ingesteld met behulp van de `DECLARE` instructie of de `SET` instructie. Initialisatie van variabelen met DECLAReren is een van de meest flexibele manieren om een variabele waarde in te stellen in Synapse SQL.

```sql
DECLARE @v  int = 0
;
```

U kunt DECLARe ook gebruiken om meer dan één variabele per keer in te stellen. U kunt niet selecteren of bijwerken gebruiken om het volgende te doen:

```sql
DECLARE @v  INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Smith')
,       @v1 INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Jones')
;
```

U kunt een variabele niet initialiseren en gebruiken in dezelfde DECLARe-instructie. Ter illustratie: het volgende voor beeld is niet toegestaan omdat * \@ P1* is geïnitialiseerd en wordt gebruikt in dezelfde Declare-instructie. In het volgende voor beeld wordt een fout weer geven.

```sql
DECLARE @p1 int = 0
,       @p2 int = (SELECT COUNT (*) FROM sys.types where is_user_defined = @p1 )
;
```

## <a name="set-values-with-set"></a>Waarden instellen met SET

SET is een gemeen schappelijke methode voor het instellen van één variabele.

De volgende instructies zijn allemaal geldige manieren om een variabele in te stellen met SET:

```sql
SET     @v = (Select max(database_id) from sys.databases);
SET     @v = 1;
SET     @v = @v+1;
SET     @v +=1;
```

U kunt slechts één variabele tegelijk instellen met SET. Samengestelde Opera tors zijn echter wel toegestaan.

## <a name="limitations"></a>Beperkingen

U kunt bijwerken niet gebruiken voor het toewijzen van variabelen.

## <a name="next-steps"></a>Volgende stappen

Zie het artikel [overzicht van Synapse SQL Development](develop-overview.md) voor meer tips voor ontwikkel aars.
