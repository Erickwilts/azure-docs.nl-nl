---
title: Aanbevolen procedures voor Azure SQL Data Sync | Microsoft Docs
description: Meer informatie over aanbevolen procedures voor het configureren en uitvoeren van Azure SQL Data Sync.
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: allenwux
ms.author: xiwu
ms.reviewer: carlrab
ms.date: 12/20/2018
ms.openlocfilehash: 01962770c011a0107abd4e035c25d6c0d45fa0a0
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68569380"
---
# <a name="best-practices-for-sql-data-sync"></a>Aanbevolen procedures voor SQL Data Sync 

In dit artikel worden aanbevolen procedures voor Azure SQL Data Sync beschreven.

Zie [Gegevens synchroniseren tussen meerdere cloud- en on-premises databases met SQL Data Sync](sql-database-sync-data.md) voor een overzicht van SQL Data Sync.

> [!IMPORTANT]
> Azure SQL Data Sync biedt op dit moment **geen** ondersteuning voor beheerde exemplaren voor Azure SQL Database.

## <a name="security-and-reliability"></a>Beveiliging en betrouw baarheid

### <a name="client-agent"></a>Client agent

-   Installeer de client agent met behulp van het minst bevoegde gebruikers account dat toegang heeft tot de netwerk service.  
-   Installeer de client agent op een computer die niet de on-premises SQL Server computer is.  
-   Registreer een on-premises data base niet met meer dan één agent.    
    -   Vermijd dit, zelfs als u verschillende tabellen synchroniseert voor verschillende synchronisatie groepen.  
    -   Het registreren van een on-premises data base met meerdere client agenten vormt uitdagingen wanneer u een van de synchronisatie groepen verwijdert.

### <a name="database-accounts-with-least-required-privileges"></a>Database accounts met mini maal vereiste bevoegdheden

-   **Voor de synchronisatie-instellingen**. Create/ALTER TABLE; Alter data base; Procedure maken; Schema selecteren/wijzigen; Maak een door de gebruiker gedefinieerd type.

-   **Voor een voortdurende synchronisatie**. Selecteren/invoegen/bijwerken/verwijderen voor tabellen die zijn geselecteerd voor synchronisatie, en op meta gegevens synchroniseren en tracerings tabellen. Machtiging voor uitvoeren voor opgeslagen procedures die zijn gemaakt door de service; Machtiging voor uitvoeren voor door de gebruiker gedefinieerde tabel typen.

-   **Voor**het ongedaan maken van de inrichting. Wijzigen van het gedeelte van de synchronisatie van tabellen. Selecteren/verwijderen bij het synchroniseren van meta gegevens tabellen; Beheer van tabellen voor synchronisatie bijhouden, opgeslagen procedures en door de gebruiker gedefinieerde typen.

Azure SQL Database ondersteunt slechts één set met referenties. Als u deze taken binnen deze beperking wilt uitvoeren, moet u rekening houden met de volgende opties:

-   Wijzig de referenties voor de verschillende fasen (bijvoorbeeld *credentials1* voor Setup en *credentials2* voor doorlopend).  
-   Wijzig de machtiging van de referenties (dat wil zeggen, de machtiging wijzigen nadat de synchronisatie is ingesteld).

## <a name="setup"></a>Instellen

### <a name="database-considerations-and-constraints"></a>Overwegingen en beperkingen voor de data base

#### <a name="sql-database-instance-size"></a>Grootte van SQL Database-exemplaar

Wanneer u een nieuw exemplaar van SQL Database maakt, stelt u de maximum grootte in zodat deze altijd groter is dan de data base die u implementeert. Als u niet de maximale grootte instelt op een waarde die groter is dan de geïmplementeerde data base, mislukt de synchronisatie. Hoewel SQL Data Sync geen automatische groei biedt, kunt u de `ALTER DATABASE` opdracht uitvoeren om de grootte van de data base te verg Roten nadat deze is gemaakt. Zorg ervoor dat u binnen de limieten voor de SQL Database-instantie grootte blijft.

> [!IMPORTANT]
> SQL Data Sync worden extra meta gegevens opgeslagen bij elke Data Base. Zorg ervoor dat u rekening houden met deze meta gegevens wanneer u de benodigde schijf ruimte berekent. De hoeveelheid toegevoegde overhead is gerelateerd aan de breedte van de tabellen (er zijn bijvoorbeeld smalle tabellen vereist voor meer overhead) en de hoeveelheid verkeer.

### <a name="table-considerations-and-constraints"></a>Tabel overwegingen en-beperkingen

#### <a name="selecting-tables"></a>Tabellen selecteren

U hoeft niet alle tabellen in een-data base in een synchronisatie groep op te maken. De tabellen die u in een synchronisatie groep opneemt, zijn van invloed op de efficiëntie en de kosten. Voeg tabellen toe en de tabellen waarvan ze afhankelijk zijn, in een synchronisatie groep, alleen als deze nodig zijn voor het bedrijf.

#### <a name="primary-keys"></a>Primaire sleutels

Elke tabel in een synchronisatie groep moet een primaire sleutel hebben. De SQL Data Sync-Service kan geen tabel synchroniseren die geen primaire sleutel heeft.

Voordat u SQL Data Sync in productie gebruikt, test u initiële en doorlopende synchronisatie prestaties.

#### <a name="empty-tables-provide-the-best-performance"></a>Lege tabellen bieden de beste prestaties

Lege tabellen bieden de beste prestaties bij de initialisatie tijd. Als de doel tabel leeg is, gebruikt de gegevens synchronisatie massaal invoegen om de gegevens te laden. Anders wordt met de gegevens synchronisatie een rij-voor-rij-vergelijking en een invoeg actie voor het controleren op conflicten gecontroleerd. Als de prestaties geen betrekking hebben, kunt u echter synchronisatie instellen tussen tabellen die al gegevens bevatten.

### <a name="provisioning-destination-databases"></a>Doel databases inrichten

SQL Data Sync biedt de Basic-voorziening voor het maken van data bases.

In deze sectie worden de beperkingen van het inrichten van SQL Data Sync beschreven.

#### <a name="autoprovisioning-limitations"></a>Beperkingen voor het autoinrichten

SQL Data Sync heeft de volgende beperkingen voor het autoinrichten:

-   Selecteer alleen de kolommen die zijn gemaakt in de doel tabel. Kolommen die geen deel uitmaken van de synchronisatie groep, worden niet ingericht in de doel tabellen.
-   Er worden alleen indexen gemaakt voor geselecteerde kolommen. Als de bron tabel index kolommen bevat die geen deel uitmaken van de synchronisatie groep, worden deze indexen niet ingericht in de doel tabellen.  
-   Indexen voor kolommen van XML-type worden niet ingericht.  
-   Er zijn geen controle beperkingen ingericht.  
-   Bestaande triggers op de bron tabellen zijn niet ingericht.  
-   Weer gaven en opgeslagen procedures worden niet gemaakt op de doel database.
-   BIJ TRAPSGEWIJS bijwerken en trapsgewijs verwijderen van trapsgewijze acties op refererende-sleutel beperkingen worden niet opnieuw gemaakt in de doel tabellen.
-   Als u decimale of numerieke kolommen hebt met een grotere precisie dan 28, kan SQL Data Sync tijdens de synchronisatie een probleem met de conversie overloop optreden. We raden u aan de precisie van decimale en numerieke kolommen te beperken tot 28 of minder.

#### <a name="recommendations"></a>Aanbevelingen

-   Gebruik de functie voor het automatisch inrichten van SQL Data Sync alleen wanneer u de service probeert uit te voeren.  
-   Richt het database schema in voor productie.

### <a name="locate-hub"></a>Locatie van de hub-data base

#### <a name="enterprise-to-cloud-scenario"></a>Scenario voor Enter prise-to-Cloud

Als u de latentie wilt minimaliseren, houdt u de hub-data base dicht bij de grootste concentratie van het database verkeer van de synchronisatie groep.

#### <a name="cloud-to-cloud-scenario"></a>Scenario voor Cloud-naar-Cloud

-   Wanneer alle data bases in een synchronisatie groep zich in één Data Center bevinden, moet de hub zich in hetzelfde Data Center bevinden. Deze configuratie vermindert de latentie en de kosten voor de overdracht van gegevens tussen data centers.
-   Wanneer de data bases in een synchronisatie groep zich in meerdere data centers bevinden, moet de hub zich in hetzelfde Data Center bevinden als de meeste data bases en database verkeer.

#### <a name="mixed-scenarios"></a>Gemengde scenario's

Pas de voor gaande richt lijnen toe op complexe synchronisatie groepen, zoals de configuraties die bestaan uit een combi natie van ondernemings-naar-Cloud-en Cloud-naar-Cloud-scenario's.

## <a name="sync"></a>Synchroniseren

### <a name="avoid-a-slow-and-costly-initial-synchronization"></a>Vermijd langzame en dure initiële synchronisatie

In deze sectie bespreken we de initiële synchronisatie van een synchronisatie groep. Meer informatie over hoe u kunt voor komen dat een initiële synchronisatie langer duurt en dat deze meer kosten in beslag neemt dan nodig is.

#### <a name="how-initial-sync-works"></a>De werking van de initiële synchronisatie

Wanneer u een synchronisatie groep maakt, moet u beginnen met gegevens in slechts één data base. Als u gegevens in meerdere data bases hebt, behandelt SQL Data Sync elke rij als een conflict dat moet worden opgelost. Deze conflict oplossing zorgt ervoor dat de initiële synchronisatie langzaam verloopt. Als u gegevens in meerdere data bases hebt, kan de initiële synchronisatie tussen enkele dagen en enkele maanden duren, afhankelijk van de grootte van de data base.

Als de data bases zich in verschillende data centers bevinden, moet elke rij worden getransporteerd tussen de verschillende data centers. Dit verhoogt de kosten van een initiële synchronisatie.

#### <a name="recommendation"></a>Aanbeveling

Als dat mogelijk is, begint u met alleen gegevens in een van de data bases van de synchronisatie groep.

### <a name="design-to-avoid-synchronization-loops"></a>Ontwerpen om synchronisatie lussen te voor komen

Een Sync-lus treedt op wanneer er kring verwijzingen zijn binnen een synchronisatie groep. In dat geval worden elke wijziging in de ene data base eindeloos en circulair gerepliceerd via de data bases in de synchronisatie groep.   

Zorg ervoor dat u de synchronisatie lussen voor komt, omdat deze de prestaties nadelig beïnvloeden en de kosten aanzienlijk kunnen verhogen.

### <a name="handling-changes-that-fail-to-propagate"></a>Wijzigingen die niet worden door gegeven

#### <a name="reasons-that-changes-fail-to-propagate"></a>Redenen waarom wijzigingen niet worden door gegeven

Wijzigingen kunnen niet worden door gegeven om een van de volgende redenen:

-   Incompatibiliteit van schema/data type.
-   Null invoegen in kolommen die geen Null-waarden bevatten.
-   De beperkingen van refererende sleutels zijn geschonden.

#### <a name="what-happens-when-changes-fail-to-propagate"></a>Wat gebeurt er wanneer wijzigingen niet worden door gegeven?

-   In de synchronisatie groep ziet u dat er een **waarschuwings** status is.
-   Details worden weer gegeven in de gebruikers interface logboek viewer van de portal.
-   Als het probleem gedurende 45 dagen niet is opgelost, wordt de data base verouderd.

> [!NOTE]
> Deze wijzigingen worden nooit door gegeven. De enige manier om in dit scenario te herstellen, is de synchronisatie groep opnieuw te maken.

#### <a name="recommendation"></a>Aanbeveling

Controleer de synchronisatie groep en de status van de data base regel matig via de portal en de logboek interface.


## <a name="maintenance"></a>Onderhoud

### <a name="avoid-out-of-date-databases-and-sync-groups"></a>Verouderde data bases en synchronisatie groepen voor komen

Een synchronisatie groep of een data base in een synchronisatie groep kan verouderd raken. Wanneer de status van een synchronisatie groep verouderd is, werkt deze **niet**meer. Wanneer de status van een Database verouderd is, kunnen gegevens verloren gaan. U kunt dit scenario het beste vermijden in plaats van het te herstellen.

#### <a name="avoid-out-of-date-databases"></a>Verouderde data bases voor komen

De status van een Data Base is ingesteld op verouderd wanneer deze gedurende 45 dagen offline is geweest. Om een verouderde status van een Data Base te vermijden, moet u ervoor zorgen dat geen van de data bases gedurende 45 dagen offline is.

#### <a name="avoid-out-of-date-sync-groups"></a>Verouderde synchronisatie groepen vermijden

De status van een synchronisatie groep is ingesteld op verouderd wanneer een wijziging in de synchronisatie groep gedurende 45 dagen of meer niet kan worden door gegeven aan de rest van de synchronisatie groep. Als u wilt voor komen dat de status van een synchronisatie groep verouderd is, controleert u het geschiedenis logboek van de synchronisatie groep regel matig. Zorg ervoor dat alle conflicten zijn opgelost en dat wijzigingen worden door gegeven in de data bases van de synchronisatie groep.

Een wijziging kan een van de volgende oorzaken mogelijk niet worden toegepast op een synchronisatie groep:

-   Incompatibiliteit van het schema tussen tabellen.
-   Gegevens incompatibiliteit tussen tabellen.
-   Het invoegen van een rij met een null-waarde in een kolom die geen Null-waarden toestaat.
-   Het bijwerken van een rij met een waarde die in strijd is met een foreign key-beperking.

Om te voor komen dat synchronisatie groepen verlopen:

-   Werk het schema bij zodat de waarden in de mislukte rijen worden toegestaan.
-   Werk de waarden van refererende sleutel bij zodat deze de waarden bevatten die in de mislukte rijen zijn opgenomen.
-   Werk de gegevens waarden in de mislukte rij bij zodat deze compatibel zijn met het schema of de refererende sleutels in de doel database.

### <a name="avoid-deprovisioning-issues"></a>Problemen voor het ongedaan maken van de inrichting vermijden

In sommige gevallen kan het opheffen van de registratie van een Data Base met een client agent ertoe leiden dat de synchronisatie mislukt.

#### <a name="scenario"></a>Scenario

1. Synchronisatie groep A is gemaakt met behulp van een SQL Database-exemplaar en een on-premises SQL Server-Data Base, die is gekoppeld aan lokale agent 1.
2. Dezelfde on-premises data base is geregistreerd bij lokale agent 2 (deze agent is niet gekoppeld aan een synchronisatie groep).
3. Bij het ongedaan maken van de registratie van de on-premises data base van lokale agent 2 worden de tracerings-en meta tabellen voor synchronisatie groep A voor de on-premises data base verwijderd.
4. Het uitvoeren van een bewerking voor de synchronisatie groep mislukt, met de volgende fout: "De huidige bewerking kan niet worden voltooid omdat de data base niet is ingericht voor synchronisatie of omdat u geen machtigingen hebt voor de synchronisatie configuratie tabellen."

#### <a name="solution"></a>Oplossing

U kunt dit scenario voor komen door een Data Base met meer dan één agent te registreren.

Herstellen uit dit scenario:

1. Verwijder de data base van elke synchronisatie groep waarvan deze deel uitmaakt.  
2. Voeg de Data Base weer toe aan elke synchronisatie groep waarvan u deze hebt verwijderd.  
3. Implementeer elke betrokken synchronisatie groep (deze actie is van toepassing op de data base).  

### <a name="modifying-your-sync-group"></a>Een synchronisatie groep wijzigen

Probeer geen data base uit een synchronisatie groep te verwijderen en bewerk vervolgens de synchronisatie groep zonder eerst een van de wijzigingen te implementeren.

In plaats daarvan verwijdert u eerst een Data Base uit een synchronisatie groep. Implementeer vervolgens de wijziging en wacht totdat het ongedaan maken van de inrichting is voltooid. Wanneer het ongedaan maken van de inrichting is voltooid, kunt u de synchronisatie groep bewerken en de wijzigingen implementeren.

Als u probeert een Data Base te verwijderen en vervolgens een synchronisatie groep te bewerken zonder eerst een van de wijzigingen te implementeren, mislukt één of de andere bewerking. De portal interface kan inconsistent worden. Als dit het geval is, vernieuwt u de pagina om de juiste status te herstellen.

## <a name="next-steps"></a>Volgende stappen
Voor meer informatie over SQL Data Sync raadpleegt u:

-   Overzicht: [Gegevens synchroniseren tussen meerdere cloud- en on-premises databases met SQL Data Sync](sql-database-sync-data.md)
-   Data Sync instellen
    - In de portal - [Zelfstudie: SQL Data Sync instellen om gegevens te synchroniseren tussen Azure SQL Database en SQL Server on-premises](sql-database-get-started-sql-data-sync.md)
    - Met PowerShell
        -  [PowerShell gebruiken om meerdere Azure SQL-databases te synchroniseren](scripts/sql-database-sync-data-between-sql-databases.md)
        -  [PowerShell gebruiken om te synchroniseren tussen een Azure SQL-database en een on-premises database](scripts/sql-database-sync-data-between-azure-onprem.md)
-   Data Sync-agent: [Data Sync-agent voor Azure SQL Data Sync](sql-database-data-sync-agent.md)
-   SQL Data Sync controleren [met Azure monitor](sql-database-sync-monitor-oms.md) -logboeken
-   Problemen oplossen: [Problemen met Azure SQL Data Sync oplossen](sql-database-troubleshoot-data-sync.md)
-   Het synchronisatieschema bijwerken
    -   Met Transact-SQL: [De replicatie van schemawijzigingen in Azure SQL Data Sync automatiseren](sql-database-update-sync-schema.md)
    -   Met PowerShell: [PowerShell gebruiken voor het bijwerken van het synchronisatieschema in een bestaande synchronisatiegroep](scripts/sql-database-sync-update-schema.md)

Voor meer informatie over SQL Database raadpleegt u:

-   [Overzicht van SQL Database](sql-database-technical-overview.md)
-   [Levenscyclus beheer van de data base](https://msdn.microsoft.com/library/jj907294.aspx)
