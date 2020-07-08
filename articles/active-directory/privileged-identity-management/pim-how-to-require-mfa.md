---
title: MFA of twee ledige en Privileged Identity Management-Azure AD | Microsoft Docs
description: Meer informatie over hoe Azure AD Privileged Identity Management (PIM) multi-factor Authentication (MFA) valideert.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: how-to
ms.workload: identity
ms.subservice: pim
ms.date: 11/08/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: bf1113f7b2f396deed849fa46108537f290b53a1
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2020
ms.locfileid: "84742093"
---
# <a name="multi-factor-authentication-and-privileged-identity-management"></a>Multi-factor Authentication en Privileged Identity Management

U wordt aangeraden multi-factor Authentication (MFA) te vereisen voor al uw beheerders. Dit vermindert het risico van een aanval vanwege een wacht woord dat is aangetast.

U kunt vereisen dat gebruikers een multi-factor Authentication-Challenge uitvoeren wanneer ze zich aanmelden. U kunt ook vereisen dat gebruikers een multi-factor Authentication-Challenge uitvoeren wanneer ze een functie activeren in Azure Active Directory (Azure AD) Privileged Identity Management (PIM). Op deze manier wordt, als de gebruiker een verificatie op basis van multi-factor Authentication niet heeft voltooid, deze door Privileged Identity Management gevraagd.

> [!IMPORTANT]
> Nu werkt Azure Multi-Factor Authentication alleen met werk-of school accounts, niet van micro soft-persoonlijke accounts (meestal een persoonlijk account dat wordt gebruikt om u aan te melden bij micro soft-services zoals Skype, Xbox of Outlook.com). Als gevolg hiervan kan iedereen met een persoonlijk account geen geschikte beheerder zijn, omdat ze geen multi-factor Authentication kunnen gebruiken om hun rollen te activeren. Als deze gebruikers de werk belasting met behulp van een Microsoft-account blijven beheren, moet u deze nu voor permanente beheerders verhogen.

## <a name="how-pim-validates-mfa"></a>Hoe PIM MFA valideert

Er zijn twee opties voor het valideren van multi-factor Authentication wanneer een gebruiker een rol activeert.

De eenvoudigste optie is afhankelijk van Azure Multi-Factor Authentication voor gebruikers die een geprivilegieerde rol activeren. Om dit te doen, moet u eerst controleren of deze gebruikers een licentie hebben, indien nodig en zijn geregistreerd voor Azure Multi-Factor Authentication. Zie voor meer informatie over het implementeren van Azure Multi-Factor Authentication [Azure-multi-factor Authentication in de Cloud implementeren](../authentication/howto-mfa-getstarted.md). Het wordt aanbevolen, maar niet vereist, dat u Azure AD configureert om multi-factor Authentication af te dwingen voor deze gebruikers wanneer ze zich aanmelden. Dit komt doordat de multi-factor Authentication-controles door Privileged Identity Management zichzelf worden uitgevoerd.

Als gebruikers on-premises verifiëren, kunt u er ook voor zorgen dat uw ID-provider verantwoordelijk is voor multi-factor Authentication. Als u bijvoorbeeld AD Federation Services hebt geconfigureerd om verificatie op basis van Smart Cards te vereisen voordat u toegang krijgt tot Azure AD, kunt u [cloud resources beveiligen met azure multi-factor Authentication en AD FS](../authentication/howto-mfa-adfs.md) bevat instructies voor het configureren van AD FS voor het verzenden van claims naar Azure AD. Wanneer een gebruiker probeert een rol te activeren, accepteert Privileged Identity Management dat multi-factor Authentication al is gevalideerd voor de gebruiker nadat het de juiste claims heeft ontvangen.

## <a name="next-steps"></a>Volgende stappen

- [Instellingen voor Azure AD-functies configureren in Privileged Identity Management](pim-how-to-change-default-settings.md)
- [Instellingen voor Azure-resource-rollen configureren in Privileged Identity Management](pim-resource-roles-configure-role-settings.md)
