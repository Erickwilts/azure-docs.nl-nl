---
title: Opschonen van uw Azure Stream Analytics-taak
description: In dit artikel ziet u verschillende methoden voor het verwijderen van uw Azure Stream Analytics-taken.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 6/21/2019
ms.custom: seodec18
ms.openlocfilehash: cb81c73f7946a10bae0470a55dcf1c0d55c2b847
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2019
ms.locfileid: "67330052"
---
# <a name="stop-or-delete-your-azure-stream-analytics-job"></a>Beëindigt of verwijdert u uw Azure Stream Analytics-taak

Azure Stream Analytics-taken kunnen eenvoudig worden gestopt of verwijderd met de Azure portal, Azure PowerShell of Azure SDK voor .net of REST-API. Een Stream Analytics-taak kan niet worden hersteld nadat deze is verwijderd.

>[!NOTE] 
>Wanneer u de Stream Analytics-taak stopt, de gegevens zich blijft voordoen alleen in de invoer- en opslag, zoals Event Hubs of Azure SQL Database. Als u gegevens uit Azure verwijdert moet, moet u de verwijdering processen voor de invoer- en bronnen van uw Stream Analytics-taak volgen.

## <a name="stop-a-job-in-azure-portal"></a>Een taak in Azure portal stoppen

Wanneer u een taak stopt, de resources zijn deprovisionned en stopt het verwerken van gebeurtenissen. Kosten met betrekking tot deze taak worden ook gestopt. Maar alle uw configuratie worden gehouden en u kunt de taak later opnieuw 

1. Meld u aan bij [Azure Portal](https://portal.azure.com). 

2. Zoek uw actieve Stream Analytics-taak en selecteert u deze.

3. Selecteer op de pagina van de Stream Analytics-taak **stoppen** om de taak te stoppen. 

   ![Azure Stream Analytics-taak stoppen](./media/stream-analytics-clean-up-your-job/stop-stream-analytics-job.png)


## <a name="delete-a-job-in-azure-portal"></a>Een taak in Azure portal verwijderen

>[!WARNING] 
>Een Stream Analytics-taak kan niet worden hersteld nadat deze is verwijderd.

1. Meld u aan bij Azure Portal. 

2. Zoek uw bestaande Stream Analytics-taak en selecteert u deze.

3. Selecteer op de pagina van de Stream Analytics-taak **verwijderen** om de taak te verwijderen. 

   ![Azure Stream Analytics-taak verwijderen](./media/stream-analytics-clean-up-your-job/delete-stream-analytics-job.png)


## <a name="stop-or-delete-a-job-using-powershell"></a>Beëindigt of verwijdert u een taak met behulp van PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Als u wilt stoppen van een taak met behulp van PowerShell, gebruikt u de [Stop-AzStreamAnalyticsJob](https://docs.microsoft.com/powershell/module/az.streamanalytics/stop-azstreamanalyticsjob) cmdlet. Als u wilt verwijderen van een taak met behulp van PowerShell, gebruikt u de [Remove-AzStreamAnalyticsJob](https://docs.microsoft.com/powershell/module/az.streamanalytics/Remove-azStreamAnalyticsJob) cmdlet.

## <a name="stop-or-delete-a-job-using-azure-sdk-for-net"></a>Beëindigt of verwijdert u een taak met Azure SDK voor .NET

Als u wilt stoppen van een taak met Azure SDK voor .NET, gebruiken de [StreamingJobsOperationsExtensions.BeginStop](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.streamanalytics.streamingjobsoperationsextensions.beginstop?view=azure-dotnet) methode. Verwijderen van een taak met Azure SDK voor .NET, [StreamingJobsOperationsExtensions.BeginDelete](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.streamanalytics.streamingjobsoperationsextensions.begindelete?view=azure-dotnet) methode.

## <a name="stop-or-delete-a-job-using-rest-api"></a>Beëindigt of verwijdert u een taak met behulp van REST-API

Als u wilt stoppen van een taak met behulp van REST-API, raadpleegt u de [stoppen](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-job#stop) methode. Als u wilt verwijderen van een taak met behulp van REST-API, raadpleegt u de [verwijderen](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-job#delete) methode.
