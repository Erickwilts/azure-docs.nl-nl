---
title: Configureren van het dashboard Azure IoT Central | Microsoft Docs
description: Als een opbouwfunctie voor expressies, informatie over het configureren van het standaarddashboard voor de Azure IoT Central-toepassing.
author: dominicbetts
ms.author: dobett
ms.date: 02/13/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: b1edd4777839c947cf8f60bb6857f939553e2603
ms.sourcegitcommit: e6d53649bfb37d01335b6bcfb9de88ac50af23bd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/09/2019
ms.locfileid: "65467635"
---
# <a name="configure-the-application-dashboard"></a>Configureren van het dashboard

De **Dashboard** is de pagina waarop wordt geladen wanneer gebruikers die toegang tot de toepassing hebben gaat u naar de URL van de toepassing. Als u een geselecteerd de **voorbeeld Contoso** of **voorbeeld Devkits** toepassingssjabloon voor het maken van uw toepassing, uw toepassing heeft een vooraf gedefinieerde dashboard. Als u ervoor kiest de **aangepaste toepassing** toepassingssjabloon, uw dashboard is leeg.

> [!NOTE]
> Gebruikers kunnen ook [hun eigen persoonlijke dashboards maken](howto-personalize-dashboard.md) gebruiken in plaats van het dashboard van de toepassing standaard.

## <a name="add-tiles"></a>Tegels toevoegen

De volgende schermafbeelding ziet u het dashboard in een toepassing gemaakt op basis van de **voorbeeld Contoso** sjabloon. Voor het aanpassen van de standaard-dashboard voor uw toepassing, selecteert u **bewerken** aan de bovenkant van de pagina.

![Dashboard voor toepassingen op basis van de sjabloon 'Contoso voorbeeld'](media/howto-configure-homepage/image1a.png)

Selecteren **bewerken**, wordt het deelvenster van de bibliotheek dashboard geopend. De bibliotheek bevat de tegels en dashboard primitieven die kunt u het dashboard aanpassen.

![Bibliotheek voor het dashboard](media/howto-configure-homepage/image2a.png)

Bijvoorbeeld, u kunt toevoegen een **apparaatinstellingen en eigenschappen** tegel om een selectie van de huidige instellingen en eigenschappen van de waarden voor een apparaat weer te geven. Om dit te doen, selecteert u eerst een **apparaat sjabloon** Selecteer vervolgens een **apparaatexemplaar**. Nadat u dat de tegel geeft een titel en selecteer een **instelling** of een **eigenschap** om weer te geven. De volgende schermafbeelding ziet u instellingen en eigenschappen die zijn geselecteerd om toe te voegen aan de tegel. Selecteer **gedaan** om op te slaan van de wijziging aan het dashboard.

!['Apparaatdetails configureren' formulier met details voor instellingen en eigenschappen](media/howto-configure-homepage/image3a.png)

Nu als een operator het standaarddashboard toepassing bekijkt, ze de nieuwe tegel met zien de **ingesteld temperatuur** instellen voor het apparaat:

![Tabblad met de weergegeven instellingen en eigenschappen voor de tegel 'Dashboard'](media/howto-configure-homepage/image4a.png)

U kunt andere tegeltypen in de bibliotheek om te ontdekken hoe u de toepassing standaarddashboard verder aanpassen verkennen.

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd over het configureren van uw Azure IoT Central standaarddashboard van toepassing, kunt u het volgende doen:

> [!div class="nextstepaction"]
> [Leer hoe u installatiekopieën voorbereidt en uploadt](howto-prepare-images.md)
