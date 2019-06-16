---
title: Geavanceerde scenario's met Azure MFA en externe VPN - Azure Active Directory
description: Stapsgewijze configuratiehandleidingen voor Azure MFA integreren met Cisco, Citrix en Juniper.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2ad2b3075ae9d5ccd7e32f039fbbbc8583cde73c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67055957"
---
# <a name="advanced-scenarios-with-azure-multi-factor-authentication-and-third-party-vpn-solutions"></a>Geavanceerde scenario's met Azure multi-factor Authentication en VPN-oplossingen van derden

Azure multi-factor Authentication kan worden gebruikt om het naadloos verbinding maken met verschillende VPN-oplossingen van derden. In dit artikel is gericht op Cisco® ASA VPN-apparaat, Citrix NetScaler SSL VPN-apparaat en het Juniper netwerken beveiligde toegang/Pulse Secure verbinding maken met beveiligde SSL VPN-apparaat. We hebben gemaakt configuratiehandleidingen om deze drie algemene apparaten op te lossen. Multi-factor Authentication-Server kan ook worden geïntegreerd met de meeste andere systemen die gebruikmaken van RADIUS, LDAP, IIS of verificatie op basis van claims voor AD FS. U vindt meer informatie in [MFA-Server-configuraties](howto-mfaserver-deploy.md#next-steps).

> [!IMPORTANT]
> Vanaf 1 juli 2019, zal Microsoft MFA-Server niet meer bieden voor nieuwe implementaties. Nieuwe klanten die willen graag meervoudige verificatie van gebruikers vereisen moeten cloud-gebaseerde Azure multi-factor Authentication gebruiken. Bestaande klanten die vóór 1 juli MFA-Server hebben geactiveerd, worden kunnen de nieuwste versie downloaden door toekomstige updates en zoals gebruikelijk activeringsreferenties genereren.

## <a name="cisco-asa-vpn-appliance-and-azure-multi-factor-authentication"></a>Cisco ASA-VPN-apparaat en Azure multi-factor Authentication
Azure multi-factor Authentication kan worden geïntegreerd met uw Cisco® ASA VPN-apparaat voor extra beveiliging voor Cisco AnyConnect® VPN aanmeldingen als portaltoegang.  U kunt de LDAP- of RADIUS-protocol gebruiken.  Selecteer een van de volgende voor het downloaden van de gedetailleerde stapsgewijze configuratiehandleidingen.

| Configuratiehandleiding | Description |
| --- | --- |
| [Cisco ASA met Anyconnect VPN- en Azure MFA-configuratie voor LDAP](https://download.microsoft.com/download/A/2/0/A201567C-C3DE-4227-AF89-4567A470899E/Cisco_ASA_Azure_MFA_LDAP.docx) | Integreer uw Cisco ASA-VPN-apparaat met Azure MFA met behulp van LDAP |
| [Cisco ASA met Anyconnect VPN- en Azure MFA-configuratie voor RADIUS](https://download.microsoft.com/download/4/5/7/4579C1CF-35B0-4FBE-8A1A-B49CB2CC0382/Cisco_ASA_Azure_MFA_RADIUS.docx) | Integreer uw Cisco ASA-VPN-apparaat met Azure MFA met behulp van RADIUS |

## <a name="citrix-netscaler-ssl-vpn-and-azure-multi-factor-authentication"></a>Citrix NetScaler SSL VPN- en Azure multi-factor Authentication
Azure multi-factor Authentication kan worden geïntegreerd met uw Citrix NetScaler SSL VPN-apparaat voor extra beveiliging voor Citrix NetScaler SSL VPN-aanmeldingen als portaltoegang.  U kunt de LDAP- of RADIUS-protocol gebruiken.  Selecteer een van de volgende voor het downloaden van de gedetailleerde stapsgewijze configuratiehandleidingen.

| Configuratiehandleiding | Description |
| --- | --- |
| [Citrix NetScaler SSL VPN- en Azure MFA-configuratie voor LDAP](https://download.microsoft.com/download/2/4/E/24E1E722-72DF-471F-A88A-D1338DB1AF83/Citrix_NS_Azure_MFA_LDAP.docx) | Uw SSL VPN van Citrix NetScaler integreren met Azure MFA-apparaat met behulp van LDAP |
| [Citrix NetScaler SSL VPN- en Azure MFA-configuratie voor RADIUS](https://download.microsoft.com/download/1/A/4/1A482764-4A63-45C2-A5EC-2B673ACCDD12/Citrix_NS_Azure_MFA_RADIUS.docx) | Integreer uw Citrix NetScaler SSL VPN-apparaat met Azure MFA met behulp van RADIUS |

## <a name="juniperpulse-secure-ssl-vpn-appliance-and-azure-multi-factor-authentication"></a>Juniper/Pulse Secure SSL VPN-apparaat en Azure multi-factor Authentication
Azure multi-factor Authentication kan worden geïntegreerd met uw Juniper/Pulse Secure SSL VPN-apparaat voor extra beveiliging voor Juniper/Pulse Secure SSL VPN-aanmeldingen als portaltoegang.  U kunt de LDAP- of RADIUS-protocol gebruiken.  Selecteer een van de volgende voor het downloaden van de gedetailleerde stapsgewijze configuratiehandleidingen.

| Configuratiehandleiding | Description |
| --- | --- |
| [Juniper/Pulse Secure SSL VPN- en Azure MFA-configuratie voor LDAP](https://download.microsoft.com/download/6/5/8/6587B418-75B1-4FCB-84D4-984BC479309E/JuniperPulse_Azure_MFA_LDAP.docx) | Uw Juniper/Pulse Secure SSL VPN integreren met Azure MFA-apparaat met behulp van LDAP |
| [Juniper/Pulse Secure SSL VPN- en Azure MFA-configuratie voor RADIUS](https://download.microsoft.com/download/7/9/A/79AB3DAD-4799-4379-B1DA-B95ABDF231DC/JuniperPulse_Azure_MFA_RADIUS.docx) | Integreer uw Juniper/Pulse Secure SSL VPN-apparaat met Azure MFA met behulp van RADIUS |

## <a name="next-steps"></a>Volgende stappen

- [Verbeter uw bestaande infrastructuur voor verificatie met de NPS-extensie voor Azure multi-factor Authentication](howto-mfa-nps-extension.md)

- [Instellingen configureren voor Azure Multi-Factor Authentication](howto-mfa-mfasettings.md)
