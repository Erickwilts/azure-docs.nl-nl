---
title: Azure Recovery Services-kluizen en-servers beheren
description: Beheer taken en waarschuwingen in een Azure Recovery Services kluis.
author: dcurwin
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 07/08/2019
ms.author: dacurwin
ms.openlocfilehash: 7e7312f942103125217c1f61ae8fe8007a49529b
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/12/2019
ms.locfileid: "68954765"
---
# <a name="monitor-and-manage-recovery-services-vaults"></a>Recovery Services-kluizen beheren en controleren

In dit artikel wordt uitgelegd hoe u het **overzichts** dashboard van Recovery Services kluis kunt gebruiken om uw Recovery Services kluizen te controleren en te beheren. Wanneer u een Recovery Services kluis in de lijst opent, wordt het overzichts dashboard voor de geselecteerde kluis geopend. Het dash board biedt verschillende details over de kluis. Er zijn *tegels* die worden weer gegeven: de status van kritieke en waarschuwings meldingen, taken in uitvoering en mislukte back-ups en de hoeveelheid lokaal redundante opslag (LRS) en geo redundante opslag (GRS). Als u een back-up maakt van virtuele Azure-machines in de kluis, worden in de [tegel **status van back-up** voor het controleren alle kritieke items weer gegeven](https://azure.microsoft.com/blog/azure-vm-backup-pre-checks/). De volgende afbeelding is het **overzichts** dashboard voor **Contoso-kluis**. Op de tegel **back-** upitems ziet u dat er negen items zijn geregistreerd bij de kluis.

![Recovery Services-kluis dashboard](./media/backup-azure-manage-windows-server/rs-vault-blade.png)

De vereisten voor dit artikel zijn: een Azure-abonnement, een Recovery Services kluis en dat er ten minste één back-upitem is geconfigureerd voor de kluis.

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]


## <a name="open-a-recovery-services-vault"></a>Een Recovery Services kluis openen

Open de kluis om waarschuwingen te controleren of beheer gegevens over een Recovery Services kluis weer te geven.

1. Meld u aan bij de [Azure Portal](https://portal.azure.com/) met uw Azure-abonnement.

2. Klik in de portal op **alle services**.

   ![Lijst met Recovery Services kluizen openen-stap 1](./media/backup-azure-manage-windows-server/open-rs-vault-list.png)

3. Typ in het dialoog venster **alle services** het **Recovery Services**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Wanneer de optie **Recovery Services kluizen** wordt weer gegeven, klikt u hierop om de lijst met Recovery Services kluizen in uw abonnement te openen.

    ![Een Recovery Services-kluis maken, stap 1](./media/backup-azure-manage-windows-server/list-of-rs-vaults.png) <br/>

4. Klik in de lijst met kluizen op een kluis om het dash board **overzicht** te openen.

    ![Recovery Services-kluis dashboard](./media/backup-azure-manage-windows-server/rs-vault-blade.png) <br/>

    Het overzichts dashboard gebruikt tegels om waarschuwingen en back-uptaak gegevens te bieden.

## <a name="monitor-backup-jobs-and-alerts"></a>Back-uptaken en waarschuwingen bewaken

Het overzichts dashboard Recovery Services kluis bevat tegels voor controle-en gebruiks informatie. De tegels in de bewakings sectie tonen kritieke en waarschuwings meldingen en taken in uitvoering en mislukt. Klik op een bepaalde waarschuwing of taak om het menu met back-upwaarschuwingen of back-uptaken te openen, gefilterd op die taak of waarschuwing.

![Back-updashboard taken](./media/backup-azure-manage-windows-server/monitor-dashboard-tiles-warning.png)

De sectie bewaking toont de resultaten van de query's van vooraf gedefinieerde **back-** upwaarschuwingen en **back-uptaken** . De bewakings tegels bieden actuele informatie over:

* Kritieke en waarschuwingen voor back-uptaken (in de afgelopen 24 uur)
* Status vooraf controleren voor Azure-Vm's: Raadpleeg de [back-up-blog op pre-check status van back-ups](https://azure.microsoft.com/blog/azure-vm-backup-pre-checks/)voor volledige informatie over de status van de controle vooraf.
* De back-uptaken worden uitgevoerd en taken die in de afgelopen 24 uur zijn mislukt.

De gebruiks tegels bieden:

* Het aantal back-upitems dat is geconfigureerd voor de kluis.
* De Azure-opslag (gescheiden door LRS en GRS) die wordt gebruikt door de kluis.

Klik op de tegels (behalve back-upopslag) om het bijbehorende menu te openen. In de bovenstaande afbeelding ziet u de tegel waarschuwingen voor back-ups met drie kritieke waarschuwingen. Als u op de rij met kritieke waarschuwingen in de tegel back-upwaarschuwingen klikt, worden de back-upwaarschuwingen gefilterd op kritieke waarschuwingen weer gegeven.

![Menu met back-upwaarschuwingen gefilterd op kritieke waarschuwingen](./media/backup-azure-manage-windows-server/critical-backup-alerts.png)

Het menu back-upwaarschuwingen, in de bovenstaande afbeelding, wordt gefilterd op: De status is actief, ernst is kritiek en tijd is de afgelopen 24 uur.

## <a name="manage-backup-alerts"></a>Back-upwaarschuwingen beheren

Als u het menu back-upwaarschuwingen wilt openen, klikt u in het menu Recovery Services kluis op **back-** upwaarschuwingen.

![Waarschuwingen voor back-ups](./media/backup-azure-manage-windows-server/backup-alerts-menu.png)

Het rapport back-upwaarschuwingen bevat de waarschuwingen voor de kluis.

![Waarschuwingen voor back-ups](./media/backup-azure-manage-windows-server/backup-alerts.png)

### <a name="alerts"></a>Waarschuwingen

In de lijst met waarschuwingen voor back-ups worden de geselecteerde gegevens voor de gefilterde waarschuwingen weer gegeven. In het menu waarschuwingen voor back-ups kunt u filteren op kritieke of waarschuwings meldingen.

| Waarschuwings niveau | Gebeurtenissen die waarschuwingen genereren |
| ----------- | ----------- |
| Kritiek | U ontvangt kritieke waarschuwingen wanneer: Back-uptaken mislukken, herstel taken mislukken en wanneer u de beveiliging op een server stopt, maar de gegevens behouden.|
| Waarschuwing | Er worden waarschuwingen weer gegeven wanneer: Back-uptaken worden voltooid met waarschuwingen, bijvoorbeeld wanneer er geen back-up wordt gemaakt van minder dan 100 bestanden als gevolg van beschadigde problemen of wanneer er een back-up van meer dan 1.000.000 bestanden wordt gemaakt. |
| Informatief | Er zijn momenteel geen informatieve waarschuwingen in gebruik. |

### <a name="viewing-alert-details"></a>Waarschuwings details weer geven

Het rapport back-upwaarschuwingen houdt acht informatie over elke waarschuwing bij. Gebruik de knop **kolommen kiezen** om de details in het rapport te bewerken.

![Waarschuwingen voor back-ups](./media/backup-azure-manage-windows-server/backup-alerts.png)

Standaard worden alle details, met uitzonde ring van de **laatste tijdstippen**, in het rapport weer gegeven.

* Waarschuwing
* Back-upitem
* Beveiligde server
* Severity
* Duration
* Aanmaaktijd
* Status
* Laatste tijdstip waarop de gebeurtenis is opgetreden

### <a name="change-the-details-in-alerts-report"></a>De details in het waarschuwingen rapport wijzigen

1. Als u de rapport gegevens wilt wijzigen, klikt u in het menu **waarschuwingen voor back-up** op **kolommen kiezen**.

   ![Waarschuwingen voor back-ups](./media/backup-azure-manage-windows-server/alerts-menu-choose-columns.png)

   Het menu **kolommen selecteren** wordt geopend.

2. Kies in het menu **kolommen selecteren** de details die u in het rapport wilt opnemen.

    ![Menu kolommen kiezen](./media/backup-azure-manage-windows-server/choose-columns-menu.png)

3. Klik op **gereed** om de wijzigingen op te slaan en het menu kolommen selecteren te sluiten.

   Als u wijzigingen aanbrengt, maar de wijzigingen niet wilt behouden, klikt u op **opnieuw instellen** om de geselecteerde instelling te retour neren naar de laatst opgeslagen configuratie.

### <a name="change-the-filter-in-alerts-report"></a>Het filter in waarschuwingen rapport wijzigen

Gebruik het menu **filter** om de ernst, de status, de begin tijd en de eind tijd voor de waarschuwingen te wijzigen.

> [!NOTE]
> Als u het filter voor back-upwaarschuwingen bewerkt, worden de kritieke of waarschuwings waarschuwingen in het overzichts Dashboard van de kluis niet gewijzigd.
>  

1. Als u het filter voor back-upwaarschuwingen wilt wijzigen, klikt u in het menu waarschuwingen voor back-up op **filter**.

   ![Menu Filter kiezen](./media/backup-azure-manage-windows-server/alerts-menu-choose-filter.png)

   Het menu filter wordt weer gegeven.

   ![Menu Filter kiezen](./media/backup-azure-manage-windows-server/filter-alert-menu.png)

2. Bewerk de ernst, de status, de begin tijd of de eind tijd en klik op **gereed** om uw wijzigingen op te slaan.

## <a name="configuring-notifications-for-alerts"></a>Meldingen configureren voor waarschuwingen

Meldingen configureren om e-mail berichten te genereren wanneer een waarschuwing of kritieke waarschuwing optreedt. U kunt elk uur e-mail waarschuwingen verzenden of wanneer een bepaalde waarschuwing wordt weer gegeven.

   ![Waarschuwingen filteren](./media/backup-azure-manage-windows-server/configure-notification.png)

E-mail meldingen zijn standaard **ingeschakeld**. Klik op **uit** om de e-mail meldingen te stoppen.

Kies op het besturings element voor de **melding** de optie **per waarschuwing** als u niet wilt groeperen of geen veel items hebt die waarschuwingen kunnen genereren. Elke waarschuwing resulteert in één melding (de standaard instelling) en er wordt direct een oplossings-e-mail verzonden.

Als u **per uur Digest**selecteert, wordt er een e-mail bericht verzonden naar de ontvangers met uitleg over de onopgeloste waarschuwingen die in het afgelopen uur zijn gegenereerd. Er wordt aan het einde van het uur een oplossings-e-mail verzonden.

Kies de ernst van de waarschuwing (kritiek of waarschuwing) die wordt gebruikt voor het genereren van e-mail. Er zijn momenteel geen informatie meldingen.

## <a name="manage-backup-items"></a>Back-upitems beheren

Een Recovery Services kluis bevat veel soorten back-upgegevens. Meer [informatie](backup-overview.md#what-can-i-back-up) over waarvan u een back-up kunt maken. Als u de verschillende servers, computers, data bases en werk belastingen wilt beheren, klikt u op de tegel **back-** upitems om de inhoud van de kluis weer te geven.

![Tegel back-upitems](./media/backup-azure-manage-windows-server/backup-items.png)

De lijst met back-upitems, geordend op het type back-upbeheer, wordt geopend.

![lijst met back-upitems](./media/backup-azure-manage-windows-server/list-backup-items.png)

Als u een specifiek type beveiligde instantie wilt verkennen, klikt u op het item in de kolom Type back-upbeheer. In de bovenstaande afbeelding zijn er bijvoorbeeld twee virtuele Azure-machines die in deze kluis worden beveiligd. Als u op **Azure virtual machine**klikt, wordt de lijst met beveiligde virtuele machines in deze kluis geopend.

![lijst met back-uptype](./media/backup-azure-manage-windows-server/list-of-protected-virtual-machines.png)

De lijst met virtuele machines bevat nuttige gegevens: de bijbehorende resource groep, vorige [controle vooraf](https://azure.microsoft.com/blog/azure-vm-backup-pre-checks/)voor de back-up, de laatste back-up status en de datum van het meest recente herstel punt. Het beletsel teken in de laatste kolom opent het menu om algemene taken te activeren. De nuttige gegevens die in kolommen zijn opgenomen, verschillen voor elk type back-up.

![lijst met back-uptype](./media/backup-azure-manage-windows-server/ellipsis-menu.png)

## <a name="manage-backup-jobs"></a>Back-uptaken beheren

De tegel **back-uptaken** in het kluis Dashboard toont het aantal taken dat wordt uitgevoerd of is mislukt in de afgelopen 24 uur. De tegel bevat een idee van het menu back-uptaken.

![Back-ups maken van items uit instellingen](./media/backup-azure-manage-windows-server/backup-jobs-tile.png)

Als u meer informatie over de taken wilt weer geven, klikt u op **in voortgang** of het openen van het menu back-uptaken dat **is** gefilterd voor die status.

### <a name="backup-jobs-menu"></a>Menu back-uptaken

Het menu **back-uptaken** bevat informatie over het item type, de werking, de status, de start tijd en de duur.  

Als u het menu back-uptaken wilt openen, klikt u in het hoofd menu van de kluis op **back-uptaken**.

![Back-ups maken van items uit instellingen](./media/backup-azure-manage-windows-server/backup-jobs-menu-item.png)

De lijst met back-uptaken wordt geopend.

![Back-ups maken van items uit instellingen](./media/backup-azure-manage-windows-server/backup-jobs-list.png)

Het menu back-uptaken toont de status van alle bewerkingen op alle back-uptypen voor de afgelopen 24 uur. Gebruik **filter** om de filters te wijzigen. De filters worden beschreven in de volgende secties.

De filters wijzigen:

1. Klik in het menu kluis back-uptaken op **filter**.

   ![Back-ups maken van items uit instellingen](./media/backup-azure-manage-windows-server/vault-backup-job-menu-filter.png)

    Het menu filter wordt geopend.

   ![Back-ups maken van items uit instellingen](./media/backup-azure-manage-windows-server/filter-menu-backup-jobs.png)

2. Kies de filter instellingen en klik op **gereed**. De gefilterde lijst wordt vernieuwd op basis van de nieuwe instellingen.

#### <a name="item-type"></a>Itemtype

Het item type is het type back-upbeheer van het beveiligde exemplaar. Er zijn vier typen; Zie de volgende lijst. U kunt alle item typen of één item type weer geven. U kunt niet twee of drie item typen selecteren. De beschik bare typen items zijn:

* Alle item typen
* Virtuele Azure-machine
* Bestanden en mappen
* Azure Storage
* Azure-workload

#### <a name="operation"></a>Bewerking

U kunt één bewerking of alle bewerkingen weer geven. U kunt twee of drie bewerkingen niet selecteren. De beschik bare bewerkingen zijn:

* Alle bewerkingen
* Registreren
* Back-up configureren
* Back-up
* Herstellen
* Back-up uitschakelen
* Back-upgegevens verwijderen

#### <a name="status"></a>Status

U kunt alle statussen of één weer geven. U kunt twee of drie statussen niet selecteren. De beschik bare statussen zijn:

* Alle statussen
* Voltooid
* Wordt uitgevoerd
* Mislukt
* Geannuleerd
* Voltooid met waarschuwingen

#### <a name="start-time"></a>Begintijd

De dag en het tijdstip waarop de query begint. De standaard waarde is een periode van 24 uur.

#### <a name="end-time"></a>Eindtijd

De dag en het tijdstip waarop de query eindigt.

### <a name="export-jobs"></a>Taken exporteren

Gebruik **export taken** om een werk blad te maken met alle taken menu gegevens. Het werk blad bevat één blad dat een samen vatting van alle taken en afzonderlijke bladen voor elke taak bevat.

Als u de taak gegevens naar een werk blad wilt exporteren, klikt u op **taken exporteren**. De service maakt een werk blad aan de hand van de naam van de kluis en de datum, maar u kunt de naam wijzigen.

## <a name="monitor-backup-usage"></a>Back-upgebruik bewaken

De tegel back-upopslag in het dash board toont de opslag die wordt gebruikt in Azure. Opslag gebruik is bedoeld voor:

* Cloud LRS-opslag gebruik dat is gekoppeld aan de kluis
* Cloud GRS-opslag gebruik dat is gekoppeld aan de kluis


## <a name="troubleshooting-monitoring-issues"></a>Problemen met bewaking oplossen

**Probleem:** Taken en/of waarschuwingen van de Azure Backup Agent worden niet weer gegeven in de portal.

**Stappen voor probleem oplossing:** Het proces, ```OBRecoveryServicesManagementAgent```, verzendt de taak-en waarschuwings gegevens naar de Azure backup-service. Dit proces kan af en toe worden vastgelopen of afgesloten.

1. Als u wilt controleren of het proces niet wordt uitgevoerd, opent u ```OBRecoveryServicesManagementAgent``` **taak beheer**en voert u de opdracht uit.

2. Als het proces niet wordt uitgevoerd, opent u het **configuratie scherm**en bladert u naar de lijst met Services. **Microsoft Azure Recovery Services beheer agent**te starten of opnieuw te starten.

    Ga voor meer informatie naar de logboeken in:<br/>
   `<AzureBackup_agent_install_folder>\Microsoft Azure Recovery Services Agent\Temp\GatewayProvider*`Bijvoorbeeld:<br/>
   `C:\Program Files\Microsoft Azure Recovery Services Agent\Temp\GatewayProvider0.errlog`

## <a name="next-steps"></a>Volgende stappen
* [Windows Server of Windows-client herstellen vanuit Azure](backup-azure-restore-windows-server.md)
* Zie [Azure backup Overview](backup-introduction-to-azure-backup.md) (Engelstalig) voor meer informatie over Azure backup
* Ga naar het [Azure Backup forum](https://go.microsoft.com/fwlink/p/?LinkId=290933)
