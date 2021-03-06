---
title: Azure Policy beveiligings basislijn voor Azure Security Bench Mark
description: De Azure Policy Security Baseline voorziet in procedure richtlijnen en resources voor het implementeren van de beveiligings aanbevelingen die zijn opgegeven in de Azure Security-Bench Mark.
author: msmbaldwin
ms.service: azure-policy
ms.topic: conceptual
ms.date: 07/02/2020
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: e8915e1c15972341befd176b412925f4e87c94f6
ms.sourcegitcommit: 2bd0a039be8126c969a795cea3b60ce8e4ce64fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/14/2021
ms.locfileid: "98201447"
---
# <a name="azure-policy-security-baseline-for-azure-security-benchmark"></a>Azure Policy beveiligings basislijn voor Azure Security Bench Mark

Deze beveiligings basislijn is van toepassing op de richt lijnen van [Azure Security](../../../security/benchmarks/overview.md) voor de Azure Policy. De Azure Security-benchmark biedt aanbevelingen voor hoe u uw cloudoplossingen in Azure kunt beveiligen. De inhoud wordt gegroepeerd op de **compatibiliteits domeinen** en **beveiligings controles** die zijn gedefinieerd door de Azure Security-benchmark en de bijbehorende richt lijnen die van toepassing zijn op Azure Policy. **Besturings elementen** die niet van toepassing zijn op Azure Policy, zijn uitgesloten. Als u wilt zien hoe Azure Policy volledig is toegewezen aan de beveiligings benchmark van Azure, raadpleegt u het [volledige Azure Policy beveiligings basislijn toewijzings bestand](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines).

Zie [regelgevings naleving: Azure Security Bench Mark](../samples/azure-security-benchmark.md)(Engelstalig) voor een overzicht van de beveiligings maatregelen van Azure Security op ingebouwde beleids definities via het ingebouwde initiatief.

Azure Policy gebruikt het _eigendom_ van de term in plaats van _verantwoordelijkheid_. Zie [Azure Policy-beleids definities](./definition-structure.md#type) en [gedeelde verantwoordelijkheid in de Cloud](../../../security/fundamentals/shared-responsibility.md)voor meer informatie over het _eigendom_.


## <a name="logging-and-monitoring"></a>Logboekregistratie en bewaking

*Zie voor meer informatie [beveiligings beheer: logboek registratie en controle](../../../security/benchmarks/security-control-logging-monitoring.md).*

### <a name="23-enable-audit-logging-for-azure-resources"></a>2,3: controle logboek registratie inschakelen voor Azure-resources

**Richt lijnen**: Azure Policy gebruikt activiteiten logboeken, die automatisch worden ingeschakeld, om gebeurtenis bron, datum, gebruiker, tijds tempel, bron adressen, doel adressen en andere nuttige elementen op te neemt.

* [Platform logboeken en metrische gegevens verzamelen met Azure Monitor](../../../azure-monitor/platform/diagnostic-settings.md)

* [Logboek registratie en verschillende logboek typen in azure](../../../azure-monitor/platform/platform-logs-overview.md)


**Monitoring door Azure Security Center**: Momenteel niet beschikbaar

**Verantwoordelijkheid**: Klant

## <a name="identity-and-access-control"></a>Identiteits- en toegangsbeheer

*Zie [beveiligings beheer: identiteits-en toegangs beheer](../../../security/benchmarks/security-control-identity-access-control.md)voor meer informatie.*

### <a name="33-use-dedicated-administrative-accounts"></a>3,3: speciale beheerders accounts gebruiken

**Richt lijnen**: Maak standaard procedures voor het gebruik van specifieke beheerders accounts. Gebruik Azure Security Center identiteits-en toegangs beheer om het aantal beheerders accounts te bewaken. 

U kunt ook een just-in-time-of just-out-Access-oplossing inschakelen met behulp van [Azure AD privileged Identity Management](../../../active-directory/privileged-identity-management/pim-configure.md) geprivilegieerde rollen of [Azure Resource Manager](../../../azure-resource-manager/management/overview.md).


**Monitoring door Azure Security Center**: Momenteel niet beschikbaar

**Verantwoordelijkheid**: Klant

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3,6: gebruik speciale machines (privileged Access workstations) voor alle beheer taken

**Richt lijnen**: gebruik paw's (privileged Access workstations) met MFA dat is geconfigureerd om Azure-resources aan te melden en te configureren.

* [Meer informatie over privileged Access workstations](https://4sysops.com/archives/understand-the-microsoft-privileged-access-workstation-paw-security-model/)

* [MFA inschakelen in Azure](../../../active-directory/authentication/howto-mfa-getstarted.md)


**Azure Security Center-bewaking**: Niet van toepassing

**Verantwoordelijkheid**: Klant

## <a name="data-protection"></a>Gegevensbeveiliging

*Zie [beveiligings beheer: gegevens beveiliging](../../../security/benchmarks/security-control-data-protection.md)voor meer informatie.*

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4,6: Azure RBAC gebruiken om de toegang tot resources te beheren

**Richt lijnen**: gebruik Azure op rollen gebaseerd toegangs beheer (Azure RBAC) voor het beheren van de toegang tot Azure Policy.

* [Azure RBAC-machtigingen in Azure Policy](../overview.md#azure-rbac-permissions-in-azure-policy)

* [Azure RBAC configureren](../../../role-based-access-control/role-assignments-portal.md)


**Monitoring door Azure Security Center**: Momenteel niet beschikbaar

**Verantwoordelijkheid**: Klant

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4,9: wijzigingen in essentiële Azure-resources vastleggen en waarschuwen

**Hulp**: gebruik Azure monitor met activiteiten Logboeken om waarschuwingen te maken wanneer wijzigingen in azure Policy plaatsvinden.

* [Waarschuwingen maken voor gebeurtenissen in het Azure-activiteiten logboek](../../../azure-monitor/platform/alerts-activity-log.md)


**Monitoring door Azure Security Center**: Momenteel niet beschikbaar

**Verantwoordelijkheid**: Klant

## <a name="inventory-and-asset-management"></a>Inventarisatie en asset-management

*Zie voor meer informatie [beveiligings beheer: inventarisatie en activa beheer](../../../security/benchmarks/security-control-inventory-asset-management.md).*

### <a name="62-maintain-asset-metadata"></a>6,2: meta gegevens van activa onderhouden

**Richt lijnen**: Tags Toep assen op Azure-resources die meta gegevens geven om ze logisch in een taxonomie te organiseren. Gebruik het Azure Policy _wijzigen_ van het effect om naleving en consistentie op te geven en af te dwingen.

* [Zelf studie: beleid maken en beheren](../tutorials/create-and-manage.md)

* [Zelfstudie: Tag governance beheren](../tutorials/govern-tags.md)


**Monitoring door Azure Security Center**: Momenteel niet beschikbaar

**Verantwoordelijkheid**: Klant

### <a name="64-define-and-maintain-an-inventory-of-approved-azure-resources"></a>6,4: een inventaris van goedgekeurde Azure-resources definiëren en onderhouden

**Richt lijnen**: een inventaris van goedgekeurde beleids definities en beleids toewijzingen maken conform uw organisatie behoeften.

**Azure Security Center-bewaking**: Niet van toepassing

**Verantwoordelijkheid**: Klant

### <a name="65-monitor-for-unapproved-azure-resources"></a>6,5: monitor voor niet-goedgekeurde Azure-resources

**Hulp**: gebruik Azure Policy om beperkingen toe te voegen voor het type resources dat kan worden gemaakt in uw abonnementen.

* [Azure Policy configureren en beheren](../tutorials/create-and-manage.md)


**Azure Security Center-bewaking**: Niet van toepassing

**Verantwoordelijkheid**: Klant

## <a name="next-steps"></a>Volgende stappen

- Zie de [Azure Security-Bench Mark](../../../security/benchmarks/overview.md)
- Meer informatie over [Azure-beveiligingsbasislijnen](../../../security/benchmarks/security-baselines-overview.md)
