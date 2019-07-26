---
title: Cheat-venster voor functie configuratie Cloud Services Microsoft Docs
description: De verschillende XPath-instellingen die u in de configuratie van de Cloud service functie kunt gebruiken om instellingen beschikbaar te stellen als een omgevings variabele.
services: cloud-services
author: georgewallace
ms.service: cloud-services
ms.topic: article
ms.date: 04/19/2017
ms.author: gwallace
ms.openlocfilehash: cd2bdc4fc4b2a135907851ca4d3034430618e0cd
ms.sourcegitcommit: 4b647be06d677151eb9db7dccc2bd7a8379e5871
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2019
ms.locfileid: "68359014"
---
# <a name="expose-role-configuration-settings-as-an-environment-variable-with-xpath"></a>Rollen configuratie-instellingen beschikbaar stellen als een omgevings variabele met XPath
In het definitie bestand van de Cloud Service Worker-service of webfunctie kunt u runtime configuratie waarden weer geven als omgevings variabelen. De volgende XPath-waarden worden ondersteund (die overeenkomen met de API-waarden).

Deze XPath-waarden zijn ook beschikbaar via de bibliotheek [micro soft. WindowsAzure. ServiceRuntime](/previous-versions/azure/reference/ee773173(v=azure.100)) . 

## <a name="app-running-in-emulator"></a>App die wordt uitgevoerd in de emulator
Geeft aan dat de app wordt uitgevoerd in de emulator.

| type | Voorbeeld |
| --- | --- |
| XPath |XPath = "/RoleEnvironment/Deployment/@emulated" |
| Code |var x = RoleEnvironment. IsEmulated; |

## <a name="deployment-id"></a>Implementatie-id
Hiermee wordt de implementatie-ID voor het exemplaar opgehaald.

| type | Voorbeeld |
| --- | --- |
| XPath |XPath = "/RoleEnvironment/Deployment/@id" |
| Code |var deploymentId = RoleEnvironment. DeploymentId; |

## <a name="role-id"></a>Rol-id
Hiermee wordt de huidige rol-ID voor het exemplaar opgehaald.

| type | Voorbeeld |
| --- | --- |
| XPath |XPath = "/RoleEnvironment/CurrentInstance/@id" |
| Code |var-ID = RoleEnvironment.CurrentRoleInstance.Id; |

## <a name="update-domain"></a>Domein bijwerken
Hiermee wordt het update domein van het exemplaar opgehaald.

| type | Voorbeeld |
| --- | --- |
| XPath |XPath = "/RoleEnvironment/CurrentInstance/@updateDomain" |
| Code |var UD = RoleEnvironment. CurrentRoleInstance. UpdateDomain; |

## <a name="fault-domain"></a>Foutdomein
Hiermee wordt het fout domein van het exemplaar opgehaald.

| type | Voorbeeld |
| --- | --- |
| XPath |XPath = "/RoleEnvironment/CurrentInstance/@faultDomain" |
| Code |var FD = RoleEnvironment. CurrentRoleInstance. FaultDomain; |

## <a name="role-name"></a>Rolnaam
Hiermee wordt de rolnaam van de instanties opgehaald.

| type | Voorbeeld |
| --- | --- |
| XPath |XPath = "/RoleEnvironment/CurrentInstance/@roleName" |
| Code |var rName = RoleEnvironment.CurrentRoleInstance.Role.Name; |

## <a name="config-setting"></a>Configuratie-instelling
Hiermee haalt u de waarde van de opgegeven configuratie-instelling op.

| type | Voorbeeld |
| --- | --- |
| XPath |xpath="/RoleEnvironment/CurrentInstance/ConfigurationSettings/ConfigurationSetting[@name='Setting1']/@value" |
| Code |var setting = RoleEnvironment. GetConfigurationSettingValue ("Setting1"); |

## <a name="local-storage-path"></a>Lokaal opslagpad
Hiermee haalt u het lokale opslagpad voor het exemplaar.

| type | Voorbeeld |
| --- | --- |
| XPath |xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='LocalStore1']/@path" |
| Code |var localResourcePath = RoleEnvironment. GetLocalResource ("LocalStore1"). RootPath; |

## <a name="local-storage-size"></a>Grootte van lokale opslag
Hiermee wordt de grootte van de lokale opslag voor het exemplaar opgehaald.

| type | Voorbeeld |
| --- | --- |
| XPath |xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='LocalStore1']/@sizeInMB" |
| Code |var localResourceSizeInMB = RoleEnvironment. GetLocalResource ("LocalStore1"). MaximumSizeInMegabytes; |

## <a name="endpoint-protocol"></a>Eindpunt Protocol
Hiermee haalt u het eindpunt protocol voor het exemplaar.

| type | Voorbeeld |
| --- | --- |
| XPath |xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@protocol" |
| Code |var-protocol = RoleEnvironment. CurrentRoleInstance. InstanceEndpoints ["Endpoint1"]. Protocolsubstatus |

## <a name="endpoint-ip"></a>IP-eind punt
Hiermee wordt het opgegeven IP-adres van het eind punt opgehaald.

| type | Voorbeeld |
| --- | --- |
| XPath |xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@address" |
| Code |var Address = RoleEnvironment. CurrentRoleInstance. InstanceEndpoints ["Endpoint1"]. IPEndpoint. Address |

## <a name="endpoint-port"></a>Eindpunt poort
Hiermee haalt u de eindpunt poort voor het exemplaar op.

| type | Voorbeeld |
| --- | --- |
| XPath |xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@port" |
| Code |var-poort = RoleEnvironment. CurrentRoleInstance. InstanceEndpoints ["Endpoint1"]. IPEndpoint. Port; |

## <a name="example"></a>Voorbeeld
Hier volgt een voor beeld van een werk rollen die een opstart taak maakt met een omgevings `TestIsEmulated` variabele met de naam ingesteld op de [ @emulated XPath-waarde](#app-running-in-emulator). 

```xml
<WorkerRole name="Role1">
    <ConfigurationSettings>
      <Setting name="Setting1" />
    </ConfigurationSettings>
    <LocalResources>
      <LocalStorage name="LocalStore1" sizeInMB="1024"/>
    </LocalResources>
    <Endpoints>
      <InternalEndpoint name="Endpoint1" protocol="tcp" />
    </Endpoints>
    <Startup>
      <Task commandLine="example.cmd inputParm">
        <Environment>
          <Variable name="TestConstant" value="Constant"/>
          <Variable name="TestEmptyValue" value=""/>
          <Variable name="TestIsEmulated">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated"/>
          </Variable>
          ...
        </Environment>
      </Task>
    </Startup>
    <Runtime>
      <Environment>
        <Variable name="TestConstant" value="Constant"/>
        <Variable name="TestEmptyValue" value=""/>
        <Variable name="TestIsEmulated">
          <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated"/>
        </Variable>
        ...
      </Environment>
    </Runtime>
    ...
</WorkerRole>
```

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het bestand [ServiceConfiguration. cscfg](cloud-services-model-and-package.md#serviceconfigurationcscfg) .

Maak een [ServicePackage. cspkg](cloud-services-model-and-package.md#servicepackagecspkg) -pakket.

[Extern bureau blad](cloud-services-role-enable-remote-desktop-new-portal.md) inschakelen voor een rol.

