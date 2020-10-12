---
title: API voor beveiligingsmodule voor Azure RTOS
description: Verwijzings-API voor de beveiligings module voor Azure RTO'S.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/07/2020
ms.author: mlottner
ms.openlocfilehash: 8191d68f701a3be6a37d8623d2d900f6aa6c1851
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "90936300"
---
# <a name="security-module-for-azure-rtos-api-preview"></a>Beveiligings module voor Azure RTO'S-API (preview)

Deze API is alleen bedoeld voor gebruik met de beveiligings module voor Azure RTO'S. Zie de [Security module voor Azure Rto's github resource](https://github.com/azure-rtos/azure-iot-preview/releases)voor meer informatie. 

## <a name="enable-security-module-for-azure-rtos"></a>Beveiligings module voor Azure RTO'S inschakelen

**nx_azure_iot_security_module_enable**

### <a name="prototype"></a>Prototype

```c
UINT nx_azure_iot_security_module_enable(NX_AZURE_IOT *nx_azure_iot_ptr);
```

### <a name="description"></a>Beschrijving

Met deze routine wordt het subsysteem Azure IoT Security module ingeschakeld. Een interne status computer beheert het verzamelen van beveiligings gebeurtenissen en verzendt deze naar Azure IoT Hub. Er is slechts één NX_AZURE_IOT_SECURITY_MODULE exemplaar vereist en dat nodig is voor het beheren van gegevens verzameling.

### <a name="parameters"></a>Parameters

| Naam | Beschrijving |
|---------|---------|
| nx_azure_iot_ptr [in]    | Een verwijzing naar een `NX_AZURE_IOT` .  |

### <a name="return-values"></a>Retourwaarden

|Retourwaarden  |Beschrijving |
|---------|---------|
|NX_AZURE_IOT_SUCCESS|   De Azure IoT-beveiligings module is ingeschakeld.     |
|NX_AZURE_IOT_FAILURE   |  Het inschakelen van de Azure IoT-beveiligings module is mislukt vanwege een interne fout.    |
|NX_AZURE_IOT_INVALID_PARAMETER   |  De beveiligings module vereist een geldig #NX_AZURE_IOT-exemplaar.      |

### <a name="allowed-from"></a>Toegestaan van

Lijnen

## <a name="disable-azure-iot-security-module"></a>De Azure IoT-beveiligings module uitschakelen

**nx_azure_iot_security_module_disable**


### <a name="prototype"></a>Prototype

```c
UINT nx_azure_iot_security_module_disable(NX_AZURE_IOT *nx_azure_iot_ptr);
```

### <a name="description"></a>Beschrijving

Met deze routine wordt het subsysteem Azure IoT Security module uitgeschakeld.

### <a name="parameters"></a>Parameters

| Naam | Beschrijving |
|---------|---------|
| nx_azure_iot_ptr [in]    | Een verwijzing naar `NX_AZURE_IOT` . Als de waarde NULL is, wordt het Singleton-exemplaar uitgeschakeld. |

### <a name="return-values"></a>Retourwaarden

|Retourwaarden  |Beschrijving |
|---------|---------|
|NX_AZURE_IOT_SUCCESS     |   Geslaagd wanneer de Azure IoT-beveiligings module is uitgeschakeld.      |
|NX_AZURE_IOT_INVALID_PARAMETER   |  Het exemplaar van Azure IoT Hub wijkt af van het samengestelde Singleton-exemplaar.       |
|NX_AZURE_IOT_FAILURE    |  De Azure IoT-beveiligings module kan niet worden uitgeschakeld vanwege een interne fout.       |

### <a name="allowed-from"></a>Toegestaan van

Lijnen


## <a name="next-steps"></a>Volgende stappen

Raadpleeg de volgende artikelen voor meer informatie over hoe u aan de slag kunt gaan met de Azure RTO'S-beveiligings module:

- Bekijk het [overzicht](iot-security-azure-rtos.md)van de beveiligings module Defender voor IOT rto's.