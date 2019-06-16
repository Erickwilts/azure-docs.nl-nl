---
title: De gebruiker geïnitieerde bewerking van Azure Automation-Runbook in Log Analytics | Microsoft Docs
description: In dit artikel wordt beschreven hoe u een Automation-runbook uitvoeren vanuit een Log Analytics search resultaat op aanvraag.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: log-analytics
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/06/2019
ms.author: magoedte
ms.openlocfilehash: 9194d5fe6553607ac5a0bb4e133da97f53790984
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "61424727"
---
# <a name="take-action-with-an-automation-runbook-from-a-log-analytics-log-search-result"></a>Onderneem actie met een Automation-Runbook uit een zoekresultaat voor Log Analytics-logboek

> [!NOTE]
> Een runbook starten vanuit de zoekresultaten is een functie van de klassieke portal zoeken in logboeken die worden op 15 februari 2019 afgeschaft. U kunt configureren dat een actiegroep die u met een runbook naast andere acties op basis van beginnen kunt een [waarschuwingsregel](../platform/alerts-log.md) in Azure Monitor.

In een zoekresultaat log in Azure Log Analytics, kunt u nu selecteren **actie ondernemen** om uit te voeren van een Automation-runbook.  Het runbook kan worden gebruikt om het probleem op te lossen of een andere actie zoals zoals het verzamelen van informatie over probleemoplossing, een e-mail verzenden of een serviceaanvraag maakt op te nemen. 


## <a name="components-and-features-used"></a>Gebruikte onderdelen en functies
* [Azure Automation-account](../../automation/automation-quickstart-create-account.md)
* [Log Analytics-werkruimte](../../azure-monitor/log-query/log-query-overview.md)

## <a name="to-initiate-runbook-from-log-search"></a>Runbook van zoeken in Logboeken initiëren

Actie ondernemen voor een gebeurtenis en een runbook in de resultaten van uw logboekbestanden voor zoeken initiëren, u beginnen met het maken van een logboekzoekopdracht en uit de resultaten kunt u een runbook op aanvraag aanroepen. Dit kan worden bereikt vanaf de zoekfunctie klassieke log in de [Azure-portal](../../azure-monitor/log-query/log-query-overview.md). In dit voorbeeld wordt een zoekopdracht in Logboeken uitvoert vanuit de Azure portal met een eenvoudige demonstratie van deze functie.

1. Klik in de Azure-portal op **alle services** en selecteer **Log Analytics**.  
2. Selecteer uw Log Analytics-werkruimte.
3. Selecteer in de werkruimte **Logboeken (klassiek)** .  
4. Op de pagina zoeken in Logboeken, moet u een zoekopdracht in Logboeken uitvoert.  
5. Uit de resultaten van de logboekzoekopdracht, klikt u op de ellips aan de linkerkant van een van de velden en in het pop-upvenster, selecteer **actie ondernemen voor**.<br><br> ![Selecteer actie ondernemen in zoekresultaat](./media/take-action/log-search-takeaction-menuoption.png) 
6. Selecteer **uitvoeren van een runbook** en selecteert u een runbook om uit te voeren.  U kunt een runbook selecteren in het Automation-account dat is gekoppeld aan de Log Analytics-werkruimte.  Houd rekening met het volgende:

    * Runbooks zijn ingedeeld op labels
    * Runbook-invoerparameter waarden kunnen worden geselecteerd rechtstreeks vanuit de velden van het zoekresultaat.  Een vervolgkeuzelijst wordt weergegeven voor het weergeven van alle beschikbare velden van het resultaat te selecteren in.  
    * U kunt kiezen uit te voeren van het runbook op een [hybrid runbook worker](../../automation/automation-hybrid-runbook-worker.md) dat u hebt geïnstalleerd op de machine waarop het probleem is als er een overeenkomstige Hybrid Runbook Worker-groep met alleen deze machine als een lid.  Als de naam van de Hybrid Worker-groep overeenkomt met de naam van de computer die in de zoekresultaten logboek, wordt klikt u vervolgens de groep automatisch geselecteerd.    

6. Nadat u op **uitvoeren**, de pagina van de runbook-taak wordt geopend zodat u kunt de status van de taak controleren.   

Als u een runbook dat is geconfigureerd om te worden [aangeroepen vanuit een Log Analytics-waarschuwing](../../automation/automation-create-alert-triggered-runbook.md), heeft een invoerparameter **WebhookData** dat wil zeggen **Object** type.  Als de invoerparameter verplicht is, moet u de lijst met zoekresultaten doorgeven aan het runbook, zodat deze kan de tekenreeks in JSON-indeling converteren naar een objecttype zodat u kunt filteren op specifieke items waarnaar u in de runbook-activiteiten verwijst.  U dit doen door het selecteren van **zoekresultaat (Object)** uit de vervolgkeuzelijst.<br><br> ![Webhook-gegevensobject voor runbook-parameter selecteren](media/take-action/select-runbook-and-properties.png)   
    
## <a name="next-steps"></a>Volgende stappen

* Controleer de [Log Analytics melden verwijzing naar de](../../azure-monitor/log-query/log-query-overview.md) om alle zoekvelden en facetten beschikbaar in Log Analytics weer te geven.
* Als u wilt weten hoe u automatisch een Automation-runbook aanroepen, Bekijk [een Azure Automation-runbook aanroepen vanuit een Log Analytics-waarschuwing](../../automation/automation-create-alert-triggered-runbook.md).  
