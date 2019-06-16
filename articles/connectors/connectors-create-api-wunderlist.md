---
title: Verbinding maken met Wunderlist vanuit Azure Logic Apps | Microsoft Docs
description: Automatiseren van taken en werkstromen die bewaken en beheren van lijsten, taken, herinneringen en meer in uw Wunderlist-account met behulp van Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.assetid: e4773ecf-3ad3-44b4-a1b5-ee5f58baeadd
ms.topic: article
tags: connectors
ms.date: 08/25/2018
ms.openlocfilehash: e3570ab1227ca388ac62bffdc74bb68b1ddc41d1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "62105663"
---
# <a name="monitor-and-manage-wunderlist-by-using-azure-logic-apps"></a>Controleren en Wunderlist beheren met behulp van Azure Logic Apps

Met Azure Logic Apps en de Wunderlist-connector, kunt u geautomatiseerde taken en werkstromen die bewaken en beheren van to-do-lijsten, taken, herinneringen en meer in uw Wunderlist-account, samen met andere acties, bijvoorbeeld:

* Monitor als nieuwe taken gemaakt, wanneer taken verwacht worden of herinneringen gebeuren.
* Maken en beheren van lijsten, notities, taken, subtaken en meer.
* Herinneringen instellen.
* Get-lijsten, taken, subtaken, herinneringen, bestanden, notities, opmerkingen en meer.

[Wunderlist](https://www.wunderlist.com/) is een service die u bij het plannen helpt, beheren en uw projecten, een lijst met taken en taken - op elk apparaat en overal te voltooien. U kunt triggers die te antwoorden krijgen van uw Wunderlist-account en de uitvoer beschikbaar voor andere acties. U kunt acties die taken met uw Wunderlist-account uitvoeren gebruiken. U kunt ook andere acties waarmee de uitvoer van de Wunderlist-acties hebben. Als nieuwe taken nadert, kunt u bijvoorbeeld berichten met de Slack-connector plaatsen. Als u geen ervaring met logische apps, raadpleegt u [wat is Azure Logic Apps?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, <a href="https://azure.microsoft.com/free/" target="_blank">registreer u dan nu voor een gratis Azure-account</a>. 

* Uw Wunderlist-account en de gebruikersreferenties

   Uw referenties toestaan dat de logische app een verbinding maken en toegang tot uw account Wunderlist.

* Basiskennis over [over het maken van logische apps](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* De logische app waar u toegang tot uw Yammer-account. Om te beginnen met een trigger Wunderlist [maken van een lege, logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md). Voor het gebruik van een Wunderlist-actie beginnen uw logische app met een andere trigger, bijvoorbeeld, de **terugkeerpatroon** trigger.

## <a name="connect-to-wunderlist"></a>Verbinding maken met Wunderlist

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en open uw logische app in Logic App Designer, als het niet al geopend.

1. Kies een pad: 

   * Typ 'wunderlist' als filter voor lege, logische apps, in het zoekvak. 
   Selecteer de gewenste trigger onder de lijst met triggers. 

     -of-

   * Voor bestaande logische apps: 
   
     * Kies onder de laatste stap waar u een actie toevoegen, **nieuwe stap**. 

       -of-

     * Tussen de stappen waar u een actie toevoegen, de aanwijzer over de pijl tussen fasen. 
     Kies het plusteken ( **+** ) die wordt weergegeven, en selecteer vervolgens **een actie toevoegen**.
     
       Typ 'wunderlist' als filter in het zoekvak. 
       Selecteer de actie die u wilt onder de lijst met acties.

1. Als u wordt gevraagd of u zich aanmeldt bij Wunderlist, meld u nu, zodat u toegang kunt toestaan.

1. Geef de benodigde informatie voor uw geselecteerde trigger of actie en doorgaan met het ontwikkelen van uw logische app-werkstroom.

## <a name="connector-reference"></a>Connector-verwijzing

Voor technische informatie over triggers en acties limieten die worden beschreven van de connector openapi (voorheen Swagger) beschrijving van de connector controleren [-verwijzingspagina](/connectors/wunderlist/).

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de [website voor feedback van Logic Apps-gebruikers](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over andere [Logic Apps-connectors](../connectors/apis-list.md)