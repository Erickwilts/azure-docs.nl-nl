---
title: VM-extensie van Azure Portal inschakelen
description: In dit artikel wordt beschreven hoe u virtuele-machine uitbreidingen implementeert voor Azure Arc-servers die worden uitgevoerd in hybride Cloud omgevingen van de Azure Portal.
ms.date: 11/06/2020
ms.topic: conceptual
ms.openlocfilehash: 48d7d4085dce893d94436fe0c6be32cfeea9cda3
ms.sourcegitcommit: 0b9fe9e23dfebf60faa9b451498951b970758103
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2020
ms.locfileid: "94359082"
---
# <a name="enable-azure-vm-extensions-from-the-azure-portal"></a>Azure VM-extensies inschakelen vanuit de Azure Portal

Dit artikel laat u zien hoe u Azure-VM-extensies, ondersteund door Azure Arc-servers, kunt implementeren en verwijderen naar een Linux-of Windows Hybrid-machine via de Azure Portal.

> [!NOTE]
> De Key Vault VM-extensie (preview) biedt geen ondersteuning voor implementatie vanuit de Azure Portal, alleen met behulp van de Azure CLI, het Azure PowerShell of het gebruik van een Azure Resource Manager sjabloon.

## <a name="enable-extensions-from-the-portal"></a>Uitbrei dingen van de portal inschakelen

Voor VM-extensies kan uw Arc worden toegepast voor de door de server beheerde machine via de Azure Portal.

1. Ga in uw browser naar de [Azure Portal](https://portal.azure.com).

2. Ga in de portal naar **servers-Azure-boog** en selecteer uw hybride machine in de lijst.

3. Kies **extensies** en selecteer vervolgens **toevoegen**. Kies de gewenste uitbrei ding in de lijst met beschik bare uitbrei dingen en volg de instructies in de wizard. In dit voor beeld wordt de VM-extensie Log Analytics geïmplementeerd.

    ![VM-extensie voor geselecteerde machine selecteren](./media/manage-vm-extensions/add-vm-extensions.png)

    In het volgende voor beeld ziet u de installatie van de Log Analytics VM-extensie van de Azure Portal:

    ![Log Analytics VM-extensie installeren](./media/manage-vm-extensions/mma-extension-config.png)

    Als u de installatie wilt volt ooien, moet u de werk ruimte-ID en de primaire sleutel opgeven. Als u niet bekend bent met het vinden van deze informatie, raadpleegt u de [werk ruimte-id en-sleutel ophalen](../../azure-monitor/platform/log-analytics-agent.md#workspace-id-and-key).

4. Nadat u de vereiste gegevens hebt bevestigd, selecteert u **maken**. Er wordt een samen vatting van de implementatie weer gegeven en u kunt de status van de implementatie controleren.

>[!NOTE]
>Hoewel meerdere uitbrei dingen tegelijk kunnen worden gebatcheerd en verwerkt, worden ze serieel geïnstalleerd. Zodra de eerste installatie van de extensie is voltooid, wordt de installatie van de volgende uitbrei ding geprobeerd.

## <a name="uninstall-extension"></a>Extensie verwijderen

U kunt een of meer uitbrei dingen van een server met Arc-functionaliteit uit de Azure Portal verwijderen. Voer de volgende stappen uit om een uitbrei ding te verwijderen.

1. Ga in uw browser naar de [Azure Portal](https://portal.azure.com).

2. Ga in de portal naar **servers-Azure-boog** en selecteer uw hybride machine in de lijst.

3. Kies **uitbrei dingen** en selecteer een uitbrei ding in de lijst met geïnstalleerde uitbrei dingen.

4. Selecteer **verwijderen** en wanneer u wordt gevraagd om te verifiëren, selecteert u **Ja** om door te gaan.

## <a name="next-steps"></a>Volgende stappen

- U kunt VM-extensies implementeren, beheren en verwijderen met behulp van de [Azure cli](manage-vm-extensions-cli.md)-, [Power shell](manage-vm-extensions-powershell.md)-of [Azure Resource Manager-sjablonen](manage-vm-extensions-template.md).

- Informatie over probleem oplossing vindt u in de [gids voor het oplossen van problemen met VM-extensies](troubleshoot-vm-extensions.md).