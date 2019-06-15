---
title: Azure IoT-Apparaatbeheer met Azure IoT-hulpprogramma's voor Visual Studio Code | Microsoft Docs
description: Gebruik de Azure IoT-Tools voor Visual Studio Code voor Azure IoT Hub-Apparaatbeheer, met daarin de directe methoden en beheeropties van de Apparaatdubbel-gewenste eigenschappen.
author: formulahendry
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/04/2019
ms.author: junhan
ms.openlocfilehash: 03df2ceb2df4d857e48f1790703a1d87647e43d0
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60401167"
---
# <a name="use-azure-iot-tools-for-visual-studio-code-for-azure-iot-hub-device-management"></a>Azure IoT-hulpprogramma's voor Visual Studio Code gebruiken voor het beheer van Azure IoT Hub-apparaten

![Diagram voor end-to-end](media/iot-hub-get-started-e2e-diagram/2.png)

[Azure IoT-hulpprogramma's](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) is een nuttig Visual Studio Code-extensie die IoT Hub beheer en ontwikkeling van IoT-toepassingen gemakkelijker maakt. Het wordt geleverd met opties die u gebruiken kunt diverse taken uit te voeren.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

| Management-optie          | Taak                    |
|----------------------------|--------------------------------|
| Directe methoden             | Een apparaat fungeren, zoals starten of stoppen van berichten verzenden of opnieuw opstarten van het apparaat maken.                                        |
| Apparaatdubbel lezen           | Ophalen van de gerapporteerde status van een apparaat. Bijvoorbeeld, meldt het apparaat dat de LED nu knippert.                                    |
| Dubbele apparaat bijwerken         | Een apparaat in een bepaalde status, zoals een LED groen instellen of het instellen van het interval voor het verzenden van telemetrie tot 30 minuten plaatsen.         |
| Cloud-naar-apparaat-berichten   | Meldingen verzenden naar een apparaat. Bijvoorbeeld, "het is zeer waarschijnlijk regen vandaag nog. Vergeet niet om een overkoepelende."              |

Zie voor meer gedetailleerde uitleg over de verschillen en richtlijnen over het gebruik van deze opties, [richtlijnen voor communicatie van apparaat-naar-cloud](iot-hub-devguide-d2c-guidance.md) en [Cloud-naar-apparaat communicatie richtlijnen](iot-hub-devguide-c2d-guidance.md).

Apparaatdubbels zijn JSON-documenten waarin statusinformatie van een apparaat (metagegevens, configuraties en voorwaarden) zijn opgeslagen. IoT Hub is een apparaatdubbel voor elk apparaat dat is verbonden met het. Zie voor meer informatie over apparaatdubbels [aan de slag met apparaatdubbels](iot-hub-node-node-twin-getstarted.md).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="what-you-learn"></a>Wat u leert

Leert u met behulp van Azure IoT-hulpprogramma's voor Visual Studio Code met verschillende beheeropties voor op uw ontwikkelcomputer.

## <a name="what-you-do"></a>Wat u allemaal doen

Azure IoT-hulpprogramma's voor Visual Studio Code uitvoeren met verschillende opties.

## <a name="what-you-need"></a>Wat u nodig hebt

* Een actief Azure-abonnement.
* Een Azure IoT-hub in uw abonnement.
* [Visual Studio Code](https://code.visualstudio.com/)
* [Azure IoT-hulpprogramma's voor VS Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) of [koppeling niet openen in Visual Studio Code](vscode:extension/vsciot-vscode.azure-iot-tools).

## <a name="sign-in-to-access-your-iot-hub"></a>Meld u aan voor toegang tot uw IoT-hub

1. In **Explorer** weergave van VS Code uit, vouw **Azure IoT Hub-apparaten** sectie in de linkerbenedenhoek.

2. Klik op **IoT-Hub selecteren** in het contextmenu.

3. Een pop-upvenster wordt weergegeven in de rechterbenedenhoek zodat u voor de eerste keer aanmelden bij Azure.

4. Nadat u zich hebt aangemeld, wordt de lijst met uw Azure-abonnement worden weergegeven en selecteer Azure-abonnement en IoT-Hub.

5. De lijst met apparaten wordt weergegeven in **Azure IoT Hub-apparaten** tabblad in een paar seconden.

   > [!Note]
   > U kunt de installatie ook voltooien door **IoT Hub-verbindingsreeks instellen** te kiezen. Geef de verbindingsreeks voor de IoT-hub die uw IoT-apparaat in verbinding met het pop-upvenster maakt.

## <a name="direct-methods"></a>Directe methoden

1. Met de rechtermuisknop op uw apparaat en selecteer **rechtstreekse aanroepmethode**. 

2. Voer de methodenaam en -nettolading in het invoervak.

3. Resultaten worden weergegeven **uitvoer** > **Azure IoT Hub Toolkit** weergeven.

## <a name="read-device-twin"></a>Apparaatdubbel lezen

1. Met de rechtermuisknop op uw apparaat en selecteer **Apparaatdubbel bewerken**. 

2. Een **azure-iot-device-twin.json** bestand wordt geopend met de inhoud van het dubbele apparaat.

## <a name="update-device-twin"></a>Dubbele apparaat bijwerken

1. Sommige wijzigingen van **tags** of **properties.desired** veld.

2. Met de rechtermuisknop op de **azure-iot-device-twin.json** bestand.

3. Selecteer **Apparaatdubbel bijwerken** om bij te werken het dubbele apparaat.

## <a name="send-cloud-to-device-messages"></a>Cloud-naar-apparaat-berichten verzenden

Berichten van uw IoT-hub worden verzonden naar uw apparaat, de volgende stappen uit:
 
1. Met de rechtermuisknop op uw apparaat en selecteer **C2D-bericht verzenden naar apparaat**. 

2. Geef het bericht in het invoervak.

3. Resultaten worden weergegeven **uitvoer** > **Azure IoT Hub Toolkit** weergeven.

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd over het gebruik van hulpprogramma's voor Azure IoT-extensie voor Visual Studio Code met verschillende opties.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
