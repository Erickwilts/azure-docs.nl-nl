---
title: Details van Azure Blockchain Workbench-database opvragen
description: Leer hoe u gegevens van een Azure Blockchain Workbench-database en -server kunt opvragen.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 05/09/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: mmercuri
manager: femila
ms.openlocfilehash: 42d119acd8880458eadc1760a7cb9713f91e3f6f
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/09/2019
ms.locfileid: "65509994"
---
# <a name="get-information-about-your-azure-blockchain-workbench-database"></a>Informatie opvragen over uw Azure Blockchain Workbench-database

In dit artikel ziet u hoe u gedetailleerde informatie kunt opvragen over een Azure Blockchain Workbench-database.

## <a name="overview"></a>Overzicht

Informatie over toepassingen, werkstromen en de uitvoering van slimme contracten kunt u opvragen met behulp views in de SQL-database van Blockchain Workbench. Ontwikkelaars kunnen deze informatie gebruiken bij het gebruik van hulpprogramma's, zoals Microsoft Excel, Power BI, Visual Studio en SQL Server Management Studio.

Ontwikkelaars hebben het volgende nodig om verbinding te kunnen maken met de database:

* Externe clienttoegang is toegestaan in de databasefirewall. In dit artikel over het configureren van een databasefirewall wordt uitgelegd hoe u toegang kunt verlenen.
* De naam van de databaseserver en van de database.

## <a name="connect-to-the-blockchain-workbench-database"></a>Verbinding maken met een database van Blockchain Workbench

Verbinding maken met de database:

1. Meld u aan bij Azure portal met een account met **eigenaar** machtigingen voor de resources die Azure Blockchain Workbench.
2. Kies **Resourcegroepen** in het linkernavigatievenster.
3. Kies de naam van de resourcegroep voor uw implementatie van Blockchain Workbench.
4. Selecteer **Type** om de lijst met resources te sorteren en kies vervolgens uw **SQL-server**. De gesorteerde lijst in de volgende schermafbeelding bevat twee SQL-databases, 'master' en een database die 'lhgn' gebruikt als de waarde voor **Voorvoegsel resource**.

   ![Lijst met gesorteerde resources van Azure Blockchain Workbench](./media/getdb-details/sorted-workbench-resource-list.png)

5. Als u gedetailleerde gegevens wilt weergeven voor de database van Blockchain Workbench, selecteert u de link voor de database met het **resourcevoorvoegsel** dat u hebt opgegeven voor het implementeren van Blockchain Workbench.

   ![Databasedetails](./media/getdb-details/workbench-db-details.png)

Met behulp van de naam van de databaseserver en de database kunt u vanuit uw ontwikkel- of rapportageprogramma verbinding maken met de database Blockchain Workbench.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Databaseweergaven in Azure Blockchain Workbench](database-views.md)