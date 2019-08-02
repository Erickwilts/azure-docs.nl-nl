---
title: SaaS-zelf studie met één Tenant-Azure SQL Database | Microsoft Docs
description: Implementeer en verken een zelfstandige SaaS-toepassing met één Tenant die gebruikmaakt van Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MightyPen
ms.author: genemi
ms.reviewer: sstein
ms.date: 11/07/2018
ms.openlocfilehash: 2e6b18e53358cad1bfe89e8c0ae7fbacec24d179
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68570207"
---
# <a name="deploy-and-explore-a-standalone-single-tenant-application-that-uses-azure-sql-database"></a>Een zelfstandige toepassing met één Tenant implementeren en verkennen die gebruikmaakt van Azure SQL Database

In deze zelf studie implementeert en bekijkt u de Wingtip tickets SaaS-voorbeeld toepassing die is ontwikkeld met behulp van de zelfstandige toepassing of het app-per-Tenant-patroon.  De toepassing is ontworpen voor het uitbrengen van functies van Azure SQL Database die het inschakelen van SaaS-scenario's met meerdere tenants vereenvoudigen.

De zelfstandige toepassing of het app-per-Tenant patroon implementeert een toepassings exemplaar voor elke Tenant.  Elke toepassing wordt geconfigureerd voor een specifieke Tenant en geïmplementeerd in een afzonderlijke Azure-resource groep. Meerdere exemplaren van de toepassing zijn ingericht om een multi tenant oplossing te bieden. Dit patroon is het meest geschikt voor kleinere aantallen tenants waarbij Tenant isolatie een topprioriteit is. Azure heeft partner Programma's waarmee resources kunnen worden geïmplementeerd in het abonnement van een Tenant en worden beheerd door een service provider op naam van de Tenant. 

In deze zelf studie implementeert u drie zelfstandige toepassingen voor drie tenants in uw Azure-abonnement.  U hebt volledige toegang om te verkennen en om te werken met de afzonderlijke toepassings onderdelen.

De bron code-en beheer scripts van de toepassing zijn beschikbaar in de [WingtipTicketsSaaS-StandaloneApp](https://github.com/Microsoft/WingtipTicketsSaaS-StandaloneApp) github opslag plaats. De toepassing is gemaakt met behulp van Visual Studio 2015 en kan niet worden geopend en gecompileerd in Visual Studio 2019 zonder te worden bijgewerkt.


In deze zelfstudie komen deze onderwerpen aan bod:

> [!div class="checklist"]
> * Hoe u de zelfstandige Wingtip tickets SaaS-toepassing implementeert.
> * Waar de bron code van de toepassing en beheer scripts worden opgehaald.
> * Over de servers en data bases die de app vormen.

Aanvullende zelf studies worden uitgebracht. Ze bieden u de mogelijkheid om een reeks beheer scenario's te verkennen op basis van dit toepassings patroon.   

## <a name="deploy-the-wingtip-tickets-saas-standalone-application"></a>De zelfstandige SaaS-toepassing Wingtip tickets implementeren

Implementeer de app voor de drie beschik bare tenants:

1. Klik op elke blauwe knop **implementeren naar Azure** om de implementatie sjabloon te openen in de [Azure Portal](https://portal.azure.com). Voor elke sjabloon zijn twee parameter waarden vereist. een naam voor een nieuwe resource groep en een gebruikers naam die deze implementatie onderscheidt van andere implementaties van de app. De volgende stap bevat details over het instellen van deze waarden.<br><br>
    <a href="https://aka.ms/deploywingtipsa-contoso" target="_blank"><img style="vertical-align:middle" src="media/saas-standaloneapp-get-started-deploy/deploy.png"/></a>**Concert zaal contoso** &nbsp;
<br><br>
    <a href="https://aka.ms/deploywingtipsa-dogwood" target="_blank"><img style="vertical-align:middle" src="media/saas-standaloneapp-get-started-deploy/deploy.png"/></a> &nbsp; **Dogwood Dojo**
<br><br>
    <a href="https://aka.ms/deploywingtipsa-fabrikam" target="_blank"><img style="vertical-align:middle" src="media/saas-standaloneapp-get-started-deploy/deploy.png"/></a> &nbsp; **Fabrikam Jazz Club**

2. Voer de vereiste parameter waarden in voor elke implementatie.

    > [!IMPORTANT]
    > Sommige verificatie-en server firewalls zijn opzettelijk onveilig voor demonstratie doeleinden. **Maak een nieuwe resource groep** voor elke toepassings implementatie.  Gebruik geen bestaande resource groep. Gebruik deze toepassing of resources die worden gemaakt, niet voor productie. Verwijder alle resource groepen wanneer u klaar bent met de toepassingen om gerelateerde facturering te stoppen.

    Het is raadzaam alleen kleine letters, cijfers en afbreek streepjes in de resource namen te gebruiken.
    * Voor **resource groep**selecteert u nieuwe maken en geeft u een kleine naam op voor de resource groep. **Wingtip-sa-\<\>locatiegebruikerishetAanbevolenpatroon\>. -\<**  Vervang de naam van de locatie voor \<de locatie van de locatie zonder spaties.\> Voor \<de\>gebruiker vervangt u de onderstaande gebruikers waarde.  Met dit patroon kunnen namen van resource groepen *Wingtip-sa-contosoconcerthall-AF1*, *Wingtip-sa-dogwooddojo-AF1*, *Wingtip-sa-fabrikamjazzclub-AF1*zijn.
    * Selecteer een **locatie** in de vervolg keuzelijst.

    * Voor de **gebruiker** : we raden een korte gebruikers waarde aan, zoals uw initialen plus een cijfer: bijvoorbeeld *AF1*.


3. **Implementeer de toepassing**.

    * Klik om akkoord te gaan met de voor waarden.
    * Klik op **Kopen**.

4. U kunt de status van alle drie de implementaties controleren door te klikken op **meldingen** (het klok pictogram rechts van het zoekvak). Het implementeren van de apps duurt ongeveer vijf minuten.


## <a name="run-the-applications"></a>De toepassingen uitvoeren

De app geeft een overzicht van locaties die gebeurtenissen hosten.  De locaties zijn de tenants van de toepassing. Elke locatie haalt een gepersonaliseerde website op om hun evenementen te vermelden en kaarten te verkopen. Locatie typen zijn onder andere concert huizen, Jazz klaveren en sport clubs. In het voor beeld bepaalt het type locatie de achtergrond foto die wordt weer gegeven op de website van de site.   In het zelfstandige app-model heeft elke locatie een afzonderlijk toepassings exemplaar met een eigen zelfstandige SQL database.

1. Open de pagina gebeurtenissen voor elk van de drie tenants in afzonderlijke browser tabbladen:

   - http://events.contosoconcerthall.&lt ;user&gt;.trafficmanager.net
   - http://events.dogwooddojo.&lt ;user&gt;.trafficmanager.net
   - http://events.fabrikamjazzclub.&lt ;user&gt;.trafficmanager.net

     (Vervang &lt;&gt; de gebruiker in elke URL door de gebruikers waarde van uw implementatie.)

   ![Events](./media/saas-standaloneapp-get-started-deploy/fabrikam.png)

De app maakt gebruik van [*Azure Traffic Manager*](../traffic-manager/traffic-manager-overview.md)om de distributie van binnenkomende aanvragen te beheren. Elk Tenant-specifiek app-exemplaar bevat de Tenant naam als onderdeel van de domein naam in de URL. Alle Tenant-Url's bevatten uw specifieke **gebruikers** waarde. De Url's volgen de volgende indeling:
- http://events.&lt ;venuename&gt;.&lt; user&gt;.trafficmanager.net

De **locatie** van de data base van elke Tenant is opgenomen in de app-instellingen van de bijbehorende geïmplementeerde app.

In een productie omgeving maakt u doorgaans een CNAME DNS-record om het [*Internet domein van een bedrijf*](../traffic-manager/traffic-manager-point-internet-domain.md) te laten verwijzen naar de URL van het Traffic Manager-profiel.


## <a name="explore-the-servers-and-tenant-databases"></a>De servers en Tenant databases verkennen

Laten we eens kijken naar enkele van de resources die zijn geïmplementeerd:

1. Blader in het [Azure Portal](https://portal.azure.com)naar de lijst met resource groepen.
2. U ziet nu de drie Tenant resource groepen.
3. Open de resource groep **Wingtip-sa-&lt;fabrikam&gt; -User** . Deze bevat de resources voor de implementatie van Fabrikam Jazz Club.  De **fabrikamjazzclub-&lt;gebruikers&gt;**  server bevat de **fabrikamjazzclub** -data base.

Elke Tenant database is een een *zelfstandige* 50 DTU-data base.

## <a name="additional-resources"></a>Aanvullende resources

<!--
* Additional [tutorials that build on the Wingtip SaaS application](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* To learn about elastic pools, see [*What is an Azure SQL elastic pool*](sql-database-elastic-pool.md)
* To learn about elastic jobs, see [*Managing scaled-out cloud databases*](elastic-jobs-overview.md)
-->

- Zie [ontwerp patronen voor SaaS-toepassingen met meerdere](saas-tenancy-app-design-patterns.md)tenants voor meer informatie over SaaS-toepassingen met meerdere tenants.

 
## <a name="delete-resource-groups-to-stop-billing"></a>Resource groepen verwijderen om de facturering te stoppen ##

Wanneer u klaar bent met het gebruik van het voor beeld, verwijdert u alle resource groepen die u hebt gemaakt om de gekoppelde facturering te stoppen.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende geleerd:

> [!div class="checklist"]
> * Hoe u de zelfstandige Wingtip tickets SaaS-toepassing implementeert.
> * Over de servers en data bases die de app vormen.
> * Voorbeeld resources verwijderen om gerelateerde facturering te stoppen.

Vervolgens kunt u de zelf studie voor het [inrichten en de catalogus](saas-standaloneapp-provision-and-catalog.md) gebruiken om het gebruik van een catalogus met tenants te verkennen die verschillende scenario's voor meerdere tenants mogelijk maakt, zoals schema beheer en Tenant-analyses.
 

