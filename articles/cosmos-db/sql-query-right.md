---
title: RECHTS in Azure Cosmos DB query taal
description: Meer informatie over de functie voor SQL-systeem functies in Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 247616d2ac4f2a5799a5896d679f6e6d5917d5a6
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "78302148"
---
# <a name="right-azure-cosmos-db"></a>RIGHT (Azure Cosmos DB)
 Retourneert het rechterdeel van een tekenreeks met het opgegeven aantal tekens.  
  
## <a name="syntax"></a>Syntaxis
  
```sql
RIGHT(<str_expr>, <num_expr>)  
```  
  
## <a name="arguments"></a>Argumenten
  
*str_expr*  
   Is de teken reeks expressie waaruit tekens moeten worden geëxtraheerd.  
  
*num_expr*  
   Is een numerieke expressie waarmee het aantal tekens wordt opgegeven.  
  
## <a name="return-types"></a>Retour typen
  
  Retourneert een teken reeks expressie.  
  
## <a name="examples"></a>Voorbeelden
  
  In het volgende voor beeld wordt het rechter deel van ' ABC ' geretourneerd voor verschillende lengte waarden.  
  
```sql
SELECT RIGHT("abc", 1) AS r1, RIGHT("abc", 2) AS r2 
```  
  
 Dit is de resultatenset.  
  
```json
[{"r1": "c", "r2": "bc"}]  
```  

## <a name="remarks"></a>Opmerkingen

Deze systeem functie maakt geen gebruik van de index.

## <a name="next-steps"></a>Volgende stappen

- [Teken reeks functies Azure Cosmos DB](sql-query-string-functions.md)
- [Systeem functies Azure Cosmos DB](sql-query-system-functions.md)
- [Inleiding tot Azure Cosmos DB](introduction.md)
