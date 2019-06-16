---
title: Detecteren van gegevensbronnen in Azure Data Catalog
description: In dit artikel ziet u hoe u voor het detecteren van geregistreerde gegevensassets met Azure Data Catalog, met inbegrip van zoeken en filteren met behulp van de treffers markeren mogelijkheden van de Azure Data Catalog-portal.
author: JasonWHowell
ms.author: jasonh
ms.service: data-catalog
ms.topic: conceptual
ms.date: 04/05/2019
ms.openlocfilehash: b21bf1b50152130d7b6edd227c87fcaca28c1e6a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "61001412"
---
# <a name="how-to-discover-data-sources-in-azure-data-catalog"></a>Detecteren van gegevensbronnen in Azure Data Catalog

## <a name="introduction"></a>Inleiding

Azure Data Catalog is een volledig beheerde cloudservice die als een systeem van registratie en detectie van bedrijfsgegevensassets fungeert. Data Catalog helpt met andere woorden, mensen detecteren, begrijpen en gebruiken van gegevensbronnen. Het helpt organisaties meer waarde halen uit hun bestaande gegevens. Nadat een gegevensbron is geregistreerd met Data Catalog, worden de metagegevens van de wordt geïndexeerd door de service, zodat u gemakkelijk terugvinden kunt voor het detecteren van de gegevens die u nodig hebt.

## <a name="searching-and-filtering"></a>Zoeken en filteren

Detectie in Data Catalog twee primaire mechanismen gebruikt: zoeken en filteren.

Zoeken is zowel intuïtief als krachtig. Standaard worden de zoektermen vergeleken met een eigenschap in de catalogus, met inbegrip van door de gebruiker gemaakte aantekeningen.

Filteren is bedoeld als aanvulling op het zoeken. U kunt specifieke kenmerken, zoals experts, gegevensbrontype, objecttype en tags selecteren. U kunt alleen overeenkomende gegevensassets weer en beperk zoekresultaten tot overeenkomende assets.

Met behulp van een combinatie van zoeken en filteren, kunt u de gegevensbronnen die zijn geregistreerd met Data Catalog voor het detecteren van de gegevensbronnen die u nodig snel te navigeren.

## <a name="search-syntax"></a>Zoeksyntaxis

Hoewel zoeken met vrije tekst standaard eenvoudige en intuïtieve is, kunt u de zoeksyntaxis van Data Catalog ook gebruiken voor meer controle over de lijst met zoekresultaten. Zoeken in gegevenscatalogus ondersteunt de volgende technieken:

| Techniek | Gebruiken | Voorbeeld |
| --- | --- | --- |
| Basiszoekopdracht |Basiszoekopdracht die gebruikmaakt van een of meer zoektermen. De resultaten zijn alle activa die overeenkomen met een eigenschap met een of meer van de opgegeven voorwaarden. |`sales data` |
| Bereik van eigenschap definiëren |Alleen gegevensbronnen waarbij de zoekterm met de opgegeven eigenschap overeenkomt retourneren. |`name:finance` |
| Booleaanse operators |Verbreed of Verfijn een zoekopdracht met Booleaanse bewerkingen. |`finance NOT corporate` |
| Groeperen met haakjes |Gebruik haakjes en groepeer delen van de query voor logische isolatie, met name in combinatie met Booleaanse operators. |`name:finance AND (tags:Q1 OR tags:Q2)` |
| Vergelijkingsoperators |Gebruik andere vergelijkingen dan gelijkheid voor eigenschappen die de gegevenstypen Numeriek en datum hebben. |`modifiedTime > "11/05/2014"` |

Zie voor meer informatie over Data Catalog-zoekopdracht, de [Azure Data Catalog](/rest/api/datacatalog/#search-syntax-reference) artikel.

## <a name="hit-highlighting"></a>Markeren

Wanneer u zoekresultaten, elk weergegeven eigenschappen die overeenkomen met de opgegeven zoektermen (zoals de naam van de activa gegevens, beschrijving en tags) zijn gemarkeerd zodat het is gemakkelijker om te bepalen waarom een bepaalde gegevensasset geretourneerd door een bepaalde zoekopdracht.

> [!NOTE]
> Als u wilt uitschakelen treffers markeren, gebruikt u de **markeren** -switch in de Data Catalog-portal.

Wanneer u de resultaten bekijkt, het altijd mogelijk niet duidelijk waarom een gegevensasset opgenomen, wordt zelfs met treffers markeren is ingeschakeld. Omdat alle eigenschappen standaard worden doorzocht, kan een gegevensasset worden geretourneerd vanwege een overeenkomst op een eigenschap op kolomniveau. En omdat meerdere gebruikers van geregistreerde gegevensassets met hun eigen labels en beschrijvingen annoteren kunnen, niet alle metagegevens wordt weergegeven in de lijst met zoekresultaten.

Tegel weergeven in de standaard, elke tegel weergegeven in de lijst met zoekresultaten bevat een **weergave zoekterm overeenkomt met** pictogram, zodat u kunt snel het aantal overeenkomsten en de locatie, en voor toegang tot deze als u wilt weergeven.

 ![Treffers markeren en zoekopdracht overeenkomt met in de Azure Data Catalog-portal](./media/data-catalog-how-to-discover/search-matches.png)

## <a name="summary"></a>Samenvatting

Omdat het registreren van een gegevensbron met Data Catalog structurele en beschrijvende metagegevens opgehaald uit de gegevensbron met de catalogusservice, wordt de gegevensbron eenvoudiger te detecteren en begrijpen. Nadat u een gegevensbron hebt geregistreerd, kunt u ontdekken met behulp van filters en zoek uit in de Data Catalog-portal.

## <a name="next-steps"></a>Volgende stappen

* Zie voor stapsgewijze informatie over het detecteren van gegevensbronnen [aan de slag met Azure Data Catalog](data-catalog-get-started.md).
