---
title: Benoemde entiteiten voor gezondheids zorg
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: include
ms.date: 10/02/2020
ms.author: aahi
ms.openlocfilehash: 44bce7c6dd2443ea0ed1851325a5b84f325f5fae
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "91779260"
---
## <a name="text-analytics-for-health-categories-entities-and-attributes"></a>Text Analytics voor status Categorieën, entiteiten en kenmerken

In [Text Analytics status](../../how-tos/text-analytics-for-health.md) worden medische concepten in de volgende categorieën gedetecteerd.  (Alleen Engelse tekst wordt ondersteund in deze container preview en er wordt slechts één model versie in elke container installatie kopie gegeven.)


| Categorie  | Beschrijving  |
|---------|---------|
| STRUCTUUR | concepten die informatie vastleggen over hoofd-en Anatomic-systemen,-sites,-locaties of-regio's. |
 | DEMOGRAFISCHE gegevens | concepten die informatie over geslacht en leeftijd vastleggen. |
 | BESTUDEREN | concepten die informatie vastleggen over diagnostische procedures en tests. |
 | GENOMICS | concepten die informatie over genen en varianten vastleggen. |
 | GEZONDHEIDS zorg | concepten die informatie vastleggen over administratieve gebeurtenissen, careische omgevingen en gezondheids zorg. |
 | MEDISCHE VOOR WAARDE | concepten die informatie vastleggen over diagnoses, symptomen of symptomen. |
 | GENEES | concepten die informatie over medicijnen vastleggen, waaronder medicijnen namen, klassen, dosering en route van beheer. |
 | BURGER | concepten die informatie vastleggen over medische relevante sociale aspecten, zoals familie relatie. |
 | GERICHT | concepten die informatie over therapeutische procedures vastleggen. |
  
Elke categorie kan twee concept groepen bevatten:

* **Entiteiten** -termen die medische concepten vastleggen, zoals diagnose, medicijnen naam of behandelings naam.  *Bronchitis* is bijvoorbeeld een diagnose en *aspirin* is een medicijnen naam.  Vermeldingen in deze groep kunnen worden gekoppeld aan een UMLS concept-ID.
* **Kenmerken** : zinnen die meer informatie bieden over de gedetecteerde entiteit, bijvoorbeeld *ernstige* , is een voor waarde kwalificatie voor *bronchitis* of *81 mg* is een dosering voor *aspirin*.  Vermeldingen in deze categorie worden niet gekoppeld aan een UMLS concept-ID.

Daarnaast herkent de service relaties tussen de verschillende concepten, met inbegrip van relaties tussen kenmerken en entiteiten, zoals de *richting* van de *hoofd structuur* of de *dosering* van *medicijn naam* en relaties tussen entiteiten, bijvoorbeeld afkortings detectie.

## <a name="anatomy"></a>Structuur

### <a name="entities"></a>Entiteiten

**BODY_STRUCTURE** -hoofd systemen, Anatomic locaties of regio's en hoofd sites. Bijvoorbeeld arm, knie, abdomen, neus, lever, hoofd, ademhalings systeem, lymphocytes.

:::image type="content" source="../../media/ta-for-health/anatomy-entities-body-structure.png" alt-text="Een voor beeld van de entiteit body-structuur.":::


:::image type="content" source="../../media/ta-for-health/anatomy-entities-body-structure-2.png" alt-text="Een voor beeld van de entiteit body-structuur.":::

### <a name="attributes"></a>Kenmerken

**Richtings** voorwaarden, zoals: Left, lateraal, Upper, posterior, die een carrosserie structuur kenmerkt.

:::image type="content" source="../../media/ta-for-health/anatomy-attributes.png" alt-text="Een voor beeld van de entiteit body-structuur.":::

### <a name="supported-relations"></a>Ondersteunde relaties

* **DIRECTION_OF_BODY_STRUCTURE**

## <a name="demographics"></a>Demografische gegevens

### <a name="entities"></a>Entiteiten

**Leeftijd** : alle leeftijds voorwaarden en zinsdelen, inclusief die van een patiënt, familie leden en anderen. Bijvoorbeeld 40-jaar-oud, 51 yo, 3 maanden oud, volwassene, zuigelingen, oude leeftijd, Young, secundair, Midden-verouderd.

:::image type="content" source="../../media/ta-for-health/age-entity.png" alt-text="Een voor beeld van de entiteit body-structuur.":::

:::image type="content" source="../../media/ta-for-health/age-entity-2.png" alt-text="Een voor beeld van de entiteit body-structuur.":::


**Gender** -termen die het geslacht van het onderwerp vermelden. Bijvoorbeeld mannelijk, vrouwelijk, vrouw, zachtman, vrouw.

:::image type="content" source="../../media/ta-for-health/gender-entity.png" alt-text="Een voor beeld van de entiteit body-structuur.":::

### <a name="attributes"></a>Kenmerken

**RELATIONAL_OPERATOR** -zinnen die de relatie tussen een demografische entiteit en aanvullende informatie uitdrukken.

:::image type="content" source="../../media/ta-for-health/relational-operator.png" alt-text="Een voor beeld van de entiteit body-structuur.":::

## <a name="examinations"></a>Onderzoek

### <a name="entities"></a>Entiteiten

**EXAMINATION_NAME** : diagnostische procedures en tests. Bijvoorbeeld, MRI, ECG, HIV test, HEMOGLOBIN, aantal Trombocyten, schaal systemen zoals Bristol van de *stoel schalen*.

:::image type="content" source="../../media/ta-for-health/exam-name-entities.png" alt-text="Een voor beeld van de entiteit body-structuur.":::

:::image type="content" source="../../media/ta-for-health/exam-name-entities-2.png" alt-text="Een voor beeld van de entiteit body-structuur.":::

### <a name="attributes"></a>Kenmerken

**Direction** -directionele termen die een onderzoek kenmerkt.

:::image type="content" source="../../media/ta-for-health/exam-direction-attribute.png" alt-text="Een voor beeld van de entiteit body-structuur.":::

**MEASUREMENT_UNIT** : de eenheid van het onderzoek. Voor beeld: in *hemoglobin > 9,5 g/DL*is de term *g/DL* de eenheid voor de test *HEMOGLOBIN* .

:::image type="content" source="../../media/ta-for-health/exam-unit-attribute.png" alt-text="Een voor beeld van de entiteit body-structuur.":::

**MEASUREMENT_VALUE** : de waarde van het onderzoek. In *hemoglobin > 9,5 g/DL*is de term *9,5* bijvoorbeeld de waarde voor de *HEMOGLOBIN* -test.

:::image type="content" source="../../media/ta-for-health/exam-value-attribute.png" alt-text="Een voor beeld van de entiteit body-structuur.":::

**RELATIONAL_OPERATOR** : zinsdelen die de relatie tussen een onderzoek en aanvullende informatie uitdrukken. Bijvoorbeeld de vereiste meet waarde voor een doel onderzoek.

:::image type="content" source="../../media/ta-for-health/exam-relational-operator-attribute.png" alt-text="Een voor beeld van de entiteit body-structuur.":::

**Tijd** – tijdelijke termen met betrekking tot het begin en/of de lengte (duur) van een onderzoek. Bijvoorbeeld wanneer de test is uitgevoerd.

:::image type="content" source="../../media/ta-for-health/exam-time-attribute.png" alt-text="Een voor beeld van de entiteit body-structuur.":::

### <a name="supported-relations"></a>Ondersteunde relaties

+ **DIRECTION_OF_EXAMINATION**
+   **RELATION_OF_EXAMINATION**
+   **TIME_OF_EXAMINATION**
+   **UNIT_OF_EXAMINATION**
+   **VALUE_OF_EXAMINATION**

## <a name="genomics"></a>Genomics

### <a name="entities"></a>Entiteiten

**Gen** : alle vermeldingen van genen. Bijvoorbeeld, MTRR, F2.

:::image type="content" source="../../media/ta-for-health/genomics-entities.png" alt-text="Een voor beeld van de entiteit body-structuur.":::

**Variant** : alle vermeldingen van gen-variaties. Bijvoorbeeld c. 524C>T, (MTRR): r.1462_1557del96
  
## <a name="healthcare"></a>Gezondheidszorg

### <a name="entities"></a>Entiteiten
  
**ADMINISTRATIVE_EVENT** : gebeurtenissen die betrekking hebben op het gezondheids systeem, maar van een administratieve/semi-administratieve aard. Bijvoorbeeld registratie, toelating, proef versie, studie vermelding, overdracht, kwijting, hospitalization, ziekenhuis verblijf. 

:::image type="content" source="../../media/ta-for-health/healthcare-event-entity.png" alt-text="Een voor beeld van de entiteit body-structuur.":::

**CARE_ENVIRONMENT** : een omgeving of locatie waar patiënten zorg voor zijn. Bijvoorbeeld: Emergency Room, kantoor van artsen, cardio unit, Hospice, zieken huis.

:::image type="content" source="../../media/ta-for-health/healthcare-environment-entity.png" alt-text="Een voor beeld van de entiteit body-structuur.":::

**HEALTHCARE_PROFESSION** : een gezondheids zorg waarbij een licentie wordt verleend of waarvoor de licentie niet is toegestaan. Bijvoorbeeld tand arts, pathologist, neurologist, radiologist, pharmacist, voeding, fysieke Therapist, chiropractor.

:::image type="content" source="../../media/ta-for-health/healthcare-profession-entity.png" alt-text="Een voor beeld van de entiteit body-structuur.":::

:::image type="content" source="../../media/ta-for-health/healthcare-profession-entity-2.png" alt-text="Een voor beeld van de entiteit body-structuur.":::

## <a name="medical-condition"></a>Medische voor waarde

### <a name="entities"></a>Entiteiten

**Diagnose** : ziekte, Syndrome, poisoning. Bijvoorbeeld, Alzheimer, HTN, CHF, de snelheid van het ruggen merg.

:::image type="content" source="../../media/ta-for-health/medical-condition-entity.png" alt-text="Een voor beeld van de entiteit body-structuur.":::

:::image type="content" source="../../media/ta-for-health/medical-condition-entity-2.png" alt-text="Een voor beeld van de entiteit body-structuur.":::

**SYMPTOM_OR_SIGN** : onderhevige of objectieve bewijzen van ziekte of andere onderzoeken. Een voor beeld: een borst pijn, pijn, dizziness, Rash, SOB, abdomen was zacht, goede bowel sounds en goed nourished.

:::image type="content" source="../../media/ta-for-health/medical-condition-symptom-entity.png" alt-text="Een voor beeld van de entiteit body-structuur.":::

:::image type="content" source="../../media/ta-for-health/medical-condition-symptom-entity-2.png" alt-text="Een voor beeld van de entiteit body-structuur.":::

### <a name="attributes"></a>Kenmerken

Voor waarden voor **CONDITION_QUALIFIER** kwaliteit die worden gebruikt om een medische voor waarde te beschrijven. Alle volgende subcategorieën worden beschouwd als kwalificaties:

1.  Time-gerelateerde expressies: Dit zijn termen die de tijd dimensie kwalitatief beschrijven, zoals plotseling, accent aigu, chronische, spant. 
2.  Kwaliteits expressies: Dit zijn termen die de aard van de medische voor waarde beschrijven, zoals verbranding, scherp.
3.  Ernst expressies: ernstig, mild, een beetje, niet-beheerd.
4.  Extensivity expressies: Local, brand, diffuus.
5.  Stralings expressies: elektromagnetische, straling.
6.  Schaal van voor waarde: in sommige gevallen wordt een voor waarde gekenmerkt door een schaal, wat een eindige, geordende lijst met waarden is. Bijvoorbeeld patiënten met fase III pancreatic kanker.
7.  Voorwaarde cursus: een term die betrekking heeft op de cursus of de voortgang van een voor waarde, zoals verbetering, verslechtering, oplossing, kwijt schel ding. 

:::image type="content" source="../../media/ta-for-health/condition-qualifier-diagnosis.png" alt-text="Een voor beeld van de entiteit body-structuur.":::

:::image type="content" source="../../media/ta-for-health/condition-qualifier-diagnosis-2.png" alt-text="Een voor beeld van de entiteit body-structuur.":::

:::image type="content" source="../../media/ta-for-health/conditional-qualifier-symptom-medication.png" alt-text="Een voor beeld van de entiteit body-structuur.":::

:::image type="content" source="../../media/ta-for-health/condition-qualifier-diagnosis-3.png" alt-text="Een voor beeld van de entiteit body-structuur.":::

:::image type="content" source="../../media/ta-for-health/condition-qualifier-symptom.png" alt-text="Een voor beeld van de entiteit body-structuur.":::

**Richtings** termen die een medische voor waarde voor een lichaam kenmerken.

:::image type="content" source="../../media/ta-for-health/medical-condition-direction-attribute.png" alt-text="Een voor beeld van de entiteit body-structuur.":::

**Frequentie** : hoe vaak een medische situatie zich voordeed, zich voordoet of zich voordoet.

:::image type="content" source="../../media/ta-for-health/medical-condition-frequency-attribute.png" alt-text="Een voor beeld van de entiteit body-structuur.":::

:::image type="content" source="../../media/ta-for-health/medical-condition-frequency-attribute-2.png" alt-text="Een voor beeld van de entiteit body-structuur.":::

**MEASUREMENT_UNIT** -de eenheid die een medische voor waarde kenmerkt. In *1,5 x2x1 cm tumor*is de term *cm* bijvoorbeeld de maat eenheid voor de *tumor*. 

:::image type="content" source="../../media/ta-for-health/medical-condition-measure-unit-attribute.png" alt-text="Een voor beeld van de entiteit body-structuur.":::

**MEASUREMENT_VALUE** : de waarde die een medische voor waarde kenmerkt. Bijvoorbeeld: in *1,5 x2x1 cm tumor*is de term *1,5 x2x1* de meet waarde voor de *tumor*. 

:::image type="content" source="../../media/ta-for-health/medical-condition-measure-value-attribute.png" alt-text="Een voor beeld van de entiteit body-structuur.":::

**RELATIONAL_OPERATOR** -zinsdelen waarmee de relatie tussen de medische voor waarde wordt versneld. Bijvoorbeeld een tijd-of meet waarde. 

:::image type="content" source="../../media/ta-for-health/medical-condition-relational-operator.png" alt-text="Een voor beeld van de entiteit body-structuur.":::

**Tijdgebonden voor** waarden met betrekking tot het begin en/of de lengte (duur) van een medische voor waarde. Bijvoorbeeld wanneer een symptoom is gestart (begin) of wanneer er een ziekte heeft plaatsgevonden.

:::image type="content" source="../../media/ta-for-health/medical-condition-time-attribute.png" alt-text="Een voor beeld van de entiteit body-structuur.":::

### <a name="supported-relations"></a>Ondersteunde relaties

+ **DIRECTION_OF_CONDITION**
+   **QUALIFIER_OF_CONDITION**
+   **TIME_OF_CONDITION**
+   **UNIT_OF_CONDITION**
+   **VALUE_OF_CONDITION**

## <a name="medication"></a>Genees

### <a name="entities"></a>Entiteiten

**MEDICATION_CLASS** : een reeks medicijnen met een soortgelijk actie mechanisme, een gerelateerde werk wijze, een vergelijk bare chemische structuur en/of worden gebruikt om dezelfde ziekte te behandelen. Bijvoorbeeld ACE-remmen, Opioid, antibiotica, pijn ontlasters.

:::image type="content" source="../../media/ta-for-health/medication-entities-class.png" alt-text="Een voor beeld van de entiteit body-structuur.":::

**MEDICATION_NAME** : medicijn vermeldingen, waaronder merk namen van auteurs rechten en niet-merk namen. Bijvoorbeeld, Advil, ibuprofen.

:::image type="content" source="../../media/ta-for-health/medication-entities-name.png" alt-text="Een voor beeld van de entiteit body-structuur.":::

### <a name="attributes"></a>Kenmerken

**Dosering** : de hoeveelheid medicijnen die is besteld. Bijvoorbeeld: natriumhydroxide natrium chloride-oplossing *1000 ml*.

:::image type="content" source="../../media/ta-for-health/medication-dosage.png" alt-text="Een voor beeld van de entiteit body-structuur.":::

**Frequentie** : hoe vaak een medicijnen moet worden genomen.

:::image type="content" source="../../media/ta-for-health/medication-frequency.png" alt-text="Een voor beeld van de entiteit body-structuur.":::

:::image type="content" source="../../media/ta-for-health/medication-frequency-2.png" alt-text="Een voor beeld van de entiteit body-structuur.":::

**MEDICATION_FORM** : de vorm van de medicijnen. Bijvoorbeeld oplossing, Pill, capsule, Tablet, patch, gel, paste, schuim, spuit, drup pels, room, stroop.

:::image type="content" source="../../media/ta-for-health/medication-form.png" alt-text="Een voor beeld van de entiteit body-structuur.":::

**MEDICATION_ROUTE** : de administratieve methode van medicijnen. Bijvoorbeeld, oraal, Vaginal, IV, epidural, onderwerpf, inademing.

:::image type="content" source="../../media/ta-for-health/medication-route.png" alt-text="Een voor beeld van de entiteit body-structuur.":::

**RELATIONAL_OPERATOR** -zinnen die de relatie tussen medicijnen en aanvullende informatie uitdrukken. Bijvoorbeeld de vereiste meet waarde.

:::image type="content" source="../../media/ta-for-health/medication-relational-operator.png" alt-text="Een voor beeld van de entiteit body-structuur.":::

:::image type="content" source="../../media/ta-for-health/medication-time.png" alt-text="Een voor beeld van de entiteit body-structuur.":::

### <a name="supported-relations"></a>Ondersteunde relaties

+ **DOSAGE_OF_MEDICATION**
+   **FORM_OF_MEDICATION**
+   **FREQUENCY_OF_MEDICATION**
+   **ROUTE_OF_MEDICATION**
+   **TIME_OF_MEDICATION**
  
## <a name="treatment"></a>Gericht

### <a name="entities"></a>Entiteiten

**TREATMENT_NAME** : therapeutische procedures. Bijvoorbeeld, chirurgische, beenmerg Transplant, TAVI, dieet.

:::image type="content" source="../../media/ta-for-health/treatment-entities-name.png" alt-text="Een voor beeld van de entiteit body-structuur.":::

### <a name="attributes"></a>Kenmerken

**Richtings** termen die een behandeling kenmerkt.

:::image type="content" source="../../media/ta-for-health/treatment-direction.png" alt-text="Een voor beeld van de entiteit body-structuur.":::

**Frequentie** : hoe vaak een behandeling plaatsvindt of zich moet voordoen.

:::image type="content" source="../../media/ta-for-health/treatment-frequency.png" alt-text="Een voor beeld van de entiteit body-structuur.":::
 
**RELATIONAL_OPERATOR** -zinnen die de relatie tussen behandeling en aanvullende informatie uitdrukken.  Bijvoorbeeld hoeveel tijd er is verstreken van de vorige procedure.

:::image type="content" source="../../media/ta-for-health/treatment-relational-operator.png" alt-text="Een voor beeld van de entiteit body-structuur.":::

**Tijdgebonden voor** waarden met betrekking tot het begin en/of de lengte (duur) van een behandeling. Bijvoorbeeld de datum waarop de behandeling is gegeven.

:::image type="content" source="../../media/ta-for-health/treatment-time.png" alt-text="Een voor beeld van de entiteit body-structuur.":::


### <a name="supported-relations"></a>Ondersteunde relaties

+ **DIRECTION_OF_TREATMENT**
+   **TIME_OF_TREATMENT**
+   **FREQUENCY_OF_TREATMENT**

## <a name="social"></a>Sociaal netwerken

### <a name="entities"></a>Entiteiten

**FAMILY_RELATION** – vermeldingen van familie leden van het onderwerp. Bijvoorbeeld: vader, dochter, item op hetzelfde niveau, ouders.

:::image type="content" source="../../media/ta-for-health/family-relation.png" alt-text="Een voor beeld van de entiteit body-structuur.":::
