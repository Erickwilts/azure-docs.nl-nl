---
title: Een back-up van SQL Server-databases maken in Azure | Microsoft Docs
description: In deze zelfstudie wordt uitgelegd hoe u een back-up van SQL Server maakt in Azure. In het artikel wordt ook uitgelegd hoe u SQL Server kunt herstellen.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: tutorial
ms.date: 04/23/2019
ms.author: raynew
ms.openlocfilehash: 2a6319565aa05f34ce31a14c5fc57e591248f4ee
ms.sourcegitcommit: d89032fee8571a683d6584ea87997519f6b5abeb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66399695"
---
# <a name="about-sql-server-backup-in-azure-vms"></a>Over SQL Server-back-ups in virtuele Azure-machines

SQL Server-databases zijn essentiële workloads waarvoor een laag Recovery Point Objective (RPO, beoogd herstelpunt) en langetermijnretentie nodig zijn. U kunt back-up van SQL Server-databases die worden uitgevoerd op virtuele Azure-machines met behulp van [Azure Backup](backup-overview.md).

## <a name="backup-process"></a>Back-upproces

Deze oplossing maakt gebruik van de SQL native-API's als u wilt maken van back-ups van uw SQL-databases.

* Nadat u de SQL Server-VM die u wilt beveiligen en op te vragen voor de databases in deze opgeeft, Azure Backup-service de Backup-extensie van een werkbelasting op de virtuele machine wordt geïnstalleerd door de naam van de `AzureBackupWindowsWorkload`  extensie.
* Deze extensie bestaat uit een coördinator en een SQL-invoegtoepassing. Terwijl de coördinator verantwoordelijk is voor het activeren van werkstromen voor verschillende bewerkingen zoals back-up, back-up en herstel configureren, is de invoegtoepassing is verantwoordelijk voor het werkelijke gegevensstroom.
* Als u voor het detecteren van databases op deze virtuele machine, maakt Azure Backup de account `NT SERVICE\AzureWLBackupPluginSvc`. Dit account wordt gebruikt voor back-up en herstel en SQL sysadmin-machtigingen vereist. Azure Backup maakt gebruik van de `NT AUTHORITY\SYSTEM` account voor database-detectie/query, zodat dit account hoeft te worden van een openbare aanmelding via SQL. Als u de SQL Server-VM hebt gemaakt vanuit de Azure Marketplace, ontvangt u mogelijk een foutbericht **UserErrorSQLNoSysadminMembership**. Als dit het geval [Volg deze instructies](backup-azure-sql-database.md).
* Zodra u trigger beveiliging configureert voor de geselecteerde databases, stelt de Backup-service u de coördinator met de back-upschema's en andere beleidsdetails, waarvan de extensie caches lokaal op de virtuele machine 
* De coördinator communiceert met de invoegtoepassing op het geplande tijdstip, en start de back-upgegevens van de SQL-server met behulp van VDI streaming.  
* De invoegtoepassing verzendt die gegevens rechtstreeks naar de recovery services-kluis, waardoor dus niet nodig voor een tijdelijke locatie. De gegevens worden versleuteld en opgeslagen door de Azure Backup-service in de storage-accounts.
* Wanneer de overdracht van gegevens voltooid is, bevestigt coordinator doorvoeren met de Backup-service.

  ![SQL-back-architectuur](./media/backup-azure-sql-database/backup-sql-overview.png)

## <a name="before-you-start"></a>Voordat u begint

Voordat u begint, controleert u of de onderstaande:

1. Zorg ervoor dat er een SQL Server-exemplaar wordt uitgevoerd in Azure. U kunt [snel een SQL Server-exemplaar maken](../virtual-machines/windows/sql/quickstart-sql-vm-create-portal.md) in de marketplace.
2. Controleer de [functie overweging](#feature-consideration-and-limitations) en [scenario ondersteuning](#scenario-support).
3. [Lees de veelgestelde vragen](faq-backup-sql-server.md) over dit scenario.

## <a name="scenario-support"></a>Scenario-ondersteuning

**Ondersteuning** | **Details**
--- | ---
**Ondersteunde implementaties** | SQL Marketplace Azure-VM's en niet-Marketplace-VM's (SQL Server handmatig geïnstalleerd) worden ondersteund.
**Ondersteunde geografische gebieden** | Australië-Zuidoost (ASE), Oost-Australië (AE) <br> Brazilië - zuid (BRS)<br> Canada-centraal (CNC), Canada-Oost (CE)<br> Zuidoost-Azië (SEA), Oost-Azië (EA) <br> VS-Oost (EUS), VS-Oost 2 (EUS2), West Centraal (WCUS), VS-West (WUS); VS-West 2 (WUS 2) Noord-centraal VS (NCUS) VS-centraal (CUS) VS Zuid-centraal (SCUS) <br> India centraal (INC), India-Zuid (INS) <br> Japan East (JPE), Japan West (JPW) <br> Korea-centraal (KRC), Korea-Zuid (KRS) <br> Noord-Europa (NE), West-Europa <br> UK-Zuid (UKS), VK-West (UKW)
**Ondersteunde besturingssystemen** | Windows Server 2016, Windows Server 2012 R2, Windows Server 2012<br/><br/> Linux wordt momenteel niet ondersteund.
**Ondersteunde SQL Server-versies** | SQL Server 2017; SQL Server 2016, SQL Server 2014, SQL Server 2012.<br/><br/> Enterprise, Standard, Web, Developer, Express.
**Ondersteunde versies van .NET** | .NET framework 4.5.2 en hoger is geïnstalleerd op de virtuele machine

## <a name="feature-consideration-and-limitations"></a>Functie overwegingen en beperkingen

- SQL Server back-up kan worden geconfigureerd in Azure portal of **PowerShell**. We bieden geen ondersteuning voor CLI.
- De oplossing wordt ondersteund op beide typen van [implementaties](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model) -virtuele machines van Azure Resource Manager en klassieke virtuele machines.
- Virtuele machine met SQL Server moet verbinding met internet voor toegang tot Azure openbare IP-adressen.
- SQL Server **Failover Cluster exemplaar (FCI)** en SQL Server Always on Failover Cluster Instance worden niet ondersteund.
- Back-up en herstelbewerkingen voor gespiegelde databases en momentopnamen van databases worden niet ondersteund.
- Met behulp van meer dan een back-upoplossingen back-up uw zelfstandige SQL Server kan-exemplaar of SQL Always on-beschikbaarheidsgroep leiden tot back-upfouten; in dat geval weerhouden.
- Back-ups van twee knooppunten van een beschikbaarheidsgroep afzonderlijk met dezelfde of verschillende oplossingen, kunnen ook leiden tot back-upfouten.
- Azure Backup ondersteunt alleen volledige en kopie-alleen volledige back-uptypen voor **alleen-lezen** databases
- Databases met zeer veel bestanden kunnen niet worden beveiligd. Het maximum aantal bestanden dat wordt ondersteund is **~ 1000**.  
- U kunt maximaal back **ongeveer 2000** SQL Server-databases in een kluis. Als u een groter aantal databases hebt, kunt u meerdere kluizen maken.
- U kunt back-up voor maximaal **50** databases in een gaan; deze beperking kunt u back-belastingen optimaliseren.
- We bieden ondersteuning voor databases tot **2TB** in grootte; voor groter dan die van prestatieproblemen kunnen afkomstig zijn.
- Als u een idee van de garantie voor het aantal databases per server kunnen worden beveiligd, moeten we Houd rekening met factoren zoals bandbreedte, VM-grootte, back-upfrequentie, de grootte van de database, enzovoort. We werken op een planner waarmee u deze getallen op uw eigen berekenen. We zullen worden gepubliceerd binnenkort.
- Back-ups zijn in het geval van beschikbaarheidsgroepen afkomstig uit de verschillende knooppunten op basis van een aantal factoren. Hieronder staat een samenvatting van de back-gedrag voor een beschikbaarheidsgroep.

### <a name="back-up-behavior-in-case-of-always-on-availability-groups"></a>Back-up gedrag in het geval van altijd op beschikbaarheidsgroepen

Het wordt aanbevolen dat de back-up is geconfigureerd op slechts één knooppunt van een Beschikbaarheidsgroep. Back-up moet altijd worden geconfigureerd in dezelfde regio als het primaire knooppunt. Met andere woorden, moet u altijd het primaire knooppunt aanwezig zijn in de regio waarin u back-up wilt configureren. Als alle knooppunten van de Beschikbaarheidsgroep zich in dezelfde regio bevinden waarin de back-up is geconfigureerd, is er geen vragen.

**Voor de regio-overschrijdende AG**
- Back-ups niet, ongeacht de back-upvoorkeur uitgevoerd van de knooppunten die zich niet in dezelfde regio bevinden waarin de back-up is geconfigureerd. Dit komt doordat de regio-overschrijdende back-ups worden niet ondersteund. Als u alleen 2 knooppunten hebt en het secundaire knooppunt in de andere regio is; in dit geval wordt de back-ups blijven optreden van het primaire knooppunt (tenzij uw back-upvoorkeur 'alleen secundaire').
- Als het geval is een failover naar een regio die anders is dan de waarin de back-up is geconfigureerd, wordt back-ups mislukken op de knooppunten in de regio van de failover.

Back-ups zijn afhankelijk van de back-upvoorkeur en typen van back-ups (volledige/differentiële/log/kopie-alleen volledig) afkomstig uit een bepaald knooppunt (primair/secundair).

- **Back-upvoorkeur: Primary**

**Type back-up** | **Node**
    --- | ---
    Volledig | Primair
    Differentiële | Primair
    Logboek |  Primair
    Kopie-alleen volledige |  Primair

- **Back-upvoorkeur: Secondary Only**

**Type back-up** | **Node**
--- | ---
Volledig | Primair
Differentiële | Primair
Logboek |  Secundair
Kopie-alleen volledige |  Secundair

- **Back-upvoorkeur: Secondary**

**Type back-up** | **Node**
--- | ---
Volledig | Primair
Differentiële | Primair
Logboek |  Secundair
Kopie-alleen volledige |  Secundair

- **Geen voorkeur back-up**

**Type back-up** | **Node**
--- | ---
Volledig | Primair
Differentiële | Primair
Logboek |  Secundair
Kopie-alleen volledige |  Secundair

## <a name="fix-sql-sysadmin-permissions"></a>Problemen met systeembeheerdersrechten voor SQL oplossen

  Als u nodig hebt om op te lossen machtigingen vanwege **UserErrorSQLNoSysadminMembership** fout, voer de onderstaande stappen te volgen:

  1. Meld u bij SQL Server Management Studio (SSMS) aan met een account met systeembeheerdersrechten voor SQL Server. Windows-verificatie zou moeten werken, tenzij u speciale machtigingen nodig hebt.
  2. Open de map **Security/Logins** op de SQL-server.

      ![De map Security/Logins openen om accounts te bekijken](./media/backup-azure-sql-database/security-login-list.png)

  3. Klik met de rechtermuisknop op de map **Logins** en selecteer **Nieuwe aanmelding**. In **Aanmelding - Nieuw** selecteert u **Zoeken**.

      ![In het dialoogvenster Aanmelding - Nieuw selecteert u Zoeken](./media/backup-azure-sql-database/new-login-search.png)

  4. Het virtuele Windows-serviceaccount **NT SERVICE\AzureWLBackupPluginSvc** is gemaakt tijdens de registratie van de virtuele machine en de SQL-detectiefase. Geef de accountnaam op, zoals wordt weergegeven in **Objectnaam invoeren die moet worden geselecteerd**. Selecteer **Namen controleren** om de naam te herleiden. Klik op **OK**.

      ![Namen controleren selecteren om de naam van de onbekende service te herleiden](./media/backup-azure-sql-database/check-name.png)

  5. Controleer of in **Serverrollen** de rol **sysadmin** is geselecteerd. Klik op **OK**. De vereiste machtigingen moeten nu bestaan.

      ![Controleren of de serverfunctie sysadmin is geselecteerd](./media/backup-azure-sql-database/sysadmin-server-role.png)

  6. Koppel de database nu aan de Recovery Services-kluis. Klik in de Azure-portal in de lijst **Beschermde servers** met de rechtermuisknop op de server met een foutstatus > **DB's opnieuw detecteren**.

      ![Controleren of de server de juiste machtigingen heeft](./media/backup-azure-sql-database/check-erroneous-server.png)

  7. Controleer de voortgang in het gebied **Meldingen**. Wanneer de geselecteerde databases zijn gevonden, wordt er een slagingsbericht weergegeven.

      ![Bericht dat de implementatie is geslaagd](./media/backup-azure-sql-database/notifications-db-discovered.png)

> [!NOTE]
> Als uw SQL-Server meerdere exemplaren van SQL Server is geïnstalleerd heeft, wordt u sysadmin-machtigingen voor moet toevoegen **NT Service\AzureWLBackupPluginSvc** account op alle SQL-exemplaren.

## <a name="next-steps"></a>Volgende stappen

- [Meer informatie over](backup-sql-server-database-azure-vms.md) back-ups van SQL Server-databases.
- [Meer informatie](restore-sql-database-azure-vm.md) over het herstellen van SQL Server-databases vanuit back-up.
- [Meer informatie](manage-monitor-sql-database-backup.md) over het beheren van SQL Server-databases vanuit back-up.
