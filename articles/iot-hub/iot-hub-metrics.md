---
title: Metrische gegevens gebruiken voor het bewaken van Azure IoT Hub | Microsoft Docs
description: Het gebruik van Azure IoT Hub metrische gegevens om te beoordelen en controleren van de algemene status van uw IoT-hubs.
author: jlian
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/24/2019
ms.author: jlian
ms.openlocfilehash: 6afebfe9a5db713e31fed0acd2e8ad7244f30037
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67274922"
---
# <a name="understand-iot-hub-metrics"></a>Informatie over metrische gegevens van IoT Hub

IoT Hub metrische gegevens geven u meer gegevens over de status van de Azure IoT-resources in uw Azure-abonnement. Metrische gegevens van IoT Hub kunt u de algemene status van de IoT Hub-service en de apparaten die zijn verbonden met het beoordelen. Gebruikersgerichte statistieken zijn belangrijk omdat ze helpen om zien wat er gebeurt met uw IoT hub en help hoofdoorzaak problemen te zonder contact opnemen met ondersteuning van Azure.

Metrische gegevens zijn standaard ingeschakeld. U kunt IoT Hub metrische gegevens van de Azure-portal weergeven.

## <a name="how-to-view-iot-hub-metrics"></a>IoT Hub metrische gegevens weergeven

1. Maak een IoT-hub. U vindt instructies over het maken van een IoT-hub in de [verzenden van telemetrie vanaf een apparaat naar IoT Hub](quickstart-send-telemetry-dotnet.md) handleiding.

2. Open de blade van uw IoT-hub. Van daaruit, klikt u op **metrische gegevens**.
   
    ![Schermopname die laat zien waar de optie metrische gegevens zich in de resourcepagina van de IoT Hub](./media/iot-hub-metrics/enable-metrics-1.png)

3. U kunt de metrische gegevens weergeven voor uw IoT-hub en aangepaste weergaven van uw metrische gegevens maken op de blade metrische gegevens. 
   
    ![Schermopname van de pagina metrische gegevens van IoT Hub](./media/iot-hub-metrics/enable-metrics-2.png)
    
4. U kunt uw metrische gegevens verzenden naar een Event Hubs-eindpunt of een Azure Storage-account door te klikken op **diagnostische instellingen**, klikt u vervolgens **diagnostische instelling toevoegen**

   ![Schermopname die laat zien waar de knop diagnostische instellingen is](./media/iot-hub-metrics/enable-metrics-3.png)

## <a name="iot-hub-metrics-and-how-to-use-them"></a>IoT Hub metrische gegevens en hoe ze te gebruiken

IoT Hub biedt verschillende metrische gegevens zodat u een overzicht van de status van uw hub en het totale aantal verbonden apparaten. U kunt gegevens uit meerdere metrische gegevens tekenen van een grotere beeld van de status van de IoT-hub kunt combineren. De volgende tabel beschrijft de metrische gegevens die elke IoT-hub worden bijgehouden, en hoe alle gegevens zich verhoudt tot de algemene status van de IoT-hub.

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|d2c<br>.telemetry<br>.ingress.<br>allProtocol|Telemetrie-bericht verzenden pogingen|Count|Totaal|Aantal telemetrieberichten dat apparaat-naar-cloud heeft geprobeerd om te worden verzonden naar uw IoT-hub|Er zijn geen dimensies|
|d2c<br>.telemetry<br>.ingress<br>.success|Berichten over telemetrie verzonden|Count|Totaal|Aantal telemetrieberichten dat is verzonden naar uw IoT hub apparaat-naar-cloud|Er zijn geen dimensies|
|c2d<br>.Commands<br>.egress<br>.complete<br>.success|Opdrachten voltooid|Count|Totaal|Aantal opdrachten voor cloud-naar-apparaat is voltooid door het apparaat|Er zijn geen dimensies|
|c2d<br>.Commands<br>.egress<br>.abandon<br>.success|Opdrachten afgebroken|Count|Totaal|Aantal cloud-naar-apparaatopdrachten afgebroken door het apparaat|Er zijn geen dimensies|
|c2d<br>.Commands<br>.egress<br>.reject<br>.success|Opdrachten geweigerd|Count|Totaal|Aantal cloud-naar-apparaatopdrachten geweigerd door het apparaat|Er zijn geen dimensies|
|apparaten<br>.totalDevices|Totaal aantal apparaten (afgeschaft)|Count|Totaal|Aantal apparaten die zijn geregistreerd met uw IoT hub|Er zijn geen dimensies|
|apparaten<br>.connectedDevices<br>.allProtocol|Verbonden apparaten (afgeschaft) |Count|Totaal|Aantal apparaten dat is verbonden met uw IoT-hub|Er zijn geen dimensies|
|d2c<br>.telemetry<br>.egress<br>.success|Routering: telemetrieberichten geleverd|Count|Totaal|Het aantal keren dat berichten zijn bezorgd bij alle eindpunten met behulp van IoT Hub-routering. Als een bericht wordt doorgestuurd naar meerdere eindpunten, wordt deze waarde verhoogd met één voor elke geslaagde levering. Als een bericht wordt bezorgd in het hetzelfde eindpunt meerdere keren, wordt deze waarde verhoogd met één voor elke geslaagde levering.|Er zijn geen dimensies|
|d2c<br>.telemetry<br>.egress<br>.dropped|Routering: telemetrieberichten verwijderd |Count|Totaal|Het aantal keren dat berichten door de IoT-Hub routering vanwege andere eindpunten zijn verwijderd. Deze waarde is, telt niet berichten die in de alternatieve route, zoals verwijderde berichten worden er niet bezorgd.|Er zijn geen dimensies|
|d2c<br>.telemetry<br>.egress<br>.orphaned|Routering: telemetrieberichten zwevende |Count|Totaal|Het aantal keren dat berichten door de IoT Hub routering zwevende omdat ze niet overeenkomen met alle regels voor doorsturen (met inbegrip van de alternatieve regel). |Er zijn geen dimensies|
|d2c<br>.telemetry<br>.egress<br>.Invalid|Routering: telemetrieberichten niet compatibel|Count|Totaal|Het aantal keren dat berichten worden verzonden vanwege compatibiliteitsproblemen met het eindpunt van de IoT-Hub routering kan niet. Deze waarde bevat geen nieuwe pogingen.|Er zijn geen dimensies|
|d2c<br>.telemetry<br>.egress<br>.fallback|Routering: berichten worden afgeleverd bij terugval|Count|Totaal|Het aantal keren dat het IoT-Hub routering berichten naar het eindpunt dat is gekoppeld aan de alternatieve route geleverd.|Er zijn geen dimensies|
|d2c<br>.endpoints<br>.egress<br>.eventHubs|Routering: berichten die naar Event Hub worden geleverd|Count|Totaal|Het aantal keren routering is IoT-Hub die berichten worden geleverd aan Event Hub-eindpunten.|Er zijn geen dimensies|
|d2c<br>.endpoints<br>.latency<br>.eventHubs|Routering: bericht latentie voor Event Hub|Milliseconden|Gemiddeld|De gemiddelde latentie (in milliseconden) tussen bericht inkomend verkeer naar IoT Hub en bericht inkomend verkeer naar een Event Hub-eindpunt.|Er zijn geen dimensies|
|d2c<br>.endpoints<br>.egress<br>.serviceBusQueues|Routering: berichten worden afgeleverd bij Service Bus-wachtrij|Count|Totaal|Het aantal keren dat het IoT Hub is routering berichten geleverd aan Service Bus-wachtrij-eindpunten.|Er zijn geen dimensies|
|d2c<br>.endpoints<br>.latency<br>.serviceBusQueues|Routering: bericht latentie voor Service Bus-wachtrij|Milliseconden|Gemiddeld|De gemiddelde latentie (in milliseconden) tussen bericht inkomend verkeer naar IoT Hub en telemetrie bericht inkomend verkeer in een Service Bus-wachtrij-eindpunt.|Er zijn geen dimensies|
|d2c<br>.endpoints<br>.egress<br>.serviceBusTopics|Routering: berichten worden afgeleverd bij Service Bus-onderwerp|Count|Totaal|Het aantal keren dat het IoT Hub is routering berichten aan Service Bus-onderwerp eindpunten geleverd.|Er zijn geen dimensies|
|d2c<br>.endpoints<br>.latency<br>.serviceBusTopics|Routering: bericht latentie voor Service Bus-onderwerp|Milliseconden|Gemiddeld|De gemiddelde latentie (in milliseconden) tussen bericht inkomend verkeer naar IoT Hub en telemetrie bericht inkomend verkeer in een Service Bus-onderwerp-eindpunt.|Er zijn geen dimensies|
|d2c<br>.endpoints<br>.egress<br>.builtIn<br>.events|Routering: berichten worden afgeleverd bij berichten/gebeurtenissen|Count|Totaal|Het aantal keren dat het IoT Hub is routering berichten aan het ingebouwde eindpunt (berichten/gebeurtenissen) geleverd. Met deze metriek wordt alleen gestart werken wanneer routering is ingeschakeld (https://aka.ms/iotrouting) voor de IoT hub.|Er zijn geen dimensies|
|d2c<br>.endpoints<br>.latency<br>. builtIn.events|Routering: bericht latentie voor berichten/gebeurtenissen|Milliseconden|Gemiddeld|De gemiddelde latentie (in milliseconden) tussen bericht inkomend verkeer naar IoT Hub en telemetrie bericht inkomend verkeer naar het eindpunt van de ingebouwde (berichten/gebeurtenissen). Met deze metriek wordt alleen gestart werken wanneer routering is ingeschakeld (https://aka.ms/iotrouting) voor de IoT hub.|Er zijn geen dimensies|
|d2c<br>.endpoints<br>.egress<br>.storage|Routering: berichten die naar de opslag worden geleverd|Count|Totaal|Het aantal keren dat het IoT Hub is routering berichten aan opslag eindpunten geleverd.|Er zijn geen dimensies|
|d2c<br>.endpoints<br>.latency<br>.storage|Routering: bericht latentie voor opslag|Milliseconden|Gemiddeld|De gemiddelde latentie (in milliseconden) tussen bericht inkomend verkeer naar IoT Hub en telemetrie bericht inkomend verkeer in een storage-eindpunt.|Er zijn geen dimensies|
|d2c<br>.endpoints<br>.egress<br>.storage<br>.bytes|Routering: gegevens geleverd aan opslag|Bytes|Totaal|De hoeveelheid gegevens (bytes) routering IoT-Hub die worden geleverd aan opslag-eindpunten.|Er zijn geen dimensies|
|d2c<br>.endpoints<br>.egress<br>.storage<br>.blobs|Routering: blobs die worden geleverd aan opslag|Count|Totaal|Het aantal keren dat het IoT-Hub routering blobs geleverd aan opslag-eindpunten.|Er zijn geen dimensies|
|EventGridDeliveries|Event Grid levering (preview)|Count|Totaal|Het aantal IoT Hub-gebeurtenissen naar Event Grid worden gepubliceerd. Gebruik de resultaat-dimensie voor het aantal geslaagde en mislukte aanvragen. Type gebeurtenis dimensie ziet u het type gebeurtenis (https://aka.ms/ioteventgrid). Zien de waar de aanvragen afkomstig zijn van, gebruikt u de dimensie van het type gebeurtenis.|Result, EventType|
|EventGridLatency|Event Grid latentie (preview)|Milliseconden|Gemiddeld|De gemiddelde latentie (in milliseconden) als de Iot Hub-gebeurtenis is gegenereerd wanneer de gebeurtenis is gepubliceerd naar Event Grid. Dit nummer is een gemiddelde tussen alle gebeurtenistypen. Gebruik de dimensie van het type gebeurtenis om te zien van de latentie van een specifiek type gebeurtenis.|EventType|
|d2c<br>.twin<br>.read<br>.success|Geslaagde dubbele leesbewerkingen van apparaten|Count|Totaal|Het aantal voltooide dubbele apparaat geïnitieerde leesbewerkingen.|Er zijn geen dimensies|
|d2c<br>.twin<br>.read<br>.failure|Dubbele leesbewerkingen van apparaten is mislukt|Count|Totaal|De telling van alle dubbele apparaat geïnitieerde leesbewerkingen mislukt.|Er zijn geen dimensies|
|d2c<br>.twin<br>.read<br>.size|Reactiegrootte van dubbele leesbewerkingen van apparaten|Bytes|Gemiddeld|De gemiddelde, de minimale en het maximale van alle geslaagde apparaat geïnitieerde twin leesbewerkingen.|Er zijn geen dimensies|
|d2c<br>.twin<br>.update<br>.success|Geslaagde apparaatdubbel werkt bij van apparaten|Count|Totaal|Het aantal voltooide apparaat geïnitieerde apparaatdubbel werkt.|Er zijn geen dimensies|
|d2c<br>.twin<br>.update<br>.failure|Apparaatdubbel werkt bij van apparaten is mislukt|Count|Totaal|De telling van alle mislukte dubbel apparaat geïnitieerde updates.|Er zijn geen dimensies|
|d2c<br>.twin<br>.update<br>.size|Grootte van apparaatdubbel werkt bij van apparaten|Bytes|Gemiddeld|De gemiddelde, de minimale en het maximale grootte van alle geslaagde apparaat geïnitieerde twin updates.|Er zijn geen dimensies|
|c2d<br>.Methods<br>.success|Geslaagde rechtstreekse methodeaanroepen|Count|Totaal|Het aantal voltooide rechtstreekse methodeaanroepen.|Er zijn geen dimensies|
|c2d<br>.Methods<br>.failure|Rechtstreekse methodeaanroepen is mislukt|Count|Totaal|De telling van alle mislukte aanroepen van de directe methode.|Er zijn geen dimensies|
|c2d<br>.Methods<br>.requestSize|Grootte van de aanvraag van een rechtstreekse methodeaanroepen|Bytes|Gemiddeld|De gemiddelde, minimale en maximale van alle geslaagde aanvragen voor directe methode.|Er zijn geen dimensies|
|c2d<br>.Methods<br>.responseSize|Reactiegrootte van rechtstreekse methodeaanroepen|Bytes|Gemiddeld|De gemiddelde, minimale en maximale van alle geslaagde antwoorden in de directe methode.|Er zijn geen dimensies|
|c2d<br>.twin<br>.read<br>.success|Geslaagde dubbele leesbewerkingen van back-end|Count|Totaal|De telling van alle geslaagde back-end-gestart dubbele leesbewerkingen.|Er zijn geen dimensies|
|c2d<br>.twin<br>.read<br>.failure|Mislukte dubbele leesbewerkingen van back-end|Count|Totaal|De telling van alle back-end-gestart dubbele leesbewerkingen mislukt.|Er zijn geen dimensies|
|c2d<br>.twin<br>.read<br>.size|Reactiegrootte van dubbele leesbewerkingen van back-end|Bytes|Gemiddeld|De gemiddelde, de minimale en het maximale van alle geslaagde back-end-gestart twin leesbewerkingen.|Er zijn geen dimensies|
|c2d<br>.twin<br>.update<br>.success|Geslaagde apparaatdubbel werkt bij vanaf back-end|Count|Totaal|De telling van alle geslaagde back-end-gestart apparaatdubbel werkt.|Er zijn geen dimensies|
|c2d<br>.twin<br>.update<br>.failure|Mislukte apparaatdubbel werkt bij vanaf back-end|Count|Totaal|De telling van alle mislukte back-end-gestart dubbele updates.|Er zijn geen dimensies|
|c2d<br>.twin<br>.update<br>.size|Grootte van apparaatdubbel werkt bij vanaf back-end|Bytes|Gemiddeld|De gemiddelde, de minimale en het maximale grootte van alle geslaagde back-end-gestart twin updates.|Er zijn geen dimensies|
|twinQueries<br>.success|Geslaagde apparaatdubbel-query 's|Count|Totaal|De telling van alle geslaagde apparaatdubbel-query's.|Er zijn geen dimensies|
|twinQueries<br>.failure|Mislukte apparaatdubbel-query 's|Count|Totaal|De telling van alle mislukte apparaatdubbel-query's.|Er zijn geen dimensies|
|twinQueries<br>.resultSize|Grootte van apparaatdubbel-query 's|Bytes|Gemiddeld|De gemiddelde, minimale en maximale van de grootte van het resultaat van alle geslaagde apparaatdubbel-query's.|Er zijn geen dimensies|
|jobs<br>.createTwinUpdateJob<br>.success|Geslaagde bewerkingen voor het maken van dubbele taken bijwerken|Count|Totaal|De telling van alle is gemaakt van dubbele update taken.|Er zijn geen dimensies|
|jobs<br>.createTwinUpdateJob<br>.failure|Mislukte bewerkingen voor het maken van dubbele taken bijwerken|Count|Totaal|De telling van alle mislukte aanmaak van dubbele taken bijwerken.|Er zijn geen dimensies|
|jobs<br>.createDirectMethodJob<br>.success|Geslaagde bewerkingen voor het maken van de methode aanroepen van taken|Count|Totaal|De telling van alle is gemaakt van een rechtstreekse methode aanroepen van taken.|Er zijn geen dimensies|
|jobs<br>.createDirectMethodJob<br>.failure|Mislukte bewerkingen voor het maken van de methode aanroepen van taken|Count|Totaal|De telling van alle mislukte aanmaak van rechtstreekse methode aanroepen van taken.|Er zijn geen dimensies|
|jobs<br>.listJobs<br>.success|Geslaagde aanroepen naar de lijst met taken|Count|Totaal|Het aantal geslaagde aanroepen naar de lijst met taken.|Er zijn geen dimensies|
|jobs<br>.listJobs<br>.failure|Mislukte aanroepen naar de lijst met taken|Count|Totaal|Het aantal mislukte aanroepen naar de lijst met taken.|Er zijn geen dimensies|
|jobs<br>.cancelJob<br>.success|Geslaagde taakannuleringen|Count|Totaal|Het aantal geslaagde aanroepen naar een taak annuleren.|Er zijn geen dimensies|
|jobs<br>.cancelJob<br>.failure|Mislukte taakannuleringen|Count|Totaal|Het aantal mislukte aanroepen naar een taak annuleren.|Er zijn geen dimensies|
|jobs<br>.queryJobs<br>.success|Taak query 's|Count|Totaal|Het aantal geslaagde aanroepen naar query taken.|Er zijn geen dimensies|
|jobs<br>.queryJobs<br>.failure|Taak query's is mislukt|Count|Totaal|Het aantal mislukte aanroepen naar query taken.|Er zijn geen dimensies|
|jobs<br>.completed|Voltooide taken|Count|Totaal|De telling van alle voltooide taken.|Er zijn geen dimensies|
|jobs<br>.failed|Mislukte taken|Count|Totaal|De telling van alle mislukte taken.|Er zijn geen dimensies|
|d2c<br>.telemetry<br>.ingress<br>.sendThrottle|Aantal beperkingsfouten|Count|Totaal|Aantal beperkingsfouten vanwege apparaat doorvoer beperkt|Er zijn geen dimensies|
|dailyMessage<br>QuotaUsed|Totaal aantal berichten dat is gebruikt|Count|Gemiddeld|Het aantal totaal aantal berichten momenteel gebruikt. Dit is een cumulatieve waarde die is ingesteld op nul om 00:00 UTC elke dag.|Er zijn geen dimensies|
|deviceDataUsage|Totaal aantal apparaat gegevensgebruik|Bytes|Totaal|Bytes overgebracht naar en van alle apparaten die zijn verbonden met IotHub|Er zijn geen dimensies|
|totalDeviceCount|Totaal aantal apparaten (preview)|Count|Gemiddeld|Aantal apparaten die zijn geregistreerd met uw IoT hub|Er zijn geen dimensies|
|Verbonden<br>DeviceCount|Verbonden apparaten (preview)|Count|Gemiddeld|Aantal apparaten dat is verbonden met uw IoT-hub|Er zijn geen dimensies|
|Configuraties|Configuratie van metrische gegevens|Count|Totaal|Metrische gegevens voor configuratiebewerkingen|Er zijn geen dimensies|

## <a name="next-steps"></a>Volgende stappen

Nu u een overzicht van metrische gegevens van IoT Hub hebt gezien, volgt u deze koppeling voor meer informatie over het beheren van Azure IoT Hub:

* [Controle van bewerkingen](iot-hub-operations-monitoring.md)

Als u wilt de mogelijkheden van IoT Hub verder verkennen, Zie:

* [Ontwikkelaarshandleiding voor IoT Hub](iot-hub-devguide.md)

* [AI implementeren op edge-apparaten met Azure IoT Edge](../iot-edge/tutorial-simulate-device-linux.md)
