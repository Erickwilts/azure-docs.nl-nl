---
title: ActiveDirectoryInteractive maakt verbinding met SQL | Microsoft Docs
description: Voorbeeld van C#-Code, met uitleg voor de verbinding met Azure SQL Database met behulp van SqlAuthenticationMethod.ActiveDirectoryInteractive modus.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: active directory
ms.devlang: ''
ms.topic: conceptual
author: GithubMirek
ms.author: MirekS
ms.reviewer: GeneMi
ms.date: 03/12/2019
manager: craigg
ms.openlocfilehash: bc7274308b8a349d16866f107eac4a57e115be9e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66160866"
---
# <a name="connect-to-azure-sql-database-with-azure-multi-factor-authentication"></a>Verbinding maken met Azure SQL Database met Azure multi-factor Authentication

Dit artikel bevat een C# programma dat verbinding met Azure SQL Database maakt. Het programma wordt gebruikt voor verificatie in de interactieve modus, die ondersteuning biedt voor [Azure multi-factor Authentication](https://docs.microsoft.com/azure/active-directory/authentication/concept-mfa-howitworks).

Zie voor meer informatie over multi-factor Authentication-ondersteuning voor SQL-hulpprogramma's, [ondersteuning van Azure Active Directory in SQL Server Data Tools (SSDT)](https://docs.microsoft.com/sql/ssdt/azure-active-directory).

## <a name="multi-factor-authentication-for-azure-sql-database"></a>Multi-factor Authentication voor Azure SQL Database

.NET Framework versie 4.7.2, de enum-waarde vanaf [ `SqlAuthenticationMethod` ](https://docs.microsoft.com/dotnet/api/system.data.sqlclient.sqlauthenticationmethod) heeft een nieuwe waarde: `ActiveDirectoryInteractive`. In een client C# -programma, de enum-waarde zorgt ervoor dat het systeem moet worden gebruikt de interactieve modus van Azure Active Directory (Azure AD) die ondersteuning biedt voor multi-factor Authentication verbinding maken met een Azure SQL database. De gebruiker die het programma uitvoert, ziet het volgende dialoogvenster:

* Een dialoogvenster waarin wordt weergegeven de naam van een Azure AD-gebruiker en wachtwoord van de gebruiker wordt gevraagd.

   Als het domein van de gebruiker is gefedereerd met Azure AD, in dit dialoogvenster niet wordt weergegeven, omdat er geen wachtwoord nodig is.

   Als het beleid voor Azure AD legt een multi-factor Authentication de gebruiker, worden de volgende twee dialoogvensters worden weergegeven.

* De eerste keer dat een gebruiker multi-factor Authentication, doorloopt geeft een dialoogvenster waarin u wordt gevraagd voor een mobiel telefoonnummer voor het verzenden van SMS-berichten op het systeem. Elk bericht bevat de *verificatiecode* die de gebruiker moet invoeren in het volgende dialoogvenster.

* Een dialoogvenster waarin u wordt gevraagd een verificatiecode multi-Factor Authentication, die het systeem naar een mobiele telefoon is verzonden.

Zie voor meer informatie over het configureren van Azure AD om te vereisen dat multi-factor Authentication [aan de slag met Azure multi-factor Authentication in de cloud](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-cloud).

Zie voor schermafbeeldingen van deze dialoogvensters [meervoudige verificatie configureren voor SQL Server Management Studio en Azure AD](sql-database-ssms-mfa-authentication-configure.md).

> [!TIP]
> U kunt zoeken naar .NET Framework-API's met de [.NET API-Browser hulpprogramma pagina](https://docs.microsoft.com/dotnet/api/).
>
> U kunt ook zoeken naar rechtstreeks met de [optioneel? term =&lt;waarde zoeken&gt; parameter](https://docs.microsoft.com/dotnet/api/?term=SqlAuthenticationMethod).

## <a name="configure-your-c-application-in-the-azure-portal"></a>Configureer uw C# toepassing in Azure portal

Voordat u begint, moet u een [Azure SQL Database-server](sql-database-get-started-portal.md) gemaakt en is beschikbaar.

### <a name="register-your-app-and-set-permissions"></a>Uw app registreren en machtigingen instellen

Gebruik van Azure AD-verificatie, uw C# programma heeft als een Azure AD-toepassing wilt registreren. Voor het registreren van een app, moet u een Azure AD-beheerder of de Azure AD aan een gebruiker toegewezen *toepassingsontwikkelaar* rol. Zie voor meer informatie over het toewijzen van rollen [beheerder en niet-beheerder rollen toewijzen aan gebruikers met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-users-assign-role-azure-portal).

Voltooien van de registratie van een app genereert en toont een **toepassings-ID**. Uw programma heeft om op te nemen van deze ID om verbinding te maken.

Om te registreren en de benodigde machtigingen instellen voor uw toepassing:

1. Selecteer in de Azure portal, **Azure Active Directory** > **App-registraties** > **nieuwe toepassing registreren**.

    ![App-registratie](media/active-directory-interactive-connect-azure-sql-db/image1.png)

    Nadat de app-registratie is gemaakt, de **toepassings-ID** waarde wordt gegenereerd en weergegeven.

    ![App-ID weergeven](media/active-directory-interactive-connect-azure-sql-db/image2.png)

2. Selecteer **geregistreerde app** > **instellingen** > **vereiste machtigingen** > **toevoegen**.

    ![Machtigingsinstellingen voor geregistreerde app](media/active-directory-interactive-connect-azure-sql-db/sshot-registered-app-settings-required-permissions-add-api-access-c32.png)

3. Selecteer **vereiste machtigingen** > **toevoegen** > **Select an API** > **Azure SQL Database**.

    ![Toegang tot API voor Azure SQL Database toevoegen](media/active-directory-interactive-connect-azure-sql-db/sshot-registered-app-settings-required-permissions-add-api-access-Azure-sql-db-d11.png)

4. Selecteer **API-toegang** > **machtigingen selecteren** > **overgedragen machtigingen**.

    ![Het overdragen van machtigingen voor API voor Azure SQL Database](media/active-directory-interactive-connect-azure-sql-db/sshot-add-api-access-azure-sql-db-delegated-permissions-checkbox-e14.png)

### <a name="set-an-azure-ad-admin-for-your-sql-database-server"></a>Een Azure AD-beheerder voor uw SQL Database-server instellen

Voor uw C# programma om uit te voeren, een Azure SQL server-beheerder moet een Azure AD-beheerder voor uw SQL Database-server toewijzen. 

Op de **SQL Server** weergeeft, schakelt **Active Directory-beheerder** > **beheerder instellen**.

Zie voor meer informatie over Azure AD-beheerders en gebruikers voor Azure SQL Database, de schermafbeeldingen in [configureren en beheren van Azure Active Directory-verificatie met SQL Database](sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server).

### <a name="add-a-non-admin-user-to-a-specific-database-optional"></a>Een gebruiker niet-beheerders toevoegen aan een specifieke database (optioneel)

Een Azure AD-beheerder voor een SQL Database-server kan worden uitgevoerd de C# voorbeeldprogramma. Een Azure AD-gebruiker kan het programma uitvoeren als ze zich in de database. Een Azure AD-SQL-beheerder of een Azure AD-gebruiker die al bestaat in de database en heeft de `ALTER ANY USER` machtiging voor de database een gebruiker kunt toevoegen.

U kunt een gebruiker toevoegen aan de database met behulp van de SQL [ `Create User` ](https://docs.microsoft.com/sql/t-sql/statements/create-user-transact-sql) opdracht. Een voorbeeld is `CREATE USER [<username>] FROM EXTERNAL PROVIDER`.

Zie voor meer informatie, [gebruik Azure Active Directory-verificatie voor verificatie met SQL Database Managed Instance of SQL Data Warehouse](sql-database-aad-authentication.md).

## <a name="new-authentication-enum-value"></a>Nieuwe verificatie enum-waarde

De C# voorbeeld is afhankelijk van de [ `System.Data.SqlClient` ](https://docs.microsoft.com/dotnet/api/system.data.sqlclient) naamruimte. Is van belang zijn speciaal voor multi-factor Authentication de enum `SqlAuthenticationMethod`, heeft de volgende waarden:

- `SqlAuthenticationMethod.ActiveDirectoryInteractive`

   Gebruik deze waarde met de naam van een Azure AD-gebruiker voor het implementeren van multi-factor Authentication. Deze waarde is de focus van dit artikel. Het genereert een interactieve ervaring door dialoogvensters wachtwoord van de gebruiker en klik vervolgens voor de validatie van de multi-factor Authentication weergegeven als multi-factor Authentication is die zijn opgelegd voor deze gebruiker. Deze waarde is beschikbaar vanaf .NET Framework versie 4.7.2.

- `SqlAuthenticationMethod.ActiveDirectoryIntegrated`

  Gebruik deze waarde voor een *federatieve* account. Voor een federatieve-account is de naam van de gebruiker bekend aan het Windows-domein. Deze verificatiemethode biedt geen ondersteuning voor multi-factor Authentication.

- `SqlAuthenticationMethod.ActiveDirectoryPassword`

  Deze waarde gebruiken voor verificatie met een Azure AD-gebruikersnaam en wachtwoord zijn vereist. Azure SQL Database doet de verificatie. Deze methode biedt geen ondersteuning voor multi-factor Authentication.

## <a name="set-c-parameter-values-from-the-azure-portal"></a>Stel C# parameterwaarden vanuit Azure portal

Voor de C# programma moeten worden uitgevoerd, moet u de juiste waarden toewijzen aan statische velden. Hier wordt weergegeven, zijn de velden met voorbeelden van waarden. Ook wordt weergegeven, zijn de Azure-portal locaties waar u de benodigde waarden kunt krijgen.

| Statische veldnaam | Voorbeeldwaarde | Waar in Azure portal |
| :---------------- | :------------ | :-------------------- |
| Az_SQLDB_svrName | "my-sqldb-svr.database.windows.net" | **SQL-servers** > **filteren op naam** |
| AzureAD_UserID | "auser\@abc.onmicrosoft.com" | **Azure Active Directory** > **gebruiker** > **nieuwe gastgebruiker** |
| Initial_DatabaseName | "myDatabase" | **SQL-servers** > **SQL-databases** |
| ClientApplicationID | "a94f9c62-97fe-4d19-b06d-111111111111" | **Azure Active Directory** > **App-registraties** > **zoeken op naam** > **toepassings-ID** |
| RedirectUri | nieuwe Uri ("https://mywebserver.com/") | **Azure Active Directory** > **App-registraties** > **zoeken op naam** >  *[uw App-registratie]*  >  **Instellingen** > **RedirectURIs**<br /><br />In dit artikel is geldige waarde is prima voor RedirectUri, omdat het hier niet wordt gebruikt. |
| &nbsp; | &nbsp; | &nbsp; |

## <a name="verify-with-sql-server-management-studio"></a>Verifiëren met SQL Server Management Studio

Voordat u de C# -programma, is het een goed idee om te controleren of uw instellingen en configuraties juist in SQL Server Management Studio (SSMS) zijn. Alle C# programma storing kan vervolgens worden teruggebracht tot de broncode.

### <a name="verify-sql-database-firewall-ip-addresses"></a>Controleer of IP-adressen voor SQL Database-firewall

SSMS uitvoeren vanaf dezelfde computer, in hetzelfde gebouw, waar u van plan bent om uit te voeren de C# programma. Voor deze test alle **verificatie** modus is OK. Als er een indicatie dat de firewall van de database-server is niet worden gebruikt voor het accepteren van uw IP-adres, Zie [Azure SQL Database op serverniveau en databaseniveau firewallregels](sql-database-firewall-configure.md) voor hulp.

### <a name="verify-azure-active-directory-multi-factor-authentication"></a>Controleer of Active Directory van Azure multi-factor Authentication

SSMS opnieuw uitvoert, dit keer met **verificatie** ingesteld op **Active Directory - Universal met ondersteuning voor MFA**. Deze optie vereist SSMS 17,5 of hoger.

Zie voor meer informatie, [multi-factor Authentication configureren voor SSMS en Azure AD](sql-database-ssms-mfa-authentication-configure.md).

> [!NOTE]
> Als u een gastgebruiker in de database bent, moet u ook de naam van de Azure AD-domein voor de database te geven: Selecteer **opties** > **AD-domein of tenant-ID**. Als u zoekt de domeinnaam in Azure portal, selecteert u **Azure Active Directory** > **aangepaste-domeinnamen**. In de C# voorbeeldprogramma biedt een domeinnaam is niet nodig.

## <a name="c-code-example"></a>Voorbeeld van C#-code

Het voorbeeld C# programma is afhankelijk van de [ *Microsoft.IdentityModel.Clients.ActiveDirectory* ](https://docs.microsoft.com/dotnet/api/microsoft.identitymodel.clients.activedirectory) dll-assembly.

Als u wilt dit pakket installeren in Visual Studio, selecteer **Project** > **NuGet-pakketten beheren**. Zoek en installeer **Microsoft.IdentityModel.Clients.ActiveDirectory**.

Dit is een voorbeeld van C# broncode.

```csharp

using System;

// Reference to Azure AD authentication assembly
using Microsoft.IdentityModel.Clients.ActiveDirectory;

using DA = System.Data;
using SC = System.Data.SqlClient;
using AD = Microsoft.IdentityModel.Clients.ActiveDirectory;
using TX = System.Text;
using TT = System.Threading.Tasks;

namespace ADInteractive5
{
    class Program
    {
        // ASSIGN YOUR VALUES TO THESE STATIC FIELDS !!
        static public string Az_SQLDB_svrName = "<Your SQL DB server>";
        static public string AzureAD_UserID = "<Your User ID>";
        static public string Initial_DatabaseName = "<Your Database>";
        // Some scenarios do not need values for the following two fields:
        static public readonly string ClientApplicationID = "<Your App ID>";
        static public readonly Uri RedirectUri = new Uri("<Your URI>");

        public static void Main(string[] args)
        {
            var provider = new ActiveDirectoryAuthProvider();

            SC.SqlAuthenticationProvider.SetProvider(
                SC.SqlAuthenticationMethod.ActiveDirectoryInteractive,
                //SC.SqlAuthenticationMethod.ActiveDirectoryIntegrated,  // Alternatives.
                //SC.SqlAuthenticationMethod.ActiveDirectoryPassword,
                provider);

            Program.Connection();
        }

        public static void Connection()
        {
            SC.SqlConnectionStringBuilder builder = new SC.SqlConnectionStringBuilder();

            // Program._  static values that you set earlier.
            builder["Data Source"] = Program.Az_SQLDB_svrName;
            builder.UserID = Program.AzureAD_UserID;
            builder["Initial Catalog"] = Program.Initial_DatabaseName;

            // This "Password" is not used with .ActiveDirectoryInteractive.
            //builder["Password"] = "<YOUR PASSWORD HERE>";

            builder["Connect Timeout"] = 15;
            builder["TrustServerCertificate"] = true;
            builder.Pooling = false;

            // Assigned enum value must match the enum given to .SetProvider().
            builder.Authentication = SC.SqlAuthenticationMethod.ActiveDirectoryInteractive;
            SC.SqlConnection sqlConnection = new SC.SqlConnection(builder.ConnectionString);

            SC.SqlCommand cmd = new SC.SqlCommand(
                "SELECT '******** MY QUERY RAN SUCCESSFULLY!! ********';",
                sqlConnection);

            try
            {
                sqlConnection.Open();
                if (sqlConnection.State == DA.ConnectionState.Open)
                {
                    var rdr = cmd.ExecuteReader();
                    var msg = new TX.StringBuilder();
                    while (rdr.Read())
                    {
                        msg.AppendLine(rdr.GetString(0));
                    }
                    Console.WriteLine(msg.ToString());
                    Console.WriteLine(":Success");
                }
                else
                {
                    Console.WriteLine(":Failed");
                }
                sqlConnection.Close();
            }
            catch (Exception ex)
            {
                Console.ForegroundColor = ConsoleColor.Red;
                Console.WriteLine("Connection failed with the following exception...");
                Console.WriteLine(ex.ToString());
                Console.ResetColor();
            }
        }
    } // EOClass Program.

    /// <summary>
    /// SqlAuthenticationProvider - Is a public class that defines 3 different Azure AD
    /// authentication methods.  The methods are supported in the new .NET 4.7.2.
    ///  . 
    /// 1. Interactive,  2. Integrated,  3. Password
    ///  . 
    /// All 3 authentication methods are based on the Azure
    /// Active Directory Authentication Library (ADAL) managed library.
    /// </summary>
    public class ActiveDirectoryAuthProvider : SC.SqlAuthenticationProvider
    {
        // Program._ more static values that you set!
        private readonly string _clientId = Program.ClientApplicationID;
        private readonly Uri _redirectUri = Program.RedirectUri;

        public override async TT.Task<SC.SqlAuthenticationToken>
            AcquireTokenAsync(SC.SqlAuthenticationParameters parameters)
        {
            AD.AuthenticationContext authContext =
                new AD.AuthenticationContext(parameters.Authority);
            authContext.CorrelationId = parameters.ConnectionId;
            AD.AuthenticationResult result;

            switch (parameters.AuthenticationMethod)
            {
                case SC.SqlAuthenticationMethod.ActiveDirectoryInteractive:
                    Console.WriteLine("In method 'AcquireTokenAsync', case_0 == '.ActiveDirectoryInteractive'.");

                    result = await authContext.AcquireTokenAsync(
                        parameters.Resource,  // "https://database.windows.net/"
                        _clientId,
                        _redirectUri,
                        new AD.PlatformParameters(AD.PromptBehavior.Auto),
                        new AD.UserIdentifier(
                            parameters.UserId,
                            AD.UserIdentifierType.RequiredDisplayableId));
                    break;

                case SC.SqlAuthenticationMethod.ActiveDirectoryIntegrated:
                    Console.WriteLine("In method 'AcquireTokenAsync', case_1 == '.ActiveDirectoryIntegrated'.");

                    result = await authContext.AcquireTokenAsync(
                        parameters.Resource,
                        _clientId,
                        new AD.UserCredential());
                    break;

                case SC.SqlAuthenticationMethod.ActiveDirectoryPassword:
                    Console.WriteLine("In method 'AcquireTokenAsync', case_2 == '.ActiveDirectoryPassword'.");

                    result = await authContext.AcquireTokenAsync(
                        parameters.Resource,
                        _clientId,
                        new AD.UserPasswordCredential(
                            parameters.UserId,
                            parameters.Password));
                    break;

                default: throw new InvalidOperationException();
            }
            return new SC.SqlAuthenticationToken(result.AccessToken, result.ExpiresOn);
        }

        public override bool IsSupported(SC.SqlAuthenticationMethod authenticationMethod)
        {
            return authenticationMethod == SC.SqlAuthenticationMethod.ActiveDirectoryIntegrated
                || authenticationMethod == SC.SqlAuthenticationMethod.ActiveDirectoryInteractive
                || authenticationMethod == SC.SqlAuthenticationMethod.ActiveDirectoryPassword;
        }
    } // EOClass ActiveDirectoryAuthProvider.
} // EONamespace.  End of entire program source code.

```

&nbsp;

Dit is een voorbeeld van de C# uitvoer testen.

```
[C:\Test\VSProj\ADInteractive5\ADInteractive5\bin\Debug\]
>> ADInteractive5.exe
In method 'AcquireTokenAsync', case_0 == '.ActiveDirectoryInteractive'.
******** MY QUERY RAN SUCCESSFULLY!! ********

:Success

[C:\Test\VSProj\ADInteractive5\ADInteractive5\bin\Debug\]
>>
```

## <a name="next-steps"></a>Volgende stappen

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> De PowerShell Azure Resource Manager-module nog steeds wordt ondersteund door Azure SQL Database, maar alle toekomstige ontwikkeling is voor de module Az.Sql. Zie voor deze cmdlets [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). De argumenten voor de opdrachten in de Az-module en de AzureRm-modules zijn vrijwel identiek zijn.

- [Get-AzSqlServerActiveDirectoryAdministrator](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlserveractivedirectoryadministrator)
