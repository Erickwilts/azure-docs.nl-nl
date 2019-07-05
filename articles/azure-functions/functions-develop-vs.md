---
title: Ontwikkel Azure Functions met Visual Studio | Microsoft Docs
description: Informatie over het ontwikkelen en testen van Azure Functions met behulp van Azure Functions Tools voor Visual Studio 2019.
services: functions
documentationcenter: .net
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.custom: vs-azure
ms.topic: conceptual
ms.date: 10/08/2018
ms.author: glenga
ms.openlocfilehash: 8ed3b42c61456f110925e34473dbb326dafc1b80
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67447720"
---
# <a name="develop-azure-functions-using-visual-studio"></a>Ontwikkel Azure Functions met Visual Studio  

Azure Functions-hulpprogramma's is een extensie voor Visual Studio kunt u ontwikkelen, testen en implementeren van C# functies naar Azure. Als deze ervaring uw eerste met Azure Functions is, kunt u meer informatie op [een inleiding tot Azure Functions](functions-overview.md).

De Azure Functions-hulpprogramma's biedt de volgende voordelen: 

* Bewerken, ontwikkelen en uitvoeren van functies op uw lokale ontwikkelcomputer. 
* Publiceer uw Azure Functions-project rechtstreeks naar Azure. 
* WebJobs kenmerken gebruiken om te declareren functiebindingen rechtstreeks in de C#-code in plaats van het onderhouden van een afzonderlijke function.json voor het binden van definities.
* Ontwikkel en implementeer vooraf gecompileerde C#-functies. Vooraf voldaan functions biedt een betere koude start prestaties dan C#-script op basis van functies. 
* Uw functies in C#-code terwijl alle van de voordelen van Visual Studio-ontwikkeling. 

Dit artikel bevat informatie over het gebruik van de Azure Functions Tools voor Visual Studio 2019 voor het ontwikkelen van C# functies en deze publiceren naar Azure. Voordat u dit artikel leest, moet u uitvoeren de [Functions-Snelstartgids voor Visual Studio](functions-create-your-first-function-visual-studio.md). 

> [!IMPORTANT]
> Combineer geen lokale ontwikkeling met portal-ontwikkeling in dezelfde functie-app. Wanneer u in een lokale-project met een functie-app publiceert, worden alle functies die u hebt ontwikkeld in de portal in het implementatieproces overschreven.

## <a name="prerequisites"></a>Vereisten

Azure Functions-hulpprogramma's is opgenomen in de Azure-ontwikkelworkload van [Visual Studio 2017](https://www.visualstudio.com/vs/), of een latere versie. Zorg ervoor dat u de **Azure-ontwikkeling** werkbelasting in de installatie van Visual Studio 2019:

![Visual Studio 2019 installeren met de Azure-ontwikkelworkload](./media/functions-create-your-first-function-visual-studio/functions-vs-workloads.png)

Zorg ervoor dat uw Visual Studio up-to-date is en dat u gebruikmaakt van de [meest recente versie](#check-your-tools-version) van de Azure Functions-hulpprogramma's.

### <a name="azure-resources"></a>Azure-resources

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

Andere resources die u nodig hebt, zoals een Azure Storage-account zijn gemaakt in uw abonnement gedurende het publicatieproces.

### <a name="check-your-tools-version"></a>Controleer uw versie van de hulpprogramma 's

1. Uit de **extra** menu, kiest u **extensies en Updates**. Vouw **geïnstalleerde** > **extra** en kies **Azure Functions en Webjobs-hulpprogramma's**.

    ![Controleer of de versie van de hulpprogramma's voor functies](./media/functions-develop-vs/functions-vstools-check-functions-tools.png)

2. Houd er rekening mee de geïnstalleerde **versie**. U kunt deze versie met de meest recente versie vergelijken [in de opmerkingen bij de release](https://github.com/Azure/Azure-Functions/blob/master/VS-AzureTools-ReleaseNotes.md). 

3. Als uw versie ouder is, werkt u uw hulpprogramma's in Visual Studio zoals wordt weergegeven in de volgende sectie.

### <a name="update-your-tools"></a>De hulpprogramma's bijwerken

1. In de **extensies en Updates** dialoogvenster Vouw **Updates** > **Visual Studio Marketplace**, kiest u **Azure Functions en Webjobs-hulpprogramma's**  en selecteer **Update**.

    ![De versie van de Functions-hulpprogramma's bijwerken](./media/functions-develop-vs/functions-vstools-update-functions-tools.png)   

2. Nadat de hulpprogramma's-update is gedownload, sluit u Visual Studio aan de hulpprogramma's bijwerken met behulp van het installatieprogramma VSIX-trigger.

3. Kies in het installatieprogramma **OK** te starten en vervolgens **wijzigen** de hulpprogramma's bijwerken. 

4. Nadat de update voltooid is, kiest u **sluiten** en Visual Studio opnieuw te starten.

## <a name="create-an-azure-functions-project"></a>Een Azure Functions-project maken

[!INCLUDE [Create a project using the Azure Functions](../../includes/functions-vstools-create.md)]

De projectsjabloon, maken een C#-project maakt, installeert de `Microsoft.NET.Sdk.Functions` -pakket NuGet, en stelt u de doelframework. Functies 1.x doelen .NET Framework en 2.x doelen .NET Standard-functies. Het nieuwe project heeft de volgende bestanden:

* **host.json**: Kunt u de host van de functies configureren. Deze instellingen gelden zowel bij het uitvoeren van lokaal en in Azure. Zie voor meer informatie, [naslaginformatie over host.json](functions-host-json.md).

* **local.settings.json**: Instellingen die worden gebruikt bij het uitvoeren van functies lokaal onderhoudt. Deze instellingen worden niet gebruikt bij het uitvoeren in Azure. Zie voor meer informatie, [lokale instellingenbestand](#local-settings-file).

    >[!IMPORTANT]
    >Omdat het bestand local.settings.json kunt geheimen bevat, moet u deze uitgesloten van uw project broncodebeheer. De **naar uitvoermap kopiëren** instellen voor dit bestand altijd moet **kopiëren indien nieuwer**. 

Zie voor meer informatie, [Functions-klassebibliotheekproject](functions-dotnet-class-library.md#functions-class-library-project).

[!INCLUDE [functions-local-settings-file](../../includes/functions-local-settings-file.md)]

Instellingen in local.settings.json worden niet automatisch geüpload wanneer u het project publiceren. Om ervoor te zorgen dat deze instellingen ook aanwezig zijn in uw functie-app in Azure, moet u ze uploaden nadat u uw project kunt publiceren. Zie voor meer informatie, [functie app-instellingen](#function-app-settings).

De waarden in **ConnectionStrings** nooit worden gepubliceerd.

De waarden voor de functie-app-instellingen kunnen ook worden gelezen in uw code als omgevingsvariabelen. Zie voor meer informatie, [omgevingsvariabelen](functions-dotnet-class-library.md#environment-variables).

## <a name="configure-the-project-for-local-development"></a>Het project configureren voor lokale ontwikkeling

De Functions-runtime wordt intern gebruikt een Azure Storage-account. Voor alle andere typen dan HTTP en webhooks activeert, moet u instellen de **Values.AzureWebJobsStorage** sleutel op een geldige verbindingsreeks voor Azure Storage-account. Uw functie-app kunt ook de [Azure-opslagemulator](../storage/common/storage-use-emulator.md) voor de **AzureWebJobsStorage** verbinding instellingen vereist voor het project. Voor het gebruik van de emulator, stel de waarde van **AzureWebJobsStorage** naar `UseDevelopmentStorage=true`. Deze instelling wijzigen in een werkelijke opslagverbinding vóór de implementatie.

De verbindingsreeks voor opslag instellen:

1. Open in Visual Studio, **Cloud Explorer**, vouw **Opslagaccount** > **uw Storage-Account**en selecteer vervolgens **eigenschappen**en kopieer de **Primary Connection String** waarde.

2. In uw project, open het bestand local.settings.json en stel de waarde van de **AzureWebJobsStorage** sleutel op de verbindingstekenreeks die u hebt gekopieerd.

3. Herhaal de vorige stap om toe te voegen unieke sleutels zijn aan de **waarden** matrix voor alle andere verbindingen die vereist zijn door uw functies.

## <a name="add-a-function-to-your-project"></a>Een functie toevoegen aan uw project

De bindingen die worden gebruikt door de functie worden in de vooraf gecompileerde functies gedefinieerd door het toepassen van kenmerken in de code. Wanneer u de Azure Functions-hulpprogramma's om te maken van uw functies van de opgegeven sjablonen gebruikt, worden deze kenmerken worden toegepast voor u. 

1. Klik in **Solution Explorer** met de rechtermuisknop op het projectknooppunt en selecteer  > **Nieuw item** **Toevoegen**. Selecteer **Azure Function**, typ een **naam** voor de klasse en klik op **toevoegen**.

2. Kies de trigger, stelt u de bindingeigenschappen en klikt u op **maken**. Het volgende voorbeeld ziet de instellingen bij het maken van een Queue storage functie geactiveerde. 

    ![Een wachtrij geactiveerde functie maken](./media/functions-develop-vs/functions-vstools-create-queuetrigger.png)

    In dit voorbeeld trigger maakt gebruik van een verbindingsreeks met een sleutel met de naam **QueueStorage**. Deze instelling voor de verbindingsreeks moet worden gedefinieerd in de [bestand local.settings.json](functions-run-local.md#local-settings-file).

3. Onderzoek de zojuist toegevoegde klasse. Ziet u een statische **uitvoeren** methode, die wordt vermeld met de **functienaam** kenmerk. Dit kenmerk geeft aan dat de methode het toegangspunt voor de functie is.

    De volgende C#-klasse vertegenwoordigt bijvoorbeeld een eenvoudige Queue storage geactiveerde functie:

    ```csharp
    using System;
    using Microsoft.Azure.WebJobs;
    using Microsoft.Azure.WebJobs.Host;
    using Microsoft.Extensions.Logging;

    namespace FunctionApp1
    {
        public static class Function1
        {
            [FunctionName("QueueTriggerCSharp")]
            public static void Run([QueueTrigger("myqueue-items", Connection = "QueueStorage")]string myQueueItem, ILogger log)
            {
                log.LogInformation($"C# Queue trigger function processed: {myQueueItem}");
            }
        }
    }
    ```

    Een kenmerk binding-specifieke wordt toegepast op elke bindende parameter doorgegeven aan de methode post point. Het kenmerk wordt de informatie over de binding als parameters. In het vorige voorbeeld, de eerste parameter heeft een **QueueTrigger** kenmerk toegepast, waarmee wordt aangegeven wachtrij geactiveerde functie. De naam van de wachtrij en de naam van instelling voor de verbindingsreeks worden doorgegeven als parameters voor de **QueueTrigger** kenmerk. Zie voor meer informatie, [Azure Queue storage-bindingen voor Azure Functions](functions-bindings-storage-queue.md#trigger---c-example).

U kunt de bovenstaande procedure meer functies toevoegen aan uw functie-app-project. Elke functie in het project een andere trigger kan hebben, maar een functie moet exact één trigger hebben. Zie voor meer informatie, [Azure Functions-triggers en bindingen concepten](functions-triggers-bindings.md).

## <a name="add-bindings"></a>Bindingen toevoegen

Net als bij triggers, invoer- en uitvoerbindingen toegevoegd aan de functie als binding kenmerken. Bindingen aan een functie als volgt toevoegen:

1. Zorg ervoor dat u hebt [geconfigureerd van het project voor lokale ontwikkeling](#configure-the-project-for-local-development).

2. Het juiste NuGet-uitbreidingspakket voor de specifieke binding toevoegen. Zie voor meer informatie, [lokale C#-ontwikkeling met behulp van Visual Studio](./functions-bindings-register.md#local-csharp) in het artikel Triggers en bindingen. De binding-specifieke NuGet-pakket vereisten vindt u in het artikel verwijzing voor de binding. Pakket-vereisten voor de trigger van Event Hubs in bijvoorbeeld vindt u de [naslagartikel voor Event Hubs-binding](functions-bindings-event-hubs.md).

3. Als er appinstellingen die de binding nodig heeft, voeg deze toe aan de **waarden** verzameling in de [lokale instellingenbestand](functions-run-local.md#local-settings-file). Deze waarden worden gebruikt wanneer de functie lokaal wordt uitgevoerd. Wanneer de functie wordt uitgevoerd in de functie-app in Azure, de [functie app-instellingen](#function-app-settings) worden gebruikt.

4. De juiste binding-kenmerk toevoegen aan de methodehandtekening. In het volgende voorbeeld wordt een wachtrijbericht de functie activeert en de Uitvoerbinding maakt u een nieuw wachtrijbericht met dezelfde tekst in een andere wachtrij.

    ```csharp
    public static class SimpleExampleWithOutput
    {
        [FunctionName("CopyQueueMessage")]
        public static void Run(
            [QueueTrigger("myqueue-items-source", Connection = "AzureWebJobsStorage")] string myQueueItem, 
            [Queue("myqueue-items-destination", Connection = "AzureWebJobsStorage")] out string myQueueItemCopy,
            ILogger log)
        {
            log.LogInformation($"CopyQueueMessage function processed: {myQueueItem}");
            myQueueItemCopy = myQueueItem;
        }
    }
    ```
   De verbinding met Queue storage is verkregen uit de `AzureWebJobsStorage` instelling. Zie voor meer informatie het artikel verwijzing voor de specifieke binding. 

[!INCLUDE [Supported triggers and bindings](../../includes/functions-bindings.md)]

## <a name="testing-functions"></a>Functies testen

Met Azure Functions Core-hulpprogramma's kunt u Azure Functions-projecten uitvoeren op uw lokale ontwikkelcomputer. De eerste keer dat u een functie vanuit Visual Studio start, wordt u gevraagd deze hulpprogramma's te installeren.

Druk op F5 om de functie testen. Accepteer desgevraagd de aanvraag van Visual Studio om Azure Functions Core (CLI)-hulpprogramma's te downloaden en installeren. Mogelijk moet u ook een firewall-uitzondering inschakelen, zodat de hulpprogramma's HTTP-aanvragen kunnen afhandelen.

Met het project dat wordt uitgevoerd, kunt u uw code testen als u de geïmplementeerde functie wilt testen. Zie voor meer informatie, [strategieën voor het testen van uw code in Azure Functions](functions-test-a-function.md). Bij het uitvoeren in de foutopsporingsmodus, worden de onderbrekingspunten druk in Visual Studio zoals verwacht. 

<!---
For an example of how to test a queue triggered function, see the [queue triggered function quickstart tutorial](functions-create-storage-queue-triggered-function.md#test-the-function).  
-->

Zie voor meer informatie over het gebruik van Azure Functions Core Tools, [Code en Azure functions lokaal testen](functions-run-local.md).

## <a name="publish-to-azure"></a>Publiceren naar Azure

Bij het publiceren vanuit Visual Studio, een van twee methoden voor het implementeren worden gebruikt:

* [Web Deploy](functions-deployment-technologies.md#web-deploy-msdeploy): pakketten en Windows-apps implementeert op een IIS-server.
* [Implementeren met het uitvoeren van pakket ingeschakeld ZIP](functions-deployment-technologies.md#zip-deploy): aanbevolen voor implementaties van Azure Functions.

Gebruik de volgende stappen voor het publiceren van uw project aan een functie-app in Azure.

[!INCLUDE [Publish the project to Azure](../../includes/functions-vstools-publish.md)]

## <a name="function-app-settings"></a>Instellingen voor functie-app

Alle instellingen die u hebt toegevoegd in de local.settings.json moeten ook worden toegevoegd aan de functie-app in Azure. Deze instellingen worden niet automatisch geüpload wanneer u het project publiceren.

De eenvoudigste manier om de vereiste instellingen uploaden naar uw functie-app in Azure is met de **Toepassingsinstellingen beheren...**  koppeling die wordt weergegeven nadat u uw project is gepubliceerd.

![](./media/functions-develop-vs/functions-vstools-app-settings.png)

U ziet nu de **toepassingsinstellingen** dialoogvenster voor de functie-app, kunt u toepassingsinstellingen voor nieuwe toevoegen of bestaande wijzigen.

![](./media/functions-develop-vs/functions-vstools-app-settings2.png)

**Lokale** vertegenwoordigt een waarde in het bestand local.settings.json en **externe** is de huidige instelling in de functie-app in Azure.  Kies **-instelling toevoegen** om een nieuwe appinstelling te maken. Gebruik de **waarde invoeren tussen lokale** koppeling naar het kopiëren van een instellingswaarde voor de **externe** veld. Wijzigingen in behandeling is geschreven naar het bestand met lokale instellingen en de functie-app wanneer u selecteert **OK**.

U kunt ook de toepassingsinstellingen in een van deze andere manieren beheren:

* [Met behulp van de Azure-portal](functions-how-to-use-azure-function-app-settings.md#settings).
* [Met behulp van de `--publish-local-settings` optie publiceren in Azure Functions Core Tools](functions-run-local.md#publish).
* [Met behulp van de Azure CLI](/cli/azure/functionapp/config/appsettings#az-functionapp-config-appsettings-set).

## <a name="monitoring-functions"></a>Functions controleren

De aanbevolen manier voor het bewaken van de uitvoering van uw functies is door uw functie-app integreren met Azure Application Insights. Wanneer u een functie-app in Azure portal maakt, wordt deze integratie voor u uitgevoerd door standaard. Echter, wanneer u uw functie-app tijdens het publiceren van Visual Studio maakt, is niet de integratie in uw functie-app in Azure uitgevoerd.

Application Insights inschakelen voor uw functie-app:

[!INCLUDE [functions-connect-new-app-insights.md](../../includes/functions-connect-new-app-insights.md)]

Zie voor meer informatie, [Monitor Azure Functions](functions-monitoring.md).

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure Functions Core Tools, [Code en Azure functions lokaal testen](functions-run-local.md).

Zie voor meer informatie over het ontwikkelen van functies als .NET-klassebibliotheken, [Azure Functions C#-naslaginformatie](functions-dotnet-class-library.md). In dit artikel bevat ook koppelingen naar voorbeelden van hoe u kunt gebruikmaken van kenmerken om aan te geven van de verschillende typen bindingen die worden ondersteund door Azure Functions.    
