---
title: Probleem bij het maken van een toepassing toepassingsproxy | Microsoft Docs
description: Het oplossen van problemen met het maken van de toepassingsproxy-toepassingen in de Azure AD-beheerportal
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/21/2018
ms.author: mimart
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 104b98cba0948ec5d0896877e54eab1e7cd4049f
ms.sourcegitcommit: be9fcaace62709cea55beb49a5bebf4f9701f7c6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2019
ms.locfileid: "65825808"
---
# <a name="problem-creating-an-application-proxy-application"></a>Probleem bij het maken van een toepassing Application Proxy 

Hieronder vindt u enkele van de algemene problemen gezichten van mensen bij het maken van een nieuwe application proxy-toepassing.

## <a name="recommended-documents"></a>Aanbevolen documenten 

Zie voor meer informatie over het maken van een Application Proxy-toepassing via de beheerportal, [toepassingen publiceren die gebruikmaken van Azure AD-toepassingsproxy](application-proxy-add-on-premises-application.md).

Als u de stappen in dit document volgen en zijn er een fout optreedt bij het maken van de toepassing, Zie de foutdetails voor meer informatie en suggesties voor het oplossen van de toepassing. De meeste foutberichten bevatten een voorgestelde oplossing. 

## <a name="specific-things-to-check"></a>Bepaalde dingen om te controleren

Algemene om fouten te voorkomen, controleert u of:

-   U bent een beheerder met de machtiging voor het maken van een toepassing Application Proxy

-   De interne URL is uniek

-   De externe URL is uniek

-   De URL's met http of https beginnen en eindigen met een '/'

-   De URL moet de naam van een domein en niet een IP-adres

Het foutbericht moet worden weergegeven in de rechterbovenhoek bij het maken van de toepassing. U kunt ook het meldingspictogram om te zien van de foutberichten selecteren.

   ![Melding prompt](./media/application-proxy-config-problem/error-message.png)

## <a name="next-steps"></a>Volgende stappen
[Toepassingsproxy inschakelen in Azure portal](application-proxy-add-on-premises-application.md)
