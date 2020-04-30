---
title: bestand opnemen
description: bestand opnemen
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 07/30/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 167d50e5c7f3049f46fd8540630e47044240809f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "81536575"
---
[Azure files](../articles/storage/files/storage-files-introduction.md) ondersteunt verificatie op basis van identiteiten via Server Message Block (SMB) via [on-premises Active Directory Domain Services (AD DS)](https://docs.microsoft.com/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview) (preview) en [Azure Active Directory Domain Services (Azure AD DS)](../articles/active-directory-domain-services/overview.md). In dit artikel wordt uitgelegd hoe Azure-bestands shares Domain Services, on-premises of in azure, kunnen gebruiken ter ondersteuning van op identiteit gebaseerde toegang tot Azure-bestands shares via SMB. Door toegang op basis van een identiteit in te scha kelen voor uw Azure-bestands shares kunt u bestaande bestands servers vervangen door Azure-bestands shares zonder uw bestaande adreslijst service te vervangen, waardoor naadloze gebruikers toegang tot shares wordt gehandhaafd. 

Azure Files dwingt autorisatie af voor gebruikers toegang tot zowel de share als de map-of bestands niveaus. Machtigings toewijzing op share niveau kan worden uitgevoerd op Azure Active Directory (Azure AD)-gebruikers of-groepen die worden beheerd via het [RBAC-model (op rollen gebaseerd toegangs beheer)](../articles/role-based-access-control/overview.md) . Met RBAC moeten de referenties die u gebruikt voor toegang tot het bestand beschikbaar zijn of worden gesynchroniseerd met Azure AD. U kunt ingebouwde RBAC-rollen zoals opslag bestands gegevens van SMB-share aan gebruikers of groepen in azure AD toewijzen om Lees toegang te verlenen aan een Azure-bestands share.

Op Directory-of bestands niveau ondersteunt Azure Files het behoud, overnemen en afdwingen van Windows- [dacl's](https://docs.microsoft.com/windows/win32/secauthz/access-control-lists) , net als bij Windows-bestands servers. U kunt ervoor kiezen om Windows-DACL'S te blijven gebruiken bij het kopiëren van gegevens over SMB tussen uw bestaande bestands share en uw Azure-bestands shares. Of u van plan bent autorisatie af te dwingen, u kunt Azure-bestands shares gebruiken om een back-up te maken van de Acl's samen met uw gegevens. 
