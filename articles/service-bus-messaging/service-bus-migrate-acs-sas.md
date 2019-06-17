---
title: Migreren van Azure Active Directory Access Control Service naar de Shared Access Signature-autorisatie | Microsoft Docs
description: Toepassingen migreren van Access Control Service naar SAS
services: service-bus-messaging
documentationcenter: ''
author: axisc
manager: timlt
editor: spelluru
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/21/2018
ms.author: aschhab
ms.openlocfilehash: 746b19062c3014caa37c6668e6c41df054a47e25
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60868159"
---
# <a name="migrate-from-azure-active-directory-access-control-service-to-shared-access-signature-authorization"></a>Migreren van Azure Active Directory Access Control Service naar de Shared Access Signature-verificatie

Service Bus-toepassingen hebt eerder een keuze van het gebruik van twee verschillende autorisatie modellen: de [Shared Access Signature (SAS)](service-bus-sas.md) token modellen die worden geleverd door Service Bus en een federatieve waar het beheer van autorisatieregels binnen wordt beheerd door de [Azure Active Directory](/azure/active-directory/) Access Control Service (ACS) en tokens die zijn verkregen van ACS worden doorgegeven aan Service Bus voor het verlenen van toegang tot de gewenste functies.

De ACS-autorisatiemodel lang is vervangen door [SAS autorisatie](service-bus-authentication-and-authorization.md) als het beste model en alle documentatie, informatie en voorbeelden uitsluitend SAS momenteel gebruikt. Bovendien is het niet meer mogelijk te maken van nieuwe Service Bus-naamruimten die zijn gekoppeld aan ACS.

SAS heeft het voordeel dat deze niet onmiddellijk afhankelijk is van een andere service is, maar rechtstreeks vanuit een client zonder eventuele schakels kan worden gebruikt door de clienttoegang geven tot de SAS-regel en regel de sleutel. SAS kan ook eenvoudig worden geïntegreerd met een benadering waarin een client moet eerst een selectievakje autorisatie met een andere service doorgeven en vervolgens een token is uitgegeven. De laatste methode is vergelijkbaar met het gebruikspatroon van ACS, maar kan verlenende toegangstokens op basis van toepassingsspecifieke voorwaarden die moeilijk te drukken in ACS.

Voor alle bestaande toepassingen die afhankelijk van ACS zijn, adviseren we klanten om hun toepassingen te vertrouwen op SAS in plaats daarvan te migreren.

## <a name="migration-scenarios"></a>Migratiescenario 's

ACS en Service Bus zijn geïntegreerd in de gedeelde kennis van een *ondertekeningssleutel*. De ondertekeningssleutel wordt gebruikt door een ACS-naamruimte voor het ondertekenen van autorisatietokens en deze wordt gebruikt door Service Bus om te verifiëren dat het token is uitgegeven door de gekoppelde ACS-naamruimte. De ACS-naamruimte bevat service-identiteiten en autorisatieregels. De verificatieregels definiëren welke service-identiteit welke token dat is uitgegeven door een externe id-provider wordt opgehaald of ingesteld welk type toegang tot een deel van de Service Bus-naamruimte in een grafiek, in de vorm van een overeenkomst langste overeenkomende voorvoegsel.

Bijvoorbeeld, een ACS-regel verleent het **verzenden** claim op het padvoorvoegsel van het `/` aan een service-identiteit, wat betekent dat een token dat is uitgegeven door ACS op basis van de regel die de client rechten verleend om te verzenden naar alle entiteiten in de naamruimte. Als het padvoorvoegsel `/abc`, de identiteit is beperkt tot verzenden naar entiteiten met de naam `abc` of georganiseerd onder dit voorvoegsel. Ervan wordt uitgegaan dat lezers van deze hulp bij de migratie al bekend met deze concepten bent.

De migratiescenario's kunnen worden onderverdeeld in drie hoofdcategorieën worden onderverdeeld:

1.  **Standaardinstellingen ongewijzigd**. Sommige klanten gebruiken een [SharedSecretTokenProvider](/dotnet/api/microsoft.servicebus.sharedsecrettokenprovider) -object doorgeven van de automatisch gegenereerde **eigenaar** service-identiteit en de geheime sleutel voor de ACS-naamruimte, in combinatie met de Service Bus-naamruimte en voeg geen nieuwe regels.

2.  **Aangepaste service-identiteiten met eenvoudige regels**. Sommige klanten toevoegen van nieuwe service-identiteiten en elke nieuwe service-identiteit verlenen **verzenden**, **luisteren**, en **beheren** machtigingen voor een specifieke entiteit.

3.  **Aangepaste service-identiteiten met complexe regels**. Enkele klanten hebben complexe regel sets in welke extern uitgegeven tokens worden toegewezen aan rechten beschikken voor de Relay of waaraan een enkele service-identiteit is toegewezen allemaal een andere rechten voor verschillende paden van de naamruimte in meerdere regels.

Voor hulp bij de migratie van complexe regelsets, kunt u contact met [ondersteuning van Azure](https://azure.microsoft.com/support/options/). De andere twee scenario's kunnen eenvoudig migreren.

### <a name="unchanged-defaults"></a>Standaardinstellingen ongewijzigd

Als uw toepassing ACS standaardwaarden niet is gewijzigd, kunt u alle vervangen [SharedSecretTokenProvider](/dotnet/api/microsoft.servicebus.sharedsecrettokenprovider) gebruik met een [SharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.sharedaccesssignaturetokenprovider) object en de vooraf geconfigureerde naamruimte gebruiken **RootManageSharedAccessKey** in plaats van de ACS **eigenaar** account. Houd er rekening mee dat zelfs met de ACS **eigenaar** -account, deze configuratie is (en nog steeds wordt) in het algemeen niet aanbevolen, omdat dit account/regel volledige management-instantie ten opzichte van de naamruimte biedt, met inbegrip van de machtiging voor het verwijderen entiteiten.

### <a name="simple-rules"></a>Eenvoudige regels

Als u de toepassing aangepaste service-identiteiten met eenvoudige regels gebruikt, is de migratie is eenvoudig in het geval waarin een ACS-service-identiteit voor toegangsbeheer op een bepaalde wachtrij is gemaakt. In dit scenario is vaak het geval in oplossingen voor SaaS-stijl waarbij elke wachtrij wordt gebruikt als een brug en de tenantsite van een of filiaal en de service-identiteit is gemaakt voor de desbetreffende site. In dit geval wordt kan de betreffende service-identiteit worden gemigreerd naar een Shared Access Signature-regel, rechtstreeks in de wachtrij. De naam van de service-identiteit kan de naam van de SAS-regel en de sleutel van de service-identiteit kan worden de SAS-sleutel voor regel. De rechten van de SAS-regel worden vervolgens geconfigureerd is gelijk aan de respectievelijk toepasselijke ACS-regel voor de entiteit.

U kunt deze nieuwe en aanvullende configuratie van de SAS in-place op elke bestaande naamruimte die is gefedereerd met ACS maken en de migratie van ACS later wordt uitgevoerd met behulp van [SharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.sharedaccesssignaturetokenprovider) in plaats van [SharedSecretTokenProvider](/dotnet/api/microsoft.servicebus.sharedsecrettokenprovider). De naamruimte hoeft niet te worden ontkoppeld van ACS.

### <a name="complex-rules"></a>Complexe regels

SAS-regels zijn niet bedoeld om te worden van accounts, maar zijn met de naam ondertekenen sleutels die zijn gekoppeld met rechten. Als zodanig scenario's waarin de toepassing veel service-identiteiten maakt en deze toegang tot de rechten voor verschillende entiteiten of de hele naamruimte vereisen nog steeds een intermediair uitgifte van tokens. Vindt u richtlijnen voor dergelijke intermediaire door [contact opnemen met ondersteuning](https://azure.microsoft.com/support/options/).

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Service Bus-verificatie, de volgende onderwerpen:

* [Service Bus-verificatie en autorisatie](service-bus-authentication-and-authorization.md)
* [Service Bus-verificatie met Shared Access Signatures](service-bus-sas.md)

