---
title: Lege edge-knooppunten op Apache Hadoop-clusters in HDInsight - Azure gebruiken
description: Hoe u een lege edge-knooppunt toevoegen aan een HDInsight-cluster die kan worden gebruikt als een client, en vervolgens testen/host uw HDInsight-toepassingen.
ms.reviewer: jasonh
author: hrasheed-msft
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 06/13/2019
ms.author: hrasheed
ms.openlocfilehash: 41313c7d484df5568fbf729b683a0e9df5325abc
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67433741"
---
# <a name="use-empty-edge-nodes-on-apache-hadoop-clusters-in-hdinsight"></a>Lege edge-knooppunten op Apache Hadoop-clusters in HDInsight gebruiken

Leer hoe u een lege edge-knooppunt toevoegen aan een HDInsight-cluster. Een lege edge-knooppunt is een Linux-machine met de dezelfde clienthulpprogramma's geïnstalleerd en geconfigureerd zoals in de hoofdknooppunten, maar zonder dat er [Apache Hadoop](https://hadoop.apache.org/) services die worden uitgevoerd. U kunt het edge-knooppunt gebruiken voor toegang tot het cluster en hosten van uw client-apps testen van uw clienttoepassingen. 

U kunt een lege edge-knooppunt toevoegen aan een bestaand HDInsight-cluster, naar een nieuw cluster bij het maken van het cluster. Toevoegen van een lege edge-knooppunt wordt uitgevoerd met behulp van Azure Resource Manager-sjabloon.  Het volgende voorbeeld laat zien hoe het werkt met behulp van een sjabloon:

    "resources": [
        {
            "name": "[concat(parameters('clusterName'),'/', variables('applicationName'))]",
            "type": "Microsoft.HDInsight/clusters/applications",
            "apiVersion": "2015-03-01-preview",
            "dependsOn": [ "[concat('Microsoft.HDInsight/clusters/',parameters('clusterName'))]" ],
            "properties": {
                "marketPlaceIdentifier": "EmptyNode",
                "computeProfile": {
                    "roles": [{
                        "name": "edgenode",
                        "targetInstanceCount": 1,
                        "hardwareProfile": {
                            "vmSize": "Standard_D3"
                        }
                    }]
                },
                "installScriptActions": [{
                    "name": "[concat('emptynode','-' ,uniquestring(variables('applicationName')))]",
                    "uri": "[parameters('installScriptAction')]",
                    "roles": ["edgenode"]
                }],
                "uninstallScriptActions": [],
                "httpsEndpoints": [],
                "applicationType": "CustomApplication"
            }
        }
    ],

Zoals weergegeven in het voorbeeld, roept u eventueel een [script actie](hdinsight-hadoop-customize-cluster-linux.md) om uit te voeren van aanvullende configuratie, zoals het installeren van [Apache Hue](hdinsight-hadoop-hue-linux.md) in het edge-knooppunt. Het script van de actie script moet openbaar toegankelijk is op het web.  Als het script wordt opgeslagen in Azure storage, gebruikt u bijvoorbeeld openbare containers of openbare blobs.

De VM-grootte van het edge-knooppunt moet voldoen aan de vereiste grootte van HDInsight-cluster worker-knooppunt vm. Zie voor de aanbevolen worker-knooppunt vm-grootten [Apache Hadoop-clusters maken in HDInsight](hdinsight-hadoop-provision-linux-clusters.md#cluster-types).

Nadat u een edge-knooppunt hebt gemaakt, kunt u verbinding maken met het edge-knooppunt met behulp van SSH en clienthulpprogramma's voor toegang tot het Hadoop-cluster in HDInsight worden uitgevoerd.

> [!WARNING]   
> Aangepaste onderdelen die zijn geïnstalleerd op het edge-knooppunt ontvangen commercieel redelijke ondersteuning van Microsoft. Dit kan leiden tot het oplossen van problemen die kunnen optreden. Of u mogelijk verwezen naar bronnen van de community voor verdere ondersteuning. Hier volgen enkele van de meest actieve sites om hulp te krijgen van de community:
>
> * [MSDN-forum voor HDInsight](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight)
> * [https://stackoverflow.com](https://stackoverflow.com).
>
> Als u een Apache-technologie gebruikt, kunt u mogelijk kunnen ondersteuning aanvragen via de Apache projectsites vinden op [ https://apache.org ](https://apache.org), zoals de [Apache Hadoop](https://hadoop.apache.org/) site.

> [!IMPORTANT]
> Ubuntu-installatiekopieën beschikbaar voor nieuwe HDInsight-cluster maken binnen drie maanden van het worden gepubliceerd. Vanaf januari 2019, actieve clusters (met inbegrip van edge-knooppunten) zijn **niet** automatisch gevuld. Klanten moeten scriptacties of andere methoden gebruiken voor het vullen van een actief cluster.  Zie voor meer informatie, [OS patches voor HDInsight](./hdinsight-os-patching.md).

## <a name="add-an-edge-node-to-an-existing-cluster"></a>Een edge-knooppunt toevoegen aan een bestaand cluster
In deze sectie maakt u een Resource Manager-sjabloon gebruiken een edge-knooppunt toevoegen aan een bestaand HDInsight-cluster.  De Resource Manager-sjabloon kunt u vinden in [GitHub](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-add-edge-node/). De Resource Manager-sjabloon wordt de actie van een script dat zich bevindt in https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-hdinsight-linux-add-edge-node/scripts/EmptyNodeSetup.sh. Het script uitvoeren niet alle acties.  Het is om te demonstreren aanroepende scriptactie van Resource Manager-sjabloon.

**Een lege edge-knooppunt toevoegen aan een bestaand cluster**

1. Klik op de volgende afbeelding om te melden bij Azure en opent u de Azure Resource Manager-sjabloon in Azure portal. 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-add-edge-node%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apps-use-edge-node/deploy-to-azure.png" alt="Deploy to Azure"></a>
3. Configureer de volgende eigenschappen:
   
   * **Abonnement**: Selecteer een Azure-abonnement gebruikt voor het maken van het cluster.
   * **Resourcegroep**: Selecteer de resourcegroep die is gebruikt voor het bestaande HDInsight-cluster.
   * **Locatie**: Selecteer de locatie van het bestaande HDInsight-cluster.
   * **Clusternaam**: Voer de naam van een bestaand HDInsight-cluster.
   * **De grootte van knooppunt van de rand**: Selecteer een van de VM-grootten. De vm-grootte moet voldoen aan de worker-knooppunt vm groottevereisten voldoet. Zie voor de aanbevolen worker-knooppunt vm-grootten [Apache Hadoop-clusters maken in HDInsight](hdinsight-hadoop-provision-linux-clusters.md#cluster-types).
   * **Edge-knooppunt voorvoegsel**: De standaardwaarde is **nieuwe**.  Gebruik de standaardwaarde, de naam van het edge-knooppunt is **nieuwe Edge**.  U kunt het voorvoegsel van de portal aanpassen. U kunt ook de volledige naam van de sjabloon aanpassen.

4. Controleer **ik ga akkoord met de voorwaarden en bepalingen bovenstaande**, en klik vervolgens op **aankoop** te maken van het edge-knooppunt.

>[!IMPORTANT]  
> Zorg ervoor dat u selecteert u de Azure-resourcegroep voor de bestaande HDInsight-cluster.  Anders krijgt u het foutbericht 'kan de bewerking voor de geneste resource niet uitvoeren. Bovenliggende resource '&lt;ClusterName > is niet gevonden. "

## <a name="add-an-edge-node-when-creating-a-cluster"></a>Een edge-knooppunt toevoegen bij het maken van een cluster
In deze sectie maakt u een Resource Manager-sjabloon gebruiken voor het maken van HDInsight-cluster met een edge-knooppunt.  De Resource Manager-sjabloon kunt u vinden in de [galerie van Azure-quickstart-sjablonen](https://azure.microsoft.com/documentation/templates/101-hdinsight-linux-with-edge-node/). De Resource Manager-sjabloon wordt de actie van een script dat zich bevindt in https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-hdinsight-linux-with-edge-node/scripts/EmptyNodeSetup.sh. Het script uitvoeren niet alle acties.  Het is om te demonstreren aanroepende scriptactie van Resource Manager-sjabloon.

**Een HDInsight-cluster maakt met een edge-knooppunt**

1. Een HDInsight-cluster maken als u nog geen abonnement hebt.  Zie [aan de slag met Hadoop in HDInsight](hadoop/apache-hadoop-linux-tutorial-get-started.md).
2. Klik op de volgende afbeelding om te melden bij Azure en opent u de Azure Resource Manager-sjabloon in Azure portal. 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-with-edge-node%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apps-use-edge-node/deploy-to-azure.png" alt="Deploy to Azure"></a>
3. Configureer de volgende eigenschappen:
   
   * **Abonnement**: Selecteer een Azure-abonnement gebruikt voor het maken van het cluster.
   * **Resourcegroep**: Maak een nieuwe resourcegroep gebruikt voor het cluster.
   * **Locatie**: Selecteer een locatie voor de resourcegroep.
   * **Clusternaam**: Voer een naam voor het nieuwe cluster te maken.
   * **Gebruikersnaam voor aanmelding cluster**: Voer de naam van het Hadoop-HTTP-gebruiker.  De standaardnaam is **admin**.
   * **Cluster-wachtwoord voor Clusteraanmelding**: Voer het wachtwoord van de Hadoop-HTTP-gebruiker.
   * **SSH-gebruikersnaam**: Voer de naam van de SSH-gebruiker. De standaardnaam is **sshuser**.
   * **SSH wachtwoord**: Voer het wachtwoord van de SSH-gebruiker.
   * **Installeren van de scriptactie**: Hou de standaardwaarde voor dit artikel te doorlopen.
     
     Sommige eigenschappen zijn vastgelegd in de sjabloon: Clustertype, aantal Cluster worker-knooppunten, de grootte van de Edge-knooppunt en de naam van de Edge-knooppunt.
4. Controleer **ik ga akkoord met de voorwaarden en bepalingen bovenstaande**, en klik vervolgens op **aankoop** voor het maken van het cluster met het edge-knooppunt.

## <a name="add-multiple-edge-nodes"></a>Meerdere edge-knooppunten toevoegen

U kunt meerdere edge-knooppunten toevoegen aan een HDInsight-cluster.  De configuratie met meerdere knooppunten edge kan alleen worden uitgevoerd met behulp van Azure Resource Manager-sjablonen.  Zie het voorbeeld van de sjabloon aan het begin van dit artikel.  U moet bijwerken de **targetInstanceCount** in overeenstemming met het nummer van edge-knooppunten die u wilt maken.

## <a name="access-an-edge-node"></a>Toegang tot een edge-knooppunt
Het edge-knooppunt ssh-eindpunt is &lt;EdgeNodeName >.&lt; Clusternaam >-ssh.azurehdinsight.net:22.  Bijvoorbeeld, nieuwe-edgenode.myedgenode0914-ssh.azurehdinsight.net:22.

Het edge-knooppunt wordt weergegeven als een toepassing in Azure portal.  De portal biedt u de informatie voor toegang tot het edge-knooppunt met behulp van SSH.

**Om te controleren of het SSH-eindpunt van de edge-knooppunt**

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Open het HDInsight-cluster met een edge-knooppunt.
3. Klik op **toepassingen**. U ziet het edge-knooppunt.  De standaardnaam is **nieuwe Edge**.
4. Klik op het edge-knooppunt. U ziet het SSH-eindpunt.

**Hive gebruiken op het edge-knooppunt**

1. Gebruik SSH om verbinding te maken met het edge-knooppunt. Zie [SSH-sleutels gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor informatie.

2. Nadat u hebt verbonden met het edge-knooppunt via SSH, gebruikt u de volgende opdracht uit om het Hive-console te openen:
   
        hive
3. Voer de volgende opdracht uit om Hive-tabellen in het cluster weer te geven:
   
        show tables;

## <a name="delete-an-edge-node"></a>Een edge-knooppunt verwijderen
U kunt een edge-knooppunt verwijderen uit de Azure-portal.

**Voor toegang tot een edge-knooppunt**

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Open het HDInsight-cluster met een edge-knooppunt.
3. Klik op **toepassingen**. U ziet een lijst met edge-knooppunten.  
4. Met de rechtermuisknop op het edge-knooppunt dat u wilt verwijderen, en klik vervolgens op **verwijderen**.
5. Klik op **Ja** om te bevestigen.

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe u een edge-knooppunt toevoegt en over toegang tot het edge-knooppunt. Zie de volgende artikelen voor meer informatie:

* [HDInsight-toepassingen installeren](hdinsight-apps-install-applications.md): Informatie over het installeren van een HDInsight-toepassing aan uw clusters.
* [Aangepaste HDInsight-toepassingen installeren](hdinsight-apps-install-custom-applications.md): informatie over het implementeren van een niet-gepubliceerde HDInsight-toepassing op HDInsight.
* [HDInsight-toepassingen publiceren](hdinsight-apps-publish-applications.md): Informatie over het publiceren van uw aangepaste HDInsight-toepassingen op Azure Marketplace.
* [MSDN: Een HDInsight-toepassing installeren](https://msdn.microsoft.com/library/mt706515.aspx): Meer informatie over het definiëren van HDInsight-toepassingen.
* [Op Linux gebaseerde HDInsight-clusters aanpassen met behulp van een scriptactie](hdinsight-hadoop-customize-cluster-linux.md): informatie over het gebruik van een scriptactie om extra toepassingen te installeren.
* [Apache Hadoop op basis van Linux-clusters in HDInsight met behulp van Resource Manager-sjablonen maken](hdinsight-hadoop-create-linux-clusters-arm-templates.md): informatie over het aanroepen van Resource Manager-sjablonen om HDInsight-clusters te maken.

