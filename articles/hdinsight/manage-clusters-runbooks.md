---
title: 'Zelfstudie: Azure Automation-runbooks gebruiken om clusters te maken - Azure HDInsight'
description: Leer hoe u Azure HDInsight-clusters maakt en verwijdert met scripts die worden uitgevoerd in de cloud met behulp van Azure Automation-runbooks.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: tutorial
ms.date: 12/27/2019
ms.openlocfilehash: 05c0aaf6cc33442fa4f36eb38eb0d6d593fc6c1f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "75553511"
---
# <a name="tutorial-create-azure-hdinsight-clusters-with-azure-automation"></a>Zelfstudie: Azure HDInsight-clusters met Azure Automation maken

Met Azure Automation kunt u scripts maken die worden uitgevoerd in de cloud en Azure-resources op aanvraag of op basis van een schema beheren. In dit artikel wordt beschreven hoe u PowerShell-runbooks maakt om Azure HDInsight-clusters te maken en verwijderen.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Modules installeren die nodig zijn voor interactie met HDInsight.
> * Referenties maken en opslaan die nodig zijn tijdens het maken van het cluster.
> * Een nieuw Azure Automation-runbook maken om een HDInsight-cluster te maken.

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

* Een bestaande [Azure Automation-account](../automation/automation-quickstart-create-account.md).
* Een bestaande [Azure Storage-account](../storage/common/storage-account-create.md) die wordt gebruikt als clusteropslag.

## <a name="install-hdinsight-modules"></a>HDInsight-modules installeren

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Selecteer uw Azure Automation-accounts.
1. Selecteer **Galerie met modules** onder **Gedeelde resources**.
1. Typ **AzureRM.Profile** in het vak en druk op enter om te zoeken. Selecteer het beschikbare zoekresultaat.
1. Op het scherm **AzureRM.profile** selecteert u **Importeren**. Schakel het selectievakje in om Azure-modules bij te werken en selecteer vervolgens **OK**.

    ![module AzureRM.profile importeren](./media/manage-clusters-runbooks/import-azurermprofile-module.png)

1. Ga terug naar de modulegalerie door **Modulegalerie** te selecteren onder **Gedeelde resources**.
1. Typ **HDInsight**. Selecteer **AzureRM.HDInsight**.

    ![door HDInsight-modules bladeren](./media/manage-clusters-runbooks/browse-modules-hdinsight.png)

1. In het deelvenster **AzureRM.HDInsight** selecteert u **Importeren** en **OK**.

    ![AzureRM.HDInsight-module importeren](./media/manage-clusters-runbooks/import-azurermhdinsight-module.png)

## <a name="create-credentials"></a>Referenties maken

1. Onder **Gedeelde resources** selecteert u **Referenties**.
1. Selecteer **Een referentie toevoegen**.
1. Voer de vereiste gegevens in het deelvenster **Nieuwe referentie** in. Deze referentie is bedoeld om het wachtwoord van het cluster op te slaan, zodat u zich kunt aanmelden bij Ambari.

    | Eigenschap | Waarde |
    | --- | --- |
    | Naam | `cluster-password` |
    | Gebruikersnaam | `admin` |
    | Wachtwoord | `SECURE_PASSWORD` |
    | Wachtwoord bevestigen | `SECURE_PASSWORD` |

1. Selecteer **Maken**.
1. Herhaal hetzelfde proces voor een nieuwe referentie `ssh-password` met gebruikersnaam `sshuser` en een wachtwoord naar keuze. Selecteer **Maken**. Deze referentie is bedoeld om het SSH-wachtwoord voor uw cluster op te slaan.

    ![referentie maken](./media/manage-clusters-runbooks/create-credentials.png)

## <a name="create-a-runbook-to-create-a-cluster"></a>Een runbook maken om een cluster te maken

1. Selecteer onder **Procesautomatisering** de optie **Runbooks**.
1. Selecteer **Een runbook maken**.
1. Voer op het deelvenster **Een runbook maken** een naam in voor het runbook, zoals `hdinsight-cluster-create`. Selecteer **PowerShell** in de vervolgkeuzelijst **Runbooktype**.
1. Selecteer **Maken**.

    ![runbook maken](./media/manage-clusters-runbooks/create-runbook.png)

1. Voer de volgende code in op het scherm **PowerShell-runbook bewerken** en selecteer **Publiceren**:

    ![runbook publiceren](./media/manage-clusters-runbooks/publish-runbook.png)

    ```powershell
    Param
    (
      [Parameter (Mandatory= $true)]
      [String] $subscriptionID,
    
      [Parameter (Mandatory= $true)]
      [String] $resourceGroup,
    
      [Parameter (Mandatory= $true)]
      [String] $storageAccount,
    
      [Parameter (Mandatory= $true)]
      [String] $containerName,
    
      [Parameter (Mandatory= $true)]
      [String] $clusterName
    )
    ### Authenticate to Azure 
    $Conn = Get-AutomationConnection -Name 'AzureRunAsConnection'
    Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
    
    # Set cluster variables
    $storageAccountKey = (Get-AzureRmStorageAccountKey –Name $storageAccount –ResourceGroupName $resourceGroup)[0].value 
    
    # Setting cluster credentials
    
    #Automation credential for Cluster Admin
    $clusterCreds = Get-AutomationPSCredential –Name 'cluster-password'
    
    #Automation credential for user to SSH into cluster
    $sshCreds = Get-AutomationPSCredential –Name 'ssh-password' 
    
    $clusterType = "Hadoop" #Use any supported cluster type (Hadoop, HBase, Storm, etc.)
    $clusterOS = "Linux"
    $clusterWorkerNodes = 3
    $clusterNodeSize = "Standard_D3_v2"
    $location = Get-AzureRmStorageAccount –StorageAccountName $storageAccount –ResourceGroupName $resourceGroup | %{$_.Location}
    
    ### Provision HDInsight cluster
    New-AzureRmHDInsightCluster –ClusterName $clusterName –ResourceGroupName $resourceGroup –Location $location –DefaultStorageAccountName "$storageAccount.blob.core.windows.net" –DefaultStorageAccountKey $storageAccountKey -DefaultStorageContainer $containerName –ClusterType $clusterType –OSType $clusterOS –Version “3.6” –HttpCredential $clusterCreds –SshCredential $sshCreds –ClusterSizeInNodes $clusterWorkerNodes –HeadNodeSize $clusterNodeSize –WorkerNodeSize $clusterNodeSize
    ```

## <a name="create-a-runbook-to-delete-a-cluster"></a>Een runbook maken om een cluster te verwijderen

1. Selecteer onder **Procesautomatisering** de optie **Runbooks**.
1. Selecteer **Een runbook maken**.
1. Voer op het deelvenster **Een runbook maken** een naam in voor het runbook, zoals `hdinsight-cluster-delete`. Selecteer **PowerShell** in de vervolgkeuzelijst **Runbooktype**.
1. Selecteer **Maken**.
1. Voer de volgende code in op het scherm **PowerShell-runbook bewerken** en selecteer **Publiceren**:

    ```powershell
    Param
    (
      [Parameter (Mandatory= $true)]
      [String] $clusterName
    )
    
    ### Authenticate to Azure 
    $Conn = Get-AutomationConnection -Name 'AzureRunAsConnection'
    Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
    
    Remove-AzureRmHDInsightCluster -ClusterName $clusterName
    ```

## <a name="execute-runbooks"></a>Runbooks uitvoeren

### <a name="create-a-cluster"></a>Een cluster maken

1. Bekijk de lijst met Runbooks voor uw Automation-account door **Runbooks** te selecteren onder **Procesautomatisering**.
1. Selecteer `hdinsight-cluster-create`, of de naam die u hebt gebruikt bij het maken van het runbook voor het maken van het cluster.
1. Selecteer **Start** om het runbook direct uit te voeren. U kunt ook runbooks plannen die periodiek worden uitgevoerd. Zie [Een runbook in Azure Automation plannen](../automation/shared-resources/schedules.md)
1. Voer de vereiste parameters voor het script in en selecteer **OK**. Hiermee maakt u een nieuw HDInsight-cluster met de naam die u hebt opgegeven in de parameter **CLUSTERNAME**.

    ![runbook voor het maken van het cluster uitvoeren](./media/manage-clusters-runbooks/execute-create-runbook.png)

### <a name="delete-a-cluster"></a>Een cluster verwijderen

Verwijder het cluster door het `hdinsight-cluster-delete`-runbook te selecteren dat u hebt gemaakt. Selecteer **Start**, voer de parameter **CLUSTERNAME** in en selecteer **OK**.

## <a name="clean-up-resources"></a>Resources opschonen

Als u de Azure Automation-account die is gemaakt om onbedoelde kosten te voorkomen niet meer nodig hebt, verwijdert u deze. Als u dit wilt doen, gaat u naar de Azure Portal, selecteert u de resourcegroep waarin u de Azure Automation-account hebt gemaakt, selecteert u de Automation-account en selecteert u vervolgens **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Apache Hadoop-clusters in HDInsight beheren met behulp van Azure PowerShell](hdinsight-administer-use-powershell.md)