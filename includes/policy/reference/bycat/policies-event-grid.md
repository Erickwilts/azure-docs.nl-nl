---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 01/08/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: f03688012100c31865fb25087f7b4049c087b43e
ms.sourcegitcommit: 8dd8d2caeb38236f79fe5bfc6909cb1a8b609f4a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2021
ms.locfileid: "98047286"
---
|Naam<br /><sub>(Azure-portal)</sub> |Beschrijving |Gevolg(en) |Versie<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Voor Azure Event Grid-domeinen moet een privékoppeling worden gebruikt](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F9830b652-8523-49cc-b1b3-e17dce1127ca) |Met Azure Private Link kunt u uw virtuele netwerk met services in Azure verbinden zonder een openbaar IP-adres bij de bron of bestemming. Het persoonlijke koppelingsplatform zorgt voor de connectiviteit tussen de consument en de services via het Azure-backbonenetwerk. Als u privé-eindpunten aan uw Event Grid-domeinen toewijst in plaats van aan de volledige service, bent u ook beschermd tegen gegevenslekken. Meer informatie op: [https://aka.ms/privateendpoints](https://aka.ms/privateendpoints). |Controle, uitgeschakeld |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Event%20Grid/EventGridDomains_EnablePrivateEndpoint_Audit.json) |
|[Voor Azure Event Grid-onderwerpen moet een privékoppeling worden gebruikt](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F4b90e17e-8448-49db-875e-bd83fb6f804f) |Met Azure Private Link kunt u uw virtuele netwerk met services in Azure verbinden zonder een openbaar IP-adres bij de bron of bestemming. Het privékoppelingsplatform zorgt voor de connectiviteit tussen de consument en de services via het Azure-backbonenetwerk. Als u privé-eindpunten toewijst aan uw onderwerpen in plaats van aan de volledige service, bent u ook beschermd tegen gegevenslekken. Zie voor meer informatie: [https://aka.ms/privateendpoints](https://aka.ms/privateendpoints). |Controle, uitgeschakeld |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Event%20Grid/EventGridTopics_EnablePrivateEndpoint_Audit.json) |
