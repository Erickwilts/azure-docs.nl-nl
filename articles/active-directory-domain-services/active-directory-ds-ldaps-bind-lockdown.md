---
title: Binden aan een Azure AD Domain Services beheerde domein met behulp van Secure LDAP (LDAPS) | Microsoft Docs
description: Verbinding maken met een Azure AD Domain Services beheerde domein met behulp van veilige LDAP (LDAPS)
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: 6871374a-0300-4275-9a45-a39a52c65ae4
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: iainfou
ms.openlocfilehash: df0b3d27eec478280a33be831a2431eccdf05a74
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67483374"
---
# <a name="bind-to-an-azure-ad-domain-services-managed-domain-using-secure-ldap-ldaps"></a>Verbinding maken met een Azure AD Domain Services beheerde domein met behulp van veilige LDAP (LDAPS)

## <a name="before-you-begin"></a>Voordat u begint
Volledige [taak 4: configureren van DNS voor toegang tot het beheerde domein van het internet](active-directory-ds-ldaps-configure-dns.md).


## <a name="task-5-bind-to-the-managed-domain-over-ldap-using-ldpexe"></a>Taak 5: Verbinding maken met het beheerde domein via LDAP met LDP.exe
U kunt het hulpprogramma LDP.exe, die is opgenomen in het pakket Remote Server Administration tools te binden en zoek via LDAP gebruiken.

Eerst LDP te openen en verbinding maken met het beheerde domein. Klik op **verbinding** en klikt u op **verbinden...**  in het menu. Geef de DNS-domeinnaam van het beheerde domein. Geef de poort moet worden gebruikt voor verbindingen. Gebruik poort 389 voor LDAP-verbindingen. Gebruik poort 636 voor LDAPS-verbindingen. Klik op **OK** knop verbinding maken met het beheerde domein.

Vervolgens verbinding maken met het beheerde domein. Klik op **verbinding** en klikt u op **verbinden...**  in het menu. Geef de referenties van een gebruikersaccount van de groep 'AAD DC Administrators'.

> [!IMPORTANT]
> Gebruikers (en serviceaccounts) uitvoeren niet eenvoudige LDAP-bindingen als u NTLM-wachtwoord-hashsynchronisatie hebt uitgeschakeld op uw Azure AD Domain Services-exemplaar.  Lees voor meer informatie over het uitschakelen van NTLM-wachtwoord-hashsynchronisatie [beveiligen van uw Azure AD DOmain Services beheerde domein](secure-your-domain.md).
>
>

Selecteer **weergave**, en selecteer vervolgens **structuur** in het menu. De Base DN-veld leeg laat, en klik op OK. Navigeer naar de container die u wilt zoeken en selecteer zoeken met de rechtermuisknop op de container.

> [!TIP]
> - Gebruikers en groepen die zijn gesynchroniseerd van Azure AD worden opgeslagen in de **AADDC gebruikers** organisatie-eenheid. Het zoekpad voor deze organisatie-eenheid ziet eruit als ```OU=AADDC Users,DC=CONTOSO100,DC=COM```.
> - Computeraccounts voor computers die zijn gekoppeld aan het beheerde domein worden opgeslagen in de **AADDC Computers** organisatie-eenheid. Het zoekpad voor deze organisatie-eenheid ziet eruit als ```OU=AADDC Computers,DC=CONTOSO100,DC=COM```.
>
>

Meer informatie - [grondbeginselen van de LDAP-query](https://docs.microsoft.com/windows/desktop/ad/creating-a-query-filter)


## <a name="task-6-lock-down-secure-ldap-access-to-your-managed-domain-over-the-internet"></a>Taak 6: Toegang van secure LDAP tot uw beheerde domein te vergrendelen via internet
> [!NOTE]
> Als u via internet LDAPS toegang tot het beheerde domein niet hebt ingeschakeld, slaat u deze taak voor de configuratie.
>
>

Voordat u deze taak uitvoert, complete de stappen die worden beschreven in [taak 3](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md).

Wanneer u LDAPS-toegang via internet met uw beheerde domein inschakelt, maakt het een beveiligingsrisico. Het beheerde domein is bereikbaar is vanaf het internet op de poort die wordt gebruikt voor secure LDAP (dat wil zeggen, poort 636). U kunt toegang tot het beheerde domein specifieke bekende IP-adressen te beperken. Maak een netwerkbeveiligingsgroep (NSG) en deze koppelen aan het subnet waarin u Azure AD Domain Services hebt ingeschakeld.

Het voorbeeld NSG in de volgende tabel wordt vergrendeld toegang van secure LDAP via internet. De regels in de NSG toestaan binnenkomende toegang van secure LDAP via TCP-poort 636 alleen uit een opgegeven set van IP-adressen. De standaardregel voor 'DenyAll' is van toepassing op alle andere binnenkomend verkeer van internet. De NSG-regel om toe te staan van LDAPS toegang via internet vanaf een opgegeven IP-adressen heeft een hogere prioriteit dan de DenyAll NSG-regel.

![Voorbeeld NSG voor het beveiligen van LDAPS toegang via internet](./media/active-directory-domain-services-admin-guide/secure-ldap-sample-nsg.png)

**Meer informatie** - [Netwerkbeveiligingsgroepen](../virtual-network/security-overview.md).


## <a name="related-content"></a>Gerelateerde inhoud
* [Azure AD Domain Services - handleiding aan de slag](create-instance.md)
* [Een Azure AD Domain Services-domein beheren](manage-domain.md)
* [Grondbeginselen van de LDAP-query](https://docs.microsoft.com/windows/desktop/ad/creating-a-query-filter)
* [Beheren van Groepsbeleid voor Azure AD Domain Services](manage-group-policy.md)
* [Netwerkbeveiligingsgroepen](../virtual-network/security-overview.md)
* [Een Netwerkbeveiligingsgroep maken](../virtual-network/tutorial-filter-network-traffic.md)


## <a name="next-step"></a>Volgende stap
[Oplossen van secure LDAP in een beheerd domein](tshoot-ldaps.md)
