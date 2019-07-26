---
title: Verbinding met extern bureaublad inschakelen voor een rol in azure Cloud Services | Microsoft Docs
description: Uw Azure Cloud service-toepassing configureren om extern bureau blad-verbindingen toe te staan
services: cloud-services
documentationcenter: ''
author: mmccrory
ms.service: cloud-services
ms.topic: article
ms.date: 11/28/2016
ms.author: memccror
ms.openlocfilehash: bea4e0c43d6ae6e0ea05c43343535195a25cf3e2
ms.sourcegitcommit: 4b647be06d677151eb9db7dccc2bd7a8379e5871
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2019
ms.locfileid: "68359516"
---
# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services"></a>Verbinding met extern bureaublad inschakelen voor een rol in azure Cloud Services

> [!div class="op_single_selector"]
> * [Azure-portal](cloud-services-role-enable-remote-desktop-new-portal.md)
> * [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)
> * [Visual Studio](cloud-services-role-enable-remote-desktop-visual-studio.md)

Met Extern bureaublad kunt u toegang krijgen tot het bureau blad van een rol die wordt uitgevoerd in Azure. U kunt een Extern bureaublad verbinding gebruiken om problemen met uw toepassing op te lossen en te onderzoeken terwijl deze wordt uitgevoerd.

U kunt tijdens de ontwikkeling een Extern bureaublad verbinding in uw rol instellen door de Extern bureaublad modules in uw service definitie op te nemen of door Extern bureaublad in te scha kelen via de Extern bureaublad extensie. De aanbevolen aanpak is het gebruik van de Extern bureaublad extensie, omdat u Extern bureaublad zelfs nadat de toepassing is geïmplementeerd, kunt inschakelen zonder dat u uw toepassing opnieuw hoeft te implementeren.

## <a name="configure-remote-desktop-from-the-azure-portal"></a>Extern bureaublad van de Azure Portal configureren

De Azure Portal gebruikt de Extern bureaublad-uitbreidings benadering zodat u Extern bureaublad zelfs nadat de toepassing is geïmplementeerd, kunt inschakelen. Met de **extern bureaublad** -instellingen voor uw Cloud service kunt u extern bureaublad inschakelen, het lokale Administrator-account wijzigen dat wordt gebruikt om verbinding te maken met de virtuele machines, het certificaat dat wordt gebruikt voor de verificatie en de verval datum in te stellen.

1. Klik op **Cloud Services**, selecteer de naam van de Cloud service en selecteer vervolgens **extern bureaublad**.

    ![Cloud Services extern bureau blad](./media/cloud-services-role-enable-remote-desktop-new-portal/CloudServices_Remote_Desktop.png)

2. Kies of u Extern bureaublad wilt inschakelen voor een afzonderlijke rol of voor alle rollen, en wijzig vervolgens de waarde van de schakelaar in **ingeschakeld**.

3. Vul de vereiste velden in voor de gebruikers naam, het wacht woord, de verval datum en het certificaat.

    ![Cloud Services extern bureau blad](./media/cloud-services-role-enable-remote-desktop-new-portal/CloudServices_Remote_Desktop_Details.png)

   > [!WARNING]
   > Alle rolinstanties worden opnieuw gestart wanneer u Extern bureaublad voor het eerst inschakelt en selecteert **OK** (vinkje). Om het opnieuw opstarten te voor komen, moet het certificaat dat wordt gebruikt voor het versleutelen van het wacht woord, worden geïnstalleerd voor de rol. Als u wilt voor komen dat de computer opnieuw wordt opgestart, uploadt u [een certificaat voor de Cloud service](cloud-services-configure-ssl-certificate-portal.md#step-3-upload-a-certificate) en keert u terug naar dit dialoog venster.

4. In **rollen**selecteert u de rol die u wilt bijwerken of selecteert u **Alles** voor alle rollen.

5. Wanneer u klaar bent met de configuratie-updates, selecteert u **Opslaan**. Het duurt enkele ogen blikken voordat de rolinstanties gereed zijn voor het ontvangen van verbindingen.

## <a name="remote-into-role-instances"></a>Extern in rolinstanties

Als Extern bureaublad is ingeschakeld voor de rollen, kunt u rechtstreeks vanuit de Azure Portal een verbinding starten:

1. Klik op **instanties** om de instellingen voor **instanties** te openen.
2. Selecteer een rolinstantie waarvoor Extern bureaublad is geconfigureerd.
3. Klik op **verbinding maken** om een RDP-bestand voor de rolinstantie te downloaden.

    ![Cloud Services extern bureau blad](./media/cloud-services-role-enable-remote-desktop-new-portal/CloudServices_Remote_Desktop_Connect.png)

4. Klik op **openen** en vervolgens op **verbinding maken** om de Extern bureaublad verbinding te starten.

>[!NOTE]
> Als uw Cloud service zich achter een NSG bevindt, moet u mogelijk regels maken die verkeer op de poorten **3389** en **20000**toestaan.  Extern bureaublad maakt gebruik van poort **3389**.  Cloud service-exemplaren worden gelijkmatig verdeeld, zodat u niet rechtstreeks kunt bepalen met welk exemplaar verbinding moet worden gemaakt.  De *RemoteForwarder* -en *remoteaccess* -agents beheren RDP-verkeer en toestaan dat de client een RDP-cookie verzendt en een afzonderlijk exemplaar opgeeft waarmee verbinding moet worden gemaakt.  De *RemoteForwarder* -en *remoteaccess* -agents vereisen dat poort **20000*** is geopend. deze kan worden geblokkeerd als u een NSG hebt.

## <a name="additional-resources"></a>Aanvullende resources

[Cloud Services configureren](cloud-services-how-to-configure-portal.md)
