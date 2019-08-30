---
title: Aan de slag met Azure IoT Hub module identity and module twin (.NET) | Microsoft Docs
description: Informatie over het maken van module-id's en het bijwerken van moduledubbels met IoT SDK's voor .NET.
author: chrissie926
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: conceptual
ms.date: 08/07/2019
ms.author: menchi
ms.openlocfilehash: abf8b18d13e2990f053dd4273645dd7b9a324c77
ms.sourcegitcommit: aaa82f3797d548c324f375b5aad5d54cb03c7288
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2019
ms.locfileid: "70147745"
---
# <a name="get-started-with-iot-hub-module-identity-and-module-twin-net"></a>Aan de slag met IoT Hub module-identiteit en-module twee (.NET)

[!INCLUDE [iot-hub-selector-module-twin-getstarted](../../includes/iot-hub-selector-module-twin-getstarted.md)]

> [!NOTE]
> [Module-id's en moduledubbels](iot-hub-devguide-module-twins.md) zijn vergelijkbaar met Azure IoT Hub-apparaat-id's en apparaatdubbels, maar bieden een hogere granulariteit. Hoewel Azure IoT Hub apparaat-id en-apparaat twee keer de back-end-toepassing in staat stellen om een apparaat te configureren en inzicht te krijgen in de voor waarden van het apparaat, bieden een module-identiteit en-module twee deze mogelijkheden voor afzonderlijke onderdelen van een apparaat. Op apparaten die geschikt zijn voor meerdere onderdelen, zoals apparaten op het besturings systeem of firmware apparaten, kunnen module-identiteiten en module-apparaatdubbels geïsoleerde configuratie en voor waarden voor elk onderdeel toestaan.

Aan het eind van deze zelfstudie beschikt u over twee .NET-consoletoepassingen:

* **CreateIdentities**. Deze app maakt een apparaat-id, een module-ID en de bijbehorende beveiligings sleutel om uw apparaat en module-clients te verbinden.

* **UpdateModuleTwinReportedProperties**. Deze app verzendt bijgewerkte module dubbele gerapporteerde eigenschappen naar uw IoT-hub.

> [!NOTE]
> Voor informatie over de Azure IoT-Sdk's die u kunt gebruiken om beide toepassingen te bouwen die op apparaten kunnen worden uitgevoerd en de back-end van uw oplossing, raadpleegt u [Azure IOT sdk's](iot-hub-devguide-sdks.md).

## <a name="prerequisites"></a>Vereisten

* Visual Studio.

* Een actief Azure-account. Als u geen account hebt, kunt u in slechts een paar minuten een [gratis account](https://azure.microsoft.com/pricing/free-trial/) maken.

## <a name="create-a-hub"></a>Een hub maken

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="get-the-iot-hub-connection-string"></a>De IoT hub-connection string ophalen

[!INCLUDE [iot-hub-howto-module-twin-shared-access-policy-text](../../includes/iot-hub-howto-module-twin-shared-access-policy-text.md)]

[!INCLUDE [iot-hub-include-find-registryrw-connection-string](../../includes/iot-hub-include-find-registryrw-connection-string.md)]

[!INCLUDE [iot-hub-get-started-create-module-identity-csharp](../../includes/iot-hub-get-started-create-module-identity-csharp.md)]

## <a name="update-the-module-twin-using-net-device-sdk"></a>De moduledubbel bijwerken met de SDK voor .NET-apparaten

In deze sectie maakt u een .NET-consoletoepassing op het gesimuleerde apparaat waarmee de gerapporteerde eigenschappen van de moduledubbel worden bijgewerkt.

Voordat u begint, moet u uw module connection string ophalen. Meld u aan bij [Azure Portal](https://portal.azure.com/). Navigeer naar uw hub en selecteer **IOT-apparaten**. **MyFirstDevice**zoeken. Selecteer **myFirstDevice** om deze te openen en selecteer vervolgens **myFirstModule** om het te openen. Kopieer in **module-identiteits Details**de **verbindings reeks (primaire sleutel)** wanneer dit nodig is in de volgende procedure.

   ![Details van de Azure Portal-module](./media/iot-hub-csharp-csharp-module-twin-getstarted/module-identity-detail.png)

1. Voeg in Visual Studio een nieuw project toe aan uw oplossing door **bestand** > **Nieuw** > **project**te selecteren. Selecteer in een nieuw project maken de optie **console-app (.NET Framework)** en selecteer **volgende**.

1. Noem het project *UpdateModuleTwinReportedProperties*. Selecteer voor **oplossing**de optie **toevoegen aan oplossing**. Zorg ervoor dat de versie van .NET Framework minimaal 4.6.1 is.

    ![Een Visual Studio-project maken](./media/iot-hub-csharp-csharp-module-twin-getstarted/configure-update-twins-csharp1.png)

1. Selecteer **maken** om het project te maken.

1. Open in Visual Studio **tools** > **NuGet package manager** > **NuGet-pakketten beheren voor oplossing**. Selecteer het tabblad **Browse**.

1. Zoek en selecteer **micro soft. Azure. devices. client**en selecteer vervolgens **installeren**.

    ![De huidige versie van de Azure IoT Hub .NET Service SDK installeren](./media/iot-hub-csharp-csharp-module-twin-getstarted/install-client-sdk.png)

1. Voeg aan het begin van het bestand **Program.cs** de volgende `using` instructies toe:

    ```csharp
    using Microsoft.Azure.Devices.Client;
    using Microsoft.Azure.Devices.Shared;
    using System.Threading.Tasks;
    using Newtonsoft.Json;
    ```

1. Voeg de volgende velden toe aan de klasse **Program**: Vervang de waarde van de tijdelijke aanduiding door de moduleverbindingsreeks.

    ```csharp
    private const string ModuleConnectionString = "<Your module connection string>";
    private static ModuleClient Client = null;
    static void ConnectionStatusChangeHandler(ConnectionStatus status, 
      ConnectionStatusChangeReason reason)
    {
        Console.WriteLine("Connection Status Changed to {0}; the reason is {1}", 
          status, reason);
    }
    ```

1. Voeg de methode **OnDesiredPropertyChanged** toe aan de klasse **Program**:

    ```csharp
    private static async Task OnDesiredPropertyChanged(TwinCollection desiredProperties, 
      object userContext)
        {
            Console.WriteLine("desired property change:");
            Console.WriteLine(JsonConvert.SerializeObject(desiredProperties));
            Console.WriteLine("Sending current time as reported property");
            TwinCollection reportedProperties = new TwinCollection
            {
                ["DateTimeLastDesiredPropertyChangeReceived"] = DateTime.Now
            };

            await Client.UpdateReportedPropertiesAsync(reportedProperties).ConfigureAwait(false);
        }
    ```

1. Voeg de volgende regels toe aan de methode **Main** :

    ```csharp
    static void Main(string[] args)
    {
        Microsoft.Azure.Devices.Client.TransportType transport = 
          Microsoft.Azure.Devices.Client.TransportType.Amqp;

        try
        {
            Client = 
              ModuleClient.CreateFromConnectionString(ModuleConnectionString, transport);
            Client.SetConnectionStatusChangesHandler(ConnectionStatusChangeHandler);
            Client.SetDesiredPropertyUpdateCallbackAsync(OnDesiredPropertyChanged, null).Wait();

            Console.WriteLine("Retrieving twin");
            var twinTask = Client.GetTwinAsync();
            twinTask.Wait();
            var twin = twinTask.Result;
            Console.WriteLine(JsonConvert.SerializeObject(twin.Properties)); 

            Console.WriteLine("Sending app start time as reported property");
            TwinCollection reportedProperties = new TwinCollection();
            reportedProperties["DateTimeLastAppLaunch"] = DateTime.Now;

            Client.UpdateReportedPropertiesAsync(reportedProperties);
        }
        catch (AggregateException ex)
        {
            Console.WriteLine("Error in sample: {0}", ex);
        }

        Console.WriteLine("Waiting for Events.  Press enter to exit...");
        Console.ReadLine();
        Client.CloseAsync().Wait();
    }
    ```

    In dit codevoorbeeld ziet u hoe u de gerapporteerde eigenschappen van de moduledubbel kunt ophalen en bijwerken met het AMQP-protocol. In de openbare preview-versie wordt alleen AMQP ondersteund voor moduledubbelbewerkingen.

1. Desgewenst kunt u deze instructies toevoegen aan de methode **Main** om een gebeurtenis te verzenden naar IOT hub vanuit uw module. Plaats deze regels onder het `try catch` blok.

    ```csharp
    Byte[] bytes = new Byte[2];
    bytes[0] = 0;
    bytes[1] = 1;
    var sendEventsTask = Client.SendEventAsync(new Message(bytes));
    sendEventsTask.Wait();
    Console.WriteLine("Event sent to IoT Hub.");
    ```

## <a name="run-the-apps"></a>De apps uitvoeren

U kunt nu de apps uitvoeren.

1. Klik in Visual Studio in **Solution Explorer**met de rechter muisknop op uw oplossing en selecteer vervolgens **opstart projecten instellen**.

1. Onder **algemene eigenschappen**selecteert u **opstart project.**

1. Selecteer **meerdere opstart projecten**, en selecteer vervolgens **Start** als actie voor de apps en klik op **OK** om uw wijzigingen te accepteren.

1. Druk op **F5** om de apps te starten.

## <a name="next-steps"></a>Volgende stappen

Als u aan de slag wilt gaan met IoT Hub en andere IoT-scenario's wilt verkennen, leest u deze artikelen:

* [Aan de slag met Apparaatbeheer](iot-hub-node-node-device-management-get-started.md)

* [Aan de slag met IoT Edge](../iot-edge/tutorial-simulate-device-linux.md)
