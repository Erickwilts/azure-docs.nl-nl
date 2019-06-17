---
title: App-groepen beheren voor Windows Virtual Desktop Preview - Azure
description: Beschrijft hoe u voor het instellen van Windows Virtual Desktop Preview tenants in Azure Active Directory.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: tutorial
ms.date: 03/21/2019
ms.author: helohr
ms.openlocfilehash: cba13012bf165a097bd1382da8ef9897b0584d28
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67066882"
---
# <a name="tutorial-manage-app-groups-for-windows-virtual-desktop-preview"></a>Zelfstudie: App-groepen beheren voor Windows Virtual Desktop Preview

De standaard-app-groep gemaakt voor een nieuwe groep van de host Windows Virtual Desktop Preview publiceert ook het volledige bureaublad. Bovendien kunt u een of meer RemoteApp-toepassingsgroepen voor de host-groep maken. Volg deze zelfstudie als u wilt een RemoteApp-app-groep maken en publiceren van afzonderlijke apps in het menu Start.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een RemoteApp-groep maken.
> * Toegang verlenen tot RemoteApps.

Voordat u begint, [downloaden en importeren van de Windows virtuele bureaublad PowerShell-module](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview) de virtuele Windows-bureaublad-module gebruiken in uw PowerShell-sessie als u dat nog niet gedaan hebt.

## <a name="create-a-remoteapp-group"></a>Een RemoteApp-groep maken

1. Voer de volgende PowerShell-cmdlet voor het maken van een nieuwe lege RemoteApp-app-groep.

   ```powershell
   New-RdsAppGroup <tenantname> <hostpoolname> <appgroupname> -ResourceType "RemoteApp"
   ```

2. (Optioneel) Als u wilt controleren of dat de app-groep is gemaakt, kunt u de volgende cmdlet als u wilt zien van een lijst van alle app-groepen voor de groep host uitvoeren.

   ```powershell
   Get-RdsAppGroup <tenantname> <hostpoolname>
   ```

3. Voer de volgende cmdlet om een lijst van start menu apps op de installatiekopie van de virtuele machine van de host-pool. Noteer de waarden voor **FilePath**, **IconPath**, **IconIndex**, en andere belangrijke informatie voor de toepassing die u wilt publiceren.

   ```powershell
   Get-RdsStartMenuApp <tenantname> <hostpoolname> <appgroupname>
   ```
   
4. Voer de volgende cmdlet om de toepassing op basis van de appalias te installeren. appalias zichtbaar wanneer u de uitvoer uit stap 3 uitvoert.

   ```powershell
   New-RdsRemoteApp <tenantname> <hostpoolname> <appgroupname> -Name <remoteappname> -AppAlias <appalias>
   ```

5. (Optioneel) Voer de volgende cmdlet voor het publiceren van een nieuwe RemoteApp aan de groep van toepassingen in stap 1 hebt gemaakt.

   ```powershell
   New-RdsRemoteApp <tenantname> <hostpoolname> <appgroupname> -Name <remoteappname> -Filepath <filepath>  -IconPath <iconpath> -IconIndex <iconindex>
   ```

6. Om te controleren dat de app is gepubliceerd, moet u de volgende cmdlet uitvoeren.

   ```powershell
   Get-RdsRemoteApp <tenantname> <hostpoolname> <appgroupname>
   ```

7. Herhaal stappen 1-5 voor elke toepassing die u wilt publiceren voor deze app-groep.
8. Voer de volgende cmdlet om gebruikers toegang tot de RemoteApps in de app-groep.

   ```powershell
   Add-RdsAppGroupUser <tenantname> <hostpoolname> <appgroupname> -UserPrincipalName <userupn>
   ```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u app-groepen maken, deze vullen met RemoteApps en gebruikers toewijzen aan de app-groep. Zie voor meer informatie over het maken van een groep van de host validatie, waarmee u service-updates voordat uitrollen naar uw productieomgeving controleren, maak een groep host om te valideren van de zelfstudie over service-updates.

> [!div class="nextstepaction"]
> [Maken van een groep host voor het valideren van de zelfstudie over service-updates](./create-validation-host-pool.md)
