---
title: bestand opnemen
description: bestand opnemen
services: digital-twins
author: dsk-2015
ms.service: digital-twins
ms.topic: include
ms.date: 6/26/2019
ms.author: dkshir
ms.custom: include file
ms.openlocfilehash: 9771e312269eb78e0dc4535a61e9287b5b169d7c
ms.sourcegitcommit: a6873b710ca07eb956d45596d4ec2c1d5dc57353
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/16/2019
ms.locfileid: "67459184"
---
1. Meld u aan bij [Azure Portal](http://portal.azure.com).

1. Selecteer **Een resource maken** in het linkerdeelvenster. Zoeken naar **digitale dubbels**, en selecteer **digitale dubbels**. Selecteer **Maken** om het implementatieproces te starten.

   ![Selecties voor het maken van een nieuw Digital Twins-exemplaar](./media/create-digital-twins-portal/create-digital-twins.png)

1. In het deelvenster **Digital Twins** voert u de volgende informatie in:
   * **Resourcenaam**: geef uw instantie van Digital Twins een unieke naam.
   * **Abonnement**: kies het abonnement dat u wilt gebruiken om deze instantie van Digital Twins te maken. 
   * **Resourcegroep**: selecteer of maak een [resourcegroep](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups) voor de instantie van Digital Twins.
   * **Locatie**: Selecteer de dichtstbijzijnde locatie voor uw apparaten.

     ![Digital Twins-deelvenster met ingevoerde gegevens](./media/create-digital-twins-portal/create-digital-twins-param.png)

1. Controleer uw Digital Twins-gegevens en selecteer **Maken**. Het kan een paar minuten duren voordat uw instantie van Digital Twins is gemaakt. U kunt de voortgang bewaken via het deelvenster **Meldingen**.

1. Open het deelvenster **Overzicht** van de instantie van Digital Twins. Zoals u ziet, wordt er een koppeling weergegeven onder **Beheer API**.

   De indeling van de URL van de **Beheer API** is `https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/swagger`. Deze URL leidt u naar de documentatie van de REST API van Azure Digital Twins, die van toepassing is op uw instantie. Zie [Het gebruik van Azure Digital Twins Swagger](../articles/digital-twins/how-to-use-swagger.md) voor informatie over hoe u deze API-documentatie dient te lezen en gebruiken.

    Wijzig de URL van de **Beheer API** als volgt: `https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/api/v1.0/`. Uw toepassing gebruikt de aangepaste URL als de basis-URL voor toegang tot uw instantie. Kopieer deze gewijzigde URL naar een tijdelijk bestand. U hebt deze URL nodig in de volgende sectie.

    ![Beheer-API](./media/create-digital-twins-portal/digital-twins-management-api.png)