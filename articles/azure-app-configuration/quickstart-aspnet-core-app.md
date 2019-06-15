---
title: Quickstart voor Azure-app-configuratie met ASP.NET Core | Microsoft Docs
description: Een quickstart voor het gebruik van Azure-app-configuratie met ASP.NET Core-apps
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.devlang: csharp
ms.topic: quickstart
ms.tgt_pltfrm: ASP.NET Core
ms.workload: tbd
ms.date: 02/24/2019
ms.author: yegu
ms.openlocfilehash: 19e0814f6e4da364a70c41e1d99fb6f1897bf0c9
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67052077"
---
# <a name="quickstart-create-an-aspnet-core-app-with-azure-app-configuration"></a>Quickstart: Een ASP.NET Core-app maken met Azure-app-configuratie

Azure-appconfiguratie is een beheerde configuratieservice in Azure. U kunt deze eenvoudig opslaan en beheren van alle instellingen van de toepassing op één plek dat gescheiden van uw code. Deze quickstart laat u zien hoe u de service kunt opnemen in een ASP.NET Core-web-app. 

Een object één sleutel-waarde-configuratie op basis van bouwt ASP.NET Core met behulp van de instellingen van een of meer gegevensbronnen die zijn opgegeven door een toepassing. Deze gegevensbronnen worden aangeduid als *configuratieproviders*. Omdat App-configuratie .NET Core-client is geïmplementeerd als zodanig een provider, is de service wordt weergegeven, zoals een andere gegevensbron.

Een code-editor kunt u de stappen in deze Quick Start. [Visual Studio Code](https://code.visualstudio.com/) is een uitstekende optie beschikbaar is op Windows, macOS en Linux-platforms.

![Quickstart voor het lokaal starten van een app](./media/quickstarts/aspnet-core-app-launch-local.png)

## <a name="prerequisites"></a>Vereisten

Als u wilt doen in deze Quick Start, installeert de [.NET Core SDK](https://dotnet.microsoft.com/download).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-an-app-configuration-store"></a>Een app-configuratiearchief maken

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-create.md)]

6. Selecteer **configuratie Explorer** >  **+ maken** om toe te voegen van de volgende sleutel-waardeparen:

    | Sleutel | Value |
    |---|---|
    | TestApp:Settings:BackgroundColor | Wit |
    | TestApp:Settings:FontSize | 24 |
    | TestApp:Settings:FontColor | Zwart |
    | TestApp:Settings:Message | Gegevens van Azure App Configuration |

    Laat **Label** en **inhoudstype** voorlopig leeg.

## <a name="create-an-aspnet-core-web-app"></a>Een ASP.NET Core-web-app maken

U gebruikt de [.NET Core-opdrachtregelinterface (CLI)](https://docs.microsoft.com/dotnet/core/tools/) te maken van een nieuwe ASP.NET Core MVC-app-webproject. Het voordeel van het gebruik van de .NET Core-CLI via Visual Studio is dat het is beschikbaar voor de Windows, macOS en Linux-platforms.

1. Maak een nieuwe map voor uw project. Voor deze Quick Start, geef deze de naam *TestAppConfig*.

2. Voer de volgende opdracht om een nieuwe ASP.NET Core MVC-app-webproject maken in de nieuwe map:

        dotnet new mvc

## <a name="add-secret-manager"></a>Secret Manager toevoegen

Voeg de [Secret Manager hulpprogramma](https://docs.microsoft.com/aspnet/core/security/app-secrets) aan uw project. Dit hulpprogramma slaat gevoelige gegevens voor ontwikkeltaken op buiten de projectstructuur. Deze aanpak voorkomt dat er per ongeluk appgeheimen worden gedeeld in de broncode.

- Open de *.csproj* bestand. Voeg een `UserSecretsId` element zoals hieronder, en vervang de waarde door uw eigen, dit meestal een GUID is. Sla het bestand op.

    ```xml
    <Project Sdk="Microsoft.NET.Sdk.Web">

    <PropertyGroup>
        <TargetFramework>netcoreapp2.1</TargetFramework>
        <UserSecretsId>79a3edd0-2092-40a2-a04d-dcb46d5ca9ed</UserSecretsId>
    </PropertyGroup>

    <ItemGroup>
        <PackageReference Include="Microsoft.AspNetCore.App" />
        <PackageReference Include="Microsoft.AspNetCore.Razor.Design" Version="2.1.2" PrivateAssets="All" />
    </ItemGroup>

    </Project>
    ```

## <a name="connect-to-an-app-configuration-store"></a>Verbinding maken met een app-configuratiearchief

1. Voeg een verwijzing naar de `Microsoft.Extensions.Configuration.AzureAppConfiguration` NuGet-pakket met de volgende opdracht:

        dotnet add package Microsoft.Extensions.Configuration.AzureAppConfiguration --version 1.0.0-preview-008520001

2. Voer de volgende opdracht om te herstellen van pakketten voor uw project:

        dotnet restore

3. Voeg een geheim met de naam *ConnectionStrings:AppConfig* toe aan Secret Manager.

    Dit geheim bevat de verbindingsreeks voor toegang tot de store van uw app-configuratie. De waarde in de volgende opdracht vervangt door de verbindingsreeks voor het opslaan van de app-configuratie.

    Deze opdracht moet worden uitgevoerd in de map met het bestand *.csproj*.

        dotnet user-secrets set ConnectionStrings:AppConfig <your_connection_string>

    Secret Manager wordt alleen gebruikt om de web-app lokaal testen. Wanneer de app wordt geïmplementeerd op [Azure App Service](https://azure.microsoft.com/services/app-service/web), bijvoorbeeld, u een toepassingsinstelling **verbindingsreeksen** in App Service in plaats van met Secret Manager voor het opslaan van de verbindingsreeks.

    Dit geheim wordt geopend met de configuratie-API. Een dubbele punt (:) werkt in de configuratienaam van de met de configuratie-API op alle ondersteunde platforms. Zie [configuratie omgeving](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/index?tabs=basicconfiguration&view=aspnetcore-2.0).

4. Open *Program.cs*, en voeg een verwijzing naar de configuratie van .NET Core-App-provider.

    ```csharp
    using Microsoft.Extensions.Configuration.AzureAppConfiguration;
    ```

5. Update de `CreateWebHostBuilder` methode voor het gebruik van App-configuratie door het aanroepen van de `config.AddAzureAppConfiguration()` methode.

    ```csharp
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                var settings = config.Build();
                config.AddAzureAppConfiguration(settings["ConnectionStrings:AppConfig"]);
            })
            .UseStartup<Startup>();
    ```

6. Open *Index.cshtml* in de weergaven > Home-directory en vervang de inhoud ervan door de volgende code:

    ```html
    @using Microsoft.Extensions.Configuration
    @inject IConfiguration Configuration

    <style>
        body {
            background-color: @Configuration["TestApp:Settings:BackgroundColor"]
        }
        h1 {
            color: @Configuration["TestApp:Settings:FontColor"];
            font-size: @Configuration["TestApp:Settings:FontSize"];
        }
    </style>

    <h1>@Configuration["TestApp:Settings:Message"]</h1>
    ```

7. Open *_Layout.cshtml* in de weergaven > gedeelde map en vervang de inhoud ervan door de volgende code:

    ```html
    <!DOCTYPE html>
    <html>
    <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>@ViewData["Title"] - hello_world</title>

        <link rel="stylesheet" href="~/lib/bootstrap/dist/css/bootstrap.css" />
        <link rel="stylesheet" href="~/css/site.css" />
    </head>
    <body>
        <div class="container body-content">
            @RenderBody()
        </div>

        <script src="~/lib/jquery/dist/jquery.js"></script>
        <script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
        <script src="~/js/site.js" asp-append-version="true"></script>

        @RenderSection("Scripts", required: false)
    </body>
    </html>
    ```

## <a name="build-and-run-the-app-locally"></a>De app lokaal compileren en uitvoeren

1. Voor het bouwen van de app met behulp van .NET Core CLI, voer de volgende opdracht in de opdrachtshell:

        dotnet build

2. Nadat de build is voltooid, voert u de volgende opdracht om de web-app lokaal uitvoeren:

        dotnet run

3. Open een browservenster en Ga naar `http://localhost:5000`, dit is de standaard-URL voor de web-app die lokaal wordt gehost.

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Volgende stappen

In deze Quick Start, u een nieuwe app-configuratie-archief hebt gemaakt en deze gebruikt met een ASP.NET Core web-app via de [App configuratieprovider](https://go.microsoft.com/fwlink/?linkid=2074664). Doorgaan naar de volgende zelfstudie waarin wordt gedemonstreerd verificatie voor meer informatie over het gebruik van App-configuratie.

> [!div class="nextstepaction"]
> [Integratie van beheerde identiteit](./howto-integrate-azure-managed-service-identity.md)
