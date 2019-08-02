---
title: Onboarding van uw Azure Stack virtuele machines naar Azure-Sentinel | Microsoft Docs
description: In dit artikel wordt beschreven hoe u de extensie van de virtuele machine voor Azure Monitor, update en configuratie beheer op virtuele machines van Azure Stack kunt inrichten en hoe u deze kunt controleren met Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2019
ms.author: rkarlin
ms.openlocfilehash: caaf708b3efd8ffbe059f8ad249b7945d31fd7cc
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2019
ms.locfileid: "68601162"
---
# <a name="connect-azure-stack-virtual-machines-to-azure-sentinel"></a>Azure Stack virtuele machines verbinden met Azure Sentinel

> [!IMPORTANT]
> Azure Sentinel is momenteel beschikbaar als open bare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.


Met Azure Sentinel kunt u uw virtuele machines die worden uitgevoerd op Azure en Azure Stack op één plek bewaken. Als u uw Azure Stack-machines aan Azure Sentinel wilt toevoegen, moet u eerst de extensie van de virtuele machine aan uw bestaande virtuele machines met Azure Stack. 

Nadat u Azure Stack machines hebt verbonden, kiest u uit een galerie met Dash boards die op basis van uw gegevens Blade inzichten hebben. Deze Dash boards kunnen eenvoudig worden aangepast aan uw behoeften.



## <a name="add-the-virtual-machine-extension"></a>De extensie van de virtuele machine toevoegen 

Voeg de extensie van de virtuele machine **Azure monitor, update en configuratie beheer** toe aan de virtuele machines die worden uitgevoerd op uw Azure stack. 

1. Meld u in een nieuw browser tabblad aan bij uw [Azure stack-Portal](https://docs.microsoft.com/azure-stack/user/azure-stack-use-portal#access-the-portal).
2. Ga naar de pagina **virtuele machines** en selecteer de virtuele machine die u met Azure Sentinel wilt beveiligen. Zie [een Windows Server-VM maken met de Azure stack Portal](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-quick-windows-portal) of [een Linux-Server-VM maken met behulp van de Azure stack Portal](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-quick-linux-portal)voor meer informatie over het maken van een virtuele machine op Azure stack.
3. Selecteer **Extensies**. De lijst met virtuele-machine uitbreidingen die op deze virtuele machine is geïnstalleerd, wordt weer gegeven.
4. Klik op het tabblad **toevoegen** . De Blade **Nieuw resource** menu wordt geopend en toont de lijst met beschik bare extensies van virtuele machines. 
5. Selecteer de uitbrei ding **Azure monitor, update en configuratie beheer** en klik op **maken**. Het venster **extensie configuratie installeren** wordt geopend.

   ![Instellingen voor Azure Monitor-, update-en configuratie beheer](./media/connect-azure-stack/azure-monitor-extension-fix.png)  

   >[!NOTE]
   > Als u de **Azure monitor-, update-en configuratie beheer** uitbreiding die wordt vermeld in uw Marketplace niet ziet, neemt u contact op met uw Azure stack-operator om deze beschikbaar te maken.

6. In het menu Sentinel van Azure selecteert u **werkruimte instellingen** gevolgd door **Geavanceerd**en kopieert u de **werk ruimte-id** en de **werkruimte sleutel (primaire sleutel)** . 
1. Plak in het venster **extensie Azure stack installatie** de waarden in de aangegeven velden en klik op **OK**.
1. Nadat de installatie van de extensie is voltooid, wordt de status ervan weer gegeven als **inrichting geslaagd**. Het kan een uur duren voordat de virtuele machine wordt weer gegeven in de Azure-Sentinel-Portal.

Zie [Windows-computers verbinden](../azure-monitor/platform/agent-windows.md#install-the-agent-using-setup-wizard)voor meer informatie over het installeren en configureren van de agent voor Windows.

Zie [problemen met Azure log Analytics Linux-agent oplossen](../azure-monitor/platform/agent-linux-troubleshoot.md)voor Linux problemen oplossen met agents.

In de Azure-Sentinel-Portal op Azure, onder **virtual machines**, hebt u een overzicht van alle vm's en computers, samen met hun status. 

## <a name="clean-up-resources"></a>Resources opschonen
Wanneer u deze niet meer nodig hebt, kunt u de extensie uit de virtuele machine verwijderen via de Azure Stack Portal.

De uitbrei ding verwijderen:

1. Open de **Azure stack Portal**.
2. Ga naar de pagina **virtuele machines** en selecteer de virtuele machine waarvan u de extensie wilt verwijderen.
3. Selecteer **uitbrei dingen**, selecteer de extensie **micro soft. EnterpriseCloud. monitoring**.
4. Klik op **installatie ongedaan maken**en bevestig uw selectie.

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de volgende artikelen voor meer informatie over Azure Sentinel:
- Meer informatie over hoe u [inzicht krijgt in uw gegevens en mogelijke bedreigingen](quickstart-get-visibility.md).
- Ga aan de slag [met het detecteren van bedreigingen met Azure Sentinel](tutorial-detect-threats.md).
- Gegevens streamen van [algemene fout indelings toestellen](connect-common-event-format.md) in azure Sentinel.
