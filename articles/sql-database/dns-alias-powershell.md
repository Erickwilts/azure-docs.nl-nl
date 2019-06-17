---
title: PowerShell voor DNS-alias Azure SQL | Microsoft Docs
description: PowerShell-cmdlets, zoals nieuwe AzSqlServerDNSAlias kunt u nieuwe clientverbindingen omleiden naar een andere Azure SQL Database-server, zonder te hoeven te zien van de configuratie van een client.
keywords: DNS-sql-database
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.devlang: PowerShell
ms.topic: conceptual
author: oslake
ms.author: moslake
ms.reviewer: genemi,amagarwa,maboja, jrasnick
manager: craigg
ms.date: 05/14/2019
ms.openlocfilehash: 4318e6557dc72dff7200beb8783575131659b77f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65797707"
---
# <a name="powershell-for-dns-alias-to-azure-sql-database"></a>PowerShell voor DNS-Alias naar Azure SQL Database

Dit artikel bevat een PowerShell-script dat laat zien hoe u een DNS-alias voor Azure SQL Database kunt beheren. Het script wordt uitgevoerd de volgende cmdlets die de volgende acties uitgevoerd:

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

De cmdlets die in het codevoorbeeld gebruikt zijn de volgende:

- [New-AzSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/az.Sql/New-azSqlServerDnsAlias): Hiermee maakt u een nieuwe DNS-alias in het systeem van de service Azure SQL Database. De alias verwijst naar Azure SQL Database-server 1.
- [Get-AzSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/az.Sql/Get-azSqlServerDnsAlias): Ophalen en weergeven van alle DNS-aliassen die zijn toegewezen aan de SQL-database-server 1.
- [Set-AzSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/az.Sql/Set-azSqlServerDnsAlias): Hiermee wijzigt u de naam van de server die de alias is geconfigureerd om te verwijzen naar, van server 1 naar 2 van de SQL-database-server.
- [Remove-AzSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/az.Sql/Remove-azSqlServerDnsAlias): Verwijder de DNS-alias van de SQL-database-server 2, met behulp van de naam van de alias.

## <a name="dns-alias-in-connection-string"></a>DNS-alias in de verbindingsreeks

Als u wilt verbinding maken met een specifieke Azure SQL Database-server, krijgt u een client, zoals SQL Server Management Studio (SSMS) de naam van de DNS-alias in plaats van de naam van de waarde true. In het volgende voorbeeld van de server tekenreeks, de alias *any-unieke-alias-name* vervangt het eerste knooppunt van de punt gescheiden in de tekenreeks van de server vier knooppunten:

- Voorbeeld van een tekenreeks-server: `any-unique-alias-name.database.windows.net`.

## <a name="prerequisites"></a>Vereisten

Als u uitvoeren van de demo PowerShell-script in dit artikel worden vermeld wilt, gelden de volgende vereisten:

- Een Azure-abonnement en het account. Voor een gratis proefversie aan, klikt u op [ https://azure.microsoft.com/free/ ] [ https://azure.microsoft.com/free/].
- Azure PowerShell-module, met de cmdlet **New-AzSqlServerDNSAlias**.
  - Als u wilt installeren of upgraden, Zie [Azure PowerShell-module installeren][install-Az-ps-84p].
  - Voer `Get-Module -ListAvailable Az;` in powershell\_ise.exe, de versie te vinden.
- Twee Azure SQL Database-servers.

## <a name="code-example"></a>Voorbeeld van code

De volgende PowerShell codevoorbeeld begint met het letterlijke waarden aan verschillende variabelen toewijzen. Als u wilt uitvoeren van de code, moet u de tijdelijke aanduiding voor waarden zodat deze overeenkomt met de werkelijke waarden in uw systeem te bewerken. Of u kunt de code alleen bestuderen. En de uitvoer van de console van de code is ook beschikbaar.

```powershell
################################################################
###    Assign prerequisites.                                 ###
################################################################
cls;

$SubscriptionName             = '<EDIT-your-subscription-name>';
[string]$SubscriptionGuid_Get = '?'; # The script assigns this value, not you.

$SqlServerDnsAliasName = '<EDIT-any-unique-alias-name>';

$1ResourceGroupName = '<EDIT-rg-1>';  # Can be same or different than $2ResourceGroupName.
$1SqlServerName     = '<EDIT-sql-1>'; # Must differ from $2SqlServerName.

$2ResourceGroupName = '<EDIT-rg-2>';
$2SqlServerName     = '<EDIT-sql-2>';

# Login to your Azure subscription, first time per session.
Write-Host "You must log into Azure once per powershell_ise.exe session,";
Write-Host "  thus type 'yes' only the first time.";
Write-Host " ";
$yesno = Read-Host '[yes/no]  Do you need to log into Azure now?';
if ('yes' -eq $yesno)
{
    Connect-AzAccount -SubscriptionName $SubscriptionName;
}

$SubscriptionGuid_Get = Get-AzSubscription `
    -SubscriptionName $SubscriptionName;

################################################################
###    Working with DNS aliasing for Azure SQL DB server.    ###
################################################################

Write-Host '[1] Assign a DNS alias to SQL DB server 1.';
New-AzSqlServerDNSAlias `
    –ResourceGroupName  $1ResourceGroupName `
    -ServerName         $1SqlServerName `
    -ServerDNSAliasName $SqlServerDnsAliasName;

Write-Host '[2] Get and list all the DNS aliases that are assigned to SQL DB server 1.';
Get-AzSqlServerDNSAlias `
    –ResourceGroupName $1ResourceGroupName `
    -ServerName        $1SqlServerName;

Write-Host '[3] Move the DNS alias from 1 to SQL DB server 2.';
Set-AzSqlServerDNSAlias `
    –ResourceGroupName  $2ResourceGroupName `
    -NewServerName      $2SqlServerName `
    -ServerDNSAliasName $SqlServerDnsAliasName `
    -OldServerResourceGroup  $1ResourceGroupName `
    -OldServerName           $1SqlServerName `
    -OldServerSubscriptionId $SubscriptionGuid_Get;

# Here your client, such as SSMS, can connect to your "$2SqlServerName"
# by using "$SqlServerDnsAliasName" in the server name.
# For example, server:  "any-unique-alias-name.database.windows.net".

# Remove-AzSqlServerDNSAlias  - would fail here for SQL DB server 1.

Write-Host '[4] Remove the DNS alias from SQL DB server 2.';
Remove-AzSqlServerDNSAlias `
    –ResourceGroupName  $2ResourceGroupName `
    -ServerName         $2SqlServerName `
    -ServerDNSAliasName $SqlServerDnsAliasName;
```

### <a name="actual-console-output-from-the-powershell-example"></a>Werkelijke console-uitvoer van het PowerShell-voorbeeld

De volgende console-uitvoer is gekopieerd en geplakt van een werkelijke uitvoeren.

```powershell
You must log into Azure once per powershell_ise.exe session,
  thus type 'yes' only the first time.

[yes/no]  Do you need to log into Azure now?: yes


Environment           : AzureCloud
Account               : gm@acorporation.com
TenantId              : 72f988bf-1111-1111-1111-111111111111
SubscriptionId        : 45651c69-2222-2222-2222-222222222222
SubscriptionName      : mysubscriptionname
CurrentStorageAccount :

[1] Assign a DNS alias to SQL DB server 1.
[2] Get the DNS alias that is assigned to SQL DB server 1.
[3] Move the DNS alias from 1 to SQL DB server 2.
[4] Remove the DNS alias from SQL DB server 2.
ResourceGroupName ServerName         ServerDNSAliasName
----------------- ----------         ------------------
gm-rg-dns-1       gm-sqldb-dns-1     unique-alias-name-food
gm-rg-dns-1       gm-sqldb-dns-1     unique-alias-name-food
gm-rg-dns-2       gm-sqldb-dns-2     unique-alias-name-food


[C:\windows\system32\]
>>
```

## <a name="next-steps"></a>Volgende stappen

Zie voor een volledige uitleg van de functie DNS-Alias voor SQL-Database, [DNS-alias voor Azure SQL Database][dns-alias-overview-37v].

<!-- Article links. -->

[https://azure.microsoft.com/free/]: https://azure.microsoft.com/free/

[install-Az-ps-84p]: https://docs.microsoft.com/powershell/azure/install-az-ps

[dns-alias-overview-37v]: dns-alias-overview.md
