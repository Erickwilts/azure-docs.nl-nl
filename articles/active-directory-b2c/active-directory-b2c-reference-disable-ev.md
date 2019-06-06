---
title: E-mailverificatie tijdens de registratie in Azure Active Directory B2C consument uitschakelen | Microsoft Docs
description: Een onderwerp laat zien hoe u e-mailverificatie tijdens de registratie in Azure Active Directory B2C consument uitschakelen.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 8b50b59b6a1f99787b842923cd6ec77ee6500f0a
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66509533"
---
# <a name="disable-email-verification-during-consumer-sign-up-in-azure-active-directory-b2c"></a>Tijdens de registratie in Azure Active Directory B2C consument e-mailverificatie uitschakelen 
Wanneer dit is ingeschakeld, biedt Azure Active Directory (Azure AD) B2C een gebruiker de mogelijkheid om u te registreren voor toepassingen door een e-mailadres en het maken van een lokaal account. Azure AD B2C zorgt ervoor dat geldige e-mailadressen doordat gebruikers om te controleren of ze tijdens het registratieproces. Dit voorkomt ook dat een kwaadwillende geautomatiseerd proces valse accounts voor de toepassingen die worden gegenereerd.

Sommige toepassingsontwikkelaars liever het overslaan van e-mailverificatie tijdens het registratieproces en hebt in plaats daarvan het e-mailadres later controleren of consumers. Ter ondersteuning hiervan, kan Azure AD B2C worden geconfigureerd voor het e-mailverificatie uitschakelen. In dat geval maakt u een soepeler aanmeldingsproces en biedt ontwikkelaars de flexibiliteit om te onderscheiden van de consumenten die hun e-mailadres van deze consumenten die nog niet hebt geverifieerd.

Meld u aan gebruikersstromen hebben standaard e-mailverificatie is ingeschakeld. Gebruik de volgende stappen uit te schakelen:

1. Klik op **gebruikersstromen**.
2. Klik op de gebruikersstroom (bijvoorbeeld ' B2C_1_SiUp') om deze te openen. 
3. Klik op **pagina-indelingen**.
4. Klik op **pagina voor het registreren van lokale account**.
5. Klik op **e-mailadres** in de **naam** kolom onder de **gebruikerskenmerken** sectie.
6. Onder **vereist verificatie**, selecteer **Nee**.
7. Klik op **Opslaan** boven aan de blade. U bent klaar.

> [!NOTE]
> Uitschakelen van e-mailverificatie in het registratieproces kan leiden tot ongewenste e-mail. Als u de standaardwaarde uitschakelt, kunt u het beste uw eigen verificatiesysteem toevoegen.
> 
>
