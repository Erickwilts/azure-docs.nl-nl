---
title: Architectuur concepten in Azure IoT Central | Microsoft Docs
description: Dit artikel bevat belangrijke concepten die betrekking hebben de architectuur van Azure IoT Central
author: dominicbetts
ms.author: dobett
ms.date: 05/31/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 4bc9a79576c3165585a4a2c897bd41bfb77c080c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66693128"
---
# <a name="azure-iot-central-architecture"></a>Azure IoT Central-architectuur

Dit artikel bevat een overzicht van de architectuur van Microsoft Azure IoT Central.

![Op het hoogste niveau architectuur](media/concepts-architecture/architecture.png)

## <a name="devices"></a>Apparaten

Apparaten uitwisselen van gegevens met uw Azure IoT Central-toepassing. Een apparaat kunt doen:

- Metingen zoals telemetrie verzenden.
- Instellingen synchroniseren met uw toepassing.

De gegevens die een apparaat met uw toepassing uitwisselen kan is in Azure IoT Central opgegeven in de sjabloon van een apparaat. Zie voor meer informatie over apparaatsjablonen [metagegevens management](#metadata-management).

Zie voor meer informatie over hoe apparaten verbinding met uw Azure IoT Central-toepassing maken, [apparaatconnectiviteit](concepts-connectivity.md).

## <a name="cloud-gateway"></a>Cloudgateway

Azure IoT Central maakt gebruik van Azure IoT Hub als een cloudgateway waarmee de connectiviteit van apparaten. Met IoT Hub kunt:

- Opname van gegevens op schaal in de cloud.
- Beheer van apparaten.
- Beveiligde connectiviteit van apparaten.

Zie voor meer informatie over IoT Hub, [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub/).

Zie voor meer informatie over de connectiviteit van apparaten in Azure IoT Central, [apparaatconnectiviteit](concepts-connectivity.md).

## <a name="data-stores"></a>Gegevensarchieven

Azure IoT Central worden toepassingsgegevens opgeslagen in de cloud. Toepassingsgegevens opgeslagen bevat:

- Apparaatsjablonen.
- Apparaat-id's.
- Metagegevens van apparaten.
- Gebruiker en rol gegevens.

Azure IoT Central maakt gebruik van een time series opslaan voor de meting die worden verzonden van uw apparaten. Time series-gegevens van apparaten die worden gebruikt door de analytics-service.

## <a name="analytics"></a>Analyse

De analytics-service is verantwoordelijk voor het genereren van de aangepaste rapportagegegevens die de toepassing worden weergegeven. Een operator kan [aanpassen van de analyse](howto-create-analytics.md) weergegeven in de toepassing. De analytics-service is gebouwd boven [Azure Time Series Insights](https://azure.microsoft.com/services/time-series-insights/) en verwerkt de meetgegevens van uw apparaten worden verzonden.

## <a name="rules-and-actions"></a>Regels en acties

[Regels en acties](howto-create-telemetry-rules.md) werken nauw samen om taken in de toepassing te automatiseren. Een opbouwfunctie kunt definiëren regels op basis van telemetrie van apparaten, zoals de temperatuur van meer dan een opgegeven drempelwaarde. Azure IoT Central gebruikt een processor voor streaming om te bepalen wanneer de regelvoorwaarden wordt voldaan. Als een voorwaarde wordt voldaan, wordt er een actie gedefinieerd door de opbouwfunctie voor geactiveerd. Een actie kan bijvoorbeeld een e-mail naar een technicus die de temperatuur van een apparaat te hoog is verzenden.

## <a name="metadata-management"></a>Beheer van metagegevens

Apparaatsjablonen definiëren in een Azure IoT Central-toepassing, het gedrag en de mogelijkheid van de typen apparaten. Bijvoorbeeld, een apparaat koelkast sjabloon Hiermee geeft u de telemetrie die een koelkast worden verzonden naar uw toepassing.

![Sjabloon-architectuur](media/concepts-architecture/template_architecture.png)

In de sjabloon van een apparaat:

- **Metingen** geeft de telemetrie van het apparaat worden verzonden naar de toepassing.
- **Instellingen** geeft de configuraties die een operator kan instellen.
- **Eigenschappen** metagegevens die een operator kan ingesteld opgeven.
- **Regels** gedrag in de toepassing op basis van gegevens die zijn verzonden vanaf een apparaat te automatiseren.
- **Dashboards** aanpasbare weergaven van een apparaat in de toepassing.

Een toepassing kan een of meer gesimuleerde en echte apparaten op basis van de sjabloon voor elk apparaat hebben.

## <a name="data-export"></a>Gegevens exporteren

In een Azure IoT Central-toepassing, kunt u [uw gegevens continu exporteren](howto-export-data-event-hubs-service-bus.md) aan uw eigen Azure Event Hubs en Azure Service Bus-instanties. U kunt uw gegevens ook periodiek exporteren naar uw Azure Blob storage-account. IoT Central kunnen metingen, apparaten en apparaatsjablonen exporteren.

## <a name="batch-device-updates"></a>Bijwerken van de batch-apparaten

In een Azure IoT Central-toepassing, kunt u [maken en uitvoeren van taken](howto-run-a-job.md) voor het beheren van verbonden apparaten. Deze taken kunt u doen bulksgewijs updates van apparaateigenschappen of instellingen of opdrachten worden uitgevoerd. U kunt bijvoorbeeld een taak voor het verhogen van de snelheid van de ventilator voor meerdere gekoeld verkoopautomaten maken.

## <a name="role-based-access-control-rbac"></a>Toegangsbeheer op basis van rollen (RBAC)

Een [de beheerder kan toegangsregels definiëren](howto-administer.md) voor een Azure IoT Central-toepassing met behulp van de vooraf gedefinieerde rollen. Een beheerder kan gebruikers toewijzen aan rollen om te bepalen welke aspecten van de toepassing voor de gebruiker toegang heeft.

## <a name="security"></a>Beveiliging

Beveiligingsfuncties in Azure IoT Central zijn onder andere:

- In-transit en inactieve gegevens versleuteld.
- Verificatie wordt verstrekt door Azure Active Directory of Microsoft-Account. Verificatie met twee factoren wordt ondersteund.
- Isolatie van volledige tenants.
- Beveiliging op apparaat.

## <a name="ui-shell"></a>UI-shell

De UI-shell is een moderne, responsieve, HTML5 browser-gebaseerde toepassing.
Een beheerder kan de gebruikersinterface van de toepassing aanpassen door aangepaste thema's toepassen en wijzigen van de help-koppelingen om te verwijzen naar uw eigen aangepaste help-resources. Zie voor meer informatie over de UI-aanpassing, [aanpassen van de Azure IoT Central gebruikersinterface](howto-customize-ui.md) artikel.

Een operator kunt toepassing persoonlijke dashboards maken. U kunt verschillende dashboards die andere gegevens weergeven en tussen deze twee schakelt hebben.

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd over de architectuur van Azure IoT Central, de voorgestelde volgende stap is te leren over [apparaatconnectiviteit](concepts-connectivity.md) in Azure IoT Central.