---
title: OPC-Twin cloud afhankelijkheden in Azure implementeren | Microsoft Docs
description: Klik hier voor meer informatie over het implementeren van OPC dubbele Azure afhankelijkheden.
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: conceptual
ms.service: iot-industrialiot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: ae9f2b05bfc6ea6315022d04c8d267d916cf282e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "61451596"
---
# <a name="deploying-dependencies-for-local-development"></a>Afhankelijkheden voor lokale ontwikkeling implementeren

In dit artikel wordt uitgelegd hoe u alleen de Azure-platformservices behoefte aan lokale ontwikkeling en foutopsporing te implementeren.   Aan het einde hebt u een resourcegroep geïmplementeerd die alles bevat wat die u nodig hebt voor lokale ontwikkeling en foutopsporing.

## <a name="deploy-azure-platform-services"></a>Azure-platform-services implementeren

1. Zorg ervoor dat u hebt PowerShell en [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-1.1.0) extensies geïnstalleerd.  Open een opdrachtprompt of terminal en voer:

   ```bash
   git clone https://github.com/Azure/azure-iiot-components
   cd azure-iiot-components
   ```

   ```bash
   deploy -type local
   ```

2. Volg de aanwijzingen voor het toewijzen van een naam aan de resourcegroep voor uw implementatie.  Het script worden alleen de afhankelijkheden aan deze resourcegroep in uw Azure-abonnement, maar niet de microservices.  Het script wordt een toepassing ook geregistreerd in Azure Active Directory.  Dit is nodig om op basis van OAUTH-verificatie te ondersteunen.  Implementatie kan enkele minuten duren.

3. Nadat het script is voltooid, kunt u de .env-bestand op te slaan.  Het bestand van de omgeving .env is het configuratiebestand van alle services en hulpprogramma's die u wilt uitvoeren op uw ontwikkelcomputer.  

## <a name="troubleshooting-deployment-failures"></a>Fouten bij het oplossen van problemen

### <a name="resource-group-name"></a>Naam van de resourcegroep

Zorg ervoor dat u een kort en eenvoudig Resourcegroepnaam.  De naam wordt ook gebruikt voor naam resources als zodanig die het moet voldoen aan resource naamgevingsvereisten.  

### <a name="azure-active-directory-aad-registration"></a>Registratie van Azure Active Directory (AAD)

Het implementatiescript probeert te registreren van AAD-toepassingen in Azure Active Directory.  Dit kan, afhankelijk van uw rechten voor de geselecteerde AAD-tenant mislukken.   Er zijn 3 opties:

1. Als u ervoor een AAD-tenant in een lijst met tenants kiest, het script nogmaals starten en kies een andere naam in de lijst.
2. U kunt ook een persoonlijke AAD-tenant implementeren, het script nogmaals starten en selecteren om deze te gebruiken.
3. Doorgaan zonder verificatie.  Nadat u uw microservices lokaal uitvoert, dit acceptabel is, maar biedt niet nabootsen voor productie-omgevingen.  

## <a name="next-steps"></a>Volgende stappen

Nu dat u dubbele OPC-services aan een bestaand project hebt geïmplementeerd, volgt de voorgestelde volgende stap:

> [!div class="nextstepaction"]
> [Meer informatie over het implementeren van OPC-Twin-modules](howto-opc-twin-deploy-modules.md)
