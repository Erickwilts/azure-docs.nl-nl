---
title: Azure resource Graph-beveiligings basislijn voor Azure Security Bench Mark
description: De beveiligings basislijn van Azure resource Graph biedt procedure richtlijnen en resources voor het implementeren van de beveiligings aanbevelingen die zijn opgegeven in de Azure Security-Bench Mark.
author: msmbaldwin
ms.service: resource-graph
ms.topic: conceptual
ms.date: 07/07/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 3a8b271a030de2ddd3aa40b5bb0af871319c0e1f
ms.sourcegitcommit: 1e6c13dc1917f85983772812a3c62c265150d1e7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2020
ms.locfileid: "86171802"
---
# <a name="azure-resource-graph-security-baseline-for-azure-security-benchmark"></a>Azure resource Graph-beveiligings basislijn voor Azure Security Bench Mark

Deze beveiligings basislijn is van toepassing op de richt lijnen [Azure Security Bench Mark](../../../security/benchmarks/overview.md) to Azure resource Graph. De Security Bench Mark van Azure biedt aanbevelingen over hoe u uw cloud oplossingen op Azure kunt beveiligen. De inhoud wordt gegroepeerd op basis van de **beveiligings controles** die zijn gedefinieerd door de Azure Security Bench Mark en de bijbehorende richt lijnen die van toepassing zijn op Azure resource Graph. **Besturings elementen** die niet van toepassing zijn op een Azure-resource grafiek, zijn uitgesloten. Zie het [volledige azure Virtual Network Security Baseline-toewijzings bestand](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines)voor meer informatie over hoe Azure resource Graph volledig is toegewezen aan de beveiligings benchmark van Azure.



## <a name="identity-and-access-control"></a>Identiteits- en toegangsbeheer

*Zie [beveiligings beheer: identiteits-en toegangs beheer](../../../security/benchmarks/security-control-identity-access-control.md)voor meer informatie.*

### <a name="310-regularly-review-and-reconcile-user-access"></a>3,10: regel matig gebruikers toegang controleren en afstemmen

**Hulp**: Azure resource Graph biedt toegang tot bron typen en eigenschappen op basis van op rollen gebaseerde toegangs beheer (RBAC). Controleer en controleer of de toegang die is verleend aan beveiligings-principals (gebruikers, groepen en service accounts) regel matig wordt gecontroleerd om ervoor te zorgen dat query's resultaten voor de juiste resources retour neren.

* [Machtigingen in Azure Resource Graph](../overview.md#permissions-in-azure-resource-graph)

* [Beoordelingen over Azure Identity Access gebruiken](../../../active-directory/governance/access-reviews-overview.md)


**Azure Security Center bewaking**: Ja

**Verantwoordelijkheid**: klant

## <a name="data-protection"></a>Gegevensbeveiliging

*Zie [beveiligings beheer: gegevens beveiliging](../../../security/benchmarks/security-control-data-protection.md)voor meer informatie.*

### <a name="46-use-role-based-access-control-to-control-access-to-resources"></a>4,6: op rollen gebaseerd toegangs beheer gebruiken voor het beheren van de toegang tot bronnen

**Hulp**: gebruik op rollen gebaseerd toegangs beheer (RBAC) om de toegang tot gegevens en resources te beheren. Als u Azure resource Graph wilt gebruiken, moet u ook de juiste toegang hebben tot de resources die u wilt doorzoeken. Deze toegang moet worden beperkt tot alleen-lezen en alleen worden verleend aan het vereiste personeel.

* [Machtigingen in Azure Resource Graph](../overview.md#permissions-in-azure-resource-graph)

* [RBAC configureren in azure](../../../role-based-access-control/role-assignments-rest.md)


**Azure Security Center bewaking**: momenteel niet beschikbaar

**Verantwoordelijkheid**: klant

## <a name="inventory-and-asset-management"></a>Inventarisatie en asset-management

*Zie voor meer informatie [beveiligings beheer: inventarisatie en activa beheer](../../../security/benchmarks/security-control-inventory-asset-management.md).*

### <a name="61-use-automated-asset-discovery-solution"></a>6,1: automatische Asset-detectie oplossing gebruiken

**Hulp**: Azure resource Graph gebruiken om alle ondersteunde resources binnen uw abonnementen, beheer groepen en tenants op te vragen en te detecteren. Zorg ervoor dat u de juiste machtigingen hebt in uw Tenant en dat u alle Azure-abonnementen kunt inventariseren, evenals de resources in uw abonnementen.

* [Query's maken met Azure resource Graph](../first-query-portal.md)

* [Meer informatie over Azure RBAC](../../../role-based-access-control/overview.md)


**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

### <a name="64-define-and-maintain-inventory-of-approved-azure-resources"></a>6,4: de inventaris van goedgekeurde Azure-resources definiëren en onderhouden

**Richt lijnen**: Maak een inventaris van goedgekeurde Azure-resources en goedgekeurde software voor reken resources conform uw organisatie behoeften. Gebruik Azure resource Graph om te zoeken naar goedgekeurde Azure-resources en wijzigings geschiedenis (preview) om moment opnamen te controleren en te zien wat er is gewijzigd.

* [Query uitvoeren op Azure-resources met Azure resource Graph](../first-query-portal.md)

* [Resourcewijzigingen ophalen](../how-to/get-resource-changes.md)


**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

### <a name="65-monitor-for-unapproved-azure-resources"></a>6,5: monitor voor niet-goedgekeurde Azure-resources

**Hulp**: Azure resource Graph gebruiken om resources in uw abonnementen, beheer groepen en tenants te doorzoeken en te detecteren. Zorg ervoor dat alle Azure-resources in de omgeving zijn goedgekeurd.

* [Query uitvoeren op Azure-resources met Azure resource Graph](../first-query-portal.md)

* [Voor beelden: start query's voor Azure resource Graph](../samples/starter.md)


**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

## <a name="next-steps"></a>Volgende stappen

- Zie de [Azure Security-Bench Mark](../../../security/benchmarks/overview.md)
- Meer informatie over [Azure-beveiligings basislijnen](../../../security/benchmarks/security-baselines-overview.md)
