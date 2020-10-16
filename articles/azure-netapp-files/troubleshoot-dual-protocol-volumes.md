---
title: Problemen met Dual-protocol volumes voor Azure NetApp Files oplossen | Microsoft Docs
description: Hierin worden fout berichten en oplossingen beschreven die u kunnen helpen bij het oplossen van problemen met dubbele protocollen voor Azure NetApp Files.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 10/01/2020
ms.author: b-juche
ms.openlocfilehash: cc046a27fec1b9e361ff840c7ae0f077e2987b67
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "91654148"
---
# <a name="troubleshoot-dual-protocol-volumes"></a>Problemen met volumes van dubbele protocollen oplossen

In dit artikel worden oplossingen beschreven voor fout situaties die u mogelijk hebt tijdens het maken of beheren van Dual-protocol volumes.

## <a name="error-conditions-and-resolutions"></a>Fout voorwaarden en oplossingen

|     Fout voorwaarden    |     Oplossing    |
|-|-|
| Het maken van het volume met twee protocollen mislukt met de fout `This Active Directory has no Server root CA Certificate` .    |     Als deze fout optreedt wanneer u een Dual-protocol volume maakt, moet u ervoor zorgen dat het basis-CA-certificaat is geüpload in uw NetApp-account.    |
| Het maken van het volume met twee protocollen mislukt met de fout `Failed to validate LDAP configuration, try again after correcting LDAP configuration` .    |  Houd rekening met de volgende oplossingen:   <ul><li>Zorg ervoor dat het vereiste basis certificaat is toegevoegd wanneer u Active Directory (AD) toevoegt aan het NetApp-account. Zie het [open bare basis certificaat van de certificerings instantie uploaden Active Directory](create-volumes-dual-protocol.md#upload-active-directory-certificate-authority-public-root-certificate).   </li><li>De PTR-record (pointer) van de computer van de AD-host ontbreekt mogelijk op de DNS-server. U moet een zone voor reverse lookup maken op de DNS-server en vervolgens een PTR-record van de AD-hostcomputer toevoegen in de zone voor reverse lookup. <br> Stel dat het IP-adres van de AD-machine is `1.1.1.1` , de hostnaam van de AD-machine (zoals gevonden met behulp van de `hostname` opdracht) `AD1` en de domein naam `myDomain.com` .  De PTR-record die is toegevoegd aan de zone voor reverse lookup moet zijn `1.1.1.1`  ->  `AD1.myDomain.com` . </li></ul>  |
| Het maken van het volume met twee protocollen mislukt met de fout `Failed to create the Active Directory machine account \\\"TESTAD-C8DD\\\". Reason: Kerberos Error: Pre-authentication information was invalid Details: Error: Machine account creation procedure failed\\n [ 434] Loaded the preliminary configuration.\\n [ 537] Successfully connected to ip 1.1.1.1, port 88 using TCP\\n**[ 950] FAILURE` . |  Deze fout geeft aan dat het AD-wacht woord onjuist is wanneer Active Directory is gekoppeld aan het NetApp-account. Werk de AD-verbinding bij met het juiste wacht woord en probeer het opnieuw. |
| Het maken van het volume met twee protocollen mislukt met de fout `Could not query DNS server. Verify that the network configuration is correct and that DNS servers are available` . |   Deze fout geeft aan dat DNS niet bereikbaar is. De reden hiervoor is dat de DNS-IP onjuist is of dat er een probleem is met het netwerk. Controleer het DNS-IP-adres dat is ingevoerd in de AD-verbinding en controleer of het IP-adres juist is. <br> Zorg er ook voor dat de AD en het volume zich in dezelfde regio bevinden en in hetzelfde VNet. Als ze zich in verschillende VNETs bevinden, moet u ervoor zorgen dat VNet-peering tot stand is gebracht tussen de twee VNets.|
| De machtiging wordt geweigerd bij het koppelen van een Dual-protocol volume. | Een volume met dubbele protocollen ondersteunt zowel de NFS-als de SMB-protocollen.  Wanneer u probeert toegang te krijgen tot het gekoppelde volume op het UNIX-systeem, probeert het systeem de UNIX-gebruiker toe te wijzen die u gebruikt voor een Windows-gebruiker. Als er geen toewijzing wordt gevonden, treedt de fout ' permission denied ' op. <br> Deze situatie is ook van toepassing wanneer u de gebruiker ' root ' gebruikt voor de toegang. <br> Om het probleem ' toestemming geweigerd ' te vermijden, moet u ervoor zorgen dat Windows Active Directory bevat `pcuser` voordat u toegang krijgt tot het koppel punt. Als u toevoegt `pcuser` nadat het probleem ' toestemming geweigerd ' is aangetroffen, wacht u 24 uur totdat de cache vermelding is gewist voordat u de toegang opnieuw probeert. |

## <a name="next-steps"></a>Volgende stappen  

* [Een volume met dubbele protocollen maken](create-volumes-dual-protocol.md)
* [Een NFS-client voor Azure NetApp Files configureren](configure-nfs-clients.md)
