---
title: Gegevens laden in Azure Data Lake Storage Gen2 met Azure Data Factory
description: Azure Data Factory gebruiken om gegevens te kopiëren in Azure Data Lake Storage Gen2
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 05/13/2019
ms.author: jingwang
ms.openlocfilehash: f8af34207eddb613f7a59bd3e3d300555e10f985
ms.sourcegitcommit: 179918af242d52664d3274370c6fdaec6c783eb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/13/2019
ms.locfileid: "65560736"
---
# <a name="load-data-into-azure-data-lake-storage-gen2-with-azure-data-factory"></a>Gegevens laden in Azure Data Lake Storage Gen2 met Azure Data Factory

Azure Data Lake Storage Gen2 is een verscheidenheid aan functies die zijn toegewezen aan de analyse van big data, die is ingebouwd in [Azure Blob-opslag](../storage/blobs/storage-blobs-introduction.md). Hiermee kunt u samenwerken met uw gegevens met behulp van beide beschermingsparadigma in het systeem en de object-opslag.

Azure Data Factory (ADF) is een volledig beheerde cloud-gebaseerde service voor gegevensintegratie. U kunt de service gebruiken voor het vullen van de lake met gegevens uit een grote verscheidenheid aan on-premises en cloud-gebaseerde gegevens worden opgeslagen en bespaar tijd bij het bouwen van uw analyseoplossingen. Voor een gedetailleerde lijst met ondersteunde connectors, Zie de tabel met [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats).

Azure Data Factory biedt een scale-out, beheerde oplossing voor gegevensverplaatsing. Vanwege de architectuur van de scale-out van ADF, kan deze gegevens met hoge doorvoer opnemen. Zie voor meer informatie, [prestaties van de Kopieeractiviteit](copy-activity-performance.md).

Dit artikel ziet u hoe u het hulpprogramma Data Factory Copy Data gebruiken om te laden van gegevens uit _Amazon Web Services-S3-service_ in _Azure Data Lake Storage Gen2_. U kunt uitvoeren van gelijksoortige stappen als u wilt kopiëren van gegevens van andere soorten gegevensarchieven.

>[!TIP]
>Raadpleeg voor het kopiëren van gegevens uit Azure Data Lake Storage Gen1 in Gen2, [dit specifieke scenario](load-azure-data-lake-storage-gen2-from-gen1.md).

## <a name="prerequisites"></a>Vereisten

* Azure-abonnement: Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/) aan voordat u begint.
* Azure Storage-account met Data Lake Storage Gen2 is ingeschakeld: Als u een Storage-account geen [maken van een account](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM).
* AWS-account met een S3-bucket die gegevens bevat: In dit artikel laat zien hoe het kopiëren van gegevens vanaf Amazon S3. U kunt andere gegevensarchieven gebruiken door gelijksoortige stappen te volgen.

## <a name="create-a-data-factory"></a>Een gegevensfactory maken

1. Selecteer in het menu links **een resource maken** > **gegevens en analyses** > **Data Factory**:
   
   ![Selectie van Data Factory in het deelvenster Nieuw](./media/quickstart-create-data-factory-portal/new-azure-data-factory-menu.png)

2. In de **nieuwe data factory** pagina, geef waarden op voor de velden die worden weergegeven in de volgende afbeelding: 
      
   ![De pagina Nieuwe data factory](./media/load-azure-data-lake-storage-gen2//new-azure-data-factory.png)
 
    * **Naam**: Voer een unieke naam op voor uw Azure-gegevensfactory. Als u de foutmelding ' naam Data factory \"LoadADLSDemo\" is niet beschikbaar, "Voer een andere naam voor de data factory. Bijvoorbeeld, kunt u de naam van de  _**uwnaam**_**ADFTutorialDataFactory**. Probeer het opnieuw maken van de data factory. Raadpleeg het onderwerp [Data Factory - Naamgevingsregels](naming-rules.md) voor meer informatie over naamgevingsregels voor Data Factory-artefacten.
    * **Abonnement**: Selecteer uw Azure-abonnement waarin u wilt maken van de data factory. 
    * **Resourcegroep**: Selecteer een bestaande resourcegroep in de vervolgkeuzelijst of Selecteer de **nieuw** optie en voer de naam van een resourcegroep. Zie [Resourcegroepen gebruiken om Azure-resources te beheren](../azure-resource-manager/resource-group-overview.md) voor meer informatie.  
    * **Version**: Selecteer **V2**.
    * **Locatie**: Selecteer de locatie voor de data factory. In de vervolgkeuzelijst worden alleen ondersteunde locaties weergegeven. De gegevensarchieven die worden gebruikt door data factory, kunnen zich in andere locaties of regio's. 

3. Selecteer **Maken**.
4. Na het aanmaken is voltooid, gaat u naar uw data factory. U ziet de **Data Factory** startpagina zoals wordt weergegeven in de volgende afbeelding: 
   
   ![Startpagina van de gegevensfactory](./media/load-azure-data-lake-storage-gen2/data-factory-home-page.png)

   Selecteer de **Author & Monitor** tegel om te starten van de toepassing voor gegevensintegratie in een afzonderlijk tabblad.

## <a name="load-data-into-azure-data-lake-storage-gen2"></a>Gegevens laden in Azure Data Lake Storage Gen2

1. In de **aan de slag** weergeeft, schakelt de **Copy Data** tegel om het hulpprogramma Copy Data starten: 

   ![De tegel Copy Data-hulpprogramma](./media/load-azure-data-lake-storage-gen2/copy-data-tool-tile.png)
2. In de **eigenschappen** pagina **CopyFromAmazonS3ToADLS** voor de **taaknaam** veld en selecteer **volgende**:

    ![De pagina Eigenschappen](./media/load-azure-data-lake-storage-gen2/copy-data-tool-properties-page.png)
3. In de **brongegevensarchief** pagina, klikt u op **+ nieuwe verbinding maken**:

    ![De pagina Brongegevensarchief](./media/load-azure-data-lake-storage-gen2/source-data-store-page.png)
    
    Selecteer **Amazon S3** uit de galerie met connector en selecteer **doorgaan**
    
    ![Pagina brongegevensarchief s3](./media/load-azure-data-lake-storage-gen2/source-data-store-page-s3.png)
    
4. In de **opgeven Amazon S3 verbinding** pagina, de volgende stappen uit:

   1. Geef de **toegangssleutel-ID** waarde.
   2. Geef de **geheime toegangssleutel** waarde.
   3. Klik op **verbinding testen** voor het valideren van de instellingen, selecteer **voltooien**.
   4. U ziet dat een nieuwe verbinding wordt gemaakt. Selecteer **Next**.
   
      ![Amazon S3-account opgeven](./media/load-azure-data-lake-storage-gen2/specify-amazon-s3-account.png)
      
5. In de **invoerbestand of invoermap kiezen** pagina, blader naar de map en elk bestand dat u wilt dat moeten worden gekopieerd. Selecteer de map/bestand, selecteer **kiezen**:

    ![Het invoerbestand of de invoermap kiezen](./media/load-azure-data-lake-storage-gen2/choose-input-folder.png)

6. Geef het gedrag op kopiëren door het controleren van de **bestanden kopiëren recursief** en **binaire kopie** opties. Selecteer **volgende**:

    ![Geef de map voor uitvoer](./media/load-azure-data-lake-storage-gen2/specify-binary-copy.png)
    
7. In de **doelgegevensarchief** pagina, klikt u op **+ nieuwe verbinding maken**, en selecteer vervolgens **Azure Data Lake Storage Gen2**, en selecteer **doorgaan**:

    ![De pagina Doelgegevensarchief](./media/load-azure-data-lake-storage-gen2/destination-data-storage-page.png)

8. In de **opgeven Azure Data Lake Storage verbinding** pagina, de volgende stappen uit:

   1. Selecteer uw Data Lake Storage Gen2 geschikt account van de "naam van het Opslagaccount" vervolgkeuzelijst.
   2. Selecteer **voltooien** om de verbinding te maken. Selecteer vervolgens **Volgende**.
   
   ![Azure Data Lake Storage Gen2 account opgeven](./media/load-azure-data-lake-storage-gen2/specify-adls.png)

9. In de **uitvoerbestand of uitvoermap kiezen** pagina **copyfroms3** als de naam van de uitvoer-map en selecteer **volgende**. ADF wordt het bijbehorende Gen2 ADLS-bestandssysteem en de submappen tijdens het kopiëren van maken als deze nog niet bestaat.

    ![Geef de map voor uitvoer](./media/load-azure-data-lake-storage-gen2/specify-adls-path.png)

10. In de **instellingen** weergeeft, schakelt **volgende** om de standaardinstellingen te gebruiken:

    ![De pagina Instellingen](./media/load-azure-data-lake-storage-gen2/copy-settings.png)
11. In de **samenvatting** pagina, Controleer de instellingen en selecteer **volgende**:

    ![Overzichtspagina](./media/load-azure-data-lake-storage-gen2/copy-summary.png)
12. In de **implementatiepagina**, selecteer **Monitor** voor het bewaken van de pijplijn:

    ![De pagina Implementatie](./media/load-azure-data-lake-storage-gen2/deployment-page.png)
13. U ziet dat het tabblad **Controleren** aan de linkerkant automatisch wordt geselecteerd. De **acties** kolom bevat koppelingen om details uitvoering van activiteit weer te geven en de pijplijn opnieuw uitvoeren:

    ![Pijplijnuitvoeringen controleren](./media/load-azure-data-lake-storage-gen2/monitor-pipeline-runs.png)

14. Als u uitvoeringen van activiteit die gekoppeld aan de pijplijnuitvoering zijn, selecteer de **uitvoeringen van activiteit weergeven** herstelkoppeling in de **acties** kolom. Omdat er slechts één activiteit (kopieeractiviteit) in de pijplijn is, ziet u slechts één vermelding in de lijst. Overschakelen naar de vorige weergave, selecteert u de **pijplijnen** koppelen aan de bovenkant. Selecteer **Vernieuwen** om de lijst te vernieuwen. 

    ![Uitvoering van activiteiten controleren](./media/load-azure-data-lake-storage-gen2/monitor-activity-runs.png)

15. Voor het controleren van de uitvoering van details voor elke activiteit kopiëren, selecteert u de **Details** koppeling (van een bril) onder **acties** in de activiteit controle weergeven. U kunt details controleren zoals de hoeveelheid gegevens gekopieerd van de bron naar het sink, de doorvoer van gegevens, de stappen worden uitgevoerd met de bijbehorende tijdsduur en configuraties gebruikt:

    ![Details uitvoering van activiteit controleren](./media/load-azure-data-lake-storage-gen2/monitor-activity-run-details.png)

16. Controleer of dat de gegevens worden gekopieerd naar uw Data Lake Storage Gen2-account.

## <a name="next-steps"></a>Volgende stappen

* [Overzicht kopieeractiviteit](copy-activity-overview.md)
* [Azure Data Lake Storage Gen2-connector](connector-azure-data-lake-storage.md)
