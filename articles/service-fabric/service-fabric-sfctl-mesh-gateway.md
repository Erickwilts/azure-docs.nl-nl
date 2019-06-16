---
title: Azure Service Fabric CLI - sfctl mesh gateway | Microsoft Docs
description: Beschrijving van de Service Fabric-CLI sfctl mesh-gateway-opdrachten.
services: service-fabric
documentationcenter: na
author: Christina-Kang
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: cli
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 12/06/2018
ms.author: bikang
ms.openlocfilehash: 27cfe380e5988f17ca79c430cde7defc7f7bc440
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60839076"
---
# <a name="sfctl-mesh-gateway"></a>sfctl mesh gateway
Ophalen en verwijderen van mesh-gateway-resources.

## <a name="commands"></a>Opdrachten

|Opdracht|Description|
| --- | --- |
| delete | Hiermee verwijdert u de Gateway-resource. |
| list | Geeft een lijst van alle gateway-resources. |
| weergeven | Hiermee haalt u de Gateway-resource met de opgegeven naam. |

## <a name="sfctl-mesh-gateway-delete"></a>sfctl mesh-gateway delete
Hiermee verwijdert u de Gateway-resource.

Hiermee verwijdert u de Gateway-resource aangeduid met de naam.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --naam -n (vereist) | De naam van de gateway-resource. |

### <a name="global-arguments"></a>Global Arguments

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-mesh-gateway-list"></a>sfctl mesh-gateway list
Geeft een lijst van alle gateway-resources.

Hiermee haalt de informatie over alle gateway-resources in een bepaalde resourcegroep. De informatie omvat de beschrijving en andere eigenschappen van de Gateway.

### <a name="global-arguments"></a>Global Arguments

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |

## <a name="sfctl-mesh-gateway-show"></a>sfctl mesh-gateway show
Hiermee haalt u de Gateway-resource met de opgegeven naam.

Hiermee haalt u de informatie over de Gateway-resource met de opgegeven naam. De informatie omvat de beschrijving en andere eigenschappen van de Gateway.

### <a name="arguments"></a>Argumenten

|Argument|Description|
| --- | --- |
| --naam -n (vereist) | De naam van de gateway-resource. |

### <a name="global-arguments"></a>Global Arguments

|Argument|Description|
| --- | --- |
| --debug | Verhoog logboekregistratie uitgebreid om weer te geven van dat alle logboeken voor foutopsporing. |
| --help -h | In dit help-bericht en afsluiten weergeven. |
| --output -o | De indeling van de uitvoer.  Toegestane waarden\: json, jsonc, tabel, tsv.  Standaard\: json. |
| --query | JMESPath-query-tekenreeks. Zie http\://jmespath.org/ voor meer informatie en voorbeelden. |
| --uitgebreide | Detailniveau van logboekregistratie verhogen. Gebruik--foutopsporing voor logboeken voor volledige foutopsporing. |


## <a name="next-steps"></a>Volgende stappen
- [Instellen van](service-fabric-cli.md) de Service Fabric-CLI.
- Meer informatie over het gebruik van de Service Fabric-CLI met behulp van de [voorbeelden van scripts](/azure/service-fabric/scripts/sfctl-upgrade-application).