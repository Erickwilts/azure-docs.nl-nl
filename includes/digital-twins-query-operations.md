---
author: baanders
description: include-bestand voor Azure Digital Apparaatdubbels-query bewerkingen
ms.service: digital-twins
ms.topic: include
ms.date: 7/28/2020
ms.author: baanders
ms.openlocfilehash: 333a7ec4ae0e5c8cbc94a603e2ccf81ee92e7d48
ms.sourcegitcommit: a92fbc09b859941ed64128db6ff72b7a7bcec6ab
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/15/2020
ms.locfileid: "92078456"
---
## <a name="query-language-features"></a>Functies voor query taal

Azure Digital Apparaatdubbels biedt uitgebreide query mogelijkheden tegen het dubbele diagram. Query's worden beschreven met behulp van de SQL-achtige syntaxis, in een query taal die vergelijkbaar is met de [IOT hub query taal](../articles/iot-hub/iot-hub-devguide-query-language.md) met veel vergelijk bare functies.

> [!NOTE]
> Alle Azure Digital Apparaatdubbels-query bewerkingen zijn hoofdletter gevoelig.

Hier volgen de bewerkingen die beschikbaar zijn in de Azure Digital Apparaatdubbels-query taal.

Digitale apparaatdubbels ophalen op basis van hun...
* model (met behulp van `IS_OF_MODEL` operator)
* eigenschappen (inclusief [Label eigenschappen](../articles/digital-twins/how-to-use-tags.md))
* natie
* relaties
  - eigenschappen van de relaties

U kunt uw query's nog verder verbeteren met de volgende bewerkingen:
* Apparaatdubbels ophalen via meerdere relatie typen ( `JOIN` query's). 
  - Tijdens de preview zijn Maxi maal vijf niveaus `JOIN` toegestaan.
* Alleen de bovenste query resultaten selecteren ( `Select TOP` operator)
* Het aantal items in een resultatenset tellen met `Select COUNT`
* Projecties gebruiken om te kiezen welke kolommen worden geretourneerd door een query
* Scalaire functies gebruiken: `IS_BOOL` , `IS_DEFINED` , `IS_NULL` , `IS_NUMBER` , `IS_OBJECT` , `IS_PRIMITIVE` , `IS_STRING` , `STARTSWITH` , `ENDSWITH` .
* Vergelijkings operatoren voor query's gebruiken: `IN` / `NIN` , `=` , `!=` , `<` , `>` , `<=` , `>=` .
* Gebruik een combi natie ( `AND` , `OR` , `NOT` operator) van `IS_OF_MODEL` , scalaire functies en vergelijkings operators.
* Voortzetting gebruiken: het query-object wordt geïnstantieerd met een pagina grootte (Maxi maal 100). U kunt de digitale apparaatdubbels één pagina tegelijk ophalen door de vervolg token op te geven in de volgende aanroepen van de API.