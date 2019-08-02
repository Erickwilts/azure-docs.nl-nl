---
title: Een openbaar eind punt-Azure SQL Database beheerd exemplaar configureren | Microsoft Docs
description: Meer informatie over het configureren van een openbaar eind punt voor een beheerd exemplaar
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: vanto, carlrab
ms.date: 05/07/2019
ms.openlocfilehash: cebe6b4ca61b835e7c77f51592c20799fe271853
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68567405"
---
# <a name="configure-public-endpoint-in-azure-sql-database-managed-instance"></a>Open bare eind punt in Azure SQL Database beheerde instantie configureren

Het open bare eind punt voor een [beheerd exemplaar](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-index) maakt gegevens toegang tot uw beheerde exemplaar mogelijk buiten het [virtuele netwerk](../virtual-network/virtual-networks-overview.md). U hebt toegang tot uw beheerde exemplaar vanuit multi tenant Azure-Services, zoals Power BI, Azure App Service of een on-premises netwerk. Als u het open bare eind punt op een beheerd exemplaar gebruikt, hoeft u geen VPN te gebruiken. Dit kan helpen om problemen met de door Voer van VPN te voor komen.

In dit artikel leert u het volgende:

> [!div class="checklist"]
> - Open bare eind punt inschakelen voor uw beheerde instantie in de Azure Portal
> - Open bare eind punt voor uw beheerde instantie inschakelen met Power shell
> - De beveiligings groep voor een beheerd exemplaar netwerk configureren om verkeer naar het open bare eind punt van het beheerde exemplaar toe te staan
> - Het open bare endpoint van het beheerde exemplaar verkrijgen connection string

## <a name="permissions"></a>Machtigingen

Als gevolg van de gevoeligheid van gegevens die zich in een beheerd exemplaar bevindt, vereist de configuratie voor het inschakelen van het open bare eind punt van het beheerde exemplaar een proces dat uit twee stappen bestaat. Deze veiligheids maatregel voldoet aan de schei ding van taken (SoD):

- Het inschakelen van een openbaar eind punt op een beheerd exemplaar moet worden uitgevoerd door de beheerder van het beheerde exemplaar. De beheerder van het beheerde exemplaar kan worden gevonden op de overzichts pagina van uw SQL Managed instance-resource.
- Verkeer toestaan met behulp van een netwerk beveiligings groep die moet worden uitgevoerd door een netwerk beheerder. Zie [machtigingen voor netwerk beveiligings groepen](../virtual-network/manage-network-security-group.md#permissions)voor meer informatie.

## <a name="enabling-public-endpoint-for-a-managed-instance-in-the-azure-portal"></a>Openbaar eind punt inschakelen voor een beheerd exemplaar in het Azure Portal

1. Start de Azure Portal op<https://portal.azure.com/.>
1. Open de resource groep met het beheerde exemplaar en selecteer het **SQL Managed instance** waarvoor u het open bare eind punt wilt configureren.
1. Selecteer op de pagina **beveiligings** instellingen het tabblad **virtueel netwerk** .
1. Selecteer op de pagina configuratie van virtueel netwerk de optie **inschakelen** en klik vervolgens op het pictogram **Opslaan** om de configuratie bij te werken.

![Mi-vnet-config. png](media/sql-database-managed-instance-public-endpoint-configure/mi-vnet-config.png)

## <a name="enabling-public-endpoint-for-a-managed-instance-using-powershell"></a>Openbaar eind punt inschakelen voor een beheerd exemplaar met behulp van Power shell

### <a name="enable-public-endpoint"></a>Openbaar eind punt inschakelen

Voer de volgende Power shell-opdrachten uit. Vervang **abonnement-id** door uw abonnements-id. Vervang ook de **naam van RG** door de resource groep voor uw beheerde exemplaar en vervang de **mi-naam** door de naam van uw beheerde exemplaar.

```powershell
Install-Module -Name Az

Import-Module Az.Accounts
Import-Module Az.Sql

Connect-AzAccount

# Use your subscription ID in place of subscription-id below

Select-AzSubscription -SubscriptionId {subscription-id}

# Replace rg-name with the resource group for your managed instance, and replace mi-name with the name of your managed instance

$mi = Get-AzSqlInstance -ResourceGroupName {rg-name} -Name {mi-name}

$mi = $mi | Set-AzSqlInstance -PublicDataEndpointEnabled $true -force
```

### <a name="disable-public-endpoint"></a>Openbaar eind punt uitschakelen

Als u het open bare eind punt wilt uitschakelen met behulp van Power shell, voert u de volgende opdracht uit (en vergeet niet om de NSG voor de binnenkomende poort 3342 te sluiten als u deze hebt geconfigureerd):

```powershell
Set-AzSqlInstance -PublicDataEndpointEnabled $false -force
```

## <a name="allow-public-endpoint-traffic-on-the-network-security-group"></a>Openbaar eindpunt verkeer toestaan voor de netwerk beveiligings groep

1. Als u de configuratie pagina van het beheerde exemplaar nog steeds geopend hebt, gaat u naar het tabblad **overzicht** . Als dat niet het geval is, gaat u terug naar uw **SQL Managed instance** -resource. Selecteer de koppeling **virtueel netwerk/subnet** , waarmee u naar de configuratie pagina van het virtuele netwerk gaat.

    ![Mi-overview. png](media/sql-database-managed-instance-public-endpoint-configure/mi-overview.png)

1. Selecteer het tabblad subnetten in het linkerdeel venster configuratie van het virtuele netwerk en noteer de **beveiligings groep** voor uw beheerde exemplaar.

    ![mi-vnet-subnet.png](media/sql-database-managed-instance-public-endpoint-configure/mi-vnet-subnet.png)

1. Ga terug naar de resource groep die uw beheerde exemplaar bevat. De naam van de **netwerk beveiligings groep** die hierboven wordt vermeld, wordt weer gegeven. Selecteer de naam om naar de configuratie pagina voor de netwerk beveiligings groep te gaan.

1. Selecteer het tabblad **binnenkomende beveiligings regels** en voeg een regel **toe** met een hogere prioriteit dan de **deny_all_inbound** -regel met de volgende instellingen: </br> </br>

    |Instelling  |Voorgestelde waarde  |Description  |
    |---------|---------|---------|
    |**Bron**     |Een IP-adres of service label         |<ul><li>Voor Azure-Services, zoals Power BI, selecteert u de Azure Cloud service-tag</li> <li>Gebruik NAT IP-adres voor uw computer of Azure-VM</li></ul> |
    |**Poort bereik van bron**     |*         |Dit op * (alle) laten staan als bron poorten worden meestal dynamisch toegewezen en als zodanig, onvoorspelbaar |
    |**Destination**     |Any         |Bestemming weglaten om verkeer naar het subnet van het beheerde exemplaar toe te staan |
    |**Poortbereiken van doel**     |3342         |Doel poort van bereik tot 3342, wat het open bare TDS-eind punt van het beheerde exemplaar is |
    |**Protocol**     |TCP         |Beheerd exemplaar gebruikt TCP-protocol voor TDS |
    |**Actie**     |Allow         |Binnenkomend verkeer naar een beheerd exemplaar via het open bare eind punt toestaan |
    |**prioriteit**     |1300         |Zorg ervoor dat deze regel een hogere prioriteit heeft dan de **deny_all_inbound** -regel |

    ![mi-nsg-rules.png](media/sql-database-managed-instance-public-endpoint-configure/mi-nsg-rules.png)

    > [!NOTE]
    > Poort 3342 wordt gebruikt voor open bare eindpunt verbindingen met een beheerd exemplaar en kan op dit moment niet worden gewijzigd.

## <a name="obtaining-the-managed-instance-public-endpoint-connection-string"></a>Het open bare endpoint van het beheerde exemplaar verkrijgen connection string

1. Ga naar de configuratie pagina voor SQL Managed instance die is ingeschakeld voor openbaar eind punt. Selecteer het tabblad **verbindings reeksen** onder de configuratie- **instellingen** .
1. Houd er rekening mee dat de hostnaam van het open bare eind punt de notatie < mi_name > bevat. **Public**. < dns_zone >. data base. Windows. net en de poort die voor de verbinding wordt gebruikt, is 3342.

    ![Mi-Public-endpoint-Conn-string. png](media/sql-database-managed-instance-public-endpoint-configure/mi-public-endpoint-conn-string.png)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [gebruik van Azure SQL database Managed instance veilig met een openbaar eind punt](sql-database-managed-instance-public-endpoint-securely.md).