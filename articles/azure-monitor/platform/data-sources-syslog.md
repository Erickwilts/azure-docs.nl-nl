---
title: Verzamelen en analyseren van Syslog-berichten in Azure Monitor | Microsoft Docs
description: Syslog is een protocol voor het vastleggen van gebeurtenis die geldt voor Linux. In dit artikel wordt beschreven hoe u verzamelen van Syslog-berichten configureren in Log Analytics en de details van de records die zij maken.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: f1d5bde4-6b86-4b8e-b5c1-3ecbaba76198
ms.service: log-analytics
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2019
ms.author: magoedte
ms.openlocfilehash: 41ea6222689516f224fc23ce6a658d17f7f81866
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60240860"
---
# <a name="syslog-data-sources-in-azure-monitor"></a>Syslog-gegevensbronnen in Azure Monitor
Syslog is een protocol voor het vastleggen van gebeurtenis die geldt voor Linux. Toepassingen wordt berichten die kunnen worden opgeslagen op de lokale computer of worden geleverd met een Syslog-collector verzonden. Wanneer de Log Analytics-agent voor Linux is geïnstalleerd, configureert het de lokale Syslog-daemon voor het doorsturen van berichten naar de agent. De agent verzendt het bericht vervolgens naar Azure Monitor waarin een record wordt gemaakt.  

> [!NOTE]
> Azure Monitor biedt ondersteuning voor verzameling van berichten die worden verzonden door rsyslog of syslog-ng het volgende, waarbij rsyslog de standaard-daemon is. De standaard syslog-daemon op versie 5 van Red Hat Enterprise Linux, CentOS en Oracle Linux-versie (sysklog) wordt niet ondersteund voor de verzameling van syslog. Syslog-gegevens verzamelen uit deze versie van deze distributies de [rsyslog-daemon](http://rsyslog.com) moet worden geïnstalleerd en geconfigureerd ter vervanging van sysklog.
>
>

![Syslog-verzameling](media/data-sources-syslog/overview.png)

De volgende mogelijkheden worden ondersteund door de Syslog-collector:

* kern
* user
* mail
* daemon
* auth
* syslog
* lpr
* news
* uucp
* cron
* authpriv
* ftp
* local0 local7

Voor elke andere faciliteit [configureren van een gegevensbron van de aangepaste logboeken](data-sources-custom-logs.md) in Azure Monitor.
 
## <a name="configuring-syslog"></a>Syslog configureren
De Log Analytics-agent voor Linux worden alleen verzameld voor gebeurtenissen met de faciliteiten en ernstcategorieën die zijn opgegeven in de configuratie. U kunt Syslog configureren via Azure portal of door het beheer van configuratiebestanden op uw Linux-agents.

### <a name="configure-syslog-in-the-azure-portal"></a>Syslog configureren in Azure portal
Configureren van Syslog uit de [menu van de gegevens in de geavanceerde instellingen](agent-data-sources.md#configuring-data-sources). Deze configuratie wordt bezorgd in het configuratiebestand op elke Linux-agent.

U kunt een nieuwe opslagruimte toevoegen door te klikken en de naam te typen **+** . Voor elke faciliteit worden alleen berichten met de geselecteerde ernstcategorieën verzameld.  Raadpleeg de ernstcategorieën voor de specifieke faciliteit die u wenst te verzamelen. U kunt geen eventuele aanvullende criteria om berichten te filteren opgeven.

![Syslog configureren](media/data-sources-syslog/configure.png)

Standaard worden alle wijzigingen in de configuratie automatisch doorgegeven naar alle agents. Als u Syslog handmatig configureren op elke Linux-agent wilt, klikt u vervolgens het selectievakje *toepassen op de onderstaande configuratie op mijn Linux-machines*.

### <a name="configure-syslog-on-linux-agent"></a>Syslog configureren op Linux-agent
Wanneer de [Log Analytics-agent is geïnstalleerd op een Linux-client](../../azure-monitor/learn/quick-collect-linux-computer.md), het installeren van een standaard syslog-configuratiebestand dat bepaalt de opslagruimte en de ernst van de berichten die worden verzameld. Dit bestand om de configuratie te wijzigen, kunt u wijzigen. Het configuratiebestand is verschillend, afhankelijk van de Syslog-daemon die op de client is geïnstalleerd.

> [!NOTE]
> Als u de syslog-configuratie bewerken, moet u de syslog-daemon voor de wijzigingen pas van kracht opnieuw opstarten.
>
>

#### <a name="rsyslog"></a>rsyslog
Het configuratiebestand voor rsyslog bevindt zich in **/etc/rsyslog.d/95-omsagent.conf**. De Standaardinhoud worden hieronder weergegeven. Dit syslog-berichten verzonden vanaf de lokale agent voor alle installaties met een niveau van de waarschuwing of hoger worden verzameld.

    kern.warning       @127.0.0.1:25224
    user.warning       @127.0.0.1:25224
    daemon.warning     @127.0.0.1:25224
    auth.warning       @127.0.0.1:25224
    syslog.warning     @127.0.0.1:25224
    uucp.warning       @127.0.0.1:25224
    authpriv.warning   @127.0.0.1:25224
    ftp.warning        @127.0.0.1:25224
    cron.warning       @127.0.0.1:25224
    local0.warning     @127.0.0.1:25224
    local1.warning     @127.0.0.1:25224
    local2.warning     @127.0.0.1:25224
    local3.warning     @127.0.0.1:25224
    local4.warning     @127.0.0.1:25224
    local5.warning     @127.0.0.1:25224
    local6.warning     @127.0.0.1:25224
    local7.warning     @127.0.0.1:25224

U kunt een faciliteit verwijderen door het verwijderen van een sectie van het configuratiebestand. U kunt de ernstcategorieën die door het wijzigen van deze faciliteit vermelding voor een bepaalde opslagruimte worden verzameld beperken. Bijvoorbeeld, als u wilt beperken van de installatie van de gebruiker voor de berichten met een ernst van fout of hoger moet u deze regel van het configuratiebestand met de volgende wijzigen:

    user.error    @127.0.0.1:25224


#### <a name="syslog-ng"></a>Syslog-ng het volgende
Het configuratiebestand voor syslog-ng het volgende is een locatie op **/etc/syslog-ng/syslog-ng.conf**.  De Standaardinhoud worden hieronder weergegeven. Dit syslog-berichten verzonden vanaf de lokale agent voor alle faciliteiten en alle ernstcategorieën worden verzameld.   

    #
    # Warnings (except iptables) in one file:
    #
    destination warn { file("/var/log/warn" fsync(yes)); };
    log { source(src); filter(f_warn); destination(warn); };

    #OMS_Destination
    destination d_oms { udp("127.0.0.1" port(25224)); };

    #OMS_facility = auth
    filter f_auth_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(auth); };
    log { source(src); filter(f_auth_oms); destination(d_oms); };

    #OMS_facility = authpriv
    filter f_authpriv_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(authpriv); };
    log { source(src); filter(f_authpriv_oms); destination(d_oms); };

    #OMS_facility = cron
    filter f_cron_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(cron); };
    log { source(src); filter(f_cron_oms); destination(d_oms); };

    #OMS_facility = daemon
    filter f_daemon_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(daemon); };
    log { source(src); filter(f_daemon_oms); destination(d_oms); };

    #OMS_facility = kern
    filter f_kern_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(kern); };
    log { source(src); filter(f_kern_oms); destination(d_oms); };

    #OMS_facility = local0
    filter f_local0_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(local0); };
    log { source(src); filter(f_local0_oms); destination(d_oms); };

    #OMS_facility = local1
    filter f_local1_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(local1); };
    log { source(src); filter(f_local1_oms); destination(d_oms); };

    #OMS_facility = mail
    filter f_mail_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(mail); };
    log { source(src); filter(f_mail_oms); destination(d_oms); };

    #OMS_facility = syslog
    filter f_syslog_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(syslog); };
    log { source(src); filter(f_syslog_oms); destination(d_oms); };

    #OMS_facility = user
    filter f_user_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(user); };
    log { source(src); filter(f_user_oms); destination(d_oms); };

U kunt een faciliteit verwijderen door het verwijderen van een sectie van het configuratiebestand. U kunt de ernstcategorieën die voor een bepaalde opslagruimte worden verzameld door deze te verwijderen uit de lijst kunt beperken.  Bijvoorbeeld, als u wilt de installatie van de gebruiker voor de berichten alleen waarschuwingen en kritieke beperken, moet u die sectie van het configuratiebestand met de volgende wijzigen:

    #OMS_facility = user
    filter f_user_oms { level(alert,crit) and facility(user); };
    log { source(src); filter(f_user_oms); destination(d_oms); };


### <a name="collecting-data-from-additional-syslog-ports"></a>Verzamelen van gegevens van extra Syslog-poorten
De Log Analytics-agent luistert naar Syslog-berichten op de lokale client op poort 25224.  Wanneer de agent is geïnstalleerd, wordt een standaardconfiguratie voor syslog toegepast en gevonden in de volgende locatie:

* Rsyslog: `/etc/rsyslog.d/95-omsagent.conf`
* Syslog-ng het volgende: `/etc/syslog-ng/syslog-ng.conf`

U kunt het poortnummer dat door het maken van twee configuratiebestanden wijzigen: een FluentD-configuratiebestand en een rsyslog of syslog-ng-bestand, afhankelijk van de Syslog-daemon die u hebt geïnstalleerd.  

* Het FluentD-configuratiebestand moet een nieuw bestand in: `/etc/opt/microsoft/omsagent/conf/omsagent.d` en vervang de waarde in de **poort** vermelding met uw aangepaste poortnummer.

        <source>
          type syslog
          port %SYSLOG_PORT%
          bind 127.0.0.1
          protocol_type udp
          tag oms.syslog
        </source>
        <filter oms.syslog.**>
          type filter_syslog
        </filter>

* Voor rsyslog, moet u een nieuw configuratiebestand zich in maken: `/etc/rsyslog.d/` en vervang de waarde % SYSLOG_PORT % met uw aangepaste poortnummer.  

    > [!NOTE]
    > Als u deze waarde in het configuratiebestand wijzigen `95-omsagent.conf`, zal worden overschreven wanneer de agent van toepassing een standaardconfiguratie is.
    >

        # OMS Syslog collection for workspace %WORKSPACE_ID%
        kern.warning              @127.0.0.1:%SYSLOG_PORT%
        user.warning              @127.0.0.1:%SYSLOG_PORT%
        daemon.warning            @127.0.0.1:%SYSLOG_PORT%
        auth.warning              @127.0.0.1:%SYSLOG_PORT%

* De configuratie van de syslog-ng het volgende moet worden gewijzigd door het kopiëren van de voorbeeldconfiguratie hieronder wordt weergegeven en de aangepaste gewijzigde instellingen toe te voegen aan het einde van het configuratiebestand van de syslog-ng.conf zich in `/etc/syslog-ng/`. Voer **niet** gebruiken het standaardlabel **% WORKSPACE_ID % _oms** of **% WORKSPACE_ID_OMS**, definiëren van een aangepaste label voor het te onderscheiden van de wijzigingen.  

    > [!NOTE]
    > Als u de standaardwaarden in het configuratiebestand wijzigt, wordt deze overschreven wanneer de agent van toepassing een standaardconfiguratie is.
    >

        filter f_custom_filter { level(warning) and facility(auth; };
        destination d_custom_dest { udp("127.0.0.1" port(%SYSLOG_PORT%)); };
        log { source(s_src); filter(f_custom_filter); destination(d_custom_dest); };

Na het voltooien van de wijzigingen de Syslog- en de Log Analytics-agent-service opnieuw worden opgestart moet zodat de configuratiewijzigingen van kracht.   

## <a name="syslog-record-properties"></a>Eigenschappen van de Syslog-record
Syslog-records zijn een type **Syslog** en hebben de eigenschappen in de volgende tabel.

| Eigenschap | Beschrijving |
|:--- |:--- |
| Computer |De computer waarop de gebeurtenis is verzameld. |
| Opslagruimte |Hiermee definieert u het gedeelte van het systeem dat het bericht wordt gegenereerd. |
| HostIP |IP-adres van het systeem het bericht te verzenden. |
| Hostnaam |De naam van het systeem het bericht te verzenden. |
| Foutcode |Ernst van de gebeurtenis. |
| SyslogMessage |Tekst van het bericht. |
| ProcessID |ID van het proces dat het bericht wordt gegenereerd. |
| eventTime |Datum en tijd waarop de gebeurtenis is gegenereerd. |

## <a name="log-queries-with-syslog-records"></a>Logboeken-query's met Syslog-records
De volgende tabel bevat voorbeelden van Logboeken-query's die Syslog-records ophalen.

| Query’s uitvoeren | Description |
|:--- |:--- |
| Syslog |Alle Syslogs. |
| Syslog &#124; waar SeverityLevel == "error" |Alle Syslog-records met de ernst van fout. |
| Syslog &#124; summarize AggregatedValue = count() by Computer |Telling van Syslog-records door de computer. |
| Syslog &#124; summarize AggregatedValue = count() by faciliteit |Telling van Syslog-records door de faciliteit. |

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [query's bijgehouden](../../azure-monitor/log-query/log-query-overview.md) om de gegevens die worden verzameld van gegevensbronnen en oplossingen te analyseren.
* Gebruik [aangepaste velden](../../azure-monitor/platform/custom-fields.md) parseren van gegevens van syslog-records in afzonderlijke velden.
* [Linux-agents configureren](../../azure-monitor/learn/quick-collect-linux-computer.md) voor het verzamelen van andere typen gegevens.
