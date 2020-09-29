---
title: Docker-containers voor de Computer Vision Lees-API installeren en uitvoeren
titleSuffix: Azure Cognitive Services
description: Gebruik de docker-container voor de Computer Vision Lees-API om tekst te detecteren en op te halen uit installatie kopieën, on-premises.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 09/28/2020
ms.author: aahi
ms.custom: seodec18, cog-serv-seo-aug-2020
keywords: on-premises, docker, container
ms.openlocfilehash: 17aa0050ed3e2a2f5a2cc83964d032caa4b8c098
ms.sourcegitcommit: a0c4499034c405ebc576e5e9ebd65084176e51e4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/29/2020
ms.locfileid: "91460438"
---
# <a name="install-and-run-docker-containers-for-the-read-api-preview"></a>Docker-containers installeren en uitvoeren voor de Lees-API (preview) 

[!INCLUDE [container hosting on the Microsoft Container Registry](../containers/includes/gated-container-hosting.md)]

Met containers kunt u de Computer Vision-API's uitvoeren in uw eigen omgeving. Containers zijn ideaal voor specifieke vereisten voor beveiliging en gegevensbeheer. In dit artikel leert u hoe u een Computer Vision-container downloadt, installeert en uitvoert.

De *Lees* container stelt u in staat om *gedrukte tekst* te detecteren en uit te pakken van afbeeldingen van verschillende objecten met verschillende Opper vlakken en achtergronden, zoals bevestigingen, posters en visite kaartjes. Daarnaast detecteert de *Lees* container *handgeschreven tekst* in afbeeldingen en biedt PDF, TIFF en ondersteuning voor meerdere pagina's. Zie de [Lees API-documentatie](concept-recognizing-text.md#read-api)voor meer informatie.

Er zijn twee versies van de 3. x-containers die beschikbaar zijn in de preview-versie. Beide versies bieden extra nauw keurigheid en functies ten opzichte van de vorige container.

De container Lees 3,0-Preview biedt het volgende:
* Nieuwe modellen voor verbeterde nauw keurigheid.
* Ondersteuning voor meerdere talen in hetzelfde document
* Ondersteuning voor: Nederlands, Engels, Frans, Duits, Italiaans, Portugees en Spaans.
* Eén bewerking voor zowel documenten als installatie kopieën.
* Ondersteuning voor grotere documenten en installatie kopieën.
* Betrouwbaarheids scores van 0 tot 1.
* Ondersteuning voor documenten met zowel gedrukte als handgeschreven tekst

De container Lees 3,1-Preview biedt dezelfde voor delen als v 3.0-Preview, met aanvullende functies:

* Ondersteuning voor vereenvoudigd Chinees en Japans.
* betrouw bare scores en labels voor gedrukte en handgeschreven tekst. 
* De mogelijkheid om alleen tekst uit geselecteerde pagina ('s) in een document op te halen.

Houd er rekening mee dat v 3.1-Preview een eerdere status van preview heeft wanneer u overweegt welke container versie u moet gebruiken. Raadpleeg de [migratie handleiding](read-container-migration-guide.md) voor meer informatie over wijzigingen in de nieuwe versies als u vandaag nog Lees 2,0-containers gebruikt.

## <a name="prerequisites"></a>Vereisten

U moet voldoen aan de volgende vereisten voordat u de containers gebruikt:

|Vereist|Doel|
|--|--|
|Docker-engine| De docker-engine moet zijn geïnstalleerd op een [hostcomputer](#the-host-computer). Docker biedt pakketten waarmee de Docker-omgeving op [MacOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) en [Linux](https://docs.docker.com/engine/installation/#supported-platforms) kan worden geconfigureerd. Zie het [Docker-overzicht](https://docs.docker.com/engine/docker-overview/) voor een inleiding tot de basisprincipes van Docker en containers.<br><br> Docker moet worden geconfigureerd zodat de containers verbinding kunnen maken met en facturerings gegevens kunnen verzenden naar Azure. <br><br> **In Windows**moet docker ook worden geconfigureerd voor de ondersteuning van Linux-containers.<br><br>|
|Vertrouwd met docker | U moet een basis kennis hebben van docker-concepten, zoals registers, opslag plaatsen, containers en container installatie kopieën, en kennis van basis `docker` opdrachten.| 
|Computer Vision resource |Als u de container wilt gebruiken, hebt u het volgende nodig:<br><br>Een Azure **Computer Vision** -resource en de bijbehorende API-sleutel de EINDPUNT-URI. Beide waarden zijn beschikbaar op de pagina overzicht en sleutels voor de resource en zijn vereist om de container te starten.<br><br>**{API_KEY}**: een van de twee beschik bare bron sleutels op de pagina **sleutels**<br><br>**{ENDPOINT_URI}**: het eind punt op de pagina **overzicht**|

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/cognitive-services/) voordat u begint.

## <a name="request-approval-to-run-the-container"></a>Goed keuring aanvragen om de container uit te voeren

Vul het [aanvraag formulier](https://aka.ms/csgate) in en verzend het om goed keuring te vragen om de container uit te voeren. 

[!INCLUDE [Request access to public preview](../../../includes/cognitive-services-containers-request-access.md)]

[!INCLUDE [Gathering required container parameters](../containers/includes/container-gathering-required-parameters.md)]

### <a name="the-host-computer"></a>De hostcomputer

[!INCLUDE [Host Computer requirements](../../../includes/cognitive-services-containers-host-computer.md)]

### <a name="advanced-vector-extension-support"></a>Ondersteuning voor geavanceerde vector uitbreidingen

De **hostcomputer** is de computer waarop de docker-container wordt uitgevoerd. De host *moet ondersteuning bieden* voor [Geavanceerde vector uitbreidingen](https://en.wikipedia.org/wiki/Advanced_Vector_Extensions#CPUs_with_AVX2) (AVX2). U kunt de AVX2-ondersteuning op Linux-hosts controleren met de volgende opdracht:

```console
grep -q avx2 /proc/cpuinfo && echo AVX2 supported || echo No AVX2 support detected
```

> [!WARNING]
> De hostcomputer is *vereist* voor de ondersteuning van AVX2. De container werkt *niet* correct zonder ondersteuning voor AVX2.

### <a name="container-requirements-and-recommendations"></a>Container vereisten en aanbevelingen

[!INCLUDE [Container requirements and recommendations](includes/container-requirements-and-recommendations.md)]

## <a name="get-the-container-image-with-docker-pull"></a>De container installatie kopie ophalen met `docker pull`

Er zijn container installatie kopieën voor lezen beschikbaar.

| Container | Container Registry/opslagplaats/naam van installatie kopie |
|-----------|------------|
| Lees 2,0-Preview | `mcr.microsoft.com/azure-cognitive-services/vision/read:2.0-preview` |
| Lees 3,0-Preview | `mcr.microsoft.com/azure-cognitive-services/vision/read:3.0-preview` |
| Lees 3,1-Preview | `mcr.microsoft.com/azure-cognitive-services/vision/read:3.1-preview` |

Gebruik de [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/) opdracht om een container installatie kopie te downloaden.

### <a name="docker-pull-for-the-read-container"></a>Docker-pull voor de Lees container

# <a name="version-31-preview"></a>[Versie 3,1-Preview](#tab/version-3-1)

```bash
docker pull mcr.microsoft.com/azure-cognitive-services/vision/read:3.1-preview
```

# <a name="version-30-preview"></a>[Versie 3,0-Preview](#tab/version-3)

```bash
docker pull mcr.microsoft.com/azure-cognitive-services/vision/read:3.0-preview
```

# <a name="version-20-preview"></a>[Versie 2,0-Preview](#tab/version-2)

```bash
docker pull mcr.microsoft.com/azure-cognitive-services/vision/read:2.0-preview
```

---

[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]

## <a name="how-to-use-the-container"></a>De container gebruiken

Wanneer de container zich op de [hostcomputer](#the-host-computer)bevindt, gebruikt u het volgende proces om met de container te werken.

1. [Voer de container uit](#run-the-container-with-docker-run)met de vereiste facturerings instellingen. Er zijn meer [voor beelden](computer-vision-resource-container-config.md) van de `docker run` opdracht beschikbaar. 
1. [Zoek het Voorspellings eindpunt van de container](#query-the-containers-prediction-endpoint)op. 

## <a name="run-the-container-with-docker-run"></a>Voer de container uit met `docker run`

Gebruik de opdracht [docker run](https://docs.docker.com/engine/reference/commandline/run/) om de container uit te voeren. Raadpleeg de [vereiste para meters verzamelen](#gathering-required-parameters) voor meer informatie over het ophalen van de `{ENDPOINT_URI}` `{API_KEY}` waarden en.

[Voor beelden](computer-vision-resource-container-config.md#example-docker-run-commands) van de `docker run` opdracht zijn beschikbaar.

# <a name="version-31-preview"></a>[Versie 3,1-Preview](#tab/version-3-1)

```bash
docker run --rm -it -p 5000:5000 --memory 18g --cpus 8 \
mcr.microsoft.com/azure-cognitive-services/vision/read:3.1-preview \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Met deze opdracht gebeurt het volgende:

* Voert de Lees container uit van de container installatie kopie.
* Wijst 8 CPU-kernen en 18 GB aan geheugen toe.
* Beschrijft TCP-poort 5000 en wijst een pseudo-TTY voor de container toe.
* Verwijdert de container automatisch nadat deze is afgesloten. De container installatie kopie is nog steeds beschikbaar op de hostcomputer.

# <a name="version-30-preview"></a>[Versie 3,0-Preview](#tab/version-3)

```bash
docker run --rm -it -p 5000:5000 --memory 18g --cpus 8 \
mcr.microsoft.com/azure-cognitive-services/vision/read:3.0-preview \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}

```

Met deze opdracht gebeurt het volgende:

* Voert de Lees container uit van de container installatie kopie.
* Wijst 8 CPU-kernen en 18 GB aan geheugen toe.
* Beschrijft TCP-poort 5000 en wijst een pseudo-TTY voor de container toe.
* Verwijdert de container automatisch nadat deze is afgesloten. De container installatie kopie is nog steeds beschikbaar op de hostcomputer.

# <a name="version-20-preview"></a>[Versie 2,0-Preview](#tab/version-2)

```bash
docker run --rm -it -p 5000:5000 --memory 16g --cpus 8 \
mcr.microsoft.com/azure-cognitive-services/vision/read:2.0-preview \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Met deze opdracht gebeurt het volgende:

* Voert de Lees container uit van de container installatie kopie.
* Wijst 8 CPU-kernen en 16 GB aan geheugen toe.
* Beschrijft TCP-poort 5000 en wijst een pseudo-TTY voor de container toe.
* Verwijdert de container automatisch nadat deze is afgesloten. De container installatie kopie is nog steeds beschikbaar op de hostcomputer.

---


Er zijn meer [voor beelden](./computer-vision-resource-container-config.md#example-docker-run-commands) van de `docker run` opdracht beschikbaar. 

> [!IMPORTANT]
> De `Eula` `Billing` Opties, en `ApiKey` moeten worden opgegeven om de container uit te voeren. anders wordt de container niet gestart.  Zie [facturering](#billing)voor meer informatie.

Als u een hogere door Voer (bijvoorbeeld bij het verwerken van bestanden met meerdere pagina's) nodig hebt, kunt u overwegen om meerdere v 3.0-of v 3.1-containers [in een Kubernetes-cluster](deploy-computer-vision-on-premises.md)te implementeren met behulp van [Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-account-create) en [Azure-wachtrij](https://docs.microsoft.com/azure/storage/queues/storage-queues-introduction).

Als u Azure Storage gebruikt om installatie kopieën op te slaan voor de verwerking, kunt u een [Connection String](https://docs.microsoft.com/azure/storage/common/storage-configure-connection-string) maken om te gebruiken bij het aanroepen van de container.

Ga als volgt te connection string:

1. Navigeer naar **opslag accounts** op de Azure Portal en zoek uw account.
2. Klik op **toegangs sleutels** in de linkernavigatiebalk.
3. Uw connection string vindt u onder de **verbindings reeks**

[!INCLUDE [Running multiple containers on the same host](../../../includes/cognitive-services-containers-run-multiple-same-host.md)]

<!--  ## Validate container is running -->

[!INCLUDE [Container's API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="query-the-containers-prediction-endpoint"></a>Een query uitvoeren op het voorspellingseindpunt van de container

De container bevat op REST gebaseerde eindpunt-API's voor queryvoorspelling. 

# <a name="version-31-preview"></a>[Versie 3,1-Preview](#tab/version-3-1)

Gebruik de host, `http://localhost:5000`, voor container-API's. U kunt het Swagger-pad weer geven op: `http://localhost:5000/swagger/vision-v3.1-preview-read/swagger.json` .

# <a name="version-30-preview"></a>[Versie 3,0-Preview](#tab/version-3)

Gebruik de host, `http://localhost:5000`, voor container-API's. U kunt het Swagger-pad weer geven op: `http://localhost:5000/swagger/vision-v3.0-preview-read/swagger.json` .

# <a name="version-20-preview"></a>[Versie 2,0-Preview](#tab/version-2)

Gebruik de host, `http://localhost:5000`, voor container-API's. U kunt het Swagger-pad weer geven op: `http://localhost:5000/swagger/vision-v2.0-preview-read/swagger.json` .

---

### <a name="asynchronous-read"></a>Asynchroon lezen


# <a name="version-31-preview"></a>[Versie 3,1-Preview](#tab/version-3-1)

U kunt de- `POST /vision/v3.1/read/analyze` en- `GET /vision/v3.1/read/operations/{operationId}` bewerkingen in concert gebruiken om asynchroon een installatie kopie te lezen, vergelijkbaar met de manier waarop de computer vision-service die bijbehorende rest-bewerkingen gebruikt. De asynchrone POST-methode retourneert een `operationId` die wordt gebruikt als id voor de HTTP GET-aanvraag.


Selecteer in de Swagger-gebruikers interface de `asyncBatchAnalyze` om deze uit te vouwen in de browser. Selecteer vervolgens **Try it out**  >  **bestand**uitproberen. In dit voor beeld gebruiken we de volgende afbeelding:

![tabbladen versus spaties](media/tabs-vs-spaces.png)

Wanneer de asynchrone POST met succes is uitgevoerd, wordt een **HTTP 202-** status code geretourneerd. Als onderdeel van de reactie bevindt zich een `operation-location` kop die het eind punt voor de aanvraag bevat.

```http
 content-length: 0
 date: Fri, 04 Sep 2020 16:23:01 GMT
 operation-location: http://localhost:5000/vision/v3.1/read/operations/a527d445-8a74-4482-8cb3-c98a65ec7ef9
 server: Kestrel
```

De `operation-location` is de volledig gekwalificeerde URL en is toegankelijk via een HTTP Get. Dit is het JSON-antwoord van het uitvoeren van de `operation-location` URL van de vorige installatie kopie:

```json
{
  "status": "succeeded",
  "createdDateTime": "2020-09-02T10:30:14Z",
  "lastUpdatedDateTime": "2020-09-02T10:30:15Z",
  "analyzeResult": {
    "version": "3.1.0",
    "readResults": [
      {
        "page": 1,
        "angle": 2.12,
        "width": 502,
        "height": 252,
        "unit": "pixel",
        "language": "",
        "lines": [
          {
            "boundingBox": [58, 42, 314, 59, 311, 123, 56, 121],
            "text": "Tabs vs",
            "appearance": {
              "style": "handwriting",
              "styleConfidence": 0.999
            },
            "words": [
              {
                "boundingBox": [85, 45, 242, 62, 241, 122, 83, 123],
                "text": "Tabs",
                "confidence": 0.981
              },
              {
                "boundingBox": [258, 64, 314, 72, 314, 123, 256, 123],
                "text": "vs",
                "confidence": 0.958
              }
            ]
          },
          {
            "boundingBox": [286, 171, 415, 165, 417, 197, 287, 201],
            "text": "paces",
            "appearance": {
              "style": "print",
              "styleConfidence": 0.603
            },
            "words": [
              {
                "boundingBox": [303, 175, 415, 167, 415, 198, 306, 199],
                "text": "paces",
                "confidence": 0.918
              }
            ]
          }
        ]
      }
    ]
  }
}
```

# <a name="version-30-preview"></a>[Versie 3,0-Preview](#tab/version-3)

U kunt de- `POST /vision/v3.0/read/analyze` en- `GET /vision/v3.0/read/operations/{operationId}` bewerkingen in concert gebruiken om asynchroon een installatie kopie te lezen, vergelijkbaar met de manier waarop de computer vision-service die bijbehorende rest-bewerkingen gebruikt. De asynchrone POST-methode retourneert een `operationId` die wordt gebruikt als id voor de HTTP GET-aanvraag.

Selecteer in de Swagger-gebruikers interface de `asyncBatchAnalyze` om deze uit te vouwen in de browser. Selecteer vervolgens **Try it out**  >  **bestand**uitproberen. In dit voor beeld gebruiken we de volgende afbeelding:

![tabbladen versus spaties](media/tabs-vs-spaces.png)

Wanneer de asynchrone POST met succes is uitgevoerd, wordt een **HTTP 202-** status code geretourneerd. Als onderdeel van de reactie bevindt zich een `operation-location` kop die het eind punt voor de aanvraag bevat.

```http
 content-length: 0
 date: Fri, 04 Sep 2020 16:23:01 GMT
 operation-location: http://localhost:5000/vision/v3.0/read/operations/a527d445-8a74-4482-8cb3-c98a65ec7ef9
 server: Kestrel
```

De `operation-location` is de volledig gekwalificeerde URL en is toegankelijk via een HTTP Get. Dit is het JSON-antwoord van het uitvoeren van de `operation-location` URL van de vorige installatie kopie:

```json
{
  "status": "succeeded",
  "createdDateTime": "2020-09-02T10:24:49Z",
  "lastUpdatedDateTime": "2020-09-02T10:24:50Z",
  "analyzeResult": {
    "version": "3.0.0",
    "readResults": [
      {
        "page": 1,
        "angle": 2.12,
        "width": 502,
        "height": 252,
        "unit": "pixel",
        "language": "",
        "lines": [
          {
            "boundingBox": [58, 42, 314, 59, 311, 123, 56, 121],
            "text": "Tabs vs",
            "words": [
              {
                "boundingBox": [85, 45, 242, 62, 241, 122, 83, 123],
                "text": "Tabs",
                "confidence": 0.981
              },
              {
                "boundingBox": [258, 64, 314, 72, 314, 123, 256, 123],
                "text": "vs",
                "confidence": 0.958
              }
            ]
          },
          {
            "boundingBox": [286, 171, 415, 165, 417, 197, 287, 201],
            "text": "paces",
            "words": [
              {
                "boundingBox": [303, 175, 415, 167, 415, 198, 306, 199],
                "text": "paces",
                "confidence": 0.918
              }
            ]
          }
        ]
      }
    ]
  }
}
```

# <a name="version-20-preview"></a>[Versie 2,0-Preview](#tab/version-2)

U kunt de- `POST /vision/v2.0/read/core/asyncBatchAnalyze` en- `GET /vision/v2.0/read/operations/{operationId}` bewerkingen in concert gebruiken om asynchroon een installatie kopie te lezen, vergelijkbaar met de manier waarop de computer vision-service die bijbehorende rest-bewerkingen gebruikt. De asynchrone POST-methode retourneert een `operationId` die wordt gebruikt als id voor de HTTP GET-aanvraag.

Selecteer in de Swagger-gebruikers interface de `asyncBatchAnalyze` om deze uit te vouwen in de browser. Selecteer vervolgens **Try it out**  >  **bestand**uitproberen. In dit voor beeld gebruiken we de volgende afbeelding:

![tabbladen versus spaties](media/tabs-vs-spaces.png)

Wanneer de asynchrone POST met succes is uitgevoerd, wordt een **HTTP 202-** status code geretourneerd. Als onderdeel van de reactie bevindt zich een `operation-location` kop die het eind punt voor de aanvraag bevat.

```http
 content-length: 0
 date: Fri, 13 Sep 2019 16:23:01 GMT
 operation-location: http://localhost:5000/vision/v2.0/read/operations/a527d445-8a74-4482-8cb3-c98a65ec7ef9
 server: Kestrel
```

De `operation-location` is de volledig gekwalificeerde URL en is toegankelijk via een HTTP Get. Dit is het JSON-antwoord van het uitvoeren van de `operation-location` URL van de vorige installatie kopie:

```json
{
  "status": "Succeeded",
  "recognitionResults": [
    {
      "page": 1,
      "clockwiseOrientation": 2.42,
      "width": 502,
      "height": 252,
      "unit": "pixel",
      "lines": [
        {
          "boundingBox": [ 56, 39, 317, 50, 313, 134, 53, 123 ],
          "text": "Tabs VS",
          "words": [
            {
              "boundingBox": [ 90, 43, 243, 53, 243, 123, 94, 125 ],
              "text": "Tabs",
              "confidence": "Low"
            },
            {
              "boundingBox": [ 259, 55, 313, 62, 313, 122, 259, 123 ],
              "text": "VS"
            }
          ]
        },
        {
          "boundingBox": [ 221, 148, 417, 146, 417, 206, 227, 218 ],
          "text": "Spaces",
          "words": [
            {
              "boundingBox": [ 230, 148, 416, 141, 419, 211, 232, 218 ],
              "text": "Spaces"
            }
          ]
        }
      ]
    }
  ]
}
```

---

> [!IMPORTANT]
> Als u meerdere Lees containers achter een load balancer implementeert, bijvoorbeeld onder docker opstellen of Kubernetes, moet u een externe cache hebben. Omdat de verwerkings container en de container voor GET-aanvragen mogelijk niet hetzelfde zijn, worden de resultaten door een externe cache opgeslagen en gedeeld in containers. Zie [Computer Vision docker-containers configureren](https://docs.microsoft.com/azure/cognitive-services/computer-vision/computer-vision-resource-container-config)voor meer informatie over de cache-instellingen.

### <a name="synchronous-read"></a>Synchrone Lees bewerking

U kunt de volgende bewerking gebruiken om een installatie kopie synchroon te lezen. 

# <a name="version-31-preview"></a>[Versie 3,1-Preview](#tab/version-3-1)

`POST /vision/v3.1/read/syncAnalyze` 

# <a name="version-30-preview"></a>[Versie 3,0-Preview](#tab/version-3)

`POST /vision/v3.0/read/syncAnalyze`

# <a name="version-20-preview"></a>[Versie 2,0-Preview](#tab/version-2)

`POST /vision/v2.0/read/core/Analyze`

---

Wanneer de afbeelding volledig is gelezen en vervolgens alleen de API retourneert een JSON-antwoord. De enige uitzonde ring hierop is als er een fout optreedt. Als er een fout optreedt, wordt de volgende JSON geretourneerd:

```json
{
    "status": "Failed"
}
```

Het JSON-antwoord object heeft dezelfde object grafiek als de asynchrone versie. Als u een Java script-gebruiker bent en type veiligheid wilt, kunt u type script gebruiken om de JSON-reactie te casten.

Voor een voor beeld van een use-case raadpleegt u de <a href="https://aka.ms/ts-read-api-types" target="_blank" rel="noopener noreferrer">type script sandbox hier <span class="docon docon-navigate-external x-hidden-focus"></span> </a> en selecteert u **uitvoeren** om het gebruiks gemak te visualiseren.

## <a name="stop-the-container"></a>De container stoppen

[!INCLUDE [How to stop the container](../../../includes/cognitive-services-containers-stop.md)]

## <a name="troubleshooting"></a>Problemen oplossen

Als u de container uitvoert met een uitvoer [koppeling](./computer-vision-resource-container-config.md#mount-settings) en logboek registratie ingeschakeld, genereert de container logboek bestanden die handig zijn om problemen op te lossen die optreden tijdens het starten of uitvoeren van de container.

[!INCLUDE [Cognitive Services FAQ note](../containers/includes/cognitive-services-faq-note.md)]

## <a name="billing"></a>Billing

De Cognitive Services-containers verzenden facturerings gegevens naar Azure, met behulp van de bijbehorende resource in uw Azure-account.

[!INCLUDE [Container's Billing Settings](../../../includes/cognitive-services-containers-how-to-billing-info.md)]

Zie [containers configureren](./computer-vision-resource-container-config.md)voor meer informatie over deze opties.

<!--blogs/samples/video course -->

[!INCLUDE [Discoverability of more container information](../../../includes/cognitive-services-containers-discoverability.md)]

## <a name="summary"></a>Samenvatting

In dit artikel hebt u concepten en werk stromen geleerd om Computer Vision containers te downloaden, te installeren en uit te voeren. Samenvatting:

* Computer Vision biedt een Linux-container voor docker, Inge kapseld voor lezen.
* Container installatie kopieën worden gedownload uit het container register container preview in Azure.
* Container installatie kopieën worden uitgevoerd in docker.
* U kunt de REST API of SDK gebruiken voor het aanroepen van bewerkingen in containers lezen door de URI van de host van de container op te geven.
* U moet factuur gegevens opgeven bij het instantiëren van een container.

> [!IMPORTANT]
> Cognitive Services-containers mogen niet worden uitgevoerd zonder te zijn verbonden met Azure voor meting. Klanten moeten de containers in staat stellen om de facturerings gegevens te allen tijde met de meet service te communiceren. Cognitive Services containers verzenden geen klant gegevens (bijvoorbeeld de afbeelding of tekst die wordt geanalyseerd) naar micro soft.

## <a name="next-steps"></a>Volgende stappen

* Containers voor configuratie-instellingen [configureren](computer-vision-resource-container-config.md) controleren
* Bekijk [Computer Vision overzicht](overview.md) voor meer informatie over het herkennen van gedrukte en handgeschreven tekst
* Raadpleeg de [Computer Vision-API](//westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) voor meer informatie over de methoden die door de container worden ondersteund.
* Raadpleeg Veelgestelde [vragen (FAQ)](FAQ.md) voor het oplossen van problemen met betrekking tot de functionaliteit van computer vision.
* Meer [Cognitive Services containers](../cognitive-services-container-support.md) gebruiken
