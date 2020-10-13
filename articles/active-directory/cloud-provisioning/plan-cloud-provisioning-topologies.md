---
title: Ondersteuning voor topologieën en scenario's voor het inrichten van Clouds Azure AD Connect
description: Meer informatie over verschillende topologieën van on-premises en Azure Active Directory (Azure AD) die gebruikmaken van Azure AD Connect Cloud inrichting.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/26/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 172e836a212f9ce097c2c4e392a7386a510c2c6b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "91266289"
---
# <a name="azure-ad-connect-cloud-provisioning-supported-topologies-and-scenarios"></a>Ondersteuning voor topologieën en scenario's voor het inrichten van Clouds Azure AD Connect
In dit artikel worden verschillende topologieën voor on-premises en Azure Active Directory (Azure AD) beschreven die gebruikmaken van Azure AD Connect Cloud inrichting. Dit artikel bevat alleen ondersteunde configuraties en scenario's.

> [!IMPORTANT]
> Micro soft biedt geen ondersteuning voor het wijzigen of bewerken van Azure AD Connect Cloud inrichting buiten de configuraties of acties die formeel zijn gedocumenteerd. Een van deze configuraties of acties kan leiden tot een inconsistente of niet-ondersteunde status van Azure AD Connect Cloud inrichting. Daarom biedt Microsoft geen technische ondersteuning voor dergelijke implementaties.

## <a name="things-to-remember-about-all-scenarios-and-topologies"></a>Wat u moet weten over alle scenario's en topologieën
Hier volgt een lijst met informatie die u moet onthouden wanneer u een oplossing selecteert.

- Gebruikers en groepen moeten uniek worden geïdentificeerd in alle forests
- De overeenkomst tussen forests vindt niet plaats met Cloud inrichting
- Een gebruiker of groep mag slechts één keer worden weer gegeven in alle forests
- Het bron anker voor objecten wordt automatisch gekozen.  Er wordt gebruikgemaakt van MS-DS-ConsistencyGuid indien aanwezig, anders wordt ObjectGUID gebruikt.
- U kunt het kenmerk dat wordt gebruikt voor het bron anker niet wijzigen.

## <a name="single-forest-single-azure-ad-tenant"></a>Eén forest, één Azure AD-Tenant
![Diagram waarin de topologie voor één forest en één Tenant wordt weer gegeven.](media/plan-cloud-provisioning-topologies/single-forest.png)

De eenvoudigste topologie is één on-premises forest met een of meer domeinen en één Azure AD-Tenant.  Zie [zelf studie: Eén forest met één Azure AD-Tenant](tutorial-single-forest.md) voor een voor beeld van dit scenario.


## <a name="multi-forest-single-azure-ad-tenant"></a>Meerdere forests, één Azure AD-Tenant
![Topologie voor een multi-forest en één Tenant](media/plan-cloud-provisioning-topologies/multi-forest.png)

Een algemene topologie is een meerdere AD-forests met een of meer domeinen en één Azure AD-Tenant.  

## <a name="existing-forest-with-azure-ad-connect-new-forest-with-cloud-provisioning"></a>Bestaand forest met Azure AD Connect, nieuw forest met Cloud inrichting
![Diagram waarin de topologie voor een bestaand forest en een nieuw forest wordt weer gegeven.](media/plan-cloud-provisioning-topologies/existing-forest-new-forest.png)

Dit scenario is een topologie die vergelijkbaar is met het scenario met meerdere forests, maar dit is een bestaande Azure AD Connect omgeving en brengt vervolgens een nieuw forest met Azure AD Connect Cloud inrichting aan.  Zie [zelf studie: een bestaand forest met één Azure AD-Tenant](tutorial-existing-forest.md) voor een voor beeld van dit scenario.

## <a name="piloting-azure-ad-connect-cloud-provisioning-in-an-existing-hybrid-ad-forest"></a>Piloten Azure AD Connect Cloud inrichting in een bestaand hybride AD-forest
![Topologie voor één forest en één Tenant ](media/plan-cloud-provisioning-topologies/migrate.png) het proef scenario omvat het bestaan van zowel Azure AD Connect als Azure AD Connect Cloud inrichting in hetzelfde forest en het bereik van de gebruikers en groepen dienovereenkomstig. Opmerking: een object moet binnen het bereik van slechts een van de hulpprogram ma's zijn. 

Zie voor een voor beeld van dit scenario [zelf studie: Pilot Azure AD Connect Cloud Provisioning in een bestaand GESYNCHRONISEERD AD-forest](tutorial-pilot-aadc-aadccp.md)



## <a name="next-steps"></a>Volgende stappen 

- [Wat is inrichting?](what-is-provisioning.md)
- [Wat is Azure AD Connect-cloudinrichting?](what-is-cloud-provisioning.md)

