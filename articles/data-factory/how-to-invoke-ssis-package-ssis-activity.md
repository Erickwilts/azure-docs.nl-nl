---
title: Uitvoeren van SSIS-pakket met activiteit in de SSIS-pakket uitvoeren - Azure | Microsoft Docs
description: Dit artikel wordt beschreven hoe u een pakket met SQL Server Integration Services (SSIS) uitvoert in Azure Data Factory-pijplijn met behulp van de activiteit uitvoeren van SSIS-pakket.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: conceptual
ms.date: 03/19/2019
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: 7287dc2fccf461cf23c45202336e3d92bc5a40aa
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66153063"
---
# <a name="run-an-ssis-package-with-the-execute-ssis-package-activity-in-azure-data-factory"></a>Uitvoeren van een SSIS-pakket met de activiteit uitvoeren van SSIS-pakket in Azure Data Factory
In dit artikel wordt beschreven hoe u een SSIS-pakket in Azure Data Factory (ADF) pijplijn uitvoeren met behulp van de activiteit uitvoeren van SSIS-pakket. 

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Een Azure-SSIS Integration Runtime (IR) maken als u nog geen al door de stapsgewijze instructies in de [zelfstudie: SSIS-pakketten implementeren in Azure](tutorial-create-azure-ssis-runtime-portal.md).

## <a name="run-a-package-in-the-azure-portal"></a>Een pakket worden uitgevoerd in Azure portal
In deze sectie maakt u ADF gebruikersinterface (UI) / pipeline-app te maken van een ADF met SSIS-pakket uitvoeren-activiteit die uw SSIS-pakket wordt uitgevoerd.

### <a name="create-a-pipeline-with-an-execute-ssis-package-activity"></a>Een pijplijn maken met een activiteit uitvoeren van SSIS-pakket
In deze stap maakt u ADF gebruikersinterface/app gebruiken om een pijplijn te maken. U een activiteit uitvoeren van SSIS-pakket toevoegen aan de pijplijn en configureer deze voor uw SSIS-pakket. 

1. Klik op de pagina ADF overzicht en thuis in Azure portal op de **Author & Monitor** tegel ADF gebruikersinterface/app starten in een afzonderlijk tabblad. 

   ![Startpagina van de gegevensfactory](./media/how-to-invoke-ssis-package-stored-procedure-activity/data-factory-home-page.png)

   Op de **aan de slag** pagina, klikt u op **pijplijn maken**: 

   ![Pagina Aan de slag](./media/how-to-invoke-ssis-package-stored-procedure-activity/get-started-page.png)

2. In de **activiteiten** werkset Vouw **algemene**, slepen en neerzetten een **SSIS-pakket uitvoeren** activiteit naar het ontwerpoppervlak voor pijplijnen. 

   ![Sleep een activiteit uitvoeren van SSIS-pakket naar het ontwerpoppervlak voor pijplijnen](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-designer.png) 

3. Op de **algemene** tabblad voor de activiteit uitvoeren van SSIS-pakket, Geef een naam en beschrijving voor de activiteit. De time-out van de optionele instelt en probeer waarden.

   ![Eigenschappen worden ingesteld op het tabblad Algemeen](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-general.png)

4. Op de **instellingen** tabblad voor de activiteit SSIS-pakket uitvoeren, selecteert u uw Azure-SSIS-IR die is gekoppeld aan de SSISDB-database waar het pakket wordt geïmplementeerd. Als het pakket maakt gebruik van Windows-verificatie voor toegang tot gegevensarchieven, zoals SQL-Servers /-bestandsshares on-premises, Azure Files, enz., controleert u de **Windows-verificatie** selectievakje in en voer in het domein, de gebruikersnaam en het wachtwoord voor het pakket de uitvoering. Als het pakket 32-bits-runtime om uit te voeren moet, controleert u de **32-bits-runtime** selectievakje. Voor **niveau van logboekregistratie**, selecteert u een vooraf gedefinieerde bereik van logboekregistratie voor de uitvoering van uw pakket. Controleer de **aangepaste** selectievakje, als u wilt de naam van uw aangepaste logboekregistratie in plaats daarvan opgeven. Als uw Azure-SSIS IR wordt uitgevoerd en de **handmatige invoer** selectievakje is uitgeschakeld, kunt u bladeren en selecteer uw bestaande mappen/projecten /-pakketten /-omgevingen in SSISDB. Klik op de **vernieuwen** knop ophalen van de zojuist toegevoegde mappen/projecten /-pakketten /-omgevingen van SSISDB, zodat ze beschikbaar voor het bladeren door en selecteren zijn. 

   ![Eigenschappen worden ingesteld op het tabblad Instellingen - automatische](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-settings.png)

   Wanneer uw Azure-SSIS IR wordt niet uitgevoerd of de **handmatige invoer** selectievakje is ingeschakeld, kunt u de paden van uw pakket en de omgeving van SSISDB rechtstreeks in de volgende indelingen: `<folder name>/<project name>/<package name>.dtsx` en `<folder name>/<environment name>`.

   ![Eigenschappen worden ingesteld op het tabblad Instellingen - handleiding](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-settings2.png)

5. Op de **SSIS Parameters** tabblad voor het uitvoeren van SSIS-pakket activiteit, wanneer uw Azure-SSIS IR wordt uitgevoerd en de **handmatige invoer** selectievakje op **instellingen** tabblad dit selectievakje uitschakelt, wordt de u kunt waarden toewijzen aan deze bestaande SSIS-parameters in de geselecteerde project/pakket van SSISDB weergegeven. Anders kunt u ze één voor één aan waarden handmatig aan hen toewijzen: Zorg ervoor dat ze bestaan en correct worden ingevoerd voor de uitvoering van uw pakket te voltooien. U kunt dynamische inhoud toevoegen aan hun waarden met behulp van expressies, functies, ADF systeemvariabelen en ADF pijplijn parameters/variabelen. U kunt ook geheimen die zijn opgeslagen in uw Azure Key Vault (AKV) als hun waarden gebruiken. Om dit te doen, klikt u op de **AZURE KEY VAULT** selectievakje naast de relevante parameter selecteren/bewerken van uw bestaande gekoppelde Azure Sleutelkluis-service of maak een nieuwe, en selecteer vervolgens de geheime naam/versie voor de parameterwaarde.  Wanneer u maken/bewerken uw AKV gekoppelde service, kunt u uw bestaande AKV selecteren/bewerken of maak een nieuwe, maar neem ADF beheerde identiteit toegang verlenen tot uw Azure-Sleutelkluis als u niet hebt gedaan, al. U kunt ook uw geheimen rechtstreeks in de volgende notatie invoeren: `<AKV linked service name>/<secret name>/<secret version>`.

   ![Eigenschappen worden ingesteld op het tabblad SSIS-Parameters](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-ssis-parameters.png)

6. Op de **Connection Managers** tabblad voor het uitvoeren van SSIS-pakket activiteit, wanneer uw Azure-SSIS IR wordt uitgevoerd en de **handmatige invoer** selectievakje op **instellingen** tabblad is dit selectievakje uitschakelt, de bestaande verbinding managers in de geselecteerde project/pakket van SSISDB wordt voor u waarden wilt toewijzen aan de bijbehorende eigenschappen weergegeven. Anders kunt u ze één voor één aan waarden handmatig toewijzen aan hun eigenschappen: Zorg ervoor dat ze bestaan en correct worden ingevoerd voor de uitvoering van uw pakket te voltooien. U kunt dynamische inhoud toevoegen aan hun eigenschapswaarden met behulp van expressies, functies, ADF systeemvariabelen en ADF pijplijn parameters/variabelen. U kunt ook geheimen in uw Azure Key Vault (AKV) opgeslagen als hun eigenschapswaarden gebruiken. Om dit te doen, klikt u op de **AZURE KEY VAULT** selectievakje naast de betreffende eigenschap selecteren/bewerken van uw bestaande gekoppelde Azure Sleutelkluis-service of maak een nieuwe, en selecteer vervolgens de geheime naam/versie voor de waarde van de eigenschap.  Wanneer u maken/bewerken uw AKV gekoppelde service, kunt u uw bestaande AKV selecteren/bewerken of maak een nieuwe, maar neem ADF beheerde identiteit toegang verlenen tot uw Azure-Sleutelkluis als u niet hebt gedaan, al. U kunt ook uw geheimen rechtstreeks in de volgende notatie invoeren: `<AKV linked service name>/<secret name>/<secret version>`.

   ![Eigenschappen worden ingesteld op het tabblad Connection Managers](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-connection-managers.png)

7. Op de **eigenschap heeft voorrang op** tabblad voor de activiteit van de SSIS-pakket uitvoeren, kunt u de paden van bestaande eigenschappen invoeren in het geselecteerde pakket van SSISDB, één voor één aan waarden handmatig aan hen toewijzen: Zorg ervoor dat ze bestaan en zijn correct ingevoerd voor de uitvoering van uw pakket te voltooien, bijvoorbeeld als u wilt overschrijven de waarde van de variabele van de gebruiker, voert u het pad in de volgende indeling: `\Package.Variables[User::YourVariableName].Value`. U kunt ook dynamische inhoud toevoegen aan hun waarden met behulp van expressies, functies, ADF systeemvariabelen en ADF pijplijn parameters/variabelen.

   ![Eigenschappen worden ingesteld op het tabblad eigenschap heeft voorrang op](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-property-overrides.png)

8. Voor het valideren van de configuratie van de pijplijn, klikt u op **valideren** op de werkbalk. Sluit het venster **Pipeline Validation Report** door op **>>** te klikken.

9. De pijplijn publiceren naar ADF door te klikken op **Alles publiceren** knop. 

### <a name="run-the-pipeline"></a>De pijplijn uitvoeren
In deze stap maakt activeren u een pijplijnuitvoering. 

1. Als u wilt een pijplijnuitvoering activeren, klikt u op **Trigger** op de werkbalk en klikt u op **nu activeren**. 

   ![Nu activeren](./media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-trigger.png)

2. Selecteer in het venster **Pijplijnuitvoering** de optie **Voltooien**. 

### <a name="monitor-the-pipeline"></a>De pijplijn bewaken

1. Ga naar het tabblad **Controleren** aan de linkerkant. U ziet de pijplijnuitvoering en de status ervan samen met andere informatie (zoals Run Start tijd). Als u de lijst wilt vernieuwen, klikt u op **Vernieuwen**.

   ![Pijplijnuitvoeringen](./media/how-to-invoke-ssis-package-stored-procedure-activity/pipeline-runs.png)

2. Klik op de koppeling **Uitvoeringen van activiteiten weergeven** in de kolom **Acties**. Ziet u slechts één activiteit uitgevoerd als de pijplijn slechts één activiteit (het uitvoeren van SSIS-pakket heeft).

   ![Uitvoering van activiteiten](./media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-runs.png)

3. U kunt het volgende uitvoeren **query** op basis van de SSISDB-database in uw Azure SQL-server om te controleren of het pakket dat wordt uitgevoerd. 

   ```sql
   select * from catalog.executions
   ```

   ![Controleer of de pakket-uitvoeringen](./media/how-to-invoke-ssis-package-stored-procedure-activity/verify-package-executions.png)

4. U kunt ook de SSISDB uitvoerings-ID ophalen uit de uitvoer van de pijplijnuitvoering voor de activiteit en de-ID gebruiken om te controleren op uitgebreidere uitvoeringslogboeken en foutberichten in SSMS.

   ![Ophalen van de id van de uitvoering.](media/how-to-invoke-ssis-package-ssis-activity/get-execution-id.png)

### <a name="schedule-the-pipeline-with-a-trigger"></a>De pijplijn met een trigger plannen

U kunt ook een geplande trigger maken voor uw pijplijn, zodat de pijplijn wordt uitgevoerd volgens een planning (elk uur, dagelijks, enz.). Zie voor een voorbeeld [maken van een data factory - gebruikersinterface van Data Factory](quickstart-create-data-factory-portal.md#trigger-the-pipeline-on-a-schedule).

## <a name="run-a-package-with-powershell"></a>Uitvoeren van een pakket met PowerShell
In deze sectie kunt u Azure PowerShell gebruiken voor het maken van een ADF-pijplijn met de SSIS-pakket uitvoeren-activiteit die uw SSIS-pakket wordt uitgevoerd. 

Installeer de nieuwste Azure PowerShell-modules door de stapsgewijze instructies in [hoe u Azure PowerShell installeren en configureren](/powershell/azure/install-az-ps).

### <a name="create-an-adf-with-azure-ssis-ir"></a>Een ADF met Azure-SSIS IR maken
U kunt gebruiken van een bestaande ADF die al op Azure-SSIS IR is ingericht of maak een nieuwe ADF met Azure-SSIS IR de stapsgewijze instructies in de [zelfstudie: SSIS-pakketten implementeren in Azure via PowerShell](https://docs.microsoft.com/azure/data-factory/tutorial-deploy-ssis-packages-azure-powershell).

### <a name="create-a-pipeline-with-an-execute-ssis-package-activity"></a>Een pijplijn maken met een activiteit uitvoeren van SSIS-pakket 
In deze stap maakt maken u een pijplijn met een activiteit uitvoeren van SSIS-pakket. De activiteit wordt uitgevoerd voor uw SSIS-pakket. 

1. Maak een JSON-bestand met de naam **RunSSISPackagePipeline.json** in de **C:\ADF\RunSSISPackage** map met inhoud die vergelijkbaar is met het volgende voorbeeld:

   > [!IMPORTANT]
   > Vervang objectnamen, beschrijvingen en paden, eigenschap en parameterwaarden, wachtwoorden en andere waarden van variabelen voordat u het bestand opslaat. 

   ```json
   {
       "name": "RunSSISPackagePipeline",
       "properties": {
           "activities": [{
               "name": "mySSISActivity",
               "description": "My SSIS package/activity description",
               "type": "ExecuteSSISPackage",
               "typeProperties": {
                   "connectVia": {
                       "referenceName": "myAzureSSISIR",
                       "type": "IntegrationRuntimeReference"
                   },
                   "executionCredential": {
                       "domain": "MyDomain",
                       "userName": "MyUsername",
                       "password": {
                           "type": "SecureString",
                           "value": "**********"
                       }
                   },
                   "runtime": "x64",
                   "loggingLevel": "Basic",
                   "packageLocation": {
                       "packagePath": "FolderName/ProjectName/PackageName.dtsx"
                   },
                   "environmentPath": "FolderName/EnvironmentName",
                   "projectParameters": {
                       "project_param_1": {
                           "value": "123"
                       },
                       "project_param_2": {
                           "value": {
                               "value": "@pipeline().parameters.MyPipelineParameter",
                               "type": "Expression"
                           }
                       }
                   },
                   "packageParameters": {
                       "package_param_1": {
                           "value": "345"
                       },
                       "package_param_2": {
                           "value": {
                               "type": "AzureKeyVaultSecret",
                               "store": {
                                   "referenceName": "myAKV",
                                   "type": "LinkedServiceReference"
                               },
                               "secretName": "MySecret"
                           }
                       }
                   },
                   "projectConnectionManagers": {
                       "MyAdonetCM": {
                           "userName": {
                               "value": "sa"
                           },
                           "passWord": {
                               "value": {
                                   "type": "SecureString",
                                   "value": "abc"
                               }
                           }
                       }
                   },
                   "packageConnectionManagers": {
                       "MyOledbCM": {
                           "userName": {
                               "value": {
                                   "value": "@pipeline().parameters.MyUsername",
                                   "type": "Expression"
                               }
                           },
                           "passWord": {
                               "value": {
                                   "type": "AzureKeyVaultSecret",
                                   "store": {
                                       "referenceName": "myAKV",
                                       "type": "LinkedServiceReference"
                                   },
                                   "secretName": "MyPassword",
                                   "secretVersion": "3a1b74e361bf4ef4a00e47053b872149"
                               }
                           }
                       }
                   },
                   "propertyOverrides": {
                       "\\Package.MaxConcurrentExecutables": {
                           "value": 8,
                           "isSensitive": false
                       }
                   }
               },
               "policy": {
                   "timeout": "0.01:00:00",
                   "retry": 0,
                   "retryIntervalInSeconds": 30
               }
           }]
       }
   }
   ```

2. Ga in Azure PowerShell, naar de `C:\ADF\RunSSISPackage` map.

3. Maak de pijplijn **RunSSISPackagePipeline**, voert de **Set AzDataFactoryV2Pipeline** cmdlet.

   ```powershell
   $DFPipeLine = Set-AzDataFactoryV2Pipeline -DataFactoryName $DataFactory.DataFactoryName `
                                                  -ResourceGroupName $ResGrp.ResourceGroupName `
                                                  -Name "RunSSISPackagePipeline"
                                                  -DefinitionFile ".\RunSSISPackagePipeline.json"
   ```

   Hier volgt een voorbeeld van uitvoer:

   ```
   PipelineName      : Adfv2QuickStartPipeline
   ResourceGroupName : <resourceGroupName>
   DataFactoryName   : <dataFactoryName>
   Activities        : {CopyFromBlobToBlob}
   Parameters        : {[inputPath, Microsoft.Azure.Management.DataFactory.Models.ParameterSpecification], [outputPath, Microsoft.Azure.Management.DataFactory.Models.ParameterSpecification]}
   ```

### <a name="run-the-pipeline"></a>De pijplijn uitvoeren
Gebruik de **Invoke-AzDataFactoryV2Pipeline** cmdlet uit te voeren van de pijplijn. De cmdlet retourneert de id voor de pijplijnuitvoering voor toekomstige controle.

```powershell
$RunId = Invoke-AzDataFactoryV2Pipeline -DataFactoryName $DataFactory.DataFactoryName `
                                             -ResourceGroupName $ResGrp.ResourceGroupName `
                                             -PipelineName $DFPipeLine.Name
```

### <a name="monitor-the-pipeline"></a>De pijplijn bewaken

Voer het volgende PowerShell-script uit om continu de status van de pijplijnuitvoering te controleren totdat het kopiëren van de gegevens is voltooid. Kopieer/plak het volgende script in het PowerShell-venster en druk op ENTER. 

```powershell
while ($True) {
    $Run = Get-AzDataFactoryV2PipelineRun -ResourceGroupName $ResGrp.ResourceGroupName `
                                               -DataFactoryName $DataFactory.DataFactoryName `
                                               -PipelineRunId $RunId

    if ($Run) {
        if ($run.Status -ne 'InProgress') {
            Write-Output ("Pipeline run finished. The status is: " +  $Run.Status)
            $Run
            break
        }
        Write-Output  "Pipeline is running...status: InProgress"
    }

    Start-Sleep -Seconds 10
}   
```

U kunt ook de pijplijn met behulp van de Azure portal controleren. Zie voor stapsgewijze instructies [bewaken van de pijplijn](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline).

### <a name="schedule-the-pipeline-with-a-trigger"></a>De pijplijn met een trigger plannen
In de vorige stap hebt u de pijplijn op aanvraag uitgevoerd. U kunt ook een planningstrigger voor het uitvoeren van de pijplijn volgens een planning (elk uur, dagelijks, enz.) maken.

1. Maak een JSON-bestand met de naam **MyTrigger.json** in **C:\ADF\RunSSISPackage** map met de volgende inhoud: 

   ```json
   {
       "properties": {
           "name": "MyTrigger",
           "type": "ScheduleTrigger",
           "typeProperties": {
               "recurrence": {
                   "frequency": "Hour",
                   "interval": 1,
                   "startTime": "2017-12-07T00:00:00-08:00",
                   "endTime": "2017-12-08T00:00:00-08:00"
               }
           },
           "pipelines": [{
               "pipelineReference": {
                   "type": "PipelineReference",
                   "referenceName": "RunSSISPackagePipeline"
               },
               "parameters": {}
           }]
       }
   }    
   ```
2. In **Azure PowerShell**, Ga naar de **C:\ADF\RunSSISPackage** map.
3. Voer de **Set AzDataFactoryV2Trigger** cmdlet, waarmee de trigger wordt gemaakt. 

   ```powershell
   Set-AzDataFactoryV2Trigger -ResourceGroupName $ResGrp.ResourceGroupName `
                                   -DataFactoryName $DataFactory.DataFactoryName `
                                   -Name "MyTrigger" -DefinitionFile ".\MyTrigger.json"
   ```
4. Standaard wordt de trigger is gestopt. De trigger starten door het uitvoeren van de **Start AzDataFactoryV2Trigger** cmdlet. 

   ```powershell
   Start-AzDataFactoryV2Trigger -ResourceGroupName $ResGrp.ResourceGroupName `
                                     -DataFactoryName $DataFactory.DataFactoryName `
                                     -Name "MyTrigger" 
   ```
5. Bevestig dat de trigger is gestart door het uitvoeren van de **Get-AzDataFactoryV2Trigger** cmdlet. 

   ```powershell
   Get-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName `
                                   -DataFactoryName $DataFactoryName `
                                   -Name "MyTrigger"     
   ```    
6. Voer de volgende opdracht uit na het volgende uur. Bijvoorbeeld, als de huidige tijd 3:25 uur UTC is, voer de opdracht uit om 16: 00 UTC. 
    
   ```powershell
   Get-AzDataFactoryV2TriggerRun -ResourceGroupName $ResourceGroupName `
                                      -DataFactoryName $DataFactoryName `
                                      -TriggerName "MyTrigger" `
                                      -TriggerRunStartedAfter "2017-12-06" `
                                      -TriggerRunStartedBefore "2017-12-09"
   ```

   U kunt de volgende query uitvoeren op basis van de SSISDB-database in uw Azure SQL-server om te controleren of het pakket dat wordt uitgevoerd. 

   ```sql
   select * from catalog.executions
   ```

## <a name="next-steps"></a>Volgende stappen
Zie het volgende blogbericht:
-   [Moderniseer en uitbreiden van uw ETL/ELT-werkstromen met SSIS-activiteiten in ADF pijplijnen](https://blogs.msdn.microsoft.com/ssis/2018/05/23/modernize-and-extend-your-etlelt-workflows-with-ssis-activities-in-adf-pipelines/)
