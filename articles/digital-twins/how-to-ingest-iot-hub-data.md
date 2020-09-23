---
title: Telemetrie opnemen vanuit IoT Hub
titleSuffix: Azure Digital Twins
description: Zie telemetrie-berichten van apparaten inslikken van IoT Hub.
author: alexkarcher-msft
ms.author: alkarche
ms.date: 9/15/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 9fa3c27f9cc35b31fc78b2a09bea725934093e63
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/22/2020
ms.locfileid: "90983328"
---
# <a name="ingest-iot-hub-telemetry-into-azure-digital-twins"></a>IoT Hub telemetrie opnemen in azure Digital Apparaatdubbels

Azure Digital Apparaatdubbels wordt gestuurd met gegevens van IoT-apparaten en andere bronnen. Een algemene bron voor de apparaatgegevens die in azure Digital Apparaatdubbels moet worden gebruikt, is [IOT hub](../iot-hub/about-iot-hub.md).

Het proces voor het opnemen van gegevens in azure Digital Apparaatdubbels is het instellen van een externe reken resource, zoals een [Azure-functie](../azure-functions/functions-overview.md), waarmee de gegevens worden ontvangen en de [DigitalTwins-api's](how-to-use-apis-sdks.md) worden gebruikt voor het instellen van eigenschappen of faxgebeurtenissen op [digitale apparaatdubbels](concepts-twins-graph.md) dienovereenkomstig. 

In dit document vindt u instructies voor het schrijven van een Azure-functie waarmee telemetrie kan worden opgenomen in IoT Hub.

## <a name="prerequisites"></a>Vereisten

Voordat u verder gaat met dit voor beeld, moet u de volgende resources instellen als vereisten:
* **Een IOT-hub**. Zie de sectie *een IOT hub maken* van [deze IOT hub Snelstartgids](../iot-hub/quickstart-send-telemetry-cli.md)voor instructies.
* **Een Azure-functie** met de juiste machtigingen voor het aanroepen van uw digitale dubbele instantie. Zie [*How to: een Azure-functie instellen voor het verwerken van gegevens*](how-to-create-azure-function.md)voor instructies. 
* **Een Azure Digital apparaatdubbels-exemplaar** dat telemetrie van uw apparaat ontvangt. Zie [*How to: een Azure Digital apparaatdubbels-exemplaar en-verificatie instellen*](./how-to-set-up-instance-portal.md)voor instructies.

### <a name="example-telemetry-scenario"></a>Voor beeld van telemetrie-scenario

In deze procedure wordt beschreven hoe u berichten van IoT Hub naar Azure Digital Apparaatdubbels verzendt met behulp van een Azure-functie. Er zijn veel mogelijke configuraties en overeenkomende strategieën die u kunt gebruiken voor het verzenden van berichten, maar het voor beeld voor dit artikel bevat de volgende onderdelen:
* Een thermo meter-apparaat in IoT Hub met een bekende apparaat-ID
* Een digitale dubbele om het apparaat te vertegenwoordigen met een overeenkomende ID

> [!NOTE]
> In dit voor beeld wordt een eenvoudige ID-overeenkomst gebruikt tussen de apparaat-ID en de bijbehorende digitale dubbele ID, maar het is mogelijk om meer geavanceerde toewijzingen van het apparaat aan de dubbele waarde toe te voegen (zoals bij een toewijzings tabel).

Wanneer een temperatuur telemetrie-gebeurtenis wordt verzonden door het Thermo staat-apparaat, wordt de telemetrie door een Azure-functie verwerkt en wordt de eigenschap *Tempe ratuur* van de digitale dubbele taak moet worden bijgewerkt. Dit scenario wordt beschreven in een diagram hieronder:

:::image type="content" source="media/how-to-ingest-iot-hub-data/events.png" alt-text="Een diagram waarin een stroom diagram wordt weer gegeven. In de grafiek verzendt een IoT Hub apparaat een temperatuur telemetrie via IoT Hub naar een Azure-functie, waarmee een temperatuur eigenschap wordt bijgewerkt op een dubbele in azure Digital Apparaatdubbels." border="false":::

## <a name="add-a-model-and-twin"></a>Een model en dubbele toevoegen

U kunt een model toevoegen/uploaden met behulp van de CLI-opdracht hieronder en vervolgens een dubbele gebruiken dit model maken dat wordt bijgewerkt met informatie uit IoT Hub.

Het model ziet er als volgt uit:
```JSON
{
  "@id": "dtmi:contosocom:DigitalTwins:Thermostat;1",
  "@type": "Interface",
  "@context": "dtmi:dtdl:context;2",
  "contents": [
    {
      "@type": "Property",
      "name": "Temperature",
      "schema": "double"
    }
  ]
}
```

Als u **dit model wilt uploaden naar uw apparaatdubbels-exemplaar**, opent u de Azure CLI en voert u de volgende opdracht uit:

```azurecli
az dt model create --models '{  "@id": "dtmi:contosocom:DigitalTwins:Thermostat;1",  "@type": "Interface",  "@context": "dtmi:dtdl:context;2",  "contents": [    {      "@type": "Property",      "name": "Temperature",      "schema": "double"    }  ]}' -n {digital_twins_instance_name}
```

[!INCLUDE [digital-twins-known-issue-cloud-shell](../../includes/digital-twins-known-issue-cloud-shell.md)]

Vervolgens moet u **een dubbele maken met behulp van dit model**. Gebruik de volgende opdracht om een dubbele en set 0,0 te maken als aanvankelijke temperatuur waarde.

```azurecli
az dt twin create --dtmi "dtmi:contosocom:DigitalTwins:Thermostat;1" --twin-id thermostat67 --properties '{"Temperature": 0.0,}' --dt-name {digital_twins_instance_name}
```

[!INCLUDE [digital-twins-known-issue-cloud-shell](../../includes/digital-twins-known-issue-cloud-shell.md)]

De uitvoer van een geslaagde, dubbele maken-opdracht moet er als volgt uitzien:
```json
{
  "$dtId": "thermostat67",
  "$etag": "W/\"0000000-9735-4f41-98d5-90d68e673e15\"",
  "$metadata": {
    "$model": "dtmi:contosocom:DigitalTwins:Thermostat;1",
    "Temperature": {
      "ackCode": 200,
      "ackDescription": "Auto-Sync",
      "ackVersion": 1,
      "desiredValue": 0.0,
      "desiredVersion": 1
    }
  },
  "Temperature": 0.0
}
```

## <a name="create-an-azure-function"></a>Een Azure-functie maken

In deze sectie wordt gebruikgemaakt van dezelfde Visual Studio-opstart stappen en Azure function-skelet van [*instructies: Stel een Azure-functie in voor het verwerken van gegevens*](how-to-create-azure-function.md). Het skelet behandelt verificatie en maakt een service-client, klaar om gegevens te verwerken en Azure Digital Apparaatdubbels-Api's te aanroepen als reactie. 

In de volgende stappen voegt u specifieke code toe voor het verwerken van IoT-telemetrie-gebeurtenissen van IoT Hub.  

### <a name="add-telemetry-processing"></a>Telemetrie-verwerking toevoegen
    
Telemetrie-gebeurtenissen worden in de vorm van berichten van het apparaat geleverd. De eerste stap bij het toevoegen van de telemetrie-verwerkings code wordt het relevante deel van dit apparaat geëxtraheerd uit de Event Grid gebeurtenis. 

Verschillende apparaten kunnen hun berichten anders structureren, zodat de code voor **deze stap afhankelijk is van het verbonden apparaat.** 

De volgende code toont een voor beeld van een eenvoudig apparaat dat telemetrie verzendt als JSON. Dit voor beeld wordt volledig verkend in [*zelf studie: verbinding maken met een end-to-end-oplossing*](./tutorial-end-to-end.md). Met de volgende code wordt de apparaat-ID gezocht van het apparaat dat het bericht heeft verzonden, evenals de waarde voor de Tempe ratuur.

```csharp
JObject deviceMessage = (JObject)JsonConvert.DeserializeObject(eventGridEvent.Data.ToString());
string deviceId = (string)deviceMessage["systemProperties"]["iothub-connection-device-id"];
var temperature = deviceMessage["body"]["Temperature"];
```

In het volgende code voorbeeld wordt de ID en de waarde voor de Tempe ratuur gebruikt en worden deze gebruikgemaakt van patches (waarbij updates worden gemaakt aan) die twee.

```csharp
//Update twin using device temperature
var uou = new UpdateOperationsUtility();
uou.AppendReplaceOp("/Temperature", temperature.Value<double>());
await client.UpdateDigitalTwinAsync(deviceId, uou.Serialize());
...
```

### <a name="update-your-azure-function-code"></a>Uw Azure-functie code bijwerken

Nu u de code van de eerdere voor beelden begrijpt, opent u de Azure-functie vanuit de sectie [*vereisten*](https://docs.microsoft.com/azure/digital-twins/how-to-ingest-iot-hub-data#prerequisites) in Visual Studio. (Als u geen Azure-functie hebt, gaat u naar de koppeling in de vereisten om er nu een te maken).

Vervang de code van uw Azure-functie door deze voorbeeld code.

```csharp
using System;
using System.Net.Http;
using Azure.Core.Pipeline;
using Azure.DigitalTwins.Core;
using Azure.DigitalTwins.Core.Serialization;
using Azure.Identity;
using Microsoft.Azure.EventGrid.Models;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.EventGrid;
using Microsoft.Extensions.Logging;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;

namespace IotHubtoTwins
{
    public class IoTHubtoTwins
    {
        private static readonly string adtInstanceUrl = Environment.GetEnvironmentVariable("ADT_SERVICE_URL");
        private static readonly HttpClient httpClient = new HttpClient();

        [FunctionName("IoTHubtoTwins")]
        public async void Run([EventGridTrigger] EventGridEvent eventGridEvent, ILogger log)
        {
            if (adtInstanceUrl == null) log.LogError("Application setting \"ADT_SERVICE_URL\" not set");

            try
            {
                //Authenticate with Digital Twins
                ManagedIdentityCredential cred = new ManagedIdentityCredential("https://digitaltwins.azure.net");
                DigitalTwinsClient client = new DigitalTwinsClient(
                    new Uri(adtInstanceUrl), cred, new DigitalTwinsClientOptions 
                    { Transport = new HttpClientTransport(httpClient) });
                log.LogInformation($"ADT service client connection created.");
            
                if (eventGridEvent != null && eventGridEvent.Data != null)
                {
                    log.LogInformation(eventGridEvent.Data.ToString());

                    // Reading deviceId and temperature for IoT Hub JSON
                    JObject deviceMessage = (JObject)JsonConvert.DeserializeObject(eventGridEvent.Data.ToString());
                    string deviceId = (string)deviceMessage["systemProperties"]["iothub-connection-device-id"];
                    var temperature = deviceMessage["body"]["Temperature"];
                    
                    log.LogInformation($"Device:{deviceId} Temperature is:{temperature}");

                    //Update twin using device temperature
                    var uou = new UpdateOperationsUtility();
                    uou.AppendReplaceOp("/Temperature", temperature.Value<double>());
                    await client.UpdateDigitalTwinAsync(deviceId, uou.Serialize());
                }
            }
            catch (Exception e)
            {
                log.LogError($"Error in ingest function: {e.Message}");
            }
        }
    }
}
```
Sla uw functie code op en publiceer de functie-app in Azure. U kunt dit doen door te verwijzen naar [*het gedeelte functie-app publiceren*](https://docs.microsoft.com/azure/digital-twins/how-to-create-azure-function#publish-the-function-app-to-azure) van [*instructies: een Azure-functie instellen voor het verwerken van gegevens*](how-to-create-azure-function.md).

Nadat de publicatie is voltooid, ziet u de uitvoer in het opdracht venster van Visual Studio, zoals hieronder wordt weer gegeven:

```cmd
1>------ Build started: Project: adtIngestFunctionSample, Configuration: Release Any CPU ------
1>adtIngestFunctionSample -> C:\Users\source\repos\Others\adtIngestFunctionSample\adtIngestFunctionSample\bin\Release\netcoreapp3.1\bin\adtIngestFunctionSample.dll
2>------ Publish started: Project: adtIngestFunctionSample, Configuration: Release Any CPU ------
2>adtIngestFunctionSample -> C:\Users\source\repos\Others\adtIngestFunctionSample\adtIngestFunctionSample\bin\Release\netcoreapp3.1\bin\adtIngestFunctionSample.dll
2>adtIngestFunctionSample -> C:\Users\source\repos\Others\adtIngestFunctionSample\adtIngestFunctionSample\obj\Release\netcoreapp3.1\PubTmp\Out\
2>Publishing C:\Users\source\repos\Others\adtIngestFunctionSample\adtIngestFunctionSample\obj\Release\netcoreapp3.1\PubTmp\adtIngestFunctionSample - 20200911112545669.zip to https://adtingestfunctionsample20200818134346.scm.azurewebsites.net/api/zipdeploy...
========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
========== Publish: 1 succeeded, 0 failed, 0 skipped ==========
```
U kunt ook de status van het publicatie proces controleren in het [Azure Portal](https://portal.azure.com/). Zoek naar de _resource groep_ en ga naar _activiteiten logboek_ en zoek naar het _publicatie profiel_ voor het publiceren van de web-app in de lijst en controleer of de status is geslaagd.

:::image type="content" source="media/how-to-ingest-iot-hub-data/azure-function-publish-activity-log.png" alt-text="Scherm afbeelding van de Azure Portal waarin de status van het publicatie proces wordt weer gegeven.":::

## <a name="connect-your-function-to-iot-hub"></a>Verbind uw functie met IoT Hub

Stel een gebeurtenis bestemming in voor hub-gegevens.
Navigeer in het [Azure Portal](https://portal.azure.com/)naar uw IOT hub-exemplaar dat u hebt gemaakt in de sectie [*vereisten*](https://docs.microsoft.com/azure/digital-twins/how-to-ingest-iot-hub-data#prerequisites) . Maak onder **gebeurtenissen**een abonnement voor uw Azure-functie.

:::image type="content" source="media/how-to-ingest-iot-hub-data/add-event-subscription.png" alt-text="Scherm afbeelding van de Azure Portal die het toevoegen van een gebeurtenis abonnement weergeeft.":::

Vul op de pagina **gebeurtenis abonnement maken** de velden als volgt in:
  1. Geef bij **naam**het abonnement een naam zoals u wilt.
  2. Kies _Event grid schema_onder **gebeurtenis schema**.
  3. Kies onder **gebeurtenis typen**het selectie vakje _telemetrie van apparaat_ en Schakel andere gebeurtenis typen uit.
  4. Selecteer onder **type eind punt**de optie _Azure function_.
  5. Kies onder **eind punt** _de optie Selecteer een eindpunt_ koppeling om een eind punt te maken.
    
:::image type="content" source="media/how-to-ingest-iot-hub-data/create-event-subscription.png" alt-text="Scherm afbeelding van de Azure Portal voor het maken van de details van het gebeurtenis abonnement":::

Controleer de onderstaande gegevens op de pagina _Azure-functie selecteren_ die wordt geopend.
 1. **Abonnement**: Uw Azure-abonnement
 2. **Resource groep**: de resource groep
 3. **Functie-app**: naam van uw functie-app
 4. **Sleuf**: _productie_
 5. **Functie**: Selecteer uw Azure-functie in de vervolg keuzelijst.

Sla uw gegevens op door de knop _selectie bevestigen_ te selecteren.            
      
:::image type="content" source="media/how-to-ingest-iot-hub-data/select-azure-function.png" alt-text="Scherm afbeelding van de Azure Portal Azure-functie selecteren":::

Selecteer de knop _maken_ om een gebeurtenis abonnement te maken.

## <a name="send-simulated-iot-data"></a>Gesimuleerde IoT-gegevens verzenden

Als u de nieuwe functie insluitingen wilt testen, gebruikt u de hand leiding van Device Simulator [*: verbinding maken met een end-to-end-oplossing*](./tutorial-end-to-end.md). Deze zelf studie wordt aangedreven door een voorbeeld project dat is geschreven in C#. De voorbeeld code bevindt zich hier: [Azure Digital apparaatdubbels](https://docs.microsoft.com/samples/azure-samples/digital-twins-samples/digital-twins-samples)-voor beelden. U gebruikt het **DeviceSimulator** -project in die opslag plaats.

In de end-to-end zelf studie voert u de volgende stappen uit:
1. [*Het gesimuleerde apparaat bij IoT Hub registreren*](./tutorial-end-to-end.md#register-the-simulated-device-with-iot-hub)
2. [*De simulatie configureren en uitvoeren*](./tutorial-end-to-end.md#configure-and-run-the-simulation)

## <a name="validate-your-results"></a>Uw resultaten valideren

Bij het uitvoeren van de bovenstaande Device Simulator wordt de waarde voor de Tempe ratuur van uw digitale-dubbele waarden gewijzigd. Voer in de Azure CLI de volgende opdracht uit om de waarde voor de Tempe ratuur te bekijken.

[!INCLUDE [digital-twins-known-issue-cloud-shell](../../includes/digital-twins-known-issue-cloud-shell.md)]

```azurecli
az dt twin query -q "select * from digitaltwins" -n {digital_twins_instance_name}
```

De uitvoer moet een temperatuur waarde als volgt bevatten:

```json
{
  "result": [
    {
      "$dtId": "thermostat67",
      "$etag": "W/\"0000000-1e83-4f7f-b448-524371f64691\"",
      "$metadata": {
        "$model": "dtmi:contosocom:DigitalTwins:Thermostat;1",
        "Temperature": {
          "ackCode": 200,
          "ackDescription": "Auto-Sync",
          "ackVersion": 1,
          "desiredValue": 69.75806974934324,
          "desiredVersion": 1
        }
      },
      "Temperature": 69.75806974934324
    }
  ]
}
```

Voer herhaaldelijk de query-opdracht uit om de gewijzigde waarde weer te geven.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het inkomen en uitkomen van gegevens met Azure Digital Apparaatdubbels:
* [*Concepten: integratie met andere services*](concepts-integration.md)