---
title: Verbinding maken met Excel Online - Azure Logic Apps
description: Beheren van gegevens met Excel Online REST API's en Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
tags: connectors
ms.topic: article
ms.date: 08/23/2018
ms.openlocfilehash: 28739ad65462acc9f2d2ed7db1e9ed14d19f032c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60311961"
---
# <a name="manage-excel-online-data-with-azure-logic-apps"></a>Excel Online-gegevens met Azure Logic Apps beheren

Met Azure Logic Apps en de Excel Online-connector, kunt u geautomatiseerde taken en werkstromen op basis van uw gegevens in Excel Online voor bedrijven of OneDrive. Deze connector beschikt u acties waarmee u werkt met uw gegevens en werkbladen, bijvoorbeeld beheren:

* Nieuwe werkbladen en tabellen maken.
* Opvragen en beheren van werkbladen, tabellen en rijen.
* Voeg enkele rijen en sleutelkolommen.

U kunt vervolgens de uitvoer van deze acties gebruiken met de acties die voor andere services. Bijvoorbeeld, als u een actie die werkbladen elke week worden gemaakt, kunt u nog een actie waarmee bevestigingse-mail verzonden via de Office 365 Outlook-connector.

Als u geen ervaring met logische apps, raadpleegt u [wat is Azure Logic Apps?](../logic-apps/logic-apps-overview.md)

> [!NOTE]
> De [Excel Online voor bedrijven](/connectors/excelonlinebusiness/) en [Excel Online voor OneDrive](/connectors/excelonline/) connectors werken met Azure Logic Apps en verschillen van de [Excel-connector voor PowerApps](/connectors/excel/).

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, <a href="https://azure.microsoft.com/free/" target="_blank">registreer u dan nu voor een gratis Azure-account</a>.

* Een [Office 365-account](https://www.office.com/) voor uw werkaccount of persoonlijk Microsoft-account

  Uw Excel-gegevens kan bestaan als een bestand met door komma's gescheiden waarden (CSV) in een opslagmap, bijvoorbeeld in OneDrive. 
  U kunt ook de dezelfde CSV-bestand met de [platte connector](../logic-apps/logic-apps-enterprise-integration-flatfile.md).

* Basiskennis over [over het maken van logische apps](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* De logische app waar u toegang tot uw gegevens Excel Online. Deze connector beschikt u alleen acties, om de logische App, selecteert u een afzonderlijke trigger, bijvoorbeeld, de **terugkeerpatroon** trigger.

## <a name="add-excel-action"></a>Excel-actie toevoegen

1. In de [Azure-portal](https://portal.azure.com), opent u uw logische app in de ontwerper van logische App, als dit niet al geopend.

1. Kies onder de trigger **nieuwe stap**.

1. Typ 'excel' als filter in het zoekvak. Selecteer de actie die u wilt onder de lijst met acties.

1. Als u hierom wordt gevraagd om aan te melden bij uw Office 365-account, kiest u **aanmelden**.

   Uw referenties toestaan dat uw logische app te maken van een verbinding met Excel Online toegang tot uw gegevens.

1. Verder biedt de benodigde informatie voor de geselecteerde actie en het bouwen van uw logic app-werkstroom.

## <a name="connector-reference"></a>Connector-verwijzing

Voor technische informatie, zoals triggers en acties limieten, zoals is beschreven in van de connector OpenAPI (voorheen Swagger)-bestanden, Zie de referentiepagina's van deze connector:

* [Excel Online voor bedrijven](/connectors/excelonlinebusiness/)
* [Excel Online voor OneDrive](/connectors/excelonline/)

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de [website voor feedback van Logic Apps-gebruikers](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over andere [Logic Apps-connectors](../connectors/apis-list.md)
