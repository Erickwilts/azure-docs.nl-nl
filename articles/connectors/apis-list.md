---
title: Connectors voor Azure Logic Apps
description: Werk stromen automatiseren met connectors voor Azure Logic Apps, waaronder ingebouwd, beheerd, on-premises, integratie account en zakelijke connectors
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.suite: integration
ms.topic: article
ms.date: 05/08/2019
ms.openlocfilehash: ab2413cfce8b87fbe1899a0b7c465c6e6c27a3f5
ms.sourcegitcommit: 9a699d7408023d3736961745c753ca3cec708f23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/16/2019
ms.locfileid: "68277648"
---
# <a name="connectors-for-azure-logic-apps"></a>Connectors voor Azure Logic Apps

Connectors bieden snelle toegang tot Azure Logic Apps gebeurtenissen, gegevens en acties in andere apps, services, systemen, protocollen en platformen. Door connectors in uw Logic apps te gebruiken, vouwt u de mogelijkheden voor uw Cloud en on-premises apps uit om taken uit te voeren met de gegevens die u maakt en al hebt.

Hoewel Logic Apps [honderden connectors](https://docs.microsoft.com/connectors)biedt, worden in dit artikel populaire en veelgebruikte connectors beschreven die worden gebruikt door duizenden apps en miljoenen uitvoeringen voor het verwerken van gegevens en informatie. Als u wilt zoeken naar de volledige lijst met connectors en de referentie gegevens van elke connector, zoals triggers, acties en limieten, raadpleegt u de referentie pagina's van de connector onder [connectors Overview](https://docs.microsoft.com/connectors)(Engelstalig). Meer informatie over [Triggers en acties](#triggers-actions), [Logic apps prijs model](../logic-apps/logic-apps-pricing.md)en [Logic apps prijs informatie](https://azure.microsoft.com/pricing/details/logic-apps/). 

> [!NOTE]
> Als u wilt integreren met een service of API die geen connector heeft, kunt u de service rechtstreeks aanroepen via een protocol zoals HTTP of een [aangepaste connector](#custom)maken.

Connectors zijn beschikbaar als ingebouwde triggers en acties of als beheerde connectors:

<a name="built-in"></a>

* [**Ingebouwde modules**](#built-ins): Deze ingebouwde triggers en acties zijn ' native ' voor het Azure Logic Apps en helpen u bij het maken van logische apps die worden uitgevoerd op aangepaste schema's, communiceren met andere eind punten, aanvragen ontvangen en erop reageren en Azure functions aanroepen, Azure API Apps (Web Apps), uw eigen Api's wordt beheerd en gepubliceerd met Azure API Management en geneste Logic apps die aanvragen kunnen ontvangen. U kunt ook ingebouwde acties gebruiken die u helpen bij het organiseren en beheren van de werk stroom van uw logische app en ook met gegevens werken.

  > [!NOTE]
  > Logic apps binnen een [Integration service-omgeving (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) hebben rechtstreeks toegang tot resources in een virtueel Azure-netwerk.
  > Wanneer u een ISE gebruikt, worden ingebouwde triggers en acties die het **kern** label weer geven, uitgevoerd in dezelfde ISE als uw logische apps. Logic apps, ingebouwde triggers en ingebouwde acties die worden uitgevoerd in uw ISE, gebruiken een prijs plan dat verschilt van het prijs plan op basis van verbruik.
  >
  > Zie [verbinding maken met virtuele Azure-netwerken vanuit Azure Logic apps](../logic-apps/connect-virtual-network-vnet-isolated-environment.md#create-logic-apps-environment)voor meer informatie over het maken van ISEs. 
  > Zie [Logic apps prijs model](../logic-apps/logic-apps-pricing.md)voor meer informatie over prijzen.

<a name="managed-connectors"></a>

* **Beheerde connectors**: Deze connectors worden geïmplementeerd en beheerd door micro soft en bieden triggers en acties voor toegang tot Cloud Services, on-premises systemen of beide, waaronder Office 365, Azure Blob Storage, SQL Server, Dynamics, Sales Force, share point en meer. Sommige connectors ondersteunen specifiek business-to-Business (B2B)-communicatie scenario's en vereisen een [integratie account](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) dat is gekoppeld aan uw logische app. Voordat u bepaalde connectors gebruikt, moet u mogelijk eerst verbindingen maken, die worden beheerd door Azure Logic Apps. 

  Als u bijvoorbeeld gebruikmaakt van micro soft BizTalk Server, kunnen uw logische apps verbinding maken met en communiceren met uw BizTalk Server met behulp van de [BizTalk Server on-premises connector](#on-premises-connectors). 
  U kunt vervolgens BizTalk-achtige bewerkingen in uw Logic apps uitbreiden of uitvoeren met behulp van de connectors voor het [integratie account](#integration-account-connectors).

  Connectors worden geclassificeerd als Standard of ENTER prise. 
  [Enter prise](#enterprise-connectors) -connectors bieden toegang tot bedrijfs systemen zoals SAP, IBM MQ en IBM 3270 voor extra kosten. Als u wilt bepalen of een connector Standard of ENTER prise is, raadpleegt u de technische details van de referentie pagina van elke connector onder [connectors Overview](https://docs.microsoft.com/connectors)(Engelstalig). 

  U kunt connectors ook identificeren met behulp van deze categorieën, hoewel sommige connectors meerdere categorieën kunnen passeren. 
  SAP is bijvoorbeeld een Enter prise-connector en een on-premises connector:

  |   |   |
  |---|---|
  | [**Beheerde API-connectors**](#managed-api-connectors) | Maak Logic apps die gebruikmaken van services zoals Azure Blob Storage, Office 365, Dynamics, Power BI, OneDrive, Sales Force, share point online en nog veel meer. |
  | [**On-premises connectors**](#on-premises-connectors) | Nadat u de [on-premises gegevens gateway][gateway-doc]hebt geïnstalleerd en ingesteld, kunnen deze connectors uw Logic apps gebruiken om toegang te krijgen tot on-premises systemen zoals SQL Server, share Point Server, Oracle DB, bestands shares en anderen. |
  | [**Connectors van het integratie account**](#integration-account-connectors) | Deze connectors zijn beschikbaar wanneer u een integratie account maakt en valideert XML, platte bestanden coderen en decoderen en B2B-berichten (Business-to-Business) met AS2-, EDIFACT-en X12-protocollen verwerkt. |
  |||

  > [!NOTE]
  > Logic apps binnen een [Integration service-omgeving (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) hebben rechtstreeks toegang tot resources in een virtueel Azure-netwerk. Wanneer u een ISE-, Standard-en Enter prise-connector gebruikt die het label **ISE** weergeeft, worden deze uitgevoerd in dezelfde ISE als uw logische apps. Connectors die het label ISE niet weer geven, worden uitgevoerd in de service globale Logic Apps.
  >
  > Voor on-premises systemen die zijn verbonden met een virtueel Azure-netwerk, injecteert u uw ISE in dat netwerk, zodat uw Logic apps rechtstreeks toegang hebben tot deze systemen met behulp van een connector met een **ISE** -label, een http-actie of een [aangepaste connector](#custom). Logic apps en connectors die worden uitgevoerd in uw ISE, gebruiken een prijs plan dat verschilt van het prijs plan op basis van verbruik. 
  >
  > Zie [verbinding maken met virtuele Azure-netwerken vanuit Azure Logic apps](../logic-apps/connect-virtual-network-vnet-isolated-environment.md#create-logic-apps-environment)voor meer informatie over het maken van ISEs.
  > Zie [Logic apps prijs model](../logic-apps/logic-apps-pricing.md)voor meer informatie over prijzen.

  Voor de volledige lijst met connectors en de referentie gegevens van elke connector, zoals acties en triggers, die zijn gedefinieerd door de beschrijving van een OpenAPI (voorheen Swagger), plus eventuele limieten, kunt u de volledige lijst vinden onder het [overzicht connectors](/connectors/). Zie [Logic apps prijs model](../logic-apps/logic-apps-pricing.md)voor prijzen en [Logic apps prijs](https://azure.microsoft.com/pricing/details/logic-apps/)informatie. 

<a name="built-ins"></a>

## <a name="built-ins"></a>Ingebouwde modules

Logic Apps biedt ingebouwde triggers en acties zodat u werk stromen op basis van een planning kunt maken, uw Logic apps kan helpen communiceren met andere apps en services, de werk stroom beheren via uw Logic apps en gegevens beheert of bewerkt.

|   |   |   |   | 
|---|---|---|---| 
| [![API-][schedule-icon]<br/>pictogram**schema**][recurrence-doc] | -Voer uw logische app uit volgens een opgegeven planning, variërend van eenvoudige tot complexe terugkeer patronen met de trigger voor **terugkeer patroon** . <p>-Pauzeer uw logische app voor een opgegeven duur met de **vertragings** actie. <p>-Pauzeer uw logische app tot de opgegeven datum en tijd met de **vertraging tot** actie. | [![API-][http-icon]<br/>pictogram**http**][http-doc] | Communiceer met elk eind punt over HTTP met triggers en acties voor HTTP, HTTP + Swagger en HTTP + webhook. | 
| [![API-][http-request-icon]<br/>pictogram**aanvraag**][http-request-doc] | -Maak uw logische app aanroepen van andere apps of services, Activeer Event Grid bron gebeurtenissen of Activeer reacties op antwoorden op Azure Security Center waarschuwingen met de **aanvraag** trigger. <p>-Antwoorden verzenden naar een app of service met de **reactie** actie. | [![API-][batch-icon]<br/>pictogram**batch**][batch-doc] | -Berichten in batches verwerken met de trigger voor **batch berichten** . <p>-Logische apps aanroepen die bestaande batch triggers hebben met de actie **berichten verzenden naar batch** . | 
| [![API-][azure-functions-icon]<br/>pictogram**Azure functions**][azure-functions-doc] | Aanroepen van Azure functions die aangepaste code fragmentenC# (of node. js) uitvoeren vanuit uw Logic apps. | [![API-][azure-api-management-icon]</br>pictogram**Azure API Management**][azure-api-management-doc] | Activeer triggers en acties die zijn gedefinieerd door uw eigen Api's die u beheert en publiceert met Azure API Management. | 
| [![API-][azure-app-services-icon]<br/>pictogram**Azure-app Services**][azure-app-services-doc] | Aanroepen van Azure API Apps, of Web Apps, gehost op Azure App Service. De triggers en acties die door deze apps worden gedefinieerd, worden weer gegeven als andere triggers voor de eerste klasse en acties wanneer Swagger is opgenomen. | [![API-][azure-logic-apps-icon]<br/>pictogram**Azure<br/>Logic apps**][nested-logic-app-doc] | Roep andere logische apps aan die beginnen met een aanvraag trigger. | 
||||| 

### <a name="control-workflow"></a>Werk stroom beheren

Logic Apps biedt ingebouwde acties voor het structureren en beheren van de acties in de werk stroom van uw logische app:

|   |   |   |   | 
|---|---|---|---| 
| [![Ingebouwde pictogram][condition-icon]<br/>**voorwaarde**][condition-doc] | Evalueer een voor waarde en voer verschillende acties uit op basis van het feit of de voor waarde waar of onwaar is. | [![Ingebouwd pictogram][for-each-icon]</br>**voor elk**][for-each-doc] | Voer dezelfde acties uit op elk item in een matrix. | 
| [![Ingebouwd pictogram][scope-icon]<br/>**bereik**][scope-doc] | Groepeer acties in *bereiken*die hun eigen status krijgen nadat de acties in het bereik zijn uitgevoerd. | [![Ingebouwde pictogram][switch-icon]</br>**Switch**][switch-doc] | Groepeer acties in *gevallen*, waaraan unieke waarden worden toegewezen, met uitzonde ring van de standaard situatie. Voer alleen die aanvraag uit waarvan de toegewezen waarde overeenkomt met het resultaat van een expressie, object of token. Als er geen overeenkomsten bestaan, voert u het standaard hoofdletter gebruik uit. | 
| [![Het ingebouwde pictogram][terminate-icon]<br/>**beëindigen**][terminate-doc] | Stop een actieve werk stroom voor logische apps. | [![Ingebouwd pictogram][until-icon]<br/>**tot**][until-doc] | Herhaal acties totdat de opgegeven voor waarde waar is of een andere status is gewijzigd. | 
||||| 

### <a name="manage-or-manipulate-data"></a>Gegevens beheren of bewerken

Logic Apps biedt ingebouwde acties voor het werken met gegevens uitvoer en de bijbehorende indelingen:  

|   |   | 
|---|---| 
| [![Ingebouwde pictogram][data-operations-icon]<br/>**gegevens bewerkingen**][data-operations-doc] | Bewerkingen uitvoeren met gegevens: <p>- **Opstellen**: Maak één uitvoer van meerdere invoer met verschillende typen. <br>- **CSV-tabel maken**: Een tabel met door komma's gescheiden waarden (CSV) maken op basis van een matrix met JSON-objecten. <br>- **HTML-tabel maken**: Een HTML-tabel maken op basis van een matrix met JSON-objecten. <br>- **Filter matrix**: Maak een matrix van items in een andere matrix die aan uw criteria voldoen. <br>- **Koppelen**: Een teken reeks maken van alle items in een matrix en deze items scheiden met het opgegeven scheidings teken. <br>- **JSON parseren**: Maak gebruikers vriendelijke tokens van eigenschappen en hun waarden in JSON-inhoud zodat u deze eigenschappen in uw werk stroom kunt gebruiken. <br>- **Selecteer**: Maak een matrix met JSON-objecten door items of waarden in een andere matrix te transformeren en deze items aan de opgegeven eigenschappen toe te wijzen. | 
| ![Ingebouwd pictogram][date-time-icon]<br/>**Datum en tijd** | Bewerkingen uitvoeren met tijds tempels: <p>- **Toevoegen aan tijd**: Voeg het opgegeven aantal eenheden toe aan een tijds tempel. <br>- **Tijd zone converteren**: Converteer een tijds tempel van de bron tijdzone naar de doel tijdzone. <br>- **Huidige tijd**: De huidige tijds tempel retour neren als een teken reeks. <br>- **Volgende tijd ophalen**: De huidige tijds tempel en de opgegeven tijds eenheden retour neren. <br>- **Vorige tijd ophalen**: Retourneert de huidige tijds tempel min de opgegeven tijds eenheden. <br>- **Aftrekken van tijd**: Een aantal tijds eenheden aftrekken van een tijds tempel. |
| [![Ingebouwde pictogram][variables-icon]<br/>**variabelen**][variables-doc] | Bewerkingen uitvoeren met variabelen: <p>- **Toevoegen aan matrix variabele**: Een waarde invoegen als laatste item in een matrix die is opgeslagen door een variabele. <br>- **Toevoegen aan teken reeks variabele**: Een waarde invoegen als het laatste teken in een teken reeks die is opgeslagen door een variabele. <br>- **Variabele verlagen**: Verklein een variabele met een constante waarde. <br>- **Oplopende variabele**: Verhoog een variabele met een constante waarde. <br>- **Variabele initialiseren**: Maak een variabele en Declareer het gegevens type en de oorspronkelijke waarde. <br>- **Variabele instellen**: Wijs een andere waarde toe aan een bestaande variabele. |
|  |  | 

<a name="managed-api-connectors"></a>

## <a name="managed-api-connectors"></a>Beheerde API-connectors

Logic Apps biedt deze populaire standaard connectors voor het automatiseren van taken, processen en werk stromen met deze services of systemen.

|   |   |   |   | 
|---|---|---|---| 
| [![API-][azure-service-bus-icon]<br/>pictogram**Azure service bus**][azure-service-bus-doc] | Beheer asynchrone berichten, sessies en onderwerp-abonnementen met de meest gebruikte connector in Logic Apps. | [![API-][sql-server-icon]<br/>pictogram**SQL Server**][sql-server-doc] | Maak verbinding met uw SQL Server on-premises of een Azure SQL Database in de Cloud, zodat u records kunt beheren, opgeslagen procedures uitvoert of query's uitvoert. | 
| [![API-][office-365-outlook-icon]<br/>pictogram**Office<br/>365 Outlook**][office-365-outlook-doc] | Maak verbinding met uw e-mail account voor Office 365 zodat u e-mail berichten, taken, agenda-items en vergaderingen, contact personen, aanvragen en meer kunt maken en beheren. | [![API-][azure-blob-storage-icon]<br/>pictogram**Azure<br/>Blob-opslag**][azure-blob-storage-doc] | Maak verbinding met uw opslag account zodat u blob-inhoud kunt maken en beheren. | 
| [![API-][sftp-icon]<br/>pictogram**SFTP**][sftp-doc] | Verbinding maken met SFTP-servers die u vanaf internet kunt gebruiken zodat u kunt werken met uw bestanden en mappen. | [![API-][sharepoint-online-icon]<br/>pictogram **<br/>share point online**][sharepoint-online-doc] | Maak verbinding met share point online, zodat u bestanden, bijlagen, mappen en meer kunt beheren. | 
| [![API-][dynamics-365-icon]<br/>pictogram**Dynamics<br/>365 CRM Online**][dynamics-365-doc] | Maak verbinding met uw Dynamics 365-account zodat u records, items en meer kunt maken en beheren. | [![API-][ftp-icon]<br/>pictogram**FTP**][ftp-doc] | Verbinding maken met FTP-servers die u vanaf internet kunt gebruiken zodat u kunt werken met uw bestanden en mappen. | 
| [![API-][salesforce-icon]<br/>pictogram**Sales Force**][salesforce-doc] | Maak verbinding met uw Sales Force-account, zodat u items zoals records, taken, objecten en meer kunt maken en beheren. | [![API-][twitter-icon]<br/>pictogram**Twitter**][twitter-doc] | Maak verbinding met uw Twitter-account zodat u tweets, volgers, uw tijd lijn en meer kunt beheren. Sla uw tweets op in SQL, Excel of share point. | 
| [![API-][azure-event-hubs-icon]<br/>pictogram**Azure Event hubs**][azure-event-hubs-doc] | Gebeurtenissen gebruiken en publiceren via een event hub. Haal bijvoorbeeld uitvoer van uw logische app met Event Hubs op en verzend die uitvoer naar een realtime analyse provider. | [![API-][azure-event-grid-icon]<br/>pictogram**Azure Event**</br>**grid**][azure-event-grid-doc] | Gebeurtenissen bewaken die zijn gepubliceerd door een Event Grid, bijvoorbeeld wanneer Azure-resources of bronnen van derden worden gewijzigd. | 
|||||

<a name="on-premises-connectors"></a>

## <a name="on-premises-connectors"></a>On-premises connectors 

Hier volgen enkele veelgebruikte standaard-connectors die Logic Apps biedt voor toegang tot gegevens en resources in on-premises systemen. Voordat u een verbinding met een on-premises systeem kunt maken, moet u eerst [een on-premises gegevens gateway downloaden, installeren en instellen][gateway-doc]. Deze gateway biedt een beveiligd communicatie kanaal zonder dat u de benodigde netwerk infrastructuur hoeft in te stellen. 

|   |   |   |   |   | 
|---|---|---|---|---| 
| ![API-pictogram][biztalk-server-icon]<br/>**BizTalk**</br> **Server** | [![][file-system-icon]<br/>**Bestandssysteem</br>**  van API-pictogram][file-system-doc] | [![API-][ibm-db2-icon]<br/>pictogram**IBM DB2**][ibm-db2-doc] | [![API-][ibm-informix-icon]<br/>pictogram**IBM** </br> **Informix**][ibm-informix-doc] | ![API-pictogram][mysql-icon]<br/>**MySQL** | 
| [![API-][oracle-db-icon]<br/>pictogram**Oracle DB**][oracle-db-doc] | ![API-pictogram][postgre-sql-icon]<br/>**PostgreSQL** | [![API-][sharepoint-server-icon]<br/>pictogram **</br> share Point server**][sharepoint-server-doc] | [![API-][sql-server-icon]<br/>pictogram**SQL</br> server**][sql-server-doc] | ![API-pictogram][teradata-icon]<br/>**Teradata** | 
|||||

<a name="integration-account-connectors"></a>

## <a name="integration-account-connectors"></a>Integratieaccountconnectoren

Logic Apps biedt standaard connectors voor het bouwen van Business-to-Business (B2B)-oplossingen met uw Logic apps wanneer u een [integratie account](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md)maakt en betaalt dat beschikbaar is via de Enterprise Integration Pack (EIP) in Azure. Met dit account kunt u B2B-artefacten maken en opslaan, zoals handels partners, overeenkomsten, kaarten, schema's, certificaten, enzovoort. Als u deze artefacten wilt gebruiken, koppelt u uw Logic apps aan uw integratie account. Als u momenteel BizTalk Server gebruikt, zijn deze connectors mogelijk al bekend.

|   |   |   |   | 
|---|---|---|---| 
| [![API-][as2-icon]<br/>pictogram**AS2</br>**  -decodering][as2-doc] | [![API-][as2-icon]<br/>pictogram **</br> AS2-code ring**][as2-doc] | [![API-][edifact-icon]<br/>pictogram**EDIFACT</br>**  -decodering][edifact-decode-doc] | [![API-][edifact-icon]<br/>pictogram **</br> EDIFACT-code ring**][edifact-encode-doc] | 
| [![API-][flat-file-decode-icon]<br/>pictogram**platte</br> bestands codering**][flat-file-decode-doc] | [![API-][flat-file-encode-icon]<br/>pictogram**platte</br> bestands codering**][flat-file-encode-doc] | [![][integration-account-icon]<br/>**Integratieaccount<br/>** API-pictogram][integration-account-doc] | [![API-][liquid-icon]<br/>pictogram**liquide**</br>**trans formaties**][json-liquid-transform-doc] | 
| [![API-][x12-icon]<br/>pictogram**X12</br>**  -decodering][x12-decode-doc] | [![API-][x12-icon]<br/>pictogram **</br> X12-code ring**][x12-encode-doc] | [![API-][xml-transform-icon]<br/>pictogram**XML-** </br>**trans formaties**][xml-transform-doc] | [![API-][xml-validate-icon]<br/>pictogram **<br/>XML-validatie**][xml-validate-doc] |  
||||| 

<a name="enterprise-connectors"></a>

## <a name="enterprise-connectors"></a>Bedrijfsconnectoren

Logic Apps biedt deze zakelijke connectors voor toegang tot bedrijfs systemen, zoals SAP en IBM MQ:

|   |   |   | 
|---|---|---| 
| [![API-][ibm-3270-icon]<br/>pictogram**IBM 3270**][ibm-3270-doc] | [![API-][ibm-mq-icon]<br/>pictogram**IBM MQ**][ibm-mq-doc] | [![API-][sap-icon]<br/>pictogram**SAP**][sap-connector-doc] |
|||| 

<a name="triggers-actions"></a>

## <a name="triggers-and-actions---more-info"></a>Triggers en acties-meer informatie

Connectors kunnen *Triggers*, *acties*of beide bieden. Een *trigger* is de eerste stap in elke logische app, die meestal de gebeurtenis specificeert waarmee de trigger wordt geactiveerd en de logische app wordt uitgevoerd. De FTP-connector heeft bijvoorbeeld een trigger waarmee uw logische app wordt gestart wanneer een bestand wordt toegevoegd of gewijzigd. Sommige triggers controleren regel matig op de opgegeven gebeurtenis of gegevens en worden geactiveerd wanneer ze de opgegeven gebeurtenis of gegevens detecteren. Andere triggers wachten zich onmiddellijk wanneer een specifieke gebeurtenis plaatsvindt of wanneer er nieuwe gegevens beschikbaar zijn. Triggers geven ook de vereiste gegevens door aan uw logische app. Uw logische app kan deze gegevens lezen en gebruiken in de werk stroom.
De Twitter-connector heeft bijvoorbeeld een trigger, ' wanneer een nieuwe tweet wordt gepost ', waarmee de inhoud van de Tweet wordt door gegeven aan de werk stroom van uw logische app. 

Wanneer een trigger wordt geactiveerd, wordt door Azure Logic Apps een exemplaar van uw logische app gemaakt en worden de *acties* in de werk stroom van de logische app uitgevoerd. Acties zijn de stappen die de trigger volgen en taken uitvoeren in de werk stroom van uw logische app. U kunt bijvoorbeeld een logische app maken die klant gegevens ophaalt uit een SQL database en die gegevens in latere acties verwerken. 

Hier volgen de algemene soorten triggers die Azure Logic Apps biedt:

* *Trigger voor terugkeer patroon*: Deze trigger wordt uitgevoerd volgens een opgegeven schema en is niet nauw verbonden met een bepaalde service of systeem.

* *Polling trigger*: Deze trigger controleert regel matig een specifieke service of systeem op basis van het opgegeven schema, controleert op nieuwe gegevens of of er een specifieke gebeurtenis is opgetreden. Als er nieuwe gegevens beschikbaar zijn of als de specifieke gebeurtenis is opgetreden, maakt en voert de trigger een nieuw exemplaar van uw logische app uit. Dit kan nu de gegevens gebruiken die als invoer worden door gegeven.

* *Push-trigger*: Met deze trigger wordt geluisterd naar nieuwe gegevens of voor een gebeurtenis. Wanneer er nieuwe gegevens beschikbaar zijn of wanneer de gebeurtenis plaatsvindt, maakt en voert de trigger een nieuw exemplaar van uw logische app uit. Dit kan nu de gegevens gebruiken die als invoer worden door gegeven.

<a name="custom"></a>

## <a name="connector-configuration"></a>Connector configuratie

De triggers en acties van elke connector bieden hun eigen eigenschappen die u kunt configureren. Voor veel connectors moet u eerst een *verbinding* met de doel service of het systeem maken en verificatie referenties of andere configuratie gegevens opgeven voordat u een trigger of actie in uw logische app kunt gebruiken. U moet bijvoorbeeld een verbinding met een Twitter-account voor toegang tot gegevens toestaan of namens u posten. 

Voor connectors die gebruikmaken van OAuth, wordt bij het aanmelden bij de service, zoals Office 365, Sales Force of GitHub, uw toegangs token versleuteld en veilig opgeslagen in een Azure-geheim archief. Andere connectors, zoals FTP en SQL, vereisen een verbinding met configuratie details, zoals het server adres, de gebruikers naam en het wacht woord. Deze gegevens over de configuratie van de verbinding zijn ook versleuteld en veilig opgeslagen. 

Verbindingen hebben toegang tot de doel service of het systeem zolang de service of het systeem dit toestaat. Voor services die gebruikmaken van Azure Active Directory (AD) OAuth-verbindingen, zoals Office 365 en Dynamics, Azure Logic Apps de toegangs tokens voor onbepaalde tijd vernieuwd. Andere services hebben mogelijk beperkingen op hoe lang Azure Logic Apps een token kan gebruiken zonder te vernieuwen. Over het algemeen worden met sommige acties alle toegangs tokens ongeldig, zoals het wijzigen van uw wacht woord.

<a name="custom"></a>

## <a name="custom-apis-and-connectors"></a>Aangepaste Api's en connectors

Als u Api's wilt aanroepen waarmee aangepaste code wordt uitgevoerd of die niet beschikbaar zijn als connectors, kunt u het Logic Apps platform uitbreiden door [aangepaste API apps te maken](../logic-apps/logic-apps-create-api-app.md). U kunt ook [aangepaste connectors maken](../logic-apps/custom-connector-overview.md) *voor rest-* of SOAP-Api's, zodat deze api's beschikbaar zijn voor elke logische app in uw Azure-abonnement.
Als u aangepaste API Apps of connectors openbaar wilt maken zodat iedereen deze kan gebruiken in azure, kunt u [connectors voor micro soft-certificering verzenden](../logic-apps/custom-connector-submit-certification.md).

> [!NOTE]
> Logic apps binnen een [Integration service-omgeving (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) hebben rechtstreeks toegang tot resources in een virtueel Azure-netwerk.
> Als u aangepaste connectors hebt die de on-premises gegevens gateway nodig hebben en u deze connectors buiten een ISE hebt gemaakt, kunnen logische apps in een ISE ook deze connectors gebruiken.
>
> Aangepaste connectors die zijn gemaakt in een ISE werken niet met de on-premises gegevens gateway. Deze connectors hebben echter rechtstreeks toegang tot on-premises gegevens bronnen die zijn verbonden met een virtueel Azure-netwerk dat als host fungeert voor de ISE. Logic apps in een ISE hebben daarom waarschijnlijk niet de gegevens gateway nodig bij het communiceren met deze resources.
>
> Zie [verbinding maken met virtuele Azure-netwerken vanuit Azure Logic apps](../logic-apps/connect-virtual-network-vnet-isolated-environment.md#create-logic-apps-environment)voor meer informatie over het maken van ISEs.

## <a name="next-steps"></a>Volgende stappen

* De [volledige lijst](https://docs.microsoft.com/connectors) van de connectors zoeken
* [Een logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md)
* [Aangepaste connectors maken voor Logic apps](https://docs.microsoft.com/connectors/custom-connectors/)
* [Aangepaste API's maken voor logische apps](../logic-apps/logic-apps-create-api-app.md)

<!--Misc doc links-->
[gateway-doc]: ../logic-apps/logic-apps-gateway-connection.md "Verbinding maken met on-premises gegevensbronnen vanuit logische apps met on-premises gegevensgateway"

<!--Built-ins doc links-->
[azure-functions-doc]: ../logic-apps/logic-apps-azure-functions.md "Logische apps integreren met Azure Functions"
[azure-api-management-doc]: ../api-management/get-started-create-service-instance.md "Een Azure API Management service-exemplaar maken voor het beheren en publiceren van uw Api's"
[azure-app-services-doc]: ../logic-apps/logic-apps-custom-hosted-api.md "Logische apps integreren met App Service API Apps."
[azure-service-bus-doc]: ./connectors-create-api-servicebus.md "Berichten vanuit de Service Bus-wachtrijen en -onderwerpen verzenden en berichten vanuit de Service Bus-wachtrijen en abonnementen ontvangen"
[batch-doc]: ../logic-apps/logic-apps-batch-process-send-receive-messages.md "Berichten in groepen of als batches verwerken"
[condition-doc]: ../logic-apps/logic-apps-control-flow-conditional-statement.md "Een voor waarde evalueren en verschillende acties uitvoeren op basis van het feit of de voor waarde waar of onwaar is"
[delay-doc]: ./connectors-native-delay.md "Vertraagde acties uitvoeren"
[for-each-doc]: ../logic-apps/logic-apps-control-flow-loops.md#foreach-loop "Dezelfde acties uitvoeren op elk item in een matrix"
[http-doc]: ./connectors-native-http.md "HTTP-connector gebruiken voor HTTP-aanroepen"
[http-request-doc]: ./connectors-native-reqres.md "Acties voor HTTP-aanvragen en -antwoorden toevoegen aan uw logische apps"
[http-response-doc]: ./connectors-native-reqres.md "Acties voor HTTP-aanvragen en -antwoorden toevoegen aan uw logische apps"
[http-swagger-doc]: ./connectors-native-http-swagger.md "HTTP + Swagger-connector gebruiken voor HTTP-aanroepen"
[http-webook-doc]: ./connectors-native-webhook.md "HTTP-webhook-acties en triggers toevoegen aan uw Logic apps"
[nested-logic-app-doc]: ../logic-apps/logic-apps-http-endpoint.md "Logische apps integreren met geneste werkstromen"
[query-doc]: ./connectors-native-query.md "Matrices selecteren en filteren met behulp van de queryactie."
[recurrence-doc]:  ./connectors-native-recurrence.md "Terugkerende acties voor logische apps activeren"
[scope-doc]: ../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md "Acties in groepen indelen, die hun eigen status krijgen nadat de acties in de groep zijn uitgevoerd" 
[switch-doc]: ../logic-apps/logic-apps-control-flow-switch-statement.md "Organiseer acties in cases, waaraan unieke waarden worden toegewezen. Voer alleen de aanvraag uit waarvan de waarde overeenkomt met het resultaat van een expressie, object of token. Als er geen overeenkomsten bestaan, voert u de standaard case uit"
[terminate-doc]: ../logic-apps/logic-apps-workflow-actions-triggers.md#terminate-action "Een actieve werk stroom voor uw logische app stoppen of annuleren"
[until-doc]: ../logic-apps/logic-apps-control-flow-loops.md#until-loop "Acties herhalen totdat de opgegeven voor waarde waar is of een andere status is gewijzigd"
[data-operations-doc]: ../logic-apps/logic-apps-perform-data-operations.md "Gegevens bewerkingen uitvoeren zoals het filteren van matrices of het maken van CSV-en HTML-tabellen"
[variables-doc]: ../logic-apps/logic-apps-create-variables-store-values.md "Bewerkingen uitvoeren met variabelen, zoals initialiseren, instellen, verhogen, verlagen en toevoegen aan een teken reeks-of matrix variabele"

<!--Managed API doc links-->
[azure-blob-storage-doc]: ./connectors-create-api-azureblobstorage.md "Bestanden in uw blobcontainer beheren met Azure Blob Storage-container."
[azure-event-grid-doc]: ../event-grid/monitor-virtual-machine-changes-event-grid-logic-app.md "Gebeurtenissen bewaken die zijn gepubliceerd door een Event Grid, bijvoorbeeld wanneer Azure-resources of bronnen van derden worden gewijzigd"
[azure-event-hubs-doc]: ./connectors-create-api-azure-event-hubs.md "Verbinding maken met Azure Event Hubs. Gebeurtenissen tussen logische apps en Event Hubs verzenden en ontvangen"
[box-doc]: ./connectors-create-api-box.md "Verbinding maken met Box. Uw bestanden uploaden, ophalen, verwijderen, vermelden, en meer"
[dropbox-doc]: ./connectors-create-api-dropbox.md "Verbinding maken met Dropbox. Uw bestanden uploaden, ophalen, verwijderen, vermelden, en meer"
[dynamics-365-doc]: ./connectors-create-api-crmonline.md "Verbinding maken met Dynamics CRM Online zodat u kunt werken met CRM Online-gegevens."
[facebook-doc]: ./connectors-create-api-facebook.md "Verbinding maken met Facebook. Iets op een tijdlijn posten, een paginafeed ophalen, en meer"
[file-system-doc]: ../logic-apps/logic-apps-using-file-connector.md "Verbinding maken met een on-premises bestandssysteem"
[ftp-doc]: ./connectors-create-api-ftp.md "Verbinding maken met een FTP-/FTPS-server voor FTP-taken, zoals het uploaden, ophalen en verwijderen van bestanden, en meer."
[github-doc]: ./connectors-create-api-github.md "Verbinding maken met GitHub en problemen bijhouden"
[google-calendar-doc]: ./connectors-create-api-googlecalendar.md "Maakt verbinding met Google Calendar en kan een agenda beheren."
[google-drive-doc]: ./connectors-create-api-googledrive.md "Verbinding maken met GoogleDrive, zodat u met uw gegevens kunt werken"
[google-sheets-doc]: ./connectors-create-api-googlesheet.md "Verbinding maken met Google spread sheets, zodat u uw bladen kunt wijzigen"
[google-tasks-doc]: ./connectors-create-api-googletasks.md "Maakt verbinding met Google Taken, zodat u uw taken kunt beheren"
[ibm-3270-doc]: ./connectors-run-3270-apps-ibm-mainframe-create-api-3270.md "Verbinding maken met 3270-apps op IBM-mainframes"
[ibm-db2-doc]: ./connectors-create-api-db2.md "Verbinding maken met IBM DB2 in de cloud of on-premises. Een rij bijwerken, een tabel ophalen, en meer"
[ibm-informix-doc]: ./connectors-create-api-informix.md "Verbinding maken met Informix in de cloud of on-premises. Een rij lezen, de tabellen vermelden, en meer"
[ibm-mq-doc]: ./connectors-create-api-mq.md "Verbinding maken met IBM MQ on-premises of in azure om berichten te verzenden en te ontvangen"
[instagram-doc]: ./connectors-create-api-instagram.md "Verbinding maken met Instagram. Gebeurtenissen activeren of erop reageren"
[mailchimp-doc]: ./connectors-create-api-mailchimp.md "Verbinding maken met uw MailChimp-account. E-mails beheren en automatiseren"
[mandrill-doc]: ./connectors-create-api-mandrill.md "Verbinding maken met Mandrill voor communicatie"
[microsoft-translator-doc]: ./connectors-create-api-microsofttranslator.md "Verbinding maken met Microsoft Translator. Tekst vertalen, talen detecteren, en meer " 
[office-365-outlook-doc]: ./connectors-create-api-office365-outlook.md "Uw Office 365-account koppelen. E-mailberichten verzenden en ontvangen, uw agenda en contactpersonen beheren, en meer"
[office-365-users-doc]: ./connectors-create-api-office365-users.md 
[office-365-video-doc]: ./connectors-create-api-office365-video.md "Informatie over video's, videolijsten, videokanalen en URL's voor het afspelen van Office 365-video's ophalen"
[onedrive-doc]: ./connectors-create-api-onedrive.md "Verbinding maken met uw persoonlijke Microsoft OneDrive. Bestanden uploaden, verwijderen, vermelden, en meer"
[onedrive-for-business-doc]: ./connectors-create-api-onedriveforbusiness.md "Verbinding maken met uw zakelijke Microsoft OneDrive. Uw bestanden uploaden, verwijderen, vermelden, en meer"
[oracle-db-doc]: ./connectors-create-api-oracledatabase.md "Verbinding maken met een Oracle-database om rijen toe te voegen, in te voegen, te verwijderen en meer"
[outlook.com-doc]: ./connectors-create-api-outlook.md "Verbinding maken met uw Outlook-postvak. Uw e-mail, agenda's en contactpersonen beheren, en meer"
[project-online-doc]: ./connectors-create-api-projectonline.md "Verbinding maken met Microsoft Project Online. Uw projecten, taken, resources beheren, en meer"
[rss-doc]: ./connectors-create-api-rss.md "Feeditems publiceren en ophalen, bewerkingen activeren wanneer een nieuw item wordt gepubliceerd in een RSS-feed."
[salesforce-doc]: ./connectors-create-api-salesforce.md "Verbinding maken met uw Salesforce-account. Accounts, leads en verkoopkansen beheren, en meer"
[sap-connector-doc]: ../logic-apps/logic-apps-using-sap-connector.md "Verbinding maken met een on-premises SAP-systeem"
[sendgrid-doc]: ./connectors-create-api-sendgrid.md "Verbinding maken met SendGrid. E-mail verzenden en ontvangers lijsten beheren"
[sftp-doc]: ./connectors-create-api-sftp.md "Verbinding maken met uw SFTP-account. Bestanden uploaden, ophalen, verwijderen, en meer"
[sharepoint-server-doc]: ./connectors-create-api-sharepointserver.md "Verbinding maken met on-premises SharePoint-server. Documenten beheren, items vermelden, en meer"
[sharepoint-online-doc]: ./connectors-create-api-sharepointonline.md "Verbinding maken met SharePoint Online. Documenten beheren, items vermelden, en meer"
[slack-doc]: ./connectors-create-api-slack.md "Verbinding maken met Slack en berichten posten op Slack-kanalen"
[smtp-doc]: ./connectors-create-api-smtp.md "Verbinding maken met een SMTP-server en e-mail met bijlagen verzenden"
[sparkpost-doc]: ./connectors-create-api-sparkpost.md "Maakt verbinding met SparkPost voor communicatie"
[sql-server-doc]: ./connectors-create-api-sqlazure.md "Verbinding maken met Azure SQL Database of SQL Server. Vermeldingen in een SQL-databasetabel maken, bijwerken, ophalen en verwijderen."
[trello-doc]: ./connectors-create-api-trello.md "Verbinding maken met Trello. Uw projecten beheren en dingen organiseren met anderen"
[twilio-doc]: ./connectors-create-api-twilio.md "Verbinding maken met Twilio. Berichten verzenden en ophalen, beschikbare nummers ophalen, binnenkomende telefoonnummers beheren, en meer."
[twitter-doc]: ./connectors-create-api-twitter.md "Verbinding maken met Twitter. Tijdlijnen ophalen, tweets posten, en meer"
[wunderlist-doc]: ./connectors-create-api-wunderlist.md "Verbinding maken met Wunderlist. Uw taken en takenlijsten beheren, alles synchroniseren, en meer"
[yammer-doc]: ./connectors-create-api-yammer.md "Verbinding maken met Yammer. Berichten posten, nieuwe berichten ophalen, en meer"
[youtube-doc]: ./connectors-create-api-youtube.md "Verbinding maken met YouTube. Uw video's en kanalen beheren"

<!--Enterprise Intregation Pack doc links-->
[as2-doc]: ../logic-apps/logic-apps-enterprise-integration-as2.md "Meer informatie over Enterprise Integration AS2."
[edifact-decode-doc]: ../logic-apps/logic-apps-enterprise-integration-EDIFACT-decode.md "Meer informatie over EDIFACT-decodering in Enterprise Integration"
[edifact-encode-doc]: ../logic-apps/logic-apps-enterprise-integration-EDIFACT-encode.md "Meer informatie over EDIFACT-codering in Enterprise Integration"
[flat-file-decode-doc]:../logic-apps/logic-apps-enterprise-integration-flatfile.md "Meer informatie over Enterprise Integration met platte bestanden."
[flat-file-encode-doc]:../logic-apps/logic-apps-enterprise-integration-flatfile.md "Meer informatie over Enterprise Integration met platte bestanden."
[integration-account-doc]: ../logic-apps/logic-apps-enterprise-integration-metadata.md "Schema's, kaarten, partners en meer opzoeken in uw integratieaccount"
[json-liquid-transform-doc]: ../logic-apps/logic-apps-enterprise-integration-liquid-transform.md "Meer informatie over JSON-transformaties met Liquid"
[x12-doc]: ../logic-apps/logic-apps-enterprise-integration-x12.md "Meer informatie over Enterprise Integration X12"
[x12-decode-doc]: ../logic-apps/logic-apps-enterprise-integration-X12-decode.md "Meer informatie over X12-decodering in Enterprise Integration"
[x12-encode-doc]: ../logic-apps/logic-apps-enterprise-integration-X12-encode.md "Meer informatie over X12-codering in Enterprise Integration"
[xml-transform-doc]: ../logic-apps/logic-apps-enterprise-integration-transform.md "Meer informatie over Enterprise Integration-transformaties."
[xml-validate-doc]: ../logic-apps/logic-apps-enterprise-integration-xml-validation.md "Meer informatie over Enterprise Integration met XML-validatie."

<!-- Built-ins icons -->
[azure-api-management-icon]: ./media/apis-list/azure-api-management.png
[azure-app-services-icon]: ./media/apis-list/azure-app-services.png
[azure-functions-icon]: ./media/apis-list/azure-functions.png
[azure-logic-apps-icon]: ./media/apis-list/azure-logic-apps.png
[batch-icon]: ./media/apis-list/batch.png
[condition-icon]: ./media/apis-list/condition.png
[data-operations-icon]: ./media/apis-list/data-operations.png
[date-time-icon]: ./media/apis-list/date-time.png
[for-each-icon]: ./media/apis-list/for-each-loop.png
[http-icon]: ./media/apis-list/http.png
[http-request-icon]: ./media/apis-list/request.png
[http-response-icon]: ./media/apis-list/response.png
[http-swagger-icon]: ./media/apis-list/http-swagger.png
[http-webhook-icon]: ./media/apis-list/http-webhook.png
[schedule-icon]: ./media/apis-list/recurrence.png
[scope-icon]: ./media/apis-list/scope.png
[switch-icon]: ./media/apis-list/switch.png
[terminate-icon]: ./media/apis-list/terminate.png
[until-icon]: ./media/apis-list/until.png
[variables-icon]: ./media/apis-list/variables.png

<!--Managed API icons-->
[appfigures-icon]: ./media/apis-list/appfigures.png
[asana-icon]: ./media/apis-list/asana.png
[azure-automation-icon]: ./media/apis-list/azure-automation.png
[azure-blob-storage-icon]: ./media/apis-list/azure-blob-storage.png
[azure-cognitive-services-text-analytics-icon]: ./media/apis-list/azure-cognitive-services-text-analytics.png
[azure-data-lake-icon]: ./media/apis-list/azure-data-lake.png
[azure-document-db-icon]: ./media/apis-list/azure-document-db.png
[azure-event-grid-icon]: ./media/apis-list/azure-event-grid.png
[azure-event-grid-publish-icon]: ./media/apis-list/azure-event-grid-publish.png
[azure-event-hubs-icon]: ./media/apis-list/azure-event-hubs.png
[azure-ml-icon]: ./media/apis-list/azure-ml.png
[azure-queues-icon]: ./media/apis-list/azure-queues.png
[azure-resource-manager-icon]: ./media/apis-list/azure-resource-manager.png
[azure-service-bus-icon]: ./media/apis-list/azure-service-bus.png
[basecamp-3-icon]: ./media/apis-list/basecamp.png
[bitbucket-icon]: ./media/apis-list/bitbucket.png
[bitly-icon]: ./media/apis-list/bitly.png
[biztalk-server-icon]: ./media/apis-list/biztalk.png
[blogger-icon]: ./media/apis-list/blogger.png
[box-icon]: ./media/apis-list/box.png
[campfire-icon]: ./media/apis-list/campfire.png
[common-data-service-icon]: ./media/apis-list/common-data-service.png
[dropbox-icon]: ./media/apis-list/dropbox.png
[dynamics-365-icon]: ./media/apis-list/dynamics-crm-online.png
[dynamics-365-financials-icon]: ./media/apis-list/dynamics-365-financials.png
[dynamics-365-operations-icon]: ./media/apis-list/dynamics-365-operations.png
[easy-redmine-icon]: ./media/apis-list/easyredmine.png
[facebook-icon]: ./media/apis-list/facebook.png
[file-system-icon]: ./media/apis-list/file-system.png
[ftp-icon]: ./media/apis-list/ftp.png
[github-icon]: ./media/apis-list/github.png
[google-calendar-icon]: ./media/apis-list/google-calendar.png
[google-drive-icon]: ./media/apis-list/google-drive.png
[google-sheets-icon]: ./media/apis-list/google-sheet.png
[google-tasks-icon]: ./media/apis-list/google-tasks.png
[hipchat-icon]: ./media/apis-list/hipchat.png
[ibm-3270-icon]: ./media/apis-list/ibm-3270.png
[ibm-db2-icon]: ./media/apis-list/ibm-db2.png
[ibm-informix-icon]: ./media/apis-list/ibm-informix.png
[ibm-mq-icon]: ./media/apis-list/ibm-mq.png
[insightly-icon]: ./media/apis-list/insightly.png
[instagram-icon]: ./media/apis-list/instagram.png
[instapaper-icon]: ./media/apis-list/instapaper.png
[jira-icon]: ./media/apis-list/jira.png
[mailchimp-icon]: ./media/apis-list/mailchimp.png
[mandrill-icon]: ./media/apis-list/mandrill.png
[microsoft-translator-icon]: ./media/apis-list/microsoft-translator.png
[mysql-icon]: ./media/apis-list/mysql.png
[office-365-outlook-icon]: ./media/apis-list/office-365.png
[office-365-users-icon]: ./media/apis-list/office-365-users.png
[office-365-video-icon]: ./media/apis-list/office-365-video.png
[onedrive-icon]: ./media/apis-list/onedrive.png
[onedrive-for-business-icon]: ./media/apis-list/onedrive-business.png
[oracle-db-icon]: ./media/apis-list/oracle-db.png
[outlook.com-icon]: ./media/apis-list/outlook.png
[pagerduty-icon]: ./media/apis-list/pagerduty.png
[pinterest-icon]: ./media/apis-list/pinterest.png
[postgre-sql-icon]: ./media/apis-list/postgre-sql.png
[project-online-icon]: ./media/apis-list/projecton-line.png
[redmine-icon]: ./media/apis-list/redmine.png
[rss-icon]: ./media/apis-list/rss.png
[salesforce-icon]: ./media/apis-list/salesforce.png
[sap-icon]: ./media/apis-list/sap.png
[send-grid-icon]: ./media/apis-list/sendgrid.png
[sftp-icon]: ./media/apis-list/sftp.png
[sharepoint-online-icon]: ./media/apis-list/sharepoint-online.png
[sharepoint-server-icon]: ./media/apis-list/sharepoint-server.png
[slack-icon]: ./media/apis-list/slack.png
[smartsheet-icon]: ./media/apis-list/smartsheet.png
[smtp-icon]: ./media/apis-list/smtp.png
[sparkpost-icon]: ./media/apis-list/sparkpost.png
[sql-server-icon]: ./media/apis-list/sql.png
[teradata-icon]: ./media/apis-list/teradata.png
[todoist-icon]: ./media/apis-list/todoist.png
[trello-icon]: ./media/apis-list/trello.png
[twilio-icon]: ./media/apis-list/twilio.png
[twitter-icon]: ./media/apis-list/twitter.png
[vimeo-icon]: ./media/apis-list/vimeo.png
[visual-studio-team-services-icon]: ./media/apis-list/visual-studio-team-services.png
[wordpress-icon]: ./media/apis-list/wordpress.png
[wunderlist-icon]: ./media/apis-list/wunderlist.png
[yammer-icon]: ./media/apis-list/yammer.png
[youtube-icon]: ./media/apis-list/youtube.png

<!-- Enterprise Integration Pack icons -->
[as2-icon]: ./media/apis-list/as2.png
[edifact-icon]: ./media/apis-list/edifact.png
[flat-file-encode-icon]: ./media/apis-list/flat-file-encoding.png
[flat-file-decode-icon]: ./media/apis-list/flat-file-decoding.png
[integration-account-icon]: ./media/apis-list/integration-account.png
[liquid-icon]: ./media/apis-list/liquid-transform.png
[x12-icon]: ./media/apis-list/x12.png
[xml-validate-icon]: ./media/apis-list/xml-validation.png
[xml-transform-icon]: ./media/apis-list/xsl-transform.png
