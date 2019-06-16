---
title: Opschonen en gegevens worden voorbereid voor Azure Machine Learning - Team Data Science Process
description: Voorverwerken en opschonen van gegevens naar voorbereiden voor het efficiënt worden gebruikt voor machine learning.
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/09/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 2b3ec3352d6e1939b195bbba87b8a824404346ae
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "61044597"
---
# <a name="tasks-to-prepare-data-for-enhanced-machine-learning"></a>Taken om gegevens voor te bereiden voor verbeterde Machine Learning
Vooraf verwerken en opschonen van gegevens zijn belangrijke taken die normaal gesproken moeten worden uitgevoerd voordat de gegevensset effectief kan worden gebruikt voor machine learning. Onbewerkte gegevens is vaak veel ruis veroorzaken en onbetrouwbare en mogelijk ontbreken waarden. Met behulp van dergelijke gegevens voor modellen kan misleidend resultaten opleveren. Deze taken maken deel uit van het Team Data Science Process (TDSP) en een eerste onderzoek van een gegevensset die wordt gebruikt om te detecteren en plannen van het vooraf verwerken vereist doorgaans als volgt. Zie voor meer instructies voor de TDSP-proces gedetailleerde, de stappen die worden beschreven de [Team Data Science Process](overview.md).

Het vooraf verwerken en opschonen van taken, zoals de gegevens verkennen-taak kunnen worden uitgevoerd in een groot aantal omgevingen, zoals SQL of Hive of Azure Machine Learning Studio, en met verschillende hulpprogramma's en talen, zoals R of Python, afhankelijk van waar uw gegevens worden opgeslagen en hoe dit wordt geformatteerd. Aangezien TDSP iteratieve van aard is, kunnen deze taken plaatsvinden op verschillende stappen in de werkstroom van het proces.

Dit artikel bevat verschillende gegevensverwerking concepten en taken die kunnen worden uitgevoerd vóór of na het ophalen van gegevens in Azure Machine Learning.

Zie voor een voorbeeld van gegevens verkennen en de voorverwerking gedaan in Azure Machine Learning studio, de [vooraf verwerken van gegevens in Azure Machine Learning Studio](https://azure.microsoft.com/documentation/videos/preprocessing-data-in-azure-ml-studio/) video.

## <a name="why-pre-process-and-clean-data"></a>Waarom voorverwerken en opschonen van gegevens?
Werkelijk bestaande gegevens uit diverse bronnen worden verzameld en processen en onregelmatigheden of beschadigde gegevens in gevaar brengt de kwaliteit van de gegevensset kunnen bevatten. De gebruikelijke data quality-problemen die zich voordoen zijn:

* **Onvolledige**: Gegevens beschikt niet over de kenmerken of met ontbrekende waarden.
* **Ruis**: Gegevens bevat onjuiste records of uitbijters.
* **Inconsistente**: Gegevens bevat conflicterende records of afwijkingen.

Kwaliteitsgegevens is een vereiste voor kwaliteit voorspellende modellen. Om te voorkomen 'garbagecollection in garbagecollection out' en de gegevenskwaliteit te verbeteren en daarom model prestaties, is het van cruciaal belang voor het uitvoeren van een scherm van de status gegevens om problemen met gegevens vroeg signaleren en te beslissen over de bijbehorende gegevens verwerken en opschonen stappen.

## <a name="what-are-some-typical-data-health-screens-that-are-employed"></a>Wat zijn enkele typische gegevens health schermen die worden gebruikt?
We kunt de algemene kwaliteit van de gegevens controleren door te controleren:

* Het aantal **records**.
* Het aantal **kenmerken** (of **functies**).
* Het kenmerk **gegevenstypen** (nominale, rangtelwoord of continue).
* Het aantal **ontbrekende waarden**.
* **-Opmaak** van de gegevens.
  * Als de gegevens zich bevinden in TSV- of CSV, controleert u dat de kolom scheidingstekens en regel scheidingstekens altijd goed scheiden van kolommen en regels.
  * Als de gegevens zich bevinden in HTML of XML-indeling, controleert u of de gegevens is goed samengesteld op basis van hun respectieve standaarden.
  * Parseren kan ook nodig zijn om gestructureerde informatie onttrekken aan semi-gestructureerde of ongestructureerde gegevens.
* **Inconsistente gegevensrecords**. Controleer het bereik van waarden zijn toegestaan. bijvoorbeeld als de gegevens student GPA, Controleer bevat of de GPA binnen het bereik is aangewezen, zeg 0 ~ 4.

Als u problemen met gegevens, **verwerkingsstappen** nodig die vaak omvat het opschonen van de ontbrekende waarden, gegevens normaliseren, onderscheidende, verwerking van de tekst te verwijderen en/of vervang ingesloten tekens die mogelijk van invloed op gegevens uitlijning, gemengde gegevenstypen in algemene velden en anderen.

**Azure Machine Learning verbruikt opgemaakte tabelgegevens**.  Als de gegevens zich al in tabelvorm, kan gegevens vooraf verwerken rechtstreeks met Azure Machine Learning in Machine Learning Studio en worden uitgevoerd.  Als gegevens niet in tabelvorm, zeg dat deel uitmaakt van XML-parsering mogelijk zijn vereist om te kunnen omzetten van de gegevens in tabelvorm.  

## <a name="what-are-some-of-the-major-tasks-in-data-pre-processing"></a>Wat zijn enkele van de belangrijkste taken in vooraf verwerken van gegevens?
* **Het opschonen van gegevens**:  Vul of ontbrekende waarden, detecteren en verwijderen van de ruis gegevens en de uitschieters.
* **Gegevenstransformatie**:  Normaliseren gegevens om dimensies en ruis te reduceren.
* **Gegevensreductie**:  Voorbeeldgegevensrecords of kenmerken voor eenvoudiger verwerking van gegevens.
* **Gegevens onderscheidende**:  Doorlopende kenmerken met bepaalde methoden van machine learning converteren naar categorische kenmerken voor het gebruiksgemak.
* **Tekst opschonen**: Verwijder de ingesloten tekens wat leiden gegevens in de uitlijning tot kunnen, zoals ingesloten tabbladen in een gegevensbestand met door tabs gescheiden ingesloten voor nieuwe regels die werken mogelijk niet meer records, enzovoort.

De volgende secties bevatten informatie over enkele van deze stappen gegevensverwerking.

## <a name="how-to-deal-with-missing-values"></a>Het omgaan met ontbrekende waarden?
Als u wilt omgaan met ontbrekende waarden, is het beste eerst het probleem bepalen de reden voor de ontbrekende waarden beter wordt verwerkt. Typische ontbrekende waarde verwerking methoden zijn:

* **Verwijdering**: Records met ontbrekende waarden te verwijderen
* **Testupdate worden uitgevoerd van vervanging**: Ontbrekende waarden vervangen door een dummy-waarde: bijvoorbeeld *onbekende* voor categorische of 0 voor numerieke waarden.
* **Vervanging betekenen**: Als de ontbrekende gegevens numeriek is, moet u de ontbrekende waarden vervangen door het gemiddelde.
* **Vervanging frequente**: Als de ontbrekende gegevens categorische is, vervangt u de ontbrekende waarden met de meest voorkomende item
* **Regressie vervanging**: Een regressiemethode ontbrekende waarden te vervangen door verminderde waarden gebruiken.  

## <a name="how-to-normalize-data"></a>Hoe kan ik gegevens normaliseren?
Gegevens normalisering past numerieke waarden aan een opgegeven bereik. Populaire normalisering methoden zijn onder andere:

* **Min-Max normalisering**: Lineair worden de gegevens transformeren naar een bereik, bijvoorbeeld tussen 0 en 1, waarbij de minimumwaarde is geschaald naar 0 en maximale waarde in 1.
* **Z-score normalisering**: Schalen op basis van gemiddelde en standaarddeviatie: het verschil tussen de gegevens en het gemiddelde delen door de standaarddeviatie.
* **Decimale schalen**: De gegevens door te verplaatsen van het decimaalteken van de kenmerkwaarde schalen.  

## <a name="how-to-discretize-data"></a>Hoe kunt u voor het onderscheiden van gegevens?
Gegevens kunnen worden onderscheiden door doorlopende waarden converteren naar de nominale kenmerken of intervallen. Er zijn enkele manieren waarop u dit doet:

* **Gelijke breedte Binning**: Het bereik van alle mogelijke waarden van een kenmerk onderverdelen in N groepen van dezelfde grootte en het toewijzen van de waarden die kunnen worden onderverdeeld in een opslaglocatie met het opslaglocatienummer van de.
* **Gelijk hoogte Binning**: Het bereik van alle mogelijke waarden van een kenmerk onderverdelen in N groepen, elk met hetzelfde aantal instanties en wijs vervolgens de waarden die kunnen worden onderverdeeld in een opslaglocatie met het opslaglocatienummer van de.  

## <a name="how-to-reduce-data"></a>Hoe kunt u het om gegevens te reduceren?
Er zijn verschillende methoden om gegevensgrootte voor de afhandeling van gegevens gemakkelijker te beperken. Afhankelijk van de grootte en het domein, kunnen de volgende methoden worden toegepast:

* **Vastleggen van steekproeven**: Voorbeeld van de records met gegevens en kiest u alleen de representatieve subset van de gegevens.
* **Steekproeven van het kenmerk**: Selecteer alleen een subset van de belangrijkste kenmerken van de gegevens.  
* **Aggregatie**: De gegevens verdelen in groepen en op te slaan van de getallen voor elke groep. De dagelijkse omzet-nummers van een restaurantketen in de afgelopen 20 jaar kan bijvoorbeeld worden geaggregeerd naar maandelijkse omzet om de grootte van de gegevens te beperken.  

## <a name="how-to-clean-text-data"></a>Hoe kunt u voor het opschonen van gegevens?
**Tekstvelden in gegevens in tabelvorm** tekens die invloed hebben op de grenzen van de uitlijning en/of de record van de kolommen kan bevatten. Voor bijvoorbeeld tabbladen in een bestand met door tabs gescheiden oorzaak kolom uitlijning ingesloten en ingesloten verbreken nieuwe-regeltekens record regels. Onjuiste tekst codering verwerking tijdens het schrijven/lezen tekst leidt tot gegevensverlies van, onbedoeld introductie van onleesbaar tekens, bijvoorbeeld nulwaarden en kan ook invloed op tekst te parseren. Zorg ervoor dat bij het parseren en het bewerken van mogelijk nodig om u te reinigen tekstvelden voor de juiste uitlijning en/of extract gestructureerde gegevens uit ongestructureerde of semi-gestructureerde tekstgegevens.

**Gegevens verkennen** biedt een vroege weergeven in de gegevens. Een aantal problemen met gegevens kan worden gezien tijdens deze stap en de bijbehorende methoden kunnen worden toegepast om die problemen op te lossen.  Het is belangrijk om te vragen, zoals wat is de oorzaak van het probleem en hoe het probleem kan zijn geïntroduceerd. Dit helpt u ook beslissen over de verwerking van data-stappen die worden genomen moeten om op te lossen. Het soort insights een zal worden afgeleid van de gegevens kan ook worden gebruikt om de inspanning gegevensverwerking prioriteit te geven.

## <a name="references"></a>Verwijzingen
> *Gegevensanalyse: Concepts and Techniques*, Third Edition, Morgan Kaufmann, 2011, Jiawei Han, Micheline Kamber, and Jian Pei
> 
> 

