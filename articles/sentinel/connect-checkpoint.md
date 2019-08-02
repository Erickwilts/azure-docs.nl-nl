---
title: Verbinding maken met controle punt gegevens naar Azure Sentinel preview | Microsoft Docs
description: Meer informatie over het verbinden van Check Point-gegevens met Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 3229233d-400d-4971-8d76-eaa0d6591d75
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/31/2019
ms.author: rkarlin
ms.openlocfilehash: ea9cb7997f86c10bb09c01dfc345aaeeef46708a
ms.sourcegitcommit: 13d5eb9657adf1c69cc8df12486470e66361224e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2019
ms.locfileid: "68679339"
---
# <a name="connect-your-check-point-appliance"></a>Uw Check Point-toestel aansluiten

> [!IMPORTANT]
> Azure Sentinel is momenteel beschikbaar als open bare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

U kunt Azure Sentinel verbinden met elk Check Point-apparaat door de logboek bestanden op te slaan als syslog CEF. Dankzij de integratie met Azure Sentinel kunt u eenvoudig analyses en query's uitvoeren op de gegevens van het logboek bestand vanaf het controle punt. Zie [verbinding maken met CEF-apparaten](connect-common-event-format.md)voor meer informatie over de manier waarop Azure Sentinel CEF-gegevens opneemt.

> [!NOTE]
> Gegevens worden opgeslagen in de geografische locatie van de werk ruimte waarop u Azure Sentinel uitvoert.

## <a name="step-1-connect-your-check-point-appliance-using-an-agent"></a>Stap 1: Uw Check Point-apparaat verbinden met een agent

Als u uw Check Point-apparaat wilt verbinden met Azure Sentinel, moet u een agent implementeren op een toegewezen machine (VM of on-premises) ter ondersteuning van de communicatie tussen het apparaat en de Azure-Sentinel. 

U kunt de agent ook hand matig implementeren op een bestaande Azure-VM, op een virtuele machine in een andere Cloud of op een on-premises machine.

> [!NOTE]
> Zorg ervoor dat u de beveiliging van de computer configureert op basis van het beveiligings beleid van uw organisatie. U kunt bijvoorbeeld uw netwerk zodanig configureren dat het wordt uitgelijnd met het beveiligings beleid van uw bedrijfs netwerk en de poorten en protocollen in de daemon wijzigen om af te stemmen op uw vereisten. 

Zie [verbinding maken met gegevens bronnen](connect-data-sources.md)om een netwerk diagram van beide opties weer te geven.

### <a name="deploy-the-agent"></a>De agent implementeren
1. Klik in de Azure-Sentinel-Portal op **gegevens connectors** en selecteer **Check Point** en **Open vervolgens connector-pagina**. 

1. Onder **de syslog-agent downloaden en installeren**, selecteert u het type computer, ofwel Azure ofwel on-premises. 
1. Selecteer in het scherm **virtuele machines** dat wordt geopend de computer die u wilt gebruiken en klik op **verbinden**.
1. Als u **agent voor virtuele Azure Linux-machines downloaden en installeren**kiest, selecteert u de computer en klikt u op **verbinding maken**. Als u ervoor kiest **om de agent te downloaden en te installeren voor niet-Azure Linux virtuele machines**, voert u in het scherm **direct agent** het script uit onder **down load en onboarding-agent voor Linux**.
 
1. Stel in het scherm connector onder **syslog configureren en door sturen**in of uw syslog-daemon **rsyslog. d** of **syslog-ng**is. 
1. Kopieer deze opdrachten en voer deze uit op uw apparaat:
     - Als u rsyslog hebt geselecteerd. d:
            
       1. Vertel de syslog-daemon om te Luis teren local_4 en naar ' Check Point ' en om de syslog-berichten te verzenden naar de Azure Sentinel-agent met behulp van poort 25226. `sudo bash -c "printf 'local4.debug  @127.0.0.1:25226\n\n:msg, contains, \"Check Point\"  @127.0.0.1:25226' > /etc/rsyslog.d/security-config-omsagent.conf"`
           
       2. Down load en installeer het [security_events-configuratie bestand](https://aka.ms/asi-syslog-config-file-linux) dat de syslog-agent configureert om te Luis teren op poort 25226. `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"`Waar {0} moet worden vervangen door de GUID van uw werk ruimte.
           
       1. De syslog-daemon opnieuw starten`sudo service rsyslog restart`
             
    - Als u syslog-aardgas hebt geselecteerd:

         1. Vertel de syslog-daemon om te Luis teren local_4 en naar ' Check Point ' en om de syslog-berichten te verzenden naar de Azure Sentinel-agent met behulp van poort 25226. `sudo bash -c "printf 'filter f_local4_oms { facility(local4); };\n  destination security_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_local4_oms); destination(security_oms); };\n\nfilter f_msg_oms { match(\"Check Point\" value(\"MESSAGE\")); };\n  destination security_msg_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_msg_oms); destination(security_msg_oms); };' > /etc/syslog-ng/security-config-omsagent.conf"`
         2. Down load en installeer het [security_events-configuratie bestand](https://aka.ms/asi-syslog-config-file-linux) dat de syslog-agent configureert om te Luis teren op poort 25226. `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"`Waar {0} moet worden vervangen door de GUID van uw werk ruimte.

        3. De syslog-daemon opnieuw starten`sudo service syslog-ng restart`
2. Start de syslog-agent opnieuw met behulp van deze opdracht:`sudo /opt/microsoft/omsagent/bin/service_control restart [{workspace GUID}]`
1. Controleer of er geen fouten zijn in het Agent logboek door de volgende opdracht uit te voeren:`tail /var/opt/microsoft/omsagent/log/omsagent.log`


 
## <a name="step-2-forward-check-point-logs-to-the-syslog-agent"></a>Stap 2: Controle logboeken naar de syslog-agent sturen

Configureer uw Check Point-toestel voor het door sturen van syslog-berichten in de CEF-indeling naar uw Azure-werk ruimte via de syslog-agent.

1. Ga naar [controle punt logboek exporteren](https://aka.ms/asi-syslog-checkpoint-forwarding).
2. Schuif omlaag naar de **basis implementatie** en volg de instructies voor het instellen van de verbinding, met behulp van de volgende richt lijnen:
   - Stel de **syslog-poort** in op **514** of de poort die u op de agent hebt ingesteld.
     - Vervang de **naam** en het IP-adres van de **doel server** in de CLI met de naam van de syslog-agent en het IP-adres.
     - Stel de indeling in op **CEF**.
3. Als u versie R 77.30 of R 80.10 gebruikt, schuift u omhoog naar **installaties** en volgt u de instructies voor het installeren van een logboek-export programma voor uw versie.
 
## <a name="step-3-validate-connectivity"></a>Stap 3: Connectiviteit valideren

Het kan 20 minuten duren voordat uw logboeken in Log Analytics worden weer gegeven. 

1. Zorg ervoor dat u de juiste functie gebruikt. De faciliteit moet hetzelfde zijn in uw apparaat en in azure Sentinel. U kunt controleren welk faciliteit bestand u gebruikt in azure Sentinel en dit wijzigen in het bestand `security-config-omsagent.conf`. 

2. Zorg ervoor dat uw logboeken de juiste poort in de syslog-agent krijgen. Voer deze opdracht uit op de computer van de syslog-agent: `tcpdump -A -ni any  port 514 -vv`Met deze opdracht wordt de logboeken weer gegeven die van het apparaat naar de syslog-machine worden gestreamd. Zorg ervoor dat Logboeken worden ontvangen van het bron apparaat op de juiste poort en de juiste faciliteit.

3. Zorg ervoor dat de logboeken die u verzendt, voldoen aan [RFC 3164](https://tools.ietf.org/html/rfc3164).

4. Op de computer waarop de syslog-agent wordt uitgevoerd, moet u ervoor zorgen dat deze poorten 514, 25226 zijn geopend `netstat -a -n:`en Luis teren met behulp van de opdracht. Meer informatie over het gebruik van deze opdracht vindt u op de [pagina netstat (8)-Linux-man](https://linux.die.net/man/8/netstat). Als de service goed luistert, ziet u dit:

   ![Azure-Sentinel-poorten](./media/connect-cef/ports.png) 

5. Zorg ervoor dat de daemon is ingesteld om te Luis teren op poort 514, waarop u de logboeken verzendt.
    - Voor rsyslog:<br>Zorg ervoor dat het bestand `/etc/rsyslog.conf` de volgende configuratie bevat:

           # provides UDP syslog reception
           module(load="imudp")
           input(type="imudp" port="514")
        
           # provides TCP syslog reception
           module(load="imtcp")
           input(type="imtcp" port="514")

      Zie [voor meer informatie imudp: Invoer module](https://www.rsyslog.com/doc/v8-stable/configuration/modules/imudp.html#imudp-udp-syslog-input-module) voor UDP syslog [en imtcp: Invoer module voor TCP syslog](https://www.rsyslog.com/doc/v8-stable/configuration/modules/imtcp.html#imtcp-tcp-syslog-input-module)

   - Voor syslog-aardgas:<br>Zorg ervoor dat het bestand `/etc/syslog-ng/syslog-ng.conf` de volgende configuratie bevat:

           # source s_network {
            network( transport(UDP) port(514));
             };
    Zie [syslog-ng Open Source Edition 3,16-Administration Guide (Engelstalig](https://www.syslog-ng.com/technical-documents/doc/syslog-ng-open-source-edition/3.16/administration-guide/19#TOPIC-956455)) voor meer informatie.

1. Controleer of er communicatie is tussen de syslog-daemon en de agent. Voer deze opdracht uit op de computer van de syslog-agent: `tcpdump -A -ni any  port 25226 -vv`Met deze opdracht wordt de logboeken weer gegeven die van het apparaat naar de syslog-machine worden gestreamd. Zorg ervoor dat de logboeken ook worden ontvangen op de agent.

6. Als bij beide opdrachten geslaagde resultaten worden weer gegeven, controleert u Log Analytics om te zien of uw logboeken arriveren. Alle gebeurtenissen die vanaf deze apparaten worden gestreamd, worden in onbewerkte vorm in log Analytics onder `CommonSecurityLog` type weer gegeven.

7. Als u wilt controleren of er fouten zijn of als de logboeken niet arriveren, kijkt u in `tail /var/opt/microsoft/omsagent/<workspace id>/log/omsagent.log`. Als wordt aangegeven dat er fouten in de logboek indeling niet overeenkomen, gaat u `/etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` naar het bestand `security_events.conf`en kijkt u of uw logboeken overeenkomen met de regex-indeling die u ziet in dit bestand.

8. Zorg ervoor dat de standaard grootte van het syslog-bericht is beperkt tot 2048 bytes (2 KB). Als de logboeken te lang zijn, werkt u het security_events. conf bij met behulp van de volgende opdracht:`message_length_limit 4096`

4. Zorg ervoor dat u deze opdrachten uitvoert:
  
   - Als u syslog-ng gebruikt, voert u deze opdrachten uit (Houd er rekening mee dat de syslog-agent opnieuw wordt gestart):

         sudo bash -c "printf 'filter f_local4_oms { facility(local4); };\n  destination security_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_local4_oms); destination(security_oms); };\n\nfilter f_msg_oms { match(\"Check Point\" value(\"MESSAGE\")); };\n  destination security_msg_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_msg_oms); destination(security_msg_oms); };' > /etc/syslog-ng/security-config-omsagent.conf"
        De syslog-daemon opnieuw starten:`sudo service syslog-ng restart`

   - Als u rsyslog gebruikt, voert u deze opdrachten uit (Houd er rekening mee dat de syslog-agent opnieuw wordt opgestart): 

         sudo bash -c "printf 'local4.debug @127.0.0.1:25226\n\n:msg, contains, "Check Point" @127.0.0.1:25226' > /etc/rsyslog.d/security-config-omsagent.conf"
     De syslog-daemon opnieuw starten:`sudo service rsyslog restart`




## <a name="next-steps"></a>Volgende stappen
In dit document hebt u geleerd hoe u apparaten van Check Point verbindt met Azure Sentinel. Raadpleeg de volgende artikelen voor meer informatie over Azure Sentinel:
- Meer informatie over hoe u [inzicht krijgt in uw gegevens en mogelijke bedreigingen](quickstart-get-visibility.md).
- Ga aan de slag [met het detecteren van bedreigingen met Azure Sentinel](tutorial-detect-threats.md).

