---
title: Azure Machine Learning-Anomaliedetectie-API - Team Data Science Process
description: API voor afwijkingsdetectie is een voorbeeld gebouwd met Microsoft Azure Machine Learning waarmee afwijkingen worden gedetecteerd in time series-gegevens met numerieke waarden zijn gelijkmatig verdeeld in de tijd.
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 06/05/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=alokkirpal, previous-ms.author=alok
ms.openlocfilehash: 16f13cd4ad580ea2f163fe87b5924c1462890972
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/12/2019
ms.locfileid: "64926186"
---
# <a name="machine-learning-anomaly-detection-api"></a>Machine Learning-Anomaliedetectie-API

> [!NOTE]
> Dit item is onderhouds werkzaamheden. We raden u aan de afwijkende [API-service](https://azure.microsoft.com/services/cognitive-services/anomaly-detector/) te gebruiken die wordt ondersteund door een galerie met machine learning-algoritmen onder Azure Cognitive Services om afwijkingen van bedrijfs-, operationele en IOT-gegevens te detecteren.

## <a name="overview"></a>Overzicht
[API voor afwijkingsdetectie](https://gallery.cortanaintelligence.com/MachineLearningAPI/Anomaly-Detection-2) volgt een voorbeeld gebouwd met Azure Machine Learning waarmee afwijkingen worden gedetecteerd in time series-gegevens met numerieke waarden zijn gelijkmatig verdeeld in de tijd.

Deze API kan de volgende typen afwijkende patronen in time series-gegevens detecteren:

* **Positieve en negatieve trends**: Wanneer het geheugen gebruik in het berekenen van een opwaartse trend bijvoorbeeld kan worden bewaken, kan dit van belang zijn, omdat dit mogelijk indicatief is voor een geheugenlek,
* **Wijzigingen in het dynamische waarden bereik**: Bij het bewaken van de uitzonde ringen die worden veroorzaakt door een Cloud service, kunnen wijzigingen in het dynamische bereik van waarden bijvoorbeeld duiden op instabiliteit in de status van de service, en
* **Pieken en spannings dips**: Bij het bewaken van het aantal mislukte aanmeldingen in een service of het aantal afhandelingen in een e-commerce site kunnen pieken of spannings dips bijvoorbeeld duiden op abnormaal gedrag.

Dergelijke wijzigingen in de waarden bijhouden gedurende een tijd en rapport doorlopende wijzigingen in hun waarden van deze machine learning-detectoren als anomaliedetectie scores. Ze vereisen geen ad-hoc drempelwaarde afstemmen en hun scores kunnen worden gebruikt voor het beheren van fout-positief-ratio. De anomaliedetectie-API handig in verschillende scenario's is zoals servicecontrole door bij te houden van KPI's na verloop van tijd gebruiksbewaking via metrische gegevens zoals het aantal zoekopdrachten, aantal klikken, door items, zoals geheugen, CPU, bewaking van toepassingsprestaties bestand wordt gelezen, enzovoort. na verloop van tijd.

De Anomaliedetectie-aanbieding wordt geleverd met handige's aan de slag te gaan.

* De [webtoepassing](https://anomalydetection-aml.azurewebsites.net/) helpt u bij het evalueren en de resultaten van de anomaliedetectie-API's voor uw gegevens te visualiseren.

> [!NOTE]
> Probeer **oplossing IT Anomaly Insights** mogelijk gemaakt door [deze API](https://gallery.cortanaintelligence.com/MachineLearningAPI/Anomaly-Detection-2)
>
<!-- This Solution is no longer available
> To get this end to end solution deployed to your Azure subscription <a href="https://gallery.cortanaintelligence.com/Solution/Anomaly-Detection-Pre-Configured-Solution-1" target="_blank">**Start here >**</a>
-->

## <a name="api-deployment"></a>API-implementatie
Als u wilt gebruiken de API, moet u deze implementeren naar uw Azure-abonnement waar deze wordt gehost als Azure Machine Learning-webservice.  U kunt dit doen vanuit de [Azure AI Gallery](https://gallery.cortanaintelligence.com/MachineLearningAPI/Anomaly-Detection-2).  Hiermee worden twee Azure Machine Learning Studio-webservices (en hun gerelateerde resources) geïmplementeerd in uw Azure-abonnement, een voor anomalie detectie met seizoensgebonden detectie en één zonder de seizoensgebonden detectie.  Zodra de implementatie is voltooid, kunt u uw Api's beheren via de pagina [Azure machine learning Studio Web Services](https://services.azureml.net/webservices/) .  Op deze pagina zich kunt u kunnen vinden van uw eindpuntlocaties, API-sleutels, evenals voorbeeldcode voor het aanroepen van de API.  Meer gedetailleerde instructies vindt u [hier](https://docs.microsoft.com/azure/machine-learning/machine-learning-manage-new-webservice).

## <a name="scaling-the-api"></a>Schalen van de API
Uw implementatie heeft standaard een gratis Dev/Test-abonnement met inbegrip van 1000 transacties per maand en 2 compute uur/maand.  U kunt upgraden naar een ander abonnement aan de hand van uw behoeften.  Meer informatie over de prijzen van verschillende abonnementen zijn beschikbaar [hier](https://azure.microsoft.com/pricing/details/machine-learning/) onder de 'Web API voor productie prijsinformatie'.

## <a name="managing-aml-plans"></a>Beheren van AML-abonnementen
U kunt uw abonnement beheren [hier](https://services.azureml.net/plans/).  Naam van het plan zal worden gebaseerd op de Resourcegroepnaam die u hebt gekozen bij het implementeren van de API plus een tekenreeks is die uniek is voor uw abonnement.  Instructies over het bijwerken van uw abonnement zijn beschikbaar [hier](https://docs.microsoft.com/azure/machine-learning/machine-learning-manage-new-webservice) onder de sectie 'Abonnementen beheren'.

## <a name="api-definition"></a>API-definitie
De webservice biedt een REST-API via HTTPS die kan worden gebruikt op verschillende manieren, met inbegrip van een web- of mobiele toepassing, R, Python, Excel, enzovoort.  U uw time series-gegevens verzenden naar deze service via een REST-API-aanroep en het uitvoeren van een combinatie van de drie anomaliedetectie-typen die hieronder worden beschreven.

## <a name="calling-the-api"></a>De API aanroepen
Als u wilt de API aanroepen, moet u weten de eindpuntlocatie en de API-sleutel.  Beide, samen met voorbeeld code voor het aanroepen van de API, zijn beschikbaar op de pagina [Azure machine learning Studio Web Services](https://services.azureml.net/webservices/) .  Navigeer naar de gewenste API en klik vervolgens op het tabblad "Verbruiken" voor deze.  Houd er rekening mee dat u de API als een Swagger-API kunt aanroepen (dat wil zeggen met de URL-parameter `format=swagger`) of als een niet - Swagger API (dat wil zeggen zonder de `format` URL-parameter).  De voorbeeldcode maakt gebruik van de Swagger-indeling.  Hieronder volgt een van de voorbeeldaanvraag en respons in niet-Swagger-indeling.  Deze voorbeelden zijn met het eindpunt seizoensgebondenheid.  Het eindpunt niet seizoensgebonden is vergelijkbaar.

### <a name="sample-request-body"></a>De hoofdtekst van de voorbeeld-aanvraag
De aanvraag bevat twee objecten: `Inputs` en `GlobalParameters`.  In de onderstaande voorbeeldaanvraag enkele parameters worden verzonden, expliciet terwijl andere niet (Schuif omlaag voor een volledige lijst met parameters voor elk eindpunt).  Parameters die niet worden verzonden, expliciet in de aanvraag gebruikt de standaardwaarden staan hieronder vermeld.

    {
                "Inputs": {
                        "input1": {
                                "ColumnNames": ["Time", "Data"],
                                "Values": [
                                        ["5/30/2010 18:07:00", "1"],
                                        ["5/30/2010 18:08:00", "1.4"],
                                        ["5/30/2010 18:09:00", "1.1"]
                                ]
                        }
                },
        "GlobalParameters": {
            "tspikedetector.sensitivity": "3",
            "zspikedetector.sensitivity": "3",
            "bileveldetector.sensitivity": "3.25",
            "detectors.spikesdips": "Both"
        }
    }

### <a name="sample-response"></a>Voorbeeldantwoord
Houd er rekening mee dat, als u wilt bekijken de `ColumnNames` veld, die u moet opnemen `details=true` als een URL-parameter in uw aanvraag.  Zie de onderstaande tabellen voor de zin achter elk van deze velden.

    {
        "Results": {
            "output1": {
                "type": "table",
                "value": {
                    "Values": [
                        ["5/30/2010 6:07:00 PM", "1", "1", "0", "0", "-0.687952590518378", "0", "-0.687952590518378", "0", "-0.687952590518378", "0"],
                        ["5/30/2010 6:08:00 PM", "1.4", "1.4", "0", "0", "-1.07030497733224", "0", "-0.884548154298423", "0", "-1.07030497733224", "0"],
                        ["5/30/2010 6:09:00 PM", "1.1", "1.1", "0", "0", "-1.30229513613974", "0", "-1.173800281031", "0", "-1.30229513613974", "0"]
                    ],
                    "ColumnNames": ["Time", "OriginalData", "ProcessedData", "TSpike", "ZSpike", "BiLevelChangeScore", "BiLevelChangeAlert", "PosTrendScore", "PosTrendAlert", "NegTrendScore", "NegTrendAlert"],
                    "ColumnTypes": ["DateTime", "Double", "Double", "Double", "Double", "Double", "Int32", "Double", "Int32", "Double", "Int32"]
                }
            }
        }
    }


## <a name="score-api"></a>Score-API
De Score-API wordt gebruikt voor het uitvoeren van detectie van afwijkingen op niet-seizoensgebonden time series-gegevens. De API wordt een aantal anomaliedetectie detectoren uitgevoerd op de gegevens en retourneert de anomaliedetectie-scores.
De afbeelding hieronder toont een voorbeeld van afwijkingen die kunnen worden gedetecteerd door de Score-API. Deze tijdreeks biedt verschillende niveau wijzigingen in 2 en 3 pieken. De rode punten geven de tijd waarop het niveau wijziging wordt gedetecteerd, terwijl de zwarte stippen de gedetecteerde pieken geven.
![Score-API][1]

### <a name="detectors"></a>Detectoren
De anomaliedetectie-API biedt ondersteuning voor detectoren in 3 hoofdcategorieën. Meer informatie over de specifieke parameters voor invoer en uitvoer voor elke detector vindt u in de volgende tabel.

| Categorie detector | Detector | Description | Invoerparameters | Uitvoer |
| --- | --- | --- | --- | --- |
| Piek detectoren |TSpike Detector |Pieken en dips n op basis van veel van de waarden afkomstig zijn van de eerste en derde kwartielen detecteren |*tspikedetector. sensitivity:* Hiermee wordt een geheel getal in het bereik 1-10, standaard waarde, gebruikt: 3D Bij hogere waarden worden er meer extreme waarden onderschept waardoor deze minder gevoelig zijn |TSpike: binaire waarden: '1' als er een piek/dip wordt gedetecteerd, '0' anders |
| Piek detectoren | ZSpike Detector |Pieken en dips n op basis van hoe ver de datapoints afkomstig van hun gemiddelde zijn detecteren |*zspikedetector. sensitivity:* waarde van geheel getal in het bereik 1-10 opgeven, standaard: 3D Bij hogere waarden worden er meer extreme waarden onderschept waardoor deze minder gevoelig zijn |ZSpike: binaire waarden: '1' als er een piek/dip wordt gedetecteerd, '0' anders |
| Trage Trend Detector |Trage Trend Detector |Detecteren van trage positieve trend aan de hand van de gevoeligheid instellen |*trenddetector. sensitivity:* drempel op de detector Score (standaard: 3,25, 3,25 – 5 is een redelijk bereik om dit te selecteren. Hoe hoger de minder gevoelige) |tscore: zwevende getal voor anomaliedetectie score op trend |
| De detectoren niveau wijzigen | Het niveau van bidirectionele wijziging Detector |Omhoog en omlaag de wijzigingen aan de hand van de gevoeligheid instellen detecteren |*bileveldetector. sensitivity:* drempel op de detector Score (standaard: 3,25, 3,25 – 5 is een redelijk bereik om dit te selecteren. Hoe hoger de minder gevoelige) |rpscore: zwevende getal voor anomaliedetectie score op omhoog en omlaag niveau wijzigen |

### <a name="parameters"></a>Parameters
Meer gedetailleerde informatie over deze invoerparameters die zijn opgegeven wordt in de onderstaande tabel vermeld:

| Invoerparameters | Description | Standaardinstelling | Type | Het geldige bereik | Voorgestelde bereik |
| --- | --- | --- | --- | --- | --- |
| detectors.historywindow |Geschiedenis (in totaal aantal gegevenspunten) gebruikt voor anomaliedetectie score berekeningen |500 |geheel getal |10-2000 |Afhankelijk van het time series |
| detectors.spikesdips | Of u wilt detecteren alleen pieken, alleen Spanningsdips, of beide |Beide |geïnventariseerd |Beide, pieken, Spanningsdips |Beide |
| bileveldetector.Sensitivity |Gevoeligheid voor in twee richtingen niveau detector wijzigen. |3.25 |double |Geen |3,25-5 (lagere waarden betekenen hogere gevoeligheid) |
| trenddetector.Sensitivity |Gevoeligheid voor positieve trend detector. |3.25 |double |Geen |3,25-5 (lagere waarden betekenen hogere gevoeligheid) |
| tspikedetector.sensitivity |Gevoeligheid voor TSpike Detector |3 |geheel getal |1-10 |3-5 (lagere waarden betekenen hogere gevoeligheid) |
| zspikedetector.sensitivity |Gevoeligheid voor ZSpike Detector |3 |geheel getal |1-10 |3-5 (lagere waarden betekenen hogere gevoeligheid) |
| postprocess.tailRows |Nummer van de meest recente gegevenspunten worden bewaard in de uitvoer-resultaten |0 |geheel getal |0 (Houd alle gegevenspunten), of geef het aantal punten met resultaten |N/A |

### <a name="output"></a>Uitvoer
De API wordt alle detectoren uitgevoerd op uw time series-gegevens en scores van afwijkingen en binaire piek indicatoren voor elk punt in tijd retourneert. De volgende tabel bevat de uitvoer van de API.

| Uitvoer | Description |
| --- | --- |
| Time |Tijdstempels van onbewerkte gegevens, of geaggregeerde (en/of) impliciete gegevens als aggregatie (en/of) ontbrekende gegevens toerekening wordt toegepast |
| Gegevens |Als de waarden van onbewerkte gegevens, of geaggregeerde (en/of) impliciete gegevens aggregatie (en/of) ontbrekende gegevens toerekening wordt toegepast |
| TSpike |Binaire indicator om aan te geven of een piek wordt gedetecteerd door TSpike Detector |
| ZSpike |Binaire indicator om aan te geven of een piek wordt gedetecteerd door ZSpike Detector |
| rpscore |Een zwevende getal vertegenwoordigt anomaliedetectie score op het niveau wijzigen in twee richtingen |
| rpalert |1/0-waarde die aangeeft dat er een niveau in twee richtingen wijzigen op basis van de invoer gevoeligheid voor anomaliedetectie |
| tscore |Een zwevende getal vertegenwoordigt anomaliedetectie score op positieve trend |
| talert |1/0-waarde die aangeeft er is een positieve trend van afwijkingen op basis van de invoer gevoeligheid |

## <a name="scorewithseasonality-api"></a>ScoreWithSeasonality API
De API ScoreWithSeasonality wordt gebruikt voor het uitvoeren van detectie van afwijkingen op tijdreeks waarvoor seizoensgebonden patronen. Deze API is handig voor het detecteren van afwijkingen in seizoensgebonden patronen.
De volgende afbeelding toont een voorbeeld van afwijkingen gedetecteerd in een seizoensgebonden tijdreeks. De tijdreeks heeft een piek (de 1e zwarte punt), twee Spanningsdips (de 2e zwarte stip en een aan het einde) en één niveau wijzigen (rode stip). Houd er rekening mee dat zowel het dip in het midden van de tijdreeks en de wijzigingen in het slechts aanmerkelijke zijn nadat seizoensgebonden onderdelen zijn verwijderd uit de reeks.
![Periodieke variatie API][2]

### <a name="detectors"></a>Detectoren
De detectoren in het eindpunt van de periodieke variatie zijn vergelijkbaar met die in het eindpunt van de niet-seizoensgebonden, maar met enigszins parameternamen (Zie hieronder).

### <a name="parameters"></a>Parameters

Meer gedetailleerde informatie over deze invoerparameters die zijn opgegeven wordt in de onderstaande tabel vermeld:

| Invoerparameters | Description | Standaardinstelling | Type | Het geldige bereik | Voorgestelde bereik |
| --- | --- | --- | --- | --- | --- |
| preprocess.aggregationInterval |Aggregatie-interval in seconden voor het verzamelen van invoer tijdreeks |0 (geen aggregatie wordt uitgevoerd) |geheel getal |0: aggregatie, > 0 anders overslaan |5 minuten op 1 dag, afhankelijk van het time series |
| preprocess.aggregationFunc |Functie die wordt gebruikt voor het verzamelen van gegevens in de opgegeven AggregationInterval |gemiddelde |geïnventariseerd |gemiddelde, som, lengte |N/A |
| preprocess.replaceMissing |Waarden gebruikt voor het rekenen, ontbrekende gegevens |lkv (laatst bekende waarde) |geïnventariseerd |nul, lkv, gemiddelde |N/A |
| detectors.historywindow |Geschiedenis (in totaal aantal gegevenspunten) gebruikt voor anomaliedetectie score berekeningen |500 |geheel getal |10-2000 |Afhankelijk van het time series |
| detectors.spikesdips | Of u wilt detecteren alleen pieken, alleen Spanningsdips, of beide |Beide |geïnventariseerd |Beide, pieken, Spanningsdips |Beide |
| bileveldetector.Sensitivity |Gevoeligheid voor in twee richtingen niveau detector wijzigen. |3.25 |double |Geen |3,25-5 (lagere waarden betekenen hogere gevoeligheid) |
| postrenddetector.Sensitivity |Gevoeligheid voor positieve trend detector. |3.25 |double |Geen |3,25-5 (lagere waarden betekenen hogere gevoeligheid) |
| negtrenddetector.Sensitivity |Gevoeligheid voor negatieve trend detector. |3.25 |double |Geen |3,25-5 (lagere waarden betekenen hogere gevoeligheid) |
| tspikedetector.sensitivity |Gevoeligheid voor TSpike Detector |3 |geheel getal |1-10 |3-5 (lagere waarden betekenen hogere gevoeligheid) |
| zspikedetector.sensitivity |Gevoeligheid voor ZSpike Detector |3 |geheel getal |1-10 |3-5 (lagere waarden betekenen hogere gevoeligheid) |
| seasonality.Enable |Of seizoensgebondenheid analyse wordt uitgevoerd |true |booleaans |waar of ONWAAR |Afhankelijk van het time series |
| seasonality.numSeasonality |Maximum aantal periodieke cycli worden gedetecteerd |1 |geheel getal |1, 2 |1-2 |
| seasonality.transform |Of seizoensgebonden (en) trend onderdelen moeten worden verwijderd voordat u detectie van afwijkingen |deseason |geïnventariseerd |None, deseason, deseasontrend |N/A |
| postprocess.tailRows |Nummer van de meest recente gegevenspunten worden bewaard in de uitvoer-resultaten |0 |geheel getal |0 (Houd alle gegevenspunten), of geef het aantal punten met resultaten |N/A |

### <a name="output"></a>Uitvoer
De API wordt alle detectoren uitgevoerd op uw time series-gegevens en scores van afwijkingen en binaire piek indicatoren voor elk punt in tijd retourneert. De volgende tabel bevat de uitvoer van de API.

| Uitvoer | Description |
| --- | --- |
| Time |Tijdstempels van onbewerkte gegevens, of geaggregeerde (en/of) impliciete gegevens als aggregatie (en/of) ontbrekende gegevens toerekening wordt toegepast |
| OriginalData |Als de waarden van onbewerkte gegevens, of geaggregeerde (en/of) impliciete gegevens aggregatie (en/of) ontbrekende gegevens toerekening wordt toegepast |
| ProcessedData |Een van de volgende: <ul><li>Tijdreeks seizoen aangepast als aanzienlijke seizoensgebondenheid is gedetecteerd en deseason optie geselecteerd.</li><li>seizoen aangepast en detrended tijdreeks als aanzienlijke seizoensgebondenheid is gedetecteerd en deseasontrend optie is geselecteerd</li><li>anders is dit hetzelfde als OriginalData</li> |
| TSpike |Binaire indicator om aan te geven of een piek wordt gedetecteerd door TSpike Detector |
| ZSpike |Binaire indicator om aan te geven of een piek wordt gedetecteerd door ZSpike Detector |
| BiLevelChangeScore |Een zwevende getal vertegenwoordigt anomaliedetectie score op niveau wijzigen |
| BiLevelChangeAlert |1/0-waarde die aangeeft er is een niveau wijzigen van afwijkingen op basis van de invoer gevoeligheid |
| PosTrendScore |Een zwevende getal vertegenwoordigt anomaliedetectie score op positieve trend |
| PosTrendAlert |1/0-waarde die aangeeft er is een positieve trend van afwijkingen op basis van de invoer gevoeligheid |
| NegTrendScore |Een zwevende getal vertegenwoordigt anomaliedetectie score op negatieve trend |
| NegTrendAlert |1/0-waarde die aangeeft er is een negatieve trend van afwijkingen op basis van de invoer gevoeligheid |

[1]: ./media/apps-anomaly-detection-api/anomaly-detection-score.png
[2]: ./media/apps-anomaly-detection-api/anomaly-detection-seasonal.png

