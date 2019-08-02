---
title: 'Zelfstudie: Een elastische groep Azure SQL Database toevoegen aan een failovergroep | Microsoft Docs'
description: Voeg een Azure SQL Database elastische pool toe aan een failovergroep met behulp van de Azure Portal, Power shell of Azure CLI.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.reviewer: sstein, carlrab
ms.date: 06/19/2019
ms.openlocfilehash: 0507b3cb64b4b12bac92cc6bc90120ab4ec56dee
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68568744"
---
# <a name="tutorial-add-an-azure-sql-database-elastic-pool-to-a-failover-group"></a>Zelfstudie: Een Azure SQL Database elastische pool toevoegen aan een failovergroep

Configureer een failovergroep voor een Azure SQL Database elastische pool en testfailover met behulp van de Azure Portal.  In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> - Maak een Azure SQL Database afzonderlijke data base.
> - Voeg de ene data base toe aan een elastische pool. 
> - Maak een [failovergroep](sql-database-auto-failover-group.md) voor een elastische pool tussen twee logische SQL-servers.
> - Testfailover.

## <a name="prerequisites"></a>Vereisten

Het volgende moet zijn geïnstalleerd om deze zelfstudie te voltooien: 

- Een Azure-abonnement. [Maak een gratis account](https://azure.microsoft.com/free/) als u er nog geen hebt.


## <a name="1---create-a-single-database"></a>1-een enkele data base maken 

[!INCLUDE [sql-database-create-single-database](includes/sql-database-create-single-database.md)]

## <a name="2---add-single-database-to-elastic-pool"></a>2-Eén data base toevoegen aan elastische pool

1. Kies **een resource maken** in de linkerbovenhoek van de [Azure Portal](https://portal.azure.com).
1. Typ `elastic pool` in het zoekvak, druk op ENTER, selecteer het pictogram **SQL elastische data base pool** en selecteer vervolgens **maken**. 

    ![Elastische pool kiezen vanuit Marketplace](media/sql-database-elastic-pool-create-failover-group-tutorial/elastic-pool-market-place.png)

1. Configureer uw elastische pool met de volgende waarden:
   - **Naam**: Geef een unieke naam op voor de elastische pool, `myElasticPool`zoals. 
   - **Abonnement**: Selecteer uw abonnement in de vervolgkeuzelijst.
   - **ResourceGroup**: Selecteer `myResourceGroup` in de vervolg keuzelijst de resource groep die u in sectie 1 hebt gemaakt. 
   - **Server**: Selecteer de server die u hebt gemaakt in sectie 1 in de vervolg keuzelijst.  

       ![Nieuwe server voor elastische pool maken](media/sql-database-elastic-pool-create-failover-group-tutorial/use-existing-server-for-elastic-pool.png)

   - **Reken-en opslag**: Selecteer **elastische pool configureren** om uw reken-, opslag-en de afzonderlijke data base aan uw elastische pool toe te voegen. Accepteer op het tabblad **pool instellingen** de standaard waarde van GEN5, met 2 vCores en 32 GB. 

1. Selecteer op de pagina **configureren** het tabblad **data bases** en kies vervolgens **Data Base toevoegen**. Kies de data base die u hebt gemaakt in sectie 1 en selecteer vervolgens **Toep assen** om deze toe te voegen aan uw elastische pool. Selecteer opnieuw **Toep assen** om de instellingen voor de elastische groep toe te passen en sluit de pagina **configureren** . 

    ![SQL-data base toevoegen aan elastische pool](media/sql-database-elastic-pool-create-failover-group-tutorial/add-database-to-elastic-pool.png)

1. Selecteer **controleren + maken** om de instellingen voor de elastische groep te controleren en selecteer vervolgens **maken** om uw elastische pool te maken. 


## <a name="3---create-the-failover-group"></a>3-de groep failover maken 
In deze stap maakt u een failovergroep [](sql-database-auto-failover-group.md) tussen een bestaande Azure SQL-Server en een nieuwe Azure SQL-Server in een andere regio. Voeg vervolgens de elastische pool toe aan de failovergroep. 


1. Selecteer **alle services** in de linkerbovenhoek van de [Azure Portal](https://portal.azure.com). 
1. Typ `sql servers` in het zoekvak. 
1. Beschrijving Selecteer het ster pictogram naast SQL-servers naar favoriete **SQL-servers** en voeg deze toe aan het navigatie deel venster aan de linkerkant. 
    
    ![SQL-servers zoeken](media/sql-database-single-database-create-failover-group-tutorial/all-services-sql-servers.png)

1. Selecteer **SQL-servers** en kies de server die u hebt gemaakt in sectie 1.
1. Selecteer **failover-groepen** in het deel venster **instellingen** en selecteer vervolgens **groep toevoegen** om een nieuwe failovergroep te maken. 

    ![Nieuwe failovergroep toevoegen](media/sql-database-elastic-pool-create-failover-group-tutorial/add-elastic-pool-to-failover-group.png)

1. Voer op de pagina **failover-groep** de volgende waarden in of Selecteer deze en selecteer vervolgens **maken**:
    - **Naam**van failovergroep: Typ een unieke naam voor de failovergroep, zoals `failovergrouptutorial`. 
    - **Secundaire server**: Selecteer de optie voor het configureren van de *vereiste instellingen* en kies vervolgens **een nieuwe server maken**. U kunt ook een al bestaande server kiezen als de secundaire server. Nadat u de volgende waarden hebt ingevoerd, selecteert u **selecteren**. 
        - **Servernaam**: Typ een unieke naam voor de secundaire server, zoals `mysqlsecondary`. 
        - **Aanmeldgegevens van serverbeheerder**: Voert`azureuser`
        - **Wachtwoord**: Typ een complex wacht woord dat voldoet aan de wachtwoord vereisten.
        - **Locatie**: Kies een locatie in de vervolg keuzelijst, zoals VS-Oost 2. Deze locatie mag niet dezelfde locatie zijn als de primaire server.

       > [!NOTE]
       > De aanmeldings-en Firewall instellingen van de server moeten overeenkomen met die van de primaire server. 
    
       ![Een secundaire server maken voor de failovergroep](media/sql-database-single-database-create-failover-group-tutorial/create-secondary-failover-server.png)

1. Zodra een secundaire server is geselecteerd, worden de **data bases in de groeps** optie ontgrendeld. Selecteer deze optie om **data bases te selecteren die u wilt toevoegen** en selecteer vervolgens de elastische pool die u hebt gemaakt in sectie 2. Er wordt een waarschuwing weer gegeven waarin u wordt gevraagd om een elastische pool te maken op de secundaire server. Selecteer de waarschuwing en selecteer **OK** om de elastische pool te maken op de secundaire server. 
        
    ![SQL-data base toevoegen aan failovergroep](media/sql-database-single-database-create-failover-group-tutorial/add-sqldb-to-failover-group.png)
        
1. Selecteer **selecteren** om de instellingen voor de elastische groep toe te passen op de failovergroep en selecteer vervolgens **maken** om uw failovergroep te maken. Door de elastische pool toe te voegen aan de groep failover, wordt het proces voor geo-replicatie automatisch gestart. 


## <a name="4---test-failover"></a>4-failover testen 
In deze stap wordt uw failover-groep overschreven naar de secundaire server en wordt er een failback uitgevoerd met behulp van de Azure Portal. 

1. Navigeer naar uw **SQL** server-server binnen het [Azure Portal](https://portal.azure.com). 
1. Selecteer **failover-groepen** in het deel venster **instellingen** en kies vervolgens de failovergroep die u hebt gemaakt in sectie 2. 
  
   ![De groep failover selecteren in de portal](media/sql-database-single-database-create-failover-group-tutorial/select-failover-group.png)

1. Controleer welke server primair is en welke server secundair is. 
1. Selecteer **failover** in het taak venster voor het uitvoeren van een failover van uw groep met elastische groepen. 
1. Selecteer **Ja** in de waarschuwing waarmee u wordt gewaarschuwd dat TDS-sessies worden losgekoppeld. 

   ![Failover van de failovergroep met uw SQL database](media/sql-database-single-database-create-failover-group-tutorial/failover-sql-db.png)

1. Controleer welke server primair is, welke server secundair is. Als de failover is geslaagd, moeten de twee servers wisselende rollen hebben. 
1. Selecteer **failover** opnieuw om de failback-groep weer in te stellen op de oorspronkelijke instellingen. 

## <a name="clean-up-resources"></a>Resources opschonen 
Resources opschonen door de resource groep te verwijderen. 

1. Navigeer naar uw resource groep in de [Azure Portal](https://portal.azure.com).
1. Selecteer **resource groep verwijderen** om alle resources in de groep en de resource groep zelf te verwijderen. 
1. Typ de naam van de resource groep, `myResourceGroup`in het tekstvak en selecteer vervolgens **verwijderen** om de resource groep te verwijderen.  


## <a name="next-steps"></a>Volgende stappen

In deze zelf studie hebt u een Azure SQL Database afzonderlijke data base aan een failovergroep toegevoegd en is de failover getest. U hebt geleerd hoe u:

> [!div class="checklist"]
> - Maak een Azure SQL Database afzonderlijke data base.
> - Voeg de ene data base toe aan een elastische pool. 
> - Maak een [failovergroep](sql-database-auto-failover-group.md) voor een elastische pool tussen twee logische SQL-servers.
> - Testfailover.

Ga verder met de volgende zelf studie over het migreren met behulp van DMS.

> [!div class="nextstepaction"]
> [Zelfstudie: SQL Server migreren naar een gegroepeerde Data Base met behulp van DMS](../dms/tutorial-sql-server-to-azure-sql.md?toc=/azure/sql-database/toc.json)
