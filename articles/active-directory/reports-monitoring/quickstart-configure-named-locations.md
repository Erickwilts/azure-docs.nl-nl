---
title: Benoemde locaties configureren in Azure Active Directory | Microsoft Docs
description: Leren hoe benoemde locaties te configureren.
services: active-directory
documentationcenter: ''
author: cawrites
manager: daveba
ms.assetid: f56e042a-78d5-4ea3-be33-94004f2a0fc3
ms.service: active-directory
ms.workload: identity
ms.subservice: report-monitor
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 11/13/2018
ms.author: chadam
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 540bc8a4dbfda83ff6e0480bc216a38742bbef2b
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/13/2019
ms.locfileid: "68988085"
---
# <a name="quickstart-configure-named-locations-in-azure-active-directory"></a>Quickstart: Benoemde locaties configureren in Azure Active Directory

Met benoemde locaties, kunt u vertrouwde IP-adresbereiken in uw organisatie een label geven. Azure AD maakt gebruik van benoemde locaties om:
- Fout-positieven in [risicogebeurtenissen](concept-risk-events.md) te detecteren. Aanmelden van een vertrouwde benoemde locaties verlaagt het aanmeldingsrisico van een gebruiker.   
- Configureer [op locatie gebaseerde voorwaardelijke toegang](../conditional-access/location-condition.md).

In deze snelstartgids leert u hoe benoemde locaties in uw omgeving te configureren.

## <a name="prerequisites"></a>Vereisten

U hebt het volgende nodig om deze quickstart te voltooien:

* Een Azure AD-tenant. Meld u aan voor een [gratis proefversie](https://azure.microsoft.com/trial/get-started-active-directory/). 
* Een gebruiker die een globale administrator is voor de tenant.
* Een IP-adresbereik dat is tot stand gebracht en betrouwbaar in uw organisatie. Het IP-adresbereik moet in **Classless Interdomain routering (CIDR)** indeling zijn.

## <a name="configure-named-locations"></a>Benoemde locaties configureren

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Selecteer **Azure Active Directory**in het linkerdeel venster en selecteer vervolgens **voorwaardelijke toegang** in het gedeelte **beveiliging** .

    ![Tabblad voorwaardelijke toegang](./media/quickstart-configure-named-locations/entrypoint.png)

3. Op de pagina **Voorwaardelijke toegang**, selecteer **Benoemde locaties** en selecteer **Nieuwe locatie**.

    ![Benoemde locatie](./media/quickstart-configure-named-locations/namedlocation.png)

6. Vul het formulier in op de nieuwe pagina. 

   * In het vak **Naam** typt u een naam voor uw benoemde locatie.
   * In de **IP-adresbereiken** typt u het IP-adresbereik in CIDR-indeling.  
   * Klik op **Create**.
    
     ![De nieuwe blade](./media/quickstart-configure-named-locations/61.png)

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie:

- [Voorwaardelijke toegang voor Azure AD](../active-directory-conditional-access-azure-portal.md).
- [Voor waarden voor locaties in voorwaardelijke toegang voor Azure AD](../conditional-access/location-condition.md)
- [Rapport voor riskante aanmeldingen](concept-risky-sign-ins.md).  
