---
title: Opnemen in Azure Data Lake Storage Gen2
description: Meer informatie over het opnemen van gegevens in Azure Data Lake Storage Gen2 in azure Synapse Analytics
services: synapse-analytics
author: djpmsft
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/15/2020
ms.author: daperlov
ms.reviewer: jrasnick
ms.openlocfilehash: cc5c72c2d0db7c17fdbc29e7fb815f1d06134730
ms.sourcegitcommit: 3fc3457b5a6d5773323237f6a06ccfb6955bfb2d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2020
ms.locfileid: "90033214"
---
# <a name="ingest-data-into-azure-data-lake-storage-gen2"></a>Gegevens opnemen in Azure Data Lake Storage Gen2 

In dit artikel leert u hoe u gegevens opneemt van de ene locatie naar de andere in een opslag account van Azure Data Lake gen 2 (Azure Data Lake gen 2) met behulp van Azure Synapse Analytics.

## <a name="prerequisites"></a>Vereisten

* **Azure-abonnement**: als u nog geen Azure-abonnement hebt, maakt u een [gratis Azure-account](https://azure.microsoft.com/free/) voordat u begint.
* **Azure Storage account**: u Azure data Lake gen 2 gebruiken als een *brongegevens* opslag. Als u geen opslag account hebt, raadpleegt u [een Azure Storage-account maken](../../storage/blobs/data-lake-storage-quickstart-create-account.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) om er een te maken.

## <a name="create-linked-services"></a>Gekoppelde services maken

In azure Synapse Analytics is een gekoppelde service waar u de verbindings gegevens voor andere services definieert. In deze sectie voegt u Azure Synapse Analytics en Azure Data Lake gen 2 toe als gekoppelde services.

1. Open Azure Synapse Analytics UX en ga naar het tabblad **beheren** .
1. Onder **externe verbindingen**selecteert u **gekoppelde services**.
1. Selecteer **Nieuw**om een gekoppelde service toe te voegen.
1. Selecteer de tegel Azure Data Lake Storage Gen2 in de lijst en selecteer **door gaan**.
1. Voer uw verificatie referenties in. Account sleutel, Service-Principal en beheerde identiteit worden momenteel ondersteunde verificatie typen. Selecteer verbinding testen om te controleren of uw referenties correct zijn. 
1. Selecteer **Maken** nadat dit is voltooid.

## <a name="create-pipeline"></a>Pijplijn maken

Een pijp lijn bevat de logische stroom voor het uitvoeren van een reeks activiteiten. In deze sectie maakt u een pijp lijn met een Kopieer activiteit waarmee gegevens uit Azure Data Lake gen 2 worden opgenomen in een SQL-groep.

1. Ga naar het tabblad **Orchestration** . Selecteer op het plus-pictogram naast de kop pijp lijnen en selecteer **pijp lijn**.
1. Sleep onder **verplaatsen en transformeren** in het deel venster activiteiten de gegevens naar het pijp lijn papier **kopiëren** .
1. Selecteer op de Kopieer activiteit en ga naar het tabblad **bron** . Selecteer **Nieuw** om een nieuwe bron-gegevensset te maken.
1. Selecteer Azure Data Lake Storage Gen2 als uw gegevens archief en selecteer door gaan.
1. Selecteer DelimitedText als uw indeling en selecteer door gaan.
1. Selecteer in het deel venster set Properties de ADLS-gekoppelde service die u hebt gemaakt. Geef het bestandspad van de bron gegevens op en geef op of de eerste rij een koptekst heeft. U kunt het schema importeren uit het bestands archief of een voorbeeld bestand. Selecteer OK wanneer u klaar bent.
1. Ga naar het tabblad **sink** . Selecteer **Nieuw** om een nieuwe Sink-gegevensset te maken.
1. Selecteer Azure Data Lake Storage Gen2 als uw gegevens archief en selecteer door gaan.
1. Selecteer DelimitedText als uw indeling en selecteer door gaan.
1. Selecteer in het deel venster set Properties de ADLS-gekoppelde service die u hebt gemaakt. Geef het pad op van de map waarnaar u gegevens wilt schrijven. Selecteer OK wanneer u klaar bent.

## <a name="debug-and-publish-pipeline"></a>Fouten opsporen en pijp lijn publiceren

Zodra u klaar bent met het configureren van de pijp lijn, kunt u een debug-uitvoering uitvoeren voordat u uw artefacten publiceert om te controleren of alles juist is.

1. Selecteer **Fouten opsporen** om fouten op te sporen in de pijplijn. De status van de pijplijnuitvoering wordt weergegeven op het tabblad **Uitvoer** onder in het venster. 
1. Zodra de pijp lijn kan worden uitgevoerd, selecteert u in de bovenste werk balk **Alles publiceren**. Deze actie publiceert entiteiten (gegevens sets en pijp lijnen) die u hebt gemaakt voor de Synapse Analytics-service.
1. Wacht tot u het bericht **Gepubliceerd** ziet. Als u meldings berichten wilt weer geven, selecteert u de knop Bell in de rechter bovenhoek. 


## <a name="trigger-and-monitor-the-pipeline"></a>De pijp lijn activeren en bewaken

In deze stap moet u de pijp lijn die u in de vorige stap hebt gepubliceerd hand matig activeren. 

1. Selecteer op de werkbalk de optie **Trigger toevoegen** en selecteer vervolgens **Nu activeren**. Selecteer op de pagina **pijplijn uitvoering** de optie **volt ooien**.  
1. Ga naar het tabblad **monitor** in de zijbalk links. U ziet een pijplijn die wordt geactiveerd door een handmatige trigger. U kunt via de links in de kolom **Acties** details van de activiteiten bekijken en de pijplijn opnieuw uitvoeren.
1. Selecteer de link **Uitvoeringen van activiteit weergeven** in de kolom **Acties** om de activiteituitvoeringen te zien die zijn gekoppeld aan de pijplijnuitvoering. Omdat er in dit voorbeeld slechts één activiteit is, ziet u slechts één vermelding in de lijst. Selecteer de link **Details** (pictogram van een bril) in de kolom **Acties** om details over de kopieerbewerking te zien. Selecteer de **pijp lijn wordt** aan de bovenkant uitgevoerd om terug te gaan naar de weer gave pijplijn uitvoeringen. Selecteer **Vernieuwen** om de weergave te vernieuwen.
1. Controleer of uw gegevens correct zijn geschreven in de SQL-groep.


## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over gegevens integratie voor Synapse Analytics het [opnemen van gegevens in een SQL-groeps](data-integration-sql-pool.md) artikel.
