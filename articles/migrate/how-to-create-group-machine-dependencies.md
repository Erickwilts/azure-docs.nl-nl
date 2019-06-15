---
title: Groep machines met behulp van machineafhankelijkheden met Azure Migrate | Microsoft Docs
description: Beschrijft hoe u een evaluatie met behulp van machineafhankelijkheden met de service Azure Migrate maakt.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 12/05/2018
ms.author: raynew
ms.openlocfilehash: af47678b19209936aed86c132a8a3f400c3a7e8f
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60596767"
---
# <a name="group-machines-using-machine-dependency-mapping"></a>Machines groeperen met behulp van machine-afhankelijkheidstoewijzing

In dit artikel wordt beschreven hoe u een groep machines voor [Azure Migrate](migrate-overview.md) beoordelen door te visualiseren afhankelijkheden van machines. Doorgaans gebruikt u deze methode als u wilt beoordelen van groepen van virtuele machines met een hogere mate van vertrouwen door machineafhankelijkheden cross controleren voordat u een evaluatie uitvoert. Visualisatie van afhankelijkheden kunt u effectief plan uw migratie naar Azure. Hiermee kunt u ervoor zorgen dat er niets is overgebleven en onverwachte storingen niet wordt uitgevoerd wanneer u naar Azure migreert. U kunt alle onderling afhankelijk zijn systemen die tegelijk migreren en bepalen of een actief systeem gebruikers nog steeds bedient of een kandidaat is voor het buiten gebruik stellen in plaats van de migratie moeten detecteren.

> [!NOTE]
> De functie voor visualisatie van afhankelijkheden is niet beschikbaar in Azure Government.

## <a name="prepare-for-dependency-visualization"></a>Voorbereiden voor visualisatie van afhankelijkheden
Azure Migrate maakt gebruik van de oplossing Serviceoverzicht in Logboeken van Azure Monitor om in te schakelen visualisatie van afhankelijkheden van machines.

### <a name="associate-a-log-analytics-workspace"></a>Een Log Analytics-werkruimte koppelen
Als u wilt gebruikmaken van de visualisatie van afhankelijkheden, moet u een Log Analytics-werkruimte, nieuwe of bestaande koppelen met een Azure Migrate-project. U kunt alleen maken of koppelen van een werkruimte in hetzelfde abonnement waar het migratieproject is gemaakt.

- Een Log Analytics-werkruimte koppelen aan een project in **overzicht**, gaat u naar **Essentials** sectie van het project, klikt u op **configuratie is vereist**

    ![Log Analytics-werkruimte koppelen](./media/concepts-dependency-visualization/associate-workspace.png)

- Tijdens het koppelen van een werkruimte, ontvangt u de optie voor een nieuwe werkruimte maken of een bestaande werkruimte koppelen:
  - Wanneer u een nieuwe werkruimte maakt, moet u een naam opgeven voor de werkruimte. De werkruimte wordt vervolgens gemaakt in een regio in dezelfde [Azure-Geografie](https://azure.microsoft.com/global-infrastructure/geographies/) als het migratieproject.
  - Wanneer u een bestaande werkruimte koppelen, kunt u kiezen uit alle werkruimten die beschikbaar is in hetzelfde abonnement als de migration-project. Houd er rekening mee dat alleen deze werkruimten worden weergegeven die zijn gemaakt in een regio waar [Serviceoverzicht wordt ondersteund](https://docs.microsoft.com/azure/azure-monitor/insights/service-map-configure#supported-azure-regions). Als u een werkruimte te koppelen, zorg ervoor dat u 'Lezer' toegang tot de werkruimte hebben.

> [!NOTE]
> U kunt de werkruimte die is gekoppeld aan een migratieproject niet wijzigen.

### <a name="download-and-install-the-vm-agents"></a>De VM-agents downloaden en installeren
Wanneer u een werkruimte configureert, moet u om te downloaden en installeren van agents op elke on-premises computer die u wilt evalueren. Bovendien, als u computers geen verbinding met internet hebt, moet u om te downloaden en installeren [Log Analytics gateway](../azure-monitor/platform/gateway.md) hierop.

1. In **overzicht**, klikt u op **beheren** > **Machines**, en selecteer de vereiste machine.
2. In de **afhankelijkheden** kolom, klikt u op **agents installeren**.
3. Op de **afhankelijkheden** pagina, downloaden en installeren van de Microsoft Monitoring Agent (MMA) en de agent voor afhankelijkheden op elke virtuele machine die u wilt evalueren.
4. Kopieer de werkruimte-id en -sleutel. U hebt deze nodig wanneer u de MMA op de on-premises machine installeren.

> [!NOTE]
> Voor het automatiseren van de installatie van agents kunt u een implementatie-hulpprogramma, zoals System Center Configuration Manager gebruiken of gebruik onze hulpprogramma partner [Intigua](https://www.intigua.com/getting-started-intigua-for-azure-migration), waarvoor een implementatieoplossing agent voor Azure Migrate.

### <a name="install-the-mma"></a>De MMA installeren

#### <a name="install-the-agent-on-a-windows-machine"></a>Installeer de agent op een Windows-machine

De agent installeren op een Windows-machine:

1. Dubbelklik op de gedownloade agent.
2. Klik op de pagina **Welkom** op **Volgende**. Klik op de pagina **Licentievoorwaarden** op **Akkoord** om de licentie te accepteren.
3. In **doelmap**, houden of wijzigen van de standaardinstallatiemap > **volgende**.
4. In **installatieopties voor Agent**, selecteer **Azure Log Analytics** > **volgende**.
5. Klik op **toevoegen** om toe te voegen een nieuwe Log Analytics-werkruimte. Plak in de werkruimte-ID en de sleutel die u hebt gekopieerd uit de portal. Klik op **volgende**.

U kunt de agent installeren vanaf de opdrachtregel of met behulp van een geautomatiseerde methode, zoals System Center Configuration Manager. [Meer informatie](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#install-and-configure-agent) over het gebruik van deze methoden voor het installeren van de MMA-agent.

#### <a name="install-the-agent-on-a-linux-machine"></a>Installeer de agent op een Linux-machine

De agent installeren op een Linux-machine:

1. De geschikte bundel (x86 of x64) overbrengen naar uw Linux-computer met scp/sftp.
2. De bundel installeren met behulp van de--install-argument.

    ```sudo sh ./omsagent-<version>.universal.x64.sh --install -w <workspace id> -s <workspace key>```

[Meer informatie](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-linux-operating-systems) over de lijst met ondersteuning voor Linux-besturingssystemen door MMA.

#### <a name="install-the-agent-on-a-machine-monitored-by-scom"></a>Installeer de agent op een computer die wordt bewaakt door SCOM

Voor machines die worden bewaakt door System Center Operations Manager 2012 R2 of hoger, is er niet nodig voor het installeren van de MMA-agent. Serviceoverzicht is een geïntegreerd met SCOM die gebruikmaakt van de MMA SCOM om de vereiste afhankelijkheid-gegevens te verzamelen. U kunt de integratie met behulp van de richtlijnen inschakelen [hier](https://docs.microsoft.com/azure/azure-monitor/insights/service-map-scom#prerequisites). Merk echter op dat de agent voor afhankelijkheden moet op deze computers geïnstalleerd.


### <a name="install-the-dependency-agent"></a>De afhankelijkheidsagent installeren
1. Voor het installeren van de agent voor afhankelijkheden op een Windows-machine, dubbelklik op het setup-bestand en volg de wizard.
2. Voor het installeren van de agent voor afhankelijkheden op een Linux-machine, installeert u toegangspunt met de volgende opdracht:

    ```sh InstallDependencyAgent-Linux64.bin```

Meer informatie over de ondersteuning van de agent afhankelijkheid voor de [Windows](../azure-monitor/insights/service-map-configure.md#supported-windows-operating-systems) en [Linux](../azure-monitor/insights/service-map-configure.md#supported-linux-operating-systems) besturingssystemen.

[Meer informatie](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#installation-script-examples) over hoe u scripts gebruiken kunt voor het installeren van de agent voor afhankelijkheden.


## <a name="create-a-group"></a>Een groep maken

1. Nadat u de agents hebt geïnstalleerd, gaat u naar de portal en klik op **beheren** > **Machines**.
2. Zoeken naar de machine waarop u de agents hebt geïnstalleerd.
3. De **afhankelijkheden** kolom voor de machine moet nu worden weergegeven als **afhankelijkheden weergeven**. Klik op de kolom om de afhankelijkheden van de machine weer te geven.
4. De afhankelijkheidskaart voor de machine wordt de volgende gegevens:
    - Inkomende (Clients) en uitgaande (Server) TCP-verbindingen naar/van de machine
        - De afhankelijke machines waarop geen de MMA- en afhankelijkheidsmonitors agent is geïnstalleerd zijn gegroepeerd op poortnummers
        - De afhankelijke machines waarvoor de MMA en de agent voor afhankelijkheden geïnstalleerd worden als afzonderlijke vakken weergegeven
    - Processen die op de machine, kunt u elk VM-vak om de processen weer te geven uitbreiden
    - Eigenschappen, zoals Fully Qualified Domain Name, besturingssysteem, enzovoort van MAC-adres van elke machine die u kunt klikken op elke VM-vak om deze gegevens weer te geven

      ![Machineafhankelijkheden weergeven](./media/how-to-create-group-machine-dependencies/machine-dependencies.png)

4. U kunt zoeken op afhankelijkheden verschillende tijdsduren door te klikken op de duur in het bereik tijdlabel. Het bereik is standaard een uur. U kunt het tijdsbereik wijzigen of geef de begin- en einddatums en duur.

   > [!NOTE]
   >    De visualisatie van afhankelijkheden gebruikersinterface biedt op dit moment geen ondersteuning voor selectie van een periode die langer dan een uur. Gebruik Azure Monitor meldt zich aan [query uitvoeren op de afhankelijkheidsgegevens](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies) gedurende een langere periode.

5. Nadat u hebt vastgesteld dat afhankelijke machines die u wilt groeperen, gebruikt u Ctrl + klikken om te selecteren van meerdere virtuele machines op de kaart en klikt u op **machines groeperen**.
6. Geef een groepsnaam op. Controleer of dat de afhankelijke machines zijn gedetecteerd door Azure Migrate.

    > [!NOTE]
    > Als een afhankelijke virtuele machine wordt niet gedetecteerd door Azure Migrate, kunt u deze niet toevoegen aan de groep. Als u wilt deze computers toevoegen aan de groep, moet u het detectieproces opnieuw uitvoeren met het juiste bereik in de vCenter-Server en zorg ervoor dat de machine is gedetecteerd door Azure Migrate.  

7. Als u wilt om een evaluatie voor deze groep te maken, selecteert u het selectievakje in om een nieuwe evaluatie voor de groep te maken.
8. Klik op **OK** om op te slaan van de groep.

Nadat de groep is gemaakt, is het aanbevolen om agents te installeren op alle computers van de groep en de groep verfijnen door te visualiseren van de afhankelijkheid van de hele groep.

## <a name="query-dependency-data-from-azure-monitor-logs"></a>Query uitvoeren op afhankelijkheidsgegevens van Azure Monitor-Logboeken

Afhankelijkheidsgegevens die zijn vastgelegd door Serviceoverzicht is beschikbaar voor het uitvoeren van query's in de Log Analytics-werkruimte die is gekoppeld aan uw Azure Migrate-project. [Meer informatie](https://docs.microsoft.com/azure/azure-monitor/insights/service-map#log-analytics-records) logboeken over de Serviceoverzicht-gegevenstabellen opvragen in Azure Monitor. 

De Kusto-query's uitvoeren:

1. Nadat u de agents hebt geïnstalleerd, gaat u naar de portal en klik op **overzicht**.
2. In **overzicht**, gaat u naar **Essentials** sectie van het project en klik op de naam van de werkruimte is opgegeven bij **OMS-werkruimte**.
3. Klik op de pagina van de Log Analytics-werkruimte **algemene** > **logboeken**.
4. Schrijf de query om afhankelijkheidsgegevens met behulp van Azure Monitor-logboeken te verzamelen. Voorbeeldquery's niet vinden in de volgende sectie.
5. Voer de query uit door te klikken op uitvoeren. 

[Meer informatie](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal) over het schrijven van Kusto-query's. 

### <a name="sample-azure-monitor-logs-queries"></a>Voorbeeld van Azure Monitor logboekregistratie van query 's

Hieronder vindt u voorbeeld-query's die u gebruiken kunt om afhankelijkheidsgegevens te extraheren. U kunt de query's om uit te pakken van uw voorkeur gegevenspunten wijzigen. Een volledige lijst van de velden in gegevensrecords van afhankelijkheid is beschikbaar [hier](https://docs.microsoft.com/azure/azure-monitor/insights/service-map#log-analytics-records). Zoek meer voorbeeldquery's [hier](https://docs.microsoft.com/azure/azure-monitor/insights/service-map#sample-log-searches).

#### <a name="summarize-inbound-connections-on-a-set-of-machines"></a>Binnenkomende verbindingen op een set van machines samenvatten

Houd er rekening mee dat de records in de tabel voor de verbindingsgegevens, VMConnection, afzonderlijke fysieke netwerkverbindingen niet weergeven. Verbindingen van meerdere fysieke netwerk zijn gegroepeerd in een logische verbinding. [Meer informatie](https://docs.microsoft.com/azure/azure-monitor/insights/service-map#connections) over hoe fysieke netwerkverbinding gegevens worden samengevoegd in één logische records in VMConnection. 

```
// the machines of interest
let ips=materialize(ServiceMapComputer_CL
| summarize ips=makeset(todynamic(Ipv4Addresses_s)) by MonitoredMachine=ResourceName_s
| mvexpand ips to typeof(string));
let StartDateTime = datetime(2019-03-25T00:00:00Z);
let EndDateTime = datetime(2019-03-30T01:00:00Z); 
VMConnection
| where Direction == 'inbound' 
| where TimeGenerated > StartDateTime and TimeGenerated  < EndDateTime
| join kind=inner (ips) on $left.DestinationIp == $right.ips
| summarize sum(LinksEstablished) by Computer, Direction, SourceIp, DestinationIp, DestinationPort
```

#### <a name="summarize-volume-of-data-sent-and-received-on-inbound-connections-between-a-set-of-machines"></a>Samenvatting van de hoeveelheid gegevens verzonden en ontvangen voor binnenkomende verbindingen tussen een set van machines

```
// the machines of interest
let ips=materialize(ServiceMapComputer_CL
| summarize ips=makeset(todynamic(Ipv4Addresses_s)) by MonitoredMachine=ResourceName_s
| mvexpand ips to typeof(string));
let StartDateTime = datetime(2019-03-25T00:00:00Z);
let EndDateTime = datetime(2019-03-30T01:00:00Z); 
VMConnection
| where Direction == 'inbound' 
| where TimeGenerated > StartDateTime and TimeGenerated  < EndDateTime
| join kind=inner (ips) on $left.DestinationIp == $right.ips
| summarize sum(BytesSent), sum(BytesReceived) by Computer, Direction, SourceIp, DestinationIp, DestinationPort
```

## <a name="next-steps"></a>Volgende stappen

- [Meer informatie](https://docs.microsoft.com/azure/migrate/resources-faq#dependency-visualization) over de veelgestelde vragen over de visualisatie van afhankelijkheden.
- [Informatie over hoe](how-to-create-group-dependencies.md) aan de groep verfijnen door groepsafhankelijkheden visualiseren.
- [Meer informatie](concepts-assessment-calculation.md) over hoe evaluaties worden berekend.
