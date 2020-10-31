---
title: BOOGCOS in Azure Cosmos DB query taal
description: Meer informatie over hoe de SQL-systeem functie BOOGCOS (arccosice) in Azure Cosmos DB de hoek in radialen retourneert, waarvan de cosinus de opgegeven numerieke expressie is
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 99730b4c67adc841a33ea9d60440b00a19f90e48
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2020
ms.locfileid: "93090950"
---
# <a name="acos-azure-cosmos-db"></a>BOOGCOS (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

 Retourneert de hoek, in radialen, waarvan de cosinus de opgegeven numerieke expressie is. Dit wordt ook wel de arccosinus genoemd.  
  
## <a name="syntax"></a>Syntaxis
  
```sql
ACOS(<numeric_expr>)  
```  
  
## <a name="arguments"></a>Argumenten
  
*numeric_expr*  
   Is een numerieke expressie.  
  
## <a name="return-types"></a>Retour typen
  
  Retourneert een numerieke expressie.  
  
## <a name="examples"></a>Voorbeelden
  
  In het volgende voor beeld wordt de waarde `ACOS` -1 geretourneerd.  
  
```sql
SELECT ACOS(-1) AS acos 
```  
  
 Dit is de resultatenset.  
  
```json
[{"acos": 3.1415926535897931}]  
```  

## <a name="remarks"></a>Opmerkingen

Deze systeem functie maakt geen gebruik van de index.

## <a name="next-steps"></a>Volgende stappen

- [Wiskundige functies Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Systeem functies Azure Cosmos DB](sql-query-system-functions.md)
- [Inleiding tot Azure Cosmos DB](introduction.md)
