---
title: 'Zelfstudie: Gebeurtenisgegevens naar Azure Synapse Analytics migreren - Azure Event Hubs'
description: 'Zelfstudie: In deze zelfstudie leest u hoe u gegevens uit uw Event Hub vastlegt in Azure Synapse Analytics met behulp van een Azure-functie die door een gebeurtenissenraster wordt geactiveerd.'
services: event-hubs
ms.date: 06/23/2020
ms.topic: tutorial
ms.custom: devx-track-csharp
ms.openlocfilehash: b2a35647422c91d6859e1889f906ae512ce41a56
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "89436609"
---
# <a name="tutorial-migrate-captured-event-hubs-data-to-azure-synapse-analytics-using-event-grid-and-azure-functions"></a>Zelfstudie: Vastgelegde Event Hub-gegevens migreren naar Azure Synapse Analytics met behulp van Event Grid en Azure Functions

Event Hubs [Capture](./event-hubs-capture-overview.md) is de eenvoudigste manier om automatisch gestreamde gegevens in Event Hubs te verzenden naar een Azure Blob Storage of Azure Data Lake Store. U kunt de gegevens vervolgens verwerken en naar andere opslagdoelen van uw keuze verzenden, zoals Azure Synapse Analytics of Cosmos DB. In deze zelfstudie leest u hoe u gegevens uit uw Event Hub vastlegt in Azure Synapse Analytics met behulp van een Azure-functie die wordt geactiveerd door een [gebeurtenissenraster](../event-grid/overview.md).

![Visual Studio](./media/store-captured-data-data-warehouse/EventGridIntegrationOverview.PNG)

- U maakt eerst een Event Hub, waarbij de functie **Capture** is ingeschakeld, en stelt een Azure Blob Storage in als doel. Gegevens die worden gegenereerd door WindTurbineGenerator, worden gestreamd naar de Event Hub en automatisch vastgelegd in Azure Storage als Avro-bestanden.
- Vervolgens maakt u een Azure Event Grid-abonnement met de Event Hubs-naamruimte als bron en het Azure Functions-eindpunt als doel.
- Wanneer een nieuw Avro-bestand wordt verzonden naar de Azure Storage Blob met de functie Event Hubs Capture, stelt Event Grid Azure Functions op de hoogte met de blob-URI. Vervolgens worden de gegevens gemigreerd van de blob naar Azure Synapse Analytics.

In deze zelfstudie voert u de volgende acties uit:

> [!div class="checklist"]
>
> - De infrastructuur implementeren
> - Code publiceren naar een Azure Functions-app
> - Event Grid-abonnement maken vanuit de Azure Functions-app
> - Voorbeeldgegevens streamen naar Event Hub.
> - Vastgelegde gegevens verifiëren in Azure Synapse Analytics

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

- [Visual Studio 2019](https://www.visualstudio.com/vs/). Zorg ervoor dat u tijdens de installatie de volgende werkbelastingen installeert: .NET-desktopontwikkeling, Azure-ontwikkeling, ASP.NET- en webontwikkeling, Node.js-ontwikkeling en Python-ontwikkeling
- Download het [Git-voorbeeld](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Azure.Messaging.EventHubs/EventHubsCaptureEventGridDemo). De voorbeeldoplossing bevat de volgende onderdelen:

  - *WindTurbineDataGenerator*: een eenvoudige uitgever waarmee voorbeeldgegevens van windturbines worden verzonden naar een Event Hub waarvoor Capture is ingeschakeld
  - *FunctionDWDumper*: een Azure-functie die een Event Grid-melding ontvangt wanneer een Avro-bestand wordt vastgelegd in de Azure Storage Blob. Het URI-pad van de blob wordt ontvangen en de inhoud wordt gelezen, waarna deze gegevens naar Azure Synapse Analytics worden gepusht.

  In dit voorbeeld wordt het meest recente Azure.Messaging.Eventhubs-pakket gebruikt. U vindt het oude voorbeeld dat gebruikmaakt van het Microsoft.Azure.EventHubs-pakket [hier](https://github.com/Azure/azure-event-hubs/tree/master/samples/e2e/EventHubsCaptureEventGridDemo).

### <a name="deploy-the-infrastructure"></a>De infrastructuur implementeren

Gebruik Azure PowerShell of de Azure CLI om de infrastructuur te implementeren die nodig is voor deze zelfstudie met behulp van deze [Azure Resource Manager-sjabloon](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/event-grid/EventHubsDataMigration.json). Met deze sjabloon maakt u de volgende bronnen:

- Event Hub waarbij de functie Capture is ingeschakeld
- Opslagaccount voor de vastgelegde gebeurtenisgegevens
- Azure App Service-plan voor het hosten van de Functions-app
- Functions-app voor het verwerken van vastgelegde gebeurtenisbestanden
- Logische SQL-server voor het hosten van het datawarehouse
- Azure Synapse Analytics voor het opslaan van de gemigreerde gegevens

In de volgende secties vindt u Azure CLI- en Azure PowerShell-opdrachten voor het implementeren van de infrastructuur die is vereist voor de zelfstudie. Werk de namen van de volgende objecten bij voordat u de opdrachten uitvoert: 

- Azure-resourcegroep 
- Regio van de resourcegroup
- Event Hubs-naamruimte
- Event Hub
- Logische SQL-server
- SQL-gebruiker (en wachtwoord)
- Azure SQL-database
- Azure Storage 
- Azure Functions-app

Het duurt even voordat alle Azure-artefacten zijn gemaakt met deze scripts. Wacht totdat het script is voltooid voordat u doorgaat. Als de implementatie mislukt, verwijdert u de resourcegroep, lost u het gerapporteerde probleem op en voert u de opdracht opnieuw uit. 

#### <a name="azure-cli"></a>Azure CLI

Voer de volgende opdrachten uit om de sjabloon te implementeren met Azure CLI:

```azurecli-interactive
az group create -l westus -n rgDataMigrationSample

az group deployment create `
  --resource-group rgDataMigrationSample `
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/event-grid/EventHubsDataMigration.json `
  --parameters eventHubNamespaceName=<event-hub-namespace> eventHubName=hubdatamigration sqlServerName=<sql-server-name> sqlServerUserName=<user-name> sqlServerPassword=<password> sqlServerDatabaseName=<database-name> storageName=<unique-storage-name> functionAppName=<app-name>
```

#### <a name="azure-powershell"></a>Azure PowerShell
Voer de volgende opdrachten uit om de sjabloon te implementeren met PowerShell:

```powershell
New-AzResourceGroup -Name rgDataMigration -Location westcentralus

New-AzResourceGroupDeployment -ResourceGroupName rgDataMigration -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/event-grid/EventHubsDataMigration.json -eventHubNamespaceName <event-hub-namespace> -eventHubName hubdatamigration -sqlServerName <sql-server-name> -sqlServerUserName <user-name> -sqlServerDatabaseName <database-name> -storageName <unique-storage-name> -functionAppName <app-name>
```

### <a name="create-a-table-in-azure-synapse-analytics"></a>Een tabel maken in Azure Synapse Analytics

Maak een tabel in Azure Synapse Analytics door het script [CreateDataWarehouseTable.sql](https://github.com/Azure/azure-event-hubs/blob/master/samples/e2e/EventHubsCaptureEventGridDemo/scripts/CreateDataWarehouseTable.sql) uit te voeren met [Visual Studio](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-query-visual-studio.md), [SQL Server Management Studio](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-query-ssms.md) of de Query-editor in de portal. 

```sql
CREATE TABLE [dbo].[Fact_WindTurbineMetrics] (
    [DeviceId] nvarchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL, 
    [MeasureTime] datetime NULL, 
    [GeneratedPower] float NULL, 
    [WindSpeed] float NULL, 
    [TurbineSpeed] float NULL
)
WITH (CLUSTERED COLUMNSTORE INDEX, DISTRIBUTION = ROUND_ROBIN);
```

## <a name="publish-code-to-the-functions-app"></a>Code publiceren naar de Functions-app

1. Open de oplossing *EventHubsCaptureEventGridDemo.sln* in Visual Studio 2019.

1. Klik in Solution Explorer met de rechtermuisknop op *FunctionEGDWDumper* en selecteer **Publish**.

   ![Functie-app publiceren](./media/store-captured-data-data-warehouse/publish-function-app.png)

1. Selecteer **Azure Function App** en **Select Existing**. Selecteer **Publiceren**.

   ![Bestaande functie-app](./media/store-captured-data-data-warehouse/pick-target.png)

1. Selecteer de functie-app die u via de sjabloon hebt geïmplementeerd. Selecteer **OK**.

   ![Functie-app selecteren](./media/store-captured-data-data-warehouse/select-function-app.png)

1. Als Visual Studio het profiel heeft geconfigureerd, selecteert u **Publish**.

   ![Selecteer Publish](./media/store-captured-data-data-warehouse/select-publish.png)

Nadat de functie is gepubliceerd, kunt u zich abonneren op de vastleggebeurtenis vanuit Event Hubs.


## <a name="create-an-event-grid-subscription-from-the-functions-app"></a>Event Grid-abonnement maken vanuit de Azure Functions-app
 
1. Ga naar de [Azure Portal](https://portal.azure.com/). Selecteer de resourcegroep en de functie-app.

   ![Functie-app weergeven](./media/store-captured-data-data-warehouse/view-function-app.png)

1. Selecteer de functie.

   ![Functie selecteren](./media/store-captured-data-data-warehouse/select-function.png)

1. Selecteer **Add Event Grid subscription**.

   ![Abonnement toevoegen](./media/store-captured-data-data-warehouse/add-event-grid-subscription.png)

1. Geef een naam op voor het Event Grid-abonnement. Gebruik **Event Hubs Namespaces** als het gebeurtenistype. Geef waarden op om uw exemplaar van de Event Hubs-naamruimte te selecteren. Laat de waarde voor het eindpunt ongewijzigd (Subscriber Endpoint). Selecteer **Maken**.

   ![Abonnement maken](./media/store-captured-data-data-warehouse/set-subscription-values.png)

## <a name="generate-sample-data"></a>Voorbeeldgegevens genereren  
U bent nu klaar met het instellen van uw Event Hub, Azure Synapse Analytics, de Azure Functions-app en het Event Grid-abonnement. U kunt WindTurbineDataGenerator.exe uitvoeren om gegevensstromen te genereren naar de Event Hub nadat u de verbindingsreeks en de naam van uw Event Hub hebt bijgewerkt in de broncode. 

1. Selecteer in de portal de naamruimte van uw gebeurtenishub. Selecteer **Verbindingsreeksen**.

   ![Selecteer Verbindingsreeksen](./media/store-captured-data-data-warehouse/event-hub-connection.png)

2. Selecteer **RootManageSharedAccessKey**.

   ![Sleutel selecteren](./media/store-captured-data-data-warehouse/show-root-key.png)

3. Kopieer **Verbindingsreeks-primaire sleutel**.

   ![Sleutel kopiëren](./media/store-captured-data-data-warehouse/copy-key.png)

4. Ga terug naar het Visual Studio-project. Open *program.cs* in het project *WindTurbineDataGenerator*.

5. Werk de waarden voor **EventHubConnectionString** en **EventHubName** bij met verbindingsreeks en de naam van uw Event Hub. 

   ```cs
   private const string EventHubConnectionString = "Endpoint=sb://demomigrationnamespace.servicebus.windows.net/...";
   private const string EventHubName = "hubdatamigration";
   ```

6. Maak de oplossing en voer de toepassing WindTurbineGenerator.exe uit. 

## <a name="verify-captured-data-in-data-warehouse"></a>Vastgelegde gegevens verifiëren in het datawarehouse
Voer na een paar minuten een query uit op de tabel in Azure Synapse Analytics. U ziet dat de gegevens die zijn gegenereerd door WindTurbineDataGenerator, naar uw Event Hub zijn gestreamd, in een Azure Storage-container zijn vastgelegd en vervolgens door Azure Functions naar de Azure Synapse Analytics-tabel zijn gemigreerd.  

## <a name="next-steps"></a>Volgende stappen 
U kunt krachtige hulpmiddelen voor gegevensvisualisatie gebruiken met uw datawarehouse om bruikbare inzichten te verkrijgen.

In dit artikel leest u hoe u [Power BI met Azure Synapse Analytics](/power-bi/connect-data/service-azure-sql-data-warehouse-with-direct-connect) gebruikt