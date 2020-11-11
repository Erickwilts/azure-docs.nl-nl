---
title: Quickstart voor Anomaly Detector-clientbibliotheek voor Python
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: mrbullwinkle
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 09/22/2020
ms.author: mbullwin
ms.openlocfilehash: c06cb7143756eed3d50fe1d2d03ce7ba8d6d9d4d
ms.sourcegitcommit: 22da82c32accf97a82919bf50b9901668dc55c97
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/08/2020
ms.locfileid: "94371642"
---
Ga aan de slag met de Anomaly Detector-clientbibliotheek voor Python. Voer de volgende stappen uit om het pakket te installeren en de algoritmen te gaan gebruiken die door de service worden geleverd. Met de Anomaly Detector-service kunt u afwijkingen in uw tijdreeksgegevens vinden door hierop automatisch de best passende modellen uit te voeren, ongeacht de branche, het scenario of het gegevensvolume.

Gebruik de Anomaly Detector-clientbibliotheek voor Python om:

* Anomalieën in uw tijdreeksgegevensset als een batchaanvraag te detecteren
* De anomaliestatus van het laatste gegevenspunt in uw tijdreeks te detecteren
* Trendwijzigingspunten in uw gegevensset te detecteren.

[Referentiedocumentatie voor bibliotheek](https://go.microsoft.com/fwlink/?linkid=2090370) | [Bibliotheekbroncode](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-anomalydetector) | [Pakket (PyPi)](https://pypi.org/project/azure-ai-anomalydetector/) | [De voorbeeldcode zoeken op GitHub](https://github.com/Azure-Samples/AnomalyDetector/blob/master/quickstarts/sdk/python-sdk-sample.py)

## <a name="prerequisites"></a>Vereisten

* [Python 3.x](https://www.python.org/)
* De [bibliotheek voor Pandas-gegevensanalyse](https://pandas.pydata.org/)
* Azure-abonnement: [Krijg een gratis abonnement](https://azure.microsoft.com/free/cognitive-services)
* Zodra u een Azure-abonnement hebt, <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesAnomalyDetector"  title="Anomaly Detector-resource maken"  target="_blank">maakt u een Anomaly Detector-resource <span class="docon docon-navigate-external x-hidden-focus"></span></a> in Azure Portal om uw sleutel en eindpunt op te halen. Wacht tot deze is geïmplementeerd en klik op de knop **Naar de resource gaan**.
    * U hebt de sleutel en het eindpunt nodig van de resource die u maakt om de toepassing te verbinden met de Anomaly Detector-API. Later in de quickstart plakt u uw sleutel en eindpunt in de onderstaande code.
    U kunt de gratis prijscategorie (`F0`) gebruiken om de service uit te proberen, en later upgraden naar een betaalde laag voor productie.


## <a name="setting-up"></a>Instellen

[!INCLUDE [anomaly-detector-environment-variables](../environment-variables.md)]

### <a name="create-a-new-python-application"></a>Een nieuwe Python-toepassing maken

 Maak een nieuw Python-bestand en importeer de volgende bibliotheken.

[!code-python[import declarations](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=imports)]

Maak variabelen voor uw sleutel als een omgevingsvariabele, het pad naar een tijdreeksgegevensbestand en de Azure-locatie van uw abonnement. Bijvoorbeeld `westus2`.

[!code-python[Vars for the key, path location and data path](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=initVars)]

### <a name="install-the-client-library"></a>De clientbibliotheek installeren

Na de installatie van Python kunt u de clientbibliotheek installeren met:

```console
pip install --upgrade azure-ai-anomalydetector
```

## <a name="object-model"></a>Objectmodel

De Anomaly Detector-client is een [AnomalyDetectorClient](/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.anomalydetectorclient?view=azure-python)-object dat met behulp van uw sleutel wordt geverifieerd bij Azure. De client kan anomaliedetectie uitvoeren op een hele gegevensset met behulp van [entire_detect()](/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.anomalydetectorclient?view=azure-python#entire-detect-body--custom-headers-none--raw-false----operation-config-), of op het laatste gegevenspunt met behulp van [Last_detect()](/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.anomalydetectorclient?view=azure-python#last-detect-body--custom-headers-none--raw-false----operation-config-). De functie [ChangePointDetectAsync](https://go.microsoft.com/fwlink/?linkid=2090370) detecteert punten die wijzigingen in een trend markeren.

Tijdreeksgegevens worden verzonden als een reeks [punten](/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.point?view=azure-python) in een [aanvraagobject](/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.request?view=azure-python). Het `Request`-object bevat eigenschappen die de gegevens beschrijven ([granulariteit](/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.granularity?view=azure-python) bijvoorbeeld) en parameters voor de anomaliedetectie.

Het antwoord van Anomaly Detector is een [LastDetectResponse](/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.lastdetectresponse?view=azure-python)-, [EntireDetectResponse](/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.entiredetectresponse?view=azure-python)- of [ChangePointDetectResponse](https://go.microsoft.com/fwlink/?linkid=2090370)-object afhankelijk van de gebruikte methode.

## <a name="code-examples"></a>Codevoorbeelden

Deze codefragmenten laten zien hoe u de volgende taken kunt uitvoeren met de Anomaly Detector-clientbibliotheek voor Python:

* [De client verifiëren](#authenticate-the-client)
* [Een tijdreeksgegevensset laden vanuit een bestand](#load-time-series-data-from-a-file)
* [Anomalieën in de volledige gegevensset detecteren](#detect-anomalies-in-the-entire-data-set)
* [De anomaliestatus van het laatste gegevenspunt detecteren](#detect-the-anomaly-status-of-the-latest-data-point)
* [De wijzigingspunten in de gegevensset detecteren](#detect-change-points-in-the-data-set)

## <a name="authenticate-the-client"></a>De client verifiëren

Voeg uw Azure-locatievariabele toe aan het eindpunt en verifieer de client met uw sleutel.

[!code-python[Client authentication](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=client)]

## <a name="load-time-series-data-from-a-file"></a>Tijdreeksgegevens uit een bestand laden

Download de voorbeeldgegevens voor deze quickstart van [GitHub](https://github.com/Azure-Samples/AnomalyDetector/blob/master/example-data/request-data.csv):
1. Klik in uw browser met de rechtermuisknop op **Onbewerkt**.
2. Klik op **Koppeling opslaan als**.
3. Sla het bestand op in de map van de toepassing als CSV-bestand.

Deze tijdreeksgegevens worden opgemaakt als CSV-bestand en worden verzonden naar de Anomaly Detector-API.

Laad uw gegevensbestand met de `read_csv()`-methode van de Pandas-bibliotheek en maak een lege lijstvariabele om uw gegevensreeksen op te slaan. Herhaal het bestand en voeg de gegevens toe als [punt](/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.point?view=azure-python)-object. Dit object bevat de tijdstempel en de numerieke waarde van de rijen van uw CSV-gegevensbestand.

[!code-python[Load the data file](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=loadDataFile)]

Maak een [aanvraag](/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.request?view=azure-python)object met uw tijdreeks en de [granulariteit](/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.granularity?view=azure-python) (of periodiciteit) van de gegevenspunten. Bijvoorbeeld `Granularity.daily`.

[!code-python[Create the request object](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=request)]

## <a name="detect-anomalies-in-the-entire-data-set"></a>Anomalieën in de volledige gegevensset detecteren

Roep de API aan om anomalieën te detecteren via de volledige tijdreeksgegevens met behulp van de [entire_detect()](/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.anomalydetectorclient?view=azure-python#entire-detect-body--custom-headers-none--raw-false----operation-config-)-methode van de client. Sla het geretourneerde [EntireDetectResponse](/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.entiredetectresponse?view=azure-python)-object op. Herhaal de `is_anomaly`-lijst van de reactie en druk de index van alle `true`-waarden af. Deze waarden komen overeen met de index van afwijkende gegevenspunten, als die zijn gevonden.

[!code-python[Batch anomaly detection sample](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=detectAnomaliesBatch)]

## <a name="detect-the-anomaly-status-of-the-latest-data-point"></a>De anomaliestatus van het laatste gegevenspunt detecteren

Roep de Anomaly Detector-API aan om te bepalen of uw laatste gegevenspunt een anomalie is met behulp van de [last_detect()](/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.anomalydetectorclient?view=azure-python#last-detect-body--custom-headers-none--raw-false----operation-config-)-methode van de client en sla het geretourneerde [LastDetectResponse](/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.lastdetectresponse?view=azure-python)-object op. De `is_anomaly`-waarde van het antwoord is een booleaanse waarde die de afwijkingsstatus van het punt opgeeft.  

[!code-python[Batch anomaly detection sample](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=latestPointDetection)]

## <a name="detect-change-points-in-the-data-set"></a>Wijzigingspunten in de gegevensset detecteren

Roep de API aan om wijzigingspunten in de tijdreeksgegevens te detecteren met behulp van de methode [detect_change_point()](https://go.microsoft.com/fwlink/?linkid=2090370) van de client. Sla het geretourneerde [ChangePointDetectResponse](https://go.microsoft.com/fwlink/?linkid=2090370)-object op. Herhaal de `is_change_point`-lijst van de reactie en druk de index van alle `true`-waarden af. Deze waarden komen overeen met de indexen van trendwijzigingspunten, als die zijn gevonden.

[!code-python[detect change points](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=changePointDetection)]

## <a name="run-the-application"></a>De toepassing uitvoeren

Voer de toepassing uit met de opdracht `python` en uw bestandsnaam.

[!INCLUDE [anomaly-detector-next-steps](../quickstart-cleanup-next-steps.md)]