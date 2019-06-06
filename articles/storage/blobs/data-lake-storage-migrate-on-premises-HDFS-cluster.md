---
title: Gebruik Azure Data Box voor het migreren van gegevens van on-premises HDFS opslaan naar Azure Storage
description: Gegevens migreren van een on-premises HDFS gegevensopslag naar Azure Storage
services: storage
author: normesta
ms.service: storage
ms.date: 06/05/2019
ms.author: normesta
ms.topic: article
ms.component: data-lake-storage-gen2
ms.openlocfilehash: 9a42135df38cde91cc6626a3f7d0328334af0a5d
ms.sourcegitcommit: 1aefdf876c95bf6c07b12eb8c5fab98e92948000
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/06/2019
ms.locfileid: "66729042"
---
# <a name="use-azure-data-box-to-migrate-data-from-an-on-premises-hdfs-store-to-azure-storage"></a>Azure Data Box gebruiken om gegevens te migreren vanuit een on-premises HDFS gegevensopslag naar Azure Storage

U kunt gegevens migreren van een on-premises HDFS gegevensopslag van uw Hadoop-cluster in Azure Storage (blob-opslag of Data Lake Storage Gen2) met behulp van een Data Box-apparaat. U kunt kiezen uit een 80 TB Data Box of een grote 770 TB Data Box.

Dit artikel helpt u deze taken uit te voeren:

:heavy_check_mark: Uw gegevens kopiëren naar een Data Box of een Data Box zware apparaat.

:heavy_check_mark: Het apparaat terug naar Microsoft verzenden.

:heavy_check_mark: De gegevens naar uw Data Lake Storage Gen2 storage-account verplaatsen.

## <a name="prerequisites"></a>Vereisten

U moet deze dingen om de migratie te voltooien.

* Een Azure Storage-account dat **niet** hiërarchische naamruimten die zijn ingeschakeld op deze hebben.

* Als u wilt gebruiken van Azure Data Lake Storage Gen2 account (een opslagaccount **heeft** hiërarchische naamruimten die zijn ingeschakeld op deze hebt), en vervolgens kunt u op dit moment maken.

* Een on-premises Hadoop-cluster met de brongegevens.

* Een [Azure Data Box-apparaat](https://azure.microsoft.com/services/storage/databox/).

    - [Uw Data Box bestellen](https://docs.microsoft.com/azure/databox/data-box-deploy-ordered) of [Data Box-zwaar](https://docs.microsoft.com/azure/databox/data-box-heavy-deploy-ordered). Tijdens het ordenen van uw apparaat, vergeet niet dat een opslagaccount kiezen **niet** hiërarchische naamruimten die zijn ingeschakeld op deze hebben. Dit komt doordat Data Box-apparaten bieden nog geen ondersteuning voor directe opname in Azure Data Lake Storage Gen2. U moet te kopiëren naar een opslagaccount en klikt u vervolgens een tweede exemplaar bij het Gen2 ADLS-account. Instructies hiervoor staan vermeld in de onderstaande stappen.
    - Bekabelen en verbinding maken met uw [Data Box](https://docs.microsoft.com/azure/databox/data-box-deploy-set-up) of [Data Box zware](https://docs.microsoft.com/azure/databox/data-box-heavy-deploy-set-up) naar een on-premises netwerk.

Als u klaar bent, laten we beginnen.

## <a name="copy-your-data-to-a-data-box-device"></a>Uw gegevens kopiëren naar een Data Box-apparaat

Als u wilt de gegevens van uw on-premises HDFS gegevensopslag kopiëren naar een Data Box-apparaat, moet u een aantal dingen instellen en vervolgens met de [DistCp](https://hadoop.apache.org/docs/stable/hadoop-distcp/DistCp.html) hulpprogramma.

Als de hoeveelheid gegevens die u wilt kopiëren meer dan de capaciteit van een enkel Data Box of die van één knooppunt op gegevens in het zware is, splitst u uw gegevensset in de grootten die in uw apparaten passen.

Volg deze stappen voor het kopiëren van gegevens via de REST API's van de Blob/objectopslag naar uw Data Box-apparaat. De interface van de REST-API maakt het apparaat weergegeven als een HDFS-archief met uw cluster. 


1. Voordat u de gegevens via REST kopiëren, de beveiliging en verbinding primitieven verbinding maken met de REST-interface op de Data Box of de gegevens in het zware identificeren Aanmelden bij de lokale web-UI van Data Box en gaat u naar **verbinding maken en kopiëren** pagina. Op basis van de Azure storage-account voor het apparaat onder **toegang krijgen tot instellingen**, zoek en selecteer **REST**.

    ![De pagina "Verbinding maken en kopiëren"](media/data-lake-storage-migrate-on-premises-HDFS-cluster/data-box-connect-rest.png)

2. In de storage-account voor toegang en het dialoogvenster voor uploaden van gegevens, Kopieer de **eindpunt van Blob service** en de **opslagaccountsleutel**. Van het eindpunt van blob service, laat de `https://` en de afsluitende slash bevatten.

    In dit geval wordt het eindpunt is: `https://mystorageaccount.blob.mydataboxno.microsoftdatabox.com/`. Het hostgedeelte van de URI die u gebruikt is: `mystorageaccount.blob.mydataboxno.microsoftdatabox.com`. Zie voor een voorbeeld hoe u [verbinding maken met REST via http](/azure/databox/data-box-deploy-copy-data-via-rest). 

     ![Het dialoogvenster "Toegang tot opslagaccount en gegevens uploaden"](media/data-lake-storage-migrate-on-premises-HDFS-cluster/data-box-connection-string-http.png)

3. Het eindpunt en de Data Box of zware van gegevens in het knooppunt IP-adres toevoegen `/etc/hosts` op elk knooppunt.

    ```    
    10.128.5.42  mystorageaccount.blob.mydataboxno.microsoftdatabox.com
    ```
    Als u van een ander mechanisme voor DNS gebruikmaakt, u moet ervoor zorgen dat de Data Box-eindpunt kan worden omgezet.
    
4. Instellen van een shell-variabele `azjars` om te verwijzen naar de `hadoop-azure` en de `microsoft-windowsazure-storage-sdk` jar-bestanden. Deze bestanden zijn in de Hadoop-installatiemap (u kunt controleren als deze bestanden aanwezig zijn met deze opdracht `ls -l $<hadoop_install_dir>/share/hadoop/tools/lib/ | grep azure` waar `<hadoop_install_dir>` is de map waar u Hadoop hebt geïnstalleerd) gebruikt u de volledige paden. 
    
    ```
    # azjars=$hadoop_install_dir/share/hadoop/tools/lib/hadoop-azure-2.6.0-cdh5.14.0.jar
    # azjars=$azjars,$hadoop_install_dir/share/hadoop/tools/lib/microsoft-windowsazure-storage-sdk-0.6.0.jar
    ```

5. De storage-container die u wilt gebruiken voor het kopiëren van gegevens maken. Als onderdeel van deze opdracht moet u ook een doelmap opgeven. Dit kan een dummy doelmap op dit moment zijn.

    ```
    # hadoop fs -libjars $azjars \
    -D fs.AbstractFileSystem.wasb.Impl=org.apache.hadoop.fs.azure.Wasb \
    -D fs.azure.account.key.[blob_service_endpoint]=[account_key] \
    -mkdir -p  wasb://[container_name]@[blob_service_endpoint]/[destination_folder]
    ```

6. Voer een lijst met opdracht uit om ervoor te zorgen dat uw container en map zijn gemaakt.

    ```
    # hadoop fs -libjars $azjars \
    -D fs.AbstractFileSystem.wasb.Impl=org.apache.hadoop.fs.azure.Wasb \
    -D fs.azure.account.key.[blob_service_endpoint]=[account_key] \
    -ls -R  wasb://[container_name]@[blob_service_endpoint]/
    ```

7. Gegevens kopiëren van het Hadoop HDFS naar gegevens in Blob-opslag, naar de container die u eerder hebt gemaakt. Als de map die u naar kopiëren wilt niet wordt gevonden, de opdracht wordt deze automatisch aangemaakt.

    ```
    # hadoop distcp \
    -libjars $azjars \
    -D fs.AbstractFileSystem.wasb.Impl=org.apache.hadoop.fs.azure.Wasb \
    -D fs.azure.account.key.[blob_service_endpoint]=[account_key] \
    -strategy dynamic -m 4 -update \
    [source_directory] \
           wasb://[container_name]@[blob_service_endpoint]/[destination_folder]       
    ```
   De `-libjars` optie wordt gebruikt om de `hadoop-azure*.jar` en de afhankelijke `azure-storage*.jar` bestanden beschikbaar voor `distcp`. Dit kan al gebeuren voor sommige clusters.
   
   Het volgende voorbeeld ziet u hoe de `distcp` opdracht wordt gebruikt om gegevens te kopiëren.
   
   ```
   # hadoop distcp \
    -libjars $azjars \
    -D fs.AbstractFileSystem.wasb.Impl=org.apache.hadoop.fs.azure.Wasb \
    -D fs.azure.account.key.mystorageaccount.blob.mydataboxno.microsoftdatabox.com=myaccountkey \
    -strategy dynamic -m 4 -update \
    /data/testfiles \
    wasb://hdfscontainer@mystorageaccount.blob.mydataboxno.microsoftdatabox.com/testfiles
   ```
  
Voor het verbeteren van de snelheid kopiëren:
- Wijzig het aantal mappers. (Maakt gebruik van het bovenstaande voorbeeld `m` = 4 mappers.)
- Probeer uit te voeren meerdere `distcp` parallel.
- Houd er rekening mee dat grote bestanden beter dan kleine bestanden uitvoeren.
    
## <a name="ship-the-data-box-to-microsoft"></a>De Data Box naar Microsoft verzenden

Volg deze stappen voor het voorbereiden en verzenden van de Data Box-apparaat naar Microsoft.

1. Nadat het kopiëren van gegevens voltooid is, worden uitgevoerd:
    
    - [Voorbereiding voor verzending op uw Data Box of de gegevens in het zware](https://docs.microsoft.com/azure/databox/data-box-deploy-copy-data-via-rest).
    - Nadat de voorbereiding van het apparaat voltooid is, moet u de stuklijst-bestanden downloaden. U gaat deze stuklijst gebruiken of manifest van de bestanden later om te controleren of de gegevens geüpload naar Azure. 
    - Het apparaat uitschakelen en verwijderen van de kabels.
2.  Een ophalen met UPS plannen. Volg de instructies voor:

    - [Verzenden van uw Data Box](https://docs.microsoft.com/azure/databox/data-box-deploy-picked-up) 
    - [Verzenden van uw Data Box-zwaar](https://docs.microsoft.com/azure/databox/data-box-heavy-deploy-picked-up).
3.  Nadat Microsoft het apparaat ontvangt, is verbonden met het netwerk van datacenters en de gegevens wordt geüpload naar het opslagaccount dat u (met hiërarchische naamruimten uitgeschakeld opgeeft) wanneer u de bestelling heeft geplaatst apparaat. Controleer of op basis van de bestanden stuklijst dat al uw gegevens is geüpload naar Azure. Nu kunt u deze gegevens verplaatsen naar een Data Lake Storage Gen2 storage-account.


## <a name="move-the-data-onto-your-data-lake-storage-gen2-storage-account"></a>De gegevens naar uw Data Lake Storage Gen2 storage-account verplaatsen

Deze stap is nodig als u Azure Data Lake Storage Gen2 als uw gegevensarchief. Als u alleen een blob storage-account zonder hiërarchische naamruimte als uw gegevensopslag gebruikt, hoeft u niet te doen in deze stap.

U kunt dit op twee manieren doen.

- Gebruik [Azure Data Factory om gegevens te verplaatsen naar ADLS Gen2](https://docs.microsoft.com/azure/data-factory/load-azure-data-lake-storage-gen2). U moet opgeven **Azure Blob Storage** als de bron.

- Gebruik uw Azure op basis van Hadoop-cluster. U kunt deze opdracht DistCp uitvoeren:

    ```bash
    hadoop distcp -Dfs.azure.account.key.{source_account}.dfs.windows.net={source_account_key} abfs://{source_container} @{source_account}.dfs.windows.net/[path] abfs://{dest_container}@{dest_account}.dfs.windows.net/[path]
    ```

Met deze opdracht kopieert zowel gegevens en metagegevens van uw storage-account in uw Data Lake Storage Gen2 storage-account.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de werking van Data Lake Storage Gen2 met HDInsight-clusters. Zie [Azure Data Lake Storage Gen2 gebruiken met Azure HDInsight-clusters](../../hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2.md).
