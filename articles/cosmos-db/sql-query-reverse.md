---
title: Azure Cosmos DB query taal omkeren
description: Meer informatie over de functie voor het terugdraaien van SQL-systemen in Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 8237918232bd8ba8edb2b8f71440ffd73a913334
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2019
ms.locfileid: "71349559"
---
# <a name="reverse-azure-cosmos-db"></a>OMGEKEERD (Azure Cosmos DB)
 Retourneert een tekenreekswaarde in de omgekeerde volgorde.  
  
## <a name="syntax"></a>Syntaxis
  
```sql
REVERSE(<str_expr>)  
```  
  
## <a name="arguments"></a>Argumenten
  
*str_expr*  
   Is een teken reeks expressie.  
  
## <a name="return-types"></a>Retour typen
  
  Retourneert een tekenreeksexpressie.  
  
## <a name="examples"></a>Voorbeelden
  
  In het volgende voor beeld ziet u hoe u `REVERSE` gebruikt in een query.  
  
```sql
SELECT REVERSE("Abc") AS reverse  
```  
  
 Hier volgt de resultatenset.  
  
```json
[{"reverse": "cbA"}]  
```  

## <a name="next-steps"></a>Volgende stappen

- [Teken reeks functies Azure Cosmos DB](sql-query-string-functions.md)
- [Systeem functies Azure Cosmos DB](sql-query-system-functions.md)
- [Inleiding tot Azure Cosmos DB](introduction.md)
