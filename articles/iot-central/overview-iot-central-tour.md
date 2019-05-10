---
title: Kennismaking met de gebruikersinterface van Azure IoT Central | Microsoft Docs
description: Als maker is het nuttig om vertrouwd te raken met de belangrijkste gebieden van de gebruikersinterface van Azure IoT Central, dat u gebruikt om een IoT-oplossing te maken.
author: dominicbetts
ms.author: dobett
ms.date: 01/24/2019
ms.topic: overview
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: philmea
ms.openlocfilehash: 8a0621d0261bfbc7ab396abf837ee7b1123352d1
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2019
ms.locfileid: "65233441"
---
# <a name="take-a-tour-of-the-azure-iot-central-ui"></a>Kennismaking met de gebruikersinterface van Azure IoT Central

In dit artikel maakt u kennis met de gebruikersinterface van Microsoft Azure IoT Central. U kunt de gebruikersinterface gebruiken om een Azure IoT Central-oplossing te maken en de daarmee verbonden apparaten te beheren en gebruiken.

Als _maker_ gebruikt u de gebruikersinterface van Azure IoT Central om uw Azure IoT Central-oplossing te definiëren. U kunt de gebruikersinterface gebruiken om:

- De typen apparaten te definiëren die met uw oplossing verbinding maken.
- De regels en acties voor uw apparaten te configureren.
- De gebruikersinterface aan te passen voor een _operator_ die uw oplossing gebruikt.

Als _operator_ gebruikt u de gebruikersinterface van Azure IoT Central om uw Azure IoT Central-oplossing te beheren. U kunt de gebruikersinterface gebruiken om:

- Uw apparaten te controleren.
- Uw apparaten te configureren.
- Problemen met uw apparaten op te lossen.
- Nieuwe apparaten inrichten.

## <a name="use-the-left-navigation-menu"></a>Gebruik het linkernavigatiemenu

Gebruik het navigatiemenu links voor toegang tot de verschillende gebieden van de toepassing. U kunt uitvouwen of samenvouwen van de navigatiebalk hiervoor **<** of **>**:

| Menu | Description |
| ---- | ----------- |
| ![Linkernavigatiemenu](media/overview-iot-central-tour/navigationbar.png) | <ul><li>De **Dashboard** knop geeft het toepassingsdashboard van uw. Als een opbouwfunctie voor expressies, kunt u het dashboard aanpassen voor uw operators. Gebruikers kunnen ook hun eigen dashboards maken.</li><li>Met de knop **Device Explorer** ziet u een lijst met de gesimuleerde en echte apparaten die aan elke apparaatsjabloon in de toepassing zijn gekoppeld. Als operator gebruikt u de **Device Explorer** om uw verbonden apparaten te beheren.</li><li>Met de knop **Apparaatsets** kunt u apparaatsets bekijken en maken. Als operator kunt u de apparaatsets maken als een logische verzameling apparaten die door een query wordt gespecificeerd.</li><li>Met de knop **Analyse** worden analytische gegevens weergegeven die zijn afgeleid van telemetriegegevens van apparaten en apparaatsets. Als operator kunt u uw apparaatgegevens aangepast weergeven voor meer inzicht in uw toepassing.</li><li>De knop **Taken** schakelt bulksgewijs beheer van apparaten doordat u taken kunt maken en uitvoeren voor het uitvoeren van updates op schaal.</li><li>De knop **Apparaatsjablonen** geeft de hulpprogramma's weer die een opbouwfunctie gebruikt om apparaatsjablonen te maken en beheren.</li><li>Met de knop **Continue gegevensexport** kan een beheerder een continue export configureren naar andere Azure-services zoals opslag en wachtrijen.</li><li>Met de knop **Beheer** worden de beheerpagina's van de toepassing weergegeven waar een administrator de instellingen, gebruikers en rollen van een toepassing kan beheren.</li></ul> |

## <a name="search-help-and-support"></a>Zoeken, hulp en ondersteuning

Het bovenste menu wordt op elke pagina weergegeven:

![Werkbalk](media/overview-iot-central-tour/toolbar.png)

- Als u wilt zoeken naar apparaatsjablonen en apparaten, voer een **zoeken** waarde.
- Kies de taal van de gebruikersinterface of thema wilt wijzigen, de **instellingen** pictogram.
- Als u wilt afmelden bij de toepassing, kiest u de **Account** pictogram.
- Als u hulp en ondersteuning nodig hebt, kiest u het keuzemenu **Help** voor een lijst met bronnen. In een proefversie, de bronnen voor ondersteuning hebben toegang tot [live chat](howto-show-hide-chat.md).

Voor de gebruikersinterface kunt u kiezen tussen een licht thema of een donker thema:

![Een thema voor de gebruikersinterface kiezen](media/overview-iot-central-tour/themes.png)

> [!NOTE]
> De optie te kiezen tussen het lichte en donkere thema's is niet beschikbaar als de beheerder een aangepast thema voor de toepassing heeft geconfigureerd.

## <a name="dashboard"></a>Dashboard

![Dashboard](media/overview-iot-central-tour/homepage.png)

Het dashboard is de eerste pagina die u ziet wanneer u zich aanmeldt bij uw Azure IoT Central-toepassing. Als een opbouwfunctie voor expressies, kunt u het dashboard voor andere gebruikers door toe te voegen, tegels aanpassen. Raadpleeg voor meer informatie de zelfstudie [Customize the Azure IoT Central operator's view](tutorial-customize-operator.md) (De operatorweergave van Azure IoT Central aanpassen). Gebruikers kunnen ook [hun eigen persoonlijke dashboards maken](howto-personalize-dashboard.md).

## <a name="device-explorer"></a>Apparatenverkenner

![Device Explorer-pagina](media/overview-iot-central-tour/explorer.png)

Op de Device Explorer-pagina worden de _apparaten_ in uw Azure IoT Central-toepassing gegroepeerd weergegeven op _apparaatsjabloon_.

* Met een apparaatsjabloon wordt een type apparaat gedefinieerd dat met uw toepassing verbinding kan maken. Raadpleeg voor meer informatie de zelfstudie [Define a new device type in your Azure IoT Central application](tutorial-define-device-type.md) (Een nieuw apparaattype definiëren in uw Azure IoT Central-toepassing).
* Een apparaat vertegenwoordigt een echt of een gesimuleerd apparaat in uw toepassing. Raadpleeg voor meer informatie de zelfstudie [Add a new device to your Azure IoT Central application](tutorial-add-device.md) (Een nieuw apparaat toevoegen aan uw Azure IoT Central-toepassing).

## <a name="device-sets"></a>Apparaatsets

![Apparaatsets-pagina](media/overview-iot-central-tour/devicesets.png)

Op de pagina _Apparaatsets_ worden de apparaatsets getoond die door de maker zijn gemaakt. Een apparaatset is een verzameling verwante apparaten. Een maker definieert een query om de apparaten te identificeren die in een apparaatset zijn opgenomen. U gebruikt apparaatsets wanneer u de analytische gegevens in uw toepassing aanpast. Raadpleeg voor meer informatie het artikel [Use device sets in your Azure IoT Central application](howto-use-device-sets.md) (Apparaatsets gebruiken in uw Azure IoT Central-toepassing).

## <a name="analytics"></a>Analytische gegevens

![Analysepagina](media/overview-iot-central-tour/analytics.png)

De pagina Analyse bevat diagrammen met informatie over hoe de apparaten die met uw toepassing zijn verbonden, zich gedragen. Een operator gebruikt deze pagina om problemen met de verbonden apparaten te controleren en te onderzoeken. De maker kan de diagrammen die op deze pagina worden weergegeven, definiëren. Raadpleeg voor meer informatie het artikel [Create custom analytics for your Azure IoT Central application](howto-use-device-sets.md) (Aangepaste analyses maken voor uw Azure IoT Central-toepassing).

## <a name="jobs"></a>Taken

![Pagina Taken](media/overview-iot-central-tour/jobs.png)

Met de taakpagina kunt u bulksgewijze beheerbewerkingen van uw apparaten uitvoeren. De opbouwfunctie maakt gebruik van deze pagina om apparaateigenschappen, instellingen en opdrachten bij te werken. Zie voor meer informatie, het artikel [Een taak uitvoeren](howto-run-a-job.md).

## <a name="device-templates"></a>Apparaatsjablonen

![De pagina Apparaatsjablonen](media/overview-iot-central-tour/templates.png)

De opbouwfunctie gebruikt de pagina Apparaatsjablonen om de apparaatsjablonen in de toepassing te maken en beheren. Raadpleeg voor meer informatie de zelfstudie [Define a new device type in your Azure IoT Central application](tutorial-define-device-type.md) (Een nieuw apparaattype definiëren in uw Azure IoT Central-toepassing).

## <a name="continuous-data-export"></a>Continue gegevensexport

![Pagina Continue gegevensexport](media/overview-iot-central-tour/export.png)

De beheerder gebruikt de pagina Continue gegevensexport om het exporteren van gegevens, zoals de telemetrie, uit de toepassing te definiëren. Andere services kunnen de geëxporteerde gegevens opslaan of die gebruiken voor analysedoeleinden. Zie het artikel [Exporteren van gegevens in Azure IoT Central](howto-export-data.md) voor meer informatie.

## <a name="administration"></a>Beheer

![Beheerpagina](media/overview-iot-central-tour/administration.png)

De beheerpagina bevat koppelingen naar hulpprogramma's die een administrator gebruikt, zoals het definiëren van gebruikers en rollen in de toepassing. Raadpleeg voor meer informatie het artikel [Administer your Azure IoT Central application](howto-administer.md) (Uw Azure IoT Central-toepassing beheren).

## <a name="next-steps"></a>Volgende stappen

Nu u een overzicht hebt van Azure IoT Central en bekend bent met de indeling van de gebruikersinterface, is de voorgestelde vervolgstap om de snelstart [Create an Azure IoT Central application](quick-deploy-iot-central.md) (Een Azure IoT Central-toepassing maken) te voltooien.