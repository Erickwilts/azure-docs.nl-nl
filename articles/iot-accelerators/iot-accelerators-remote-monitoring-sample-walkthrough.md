---
title: Externe bewaking overzicht van de oplossingsversneller - Azure | Microsoft Docs
description: Een overzicht van de oplossingsverbetering voor externe controle.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 03/08/2019
ms.author: dobett
ms.openlocfilehash: af09ea39f373d518d5600e3fa46adc378fd9236d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "61442530"
---
# <a name="remote-monitoring-solution-accelerator-overview"></a>Overzicht van de verbetering voor de externe bewakingsoplossing

Externe bewaking [oplossingsverbetering](../iot-accelerators/about-iot-accelerators.md) een end-to-end oplossing voor prestatiecontrole voor meerdere machines geïmplementeerd op verafgelegen locaties bevinden. De oplossing combineert belangrijke Azure-services voor een algemene implementatie van het bedrijfsscenario. U kunt de oplossing gebruiken als uitgangspunt voor uw eigen implementatie en [aanpassen](../iot-accelerators/iot-accelerators-remote-monitoring-customize.md) te voldoen aan uw eigen specifieke zakelijke vereisten.

Dit artikel begeleidt u bij sommige van de belangrijkste elementen van de oplossing voor externe controle zodat u begrijpt hoe deze werkt. Deze kennis helpt u bij:

* Het oplossen van problemen met de oplossing.
* Het plannen van een aanpassing van de oplossing zodat deze voldoet aan uw eigen specifieke vereisten.
* Het ontwerpen van uw eigen IoT-oplossing die gebruikmaakt van Azure-services.

De Remote Monitoring solution accelerator code is beschikbaar op GitHub:

* [.NET](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet)
* [Java](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java)

## <a name="logical-architecture"></a>Logische architectuur

Het volgende diagram geeft een overzicht van de logische onderdelen van de oplossing voor externe controle accelerator overlay bekijken op de [IoT-architectuur](../iot-fundamentals/iot-introduction.md):

![Logische architectuur](./media/iot-accelerators-remote-monitoring-sample-walkthrough/remote-monitoring-architecture.png)

## <a name="why-microservices"></a>Waarom microservices?

Cloudarchitectuur heeft zich ontwikkeld sinds de eerste oplossingsversnellers de release van Microsoft. [Microservices](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/) hebben naar voren gekomen als een bewezen praktijken om schaal en flexibiliteit zonder dat dit ten koste gaat van de ontwikkelingssnelheid van de. Verschillende Microsoft-services gebruiken dit architectuurpatroon intern met geweldige betrouwbaarheid en schaalbaarheid resultaten. De bijgewerkte oplossingsversnellers plaats deze geleerde lessen in de praktijk, zodat u ook van deze profiteren kunt.

> [!TIP]
> Zie voor meer informatie over microservicearchitectuur [.NET Application Architecture](https://www.microsoft.com/net/learn/architecture) en [Microservices: Een toepassing revolution mogelijk gemaakt door de cloud](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/).

## <a name="device-connectivity"></a>Connectiviteit van apparaten

De oplossing omvat de volgende onderdelen in het gedeelte van de connectiviteit apparaat van de logische architectuur:

### <a name="real-devices"></a>Echte apparaten

U kunt echte apparaten verbinden met de oplossing. U kunt het gedrag van uw gesimuleerde apparaten met behulp van de Azure IoT device SDK's kunt implementeren.

U kunt echte apparaten vanuit het dashboard in de portal van de oplossing inrichten.

### <a name="device-simulation-microservice"></a>Apparaat simulatie microservice

De oplossing omvat de [apparaat simulatie microservice](https://github.com/Azure/remote-monitoring-services-dotnet/tree/master/device-simulation) waarmee u een groep gesimuleerde apparaten beheren vanuit de portal van de oplossing voor het testen van de end-to-end-stroom in de oplossing. De gesimuleerde apparaten:

* Apparaat-naar-cloud telemetrie genereren.
* Reageren op cloud-naar-apparaat methodeaanroepen van IoT-Hub.

De microservices biedt een RESTful-eindpunt kunt maken, starten en stoppen van simulaties. Elke simulatie bestaat uit een set virtuele apparaten van verschillende typen, die telemetrie verzenden en reageren op methodeaanroepen.

U kunt gesimuleerde apparaten vanuit het dashboard in de portal van de oplossing inrichten.

### <a name="iot-hub"></a>IoT Hub

De [IoT-hub](../iot-hub/index.yml) neemt telemetrie die van de reële en gesimuleerde apparaten worden verzonden naar de cloud. De IoT-hub maakt de telemetrie beschikbaar voor de services in de back-end oplossing voor IoT voor verwerking.

De IoT Hub in de oplossing doet ook het volgende:

* Een identiteitsregister waarin de id's en verificatiesleutels van de apparaten die verbinding mogen maken met de portal.
* Methoden op uw apparaten roept namens de solution accelerator.
* Onderhoudt apparaatdubbels voor alle geregistreerde apparaten. Een apparaatdubbel slaat de eigenschapswaarden op die door een apparaat worden gerapporteerd. Een apparaatdubbel slaat ook gewenste eigenschappen op die in de oplossingsportal zijn ingesteld, zodat het apparaat deze kan ophalen wanneer opnieuw verbinding wordt maakt.
* Plant taken voor het instellen van eigenschappen voor meerdere apparaten of voor het aanroepen van methoden op meerdere apparaten.

## <a name="data-processing-and-analytics"></a>Gegevensverwerking en -analyse

De oplossing omvat de volgende onderdelen in de gegevensverwerking en analytics deel uit van de logische architectuur:

### <a name="iot-hub-manager-microservice"></a>IoT Hub manager microservice

De oplossing omvat de [IoT-Hub manager microservice](https://github.com/Azure/remote-monitoring-services-dotnet/tree/master/iothub-manager) voor het afhandelen van interacties met uw IoT-hub, zoals:

* Het maken en beheren van IoT-apparaten.
* Dubbele apparaten beheren.
* Aanroepen van methoden op apparaten.
* Het beheren van IoT-referenties.

Deze service wordt ook uitgevoerd IoT Hub query's om op te halen die behoren tot de gebruiker gedefinieerde groepen apparaten.

De microservices biedt een RESTful-eindpunt voor het beheren van apparaten en apparaatdubbels, aanroepen van methoden en IoT Hub-query's uitvoeren.

### <a name="device-telemetry-microservice"></a>Apparaat telemetrie microservice

De [apparaat telemetrie microservice](https://github.com/Azure/remote-monitoring-services-dotnet/tree/master/device-telemetry) biedt een RESTful-eindpunt voor lezen-toegang tot telemetrie van apparaten die zijn opgeslagen in Time Series Insights. De RESTful-eindpunt kan ook CRUD-bewerkingen op regels en toegang voor lezen/schrijven voor definities van de waarschuwing uit de opslag.

### <a name="storage-adapter-microservice"></a>Opslag-adapter microservice

De [opslag adapter microservice](https://github.com/Azure/remote-monitoring-services-dotnet/tree/master/storage-adapter) beheert van sleutel / waarde-paren, waarbij de semantiek voor storage-service en presenteren van een eenvoudige interface voor het opslaan van gegevens van elke indeling met behulp van Azure Cosmos DB.

Waarden zijn ingedeeld in verzamelingen. U kunt werken op de afzonderlijke waarden of hele verzamelingen ophalen. Complexe gegevensstructuren worden geserialiseerd met de clients en beheerd als eenvoudige tekst-nettolading.

De service biedt een RESTful-eindpunt voor CRUD-bewerkingen op sleutel / waarde-paren. Waarden

### <a name="azure-cosmos-db"></a>Azure Cosmos DB

Solution accelerator implementaties gebruiken [Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/) voor het opslaan van regels, waarschuwingen, configuratie-instellingen en alle andere koude opslag.

### <a name="azure-stream-analytics-manager-microservice"></a>Azure Stream Analytics manager microservice

De [Azure Stream Analytics manager microservice](https://github.com/Azure/remote-monitoring-services-dotnet/tree/master/asa-manager) Azure Stream Analytics (ASA)-taken, inclusief het instellen van de configuratie hiervan, starten en stoppen en hun status controleren worden beheerd.

De ASA-taak wordt ondersteund door twee sets van verwijzingsgegevens. Definieert u regels voor één gegevensset en een apparaatgroepen definieert. De referentiegegevens regels is gegenereerd op basis van de informatie die wordt beheerd door het apparaat telemetrie-microservice. De Azure Stream Analytics manager microservice transformeert telemetrie regels naar logica voor de verwerking van stromen.

De referentiegegevens voor groepen van apparaten wordt gebruikt om te bepalen welke groep van regels toe te passen op een binnenkomende telemetrie-bericht. De groepen apparaten worden beheerd door de configuratie van microservice en gebruik Azure IoT Hub apparaatdubbel-query's.

De ASA-jobs ervoor zorgen dat de telemetrie van de verbonden apparaten Time Series Insights voor opslag en analyse.

### <a name="azure-stream-analytics"></a>Azure Stream Analytics

[Azure Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/) is een engine voor gebeurtenisverwerking waarmee u grote volumes aan gegevensstromen van apparaten onderzoeken.

### <a name="azure-time-series-insights"></a>Azure Time Series Insights

[Azure Time Series Insights](https://docs.microsoft.com/azure/time-series-insights/) winkels de telemetrie van de apparaten die zijn verbonden met de oplossingsversnellers. Ook kunt met visualiseren en query's telemetrie van apparaten in de web-UI van de oplossing.

> [!NOTE]
> Time Series Insights is momenteel niet beschikbaar in de Azure China-cloud. Nieuwe Remote Monitoring solution accelerator implementaties in de cloud met Azure China Cosmos DB gebruiken voor alle opslag.

### <a name="configuration-microservice"></a>Configuratie van microservice

De [configuratie microservice](https://github.com/Azure/remote-monitoring-services-dotnet/tree/master/config) biedt een RESTful-eindpunt voor CRUD-bewerkingen op apparaatgroepen, Oplossingsinstellingen en gebruikersinstellingen in de solution accelerator. Het werkt met de opslag-adapter microservice om vast te leggen van de configuratiegegevens.

### <a name="authentication-and-authorization-microservice"></a>Verificatie en autorisatie microservice

De [verificatie en autorisatie microservice](https://github.com/Azure/remote-monitoring-services-dotnet/tree/master/auth) beheert de gebruikers die zijn gemachtigd voor toegang tot de solution accelerator. Beheer van gebruikers kan worden gedaan met behulp van een identity-serviceprovider die ondersteuning biedt voor [OpenId Connect](https://openid.net/connect/).

### <a name="azure-active-directory"></a>Azure Active Directory

Solution accelerator implementaties gebruiken [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) als OpenID Connect-provider. Azure Active Directory gebruikersgegevens worden opgeslagen en vindt u certificaten voor het valideren van JWT-token handtekeningen.

## <a name="presentation"></a>Presentatie

De oplossing omvat de volgende onderdelen in het gedeelte van de presentatie van de logische architectuur:

De [online gebruikersinterface is een toepassing reageren Javascript](https://github.com/Azure/pcs-remote-monitoring-webui). De toepassing:

* Maakt alleen gebruik Javascript reageren en volledig in de browser wordt uitgevoerd.
* Is opgemaakt met CSS.
* Communiceert met openbare gerichte microservices via AJAX-aanroepen.

De gebruikersinterface geeft alle functionaliteit van de solution accelerator en communiceert met andere microservices, zoals:

* De verificatie en autorisatie microservice om gebruikersgegevens te beschermen.
* De IoT Hub manager microservice weergeven en beheren van de IoT-apparaten.

De gebruikersinterface kan worden geïntegreerd met de Azure Time Series Insights-Verkenner om in te schakelen van query's en analyse van de telemetrie van apparaten.

De configuratie van microservice kunt de gebruikersinterface voor het opslaan en ophalen van configuratie-instellingen.

## <a name="next-steps"></a>Volgende stappen

Als u de documentatie van de bron-code en ontwikkelaars verkennen wilt, begint u met een van de twee GitHub-opslagplaatsen:

* [De oplossingsversneller voor externe controle met Azure IoT (.NET)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet).
* [De oplossingsversneller voor externe controle met Azure IoT (Java)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java).

Gedetailleerde oplossing architectuurdiagrammen:
* [De oplossingsversneller voor externe controle architectuur](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Architecture).

Zie voor meer informatie over de oplossingsverbetering voor externe controle [de oplossingsversnellers aanpassen](../iot-accelerators/iot-accelerators-remote-monitoring-customize.md).
