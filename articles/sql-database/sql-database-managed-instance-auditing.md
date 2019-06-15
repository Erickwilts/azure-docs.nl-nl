---
title: Azure SQL Database managed instance-controle | Microsoft Docs
description: Leer hoe u aan de slag met Azure SQL Database managed instance auditing met T-SQL
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
f1_keywords:
- mi.azure.sqlaudit.general.f1
author: vainolo
ms.author: arib
ms.reviewer: vanto
manager: craigg
ms.date: 04/08/2019
ms.openlocfilehash: 6ada2a5e505bfe37f4f9a956570d8b6f38f55e55
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60702770"
---
# <a name="get-started-with-azure-sql-database-managed-instance-auditing"></a>Aan de slag met Azure SQL Database managed instance-controle

[Beheerd exemplaar](sql-database-managed-instance.md) auditing worden databasegebeurtenissen bijgehouden en schrijft deze naar een auditlogboek in uw Azure storage-account. Ook controle:

- Helpt u naleving van regelgeving, inzicht in de databaseactiviteiten en inzicht krijgen in discrepanties en afwijkingen die kunnen wijzen op problemen voor het bedrijf of vermoedelijke beveiligingsschendingen.
- Hiermee wordt en vergemakkelijkt de naleving van standaarden voor compliance, hoewel het nalevingsbeleid geen garantie. Zie voor meer informatie over Azure-programma's die ondersteuning voor standaarden naleving, de [Azure Trust Center](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942) waar u de meest recente lijst met SQL Database-nalevingscertificeringen kunt vinden.

## <a name="set-up-auditing-for-your-server-to-azure-storage"></a>Instellen van de controle voor uw server naar Azure storage

Het volgende gedeelte bevat de configuratie van de controle op uw beheerde exemplaar.

1. Ga naar de [Azure Portal](https://portal.azure.com).
1. Maken van een Azure Storage **container** waar de logboeken voor controle worden opgeslagen.

   1. Navigeer naar de Azure-opslag waar u wilt voor het opslaan van uw auditlogboeken.

      > [!IMPORTANT]
      > Een opslagaccount in dezelfde regio als het beheerde exemplaar gebruiken om te voorkomen dat de regio-overschrijdende lees-/ schrijfbewerkingen.

   1. In de storage-account, gaat u naar **overzicht** en klikt u op **Blobs**.

      ![Azure Blob-widget](./media/sql-managed-instance-auditing/1_blobs_widget.png)

   1. In het bovenste menu, klikt u op **+ Container** om een nieuwe container te maken.

      ![Pictogram voor blob-container maken](./media/sql-managed-instance-auditing/2_create_container_button.png)

   1. Geef een container **naam**, openbare toegang instellen voor **persoonlijke**, en klik vervolgens op **OK**.

      ![Configuratie van de blob-container maken](./media/sql-managed-instance-auditing/3_create_container_config.png)

1. Na het maken van de container voor de Audit logboeken er zijn twee manieren om te configureren als het doel van de auditlogboeken: [met T-SQL](#blobtsql) of [met behulp van SQL Server Management Studio (SSMS) UI](#blobssms):

   - <a id="blobtsql"></a>BLOB-opslag met behulp van T-SQL-controlelogboeken configureren:

     1. In de lijst met containers, klikt u op de nieuwe container en klik vervolgens op **containereigenschappen**.

        ![Knop voor BLOB-container-eigenschappen](./media/sql-managed-instance-auditing/4_container_properties_button.png)

     1. Kopieer de URL van de container door te klikken op het pictogram voor kopiëren en sla de URL (bijvoorbeeld in Kladblok) op voor toekomstig gebruik. De container-URL-notatie moet zijn `https://<StorageName>.blob.core.windows.net/<ContainerName>`

        ![De URL van het exemplaar voor BLOB-container](./media/sql-managed-instance-auditing/5_container_copy_name.png)

     1. Genereren van een Azure Storage **SAS-Token** verlenen beheerd exemplaar controle toegangsrechten tot het opslagaccount:

        - Navigeer naar de Azure Storage-account waar u de container in de vorige stap hebt gemaakt.

        - Klik op **handtekening voor gedeelde toegang** in het menu instellingen voor de opslag.

          ![Shared access signature pictogram in instellingenmenu opslag](./media/sql-managed-instance-auditing/6_storage_settings_menu.png)

        - De SAS is als volgt configureren:

          - **Toegestane services**: Blob

          - **Begindatum**: tijdzone-gerelateerde om problemen te voorkomen, is het aanbevolen gebruik van de datum van gisteren

          - **Einddatum**: Kies de datum waarop deze SAS-Token verloopt

            > [!NOTE]
            > Verleng het token na het verstrijken ter voorkoming van fouten voor controle.

          - Klik op **SAS genereren**.
            
            ![SAS-configuratie](./media/sql-managed-instance-auditing/7_sas_configure.png)

        - Nadat u hebt geklikt op SAS genereren, de SAS-Token wordt weergegeven aan de onderkant. Kopieer het token door te klikken op het pictogram voor kopiëren en sla het op (bijvoorbeeld in Kladblok) voor toekomstig gebruik.

          ![Kopieer de SAS-token](./media/sql-managed-instance-auditing/8_sas_copy.png)

          > [!IMPORTANT]
          > Verwijder het vraagteken ("?") tekens vanaf het begin van het token.

     1. Verbinding maken met uw beheerde exemplaar via SQL Server Management Studio (SSMS) of een andere ondersteunde hulpprogramma.

     1. Voer de volgende T-SQL-instructie op **een nieuwe referentie maken** met behulp van de Container-URL en de SAS-Token die u hebt gemaakt in de vorige stappen:

        ```SQL
        CREATE CREDENTIAL [<container_url>]
        WITH IDENTITY='SHARED ACCESS SIGNATURE',
        SECRET = '<SAS KEY>'
        GO
        ```

     1. Voer de volgende T-SQL-instructie voor het maken van een nieuwe Server Audit (Kies de naam van uw eigen controleren, gebruikt u de Container-URL die u hebt gemaakt in de vorige stappen). Indien niet opgegeven, `RETENTION_DAYS` standaardwaarde is 0 (onbeperkte bewaarperiode):

        ```SQL
        CREATE SERVER AUDIT [<your_audit_name>]
        TO URL ( PATH ='<container_url>' [, RETENTION_DAYS =  integer ])
        GO
        ```

        1. Gaat u door met [het maken van een Server Audit Specification of Auditspecificatie](#createspec)

   - <a id="blobssms"></a>Configureer de blob-opslag voor logboeken voor controle met behulp van SQL Server Management Studio (SSMS) 18 (Preview):

     1. Verbinding maken met het beheerde exemplaar met behulp van SQL Server Management Studio (SSMS) de gebruikersinterface.

     1. Vouw de opmerking hoofdmap van het Object Explorer.

     1. Vouw de **Security** knooppunt, klik met de rechtermuisknop op de **controles** -knooppunt en klik op 'Nieuwe Audit':

        ![Vouw beveiliging en controle-knooppunt](./media/sql-managed-instance-auditing/10_mi_SSMS_new_audit.png)

     1. Zorg ervoor dat 'URL' is geselecteerd in **Audit bestemming** en klikt u op **Bladeren**:

        ![Azure-opslag bladeren](./media/sql-managed-instance-auditing/11_mi_SSMS_audit_browse.png)

     1. (Optioneel) Meld u aan bij uw Azure-account:

        ![Aanmelden bij Azure](./media/sql-managed-instance-auditing/12_mi_SSMS_sign_in_to_azure.png)

     1. Selecteer een abonnement, de storage-account en de Blob-container in de vervolgkeuzelijsten of uw eigen container maken door te klikken op **maken**. Wanneer u klaar bent met klikt u op **OK**:

        ![Azure-abonnement, het storage-account en blob-container selecteren](./media/sql-managed-instance-auditing/13_mi_SSMS_select_subscription_account_container.png)

     1. Klik op **OK** in het dialoogvenster 'Audit maken'.

1. <a id="createspec"></a>Na het configureren van de Blob-container als doel voor de auditlogboeken maken een Server Audit Specification of Database Audit Specification zoals u zou doen voor SQL Server:

   - [Handleiding voor Server audit specification T-SQL maken](https://docs.microsoft.com/sql/t-sql/statements/create-server-audit-specification-transact-sql)
   - [Maken van Database audit specification T-SQL-handleiding](https://docs.microsoft.com/sql/t-sql/statements/create-database-audit-specification-transact-sql)

1. Schakel de controle van de server die u hebt gemaakt in stap 6:

    ```SQL
    ALTER SERVER AUDIT [<your_audit_name>]
    WITH (STATE=ON);
    GO
    ```

Meer informatie:

- [Controle van de verschillen tussen individuele databases, elastische pool, s en beheerde exemplaren in Azure SQL Database en de databases in SQL Server](#auditing-differences-between-databases-in-azure-sql-database-and-databases-in-sql-server)
- [SERVERAUDIT MAKEN](https://docs.microsoft.com/sql/t-sql/statements/create-server-audit-transact-sql)
- [ALTER SERVERAUDIT](https://docs.microsoft.com/sql/t-sql/statements/alter-server-audit-transact-sql)

## <a name="set-up-auditing-for-your-server-to-event-hub-or-azure-monitor-logs"></a>Instellen van de controle voor uw server naar Event Hub of Azure Monitor-Logboeken

Controlelogboeken van een beheerd exemplaar kunnen worden verzonden naar zelfs Hubs of de logboeken van Azure Monitor. Deze sectie wordt beschreven hoe u dit wilt configureren:

1. Navigeer de [Azure Portal](https://portal.azure.com/) met het beheerde exemplaar.

2. Klik op **diagnostische instellingen**.

3. Klik op **diagnostische gegevens inschakelen**. Als diagnostische gegevens al is ingeschakeld de *+ diagnostische instelling toevoegen* in plaats daarvan wordt weergegeven.

4. Selecteer **SQLSecurityAuditEvents** in de lijst van Logboeken.

5. Selecteer een doel voor de controlegebeurtenissen - Event Hub en/of de logboeken van Azure Monitor. Configureren voor elk doel van de vereiste parameters (bijvoorbeeld Log Analytics-werkruimte).

6. Klik op **Opslaan**.

    ![Diagnostische instellingen configureren](./media/sql-managed-instance-auditing/9_mi_configure_diagnostics.png)

7. Verbinding maken met het beheerde exemplaar via **SQL Server Management Studio (SSMS)** of een andere ondersteunde client.

8. Voer de volgende T-SQL-instructie voor het maken van een serveraudit:

    ```SQL
    CREATE SERVER AUDIT [<your_audit_name>] TO EXTERNAL_MONITOR;
    GO
    ```

9. Maak een auditspecificatie server of een auditspecificatie zoals u zou voor SQL Server doen:

   - [Handleiding voor Server audit specification T-SQL maken](https://docs.microsoft.com/sql/t-sql/statements/create-server-audit-specification-transact-sql)
   - [Maken van Database audit specification T-SQL-handleiding](https://docs.microsoft.com/sql/t-sql/statements/create-database-audit-specification-transact-sql)

10. Schakel de controle van de server hebt gemaakt in stap 8:
 
    ```SQL
    ALTER SERVER AUDIT [<your_audit_name>] WITH (STATE=ON);
    GO
    ```

## <a name="consume-audit-logs"></a>Auditlogboeken

### <a name="consume-logs-stored-in-azure-storage"></a>Logboeken die zijn opgeslagen in Azure Storage gebruiken

Er zijn verschillende methoden die u gebruiken kunt om blob auditing logboeken weer te geven.

- Gebruik de systeemfunctie `sys.fn_get_audit_file` (T-SQL) om te retourneren van de audit log-gegevens in tabelvorm. Zie voor meer informatie over het gebruik van deze functie, de [sys.fn_get_audit_file documentatie](https://docs.microsoft.com/sql/relational-databases/system-functions/sys-fn-get-audit-file-transact-sql).

- U kunt de logboeken voor controle verkennen met behulp van een hulpprogramma zoals [Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/). In Azure storage worden controleren logboeken opgeslagen als een verzameling van blob-bestanden in een container die is gedefinieerd voor het opslaan van de auditlogboeken. Zie voor meer informatie over de hiërarchie van de map storage naamgevingsregels en logboekindeling, de [Blobverwijzing Audit Log indeling](https://go.microsoft.com/fwlink/?linkid=829599).

- Voor een volledige lijst van audit log verbruikmethoden, raadpleegt u de [aan de slag met SQL database auditing](sql-database-auditing.md).

### <a name="consume-logs-stored-in-event-hub"></a>Logboeken die zijn opgeslagen in de Event Hub gebruiken

Als u wilt controleren Logboeken gegevens uit Event Hub gebruiken, moet u voor het instellen van een stroom gebeurtenissen gebruiken en te schrijven naar een doel. Zie voor meer informatie de documentatie over Azure Event Hubs.

### <a name="consume-and-analyze-logs-stored-in-azure-monitor-logs"></a>Verbruiken en logboeken die zijn opgeslagen in Azure controleren logboeken analyseren

Als u de logboeken voor controle worden vastgelegd in Logboeken van Azure Monitor, zijn ze beschikbaar in de Log Analytics-werkruimte waar u geavanceerde zoekopdrachten op de controlegegevens uitvoeren kunt. Als uitgangspunt, gaat u naar de Log Analytics-werkruimte en klik vervolgens onder *algemene* sectie Klik *logboeken* en voer een eenvoudige query, zoals: `search "SQLSecurityAuditEvents"` om weer te geven van de audit-Logboeken.  

Logboeken in Azure Monitor kunt u realtime operationele inzichten met behulp van geïntegreerde Zoek- en aangepaste dashboards voor het analyseren van miljoenen records gemakkelijk in uw werkbelastingen en servers. Zie voor meer nuttige informatie over opdrachten en Azure Monitor logboeken zoektaal, [Azure Monitor-verwijzing naar de logboeken](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview).

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="auditing-differences-between-databases-in-azure-sql-database-and-databases-in-sql-server"></a>Controle van de verschillen tussen databases in Azure SQL Database en -databases in SQL Server

De belangrijkste verschillen tussen de controle van databases in Azure SQL Database en de databases in SQL Server zijn:

- Met de implementatie-optie voor het beheerde exemplaar in Azure SQL Database, controle werkt op het serverniveau en de winkels `.xel` logboekbestanden in Azure Blob-opslag.
- Met de individuele databases en elastische pool implementatie-opties in Azure SQL Database auditing werkt op het databaseniveau van de.
- In on-premises SQL Server / virtuele machines, audit werkt op de server niveau, maar gebeurtenissen worden opgeslagen in bestanden system/windows-gebeurtenislogboeken.

XEvent controle voor beheerd exemplaar biedt ondersteuning voor Azure Blob storage-doelen. Bestands- en windows logboeken zijn **niet ondersteund**.

De sleutel verschillen de `CREATE AUDIT` syntaxis voor het controleren van Azure Blob-opslag zijn:

- Een nieuwe syntaxis `TO URL` wordt geleverd en kunt u de URL van de Azure blob Storage-container op te geven waar de `.xel` bestanden worden geplaatst.
- Een nieuwe syntaxis `TO EXTERNAL MONITOR` is opgegeven voor het inschakelen van zelfs Hub en Azure Monitor logboeken doelen.
- De syntaxis van de `TO FILE` is **niet ondersteund** omdat SQL Database heeft geen toegang Windows-bestandsshares tot.
- Optie voor afsluiten is **niet ondersteund**.
- `queue_delay` 0 is **niet ondersteund**.

## <a name="next-steps"></a>Volgende stappen

- Voor een volledige lijst van audit log verbruikmethoden, raadpleegt u de [aan de slag met SQL database auditing](sql-database-auditing.md).
- Zie voor meer informatie over Azure-programma's die ondersteuning voor standaarden naleving, de [Azure Trust Center](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942) waar u de meest recente lijst met SQL Database-nalevingscertificeringen kunt vinden.

<!--Image references-->









