---
title: Validatie activiteit in Azure Data Factory
description: De validatie activiteit blijft de uitvoering van de pijp lijn voortzetten totdat de gekoppelde gegevensset wordt gevalideerd met bepaalde criteria die de gebruiker opgeeft.
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 03/25/2019
ms.openlocfilehash: f63c78c59d7d6be3c66ea0785389eff73e3bff60
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73678371"
---
# <a name="validation-activity-in-azure-data-factory"></a>Validatie activiteit in Azure Data Factory
U kunt een validatie in een pijp lijn gebruiken om ervoor te zorgen dat de pijp lijn alleen de uitvoering voortzet wanneer het de gekoppelde gegevensset bevat, die aan de opgegeven criteria voldoet, of er is een time-out opgetreden.


## <a name="syntax"></a>Syntaxis

```json

{
    "name": "Validation_Activity",
    "type": "Validation",
    "typeProperties": {
        "dataset": {
            "referenceName": "Storage_File",
            "type": "DatasetReference"
        },
        "timeout": "7.00:00:00",
        "sleep": 10,
        "minimumSize": 20
    }
},
{
    "name": "Validation_Activity_Folder",
    "type": "Validation",
    "typeProperties": {
        "dataset": {
            "referenceName": "Storage_Folder",
            "type": "DatasetReference"
        },
        "timeout": "7.00:00:00",
        "sleep": 10,
        "childItems": true
    }
}

```


## <a name="type-properties"></a>Type-eigenschappen

Eigenschap | Beschrijving | Toegestane waarden | Vereist
-------- | ----------- | -------------- | --------
naam | Naam van de activiteit validatie | Tekenreeks | Ja |
type | Moet worden ingesteld op **validatie**. | Tekenreeks | Ja |
Sets | De uitvoering van de activiteit wordt geblokkeerd totdat deze de gegevensset bevat die aan de opgegeven criteria voldoet, of er is een time-out opgetreden. De gegeven gegevensset moet de eigenschap MinimumSize of ChildItems ondersteunen. | Verwijzing naar gegevensset | Ja |
timeout | Hiermee geeft u de time-out op voor de activiteit die moet worden uitgevoerd. Als er geen waarde is opgegeven, is de standaard waarde 7 dagen ("7.00:00:00"). De notatie is d. uu: mm: SS | Tekenreeks | Nee |
dwars | Een vertraging in seconden tussen de validatie pogingen. Als er geen waarde is opgegeven, is de standaard waarde 10 seconden. | Geheel getal | Nee |
childItems | Hiermee wordt gecontroleerd of de map onderliggende items heeft. Kan worden ingesteld op-True: Controleer of de map bestaat en of deze items bevat. Blokken tot ten minste één item aanwezig is in de map of de time-outwaarde is bereikt.-onwaar: Controleer of de map bestaat en of deze leeg is. Blokken totdat de map leeg is of totdat de time-outwaarde is bereikt. Als er geen waarde is opgegeven, wordt de activiteit geblokkeerd totdat de map bestaat of totdat de time-out is bereikt. | Booleaans | Nee |
minimumSize | Minimale grootte van een bestand in bytes. Als er geen waarde is opgegeven, is de standaard waarde 0 bytes | Geheel getal | Nee |


## <a name="next-steps"></a>Volgende stappen
Zie andere controle stroom activiteiten die door Data Factory worden ondersteund:

- [If Condition Activity](control-flow-if-condition-activity.md)
- [Execute Pipeline Activity](control-flow-execute-pipeline-activity.md)
- [Voor elke activiteit](control-flow-for-each-activity.md)
- [Get Metadata Activity](control-flow-get-metadata-activity.md)
- [Lookup Activity](control-flow-lookup-activity.md)
- [Webactiviteit](control-flow-web-activity.md)
- [Until Activity](control-flow-until-activity.md)
