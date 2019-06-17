---
title: Stream Analytics gegevens in het Edge-taken in Azure Stream Analytics-hulpprogramma's voor Visual Studio
description: In dit artikel wordt beschreven hoe u schrijven, fouten opsporen en uw Stream Analytics gegevens in het Edge-taken met behulp van de Stream Analytics-hulpprogramma's voor Visual Studio maken.
services: stream-analytics
author: su-jie
ms.author: sujie
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/07/2018
ms.custom: seodec18
ms.openlocfilehash: 242fb2daebfe9eb6e5a0c73c2c4c0e91a3131032
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66304162"
---
# <a name="develop-stream-analytics-data-box-edge-jobs-using-visual-studio-tools"></a>Stream Analytics-gegevens in het Edge-taken met behulp van Visual Studio-hulpprogramma's ontwikkelen

In deze zelfstudie leert u hoe u Stream Analytics-hulpprogramma's voor Visual Studio. Leert u hoe u schrijven, fouten opsporen en uw Stream Analytics gegevens in het Edge-taken maken. Nadat u maken en testen van de taak, gaat u naar de Azure portal om te implementeren op uw apparaten. 

## <a name="prerequisites"></a>Vereisten

U moet de volgende vereisten om deze zelfstudie te voltooien:

* Installeer [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/), [Visual Studio 2015](https://www.visualstudio.com/vs/older-downloads/), of [Visual Studio 2013 Update 4](https://www.microsoft.com/download/details.aspx?id=45326). Enterprise- (Ultimate/Premium), Professional- en Community-edities worden ondersteund. Express-editie wordt niet ondersteund.  

* Ga als volgt de [installatie-instructies](stream-analytics-tools-for-visual-studio-edge-jobs.md) Stream Analytics-hulpprogramma's voor Visual Studio installeren.
 
## <a name="create-a-stream-analytics-data-box-edge-project"></a>Een Stream Analytics-gegevens in het Edge-project maken 

Selecteer in Visual Studio, **bestand** > **nieuw** > **Project**. Navigeer naar de **sjablonen** lijst aan de linkerkant > Vouw **Azure Stream Analytics** > **Stream Analytics Edge**  >   **Azure Stream Analytics Edge-toepassing**. Geef een naam voor uw project en selecteer op de naam, locatie en oplossing **OK**.

![Nieuwe Stream Analytics-gegevens in het Edge-project in Visual Studio](./media/stream-analytics-tools-for-visual-studio-edge-jobs/new-stream-analytics-edge-project.png)

Nadat het project wordt gemaakt, gaat u naar de **Solution Explorer** om de mappenhiërarchie weer te geven.

![Solution explorer-weergave van Stream Analytics-gegevens in het Edge-taak](./media/stream-analytics-tools-for-visual-studio-edge-jobs/edge-project-in-solution-explorer.png)

 
## <a name="choose-the-correct-subscription"></a>Kies het juiste abonnement

1. Vanuit uw Visual Studio **weergave** in het menu **Server Explorer**.  

2. Klik met de rechtermuisknop op **Azure** > Selecteer **verbinding maken met Microsoft Azure-abonnement** > en meld u aan met uw Azure-account.

## <a name="define-inputs"></a>Invoer definiëren

1. Uit de **Solution Explorer**, vouw de **invoer** knooppunt ziet u een invoer met de naam **EdgeInput.json**. Dubbelklik om de instellingen ervan weer te geven.  

2. Brontype ingesteld op **gegevens Stream**. Bron wordt ingesteld op **Edge Hub**, gebeurtenis serialisatie-indeling naar **Json**, en coderen naar **UTF8**. (Optioneel) u kunt de naam van de **invoer Alias**, laten we dit zo voor dit voorbeeld. Als u de naam van de ingevoerde alias, gebruikt u de naam die u hebt opgegeven bij het definiëren van de query. Selecteer **Opslaan** om de instellingen op te slaan.  
   ![Stream Analytics-taak invoer-configuratie](./media/stream-analytics-tools-for-visual-studio-edge-jobs/stream-analytics-input-configuration.png)
 


## <a name="define-outputs"></a>Uitvoer definiëren

1. Uit de **Solution Explorer**, vouw de **uitvoer** knooppunt ziet u uitvoer met de naam **EdgeOutput.json**. Dubbelklik om de instellingen ervan weer te geven.  

2. Zorg ervoor dat u het instellen van Sink selecteren **Edge Hub**, serialisatie-indeling voor gebeurtenissen ingesteld op **Json**, stel coderen naar **UTF8**, en stel de indeling van **matrix**. (Optioneel) u kunt de naam van de **uitvoeralias**, laten we dit zo voor dit voorbeeld. Als u de naam van de uitvoeralias, gebruikt u de naam die u hebt opgegeven bij het definiëren van de query. Selecteer **Opslaan** om de instellingen op te slaan. 
   ![Stream Analytics job output-configuratie](./media/stream-analytics-tools-for-visual-studio-edge-jobs/stream-analytics-output-configuration.png)
 
## <a name="define-the-transformation-query"></a>De transformatiequery definiëren

Stream Analytics-taken geïmplementeerd in de Stream Analytics-gegevens in het Edge-omgevingen ondersteunen de meeste [: referentie voor querytaal voor Stream Analytics](https://msdn.microsoft.com/azure/stream-analytics/reference/stream-analytics-query-language-reference?f=255&MSPPError=-2147217396). De volgende bewerkingen worden niet echter nog ondersteund voor Stream Analytics gegevens in het Edge-taken: 


|**Categorie**  | **Opdracht**  |
|---------|---------|
|Georuimtelijke operators |<ul><li>CreatePoint</li><li>CreatePolygon</li><li>CreateLineString</li><li>ST_DISTANCE</li><li>ST_WITHIN</li><li>ST_OVERLAPS</li><li>ST_INTERSECTS</li></ul> |
|Andere operatoren | <ul><li>PARTITION BY</li><li>TIJDSTEMPEL DOOR VIA</li><li>DISTINCT</li><li>De expressieparameter in de COUNT-operator</li><li>Wachttijden van microseconden in datum- en tijdfuncties</li><li>JavaScript-UDA (deze functie is nog in preview voor taken die zijn geïmplementeerd in de cloud)</li></ul>   |

Wanneer u een Stream Analytics gegevens in het Edge-taak in de portal maakt, waarschuwt de compiler automatisch u als u een ondersteunde operator niet gebruikt.

Vanuit uw Visual Studio, kunt u de volgende transformatiequery definiëren in de query-editor (**script.asaql bestand**)

```sql
SELECT * INTO EdgeOutput
FROM EdgeInput 
```

## <a name="test-the-job-locally"></a>De taak lokaal testen

Als u wilt testen van de query moet u lokaal de voorbeeldgegevens uploaden. U kunt voorbeeldgegevens krijgen door het downloaden van de registratie van de [GitHub-opslagplaats](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/Registration.json) en sla deze op uw lokale computer. 

1. Als u wilt uploaden voorbeeldgegevens, klik met de rechtermuisknop op **EdgeInput.json** -bestand en kies **lokale invoer toevoegen**  

2. In het pop-upvenster > **Bladeren** de voorbeeldgegevens uit het lokale pad > Selecteer **opslaan**.
   ![Lokale invoer configuratie in Visual Studio](./media/stream-analytics-tools-for-visual-studio-edge-jobs/stream-analytics-local-input-configuration.png)
 
3. Een bestand met de naam **local_EdgeInput.json** wordt automatisch toegevoegd aan de map invoer.  
4. u kunt deze lokaal uitvoeren of verzenden naar Azure. Als u wilt testen van de query, selecteert u **lokaal uitvoeren**.  
   ![Stream Analytics-taak opties voor het uitvoeren in Visual Studio](./media/stream-analytics-tools-for-visual-studio-edge-jobs/stream-analytics-visual-stuidio-run-options.png)
 
5. Het opdrachtpromptvenster toont de status van de taak. Wanneer de taak is uitgevoerd, maakt een map die als '2018-02-23-11-31-42 eruitziet' in het pad voor de project 'Visual Studio 2015\Projects\MyASAEdgejob\MyASAEdgejob\ASALocalRun\2018-02-23-11-31-42'. Navigeer naar het pad om de resultaten in de lokale map weer te geven:

   U kunt ook aanmelden bij de Azure-portal en controleer of de taak is gemaakt. 

   ![Stream Analytics-taak resultaat-map](./media/stream-analytics-tools-for-visual-studio-edge-jobs/stream-analytics-job-result-folder.png)

## <a name="submit-the-job-to-azure"></a>Verzenden van de taak naar Azure

1. Voordat u de taak naar Azure verzenden, moet u verbinding met uw Azure-abonnement. Open **Server Explorer** > Klik met de rechtermuisknop op **Azure** > **verbinding maken met Microsoft Azure-abonnement** > aanmelden bij uw Azure-abonnement.  

2. Als u wilt verzenden van de taak naar Azure, gaat u naar de query-editor > Selecteer **verzenden naar Azure**.  

3. Er wordt een pop-upvenster geopend. Kies een bestaande Stream Analytics gegevens in het Edge-taak bijwerken of een nieuwe maken. Wanneer u een bestaande taak bijwerken, taakconfiguratie, in dit scenario wordt vervangen, publiceert u een nieuwe taak. Selecteer **een nieuwe Azure Stream Analytics-taak maken** > Voer een naam voor uw taak iets zoals **MyASAEdgeJob** > Kies de vereiste **abonnement**, **Resourcegroep**, en **locatie** > Selecteer **indienen**.

   ![Stream Analytics-taak verzenden naar Azure vanuit Visual Studio](./media/stream-analytics-tools-for-visual-studio-edge-jobs/submit-stream-analytics-job-to-azure.png)
 
   Nu is uw Stream Analytics gegevens in het Edge-taak gemaakt. U kunt verwijzen naar de [taken uitvoeren in IoT Edge-zelfstudie](stream-analytics-edge.md) voor informatie over het implementeren op uw apparaten. 

## <a name="manage-the-job"></a>Beheren van de taak 

U kunt de status van de taak en het taakdiagram in Server Explorer bekijken. Van **Stream Analytics** in **Server Explorer**, vouw het abonnement en de resourcegroep waar u de Stream Analytics gegevens in het Edge-taak hebt geïmplementeerd. U kunt de MyASAEdgejob met de status bekijken **gemaakt**. Vouw het taakknooppunt van uw uit en dubbelklik erop om de taakweergave te openen.

![Beheeropties voor server explorer-taak](./media/stream-analytics-tools-for-visual-studio-edge-jobs/server-explorer-options.png)
 
Het venster taak biedt u bewerkingen zoals het vernieuwen van de taak, de taak wordt verwijderd en de taak te openen vanuit Azure portal.

![Taakdiagram en andere opties in Visual Studio](./media/stream-analytics-tools-for-visual-studio-edge-jobs/job-diagram-and-other-options.png) 

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over Azure IoT Edge](../iot-edge/about-iot-edge.md)
* [ASA on IoT Edge-zelfstudie](../iot-edge/tutorial-deploy-stream-analytics.md)
* [Feedback verzenden naar het team met behulp van deze enquête](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR2czagZ-i_9Cg6NhAZlH9ypUMjNEM0RDVU9CVTBQWDdYTlk0UDNTTFdUTC4u) 
