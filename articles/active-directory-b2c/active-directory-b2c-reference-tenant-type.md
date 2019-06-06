---
title: Regionale beschikbaarheid en opslaglocaties in Azure Active Directory B2C | Microsoft Docs
description: Een onderwerp over de verschillende typen Azure Active Directory B2C-tenants.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/10/2017
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 30f13a3b85e68babcaef62b9200a296105b3ce83
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66509009"
---
# <a name="azure-active-directory-b2c-region-availability--data-residency"></a>Azure Active Directory B2C: Regionale beschikbaarheid en opslaglocaties
Beschikbaarheid in regio's en gegevensopslag zijn twee zeer verschillende concepten die anders van toepassing op Azure AD B2C van de rest van Azure. In dit artikel worden de verschillen tussen deze twee concepten uitgelegd en hoe ze van toepassing op Azure ten opzichte van Azure AD B2C vergelijken.

## <a name="summary"></a>Samenvatting
Azure AD B2C is **algemeen beschikbaar over de hele wereld** met de optie voor **gegevensresidentie in de Verenigde Staten of Europa**.

## <a name="concepts"></a>Concepten
* **Beschikbaarheid in regio** verwijst naar waar een service beschikbaar voor gebruik is.
* **Gegevensresidentie** verwijst naar waar de gebruikersgegevens zijn opgeslagen.

## <a name="region-availability"></a>Beschikbaarheid in regio’s
Azure AD B2C is wereldwijd beschikbaar via de openbare cloud van Azure. 

Dit verschilt van het model meeste andere Azure-services-Volg die Combineer beschikbaarheid met gegevensresidentie. Ziet u voorbeelden van deze in een van beide Azure [beschikbare producten per regio](https://azure.microsoft.com/regions/services/) pagina en de [Active Directory B2C-prijscalculator](https://azure.microsoft.com/pricing/details/active-directory-b2c/).

## <a name="data-residency"></a>Gegevensresidentie
Azure AD B2C slaat gebruikersgegevens in de Verenigde Staten of Europa.

Gegevensresidentie wordt bepaald op basis van welke land/regio is geselecteerd als [het maken van een Azure AD B2C-tenant](active-directory-b2c-get-started.md).

![Schermafbeelding van een preview-tenant](./media/active-directory-b2c-reference-tenant-type/data-residency-b2c-tenant.png)

Gegevens zich bevinden in de Verenigde Staten voor de volgende landen/regio's:

> United States, Canada, Costa Rica, Dominican Republic, El Salvador, Guatemala, Mexico, Panama, Puerto Rico and Trinidad & Tobago

Gegevens zich in Europa bevinden voor de volgende landen/regio's:

> Algerije, Oostenrijk, Azerbeidzjan, Bahrein, Belarus, België, Bulgarije, Kroatië, Cyprus, Tsjechië, Denemarken, Egypte, Estland, Finland, Frankrijk, Duitsland, Griekenland, Hongarije, IJsland, Ierland, Israël, Italië, Jordanië, Kazachstan, Kenia, Koeweit, Letland, Libanon Liechtenstein, Litouwen, Luxemburg, Noord Macedonië, Malta, Montenegro, Marokko, Nederland, Nigeria, Noorwegen, Oman, Pakistan, Polen, Portugal, Qatar, Roemenië, Rusland, Saoedi-Arabië, Servië, Slowakije, Slovenië, Zuid-Afrika, Spanje, Zweden, Zwitserland, Tunesië, Turkije, Oekraïne, Verenigde Arabische Emiraten en Verenigd Koninkrijk.

De overige landen/regio's zijn momenteel wordt toegevoegd aan de lijst.  U kunt nog steeds Azure AD B2C door selectie van een van de bovenstaande landen/regio nu gebruiken.

> Afghanistan, Argentinië, Australië, Brazilië, Chili, Colombia, Ecuador, Hongkong SAR, India, Indonesië, Irak, Japan, Korea, Maleisië, Nieuw-Zeeland, Paraguay, Peru, Filippijnen, Singapore, Sri Lanka, Taiwan, Thailand, Uruguay en Venezuela.

## <a name="preview-tenant"></a>Preview-tenant
Als u een B2C-tenant hebt gemaakt tijdens de preview-periode van Azure AD B2C, is het waarschijnlijk dat uw **type Tenant** zegt **Preview-tenant**. Als dit het geval is, moet u uw tenant alleen voor ontwikkeling en tests en niet voor productie-apps gebruiken.

> [!IMPORTANT]
> Er is geen migratiepad van een preview van B2C-tenant naar productieschaal B2C-tenant. Let op: Er zijn bekende problemen wanneer u een preview van B2C-tenant verwijderen en opnieuw maken van een productieschaal B2C-tenant met dezelfde domeinnaam. U moet een productieschaal B2C-tenant maken met de naam van een ander domein.


![Schermafbeelding van een preview-tenant](./media/active-directory-b2c-reference-tenant-type/preview-b2c-tenant.png)
