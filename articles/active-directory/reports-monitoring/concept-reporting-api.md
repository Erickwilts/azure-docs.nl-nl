---
title: Aan de slag met de rapportage-API voor Azure AD | Microsoft Docs
description: Aan de slag met de API voor Azure Active Directory rapportage
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 8813b911-a4ec-4234-8474-2eef9afea11e
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: c841195cacd8c8d7698f69435572587bf04c3d67
ms.sourcegitcommit: ae8b23ab3488a2bbbf4c7ad49e285352f2d67a68
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2019
ms.locfileid: "74008363"
---
# <a name="get-started-with-the-azure-active-directory-reporting-api"></a>Aan de slag met de API voor Azure Active Directory rapportage

Azure Active Directory biedt u diverse [rapporten](overview-reports.md), met nuttige informatie voor toepassingen, zoals Siem-systemen, controle en Business Intelligence-hulpprogram ma's. 

Met de Microsoft Graph-API voor Azure AD-rapporten kunt u programmatisch toegang krijgen tot de gegevens via een set op REST gebaseerde Api's. U kunt deze API's vanuit een groot aantal computertalen en hulpprogramma's aanroepen.

Dit artikel bevat een overzicht van de rapportage-API, inclusief manieren om toegang te krijgen tot het rapport.

Zie [ondersteuning voor Azure Active Directory krijgen](https://docs.microsoft.com/azure/active-directory/active-directory-troubleshooting-support-howto)als u problemen ondervindt.

## <a name="prerequisites"></a>Vereisten

Voor toegang tot de rapportage-API, met of zonder tussen komst van de gebruiker, moet u het volgende doen:

1. Rollen toewijzen (beveiligings lezer, beveiligings beheerder, globale beheerder)
2. Een toepassing registreren
3. Machtigingen verlenen
4. Configuratie-instellingen verzamelen

Zie voor gedetailleerde instructies de [vereisten voor toegang tot de Azure Active Directory rapportage-API](howto-configure-prerequisites-for-reporting-api.md). 

## <a name="api-endpoints"></a>API-eind punten 

Het Microsoft Graph API-eind punt voor audit Logboeken is `https://graph.microsoft.com/beta/auditLogs/directoryAudits` en het Microsoft Graph API-eind punt voor aanmeldingen is `https://graph.microsoft.com/beta/auditLogs/signIns`. Zie de naslag informatie over de [controle-API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit) en [aanmeldings-API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signIn)voor meer gegevens.

Daarnaast kunt u de [API voor risico detectie van identiteits beveiliging](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/identityriskevent) gebruiken om programmatisch toegang te krijgen tot beveiligings detecties met behulp van Microsoft Graph. Zie [aan de slag met Azure Active Directory Identity Protection en Microsoft Graph](../identity-protection/graph-get-started.md)voor meer informatie. 

> [!NOTE]
>  De **https:\/\/graph.windows.net\/\<Tenant naam\>\/rapporten**\/eind punt wordt afgeschaft. Gebruik de nieuwe API-eind punten die hierboven worden beschreven om via een programma toegang te krijgen tot de activiteiten en beveiligings rapporten.
  
## <a name="apis-with-graph-explorer"></a>Api's met Graph Explorer

U kunt de [MSGraph Explorer](https://developer.microsoft.com/graph/graph-explorer) gebruiken om uw aanmeld-en controle-API-gegevens te verifiëren. Meld u aan bij uw account met behulp van beide aanmeld knoppen in de gebruikers interface van Graph Explorer en stel **AuditLog. Read. all** en **Directory. Read. all** in voor uw Tenant, zoals wordt weer gegeven.   

![Graph Explorer](./media/concept-reporting-api/graph-explorer.png)

![Machtigingen voor gebruikers interface wijzigen](./media/concept-reporting-api/modify-permissions.png)

## <a name="use-certificates-to-access-the-azure-ad-reporting-api"></a>Certificaten gebruiken voor toegang tot de Azure AD-rapportage-API 

Gebruik de rapportage-API van Azure AD met certificaten als u rapportage gegevens wilt ophalen zonder tussen komst van de gebruiker.

Zie [gegevens ophalen met de rapportage-API van Azure AD met certificaten](tutorial-access-api-with-certificates.md)voor gedetailleerde instructies.

## <a name="next-steps"></a>Volgende stappen

 * [Vereisten voor toegang tot de rapportage-API](howto-configure-prerequisites-for-reporting-api.md) 
 * [Gegevens ophalen met de rapportage-API van Azure AD met certificaten](tutorial-access-api-with-certificates.md)
 * [Fouten in de rapportage-API van Azure AD oplossen](troubleshoot-graph-api.md)


