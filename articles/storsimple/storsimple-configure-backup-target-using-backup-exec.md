---
title: StorSimple 8000-serie als back-updoel met Backup Exec | Microsoft Docs
description: Beschrijving van de configuratie van de back-updoel StorSimple met Veritas Backup Exec.
services: storsimple
documentationcenter: ''
author: harshakirank
manager: matd
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/05/2016
ms.author: hkanna
ms.openlocfilehash: e11d541f0450c0de4ba6d60f889fc7471b1fa1aa
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60724317"
---
# <a name="storsimple-as-a-backup-target-with-backup-exec"></a>StorSimple als back-updoel met Backup Exec

## <a name="overview"></a>Overzicht

Azure StorSimple is een oplossing voor hybride cloudopslag van Microsoft. De complexiteit van de oplossing die exponentiële gegevenstoename adressen StorSimple met behulp van een Azure storage-account als een uitbreiding van de on-premises-oplossing en gegevens automatisch in lagen in on-premises opslag en opslag in de cloud.

In dit artikel wordt besproken hoe de integratie van de StorSimple met Veritas Backup Exec en aanbevolen procedures voor het integreren van beide oplossingen. We doen ook aanbevelingen voor het instellen van back-up Exec beste integreren met StorSimple. We stellen Veritas aanbevolen procedures, back-architecten en administrators voor de beste manier om Exec back-up instellen om te voldoen aan de vereisten voor afzonderlijke back-up- en service level agreements (Sla's).

Hoewel we laten configuratiestappen en belangrijkste concepten zien, is in dit artikel geen stapsgewijze handleiding-configuratie of de installatie. Gaan we ervan uit dat de basisonderdelen van en de infrastructuur zijn en klaar voor de ondersteuning van de concepten die worden beschreven.

### <a name="who-should-read-this"></a>Wie is dit?

De informatie in dit artikel is vooral handig voor back-upbeheerders, opslagbeheerders en opslag-architecten die kennis van opslag, Windows Server 2012 R2, Ethernet, cloudservices en Backup Exec hebben.

## <a name="supported-versions"></a>Ondersteunde versies

-   [Back-up Exec 16 en latere versies](http://backupexec.com/compatibility)
-   [StorSimple Update 3 en latere versies](storsimple-overview.md#storsimple-workload-summary)


## <a name="why-storsimple-as-a-backup-target"></a>Waarom StorSimple als back-updoel?

StorSimple is een goede keuze voor een back-updoel omdat:

-   Het biedt standaard, lokale opslag voor back-uptoepassingen om te gebruiken als een snelle back-upbestemming, zonder deze te wijzigen. U kunt ook StorSimple gebruiken voor een snel herstel van recente back-ups.
-   De cloud tiering is volledig geïntegreerd met een Azure-cloud storage-account voordelige Azure Storage gebruiken.
-   Het biedt automatisch offsiteopslag voor herstel na noodgevallen.

## <a name="key-concepts"></a>Belangrijkste concepten

Net als bij een storage-oplossing, een zorgvuldige evaluatie van de opslagprestaties van de oplossing, sla's, is de snelheid van de wijzigings- en groei van de capaciteitsbehoeften essentieel voor succes. Het belangrijkste idee is dat door de introductie van een cloudlaag, uw toegangstijden en doorvoercapaciteit naar de cloud play een belangrijke rol in de mogelijkheid van StorSimple om zijn werk te doen.

StorSimple is ontworpen om opslag te bieden tot toepassingen die worden uitgevoerd op een goed gedefinieerde werkset van gegevens (gegevens). In dit model, de werkset van gegevens is opgeslagen op de lokale lagen en de resterende vrije/koude/gearchiveerd set gegevens naar de cloud is gelaagd. Dit model wordt weergegeven in de volgende afbeelding. Bijna vaste groene lijn staat voor de gegevens die zijn opgeslagen op de lokale lagen van de StorSimple-apparaat. De rode lijn staat voor de totale hoeveelheid gegevens die zijn opgeslagen op de StorSimple-oplossing voor alle lagen. De ruimte tussen de vaste groene lijn en de exponentiële rode kromme vertegenwoordigt de totale hoeveelheid gegevens die zijn opgeslagen in de cloud.

**StorSimple opslaglagen**
![cloudlagen StorSimple-diagram](./media/storsimple-configure-backup-target-using-backup-exec/image1.jpg)

Met deze architectuur in gedachten vindt u dat StorSimple is ideaal als een back-updoel wilt gebruiken. U kunt StorSimple te gebruiken:
-   Uw meest frequente herstelbewerkingen uitvoeren vanaf de lokale werkset van gegevens.
-   De cloud gebruiken voor offsite-noodherstel en oudere gegevens waar herstelbewerkingen zijn minder frequent optreden.

## <a name="storsimple-benefits"></a>Voordelen van StorSimple

StorSimple biedt een on-premises-oplossing die naadloos is geïntegreerd met Microsoft Azure, door te profiteren van naadloze toegang tot on-premises en cloud-opslag.

StorSimple maakt gebruik van automatische lagen tussen de on-premises-apparaat met SSD-apparaat (SSD) en serial attached SCSI (SAS) storage en Azure Storage. Automatische lagen, houdt veelgebruikte gegevens lokaal op de SSD- en SAS-lagen. Wordt weinig gebruikte gegevens verplaatst naar Azure Storage.

StorSimple biedt deze voordelen:

-   De unieke deduplicatie en compressie-algoritmen die gebruikmaken van de cloud te bereiken ongekende Ontdubbeling niveaus
-   Hoge beschikbaarheid
-   Geo-replicatie met behulp van Azure geo-replicatie
-   Integratie van Azure
-   Versleuteling van gegevens in de cloud
-   Verbeterde noodherstel en naleving

Hoewel StorSimple geeft fundamenteel twee belangrijkste implementatiescenario's (back-updoel primaire en secundaire back-updoel), is het een gewone, blokopslagapparaat. StorSimple biedt alle compressie en Ontdubbeling. Naadloos wordt verzonden en opgehaald van gegevens tussen de cloud en de toepassing en het bestandssysteem.

Zie voor meer informatie over StorSimple [StorSimple 8000-serie: Oplossing voor hybride cloudopslag](storsimple-overview.md). U kunt ook controleren de [technische specificaties van de StorSimple 8000-serie](storsimple-technical-specifications-and-compliance.md).

> [!IMPORTANT]
> Met behulp van een storsimple-oplossing wordt apparaat als een back-updoel alleen ondersteund voor StorSimple 8000-Update 3 en latere versies.

## <a name="architecture-overview"></a>Overzicht van de architectuur

De volgende tabellen tonen de initiële richtlijnen voor apparaat-model-naar-architectuur.

**StorSimple-capaciteit voor lokale en opslag in de cloud**

| Opslagcapaciteit       | 8100          | 8600            |
|------------------------|---------------|-----------------|
| Lokale opslagcapaciteit | &lt; 10 TiB\*  | &lt; 20 TiB\*  |
| Cloudopslagcapaciteit | &gt; 200 TiB\* | &gt; 500 TiB\* |

\* Maximale grootte wordt ervan uitgegaan dat er geen gegevensontdubbeling of compressie.

**StorSimple-capaciteiten voor primaire en secundaire back-ups**

| Back-scenario  | Lokale opslagcapaciteit  | Cloudopslagcapaciteit  |
|---|---|---|
| Primaire back-up  | Recente back-ups opgeslagen op lokale opslag voor snel herstel om te voldoen aan de beoogde herstelpunt (RPO) | Back-upgeschiedenis (RPO) past in de cloud-capaciteit |
| Secundaire back-up | Secundaire kopie van de back-upgegevens kan worden opgeslagen in de cloud-capaciteit  | N/A  |

## <a name="storsimple-as-a-primary-backup-target"></a>StorSimple als primaire back-updoel

In dit scenario worden de StorSimple-volumes weergegeven voor de back-uptoepassing als de enige opslagruimte voor back-ups. De volgende afbeelding toont de oplossingsarchitectuur van een waarin alle back-ups gebruik StorSimple volumes voor back-ups en herstelbewerkingen gelaagde.

![StorSimple als een logisch diagram van de primaire back-updoel](./media/storsimple-configure-backup-target-using-backup-exec/primarybackuptargetlogicaldiagram.png)

### <a name="primary-target-backup-logical-steps"></a>Primaire doel van de back-up logische stappen

1.  De back-upserver neemt contact op met de back-upagent doel en de backup-agent verzendt gegevens naar de back-upserver.
2.  De back-upserver schrijft gegevens naar de StorSimple gelaagde volumes.
3.  De back-upserver van de catalogusdatabase bijgewerkt en klikt u vervolgens klaar is met de back-uptaak.
4.  Een momentopname-script wordt geactiveerd StorSimple cloud snapshot manager (start of verwijderen).
5.  De back-upserver worden verlopen back-ups op basis van een bewaarbeleid verwijderd.


### <a name="primary-target-restore-logical-steps"></a>Primaire doel terugzetten logische stappen

1.  De back-upserver Start herstellen van de juiste gegevens uit de opslagplaats voor opslag.
2.  De back-upagent ontvangt de gegevens van de back-upserver.
3.  De back-upserver klaar is met de hersteltaak.

## <a name="storsimple-as-a-secondary-backup-target"></a>StorSimple als secundaire back-updoel

In dit scenario, worden StorSimple-volumes voornamelijk gebruikt voor langdurige bewaarperioden en archiveren.

De volgende afbeelding ziet u een architectuur die eerste back-ups en herstellen van een hoogwaardige doelvolume. Deze back-ups worden gekopieerd en opgeslagen in een storsimple-oplossing gelaagd volume op een vast schema.

Het is belangrijk om de grootte van het volume met hoge prestaties, zodat uw retentie capaciteit en prestaties vereisten kunnen worden verwerkt.

![StorSimple als een secundaire back-updoel logisch diagram](./media/storsimple-configure-backup-target-using-backup-exec/secondarybackuptargetlogicaldiagram.png)

### <a name="secondary-target-backup-logical-steps"></a>Secundaire doel van de back-up logische stappen

1.  De back-upserver neemt contact op met de back-upagent doel en de backup-agent verzendt gegevens naar de back-upserver.
2.  De back-upserver schrijft gegevens naar opslag met hoge prestaties.
3.  De back-upserver van de catalogusdatabase bijgewerkt en klikt u vervolgens klaar is met de back-uptaak.
4.  De back-upserver worden back-ups voor StorSimple op basis van een bewaarbeleid gekopieerd.
5.  Een momentopname-script wordt geactiveerd StorSimple cloud snapshot manager (start of verwijderen).
6.  De back-upserver worden verlopen back-ups op basis van een bewaarbeleid verwijderd.

### <a name="secondary-target-restore-logical-steps"></a>Secundaire doel terugzetten logische stappen

1.  De back-upserver Start herstellen van de juiste gegevens uit de opslagplaats voor opslag.
2.  De back-upagent ontvangt de gegevens van de back-upserver.
3.  De back-upserver klaar is met de hersteltaak.

## <a name="deploy-the-solution"></a>De oplossing implementeren

Implementatie van de oplossing zijn drie stappen vereist:
1. Bereid de netwerkinfrastructuur.
2. Uw StorSimple-apparaat implementeren als een back-updoel.
3. Backup Exec implementeren.

Elke stap wordt besproken in de volgende secties.

### <a name="set-up-the-network"></a>Het netwerk instellen

Omdat StorSimple een oplossing die geïntegreerd met de Azure-cloud is, vereist StorSimple een actieve verbinding met de Azure-cloud. Deze verbinding wordt gebruikt voor bewerkingen zoals het cloudmomentopnamen, beheer en metagegevens overbrengen en naar laag oudere, minder gebruikte gegevens naar Azure-cloudopslag.

Voor de oplossing om optimaal te presteren, raden we aan dat u deze netwerken aanbevolen procedures volgt:

-   De koppeling die verbinding maakt StorSimple meerdere lagen naar Azure moet voldoen aan uw eisen aan de bandbreedte. Om dit te doen, het vereiste niveau van de Quality of Service (QoS) van toepassing op uw infrastructuur switches zodat deze overeenkomen met uw RPO en recovery time objective (RTO) Sla's.
-   Maximale toegangslatentie van Azure Blob-opslag moet ongeveer 80 ms.

### <a name="deploy-storsimple"></a>StorSimple implementeren

Zie voor een stapsgewijze uitleg van de implementatie StorSimple, [uw on-premises StorSimple-apparaat implementeren](storsimple-deployment-walkthrough-u2.md).

### <a name="deploy-backup-exec"></a>Backup Exec implementeren

Zie voor aanbevolen procedures voor de back-up Exec-installatie, [aanbevolen procedures voor de installatie van de back-up Exec](https://www.veritas.com/content/support/en_US/doc/72686287-131623464-0/v70444238-131623464).

## <a name="set-up-the-solution"></a>Instellen van de oplossing

In deze sectie ziet enkele configuratievoorbeelden. De volgende voorbeelden en aanbevelingen ziet de meest elementaire en fundamentele uitvoering. Deze implementatie mogelijk niet van toepassing zijn rechtstreeks aan uw specifieke behoeften van de back-up.

### <a name="set-up-storsimple"></a>Instellen van StorSimple

| StorSimple-implementatietaken  | Aanvullende opmerkingen |
|---|---|
| Uw on-premises StorSimple-apparaat implementeren. | Ondersteunde versies: Update 3 en latere versies. |
| Schakel in het back-updoel. | Gebruik deze opdrachten inschakelen of uitschakelen van back-updoel modus en status ophalen. Zie voor meer informatie, [verbinding maken met een StorSimple-apparaat op afstand](storsimple-remote-connect.md).</br> Om in te schakelen op back-upmodus: `Set-HCSBackupApplianceMode -enable`. </br> Back-modus uit te schakelen: `Set-HCSBackupApplianceMode -disable`. </br> Om op te halen van de huidige status van de instellingen voor back-upmodus: `Get-HCSBackupApplianceMode`. |
| Maak een algemene volumecontainer voor het volume waarin de back-upgegevens. Alle gegevens in een volumecontainer is ontdubbeld. | StorSimple-volumecontainers definiëren domeinen met Ontdubbeling.  |
| StorSimple-volumes maken. | Maak volumes met grootten als dicht bij het verwachte gebruik mogelijk, omdat de grootte van volume is van invloed op de duur van de tijd van cloudmomentopname. Voor informatie over hoe u het formaat van een volume, leest u over [bewaarbeleid](#retention-policies).</br> </br> Gebruik StorSimple gelaagde volumes en selecteert u de **dit volume gebruiken voor minder frequent gebruikte gearchiveerde gegevens** selectievakje. </br> Gebruik alleen lokaal vastgemaakt volumes wordt niet ondersteund. |
| Een unieke StorSimple-back-upbeleid voor alle back-updoel volumes maken. | Een StorSimple-back-upbeleid definieert de volumegroep consistentie. |
| De planning uitschakelen als de momentopnamen zijn verlopen. | Momentopnamen worden geactiveerd als een bewerking na verwerking. |

### <a name="set-up-the-host-backup-server-storage"></a>Instellen van de opslag van de back-upserver van host

Instellen van de opslag van de back-upserver host op basis van deze richtlijnen:  

- Gebruik geen spanned volumes (gemaakt door Windows Schijfbeheer). Spanned schijven worden niet ondersteund.
- De volumes formatteren met NTFS met een toewijzingsgrootte van 64 KB.
- De StorSimple-volumes rechtstreeks naar de back-up Exec-server worden toegewezen.
    - Gebruik iSCSI voor fysieke servers.
    - Gebruik Passthrough-schijven voor virtuele servers.

## <a name="best-practices-for-storsimple-and-backup-exec"></a>Aanbevolen procedures voor het StorSimple- en Backup Exec

Instellen van uw oplossing op basis van de richtlijnen in de volgende secties.

### <a name="operating-system-best-practices"></a>Aanbevolen procedures voor besturingssysteem

- Windows Server-versleuteling en Ontdubbeling voor het NTFS-bestandssysteem uitgeschakeld.
- Uitschakelen van Windows Server-defragmentatie op de StorSimple-volumes.
- Indexeren van Windows Server op het StorSimple-volumes uitschakelen.
- Voer een antivirusprogramma uit op de bronhost (niet op basis van de StorSimple-volumes).
- De standaardwaarde uitschakelen [onderhoud van Windows Server](https://msdn.microsoft.com/library/windows/desktop/hh848037.aspx) in Taakbeheer. Dit op een van de volgende manieren doen:
  - De configurator onderhoud in Windows Taakplanner uitschakelen.
  - Download [PsExec](https://technet.microsoft.com/sysinternals/bb897553.aspx) van Windows Sysinternals. Nadat u PsExec hebt gedownload, voert u Azure PowerShell als beheerder, en typ:
    ```powershell
    psexec \\%computername% -s schtasks /change /tn “MicrosoftWindowsTaskSchedulerMaintenance Configurator" /disable
    ```

### <a name="storsimple-best-practices"></a>Aanbevolen procedures voor StorSimple

  -   Zorg dat het StorSimple-apparaat is bijgewerkt naar [Update 3 of hoger](storsimple-install-update-3.md).
  -   Isoleren iSCSI en cloudverkeer. ISCSI-specifieke verbindingen gebruiken voor verkeer tussen StorSimple en de back-upserver.
  -   Zorg ervoor dat uw StorSimple-apparaat een toegewezen back-updoel is. Gemengde werkbelastingen worden niet ondersteund omdat ze invloed heeft op uw RTO en RPO.

### <a name="backup-exec-best-practices"></a>Aanbevolen procedures voor back-up Exec

-   Backup Exec moet worden geïnstalleerd op een lokaal station van de server en niet op een StorSimple-volume.
-   De opslag van de back-up Exec **gelijktijdige schrijfbewerkingen** naar het maximaal toegestane aantal.
    -   De opslag van de back-up Exec **blokkeren en de buffer** tot 512 KB.
    -   Back-up Exec opslag inschakelen **gebufferde lezen en schrijven**.
-   StorSimple biedt ondersteuning voor back-up Exec volledige en incrementele back-ups. U wordt aangeraden geen synthetische en differentiële back-ups te gebruiken.
-   Back-upgegevens bestanden moeten bevatten alleen gegevens voor een specifieke taak. Bijvoorbeeld geen media worden toegevoegd voor verschillende taken zijn toegestaan.
-   Controle van de taak niet uitschakelen. Indien nodig, kan verificatie moet worden gepland na de meest recente back-uptaak. Het is belangrijk om te begrijpen dat deze taak is van invloed op uw back-upvenster.
-   Selecteer **opslag** > **uw schijf** > **Details** > **eigenschappen**. Uitschakelen **vooraf toewijzen van schijfruimte**.

Zie voor de meest recente back-up Exec instellingen en aanbevolen procedures voor het implementeren van deze vereisten, [de website Veritas](https://www.veritas.com).

## <a name="retention-policies"></a>Bewaarbeleid

Een van de meest voorkomende typen van de back-upretentie-beleid is een zogenaamde grootvader vader en zoon (algemene)-beleid. Een incrementele back-up dagelijks wordt uitgevoerd in een beleid algemene en volledige back-ups wekelijkse en maandelijkse klaar bent. Deze resultaten van Groepsbeleid in zes StorSimple gelaagde volumes. Een volume bevat de wekelijkse, maandelijkse en jaarlijkse volledige back-ups. De vijf volumes opslaan dagelijkse incrementele back-ups.

In het volgende voorbeeld gebruiken we een algemene draaiing. Het voorbeeld wordt ervan uitgegaan:

-   Niet-ontdubbelde of gecomprimeerde gegevens worden gebruikt.
-   Volledige back-ups zijn 1 TiB.
-   Dagelijkse incrementele back-ups zijn 500 GiB.
-   Vier wekelijkse back-ups worden bewaard gedurende een maand.
-   Twaalf maandelijkse back-ups worden voor een jaar bewaard.
-   Een jaarlijkse back-up wordt bewaard gedurende tien jaar.

Op basis van de voorgaande veronderstellingen, maak een 26-TiB StorSimple gelaagd volume voor de maandelijkse en jaarlijkse volledige back-ups. Maken van een 5-TiB StorSimple gelaagd volume voor elk van de incrementele dagelijkse back-ups.

| Type back-up bewaren | Grootte (TiB) | Algemene vermenigvuldiger\* | Totale capaciteit (TiB)  |
|---|---|---|---|
| Wekelijkse volledige | 1 | 4  | 4 |
| Dagelijkse incrementele | 0.5 | 20 (cycli gelijk aantal weken per maand) | 12 (2 voor extra quota) |
| Maandelijks volledige | 1 | 12 | 12 |
| Jaarlijks volledige | 1  | 10 | 10 |
| Algemene vereisten |   | 38 |   |
| Uitbreiding van uw quotum  | 4  |   | 42 totale algemene vereiste  |

\* De algemene vermenigvuldiger is het aantal exemplaren dat u wilt beveiligen en te behouden om te voldoen aan de vereisten van uw back-upbeleid.

## <a name="set-up-backup-exec-storage"></a>Back-up Exec opslag instellen

### <a name="to-set-up-backup-exec-storage"></a>Back-up Exec opslag instellen

1.  Selecteer in de beheerconsole voor Backup Exec **opslag** > **opslag configureren** > **schijven gebaseerde opslag**  >   **Volgende**.

    ![Back-up van de console Groepsbeleidsbeheer Exec, opslagpagina configureren](./media/storsimple-configure-backup-target-using-backup-exec/image4.png)

2.  Selecteer **schijfopslag**, en selecteer vervolgens **volgende**.

    ![Back-up Exec-beheerconsole, selecteer storage-pagina](./media/storsimple-configure-backup-target-using-backup-exec/image5.png)

3.  Voer de naam van een vertegenwoordiger, bijvoorbeeld: **zaterdag volledige**, en een beschrijving. Selecteer **Next**.

    ![Pagina back-up Exec management console, de naam en beschrijving](./media/storsimple-configure-backup-target-using-backup-exec/image7.png)

4.  Selecteer de schijf waarop u wilt maken van het schijfopslagapparaat en selecteer vervolgens **volgende**.

    ![Back-up Exec-beheerconsole, opslag-pagina voor het selecteren van schijf](./media/storsimple-configure-backup-target-using-backup-exec/image9.png)

5.  Verhoog het aantal schrijfbewerkingen naar **16**, en selecteer vervolgens **volgende**.

    ![Back-up Exec beheerconsole voor gelijktijdige bewerkingen instellingenpagina schrijven](./media/storsimple-configure-backup-target-using-backup-exec/image10.png)

6.  Controleer de instellingen en selecteer vervolgens **voltooien**.

    ![Back-up Exec-beheerconsole, opslag-configuratie-overzichtspagina](./media/storsimple-configure-backup-target-using-backup-exec/image11.png)

7.  Aan het einde van elke toewijzing volume, wijzigt u de apparaatinstellingen opslag zodat deze overeenkomt met deze aanbevolen op [aanbevolen procedures voor StorSimple en back-up Exec](#best-practices-for-storsimple-and-backup-exec).

    ![Back-up Exec-beheerconsole, opslag-Apparaatpagina instellingen](./media/storsimple-configure-backup-target-using-backup-exec/image12.png)

8.  Herhaal stappen 1-7 totdat u bent klaar voor het toewijzen van uw StorSimple-volumes aan back-up Exec.

## <a name="set-up-storsimple-as-a-primary-backup-target"></a>StorSimple instellen als primaire back-updoel

> [!NOTE]
> Het herstellen van gegevens vanuit een back-up die is gelaagde naar de cloud vindt plaats met een snelheid van de cloud.

De volgende afbeelding ziet u de toewijzing van een typische volume naar een back-uptaak. In dit geval alle wekelijkse back-ups worden toegewezen aan de volledige schijf zaterdag en de incrementele back-ups worden toegewezen aan incrementele schijven van maandag tot en met vrijdag. Alle back-ups en herstelbewerkingen van een storsimple-oplossing zijn een gelaagd volume.

![Primaire back-updoel configuratie logisch diagram](./media/storsimple-configure-backup-target-using-backup-exec/primarybackuptargetdiagram.png)

### <a name="storsimple-as-a-primary-backup-target-gfs-schedule-example"></a>StorSimple als primaire back-updoel algemene voorbeeld plannen

Hier volgt een voorbeeld van een algemene rotatieschema vier weken, maandelijkse en jaarlijkse:

| Frequentie/back-up-type | Volledig | Incrementele (dagen 1-5)  |   
|---|---|---|
| Wekelijks (weken 1-4) | zaterdag | Monday-Friday |
| Maandelijks  | zaterdag  |   |
| Per jaar | zaterdag  |   |


### <a name="assign-storsimple-volumes-to-a-backup-exec-backup-job"></a>StorSimple-volumes toewijzen aan een back-uptaak Backup Exec

De volgende procedure wordt ervan uitgegaan dat Backup Exec en de doelhost zijn geconfigureerd in overeenstemming met de richtlijnen van de agent Backup Exec.

#### <a name="to-assign-storsimple-volumes-to-a-backup-exec-backup-job"></a>StorSimple-volumes toewijzen aan een back-uptaak Backup Exec

1.  Selecteer in de beheerconsole voor Backup Exec **Host** > **back-up** > **back-up naar schijf**.

    ![Back-up Exec-beheerconsole, selecteer host, back-up en back-up op schijf](./media/storsimple-configure-backup-target-using-backup-exec/image14.png)

2.  In de **back-up definitie eigenschappen** dialoogvenster onder **back-up**, selecteer **bewerken**.

    ![Back-up Exec management console, in het dialoogvenster Eigenschappen van back-up-definitie](./media/storsimple-configure-backup-target-using-backup-exec/image15.png)

3.  Uw volledige en incrementele back-ups instellen, zodat ze voldoen aan uw vereisten RPO en RTO en aan aanbevolen procedures voor Veritas voldoen.

4.  In de **voor back-up** in het dialoogvenster, selecteer **opslag**.

    ![Back-up Exec management console, in het dialoogvenster voor back-up opties voor opslag](./media/storsimple-configure-backup-target-using-backup-exec/image16.png)

5.  Bijbehorende StorSimple-volumes toewijzen aan uw back-upschema.

    > [!NOTE]
    > **Compressie** en **versleutelingstype** zijn ingesteld op **geen**.

6.  Onder **controleren**, selecteer de **gegevens voor deze taak niet controleren** selectievakje. Met deze optie kan van invloed zijn op StorSimple opslaglagen.

    > [!NOTE]
    > Defragmentatie, indexering en achtergrond verificatie beïnvloeden negatief de StorSimple-lagen.

    ![Back-up Exec management console voor back-up-instellingen controleren](./media/storsimple-configure-backup-target-using-backup-exec/image17.png)

7.  Wanneer u de rest van uw back-upopties om te voldoen aan uw vereisten hebt ingesteld, selecteert u **OK** om te voltooien.

## <a name="set-up-storsimple-as-a-secondary-backup-target"></a>StorSimple instellen als een secundaire back-updoel

> [!NOTE]
>Gegevens herstellen vanaf een back-up die naar de cloud is gelaagde optreden met een snelheid van de cloud.

In dit model, moet u een opslagmedium (met uitzondering van StorSimple) hebben om te fungeren als een tijdelijke cache. U kunt bijvoorbeeld een redundante matrix van onafhankelijke schijven (RAID) volume voor ruimte, input/output (I/O), en bandbreedte. Het is raadzaam om met behulp van RAID-5, 50 en 10.

De volgende afbeelding toont typische korte bewaartermijn lokaal (op de server) volumes en langdurige bewaarperioden archieven volumes. In dit scenario worden alle back-ups uitgevoerd op het lokale (op de server) RAID-volume. Deze back-ups worden periodiek gedupliceerd en gearchiveerd op een volume archieven. Het is belangrijk om de grootte van uw lokale (op de server) RAID-volume, zodat uw op korte termijn capaciteit en prestaties van vereisten voor de bewaarperiode kan worden verwerkt.

### <a name="storsimple-as-a-secondary-backup-target-gfs-example"></a>StorSimple als een voorbeeld van de algemene secundaire back-updoel

![StorSimple als secundaire back-updoel logisch diagram](./media/storsimple-configure-backup-target-using-backup-exec/secondarybackuptargetdiagram.png)

De volgende tabel laat zien hoe het instellen van back-ups uit te voeren op de lokale en de StorSimple-schijven. Capaciteitsvereisten voor afzonderlijke en de totale bevat.

### <a name="backup-configuration-and-capacity-requirements"></a>Back-upconfiguratie en capaciteitsvereisten van opslaggroepen

| Type back-up en retentie | Geconfigureerde opslag | Grootte (TiB) | Algemene vermenigvuldiger | Totale capaciteit\* (TiB) |
|---|---|---|---|---|
| Week 1 (volledige en incrementele) |Lokale schijf (op korte termijn)| 1 | 1 | 1 |
| StorSimple weken 2-4 |StorSimple-schijf (langlopende) | 1 | 4 | 4 |
| Maandelijks volledige |StorSimple-schijf (langlopende) | 1 | 12 | 12 |
| Jaarlijks volledige |StorSimple-schijf (langlopende) | 1 | 1 | 1 |
|Vereiste grootte van algemene volumes |  |  |  | 18*|

\* Totale capaciteit omvat 17 TiB van StorSimple-schijven en 1 TiB aan lokale RAID-volume.


### <a name="gfs-example-schedule-gfs-rotation-weekly-monthly-and-yearly-schedule"></a>Algemene voorbeeldschema: Algemene rotatie wekelijkse, maandelijkse en jaarlijkse planning

| Wekelijks | Volledig | Incrementele dag 1 | Incrementele dag 2 | Incrementele dag 3 | Incrementele dag 4 | Incrementele dag 5 |
|---|---|---|---|---|---|---|
| Week 1 | Lokale RAID-volume  | Lokale RAID-volume | Lokale RAID-volume | Lokale RAID-volume | Lokale RAID-volume | Lokale RAID-volume |
| Week 2 | StorSimple weken 2-4 |   |   |   |   |   |
| Week 3 | StorSimple weken 2-4 |   |   |   |   |   |
| Week 4 | StorSimple weken 2-4 |   |   |   |   |   |
| Maandelijks | Maandelijks StorSimple |   |   |   |   |   |
| Per jaar | StorSimple jaarlijks  |   |   |   |   |   |


### <a name="assign-storsimple-volumes-to-a-backup-exec-archive-and-deduplication-job"></a>StorSimple-volumes toewijzen aan een archief Exec back-up en Ontdubbeling van taak

#### <a name="to-assign-storsimple-volumes-to-a-backup-exec-archive-and-duplication-job"></a>StorSimple-volumes toewijzen aan een taak van archief- en duplicatie Backup Exec

1.  Met de rechtermuisknop op de taak die u wilt archiveren naar een StorSimple-volume en selecteer vervolgens in de beheerconsole voor Backup Exec **back-up definitie eigenschappen** > **bewerken**.

    ![Back-up Exec-beheerconsole, tabblad Eigenschappen van de back-up-definitie](./media/storsimple-configure-backup-target-using-backup-exec/image19.png)

2.  Selecteer **fase toevoegen** > **dupliceren naar schijf** > **bewerken**.

    ![Back-up van de console Groepsbeleidsbeheer Exec, fase toevoegen](./media/storsimple-configure-backup-target-using-backup-exec/image20.png)

3.  In de **dubbele opties** dialoogvenster vak, selecteert u de waarden die u gebruiken wilt voor **bron** en **planning**.

    ![Back-up Exec-beheerconsole, definities van back-eigenschappen en opties voor dubbele](./media/storsimple-configure-backup-target-using-backup-exec/image21.png)

4.  In de **opslag** vervolgkeuzelijst, selecteert u het StorSimple-volume waar u de taak archiveren voor het opslaan van de gegevens.

    ![Back-up Exec-beheerconsole, definities van back-eigenschappen en opties voor dubbele](./media/storsimple-configure-backup-target-using-backup-exec/image22.png)

5.  Selecteer **controleren**, en selecteer vervolgens de **gegevens voor deze taak niet controleren** selectievakje.

    ![Back-up Exec-beheerconsole, definities van back-eigenschappen en opties voor dubbele](./media/storsimple-configure-backup-target-using-backup-exec/image23.png)

6.  Selecteer **OK**.

    ![Back-up Exec-beheerconsole, definities van back-eigenschappen en opties voor dubbele](./media/storsimple-configure-backup-target-using-backup-exec/image24.png)

7.  In de **back-up** kolom, een nieuwe fase toevoegen. Gebruik voor de bron, **incrementele**. Voor het doel is, kiest u het StorSimple-volume waar de incrementele back-uptaak is gearchiveerd. Herhaal stappen 1-6.

## <a name="storsimple-cloud-snapshots"></a>StorSimple cloud-momentopnamen

StorSimple cloud-momentopnamen bescherming van de gegevens die zich in uw StorSimple-apparaat bevinden. Het maken van een cloud-momentopname is gelijk aan het verzenden van lokale back-uptapes op een gebouw van een externe locatie. Als u geografisch redundante opslag met Azure, is het maken van een cloud-momentopname gelijk aan het verzenden van back-uptapes op meerdere sites zijn. Als u herstellen van een apparaat na een noodgeval wilt, kunt u mogelijk een andere StorSimple-apparaat online brengen en een failover. Na de failover, zou u kunnen krijgen tot de gegevens (snelheden cloud) van de meest recente cloudmomentopname zijn.

De volgende sectie wordt beschreven hoe u een korte script om te starten en verwijderen van StorSimple cloud-momentopnamen tijdens back-up na de verwerking van maken.

> [!NOTE]
> Momentopnamen die zijn handmatig of via een programma gemaakt Volg het vervalbeleid voor StorSimple snapshot niet. Deze momentopnamen moeten handmatig of via een programma verwijderen.

### <a name="start-and-delete-cloud-snapshots-by-using-a-script"></a>Start en cloudmomentopnamen verwijderen met behulp van een script

> [!NOTE]
> Evalueer zorgvuldig de naleving en de gegevens bewaren gevolgen voordat u een StorSimple-momentopname verwijderen. Zie voor meer informatie over het uitvoeren van een script dat na de [Backup Exec documentatie](https://www.veritas.com/support/en_US/article.100032497.html).

### <a name="backup-lifecycle"></a>Back-levenscyclus

![Back-Lifecycle-diagram](./media/storsimple-configure-backup-target-using-backup-exec/backuplifecycle.png)

### <a name="requirements"></a>Vereisten

-   De server waarop het script wordt uitgevoerd moet toegang hebben tot Azure-cloudresources.
-   Het gebruikersaccount moet de vereiste machtigingen hebben.
-   Een back-upbeleid van StorSimple met de bijbehorende StorSimple-volumes moet worden ingesteld maar niet is ingeschakeld.
-   U moet de naam van de StorSimple-resource, de registratiesleutel, apparaat-id op de naam en een back-beleid wordt gebruikt.

### <a name="to-start-or-delete-a-cloud-snapshot"></a>Om te starten of een cloudmomentopname verwijderen

1. [Installeer Azure PowerShell](/powershell/azure/overview).
2. Downloaden en installeren [beheren CloudSnapshots.ps1](https://github.com/anoobbacker/storsimpledevicemgmttools/blob/master/Manage-CloudSnapshots.ps1) PowerShell-script.
3. Voer op de server waarop het script wordt uitgevoerd, PowerShell als beheerder. Zorg ervoor dat u het script uitgevoerd `-WhatIf $true` om te zien wat het script verandert maakt. Nadat de validatie voltooid is, doorgeven `-WhatIf $false`. Voer de onderstaande opdracht:
   ```powershell
   .\Manage-CloudSnapshots.ps1 -SubscriptionId [Subscription Id] -TenantId [Tenant ID] -ResourceGroupName [Resource Group Name] -ManagerName [StorSimple Device Manager Name] -DeviceName [device name] -BackupPolicyName [backup policyname] -RetentionInDays [Retention days] -WhatIf [$true or $false]
   ```
4. Het script toevoegen aan uw back-uptaak in Backup Exec door het bewerken van uw back-up Exec taak options vooraf verwerken en na het verwerken van opdrachten.

   ![Back-up Exec-console, back-upopties, tabblad vóór en na verwerking opdrachten](./media/storsimple-configure-backup-target-using-backup-exec/image25.png)

> [!NOTE]
> Het is raadzaam dat u uw back-upbeleid van StorSimple cloud-momentopname als een na verwerking script aan het einde van uw dagelijkse back-uptaak uitvoeren. Voor meer informatie over hoe u het back-up en herstellen van uw back-uptoepassing omgeving om te voldoen aan de RPO en de RTO, neem contact op met uw back-architect.

## <a name="storsimple-as-a-restore-source"></a>StorSimple als een bron herstellen

Herstelt vanuit een StorSimple-apparaat werk, zoals het terugzetten van een apparaat met blokopslag. Herstellen van gegevens die naar de cloud is gelaagd vindt plaats met een snelheid van de cloud. Voor lokale gegevens plaatsvinden herstelt op de snelheid van de lokale schijf van het apparaat. Zie de documentatie voor Backup Exec voor meer informatie over het uitvoeren van een herstelpunt. Het is raadzaam dat u aan best practices voor Backup Exec terugzetten voldoet.

## <a name="storsimple-failover-and-disaster-recovery"></a>StorSimple failover en herstel na noodgevallen

> [!NOTE]
> Voor back-updoel scenario's, wordt StorSimple-Cloudapparaat niet ondersteund als het doel van een herstel.

Na een noodgeval kan worden veroorzaakt door een verscheidenheid aan factoren. De volgende tabel bevat algemene herstel na noodgevallen.

| Scenario | Impact | Herstellen | Opmerkingen |
|---|---|---|---|
| Fout voor StorSimple-apparaat | Back-up en herstelbewerkingen zijn onderbroken. | Vervang het mislukte apparaat en uit te voeren [StorSimple failover en herstel na noodgevallen](storsimple-device-failover-disaster-recovery.md). | Als u uitvoeren van een herstelbewerking na het herstel van apparaat wilt, worden de volledige gegevens wordt vanuit de cloud naar het nieuwe apparaat opgehaald. Alle bewerkingen zijn met een snelheid van de cloud. Het proces voor indexering en catalogiseren scannen kan leiden tot alle back-upsets worden gescand en opgehaald uit de cloudlaag naar de lokale apparaat-laag, dit kan een tijdrovend proces. |
| Back-up Exec server mislukt | Back-up en herstelbewerkingen zijn onderbroken. | Opnieuw opbouwen van de back-upserver en database terugzetten, zoals beschreven in [hoe u een handmatige back-up en herstellen van back-up uitvoeren (BEDB)-database doet](http://www.veritas.com/docs/000041083). | U moet opnieuw samenstellen of herstellen van de back-up Exec-server op de site voor noodherstel. De database herstellen naar de meest recente. Als de herstelde database met Backup Exec niet gesynchroniseerd met uw meest recente back-uptaken is, is indexeren en catalogiseren vereist. Deze index en de catalogus opnieuw scannen proces kunnen leiden tot alle back-upsets worden gescand en opgehaald uit de cloudlaag naar de prijscategorie van het lokale apparaat. Hierdoor kunt u meer tijd-intensieve. |
| Sitefout die in het verlies van gegevens van de Backup-server en de StorSimple resulteert | Back-up en herstelbewerkingen zijn onderbroken. | StorSimple eerst terugzetten en herstel vervolgens back-up uitvoeren. | StorSimple eerst terugzetten en herstel vervolgens back-up uitvoeren. Als u uitvoeren van een herstelbewerking na het herstel van apparaat wilt, worden de volledige gegevens wordt opgehaald vanuit de cloud naar het nieuwe apparaat. Alle bewerkingen zijn met een snelheid van de cloud. |

## <a name="references"></a>Verwijzingen

De volgende documenten werd voor dit artikel verwezen:

- [Multipath i/o-instelling van StorSimple](storsimple-configure-mpio-windows-server.md)
- [Scenario's voor opslag: Thin provisioning](https://msdn.microsoft.com/library/windows/hardware/dn265487.aspx)
- [Met behulp van de GPT-schijven](https://msdn.microsoft.com/windows/hardware/gg463524.aspx#EHD)
- [Instellen van schaduwkopieën voor gedeelde mappen](https://technet.microsoft.com/library/cc771893.aspx)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [terugzetten vanuit een back-upset](storsimple-restore-from-backup-set-u2.md).
- Meer informatie over het uitvoeren van [apparaat failover en herstel na noodgevallen](storsimple-device-failover-disaster-recovery.md).
