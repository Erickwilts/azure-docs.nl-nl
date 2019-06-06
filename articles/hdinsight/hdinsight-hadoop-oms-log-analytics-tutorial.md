---
title: Gebruik Azure Monitor-logboeken voor het bewaken van Azure HDInsight-clusters
description: Informatie over het gebruik van Azure Monitor-logboeken voor het bewaken van taken die worden uitgevoerd in een HDInsight-cluster.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: 16659a335ef6126e75f5a9a99784e71afa056bef
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66479260"
---
# <a name="use-azure-monitor-logs-to-monitor-hdinsight-clusters"></a>Gebruik Azure Monitor-logboeken voor het controleren van HDInsight-clusters

Meer informatie over het inschakelen van Azure Monitor-logboeken voor het bewaken van bewerkingen voor Hadoop-cluster in HDInsight en het toevoegen van een HDInsight voor controle.

[Logboeken in Azure Monitor](../log-analytics/log-analytics-overview.md) is een service in Azure Monitor die uw cloud en on-premises omgevingen voor het onderhouden van hun beschikbaarheid en prestaties. De service verzamelt gegevens afkomstig van resources in uw cloud- en on-premises omgevingen en van andere bewakingsprogramma's om analyse over meerdere resources aan te bieden.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

* **Een Log Analytics-werkruimte**. U kunt deze werkruimte zien als een unieke omgeving van Azure Monitor zich aanmeldt met een eigen gegevensopslagplaats, gegevensbronnen en oplossingen. Zie voor instructies [een Log Analytics-werkruimte maken](../azure-monitor/learn/quick-collect-azurevm.md#create-a-workspace).

* **Een Azure HDInsight-cluster**. Op dit moment kunt u logboeken van Azure Monitor met de volgende typen van de HDInsight-cluster:

  * Hadoop
  * HBase
  * Interactive Query
  * Kafka
  * Spark
  * Storm

  Zie voor instructies over het maken van een HDInsight-cluster [aan de slag met Azure HDInsight](hadoop/apache-hadoop-linux-tutorial-get-started.md).  

* **Azure PowerShell Az-module**.  Zie [Maak kennis met de nieuwe module voor Azure PowerShell Az](https://docs.microsoft.com/powershell/azure/new-azureps-module-az).

> [!NOTE]  
> Het verdient aanbeveling om zowel het HDInsight-cluster en de Log Analytics-werkruimte in dezelfde regio voor betere prestaties. Logboeken in Azure Monitor is niet beschikbaar in alle Azure-regio's.

## <a name="enable-azure-monitor-logs-by-using-the-portal"></a>Logboeken van Azure Monitor inschakelen met behulp van de portal

In deze sectie configureert u een bestaand HDInsight Hadoop-cluster voor het gebruik van een Azure Log Analytics-werkruimte voor het bewaken van taken, logboeken voor foutopsporing, enz.

1. Uit de [Azure-portal](https://portal.azure.com/), selecteert u uw cluster.  Zie [clusters tonen en vermelden](./hdinsight-administer-use-portal-linux.md#showClusters) voor instructies. Het cluster wordt in een nieuwe portal-pagina geopend.

1. Aan de linkerkant, onder **bewaking**, selecteer **Operations Management Suite**.

1. In de hoofdweergave onder **OMS-controle**, selecteer **inschakelen**.

1. Uit de **een werkruimte selecteren** vervolgkeuzelijst, selecteert u een bestaande Log Analytics-werkruimte.

1. Selecteer **Opslaan**.  Het duurt een paar minuten om op te slaan van de instelling.

    ![Schakel de bewaking voor HDInsight-clusters](./media/hdinsight-hadoop-oms-log-analytics-tutorial/hdinsight-enable-monitoring.png "Schakel bewaking voor HDInsight-clusters")

## <a name="enable-azure-monitor-logs-by-using-azure-powershell"></a>Logboeken van Azure Monitor inschakelen met behulp van Azure PowerShell

U kunt Azure Monitor-logboeken met behulp van de Az van Azure PowerShell-module inschakelen [inschakelen AzHDInsightOperationsManagementSuite](https://docs.microsoft.com/powershell/module/az.hdinsight/enable-azhdinsightoperationsmanagementsuite) cmdlet.

```powershell
# Enter user information
$resourceGroup = "<your-resource-group>"
$cluster = "<your-cluster>"
$LAW = "<your-Log-Analytics-workspace>"
# End of user input

# obtain workspace id for defined Log Analytics workspace
$WorkspaceId = (Get-AzOperationalInsightsWorkspace -ResourceGroupName $resourceGroup -Name $LAW).CustomerId

# obtain primary key for defined Log Analytics workspace
$PrimaryKey = (Get-AzOperationalInsightsWorkspace -ResourceGroupName $resourceGroup -Name $LAW | Get-AzOperationalInsightsWorkspaceSharedKeys).PrimarySharedKey

# Enables Operations Management Suite
Enable-AzHDInsightOperationsManagementSuite -ResourceGroupName $resourceGroup -Name $cluster -WorkspaceId $WorkspaceId -PrimaryKey $PrimaryKey
```

Om uit te schakelen, het gebruik de [uitschakelen AzHDInsightOperationsManagementSuite](https://docs.microsoft.com/powershell/module/az.hdinsight/disable-azhdinsightoperationsmanagementsuite) cmdlet:

```powershell
Disable-AzHDInsightOperationsManagementSuite -Name "<your-cluster>"
```

## <a name="install-hdinsight-cluster-management-solutions"></a>Installeren van HDInsight-cluster-beheeroplossingen

HDInsight biedt van de cluster-specifieke beheeroplossingen die u voor Azure Monitor-logboeken toevoegen kunt. [Beheeroplossingen](../log-analytics/log-analytics-add-solutions.md) functionaliteit toevoegen aan Azure Monitor-Logboeken, aanvullende gegevens en analysehulpprogramma's bieden. Deze oplossingen belangrijke prestatiegegevens verzamelen van uw HDInsight-clusters en over de hulpprogramma's om te zoeken naar de metrische gegevens. Deze oplossingen bieden ook visualisaties en dashboards voor de meeste clustertypen in HDInsight wordt ondersteund. Met behulp van de metrische gegevens die u verzamelt van de oplossing, kunt u aangepaste regels voor bewaking en waarschuwingen kunt maken.

Dit zijn de beschikbare HDInsight-oplossingen:

* HDInsight Hadoop-bewaking
* HDInsight HBase-bewaking
* HDInsight Interactive Query-bewaking
* HDInsight Kafka-bewaking
* HDInsight Spark-bewaking
* HDInsight Storm-bewaking

Zie voor instructies voor het installeren van een oplossing voor [oplossingen in Azure](../azure-monitor/insights/solutions.md#install-a-monitoring-solution). Als u wilt experimenteren, een oplossing voor bewaking van HDInsight Hadoop te installeren. Wanneer dit is voltooid, ziet u een **HDInsightHadoop** tegel vermeld onder **samenvatting**. Selecteer de **HDInsightHadoop** tegel. De oplossing HDInsightHadoop ziet eruit zoals:

![Bewakingsweergave oplossing voor HDInsight](media/hdinsight-hadoop-oms-log-analytics-tutorial/hdinsight-oms-hdinsight-hadoop-monitoring-solution.png)

Omdat het cluster een nieuwe cluster is, weergeven niet alle activiteiten in het rapport.

## <a name="next-steps"></a>Volgende stappen

* [Query Azure Monitor-logboeken voor het controleren van HDInsight-clusters](hdinsight-hadoop-oms-log-analytics-use-queries.md)
