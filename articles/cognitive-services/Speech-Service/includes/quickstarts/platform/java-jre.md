---
title: 'Quickstart: Instelling platform Speech SDK voor Java (Windows, Linux, macOS): Speech-service'
titleSuffix: Azure Cognitive Services
description: Gebruik deze handleiding om uw platform in te stellen voor het gebruik van Java (Windows, Linux, macOS) met de Speech-service SDK.
services: cognitive-services
author: markamos
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 10/15/2020
ms.custom: devx-track-java
ms.author: erhopf
ms.openlocfilehash: 093e5482896e2af8008f20826e30443bdeb9aae9
ms.sourcegitcommit: 93329b2fcdb9b4091dbd632ee031801f74beb05b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/15/2020
ms.locfileid: "92097028"
---
In deze gids ontdekt u hoe u de [Speech SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) kunt installeren voor 64-bits Java 8 JRE. Als u enkel de pakketnaam wilt om zelf aan de slag te gaan, dan is de Java SDK niet beschikbaar in de centrale Maven-opslagplaats. Of u nu Gradle of een `pom.xml`-afhankelijkheidsbestand gebruikt, u moet een aangepaste opslagplaats toevoegen die verwijst naar `https://csspeechstorage.blob.core.windows.net/maven/` (zie hieronder voor de pakketnaam).

> [!NOTE]
> Zie [Speech Devices SDK](~/articles/cognitive-services/speech-service/speech-devices-sdk.md) voor de Speech Devices-SDK en het Roobo-apparaat.

[!INCLUDE [License Notice](~/includes/cognitive-services-speech-service-license-notice.md)]

## <a name="supported-operating-systems"></a>Ondersteunde besturingssystemen

- Het Java Speech-SDK-pakket is beschikbaar voor deze besturingssystemen:
  - Windows: Enkel 64-bits
  - Mac: macOS X-versie 10.13 of hoger
  - Linux: Enkel 64-bits op Ubuntu 16.04, Ubuntu 18.04, Debian 9, RHEL 7/8, CentOS 7/8

## <a name="prerequisites"></a>Vereisten

- [Java 8](https://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) of [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html)

- [Eclips Java IDE](https://www.eclipse.org/downloads/) (hiervoor moet Java al geïnstalleerd zijn)
- Voor ondersteunde Linux-platformen moeten bepaalde bibliotheken zijn geïnstalleerd (`libssl` voor de ondersteuning van Secure Sockets Layer en `libasound2` voor de ondersteuning van geluid). Zoek uw distributie hieronder voor de opdracht waarmee u de correcte versies van deze bibliotheken kunt installeren.

  - Voer op Ubuntu de volgende opdrachten uit om de vereiste pakketten te installeren:

    ```sh
    sudo apt-get update
    sudo apt-get install build-essential libssl1.0.0 libasound2
    ```

  - Voer op Debian 9 de volgende opdrachten uit om de vereiste pakketten te installeren:

    ```sh
    sudo apt-get update
    sudo apt-get install build-essential libssl1.0.2 libasound2
    ```

  - Voer op RHEL/CentOS de volgende opdrachten uit om de vereiste pakketten te installeren:

    ```sh
    sudo yum update
    sudo yum install alsa-lib java-1.8.0-openjdk-devel openssl
    ```

> [!NOTE]
> - Volg op RHEL/CentOS 7 de instructies op [RHEL/CentOS 7 configureren voor Speech SDK](~/articles/cognitive-services/speech-service/how-to-configure-rhel-centos-7.md).
> - Volg op RHEL/CentOS 8 de instructies op [OpenSSL configureren voor Linux](~/articles/cognitive-services/speech-service/how-to-configure-openssl-linux.md).

- In Windows hebt u het [Microsoft Visual C++ Redistributable for Visual Studio 2019](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) voor uw platform nodig. Houd er rekening mee dat als u dit voor het eerst installeert, Windows opnieuw moet worden opgestart voordat u doorgaat met deze gids.

## <a name="create-an-eclipse-project-and-install-the-speech-sdk"></a>Een Eclipse-project maken en de Speech SDK installeren

[!INCLUDE [](~/includes/cognitive-services-speech-service-quickstart-java-create-proj.md)]

## <a name="next-steps"></a>Volgende stappen

[!INCLUDE [windows](../quickstart-list.md)]
