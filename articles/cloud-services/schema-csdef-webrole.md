---
title: Azure Cloud Services Def. WebRole Schema | Microsoft Docs
ms.custom: ''
ms.date: 04/14/2015
services: cloud-services
ms.reviewer: ''
ms.service: cloud-services
ms.suite: ''
ms.tgt_pltfrm: ''
ms.topic: reference
ms.assetid: 85368e4e-a0db-4c02-8dbc-8e2928fa6091
caps.latest.revision: 60
author: jpconnock
ms.author: jeconnoc
manager: timlt
ms.openlocfilehash: 0bb0946ea48a4c206d6bfe683da0835aca9b198b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60613231"
---
# <a name="azure-cloud-services-definition-webrole-schema"></a>Azure Cloud Services-definitie WebRole Schema
De Azure-web-rol is een rol die is aangepast voor web application programming ondersteund door IIS 7, zoals ASP.NET, PHP, Windows Communication Foundation en FastCGI.

De standaardextensie voor het servicedefinitiebestand is .csdef.

## <a name="basic-service-definition-schema-for-a-web-role"></a>Definitieschema voor Basic-service voor een Webrol  
De eenvoudige indeling van een service-definitie-bestand met een Webrol is als volgt.

```xml
<ServiceDefinition …>  
  <WebRole name="<web-role-name>" vmsize="<web-role-size>" enableNativeCodeExecution="[true|false]">  
    <Certificates>  
      <Certificate name="<certificate-name>" storeLocation="<certificate-store>" storeName="<store-name>" />  
    </Certificates>      
    <ConfigurationSettings>  
      <Setting name="<setting-name>" />  
    </ConfigurationSettings>  
    <Imports>  
      <Import moduleName="<import-module>"/>  
    </Imports>  
    <Endpoints>  
      <InputEndpoint certificate="<certificate-name>" ignoreRoleInstanceStatus="[true|false]" name="<input-endpoint-name>" protocol="[http|https|tcp|udp]" localPort="<port-number>" port="<port-number>" loadBalancerProbe="<load-balancer-probe-name>" />  
      <InternalEndpoint name="<internal-endpoint-name>" protocol="[http|tcp|udp|any]" port="<port-number>">  
         <FixedPort port="<port-number>"/>  
         <FixedPortRange min="<minimum-port-number>" max="<maximum-port-number>"/>  
      </InternalEndpoint>  
     <InstanceInputEndpoint name="<instance-input-endpoint-name>" localPort="<port-number>" protocol="[udp|tcp]">  
         <AllocatePublicPortFrom>  
            <FixedPortRange min="<minimum-port-number>" max="<maximum-port-number>"/>  
         </AllocatePublicPortFrom>  
      </InstanceInputEndpoint>  
    </Endpoints>  
    <LocalResources>  
      <LocalStorage name="<local-store-name>" cleanOnRoleRecycle="[true|false]" sizeInMB="<size-in-megabytes>" />  
    </LocalResources>  
    <LocalStorage name="<local-store-name>" cleanOnRoleRecycle="[true|false]" sizeInMB="<size-in-megabytes>" />  
    <Runtime executionContext="[limited|elevated]">  
      <Environment>  
         <Variable name="<variable-name>" value="<variable-value>">  
            <RoleInstanceValue xpath="<xpath-to-role-environment-settings>"/>  
          </Variable>            
      </Environment>  
      <EntryPoint>  
         <NetFxEntryPoint assemblyName="<name-of-assembly-containing-entrypoint>" targetFrameworkVersion="<.net-framework-version>"/>  
      </EntryPoint>  
    </Runtime>  
    <Sites>  
      <Site name="<web-site-name>">  
        <VirtualApplication name="<application-name>" physicalDirectory="<directory-path>"/>  
        <VirtualDirectory name="<directory-path>" physicalDirectory="<directory-path>"/>  
        <Bindings>  
          <Binding name="<binding-name>" endpointName="<endpoint-name-bound-to>" hostHeader="<url-of-the-site>"/>  
        </Bindings>  
      </Site>  
    </Sites>  
    <Startup priority="<for-internal-use-only>">  
      <Task commandLine="<command-to=execute>" executionContext="[limited|elevated]" taskType="[simple|foreground|background]">  
        <Environment>  
         <Variable name="<variable-name>" value="<variable-value>">  
            <RoleInstanceValue xpath="<xpath-to-role-environment-settings>"/>  
          </Variable>            
        </Environment>  
      </Task>  
    </Startup>  
    <Contents>  
      <Content destination="<destination-folder-name>" >  
        <SourceDirectory path="<local-source-directory>" />  
      </Content>  
    </Contents>  
  </WebRole>  
</ServiceDefinition>  
```  

## <a name="schema-elements"></a>Schema-elementen  
Het servicedefinitiebestand bevat deze elementen in detail in de volgende secties in dit onderwerp beschreven:  

[WebRole](#WebRole)

[ConfigurationSettings](#ConfigurationSettings)

[Instelling](#Setting)

[LocalResources](#LocalResources)

[LocalStorage](#LocalStorage)

[Eindpunten](#Endpoints)

[InternalEndpoint](#InternalEndpoint)

[InstanceInputEndpoint](#InstanceInputEndpoint)

[AllocatePublicPortFrom](#AllocatePublicPortFrom)

[FixedPort](#FixedPort)

[FixedPortRange](#FixedPortRange)

[Certificaten](#Certificates)

[Certificaat](#Certificate)

[Invoer](#Imports)

[Importeren](#Import)

[Runtime](#Runtime)

[Omgeving](#Environment)

[Variable](#Variable)

[RoleInstanceValue](#RoleInstanceValue)

[NetFxEntryPoint](#NetFxEntryPoint)

[Sites](#Sites)

[Site](#Site)

[VirtualApplication](#VirtualApplication)

[VirtualApplication](#VirtualApplication)

[Bindingen](#Bindings)

[Binding](#Binding)

[Opstarten](#Startup)

[Taak](#Task)

[Inhoud](#Contents)

[Inhoud](#Content)

[SourceDirectory](#SourceDirectory)

##  <a name="WebRole"></a> WebRole  
De `WebRole` element beschrijft een rol die is aangepast voor web application programming, ondersteund door IIS 7- en ASP.NET. Een service kan nul of meer webrollen bevatten.

De volgende tabel beschrijft de kenmerken van de `WebRole` element.

| Kenmerk | Type | Description |  
| --------- | ---- | ----------- |  
|name|string|Vereist. De naam voor de Webrol. De naam van de rol moet uniek zijn.|  
|enableNativeCodeExecution|booleaans|Optioneel. De standaardwaarde is `true`; systeemeigen uitvoering van code en volledig vertrouwen zijn standaard ingeschakeld. Dit kenmerk instelt op `false` uitschakelen van systeemeigen code uitvoeren voor de Webrol en gebruik in plaats daarvan Azure gedeeltelijk vertrouwen.|  
|vmsize|string|Optioneel. Stel deze waarde wijzigen van de grootte van de virtuele machine die wordt toegewezen aan de rol. De standaardwaarde is `Small`. Zie voor meer informatie, [VM-groottes voor Cloud Services](cloud-services-sizes-specs.md).|  

##  <a name="ConfigurationSettings"></a> ConfigurationSettings  
De `ConfigurationSettings` -element worden beschreven voor het verzamelen van configuratie-instellingen voor een Webrol. Dit element is het bovenliggende lid van de `Setting` element.

##  <a name="Setting"></a> Instelling  
De `Setting` -element een naam en waarde-paar dat Hiermee geeft u een configuratie-instelling voor een exemplaar van een rol worden beschreven.

De volgende tabel beschrijft de kenmerken van de `Setting` element.

| Kenmerk | Type | Description |  
| --------- | ---- | ----------- |  
|name|string|Vereist. Een unieke naam voor de configuratie-instelling.|  

De configuratie-instellingen voor een rol zijn naam / waarde-paren die zijn gedeclareerd in het servicedefinitiebestand en stel in het configuratiebestand van de service.

##  <a name="LocalResources"></a> LocalResources  
De `LocalResources` element beschrijving van de verzameling van resources voor lokale opslag voor een Webrol. Dit element is het bovenliggende lid van de `LocalStorage` element.

##  <a name="LocalStorage"></a> LocalStorage  
De `LocalStorage` element identificeert een lokale opslag-resource waarmee de ruimte die bestandssysteem voor de service tijdens runtime. Een rol kan nul of meer resources voor lokale opslag definiëren.

> [!NOTE]
>  De `LocalStorage` element kan worden weergegeven als een onderliggende site van de `WebRole` element ter ondersteuning van compatibiliteit met eerdere versies van de Azure SDK.

De volgende tabel beschrijft de kenmerken van de `LocalStorage` element.

| Kenmerk | Type | Description |  
| --------- | ---- | ----------- |  
|name|string|Vereist. Een unieke naam voor het lokale archief.|  
|cleanOnRoleRecycle|booleaans|Optioneel. Geeft aan of het lokale archief moet worden verwijderd wanneer de rol opnieuw wordt opgestart. Standaardwaarde is `true`.|  
|sizeInMb|int|Optioneel. De gewenste hoeveelheid opslagruimte om toe te wijzen voor het lokale archief, in MB. Indien niet opgegeven, is de standaard-opslagruimte toegewezen 100 MB. De minimale hoeveelheid opslagruimte die kan worden toegewezen is 1 MB.<br /><br /> De maximale grootte van de lokale bronnen is afhankelijk van de grootte van de virtuele machine. Zie voor meer informatie, [VM-groottes voor Cloud Services](cloud-services-sizes-specs.md).|  
  
De naam van de map die is toegewezen aan de lokale opslag-resource komt overeen met de opgegeven waarde voor het kenmerk name.

##  <a name="Endpoints"></a> Eindpunten  
De `Endpoints` -element worden beschreven voor het verzamelen van invoer (extern), interne en exemplaar invoer-eindpunten voor een rol. Dit element is het bovenliggende lid van de `InputEndpoint`, `InternalEndpoint`, en `InstanceInputEndpoint` elementen.

Invoer- en interne eindpunten worden afzonderlijk toegewezen. Een service kan een totaal van 25 invoer, intern, en exemplaar invoer eindpunten die kunnen worden toegewezen voor de 25 rollen toegestaan in een service. Bijvoorbeeld, als 5 rollen hebben u 5 invoereindpunten per rol kunt toewijzen of kunt u 25 invoereindpunten voor één rol toewijzen of kunt u 1 invoereindpunt elke 25 rollen toewijzen.

> [!NOTE]
>  Elke rol die geïmplementeerd is één exemplaar per rol vereist. De standaardwaarde voor een abonnement ingericht is beperkt tot 20 kernen en dus is beperkt tot 20 exemplaren van een rol. Als uw toepassing meer instanties vereist, dan wordt geleverd door de standaard Zie provisioning [ondersteuning voor facturering, Abonnementsbeheer en Quota](https://azure.microsoft.com/support/options/) voor meer informatie over het vergroten van uw quotum.

##  <a name="InputEndpoint"></a> InputEndpoint  
De `InputEndpoint` -element worden beschreven van een extern eindpunt naar een Webrol.

U kunt meerdere eindpunten die zijn een combinatie van HTTP, HTTPS, UDP en TCP-eindpunten definiëren. U kunt een ander poortnummer die u voor een invoereindpunt kiest opgeven, maar de poortnummers die is opgegeven voor elke rol in de service moet uniek zijn. Bijvoorbeeld, als u opgeeft dat een Webrol maakt gebruik van poort 80 voor HTTP en poort 443 voor HTTPS, kan vervolgens geeft u een tweede Webrol poort 8080 voor HTTP en poort 8043 gebruikt voor HTTPS.

De volgende tabel beschrijft de kenmerken van de `InputEndpoint` element.

| Kenmerk | Type | Description |  
| --------- | ---- | ----------- |  
|name|string|Vereist. Een unieke naam voor het externe eindpunt.|  
|protocol|string|Vereist. Het transportprotocol voor het externe eindpunt. Mogelijke waarden zijn voor een Webrol `HTTP`, `HTTPS`, `UDP`, of `TCP`.|  
|port|int|Vereist. De poort voor het externe eindpunt. U kunt opgeven dat een ander poortnummer die u kiest, maar de poortnummers die is opgegeven voor elke rol in de service moet uniek zijn.<br /><br /> Mogelijke waarden liggen tussen 1 en 65535, inclusief (Azure SDK versie 1.7 of hoger).|  
|certificaat|string|Vereist voor een HTTPS-eindpunt. De naam van een certificaat dat is gedefinieerd door een `Certificate` element.|  
|localPort|int|Optioneel. Hiermee geeft u een poort die wordt gebruikt voor interne verbindingen op het eindpunt. De `localPort` kenmerk wijst de externe poort op het eindpunt met een interne poort op een rol. Dit is handig in scenario's waarin een rol met een interne onderdeel op een andere poort communiceren moet dat verschilt van de naam die wordt blootgesteld extern.<br /><br /> Indien niet opgegeven, de waarde van `localPort` is hetzelfde als de `port` kenmerk. Stel de waarde van `localPort` naar ' * ' voor het automatisch toewijzen van een niet-toegewezen poort die kan worden gedetecteerd met behulp van de runtime-API.<br /><br /> Mogelijke waarden liggen tussen 1 en 65535, inclusief (Azure SDK versie 1.7 of hoger).<br /><br /> De `localPort` kenmerk is alleen beschikbaar via de Azure SDK-versie 1.3 of hoger.|  
|ignoreRoleInstanceStatus|booleaans|Optioneel. Als de waarde van dit kenmerk is ingesteld op `true`, de status van een service wordt genegeerd en het eindpunt niet worden verwijderd door de load balancer. De waarde instelt op `true` handig voor foutopsporing bezet exemplaren van een service. De standaardwaarde is `false`. **Opmerking:**  Een eindpunt kan nog steeds verkeer ontvangen, zelfs wanneer de rol niet gereed is.|  
|loadBalancerProbe|string|Optioneel. De naam van de load balancer-test die zijn gekoppeld aan het invoereindpunt. Zie voor meer informatie, [LoadBalancerProbe Schema](schema-csdef-loadbalancerprobe.md).|  

##  <a name="InternalEndpoint"></a> InternalEndpoint  
De `InternalEndpoint` -element een intern eindpunt dat aan een Webrol worden beschreven. Een intern eindpunt dat is alleen beschikbaar voor andere rolinstanties in de service. het is niet beschikbaar is voor clients buiten de service. Webrollen die geen bevatten de `Sites` element kan alleen een enkele interne HTTP-, UDP of TCP-eindpunt hebben.

De volgende tabel beschrijft de kenmerken van de `InternalEndpoint` element.

| Kenmerk | Type | Description |  
| --------- | ---- | ----------- |  
|name|string|Vereist. Een unieke naam voor het interne eindpunt.|  
|protocol|string|Vereist. Het transportprotocol voor het interne eindpunt. Mogelijke waarden zijn `HTTP`, `TCP`, `UDP`, of `ANY`.<br /><br /> Een waarde van `ANY` geeft aan dat elk protocol, een willekeurige poort is toegestaan.|  
|port|int|Optioneel. De poort die wordt gebruikt voor interne taakverdeling verbindingen op het eindpunt. Een gelijke taakverdeling eindpunt maakt gebruik van twee poorten. De poort die wordt gebruikt voor het openbare IP-adres en de poort die wordt gebruikt op het privé IP-adres. Deze zijn meestal worden ze ingesteld op dezelfde, maar u kunt kiezen voor het gebruik van verschillende poorten.<br /><br /> Mogelijke waarden liggen tussen 1 en 65535, inclusief (Azure SDK versie 1.7 of hoger).<br /><br /> De `Port` kenmerk is alleen beschikbaar via de Azure SDK-versie 1.3 of hoger.|  

##  <a name="InstanceInputEndpoint"></a> InstanceInputEndpoint  
De `InstanceInputEndpoint` element beschrijft een invoereindpunt exemplaar naar een Webrol. Een invoereindpunt exemplaar is gekoppeld aan een specifieke rol-exemplaar met behulp van doorsturen via poort in de load balancer. Een invoereindpunt voor elke instantie wordt toegewezen aan een specifieke poort uit een verscheidenheid aan mogelijke poorten. Dit element is het bovenliggende lid van de `AllocatePublicPortFrom` element.

De `InstanceInputEndpoint` element is alleen beschikbaar via de Azure SDK-versie 1.7 of hoger.

De volgende tabel beschrijft de kenmerken van de `InstanceInputEndpoint` element.
  
| Kenmerk | Type | Description |  
| --------- | ---- | ----------- |  
|name|string|Vereist. Een unieke naam voor het eindpunt.|  
|localPort|int|Vereist. Hiermee geeft u de interne poort die door alle rolinstanties aan om te kunnen ontvangen van binnenkomend verkeer doorgestuurd van de load balancer luistert. Mogelijke waarden liggen tussen 1 en 65535 op.|  
|protocol|string|Vereist. Het transportprotocol voor het interne eindpunt. Mogelijke waarden zijn `udp` en `tcp`. Gebruik `tcp` voor http/https op basis van verkeer.|  
  
##  <a name="AllocatePublicPortFrom"></a> AllocatePublicPortFrom  
De `AllocatePublicPortFrom` -element het bereik van openbare poort die door externe klanten kan worden gebruikt voor toegang tot een invoereindpunt voor elke instantie worden beschreven. Het openbare (VIP)-poortnummer dat is toegewezen uit dit bereik en toegewezen aan elke afzonderlijke rol exemplaar eindpunt tijdens de implementatie van de tenant en update. Dit element is het bovenliggende lid van de `FixedPortRange` element.

De `AllocatePublicPortFrom` element is alleen beschikbaar via de Azure SDK-versie 1.7 of hoger.

##  <a name="FixedPort"></a> FixedPort  
De `FixedPort` element Hiermee geeft u de poort voor het interne eindpunt, welke Hiermee wordt de met gelijke taakverdeling verbindingen op het eindpunt.

De `FixedPort` element is alleen beschikbaar via de Azure SDK-versie 1.3 of hoger.

De volgende tabel beschrijft de kenmerken van de `FixedPort` element.

| Kenmerk | Type | Description |  
| --------- | ---- | ----------- |  
|port|int|Vereist. De poort voor het interne eindpunt. Dit heeft hetzelfde effect als de instelling de `FixedPortRange` min en max op dezelfde poort.<br /><br /> Mogelijke waarden liggen tussen 1 en 65535, inclusief (Azure SDK versie 1.7 of hoger).|  

##  <a name="FixedPortRange"></a> FixedPortRange  
De `FixedPortRange` element Hiermee geeft u het bereik van poorten die zijn toegewezen aan de interne eindpunt of een invoereindpunt exemplaar en stelt de poort die wordt gebruikt voor load balanced verbindingen op het eindpunt.

> [!NOTE]
>  De `FixedPortRange` element werkt anders, afhankelijk van het element waarin deze zich bevindt. Wanneer de `FixedPortRange` element heeft de `InternalEndpoint` -element, wordt alle poorten op de load balancer binnen het bereik van de kenmerken min en max voor alle virtuele machines waarop de rol wordt uitgevoerd. Wanneer de `FixedPortRange` element heeft de `InstanceInputEndpoint` -element, wordt slechts één poort binnen het bereik van de kenmerken min en max op elke virtuele machine met de rol.

De `FixedPortRange` element is alleen beschikbaar via de Azure SDK-versie 1.3 of hoger.

De volgende tabel beschrijft de kenmerken van de `FixedPortRange` element.

| Kenmerk | Type | Description |  
| --------- | ---- | ----------- |  
|min.|int|Vereist. De minimale poort in het bereik. Mogelijke waarden liggen tussen 1 en 65535, inclusief (Azure SDK versie 1.7 of hoger).|  
|max|string|Vereist. De maximale poort in het bereik. Mogelijke waarden liggen tussen 1 en 65535, inclusief (Azure SDK versie 1.7 of hoger).|  

##  <a name="Certificates"></a> Certificaten  
De `Certificates` -element worden beschreven voor het verzamelen van certificaten voor een Webrol. Dit element is het bovenliggende lid van de `Certificate` element. Een rol kan een onbeperkt aantal gekoppelde certificaten hebben. Zie voor meer informatie over het gebruik van het element certificaten [wijzigen van de definitie van de Service van het bestand met een certificaat](cloud-services-configure-ssl-certificate-portal.md#step-2-modify-the-service-definition-and-configuration-files).

##  <a name="Certificate"></a> Certificaat  
De `Certificate` element wordt een certificaat dat is gekoppeld aan een Webrol beschreven.

De volgende tabel beschrijft de kenmerken van de `Certificate` element.

| Kenmerk | Type | Description |  
| --------- | ---- | ----------- |  
|name|string|Vereist. Een naam op voor dit certificaat, dat wordt gebruikt om te verwijzen naar deze wanneer deze gekoppeld aan een HTTPS is `InputEndpoint` element.|  
|storeLocation|string|Vereist. De locatie van het certificaatarchief waar dit certificaat kan worden gevonden op de lokale computer. Mogelijke waarden zijn `CurrentUser` en `LocalMachine`.|  
|storeName|string|Vereist. De naam van het certificaatarchief waar dit certificaat bevindt zich op de lokale computer. Mogelijke waarden zijn onder andere de namen van de ingebouwde store `My`, `Root`, `CA`, `Trust`, `Disallowed`, `TrustedPeople`, `TrustedPublisher`, `AuthRoot`, `AddressBook`, of de naam van een aangepast archief. Als de naam van een aangepast archief is opgegeven, wordt de store automatisch gemaakt.|  
|permissionLevel|string|Optioneel. Hiermee geeft u de machtigingen voor toegang krijgen tot de rol-processen. Als u wilt dat alleen met verhoogde bevoegdheden processen kunnen toegang tot de persoonlijke sleutel, geeft u vervolgens `elevated` machtiging. `limitedOrElevated` machtiging is mogelijk alle rol processen voor toegang tot de persoonlijke sleutel. Mogelijke waarden zijn `limitedOrElevated` en `elevated`. De standaardwaarde is `limitedOrElevated`.|  

##  <a name="Imports"></a> Invoer  
De `Imports` -element een verzameling importeren van modules voor een Webrol die onderdelen aan het gastbesturingssysteem toevoegen worden beschreven. Dit element is het bovenliggende lid van de `Import` element. Dit element is optioneel en een rol kan slechts één invoer blok hebben. 

De `Imports` element is alleen beschikbaar via de Azure SDK-versie 1.3 of hoger.

##  <a name="Import"></a> Importeren  
De `Import` element Hiermee geeft u een module toevoegen aan het gastbesturingssysteem.

De `Import` element is alleen beschikbaar via de Azure SDK-versie 1.3 of hoger.

De volgende tabel beschrijft de kenmerken van de `Import` element.

| Kenmerk | Type | Description |  
| --------- | ---- | ----------- |  
|moduleName|string|Vereist. De naam van de module te importeren. Geldige import-modules zijn:<br /><br /> -   RemoteAccess<br />-RemoteForwarder<br />-Diagnostics<br /><br /> De modules RemoteAccess en RemoteForwarder kunnen u uw rolinstantie voor verbindingen met extern bureaublad configureren. Zie voor meer informatie [verbinding met extern bureaublad inschakelen](cloud-services-role-enable-remote-desktop-new-portal.md).<br /><br /> De module voor diagnostische gegevens kunt u voor het verzamelen van diagnostische gegevens voor een rolinstantie.|  

##  <a name="Runtime"></a> Runtime  
De `Runtime` element een verzameling van omgeving variabele instellingen voor een Webrol die de runtime-omgeving van het Azure-host-proces bepalen wordt beschreven. Dit element is het bovenliggende lid van de `Environment` element. Dit element is optioneel en een rol kan slechts één runtime-blok hebben.

De `Runtime` element is alleen beschikbaar via de Azure SDK-versie 1.3 of hoger.

De volgende tabel beschrijft de kenmerken van de `Runtime` element:  

| Kenmerk | Type | Description |  
| --------- | ---- | ----------- |  
|executionContext|string|Optioneel. Hiermee geeft u de context waarin het proces van de rol wordt gestart. De standaardcontext is `limited`.<br /><br /> -   `limited` : Het proces wordt gestart zonder Administrator-bevoegdheden.<br />-   `elevated` : Het proces wordt gestart met Administrator-bevoegdheden.|  

##  <a name="Environment"></a> Omgeving  
De `Environment` element beschrijft een reeks omgeving variabele instellingen voor een Webrol. Dit element is het bovenliggende lid van de `Variable` element. Een rol mogelijk een willekeurig aantal omgevingsvariabelen die worden ingesteld.

##  <a name="Variable"></a> Variabele  
De `Variable` element Hiermee geeft u een omgevingsvariabele instellen in het gastbesturingssysteem.

De `Variable` element is alleen beschikbaar via de Azure SDK-versie 1.3 of hoger.

De volgende tabel beschrijft de kenmerken van de `Variable` element:  

| Kenmerk | Type | Description |  
| --------- | ---- | ----------- |  
|name|string|Vereist. De naam van de omgevingsvariabele om in te stellen.|  
|value|string|Optioneel. De waarde om in te stellen voor de omgevingsvariabele. U moet een waardekenmerk bevatten of een `RoleInstanceValue` element.|  

##  <a name="RoleInstanceValue"></a> RoleInstanceValue  
De `RoleInstanceValue` element Hiermee geeft u het xPath waaruit u wilt ophalen van de waarde van de variabele.

De volgende tabel beschrijft de kenmerken van de `RoleInstanceValue` element.

| Kenmerk | Type | Description |  
| --------- | ---- | ----------- |  
|XPath|string|Optioneel. Pad naar de locatie van de implementatie-instellingen voor het exemplaar. Zie voor meer informatie, [configuratievariabelen met XPath](cloud-services-role-config-xpath.md).<br /><br /> U moet een waardekenmerk bevatten of een `RoleInstanceValue` element.|  

##  <a name="EntryPoint"></a> EntryPoint  
De `EntryPoint` element Hiermee geeft u het toegangspunt voor een rol. Dit element is het bovenliggende lid van de `NetFxEntryPoint` elementen. Deze elementen kunnen u een toepassing dan de standaard WaWorkerHost.exe om te fungeren als de functie-ingangspunt opgeven.

De `EntryPoint` element is alleen beschikbaar via de Azure SDK-versie 1.5 of hoger.

##  <a name="NetFxEntryPoint"></a> NetFxEntryPoint  
De `NetFxEntryPoint` element Hiermee geeft u het programma voor een rol uit te voeren.

> [!NOTE]
>  De `NetFxEntryPoint` element is alleen beschikbaar via de Azure SDK-versie 1.5 of hoger.

De volgende tabel beschrijft de kenmerken van de `NetFxEntryPoint` element.

| Kenmerk | Type | Description |  
| --------- | ---- | ----------- |  
|assemblyName|string|Vereist. Het pad en de naam van de assembly waarin het toegangspunt. Het pad is ten opzichte van de map  **\\%ROLEROOT%\Approot** (Geef geen  **\\%ROLEROOT%\Approot** in `commandLine`, wordt ervan uitgegaan). **% ROLEROOT** wordt een omgevingsvariabele onderhouden door Azure en staat voor de locatie van de basismap voor uw rol. De  **\\%ROLEROOT%\Approot** map vertegenwoordigt de map voor uw rol.<br /><br /> Voor de HWC het pad is altijd relatief aan de  **\\%ROLEROOT%\Approot\bin** map.<br /><br /> Voor volledige IIS en IIS Express-rollen, als de assembly kan niet worden gevonden ten opzichte  **\\%ROLEROOT%\Approot** map, de  **\\%ROLEROOT%\Approot\bin** wordt gezocht.<br /><br /> Deze terugvallen gedrag voor volledige IIS is niet een best practice aangeraden en mogelijk verwijderd uit toekomstige versies.|  
|targetFrameworkVersion|string|Vereist. De versie van .NET framework waarop de assembly is gemaakt. Bijvoorbeeld `targetFrameworkVersion="v4.0"`.|  

##  <a name="Sites"></a> Sites  
De `Sites` -element worden beschreven van een verzameling van websites en web-apps die worden gehost in een Webrol. Dit element is het bovenliggende lid van de `Site` element. Als u geen opgeeft een `Sites` -element, uw Webrol wordt gehost als legacy-Webrol en u kunt slechts één website die wordt gehost in uw Webrol hebben. Dit element is optioneel en een rol kan slechts één sites blokkeren.

De `Sites` element is alleen beschikbaar via de Azure SDK-versie 1.3 of hoger.

##  <a name="Site"></a> Site  
De `Site` element Hiermee geeft u een website of web-toepassing die deel uitmaakt van de Webrol.

De `Site` element is alleen beschikbaar via de Azure SDK-versie 1.3 of hoger.

De volgende tabel beschrijft de kenmerken van de `Site` element.

| Kenmerk | Type | Description |  
| --------- | ---- | ----------- |  
|name|string|Vereist. De naam van de website of toepassing.|  
|physicalDirectory|string|De locatie van de map met inhoud voor de hoofdmap van de site. De locatie kan worden opgegeven als een absoluut pad of ten opzichte van het .csdef-locatie.|  

##  <a name="VirtualApplication"></a> VirtualApplication  
De `VirtualApplication` element een toepassing wordt gedefinieerd in Internet Information Services (IIS) 7 is een groepering van bestanden die levert inhoud of services via protocollen, zoals HTTP. Wanneer u een toepassing in IIS 7 maakt, wordt het pad van de toepassing onderdeel van de URL van de site.

De `VirtualApplication` element is alleen beschikbaar via de Azure SDK-versie 1.3 of hoger.

De volgende tabel beschrijft de kenmerken van de `VirtualApplication` element.

| Kenmerk | Type | Description |  
| --------- | ---- | ----------- |  
|name|string|Vereist. Hiermee geeft u een unieke naam in voor de virtuele toepassing.|  
|physicalDirectory|string|Vereist. Hiermee geeft u het pad op de ontwikkelcomputer dat de virtuele toepassing bevat. In de rekenemulator wordt IIS geconfigureerd voor het ophalen van inhoud vanaf deze locatie. Wanneer u de Azure implementeert, wordt de inhoud van de fysieke map zijn verpakt samen met de rest van de service. Wanneer de servicepakket wordt geïmplementeerd naar Azure, wordt IIS geconfigureerd met de locatie van de uitgepakte inhoud.|  

##  <a name="VirtualDirectory"></a> VirtualDirectory  
De `VirtualDirectory` element Hiermee geeft u de naam van een map (ook wel pad genoemd) dat u in IIS opgeeft en worden toegewezen aan een fysieke map op een lokale of externe server.

De `VirtualDirectory` element is alleen beschikbaar via de Azure SDK-versie 1.3 of hoger.

De volgende tabel beschrijft de kenmerken van de `VirtualDirectory` element.

| Kenmerk | Type | Description |  
| --------- | ---- | ----------- |  
|name|string|Vereist. Hiermee geeft u een unieke naam in voor de virtuele map.|  
|value|physicalDirectory|Vereist. Hiermee geeft u het pad op de ontwikkelcomputer waarin de website of de inhoud van de virtuele map. In de rekenemulator wordt IIS geconfigureerd voor het ophalen van inhoud vanaf deze locatie. Wanneer u de Azure implementeert, wordt de inhoud van de fysieke map zijn verpakt samen met de rest van de service. Wanneer de servicepakket wordt geïmplementeerd naar Azure, wordt IIS geconfigureerd met de locatie van de uitgepakte inhoud.|  

##  <a name="Bindings"></a> Bindingen  
De `Bindings` -element een verzameling van bindingen voor een website worden beschreven. Het is het bovenliggende element van de `Binding` element. Het element is vereist voor elke `Site` element. Zie voor meer informatie over het configureren van eindpunten [communicatie inschakelen voor Rolinstanties](cloud-services-enable-communication-role-instances.md).

De `Bindings` element is alleen beschikbaar via de Azure SDK-versie 1.3 of hoger.

##  <a name="Binding"></a> Binding  
De `Binding` element Hiermee geeft u de configuratie-informatie die nodig is voor aanvragen om te communiceren met een website of web-toepassing.

De `Binding` element is alleen beschikbaar via de Azure SDK-versie 1.3 of hoger.

| Kenmerk | Type | Description |  
| --------- | ---- | ----------- |  
|name|string|Vereist. Hiermee geeft u een unieke naam in voor de binding.|  
|endpointName|string|Vereist. Hiermee geeft u de naam van het eindpunt moet worden verbonden.|  
|hostHeader|string|Optioneel. Hiermee geeft u de naam van een host waarmee u voor het hosten van meerdere sites met verschillende hostnamen, op de combinatie van een enkel IP-adres/poort.|  

##  <a name="Startup"></a> Opstarten  
De `Startup` -element worden beschreven van een verzameling taken die worden uitgevoerd wanneer de rol wordt gestart. Dit element mag het bovenliggende lid van de `Variable` element. Zie voor meer informatie over het gebruik van de rol opstarttaken [opstarttaken configureren](cloud-services-startup-tasks.md). Dit element is optioneel en een rol kan slechts één opstarten blok hebben.

De volgende tabel beschrijft het kenmerk van de `Startup` element.

| Kenmerk | Type | Description |  
| --------- | ---- | ----------- |  
|prioriteit|int|Alleen voor intern gebruik.|  

##  <a name="Task"></a> Taak  
De `Task` element Hiermee geeft u de opstarttaak die plaatsvindt wanneer de rol wordt gestart. Opstarttaken kunnen worden gebruikt om de taken uitvoeren die voorbereiden van de rol voor deze installatie van software-onderdelen of andere toepassingen worden uitgevoerd. Taken uitvoeren in de volgorde waarin ze worden weergegeven in de `Startup` element blokkeren.

De `Task` element is alleen beschikbaar via de Azure SDK-versie 1.3 of hoger.

De volgende tabel beschrijft de kenmerken van de `Task` element.

| Kenmerk | Type | Description |  
| --------- | ---- | ----------- |  
|Opdrachtregel|string|Vereist. Een script, zoals een CMD-bestand met de opdrachten om uit te voeren. Opdracht en batch opstartbestanden moeten worden opgeslagen in de ANSI-indeling. Bestandsindelingen die een markering bytevolgorde die is ingesteld op het begin van het bestand verwerkt niet correct.|  
|executionContext|string|Hiermee geeft u de context waarin het script wordt uitgevoerd.<br /><br /> -   `limited` [Standaard] – uitvoeren met de dezelfde bevoegdheden als de rol die het proces host.<br />-   `elevated` – Uitgevoerd met beheerdersbevoegdheden.|  
|taskType|string|Hiermee geeft u het uitvoeringsgedrag van de opdracht.<br /><br /> -   `simple` [Standaard] – systeem wacht tot de taak om af te sluiten voordat andere taken worden gestart.<br />-   `background` – Systeem wacht niet totdat de taak om af te sluiten.<br />-   `foreground` – Vergelijkbaar met de achtergrond, behalve de rol niet totdat alle taken van voorgrond afsluit opnieuw wordt opgestart.|  

##  <a name="Contents"></a> Inhoud  
De `Contents` -element worden beschreven voor het verzamelen van inhoud voor een Webrol. Dit element is het bovenliggende lid van de `Content` element.

De `Contents` element is alleen beschikbaar via de Azure SDK-versie 1.5 of hoger.

##  <a name="Content"></a> Inhoud  
De `Content` element wordt gedefinieerd voor de locatie van inhoud wordt gekopieerd naar de virtuele machine van Azure en het doelpad waarnaar deze is gekopieerd.

De `Content` element is alleen beschikbaar via de Azure SDK-versie 1.5 of hoger.

De volgende tabel beschrijft de kenmerken van de `Content` element.

| Kenmerk | Type | Description |  
| --------- | ---- | ----------- |  
|destination|string|Vereist. De locatie op de Azure-machine waarop de inhoud wordt geplaatst. Deze locatie is ten opzichte van de map **%ROLEROOT%\Approot**.|  

Dit element is het bovenliggende element van de `SourceDirectory` element.

##  <a name="SourceDirectory"></a> SourceDirectory  
De `SourceDirectory` element wordt gedefinieerd voor de lokale map waarin inhoud wordt gekopieerd. Dit element gebruiken om op te geven van de lokale inhoud kopiëren naar de virtuele machine van Azure.

De `SourceDirectory` element is alleen beschikbaar via de Azure SDK-versie 1.5 of hoger.

De volgende tabel beschrijft de kenmerken van de `SourceDirectory` element.

| Kenmerk | Type | Description |  
| --------- | ---- | ----------- |  
|pad|string|Vereist. Relatieve of absolute pad van een lokale map waarvan de inhoud wordt gekopieerd naar de virtuele machine van Azure. Uitbreiding van omgevingsvariabelen in het mappad wordt ondersteund.|  
  
## <a name="see-also"></a>Zie ook
[(Klassiek) Definitieschema voor cloud Service](schema-csdef-file.md)
