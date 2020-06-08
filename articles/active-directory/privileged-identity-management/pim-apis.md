---
title: Microsoft Graph-API's voor PIM (preview) - Azure AD | Microsoft Docs
description: Bevat informatie over het gebruik van de Microsoft Graph-API's voor Azure AD PIM (Privileged Identity Management) (preview).
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.workload: identity
ms.subservice: pim
ms.topic: overview
ms.date: 01/02/2020
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 35d5bb13cf9c3735c61173177a3b94f4594c9272
ms.sourcegitcommit: 958f086136f10903c44c92463845b9f3a6a5275f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2020
ms.locfileid: "83714131"
---
# <a name="microsoft-graph-apis-for-privileged-identity-management-preview"></a>Microsoft Graph-API's voor PIM (Privileged Identity Management) (preview)

U kunt Privileged Identity Management-taken uitvoeren met behulp van de [Microsoft Graph-API's](https://developer.microsoft.com/graph/docs/concepts/overview) voor Azure Active Directory. In dit artikel worden enkele belangrijke concepten beschreven bij het gebruik van de Microsoft Graph-API's voor Privileged Identity Management.

Zie de [naslag bij Azure AD Privileged Identity Management-API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/privilegedidentitymanagement_root) voor meer informatie over de Microsoft Graph-API's.

> [!IMPORTANT]
> API's onder de /bèta-versie in Microsoft Graph zijn beschikbaar als preview en kunnen worden gewijzigd. Het gebruik van deze API's in productie-apps wordt niet ondersteund.

## <a name="required-permissions"></a>Vereiste machtigingen

Voor het aanroepen van de Microsoft Graph-API's voor Privileged Identity Management, hebt u **een of meer** van de volgende machtigingen nodig:

- `Directory.AccessAsUser.All`
- `Directory.Read.All`
- `Directory.ReadWrite.All`
- `PrivilegedAccess.ReadWrite.AzureAD`

### <a name="set-permissions"></a>Machtigingen instellen

Toepassingen die de Microsoft Graph-API's voor Privileged Identity Management willen aanroepen, moeten beschikken over de vereiste machtigingen. De eenvoudigste manier om de vereiste machtigingen op te geven, is door het [toestemmingsframework van Azure AD-](../develop/consent-framework.md) te gebruiken.

### <a name="set-permissions-in-graph-explorer"></a>Machtigingen instellen in Graph Explorer

Als u Graph Explorer gebruikt om uw aanroepen te testen, kunt u de machtigingen opgeven in het hulpprogramma.

1. Meld u als globale beheerder aan bij [Graph Explorer](https://developer.microsoft.com/graph/graph-explorer).

1. Klik op **Machtigingen wijzigen**.

    ![Graph Explorer - machtigingen wijzigen](./media/pim-apis/graph-explorer.png)

1. Schakel de selectievakjes in naast de machtigingen die u wilt opnemen. `PrivilegedAccess.ReadWrite.AzureAD` is nog niet beschikbaar in Graph Explorer.

    ![Graph Explorer - machtigingen wijzigen](./media/pim-apis/graph-explorer-modify-permissions.png)

1. Klik op **Machtigingen wijzigen** om de wijzigingen in de machtigingen toe te passen.

## <a name="next-steps"></a>Volgende stappen

- [Naslag bij Azure AD Privileged Identity Management-API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/privilegedidentitymanagement_root)
