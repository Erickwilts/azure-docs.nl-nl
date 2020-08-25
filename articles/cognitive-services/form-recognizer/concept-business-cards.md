---
title: Visite kaartjes-formulier herkenning
titleSuffix: Azure Cognitive Services
description: Leer concepten met betrekking tot analyse van visite kaartjes met het formulier Recognizer API-gebruik en limieten.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: conceptual
ms.date: 08/17/2019
ms.author: pafarley
ms.openlocfilehash: 039f7343bcef64db9ad9eae558cd3e97f3678c59
ms.sourcegitcommit: c5021f2095e25750eb34fd0b866adf5d81d56c3a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/25/2020
ms.locfileid: "88799278"
---
# <a name="business-card-concepts"></a>Concepten van visitekaartjes

Met een van de vooraf ontwikkelde modellen kan Azure Form Recognizer sleutel waardeparen analyseren en extra heren uit visite kaartjes. De Business Card-API combineert krachtige functies voor optische teken herkenning (OCR) met ons model voor visite kaartjes voor het extra heren van belang rijke informatie uit visite kaartjes in het Engels. Hiermee worden persoonlijke contact gegevens, bedrijfs naam, functie titel en meer opgehaald. De vooraf ontwikkelde Business Card-API is openbaar beschikbaar in de preview-versie van de formulier Recognizer v 2.1. 

## <a name="what-does-the-business-card-api-do"></a>Wat doet de Business Card API?

De Business Card-API extraheert belang rijke velden van visite kaartjes en retourneert deze in een geordend JSON-antwoord.

![Contoso-geitemde installatie kopie van FOTT + JSON-uitvoer](./media/business-card-english.jpg)

### <a name="fields-extracted"></a>Geëxtraheerde velden: 
* Namen van contact personen 
* Voornaam 
* Achternaam 
* Bedrijfs namen 
* Afdelingen 
* Taak titels 
* E-mails 
* Websites 
* Adressen 
* Telefoon nummers 
  * Mobiele telefoons 
  * Faxberichten 
  * Werk telefoons 
  * Andere telefoons 

De Business Card-API retourneert ook alle herkende tekst van de visite kaart. Deze OCR-uitvoer is opgenomen in het JSON-antwoord.  

### <a name="input-requirements"></a>Invoer vereisten 

[!INCLUDE [input reqs](./includes/input-requirements-receipts.md)]

## <a name="the-analyze-business-card-operation"></a>De bewerking visite kaartje analyseren

De [kaart](https://westcentralus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-1-preview-1/operations/AnalyzeBusinessCardAsync) voor het analyseren van een visite kaartje neemt een afbeelding of PDF van een visite kaartje als invoer en extraheert de waarden van interest en text. De aanroep retourneert een veld met een antwoord header met de naam `Operation-Location` . De `Operation-Location` waarde is een URL die de resultaat-id bevat die in de volgende stap moet worden gebruikt.

|Reactie header| Resultaten-URL |
|:-----|:----|
|Bewerkings locatie | `https://cognitiveservice/formrecognizer/v2.1-preview.1/prebuilt/businessCard/analyzeResults/49a36324-fc4b-4387-aa06-090cfbf0064f` |

## <a name="the-get-analyze-business-card-result-operation"></a>De resultaat bewerking voor het analyseren van bedrijfs kaarten ophalen

De tweede stap bestaat uit het aanroepen van de [resultaat bewerking analyse van bedrijfs kaart ophalen](https://westcentralus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-1-preview-1/operations/GetAnalyzeBusinessCardResult) . Met deze bewerking wordt de resultaat-ID gebruikt die is gemaakt door de bewerking visite kaartje analyseren. Er wordt een JSON-antwoord geretourneerd dat een **status** veld met de volgende mogelijke waarden bevat. U roept deze bewerking iteratief aan tot deze met de **geslaagde** waarde wordt geretourneerd. Gebruik een interval van 3 tot 5 seconden om te voor komen dat het aantal aanvragen per seconde (RPS) wordt overschreden.

|Veld| Type | Mogelijke waarden |
|:-----|:----:|:----|
|status | tekenreeks | notStarted: de analyse bewerking is niet gestart. |
| |  | uitvoeren: de analyse bewerking wordt uitgevoerd. |
| |  | mislukt: de analyse bewerking is mislukt. |
| |  | geslaagd: de analyse bewerking is voltooid. |

Wanneer het veld **status** de waarde **geslaagd** heeft, bevat het JSON-antwoord de resultaten van de kaart en tekst herkenning. Het resultaat van het visite kaartje is ingedeeld als een woorden lijst met benoemde veld waarden, waarbij elke waarde de geëxtraheerde tekst, genormaliseerde waarde, het begrenzingsvak, betrouw baarheid en bijbehorende woord elementen bevat. Het resultaat van de tekst herkenning is ingedeeld als een hiërarchie van lijnen en woorden, met tekst, selectie kader en betrouwbaarheids informatie.

![voorbeeld uitvoer van visite kaartjes](./media/business-card-results.png)

### <a name="sample-json-output"></a>Voor beeld van JSON-uitvoer

Zie het volgende voor beeld van een geslaagde JSON-reactie: het knoop punt ' readResults ' bevat alle herkende tekst. De tekst wordt geordend op pagina, vervolgens per regel en vervolgens op afzonderlijke woorden. Het knoop punt ' documentResults ' bevat de bedrijfsspecifieke waarden die het model heeft gedetecteerd. Hier vindt u nuttige sleutel/waarde-paren zoals de voor naam, achternaam, bedrijfs naam en nog veel meer.

```json
{
    "status": "succeeded",
    "createdDateTime": "2020-08-20T17:41:19Z",
    "lastUpdatedDateTime": "2020-08-20T17:41:24Z",
    "analyzeResult": {
        "version": "2.1.0",
        "readResults": [
            {
                "page": 1,
                "angle": -17.0956,
                "width": 4032,
                "height": 3024,
                "unit": "pixel",
                "lines": 
                          {
                        "text": "Dr. Avery Smith",
                        "boundingBox": [
                            419.3,
                            1154.6,
                            1589.6,
                            877.9,
                            1618.9,
                            1001.7,
                            448.6,
                            1278.4
                        ],
                        "words": [
                            {
                                "text": "Dr.",
                                "boundingBox": [
                                    419,
                            ]
    
            }
        ],
        "documentResults": [
            {
                "docType": "prebuilt:businesscard",
                "pageRange": [
                    1,
                    1
                ],
                "fields": {
                    "ContactNames": {
                        "type": "array",
                        "valueArray": [
                            {
                                "type": "object",
                                "valueObject": {
                                    "FirstName": {
                                        "type": "string",
                                        "valueString": "Avery",
                                        "text": "Avery",
                                        "boundingBox": [
                                            703,
                                            1096,
                                            1134,
                                            989,
                                            1165,
                                            1109,
                                            733,
                                            1206
                                        ],
                                        "page": 1
                                    },
                                    "LastName": {
                                        "type": "string",
                                        "valueString": "Smith",
                                        "text": "Smith",
                                        "boundingBox": [
                                            1186,
                                            976,
                                            1585,
                                            879,
                                            1618,
                                            998,
                                            1218,
                                            1096
                                        ],
                                        "page": 1
                                    }
                                },
                                "text": "Dr. Avery Smith",
                                "boundingBox": [
                                    419.3,
                                    1154.6,
                                    1589.6,
                                    877.9,
                                    1618.9,
                                    1001.7,
                                    448.6,
                                    1278.4
                                ],
                                "confidence": 0.97
                            }
                        ]
                    },
                    "JobTitles": {
                        "type": "array",
                        "valueArray": [
                            {
                                "type": "string",
                                "valueString": "Senior Researcher",
                                "text": "Senior Researcher",
                                "boundingBox": [
                                    451.8,
                                    1301.9,
                                    1313.5,
                                    1099.9,
                                    1333.8,
                                    1186.7,
                                    472.2,
                                    1388.7
                                ],
                                "page": 1,
                                "confidence": 0.99
                            }
                        ]
                    },
                    "Departments": {
                        "type": "array",
                        "valueArray": [
                            {
                                "type": "string",
                                "valueString": "Cloud & Al Department",
                                "text": "Cloud & Al Department",
                                "boundingBox": [
                                    480.1,
                                    1403.3,
                                    1590.5,
                                    1129.6,
                                    1612.6,
                                    1219.6,
                                    502.3,
                                    1493.3
                                ],
                                "page": 1,
                                "confidence": 0.99
                            }
                        ]
                    },
                    "Emails": {
                        "type": "array",
                        "valueArray": [
                            {
                                "type": "string",
                                "valueString": "avery.smith@contoso.com",
                                "text": "avery.smith@contoso.com",
                                "boundingBox": [
                                    2107,
                                    934,
                                    2917,
                                    696,
                                    2935,
                                    764,
                                    2126,
                                    995
                                ],
                                "page": 1,
                                "confidence": 0.99
                            }
                        ]
                    },
                    "Websites": {
                        "type": "array",
                        "valueArray": [
                            {
                                "type": "string",
                                "valueString": "https://www.contoso.com/",
                                "text": "https://www.contoso.com/",
                                "boundingBox": [
                                    2121,
                                    1002,
                                    2992,
                                    755,
                                    3014,
                                    826,
                                    2143,
                                    1077
                                ],
                                "page": 1,
                                "confidence": 0.995
                            }
                        ]
                    },
                    "MobilePhones": {
                        "type": "array",
                        "valueArray": [
                            {
                                "type": "phoneNumber",
                                "text": "+44 (0) 7911 123456",
                                "boundingBox": [
                                    2434.9,
                                    1033.3,
                                    3072,
                                    836,
                                    3096.2,
                                    914.3,
                                    2459.1,
                                    1111.6
                                ],
                                "page": 1,
                                "confidence": 0.99
                            }
                        ]
                    },
                    "OtherPhones": {
                        "type": "array",
                        "valueArray": [
                            {
                                "type": "phoneNumber",
                                "text": "+44 (0) 20 9876 5432",
                                "boundingBox": [
                                    2473.2,
                                    1115.4,
                                    3139.2,
                                    907.7,
                                    3163.2,
                                    984.7,
                                    2497.2,
                                    1192.4
                                ],
                                "page": 1,
                                "confidence": 0.99
                            }
                        ]
                    },
                    "Faxes": {
                        "type": "array",
                        "valueArray": [
                            {
                                "type": "phoneNumber",
                                "text": "+44 (0) 20 6789 2345",
                                "boundingBox": [
                                    2525,
                                    1185.4,
                                    3192.4,
                                    977.9,
                                    3217.9,
                                    1060,
                                    2550.5,
                                    1267.5
                                ],
                                "page": 1,
                                "confidence": 0.99
                            }
                        ]
                    },
                    "Addresses": {
                        "type": "array",
                        "valueArray": [
                            {
                                "type": "string",
                                "valueString": "2 Kingdom Street Paddington, London, W2 6BD",
                                "text": "2 Kingdom Street Paddington, London, W2 6BD",
                                "boundingBox": [
                                    1230,
                                    2138,
                                    2535.2,
                                    1678.6,
                                    2614.2,
                                    1903.1,
                                    1309,
                                    2362.5
                                ],
                                "page": 1,
                                "confidence": 0.977
                            }
                        ]
                    },
                    "CompanyNames": {
                        "type": "array",
                        "valueArray": [
                            {
                                "type": "string",
                                "valueString": "Contoso",
                                "text": "Contoso",
                                "boundingBox": [
                                    1152,
                                    1916,
                                    2293,
                                    1552,
                                    2358,
                                    1733,
                                    1219,
                                    2105
                                ],
                                "page": 1,
                                "confidence": 0.97
                            }
                        ]
                    }
                }
            }
        ]
    }
}
```

Volg de Snelstartgids voor het [extra heren van gegevens van visite kaartjes](./QuickStarts/python-business-cards.md) voor het implementeren van gegevens extractie met behulp van python en de rest API.

## <a name="customer-scenarios"></a>Klant Scenario's  

De gegevens die zijn geëxtraheerd met de Business Card API kunnen worden gebruikt voor het uitvoeren van verschillende taken. Als u deze contact gegevens uitpakt, bespaart u automatisch tijd voor die in client gerichte rollen. Hier volgen enkele voor beelden van wat onze klanten hebben gedaan met de API van het visite kaartje:

* Haal contact gegevens op uit visite kaartjes en maak snel contact met uw telefoon. 
* Integreer met CRM om automatisch contact te maken met installatie kopieën van visite kaartjes. 
* Houd de verkoop kansen bij.  
* Haal contact gegevens in bulk op uit bestaande afbeeldingen van visite kaartjes. 

De Business Card-API voorziet ook in de [verwerkings functie voor AIBuilder-bedrijfs kaarten](https://docs.microsoft.com/ai-builder/prebuilt-business-card).

## <a name="next-steps"></a>Volgende stappen

- Volg de Snelstartgids om aan de slag te gaan met de [Snelstartgids voor Business Cards API python](./quickstarts/python-business-cards.md)
- Meer informatie over de [rest API voor formulier herkenning](https://westcentralus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-1-preview-1/operations/AnalyzeBusinessCardAsync)
- Meer informatie over de [formulier herkenning](overview.md)


