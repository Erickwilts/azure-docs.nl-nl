---
title: Apache Hive logboeken schijf ruimte invullen-Azure HDInsight
description: De Apache Hive-logboeken vullen de schijf ruimte op de hoofd knooppunten in azure HDInsight.
ms.service: hdinsight
ms.topic: troubleshooting
author: nisgoel
ms.author: nisgoel
ms.reviewer: jasonh
ms.date: 10/05/2020
ms.openlocfilehash: 5554a66927fc70f22ec552b938ae62038a04acb9
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2020
ms.locfileid: "92533016"
---
# <a name="scenario-apache-hive-logs-are-filling-up-the-disk-space-on-the-head-nodes-in-azure-hdinsight"></a>Scenario: Apache Hive-logboeken worden de schijf ruimte op de hoofd knooppunten in azure HDInsight gevuld

In dit artikel worden de stappen beschreven voor het oplossen van problemen en mogelijke oplossingen voor problemen met betrekking tot onvoldoende schijf ruimte op de hoofd knooppunten in azure HDInsight-clusters.

## <a name="issue"></a>Probleem

Op een Apache Hive-LLAP-cluster nemen ongewenste Logboeken de volledige schijf ruimte op de hoofd knooppunten in beslag. Als gevolg hiervan kunnen de volgende problemen worden weer gegeven.

1. SSH-toegang mislukt omdat er geen ruimte meer is op het hoofd knooppunt.
2. Ambari geeft *HTTP-fout: de 503-Service is niet beschikbaar* .
3. HiveServer2 Interactive kan niet opnieuw worden opgestart.

In de `ambari-agent` Logboeken ziet u het volgende wanneer het probleem optreedt.
```
ambari_agent - Controller.py - [54697] - Controller - ERROR - Error:[Errno 28] No space left on device
```
```
ambari_agent - HostCheckReportFileHandler.py - [54697] - ambari_agent.HostCheckReportFileHandler - ERROR - Can't write host check file at /var/lib/ambari-agent/data/hostcheck.result
```

## <a name="cause"></a>Oorzaak

In geavanceerde configuraties van Hive-log4j wordt het huidige standaard schema voor verwijdering ingesteld op bestanden die ouder zijn dan 30 dagen, op basis van de datum van laatste wijziging.

## <a name="resolution"></a>Oplossing

1. Ga naar samen vatting van Hive-onderdeel in de Ambari-Portal en klik op het `Configs` tabblad.

2. Ga naar de `Advanced hive-log4j` sectie in geavanceerde instellingen.

3. Stel `appender.RFA.strategy.action.condition.age` de para meter in op een leeftijd van uw keuze. Voor beeld 14 dagen: `appender.RFA.strategy.action.condition.age = 14D`

4. Als u geen gerelateerde instellingen ziet, moet u de volgende instellingen toevoegen.
    ```
    # automatically delete hive log
    appender.RFA.strategy.action.type = Delete
    appender.RFA.strategy.action.basePath = ${sys:hive.log.dir}
    appender.RFA.strategy.action.condition.type = IfLastModified
    appender.RFA.strategy.action.condition.age = 30D
    appender.RFA.strategy.action.PathConditions.type = IfFileName
    appender.RFA.strategy.action.PathConditions.regex = hive*.*log.*
    ```

5. Stel `hive.root.logger` `INFO,RFA` als volgt in. De standaard instelling is DEBUG, waardoor logboeken erg groot worden.

    ```
    # Define some default values that can be overridden by system properties
    hive.log.threshold=ALL
    hive.root.logger=INFO,RFA
    hive.log.dir=${java.io.tmpdir}/${user.name}
    hive.log.file=hive.log
    ```

6. Sla de configuraties op en start de vereiste onderdelen opnieuw.

## <a name="next-steps"></a>Volgende stappen

Als u het probleem niet ziet of als u het probleem niet kunt oplossen, gaat u naar een van de volgende kanalen voor meer ondersteuning:

* Krijg antwoorden van Azure-experts via de [ondersteuning van Azure Community](https://azure.microsoft.com/support/community/).

* Maak verbinding met [@AzureSupport](https://twitter.com/azuresupport) -het officiële Microsoft Azure account voor het verbeteren van de gebruikers ervaring door de Azure-community te verbinden met de juiste resources: antwoorden, ondersteuning en experts.

* Als u meer hulp nodig hebt, kunt u een ondersteunings aanvraag indienen via de [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Selecteer **ondersteuning** in de menu balk of open de hub **Help en ondersteuning** . Lees voor meer gedetailleerde informatie [hoe u een ondersteunings aanvraag voor Azure maakt](../../azure-portal/supportability/how-to-create-azure-support-request.md). De toegang tot abonnementen voor abonnements beheer en facturering is inbegrepen bij uw Microsoft Azure-abonnement en technische ondersteuning wordt geleverd via een van de [ondersteunings abonnementen voor Azure](https://azure.microsoft.com/support/plans/).