---
title: Handleiding voor het installeren en implementeren van Linux C-agent voor IoT-agent Preview van Azure Security Center | Microsoft Docs
description: Informatie over het installeren van de Azure Security Center voor IoT-agent op zowel 32-bits en 64-bits Linux.
services: asc-for-iot
ms.service: ascforiot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 3ccf2aec-106a-4d2c-8079-5f3e8f2afdcb
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/28/2019
ms.author: mlottner
ms.openlocfilehash: 49ebb5932e1e918330625fd0df98811873dd5cd5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65200659"
---
# <a name="deploy-azure-security-center-for-iot-c-based-security-agent-for-linux"></a>Azure Security Center voor beveiliging op basis van IoT C-agent voor Linux implementeren

> [!IMPORTANT]
> Azure Security Center voor IoT is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

Deze handleiding wordt uitgelegd hoe te installeren en implementeren van de Azure Security Center (ASC) voor beveiliging op basis van IoT C-agent op Linux.

In deze handleiding leert u het volgende: 
> [!div class="checklist"]
> * Installeren
> * Implementatie verifiëren
> * De agent verwijderen
> * Problemen oplossen 

## <a name="prerequisites"></a>Vereisten

Zie voor andere platforms en versies van de agent [kiest u de juiste security agent](how-to-deploy-agent.md).

1. Voor het implementeren van de security-agent zijn lokale administratorrechten op de computer die u wilt installeren op (sudo) vereist.

1. [Maken van een security module](quickstart-create-security-twin.md) voor het apparaat.

## <a name="installation"></a>Installatie 

Als u wilt installeren en implementeren van de security-agent, het volgende doen:


1. De meest recente versie downloaden op de computer van [Github](https://aka.ms/iot-security-github-c).

1. Pak de inhoud van het pakket uit en navigeer naar de _/Install_ map.

1. Actieve Voeg machtigingen toe aan de **InstallSecurityAgent script** door het uitvoeren van de volgende:
    
   ```
   chmod +x InstallSecurityAgent.sh
   ```

1. Voer vervolgens: 

   ```
   ./InstallSecurityAgent.sh -aui <authentication identity> -aum <authentication method> -f <file path> -hn <host name> -di <device id> -i
   ```
   
   Zie [verificatie configureren](concept-security-agent-authentication-methods.md) voor meer informatie over parameters voor verificatie.

Dit script doet het volgende:

1. Vereiste onderdelen installeert.

2. Hiermee voegt u een servicegebruiker toe (met interactieve aanmelding uitgeschakeld).

3. Installeert de agent als een **Daemon** -wordt ervan uitgegaan dat het apparaat gebruikmaakt van **systemd** voor service management.

4. Hiermee configureert u de agent met de opgegeven verificatieparameters. 

Voer het script voor extra hulp met de parameter Help-informatie: 
    
    ./InstallSecurityAgent.sh --help

### <a name="uninstall-the-agent"></a>De agent verwijderen

Voer het script met de agent verwijderen, de-parameter verwijderen:

    ./InstallSecurityAgent.sh -–uninstall

## <a name="troubleshooting"></a>Problemen oplossen
Controleer de implementatiestatus door uit te voeren:

    systemctl status ASCIoTAgent.service


## <a name="next-steps"></a>Volgende stappen
- Lees de ASC voor IoT-service [overzicht](overview.md)
- Meer informatie over ASC voor IoT [architectuur](architecture.md)
- Schakel de [service](quickstart-onboard-iot-hub.md)
- Lees de [Veelgestelde vragen](resources-frequently-asked-questions.md)
- Inzicht in [beveiligingswaarschuwingen](concept-security-alerts.md)