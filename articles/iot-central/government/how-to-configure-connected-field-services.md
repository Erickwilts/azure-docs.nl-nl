---
title: Uw Azure IoT Central-toepassing verbinden met Dynamics 365 Field Services | Microsoft Docs
description: Meer informatie over hoe u een end-to-end-oplossing bouwt met de veld Service Azure IoT Central en Dynamics 365
author: miriambrus
ms.author: miriamb
ms.date: 10/23/2019
ms.topic: overview
ms.service: iot-central
services: iot-central
ms.openlocfilehash: ae266495db1d6b94a43aa962a3e9b63a8115c526
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/05/2020
ms.locfileid: "77017664"
---
# <a name="build-end-to-end-solution-with-azure-iot-central-and-dynamics-365-field-service"></a>End-to-end-oplossing bouwen met de veld Service Azure IoT Central en Dynamics 365 



Als ontwerper kunt u de integratie van uw Azure IoT Central-toepassing naar andere bedrijfs systemen inschakelen. 


In een oplossing voor een verbonden afval beheer kunt u bijvoorbeeld de verzen ding van heftrucks voor vuilnis verzamelingen optimaliseren. De optimalisatie kan worden uitgevoerd op basis van IoT Sens oren-gegevens van verbonden afval bakken. In uw [IOT Central verbonden afval beheer toepassing](./tutorial-connected-waste-management.md) kunt u regels en acties configureren en deze instellen om waarschuwingen te maken in de Dynamics-veld service. Dit scenario wordt bereikt door gebruik te maken van Microsoft Flow, dat rechtstreeks in IoT Central wordt geconfigureerd voor het automatiseren van werk stromen tussen toepassingen en services. Daarnaast kunnen gegevens op basis van service activiteiten in de veld service worden teruggestuurd naar Azure IoT Central. 

## <a name="how-to-connect-your-azure-iot-central-application-with-dynamics-365-field-services"></a>Uw Azure IoT Central-toepassing verbinden met Dynamics 365 Field Services 

De onderstaande integratie processen kunnen eenvoudig worden geïmplementeerd op basis van een zuivere configuratie-ervaring:
* Azure IoT Central kan informatie over anomalieën van apparaten verzenden naar de verbonden veld Service (als een IoT-waarschuwing) voor diagnose.
* Met de verbonden veld service kunnen cases of werk orders worden gemaakt die worden geactiveerd door afwijkingen van het apparaat.
* Verbonden veld service kan technici plannen voor inspectie om uitval incidenten te voor komen.
* Het dash board van Azure IoT Central kan worden bijgewerkt met relevante service-en plannings informatie.


## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [IOT Central government-sjablonen](./overview-iot-central-government.md)
* Meer informatie over [IOT Central](https://docs.microsoft.com/azure/iot-central/core/overview-iot-central)
* Meer informatie over [Dynamics 365 Field Services](https://docs.microsoft.com/dynamics365/field-service/cfs-iot-overview)
