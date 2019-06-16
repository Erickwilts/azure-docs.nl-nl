---
title: Azure Data Factory toewijzing gegevensstroom afgeleide kolom transformatie
description: Informatie over het transformeren van gegevens op schaal met Azure Data Factory toewijzing Flow afgeleide kolom gegevenstransformatie
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/08/2018
ms.openlocfilehash: 6568e5ebf356bb0e6b4ac8ff6059cd093f8da821
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64917569"
---
# <a name="mapping-data-flow-derived-column-transformation"></a>Toewijzing van de gegevensstroom afgeleide kolom transformatie

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Gebruik de transformatie afgeleide kolom voor het genereren van nieuwe kolommen in de gegevensstroom of om bestaande velden wijzigen.

![kolom afleiden](media/data-flow/dc1.png "afgeleide kolom")

U kunt meerdere afgeleide kolom acties uitvoeren in een enkele afgeleide kolom-transformatie. Klik op 'Kolom toevoegen' voor het transformeren van meer dan 1 kolom in de transformatiestap één.

Selecteer een bestaande kolom te overschrijven met een nieuwe waarde van de afgeleide in het veld kolom, of klikt u op 'Nieuwe kolom maken' voor het genereren van een nieuwe kolom met de zojuist afgeleide waarde.

De opbouwfunctie voor expressies waar u de expressie voor de afgeleide kolommen met behulp van functies van de expressie kunt bouwen, het tekstvak voor expressie wordt geopend.

## <a name="column-patterns"></a>Kolompatronen

Als uw kolomnamen variabele van uw bronnen, kunt u desgewenst transformaties binnen het afgeleide kolom maken met behulp van de kolom patronen in plaats van kolommen met de naam. Zie de [Schema Drift](concepts-data-flow-schema-drift.md) artikel voor meer informatie.

![patroon voor kolom](media/data-flow/columnpattern.png "kolom patronen")

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de [Data Factory expressietaal voor transformaties](https://aka.ms/dataflowexpressions) en de [opbouwfunctie voor expressies](concepts-data-flow-expression-builder.md)
