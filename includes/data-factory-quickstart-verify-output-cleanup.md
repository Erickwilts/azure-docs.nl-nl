---
author: linda33wj
ms.service: data-factory
ms.topic: include
ms.date: 11/09/2018
ms.author: jingwang
ms.openlocfilehash: 309ef92b33d5bbdf8e8aed6b162ed9428a669c87
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67176505"
---
## <a name="verify-the-output"></a>De uitvoer controleren
De uitvoermap wordt automatisch door de pijplijn gemaakt in de blobcontainer adftutorial. Vervolgens wordt het bestand emp.txt gekopieerd van de invoermap naar de uitvoermap. 

1. Klik in Azure Portal op de pagina met de **adftutorial**-container op **Vernieuwen** om de uitvoermap weer te geven. 
    
    ![Vernieuwen](media/data-factory-quickstart-verify-output-cleanup/output-refresh.png)
2. Klik in de lijst met mappen op **Uitvoer**. 
2. Controleer of het bestand **emp.txt** naar de uitvoermap is gekopieerd. 

    ![Vernieuwen](media/data-factory-quickstart-verify-output-cleanup/output-file.png)

## <a name="clean-up-resources"></a>Resources opschonen
De resources die u hebt gemaakt in de Quick Start kunt u op twee manieren opschonen. U kunt de [Azure-resourcegroep](../articles/azure-resource-manager/resource-group-overview.md) verwijderen, met alle resources uit de resourcegroep. Als u de andere resources intact wilt houden, verwijdert u alleen de data factory die u in deze zelfstudie hebt gemaakt.

Als u een resourcegroep verwijdert, worden alle resources die deze bevat, met inbegrip van de data factory's, ook verwijderd. Voer de volgende opdracht uit om de gehele resourcegroep te verwijderen: 
```powershell
Remove-AzResourceGroup -ResourceGroupName $resourcegroupname
```

Opmerking: het verwijderen van een resourcegroep kan enige tijd duren. Even geduld met het proces

Als u alleen de data factory wilt verwijderen en niet de gehele resourcegroep, moet u de volgende opdracht uitvoeren: 

```powershell
Remove-AzDataFactoryV2 -Name $dataFactoryName -ResourceGroupName $resourceGroupName
```