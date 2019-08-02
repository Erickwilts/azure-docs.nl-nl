---
title: Apache Spark taak wordt traag uitgevoerd wanneer de Azure storage-container veel bestanden in azure HDInsight bevat
description: Apache Spark taak wordt traag uitgevoerd wanneer de Azure storage-container veel bestanden in azure HDInsight bevat
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.date: 07/29/2019
ms.openlocfilehash: 78dff1b9d9db4e54ab1a8f7203088753e206c610
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2019
ms.locfileid: "68641953"
---
# <a name="scenario-apache-spark-job-run-slowly-when-the-azure-storage-container-contains-many-files-in-azure-hdinsight"></a>Scenario: Apache Spark taak langzaam uitgevoerd wanneer de Azure storage-container veel bestanden in azure HDInsight bevat

In dit artikel worden probleemoplossings stappen en mogelijke oplossingen voor problemen beschreven bij het gebruik van Apache Spark-onderdelen in azure HDInsight-clusters.

## <a name="issue"></a>Probleem

Bij het uitvoeren van een HDInsight-cluster wordt de Apache Spark-taak die naar de Azure storage-container schrijft, traag wanneer er veel bestanden/submappen zijn. Het duurt bijvoorbeeld 20 seconden wanneer u naar een nieuwe container schrijft, maar ongeveer 2 minuten wanneer u naar een container met van persoonlijkheden-bestanden schrijft.

## <a name="cause"></a>Oorzaak

Dit is een bekend Spark-probleem. De vertraging is afkomstig van de `ListBlob` - `GetBlobProperties` en-bewerkingen tijdens de uitvoering van Spark-taken.

Voor het bijhouden van partities moet Spark een met `FileStatusCache` informatie over de directory-structuur onderhouden. Met deze cache kunnen Spark de paden parseren en op de hoogte zijn van de beschik bare partities. Het voor deel van het bijhouden van partities is dat Spark alleen de benodigde bestanden aanraakt wanneer u gegevens leest. Om deze informatie up-to-date te houden wanneer u nieuwe gegevens schrijft, moet Spark alle bestanden in de map weer geven en deze cache bijwerken.

Telkens wanneer u de Directory bijwerkt in Spark 1,6, moet u (1) de cache wissen (2) recursief alle bestanden weer geven en (3) de volledige cache bijwerken. Dit leidt tot veel vermeldings bewerkingen.

In Spark 2,1, terwijl de cache na elke schrijf bewerking niet hoeft te worden bijgewerkt, controleert Spark of een bestaande partitie kolom overeenkomt met de voor waarde die wordt voorgesteld in de huidige schrijf aanvraag, zodat deze ook aan het begin van elke schrijf bewerking wordt weer gegeven.

In Spark 2,2, bij het schrijven van gegevens met de toevoeg modus, moet dit prestatie probleem worden opgelost.

## <a name="resolution"></a>Oplossing

Wanneer u een gegevensgestuurde gegevensset maakt, is het belang rijk dat u een partitie schema gebruikt waarmee het aantal bestanden wordt beperkt dat door Spark moet worden weer gegeven om `FileStatusCache`de te kunnen bijwerken.

Voor elke ne micro batch waarbij N% 100 = = 0 (100 is een voor beeld), verplaatst u de bestaande gegevens naar een andere map, die door Spark kan worden geladen.

## <a name="next-steps"></a>Volgende stappen

Als u het probleem niet ziet of als u het probleem niet kunt oplossen, gaat u naar een van de volgende kanalen voor meer ondersteuning:

* Krijg antwoorden van Azure-experts via de [ondersteuning van Azure Community](https://azure.microsoft.com/support/community/).

* Maak verbinding [@AzureSupport](https://twitter.com/azuresupport) met-het officiële Microsoft Azure account voor het verbeteren van de gebruikers ervaring door de Azure-community te verbinden met de juiste resources: antwoorden, ondersteuning en experts.

* Als u meer hulp nodig hebt, kunt u een ondersteunings aanvraag indienen via de [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Selecteer **ondersteuning** in de menu balk of open de hub **Help en ondersteuning** . Lees voor meer gedetailleerde informatie [hoe u een ondersteunings aanvraag voor Azure maakt](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). De toegang tot abonnementen voor abonnements beheer en facturering is inbegrepen bij uw Microsoft Azure-abonnement en technische ondersteuning wordt geleverd via een van de ondersteunings [abonnementen voor Azure](https://azure.microsoft.com/support/plans/).
