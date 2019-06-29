---
title: Zin koppelen en de uitlijning - aangepaste Translator
titleSuffix: Azure Cognitive Services
description: Tijdens de uitvoering van de training, zinnen aanwezig zijn in parallelle documenten gekoppeld of uitgelijnd. Aangepaste Translator leert vertalingen één zin op een tijdstip, door te lezen van een zin, de vertaling van deze zin. Vervolgens worden uitgelijnd woorden en woordgroepen in deze twee zinnen met elkaar.
author: swmachan
manager: christw
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 02/21/2019
ms.author: swmachan
ms.topic: conceptual
ms.openlocfilehash: f73c40704e10a8e2368ee1eb369ee3dccdf269ee
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67448320"
---
# <a name="sentence-pairing-and-alignment-in-parallel-documents"></a>Zin koppelen en de uitlijning in parallelle documenten

Tijdens de training, zinnen aanwezig zijn in parallelle documenten gekoppeld of uitgelijnd. Aangepaste Translator rapporteert het aantal zinnen kon worden gekoppeld als de zinnen uitgelijnd in elk van de gegevenssets.

## <a name="pairing-and-alignment-process"></a>Koppelen en de uitlijning proces

Aangepaste Translator leert vertalingen van een zin zinnen op een tijdstip. Deze metingen een zin uit de gegevensbron, waarna de vertaling van deze zin tot het doel. Vervolgens worden uitgelijnd woorden en woordgroepen in deze twee zinnen met elkaar. Dit proces kunt te maken van een overzicht van de woorden en woordgroepen in één zin naar de equivalente woorden en zinnen in de vertaling van deze zin. Uitlijning probeert om ervoor te zorgen dat het systeem traint op zinnen die vertalingen van elkaar zijn.

## <a name="pre-aligned-documents"></a>Vooraf uitgelijnde documenten

Als u weet dat u parallelle documenten hebt toegevoegd, kan u de uitlijning zin overschrijven door te geven van vooraf uitgelijnde tekstbestanden. U kunt alle zinnen extraheren uit beide documenten in een tekstbestand, georganiseerd één zin per regel en uploaden met een `.align` extensie. De `.align` extensie aangepaste Translator geeft aan dat de zin uitlijning moeten worden overgeslagen.

Probeer om ervoor te zorgen dat er één zin per regel in de bestanden voor de beste resultaten. Geen tekens voor nieuwe regels in een zin als dit ervoor de slechte uitlijning dat zorgt.

## <a name="suggested-minimum-number-of-extracted-and-aligned-sentences"></a>Voorgestelde minimum aantal opgehaalde en uitgelijnde zinnen

Voor een training te voltooien, ziet de volgende tabel u het minimum aantal opgehaalde zinnen en uitgelijnde zinnen vereist in elke gegevensset. Het aanbevolen minimum aantal opgehaalde zinnen is veel hoger dan de aanbevolen minimum aantal uitgelijnde zinnen rekening te houden met het feit dat de uitlijning zin mogelijk niet alle geëxtraheerde zinnen is uitgelijnd.

| Gegevensset   | Voorgestelde minimale uitgepakte zin tellen | Voorgestelde minimale uitgelijnde zin tellen | Maximale uitgelijnde zin tellen |
|------------|--------------------------------------------|------------------------------------------|--------------------------------|
| Training   | 10.000                                     | 2,000                                    | Geen bovengrens                 |
| Afstemmen     | 2,000                                      | 500                                      | 2,500                          |
| Testen    | 2,000                                      | 500                                      | 2,500                          |
| Woordenlijst | 0                                          | 0                                        | Geen bovengrens                 |

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het gebruik van een [woordenlijst](what-is-dictionary.md) in aangepaste Translator.
