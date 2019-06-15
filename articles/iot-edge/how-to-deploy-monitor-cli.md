---
title: Automatische implementaties maken uit vanaf de opdrachtregel - Azure IoT Edge | Microsoft Docs
description: De IoT-extensie voor Azure CLI gebruiken voor automatische implementaties voor groepen van IoT Edge-apparaten maken
keywords: ''
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 02/19/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: f93d9eaefe18dd012a639cd26636b56b9eb09249
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60595151"
---
# <a name="deploy-and-monitor-iot-edge-modules-at-scale-using-the-azure-cli"></a>Implementeren en bewaken van IoT Edge-modules op schaal met behulp van de Azure CLI

[!INCLUDE [iot-edge-how-to-deploy-monitor-selector](../../includes/iot-edge-how-to-deploy-monitor-selector.md)]

Azure IoT Edge kunt u analytics verplaatsen naar de rand, en biedt een cloudinterface die u kunt beheren en bewaken van uw IoT Edge-apparaten op afstand. De mogelijkheid voor het beheren van apparaten op afstand is belangrijker zoals Internet of Things-oplossingen er die grotere en complexere groeien zijn. Azure IoT Edge is ontworpen ter ondersteuning van uw zakelijke doelstellingen, ongeacht het aantal apparaten die u toevoegt.

U kunt afzonderlijke apparaten beheren en implementeren van modules op deze één voor één. Echter, als u wilt om apparaten op grote schaal te wijzigen, kunt u een **automatische implementatie van IoT Edge**, die deel uitmaakt van automatische Apparaatbeheer in IoT Hub. Implementaties zijn dynamische processen waarmee u kunt meerdere modules in één keer implementeren op meerdere apparaten, de status en integriteit van de modules volgen en wijzig indien nodig. 

In dit artikel hebt instellen u Azure CLI en de IoT-extensie. Vervolgens leert u hoe u modules naar een set met IoT Edge-apparaten implementeren en de voortgang bekijken met behulp van de beschikbare CLI-opdrachten.

## <a name="cli-prerequisites"></a>CLI-vereisten

* Een [IoT-hub](../iot-hub/iot-hub-create-using-cli.md) in uw Azure-abonnement. 
* [IoT Edge-apparaten](how-to-register-device-cli.md) met IoT Edge-runtime geïnstalleerd.
* [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) in uw omgeving. Uw Azure CLI-versie moet ten minste 2.0.24 of hoger. Gebruik `az –-version` om de versie te valideren. In deze versie worden az-extensie-opdrachten ondersteund en is voor het eerst het Knack-opdrachtframework opgenomen. 
* De [IoT-extensie voor Azure CLI](https://github.com/Azure/azure-iot-cli-extension).

## <a name="configure-a-deployment-manifest"></a>Een manifest van de implementatie configureren

Het manifest voor een implementatie is een JSON-document waarin wordt beschreven welke modules te implementeren, hoe gegevens stromen tussen de modules, en de gewenste eigenschappen van de moduledubbels. Zie voor meer informatie over hoe implementatie werk manifesten en hoe ze worden gemaakt, [te begrijpen hoe IoT Edge-modules kunnen worden gebruikt, geconfigureerd en opnieuw gebruikt](module-composition.md).

Sla het manifest van implementatie lokaal als een txt-bestand voor het implementeren van modules met behulp van Azure CLI. Wanneer u de opdracht uit om de configuratie van toepassing op het apparaat uitvoert, wordt u het pad in de volgende sectie. 

Hier volgt een manifest eenvoudige implementatie met één module als een voorbeeld:

   ```json
   {
        "content": {
         "modulesContent": {
           "$edgeAgent": {
             "properties.desired": {
               "schemaVersion": "1.0",
               "runtime": {
                 "type": "docker",
                 "settings": {
                   "minDockerVersion": "v1.25",
                   "loggingOptions": "",
                   "registryCredentials": {
                     "registryName": {
                       "username": "",
                       "password": "",
                       "address": ""
                     }
                   }
               },
               "systemModules": {
                 "edgeAgent": {
                   "type": "docker",
                   "settings": {
                     "image": "mcr.microsoft.com/azureiotedge-agent:1.0",
                     "createOptions": "{}"
                   }
                 },
                 "edgeHub": {
                   "type": "docker",
                   "status": "running",
                   "restartPolicy": "always",
                   "settings": {
                     "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
                     "createOptions": "{}"
                   }
                 }
               },
               "modules": {
                 "tempSensor": {
                   "version": "1.0",
                   "type": "docker",
                   "status": "running",
                   "restartPolicy": "always",
                   "settings": {
                     "image": "mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0",
                     "createOptions": "{}"
                   }
                 }
               }
             }
           },
           "$edgeHub": {
             "properties.desired": {
               "schemaVersion": "1.0",
               "routes": {
                   "route": "FROM /* INTO $upstream"
               },
               "storeAndForwardConfiguration": {
                 "timeToLiveSecs": 7200
               }
             }
           },
           "tempSensor": {
             "properties.desired": {}
           }
         }
       }
   }
   ```

## <a name="identify-devices-using-tags"></a>Identificatie van apparaten met behulp van tags

Voordat u een implementatie maken kunt, moet u opgeven welke apparaten die u wilt toepassen. Azure IoT Edge-apparaten met identificeert **tags** op het dubbele apparaat. Elk apparaat kan meerdere labels, en u ze een manier die zinvol is voor uw oplossing kunt definiëren. Als u een campus van slimme gebouwen beheert, kunt u bijvoorbeeld de volgende codes toevoegen aan een apparaat:

```json
"tags":{
    "location":{
        "building": "20",
        "floor": "2"
    },
    "roomtype": "conference",
    "environment": "prod"
}
```

Zie voor meer informatie over apparaatdubbels en tags [apparaatdubbels begrijpen en gebruiken in IoT Hub](../iot-hub/iot-hub-devguide-device-twins.md).

## <a name="create-a-deployment"></a>Een implementatie maken

U implementeren modules naar uw apparaten door het maken van een implementatie die uit een manifest voor de implementatie, evenals andere parameters bestaat. 

Gebruik de volgende opdracht om een implementatie te maken:

   ```cli
   az iot edge deployment create --deployment-id [deployment id] --hub-name [hub name] --content [file path] --labels "[labels]" --target-condition "[target query]" --priority [int]
   ```

* **--implementatie-id** -de naam van de implementatie die wordt gemaakt in de IoT-hub. Geef uw implementatie een unieke naam die maximaal 128 kleine letters. Vermijd spaties en de volgende ongeldige tekens: `& ^ [ ] { } \ | " < > /`.
* **--hubnaam** -naam van de IoT-hub waarin de implementatie wordt gemaakt. De hub moet zich in het huidige abonnement. Schakel over naar het gewenste abonnement met de opdracht `az account set -s [subscription name]`.
* **--inhoud** -bestandspad naar de implementatie manifest van de JSON. 
* **--labels** -labels voor het bijhouden van uw implementaties toevoegen. Labels zijn naam, waarde-paren die uw implementatie te beschrijven. Labels worden JSON opmaak voor de namen en waarden. Bijvoorbeeld: `{"HostPlatform":"Linux", "Version:"3.0.1"}`
* **--doelvoorwaarde** -Geef een doelvoorwaarde om te bepalen welke apparaten doelgroepen voor deze implementatie. De voorwaarde is gebaseerd op het apparaat apparaatdubbel-tags of apparaatdubbel gerapporteerde eigenschappen en moet overeenkomen met de indeling van de expressie. Bijvoorbeeld `tags.environment='test' and properties.reported.devicemodel='4000x'`. 
* **--prioriteit** -een positief geheel getal zijn. In het geval dat twee of meer implementaties zijn gericht op hetzelfde apparaat, wordt de implementatie met de hoogste numerieke waarde voor prioriteit wordt toegepast.

## <a name="monitor-a-deployment"></a>Controleer de implementatie van een

Gebruik de volgende opdracht om de inhoud van een implementatie weer te geven:

   ```cli
az iot edge deployment show --deployment-id [deployment id] --hub-name [hub name]
   ```

* **--implementatie-id** -de naam van de implementatie die deel uitmaakt van de IoT-hub.
* **--hubnaam** -naam van de IoT-hub waarin de implementatie bestaat. De hub moet zich in het huidige abonnement. Schakel over naar het gewenste abonnement met de opdracht `az account set -s [subscription name]`

Controleer de implementatie in het opdrachtvenster. De **metrische gegevens** eigenschap bevat een aantal voor elke metrische gegevens die wordt geëvalueerd door elke hub:

* **targetedCount** -een systeem-metric die Hiermee geeft u het aantal dubbele apparaten in IoT-Hub die overeenkomen met de doelitems voorwaarde.
* **appliedCount** -een systeem metrische waarde geeft het aantal apparaten waarvoor de implementatie-inhoud toegepast op hun moduledubbels in IoT Hub.
* **reportedSuccessfulCount** -apparaat metric die Hiermee geeft u het nummer van Edge-apparaten in de implementatie voltooid van de IoT Edge-runtime client melden.
* **reportedFailedCount** -apparaat metric die Hiermee geeft u het nummer van Edge-apparaten in de implementatie reporting fout van de IoT Edge-runtime-client.

U kunt een lijst van apparaat-id's of objecten voor elk van de metrische gegevens weergeven met behulp van de volgende opdracht uit:

   ```cli
az iot edge deployment show-metric --deployment-id [deployment id] --metric-id [metric id] --hub-name [hub name] 
   ```

* **--implementatie-id** -de naam van de implementatie die deel uitmaakt van de IoT-hub.
* **--metriek-id** : de naam van de metrische gegevens waarvoor u wilt bijvoorbeeld de lijst met apparaat-id's, Zie `reportedFailedCount`
* **--hubnaam** -naam van de IoT-hub waarin de implementatie bestaat. De hub moet zich in het huidige abonnement. Schakel over naar het gewenste abonnement met de opdracht `az account set -s [subscription name]`

## <a name="modify-a-deployment"></a>Een implementatie wijzigen

Wanneer u een implementatie wijzigt, worden de wijzigingen onmiddellijk gerepliceerd naar alle apparaten uit de doelgroep. 

Als u de doelvoorwaarde bijwerkt, gebeuren de volgende updates:

* Als een apparaat niet voldoet aan de oude doelvoorwaarde, maar voldoet aan de nieuwe doelvoorwaarde en deze implementatie de hoogste prioriteit voor het apparaat is, wordt klikt u vervolgens deze implementatie toegepast op het apparaat. 
* Als een apparaat op dit moment met deze implementatie niet meer voldoet aan de doelvoorwaarde, verwijdert deze implementatie en neemt de volgende implementatie met hoogste prioriteit. 
* Als een apparaat op dit moment met deze implementatie niet meer voldoet aan de doelvoorwaarde en niet voldoet aan de doelvoorwaarde van alle andere implementaties, treedt er geen wijziging op op het apparaat. Het apparaat wordt uitgevoerd de huidige modules in hun huidige status, maar niet als onderdeel van deze implementatie niet meer wordt beheerd. Als deze voldoet aan de doelvoorwaarde van elke andere implementatie, deze implementatie wordt verwijderd en wordt op de nieuwe computer. 

Gebruik de volgende opdracht uit om bij te werken van een implementatie:

   ```cli
az iot edge deployment update --deployment-id [deployment id] --hub-name [hub name] --set [property1.property2='value']
   ```

* **--implementatie-id** -de naam van de implementatie die deel uitmaakt van de IoT-hub.
* **--hubnaam** -naam van de IoT-hub waarin de implementatie bestaat. De hub moet zich in het huidige abonnement. Schakel over naar het gewenste abonnement met de opdracht `az account set -s [subscription name]`
* **--ingesteld** -Update een eigenschap in de implementatie. U kunt de volgende eigenschappen bijwerken:
  * targetCondition - voorbeeld `targetCondition=tags.location.state='Oregon'`
  * labels 
  * priority


## <a name="delete-a-deployment"></a>Een implementatie verwijderen

Wanneer u een implementatie verwijdert, worden alle apparaten op de volgende implementatie met hoogste prioriteit. Als uw apparaten niet voldoen aan de doelvoorwaarde van elke andere implementatie, klikt u vervolgens de modules niet verwijderd wanneer de implementatie wordt verwijderd. 

Gebruik de volgende opdracht uit om te verwijderen van een implementatie:

   ```cli
az iot edge deployment delete --deployment-id [deployment id] --hub-name [hub name] 
   ```

* **--implementatie-id** -de naam van de implementatie die deel uitmaakt van de IoT-hub.
* **--hubnaam** -naam van de IoT-hub waarin de implementatie bestaat. De hub moet zich in het huidige abonnement. Schakel over naar het gewenste abonnement met de opdracht `az account set -s [subscription name]`

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [modules naar Edge-apparaten implementeren](module-deployment-monitoring.md).
