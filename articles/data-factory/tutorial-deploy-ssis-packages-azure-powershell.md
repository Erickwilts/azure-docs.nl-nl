---
title: De Azure-SSIS-integratieruntime inrichten met PowerShell | Microsoft Docs
description: Informatie over het inrichten van de Azure-SSIS-integratieruntime in Azure Data Factory met PowerShell, zodat u SSIS-pakketten in Azure kunt implementeren en uitvoeren.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: tutorial
ms.date: 10/28/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: cba8af369beac935834da8d2073510e0f997648b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "66123460"
---
# <a name="provision-the-azure-ssis-integration-runtime-in-azure-data-factory-with-powershell"></a>De Azure-SSIS-integratieruntime inrichten in Azure Data Factory met PowerShell
Deze zelfstudie bevat de stappen voor het inrichten van een Azure SSIS Integration Runtime (IR) in Azure Data Factory. Vervolgens kunt u SSDT (SQL Server Data Tools) of SSMS (SQL Server Management Studio) gebruiken om SSIS-pakketten (SQL Server Integration Services) te implementeren en uit te voeren in deze runtime van Azure. In deze zelfstudie voert u de volgende stappen uit:

> [!NOTE]
> In dit artikel wordt Azure PowerShell gebruikt voor het inrichten van een Azure SSIS-IR. Als u de gebruikersinterface (UI) van Data Factory wilt gebruiken voor het inrichten van een Azure SSIS-IR, gaat u naar [Zelfstudie: een Azure-SSIS-integratieruntime maken](tutorial-create-azure-ssis-runtime-portal.md). 

> [!div class="checklist"]
> * Een data factory maken.
> * Een Azure SSIS Integration Runtime maken
> * De Azure SSIS Integration Runtime starten
> * SSIS-pakketten implementeren
> * Het volledige script bekijken

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

- **Azure-abonnement**. Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint. Zie [Overzicht van Integration Runtime in Azure-SSIS](concepts-integration-runtime.md#azure-ssis-integration-runtime) voor algemene informatie over Azure-SSIS IR. 
- **Azure SQL Database-server**. Als u nog geen databaseserver hebt, maakt u die in Azure Portal voordat u begint. Deze server fungeert als host voor de SSIS-catalogusdatabase (SSISDB). Het wordt aangeraden om de databaseserver in dezelfde Azure-regio te maken als de Integration Runtime. Met deze configuratie kan de Integration Runtime uitvoeringslogboeken wegschrijven naar SSISDB zonder dat hierbij Azure-regio's worden overschreden. 
    - Op basis van de geselecteerde databaseserver kan SSISDB namens u worden gemaakt als een enkele database, als onderdeel van een elastische pool of in een beheerd exemplaar. Deze is toegankelijk in een openbaar netwerk of kan worden toegevoegd aan een virtueel netwerk. Voor hulp bij de keuze voor het type databaseserver waarin u SSISDB host, raadpleegt u [Individuele database/elastische pools van Azure SQL Database vergelijken met Beheerd exemplaar](../data-factory/create-azure-ssis-integration-runtime.md#compare-sql-database-single-databaseelastic-pool-and-sql-database-managed-instance). Als u Azure SQL Database gebruikt met een service-eindpunt voor een virtueel netwerk/een beheerd exemplaar voor het hosten van SSISDB of toegang tot on-premises gegevens vereist, moet u uw Azure-SSIS IR toevoegen aan een virtueel netwerk. Zie [Azure-SSIS IR in een virtueel netwerk maken](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime). 
    - Controleer of de instelling **Toegang tot Azure-services toestaan** is ingesteld op **AAN** voor de databaseserver. Deze instelling is niet van toepassing wanneer u Azure SQL Database met een service-eindpunt voor een virtueel netwerk/Beheerd exemplaar gebruikt voor het hosten van SSISDB. Zie [Secure your Azure SQL database](../sql-database/sql-database-security-tutorial.md#create-firewall-rules) (Azure SQL-database beveiligen) voor meer informatie. Zie voor meer informatie over het inschakelen van deze instelling met behulp van PowerShell [New-AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule). 
    - Voeg het IP-adres van de clientcomputer (of een reeks IP-adressen dat het IP-adres van de clientcomputer bevat) toe aan de lijst met client-IP-adressen in de instellingen van de firewall voor de databaseserver. Zie [Overzicht van firewallregels op Azure SQL Database-serverniveau en -databaseniveau](../sql-database/sql-database-firewall-configure.md) voor meer informatie. 
    - U kunt verbinding maken met de databaseserver via SQL-verificatie met de beheerdersreferenties van de server of AAD-verificaties (Azure Active Directory) met de beheerde identiteit voor uw ADF (Azure Data Factory).  Voor het laatste moet u de beheerde identiteit voor de ADF toevoegen aan een AAD-groep met machtigingen voor toegang tot de databaseserver. Zie [Azure-SSIS IR met AAD-verificaties maken](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime). 
    - Controleer of uw Azure SQL Database-server geen SSIS-catalogus (SSISDB-database) heeft. Het inrichten van Azure-SSIS-IR biedt geen ondersteuning voor het gebruik van een bestaande SSIS-catalogus. 
- **Azure PowerShell**. Volg de instructies in [How to install and configure Azure PowerShell](/powershell/azure/install-Az-ps) (Azure PowerShell installeren en configureren). U gebruikt PowerShell om een script uit te voeren voor het inrichten van een Azure SSIS Integration Runtime die SSIS-pakketten uitvoert in de cloud. 

> [!NOTE]
> - Zie [Beschikbaarheid van ADF en SSIS IR per regio](https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all) voor een lijst met Azure-regio's waarin Data Factory en Azure-SSIS Integration Runtime momenteel beschikbaar zijn. 

## <a name="launch-windows-powershell-ise"></a>Windows PowerShell ISE starten
Start **Windows PowerShell ISE** met beheerdersbevoegdheden. 

## <a name="create-variables"></a>Variabelen maken
Kopieer en plak het volgende script: Geef waarden op voor de variabelen. Voor een lijst met ondersteunde **prijscategorieën** voor Azure SQL Database raadpleegt u [SQL Database resource limits](../sql-database/sql-database-resource-limits.md) (Limieten voor SQL Database-resources).

```powershell
# Azure Data Factory information 
# If your input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$"
$SubscriptionName = "[Azure subscription name]"
$ResourceGroupName = "[Azure resource group name]"
# Data factory name. Must be globally unique
$DataFactoryName = "[Data factory name]"
$DataFactoryLocation = "EastUS"

# Azure-SSIS integration runtime information - This is a Data Factory compute resource for running SSIS packages
$AzureSSISName = "[Specify a name for your Azure-SSIS IR]"
$AzureSSISDescription = "[Specify a description for your Azure-SSIS IR]"
$AzureSSISLocation = "EastUS"
# For supported node sizes, see https://azure.microsoft.com/pricing/details/data-factory/ssis/
$AzureSSISNodeSize = "Standard_D8_v3"
# 1-10 nodes are currently supported
$AzureSSISNodeNumber = 2 
# Azure-SSIS IR edition/license info: Standard or Enterprise 
$AzureSSISEdition = "Standard" # Standard by default, while Enterprise lets you use advanced/premium features on your Azure-SSIS IR
# Azure-SSIS IR hybrid usage info: LicenseIncluded or BasePrice
$AzureSSISLicenseType = "LicenseIncluded" # LicenseIncluded by default, while BasePrice lets you bring your own on-premises SQL Server license to earn cost savings from Azure Hybrid Benefit (AHB) option
# For a Standard_D1_v2 node, 1-4 parallel executions per node are supported, but for other nodes, 1-8 are currently supported
$AzureSSISMaxParallelExecutionsPerNode = 8
# Custom setup info
$SetupScriptContainerSasUri = "" # OPTIONAL to provide SAS URI of blob container where your custom setup script and its associated files are stored

# SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name].database.windows.net" # WARNING: Please ensure that there is no existing SSISDB, so we can prepare and manage one on your behalf    
$SSISDBServerAdminUserName = "[your server admin username for SQL authentication]"
$SSISDBServerAdminPassword = "[your server admin password for SQL authentication]"
# For the basic pricing tier, specify "Basic", not "B" - For standard/premium/elastic pool tiers, specify "S0", "S1", "S2", "S3", etc.
$SSISDBPricingTier = "[Basic|S0|S1|S2|S3|S4|S6|S7|S9|S12|P1|P2|P4|P6|P11|P15|…|ELASTIC_POOL(name = <elastic_pool_name>)]"
```

## <a name="validate-the-connection-to-database"></a>Valideren van verbinding met de database
Voeg het volgende script toe om server.database.windows.net van uw Azure SQL Database-server te valideren, `<servername>.database.windows.net`. 

```powershell
$SSISDBConnectionString = "Data Source=" + $SSISDBServerEndpoint + ";User ID=" + $SSISDBServerAdminUserName + ";Password=" + $SSISDBServerAdminPassword    
$sqlConnection = New-Object System.Data.SqlClient.SqlConnection $SSISDBConnectionString;
Try
{
    $sqlConnection.Open();
}
Catch [System.Data.SqlClient.SqlException]
{
    Write-Warning "Cannot connect to your Azure SQL Database server, exception: $_";
    Write-Warning "Please make sure the server you specified has already been created. Do you want to proceed? [Y/N]"
    $yn = Read-Host
    if(!($yn -ieq "Y"))
    {
        Return;
    } 
}
```

Zie het volgende voorbeeld als u een Azure SQL-database onderdeel van het script wilt maken: 

Stel waarden in voor de variabelen die nog niet zijn gedefinieerd. Bijvoorbeeld: SSISDBServerName, FirewallIPAddress. 

```powershell
New-AzSqlServer -ResourceGroupName $ResourceGroupName `
    -ServerName $SSISDBServerName `
    -Location $DataFactoryLocation `
    -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $SSISDBServerAdminUserName, $(ConvertTo-SecureString -String $SSISDBServerAdminPassword -AsPlainText -Force))    

New-AzSqlServerFirewallRule -ResourceGroupName $ResourceGroupName `
    -ServerName $SSISDBServerName `
    -FirewallRuleName "ClientIPAddress_$today" -StartIpAddress $FirewallIPAddress -EndIpAddress $FirewallIPAddress

New-AzSqlServerFirewallRule -ResourceGroupName $ResourceGroupName -ServerName $SSISDBServerName -AllowAllAzureIPs
```

## <a name="log-in-and-select-subscription"></a>Aanmelden en abonnement selecteren
Voeg de volgende code toe aan het script om u aan te melden en uw Azure-abonnement te selecteren: 

```powershell
Connect-AzAccount
Select-AzSubscription -SubscriptionName $SubscriptionName
```

## <a name="create-a-resource-group"></a>Een resourcegroep maken
Maak een [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) met behulp van de [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) opdracht. Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en groepsgewijs worden beheerd. In het volgende voorbeeld wordt een resourcegroep met de naam `myResourceGroup` gemaakt op de locatie `westeurope`.

Als uw resourcegroep al bestaat, hoeft u deze code niet naar het script te kopiëren. 

```powershell
New-AzResourceGroup -Location $DataFactoryLocation -Name $ResourceGroupName
```

## <a name="create-a-data-factory"></a>Een gegevensfactory maken
Voer de volgende opdracht uit om een data factory te maken:

```powershell
Set-AzDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                         -Location $DataFactoryLocation `
                         -Name $DataFactoryName
```

## <a name="create-an-integration-runtime"></a>Een Integration Runtime maken
Voer de volgende opdracht uit om een Azure SSIS Integration Runtime te maken die SSIS-pakketten uitvoert in Azure: 

```powershell
$secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
$serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)
  
Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                           -DataFactoryName $DataFactoryName `
                                           -Name $AzureSSISName `
                                           -Description $AzureSSISDescription `
                                           -Type Managed `
                                           -Location $AzureSSISLocation `
                                           -NodeSize $AzureSSISNodeSize `
                                           -NodeCount $AzureSSISNodeNumber `
                                           -Edition $AzureSSISEdition `
                                           -LicenseType $AzureSSISLicenseType `
                                           -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                           -CatalogServerEndpoint $SSISDBServerEndpoint `
                                           -CatalogAdminCredential $serverCreds `
                                           -CatalogPricingTier $SSISDBPricingTier

if(![string]::IsNullOrEmpty($SetupScriptContainerSasUri))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -SetupScriptContainerSasUri $SetupScriptContainerSasUri
}
```

## <a name="start-integration-runtime"></a>Integration Runtime starten
Voer de volgende opdracht uit om de Azure SSIS Integration Runtime te starten: 

```powershell
write-host("##### Starting your Azure-SSIS integration runtime. This command takes 20 to 30 minutes to complete. #####")
Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")                                  
```
Het duurt **20-30 minuten** voordat deze opdracht is voltooid. 

## <a name="deploy-ssis-packages"></a>SSIS-pakketten implementeren
Gebruik nu SQL Server Data Tools (SSDT) of SQL Server Management Studio (SSMS) om uw SSIS-pakketten te implementeren in Azure. Maak verbinding met de Azure SQL-server waarop de SSIS-catalogus (SSISDB) is opgeslagen. De naam van de Azure SQL Database-server heeft de notatie `<servername>.database.windows.net`. 

Raadpleeg de volgende artikelen uit de SSIS-documentatie: 

- [Deploy, run, and monitor an SSIS package on Azure](/sql/integration-services/lift-shift/ssis-azure-deploy-run-monitor-tutorial) (SSIS-pakket implementeren, uitvoeren en bewaken in Azure)   
- [Connect to SSIS catalog on Azure](/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database) (Verbinding maken met een SSIS-catalogus in Azure)
- [Schedule package execution on Azure](/sql/integration-services/lift-shift/ssis-azure-schedule-packages) (Pakketuitvoering plannen in Azure)
- [Connect to on-premises data sources with Windows authentication](/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth) (Verbinding maken met on-premises gegevensbronnen met Windows-verificatie) 

## <a name="full-script"></a>Volledige script
Met het PowerShell-script in deze sectie wordt er een instantie van de Azure SSIS Integration Runtime geconfigureerd die SSIS-pakketten uitvoert in de cloud. Nadat u dit script hebt uitgevoerd, kunt u SSIS-pakketten implementeren en uitvoeren in de Microsoft Azure-cloud waarbij SSISDB wordt gehost in Azure SQL Database.

1. Start Windows PowerShell ISE (Integrated Scripting Environment).
2. Voer in ISE de volgende opdracht uit vanaf de opdrachtprompt.    
    ```powershell
    Set-ExecutionPolicy Unrestricted -Scope CurrentUser
    ```
3. Kopieer het PowerShell-script in deze sectie en plak het in ISE.
4. Geef aan het begin van het script de juiste waarden voor alle parameters op.
5. Voer het script uit. De opdracht `Start-AzDataFactoryV2IntegrationRuntime` aan het einde van het script neemt **20-30 minuten** in beslag.

> [!NOTE]
> - Het script maakt verbinding met uw Azure SQL Database-server om de SSIS-catalogusdatabase (SSISDB) voor te bereiden.
> 
> - Wanneer u een exemplaar van een Azure-SSI-IR inricht, worden ook het Azure Feature Pack voor SSIS en de Access Redistributable geïnstalleerd. Deze onderdelen bieden connectiviteit met Excel- en Access-bestanden en met verschillende Azure-gegevensbronnen, naast de gegevensbronnen die worden ondersteund door de ingebouwde onderdelen. U kunt ook extra onderdelen installeren. Zie [Aangepaste instelling voor de Azure-SSIS-integratieruntime](how-to-configure-azure-ssis-ir-custom-setup.md) voor meer informatie.

Voor een lijst met ondersteunde **prijscategorieën** voor Azure SQL Database raadpleegt u [SQL Database resource limits](../sql-database/sql-database-resource-limits.md) (Limieten voor SQL Database-resources). 

Zie [Beschikbaarheid van ADF en SSIS IR per regio](https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all) voor een lijst met Azure-regio's waarin Data Factory en Azure-SSIS Integration Runtime momenteel beschikbaar zijn. 

```powershell
# Azure Data Factory information 
# If your input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$"
$SubscriptionName = "[Azure subscription name]"
$ResourceGroupName = "[Azure resource group name]"
# Data factory name. Must be globally unique
$DataFactoryName = "[Data factory name]"
$DataFactoryLocation = "EastUS"

# Azure-SSIS integration runtime information - This is a Data Factory compute resource for running SSIS packages
$AzureSSISName = "[Specify a name for your Azure-SSIS IR]"
$AzureSSISDescription = "[Specify a description for your Azure-SSIS IR]"
$AzureSSISLocation = "EastUS"
# For supported node sizes, see https://azure.microsoft.com/pricing/details/data-factory/ssis/
$AzureSSISNodeSize = "Standard_D8_v3"
# 1-10 nodes are currently supported
$AzureSSISNodeNumber = 2 
# Azure-SSIS IR edition/license info: Standard or Enterprise 
$AzureSSISEdition = "Standard" # Standard by default, while Enterprise lets you use advanced/premium features on your Azure-SSIS IR
# Azure-SSIS IR hybrid usage info: LicenseIncluded or BasePrice
$AzureSSISLicenseType = "LicenseIncluded" # LicenseIncluded by default, while BasePrice lets you bring your own on-premises SQL Server license to earn cost savings from Azure Hybrid Benefit (AHB) option
# For a Standard_D1_v2 node, 1-4 parallel executions per node are supported, but for other nodes, 1-8 are currently supported
$AzureSSISMaxParallelExecutionsPerNode = 8
# Custom setup info
$SetupScriptContainerSasUri = "" # OPTIONAL to provide SAS URI of blob container where your custom setup script and its associated files are stored

# SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name].database.windows.net" # WARNING: Please ensure that there is no existing SSISDB, so we can prepare and manage one on your behalf    
$SSISDBServerAdminUserName = "[your server admin username for SQL authentication]"
$SSISDBServerAdminPassword = "[your server admin password for SQL authentication]"
# For the basic pricing tier, specify "Basic", not "B" - For standard/premium/elastic pool tiers, specify "S0", "S1", "S2", "S3", etc.
$SSISDBPricingTier = "[Basic|S0|S1|S2|S3|S4|S6|S7|S9|S12|P1|P2|P4|P6|P11|P15|…|ELASTIC_POOL(name = <elastic_pool_name>)]"

$SSISDBConnectionString = "Data Source=" + $SSISDBServerEndpoint + ";User ID=" + $SSISDBServerAdminUserName + ";Password=" + $SSISDBServerAdminPassword    
$sqlConnection = New-Object System.Data.SqlClient.SqlConnection $SSISDBConnectionString;
Try
{
    $sqlConnection.Open();
}
Catch [System.Data.SqlClient.SqlException]
{
    Write-Warning "Cannot connect to your Azure SQL Database server, exception: $_";
    Write-Warning "Please make sure the server you specified has already been created. Do you want to proceed? [Y/N]"
    $yn = Read-Host
    if(!($yn -ieq "Y"))
    {
        Return;
    } 
}

Connect-AzAccount
Select-AzSubscription -SubscriptionName $SubscriptionName

Set-AzDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                         -Location $DataFactoryLocation `
                         -Name $DataFactoryName
    
$secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
$serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)
    
Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                           -DataFactoryName $DataFactoryName `
                                           -Name $AzureSSISName `
                                           -Description $AzureSSISDescription `
                                           -Type Managed `
                                           -Location $AzureSSISLocation `
                                           -NodeSize $AzureSSISNodeSize `
                                           -NodeCount $AzureSSISNodeNumber `
                                           -Edition $AzureSSISEdition `
                                           -LicenseType $AzureSSISLicenseType `
                                           -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                           -CatalogServerEndpoint $SSISDBServerEndpoint `
                                           -CatalogAdminCredential $serverCreds `
                                           -CatalogPricingTier $SSISDBPricingTier

if(![string]::IsNullOrEmpty($SetupScriptContainerSasUri))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -SetupScriptContainerSasUri $SetupScriptContainerSasUri
}

write-host("##### Starting your Azure-SSIS integration runtime. This command takes 20 to 30 minutes to complete. #####")
Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")
```

## <a name="join-azure-ssis-ir-to-a-virtual-network"></a>Azure-SSIS-integratieruntime toevoegen aan een virtueel netwerk
Als u Azure SQL Database gebruikt met service-eindpunten voor een virtueel netwerk/een beheerd exemplaar die een virtueel netwerk verbindt om SSISDB te hosten, moet u ook uw Azure-SSIS-integratieruntime bij hetzelfde virtuele netwerk voegen. In Azure Data Factory kunt u uw Azure-SSIS-integratieruntime toevoegen aan een virtueel netwerk. Zie [Een Azure-SSIS-integratieruntime toevoegen aan een virtueel netwerk](join-azure-ssis-integration-runtime-virtual-network.md) voor meer informatie.

Zie [Een Azure SSIS-integratieruntime maken](create-azure-ssis-integration-runtime.md) voor een volledig script voor het maken van een Azure-SSIS-integratieruntime die wordt toegevoegd aan een virtueel netwerk.

## <a name="monitor-and-manage-azure-ssis-ir"></a>Azure-SSIS IR controleren en beheren
Zie de volgende artikelen voor meer informatie over het controleren en beheren van een Azure-SSIS IR. 

- [Monitor an Azure-SSIS integration runtime](monitor-integration-runtime.md#azure-ssis-integration-runtime) (Een Azure SSIS Integration Runtime controleren)
- [Manage an Azure-SSIS integration runtime](manage-azure-ssis-integration-runtime.md) (Een Azure SSIS Integration Runtime beheren)

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie heeft u het volgende geleerd: 

> [!div class="checklist"]
> * Een data factory maken.
> * Een Azure SSIS Integration Runtime maken
> * De Azure SSIS Integration Runtime starten
> * SSIS-pakketten implementeren
> * Het volledige script bekijken

Voor meer informatie over uw Azure-SSIS-integratieruntime, gaat u verder met het volgende artikel:

> [!div class="nextstepaction"]
>[Azure-SSIS IR aanpassen](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup)
