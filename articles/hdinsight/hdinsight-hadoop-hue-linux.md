---
title: HUE met Hadoop op HDInsight Linux gebaseerde clusters - Azure
description: Leer hoe u Hue installeren op HDInsight-clusters en voor het routeren van de aanvragen naar de Hue-tunneling gebruiken. Hue opslag bladeren en uitvoeren van Hive of Pig gebruiken.
keywords: HUE hadoop
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 12/11/2017
ms.author: hrasheed
ms.openlocfilehash: 74ccf2af2d379b3c1966543885df6ebe5cf0f47a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67059436"
---
# <a name="install-and-use-hue-on-hdinsight-hadoop-clusters"></a>Installeren en Hue gebruiken op HDInsight Hadoop-clusters

Leer hoe u Hue installeren op HDInsight-clusters en voor het routeren van de aanvragen naar de Hue-tunneling gebruiken.

## <a name="what-is-hue"></a>Wat is Hue?
HUE is een set van webtoepassingen die worden gebruikt om te communiceren met een Apache Hadoop-cluster. U kunt Hue gebruiken om te bladeren, de opslag die is gekoppeld aan een Hadoop-cluster (in het geval van een HDInsight-clusters WASB), uitvoeren van Hive-taken en Pig-scripts, enzovoort. De volgende onderdelen zijn beschikbaar met Hue-installaties op een HDInsight Hadoop-cluster.

* Bijenwas Hive-Editor
* Apache Pig
* Metastore-manager
* Apache Oozie
* FileBrowser (die praat met standaardcontainer WASB)
* Job Browser

> [!WARNING]  
> Onderdelen van het HDInsight-cluster volledig worden ondersteund en Microsoft Support kunnen opsporen en oplossen van problemen met betrekking tot deze onderdelen.
>
> Aangepaste onderdelen commercieel redelijke ondersteuning om het probleem verder oplossen met u te helpen te ontvangen. Dit kan resulteren in het oplossen van het probleem of vraag of u contact opnemen met beschikbare kanalen voor de open source-technologieën waar uitgebreide expertise voor deze technologie kan worden gevonden. Er zijn bijvoorbeeld veel communitysites die kunnen worden gebruikt, zoals: [MSDN-forum voor HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [ https://stackoverflow.com ](https://stackoverflow.com). Ook Apache-projecten project-sites hebben op [ https://apache.org ](https://apache.org), bijvoorbeeld: [Hadoop](https://hadoop.apache.org/).
>
>

## <a name="install-hue-using-script-actions"></a>Met behulp van scriptacties Hue installeren

Hue installeren op een Linux gebaseerde HDInsight-cluster met het script is beschikbaar op https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh. Hue installeren op clusters met Azure Storage-Blobs (WASB) of Azure Data Lake Storage als standaardopslag kunt u dit script gebruiken.

Deze sectie vindt u instructies over het gebruik van het script bij het inrichten van het cluster met behulp van de Azure-portal.

> [!NOTE]  
> Azure PowerShell, de klassieke Azure-CLI, de HDInsight .NET SDK of Azure Resource Manager-sjablonen kunnen ook worden gebruikt om toe te passen van scriptacties. U kunt ook scriptacties toepassen op clusters al wordt uitgevoerd. Zie voor meer informatie, [aanpassen HDInsight-clusters met scriptacties](hdinsight-hadoop-customize-cluster-linux.md).
>
>

1. Start het inrichten van een cluster met behulp van de stappen in [inrichten HDInsight-clusters op Linux](hdinsight-hadoop-provision-linux-clusters.md), maar ze niet worden voltooid wordt ingericht.

   > [!NOTE]  
   > Hue installeren op HDInsight-clusters, de grootte van de aanbevolen hoofdknooppunt is ten minste A4 (8 kernen, 14 GB geheugen).
   >
   >
2. Op de **optionele configuratie** Selecteer **scriptacties**, en geef de informatie, zoals hieronder weergegeven:

    ![Script actieparameters bieden voor Hue](./media/hdinsight-hadoop-hue-linux/hue-script-action.png "script actieparameters bieden voor Hue")

   * **NAAM**: Voer een beschrijvende naam voor de scriptactie.
   * **SCRIPT-URI**: https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh
   * **HEAD**: Schakel deze optie.
   * **WERKNEMER**: Laat dit leeg.
   * **ZOOKEEPER**: Laat dit leeg.
   * **PARAMETERS**: Laat dit leeg.
3. Aan de onderkant van de **scriptacties**, gebruikt u de **selecteren** om de configuratie op te slaan. Gebruik tot slot de **Selecteer** knop aan de onderkant van de **optionele configuratie** blade de optionele configuratie-gegevens op te slaan.
4. Doorgaan met het inrichten van het cluster, zoals beschreven in [inrichten HDInsight-clusters op Linux](hdinsight-hadoop-provision-linux-clusters.md).

## <a name="use-hue-with-hdinsight-clusters"></a>Hue gebruiken met HDInsight-clusters

SSH-Tunneling is de enige manier om de Hue op het cluster toegang heeft wanneer deze wordt uitgevoerd. Tunneling via SSH, kan het verkeer en Ga rechtstreeks naar het hoofdknooppunt van het cluster waarop Hue wordt uitgevoerd. Nadat het cluster de inrichting is voltooid, gebruikt u de volgende stappen uit Hue gebruiken op een HDInsight Linux-cluster.

> [!NOTE]  
> U wordt aangeraden met behulp van web-Firefox-browser de onderstaande instructies te volgen.
>
>

1. Gebruik de informatie in [SSH-Tunneling gebruiken voor toegang tot de Apache Ambari-Webgebruikersinterface, ResourceManager JobHistory, NameNode, Oozie en andere webgebruikersinterfaces van](hdinsight-linux-ambari-ssh-tunnel.md) maken van een SSH-tunnel vanuit het clientsysteem naar het HDInsight-cluster en vervolgens configureren uw webbrowser de SSH-tunnel gebruiken als een proxy.

2. Nadat u hebt een SSH-tunnel gemaakt en uw browser naar de proxy het verkeer daarlangs geconfigureerd, moet u de hostnaam van het primaire hoofdknooppunt. U kunt dit doen door verbinding te maken met het cluster via SSH op poort 22. Bijvoorbeeld, `ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net` waar **gebruikersnaam** is de naam van de SSH-gebruiker en **CLUSTERNAME** is de naam van uw cluster.

    Zie [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

3. Eenmaal verbinding hebben, gebruikt u de volgende opdracht uit om op te halen van de volledig gekwalificeerde domeinnaam van het primaire hoofdknooppunt:

        hostname -f

    Hiermee herstelt u een naam die vergelijkbaar is met het volgende:

        hn0-myhdi-nfebtpfdv1nubcidphpap2eq2b.ex.internal.cloudapp.net

    Dit is de hostnaam van het primaire hoofdknooppunt waar de Hue-website zich bevindt.
4. De browser gebruiken om de Hue-portal HTTP te openen:\//HOSTNAME:8888. HOSTNAAM vervangen door de naam die u hebt verkregen in de vorige stap.

   > [!NOTE]  
   > Wanneer u zich aanmeldt voor de eerste keer, wordt u gevraagd om een account voor aanmelding bij de Hue-portal te maken. De referenties die u hier opgeeft, worden beperkt tot de portal en niet gerelateerd zijn aan de beheerder of de SSH-gebruikersreferenties die u hebt opgegeven tijdens het inrichten het cluster.
   >
   >

    ![Meld u aan bij de Hue-portal](./media/hdinsight-hadoop-hue-linux/hdinsight-hue-portal-login.png "Geef referenties op voor de Hue-portal")

### <a name="run-a-hive-query"></a>Een Hive-query uitvoeren
1. De Hue-portal, klik op **Query Editors**, en klik vervolgens op **Hive** om het Hive-editor te openen.

    ![Hive gebruiken](./media/hdinsight-hadoop-hue-linux/hdinsight-hue-portal-use-hive.png "Hive gebruiken")
2. Op de **helpen** tabblad onder **Database**, ziet u **hivesampletable**. Dit is een voorbeeldtabel die wordt geleverd met alle Hadoop-clusters op HDInsight. Voer een voorbeeldquery in het rechter deelvenster en de uitvoer zien op de **resultaten** tabblad in het deelvenster onder, zoals wordt weergegeven in de schermopname.

    ![Hive-query uitvoeren](./media/hdinsight-hadoop-hue-linux/hdinsight-hue-portal-hive-query.png "uitvoeren Hive-query")

    U kunt ook de **grafiek** tabblad om te bekijken van een visuele representatie van het resultaat.

### <a name="browse-the-cluster-storage"></a>De clusteropslag bladeren
1. De Hue-portal, klik op **bestandsbrowser** in de rechterbovenhoek van de menubalk.
2. De browser wordt standaard geopend op de **/gebruiker/myuser** directory. Klik op de slash vlak voor de map in het pad naar de hoofdmap van de Azure storage-container die zijn gekoppeld aan het cluster.

    ![Gebruik bestandsbrowser](./media/hdinsight-hadoop-hue-linux/hdinsight-hue-portal-file-browser.png "bestandsbrowser gebruiken")
3. Met de rechtermuisknop op een bestand of map om te zien van de beschikbare bewerkingen. Gebruik de **uploaden** knop in de rechterhoek bestanden uploaden naar de huidige map. Gebruik de **nieuw** knop om nieuwe bestanden of mappen te maken.

> [!NOTE]  
> De Hue-bestand-browser kunt alleen de inhoud van de standaard-container die zijn gekoppeld aan het HDInsight-cluster weergeven. Alle extra opslagruimte accounts/containers die u mogelijk hebt gekoppeld aan het cluster zich niet toegankelijk is via de browser. De extra containers die zijn gekoppeld aan het cluster wordt echter altijd worden toegankelijk is voor de Hive-taken. Bijvoorbeeld, als u de opdracht hebt ingevoerd `dfs -ls wasb://newcontainer@mystore.blob.core.windows.net` in de Hive-editor, ziet u de inhoud van andere containers ook. In deze opdracht **newcontainer** is niet de standaard-container die zijn gekoppeld aan een cluster.
>
>

## <a name="important-considerations"></a>Belangrijke overwegingen
1. Het script gebruikt voor het installeren van Hue installeert deze alleen op het primaire hoofdknooppunt van het cluster.

2. Tijdens de installatie, worden meerdere Hadoop-services (HDFS, YARN, MR2, Oozie) voor het bijwerken van de configuratie opnieuw gestart. Nadat het script is voltooid Hue installeren, is het duurt even voordat andere Hadoop-services om opnieuw te starten. Dit kan de Hue-prestaties in eerste instantie beïnvloeden. Nadat alle services worden gestart, is Hue is volledig functioneel.
3. HUE begrijpt niet Apache Tez-taken, de huidige standaardinstelling voor Hive. Als u MapReduce gebruiken als de engine voor het uitvoeren van Hive wilt, werkt u het script voor het gebruik van de volgende opdracht in uw script:

        set hive.execution.engine=mr;

4. Met Linux-clusters hebt u een scenario waarbij uw services worden uitgevoerd op het primaire hoofdknooppunt terwijl de Resource Manager kan worden uitgevoerd op de secundaire server. Dit scenario kan leiden tot fouten (Zie hieronder) bij het gebruik van Hue details wilt weergeven van taken wordt uitgevoerd op het cluster. U kunt echter de taakdetails weergeven wanneer de taak is voltooid.

   ![HUE-portal fout](./media/hdinsight-hadoop-hue-linux/hdinsight-hue-portal-error.png "Hue-portal fout")

   Dit is vanwege een bekend probleem. Als tijdelijke oplossing, Ambari zodanig aanpassen dat de actieve Resource Manager wordt ook uitgevoerd op het primaire hoofdknooppunt.
5. HUE begrijpt WebHDFS terwijl het HDInsight-clusters gebruiken met behulp van Azure Storage `wasb://`. Het aangepaste script gebruikt in combinatie met scriptactie installeert dus WebWasb, dit een service met WebHDFS compatibele is voor communicatie met WASB. Ja, hoewel de Hue-portal HDFS op plaatsen zegt (zoals wanneer u de muisaanwijzer over de **bestandsbrowser**), moet dit worden beschouwd als WASB.

## <a name="next-steps"></a>Volgende stappen
* [Apache Giraph installeren op HDInsight-clusters](hdinsight-hadoop-giraph-install-linux.md). Aanpassing van de cluster Giraph installeren op HDInsight Hadoop-clusters gebruiken. Giraph kunt u met behulp van Hadoop graph-verwerking uit te voeren en deze kan worden gebruikt met Azure HDInsight.
* [R installeren op HDInsight-clusters](hdinsight-hadoop-r-scripts-linux.md). Aanpassing van de cluster R installeren op HDInsight Hadoop-clusters gebruiken. R is een open source-taal en omgeving voor statistische computing. Het bevat honderden ingebouwde statistische functies en een eigen programmeertaal die aspecten van het programmeren van functionele en objectgeoriënteerde combineert. Het biedt ook uitgebreide grafische mogelijkheden.

[powershell-install-configure]: install-configure-powershell-linux.md
[hdinsight-provision]: hdinsight-provision-clusters-linux.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
