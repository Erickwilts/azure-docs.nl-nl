---
title: Containers configureren
titlesuffix: Text Analytics - Azure Cognitive Services
description: Text Analytics biedt elke container met een gemeenschappelijk framework van de configuratie, zodat u eenvoudig kunt configureren en beheren van instellingen voor opslag, logboekregistratie en Telemetrie en beveiliging voor uw containers.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: diberry
ms.openlocfilehash: 1333aefc145e95223624f42a28ec0bb31ab70065
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60828119"
---
# <a name="configure-text-analytics-docker-containers"></a>Configureren van de Text Analytics docker-containers

Text Analytics biedt elke container met een gemeenschappelijk framework van de configuratie, zodat u eenvoudig kunt configureren en beheren van instellingen voor opslag, logboekregistratie en Telemetrie en beveiliging voor uw containers.

## <a name="configuration-settings"></a>Configuratie-instellingen

[!INCLUDE [Container shared configuration settings table](../../../includes/cognitive-services-containers-configuration-shared-settings-table.md)]

> [!IMPORTANT]
> De [ `ApiKey` ](#apikey-configuration-setting), [ `Billing` ](#billing-configuration-setting), en [ `Eula` ](#eula-setting) instellingen samen worden gebruikt en u moet geldige waarden opgeven voor alle drie deze, anders uw container start niet. Zie voor meer informatie over het gebruik van deze configuratie-instellingen voor het starten van een container [facturering](how-tos/text-analytics-how-to-install-containers.md#billing).

## <a name="apikey-configuration-setting"></a>ApiKey configuratie-instelling

De `ApiKey` instelling geeft u aan de Azure-resource-sleutel die wordt gebruikt voor het bijhouden van informatie over facturering voor de container. U moet een waarde opgeven voor de ApiKey en de waarde moet een geldige sleutel voor de _Cognitive Services_ resource die is opgegeven voor de [ `Billing` ](#billing-configuration-setting) configuratie-instelling.

Deze instelling kan worden gevonden in de volgende plaats:

* Azure Portal: **Cognitive Services** resourcebeheer onder **sleutels**

## <a name="applicationinsights-setting"></a>Application Insights-instelling

[!INCLUDE [Container shared configuration ApplicationInsights settings](../../../includes/cognitive-services-containers-configuration-shared-settings-application-insights.md)]

## <a name="billing-configuration-setting"></a>Facturering van configuratie-instelling

De `Billing` instelling geeft u aan de URI van het eindpunt van de _Cognitive Services_ resource in Azure gebruikt voor het meten van factureringsgegevens voor de container. U moet een waarde voor deze configuratie-instelling opgeven en de waarde moet een geldige URI van het eindpunt voor een __Cognitive Services_ resource in Azure. Gebruik de container rapporteert over elke 10 tot 15 minuten.

Deze instelling kan worden gevonden in de volgende plaats:

* Azure Portal: **Cognitive Services** overzicht, met het label `Endpoint`

U moet toevoegen de `text/analytics/v2.0` routering naar de URI van het eindpunt, zoals wordt weergegeven in het volgende BILLING_ENDPOINT_URI-voorbeeld.

|Vereist| Name | Gegevenstype | Description |
|--|------|-----------|-------------|
|Ja| `Billing` | Reeks | URI van de facturering-eindpunt<br><br>Voorbeeld:<br>`Billing=https://westus.api.cognitive.microsoft.com/text/analytics/v2.1` |

## <a name="eula-setting"></a>Overeenkomst instelling

[!INCLUDE [Container shared configuration eula settings](../../../includes/cognitive-services-containers-configuration-shared-settings-eula.md)]

## <a name="fluentd-settings"></a>Fluentd-instellingen

[!INCLUDE [Container shared configuration fluentd settings](../../../includes/cognitive-services-containers-configuration-shared-settings-fluentd.md)]

## <a name="http-proxy-credentials-settings"></a>HTTP-proxy-instellingen voor referenties

[!INCLUDE [Container shared configuration fluentd settings](../../../includes/cognitive-services-containers-configuration-shared-settings-http-proxy.md)]

## <a name="logging-settings"></a>Instellingen voor logboekregistratie
 
[!INCLUDE [Container shared configuration logging settings](../../../includes/cognitive-services-containers-configuration-shared-settings-logging.md)]

## <a name="mount-settings"></a>Instellingen voor koppelen

Gebruik bind koppelt om te lezen en schrijven van gegevens naar en van de container. U kunt opgeven van een koppelpunt invoer of uitvoer koppelen door op te geven de `--mount` optie in de [docker uitvoeren](https://docs.docker.com/engine/reference/commandline/run/) opdracht.

De Text Analytics-containers gebruik geen invoer of uitvoer koppelt training of service-gegevens op te slaan. 

De exacte syntaxis van de locatie van de host koppelen, is afhankelijk van het hostbesturingssysteem. Bovendien de [hostcomputer](how-tos/text-analytics-how-to-install-containers.md#the-host-computer)van koppelpunten locatie is mogelijk niet toegankelijk is vanwege een conflict tussen de machtigingen die wordt gebruikt door de docker-service-account en de host koppelen locatie machtigingen. 

|Optioneel| Name | Gegevenstype | Description |
|-------|------|-----------|-------------|
|Niet toegestaan| `Input` | String | Text Analytics-containers gebruik dit niet.|
|Optioneel| `Output` | String | Het doel van de uitvoer-koppelpunt. De standaardwaarde is `/output`. Dit is de locatie van de logboeken. Dit omvat de logboeken voor containers. <br><br>Voorbeeld:<br>`--mount type=bind,src=c:\output,target=/output`|

## <a name="example-docker-run-commands"></a>Voorbeeld van de docker-opdrachten uitvoeren 

De volgende voorbeelden gebruiken de configuratie-instellingen om te laten zien hoe u om te schrijven en gebruik `docker run` opdrachten.  Zodra actief is, de container blijft actief totdat u [stoppen](how-tos/text-analytics-how-to-install-containers.md#stop-the-container) deze.

* **Voortzetting van regel tekens**: De docker-opdrachten in de volgende secties gebruiken de backslash `\`, als een voortzetting van regel tekens. Vervang of verwijder deze op basis van het hostbesturingssysteem vereisten. 
* **Argument order**: Wijzig de volgorde van de argumenten niet, tenzij u bekend bent met docker-containers.

U moet toevoegen de `text/analytics/v2.0` routering naar de URI van het eindpunt, zoals wordt weergegeven in het volgende BILLING_ENDPOINT_URI-voorbeeld.

Vervang {_argument_name_} door uw eigen waarden:

| Tijdelijke aanduiding | Waarde | Indeling of voorbeeld |
|-------------|-------|---|
|{BILLING_KEY} | De eindpuntsleutel van de `Cognitive Services` beschikbaar zijn op de Azure resource `Cognitive Services` op de pagina sleutels. |xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx|
|{BILLING_ENDPOINT_URI} | De facturering waarde van het eindpunt is beschikbaar op de Azure `Cognitive Services` pagina overzicht.|`https://westus.api.cognitive.microsoft.com/text/analytics/v2.1`|

> [!IMPORTANT]
> De `Eula`, `Billing`, en `ApiKey` opties moeten worden opgegeven voor het uitvoeren van de container; anders wordt de container niet start.  Zie voor meer informatie, [facturering](how-tos/text-analytics-how-to-install-containers.md#billing).
> De waarde ApiKey is de **sleutel** met de Azure- `Cognitive Services` resourcepagina sleutels. 

## <a name="key-phrase-extraction-container-docker-examples"></a>Sleuteluitdrukkingen extraheren container docker-voorbeelden

De volgende docker-voorbeelden zijn voor de container van sleuteluitdrukkingen extraheren. 

### <a name="basic-example"></a>Eenvoudige voorbeeld 

  ```
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 mcr.microsoft.com/azure-cognitive-services/keyphrase Eula=accept Billing={BILLING_ENDPOINT_URI} ApiKey={BILLING_KEY} 
  ```

### <a name="logging-example"></a>Voorbeeld van de logboekregistratie 

  ```
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 mcr.microsoft.com/azure-cognitive-services/keyphrase Eula=accept Billing={BILLING_ENDPOINT_URI} ApiKey={BILLING_KEY} Logging:Console:LogLevel:Default=Information
  ```

## <a name="language-detection-container-docker-examples"></a>Taal detecteren container docker-voorbeelden

De volgende docker-voorbeelden zijn voor de taal detecteren-container. 

### <a name="basic-example"></a>Eenvoudige voorbeeld

  ```
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 mcr.microsoft.com/azure-cognitive-services/language Eula=accept Billing={BILLING_ENDPOINT_URI} ApiKey={BILLING_KEY} 
  ```

### <a name="logging-example"></a>Voorbeeld van de logboekregistratie

  ```
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 mcr.microsoft.com/azure-cognitive-services/language Eula=accept Billing={BILLING_ENDPOINT_URI} ApiKey={BILLING_KEY} Logging:Console:LogLevel:Default=Information
  ```
 
## <a name="sentiment-analysis-container-docker-examples"></a>Sentiment-analyse container docker-voorbeelden

De volgende docker-voorbeelden zijn voor de container sentiment-analyse. 

### <a name="basic-example"></a>Eenvoudige voorbeeld

  ```
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 mcr.microsoft.com/azure-cognitive-services/sentiment Eula=accept Billing={BILLING_ENDPOINT_URI} ApiKey={BILLING_KEY} 
  ```

### <a name="logging-example"></a>Voorbeeld van de logboekregistratie

  ```
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 mcr.microsoft.com/azure-cognitive-services/sentiment Eula=accept Billing={BILLING_ENDPOINT_URI} ApiKey={BILLING_KEY} Logging:Console:LogLevel:Default=Information
  ```

## <a name="next-steps"></a>Volgende stappen

* Beoordeling [over het installeren en uitvoeren van containers](how-tos/text-analytics-how-to-install-containers.md)
* Meer [Cognitive Services-Containers](../cognitive-services-container-support.md)
