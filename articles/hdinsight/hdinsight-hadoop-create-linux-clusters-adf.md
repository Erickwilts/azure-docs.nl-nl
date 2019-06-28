---
title: 'Zelfstudie: Apache Hadoop-clusters op aanvraag maken in Azure HDInsight met Data Factory '
description: 'Zelfstudie: informatie over het maken van on-demand Apache Hadoop-clusters in HDInsight met behulp van Azure Data Factory.'
author: hrasheed-msft
ms.reviewer: jasonh
ms.author: hrasheed
ms.service: hdinsight
ms.topic: tutorial
ms.date: 04/18/2019
ms.openlocfilehash: 64f016ac0fa572cb8cf8504902108cffae267cec
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67293285"
---
# <a name="tutorial-create-on-demand-apache-hadoop-clusters-in-hdinsight-using-azure-data-factory"></a>Zelfstudie: Op aanvraag Apache Hadoop-clusters in HDInsight met behulp van Azure Data Factory maken
[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

In dit artikel leert u over het maken van een [Apache Hadoop](https://hadoop.apache.org/) cluster, on-demand, in Azure HDInsight met behulp van Azure Data Factory. Vervolgens gebruikt u gegevenspijplijnen in Azure Data Factory Hive-taken uitvoeren en verwijderen van het cluster. Aan het einde van deze zelfstudie leert u hoe u voor het operationeel maken van een big data-taak uitgevoerd waar het cluster te maken, taak uitvoeren en verwijderen van de cluster worden uitgevoerd volgens een schema.

Deze zelfstudie bestaat uit de volgende taken: 

> [!div class="checklist"]
> * Een Azure-opslagaccount maken
> * Inzicht in Azure Data Factory-activiteit
> * Een data factory maken met Azure portal
> * Gekoppelde services maken
> * Een pijplijn maken
> * Een pijplijn activeren
> * Een pijplijn bewaken
> * De uitvoer controleren

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

* De PowerShell [Az Module](https://docs.microsoft.com/powershell/azure/overview) geïnstalleerd.

* Een Azure Active Directory-service-principal. Als u de service-principal hebt gemaakt, moet u om op te halen de **toepassings-ID** en **verificatiesleutel** met behulp van de instructies in het gekoppelde artikel. U hebt deze waarden later in deze zelfstudie nodig. Controleer ook of de service-principal is lid van de *Inzender* rol van het abonnement of de resourcegroep waarin het cluster is gemaakt. Zie voor instructies voor het ophalen van de vereiste waarden en de juiste rollen toewijzen [maken van een Azure Active Directory service-principal](../active-directory/develop/howto-create-service-principal-portal.md).

## <a name="create-preliminary-azure-objects"></a>Voorlopige Azure objecten maken

In deze sectie maakt u diverse objecten die worden gebruikt voor het maken van on-demand HDInsight-cluster. Het opslagaccount bevat het voorbeeld [HiveQL](https://cwiki.apache.org/confluence/display/Hive/LanguageManual) script (`hivescript.hql`) dat u gebruikt voor het simuleren van een voorbeeld van een [Apache Hive](https://hive.apache.org/) taak die wordt uitgevoerd op het cluster.

Deze sectie wordt een Azure PowerShell-script voor het maken van de storage-account en kopiëren via de vereiste bestanden in de storage-account. De Azure PowerShell-voorbeeldscript in deze sectie worden de volgende taken uitgevoerd:

1. Als u zich aanmeldt bij Azure.
2. Hiermee maakt u een Azure-resourcegroep.
3. Hiermee maakt u een Azure-opslagaccount.
4. Hiermee maakt u een Blob-container in de storage-account
5. Kopieert het voorbeeld HiveQL-script (**hivescript.hql**) de Blob-container. Het script is beschikbaar op [ https://hditutorialdata.blob.core.windows.net/adfv2hiveactivity/hivescripts/hivescript.hql ](https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql). Het voorbeeldscript is al beschikbaar in een andere openbare Blob-container. Het onderstaande PowerShell-script maakt een kopie van deze bestanden in de Azure Storage-account die wordt gemaakt.

> [!WARNING]  
> Type opslagaccount `BlobStorage` kan niet worden gebruikt voor HDInsight-clusters.

**Een storage-account maken en kopieer de bestanden met behulp van Azure PowerShell:**

> [!IMPORTANT]  
> Geef namen voor de Azure-resourcegroep en de Azure storage-account dat door het script wordt gemaakt.
> Noteer **groepsnaam voor accountresources**, **opslagaccountnaam**, en **opslagaccountsleutel** output door het script. U moet deze in de volgende sectie.

```powershell
$resourceGroupName = "<Azure Resource Group Name>"
$storageAccountName = "<Azure Storage Account Name>"
$location = "East US"

$sourceStorageAccountName = "hditutorialdata"  
$sourceContainerName = "adfv2hiveactivity"

$destStorageAccountName = $storageAccountName
$destContainerName = "adfgetstarted" # don't change this value.

####################################
# Connect to Azure
####################################
#region - Connect to Azure subscription
Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
$sub = Get-AzSubscription -ErrorAction SilentlyContinue
if(-not($sub))
{
    Connect-AzAccount
}
#endregion

####################################
# Create a resource group, storage, and container
####################################

#region - create Azure resources
Write-Host "`nCreating resource group, storage account and blob container ..." -ForegroundColor Green

New-AzResourceGroup `
    -Name $resourceGroupName `
    -Location $location

New-AzStorageAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $destStorageAccountName `
    -Kind StorageV2 `
    -Location $location `
    -SkuName Standard_LRS `
    -EnableHttpsTrafficOnly 1

$destStorageAccountKey = (Get-AzStorageAccountKey `
    -ResourceGroupName $resourceGroupName `
    -Name $destStorageAccountName)[0].Value

$sourceContext = New-AzStorageContext `
    -StorageAccountName $sourceStorageAccountName `
    -Anonymous

$destContext = New-AzStorageContext `
    -StorageAccountName $destStorageAccountName `
    -StorageAccountKey $destStorageAccountKey

New-AzStorageContainer `
    -Name $destContainerName `
    -Context $destContext
#endregion

####################################
# Copy files
####################################
#region - copy files
Write-Host "`nCopying files ..." -ForegroundColor Green

$blobs = Get-AzStorageBlob `
    -Context $sourceContext `
    -Container $sourceContainerName

$blobs|Start-AzStorageBlobCopy `
    -DestContext $destContext `
    -DestContainer $destContainerName

Write-Host "`nCopied files ..." -ForegroundColor Green
Get-AzStorageBlob `
    -Context $destContext `
    -Container $destContainerName
#endregion

Write-host "`nYou will use the following values:" -ForegroundColor Green
write-host "`nResource group name: $resourceGroupName"
Write-host "Storage Account Name: $destStorageAccountName"
write-host "Storage Account Key: $destStorageAccountKey"

Write-host "`nScript completed" -ForegroundColor Green
```

**Om te controleren of het opslagaccount is gemaakt**

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **resourcegroepen** in het linkerdeelvenster.
3. Selecteer de Resourcegroepnaam die u hebt gemaakt in uw PowerShell-script. Als er te veel resourcegroepen die worden vermeld, gebruikt u het filter.
4. Op de **Resources** tegel, ziet u een resource in de lijst, tenzij u de resourcegroep met andere projecten delen. Deze resource is de storage-account met de naam die u eerder hebt opgegeven. Selecteer de naam van het opslagaccount.
5. Selecteer de **Blobs** tegels.
6. Selecteer de **adfgetstarted** container. Ziet u een map met de naam **hivescripts**.
7. Open de map en zorg ervoor dat deze de voorbeeld-scriptbestand bevat **hivescript.hql**.

## <a name="understand-the-azure-data-factory-activity"></a>Inzicht in de Azure Data Factory-activiteit

[Azure Data Factory](../data-factory/introduction.md) wordt georganiseerd en de verplaatsing en transformatie van gegevens worden geautomatiseerd. Azure Data Factory kunt maken van een HDInsight Hadoop-cluster just-in-time voor het verwerken van een segment invoergegevens en verwijderen van het cluster wanneer de verwerking voltooid is. 

In Azure Data Factory hebben een data factory een of meer pijplijnen. Een pijplijn heeft één of meer activiteiten. Er zijn twee soorten activiteiten:

- [Activiteiten voor gegevensverplaatsing](../data-factory/copy-activity-overview.md) -gebruik van activiteiten voor gegevensverplaatsing om gegevens te verplaatsen van een brongegevensarchief naar een doelgegevensarchief.
- [Activiteiten voor gegevenstransformatie](../data-factory/transform-data.md). Kunt u activiteiten voor gegevenstransformatie gegevens transformeren en verwerken. HDInsight Hive-activiteit is een van de activiteiten voor gegevenstransformatie ondersteund door Data Factory. U de Hive-transformatie-activiteit gebruiken in deze zelfstudie.

In dit artikel configureert u de Hive-activiteit voor het maken van een on-demand HDInsight Hadoop-cluster. Wanneer de activiteit wordt uitgevoerd om gegevens te verwerken, is dit wat er gebeurt:

1. Een HDInsight Hadoop-cluster wordt automatisch gemaakt voor u just-in-time voor het verwerken van het segment. 

2. De ingevoerde gegevens worden verwerkt door een HiveQL-script uitgevoerd op het cluster. In deze zelfstudie worden de volgende acties uitgevoerd door het HiveQL-script dat is gekoppeld aan het hive-activiteit:

    - Maakt gebruik van de bestaande tabel (*hivesampletable*) te maken van een andere tabel **HiveSampleOut**.
    - Vult de **HiveSampleOut** tabel met alleen bepaalde kolommen uit de oorspronkelijke *hivesampletable*.
    
3. Het HDInsight Hadoop-cluster wordt verwijderd nadat de verwerking voltooid is en het cluster niet actief voor de geconfigureerde hoeveelheid tijd (timeToLive-instelling is). Als het volgende gegevenssegment voor verwerking met deze timeToLive niet-actieve tijd beschikbaar is, wordt hetzelfde cluster wordt gebruikt voor het verwerken van het segment.  

## <a name="create-a-data-factory"></a>Een gegevensfactory maken

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

2. In het menu links, gaat u naar **+ een resource maken** > **Analytics** > **Data Factory**.

    ![Azure Data Factory in de portal](./media/hdinsight-hadoop-create-linux-clusters-adf/data-factory-azure-portal.png "Azure Data Factory in de portal")

3. Typ of Selecteer de volgende waarden voor de **nieuwe data factory** tegel:

    |Eigenschap  |Value  |
    |---------|---------|
    |Name | Voer een naam voor de data factory. Deze naam moet wereldwijd uniek zijn.|
    |Abonnement | Selecteer uw Azure-abonnement. |
    |Resourcegroep | Selecteer **gebruik bestaande** en selecteer vervolgens de resourcegroep die u hebt gemaakt met de PowerShell-script. |
    |Version | Laat op **V2**. |
    |Locatie | De locatie is automatisch ingesteld op de locatie die u hebt opgegeven tijdens het maken van de resourcegroep eerder. Voor deze zelfstudie, de locatie is ingesteld op **VS-Oost**. |

    ![Azure Data Factory maken met Azure portal](./media/hdinsight-hadoop-create-linux-clusters-adf/create-data-factory-portal.png "maken Azure Data Factory met behulp van Azure portal")

4. Selecteer **Maken**. Het maken van een data factory kan duren voordat tussen 2 tot 4 minuten.

5. Nadat de gegevensfactory is gemaakt, ontvangt u een **implementatie is voltooid** melding met een **naar de resource gaan** knop.  Selecteer **naar de resource gaan** om de weergave van de standaard Data Factory te openen.

6. Selecteer **Author & Monitor** om te starten van de Azure Data Factory voor ontwerp en controle van de portal.

    ![Overzicht van Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/data-factory-portal-overview.png "Azure Data Factory-overzicht")

## <a name="create-linked-services"></a>Gekoppelde services maken

In deze sectie maakt maken u twee gekoppelde services in uw data factory.

- Een **gekoppelde Azure Storage-service** waarmee een Azure-opslagaccount wordt gekoppeld aan de gegevensfactory. Deze opslag wordt gebruikt voor het HDInsight-cluster op aanvraag. Het bevat ook het Hive-script dat wordt uitgevoerd op het cluster.
- Een **gekoppelde HDInsight-service op aanvraag**. Azure Data Factory wordt automatisch een HDInsight-cluster maakt en het Hive-script wordt uitgevoerd. Het HDInsight-cluster wordt vervolgens verwijderd als het cluster gedurende een vooraf geconfigureerde tijd inactief is geweest.

### <a name="create-an-azure-storage-linked-service"></a>Een gekoppelde Azure Storage-service maken

1. In het linkerdeelvenster van de **aan de slag** weergeeft, schakelt de **auteur** pictogram.

    ![Maak een gekoppelde Azure Data Factory-service](./media/hdinsight-hadoop-create-linux-clusters-adf/data-factory-edit-tab.png "een gekoppelde Azure Data Factory-service maken")

2. Selecteer **verbindingen** in de linkerbenedenhoek van het venster en selecteer vervolgens **+ nieuw**.

    ![Verbindingen maken in Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/data-factory-create-new-connection.png "verbindingen in Azure Data Factory maken")

3. In de **nieuwe gekoppelde Service** in het dialoogvenster, selecteer **Azure Blob Storage** en selecteer vervolgens **doorgaan**.

    ![Gekoppelde maken Azure Storage-service voor Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-storage-linked-service.png "maken Azure Storage gekoppelde service voor Data Factory")

4. Geef de volgende waarden voor de gekoppelde storage-service:

    |Eigenschap |Value |
    |---|---|
    |Name |Voer `HDIStorageLinkedService` in.|
    |Azure-abonnement |Selecteer uw abonnement in de vervolgkeuzelijst.|
    |Naam van opslagaccount |Selecteer het Azure Storage-account dat u hebt gemaakt als onderdeel van het PowerShell-script.|

    Selecteer vervolgens **Voltooien**.

    ![Geef de naam op voor Azure Storage gekoppelde service](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-storage-linked-service-details.png "Geef de naam op voor Azure Storage gekoppelde service")

### <a name="create-an-on-demand-hdinsight-linked-service"></a>Een gekoppelde HDInsight-service op aanvraag maken

1. Selecteer nogmaals de knop **+ Nieuw** om een andere gekoppelde service te maken.

2. In de **nieuwe gekoppelde Service** venster de **Compute** tabblad.

3. Selecteer **Azure HDInsight**, en selecteer vervolgens **doorgaan**.

    ![Create HDInsight gekoppelde service voor Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-linked-service.png "HDInsight maken gekoppelde service voor Azure Data Factory")

4. In de **nieuwe gekoppelde Service** venster, voer de volgende waarden en laat de rest standaard:

    | Eigenschap | Value |
    | --- | --- |
    | Name | Voer `HDInsightLinkedService` in.|
    | Type | Selecteer **On-demand HDInsight**. |
    | Een gekoppelde Azure Storage-service | Selecteer `HDIStorageLinkedService`. |
    | Clustertype | Selecteer **hadoop** |
    | Time To Live | Geef de duur die u het HDInsight-cluster wilt moet beschikbaar zijn voordat het wordt automatisch verwijderd.|
    | Service-principal-ID | Geef de toepassings-ID van de service-principal voor Azure Active Directory die u hebt gemaakt als onderdeel van de vereisten. |
    | Sleutel van service-principal | Geef de verificatiesleutel voor de Azure Active Directory service-principal. |
    | Het voorvoegsel van cluster | Geef een waarde die wordt voorafgegaan aan de clustertypen die zijn gemaakt door de data factory. |
    |Abonnement |Selecteer uw abonnement in de vervolgkeuzelijst.|
    | Resourcegroep selecteren | Selecteer de resourcegroep die u hebt gemaakt als onderdeel van het PowerShell-script dat u eerder hebt gebruikt.|
    |Regio selecteren | Selecteer een regio in de vervolgkeuzelijst.|
    | OS-type/Cluster SSH-gebruikersnaam | Voer de naam van een SSH-gebruiker meestal `sshuser`. |
    | OS-type/Cluster SSH-wachtwoord | Geef een wachtwoord op voor de SSH-gebruiker |
    | Naam van besturingssysteem clustertype/gebruiker | Voer een gebruikersnaam cluster vaak `admin`. |
    | OS-type/Cluster gebruikerswachtwoord | Een wachtwoord opgeven voor de clustergebruiker. |

    Selecteer vervolgens **Voltooien**.

    ![Geef waarden voor HDInsight gekoppelde service](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-linked-service-details.png "Geef waarden voor HDInsight gekoppelde service")

## <a name="create-a-pipeline"></a>Een pijplijn maken

1. Selecteer de knop **+** (plus) en selecteer vervolgens **Pijplijn**.

    ![Een pijplijn maakt in Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-create-pipeline.png "een pijplijn maakt in Azure Data Factory")

2. In de **activiteiten** werkset Vouw **HDInsight**, en sleep de **Hive** activiteit naar het ontwerpoppervlak voor pijplijnen. In de **algemene** tabblad, Geef een naam op voor de activiteit.

    ![Activiteiten toevoegen aan de Data Factory-pijplijn](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-add-hive-pipeline.png "activiteiten toevoegen aan de Data Factory-pijplijn")

3. Zorg ervoor dat u hebt de Hive-activiteit die is geselecteerd, selecteer de **HDI-Cluster** tabblad, en van de **gekoppelde Service HDInsight** vervolgkeuzelijst, selecteer de gekoppelde service dat u eerder hebt gemaakt,  **HDinightLinkedService**, voor HDInsight.

    ![Geef HDInsight-clusterdetails op voor de pijplijn](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-hive-activity-select-hdinsight-linked-service.png "bieden HDInsight-clusterdetails voor de pijplijn")

4. Selecteer de **Script** tabblad en voer de volgende stappen uit:

    1. Voor **Script gekoppelde Service**, selecteer **HDIStorageLinkedService** uit de vervolgkeuzelijst. Deze waarde is de gekoppelde storage-service die u eerder hebt gemaakt.

    1. Voor **bestandspad**, selecteer **Browse Storage** en navigeer naar de locatie waar de voorbeeld-Hive-script beschikbaar is. Als u eerder hebt uitgevoerd van het PowerShell-script, deze locatie moet zijn `adfgetstarted/hivescripts/hivescript.hql`.

        ![Geef details van de Hive-script voor de pijplijn](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-provide-script-path.png "bieden Hive-script details voor de pijplijn")

    1. Onder **Geavanceerd** > **Parameters**, selecteer **automatisch ingevuld uit het script**. Deze optie ziet er uit voor de parameters waarvoor waarden tijdens runtime in de Hive-script. Het script dat u gebruikt (**hivescript.hql**) heeft een **uitvoer** parameter. Geef de **waarde** in de indeling `wasb://adfgetstarted@<StorageAccount>.blob.core.windows.net/outputfolder/` om te verwijzen naar een bestaande map op uw Azure-opslag. Het pad is hoofdlettergevoelig. Dit is het pad waar u de uitvoer van het script wordt opgeslagen.
    
        ![Geef parameters op voor de Hive-script](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-provide-script-parameters.png "parameters opgeven voor het Hive-script")

1. Selecteer **valideren** voor het valideren van de pijplijn. Selecteer de **>>** (pijl-rechts) om het validatievenster te sluiten.

    ![Valideren van de Azure Data Factory-pijplijn](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-validate-all.png "valideren van de Azure Data Factory-pijplijn")

1. Selecteer ten slotte **Alles publiceren** de artefacten publiceren naar Azure Data Factory.

    ![Publiceren van de Azure Data Factory-pijplijn](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-publish-pipeline.png "publiceren van de Azure Data Factory-pijplijn")

## <a name="trigger-a-pipeline"></a>Een pijplijn activeren

1. Selecteer in de werkbalk op het ontwerpoppervlak voor pijplijnen **toevoegen trigger** > **nu activeren**.

    ![De Azure Data Factory-pijplijn activeren](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-trigger-pipeline.png "de Azure Data Factory-pijplijn activeren")

2. Selecteer **voltooien** in het pop-zijbalk.

## <a name="monitor-a-pipeline"></a>Een pijplijn bewaken

1. Ga naar het tabblad **Controleren** aan de linkerkant. U ziet een pijplijn die worden uitgevoerd in de lijst **Pipeline Runs**. U ziet de status van de uitvoering onder de **Status** kolom.

    ![Bewaken van de Azure Data Factory-pijplijn](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-monitor-pipeline.png "bewaken van de Azure Data Factory-pijplijn")

1. Selecteer **Vernieuwen** om de status te vernieuwen.

1. U kunt ook selecteren de **uitvoeringen van activiteit weergeven** pictogram om te zien van de activiteit die wordt uitgevoerd die is gekoppeld aan de pijplijn. In de onderstaande schermafbeelding ziet u slechts één activiteit die wordt uitgevoerd, omdat er slechts één activiteit in de pijplijn die u hebt gemaakt. Als u wilt overschakelen naar de vorige weergave, selecteert u **pijplijnen** boven aan de pagina.

    ![De Azure Data Factory pipeline-activiteit controleren](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-monitor-pipeline-activity.png "bewaken van de Azure Data Factory pipeline-activiteit")

## <a name="verify-the-output"></a>De uitvoer controleren

1. Om te controleren of de uitvoer, in de Azure-portal gaat u naar het opslagaccount dat u voor deze zelfstudie gebruikt. U ziet de volgende mappen of containers:

    - U ziet een **adfgerstarted/outputfolder** die de uitvoer van de Hive-script is uitgevoerd als onderdeel van de pijplijn bevat.

    - U ziet een **adfhdidatafactory -\<gekoppeld-service-name >-\<tijdstempel >** container. Deze container is de standaardlocatie voor de opslag van het HDInsight-cluster dat is gemaakt als onderdeel van de pijplijnuitvoering.

    - U ziet een **adfjobs** container met de Azure Data Factory-taak zich aanmeldt.  

        ![Controleer of de uitvoer van Azure Data Factory-pijplijn](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-verify-output.png "de uitvoer van Azure Data Factory-pijplijn controleren")

## <a name="clean-up-the-tutorial"></a>De zelfstudie opschonen

Met het on-demand HDInsight-cluster maken hoeft u niet expliciet verwijderen van het HDInsight-cluster. Het cluster is verwijderd op basis van de configuratie die u hebt opgegeven tijdens het maken van de pijplijn. Zelfs nadat het cluster is verwijderd, blijven de storage-accounts die zijn gekoppeld aan het cluster echter bestaan. Dit gedrag is inherent aan het ontwerp, zodat u uw gegevens kunt behouden. Als u niet behouden van de gegevens wilt, kunt u het opslagaccount dat u hebt gemaakt verwijderen.

U kunt ook de hele resourcegroep die u hebt gemaakt voor deze zelfstudie verwijderen. Hiermee verwijdert u het opslagaccount en de Azure Data Factory die u hebt gemaakt.

### <a name="delete-the-resource-group"></a>De resourcegroep verwijderen

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Selecteer **resourcegroepen** in het linkerdeelvenster.
1. Selecteer de Resourcegroepnaam die u hebt gemaakt in uw PowerShell-script. Als er te veel resourcegroepen die worden vermeld, gebruikt u het filter. Hiermee opent u de resourcegroep.
1. Op de **Resources** tegel, u moet het standaardaccount voor opslag en de data factory, tenzij u de resourcegroep met andere projecten delen weergegeven.
1. Selecteer **Resourcegroep verwijderen**. In dat geval worden de storage-account en de gegevens die zijn opgeslagen in het opslagaccount verwijderd.

    ![Resourcegroep verwijderen](./media/hdinsight-hadoop-create-linux-clusters-adf/delete-resource-group.png "resourcegroep verwijderen")

1. Voer de naam van de resourcegroep om te bevestigen en selecteer vervolgens **verwijderen**.


## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe u Azure Data Factory gebruiken om te maken van on-demand HDInsight-cluster en voer [Apache Hive](https://hive.apache.org/) taken. Ga naar het volgende artikel voor meer informatie over het maken van HDInsight-clusters met aangepaste configuratie.

> [!div class="nextstepaction"]
>[Azure HDInsight-clusters maken met aangepaste configuratie](hdinsight-hadoop-provision-linux-clusters.md)


