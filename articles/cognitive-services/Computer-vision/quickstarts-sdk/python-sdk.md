---
title: 'Quickstart: Python-SDK'
titleSuffix: Azure Cognitive Services
description: In deze snelstartgids leert u hoe u de Python-SDK gebruiken voor algemene taken, zoals analyseren afbeelding, beschrijving, tekst herkennen en genereren van miniatuur.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 04/17/2019
ms.author: pafarley
ms.openlocfilehash: c03568ece97bdaad86f4564debf9f3b2fa14c6ed
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67786652"
---
# <a name="azure-cognitive-services-computer-vision-sdk-for-python"></a>Azure Cognitive Services Computer Vision SDK voor Python

De Computer Vision-service geeft ontwikkelaars toegang tot geavanceerde algoritmen voor het verwerken van afbeeldingen en het retourneren van informatie. Met Computer Vision-algoritmen kunt u de inhoud van een afbeelding op verschillende manieren analyseren, afhankelijk van de visuele kenmerken waarin u geïnteresseerd bent.

* [Een afbeelding analyseren](#analyze-an-image)
* [Lijst met onderwerpdomeinen ophalen](#get-subject-domain-list)
* [Een afbeelding op domein analyseren](#analyze-an-image-by-domain)
* [Tekstbeschrijving van een afbeelding ophalen](#get-text-description-of-an-image)
* [Handgeschreven tekst uit een afbeelding ophalen](#get-text-from-image)
* [Miniatuur genereren](#generate-thumbnail)

Zie voor meer informatie over deze service [wat is er Computer Vision?][computervision_docs].

Zoekt u meer documentatie?

* [SDK-naslagdocumentatie](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision)
* [Documentatie over Cognitive Services Computer Vision](https://docs.microsoft.com/azure/cognitive-services/computer-vision/)

## <a name="prerequisites"></a>Vereisten

* [Python 3.6+][python]
* Gratis [Computer Vision-sleutel][computervision_resource] and associated endpoint. You need these values when you create the instance of the [ComputerVisionClient][ref_computervisionclient] clientobject. Gebruik een van de volgende methoden om deze waarden te verkrijgen.

### <a name="if-you-dont-have-an-azure-subscription"></a>Als u geen Azure-abonnement hebt

Maak een gratis sleutel geldig 7 dagen met de **[uitproberen][computervision_resource]** ervaring voor de Computer Vision-service. Wanneer de sleutel is gemaakt, kopieert u de naam van de sleutel en het eindpunt. U hebt deze nodig om [de client te maken](#create-client).

Houd het volgende nadat de sleutel is gemaakt:

* Sleutelwaarde: een tekenreeks van 32 tekens met de indeling van `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`
* Sleutel-eindpunt: de basis-URL eindpunt, https\://westcentralus.api.cognitive.microsoft.com

### <a name="if-you-have-an-azure-subscription"></a>Als u een Azure-abonnement hebt

De eenvoudigste methode voor het maken van een resource in uw abonnement is het gebruik van de volgende [Azure CLI][azure_cli] opdracht. Hiermee maakt u een Cognitive Service-sleutel die heel veel cognitive services kan worden gebruikt. Moet u ervoor kiezen de _bestaande_ Resourcegroepnaam, bijvoorbeeld 'Mijn-cogserv-groeperen' en de nieuwe computer vision-resource een naam, zoals 'Mijn-computer-vision-resource'.

```Bash
RES_REGION=westeurope
RES_GROUP=<resourcegroup-name>
ACCT_NAME=<computervision-account-name>

az cognitiveservices account create \
    --resource-group $RES_GROUP \
    --name $ACCT_NAME \
    --location $RES_REGION \
    --kind CognitiveServices \
    --sku S0 \
    --yes
```

<!--
## Installation

Install the Azure Cognitive Services Computer Vision SDK with [pip][pip], optionally within a [virtual environment][venv].

### Configure a virtual environment (optional)

Although not required, you can keep your base system and Azure SDK environments isolated from one another if you use a [virtual environment][virtualenv]. Execute the following commands to configure and then enter a virtual environment with [venv][venv], such as `cogsrv-vision-env`:

```Bash
python3 -m venv cogsrv-vision-env
source cogsrv-vision-env/bin/activate
```
-->

### <a name="install-the-sdk"></a>De SDK installeren

De Azure Cognitive Services-Computer Vision-SDK voor Python installeren [pakket][pypi_computervision] with [pip][pip]:

```Bash
pip install azure-cognitiveservices-vision-computervision
```

## <a name="authentication"></a>Authentication

Als u uw Computer Vision-resource maakt, moet u de **eindpunt**, en een van de **accountsleutels** om de clientobject te maken.

Deze waarden gebruiken bij het maken van het exemplaar van de [ComputerVisionClient][ref_computervisionclient] clientobject.

Gebruik bijvoorbeeld de Bash-terminal de omgevingsvariabelen instellen:

```Bash
ACCOUNT_ENDPOINT=<resourcegroup-name>
ACCT_NAME=<computervision-account-name>
```

### <a name="for-azure-subscription-users-get-credentials-for-key-and-endpoint"></a>Voor gebruikers van Azure-abonnement, kunt u referenties ophalen voor sleutel en het eindpunt

Als u niet meer uw eindpunt en de sleutel weet, kunt u de volgende methode te zoeken. Als u maken van een sleutel en -eindpunt wilt, kunt u de methode voor het [Azure-abonnement houders](#if-you-have-an-azure-subscription) of voor [gebruikers zonder een Azure-abonnement](#if-you-dont-have-an-azure-subscription).

Gebruik de [Azure CLI][cloud_shell] codefragment hieronder voor het vullen van twee omgevingsvariabelen met de Computer Vision-account **eindpunt** en een van de **sleutels** (u vindt hier ook deze waarden in de [Azure-portal][azure_portal]). Het codefragment is geformatteerd voor de Bash-shell.

```Bash
RES_GROUP=<resourcegroup-name>
ACCT_NAME=<computervision-account-name>

export ACCOUNT_ENDPOINT=$(az cognitiveservices account show \
    --resource-group $RES_GROUP \
    --name $ACCT_NAME \
    --query endpoint \
    --output tsv)

export ACCOUNT_KEY=$(az cognitiveservices account keys list \
    --resource-group $RES_GROUP \
    --name $ACCT_NAME \
    --query key1 \
    --output tsv)
```


### <a name="create-client"></a>Client maken

Ophalen van het eindpunt en de sleutel van omgevingsvariabelen en maak vervolgens de [ComputerVisionClient][ref_computervisionclient] clientobject.

```Python
from azure.cognitiveservices.vision.computervision import ComputerVisionClient
from azure.cognitiveservices.vision.computervision.models import VisualFeatureTypes
from msrest.authentication import CognitiveServicesCredentials

# Get endpoint and key from environment variables
import os
endpoint = os.environ['ACCOUNT_ENDPOINT']
key = os.environ['ACCOUNT_KEY']

# Set credentials
credentials = CognitiveServicesCredentials(key)

# Create client
client = ComputerVisionClient(endpoint, credentials)
```

## <a name="examples"></a>Voorbeelden

U moet een [ComputerVisionClient][ref_computervisionclient] clientobject voordat u een van de volgende taken uitvoeren.

### <a name="analyze-an-image"></a>Een afbeelding analyseren

U kunt een afbeelding voor bepaalde functies met analyseren [ `analyze_image` ][ref_computervisionclient_analyze_image] . Use the [`visual_features`][ref_computervision_model_visualfeatures] eigenschap voor het instellen van de analyse uit te voeren op de afbeelding. Veelvoorkomende waarden zijn `VisualFeatureTypes.tags` en `VisualFeatureTypes.description`.

```Python
url = "https://upload.wikimedia.org/wikipedia/commons/thumb/1/12/Broadway_and_Times_Square_by_night.jpg/450px-Broadway_and_Times_Square_by_night.jpg"

image_analysis = client.analyze_image(url,visual_features=[VisualFeatureTypes.tags])

for tag in image_analysis.tags:
    print(tag)
```

### <a name="get-subject-domain-list"></a>Lijst met onderwerpdomeinen ophalen

Bekijk de onderwerpsdomeinen gebruikt voor het analyseren van de installatiekopie met [ `list_models` ][ref_computervisionclient_list_models]. Deze domeinnamen worden gebruikt bij het [analyseren van een afbeelding op domein](#analyze-an-image-by-domain). Een voorbeeld van een domein is `landmarks`.

```Python
models = client.list_models()

for x in models.models_property:
    print(x)
```

### <a name="analyze-an-image-by-domain"></a>Een afbeelding op domein analyseren

U kunt een installatiekopie per onderwerp domein met analyseren [ `analyze_image_by_domain` ][ref_computervisionclient_analyze_image_by_domain]. Haal de [lijst met ondersteunde onderwerpdomeinen](#get-subject-domain-list) op zodat u de juiste domeinnaam kunt gebruiken.

```Python
# type of prediction
domain = "landmarks"

# Public domain image of Eiffel tower
url = "https://images.pexels.com/photos/338515/pexels-photo-338515.jpeg"

# English language response
language = "en"

analysis = client.analyze_image_by_domain(domain, url, language)

for landmark in analysis.result["landmarks"]:
    print(landmark["name"])
    print(landmark["confidence"])
```

### <a name="get-text-description-of-an-image"></a>Tekstbeschrijving van een afbeelding ophalen

Krijgt u een tekstbeschrijving van de taal op basis van van een afbeelding met [ `describe_image` ][ref_computervisionclient_describe_image]. Vraag verschillende beschrijvingen op met de eigenschap `max_description` als u een tekstanalyse uitvoert voor trefwoorden die aan de afbeelding zijn gekoppeld. Voorbeelden van een tekstbeschrijving voor de volgende afbeelding zijn onder meer `a train crossing a bridge over a body of water`, `a large bridge over a body of water` en `a train crossing a bridge over a large body of water`.

```Python
domain = "landmarks"
url = "http://www.public-domain-photos.com/free-stock-photos-4/travel/san-francisco/golden-gate-bridge-in-san-francisco.jpg"
language = "en"
max_descriptions = 3

analysis = client.describe_image(url, max_descriptions, language)

for caption in analysis.captions:
    print(caption.text)
    print(caption.confidence)
```

### <a name="get-text-from-image"></a>Tekst uit een afbeelding ophalen

U kunt handgeschreven of gedrukte tekst uit een afbeelding ophalen. Hiervoor twee verzoeken aan de SDK: [ `batch_read_file` ](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python) en [ `get_read_operation_result` ](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python). De aanroep van `batch_read_file` asynchroon is. In de resultaten van de `get_read_operation_result` aanroepen, moet u controleren als de eerste aanroep is voltooid met [ `TextOperationStatusCodes` ][ref_computervision_model_textoperationstatuscodes] voordat de gegevens te extraheren. De resultaten bevatten de tekst en de coördinaten van het begrenzingsvak voor de tekst.

```Python
# import models
from azure.cognitiveservices.vision.computervision.models import TextOperationStatusCodes
import time

url = "https://azurecomcdn.azureedge.net/cvt-1979217d3d0d31c5c87cbd991bccfee2d184b55eeb4081200012bdaf6a65601a/images/shared/cognitive-services-demos/read-text/read-1-thumbnail.png"
raw = True
custom_headers = None
numberOfCharsInOperationId = 36

# Async SDK call
rawHttpResponse = client.batch_read_file(url, custom_headers,  raw)

# Get ID from returned headers
operationLocation = rawHttpResponse.headers["Operation-Location"]
idLocation = len(operationLocation) - numberOfCharsInOperationId
operationId = operationLocation[idLocation:]

# SDK call
while True:
    result = client.get_read_operation_result(operationId)
    if result.status not in ['NotStarted', 'Running']:
        break
    time.sleep(1)

# Get data
if result.status == TextOperationStatusCodes.succeeded:
    for textResult in result.recognition_results:
        for line in textResult.lines:
            print(line.text)
            print(line.bounding_box)
```

### <a name="generate-thumbnail"></a>Miniatuur genereren

U kunt een miniatuur (JPG) van een afbeelding met genereren [ `generate_thumbnail` ][ref_computervisionclient_generate_thumbnail]. De miniatuur hoeft niet dezelfde verhoudingen te hebben als de oorspronkelijke afbeelding.

Installeer **Pillow** om dit voorbeeld te gebruiken:

```bash
pip install Pillow
```

Wanneer Pillow is geïnstalleerd, gebruikt u het pakket in het volgende codevoorbeeld om de miniatuurafbeelding te genereren.

```Python
# Pillow package
from PIL import Image

# IO package to create local image
import io

width = 50
height = 50
url = "http://www.public-domain-photos.com/free-stock-photos-4/travel/san-francisco/golden-gate-bridge-in-san-francisco.jpg"

thumbnail = client.generate_thumbnail(width, height, url)

for x in thumbnail:
    image = Image.open(io.BytesIO(x))

image.save('thumbnail.jpg')
```

## <a name="troubleshooting"></a>Problemen oplossen

### <a name="general"></a>Algemeen

Wanneer u communiceert met de [ComputerVisionClient][ref_computervisionclient] client object using the Python SDK, the [`ComputerVisionErrorException`][ref_computervision_computervisionerrorexception] klasse wordt gebruikt voor fouten retourneren. Fouten die door de service worden geretourneerd, komen overeen met de HTTP-statuscodes die voor REST API-aanvragen worden geretourneerd.

Als u bijvoorbeeld probeert een afbeelding met een ongeldige sleutel te analyseren, wordt de fout `401` geretourneerd. In het volgende codefragment de [fout][ref_httpfailure] zonder problemen wordt verwerkt door de uitzondering vastgelegd en aanvullende informatie over de fout is weergegeven.

```Python

domain = "landmarks"
url = "http://www.public-domain-photos.com/free-stock-photos-4/travel/san-francisco/golden-gate-bridge-in-san-francisco.jpg"
language = "en"
max_descriptions = 3

try:
    analysis = client.describe_image(url, max_descriptions, language)

    for caption in analysis.captions:
        print(caption.text)
        print(caption.confidence)
except HTTPFailure as e:
    if e.status_code == 401:
        print("Error unauthorized. Make sure your key and endpoint are correct.")
    else:
        raise
```

### <a name="handle-transient-errors-with-retries"></a>Tijdelijke fouten verwerken met nieuwe pogingen

Tijdens het werken met de [ComputerVisionClient][ref_computervisionclient] client, you might encounter transient failures caused by [rate limits][computervision_request_units] afgedwongen door de service of andere tijdelijke problemen zoals netwerkstoringen. Zie voor meer informatie over het afhandelen van dit soort fouten [patroon voor opnieuw proberen][azure_pattern_retry] in de handleiding Cloudontwerppatronen en de verwante [circuitonderbrekerpatroon][azure_pattern_circuit_breaker].

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Inhoudstags toepassen op afbeeldingen](../concept-tagging-images.md)

<!-- LINKS -->
[pip]: https://pypi.org/project/pip/
[python]: https://www.python.org/downloads/

[azure_cli]: https://docs.microsoft.com/cli/azure/cognitiveservices/account?view=azure-cli-latest#az-cognitiveservices-account-create
[azure_pattern_circuit_breaker]: https://docs.microsoft.com/azure/architecture/patterns/circuit-breaker
[azure_pattern_retry]: https://docs.microsoft.com/azure/architecture/patterns/retry
[azure_portal]: https://portal.azure.com
[azure_sub]: https://azure.microsoft.com/free/

[cloud_shell]: https://docs.microsoft.com/azure/cloud-shell/overview

[venv]: https://docs.python.org/3/library/venv.html
[virtualenv]: https://virtualenv.pypa.io

[source_code]: https://github.com/Azure/azure-sdk-for-python/tree/master/azure-cognitiveservices-vision-computervision

[pypi_computervision]:https://pypi.org/project/azure-cognitiveservices-vision-computervision/
[pypi_pillow]:https://pypi.org/project/Pillow/

[ref_computervision_sdk]: https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision?view=azure-python
[ref_computervision_computervisionerrorexception]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.models.computervisionerrorexception?view=azure-python
[ref_httpfailure]: https://docs.microsoft.com/python/api/msrest/msrest.exceptions.httpoperationerror?view=azure-python


[computervision_resource]: https://azure.microsoft.com/try/cognitive-services/?

[computervision_docs]: https://docs.microsoft.com/azure/cognitive-services/computer-vision/home

[ref_computervisionclient]: https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python

[ref_computervisionclient_analyze_image]: https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python

[ref_computervisionclient_list_models]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python

[ref_computervisionclient_analyze_image_by_domain]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python

[ref_computervisionclient_describe_image]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python

[ref_computervisionclient_get_text_operation_result]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python

[ref_computervisionclient_generate_thumbnail]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python


[ref_computervision_model_visualfeatures]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.models.visualfeaturetypes?view=azure-python

[ref_computervision_model_textoperationstatuscodes]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.models.textoperationstatuscodes?view=azure-python

[computervision_request_units]:https://azure.microsoft.com/pricing/details/cognitive-services/computer-vision/
