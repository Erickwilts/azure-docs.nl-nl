---
title: Begrijpen en oplossen van fouten van WebHCat op HDInsight - Azure
description: Leer hoe naar informatie over veelvoorkomende fouten geretourneerd door WebHCat op HDInsight en hoe u ze op te lossen.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: hrasheed
ms.openlocfilehash: cfbd42a67f9c9d6c66df3787b53575dc9e918e35
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67067977"
---
# <a name="understand-and-resolve-errors-received-from-webhcat-on-hdinsight"></a>Begrijpen en oplossen van fouten die zijn ontvangen van WebHCat op HDInsight

Meer informatie over fouten die zijn ontvangen als u de WebHCat voor HDInsight, en hoe u ze op te lossen. WebHCat wordt intern gebruikt door de client-side-hulpprogramma's zoals Azure PowerShell en de Data Lake Tools voor Visual Studio.

## <a name="what-is-webhcat"></a>Wat is WebHCat

[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) is een REST-API voor [HCatalog](https://cwiki.apache.org/confluence/display/Hive/HCatalog), een tabel- en storage management-laag voor Apache Hadoop. WebHCat is standaard ingeschakeld op HDInsight-clusters, en wordt gebruikt door verschillende hulpprogramma's voor het verzenden van taken, taakstatus, enz., zonder in te loggen met het cluster ophalen.

## <a name="modifying-configuration"></a>-Configuratie wijzigen

> [!IMPORTANT]  
> Aantal van de fouten die worden vermeld in dit document optreden met een geconfigureerde maximum is overschreden. Wanneer de resolutie-stap wordt vermeld dat u kunt een waarde wijzigen, moet u een van de volgende gebruiken om uit te voeren van de wijziging:

* Voor **Windows** clusters: Een scriptactie gebruiken om te configureren van de waarde tijdens het maken van clusters. Zie voor meer informatie, [scriptacties ontwikkelen](hdinsight-hadoop-script-actions-linux.md).

* Voor **Linux** clusters: Gebruik Apache Ambari (web- of REST-API) om de waarde te wijzigen. Zie voor meer informatie, [beheren HDInsight met Apache Ambari](hdinsight-hadoop-manage-ambari.md)


### <a name="default-configuration"></a>Standaardconfiguratie

Als de volgende standaardwaarden zijn overschreden, kan WebHCat prestaties verslechteren of er fouten optreden:

| Instelling | Wat het doet | Standaardwaarde |
| --- | --- | --- |
| [yarn.scheduler.capacity.maximum-applications][maximum-applications] |Het maximale aantal taken dat gelijktijdig actief kan zijn (in behandeling of wordt uitgevoerd) |10\.000 |
| [templeton.exec.max-procs][max-procs] |Het maximale aantal aanvragen die gelijktijdig kunnen worden weergegeven |20 |
| [mapreduce.jobhistory.max-age-ms][max-age-ms] |Het aantal dagen dat u de taakgeschiedenis worden bewaard |7 dagen |

## <a name="too-many-requests"></a>Te veel aanvragen

**HTTP-statuscode**: 429

| Oorzaak | Oplossing |
| --- | --- |
| U hebt het maximum aantal gelijktijdige aanvragen bediend door WebHCat per minuut (standaard 20) overschreden |Verlaag uw workload om ervoor te zorgen dat u niet verzenden van meer dan het maximum aantal gelijktijdige aanvragen of de limiet voor gelijktijdige aanvraag verhogen door het wijzigen van `templeton.exec.max-procs`. Zie voor meer informatie, [configuratie wijzigen](#modifying-configuration) |

## <a name="server-unavailable"></a>Server is niet beschikbaar

**HTTP-statuscode**: 503

| Oorzaak | Oplossing |
| --- | --- |
| Deze statuscode treedt meestal op tijdens de failover tussen de primaire en secundaire hoofdknooppunt voor het cluster |Wacht twee minuten en probeer het opnieuw |

## <a name="bad-request-content-could-not-find-job"></a>Ongeldige aanvraag inhoud: Kan de taak niet vinden.

**HTTP-statuscode**: 400

| Oorzaak | Oplossing |
| --- | --- |
| Taakdetails zijn opgeschoond door de taakgeschiedenis Opruimprogramma |De bewaartermijn van Taakgeschiedenis is 7 dagen. De gebruikelijke bewaarperiode kan worden gewijzigd door het wijzigen van `mapreduce.jobhistory.max-age-ms`. Zie voor meer informatie, [configuratie wijzigen](#modifying-configuration) |
| Taak is afgebroken vanwege een failover |Probeer het opnieuw verzenden van taken voor maximaal twee minuten |
| Er is een ongeldige taak-ID gebruikt |Controleer of de taak-ID juist is |

## <a name="bad-gateway"></a>Ongeldige gateway

**HTTP-statuscode**: 502

| Oorzaak | Oplossing |
| --- | --- |
| Interne garbagecollection plaatsvindt in het proces van WebHCat |Wachten op garbagecollection voltooid of de WebHCat-service opnieuw starten |
| Time-out voor wachten op een reactie van de ResourceManager-service. Deze fout kan optreden wanneer het aantal actieve toepassingen het geconfigureerde maximum (standaard 10.000 wordt) |Wachten op de momenteel actieve taken om te voltooien of de taaklimiet voor gelijktijdige verhogen door het wijzigen van `yarn.scheduler.capacity.maximum-applications`. Zie voor meer informatie de [wijzigen configuratie](#modifying-configuration) sectie. |
| Het ophalen van alle taken via de [GET /jobs](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference+Jobs) aanroep bij `Fields` is ingesteld op `*` |Kan niet worden opgehaald *alle* taakgegevens. Gebruik in plaats daarvan `jobid` ophalen van gegevens voor taken die alleen groter is dan een bepaalde taak-ID. Of gebruik geen `Fields` |
| De WebHCat-service is niet beschikbaar tijdens de failover van hoofdknooppunt |Wacht twee minuten en probeer het opnieuw |
| Er zijn meer dan 500 in behandeling zijnde taken die worden verzonden via WebHCat |Wacht totdat het momenteel in behandeling zijnde taken zijn voltooid voordat u meer taken verzendt |

[maximum-applications]: https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.3/bk_system-admin-guide/content/setting_application_limits.html
[max-procs]: https://cwiki.apache.org/confluence/display/Hive/WebHCat+Configure#WebHCatConfigure-WebHCatConfiguration
[max-age-ms]: https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.6.0/ds_Hadoop/hadoop-mapreduce-client/hadoop-mapreduce-client-core/mapred-default.xml
