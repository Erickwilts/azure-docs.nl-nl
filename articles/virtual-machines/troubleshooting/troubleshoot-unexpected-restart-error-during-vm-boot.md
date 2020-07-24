---
title: 'Opstarten met het besturings systeem: de computer is onverwacht opnieuw opgestart of er is een onverwachte fout opgetreden'
description: In dit artikel worden de stappen beschreven voor het oplossen van problemen waarbij de VM een onverwachte herstart of-fout ondervindt tijdens de installatie van Windows.
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: 1d249a4e-71ba-475d-8461-31ff13e57811
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 06/22/2020
ms.author: v-mibufo
ms.openlocfilehash: 186b1c46303be59e191a1754361e07a2003b997a
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2020
ms.locfileid: "87036179"
---
# <a name="os-start-up--computer-restarted-unexpectedly-or-encountered-an-unexpected-error"></a>Opstarten met het besturings systeem: de computer is onverwacht opnieuw opgestart of er is een onverwachte fout opgetreden

Dit artikel bevat stappen voor het oplossen van problemen waarbij de virtuele machine (VM) een onverwachte herstart of fout ondervindt tijdens de installatie van Windows.

## <a name="symptom"></a>Symptoom

Wanneer u [Diagnostische gegevens over opstarten](./boot-diagnostics.md) gebruikt om de scherm opname van de virtuele machine weer te geven, ziet u dat de scherm opname wordt weer gegeven met de volgende fout:

**De computer is onverwacht opnieuw opgestart of er is een onverwachte fout opgetreden. De Windows-installatie kan niet worden voortgezet. Als u Windows wilt installeren, klikt u op OK om de computer opnieuw op te starten en start u de installatie opnieuw.**

![Er is een fout opgetreden tijdens de installatie van Windows: de computer is onverwacht opnieuw opgestart of er is een onverwachte fout opgetreden. De Windows-installatie kan niet worden voortgezet. Als u Windows wilt installeren, klikt u op OK om de computer opnieuw op te starten en start u de installatie opnieuw.](./media/unexpected-restart-error-during-vm-boot/1.png)
 
![Fout bij het starten van services: de computer is onverwacht opnieuw opgestart of er is een onverwachte fout opgetreden. De Windows-installatie kan niet worden voortgezet. Als u Windows wilt installeren, klikt u op OK om de computer opnieuw op te starten en start u de installatie opnieuw.](./media/unexpected-restart-error-during-vm-boot/2.png)

## <a name="cause"></a>Oorzaak

Er wordt geprobeerd een [gegeneraliseerde installatie kopie](/windows-hardware/manufacture/desktop/sysprep--generalize--a-windows-installation)te starten met de computer, maar er is een probleem opgetreden vanwege een aangepast antwoord bestand (unattend.xml) dat wordt verwerkt. Aangepaste antwoordbestanden worden niet ondersteund in Azure. 

Het antwoord bestand is een speciaal XML-bestand met instellings definities en-waarden voor de configuratie-instellingen die u wilt automatiseren tijdens de installatie van een installatie van een Windows Server-besturings systeem. De configuratie opties bevatten instructies voor het partitioneren van schijven, waar u de Windows-installatie kopie kunt vinden die moet worden geïnstalleerd, de product codes die moeten worden toegepast en andere opdrachten die u wilt uitvoeren.

In Azure worden aangepaste antwoord bestanden niet ondersteund. Als u een aangepast **Unattend.xml** bestand hebt opgegeven met behulp van de `sysprep /unattend:<your file’s name>` optie, kan deze fout optreden.

Gebruik in azure de optie **systeem OOBE (out-of-Box Experience) opgeven** in **Sysprep.exe**of gebruik in `sysprep /oobe` plaats van het Unattend.xml bestand.

Dit probleem wordt meestal gemaakt wanneer u **Sysprep.exe** gebruikt met een on-premises VM om een gegeneraliseerde VM naar Azure te uploaden. In dit geval is het wellicht ook belang rijk om een gegeneraliseerde virtuele machine goed te uploaden.

## <a name="solution"></a>Oplossing

### <a name="replace-unattended-answer-file-option"></a>Optie voor antwoord bestand voor installatie zonder toezicht vervangen

Deze situatie doet zich voor wanneer een afbeelding is voor bereid voor gebruik in azure, maar een aangepast antwoord bestand heeft gebruikt dat niet wordt ondersteund in azure, en u hebt **Sysprep** gebruikt met een vlag die vergelijkbaar is met de volgende opdracht:

`sysprep /oobe /generalize /unattend:<NameOfYourAnswerFile.XML> /shutdown`

- Vervang in de vorige opdracht door `<NameOfYourAnswerFile.XML>` de naam van het bestand.

U kunt dit probleem oplossen door [de Azure-richt lijnen voor het voorbereiden/vastleggen van een installatie kopie](../windows/upload-generalized-managed.md) te volgen en een nieuwe gegeneraliseerde installatie kopie voor te bereiden. Gebruik bij Sysprep geen `/unattend:<answerfile>` vlag. Gebruik in plaats daarvan alleen de onderstaande vlaggen:

`sysprep /oobe /generalize /shutdown`

- **Out-of-Box-Experience** (OOBE) is de ondersteunde instelling voor virtuele Azure-machines.

U kunt ook de **gebruikers interface van het hulp programma voor systeem voorbereiding** gebruiken om dezelfde taak uit te voeren als de bovenstaande opdracht door de opties te selecteren die hieronder worden weer gegeven:

- Out-of-Box-Experience invoeren
- Generaliseren
- Afsluiten
 
![Venster hulp programma voor systeem voorbereiding met opties voor OOBE, generalize en afsluiten geselecteerd.](./media/unexpected-restart-error-during-vm-boot/3.png)
