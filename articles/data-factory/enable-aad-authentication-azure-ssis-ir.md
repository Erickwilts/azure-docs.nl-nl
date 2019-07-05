---
title: Azure Active Directory-verificatie inschakelen voor Azure-SSIS Integration Runtime | Microsoft Docs
description: Dit artikel wordt beschreven hoe u Azure Active Directory-verificatie met de beheerde identiteit voor Azure Data Factory te maken van Azure-SSIS Integration Runtime in te schakelen.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: conceptual
ms.date: 5/14/2019
author: swinarko
ms.author: sawinark
manager: craigg
ms.openlocfilehash: f3d0aaee624bdba169f13313bb57a3ebe8075592
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67490055"
---
# <a name="enable-azure-active-directory-authentication-for-azure-ssis-integration-runtime"></a>Azure Active Directory-verificatie inschakelen voor Azure-SSIS Integration Runtime

Dit artikel leest u hoe u Azure Active Directory (Azure AD)-verificatie met de beheerde identiteit voor uw Azure Data Factory (ADF) inschakelen en gebruiken in plaats van SQL-verificatie om te maken van een Azure-SSIS Integration Runtime (IR) die op zijn beurt wordt ingericht SSIS-catalogusdatabase (SSISDB) in Azure SQL Database-server/beheerd exemplaar uit uw naam.

Zie voor meer informatie over de beheerde identiteit voor uw ADF [beheerde identiy voor Data Factory](https://docs.microsoft.com/azure/data-factory/data-factory-service-identity).

> [!NOTE]
>-  In dit scenario, Azure AD-verificatie met de beheerde identiteit voor uw ADF wordt alleen gebruikt bij het maken en verdere vanaf bewerkingen van uw SSIS-IR wordt in de inrichten en verbinding maken met SSISDB. Uitvoeringen van SSIS-pakket, wordt nog steeds uw SSIS-IR verbinding maken met SQL-verificatie gebruiken met volledig beheerde accounts die zijn gemaakt tijdens het inrichten van SSISDB SSISDB.
>-  Als u al uw SSIS-IR hebt gemaakt met behulp van SQL-verificatie, wordt u deze voor het gebruik van Azure AD-verificatie via PowerShell op dit moment niet opnieuw kunt configureren, maar wordt u kunt dit doen via Azure portal/ADF-app. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="enable-azure-ad-on-azure-sql-database"></a>Azure AD in Azure SQL Database inschakelen

Azure SQL Database-server biedt ondersteuning voor het maken van een database met een Azure AD-gebruiker. Eerst moet u een Azure AD-groep maken met de beheerde identiteit voor uw ADF als een lid. Vervolgens moet u een Azure AD-gebruiker instellen als de Active Directory-beheerder voor uw Azure SQL Database-server en maak verbinding met het op SQL Server Management Studio (SSMS) hebt met behulp van die gebruiker. Tot slot moet u een contained-gebruiker die de Azure AD-groep, zodat de beheerde identiteit voor uw ADF kan worden gebruikt door Azure-SSIS IR maken SSISDB namens maken.

### <a name="create-an-azure-ad-group-with-the-managed-identity-for-your-adf-as-a-member"></a>Maken van een Azure AD-groep met de beheerde identiteit voor uw ADF als een lid

U kunt een bestaande Azure AD-groep of maak een nieuwe Azure AD PowerShell gebruiken.

1.  Installeer de [Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2) module.

2.  Meld u aan met behulp van `Connect-AzureAD`, voert u de volgende cmdlet om een groep te maken en opslaan in een variabele:

    ```powershell
    $Group = New-AzureADGroup -DisplayName "SSISIrGroup" `
                              -MailEnabled $false `
                              -SecurityEnabled $true `
                              -MailNickName "NotSet"
    ```

    Het resultaat ziet er het volgende voorbeeld, waarin de variabele waarde wordt ook weergegeven:

    ```powershell
    $Group

    ObjectId DisplayName Description
    -------- ----------- -----------
    6de75f3c-8b2f-4bf4-b9f8-78cc60a18050 SSISIrGroup
    ```

3.  De beheerde identiteit voor uw ADF toevoegen aan de groep. U kunt het artikel volgen [beheerde identiy voor Data Factory](https://docs.microsoft.com/azure/data-factory/data-factory-service-identity) om op te halen van de principal beheerde identiteit Object-ID (bijvoorbeeld 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc, maar gebruik geen beheerde identiteit toepassings-ID voor dit doel).

    ```powershell
    Add-AzureAdGroupMember -ObjectId $Group.ObjectId -RefObjectId 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc
    ```

    U kunt ook het groepslidmaatschap daarna controleren.

    ```powershell
    Get-AzureAdGroupMember -ObjectId $Group.ObjectId
    ```

### <a name="configure-azure-ad-authentication-for-azure-sql-database-server"></a>Azure AD-verificatie voor Azure SQL Database-server configureren

U kunt [configureren en beheren van Azure AD-verificatie met behulp van SQL](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure) met behulp van de volgende stappen uit:

1.  Selecteer in Azure portal, **alle services** -> **SQL-servers** in de navigatie aan de linkerkant.

2.  Selecteer uw Azure SQL Database-server kan worden geconfigureerd met Azure AD-verificatie.

3.  In de **instellingen** sectie van de blade, selecteer **Active Directory-beheerder**.

4.  Selecteer in de opdrachtbalk **beheerder instellen**.

5.  Selecteer een Azure AD-gebruikersaccount moet de beheerder van de server worden gemaakt en selecteer vervolgens **selecteren.**

6.  Selecteer in de opdrachtbalk **opslaan.**

### <a name="create-a-contained-user-in-azure-sql-database-server-representing-the-azure-ad-group"></a>Een contained-gebruiker maken in Azure SQL Database-server die de Azure AD-groep

Voor deze stap moet u [Microsoft SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS).

1. Start SSMS.

2. In de **verbinding maken met Server** dialoogvenster, geef de naam van uw Azure SQL Database-server in de **servernaam** veld.

3. In de **verificatie** veld **Active Directory - Universal met ondersteuning voor MFA** (u kunt ook de andere twee typen van Active Directory-verificatie, Zie [configureren en beheren Azure AD-verificatie met behulp van SQL](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure)).

4. In de **gebruikersnaam** en voer de naam van Azure AD-account die u hebt ingesteld als de beheerder van de server, bijvoorbeeld testuser@xxxonline.com.

5. Selecteer **Connect** en het aanmeldingsproces te voltooien.

6. In de **Objectverkenner**, vouw de **Databases** -> **systeemdatabases** map.

7. Met de rechtermuisknop op **master** database en selecteer **nieuwe query**.

8. Voer in het queryvenster de volgende T-SQL-opdracht, en selecteer **Execute** op de werkbalk.

   ```sql
   CREATE USER [SSISIrGroup] FROM EXTERNAL PROVIDER
   ```

   De opdracht voltooid is, het maken van een contained-gebruiker om weer te geven van de groep.

9. Schakelt u het queryvenster, voer de volgende T-SQL-opdracht, en selecteer **Execute** op de werkbalk.

   ```sql
   ALTER ROLE dbmanager ADD MEMBER [SSISIrGroup]
   ```

   De opdracht voltooid is, de mogelijkheid om een database (SSISDB) te maken van de ingesloten gebruiker verlenen.

10. Als uw SSISDB is gemaakt met behulp van SQL-verificatie en u overschakelen wilt voor het gebruik van Azure AD-verificatie voor uw Azure-SSIS IR om deze te openen, met de rechtermuisknop op **SSISDB** database en selecteer **nieuwe query**.

11. Voer in het queryvenster de volgende T-SQL-opdracht, en selecteer **Execute** op de werkbalk.

    ```sql
    CREATE USER [SSISIrGroup] FROM EXTERNAL PROVIDER
    ```

    De opdracht voltooid is, het maken van een contained-gebruiker om weer te geven van de groep.

12. Schakelt u het queryvenster, voer de volgende T-SQL-opdracht, en selecteer **Execute** op de werkbalk.

    ```sql
    ALTER ROLE db_owner ADD MEMBER [SSISIrGroup]
    ```

    De opdracht voltooid is, de ingesloten gebruikers de mogelijkheid voor toegang tot SSISDB verlenen.

## <a name="enable-azure-ad-on-azure-sql-database-managed-instance"></a>Azure AD op beheerd exemplaar voor Azure SQL Database inschakelen

Azure SQL Database Managed Instance biedt ondersteuning voor het maken van een database met de beheerde identiteit voor uw ADF rechtstreeks. U moet niet deelnemen aan de beheerde identiteit voor uw ADF kunt u een Azure AD-groep of een contained-gebruiker die van die groep in het beheerde exemplaar te maken.

### <a name="configure-azure-ad-authentication-for-azure-sql-database-managed-instance"></a>Azure AD-verificatie configureren voor Azure SQL Database Managed Instance

Volg de stappen in [inrichten van een Azure Active Directory-beheerder voor uw beheerde exemplaar](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure#provision-an-azure-active-directory-administrator-for-your-managed-instance).

### <a name="add-the-managed-identity-for-your-adf-as-a-user-in-azure-sql-database-managed-instance"></a>De beheerde identiteit voor uw ADF toevoegen als een gebruiker in Azure SQL Database Managed Instance

Voor deze stap moet u [Microsoft SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS).

1.  Start SSMS.

2.  Verbinding maken met uw beheerde exemplaar met een SQL Server-account dat een **sysadmin**. Dit is een tijdelijke beperking die wordt verwijderd nadat Azure AD server-principals (aanmeldingen) voor Azure SQL Database Managed Instance algemene beschikbaarheid. U kunt de volgende fout wordt weergegeven als u probeert een Azure AD-beheerdersaccount gebruiken om te maken van de aanmelding: Msg 15247, 16 niveau 1 staat, regel 1 gebruiker heeft geen machtiging om deze actie uitvoeren.

3.  In de **Objectverkenner**, vouw de **Databases** -> **systeemdatabases** map.

4.  Met de rechtermuisknop op **master** database en selecteer **nieuwe query**.

5.  Haal de beheerde identiteit voor uw ADF. Kunt u het artikel volgen [beheerde identiy voor Data Factory](https://docs.microsoft.com/azure/data-factory/data-factory-service-identity) naar de principal beheerde identiteit toepassings-ID ophalen (maar geen beheerde identiteit Object-ID gebruiken voor dit doel).

6.  Voer in het queryvenster de volgende T-SQL-script als u wilt converteren van de beheerde identiteit voor uw ADF in binaire type:

    ```sql
    DECLARE @applicationId uniqueidentifier = '{your Managed Identity Application ID}'
    select CAST(@applicationId AS varbinary)
    ```
    
    De opdracht voltooid is, de beheerde identiteit voor uw ADF als binair weergeeft.

7.  Wissen van de query-venster en voer de volgende T-SQL-script voor het toevoegen van de beheerde identiteit voor uw ADF als een gebruiker

    ```sql
    CREATE LOGIN [{a name for the managed identity}] FROM EXTERNAL PROVIDER with SID = {your Managed Identity Application ID as binary}, TYPE = E
    ALTER SERVER ROLE [dbcreator] ADD MEMBER [{the managed identity name}]
    ALTER SERVER ROLE [securityadmin] ADD MEMBER [{the managed identity name}]
    ```
    
    De opdracht voltooid is, de beheerde identiteit verlening voor uw ADF de mogelijkheid om een database (SSISDB) te maken.

8.  Als uw SSISDB is gemaakt met behulp van SQL-verificatie en u overschakelen wilt voor het gebruik van Azure AD-verificatie voor uw Azure-SSIS IR om deze te openen, met de rechtermuisknop op **SSISDB** database en selecteer **nieuwe query**.

9.  Voer in het queryvenster de volgende T-SQL-opdracht, en selecteer **Execute** op de werkbalk.

    ```sql
    CREATE USER [{the managed identity name}] FOR LOGIN [{the managed identity name}] WITH DEFAULT_SCHEMA = dbo
    ALTER ROLE db_owner ADD MEMBER [{the managed identity name}]
    ```

    De opdracht voltooid is, de beheerde identiteit verlening voor uw ADF de mogelijkheid voor toegang tot SSISDB.

## <a name="provision-azure-ssis-ir-in-azure-portaladf-app"></a>Azure-SSIS IR inrichten in Azure portal/ADF-app

Wanneer u uw Azure-SSIS-IR in Azure portal/ADF-app te richten op **SQL-instellingen** weergeeft, schakelt **gebruik AAD-verificatie met de beheerde identiteit voor uw ADF** optie. De volgende schermafbeelding ziet u de instellingen voor IR met Azure SQL Database-server waarop SSISDB wordt gehost. Voor IR met Managed Instance hosten van SSISDB, de **Catalog Database-servicelaag** en **kunnen Azure-services toegang krijgen tot** instellingen zijn niet van toepassing, terwijl andere instellingen hetzelfde zijn.

Zie voor meer informatie over het maken van een Azure-SSIS IR [maken van een Azure-SSIS integratieruntime in Azure Data Factory](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime).

![Instellingen voor de Azure-SSIS integratieruntime](media/enable-aad-authentication-azure-ssis-ir/enable-aad-authentication.png)

## <a name="provision-azure-ssis-ir-with-powershell"></a>Azure-SSIS IR inrichten met PowerShell

Voor het inrichten van uw Azure-SSIS IR met PowerShell, moet u de volgende dingen doen:

1.  Installeer [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/tag/v5.5.0-March2018) module.

2.  In het script niet instelt `CatalogAdminCredential` parameter. Bijvoorbeeld:

    ```powershell
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -Description $AzureSSISDescription `
                                               -Type Managed `
                                               -Location $AzureSSISLocation `
                                               -NodeSize $AzureSSISNodeSize `
                                               -NodeCount $AzureSSISNodeNumber `
                                               -Edition $AzureSSISEdition `
                                               -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                               -CatalogServerEndpoint $SSISDBServerEndpoint `
                                               -CatalogPricingTier $SSISDBPricingTier

    Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                                 -DataFactoryName $DataFactoryName `
                                                 -Name $AzureSSISName
   ```
