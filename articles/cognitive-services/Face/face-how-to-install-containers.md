---
title: Containers installeren en uitvoeren-FACE-API
titleSuffix: Azure Cognitive Services
description: In dit artikel wordt beschreven hoe u containers kunt downloaden, installeren en uitvoeren voor een gezicht in deze stapsgewijze zelf studie.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: dapine
ms.openlocfilehash: 574f6bead9cac384c72d2d0cd35353eb571a9490
ms.sourcegitcommit: b77e97709663c0c9f84d95c1f0578fcfcb3b2a6c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/22/2019
ms.locfileid: "74327038"
---
# <a name="install-and-run-face-containers-preview"></a>Face-containers installeren en uitvoeren (preview-versie)

Azure Cognitive Services gezicht biedt een gestandaardiseerde Linux-container voor docker die menselijke gezichten in afbeeldingen detecteert. Er worden ook kenmerken geïdentificeerd, waaronder gezichts bezienswaardigheden zoals neus en ogen, geslacht, leeftijd en andere computer-voorspelde gezichts functies. Naast detectie kan het gezicht controleren of twee gezichten in dezelfde afbeelding of verschillende afbeeldingen hetzelfde zijn door gebruik te maken van een betrouwbaarheids Score. Gezicht kan ook gezichten vergelijken met een Data Base om te zien of er al een vergelijkbaar of identiek gezicht bestaat. Het kan ook vergelijk bare gezichten in groepen organiseren door gebruik te maken van gedeelde visuele elementen.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

U moet voldoen aan de volgende vereisten voordat u de Face-API-containers gebruikt.

|Vereist|Doel|
|--|--|
|Docker-engine| De docker-engine moet zijn geïnstalleerd op een [hostcomputer](#the-host-computer). Docker biedt pakketten voor het configureren van de docker-omgeving op [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/)en [Linux](https://docs.docker.com/engine/installation/#supported-platforms). Zie het [docker-overzicht](https://docs.docker.com/engine/docker-overview/)voor een primer op basis van docker en container.<br><br> Docker moet worden geconfigureerd, zodat de containers om te verbinden met en facturering gegevens verzenden naar Azure. <br><br> In Windows moet docker ook worden geconfigureerd voor de ondersteuning van Linux-containers.<br><br>|
|Vertrouwd met docker | U hebt een basis informatie nodig over docker-concepten, zoals registers, opslag plaatsen, containers en container installatie kopieën. U hebt ook kennis nodig van basis `docker`-opdrachten.| 
|Gezichts bron |Als u de container wilt gebruiken, hebt u het volgende nodig:<br><br>Een Azure **Face** -resource en de bijbehorende API-sleutel en de EINDPUNT-URI. Beide waarden zijn beschikbaar op het **overzicht** en op de pagina **sleutels** voor de resource. Ze zijn verplicht om de container te starten.<br><br>**{API_KEY}** : een van de twee beschik bare bron sleutels op de pagina **sleutels**<br><br>**{ENDPOINT_URI}** : het eind punt op de pagina **overzicht**

[!INCLUDE [Gathering required container parameters](../containers/includes/container-gathering-required-parameters.md)]

## <a name="request-access-to-the-private-container-registry"></a>Aanvraag voor toegang tot de privécontainerregister

[!INCLUDE [Request access to private container registry](../../../includes/cognitive-services-containers-request-access.md)]

### <a name="the-host-computer"></a>De hostcomputer

[!INCLUDE [Host Computer requirements](../../../includes/cognitive-services-containers-host-computer.md)]

### <a name="container-requirements-and-recommendations"></a>Containervereisten en aanbevelingen

De volgende tabel beschrijft de minimale en aanbevolen CPU-kernen en het geheugen die moeten worden toegewezen voor elke Face-API-container.

| Container | Minimum | Aanbevolen | Transacties per seconde<br>(Minimum, maximum)|
|-----------|---------|-------------|--|
|Face | 1 Core, 2 GB geheugen | 1 kern geheugen van 4 GB |10, 20|

* Elke kern moet ten minste 2,6 GHz of sneller zijn.
* Trans acties per seconde (TPS).

Core en geheugen komen overeen met de instellingen `--cpus` en `--memory` die worden gebruikt als onderdeel van de `docker run` opdracht.

## <a name="get-the-container-image-with-docker-pull"></a>Container installatie kopie ophalen met docker-pull

Er zijn container installatie kopieën voor de Face-API beschikbaar. 

| Container | Opslagplaats |
|-----------|------------|
| Face | `containerpreview.azurecr.io/microsoft/cognitive-services-face:latest` |

[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]

### <a name="docker-pull-for-the-face-container"></a>Docker-pull voor de face-container

```
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-face:latest
```

## <a name="use-the-container"></a>De container gebruiken

Nadat de container zich op de [hostcomputer](#the-host-computer)bevindt, gebruikt u het volgende proces om met de container te werken.

1. [Voer de container uit](#run-the-container-with-docker-run) met de vereiste facturerings instellingen. Er zijn meer [voor beelden](./face-resource-container-config.md#example-docker-run-commands) van de `docker run`-opdracht beschikbaar. 
1. [Zoek het Voorspellings eindpunt van de container](#query-the-containers-prediction-endpoint)op. 

## <a name="run-the-container-with-docker-run"></a>De container uitvoeren met docker-uitvoering

Gebruik de opdracht [docker run](https://docs.docker.com/engine/reference/commandline/run/) om de container uit te voeren. Raadpleeg de [vereiste para meters verzamelen](#gathering-required-parameters) voor meer informatie over het ophalen van de `{ENDPOINT_URI}` en `{API_KEY}` waarden.

[Voor beelden](face-resource-container-config.md#example-docker-run-commands) van de opdracht `docker run` zijn beschikbaar.

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
containerpreview.azurecr.io/microsoft/cognitive-services-face \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Deze opdracht:

* Voert een face-container uit vanuit de container installatie kopie.
* Wijst één CPU-kern en 4 GB aan geheugen toe.
* Beschrijft TCP-poort 5000 en wijst een pseudo-TTY voor de container toe.
* Verwijdert de container automatisch nadat deze is afgesloten. De container installatie kopie is nog steeds beschikbaar op de hostcomputer. 

Er zijn meer [voor beelden](./face-resource-container-config.md#example-docker-run-commands) van de `docker run`-opdracht beschikbaar. 

> [!IMPORTANT]
> De opties `Eula`, `Billing`en `ApiKey` moeten worden opgegeven om de container uit te voeren of de container start niet. Zie [facturering](#billing)voor meer informatie.

[!INCLUDE [Running multiple containers on the same host](../../../includes/cognitive-services-containers-run-multiple-same-host.md)]


## <a name="query-the-containers-prediction-endpoint"></a>Query uitvoeren op het prediction-eind punt van de container

De container bevat op REST gebaseerde query Voorspellings eindpunt-Api's. 

Gebruik de host, `http://localhost:5000`voor container-Api's.


<!--  ## Validate container is running -->

[!INCLUDE [Container's API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="stop-the-container"></a>De container stoppen

[!INCLUDE [How to stop the container](../../../includes/cognitive-services-containers-stop.md)]

## <a name="troubleshooting"></a>Problemen oplossen

Als u de container uitvoert met een uitvoer [koppeling](./face-resource-container-config.md#mount-settings) en logboek registratie is ingeschakeld, genereert de container logboek bestanden die handig zijn om problemen op te lossen die optreden tijdens het starten of uitvoeren van de container.

[!INCLUDE [Cognitive Services FAQ note](../containers/includes/cognitive-services-faq-note.md)]

## <a name="billing"></a>Billing

De Face-API-containers verzenden facturerings gegevens naar Azure met behulp van een Face-API resource in uw Azure-account. 

[!INCLUDE [Container's Billing Settings](../../../includes/cognitive-services-containers-how-to-billing-info.md)]

Zie [containers configureren](./face-resource-container-config.md)voor meer informatie over deze opties.

<!--blogs/samples/video coures -->

[!INCLUDE [Discoverability of more container information](../../../includes/cognitive-services-containers-discoverability.md)]

## <a name="summary"></a>Samenvatting

In dit artikel hebt u concepten en werk stromen geleerd om Face-API containers te downloaden, te installeren en uit te voeren. Samenvatting:

* Container installatie kopieën worden gedownload van de Azure Container Registry.
* Containerinstallatiekopieën uitvoeren in Docker.
* U kunt de REST API of de SDK gebruiken om bewerkingen in Face-API containers aan te roepen door de URI van de host op te geven van de container.
* U moet de facturerings gegevens opgeven wanneer u een container maakt.

> [!IMPORTANT]
> Cognitive Services-containers mogen niet worden uitgevoerd zonder te zijn verbonden met Azure voor meting. Klanten moeten de containers in staat stellen om de facturerings gegevens te allen tijde met de meet service te communiceren. Cognitive Services containers verzenden geen klant gegevens, zoals de afbeelding of de tekst die wordt geanalyseerd, naar micro soft.

## <a name="next-steps"></a>Volgende stappen

* Zie [containers configureren](face-resource-container-config.md)voor configuratie-instellingen.
* Zie [gezichts overzicht](Overview.md)voor meer informatie over het detecteren en identificeren van gezichten.
* Zie de [Face-API](//westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)voor meer informatie over de methoden die door de container worden ondersteund.
* Zie [Cognitive Services containers](../cognitive-services-container-support.md)als u meer Cognitive Services containers wilt gebruiken.
