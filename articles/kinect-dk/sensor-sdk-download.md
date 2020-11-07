---
title: Azure Kinect sensor-SDK downloaden
description: Meer informatie over het downloaden en installeren van de Azure Kinect sensor SDK op Windows en Linux.
author: Brent-A
ms.author: brenta
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: conceptual
keywords: Azure, kinect, SDK, down load update, nieuwste, beschikbaar, installeren
ms.openlocfilehash: 48a3df3962796cf561057e8ec85b754c974200f4
ms.sourcegitcommit: 0b9fe9e23dfebf60faa9b451498951b970758103
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2020
ms.locfileid: "94358502"
---
# <a name="azure-kinect-sensor-sdk-download"></a>Azure Kinect sensor-SDK downloaden

Deze pagina bevat de Download koppelingen voor elke versie van de Azure Kinect sensor SDK. Het installatie programma bevat alle benodigde bestanden voor het ontwikkelen van Azure Kinect.

## <a name="azure-kinect-sensor-sdk-contents"></a>Inhoud van Azure Kinect sensor SDK

- Kopteksten en bibliotheken om een toepassing te bouwen met behulp van Azure Kinect DK.
- Herdistribueerbare dll-bestanden die nodig zijn voor toepassingen die gebruikmaken van Azure Kinect DK.
- De [Azure Kinect-Viewer](azure-kinect-viewer.md).
- De [Azure Kinect-recorder](azure-kinect-recorder.md).
- Het [hulp programma Azure Kinect-firmware](azure-kinect-firmware-tool.md).

## <a name="windows-download-link"></a>Koppeling naar Windows downloaden

[Micro soft Installer](https://download.microsoft.com/download/3/d/6/3d6d9e99-a251-4cf3-8c6a-8e108e960b4b/Azure%20Kinect%20SDK%201.4.1.exe)  |  [Github-bron code](https://github.com/microsoft/Azure-Kinect-Sensor-SDK/issues/1093)

> [!NOTE]
> Onthoud bij het installeren van de SDK het pad dat u wilt installeren. Bijvoorbeeld ' C:\Program Files\Azure Kinect SDK 1,2 '. U vindt de hulpprogram ma's waarnaar wordt verwezen in artikelen in dit pad.

U kunt vorige versies van de Azure Kinect-sensor-SDK en-firmware vinden op [github](https://github.com/microsoft/Azure-Kinect-Sensor-SDK/blob/develop/docs/usage.md).

## <a name="linux-installation-instructions"></a>Linux-installatie-instructies

Momenteel is de enige ondersteunde distributie Ubuntu 18,04. Zie [Deze pagina](https://aka.ms/azurekinectfeedback)als u ondersteuning voor andere distributies wilt aanvragen.

Eerst moet u de [opslag plaats van micro soft-pakket](https://packages.microsoft.com/)configureren. Volg hiervoor de instructies [hier](/windows-server/administration/linux-package-repository-for-microsoft-software).

U kunt nu de benodigde pakketten installeren. Het `k4a-tools` pakket bevat de [Azure Kinect-Viewer](azure-kinect-viewer.md), de [Azure Kinect-recorder](record-sensor-streams-file.md)en het [hulp programma Azure Kinect-firmware](azure-kinect-firmware-tool.md). Voer uit om de installatie uit te voeren.

 `sudo apt install k4a-tools`

 Het `libk4a<major>.<minor>-dev` pakket bevat de kop-en cmake-bestanden die moeten worden gebouwd `libk4a` .
Het `libk4a<major>.<minor>` pakket bevat de gedeelde objecten die nodig zijn voor het uitvoeren van uitvoer bare bestanden die afhankelijk zijn van `libk4a` .

 Voor de basis zelf studies is het `libk4a<major>.<minor>-dev` pakket vereist. Voer uit om de installatie uit te voeren.

 `sudo apt install libk4a1.1-dev`

Als de opdracht is geslaagd, is de SDK klaar voor gebruik.

## <a name="change-log-and-older-versions"></a>Wijzigingen logboek en oudere versies

U kunt het wijzigingslog bestand voor de Azure Kinect sensor [-SDK vinden](https://github.com/microsoft/Azure-Kinect-Sensor-SDK/blob/develop/CHANGELOG.md).

Als u een oudere versie van de Azure Kinect sensor SDK nodig hebt, kunt u deze [hier](https://github.com/microsoft/Azure-Kinect-Sensor-SDK/blob/develop/docs/usage.md)vinden.

## <a name="next-steps"></a>Volgende stappen

[Azure Kinect DK installeren](set-up-azure-kinect-dk.md)