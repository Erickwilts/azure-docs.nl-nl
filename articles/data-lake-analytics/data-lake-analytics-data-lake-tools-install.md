---
title: Azure Data Lake-tools voor Visual Studio installeren
description: In dit artikel wordt beschreven hoe u Azure Data Lake Tools voor Visual Studio installeren.
services: data-lake-analytics
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: ad8a6992-02c7-47d4-a108-62fc5a0777a3
ms.topic: conceptual
ms.date: 05/03/2018
ms.openlocfilehash: 3269d38b691ec4dd9573a241c89dadc285787143
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60408111"
---
# <a name="install-data-lake-tools-for-visual-studio"></a>Data Lake Tools voor Visual Studio installeren

Informatie over het gebruik van de Visual Studio voor het maken van Azure Data Lake Analytics-accounts, het definiëren van taken in [U-SQL](data-lake-analytics-u-sql-get-started.md) en het verzenden van taken naar de Data Lake Analytics-service. Zie [Overzicht van Azure Data Lake Analytics](data-lake-analytics-overview.md) voor meer informatie over Data Lake Analytics.

## <a name="prerequisites"></a>Vereisten

* **Visual Studio**: Alle versies behalve Express worden ondersteund.
    * Visual Studio 2017
    * Visual Studio 2015
    * Visual Studio 2013
* **Microsoft Azure SDK voor .NET** versie 2.7.1 of hoger.  U kunt dit installeren met het [webplatforminstallatieprogramma](https://www.microsoft.com/web/downloads/platform.aspx).
* Een **Data Lake Analytics**-account. Zie [Aan de slag met Azure Data Lake Analytics met Azure Portal](data-lake-analytics-get-started-portal.md) om een account te maken.

## <a name="install-azure-data-lake-tools-for-visual-studio-2017"></a>Azure Data Lake-tools voor Visual Studio 2017 installeren

Azure Data Lake Tools voor Visual Studio wordt ondersteund in Visual Studio 2017 15.3 en hoger. Het hulpprogramma maakt deel uit van de workloads **Gegevensopslag en -verwerking** en **Azure Development** in het installatieprogramma van Visual Studio. Schakel een van deze twee workloads in als onderdeel van uw installatie van Visual Studio.  

Schakel de **gegevensopslag en verwerking** workload zoals wordt weergegeven: ![Gegevensopslag en verwerking van workload inschakelen](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-tools-for-vs-2017-install-01.png)

Schakel de **Azure-ontwikkeling** workload zoals wordt weergegeven: ![Azure-ontwikkelworkload inschakelen](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-tools-for-vs-2017-install-02.png)

## <a name="install-azure-data-lake-tools-for-visual-studio-2013-and-2015"></a>Azure Data Lake Tools voor Visual Studio 2013 en 2015 installeren

Download en installeer Azure Data Lake-tools voor Visual Studio [via het Downloadcentrum](https://aka.ms/adltoolsvs). Na installatie moet u zich het volgende realiseren:
* Het knooppunt **Server Explorer** > **Azure** bevat een **Data Lake Analytics**-knooppunt. 
* Het menu **Extra** bevat een **Data Lake**-item.


## <a name="next-steps"></a>Volgende stappen
* Zie [Accessing diagnostics logs for Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md) (Diagnostische logboeken openen voor Azure Data Lake Analytics) voor logboekregistratie van diagnostische informatie.
* Zie [Websitelogboeken analyseren met Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md) voor een complexere query.
* Als u wilt de vertex execution view gebruiken, Zie [de Vertex Execution View in Data Lake Tools voor Visual Studio gebruiken](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md)
