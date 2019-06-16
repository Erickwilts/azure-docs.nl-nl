---
title: Geef de contactgegevens voor beveiliging in Azure Security Center | Microsoft Docs
description: Dit document laat zien hoe u voor de contactgegevens voor beveiliging in Azure Security Center.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 26b5dcb4-ce3f-4f22-8d56-d2bf743cfc90
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 1/9/2018
ms.author: rkarlin
ms.openlocfilehash: b6babf7d5d5a0f5796efa9418044366c6a135ed9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60909281"
---
# <a name="provide-security-contact-details-in-azure-security-center"></a>Geef de contactgegevens voor beveiliging in Azure Security Center
Azure Security Center wordt aanbevolen dat u contactgegevens voor beveiliging voor uw Azure-abonnement, bieden als u dat nog niet gedaan hebt. Deze informatie wordt gebruikt door Microsoft om contact met u op te nemen als door Microsoft Security Response Center (MSRC) wordt gedetecteerd dat uw klantgegevens zijn geopend door een illegale of niet-geautoriseerde derde. MSRC voert selecteert u beveiligingsbewaking van de Azure-netwerk en de infrastructuur en threat intelligence en misbruik klachten ontvangt van een derde partij.

Er wordt een melding per e-mail verzonden de eerste keer dat een waarschuwing plaatsvindt en alleen voor waarschuwingen met een hoge urgentie. E-mailvoorkeuren kunnen alleen worden geconfigureerd voor abonnementsbeleid. Resourcegroepen binnen een abonnement nemen deze instellingen over. 

E-mailmeldingen voor waarschuwingen worden verzonden volgens de volgende richtlijnen:
- Alleen voor waarschuwingen met hoge urgentie
- Aan één e-mailontvanger per type waarschuwing, per dag  
- Niet meer dan 3 e-mailberichten worden verzonden naar een enkele ontvanger in één dag
- Elk e-mailbericht bevat een afzonderlijke waarschuwing, geen verzameling waarschuwingen
 
Als er bijvoorbeeld al een e-mailbericht is verstuurd om u te waarschuwen voor een RDP-aanval, krijgt u op dezelfde dag niet nog een e-mailbericht over een RDP-aanval, zelfs niet als hierdoor een nieuwe waarschuwing wordt geactiveerd. 
 

> [!NOTE]
> In dit document wordt de service geïntroduceerd aan de hand van een voorbeeldimplementatie.  Dit is geen stapsgewijze handleiding.
>
>

## <a name="implement-the-recommendation"></a>De aanbeveling voor het implementeren
1. Onder **aanbevelingen**, selecteer **contactgegevens voor beveiliging verstrekken**.
   ![Contactpersoon voor beveiliging verstrekken][1]
2. Selecteer het Azure-abonnement voor de contactgegevens op.
3. Hiermee opent u **e-mailmeldingen**.

   ![Contactgegevens voor beveiliging verstrekken][2]

   * De beveiliging e-mailadres of de adressen van elkaar gescheiden door komma's invoeren. Er is een limiet aan het aantal e-mailadressen die u kunt invoeren.
   * Voer een security internationaal telefoonnummer toe.
   * Schakel de optie voor het ontvangen van e-mailberichten over waarschuwingen met hoge urgentie, **Stuur mij e-mails over waarschuwingen**.
   * In de toekomst, hebt u de optie voor het e-mailmeldingen verzenden naar abonnementseigenaren. Deze optie is momenteel lichter gekleurd weergegeven.
   * Selecteer **opslaan** de contactgegevens voor beveiliging toepassen op uw abonnement.

## <a name="see-also"></a>Zie ook
Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Setting security policies in Azure Security Center](tutorial-security-policy.md) (Beveiligingsbeleid instellen in Azure Security Center): leer hoe u beveiligingsbeleid voor uw Azure-abonnementen en -resourcegroepen configureert.
* [Aanbevelingen voor beveiliging in Azure Security Center beheren](security-center-recommendations.md) --Leer hoe aanbevelingen helpen u uw Azure-resources te beveiligen.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md) --informatie over het bewaken van de status van uw Azure-resources.
* [Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) (Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center): ontdek hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Partneroplossingen bewaken met Azure Security Center](security-center-partner-solutions.md): leer hoe u de integriteitsstatus van uw partneroplossingen kunt bewaken.
* [Azure Security Center FAQ](security-center-faq.md) (Veelgestelde vragen over Azure Security Center): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-Beveiligingsblog](https://blogs.msdn.com/b/azuresecurity/) --de meest recente Azure-beveiliging nieuws en informatie.

<!--Image references-->
[1]: ./media/security-center-provide-security-contacts/provide-contacts.png
[2]:./media/security-center-provide-security-contacts/provide-contact-details.png
