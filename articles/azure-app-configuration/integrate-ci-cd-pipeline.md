---
title: Zelfstudie voor het integreren met een continue integratie en levering-pijplijn met behulp van Azure App-configuratie | Microsoft Docs
description: In deze zelfstudie leert u hoe u voor het genereren van een configuratiebestand met behulp van gegevens in Azure App-configuratie tijdens de continue integratie en levering
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.topic: tutorial
ms.date: 02/24/2019
ms.author: yegu
ms.custom: mvc
ms.openlocfilehash: 26bd49af7245d6e6dde3162a2e1d95c54f13e35b
ms.sourcegitcommit: 300cd05584101affac1060c2863200f1ebda76b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65415951"
---
# <a name="integrate-with-a-cicd-pipeline"></a>Integreren met een CI/CD-pijplijn

Dit artikel beschrijft de verschillende manieren van het gebruik van gegevens van Azure App-configuratie in een continue integratie en continue implementatie.

## <a name="use-app-configuration-in-your-azure-devops-pipeline"></a>App-configuratie gebruiken in uw Azure DevOps-pijplijn

Als u een Azure DevOps-pijplijn hebt, kunt u sleutel-waarden ophalen uit de App-configuratie en stelt deze in als variabelen. De [Azure App-configuratie DevOps-extensie](https://go.microsoft.com/fwlink/?linkid=2091063) is een invoegtoepassing-module die deze functionaliteit biedt. Gewoon de instructies voor het gebruik van de extensie in een build of vrijgeven van de takenreeks.

## <a name="deploy-app-configuration-data-with-your-application"></a>Gegevens van App-configuratie met uw toepassing implementeren

Uw toepassing kan niet worden uitgevoerd als deze is afhankelijk van Azure App-configuratie en niet meer bereikbaar is. U kunt de tolerantie van uw toepassing te maken met een dergelijke gebeurtenis, maar onwaarschijnlijk is dat er zou gebeuren verbeteren. Om dit te doen, de huidige configuratiegegevens voor het pakket naar een bestand dat lokaal geïmplementeerd met de toepassing en geladen tijdens het opstarten. Deze aanpak zorgt ervoor dat uw toepassing standaardwaarden voor de instelling ten minste heeft. Deze waarden worden overschreven door nieuwe wijzigingen in een configuratie van apps wanneer deze beschikbaar is.

Met behulp van de [exporteren](./howto-import-export-data.md#export-data) functie van Azure App-configuratie, kunt u het proces van het ophalen van de huidige van configuratiegegevens als een enkel bestand automatiseren. Dit bestand vervolgens insluiten in een stap build of implementatie in uw continue integratie en continue implementatie (CI/CD)-pijplijn.

Het volgende voorbeeld ziet u hoe u met de configuratie van de App gegevens als een build-stap voor de web-app die is geïntroduceerd in de QuickStart. Voordat u doorgaat, voltooien [maken van een ASP.NET Core-app met App-configuratie](./quickstart-aspnet-core-app.md) eerste.

Een code-editor kunt u de stappen in deze zelfstudie doet. [Visual Studio Code](https://code.visualstudio.com/) is een uitstekende optie beschikbaar is op Windows, macOS en Linux-platforms.

### <a name="prerequisites"></a>Vereisten

Als u lokaal hebt gemaakt, downloadt en installeert de [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) als u dat nog niet gedaan hebt.

Hiervoor een build cloud met Azure DevOps bijvoorbeeld, zorg ervoor dat de [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) in uw build-systeem is geïnstalleerd.

### <a name="export-an-app-configuration-store"></a>Opslaan van de configuratie van een app exporteren

1. Open uw *.csproj* bestand en voeg het volgende script toe:

    ```xml
    <Target Name="Export file" AfterTargets="Build">
        <Message Text="Export the configurations to a temp file. " />
        <Exec WorkingDirectory="$(MSBuildProjectDirectory)" Condition="$(ConnectionString) != ''" Command="az appconfig kv export -f $(OutDir)\azureappconfig.json --format json --separator : --connection-string $(ConnectionString)" />
    </Target>
    ```

    Voeg de *ConnectionString* die zijn gekoppeld aan het opslaan van de app-configuratie als een omgevingsvariabele.

2. Open *Program.cs*, en werk de `CreateWebHostBuilder` methode voor het gebruik van het geëxporteerde bestand met JSON door het aanroepen van de `config.AddJsonFile()` methode.

    ```csharp
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                var directory = System.IO.Path.GetDirectoryName(Assembly.GetExecutingAssembly().Location);
                var settings = config.Build();

                config.AddJsonFile(Path.Combine(directory, "azureappconfig.json"));
                config.AddAzureAppConfiguration(settings["ConnectionStrings:AppConfig"]);
            })
            .UseStartup<Startup>();
    ```

### <a name="build-and-run-the-app-locally"></a>De app lokaal compileren en uitvoeren

1. Stel een omgevingsvariabele met de naam **ConnectionString**, en stel deze in op de toegangssleutel voor het opslaan van de app-configuratie. Als u de Windows-opdrachtprompt, voer de volgende opdracht uit en start opnieuw op de opdrachtprompt om toe te staan van de wijziging door te voeren:

        setx ConnectionString "connection-string-of-your-app-configuration-store"

    Als u Windows PowerShell gebruikt, voert u de volgende opdracht uit:

        $Env:ConnectionString = "connection-string-of-your-app-configuration-store"

    Als u Mac OS of Linux gebruikt, voert u de volgende opdracht uit:

        export ConnectionString='connection-string-of-your-app-configuration-store'

2. Voor het bouwen van de app met behulp van .NET Core CLI, voer de volgende opdracht in de opdrachtshell:

        dotnet build

3. Nadat de build is voltooid, voert u de volgende opdracht om de web-app lokaal uitvoeren:

        dotnet run

4. Open een browservenster en Ga naar `http://localhost:5000`, dit is de standaard-URL voor de web-app die lokaal wordt gehost.

    ![Quickstart voor het lokaal starten van een app](./media/quickstarts/aspnet-core-app-launch-local.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u Azure App configuratiegegevens die moeten worden gebruikt in een implementatiepijplijn geëxporteerd. Voor meer informatie over het gebruik van App-configuratie, gaat u naar de Azure CLI-voorbeelden.

> [!div class="nextstepaction"]
> [Integratie van beheerde identiteit](./howto-integrate-azure-managed-service-identity.md)
