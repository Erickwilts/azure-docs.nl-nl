---
title: Azure AD-gebruikers maken met behulp van service-principals
description: In deze zelfstudie wordt stapsgewijs uitgelegd hoe u een Azure AD-gebruiker maakt met Azure AD-toepassingen (service-principals) in Azure SQL Database en Azure Synapse Analytics
ms.service: sql-database
ms.subservice: security
ms.custom: azure-synapse
ms.topic: tutorial
author: GithubMirek
ms.author: mireks
ms.reviewer: vanto
ms.date: 08/17/2020
ms.openlocfilehash: 453821e99f53a90a076ff13f010f2031a055cbf6
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "91444173"
---
# <a name="tutorial-create-azure-ad-users-using-azure-ad-applications"></a>Zelfstudie: Azure AD-gebruikers maken met behulp van Azure AD-toepassingen

[!INCLUDE[appliesto-sqldb-asa](../includes/appliesto-sqldb-asa.md)]

> [!NOTE]
> Dit artikel is beschikbaar in de **openbare preview-versie**. Raadpleeg [Azure Active Directory-service-principal met Azure SQL](authentication-aad-service-principal.md) voor meer informatie. In dit artikel wordt gebruikgemaakt van Azure SQL Database om de benodigde stappen van de zelfstudie te demonstreren, maar deze stappen kunnen ook worden uitgevoerd in [Azure Synapse Analytics](../../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md).

Dit artikel begeleidt u door het proces van het maken van Azure AD-gebruikers in Azure SQL Database, met behulp van Azure-service-principals (Azure AD-toepassingen). Deze functionaliteit bestaat al in een Azure SQL Managed Instance, maar wordt nu geïntroduceerd in Azure SQL Database en Azure Synapse Analytics. Ter ondersteuning van dit scenario moet een Azure AD-identiteit worden gegenereerd en toegewezen aan de logische Azure SQL-server.

Raadpleeg het artikel [Azure Active Directory-verificatie gebruiken](authentication-aad-overview.md) voor meer informatie over Azure AD-verificatie voor Azure SQL.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> - Een identiteit toewijzen aan de logische Azure SQL-server
> - De machtiging Adreslijstlezers toewijzen aan de identiteit van de logische SQL-server
> - Een service-principal (een Azure AD-toepassing) maken in Azure AD
> - Een service-principal-gebruiker maken in Azure SQL Database
> - Een andere Azure AD-gebruiker maken in SQL Database met behulp van een Azure AD-service-principal-gebruiker

## <a name="prerequisites"></a>Vereisten

- Een bestaande implementatie van [Azure SQL Database](single-database-create-quickstart.md) of [Azure Synapse Analytics](../../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md). We gaan ervan uit dat u een werkende SQL-database hebt voor deze zelfstudie.
- Toegang tot een al bestaande Azure Active Directory.
- De module [Az.Sql 2.9.0](https://www.powershellgallery.com/packages/Az.Sql/2.9.0) of hoger is vereist, wanneer u PowerShell gebruikt om een afzonderlijke Azure AD-toepassing in te stellen als Azure AD-beheerder voor Azure SQL. Zorg ervoor dat u een upgrade uitvoert naar de nieuwste module.

## <a name="assign-an-identity-to-the-azure-sql-logical-server"></a>Wijs een identiteit toe aan de logische Azure SQL-server.

1. Maak verbinding met de Azure Active Directory. U hebt uw Tenant-id nodig. Deze vindt u door in de [Azure-portal](https://portal.azure.com) naar uw **Azure Active Directory**-resource te gaan. In het deelvenster **Overzicht** ziet u de **Tenant-id**. Voer de volgende PowerShell-opdracht uit:

    - Vervang `<TenantId>` door de **Tenant-id**.

    ```powershell
    Connect-AzAccount -Tenant <TenantId>
    ```

    Registreer de `TenantId` voor toekomstig gebruik in deze zelfstudie.

1. Genereer een Azure AD-identiteit en wijs deze toe aan de logische Azure SQL-server. Voer de volgende PowerShell-opdracht uit:

    - Vervang `<resource group>` en `<server name>` door uw resources. Als de servernaam `myserver.database.windows.net` is, vervangt u `<server name>` door `myserver`.

    ```powershell
    Set-AzSqlServer -ResourceGroupName <resource group> -ServerName <server name> -AssignIdentity
    ```

    Zie de opdracht [Set-AzSqlServer](https://docs.microsoft.com/powershell/module/az.sql/set-azsqlserver) voor meer informatie.

    > [!IMPORTANT]
    > Als een Azure AD-identiteit is ingesteld voor de logische Azure SQL-server, moet de machtiging [**Adreslijstlezers**](../../active-directory/users-groups-roles/directory-assign-admin-roles.md#directory-readers) zijn verleend aan de identiteit. We doorlopen deze stap in de volgende sectie. **Sla deze stap niet over** aangezien Azure AD-verificatie dan niet meer werkt.

    - Als u in het verleden de opdracht [New-AzSqlServer](https://docs.microsoft.com/powershell/module/az.sql/new-azsqlserver) met de parameter `AssignIdentity` hebt gebruikt om een nieuwe SQL-server te maken, moet u de opdracht [Set-AzSqlServer](https://docs.microsoft.com/powershell/module/az.sql/set-azsqlserver) later uitvoeren als een afzonderlijke opdracht om deze eigenschap in te schakelen in Azure Fabric.

1. Controleer of het toewijzen van de server-id is geslaagd. Voer de volgende PowerShell-opdracht uit:

    - Vervang `<resource group>` en `<server name>` door uw resources. Als de servernaam `myserver.database.windows.net` is, vervangt u `<server name>` door `myserver`.
    
    ```powershell
    $xyz = Get-AzSqlServer  -ResourceGroupName <resource group> -ServerName <server name>
    $xyz.identity
    ```

    In de uitvoer worden nu `PrincipalId`, `Type` en `TenantId` weergegeven. De toegewezen identiteit is de `PrincipalId`.

1. U kunt de identiteit ook controleren door naar de [Azure-portal](https://portal.azure.com) te gaan.

    - Ga onder de **Azure Active Directory**-resource naar **Bedrijfstoepassingen**. Typ de naam van de logische SQL-server. U ziet nu dat er een **object-id** is gekoppeld aan de resource.
    
    :::image type="content" source="media/authentication-aad-service-principals-tutorial/enterprise-applications-object-id.png" alt-text="object-id":::

## <a name="assign-directory-readers-permission-to-the-sql-logical-server-identity"></a>De machtiging Adreslijstlezers toewijzen aan de identiteit van de logische SQL-server

De machtiging Azure AD `Directory Readers` moet zijn verleend aan de identiteit van de server om ervoor te zorgen dat de toegewezen Azure AD-identiteit juist werkt voor Azure SQL.

Voer het volgende script uit om deze vereiste machtiging te verlenen.

> [!NOTE] 
> Dit script moet worden uitgevoerd met een Azure AD `Global Administrator` of een `Privileged Roles Administrator`.
>
> In **openbare preview** kunt u de rol `Directory Readers` toewijzen aan een groep in Azure AD. De groepseigenaren kunnen de beheerde identiteit vervolgens toevoegen als lid van deze groep, waardoor het niet nodig is dat een `Global Administrator` of `Privileged Roles Administrator` de rol `Directory Readers` moet toewijzen. Zie [Rol Directory Readers in Azure Active Directory voor Azure SQL](authentication-aad-directory-readers-role.md) voor meer informatie over deze functie.

- Vervang `<TenantId>` door de `TenantId` die eerder is verkregen.
- Vervang `<server name>` door de naam van de logische SQL-server. Als de servernaam `myserver.database.windows.net` is, vervangt u `<server name>` door `myserver`.

```powershell
# This script grants Azure "Directory Readers" permission to a Service Principal representing the Azure SQL logical server
# It can be executed only by a "Global Administrator" or "Privileged Roles Administrator" type of user.
# To check if the "Directory Readers" permission was granted, execute this script again

Import-Module AzureAD
Connect-AzureAD -TenantId "<TenantId>"    #Enter your actual TenantId
$AssignIdentityName = "<server name>"     #Enter Azure SQL logical server name
 
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
$roleMember = Get-AzureADServicePrincipal -SearchString $AssignIdentityName
$roleMember.Count
if ($roleMember -eq $null) {
    Write-Output "Error: No Service Principals with name '$($AssignIdentityName)', make sure that AssignIdentityName parameter was entered correctly."
    exit
}

if (-not ($roleMember.Count -eq 1)) {
    Write-Output "Error: More than one service principal with name pattern '$($AssignIdentityName)'"
    Write-Output "Dumping selected service principals...."
    $roleMember
    exit
}
 
# Check if service principal is already member of readers role
$allDirReaders = Get-AzureADDirectoryRoleMember -ObjectId $role.ObjectId
$selDirReader = $allDirReaders | where{$_.ObjectId -match $roleMember.ObjectId}
 
if ($selDirReader -eq $null) {
    # Add principal to readers role
    Write-Output "Adding service principal '$($AssignIdentityName)' to 'Directory Readers' role'..."
    Add-AzureADDirectoryRoleMember -ObjectId $role.ObjectId -RefObjectId $roleMember.ObjectId
    Write-Output "'$($AssignIdentityName)' service principal added to 'Directory Readers' role'..."
 
    #Write-Output "Dumping service principal '$($AssignIdentityName)':"
    #$allDirReaders = Get-AzureADDirectoryRoleMember -ObjectId $role.ObjectId
    #$allDirReaders | where{$_.ObjectId -match $roleMember.ObjectId}
} else {
    Write-Output "Service principal '$($AssignIdentityName)' is already member of 'Directory Readers' role'."
}
```

> [!NOTE]
> De uitvoer van dit script hierboven geeft aan of de machtiging Adreslijstlezers is verleend aan de identiteit. U kunt het script opnieuw uitvoeren als u niet zeker weet of de machtiging is verleend.

Raadpleeg [Azure AD-beheerder inrichten (SQL Managed Instance)](authentication-aad-configure.md#powershell) voor een soortgelijke benadering van het instellen van de machtiging **Adreslijstlezers** voor SQL Managed Instance.

## <a name="create-a-service-principal-an-azure-ad-application-in-azure-ad"></a>Een service-principal (een Azure AD-toepassing) maken in Azure AD

1. Volg deze handleiding om [uw app te registreren en machtigingen in te stellen](active-directory-interactive-connect-azure-sql-db.md#register-your-app-and-set-permissions).

    Zorg ervoor dat u zowel de **Toepassingsmachtigingen** als de **Gedelegeerde machtigingen** toevoegt.

    :::image type="content" source="media/authentication-aad-service-principals-tutorial/aad-apps.png" alt-text="object-id":::

    :::image type="content" source="media/authentication-aad-service-principals-tutorial/aad-app-registration-api-permissions.png" alt-text="object-id":::

2. U moet ook een clientgeheim maken om u aan te melden. Volg deze handleiding om [een certificaat te uploaden of een geheim te maken voor aanmelden](../../active-directory/develop/howto-create-service-principal-portal.md#authentication-two-options).

3. Leg de volgende informatie uit de toepassingsregistratie vast. Deze vindt u in het deelvenster **Overzicht**:
    - **Toepassings-id**
    - **Tenant-id**: dit is dezelfde id als eerder

In deze zelfstudie wordt gebruikgemaakt van *AppSP* als de hoofd-service-principal en *myapp* als de tweede service-principal-gebruiker die met *AppSP* wordt gemaakt in Azure SQL. U moet twee toepassingen maken, *AppSP* en *myapp*.

Voor meer informatie over het maken van een Azure AD-toepassing, raadpleegt u het artikel [Uitleg: Gebruik de portal voor het maken van een Azure AD-toepassing en service-principal die toegang hebben tot resources](../../active-directory/develop/howto-create-service-principal-portal.md).


## <a name="create-the-service-principal-user-in-azure-sql-database"></a>De service-principal-gebruiker maken in Azure SQL Database

Zodra een service-principal is gemaakt in Azure AD, maakt u de gebruiker in SQL Database. U moet verbinding maken met uw SQL-database via een geldige aanmelding met machtigingen voor het maken van gebruikers in de database.

1. Maak de gebruiker *AppSP* in de SQL-database met behulp van de volgende T-SQL-opdracht:

    ```sql
    CREATE USER [AppSP] FROM EXTERNAL PROVIDER
    GO
    ```

2. Verleen de machtiging `db_owner` aan *AppSP*. Dit stelt de gebruiker in staat om andere AD-gebruikers te maken in de database.

    ```sql
    EXEC sp_addrolemember 'db_owner', [AppSP]
    GO
    ```

    Zie [sp_addrolemember](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql) voor meer informatie

    U kunt ook de machtiging `ALTER ANY USER` verlenen in plaats van de rol `db_owner` toe te wijzen. Dit stelt de service-principal in staat om andere Azure AD-gebruikers toe te voegen.

    ```sql
    GRANT ALTER ANY USER TO [AppSp]
    GO
    ```

    > [!NOTE]
    > De bovenstaande instelling is niet vereist wanneer *AppSP* is ingesteld als een Azure AD-beheerder voor de server. Als u de service-principal wilt instellen als een AD-beheerder voor de logische SQL-server, kunt u dit doen via de Azure-portal, of via PowerShell- of Azure CLI-opdrachten. Raadpleeg [Azure AD-beheerder (SQL Database) inrichten](authentication-aad-configure.md?tabs=azure-powershell#powershell-for-sql-database-and-azure-synapse) voor meer informatie.

## <a name="create-an-azure-ad-user-in-sql-database-using-an-azure-ad-service-principal"></a>Een Azure AD-gebruiker maken in SQL Database met behulp van een Azure AD-service-principal

> [!IMPORTANT]
> De service-principal die wordt gebruikt voor aanmelden bij SQL Database, moet een clientgeheim hebben. Als dit niet het geval is, volgt u stap 2 van [Een service-principal (een Azure AD-toepassing) maken in Azure AD](#create-a-service-principal-an-azure-ad-application-in-azure-ad). Dit clientgeheim moet worden toegevoegd als een invoerparameter in het onderstaande script.

1. Gebruik het volgende script om een Azure AD-service-principal-gebruiker *myapp* te maken met behulp van de service-principal *AppSP*.

    - Vervang `<TenantId>` door de `TenantId` die eerder is verkregen.
    - Vervang `<ClientId>` door de `ClientId` die eerder is verkregen.
    - Vervang `<ClientSecret>` door het clientgeheim dat u eerder hebt gemaakt.
    - Vervang `<server name>` door de naam van de logische SQL-server. Als de servernaam `myserver.database.windows.net` is, vervangt u `<server name>` door `myserver`.
    - Vervang `<database name>` door de naam van de SQL-database.

    ```powershell
    # PowerShell script for creating a new SQL user called myapp using application AppSP with secret

    $tenantId = "<TenantId>"   #  tenantID (Azure Directory ID) were AppSP resides
    $clientId = "<ClientId>"   #  AppID also ClientID for AppSP     
    $clientSecret = "<ClientSecret>"   #  client secret for AppSP 
    $Resource = "https://database.windows.net/"
    
    $adalPath  = "${env:ProgramFiles}\WindowsPowerShell\Modules\AzureRM.profile\5.8.3"
    # To install the latest AzureRM.profile version execute  -Install-Module -Name AzureRM.profile
    $adal      = "$adalPath\Microsoft.IdentityModel.Clients.ActiveDirectory.dll"
    $adalforms = "$adalPath\Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll"
    [System.Reflection.Assembly]::LoadFrom($adal) | Out-Null
      $resourceAppIdURI = 'https://database.windows.net/'

      # Set Authority to Azure AD Tenant
      $authority = 'https://login.windows.net/' + $tenantId

      $ClientCred = [Microsoft.IdentityModel.Clients.ActiveDirectory.ClientCredential]::new($clientId, $clientSecret)
      $authContext = [Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext]::new($authority)
      $authResult = $authContext.AcquireTokenAsync($resourceAppIdURI,$ClientCred)
      $Tok = $authResult.Result.CreateAuthorizationHeader()
      $Tok=$Tok.Replace("Bearer ","")
      Write-host "token"
      $Tok
      Write-host  " "

    $SQLServerName = "<server name>"    # Azure SQL logical server name 
    Write-Host "Create SQL connectionstring"
    $conn = New-Object System.Data.SqlClient.SQLConnection 
    $DatabaseName = "<database name>"     # Azure SQL database name
    $conn.ConnectionString = "Data Source=$SQLServerName.database.windows.net;Initial Catalog=$DatabaseName;Connect Timeout=30"
    $conn.AccessToken = $Tok
    
    Write-host "Connect to database and execute SQL script"
    $conn.Open() 
    $ddlstmt = 'CREATE USER [myapp] FROM EXTERNAL PROVIDER;'
    Write-host " "
    Write-host "SQL DDL command"
    $ddlstmt
    $command = New-Object -TypeName System.Data.SqlClient.SqlCommand($ddlstmt, $conn)       
    
    Write-host "results"
    $command.ExecuteNonQuery()
    $conn.Close()  
    ``` 

    U kunt ook het codevoorbeeld gebruiken uit de blog: [Verificatie via Azure AD-service-principal bij SQL DB - codevoorbeeld](https://techcommunity.microsoft.com/t5/azure-sql-database/azure-ad-service-principal-authentication-to-sql-db-code-sample/ba-p/481467). Wijzig het script om een DDL-instructie `CREATE USER [myapp] FROM EXTERNAL PROVIDER` uit te voeren. Hetzelfde script kan worden gebruikt om een gewone Azure AD-gebruikersgroep te maken in SQL Database.

    > [!NOTE]
    > Als u de module AzureRM.profile wilt installeren, moet u PowerShell openen als beheerder. U kunt de volgende opdrachten gebruiken om automatisch de meest recente AzureRM.profile-versie te installeren, en `$adalpath` instellen voor het bovenstaande script:
    > 
    > ```powershell
    > Install-Module AzureRM.profile -force
    > Import-Module AzureRM.profile
    > $version = (Get-Module -Name AzureRM.profile).Version.toString()
    > $adalPath = "${env:ProgramFiles}\WindowsPowerShell\Modules\AzureRM.profile\${version}"
    > ```
    
2. Controleer of de gebruiker *myapp* bestaat in de database door de volgende opdracht uit te voeren:

    ```sql
    SELECT name, type, type_desc, CAST(CAST(sid as varbinary(16)) as uniqueidentifier) as appId from sys.database_principals WHERE name = 'myapp'
    GO
    ```

    De uitvoer moet er ongeveer als volgt uitzien:

    ```output
    name    type    type_desc   appId
    myapp   E   EXTERNAL_USER   6d228f48-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ```

## <a name="next-steps"></a>Volgende stappen

- [Azure Active Directory-service-principal met Azure SQL](authentication-aad-service-principal.md)
- [Wat zijn beheerde identiteiten voor Azure-resources?](../../active-directory/managed-identities-azure-resources/overview.md)
- [Beheerde identiteiten gebruiken voor App Service en Azure Functions](../../app-service/overview-managed-identity.md)
- [Verificatie via Azure AD-service-principal bij SQL DB - codevoorbeeld](https://techcommunity.microsoft.com/t5/azure-sql-database/azure-ad-service-principal-authentication-to-sql-db-code-sample/ba-p/481467)
- [Toepassings- en service-principal-objecten in Azure Active Directory](../../active-directory/develop/app-objects-and-service-principals.md)
- [Een Azure-service-principal maken met Azure PowerShell](https://docs.microsoft.com/powershell/azure/create-azure-service-principal-azureps)
- [Rol Directory Readers in Azure Active Directory voor Azure SQL](authentication-aad-directory-readers-role.md)
