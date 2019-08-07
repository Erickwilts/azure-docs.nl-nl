---
title: Een groep gebruiken voor het beheren van toegang tot SaaS-toepassingen-Azure Active Directory | Microsoft Docs
description: Groepen in Azure Active Directory gebruiken om toegang toe te wijzen aan SaaS-toepassingen die zijn geïntegreerd met Azure Active Directory.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 08/01/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 75fdd0fcd4ceeeb0f680b2cbdb743d30d236c93d
ms.sourcegitcommit: c662440cf854139b72c998f854a0b9adcd7158bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2019
ms.locfileid: "68736505"
---
# <a name="using-a-group-to-manage-access-to-saas-applications"></a>Een groep gebruiken om SaaS-toepassingen te beheren

Met Azure Active Directory (Azure AD) met een Azure AD Premium licentie abonnement kunt u groepen gebruiken om toegang toe te wijzen aan een SaaS-toepassing die is geïntegreerd met Azure AD. Als u bijvoorbeeld toegang wilt toewijzen voor de marketing afdeling voor het gebruik van vijf verschillende SaaS-toepassingen, kunt u een groep maken die de gebruikers in de marketing afdeling bevat en die groep vervolgens toewijzen aan deze vijf SaaS-toepassingen die nodig zijn voor de marketing afdeling. Op deze manier kunt u tijd besparen door het lidmaatschap van de marketing afdeling op één plek te beheren. Gebruikers worden vervolgens toegewezen aan de toepassing wanneer ze worden toegevoegd als leden van de marketing groep en hun toewijzingen worden verwijderd uit de toepassing wanneer ze uit de groep Marketing worden verwijderd. Deze mogelijkheid kan worden gebruikt met honderden toepassingen die u kunt toevoegen in de Azure AD-toepassings galerie.

> [!IMPORTANT]
> U kunt deze functie alleen gebruiken nadat u een Azure AD Premium proef abonnement hebt gestart of Azure AD Premium licentie plan hebt aangeschaft.
> Toewijzing op basis van een groep wordt alleen ondersteund voor beveiligings groepen.
> Op dit moment wordt genest groepslidmaatschap niet ondersteund voor toewijzing aan een toepassing op basis van een groep.

## <a name="to-assign-access-for-a-user-or-group-to-a-saas-application"></a>Toegang voor een gebruiker of groep toewijzen aan een SaaS-toepassing

1. Selecteer in het [Azure AD-beheer centrum](https://aad.portal.azure.com) **bedrijfs toepassingen**.
2. Selecteer een toepassing die u hebt toegevoegd in de toepassings galerie om deze te openen.
3. Selecteer **gebruikers en groepen**en selecteer vervolgens **gebruiker toevoegen**.
4. Bij **toewijzing toevoegen**selecteert u **gebruikers en groepen** om de selectie lijst **gebruikers en groepen** te openen.
6. Selecteer zoveel groepen of gebruikers als u wilt en klik of tik op **selecteren** om ze toe te voegen aan de lijst **toewijzing toevoegen** . U kunt in deze fase ook een rol toewijzen aan een gebruiker.
7. Selecteer **toewijzen** om de gebruikers of groepen toe te wijzen aan de geselecteerde bedrijfs toepassing.

## <a name="next-steps"></a>Volgende stappen
Deze artikelen bevatten aanvullende informatie over Azure Active Directory.

* [Managing access to resources with Azure Active Directory groups](../fundamentals/active-directory-manage-groups.md) (Toegang tot resources beheren met Azure Active Directory-groepen)
* [Application Management in Azure Active Directory](../manage-apps/what-is-application-management.md) (Toepassingsbeheer in Azure Active Directory)
* [Azure Active Directory cmdlets for configuring group settings](groups-settings-cmdlets.md) (Azure Active Directory-cmdlets voor het configureren van groepsinstellingen)
* [Wat is Azure Active Directory?](../fundamentals/active-directory-whatis.md)
* [Uw on-premises identiteiten integreren met Azure Active Directory](../hybrid/whatis-hybrid-identity.md)
