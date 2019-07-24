---
title: Ondersteunde Kubernetes-versies in de Azure Kubernetes-service
description: Meer informatie over het ondersteunings beleid voor Kubernetes-versies en de levens cyclus van clusters in azure Kubernetes service (AKS)
services: container-service
author: sauryadas
ms.service: container-service
ms.topic: article
ms.date: 05/20/2019
ms.author: saudas
ms.openlocfilehash: a4726444e2103228cd26be3d3a6d516b31e315ee
ms.sourcegitcommit: 770b060438122f090ab90d81e3ff2f023455213b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2019
ms.locfileid: "68304316"
---
# <a name="supported-kubernetes-versions-in-azure-kubernetes-service-aks"></a>Ondersteunde Kubernetes-versies in azure Kubernetes service (AKS)

Ongeveer elke drie maanden wordt een secundaire versie uitgebracht door de Kubernetes-community. Deze releases bevatten nieuwe functies en verbeteringen. Patchreleases worden vaker uitgebracht (soms wekelijks) en zijn alleen bedoeld voor essentiële bugfixes in een secundaire versie. Deze patches bevatten oplossingen voor beveiligings problemen of grote fouten die van invloed zijn op een groot aantal klanten en producten die worden uitgevoerd op productie op basis van Kubernetes.

AKS streeft ernaar nieuwe Kubernetes-versies te certificeren en vrij te geven binnen 30 dagen na een upstream-release, afhankelijk van de stabiliteit van de release.

## <a name="kubernetes-versions"></a>Kubernetes-versies

Kubernetes maakt gebruik van het standaard versie beheer schema van [semantische versie](https://semver.org/) . Dit betekent dat elke versie van Kubernetes dit Nummerings schema volgt:

```
[major].[minor].[patch]

Example:
  1.12.14
  1.12.15
  1.13.7
```

Elk nummer in de versie geeft algemene compatibiliteit met de vorige versie aan:

* Primaire versies worden gewijzigd wanneer incompatibele API-wijzigingen of achterwaartse compatibiliteit mogelijk wordt verbroken.
* Secundaire versies worden gewijzigd wanneer er wijzigingen in de functionaliteit worden aangebracht die achterwaarts compatibel zijn met de andere secundaire releases.
* Patch versies worden gewijzigd wanneer er achterwaarts compatibele fout oplossingen worden uitgevoerd.

Over het algemeen moeten gebruikers de nieuwste patch release van de secundaire versie uitvoeren, bijvoorbeeld als uw productie cluster op *1.13.6* is en *1.13.7* de meest recente beschik bare patch versie beschikbaar is voor de *1,13* -serie. u moet een upgrade uitvoeren naar *1.13.7* zodra u er zeker van kunt zijn dat uw cluster volledig wordt gerepareerd en ondersteund.

## <a name="kubernetes-version-support-policy"></a>Beleid voor ondersteuning van Kubernetes-versies

AKS ondersteunt vier secundaire versies van Kubernetes:

* De huidige secundaire versie die wordt uitgebracht in AKS (N)
* Die eerdere secundaire versies. Elke ondersteunde secundaire versie biedt ook ondersteuning voor twee stabiele patches.

Dit staat bekend als ' N-3 '-(N (meest recente versie)-3 (secundaire versies)).

Als AKS bijvoorbeeld in de inleiding *1.13. x* vandaag introduceert, wordt ondersteuning geboden voor de volgende versies:

Nieuwe secundaire versie    |    Lijst met ondersteunde versies
-----------------    |    ----------------------
1.13. x               |    1.12. a, 1.12. b, 1.11. a, 1.11. b, 1,10. a, 1,10. b

Waarbij "x" en ". a" en ". b" representatieve patch versies zijn.

Zie ' communicatie ' hieronder voor meer informatie over de communicatie met versie wijzigingen en verwachtingen.

Wanneer een nieuwe secundaire versie wordt geïntroduceerd, worden de oudste secundaire versie-en patch releases die worden ondersteund, afgeschaft en verwijderd. Als de huidige lijst met ondersteunde versies bijvoorbeeld:

```
1.12.a
1.12.b
1.11.a
1.11.b
1.10.a
1.10.b
1.9.a
1.9.b
```

En AKS Releases 1.13. x. Dit betekent dat de versie van 1,9. x (alle versies van 1,9) wordt verwijderd en niet meer wordt ondersteund.

> [!NOTE]
> Houd er rekening mee dat als klanten een niet-ondersteunde Kubernetes-versie uitvoeren, wordt gevraagd om een upgrade uit te voeren bij het aanvragen van ondersteuning voor het cluster. Clusters met niet-ondersteunde Kubernetes-releases vallen niet onder het [beleid voor AKS-ondersteuning](https://docs.microsoft.com/azure/aks/support-policies).


Naast het bovenstaande in secundaire versies ondersteunt AKS ook de twee nieuwste patches van een bepaalde secundaire versie. Bijvoorbeeld, met de volgende ondersteunde versies:

```
Current Supported Version List
------------------------------
1.12.1, 1.12.2, 1.11.4, 1.11.5
```

Als de Kubernetes van 1.12.3 en 1.11.6 en AKS van de upstream-versie van deze patch versies worden vrijgegeven, worden de oudste patch versies afgeschaft en verwijderd, en wordt de lijst met ondersteunde versies:

```
New Supported Version List
----------------------
1.12.*2*, 1.12.*3*, 1.11.*5*, 1.11.*6*
```

> [!NOTE]
> Klanten mogen het maken van clusters, CI of andere geautomatiseerde taken niet aan specifieke patch releases vastmaken. 

### <a name="communications"></a>Communicatie

* Voor nieuwe **secundaire** versies van Kubernetes
  * Alle gebruikers worden op de hoogte gesteld van de nieuwe versie en de versie die wordt verwijderd.
  * Wanneer een nieuwe patch versie wordt uitgebracht, wordt de oudste patch release op hetzelfde moment verwijderd.
  * Klanten hebben **60 dagen** vanaf de datum van de open bare melding om een upgrade naar een ondersteunde versie van de kleine release te kunnen uitvoeren.
* Voor nieuwe **patch** versies van Kubernetes
  * Alle gebruikers ontvangen een melding dat de nieuwe patch versie wordt vrijgegeven en een upgrade naar de nieuwste patch release.
  * Gebruikers hebben **30 dagen** een upgrade naar een nieuwere, ondersteunde patch release. Gebruikers hebben **30 dagen** voor een upgrade naar een ondersteunde patch release voordat de oudste wordt verwijderd.

AKS definieert "vrijgegeven" als algemene Beschik baarheid, ingeschakeld in alle SLO-en Quality of Service metingen en beschikbaar in alle regio's.

> [!NOTE]
> Klanten worden op de hoogte gesteld van Kubernetes-versie releases en afschaffing, wanneer een secundaire versie wordt afgeschaft/verwijderd, krijgen 60 dagen de tijd om een upgrade naar een ondersteunde release uit te kunnen geven. In het geval van patch releases krijgen klanten 30 dagen de tijd om bij te werken naar een ondersteunde release.

Meldingen worden verzonden via:

* [Release opmerkingen bij AKS](https://aka.ms/aks/releasenotes)
* Azure Portal meldingen
* [Azure update-kanaal][azure-update-channel]

### <a name="policy-exceptions"></a>Uitzonde ringen van beleid

AKS behoudt zich het recht voor om nieuwe/bestaande versies toe te voegen of te verwijderen die zijn geïdentificeerd om een of meer kritieke productie problemen op te lossen, zonder voorafgaande kennisgeving.

Specifieke patch releases kunnen worden overgeslagen of de implementatie wordt versneld, afhankelijk van de ernst van de fout of het beveiligings probleem.

### <a name="azure-portal-and-cli-default-versions"></a>Standaard versies van Azure Portal en CLI

Wanneer u een AKS-cluster implementeert in de portal of met de Azure CLI, wordt het cluster altijd ingesteld op de N-1 secundaire versie en de meest recente patch. Als AKS bijvoorbeeld *1.13. x*, *1.12. a* + *1.12. b*, *1.11. a* + *1.11. b*, *1,10. a* + *1,10 b*ondersteunt, is de standaard versie voor nieuwe clusters *1.12. b* .

AKS standaard ingesteld op N-1 (minor. latestPatch, bijvoorbeeld 1.12. b) om klanten standaard een bekende, stabiele en patched versie te bieden.

## <a name="list-currently-supported-versions"></a>Momenteel ondersteunde versies weer geven

Gebruik de opdracht [AZ AKS Get-verse][az-aks-get-versions] voor meer informatie over welke versies momenteel beschikbaar zijn voor uw abonnement en regio. In het volgende voor beeld ziet u een lijst met  de beschik bare Kubernetes-versies voor de regio oostus:

```azurecli-interactive
az aks get-versions --location eastus --output table
```

De uitvoer is vergelijkbaar met het volgende voor beeld, waarin wordt aangegeven dat Kubernetes-versie *1.13.5* de meest recente versie is die beschikbaar is:

```
KubernetesVersion    Upgrades
-------------------  ------------------------
1.13.5               None available
1.12.7               1.13.5
1.12.6               1.12.7, 1.13.5
1.11.9               1.12.6, 1.12.7
1.11.8               1.11.9, 1.12.6, 1.12.7
1.10.13              1.11.8, 1.11.9
1.10.12              1.10.13, 1.11.8, 1.11.9
```

## <a name="faq"></a>Veelgestelde vragen

**Wat gebeurt er wanneer een klant een Kubernetes-cluster upgradet met een secundaire versie die niet wordt ondersteund?**

Als u een *n-4-* versie hebt, bent u buiten ondersteuning en wordt u gevraagd om te upgraden. Als uw upgrade van versie n-4 naar n-3 slaagt, bent u nu in ons ondersteunings beleid. Bijvoorbeeld:

- Als de ondersteunde AKS-versies *1.13. x*, *1.12. a* + *1.12. b*, *1.11. c* + *1.11 d*en *1,10. e* zijn + *1,10 f* en u bent op *1,9. g* of *1,9. h*, u valt buiten de ondersteuning.
- Als de upgrade van *1,9. g* of *1,9. h* tot *1,10. e* of *1,10. f* slaagt, u bent weer terug in het kader van ons ondersteunings beleid.

Upgrades naar oudere versies dan *n-4* worden niet ondersteund. In dergelijke gevallen raden klanten aan nieuwe AKS-clusters te maken en hun werk belastingen opnieuw te implementeren.

**Wat betekent ' buiten het ondersteunings team '**

' Buiten het ondersteunings team ' betekent dat de versie die u uitvoert, zich buiten de lijst met ondersteunde versies bevindt en u wordt gevraagd het cluster bij te werken naar een ondersteunde versie bij het aanvragen van ondersteuning. Daarnaast maakt AKS geen runtime of andere garanties voor clusters buiten de lijst met ondersteunde versies.

**Wat gebeurt er wanneer een klant een Kubernetes-cluster schaalt met een secundaire versie die niet wordt ondersteund?**

Voor secundaire versies die niet worden ondersteund door AKS, blijft het schalen in of uit zonder problemen te voor komen.

**Kan een klant permanent blijven beschikken over een Kubernetes-versie?**

Ja. Als het cluster echter niet is opgenomen in een van de versies die worden ondersteund door AKS, heeft het cluster geen ondersteuning meer voor het AKS-ondersteunings beleid. Uw cluster wordt niet automatisch bijgewerkt of verwijderd door Azure.

**Welke versie ondersteunt de Master als het agent cluster zich niet in een van de ondersteunde AKS-versies bevindt?**

De Master wordt automatisch bijgewerkt naar de meest recente ondersteunde versie.

## <a name="next-steps"></a>Volgende stappen

Zie [een Azure Kubernetes service-cluster (AKS) bijwerken][aks-upgrade]voor meer informatie over het upgraden van uw cluster.

<!-- LINKS - External -->
[aks-engine]: https://github.com/Azure/aks-engine
[azure-update-channel]: https://azure.microsoft.com/updates/?product=kubernetes-service

<!-- LINKS - Internal -->
[aks-upgrade]: upgrade-cluster.md
[az-aks-get-versions]: /cli/azure/aks#az-aks-get-versions
