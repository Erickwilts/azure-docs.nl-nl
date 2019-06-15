---
title: Transparante gegevensversleuteling inschakelt voor Stretch Database TSQL - Azure | Microsoft Docs
description: Transparent Data Encryption (TDE) voor SQL Server Stretch Database op Azure TSQL inschakelen
services: sql-server-stretch-database
documentationcenter: ''
ms.assetid: 27753d91-9ca2-4d47-b34d-b5e2c2f029bb
ms.service: sql-server-stretch-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 01/23/2017
author: blazem-msft
ms.author: blazem
ms.reviewer: jroth
manager: jroth
ms.openlocfilehash: 9718db18ea675fa744262f0736aff3c07732e1d1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66002876"
---
# <a name="enable-transparent-data-encryption-tde-for-stretch-database-on-azure-transact-sql"></a>Transparent Data Encryption (TDE) inschakelen voor Stretch Database in Azure (Transact-SQL)
> [!div class="op_single_selector"]
> * [Azure Portal](sql-server-stretch-database-encryption-tde.md)
> * [TSQL](sql-server-stretch-database-tde-tsql.md)
>
>

Transparante gegevensversleuteling (TDE) helpt te beschermen tegen de dreiging van schadelijke activiteiten door het uitvoeren van real-time versleuteling en ontsleuteling van de database, gekoppelde back-ups en transactielogboekbestanden in rust zonder wijzigingen aan de toepassing.

De opslag van een volledige database versleutelt TDE met behulp van een symmetrische sleutel met de naam de databaseversleutelingssleutel. De versleutelingssleutel van de database wordt beveiligd door een ingebouwde servercertificaat. Het certificaat van de ingebouwde is uniek voor elke Azure-server. Microsoft draait automatisch deze certificaten ten minste elke 90 dagen. Zie voor een algemene beschrijving van TDE [TDE (Transparent Data Encryption)].

## <a name="enabling-encryption"></a>Versleuteling is ingeschakeld
Als u wilt TDE inschakelen voor een Azure-database die de gegevens opslaat gemigreerd van een Stretch-functionaliteit van SQL Server-database, het volgende doen:

1. Verbinding maken met de *master* database op de Azure-server die als host fungeert voor de database met behulp van een aanmelding die een beheerder of lid zijn van de **dbmanager** rol in de hoofddatabase
2. Voer de volgende instructie voor het versleutelen van de database uit.

```sql
ALTER DATABASE [database_name] SET ENCRYPTION ON;
```

## <a name="disabling-encryption"></a>Als u versleuteling uitschakelt
Als u wilt TDE uitschakelen voor een Azure-database die de gegevens opslaat gemigreerd van een Stretch-functionaliteit van SQL Server-database, het volgende doen:

1. Verbinding maken met de *master* database met behulp van een aanmelding die een beheerder of lid zijn van de **dbmanager** rol in de hoofddatabase
2. Voer de volgende instructie voor het versleutelen van de database uit.

```sql
ALTER DATABASE [database_name] SET ENCRYPTION OFF;
```

## <a name="verifying-encryption"></a>Controleren van versleuteling
Om te controleren of de status voor schijfversleuteling voor een Azure-database die de gegevens opslaat gemigreerd van een Stretch-functionaliteit van SQL Server-database, moet u de volgende dingen doen:

1. Verbinding maken met de *master* of exemplaar in de database met behulp van een aanmelding die een beheerder of lid zijn van de **dbmanager** rol in de hoofddatabase
2. Voer de volgende instructie voor het versleutelen van de database uit.

```sql
SELECT
    [name],
    [is_encrypted]
FROM
    sys.databases;
```

Een resultaat van ```1``` geeft aan dat een versleutelde database ```0``` geeft aan dat een niet-versleutelde database.

<!--Anchors-->
[TDE (Transparent Data Encryption)]: https://msdn.microsoft.com/library/bb934049.aspx


<!--Image references-->

<!--Link references-->
