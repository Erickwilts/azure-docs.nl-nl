---
title: Azure AD beveiligde hybride toegang | Microsoft Docs
description: In dit artikel worden partner oplossingen beschreven voor het integreren van uw bestaande on-premises, open bare Cloud of persoonlijke Cloud toepassingen met Azure AD. Beveilig uw verouderde apps door app Delivery controllers of Networks te verbinden met Azure AD.
services: active-directory
author: gargi-sinha
manager: martinco
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: how-to
ms.workload: identity
ms.date: 9/10/2020
ms.author: gasinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5a936552bd7d3b97e2c027f7b0d23abd7c21b6e2
ms.sourcegitcommit: 8e7316bd4c4991de62ea485adca30065e5b86c67
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/17/2020
ms.locfileid: "94652192"
---
# <a name="secure-hybrid-access-secure-legacy-apps-with-azure-active-directory"></a>Beveiligde hybride toegang: beveiligde verouderde apps met Azure Active Directory

U kunt nu uw lokale en Cloud toepassingen voor verouderde verificatie beveiligen door deze te verbinden met Azure Active Directory (AD) met:

- [Azure AD-toepassingsproxy](#secure-hybrid-access-sha-through-azure-ad-application-proxy)

- [Uw bestaande controllers en netwerken voor de levering van toepassingen](#sha-through-networking-and-delivery-controllers)

- [VPN-toepassingen (virtueel particulier netwerk) en SDP (Software-Defined-verbindingen)](#sha-through-vpn-and-sdp-applications)

U kunt de ruimte overbruggen en uw beveiligings postuur versterken voor alle toepassingen met Azure AD-mogelijkheden, zoals [voorwaardelijke toegang tot](../conditional-access/overview.md) Azure AD en Azure AD [Identity Protection](../identity-protection/overview-identity-protection.md).

## <a name="secure-hybrid-access-sha-through-azure-ad-application-proxy"></a>Secure Hybrid Access (SHA) via Azure AD-toepassingsproxy
  
Met [toepassings proxy](./what-is-application-proxy.md) kunt u [veilige externe toegang](./application-proxy.md) tot uw on-premises webtoepassingen bieden. Uw gebruikers hoeven geen VPN te gebruiken. Gebruikers profiteren van een eenvoudige verbinding met hun toepassingen vanaf elk apparaat na een [eenmalige aanmelding](./add-application-portal-setup-sso.md). Toepassings proxy biedt externe toegang als een service en biedt u de mogelijkheid om [uw on-premises toepassingen eenvoudig te publiceren](./application-proxy-add-on-premises-application.md) naar gebruikers buiten het bedrijfs netwerk. U kunt hiermee uw beheer van Cloud toegang schalen zonder dat u uw on-premises toepassingen hoeft te wijzigen. [Plan een Azure AD-toepassingsproxy-implementatie](./application-proxy-deployment-plan.md) als een volgende stap.

## <a name="azure-ad-partner-integrations"></a>Integraties van Azure AD-partner

### <a name="sha-through-networking-and-delivery-controllers"></a>SHA via netwerk-en bezorgings controllers

Naast [Azure AD-toepassingsproxy](./what-is-application-proxy.md), zodat u het [vertrouwens raamwerk met de waarde nul](https://www.microsoft.com/security/blog/2020/04/02/announcing-microsoft-zero-trust-assessment-tool/)kunt gebruiken, micro soft-partners met externe providers. U kunt uw bestaande netwerk-en bezorgings controllers gebruiken en eenvoudig verouderde toepassingen beveiligen die essentieel zijn voor uw bedrijfs processen, maar die u niet beveiligt met Azure AD. Waarschijnlijk hebt u al alles wat u nodig hebt om deze toepassingen te beveiligen.

![Afbeelding toont beveiligde hybride toegang met netwerk partners en app proxy](./media/secure-hybrid-access/secure-hybrid-access.png)

De volgende netwerk leveranciers bieden vooraf ontwikkelde oplossingen en gedetailleerde richt lijnen voor de integratie met Azure AD.

- [Akamai Enter prise Application Access (EAA)](../saas-apps/akamai-tutorial.md)

- [Citrix Application Delivery controller (ADC)](../saas-apps/citrix-netscaler-tutorial.md)

- [F5 BIG-IP APM](../saas-apps/headerf5-tutorial.md)

- [Kemp](../saas-apps/kemp-tutorial.md)

### <a name="sha-through-vpn-and-sdp-applications"></a>SHA via VPN-en SDP-toepassingen

Met VPN-en SDP-oplossingen kunt u op elk gewenst moment beveiligde toegang bieden tot uw bedrijfs netwerk vanaf elk apparaat, op elke locatie, terwijl u de gegevens van uw organisatie beveiligt. Door Azure AD als id-provider (IDP) te hebben, kunt u moderne verificatie-en autorisatie methoden gebruiken, zoals [eenmalige aanmelding](./what-is-single-sign-on.md) voor Azure AD en [multi-factor Authentication](../authentication/concept-mfa-howitworks.md) om uw on-premises verouderde toepassingen te beveiligen.  

![Afbeelding toont beveiligde hybride toegang met VPN-partners en app proxy ](./media/secure-hybrid-access/app-proxy-vpn.png)

De volgende VPN-en SDP-leveranciers bieden vooraf ontwikkelde oplossingen en gedetailleerde richt lijnen voor de integratie met Azure AD.

• [Cisco AnyConnect](../saas-apps/cisco-anyconnect.md)

• [Fortinet](../saas-apps/fortigate-ssl-vpn-tutorial.md)

• [F5 BIG-IP apm](../saas-apps/headerf5-tutorial.md)

• [Palo Alto-netwerken wereld wijd beveiligen](../saas-apps/paloaltoadmin-tutorial.md)

• [Zscaler private Access (ZPA)](../saas-apps/zscalerprivateaccess-tutorial.md)