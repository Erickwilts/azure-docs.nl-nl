---
title: Taken bewaken in Azure Data Lake Analytics met Azure portal
description: In dit artikel wordt beschreven hoe u de Azure portal gebruiken voor het oplossen van Azure Data Lake Analytics-taken.
services: data-lake-analytics
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: b7066d81-3142-474f-8a34-32b0b39656dc
ms.topic: conceptual
ms.date: 12/05/2016
ms.openlocfilehash: 40864bab068659be016161f7dc40243ebbd45174
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60812581"
---
# <a name="monitor-jobs-in-azure-data-lake-analytics-using-the-azure-portal"></a>Taken bewaken in Azure Data Lake Analytics met behulp van de Azure-Portal

**Om te zien van alle taken**

1. Vanuit de Azure-portal, klikt u op **Microsoft Azure** in de linkerbovenhoek.
2. Klik op de tegel met de naam van uw Data Lake Analytics-account.  Samenvatting van de taak wordt weergegeven op de **Taakbeheer** tegel.

    ![Beheer van Azure Data Lake Analytics-taak](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-job-management.png)

    De taak Management biedt een overzicht van de status van de taak. Er is een mislukte taak.
3. Klik op de **Taakbeheer** tegel om te zien van de taken. De taken zijn onderverdeeld in **met**, **in de wachtrij geplaatst**, en **beëindigd**. U ziet de mislukte taak in de **beëindigd** sectie. Het moet eerste item in de lijst zijn. Wanneer u een groot aantal taken hebt, kunt u klikken op **Filter** om u te helpen u bij het zoeken van taken.

    ![Azure Data Lake Analytics taken filteren](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-filter-jobs.png)
4. Klik op de mislukte taak uit de lijst om de taakdetails openen:

    ![Azure Data Lake Analytics taak is mislukt](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-failed-job.png)

    U ziet dat de **opnieuw indienen** knop. Nadat u het probleem wordt opgelost, kunt u de taak opnieuw indienen.
5. Klik op de gemarkeerde onderdeel in de vorige schermafbeelding te openen van de foutdetails.  U ziet er ongeveer als:

    ![Azure Data Lake Analytics taakdetails is mislukt](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-failed-job-details.png)

    Het vertelt u dat de bronmap is niet gevonden.
6. Klik op **Script dubbele**.
7. Update de **FROM** pad naar:

    "/ Samples/Data/SearchLog.tsv"
8. Klik op **Taak verzenden**.

## <a name="see-also"></a>Zie ook
* [Overzicht van Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [Aan de slag met Azure Data Lake Analytics met Azure PowerShell](data-lake-analytics-get-started-powershell.md)
* [Azure Data Lake Analytics beheren met Azure Portal](data-lake-analytics-manage-use-portal.md)
