---
title: Bewaking van toepassingsprestaties voor Java-web-apps in Azure Application Insights | Microsoft Docs
description: Uitgebreide gebruiksbewaking van prestaties en van Java-website met Application Insights.
services: application-insights
documentationcenter: java
author: mrbullwinkle
manager: carmonm
ms.assetid: 84017a48-1cb3-40c8-aab1-ff68d65e2128
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: mbullwin
ms.openlocfilehash: ce5f7ab1e6751a9ce68aa2d9c466a112c9cac182
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60900605"
---
# <a name="monitor-dependencies-caught-exceptions-and-method-execution-times-in-java-web-apps"></a>Afhankelijkheden, uitzonderingen onderschept en methode uitvoertijd in Java-web-apps bewaken


Als u hebt [uw Java-web-app met Application Insights geïnstrumenteerd][java], kunt u de Java-Agent om meer inzicht te krijgen, zonder codewijzigingen:

* **Afhankelijkheden:** Gegevens over gesprekken die uw toepassing voor andere onderdelen maakt, met inbegrip van:
  * **REST-aanroepen** aangebracht via HttpClient, OkHttp en RestTemplate (Spring) worden vastgelegd.
  * **Redis** aanroepen via de client Jedis worden vastgelegd.
  * **[JDBC aanroepen](https://docs.oracle.com/javase/7/docs/technotes/guides/jdbc/)**  -MySQL, SQL Server en Oracle DB-opdrachten automatisch worden vastgelegd. Als de aanroep langer dan 10s duurt, rapporteert de agent voor MySQL, het queryplan.
* **Gevangen uitzonderingen:** Informatie over uitzonderingen die door uw code worden uitgevoerd.
* **Methode uitvoeringstijd:** Informatie over de tijd die nodig is voor het uitvoeren van specifieke methoden.

Voor het gebruik van de Java-agent, installeren u deze op uw server. Uw web-apps moeten zijn uitgerust met de [Application Insights Java SDK][java]. 

## <a name="install-the-application-insights-agent-for-java"></a>De Application Insights-agent voor Java installeren
1. Op de computer waarop u uw Java-server wordt uitgevoerd [downloaden van de agent](https://github.com/Microsoft/ApplicationInsights-Java/releases/latest). Zorg ervoor dat u het downloaden van de dezelfde versie van Java-Agent als de pakketten core- en web Application Insights Java SDK.
2. Bewerk het opstartscript van de toepassing-server en toevoegen van de volgende JVM:
   
    `javaagent:`*volledige pad naar het JAR-bestand van agent*
   
    Bijvoorbeeld, in Tomcat op een Linux-machine:
   
    `export JAVA_OPTS="$JAVA_OPTS -javaagent:<full path to agent JAR file>"`
3. Start de toepassingsserver van uw opnieuw.

## <a name="configure-the-agent"></a>De agent configureren
Maak een bestand met de naam `AI-Agent.xml` en plaats deze in dezelfde map als de agent JAR-bestand.

Hiermee stelt u de inhoud van het xml-bestand. Het volgende voorbeeld als u wilt opnemen in of uitsluiten van de functies die u wilt bewerken.

```XML

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationInsightsAgent>
      <Instrumentation>

        <!-- Collect remote dependency data -->
        <BuiltIn enabled="true">
           <!-- Disable Redis or alter threshold call duration above which arguments are sent.
               Defaults: enabled, 10000 ms -->
           <Jedis enabled="true" thresholdInMS="1000"/>

           <!-- Set SQL query duration above which query plan is reported (MySQL, PostgreSQL). Default is 10000 ms. -->
           <MaxStatementQueryLimitInMS>1000</MaxStatementQueryLimitInMS>
        </BuiltIn>

        <!-- Collect data about caught exceptions
             and method execution times -->

        <Class name="com.myCompany.MyClass">
           <Method name="methodOne"
               reportCaughtExceptions="true"
               reportExecutionTime="true"
               />
           <!-- Report on the particular signature
                void methodTwo(String, int) -->
           <Method name="methodTwo"
              reportExecutionTime="true"
              signature="(Ljava/lang/String;I)V" />
        </Class>

      </Instrumentation>
    </ApplicationInsightsAgent>

```

U moet inschakelen rapporten uitzondering en de timing van de methode voor de afzonderlijke methoden.

Standaard `reportExecutionTime` is ingesteld op true en `reportCaughtExceptions` is ingesteld op false.

## <a name="additional-config-spring-boot"></a>Aanvullende configuratie (Spring Boot)

`java -javaagent:/path/to/agent.jar -jar path/to/TestApp.jar`

Voor Azure App Services het volgende doen:

* Selecteer Instellingen > Toepassingsinstellingen
* Voeg een nieuw sleutelwaardepaar toe bij App-instellingen:

Sleutel: `JAVA_OPTS` Waarde: `-javaagent:D:/home/site/wwwroot/applicationinsights-agent-2.3.1-SNAPSHOT.jar`

Controleer de versies voor de meest recente versie van de Java-agent [hier](https://github.com/Microsoft/ApplicationInsights-Java/releases
). 

De agent moet worden verpakt als een resource in uw project, zodat het einde van in de map D:/home/site/wwwroot/map. U kunt bevestigen dat de agent zich in de juiste App Service-map door te gaan naar **ontwikkeltools** > **geavanceerde hulpmiddelen** > **Foutopsporingsconsole**en onderzoek van de inhoud van de map van de site.    

* Sla de instellingen op en start de app opnieuw. (Deze stappen alleen van toepassing op App-Services die worden uitgevoerd op Windows.)

> [!NOTE]
> AI-Agent.xml en de agent jar-bestand moeten zich in dezelfde map. Ze worden vaak samen geplaatst de `/resources` map van het project.  

### <a name="spring-rest-template"></a>Spring Rest-sjabloon

In de volgorde voor Application Insights is softwareontwikkelaars HTTP-aanroepen met behulp van de Spring Rest-sjabloon, is het gebruik van de Apache HTTP-Client vereist. Spring van Rest-sjabloon is standaard niet geconfigureerd voor het gebruik van de Apache HTTP-Client. Door op te geven [HttpComponentsClientHttpRequestfactory](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/http/client/HttpComponentsClientHttpRequestFactory.html) in de constructor van een sjabloon van de Rest Spring Apache HTTP wordt gebruikt.

Hier volgt een voorbeeld van hoe u dit doet met Spring bonen. Dit is een zeer eenvoudig voorbeeld dat gebruikmaakt van de standaardinstellingen van de klasse factory.

```java
@bean
public ClientHttpRequestFactory httpRequestFactory() {
return new HttpComponentsClientHttpRequestFactory()
}
@Bean(name = 'myRestTemplate')
public RestTemplate dcrAccessRestTemplate() {
    return new RestTemplate(httpRequestFactory())
}
```

#### <a name="enable-w3c-distributed-tracing"></a>W3C gedistribueerde tracering inschakelen

Voeg de volgende AI-Agent.xml:

```xml
<Instrumentation>
        <BuiltIn enabled="true">
            <HTTP enabled="true" W3C="true" enableW3CBackCompat="true"/>
        </BuiltIn>
    </Instrumentation>
```

> [!NOTE]
> Modus voor achterwaartse compatibiliteit is standaard ingeschakeld en de enableW3CBackCompat-parameter is optioneel en moet worden gebruikt alleen wanneer u wilt uitschakelen. 

In het ideale geval is dit het geval wanneer alle services zijn bijgewerkt naar een nieuwere versie van de SDK's ondersteunen W3C-protocol. Het is raadzaam om te verplaatsen naar de nieuwere versie van de SDK's met ondersteuning voor W3C zo snel mogelijk.

Zorg ervoor dat **beide [binnenkomende](correlation.md#w3c-distributed-tracing) en uitgaande (agent)-configuraties** exact hetzelfde zijn.

## <a name="view-the-data"></a>De gegevens bekijken
In de Application Insights-resource, samengevoegde externe afhankelijkheden en de methode uitvoertijd weergegeven [onder de tegel prestaties][metrics].

Als u wilt zoeken naar afzonderlijke exemplaren van afhankelijkheden, uitzonderingen en methode rapporten, open [zoeken][diagnostic].

[Vaststellen van afhankelijkheidsproblemen met - meer](../../azure-monitor/app/asp-net-dependencies.md#diagnosis).

## <a name="questions-problems"></a>Vragen? Problemen?
* Zijn er geen gegevens? [Set firewall-uitzonderingen](../../azure-monitor/app/ip-addresses.md)
* [Problemen met Java oplossen](java-troubleshoot.md)

<!--Link references-->

[api]: ../../azure-monitor/app/api-custom-events-metrics.md
[apiexceptions]: ../../azure-monitor/app/api-custom-events-metrics.md#track-exception
[availability]: ../../azure-monitor/app/monitor-web-app-availability.md
[diagnostic]: ../../azure-monitor/app/diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[java]: java-get-started.md
[javalogs]: java-trace-logs.md
[metrics]: ../../azure-monitor/app/metrics-explorer.md
