---
title: Quickstart voor Azure App Configuration met .NET Core | Microsoft Docs
description: Een quickstart voor het gebruik van Azure App Configuration met .NET Core-apps
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.devlang: csharp
ms.topic: quickstart
ms.tgt_pltfrm: .NET Core
ms.workload: tbd
ms.date: 02/24/2019
ms.author: yegu
ms.openlocfilehash: 0bf4aff8e0bae3e84e6383ec560dbfe67e30b994
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65408727"
---
# <a name="quickstart-create-a-net-core-app-with-app-configuration"></a>Quickstart: Maken van een .NET Core-app met App-configuratie

Azure-appconfiguratie is een beheerde configuratieservice in Azure. U kunt deze eenvoudig opslaan en beheren van alle instellingen van de toepassing op één plek dat gescheiden van uw code. Deze quickstart laat u zien hoe u de service kunt opnemen in een .NET Core-console-app.

Een code-editor kunt u de stappen in deze Quick Start. [Visual Studio Code](https://code.visualstudio.com/) is een uitstekende optie beschikbaar is op Windows, macOS en Linux-platforms.

![Quickstart app uitvoeren](./media/quickstarts/dotnet-core-app-run.png)

## <a name="prerequisites"></a>Vereisten

Als u wilt doen in deze Quick Start, installeert de [.NET Core SDK](https://dotnet.microsoft.com/download).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-an-app-configuration-store"></a>Een app-configuratiearchief maken

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-create.md)]

6. Selecteer **configuratie Explorer** > **+ maken** om toe te voegen van de volgende sleutel-waardeparen:

    | Sleutel | Value |
    |---|---|
    | TestApp:Settings:Message | Gegevens van Azure App Configuration |

    Laat **Label** en **inhoudstype** voorlopig leeg.

## <a name="create-a-net-core-console-app"></a>Een .NET Core-console-app maken

U gebruikt de [.NET Core-opdrachtregelinterface (CLI)](https://docs.microsoft.com/dotnet/core/tools/) te maken van een nieuwe .NET Core console app-project. Het voordeel van het gebruik van de .NET Core-CLI via Visual Studio is dat het is beschikbaar voor de Windows, macOS en Linux-platforms.

1. Maak een nieuwe map voor uw project.

2. Voer de volgende opdracht om een nieuwe ASP.NET Core MVC-app-webproject maken in de nieuwe map:

        dotnet new console

## <a name="connect-to-an-app-configuration-store"></a>Verbinding maken met een app-configuratiearchief

1. Voeg een verwijzing naar de `Microsoft.Extensions.Configuration.AzureAppConfiguration` NuGet-pakket met de volgende opdracht:

        dotnet add package Microsoft.Extensions.Configuration.AzureAppConfiguration --version 1.0.0-preview-008520001

2. Voer de volgende opdracht om te herstellen van pakketten voor uw project:

        dotnet restore

3. Open *Program.cs*, en voeg een verwijzing naar de configuratie van .NET Core-App-provider.

    ```csharp
    using Microsoft.Extensions.Configuration;
    using Microsoft.Extensions.Configuration.AzureAppConfiguration;
    ```

4. Update de `Main` methode voor het gebruik van App-configuratie door het aanroepen van de `builder.AddAzureAppConfiguration()` methode.

    ```csharp
    static void Main(string[] args)
    {
        var builder = new ConfigurationBuilder();
        builder.AddAzureAppConfiguration(Environment.GetEnvironmentVariable("ConnectionString"));

        var config = builder.Build();
        Console.WriteLine(config["TestApp:Settings:Message"] ?? "Hello world!");
    }
    ```

## <a name="build-and-run-the-app-locally"></a>De app lokaal compileren en uitvoeren

1. Stel een omgevingsvariabele met de naam **ConnectionString**, en stel deze in op de toegangssleutel voor het opslaan van de app-configuratie. Als u de Windows-opdrachtprompt, voer de volgende opdracht uit en start opnieuw op de opdrachtprompt om toe te staan van de wijziging door te voeren:

        setx ConnectionString "connection-string-of-your-app-configuration-store"

    Als u Windows PowerShell gebruikt, voert u de volgende opdracht uit:

        $Env:ConnectionString = "connection-string-of-your-app-configuration-store"

    Als u Mac OS of Linux gebruikt, voert u de volgende opdracht uit:

        export ConnectionString='connection-string-of-your-app-configuration-store'

2. Voer de volgende opdracht om de console-app te bouwen:

        dotnet build

3. Nadat de build is voltooid, voert u de volgende opdracht om de app lokaal uitvoeren:

        dotnet run

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids u een nieuwe app-configuratie-archief hebt gemaakt en deze gebruikt met een .NET Core-console-app via de [App configuratieprovider](https://go.microsoft.com/fwlink/?linkid=2074664). Doorgaan naar de volgende zelfstudie waarin wordt gedemonstreerd verificatie voor meer informatie over het gebruik van App-configuratie.

> [!div class="nextstepaction"]
> [Integratie van beheerde identiteit](./howto-integrate-azure-managed-service-identity.md)
