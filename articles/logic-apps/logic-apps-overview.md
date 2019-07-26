---
title: Enter prise Integration met Azure Logic Apps
description: Overzicht van het bouwen van oplossingen voor bedrijfs integratie door taken, werk stromen en bedrijfs processen te automatiseren en te organiseren waarmee apps, gegevens, services en systemen in ondernemingen en organisaties worden geïntegreerd. Maak oplossingen voor gegevensintegratie, systeemintegratie, Enterprise Application Integration (EAI) en indelingsscenario's.
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
manager: carmonm
ms.reviewer: klam, LADocs
ms.topic: overview
ms.custom: mvc
ms.date: 6/29/2018
ms.openlocfilehash: f25ade0e984c98b9cbc8c4efa93f300c3ed93b14
ms.sourcegitcommit: 04ec7b5fa7a92a4eb72fca6c6cb617be35d30d0c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/22/2019
ms.locfileid: "68385482"
---
# <a name="what-is-azure-logic-apps"></a>Wat is Azure Logic Apps?

[Azure Logic apps](https://azure.microsoft.com/services/logic-apps) is een Cloud service die u helpt bij het plannen, automatiseren en organiseren van taken, bedrijfs processen en [werk stromen](#logic-app-concepts) wanneer u apps, gegevens, systemen en services in ondernemingen of organisaties wilt integreren. Logic Apps vereenvoudigt het ontwerpen en maken van schaalbare oplossingen voor [app-integratie](https://azure.microsoft.com/product-categories/integration/), gegevensintegratie, systeemintegratie, Enterprise Application Integration (EAI) en communicatie voor business-to-business (B2B), in de cloud, on-premises of beide.

Dit zijn enkele voorbeelden van workloads die u met logische apps kunt automatiseren:

* Orders verwerken en rondsturen in cloudservices en on-premises systemen.
* E-mailmeldingen verzenden met Office 365 wanneer gebeurtenissen plaatsvinden in verschillende systemen, apps en services.
* Geüploade bestanden verplaatsen van een SFTP- of FTP-server naar Azure Storage. 
* Tweets bewaken voor een bepaald onderwerp, de feel analyseren en waarschuwingen of taken maken voor items die nagekeken moeten worden.

Als u oplossingen voor bedrijfs integratie wilt bouwen met Azure Logic Apps, kunt u kiezen uit een groeiende galerie met honderden kant-en- [klare connectors](../connectors/apis-list.md), waaronder services zoals Azure service bus, functies en opslag. SQL, Office 365, Dynamics, Sales Force, BizTalk, SAP, Oracle DB, bestands shares en meer. [Connectors](#logic-app-concepts) bieden [triggers](#logic-app-concepts), [acties](#logic-app-concepts) of beide om logische apps te maken voor veilige toegang en verwerking van gegevens in realtime.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Introducing-Azure-Logic-Apps/player]

## <a name="how-does-logic-apps-work"></a>Hoe werkt Logic Apps? 

Iedere werkstroom van logische apps begint met een trigger, die wordt geactiveerd wanneer een bepaalde gebeurtenis wordt uitgevoerd of wanneer nieuwe beschikbare gegevens aan bepaalde criteria voldoen. Veel triggers die door de connectors in Logic Apps zijn opgegeven, bevatten elementaire plannings mogelijkheden, zodat u kunt instellen hoe vaak uw workloads worden uitgevoerd. Voor complexere planningen of geavanceerde terugkeer patronen kunt u een terugkeer patroon trigger gebruiken als de eerste stap in een werk stroom. Meer informatie over [werk stromen op basis van een planning](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md).

Telkens wanneer de trigger wordt geactiveerd, maakt de Logic Apps-engine een exemplaar van een logische app waarmee de acties in de werkstroom worden uitgevoerd. Deze acties kunnen ook bestaan uit gegevensconversies en datatransportbesturing, zoals voorwaardelijke instructies, switch-instructies, lussen en vertakking. Deze logische app begint bijvoorbeeld met een Dynamics 365-trigger met 'Wanneer een record wordt bijgewerkt' als ingebouwd criterium. Als de trigger een gebeurtenis detecteert die aan dit criterium voldoet, wordt de trigger geactiveerd, die vervolgens de acties van de werkstroom uitvoert. Hier bestaan de acties uit XML-transformatie, gegevensupdates, vertakking van beslissingen en e-mailmeldingen.

![Ontwerper van logische apps - voorbeeld van logische app](./media/logic-apps-overview/overview.png)

U kunt uw logische apps visueel compileren met Ontwerper van logische apps, een functie die beschikbaar is op Azure Portal via uw browser en in Visual Studio. Voor meer aangepaste logische apps kunt u definities voor logische apps maken of bewerken in JavaScript Object Notation (JSON) door in de editor 'codeweergave' te werken. U kunt voor bepaalde taken ook Azure PowerShell-opdrachten en Azure Resource Manager-sjablonen gebruiken. Logische apps worden geïmplementeerd en uitgevoerd in de cloud op Azure. Bekijk deze video voor een meer uitvoerige inleiding: [Use Azure Enterprise Integration Services to run cloud apps at scale](https://channel9.msdn.com/Events/Connect/2017/T119/)

## <a name="why-use-logic-apps"></a>Waarom Logic Apps?

Nu bedrijven steeds meer opschuiven naar digitalisering, kunnen logische apps helpen om oudere en moderne en geavanceerde systemen gemakkelijk en snel met elkaar te verbinden met behulp van vooraf gedefinieerde API's als door Microsoft beheerde connectors. Op die manier kunt u zich richten op de bedrijfslogica en functionaliteit van uw apps. U hoeft zich geen zorgen te maken over het compileren, hosten, schalen, beheren, onderhouden en controleren van uw apps. Logic Apps werkt dat allemaal voor u af. Bovendien betaalt u alleen voor wat u gebruikt op basis van een [prijsmodel](../logic-apps/logic-apps-pricing.md). 

In veel gevallen hoeft u ook geen code te schrijven. Maar als u toch wat code moet schrijven, kunt u codefragmenten maken met [Azure Functions](../azure-functions/functions-overview.md) en die code op aanvraag uitvoeren vanuit logische apps. En als uw logische apps moeten communiceren met gebeurtenissen uit Azure-services, aangepaste apps of andere oplossingen, kunt u [Azure Event Grid](../event-grid/overview.md) gebruiken in combinatie met uw logische apps voor het controleren, routeren en publiceren van gebeurtenissen.

Logic Apps, Functions en Event Grid worden volledig beheerd door Microsoft Azure, zodat u zich geen zorgen hoeft te maken over het compileren, hosten, schalen, beheren, controleren en onderhouden van uw oplossingen. Omdat u [apps en oplossingen 'zonder server’](../logic-apps/logic-apps-serverless-overview.md) kunt maken, hoeft u zich alleen bezig te houden met de bedrijfslogica. Deze services worden automatisch geschaald om aan uw behoeften te voldoen, integraties te versnellen en u te helpen bij het maken van robuuste cloud-apps met minimale code. Bovendien betaalt u alleen voor wat u gebruikt op basis van een [prijsmodel](../logic-apps/logic-apps-pricing.md). 

Lees deze [klantervaringen](https://aka.ms/logic-apps-customer-stories) als u wilt weten hoe bedrijven hun flexibiliteit verbeterden en zich meer wisten te richten op hun kernactiviteiten door Logic Apps te combineren met andere Azure-services en Microsoft-producten.

Hier vindt u meer informatie over de mogelijkheden en voordelen van Logic Apps:

### <a name="visually-build-workflows-with-easy-to-use-tools"></a>Werkstromen visueel samenstellen met eenvoudig te gebruiken hulpprogramma's

Bespaar tijd en vereenvoudig complexe processen met hulpprogramma's voor visueel ontwerp. Maak logische apps van begin tot eind met behulp van Ontwerper van logische apps via uw browser in Azure Portal of in Visual Studio. Begin uw werkstroom met een trigger en voeg een willekeurig aantal acties toe vanuit de [galerie met connectors](../connectors/apis-list.md).

### <a name="get-started-faster-with-logic-app-templates"></a>Sneller aan de slag met sjablonen voor logische apps

Stel veelgebruikte oplossingen sneller samen met vooraf gedefinieerde werkstromen uit de [sjablonengalerie](../logic-apps/logic-apps-create-logic-apps-from-templates.md). Sjablonen variëren van eenvoudige connectiviteit voor SaaS-apps (software as a service) tot geavanceerde B2B-oplossingen. Maar daarnaast zijn er ook sjablonen gewoon voor het plezier. Informatie over [het maken van logische apps met vooraf gedefinieerde sjablonen](../logic-apps/logic-apps-create-logic-apps-from-templates.md).

### <a name="connect-disparate-systems-across-different-environments"></a>Ongelijksoortige systemen in verschillende omgevingen met elkaar verbinden

Sommige patronen en werkstromen zijn gemakkelijk te beschrijven, maar moeilijk in code te implementeren. Met logische apps kunt u naadloos verbinding maken met ongelijksoortige systemen in on-premises en cloud-omgevingen. U kunt bijvoorbeeld een marketingoplossing in de cloud verbinden met een on-premises factureringssysteem of een berichtenservice via API's en systemen met een Enterprise Service Bus centraliseren. Logische apps bieden een snelle, betrouwbare en consistente manier om herbruikbare en herconfigureerbare oplossingen voor deze scenario's te leveren.

### <a name="first-class-support-for-enterprise-integration-and-b2b-scenarios"></a>Eersteklas ondersteuning voor bedrijfsintegratie en B2B-scenario's

Bedrijven en organisaties communiceren elektronisch met elkaar met behulp van gestandaardiseerde maar verschillende berichtprotocollen en -indelingen, zoals EDIFACT, AS2 en X12. Met de functies in het [Enterprise Integration Pack (EIP)](../logic-apps/logic-apps-enterprise-integration-overview.md) kunt u logische apps bouwen voor het omzetten van berichtindelingen in gebruik bij uw partners naar indelingen die de systemen van uw organisatie kunnen interpreteren en verwerken. Met Logic Apps worden deze uitwisselingen soepel en veilig verwerkt met versleuteling en digitale handtekeningen.

Begin klein met uw huidige systemen en services en breid dan in uw eigen tempo verder uit. Wanneer u klaar bent, helpen Logic Apps en de EIP u bij het implementeren van en omhoog schalen naar meer volwassen integratiescenario's door onder andere de volgende mogelijkheden te bieden:

* Ga uit van deze producten en services:

  * [Microsoft BizTalk Server](https://docs.microsoft.com/biztalk/core/introducing-biztalk-server)
  * [Azure Service Bus](../service-bus-messaging/service-bus-messaging-overview.md)
  * [Azure Functions](../azure-functions/functions-overview.md)
  * [Azure API Management](../api-management/api-management-key-concepts.md)

* [XML-berichten](../logic-apps/logic-apps-enterprise-integration-xml.md) verwerken
* [platte bestanden](../logic-apps/logic-apps-enterprise-integration-flatfile.md) verwerken
* Berichten uitwisselen met [EDIFACT](../logic-apps/logic-apps-enterprise-integration-edifact.md)-, [AS2](../logic-apps/logic-apps-enterprise-integration-as2.md)- en [X12](../logic-apps/logic-apps-enterprise-integration-x12.md)-protocollen
* Gebruik [integratieaccounts](../logic-apps/logic-apps-enterprise-integration-accounts.md) om deze B2B-artefacten en meer in één locatie op te slaan en te beheren:

  * [Partners](../logic-apps/logic-apps-enterprise-integration-partners.md)
  * [Overeenkomsten](../logic-apps/logic-apps-enterprise-integration-agreements.md) 
  * [Toewijzingen voor XML-transformatie](../logic-apps/logic-apps-enterprise-integration-maps.md)
  * [XML-validatieschema’s](../logic-apps/logic-apps-enterprise-integration-schemas.md)
   
Als u bijvoorbeeld Microsoft BizTalk Server gebruikt, kunnen logische apps communiceren met de BizTalk-server met behulp van de [BizTalk Server-connector](../connectors/apis-list.md#on-premises-connectors). U kunt vervolgens BizTalk-gerelateerde bewerkingen uitbreiden of uitvoeren in uw logische apps door [integratieaccountconnectors](../connectors/apis-list.md#integration-account-connectors) op te nemen. Deze zijn beschikbaar in het Enterprise Integration Pack. 

Andersom kan BizTalk Server ook verbinding maken en communiceren met logische apps met behulp van de [Microsoft BizTalk Server-adapter voor Logic Apps](https://www.microsoft.com/download/details.aspx?id=54287). Meer informatie over het [instellen en gebruiken van de BizTalk Server-adapter](https://docs.microsoft.com/biztalk/core/logic-app-adapter) in BizTalk Server.

### <a name="write-once-reuse-often"></a>Eenmaal schrijven, vaak opnieuw gebruiken

Maak uw logische apps als Azure Resource Manager sjablonen, zodat u de [implementatie van logische](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md) apps in meerdere omgevingen en regio's kunt automatiseren.

### <a name="built-in-extensibility"></a>Ingebouwde uitbreidbaarheid

Als u de connector om de aangepaste code uit te voeren niet kunt vinden, kunt u logische apps uitbreiden door uw eigen codefragmenten op aanvraag te maken en aan te roepen via [Azure Functions](../azure-functions/functions-overview.md). Maak uw eigen [API's](../logic-apps/logic-apps-create-api-app.md) en [aangepaste connectors](../logic-apps/custom-connector-overview.md) die u kunt aanroepen vanuit logic apps.

### <a name="pay-only-for-what-you-use"></a>Alleen betalen voor wat u gebruikt
  
Logic Apps gebruikt [prijzen en metingen](../logic-apps/logic-apps-pricing.md) op basis van verbruik tenzij u eerder logische apps hebt gemaakt met App Service-plannen.

Meer informatie over Logic Apps vindt u in deze inleidende video's:

* [Integratie met Logic Apps - Van beginner tot expert](https://channel9.msdn.com/Events/Build/2017/C9R17)
* [Bedrijfsintegratie met Microsoft Azure Logic Apps](https://channel9.msdn.com/Events/Ignite/Microsoft-Ignite-Orlando-2017/BRK2188)
* [Geavanceerde bedrijfsprocessen samenstellen Met Logic Apps](https://channel9.msdn.com/Events/Ignite/Microsoft-Ignite-Orlando-2017/BRK3179)

<a name="logic-app-concepts"></a>

## <a name="key-terms"></a>Belangrijkste termen

* **Werkstroom**: het visualiseren, ontwerpen, compileren, automatiseren en implementeren van bedrijfsprocessen als een reeks stappen.

* **Beheerde connectors**: uw logische apps hebben toegang tot gegevens, services en systemen nodig. U kunt vooraf gedefinieerde, door Microsoft beheerde connectors gebruiken die zijn ontworpen om verbinding te maken met uw gegevens en om deze te openen en ermee te werken. Zie [Connectors voor Azure Logic Apps](../connectors/apis-list.md)

* **Triggers**: veel door Microsoft beheerde connectors bieden triggers die worden geactiveerd wanneer gebeurtenissen of nieuwe gegevens voldoen aan opgegeven voorwaarden. Een gebeurtenis kan bijvoorbeeld een e-mailbericht ontvangen of wijzigingen detecteren in uw Azure Storage-account. Telkens wanneer de trigger wordt geactiveerd, maakt de Logic Apps-engine een nieuw exemplaar van een logische app waarmee de werkstroom wordt gestart.

* **Acties**: acties zijn alle stappen die na de trigger plaatsvinden. Elke actie verwijst doorgaans naar een bewerking die wordt gedefinieerd door een beheerde connector, aangepaste API of aangepaste connector.

* **Enterprise Integration Pack**: voor meer geavanceerde integratiescenario's bevat Logic Apps mogelijkheden van Biztalk Server. De Enterprise Integration Pack bevat connectors waarmee logische apps eenvoudig validaties, transformaties en andere acties kunnen uitvoeren.

## <a name="how-does-logic-apps-differ-from-functions-webjobs-and-flow"></a>Hoe verschilt Logic Apps van Functions, WebJobs en Flow?

Met al deze services kunt u ongelijksoortige systemen aan elkaar koppelen. Omdat elke service zijn eigen voordelen heeft, is het combineren van hun verschillende mogelijkheden de beste manier om snel een schaalbaar en compleet integratiesysteem samen te stellen. Zie [Kiezen tussen Flow, Logic Apps, Functions en WebJobs](../azure-functions/functions-compare-logic-apps-ms-flow-webjobs.md) voor meer informatie.

## <a name="get-started"></a>Aan de slag 

Logic Apps is een van de vele services die worden gehost op Microsoft Azure. Voordat u kunt beginnen, hebt u een Azure-abonnement nodig. Als u nog geen abonnement hebt, [meld u dan aan voor een gratis Azure-account](https://azure.microsoft.com/free/). 

Als u wel een Azure-abonnement hebt, kunt u deze [quickstart doornemen voor het maken van uw eerste logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md), die via een RSS-feed controleert op nieuwe inhoud op een website en een e-mail stuurt wanneer er nieuwe inhoud is.

## <a name="next-steps"></a>Volgende stappen

* [Verkeer controleren met een logische app op basis van een planning](../logic-apps/tutorial-build-schedule-recurring-logic-app-workflow.md)
* Meer informatie over [oplossingen zonder server met Azure](../logic-apps/logic-apps-serverless-overview.md)
* Meer informatie over [B2B-integratie met het Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md)
