---
title: RDP-eigenschappen aanpassen met Power shell-Azure
description: RDP-eigenschappen voor virtuele Windows-Bureau bladen aanpassen met Power shell-cmdlets.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 06/03/2019
ms.author: helohr
ms.openlocfilehash: 624edaea9a0fb56e34eb83f033dfdab64985bd5c
ms.sourcegitcommit: 78ebf29ee6be84b415c558f43d34cbe1bcc0b38a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/12/2019
ms.locfileid: "68950716"
---
# <a name="customize-remote-desktop-protocol-properties-for-a-host-pool"></a>Remote Desktop Protocol eigenschappen voor een hostgroep aanpassen

Als u de eigenschappen van de Remote Desktop Protocol (RDP) van een hostgroep wilt aanpassen, zoals de ervaring voor meerdere monitors en audio-omleiding, kunt u een optimale ervaring bieden aan uw gebruikers op basis van hun behoeften. U kunt RDP-eigenschappen in virtueel bureau blad van Windows aanpassen met behulp van de para meter **-CustomRdpProperty** in de cmdlet **set-RdsHostPool** .

Zie [extern bureaublad RDP-Bestands instellingen](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/rdp-files) voor een volledige lijst met ondersteunde eigenschappen en hun standaard waarden.

[Down load en Importeer eerst de Windows Virtual Desktop Power shell-module](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview) voor gebruik in uw Power shell-sessie als u dat nog niet hebt gedaan.

## <a name="add-or-edit-a-single-custom-rdp-property"></a>Eén aangepaste RDP-eigenschap toevoegen of bewerken

Als u één aangepaste RDP-eigenschap wilt toevoegen of bewerken, voert u de volgende Power shell-cmdlet uit:

```powershell
Set-RdsHostPool -TenantName <tenantname> -Name <hostpoolname> -CustomRdpProperty "<property>"
```
![Een scherm opname van de Power shell-cmdlet Get-RDSRemoteApp met de naam en FriendlyName is gemarkeerd.](media/singlecustomrdpproperty.png)

## <a name="add-or-edit-multiple-custom-rdp-properties"></a>Meerdere aangepaste RDP-eigenschappen toevoegen of bewerken

Als u meerdere aangepaste RDP-eigenschappen wilt toevoegen of bewerken, voert u de volgende Power shell-cmdlets uit door de aangepaste RDP-eigenschappen op te geven als een door punt komma's gescheiden teken reeks:

```powershell
$properties="<property1>;<property2>;<property3>"
Set-RdsHostPool -TenantName <tenantname> -Name <hostpoolname> -CustomRdpProperty $properties
```
![Een scherm opname van de Power shell-cmdlet Get-RDSRemoteApp met de naam en FriendlyName is gemarkeerd.](media/multiplecustomrdpproperty.png)

## <a name="reset-all-custom-rdp-properties"></a>Alle aangepaste RDP-eigenschappen opnieuw instellen

U kunt afzonderlijke aangepaste RDP-eigenschappen opnieuw instellen op de standaard waarden door de instructies in [een enkele aangepaste RDP-eigenschap toevoegen of bewerken](#add-or-edit-a-single-custom-rdp-property)te volgen, of u kunt alle aangepaste RDP-eigenschappen voor een hostgroep opnieuw instellen door de volgende Power shell-cmdlet uit te voeren:

```powershell
Set-RdsHostPool -TenantName <tenantname> -Name <hostpoolname> -CustomRdpProperty ""
```
![Een scherm opname van de Power shell-cmdlet Get-RDSRemoteApp met de naam en FriendlyName is gemarkeerd.](media/resetcustomrdpproperty.png)

## <a name="next-steps"></a>Volgende stappen

Nu u de RDP-eigenschappen voor een bepaalde hostgroep hebt aangepast, kunt u zich aanmelden bij een virtueel-bureaubladclient van Windows om ze te testen als onderdeel van een gebruikers sessie. Als u dit wilt doen, gaat u naar de uitleg verbinding maken met Windows virtueel bureau blad:

- [Verbinding maken vanaf Windows 10 en Windows 7](connect-windows-7-and-10.md)
- [Verbinding maken via een webbrowser](connect-web.md)
