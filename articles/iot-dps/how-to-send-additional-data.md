---
title: Een nettolading overdragen tussen apparaten en Azure Device Provisioning Service
description: In dit document wordt beschreven hoe u een Payload tussen Device Provisioning Service (DPS) overdraagt
author: menchi
ms.author: menchi
ms.date: 02/11/2020
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
ms.openlocfilehash: de6bb3fe0879a69467283e93a04a355876a02cba
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "80246684"
---
# <a name="how-to-transfer-a-payload-between-device-and-dps"></a>Een Payload tussen apparaat en DPS overdragen
Soms heeft DPS meer gegevens nodig van apparaten om ze goed in te richten op de juiste IoT Hub en dat de gegevens moeten worden verstrekt door het apparaat. Omgekeerd kan DPS gegevens naar het apparaat retour neren om logica aan de client zijde te vergemakkelijken. 

## <a name="when-to-use-it"></a>Wanneer te gebruiken
Deze functie kan worden gebruikt als een uitbrei ding voor [aangepaste toewijzing](https://docs.microsoft.com/azure/iot-dps/how-to-use-custom-allocation-policies). U wilt bijvoorbeeld uw apparaten toewijzen op basis van het model apparaat zonder menselijke tussen komst. In dit geval gebruikt u [aangepaste toewijzing](https://docs.microsoft.com/azure/iot-dps/how-to-use-custom-allocation-policies). U kunt het apparaat zo configureren dat de model gegevens worden gerapporteerd als onderdeel van de registratie van het [apparaat](https://docs.microsoft.com/rest/api/iot-dps/runtimeregistration/registerdevice). De nettolading van het apparaat wordt door DPS doorgestuurd naar de aangepaste toewijzings-webhook. En uw functie kan bepalen welke IoT Hub dit apparaat gaat gebruiken wanneer het apparaat-model informatie ontvangt. En als de webhook sommige gegevens naar het apparaat wil retour neren, worden de gegevens weer gegeven als een teken reeks in het webhook-antwoord.  

## <a name="device-sends-data-payload-to-dps"></a>Apparaat verzendt gegevens lading naar DPS
Wanneer uw apparaat een [aanroep van een registratie-apparaat](https://docs.microsoft.com/rest/api/iot-dps/runtimeregistration/registerdevice) naar DPS verzendt, kan de registratie oproep worden uitgebreid om andere velden in de hoofd tekst te maken. De hoofd tekst ziet er als volgt uit: 
   ```
   { 
       “registrationId”: “mydevice”, 
       “tpm”:                
       { 
           “endorsementKey”: “stuff”, 
           “storageRootKey”: “things” 
       }, 
       “payload”: “your additional data goes here. It can be nested JSON.” 
    } 
   ```

## <a name="dps-returns-data-to-the-device"></a>DPS retourneert gegevens naar het apparaat
Als de webhook voor het aangepaste toewijzings beleid sommige gegevens naar het apparaat wil retour neren, worden de gegevens weer gegeven als een teken reeks in het antwoord van de webhook. De wijziging bevindt zich in de sectie Payload hieronder. 
   ```
   { 
       "iotHubHostName": "sample-iot-hub-1.azure-devices.net", 
       "initialTwin": { 
           "tags": { 
               "tag1": true 
               }, 
               "properties": { 
                   "desired": { 
                       "tag2": true 
                    } 
                } 
            }, 
        "payload": "whatever is returned by the webhook" 
    } 
   ```

## <a name="sdk-support"></a>SDK-ondersteuning
Deze functie is beschikbaar in C, C#, JAVA en Node.js [client-sdk's](https://docs.microsoft.com/azure/iot-dps/).  

## <a name="next-steps"></a>Volgende stappen
* Ontwikkelen met behulp van de [Azure IOT SDK]( https://github.com/Azure/azure-iot-sdks) voor Azure IOT hub en Azure IOT hub Device Provisioning Service
