---
title: bestand opnemen
description: bestand opnemen
services: cosmos-db
author: rimman
ms.service: cosmos-db
ms.topic: include
ms.date: 04/08/2019
ms.author: rimman
ms.custom: include file
ms.openlocfilehash: 5d57d7e18befba175a5a8a825494ce512644b5a2
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "67175788"
---
1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
1. Selecteer **Een resource maken** > **Databases** > **Azure Cosmos DB**.
   
   ![Het deelvenster Databases in Azure Portal](./media/cosmos-db-create-dbaccount/create-nosql-db-databases-json-tutorial-1.png)

1. Voer op de pagina **Azure Cosmos DB account maken** de basis instellingen in voor het nieuwe Azure Cosmos-account. 
 
    |Instelling|Value|Description |
    |---|---|---|
    |Subscription|Abonnementsnaam|Selecteer het Azure-abonnement dat u wilt gebruiken voor dit Azure Cosmos-account. |
    |Resourcegroep|Resourcegroepnaam|Selecteer een resource groep of selecteer **nieuwe maken**en voer vervolgens een unieke naam in voor de nieuwe resource groep. |
    | Accountnaam|Voer een unieke naam in|Voer een naam in om uw Azure Cosmos-account aan te duiden. Gebruik een unieke id omdat *documents.azure.com* is toegevoegd aan de id die u hebt opgegeven om uw URI te maken.<br><br>De id mag alleen kleine letters, cijfers en het koppelteken (-) bevatten. De waarde moet tussen de 3-31 tekens lang zijn.|
    | API|Core (SQL)|De API bepaalt het type te maken account. Azure Cosmos DB heeft vijf API's: Core (SQL) en MongoDB voor document gegevens, Gremlin voor grafiek gegevens, Azure Table en Cassandra. Op dit moment moet u voor elke API een afzonderlijk account maken. <br><br>Selecteer **kern (SQL)** om een document database en-query te maken met behulp van de SQL-syntaxis. <br><br>[Meer informatie over de SQL-API](../articles/cosmos-db/documentdb-introduction.md).|
    | Location|Selecteer de regio het dichtst in de buurt van uw gebruikers|Selecteer een geografische locatie waar u het Azure Cosmos DB-account wilt hosten. Gebruik de locatie die zich het dichtst bij uw gebruikers bevindt om hen de snelste toegang tot de gegevens te geven.|
   
   ![De pagina Nieuw account voor Azure Cosmos DB](./media/cosmos-db-create-dbaccount/azure-cosmos-db-create-new-account.png)

1. Selecteer **Controleren + maken**. U kunt de secties **netwerk** en **Tags** overs Laan. 

1. Controleer de account instellingen en selecteer vervolgens **maken**. Het duurt enkele minuten om het account te maken. Wacht tot de portal pagina **uw implementatie is voltooid**. 

    ![Het deelvenster Meldingen in Azure Portal](./media/cosmos-db-create-dbaccount/azure-cosmos-db-account-created.png)

1. Selecteer **Ga naar resource** om naar de pagina Azure Cosmos DB-account te gaan. 

    ![De pagina Azure Cosmos DB-account](./media/cosmos-db-create-dbaccount/azure-cosmos-db-account-created-2.png)
