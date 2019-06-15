---
title: Configureren van verificatie in Azure Active Directory - SQL | Microsoft Docs
description: Leer hoe u verbinding maken met SQL Database Managed Instance en SQL Data Warehouse met behulp van Azure Active Directory-verificatie - na het configureren van Azure AD.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: data warehouse
ms.devlang: ''
ms.topic: conceptual
author: GithubMirek
ms.author: mireks
ms.reviewer: vanto, carlrab
manager: craigg
ms.date: 03/12/2019
ms.openlocfilehash: abb4a43176026fca5a80409ade13af1f8f96d9f1
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60390483"
---
# <a name="configure-and-manage-azure-active-directory-authentication-with-sql"></a>Configureren en beheren van Azure Active Directory-verificatie met behulp van SQL

Dit artikel leest u hoe u kunt maken en vullen van Azure AD en vervolgens Azure AD gebruiken met Azure [SQL-Database](sql-database-technical-overview.md), [Managed Instance](sql-database-managed-instance.md), en [SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md). Zie voor een overzicht [Azure Active Directory-verificatie](sql-database-aad-authentication.md).

> [!NOTE]
> In dit artikel is van toepassing op Azure SQL-server en op zowel SQL Database en SQL Data Warehouse-databases die zijn gemaakt op de Azure SQL-server. Voor het gemak wordt de term 'SQL Database' gebruikt wanneer er wordt verwezen naar zowel SQL Database als SQL Data Warehouse.
> [!IMPORTANT]  
> Verbinding maken met SQL Server die wordt uitgevoerd op een Azure-VM wordt niet ondersteund met behulp van Azure Active Directory-account. Gebruik in plaats daarvan een domein Active Directory-account.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> De PowerShell Azure Resource Manager-module nog steeds wordt ondersteund door Azure SQL Database, maar alle toekomstige ontwikkeling is voor de module Az.Sql. Zie voor deze cmdlets [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). De argumenten voor de opdrachten in de Az-module en de AzureRm-modules zijn vrijwel identiek zijn.

## <a name="create-and-populate-an-azure-ad"></a>Maken en vullen van een Azure AD

Maak een Azure AD en deze vullen met gebruikers en groepen. Azure AD kan de eerste Azure AD worden beheerde domein. Azure AD kan ook worden voor een on-premises Active Directory Domain Services die is gefedereerd met Azure AD.

Zie [Uw on-premises identiteiten integreren met Azure Active Directory](../active-directory/hybrid/whatis-hybrid-identity.md), [Uw domeinnaam toevoegen in Azure AD](../active-directory/active-directory-domains-add-azure-portal.md), [Microsoft Azure ondersteunt nu federatie met Windows Server Active Directory](https://azure.microsoft.com/blog/20../../windows-azure-now-supports-federation-with-windows-server-active-directory/), [Uw Azure AD-adreslijst beheren](../active-directory/fundamentals/active-directory-administer.md), [Azure AD beheren met Windows PowerShell](/powershell/azure/overview) en [Poorten en protocollen waarvoor hybride identiteit is vereist](../active-directory/hybrid/reference-connect-ports.md) voor meer informatie.

## <a name="associate-or-add-an-azure-subscription-to-azure-active-directory"></a>Koppelen of een Azure-abonnement toevoegen aan Azure Active Directory

1. Koppelt uw Azure-abonnement met Azure Active Directory door de map een vertrouwde map voor het Azure-abonnement met de database. Zie voor meer informatie, [hoe Azure-abonnementen worden gekoppeld aan Azure AD](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md).
2. Gebruik de directorywisselaar in Azure portal om te schakelen naar het abonnement dat is gekoppeld aan het domein.

   **Aanvullende informatie:** Voor elk Azure-abonnement is er een vertrouwensrelatie met een Azure AD-exemplaar. Dit betekent dat er op die directory wordt vertrouwd voor het verifiëren van gebruikers, services en apparaten. Meerdere abonnementen kunnen dezelfde directory vertrouwen, maar een abonnement vertrouwt slechts één directory. De vertrouwensrelatie die een abonnement heeft met een directory is anders dan de relatie die een abonnement heeft met andere resources in Azure (websites, databases, enzovoort); deze resources lijken meer op onderliggende resources van een abonnement. Als een abonnement is verlopen, wordt toegang tot de andere resources die zijn gekoppeld aan het abonnement ook geblokkeerd. De directory blijft echter wel aanwezig in Azure, en u kunt er een ander abonnement aan koppelen om de directorygebruikers te blijven beheren. Zie voor meer informatie over resources [inzicht krijgen in resourcetoegang in Azure](../active-directory/active-directory-b2b-admin-add-users.md). Voor meer informatie over dit vertrouwde relatie Zie [koppelen of een Azure-abonnement toevoegen aan Azure Active Directory](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md).

## <a name="create-an-azure-ad-administrator-for-azure-sql-server"></a>Een Azure AD-beheerder voor Azure SQL-server maken

Elke Azure SQL-server (die als host fungeert voor een SQL-Database of SQL Data Warehouse) begint met een administrator-account van één server die de beheerder van de hele Azure SQL-server. Een tweede SQL Server-beheerder moet worden gemaakt, die een Azure AD-account. Deze principal is gemaakt als een ingesloten databasegebruiker in de database master. Als beheerder, de server-beheerdersaccounts lid zijn van de **db_owner** rol in elke gebruiker van de database en voert u elke gebruikersdatabase als de **dbo** gebruiker. Zie voor meer informatie over de beheerdersaccounts server [Databases en aanmeldingen in Azure SQL Database beheren](sql-database-manage-logins.md).

Wanneer u Azure Active Directory met geo-replicatie, moet de Azure Active Directory-beheerder voor zowel de primaire en secundaire servers worden geconfigureerd. Als een server heeft geen beheerder van de Azure Active Directory, wordt Azure Active Directory-aanmeldingen en gebruikers een ' kan geen verbinding maken' op server-fout.

> [!NOTE]
> Gebruikers die niet zijn gebaseerd op een Azure AD-account (met inbegrip van de administrator-account van de Azure SQL-server), kan Azure AD-gebruikers, maken, omdat ze geen machtiging voor het valideren van de voorgestelde databasegebruikers met de Azure AD.

## <a name="provision-an-azure-active-directory-administrator-for-your-managed-instance"></a>Inrichten van een Azure Active Directory-beheerder voor uw beheerde exemplaar

> [!IMPORTANT]
> Volg deze stappen alleen als u een beheerd exemplaar inricht. Met deze bewerking kan alleen worden uitgevoerd door globale of bedrijfsbeheerder beheerder in Azure AD. Volgende stappen beschrijven het proces voor het verlenen van machtigingen voor gebruikers met verschillende bevoegdheden in de directory.

Het beheerde exemplaar moeten toegangsmachtigingen voor het lezen van Azure AD om uit te voeren is taken, zoals verificatie van gebruikers via het lidmaatschap van beveiligingsgroep of het maken van nieuwe gebruikers. Om dit te werken, moet u machtigingen verlenen voor beheerd exemplaar voor het lezen van Azure AD. Er zijn twee manieren om dit te doen: vanuit de Portal en PowerShell. De volgende stappen beide methoden.

1. Selecteer de verbinding met de vervolgkeuzelijst een lijst van mogelijke Active Directory's in de rechterbovenhoek van de Azure Portal.
2. Kies de juiste Active Directory als de standaard-Azure AD.

   Deze stap koppelt u het abonnement dat is gekoppeld aan Active Directory met Managed Instance is ervoor te zorgen dat hetzelfde abonnement wordt gebruikt voor zowel Azure AD en het beheerde exemplaar.
3. Navigeer naar Managed Instance en een resourcegroep selecteren die u wilt gebruiken voor Azure AD-integratie.

   ![AAD](./media/sql-database-aad-authentication/aad.png)

4. Selecteer de banner boven op de pagina Active Directory-beheerder en machtigingen verlenen voor de huidige gebruiker. Als u bent aangemeld als beheerder van de globale of bedrijfsbeheerder in Azure AD, kunt u dit doen vanuit Azure portal of met behulp van PowerShell met het onderstaande script.

    ![verlenen van machtigingen-portal](./media/sql-database-aad-authentication/grant-permissions.png)

    ```powershell
    # Gives Azure Active Directory read permission to a Service Principal representing the Managed Instance.
    # Can be executed only by a "Company Administrator" or "Global Administrator" type of user.

    $aadTenant = "<YourTenantId>" # Enter your tenant ID
    $managedInstanceName = "MyManagedInstance"

    # Get Azure AD role "Directory Users" and create if it doesn't exist
    $roleName = "Directory Readers"
    $role = Get-AzureADDirectoryRole | Where-Object {$_.displayName -eq $roleName}
    if ($role -eq $null) {
        # Instantiate an instance of the role template
        $roleTemplate = Get-AzureADDirectoryRoleTemplate | Where-Object {$_.displayName -eq $roleName}
        Enable-AzureADDirectoryRole -RoleTemplateId $roleTemplate.ObjectId
        $role = Get-AzureADDirectoryRole | Where-Object {$_.displayName -eq $roleName}
    }

    # Get service principal for managed instance
    $roleMember = Get-AzureADServicePrincipal -SearchString $managedInstanceName
    $roleMember.Count
    if ($roleMember -eq $null)
    {
        Write-Output "Error: No Service Principals with name '$    ($managedInstanceName)', make sure that managedInstanceName parameter was     entered correctly."
        exit
    }
    if (-not ($roleMember.Count -eq 1))
    {
        Write-Output "Error: More than one service principal with name pattern '$    ($managedInstanceName)'"
        Write-Output "Dumping selected service principals...."
        $roleMember
        exit
    }

    # Check if service principal is already member of readers role
    $allDirReaders = Get-AzureADDirectoryRoleMember -ObjectId $role.ObjectId
    $selDirReader = $allDirReaders | where{$_.ObjectId -match     $roleMember.ObjectId}

    if ($selDirReader -eq $null)
    {
        # Add principal to readers role
        Write-Output "Adding service principal '$($managedInstanceName)' to     'Directory Readers' role'..."
        Add-AzureADDirectoryRoleMember -ObjectId $role.ObjectId -RefObjectId     $roleMember.ObjectId
        Write-Output "'$($managedInstanceName)' service principal added to     'Directory Readers' role'..."

        #Write-Output "Dumping service principal '$($managedInstanceName)':"
        #$allDirReaders = Get-AzureADDirectoryRoleMember -ObjectId $role.ObjectId
        #$allDirReaders | where{$_.ObjectId -match $roleMember.ObjectId}
    }
    else
    {
        Write-Output "Service principal '$($managedInstanceName)' is already     member of 'Directory Readers' role'."
    }
    ```

5. Nadat de bewerking is voltooid, wordt de volgende melding weergegeven in de rechterbovenhoek:

    ![voltooid](./media/sql-database-aad-authentication/success.png)

6. U kunt nu uw Azure AD-beheerder voor uw beheerde exemplaar. Voor die op de pagina Active Directory-beheerder, selecteer **beheerder instellen** opdracht.

    ![set-admin](./media/sql-database-aad-authentication/set-admin.png)

7. Selecteer in de AAD-beheerpagina, zoekt u een gebruiker, de gebruiker of groep moet een beheerder, en selecteer vervolgens **Selecteer**.

   De pagina Active Directory-beheerder ziet u alle leden en groepen van uw Active Directory. Gebruikers of groepen die zijn lichter gekleurd kunnen niet worden geselecteerd omdat ze worden niet ondersteund als Azure AD-beheerder. Zie de lijst met ondersteunde beheerders in [Azure AD-functies en beperkingen](sql-database-aad-authentication.md#azure-ad-features-and-limitations). Op rollen gebaseerd toegangsbeheer (RBAC) geldt alleen voor de Azure-portal en wordt niet doorgegeven aan SQL Server.

    ![toevoegen-beheerder](./media/sql-database-aad-authentication/add-admin.png)

8. Selecteer aan de bovenkant van de pagina Active Directory-beheerder, **opslaan**.

    ![Opslaan](./media/sql-database-aad-authentication/save.png)

    Het wijzigen van de beheerder kan enkele minuten duren. De nieuwe beheerder wordt vervolgens weergegeven in het vak van Active Directory-beheerder.

Nadat u hebt een Azure AD-beheerder voor uw beheerde exemplaar wordt ingericht, kunt u beginnen met het maken van Azure AD server-principals (aanmeldingen) (**openbare preview**) met de <a href="/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current">CREATE LOGIN</a> syntaxis. Zie voor meer informatie, [beheerd exemplaar overzicht](sql-database-managed-instance.md#azure-active-directory-integration).

> [!TIP]
> Als u later een beheerder, aan de bovenkant van de pagina Active Directory-beheerder selecteert **beheerder verwijderen**, en selecteer vervolgens **opslaan**.

## <a name="provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server"></a>Een Azure Active Directory-beheerder voor uw Azure SQL Database-server inrichten

> [!IMPORTANT]
> Volg deze stappen alleen als u een Azure SQL Database-server of de Data Warehouse inricht.

De volgende twee procedures laten zien hoe u voor het inrichten van een Azure Active Directory-beheerder voor uw Azure SQL-server in Azure portal en met behulp van PowerShell.

### <a name="azure-portal"></a>Azure Portal

1. Ga naar de [Azure-portal](https://portal.azure.com/) en selecteer in de rechterbovenhoek uw verbinding om een lijst met mogelijke Active Directories weer te geven. Kies de juiste Active Directory als de standaard-Azure AD. In deze stap wordt de aan het abonnement gekoppelde Active Directory gekoppeld aan de Azure SQL-server, zodat u zeker weet dat hetzelfde abonnement wordt gebruikt voor zowel Azure AD als SQL Server. (De Azure SQL-server kan worden gehost, Azure SQL Database of Azure SQL Data Warehouse.) ![Kies ad][8]

2. Selecteer in de linker banner **alle services**, en in het filtertype in **SQL server**. Selecteer **Sql-Servers**.

    ![sqlservers.png](media/sql-database-aad-authentication/sqlservers.png)

    >[!NOTE]
    > Op deze pagina voordat u **SQL-servers**, kunt u de **star** naast de naam van de *favoriete* de categorie en voeg **SQL-servers**naar de linker navigatiebalk.

3. Op **SQL Server** weergeeft, schakelt **Active Directory-beheerder**.
4. In de **Active Directory-beheerder** weergeeft, schakelt **beheerder instellen**.  ![active directory selecteren](./media/sql-database-aad-authentication/select-active-directory.png)  

5. Zoek op de pagina **Beheerder toevoegen** een gebruiker. Selecteer de gebruiker of groep die beheerder moet zijn en selecteer **Selecteren**. (Op de pagina Active Directory-beheerder ziet u alle leden en groepen van uw Active Directory.) Gebruikers of groepen die grijs zijn gekleurd, kunnen niet worden geselecteerd omdat ze niet worden ondersteund als beheerders voor Azure AD. (Zie de lijst met ondersteunde beheerders in de sectie **Azure AD-functies en -beperkingen** van [Azure Active Directory-verificatie gebruiken voor verificatie met behulp van SQL Database of SQL Data Warehouse](sql-database-aad-authentication.md).) Op rollen gebaseerd toegangsbeheer (RBAC) is alleen van toepassing op de portal en wordt niet doorgegeven aan SQL Server.
    ![beheerder selecteren](./media/sql-database-aad-authentication/select-admin.png)  

6. Selecteer boven aan de pagina **Active Directory-beheerder** de optie **Opslaan**.
    ![beheerder opslaan](./media/sql-database-aad-authentication/save-admin.png)

Het wijzigen van de beheerder kan enkele minuten duren. Vervolgens wordt de nieuwe beheerder weergegeven in het vak **Active Directory-beheerder**.

   > [!NOTE]
   > Als u de Azure AD-beheerder instelt, mag de naam van de beheerder (gebruiker of groep) niet al aanwezig zijn in de virtuele hoofddatabase als een gebruiker van SQL Server-verificatie. Indien wel aanwezig, mislukt het instellen van de Azure AD-beheerder, waarna het maken ervan wordt teruggedraaid en er wordt aangegeven dat deze beheerder (naam) al bestaat. Omdat een dergelijke gebruiker van SQL Server-verificatie geen deel uitmaakt van Azure AD, mislukt het verbinding maken met de server met Azure AD-verificatie.

Later verwijderen van een beheerder, aan de bovenkant van de **Active Directory-beheerder** weergeeft, schakelt **beheerder verwijderen**, en selecteer vervolgens **opslaan**.

### <a name="powershell"></a>PowerShell

PowerShell-cmdlets, hebt u nodig hebt van Azure PowerShell installeren en uitvoeren. Zie voor gedetailleerde informatie [Installeren en configureren van Azure PowerShell](/powershell/azure/overview). Voor het inrichten van een Azure AD-beheerder, voert u de volgende Azure PowerShell-opdrachten uit:

- Connect-AzAccount
- Select-AzSubscription

Cmdlets gebruikt voor het inrichten en beheren van Azure AD-beheerder:

| Naam van cmdlet | Description |
| --- | --- |
| [Set-AzSqlServerActiveDirectoryAdministrator](/powershell/module/az.sql/set-azsqlserveractivedirectoryadministrator) |Richt een Azure Active Directory-beheerder voor Azure SQL-server of Azure SQL Data Warehouse. (Moet zich in het huidige abonnement.) |
| [Remove-AzSqlServerActiveDirectoryAdministrator](/powershell/module/az.sql/remove-azsqlserveractivedirectoryadministrator) |Hiermee verwijdert u een Azure Active Directory-beheerder voor Azure SQL-server of Azure SQL Data Warehouse. |
| [Get-AzSqlServerActiveDirectoryAdministrator](/powershell/module/az.sql/get-azsqlserveractivedirectoryadministrator) |Retourneert informatie over een Azure Active Directory-beheerder die momenteel zijn geconfigureerd voor de Azure SQL-server of Azure SQL Data Warehouse. |

Gebruik PowerShell-opdracht get-help voor meer informatie voor elk van deze opdrachten, bijvoorbeeld ``get-help Set-AzSqlServerActiveDirectoryAdministrator``.

Het volgende script bepalingen een Azure AD-beheerder-groep met de naam **DBA_Group** (object-ID `40b79501-b343-44ed-9ce7-da4c8cc7353f`) voor de **demo_server** -server in een resourcegroep met de naam **23-groep**:

```powershell
Set-AzSqlServerActiveDirectoryAdministrator -ResourceGroupName "Group-23"
-ServerName "demo_server" -DisplayName "DBA_Group"
```

De **DisplayName** invoerparameter accepteert u de weergavenaam van de Azure AD of de User Principal-naam. Bijvoorbeeld, ``DisplayName="John Smith"`` en ``DisplayName="johns@contoso.com"``. Weergavenaam wordt ondersteund voor Azure AD-beveiligingsgroepen alleen de Azure AD.

> [!NOTE]
> De Azure PowerShell-opdracht ```Set-AzSqlServerActiveDirectoryAdministrator``` voorkomt niet dat u Azure AD-beheerders voor niet-ondersteunde gebruikers inrichten. Een niet-ondersteunde gebruiker kan worden ingericht, maar kan geen verbinding maken met een database.

Het volgende voorbeeld wordt de optionele **ObjectID**:

```powershell
Set-AzSqlServerActiveDirectoryAdministrator -ResourceGroupName "Group-23"
-ServerName "demo_server" -DisplayName "DBA_Group" -ObjectId "40b79501-b343-44ed-9ce7-da4c8cc7353f"
```

> [!NOTE]
> De Azure AD **ObjectID** is vereist wanneer de **DisplayName** is niet uniek. Om op te halen de **ObjectID** en **DisplayName** waarden, gebruikt u de Active Directory-sectie van de klassieke Azure Portal en bekijk de eigenschappen van een gebruiker of groep.

Het volgende voorbeeld retourneert informatie over de huidige Azure AD-beheerder voor Azure SQL-server:

```powershell
Get-AzSqlServerActiveDirectoryAdministrator -ResourceGroupName "Group-23" -ServerName "demo_server" | Format-List
```

Het volgende voorbeeld verwijdert u een Azure AD-beheerder:

```powershell
Remove-AzSqlServerActiveDirectoryAdministrator -ResourceGroupName "Group-23" -ServerName "demo_server"
```

U kunt ook Azure Active Directory-beheerder inrichten met behulp van de REST API's. Zie voor meer informatie, [Service Management REST API-verwijzing en bewerkingen voor Azure SQL Database-bewerkingen voor Azure SQL Database](https://docs.microsoft.com/rest/api/sql/)

### <a name="cli"></a>CLI  

U kunt ook een Azure AD-beheerder inrichten door het aanroepen van de volgende CLI-opdrachten:

| Opdracht | Description |
| --- | --- |
|[AZ sql server ad-beheerder maken](https://docs.microsoft.com/cli/azure/sql/server/ad-admin#az-sql-server-ad-admin-create) |Richt een Azure Active Directory-beheerder voor Azure SQL-server of Azure SQL Data Warehouse. (Moet zich in het huidige abonnement.) |
|[AZ sql server ad-beheerder verwijderen](https://docs.microsoft.com/cli/azure/sql/server/ad-admin#az-sql-server-ad-admin-delete) |Hiermee verwijdert u een Azure Active Directory-beheerder voor Azure SQL-server of Azure SQL Data Warehouse. |
|[lijst met AZ sql server ad-beheerder](https://docs.microsoft.com/cli/azure/sql/server/ad-admin#az-sql-server-ad-admin-list) |Retourneert informatie over een Azure Active Directory-beheerder die momenteel zijn geconfigureerd voor de Azure SQL-server of Azure SQL Data Warehouse. |
|[AZ sql server ad-beheerder bijwerken](https://docs.microsoft.com/cli/azure/sql/server/ad-admin#az-sql-server-ad-admin-update) |De Active Directory-beheerder voor een Azure SQL-server of Azure SQL Data Warehouse-updates. |

Zie voor meer informatie over de CLI-opdrachten, [SQL - az sql](https://docs.microsoft.com/cli/azure/sql/server).  

## <a name="configure-your-client-computers"></a>Uw clientcomputers configureren

Op alle clientcomputers waarvan uw toepassingen of gebruikers verbinding maken met Azure SQL Database of Azure SQL Data Warehouse met behulp van Azure AD-identiteiten, moet u de volgende software installeren:

- .NET framework 4.6 of hoger uit [ https://msdn.microsoft.com/library/5a4x27ek.aspx ](https://msdn.microsoft.com/library/5a4x27ek.aspx).
- Azure Active Directory-Verificatiebibliotheek voor SQL Server (**ADALSQL. DLL-bestand**) is beschikbaar in meerdere talen (x86 en amd64) uit het Downloadcentrum op [Microsoft Active Directory-Verificatiebibliotheek voor Microsoft SQL Server](https://www.microsoft.com/download/details.aspx?id=48742).

U kunt voldoen aan deze voorwaarden voldoet:

- Installeren van een [SQL Server 2016 Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) of [SQL Server Data Tools voor Visual Studio 2015](https://msdn.microsoft.com/library/mt204009.aspx) voldoet aan de vereiste .NET Framework 4.6.
- SSMS-versie installeert de x86 van **ADALSQL. DLL-bestand**.
- SSDT installeert de amd64-versie van **ADALSQL. DLL-bestand**.
- De meest recente Visual Studio van [Visual Studio-Downloads](https://www.visualstudio.com/downloads/download-visual-studio-vs) voldoet aan de vereiste .NET Framework 4.6, maar niet de vereiste amd64-versie van geïnstalleerd **ADALSQL. DLL-bestand**.

## <a name="create-contained-database-users-in-your-database-mapped-to-azure-ad-identities"></a>Maak ingesloten databasegebruikers in de database die is toegewezen aan Azure AD-identiteiten

>[!IMPORTANT]
>Beheerd exemplaar biedt nu ondersteuning voor Azure AD server-principals (aanmeldingen) (**preview-versie**), waarmee u kunt aanmeldingen kan maken van Azure AD-gebruikers, groepen of toepassingen. Azure AD server-principals (aanmeldingen) biedt de mogelijkheid om te verifiëren met uw beheerde exemplaar zonder databasegebruikers moet worden gemaakt als een ingesloten databasegebruiker. Zie voor meer informatie, [beheerd exemplaar overzicht](sql-database-managed-instance.md#azure-active-directory-integration). Zie voor de syntaxis voor het maken van Azure AD server-principals (aanmeldingen), <a href="/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current">CREATE LOGIN</a>.

Voor verificatie met Azure Active Directory is vereist dat databasegebruikers als gebruikers van ingesloten databases worden gemaakt. Een gebruiker van een ingesloten database op basis van een Azure AD-identiteit is een databasegebruiker die zich niet kan aanmelden bij de hoofddatabase en die wordt verwezen naar een identiteit in de Azure AD-directory die aan de database is gekoppeld. De Azure AD-identiteit kan een individueel gebruikersaccount of een groep zijn. Zie [Contained Database Users- Making Your Database Portable](https://msdn.microsoft.com/library/ff929188.aspx) (Gebruikers van ingesloten databases: een draagbare database maken) voor meer informatie over gebruikers van ingesloten databases.

> [!NOTE]
> Databasegebruikers kunnen niet in de Azure-portal worden gemaakt (met uitzondering van beheerders). RBAC-rollen worden niet doorgegeven aan SQL Server, SQL Database of SQL Data Warehouse. Azure RBAC-rollen worden gebruikt voor het beheren van Azure-resources en zijn niet van toepassing op databasemachtigingen. Bijvoorbeeld: de rol **Inzender voor SQL Server** verleent geen toegang om verbinding te maken met SQL Database of SQL Data Warehouse. De toestemming tot het verlenen van toegang moet rechtstreeks plaatsvinden in de database met behulp van Transact-SQL-instructies.
> [!WARNING]
> Speciale tekens als de dubbele punt `:` of de ampersand `&` worden niet ondersteund indien deze worden opgenomen als gebruikersnamen in de instructies T-SQL CREATE LOGIN of CREATE USER.

Voor het maken van een Azure AD-gebruiker op basis van die database (met uitzondering van de beheerder van de server die eigenaar is van de database), verbinding maken met de database met een Azure AD-identiteit als een gebruiker met ten minste de **ALTER ANY USER** machtiging. Gebruik vervolgens de volgende Transact-SQL-syntaxis:

```sql
CREATE USER <Azure_AD_principal_name> FROM EXTERNAL PROVIDER;
```

*Azure_AD_principal_name* mag de user principal name van een Azure AD-gebruiker of de weergavenaam voor een Azure AD-groep.

**Voorbeelden:** Een ingesloten database te maken gebruiker een Azure AD representeren federatief of beheerd domeingebruiker:

```sql
CREATE USER [bob@contoso.com] FROM EXTERNAL PROVIDER;
CREATE USER [alice@fabrikam.onmicrosoft.com] FROM EXTERNAL PROVIDER;
```

Geef de weergavenaam van een beveiligingsgroep voor het maken van een ingesloten databasegebruiker een Azure AD representeren of federatieve domeingroep:

```sql
CREATE USER [ICU Nurses] FROM EXTERNAL PROVIDER;
```

Een ingesloten databasegebruiker die een toepassing die verbinding maakt met behulp van een Azure AD-token maken:

```sql
CREATE USER [appName] FROM EXTERNAL PROVIDER;
```

> [!TIP]
> U kan niet rechtstreeks een gebruiker uit een Azure Active Directory dan de Azure Active Directory die is gekoppeld aan uw Azure-abonnement maken. Leden van andere Active Directory's die geïmporteerd gebruikers in de bijbehorende Active Directory zijn (ook wel externe gebruikers) kunnen echter worden toegevoegd aan een Active Directory-groep in de Active Directory-tenant. Als u een ingesloten databasegebruiker voor die AD-groep maakt, kunnen de gebruikers van de externe Active Directory toegang tot SQL-Database.

Voor meer informatie over het maken van die database-gebruikers op basis van Azure Active Directory-identiteiten, Zie [CREATE USER (Transact-SQL)](https://msdn.microsoft.com/library/ms173463.aspx).

> [!NOTE]
> Verwijderen van de Azure Active Directory-beheerder voor Azure SQL-server wordt voorkomen dat een gebruiker van Azure AD-verificatie verbinding te maken met de server. Indien nodig, onbruikbaar Azure AD-gebruikers kunnen handmatig worden verwijderd door de beheerder van een SQL-Database.
> [!NOTE]
> Als u ontvangt een **verbinding is een time-out**, moet u instellen de `TransparentNetworkIPResolution` parameter van de verbindingsreeks op false. Zie voor meer informatie, [time-out verbindingsprobleem met .NET Framework 4.6.1 - TransparentNetworkIPResolution](https://blogs.msdn.microsoft.com/dataaccesstechnologies/20../../connection-timeout-issue-with-net-framework-4-6-1-transparentnetworkipresolution/).

Wanneer u een gebruiker van een database maakt, krijgt die gebruiker de **CONNECT** machtiging en verbinding kunt maken aan die database als een lid van de **openbare** rol. In eerste instantie alleen machtigingen voor het beschikbaar voor de gebruiker zijn geen machtigingen verleend aan de **openbare** rol of machtigingen verleend aan de Azure AD-groepen dat ze lid van zijn. Wanneer u een Azure AD-gebruiker op basis van die database inricht, kunt u de gebruiker aanvullende machtigingen verlenen, dezelfde manier als u machtigingen aan een ander type gebruiker verlenen. Normaal gesproken machtigingen te verlenen, en gebruikers toevoegen aan rollen. Zie voor meer informatie, [Database-Engine machtiging Basics](https://social.technet.microsoft.com/wiki/contents/articles/4433.database-engine-permission-basics.aspx). Zie voor meer informatie over de speciale SQL-Database-functies, [Databases en aanmeldingen in Azure SQL Database beheren](sql-database-manage-logins.md).
Een federatief domein-gebruikersaccount dat is geïmporteerd in een beheerd domein als een externe gebruiker moet de identiteit van de beheerde domein gebruiken.

> [!NOTE]
> Azure AD-gebruikers worden in de metagegevens van de database gemarkeerd met het type E (EXTERNAL_USER) en voor groepen met het type X (EXTERNAL_GROUPS). Zie [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx) voor meer informatie.
>

## <a name="connect-to-the-user-database-or-data-warehouse-by-using-ssms-or-ssdt"></a>Verbinding maken met de gebruiker database of het datawarehouse met behulp van SSMS of SSDT  

Als u wilt controleren of de Azure AD-beheerder correct is ingesteld, verbinding maken met de **master** database met behulp van de administrator-account van Azure AD.
Verbinding maken met de database met een Azure AD-identiteit die toegang tot de database heeft voor het inrichten van een Azure AD-gebruiker op basis van die database (met uitzondering van de beheerder van de server die eigenaar is van de database).

> [!IMPORTANT]
> Ondersteuning voor Azure Active Directory-verificatie is beschikbaar met [SQL Server 2016 Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) en [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx) in Visual Studio 2015. De release van augustus 2016 van SSMS biedt ook ondersteuning voor universele verificatie van Active Directory, zodat beheerders vereisen dat multi-factor Authentication met een telefonische oproep, SMS-bericht, smartcards en pincode of mobiele app-meldingen.

## <a name="using-an-azure-ad-identity-to-connect-using-ssms-or-ssdt"></a>Met behulp van een Azure AD-identiteit verbinding maken met behulp van SSMS of SSDT

De volgende procedures laten zien hoe u verbinding maken met een SQL-database met een Azure AD-identiteit met behulp van SQL Server Management Studio of SQL Server-Database-hulpprogramma's.

### <a name="active-directory-integrated-authentication"></a>Geïntegreerde verificatie van Active Directory

Gebruik deze methode als u bent aangemeld bij Windows met uw Azure Active Directory-referenties van een federatief domein.

1. Management Studio of gegevens-hulpprogramma's starten en klik in de **verbinding maken met Server** (of **verbinding maken met Database-Engine**) in het dialoogvenster de **verificatie** Schakel  **Active Directory - geïntegreerd**. Er is geen wachtwoord nodig is of omdat uw bestaande referenties voor de verbinding wordt weergegeven, kan worden ingevoerd.

    ![Selecteer AD geïntegreerde verificatie][11]
2. Selecteer de **opties** knop, en klik op de **verbindingseigenschappen** pagina, in de **verbinding maken met database** typt u de naam van de gebruikersdatabase die u verbinding maken wilt met. (De **AD-domein of tenant-ID**"optie wordt alleen ondersteund voor **Universal met MFA verbinding** opties, anders wordt deze grijs.)  

    ![Selecteer de databasenaam][13]

## <a name="active-directory-password-authentication"></a>Active Directory-wachtwoordverificatie

Gebruik deze methode wanneer u verbinding maakt met een Azure AD UPN-naam met behulp van de Azure AD beheerde domein. U kunt deze ook gebruiken voor federatieve accounts geen toegang tot het domein, bijvoorbeeld bij het op afstand werken.

Gebruik deze methode om te verifiëren naar SQL DB/DW met Azure AD voor systeemeigen of gefedereerd Azure AD-gebruikers. Een systeemeigen gebruiker is een expliciet in Azure AD hebt gemaakt en wordt geverifieerd met gebruikersnaam en wachtwoord, terwijl een federatieve gebruiker een Windows-gebruiker is met het domein is gefedereerd met Azure AD. De laatste methode (met behulp van de gebruiker en wachtwoord) kan worden gebruikt wanneer een gebruiker wil gebruiken hun windows-referentie, maar hun lokale computer niet is verbonden met het domein (bijvoorbeeld met behulp van een externe toegang). In dit geval een Windows-gebruiker kan duiden op hun domeinaccount en wachtwoord en naar SQL DB/DW met federatieve referenties kan worden geverifieerd.

1. Management Studio of gegevens-hulpprogramma's starten en klik in de **verbinding maken met Server** (of **verbinding maken met Database-Engine**) in het dialoogvenster de **verificatie** Schakel  **Active Directory - wachtwoord**.
2. In de **gebruikersnaam** typt u de naam van uw Azure Active Directory-gebruikersaccount in de indeling **gebruikersnaam\@domein.com**. Gebruikersnamen moet een van de Azure Active Directory-account of een account van een domein federeren met de Azure Active Directory.
3. In de **wachtwoord** vak, typ het wachtwoord van de gebruiker voor de Azure Active Directory-account of domeinaccount federatieve.

    ![Selecteer verificatie van AD-wachtwoord][12]
4. Selecteer de **opties** knop, en klik op de **verbindingseigenschappen** pagina, in de **verbinding maken met database** typt u de naam van de gebruikersdatabase die u verbinding maken wilt met. (Zie de afbeelding in de vorige optie.)

## <a name="using-an-azure-ad-identity-to-connect-from-a-client-application"></a>Met behulp van een Azure AD-identiteit verbinding maken vanaf een clienttoepassing

De volgende procedures laten zien hoe u verbinding maakt met een SQL-database met een Azure AD-identiteit van een clienttoepassing.

### <a name="active-directory-integrated-authentication"></a>Geïntegreerde verificatie van Active Directory

Als u wilt gebruikmaken van geïntegreerde Windows-verificatie, moet de Active Directory van uw domein worden gefedereerd met Azure Active Directory. De clienttoepassing (of een service) verbinding maken met de database moet worden uitgevoerd op een domein computer onder de domeinreferenties van een gebruiker.

Als u wilt verbinden met een database met behulp van geïntegreerde verificatie en een Azure AD-identiteit, moet het sleutelwoord verificatie in de verbindingsreeks van de database zijn ingesteld op Active Directory geïntegreerde. De volgende C#-codevoorbeeld maakt gebruik van ADO .NET.

```C#
string ConnectionString =
@"Data Source=n9lxnyuzhv.database.windows.net; Authentication=Active Directory Integrated; Initial Catalog=testdb;";
SqlConnection conn = new SqlConnection(ConnectionString);
conn.Open();
```

Het sleutelwoord van de verbindingsreeks ``Integrated Security=True`` wordt niet ondersteund voor het verbinden met Azure SQL Database. Bij het maken van een ODBC-verbinding, moet u spaties verwijderen en het instellen van verificatie met 'ActiveDirectoryIntegrated'.

### <a name="active-directory-password-authentication"></a>Active Directory-wachtwoordverificatie

Voor verbinding met een database met behulp van geïntegreerde verificatie en een Azure AD-identiteit, moet het sleutelwoord verificatie worden ingesteld op Active Directory-wachtwoord. De verbindingsreeks moet gebruiker-ID/gebruikers-id en wachtwoord/PWD trefwoorden en waarden bevatten. De volgende C#-codevoorbeeld maakt gebruik van ADO .NET.

```C#
string ConnectionString =
@"Data Source=n9lxnyuzhv.database.windows.net; Authentication=Active Directory Password; Initial Catalog=testdb;  UID=bob@contoso.onmicrosoft.com; PWD=MyPassWord!";
SqlConnection conn = new SqlConnection(ConnectionString);
conn.Open();
```

Meer informatie over Azure Active Directory-verificatiemethoden met behulp van de demo-codevoorbeelden die beschikbaar zijn op [GitHub Demo van Azure AD-verificatie](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/security/azure-active-directory-auth).

## <a name="azure-ad-token"></a>Azure AD-token

Deze verificatiemethode kan middelste laag services verbinding maken met Azure SQL Database of Azure SQL Data Warehouse met het verkrijgen van een token uit Azure Active Directory (AAD). Hiermee kunt geavanceerde scenario's, met inbegrip van verificatie op basis van certificaten. U moet vier eenvoudige stappen voor het gebruik van Azure AD-token verificatie uitvoeren:

1. Uw toepassing registreren bij Azure Active Directory en de client-id niet ophalen voor uw code.
2. Maak een databasegebruiker voor de toepassing. (Voltooid eerder in stap 6).
3. Een certificaat maken op de client-computer wordt uitgevoerd de toepassing.
4. Het certificaat als een sleutel voor uw toepassing toevoegen.

Voorbeeld-verbindingsreeks:

```c#
string ConnectionString =@"Data Source=n9lxnyuzhv.database.windows.net; Initial Catalog=testdb;"
SqlConnection conn = new SqlConnection(ConnectionString);
conn.AccessToken = "Your JWT token"
conn.Open();
```

Zie voor meer informatie, [SQL Server Security-Blog](https://blogs.msdn.microsoft.com/sqlsecurity/20../../token-based-authentication-support-for-azure-sql-db-using-azure-ad-auth/). Zie voor meer informatie over het toevoegen van een certificaat [aan de slag met verificatie op basis van certificaten in Azure Active Directory](../active-directory/authentication/active-directory-certificate-based-authentication-get-started.md).

### <a name="sqlcmd"></a>sqlcmd

De volgende instructies, verbinding maken met behulp van versie 13.1 van sqlcmd, die beschikbaar via is de [Downloadcentrum](https://go.microsoft.com/fwlink/?LinkID=825643).

```cmd
sqlcmd -S Target_DB_or_DW.testsrv.database.windows.net  -G  
sqlcmd -S Target_DB_or_DW.testsrv.database.windows.net -U bob@contoso.com -P MyAADPassword -G -l 30
```

## <a name="next-steps"></a>Volgende stappen

- Zie [SQL Database-toegang en -beheer](sql-database-control-access.md) voor een overzicht van toegang en beheer in SQL Database.
- Zie [Aanmeldingen, gebruikers en databaserollen](sql-database-manage-logins.md) voor een overzicht van aanmeldingen, gebruikers en databaserollen in SQL Database.
- Zie [Principals](https://msdn.microsoft.com/library/ms181127.aspx) voor meer informatie over database-principals.
- Zie [Databaserollen](https://msdn.microsoft.com/library/ms189121.aspx) voor meer informatie over databaserollen.
- Zie [SQL Database-firewallregels](sql-database-firewall-configure.md) voor meer informatie over de firewallregels in SQL Database.

<!--Image references-->

[1]: ./media/sql-database-aad-authentication/1aad-auth-diagram.png
[2]: ./media/sql-database-aad-authentication/2subscription-relationship.png
[3]: ./media/sql-database-aad-authentication/3admin-structure.png
[4]: ./media/sql-database-aad-authentication/4select-subscription.png
[5]: ./media/sql-database-aad-authentication/5ad-settings-portal.png
[6]: ./media/sql-database-aad-authentication/6edit-directory-select.png
[7]: ./media/sql-database-aad-authentication/7edit-directory-confirm.png
[8]: ./media/sql-database-aad-authentication/8choose-ad.png
[10]: ./media/sql-database-aad-authentication/10choose-admin.png
[11]: ./media/sql-database-aad-authentication/active-directory-integrated.png
[12]: ./media/sql-database-aad-authentication/12connect-using-pw-auth2.png
[13]: ./media/sql-database-aad-authentication/13connect-to-db2.png
