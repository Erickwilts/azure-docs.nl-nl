---
title: Advanced Threat Protection voor Azure Storage
description: Configureer Azure Storage Advanced Threat Protection om afwijkingen in de account activiteit te detecteren en u te informeren over mogelijk schadelijke pogingen om toegang te krijgen tot uw account.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 04/03/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.openlocfilehash: 7d4f36be51591d6be2b4c42eb8a8950ab52a0258
ms.sourcegitcommit: f7998db5e6ba35cbf2a133174027dc8ccf8ce957
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/05/2019
ms.locfileid: "68782570"
---
# <a name="advanced-threat-protection-for-azure-storage"></a>Advanced Threat Protection voor Azure Storage

Advanced Threat Protection voor Azure Storage biedt een extra laag met beveiligingsinformatie voor de detectie van ongebruikelijke en mogelijk schadelijke pogingen om toegang te verkrijgen tot of aanvallen op opslagaccounts. Deze beveiligingslaag biedt u de mogelijkheid bedreigingen te verhelpen zonder dat u een beveiligings expert of beveiligings bewakings systemen hoeft te beheren. 

Beveiligings waarschuwingen worden geactiveerd wanneer afwijkingen in de activiteit optreden.  Deze beveiligings waarschuwingen zijn geïntegreerd met [Azure Security Center](https://azure.microsoft.com/services/security-center/)en worden ook via e-mail verzonden naar abonnements beheerders, met details over verdachte activiteiten en aanbevelingen voor het onderzoeken en oplossen van bedreigingen.

> [!NOTE]
> * Advanced Threat Protection voor Azure Storage is momenteel alleen beschikbaar voor de Blob-opslag.
> * Voor prijs informatie, inclusief een gratis proef versie van 30 dagen, raadpleegt u de [pagina met Azure Security Center prijzen]( https://azure.microsoft.com/pricing/details/security-center/).
> * ATP voor Azure Storage-functie is momenteel niet beschikbaar in azure Government en soevereine Cloud regio's.

Geavanceerde bedreigings beveiliging voor Azure Storage opname van Diagnostische logboeken met lees-, schrijf-en verwijder aanvragen naar Blob-opslag voor detectie van bedreigingen. Als u de waarschuwingen van Advanced Threat Protection wilt onderzoeken, kunt u gerelateerde opslag activiteiten bekijken met Opslaganalyse logboek registratie. Zie [Opslaganalyse logboek registratie configureren](storage-monitor-storage-account.md#configure-logging)voor meer informatie.

## <a name="set-up-advanced-threat-protection"></a>Geavanceerde bedreigings beveiliging instellen 

### <a name="using-the-portal"></a>De portal gebruiken

1. Start de Azure Portal op [https://portal.azure.com](https://portal.azure.com/).

2. Ga naar de configuratie pagina van het Azure Storage account dat u wilt beveiligen. Selecteer op de pagina **instellingen** de optie **geavanceerde beveiliging tegen bedreigingen**.

3. In de Blade **geavanceerde beveiliging tegen bedreigingen** configureren
    * Advanced *Threat Protection* inschakelen
    * Klik op **Opslaan** om het nieuwe of bijgewerkte geavanceerde beveiligings beleid voor bedreigingen op te slaan. (Prijzen in de afbeelding zijn alleen bedoeld als voor beeld.)

![Azure Storage Advanced Threat Protection inschakelen](./media/storage-advanced-threat-protection/storage-advanced-threat-protection-turn-on.png)

### <a name="using-azure-security-center"></a>Met behulp van Azure Security Center

Wanneer u zich abonneert op de Standard-laag in Azure Security Center, wordt geavanceerde beveiliging tegen bedreigingen automatisch ingesteld op al uw opslag accounts. U kunt de geavanceerde beveiliging tegen bedreigingen voor uw opslag accounts onder een specifiek abonnement als volgt in-of uitschakelen:

1. Start **Azure Security Center** in het [Azure Portal](https://portal.azure.com).
1. Klik in het hoofd menu op **prijzen & instellingen**.
1. Klik op het abonnement dat u wilt in-of uitschakelen bedreigings beveiliging voor de bijbehorende opslag accounts.

    ![Abonnement selecteren](./media/storage-advanced-threat-protection/storage-advanced-threat-protection-subscription.png)

1. Klik op **prijs categorie**.
1. Klik in de sectie **prijs categorie op resource type selecteren** in de rij **opslag accounts** op **ingeschakeld** of **uitgeschakeld**.

    ![ATP in Security Center inschakelen](./media/storage-advanced-threat-protection/storage-advanced-threat-protection-pricing2.png)
1. Klik op **Opslaan**.

### <a name="using-azure-resource-manager-templates"></a>Azure Resource Manager sjablonen gebruiken

Gebruik een Azure Resource Manager sjabloon om een Azure Storage account te implementeren waarop Advanced Threat Protection is ingeschakeld. Zie [opslag account met geavanceerde beveiliging tegen bedreigingen](https://azure.microsoft.com/resources/templates/201-storage-advanced-threat-protection-create/)voor meer informatie.

### <a name="using-azure-policy"></a>Azure Policy gebruiken

Gebruik een Azure Policy om geavanceerde bedreigings beveiliging in te scha kelen voor opslag accounts onder een specifiek abonnement of resource groep.

1. Start de pagina Azure **Policy-Definitions** .

1. Zoek naar het beleid **geavanceerde beveiliging tegen bedreigingen implementeren op opslag accounts** .

     ![Zoek beleid](./media/storage-advanced-threat-protection/storage-atp-policy-definitions.png)
  
1. Selecteer een Azure-abonnement of resource groep.

    ![Abonnement of groep selecteren](./media/storage-advanced-threat-protection/storage-atp-policy2.png)

1. Wijs het beleid toe.

    ![Pagina beleids definities](./media/storage-advanced-threat-protection/storage-atp-policy1.png)

### <a name="using-rest-api"></a>REST API gebruiken
Gebruik rest API-opdrachten om de geavanceerde instelling voor beveiliging tegen bedreigingen te maken, bij te werken of op te halen voor een specifiek opslag account.

* [Advanced Threat Protection-maken](https://docs.microsoft.com/rest/api/securitycenter/advancedthreatprotection/create)
* [Advanced Threat Protection-ophalen](https://docs.microsoft.com/rest/api/securitycenter/advancedthreatprotection/get)

### <a name="using-azure-powershell"></a>Azure PowerShell gebruiken

Gebruik de volgende Power shell-cmdlets:

  * [Advanced Threat Protection inschakelen](https://docs.microsoft.com/powershell/module/az.security/enable-azsecurityadvancedthreatprotection)
  * [Geavanceerde beveiliging tegen bedreigingen verkrijgen](https://docs.microsoft.com/powershell/module/az.security/get-azsecurityadvancedthreatprotection)
  * [Geavanceerde bedreigings beveiliging uitschakelen](https://docs.microsoft.com/powershell/module/az.security/disable-azsecurityadvancedthreatprotection)

## <a name="explore-security-anomalies"></a>Beveiligings afwijkingen verkennen

Wanneer er afwijkingen optreden bij de opslag activiteit, ontvangt u een e-mail melding met informatie over de verdachte beveiligings gebeurtenis. Details van de gebeurtenis zijn onder andere:

* De aard van de anomalie
* De naam van het opslag account
* De tijd van de gebeurtenis
* Het opslag type
* De mogelijke oorzaken 
* De onderzoek stappen
* De herstels tappen


Het e-mail bericht bevat ook informatie over mogelijke oorzaken en aanbevolen acties voor het onderzoeken en oplossen van mogelijke bedreigingen.

![E-mail bericht over de geavanceerde beveiliging tegen bedreigingen Azure Storage](./media/storage-advanced-threat-protection/storage-advanced-threat-protection-alert-email.png)

U kunt uw huidige beveiligings waarschuwingen controleren en beheren via de [tegel beveiligings waarschuwingen](../../security-center/security-center-managing-and-responding-alerts.md#managing-security-alerts)van Azure Security Center. Als u op een specifieke waarschuwing klikt, vindt u details en acties voor het onderzoeken van de huidige dreiging en het adresseren van toekomstige bedreigingen.

![E-mail bericht over de geavanceerde beveiliging tegen bedreigingen Azure Storage](./media/storage-advanced-threat-protection/storage-advanced-threat-protection-alert.png)

## <a name="protection-alerts"></a>Beveiligings waarschuwingen

Waarschuwingen worden gegenereerd door ongebruikelijke en mogelijk schadelijke pogingen om opslag accounts te openen of misbruik te maken. Zie [Azure Storage](../../security-center/security-center-alerts-data-services.md#azure-storage) -waarschuwingen voor een lijst met deze waarschuwingen

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [Logboeken in azure Storage-accounts](/rest/api/storageservices/About-Storage-Analytics-Logging)

* Meer informatie over [Azure Security Center](../../security-center/security-center-intro.md)
