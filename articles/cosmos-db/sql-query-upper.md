---
title: BOVENSTE in Azure Cosmos DB query taal
description: Meer informatie over de functie van SQL-systeem in Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/04/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: fbec4033d093d83925d1682eb7882587e0dc6ca5
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2020
ms.locfileid: "93093636"
---
# <a name="upper-azure-cosmos-db"></a>UPPER (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

 Retourneert een tekenreeksexpressie na het converteren van tekens in kleine letters naar hoofdletters.  

De bovenste systeem functie maakt geen gebruik van de index. Als u regel matig niet-hoofdletter gevoelige vergelijkingen wilt maken, kan de functie van het bovenste systeem een aanzienlijke hoeveelheid van RU verbruiken. Als dit het geval is, kunt u, in plaats van de functie voor het bovenste systeem, om gegevens telkens te normaliseren voor vergelijkingen, het hoofdletter gebruik tijdens het invoegen normaliseren. Vervolgens wordt een query zoals SELECT * FROM c waarbij UPPER (c. name) = ' BOB ' gewoon geselecteerd * van c waarbij c.name = ' BOB '.

## <a name="syntax"></a>Syntaxis
  
```sql
UPPER(<str_expr>)  
```  
  
## <a name="arguments"></a>Argumenten
  
*str_expr*  
   Is een teken reeks expressie.  
  
## <a name="return-types"></a>Retour typen
  
  Retourneert een teken reeks expressie.  
  
## <a name="examples"></a>Voorbeelden
  
  In het volgende voor beeld ziet u hoe u kunt gebruiken `UPPER` in een query  
  
```sql
SELECT UPPER("Abc") AS upper  
```  
  
 Dit is de resultatenset.  
  
```json
[{"upper": "ABC"}]  
```

## <a name="remarks"></a>Opmerkingen

Deze systeem functie maakt geen gebruik van de index.

## <a name="next-steps"></a>Volgende stappen

- [Teken reeks functies Azure Cosmos DB](sql-query-string-functions.md)
- [Systeem functies Azure Cosmos DB](sql-query-system-functions.md)
- [Inleiding tot Azure Cosmos DB](introduction.md)
