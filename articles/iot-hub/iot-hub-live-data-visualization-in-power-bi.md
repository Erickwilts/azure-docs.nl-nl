---
title: Realtime gegevensvisualisatie van sensorgegevens uit Azure IoT Hub-Power BI | Microsoft Docs
description: Power BI gebruiken om temperatuur en vochtigheid gegevens die worden verzameld van de sensor en verzonden naar uw Azure-IoT-hub te visualiseren.
author: robinsh
keywords: realtime gegevensvisualisatie, live gegevensvisualisatie, gegevensvisualisatie sensor
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 4/11/2018
ms.author: robinsh
ms.openlocfilehash: 3e932048b41e9af149f14a814a1c92d86bd26f29
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66479865"
---
# <a name="visualize-real-time-sensor-data-from-azure-iot-hub-using-power-bi"></a>Real-time sensorgegevens uit Azure IoT Hub met behulp van Power BI visualiseren

![Diagram voor end-to-end](./media/iot-hub-live-data-visualization-in-power-bi/1_end-to-end-diagram.png)


[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a>Wat u leert

U informatie over het visualiseren van realtime-sensorgegevens die uw Azure-IoT-hub ontvangt via Power BI. Als u wilt proberen toe visualiseren van de gegevens in uw IoT-hub met een web-app, raadpleegt u [een web-app gebruiken voor het visualiseren van realtime-sensorgegevens uit Azure IoT Hub](iot-hub-live-data-visualization-in-web-apps.md).

## <a name="what-you-do"></a>Wat u allemaal doen

* Bereid u voor uw IoT-hub voor toegang tot gegevens door toe te voegen een consumergroep.

* Maken, configureren en uitvoeren van een Stream Analytics-taak voor overdracht van gegevens van uw IoT-hub aan uw Power BI-account.

* Maken en publiceren van een Power BI-rapport om de gegevens te visualiseren.

## <a name="what-you-need"></a>Wat u nodig hebt

* Voltooi de [Raspberry Pi online simulator](iot-hub-raspberry-pi-web-simulator-get-started.md) zelfstudie of een van de apparaat-zelfstudies, bijvoorbeeld [Raspberry Pi met node.js](iot-hub-raspberry-pi-kit-node-get-started.md). Deze betrekking hebben op de volgende vereisten:
  
  * Een actief Azure-abonnement.
  * Een Azure IoT-hub in uw abonnement.
  * Een clienttoepassing die berichten naar uw Azure-IoT-hub verzendt.

* Een Power BI-account. ([Power BI gratis proberen](https://powerbi.microsoft.com/))

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="create-configure-and-run-a-stream-analytics-job"></a>Maken, configureren en uitvoeren van een Stream Analytics-taak

Laten we beginnen met het maken van een Stream Analytics-taak. Nadat u de taak hebt gemaakt, definieert u de invoer, uitvoer en de query die wordt gebruikt om de gegevens te halen.

### <a name="create-a-stream-analytics-job"></a>Een Stream Analytics-taak maken

1. Klik [in de Azure Portal](https://portal.azure.com) op **Een resource maken** > **Internet of Things** > **Stream Analytics-taak**.

2. Voer de volgende informatie in voor de taak.

   **Taaknaam**: De naam van de taak. De naam moet wereldwijd uniek zijn.

   **Resourcegroep**: Gebruik dezelfde resourcegroep bevinden die gebruikmaakt van uw IoT-hub.

   **Locatie**: Gebruik de dezelfde locatie als uw resourcegroep.

   **Vastmaken aan dashboard**: Schakel deze optie voor eenvoudige toegang voor uw IoT-hub vanuit het dashboard.

   ![Een Stream Analytics-taak maken in Azure](./media/iot-hub-live-data-visualization-in-power-bi/2_create-stream-analytics-job-azure.png)

3. Klik op **Create**.

### <a name="add-an-input-to-the-stream-analytics-job"></a>Een invoer aan de Stream Analytics-taak toevoegen

1. Open de Stream Analytics-taak.

2. Klik onder **Taaktopologie** op **Invoer**.

3. In de **invoer** deelvenster, klikt u op **Stroominvoer toevoegen**, en voer de volgende informatie:

   **Invoeralias**: De unieke alias voor de invoer en selecteer **bieden IoT Hub-instellingen handmatig** hieronder.

   **Bron**: Selecteer **IoT-hub**.
   
   **Eindpunt**: Klik op **Messaging**.

   **Consumentengroep**: Selecteer de consumergroep die u zojuist hebt gemaakt.

4. Klik op **Create**.

   ![Invoer voor een Stream Analytics-taak toevoegen in Azure](./media/iot-hub-live-data-visualization-in-power-bi/3_add-input-to-stream-analytics-job-azure.png)

### <a name="add-an-output-to-the-stream-analytics-job"></a>Een uitvoer aan de Stream Analytics-taak toevoegen

1. Klik onder **Taaktopologie** op **Uitvoer**.

2. In de **uitvoer** deelvenster, klikt u op **toevoegen** en **Power BI**, en voer de volgende informatie:

   **Uitvoeralias**: De alias die uniek is voor de uitvoer.

   **Werkruimte groep**: Selecteer de groepswerkruimte van uw doel.

   **Naam van de gegevensset**: Voer een naam van de gegevensset.

   **Tabelnaam**: Voer een tabelnaam in.

3. Klik op **autoriseren**, en vervolgens meldt u zich bij uw Power BI-account.

4. Klik op **Create**.

   ![Uitvoer toevoegen aan een Stream Analytics-taak in Azure](./media/iot-hub-live-data-visualization-in-power-bi/4_add-output-to-stream-analytics-job-azure.png)

### <a name="configure-the-query-of-the-stream-analytics-job"></a>De query van de Stream Analytics-taak configureren

1. Klik onder **Taaktopologie** op **Query**.

2. Vervang `[YourInputAlias]` door de invoeralias van de taak.

3. Vervang `[YourOutputAlias]` door de uitvoeralias van de taak.

4. Klik op **Opslaan**.

   ![Een query toevoegen aan een Stream Analytics-taak in Azure](./media/iot-hub-live-data-visualization-in-power-bi/5_add-query-stream-analytics-job-azure.png)

### <a name="run-the-stream-analytics-job"></a>De Stream Analytics-taak uitvoeren

Klik in de Stream Analytics-taak op **Start** > **Nu** > **Start**. Zodra de taak kan worden gestart, wordt de taakstatus veranderd van **Gestopt** naar **In uitvoering**.

![Een Stream Analytics-taak uitvoeren in Azure](./media/iot-hub-live-data-visualization-in-power-bi/6_run-stream-analytics-job-azure.png)

## <a name="create-and-publish-a-power-bi-report-to-visualize-the-data"></a>Maken en publiceren van een Power BI-rapport om de gegevens te visualiseren

1. Controleer of dat de voorbeeldtoepassing wordt uitgevoerd op uw apparaat. Als u niet het geval is, kunt u verwijzen naar de zelfstudies onder [instellen van uw apparaat](https://docs.microsoft.com/azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started).

2. Meld u aan bij uw [Power BI](https://powerbi.microsoft.com/en-us/)-account.

3. Klik op de werkruimte die u hebt gebruikt, **mijn werkruimte**.

4. Klik op **Gegevenssets**.

   U ziet de gegevensset die u hebt opgegeven tijdens het maken van de uitvoer voor de Stream Analytics-taak.

5. Voor de gegevensset die u hebt gemaakt, klikt u op **rapport toevoegen** (het eerste pictogram aan de rechterkant van de naam van de gegevensset).

   ![Een Microsoft Power BI-rapport maken](./media/iot-hub-live-data-visualization-in-power-bi/7_create-power-bi-report-microsoft.png)

6. Maak een lijndiagram om in realtime de temperatuur gedurende een bepaalde periode weer te geven.

   1. Voeg op de rapportpagina maken, een lijndiagram.

   2. Klap in het deelvenster **Velden** de tabel uit die u hebt opgegeven toen u de uitvoer voor de Stream Analytics-taak hebt gemaakt.
   
   3. Sleep **EventEnqueuedUtcTime** naar **As** in het deelvenster **Visualisaties**.
   
   4. Sleep **Temperatuur** naar **Waarden**.

      Er wordt een lijndiagram gemaakt. De x-as geeft de datum en tijd in UTC-tijdzone aan. De y-as geeft de temperatuur van de sensor aan.

      ![Een lijndiagram voor temperatuur toevoegen aan een Microsoft Power BI-rapport](./media/iot-hub-live-data-visualization-in-power-bi/8_add-line-chart-for-temperature-to-power-bi-report-microsoft.png)

7. Maak een ander lijndiagram om in realtime de vochtigheid gedurende een bepaalde periode weer te geven. Volg de bovenstaande dezelfde stappen om dit te doen, en plaats **EventEnqueuedUtcTime** op de x-as en **vochtigheid** op de y-as.

   ![Een lijndiagram voor de vochtigheid toevoegen aan een Microsoft Power BI-rapport](./media/iot-hub-live-data-visualization-in-power-bi/9_add-line-chart-for-humidity-to-power-bi-report-microsoft.png)

8. Klik op **Opslaan** om het rapport op te slaan.

9. Klik op **rapporten** in het linkerdeelvenster en klik vervolgens op het rapport dat u zojuist hebt gemaakt.

10. Klik op **bestand** > **publiceren op Internet**.

11. Klik op **invoegcode maken**, en klik vervolgens op **publiceren**.

Krijgt u de koppeling naar het rapport dat u met iedereen voor toegang tot rapporten en een codefragment delen kunt voor het integreren van het rapport in uw blog of website.

![Een Microsoft Power BI-rapport publiceren](./media/iot-hub-live-data-visualization-in-power-bi/10_publish-power-bi-report-microsoft.png)

Microsoft biedt ook de [mobiele Power BI-apps](https://powerbi.microsoft.com/en-us/documentation/powerbi-power-bi-apps-for-mobile-devices/) voor het weergeven en interactie met uw Power BI-dashboards en rapporten op uw mobiele apparaat.

## <a name="next-steps"></a>Volgende stappen

U hebt Power BI is gebruikt voor het visualiseren van realtime-sensorgegevens uit uw Azure-IoT-hub.

Zie voor een andere manier om gegevens van Azure IoT Hub te visualiseren, [een web-app gebruiken voor het visualiseren van realtime-sensorgegevens uit Azure IoT Hub](iot-hub-live-data-visualization-in-web-apps.md).

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
