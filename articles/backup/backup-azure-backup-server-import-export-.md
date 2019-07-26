---
title: Azure Backup-offline back-up voor DPM en Azure Backup Server
description: Meer informatie over hoe u met behulp van Azure Backup gegevens via het netwerk kunt verzenden met de Azure import/export-service. In dit artikel wordt uitgelegd hoe u de offline seeding van de eerste back-upgegevens maakt met behulp van de Azure import-export service.
author: saurabhsensharma
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 5/8/2018
ms.author: saurse
ms.openlocfilehash: 8a8571230b24d76482c505ec22d6faaa0caec5e6
ms.sourcegitcommit: c72ddb56b5657b2adeb3c4608c3d4c56e3421f2c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68466725"
---
# <a name="offline-backup-workflow-for-dpm-and-azure-backup-server"></a>Werk stroom voor offline back-ups voor DPM en Azure Backup Server
Azure Backup heeft verschillende ingebouwde efficiency waarmee netwerk-en opslag kosten worden bespaard tijdens de eerste volledige back-ups van gegevens naar Azure. Bij eerste volledige back-ups worden grote hoeveel heden gegevens overgebracht en is er meer netwerk bandbreedte nodig in vergelijking met de volgende back-ups die alleen de Deltas/toenames overdragen. Azure Backup comprimeert de eerste back-ups. Dankzij het proces van offline seeding kan Azure Backup schijven gebruiken om de gecomprimeerde initiële back-upgegevens offline te uploaden naar Azure.

Het offline-seeding proces van Azure Backup is nauw geïntegreerd met de [Azure import/export-service](../storage/common/storage-import-export-service.md) waarmee u gegevens naar Azure kunt overdragen met behulp van schijven. Als u een TBs hebt van de eerste back-upgegevens die moeten worden overgedragen via een netwerk met een hoge latentie en een lage band breedte, kunt u de werk stroom voor offline seeding gebruiken om de eerste back-up op een of meer harde schijven naar een Azure-Data Center te verzenden. Dit artikel bevat een overzicht en meer informatie over de stappen voor het volt ooien van deze werk stroom voor System Center DPM en Azure Backup Server.

> [!NOTE]
> Het proces van offline back-up voor de Microsoft Azure Recovery Services-agent (MARS) verschilt van System Center DPM en Azure Backup Server. Zie [dit artikel](backup-azure-backup-import-export.md)voor meer informatie over het gebruik van offline back-ups met Mars agent. Offline back-ups worden niet ondersteund voor systeem status back-ups die zijn uitgevoerd met behulp van de Azure Backup-Agent.
>

## <a name="overview"></a>Overzicht
Met de offline-seeding-mogelijkheid van Azure Backup en Azure import/export is het eenvoudig om de gegevens offline naar Azure te uploaden met behulp van schijven. Het offline back-upproces bestaat uit de volgende stappen:

> [!div class="checklist"]
> * De back-upgegevens, in plaats van via het netwerk te verzenden, worden naar een *faserings locatie* geschreven
> * De gegevens op de *faserings locatie* worden vervolgens naar een of meer SATA-schijven geschreven met behulp van het *AzureOfflineBackupDiskPrep* -hulp programma
> * Er wordt automatisch een Azure import-taak gemaakt door het hulp programma
> * De SATA-schijven worden vervolgens verzonden naar het dichtstbijzijnde Azure-Data Center
> * Nadat het uploaden van de back-upgegevens naar Azure is voltooid, Azure Backup de back-upgegevens naar de back-upkluis gekopieerd en de incrementele back-ups worden gepland.

## <a name="supported-configurations"></a>Ondersteunde configuraties
Offline back-ups worden ondersteund voor alle implementatie modellen van Azure Backup die een externe back-up van gegevens van on-premises naar de Microsoft Cloud maken. Dit omvat

> [!div class="checklist"]
> * Back-ups maken van bestanden en mappen met de Microsoft Azure Recovery Services-agent (MARS) of de Azure Backup-Agent.
> * Back-ups van alle werk belastingen en bestanden met System Center Data Protection Manager (SC DPM)
> * Back-ups maken van alle werk belastingen en bestanden met Microsoft Azure Backup Server <br/>

## <a name="prerequisites"></a>Vereisten
Zorg ervoor dat aan de volgende vereisten wordt voldaan voordat u de werk stroom voor offline back-ups initieert
* Er is een [Recovery Services kluis](backup-azure-recovery-services-vault-overview.md) gemaakt. Als u er een wilt maken, raadpleegt u de stappen in [dit artikel](tutorial-backup-windows-server-to-azure.md#create-a-recovery-services-vault)
* Azure Backup Agent of Azure Backup Server of SC DPM is geïnstalleerd op de Windows Server/Windows-client, indien van toepassing en de computer is geregistreerd bij de Recovery Services kluis. Zorg ervoor dat alleen de [meest recente versie van Azure backup](https://go.microsoft.com/fwlink/?linkid=229525) wordt gebruikt.
* [Down load het Azure Publish settings-bestand](https://portal.azure.com/#blade/Microsoft_Azure_ClassicResources/PublishingProfileBlade) op de computer van waaruit u een back-up van uw gegevens wilt maken. Het abonnement waaruit u het publicatie-instellingen bestand downloadt, kan afwijken van het abonnement dat de Recovery Services kluis bevat. Als uw abonnement is gekoppeld aan soevereine Azure-Clouds, gebruikt u de volgende koppelingen voor het downloaden van het Azure Publish settings-bestand.

    | Soevereine Cloud regio | Bestands koppeling van Azure Publish-instellingen |
    | --- | --- |
    | Verenigde Staten | [Link](https://portal.azure.us#blade/Microsoft_Azure_ClassicResources/PublishingProfileBlade) |
    | China | [Link](https://portal.azure.cn/#blade/Microsoft_Azure_ClassicResources/PublishingProfileBlade) |

* Een Azure Storage account met het *klassieke* implementatie model is gemaakt in het abonnement van waaruit u het bestand met publicatie-instellingen hebt gedownload, zoals hieronder wordt weer gegeven:

  ![Een klassiek opslag account maken](./media/backup-azure-backup-import-export/storageaccountclassiccreate.png)

* Een faserings locatie, die een netwerk share kan zijn of een extra station op de computer, intern of extern, met voldoende schijf ruimte om uw eerste kopie te bewaren, wordt gemaakt. Als u bijvoorbeeld een back-up probeert te maken van een bestands server van 500 GB, moet u ervoor zorgen dat de tijdelijke ruimte ten minste 500 GB is. (Er wordt een kleiner bedrag gebruikt vanwege compressie.)
* Met betrekking tot schijven die naar Azure worden verzonden, moet u ervoor zorgen dat er slechts 2,5-SSD-of 2,5-inch of 3,5-inch interne harde schijven van SATA worden gebruikt. U kunt harde schijven van Maxi maal 10 TB gebruiken. Raadpleeg de [documentatie van de Azure import/export-service](../storage/common/storage-import-export-requirements.md#supported-hardware) voor de meest recente set stations die door de service worden ondersteund.
* De SATA-schijven moeten worden aangesloten op een computer (waarnaar wordt verwezen als een *Kopieer computer*) van waaruit de kopie van de back-upgegevens van de *faserings locatie* naar de SATA-schijven is voltooid. Zorg ervoor dat BitLocker is ingeschakeld op de *Kopieer computer*

## <a name="workflow"></a>Werkstroom
De informatie in deze sectie helpt u bij het volt ooien van de werk stroom voor offline back-ups, zodat uw gegevens kunnen worden geleverd aan een Azure-Data Center en naar Azure Storage worden geüpload. Als u vragen hebt over de import service of een aspect van het proces, raadpleegt u de documentatie overzicht van het importeren van de [service](../storage/common/storage-import-export-service.md) .

### <a name="initiate-offline-backup"></a>Offline back-up initiëren
1. Wanneer u een back-up plant, ziet u het volgende scherm (in Windows Server, Windows-client of System Center Data Protection Manager).

    ![Scherm importeren](./media/backup-azure-backup-import-export/offlineBackupscreenInputs.png)

    Hier ziet u het bijbehorende scherm in System Center Data Protection Manager: <br/>
    ![Het import scherm van SC DPM en Azure Backup Server](./media/backup-azure-backup-import-export/dpmoffline.png)

    De beschrijving van de invoer is als volgt:

   * **Faserings locatie**: De tijdelijke opslag locatie waarnaar de eerste back-up wordt geschreven. De faserings locatie kan zich op een netwerk share of een lokale computer bezien. Als de computer en bron computer verschillend zijn, raden we u aan het volledige netwerkpad van de faserings locatie op te geven.
   * **Azure import-taak naam**: De unieke naam waarmee Azure import-service en Azure Backup de overdracht van gegevens die op schijven worden verzonden, worden gevolgd in Azure.
   * **Azure Publish-instellingen**: Geef het lokale pad naar het bestand met publicatie-instellingen op.
   * **Azure-abonnements-id**: De Azure-abonnements-ID van het abonnement waarvandaan u het Azure Publish settings-bestand hebt gedownload.
   * **Azure Storage account**: De naam van het opslag account in het Azure-abonnement dat is gekoppeld aan het Azure Publish settings-bestand.
   * **Azure storage container**: De naam van de doel opslag-Blob in het Azure-opslag account waarin de back-upgegevens worden geïmporteerd.

     Sla de *faserings locatie* en de naam van de *Azure import-taak* op die u hebt opgegeven om de schijven voor te bereiden.  

2. Voltooi de werk stroom en klik op **Nu back** -up maken in de beheer console van Azure backup agent om de kopie van de offline back-up te initiëren. Als onderdeel van deze stap wordt de eerste back-up naar het faserings gebied geschreven.

    ![Nu back-up maken](./media/backup-azure-backup-import-export/backupnow.png)

    Als u de bijbehorende werk stroom in System Center Data Protection Manager of Azure Backup Server wilt volt ooien, klikt u met de rechter muisknop op de **beveiligings groep**en kiest u vervolgens de optie **herstel punt maken** . Vervolgens kiest u de optie voor **online beveiliging** .

    ![SC DPM en Azure Backup Server nu back-up maken](./media/backup-azure-backup-import-export/dpmbackupnow.png)

    Nadat de bewerking is voltooid, kan de faserings locatie worden gebruikt voor het voorbereiden van de schijf.

    ![Voortgang van back-up](./media/backup-azure-backup-import-export/opbackupnow.png)

### <a name="prepare-sata-drives-and-ship-to-azure"></a>SATA-schijven voorbereiden en verzenden naar Azure
Het hulp programma *AzureOfflineBackupDiskPrep* wordt gebruikt om de SATA-schijven voor te bereiden die worden verzonden naar het dichtstbijzijnde Azure-Data Center. Dit hulp programma is beschikbaar in de installatiemap van de Recovery Services agent in het volgende pad:

*\\Hulppr.\\Microsoft Azure Recovery Servicese agent\\*

1. Ga naar de map en kopieer de Directory **AzureOfflineBackupDiskPrep** naar een kopie van de computer waarop de SATA-schijven die moeten worden voor bereid, zijn verbonden. Zorg ervoor dat het volgende betrekking heeft op de Kopieer computer:

   * De Kopieer computer heeft toegang tot de faserings locatie voor de werk stroom voor offline seeding met behulp van hetzelfde netwerkpad dat is gegeven in de werk stroom **offline back-up initiëren** .
   * BitLocker is ingeschakeld op de Kopieer computer.
   * De Kopieer computer heeft toegang tot de Azure Portal.

     Als dat nodig is, kan de Kopieer computer gelijk zijn aan die van de bron computer.

     > [!IMPORTANT]
     > Als de bron computer een virtuele machine is, is het verplicht een andere fysieke server of client computer als Kopieer computer te gebruiken.


2. Open een opdracht prompt met verhoogde bevoegdheid op de computer kopiëren met de map *AzureOfflineBackupDiskPrep* Utility als de huidige map en voer de volgende opdracht uit:

    `*.\AzureOfflineBackupDiskPrep.exe*   s:<*Staging Location Path*>   [p:<*Path to AzurePublishSettingsFile*>]`

    | Parameter | Description |
    | --- | --- |
    | s:&lt;*pad naar locatie van tijdelijke bestanden*&gt; |Verplichte invoer die wordt gebruikt om het pad naar de faserings locatie op te geven die u hebt ingevoerd in de werk stroom **offline back-up initiëren** . |
    | p:&lt;*pad naar PublishSettingsFile*&gt; |Optionele invoer die wordt gebruikt om het pad naar het **Azure Publish Settings** -bestand op te geven dat u hebt ingevoerd in de werk stroom **offline back-up initiëren** . |

    > [!NOTE]
    > Het &lt;pad naar de&gt; AzurePublishSettingFile-waarde is verplicht wanneer de Kopieer computer en de bron computer verschillend zijn.
    >
    >

    Wanneer u de opdracht uitvoert, vraagt het hulp programma de selectie van de Azure import-taak aan die overeenkomt met de stations die moeten worden voor bereid. Als er slechts één import taak is gekoppeld aan de gegeven faserings locatie, ziet u een scherm zoals de volgende.

    ![Invoer van hulp programma voor Azure-schijf voorbereiding](./media/backup-azure-backup-import-export/azureDiskPreparationToolDriveInput.png) <br/>

3. Voer de stationsletter zonder de afsluitende dubbele punt in voor de gekoppelde schijf die u wilt voorbereiden voor overdracht naar Azure. Geef een bevestiging voor de Format teren van het station op wanneer u hierom wordt gevraagd.

    Het hulp programma begint vervolgens met het voorbereiden van de schijf en het kopiëren van de back-upgegevens. Mogelijk moet u extra schijven toevoegen wanneer u hierom wordt gevraagd, in het geval dat de schijf niet voldoende ruimte heeft voor de back-upgegevens. <br/>

    Aan het einde van een geslaagde uitvoering van het hulp programma, worden een of meer schijven die u hebt geleverd, voor bereid voor verzen ding naar Azure. Daarnaast wordt een import taak met de naam die u hebt ingevoerd tijdens de werk stroom voor het initiëren van **offline back-ups** , gemaakt in Azure. Ten slotte wordt in het hulp programma het verzend adres weer gegeven naar het Azure-Data Center waar de schijven moeten worden verzonden.

    ![Voorbereiden van Azure-schijf is voltooid](./media/backup-azure-backup-import-export/azureDiskPreparationToolSuccess.png)<br/>

4. Aan het einde van de uitvoering van de opdracht ziet u ook de optie om de verzend gegevens bij te werken, zoals hieronder wordt weer gegeven:

    ![Optie verzend gegevens bijwerken](./media/backup-azure-backup-import-export/updateshippingutility.png)<br/>

5. U kunt de details direct opgeven. Het hulp programma begeleidt u bij het proces voor het uitvoeren van een reeks invoer. Als u echter geen informatie hebt, zoals tracking nummer of andere details die betrekking hebben op verzen ding, kunt u de sessie beëindigen. De stappen voor het bijwerken van de verzend gegevens vindt u verderop in dit artikel.

6. Verzend de schijven naar het adres dat het hulp programma heeft geleverd en behoud het tracking nummer voor toekomstige referentie.

   > [!IMPORTANT]
   > Er kunnen niet twee Azure-import taken hetzelfde tracking nummer hebben. Zorg ervoor dat de stations die worden voor bereid met het hulp programma onder één Azure import-taak samen worden verzonden in één pakket en dat er één uniek Volg nummer voor het pakket is. Combi neer geen stations die zijn voor bereid als onderdeel van **verschillende** Azure-import taken in één pakket.

5. Wanneer u de informatie over het tracerings nummer hebt, gaat u naar de bron computer, waar de taak moet worden geïmporteerd en voert u de volgende opdracht uit in een opdracht prompt met verhoogde bevoegdheid met de map *AzureOfflineBackupDiskPrep* Utility als de huidige map:

   `*.\AzureOfflineBackupDiskPrep.exe*  u:`

   U kunt optioneel de volgende opdracht uitvoeren vanaf een andere computer, zoals het kopiëren van de *computer*, met de map *AzureOfflineBackupDiskPrep* Utility als de huidige map:

   `*.\AzureOfflineBackupDiskPrep.exe*  u:  s:<*Staging Location Path*>   p:<*Path to AzurePublishSettingsFile*>`

    | Parameter | Description |
    | --- | --- |
    | h | Verplichte invoer die wordt gebruikt om verzend gegevens bij te werken voor een Azure import-taak |
    | s:&lt;*pad naar locatie van tijdelijke bestanden*&gt; | Verplichte invoer wanneer de opdracht niet wordt uitgevoerd op de bron computer. Hiermee geeft u het pad op naar de faserings locatie die u hebt ingevoerd in de werk stroom **offline back-up initiëren** . |
    | p:&lt;*pad naar PublishSettingsFile*&gt; | Verplichte invoer wanneer de opdracht niet wordt uitgevoerd op de bron computer. Hiermee geeft u het pad op naar het **Azure Publish Settings** -bestand dat u hebt ingevoerd in de werk stroom **offline back-up initiëren** . |

    Het hulp programma detecteert automatisch de import taak die op de bron computer wacht of de import taken die zijn gekoppeld aan de faserings locatie wanneer de opdracht wordt uitgevoerd op een andere computer. Vervolgens krijgt u de optie om de verzend gegevens bij te werken via een reeks invoer, zoals hieronder wordt weer gegeven:

    ![Verzend gegevens invoeren](./media/backup-azure-backup-import-export/shippinginputs.png)<br/>

6. Zodra alle invoer is gegeven, controleert u de details aandachtig en legt u de verzend gegevens die u hebt ontvangen door *Ja*te typen.

    ![Verzend gegevens controleren](./media/backup-azure-backup-import-export/reviewshippinginformation.png)<br/>

7. Bij het bijwerken van de verzend gegevens is het hulp programma voorzien van een lokale locatie waar de verzend gegevens die u hebt ingevoerd, worden opgeslagen, zoals hieronder wordt weer gegeven

    ![Verzend gegevens opslaan](./media/backup-azure-backup-import-export/storingshippinginformation.png)<br/>

   > [!IMPORTANT]
   > Zorg ervoor dat de stations het Azure-Data Center binnen twee weken na de levering van de verzend gegevens bereiken met het hulp programma *AzureOfflineBackupDiskPrep* . Als u dit niet doet, kan dit leiden tot de stations die niet worden verwerkt.  

Zodra u de bovenstaande stappen hebt voltooid, is het Azure-Data Center klaar om de stations te ontvangen en de back-upgegevens van de stations naar het door u gemaakte Azure-opslag account van het klassieke type over te dragen.

### <a name="time-to-process-the-drives"></a>Tijd voor het verwerken van de stations
De hoeveelheid tijd die nodig is voor het verwerken van een Azure import-taak is afhankelijk van verschillende factoren, zoals de verzend tijd, het taak type, het type en de grootte van de gegevens die worden gekopieerd en de grootte van de geleverde schijven. De Azure import/export-service heeft geen SLA, maar nadat de schijven zijn ontvangen, streeft de service naar het volt ooien van de back-upgegevens naar uw Azure Storage-account in 7 tot tien dagen. De volgende sectie bevat informatie over hoe u de status van de Azure import-taak kunt controleren.

### <a name="monitoring-azure-import-job-status"></a>De status van de Azure import-taak controleren
Hoewel uw stations onderweg of in het Azure-Data Center worden gekopieerd naar het opslag account, toont de Azure Backup Agent of SC DPM of de Azure Backup Server-console op de bron computer de volgende taak status voor uw geplande back-ups.

  `Waiting for Azure Import Job to complete. Please check on Azure Management portal for more information on job status`

Volg de onderstaande stappen om de status van de import taak te controleren.
1. Open een opdracht prompt met verhoogde bevoegdheid op de bron computer en voer de volgende opdracht uit:

     `AzureOfflineBackupDiskPrep.exe u:`

2.  In de uitvoer ziet u de huidige status van de import taak, zoals hieronder wordt weer gegeven:

    ![Status van import taak controleren](./media/backup-azure-backup-import-export/importjobstatusreporting.png)<br/>

Zie [dit artikel](../storage/common/storage-import-export-view-drive-status.md) voor meer informatie over de verschillende statussen van de Azure import-taak

### <a name="complete-the-workflow"></a>De werk stroom volt ooien
Nadat de import taak is voltooid, zijn de eerste back-upgegevens beschikbaar in uw opslag account. Op het moment van de volgende geplande back-up kopieert Azure backup de inhoud van de gegevens uit het opslag account naar de Recovery Services kluis, zoals hieronder wordt weer gegeven:

   ![Gegevens kopiëren naar Recovery Services kluis](./media/backup-azure-backup-import-export/copyingfromstorageaccounttoazurebackup.png)<br/>

Op het moment van de volgende geplande back-up voert Azure Backup incrementele back-up uit via de eerste back-upkopie.

## <a name="next-steps"></a>Volgende stappen
* Voor vragen over de Azure import/export-werk stroom raadpleegt u [de Microsoft Azure import/export-service gebruiken om gegevens over te dragen naar de Blob-opslag](../storage/common/storage-import-export-service.md).
* Raadpleeg de sectie offline back-up van de Azure Backup [Veelgestelde vragen](backup-azure-backup-faq.md) voor vragen over de werk stroom.
