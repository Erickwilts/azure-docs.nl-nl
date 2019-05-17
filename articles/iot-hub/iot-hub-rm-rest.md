---
title: Een Azure-IoT-hub met behulp van de resourceprovider REST-API maken | Microsoft Docs
description: Het gebruik van de resourceprovider REST-API te maken van een IoT-Hub.
author: robinsh
manager: philmea
ms.author: robinsh
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: conceptual
ms.date: 08/08/2017
ms.openlocfilehash: 6d91f5e61dfd7c3cb4d1869edf0c6cb8c2c85190
ms.sourcegitcommit: be9fcaace62709cea55beb49a5bebf4f9701f7c6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2019
ms.locfileid: "65827506"
---
# <a name="create-an-iot-hub-using-the-resource-provider-rest-api-net"></a>Maken van een IoT-hub met behulp van de resourceprovider REST-API (.NET)

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

U kunt de [REST API voor IoT Hub-resourceprovider](https://docs.microsoft.com/rest/api/iothub/iothubresource) maken en beheren van Azure IoT-hubs via een programma. Deze zelfstudie leert u hoe u met de IoT Hub resourceprovider REST-API te maken van een IoT-hub uit een C#-programma.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Visual Studio.

* Een actief Azure-account. Als u geen account hebt, kunt u een [gratis account](https://azure.microsoft.com/pricing/free-trial/) binnen een paar minuten.

* [Azure PowerShell 1.0](https://docs.microsoft.com/powershell/azure/install-Az-ps) of hoger.

[!INCLUDE [iot-hub-prepare-resource-manager](../../includes/iot-hub-prepare-resource-manager.md)]

## <a name="prepare-your-visual-studio-project"></a>Visual Studio-project voorbereiden

1. Maak in Visual Studio een Visual C# Windows Classic Desktop-project met de **Console-App (.NET Framework)** projectsjabloon. Noem het project **CreateIoTHubREST**.

2. Klik in Solution Explorer met de rechtermuisknop op uw project en klik vervolgens op **NuGet-pakketten beheren**.

3. Controleer in NuGet Package Manager **Include prerelease**, en klik op de **Bladeren** zoekopdracht met resultatenpagina voor **Microsoft.Azure.Management.ResourceManager**. Selecteer het pakket, klikt u op **installeren**in **wijzigingen controleren** klikt u op **OK**, klikt u vervolgens op **ik ga akkoord** te accepteren van de licenties.

4. Zoeken in NuGet Package Manager **Microsoft.IdentityModel.Clients.ActiveDirectory**.  Klik op **installeren**in **wijzigingen controleren** klikt u op **OK**, klikt u vervolgens op **ik ga akkoord** om de licentie te accepteren.

5. Vervang in Program.cs de bestaande **met behulp van** instructies met de volgende code:

    ```csharp
    using System;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using System.Text;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Newtonsoft.Json;
    using Microsoft.Rest;
    using System.Linq;
    using System.Threading;
    ```

6. Toevoegen in Program.cs de volgende statische variabelen, vervang de tijdelijke aanduiding voor waarden. U hebt genoteerd een **ApplicationId**, **SubscriptionId**, **TenantId**, en **wachtwoord** eerder in deze zelfstudie. **De naam van resourcegroep** is de naam van de resourcegroep die u gebruikt bij het maken van de IoT-hub. U kunt een reeds bestaande of een nieuwe resourcegroep gebruiken. **De naam van IoT Hub** is de naam van de IoT-Hub die u, zoals maakt **MyIoTHub**. De naam van uw IoT-hub moet globaal uniek zijn. **Naam van de implementatie** is een naam voor de implementatie, zoals **Deployment_01**.

    ```csharp
    static string applicationId = "{Your ApplicationId}";
    static string subscriptionId = "{Your SubscriptionId}";
    static string tenantId = "{Your TenantId}";
    static string password = "{Your application Password}";

    static string rgName = "{Resource group name}";
    static string iotHubName = "{IoT Hub name including your initials}";
    ```
   
    [!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

[!INCLUDE [iot-hub-get-access-token](../../includes/iot-hub-get-access-token.md)]

## <a name="use-the-resource-provider-rest-api-to-create-an-iot-hub"></a>Gebruik de resourceprovider REST API voor het maken van een IoT-hub

Gebruik de [REST API voor IoT Hub-resourceprovider](https://docs.microsoft.com/rest/api/iothub/iothubresource) te maken van een IoT-hub in de resourcegroep. U kunt ook de resourceprovider REST-API gebruiken om een bestaande IoT-hub te wijzigen.

1. Voeg de volgende methode toe aan Program.cs:

    ```csharp
    static void CreateIoTHub(string token)
    {

    }
    ```

2. Voeg de volgende code aan de **CreateIoTHub** methode. Deze code maakt een **HttpClient** object met het verificatietoken in de headers:

    ```csharp
    HttpClient client = new HttpClient();
    client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", token);
    ```

3. Voeg de volgende code aan de **CreateIoTHub** methode. Deze code beschrijft de IoT-hub maken en genereert een JSON-weergave. Zie voor de huidige lijst met locaties die ondersteuning bieden voor IoT Hub [Azure Status](https://azure.microsoft.com/status/):

    ```csharp
    var description = new
    {
      name = iotHubName,
      location = "East US",
      sku = new
      {
        name = "S1",
        tier = "Standard",
        capacity = 1
      }
    };

    var json = JsonConvert.SerializeObject(description, Formatting.Indented);
    ```

4. Voeg de volgende code aan de **CreateIoTHub** methode. Deze code verzendt de REST-aanvraag naar Azure. De code vervolgens controleert de reactie en wordt de URL die u gebruiken kunt voor het bewaken van de status van de Implementatietaak opgehaald:

    ```csharp
    var content = new StringContent(JsonConvert.SerializeObject(description), Encoding.UTF8, "application/json");
    var requestUri = string.Format("https://management.azure.com/subscriptions/{0}/resourcegroups/{1}/providers/Microsoft.devices/IotHubs/{2}?api-version=2016-02-03", subscriptionId, rgName, iotHubName);
    var result = client.PutAsync(requestUri, content).Result;

    if (!result.IsSuccessStatusCode)
    {
      Console.WriteLine("Failed {0}", result.Content.ReadAsStringAsync().Result);
      return;
    }

    var asyncStatusUri = result.Headers.GetValues("Azure-AsyncOperation").First();
    ```

5. Voeg de volgende code toe aan het einde van de **CreateIoTHub** methode. Deze code gebruikt de **asyncStatusUri** adres opgehaald in de vorige stap na afloop van de installatie is voltooid:

    ```csharp
    string body;
    do
    {
      Thread.Sleep(10000);
      HttpResponseMessage deploymentstatus = client.GetAsync(asyncStatusUri).Result;
      body = deploymentstatus.Content.ReadAsStringAsync().Result;
    } while (body == "{\"status\":\"Running\"}");
    ```

6. Voeg de volgende code toe aan het einde van de **CreateIoTHub** methode. Deze code worden opgehaald van de sleutels van de IoT hub die u hebt gemaakt en worden ze naar de console afgedrukt:

    ```csharp
    var listKeysUri = string.Format("https://management.azure.com/subscriptions/{0}/resourceGroups/{1}/providers/Microsoft.Devices/IotHubs/{2}/IoTHubKeys/listkeys?api-version=2016-02-03", subscriptionId, rgName, iotHubName);
    var keysresults = client.PostAsync(listKeysUri, null).Result;

    Console.WriteLine("Keys: {0}", keysresults.Content.ReadAsStringAsync().Result);
    ```

## <a name="complete-and-run-the-application"></a>Voltooien en de toepassing wordt uitgevoerd

U kunt de toepassing nu uitvoeren door het aanroepen van de **CreateIoTHub** methode voordat u bouwen en uitvoeren.

1. Voeg de volgende code toe aan het einde van de **Main** methode:

    ```csharp
    CreateIoTHub(token.AccessToken);
    Console.ReadLine();
    ```

2. Klik op **bouwen** en vervolgens **-oplossing bouwen**. Eventuele fouten te corrigeren.

3. Klik op **Debug** en vervolgens **Start Debugging** de toepassing uit te voeren. Het duurt enkele minuten voor de implementatie om uit te voeren.

4. Als u wilt controleren of uw toepassing de nieuwe IoT hub toegevoegd, gaat u naar de [Azure-portal](https://portal.azure.com/) en uw lijst met resources weer te geven. U kunt ook de **Get-AzResource** PowerShell-cmdlet.

> [!NOTE]
> In dit voorbeeld van de toepassing wordt toegevoegd een Standard IoT-Hub van S1, die in rekening worden gebracht. Wanneer u klaar bent, kunt u de IoT-hub via verwijderen de [Azure-portal](https://portal.azure.com/) of met behulp van de **Remove-AzResource** PowerShell-cmdlet als u klaar bent.

## <a name="next-steps"></a>Volgende stappen

Nu u een IoT-hub met behulp van de resourceprovider REST-API hebt geïmplementeerd, kunt u verder te verkennen:

* Meer informatie over de mogelijkheden van de [REST API voor IoT Hub-resourceprovider](https://docs.microsoft.com/rest/api/iothub/iothubresource).

* Lezen [overzicht van Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) voor meer informatie over de mogelijkheden van Azure Resource Manager.

Zie de volgende artikelen voor meer informatie over het ontwikkelen van IoT-Hub:

* [Inleiding tot C SDK](iot-hub-device-sdk-c-intro.md)

* [SDK's voor Azure IoT](iot-hub-devguide-sdks.md)

Als u wilt de mogelijkheden van IoT Hub verder verkennen, Zie:

* [AI implementeren op edge-apparaten met Azure IoT Edge](../iot-edge/tutorial-simulate-device-linux.md)