---
title: Versleuteling-at-rest in Azure Cosmos DB
description: Meer informatie over hoe Azure Cosmos DB biedt versleuteling van data-at-rest en hoe deze is geïmplementeerd.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: sngun
ms.custom: seodec18
ms.openlocfilehash: b88b32b3ae2d4939d26fc268e25c87b1fb70eee5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66237229"
---
# <a name="data-encryption-in-azure-cosmos-db"></a>Versleuteling van gegevens in Azure Cosmos DB 

Versleuteling-at-rest is een woordgroep die vaak naar de versleuteling van gegevens op niet-vluchtig opslagapparaten, verwijst zoals Solid-State stations (SSD's) en harde schijven (HDD's). Cosmos DB slaat de bijbehorende primaire databases op SSD's. De mediabijlagen en back-ups worden opgeslagen in Azure Blob-opslag wordt gewoonlijk ondersteund door HDD. Met het uitbrengen van versleuteling-at-rest voor Cosmos DB, zijn alle databases, mediabijlagen en back-ups worden gecodeerd. Uw gegevens nu in transit versleuteld (via het netwerk) en at-rest (niet-vluchtig opslag), geeft u end-to-end versleuteling.

Als een PaaS-service, Cosmos DB heel eenvoudig te gebruiken is. Omdat alle gebruikersgegevens zijn opgeslagen in Cosmos DB is versleuteld in rust en transport, hebt u geen geen actie te ondernemen. Een andere manier om dit is dat versleuteling-at-rest 'op' standaard is. Er zijn geen besturingselementen om te schakelen in- of uitschakelen. Azure Cosmos DB maakt gebruik van AES-256-codering in alle regio's waar het account wordt uitgevoerd. We bieden deze functie terwijl we doorgaan om te voldoen aan onze [beschikbaarheid en prestaties van SLA's](https://azure.microsoft.com/support/legal/sla/cosmos-db).

## <a name="implementation-of-encryption-at-rest-for-azure-cosmos-db"></a>Implementatie van versleuteling-at-rest voor Azure Cosmos DB

Versleuteling-at-rest wordt geïmplementeerd met behulp van diverse beveiligingstechnologieën, met inbegrip van beveiligde opslag van clustersleutels systemen, versleutelde netwerken en cryptografische API's. Systemen die ontsleutelen en te verwerken gegevens heeft om te communiceren met systemen die sleutels beheren. Het diagram toont hoe de opslag van versleutelde gegevens en het beheer van de sleutels worden gescheiden. 

![Diagram ontwerpen](./media/database-encryption-at-rest/design-diagram.png)

De basisprincipes van een gebruikersaanvraag is als volgt:
- Het gebruikersaccount van de database gereed is gemaakt en storage-sleutels worden opgehaald via een aanvraag bij de Resourceprovider van de Management-Service.
- Een gebruiker maakt een verbinding met Cosmos DB via HTTPS/beveiligde transport. (Als de SDK's abstract de details.)
- De gebruiker verzendt een JSON-document via het eerder gemaakte beveiligde verbinding worden opgeslagen.
- Het JSON-document is geïndexeerd, tenzij de gebruiker heeft uitgeschakeld indexeren.
- Zowel de JSON-document en index de gegevens worden geschreven naar beveiligde opslag.
- Gegevens worden regelmatig lezen van de beveiligde opslag en back-ups op de Azure versleutelde Blob Store.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

### <a name="q-how-much-more-does-azure-storage-cost-if-storage-service-encryption-is-enabled"></a>V: Hoe veel meer kost Azure Storage als de Storage-Serviceversleuteling is ingeschakeld?
A: Er is geen extra kosten.

### <a name="q-who-manages-the-encryption-keys"></a>V: Wie de versleutelingssleutels beheert?
A: De sleutels worden beheerd door Microsoft.

### <a name="q-how-often-are-encryption-keys-rotated"></a>V: Hoe vaak worden versleutelingssleutels gedraaid?
A: Microsoft heeft een set van interne richtlijnen voor codering sleutelroulatie, waarbij Cosmos DB. De specifieke richtlijnen zijn niet gepubliceerd. Microsoft publiceren de [Security Development Lifecycle (SDL)](https://www.microsoft.com/sdl/default.aspx), die wordt gezien als een subset van de interne richtlijnen en aanbevolen procedures voor nuttig is voor ontwikkelaars.

### <a name="q-can-i-use-my-own-encryption-keys"></a>V: Kan ik mijn eigen versleutelingssleutels gebruiken?
A: Cosmos DB is een PaaS-service, en we hard gewerkt om u te houden van de service eenvoudig te gebruiken. Ons opgevallen dat deze vraag is vaak gevraagd als een proxy-vraag voor het voldoen aan een nalevingsvereiste zoals PCI-DSS. We hebben als onderdeel van het bouwen van deze functie, met naleving auditors om ervoor te zorgen dat klanten die gebruikmaken van Cosmos DB aan hun eisen voldoen zonder de noodzaak om te sleutels zelf beheren.

### <a name="q-what-regions-have-encryption-turned-on"></a>V: Welke regio's hebben versleuteling ingeschakeld?
A: Alle regio's van Azure Cosmos DB hebt versleuteling is ingeschakeld voor alle gebruikersgegevens.

### <a name="q-does-encryption-affect-the-performance-latency-and-throughput-slas"></a>V: Versleuteling van invloed zijn op de prestaties van latentie en doorvoer Sla's?
A: Er is geen impact of de wijzigingen op de prestaties van SLA's nu dat versleuteling-at-rest wordt ingeschakeld voor alle bestaande en nieuwe accounts. U kunt meer lezen over de [SLA voor Cosmos DB](https://azure.microsoft.com/support/legal/sla/cosmos-db) pagina om te bekijken van de meest recente garanties.

### <a name="q-does-the-local-emulator-support-encryption-at-rest"></a>V: Ondersteunt de lokale emulator versleuteling-at-rest?
A: De emulator is een hulpprogramma voor het ontwikkelen en testen van zelfstandige en gebruikt niet de key management-services die de beheerde Cosmos DB-service gebruikt. Onze aanbeveling is het inschakelen van BitLocker op stations waar u gevoelige emulator testgegevens opslaat. De [emulator biedt ondersteuning voor het wijzigen van de standaardgegevensmap](local-emulator.md) of via een bekende locatie.

## <a name="next-steps"></a>Volgende stappen

Zie voor een overzicht van Cosmos DB-beveiliging en de meest recente verbeteringen [beveiliging van Azure Cosmos DB-database](database-security.md).
Zie voor meer informatie over Microsoft-certificeringen, de [Azure Trust Center](https://azure.microsoft.com/support/trust-center/).
