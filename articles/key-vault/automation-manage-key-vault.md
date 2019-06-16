---
title: Beheren van Azure Key Vault met Azure Automation - Azure Key Vault | Microsoft Docs
description: Meer informatie over hoe Azure Automation-service kan worden gebruikt voor het beheren van Azure Key Vault.
services: Key-Vault, automation
author: mgoedtel
manager: jwhit
editor: ''
ms.service: key-vault
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: magoedte
ms.openlocfilehash: 87fdb565f47e7d88342c82bd5940c1ddb9c137e6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64712119"
---
# <a name="managing-azure-key-vault-using-azure-automation"></a>Azure Key Vault met Azure Automation beheren

Deze handleiding vindt u de Azure Automation-service en hoe deze kan worden gebruikt voor een vereenvoudigd beheer van uw sleutels en geheimen in Azure Key Vault.

## <a name="what-is-azure-automation"></a>Wat is Azure Automation?

[Azure Automation](../automation/automation-intro.md) is een Azure-service voor het vereenvoudigen van cloudbeheer via procesautomatisering en configuratie van gewenste status. Met Azure Automation, handmatige, kunnen herhaald, langlopende, foutgevoelige taken worden geautomatiseerd om de betrouwbaarheid, efficiëntie en tijd tot waarde voor uw organisatie te verhogen.

Azure Automation biedt een zeer betrouwbare, maximaal beschikbare engine voor werkstroomuitvoering die kan worden geschaald om te voldoen aan uw behoeften. In Azure Automation, kunnen processen worden gestart handmatig, door systemen 3rd derden of met regelmatige tussenpozen zodat taken gebeuren precies wanneer dat nodig is.

Zowel de operationele overhead en vrij IT en DevOps-personeel te concentreren op werk dat bedrijfswaarde door uw taken voor cloudbeheer automatisch wordt uitgevoerd door Azure Automation.

## <a name="how-can-azure-automation-help-manage-azure-key-vault"></a>Hoe u kunt Azure Automation beheren van Azure Key Vault?

Key Vault kunnen worden beheerd in Azure Automation met behulp van de [AzureRM Key Vault-cmdlets](https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) en [Azure Classic Key Vault-cmdlets](https://docs.microsoft.com/powershell/module/servicemanagement/azure). De Azure-module voor het beheer van klassieke Key Vault automatisch in Azure Automation beschikbaar is en u kunt importeren de [module van AzureRM-KeyVault](https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) in Azure Automation, zodat u veel van uw Key Vault-beheertaken kunt uitvoeren in de service. Zie voor meer informatie over het importeren van de module in Azure Automation, [modules in Azure Automation beheren](../automation/shared-resources/modules.md) u deze cmdlets in Azure Automation met de cmdlets voor andere Azure-services om complexe taken over te automatiseren kan ook worden gekoppeld Azure-services en 3e systemen van derden.

Met de Azure Key Vault-cmdlets kunt u onder andere taken uitvoeren: 

* Maken en configureren van een key vault
* Maken of importeren van een sleutel
* Maken of bijwerken van een geheim
* Kenmerken van een sleutel bijwerken
* Een sleutel of geheim ophalen
* Een sleutel of geheim verwijderen

Hier volgen enkele voorbeelden van het gebruik van PowerShell voor het beheren van Key Vault:  

* [Azure Key Vault - stap voor stap](https://blogs.technet.microsoft.com/kv/2015/06/02/azure-key-vault-step-by-step)
* [Instellen en configureren van een Azure Key Vault](https://www.simple-talk.com/cloud/platform-as-a-service/setting-up-and-configuring-an-azure-key-vault)

## <a name="next-steps"></a>Volgende stappen

Nu dat u de basisprincipes van Azure Automation en hoe deze kan worden gebruikt voor het beheren van Azure Key Vault hebt geleerd, volgt u deze koppelingen voor meer informatie over Azure Automation.

* Zie de Azure Automation [zelfstudie aan de slag](../automation/automation-first-runbook-graphical.md).
* Zie de [Azure Key Vault PowerShell-scripts](https://gallery.technet.microsoft.com/scriptcenter/site/search?query=azure%20key%20vault&f%5B0%5D.Value=azure%20key%20vault&f%5B0%5D.Type=SearchText&ac=5).

