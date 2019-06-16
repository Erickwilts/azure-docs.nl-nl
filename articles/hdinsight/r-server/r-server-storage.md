---
title: Azure Storage-oplossingen voor ML-Services op HDInsight - Azure
description: Meer informatie over de verschillende opslagopties met ML-Services op HDInsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/27/2018
ms.openlocfilehash: cb0c350df3056636701b5ff5d3962e2a0e96f40d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64696355"
---
# <a name="azure-storage-solutions-for-ml-services-on-azure-hdinsight"></a>Azure Storage-oplossingen voor ML-Services op Azure HDInsight

ML-Services op HDInsight kunt u een verscheidenheid aan oplossingen voor opslag gebruiken om vast te leggen van gegevens, code of objecten die bevatten de resultaten van analyse. Deze omvatten de volgende opties:

- [Azure Blob](https://azure.microsoft.com/services/storage/blobs/)
- [Azure Data Lake Storage](https://azure.microsoft.com/services/storage/data-lake-storage/)
- [Azure File storage](https://azure.microsoft.com/services/storage/files/)

U hebt ook de mogelijkheid om toegang tot meerdere Azure storage-accounts of containers met uw HDInsight-cluster. Azure File storage is een handige gegevens voor de opslagoptie voor gebruik op het edge-knooppunt waarmee u een Azure Storage file share koppelen voor, bijvoorbeeld, het Linux-bestandssysteem. Maar Azure-bestandsshares kunnen worden gekoppeld en die worden gebruikt door een systeem met een ondersteund besturingssysteem, zoals Windows of Linux. 

Wanneer u een Apache Hadoop-cluster in HDInsight maakt, u opgeven of een **Azure storage** account of **Data Lake Storage**. Een specifieke storage-container uit dat account bevat het bestandssysteem voor het cluster dat u (bijvoorbeeld, de Hadoop Distributed File System maakt). Zie voor meer informatie over en richtlijnen:

- [Azure storage gebruiken met HDInsight](../hdinsight-hadoop-use-blob-storage.md)
- [Data Lake Storage gebruiken met Azure HDInsight-clusters](../hdinsight-hadoop-use-data-lake-store.md)

## <a name="use-azure-blob-storage-accounts-with-ml-services-cluster"></a>Azure Blob storage-accounts met ML-Services-cluster gebruiken

Als u meer dan één opslagaccount opgegeven bij het maken van uw cluster ML-Services, heeft de volgende instructies wordt uitgelegd hoe u een secundaire account gebruiken voor toegang tot gegevens en bewerkingen op een cluster ML-Services worden weergegeven. Wordt ervan uitgegaan dat de volgende storage-accounts en -container: **storage1** en een standaard-container met de naam **container1**, en **storage2** met **container2**.

> [!WARNING]  
> Voor testdoeleinden, prestaties, is het HDInsight-cluster gemaakt in hetzelfde Datacenter als het primaire opslagaccount die u opgeeft. Met behulp van een storage-account in een andere locatie dan het HDInsight-cluster wordt niet ondersteund.

### <a name="use-the-default-storage-with-ml-services-on-hdinsight"></a>Gebruik de standaardopslag met ML-Services op HDInsight

1. Met behulp van een SSH-client, verbinding maken met het edge-knooppunt van het cluster. Zie voor meer informatie over het gebruik van SSH met HDInsight-clusters [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).
  
2. Een voorbeeldbestand, mysamplefile.csv, kopiëren naar de map/share. 

        hadoop fs –mkdir /share
        hadoop fs –copyFromLocal mycsv.scv /share  

3. Ga naar de R Studio of een andere R-console, en om te stellen van de naam van knooppunt voor R-code schrijven **standaard** en de locatie van het bestand dat u wilt openen.  

        myNameNode <- "default"
        myPort <- 0

        #Location of the data:  
        bigDataDirRoot <- "/share"  

        #Define Spark compute context:
        mySparkCluster <- RxSpark(nameNode=myNameNode, consoleOutput=TRUE)

        #Set compute context:
        rxSetComputeContext(mySparkCluster)

        #Define the Hadoop Distributed File System (HDFS) file system:
        hdfsFS <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)

        #Specify the input file to analyze in HDFS:
        inputFile <-file.path(bigDataDirRoot,"mysamplefile.csv")

Alle verwijzingen naar map- en wijst u naar het opslagaccount dat `wasb://container1@storage1.blob.core.windows.net`. Dit is de **storage-account standaard** die is gekoppeld aan het HDInsight-cluster.

### <a name="use-the-additional-storage-with-ml-services-on-hdinsight"></a>De extra opslagruimte met ML-Services op HDInsight gebruiken

Stel nu dat u wilt verwerken van een bestand met de naam mysamplefile1.csv die zich in de /private map van **container2** in **storage2**.

In uw R-code, wijst u de naam van knooppunt-verwijzing naar de **storage2** storage-account.

    myNameNode <- "wasb://container2@storage2.blob.core.windows.net"
    myPort <- 0

    #Location of the data:
    bigDataDirRoot <- "/private"

    #Define Spark compute context:
    mySparkCluster <- RxSpark(consoleOutput=TRUE, nameNode=myNameNode, port=myPort)

    #Set compute context:
    rxSetComputeContext(mySparkCluster)

    #Define HDFS file system:
    hdfsFS <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)

    #Specify the input file to analyze in HDFS:
    inputFile <-file.path(bigDataDirRoot,"mysamplefile1.csv")

Alle van de map- en verwijzingen verwijst nu naar het opslagaccount dat `wasb://container2@storage2.blob.core.windows.net`. Dit is de **naam knooppunt** die u hebt opgegeven.

U moet configureren de `/user/RevoShare/<SSH username>` map op **storage2** als volgt:


    hadoop fs -mkdir wasb://container2@storage2.blob.core.windows.net/user
    hadoop fs -mkdir wasb://container2@storage2.blob.core.windows.net/user/RevoShare
    hadoop fs -mkdir wasb://container2@storage2.blob.core.windows.net/user/RevoShare/<RDP username>

## <a name="use-azure-data-lake-storage-with-ml-services-cluster"></a>Azure Data Lake Storage gebruiken met ML-Services-cluster 

Voor het gebruik van Data Lake-opslag met uw HDInsight-cluster, moet u uw cluster toegang geven tot de Azure Data Lake-opslag die u wilt gebruiken. Zie voor instructies over het gebruik van de Azure-portal een HDInsight-cluster maken met een Azure Data Lake Storage-account als de standaardopslag of als extra opslag [een HDInsight-cluster maken met Data Lake Storage met behulp van Azure portal](../../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).

Vervolgens gebruikt u de opslag in uw R-script veel zoals u een secundaire Azure-opslagaccount hebt gedaan, zoals beschreven in de vorige procedure.

### <a name="add-cluster-access-to-your-azure-data-lake-storage"></a>Toegang tot het cluster toevoegen aan uw Azure Data Lake Storage
U toegang tot Data Lake-opslag met behulp van een Service-Principal voor Azure Active Directory (Azure AD) die is gekoppeld aan uw HDInsight-cluster.

1. Wanneer u uw HDInsight-cluster maakt, selecteert u **Cluster AAD-identiteit** uit de **gegevensbron** tabblad.

2. In de **Cluster AAD-identiteit** dialoogvenster onder **AD-Service-Principal selecteren**, selecteer **nieuw**.

Nadat u de Service-Principal een naam geven en een wachtwoord voor het maken, klikt u op **ADLS-toegang beheren** de Service-Principal koppelen aan uw Data Lake-opslag.

Het is ook mogelijk toegang tot het cluster toevoegen aan een of meer Data Lake Storage-accounts maken van een cluster te volgen. Open de Azure portal-vermelding voor een Data Lake-opslag en Ga naar **Data Explorer > toegang > toevoegen**. 

### <a name="how-to-access-data-lake-storage-gen1-from-ml-services-on-hdinsight"></a>Toegang tot Data Lake Storage Gen1 van ML-Services op HDInsight

Nadat u toegang tot Data Lake Storage Gen1 gegeven hebt, kunt u de opslag in ML-Services-cluster in HDInsight de manier waarop u een secundaire Azure-opslagaccount dat zou doen. Het enige verschil is dat het voorvoegsel **wasb: / /** wordt gewijzigd in **adl: / /** als volgt:


    # Point to the ADL Storage (e.g. ADLtest)
    myNameNode <- "adl://rkadl1.azuredatalakestore.net"
    myPort <- 0

    # Location of the data (assumes a /share directory on the ADL account)
    bigDataDirRoot <- "/share"  

    # Define Spark compute context
    mySparkCluster <- RxSpark(consoleOutput=TRUE, nameNode=myNameNode, port=myPort)

    # Set compute context
    rxSetComputeContext(mySparkCluster)

    # Define HDFS file system
    hdfsFS <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)

    # Specify the input file in HDFS to analyze
    inputFile <-file.path(bigDataDirRoot,"mysamplefile.csv")

De volgende opdrachten worden gebruikt voor het Data Lake Storage Gen1-account configureren met de map RevoShare en het toevoegen van het CSV-voorbeeldbestand uit het vorige voorbeeld:


    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user
    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user/RevoShare
    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user/RevoShare/<user>

    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/share

    hadoop fs -copyFromLocal /usr/lib64/R Server-7.4.1/library/RevoScaleR/SampleData/mysamplefile.csv adl://rkadl1.azuredatalakestore.net/share

    hadoop fs –ls adl://rkadl1.azuredatalakestore.net/share


## <a name="use-azure-file-storage-with-ml-services-on-hdinsight"></a>Azure File storage gebruiken met ML-Services op HDInsight

Er is ook een handige gegevens voor de opslagoptie voor gebruik op het edge-knooppunt met de naam [Azure Files](https://azure.microsoft.com/services/storage/files/). Hiermee kunt u een Azure Storage-bestandsshare naar het Linux-bestandssysteem koppelen. Deze optie is handig voor het opslaan van gegevensbestanden, R-scripts en resultaatobjecten die mogelijk ook later nodig zijn, met name wanneer het verstandig om het gebruik van de systeemeigen bestandssysteem op het edge-knooppunt in plaats van HDFS. 

Een groot voordeel van Azure Files is dat de bestandsshares kunnen worden gekoppeld en die worden gebruikt door een systeem met een ondersteund besturingssysteem, zoals Windows of Linux. Het kan bijvoorbeeld worden gebruikt door een andere HDInsight-cluster waaraan u of iemand in uw team, door een Azure-VM of zelfs door een on-premises systeem. Zie voor meer informatie:

- [Azure File Storage gebruiken met Linux](../../storage/files/storage-how-to-use-files-linux.md)
- [Azure File storage in Windows gebruiken](../../storage/files/storage-dotnet-how-to-use-files.md)


## <a name="next-steps"></a>Volgende stappen

* [Overzicht van Services voor ML-cluster in HDInsight](r-server-overview.md)
* [Aan de slag met ML-Services-cluster op Apache Hadoop](r-server-get-started.md)
* [Opties voor compute-context voor ML Services-cluster in HDInsight](r-server-compute-contexts.md)
* [Azure Data Lake Storage Gen2 gebruiken met Azure HDInsight-clusters](../hdinsight-hadoop-use-data-lake-storage-gen2.md)
