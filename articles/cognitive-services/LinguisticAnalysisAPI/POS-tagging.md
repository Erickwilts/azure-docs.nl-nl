---
title: Part-of-Speech Tagging - linguïstische analyse-API
description: Meer informatie over hoe Part-of-Speech tags in de linguïstische analyse-API identificeert de categorie of spraak van elk woord uit tekst.
services: cognitive-services
author: RichardSunMS
manager: nitinme
ms.service: cognitive-services
ms.subservice: linguistic-analysis
ms.topic: conceptual
ms.date: 09/27/2016
ms.author: lesun
ROBOTS: NOINDEX
ms.openlocfilehash: 0269397b0f8da66d2bafecfb427ba705fdfff001
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60394486"
---
# <a name="part-of-speech-tagging"></a>Part-of-Speech Tagging

> [!IMPORTANT]
> De preview van Linguistic Analysis is op 9 augustus 2018 uit gebruik genomen. We raden u aan [Azure Machine Learning-tekstanalysemodules](https://docs.microsoft.com/azure/machine-learning/studio-module-reference/text-analytics) te gebruiken voor tekstverwerking en -analyse.

## <a name="background-and-motivation"></a>Achtergrond en motivatie

Zodra een SMS-bericht is gescheiden in zinnen en tokens, wordt de volgende stap van de analyse is het identificeren van de categorie of part-of-speech van elk woord.
Categorieën, zoals het gaat hierbij *zelfstandig naamwoord* (algemeen die mensen, plaatsen, dingen, ideeën, enz.) en *term* (algemeen die acties, verandert van status, enzovoort. Voor sommige woorden, de part-of-speech eenduidig is (bijvoorbeeld *quagmire* is alleen een zelfstandig naamwoord), maar voor nog veel meer, is het moeilijk te zien.
*Tabel* kan moet een plaats waar u zich bevinden (of 2D-indeling van de getallen), maar u kunt ook 'tabel een discussie'.

## <a name="list-of-part-of-speech-tags"></a>Lijst met Part-of-Speech Tags

| Tag | Description | Voorbeeld van de woorden |
|-----|-------------|---------------|
| $ | dollar | $ |
| \`\` | aanhalingsteken openen | \` \`\` |
| '' | aanhalingsteken sluiten | ' '' |
| ( | openingshaakje | ( [ { |
| ) | haakje sluiten | ) ] } |
| , | door komma 's | , |
| -- | streepje | -- |
| . | zin eindteken | . ! ? |
| : | dubbele punt of de drie puntjes | : ; ... |
| CC | combinatie, coördineren | en maar of nog|
| CD | cijfer, kardinaalcurve | nine 20 1980 '96 |
| DT | determiner |een de een alle beide geen van beide|
| EX | Er zijn existentieel: | Er zijn |
| FW | refererende word | enfant terrible hoi polloi je ne sais quoi |
| IN | voorzetsel of samen te brengen| in binnen als bij of |
| JJ | bijvoeglijk naamwoord of cijfer, volgnummer | achtste vrij execrable multimodale |
| JJR | bijvoeglijk naamwoord, vergelijkende | sneller beter goedkoper |
| JJS | bijvoeglijk naamwoord, superlative | beste snelste goedkoopste |
| LS | lijst met item-markering | (a) (b) 1 2 A B A. B. |
| MD | modale assistent | kan mogelijk moeten wordt kan mogelijk moeten zou |
| NN | zelfstandig naamwoord, algemene, singuliere of voor massaopslag | achtergebleven geld schoen |
| NNP | de juiste, singuliere zelfstandig naamwoord, | Kennedy Roosevelt Chicago Weehauken |
| NNPS | de juiste, plural zelfstandig naamwoord, | Springfields struiken |
| NNS | zelfstandig naamwoord, algemene, plural | onderdelen muizen velden |
| PDT | vooraf determiner | alle beide halve veel helemaal dergelijke ervoor dat dit |
| POS | genitive markering | ' van |
| PRP | Overeenkomst, persoonlijke | ze hij het ik we ze u |
| PRP$ | Overeenkomst, possessive | haar zijn de mijn onze hun uw |
| RB | bewerkingsparameter | klinisch alleen |
| RBR | bewerkingsparameter, vergelijkende | meer versomberden grander graver groter grimmer moeilijker zwaardere gezondere zwaardere hogere echter groter hoger slanker zijn omslachtiger minder perfect minder lonelier meer harder kleine meer... |
| RESOURCESTRUCTUUR | bewerkingsparameter, superlative | beste grootste bluntest vroegste verst eerste verst moeilijkst heartiest hoogste grootste minst minder meeste dichtstbijzijnde tweede strengste slechtste |
| RP | Particle | op off-up maken van informatie over |
| SYM | Symbool | % & |
| AAN | "tot" als voorzetsel of infinitive markering | tot |
| OJEE | Tussenwerpsel | ojee zeer Blije howdy Hallo |
| VB | term baseform | Geef elk gewenst moment toewijzen |
| VBD | term verleden tijd | heeft toegewezen flew |
| VBG | term, aanwezig participle of gerundium | toegewezen Vliegende geven |
| VBN | term verleden participle | toegewezen krijgen alleen gesynchroniseerd |
| VBP | term, de tegenwoordige aanwezig, niet 3e persoon rapportgebruiker | Geef elk gewenst moment toewijzen |
| VBZ | term, de tegenwoordige aanwezig, 3e persoon rapportgebruiker | biedt wijst vliegt naar |
| WDT | W determiner | die welke die |
| WP | WH-pronoun | wie wie |
| WP$ | W-overeenkomst, possessive | waarvan |
| WRB | Wh-adverb | hoe echter wanneer waar |

## <a name="specification"></a>Specificatie

Als voor tokeniseren, we zijn afhankelijk van de specificatie van de [Penn Treebank](https://catalog.ldc.upenn.edu/LDC99T42).
