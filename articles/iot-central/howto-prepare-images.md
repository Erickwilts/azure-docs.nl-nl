---
title: Afbeeldingen uploaden naar uw Azure IoT Central-toepassing | Microsoft Docs
description: Als een opbouwfunctie voor expressies, informatie over het voorbereiden en afbeeldingen uploaden naar uw Azure IoT Central-toepassing.
author: dominicbetts
ms.author: dobett
ms.date: 02/05/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: a20662c2fc9b416fefce89a6ebe706307ee71bb7
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2019
ms.locfileid: "65236483"
---
# <a name="prepare-and-upload-images-to-your-azure-iot-central-application"></a>Voorbereiden en afbeeldingen uploaden naar uw Azure IoT Central-toepassing

Dit artikel wordt beschreven hoe, als een opbouwfunctie voor expressies, kunt u uw Azure IoT Central-toepassing uploaden van aangepaste installatiekopieën. U kunt bijvoorbeeld een apparaatdashboard met een afbeelding van het apparaat.

## <a name="before-you-begin"></a>Voordat u begint

U hebt het volgende nodig om de stappen in dit artikel uit te voeren:

1. Een Azure IoT Central-toepassing. Zie voor meer informatie de [snelstart over het maken van een toepassing](quick-deploy-iot-central.md).
1. Een hulpprogramma voor het schalen en het formaat van afbeeldingsbestanden.

## <a name="choose-where-to-use-custom-images"></a>Kies waar u aangepaste installatiekopieën kunt gebruiken

U kunt aangepaste installatiekopieën toevoegen aan de volgende locaties in een Azure IoT Central-toepassing:

* De **mijn toepassingen** pagina

    ![Een installatiekopie op de pagina voor het beheer van toepassingen](media/howto-prepare-images/applicationmanager.png)

* Het dashboard

    ![Afbeelding in toepassingsdashboard](media/howto-prepare-images/homepage.png)

* Een apparaat-sjabloon

    ![Afbeelding van apparaat-sjabloon](media/howto-prepare-images/devicetemplate.png)

* Een tegel op een apparaatdashboard

    ![Een installatiekopie op de tegel van apparaat](media/howto-prepare-images/devicetile.png)

* Dashboard een tegel op een apparaat instellen

    ![Afbeelding op de tegel van apparaat instellen](media/howto-prepare-images/devicesettile.png)

## <a name="prepare-the-images"></a>De installatiekopieën voorbereiden

Op alle vier locaties, kunt u PNG-, GIF- of JPEG-afbeeldingen.

De volgende tabel geeft een overzicht van de grootte van de installatiekopie die u kunt gebruiken:

| Locatie | Grootten |
| -------- | ------ |
| Toepassingsbeheer | 268x160 px |
| Apparaatsjabloon | 64 x 64 pixels |
| Dashboard-tegels | De kleinste grootte tegel is 200 x 200 pixels, grotere tegels mag vierkant of rechthoekig veelvouden van kleine tegels. Bijvoorbeeld 200 x 400 px, 400 x 200 px of 400 x 400 px |

Voor de beste weergave in de toepassing, moet u de installatiekopieën die overeenkomen met de dimensies die wordt weergegeven in de vorige tabel maken.

## <a name="upload-the-images"></a>De afbeeldingen uploaden

De volgende secties wordt beschreven hoe u de installatiekopieën in de verschillende locaties te uploaden:

### <a name="application-manager"></a>Toepassingsbeheer

Het uploaden van een installatiekopie moet worden gebruikt op de **mijn toepassingen** pagina, gaat u naar de **toepassingsinstellingen** pagina in de **beheer** sectie. U moet een beheerder om deze taak te voltooien:

![Afbeelding van de toepassing uploaden](media/howto-prepare-images/uploadapplicationmanager.png)

Selecteer de **Toepassingsinstallatiekopie** tegel om een afbeelding te uploaden (268 x 160 px) van uw lokale computer.

### <a name="application-dashboard"></a>Toepassingsdashboard

Als u wilt uploaden een afbeelding op het dashboard, gaat u naar de **Dashboard** pagina van uw toepassing en selecteer **bewerken**. U moet een opbouwfunctie om deze taak te voltooien:

![Dashboard-installatiekopie uploaden](media/howto-prepare-images/uploadhomepage.png)

Onder **installatiekopie configureren**, selecteer de **installatiekopie** tegel voor het uploaden van een installatiekopie van uw lokale computer. De kleinste grootte tegel is 200 x 200 pixels, grotere tegels mag vierkant of rechthoekig veelvouden van kleine tegels. Bijvoorbeeld 200 x 400 px, 400 x 200 px of 400 x 400 px.

**Sla** de geüploade installatiekopie. U kunt het formaat van het in de bewerkingsmodus. Selecteer **gedaan** wanneer u klaar bent.

### <a name="device-template"></a>Apparaatsjabloon

Als u wilt uploaden een installatiekopie van een sjabloon van het apparaat, gaat u naar **Apparaatsjablonen** en kies de sjabloon van het apparaat. U moet een opbouwfunctie om deze taak te voltooien:

![Installatiekopie van het apparaat sjabloon uploaden](media/howto-prepare-images/uploaddevicetemplate.png)

Selecteer de tegel de installatiekopie van een afbeelding uploaden (64 x 64 pixels) van uw lokale computer.

### <a name="device-dashboard"></a>Apparaatdashboard

Als u wilt uploaden een installatiekopie op een apparaatdashboard, gaat u naar **Apparaatsjablonen** en kies de sjabloon van het apparaat. Kies vervolgens de **Dashboard** tabblad. U moet een opbouwfunctie om deze taak te voltooien:

![Installatiekopie van het dashboard apparaat uploaden](media/howto-prepare-images/uploaddevicedashboard.png)

Onder **installatiekopie configureren**, selecteer de **installatiekopie** tegel en kies vervolgens het bestand te uploaden vanaf uw lokale computer. De kleinste grootte tegel is 200 x 200 pixels, grotere tegels mag vierkant of rechthoekig veelvouden van kleine tegels. Bijvoorbeeld 200 x 400 px, 400 x 200 px of 400 x 400 px.

**Sla** de geüploade installatiekopie. U kunt vergroten of verkleinen en de positie in de bewerkingsmodus. Selecteer **gedaan** wanneer u klaar bent.

### <a name="device-set-dashboard"></a>Apparaat instellen dashboard

Als u wilt uploaden een installatiekopie op een apparaat instellen-dashboard, gaat u naar **Apparaatsets** en kies de apparaatset en klik vervolgens op een apparaat. Kies vervolgens de **Dashboard** pagina en selecteer **bewerken**:

![Uploaden van dashboard installatiekopie van het apparaat instellen](media/howto-prepare-images/uploaddevicesetdashboard.png)

Onder **installatiekopie configureren**, selecteer de **installatiekopie** tegel voor het uploaden van een installatiekopie van uw lokale computer. De kleinste grootte tegel is 200 x 200 pixels, grotere tegels mag vierkant of rechthoekig veelvouden van kleine tegels. Bijvoorbeeld 200 x 400 px, 400 x 200 px of 400 x 400 px.

**Sla** de geüploade installatiekopie. U kunt vergroten of verkleinen en de positie in de bewerkingsmodus. Selecteer **gedaan** wanneer u klaar bent.

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u installatiekopieën voorbereidt en uploadt naar uw Azure IoT Central-toepassing, vindt hier u de voorgestelde volgende stappen:

* [De Azure IoT Central gebruikersinterface aanpassen](./howto-customize-ui.md)
* [Configureren van het dashboard](./howto-configure-homepage.md)
* [Beheer van apparaten in uw Azure IoT Central-toepassing](howto-manage-devices.md)