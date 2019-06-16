---
title: Azure IoT Hub migreren naar diagnostische instellingen | Microsoft Docs
description: Het bijwerken van Azure IoT Hub voor het gebruik van Azure diagnostics-instellingen in plaats van bewerkingen voor het controleren van de status van bewerkingen op uw IoT-hub in realtime controleren.
author: kgremban
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 03/11/2019
ms.author: kgremban
ms.openlocfilehash: b6cde8402c699a7477cd0efc79a44b3f5e150ad0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66146322"
---
# <a name="migrate-your-iot-hub-from-operations-monitoring-to-diagnostics-settings"></a>Migreren van uw IoT-Hub van bewerkingen controleren naar diagnostische instellingen

Klanten die gebruikmaken van [bewerkingen controleren](iot-hub-operations-monitoring.md) om bij te houden van de status van bewerkingen in IoT Hub kunnen migreren die werkstroom [Azure diagnostische instellingen](../azure-monitor/platform/diagnostic-logs-overview.md), een functie van Azure Monitor. Instellingen voor diagnostische gegevens leveren resourceniveau diagnostische gegevens voor veel Azure-services.

**De bewakingsfunctionaliteit van IoT Hub is afgeschaft bewerkingen**, en is verwijderd uit de portal. In dit artikel bevat stappen voor het verplaatsen van uw workloads van bewerkingen controleren naar diagnostische instellingen. Zie voor meer informatie over de tijdlijn afschaffing [bewaken van uw Azure-IoT-oplossingen met Azure Monitor en Azure Resource Health](https://azure.microsoft.com/blog/monitor-your-azure-iot-solutions-with-azure-monitor-and-azure-resource-health/).

## <a name="update-iot-hub"></a>IoT Hub bijwerken

Voor het bijwerken van uw IoT-Hub in Azure portal, eerst de diagnostische instellingen inschakelen en uitschakelen van bewerkingen controleren.  

[!INCLUDE [iot-hub-diagnostics-settings](../../includes/iot-hub-diagnostics-settings.md)]

### <a name="turn-off-operations-monitoring"></a>Bewerkingen controleren uitschakelen

> [!NOTE]
> Als van 11 maart 2019, de bewerkingen die functie voor toepassingsbewaking wordt verwijderd uit Azure portal-interface van IoT-Hub. De onderstaande stappen niet langer van toepassing. Als u wilt migreren, zorg ervoor dat de juiste categorieën zijn ingeschakeld in de diagnostische instellingen van Azure Monitor hierboven.

Nadat u de nieuwe instellingen voor diagnostische gegevens in uw werkstroom hebt getest, kunt u de bewerkingen controleren van functie uitschakelen. 

1. Selecteer in uw IoT Hub-menu **bewerkingen controleren**.

2. Selecteer onder elke categorie bewaking **geen**.

3. Sla de bewerkingen die het bewaken van wijzigingen.

## <a name="update-applications-that-use-operations-monitoring"></a>Bijwerken van toepassingen die gebruikmaken van bewerkingen controleren

De schema's voor bewerkingen controleren en instellingen voor diagnostische gegevens afwijken enigszins. Het is belangrijk dat u de toepassingen die gebruikmaken van bewerkingen controleren vandaag om toe te wijzen aan het schema dat wordt gebruikt door de diagnostische instellingen bijwerken. 

Diagnostische instellingen biedt ook vijf nieuwe categorieën voor het bijhouden. Na het bijwerken van toepassingen voor de bestaande schema's, evenals de nieuwe categorieën worden toegevoegd:

* Cloud-naar-apparaat dubbele bewerkingen
* Dubbele apparaat-naar-cloud-bewerkingen
* Apparaatdubbel-query 's
* Taakbewerkingen
* Directe methoden

Zie voor de specifieke schemastructuren, [inzicht in het schema voor de diagnostische instellingen](iot-hub-monitor-resource-health.md#understand-the-logs).

## <a name="monitoring-device-connect-and-disconnect-events-with-low-latency"></a>Bewaking van apparaat verbinding maken en gebeurtenissen met een lage latentie verbreken

Voor het bewaken van apparaat verbinding maakt en disconnect-gebeurtenissen in de productieomgeving, wordt aangeraden zich abonneert op de [ **apparaat verbroken** gebeurtenis](iot-hub-event-grid.md#event-types) op Event Grid waarschuwingen ontvangen en controleren van de verbindingsstatus van het apparaat. Gebruik deze [zelfstudie](iot-hub-how-to-order-connection-state-events.md) voor informatie over het apparaat is verbonden en het apparaat verbroken gebeurtenissen uit IoT Hub in uw IoT-oplossing integreren.

## <a name="next-steps"></a>Volgende stappen

[De status van Azure IoT Hub bewaken en snel problemen vaststellen](iot-hub-monitor-resource-health.md)
