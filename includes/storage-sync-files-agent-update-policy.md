---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 12/11/2018
ms.author: tamram
ms.openlocfilehash: 9b8812b1fca6a72a69f06a6c0278da8ee4d4c852
ms.sourcegitcommit: 441e59b8657a1eb1538c848b9b78c2e9e1b6cfd5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67841507"
---
De Azure File Sync-agent wordt bijgewerkt regelmatig nieuwe functionaliteit toe te voegen en om problemen te verhelpen. Het is raadzaam om dat u bij het configureren van Microsoft Update om updates voor de Azure File Sync-agent als ze beschikbaar.

#### <a name="major-vs-minor-agent-versions"></a>Primaire en secundaire agent-versies
* Primaire agent-versies zijn vaak bevat nieuwe functies en steeds meer hebben als het eerste deel van het versienummer. Bijvoorbeeld: \*2.\*.\*\*
* Secundaire agent-versies worden ook 'patches' genoemd en vaker dan primaire versies worden uitgebracht. Ze bevatten vaak bugfixes en verbeteringen in kleinere, maar er zijn geen nieuwe functies. Bijvoorbeeld: \* \*.3.\*\*

#### <a name="upgrade-paths"></a>Upgradepaden
Er zijn vier goedgekeurd en getest manieren om de Azure File Sync-agent-updates te installeren. 
1. **(Aanbevolen) Microsoft Update voor het automatisch downloaden en installeren van agentupdates configureren.**  
    We raden altijd aan te nemen van elke Azure File Sync-update om te controleren of dat u toegang hebt tot de meest recente oplossingen voor de server-agent. Microsoft Update is dit proces naadloze door automatisch downloaden en installeren van updates voor u.
2. **Gebruik AfsUpdater.exe naar agentupdates downloaden en installeren.**  
    De AfsUpdater.exe bevindt zich in de map voor agentinstallatie. Dubbelklik op het uitvoerbare bestand te downloaden en installeren van agentupdates. 
3. **Patch uitvoeren voor een bestaande Azure File Sync-agent met behulp van een bestand van de patch voor Microsoft Update of een uitvoerbaar bestand .msp. De meest recente updatepakket van Azure File Sync kan worden gedownload vanaf de [Microsoft Update-catalogus](https://www.catalog.update.microsoft.com/Search.aspx?q=Azure%20File%20Sync).**  
    Uitvoeren van een uitvoerbaar bestand .msp, werkt de Azure File Sync-installatie met dezelfde methode die automatisch door Microsoft Update wordt gebruikt in de vorige upgrade-pad. Uitvoering van een patch voor Microsoft Update, wordt een in-place upgrade van een Azure File Sync-installatie uitvoeren.
4. **Download het nieuwste installatieprogramma van Azure File Sync-agent van de [Microsoft Download Center](https://go.microsoft.com/fwlink/?linkid=858257).**  
    Als u een bestaande installatie van de Azure File Sync-agent bijwerken, verwijdert u de oude versie en installeert u de meest recente versie van het gedownloade installatieprogramma. De serverregistratie, synchronisatiegroepen en andere instellingen worden beheerd door het installatieprogramma van Azure File Sync.

#### <a name="automatic-agent-lifecycle-management"></a>Levenscyclusbeheer voor automatisch agentbeheer
Met de agent-versie 6, heeft het bestand sync-team een functie van de automatische upgrade agent geïntroduceerd. U kunt een van twee modi selecteren en opgeven van een onderhoudsvenster waarin de upgrade wordt uitgevoerd op de server. Deze functie is ontworpen voor hulp bij het levenscyclusbeheer van de agent door het aanbieden van een guardrail zo wordt voorkomen dat de agent van verlopen of waardoor het een zonder gedoe, blijf op de huidige instelling.
1. De **standaardinstelling** wordt geprobeerd om te voorkomen dat de agent van verlopen. Binnen 21 dagen van de geplaatste vervaldatum van een agent probeert de agent zelf bijwerken. Een poging om te upgraden van één keer per week in 21 dagen vóór de vervaldatum en in het geselecteerde onderhoudsvenster wordt gestart. **Deze optie heeft geen elimineert de noodzaak voor het nemen van reguliere Microsoft Update-patches.**
2. (Optioneel) u kunt selecteren dat de agent automatisch zelf bijgewerkt, als een nieuwe agentversie beschikbaar (op dit moment niet van toepassing op geclusterde servers). Deze update zal optreden tijdens het geselecteerde onderhoudsvenster en toestaan dat de server om te profiteren van nieuwe functies en verbeteringen zodra ze algemeen beschikbaar. Dit is de aanbevolen, zorgeloos instelling dat belangrijke agent-versies, evenals reguliere update patches voor uw server. Elke agent die zijn uitgebracht is op de kwaliteit van de algemene beschikbaarheid. Zelfs als u het automatisch bijwerken wanneer een nieuwe versie beschikbaar is, kan niet worden aangeboden de update onmiddellijk na de release. Nieuwe agents in eerste instantie worden aangeboden aan een klein aantal servers en vervolgens we het aanbod geleidelijk uitbreiden. Zodra flighting voltooid is, de agent ook beschikbaar op Microsoft Update en [Microsoft Download Center](https://go.microsoft.com/fwlink/?linkid=858257).

#### <a name="agent-lifecycle-and-change-management-guarantees"></a>Agent garanties voor levenscyclus en wijzigen
Azure File Sync is een cloudservice, die voortdurend nieuwe functies en verbeteringen worden. Dit betekent dat een specifieke versie van de Azure File Sync-agent kan alleen worden ondersteund voor een beperkte periode. Het kader van uw implementatie, garanderen de volgende regels dat u voldoende tijd en de melding voor agent-updates of upgrades worden uitgevoerd in een proces voor het beheren van uw wijziging:

- Primaire agent-versies worden ondersteund voor ten minste zes maanden vanaf de datum van de eerste release.
- We garanderen dat er is een overlapping van ten minste drie maanden, tussen de ondersteuning van grote agent-versies. 
- Waarschuwingen worden gegeven voor de geregistreerde servers met behulp van een-op-binnenkort verlopen agent ten minste drie maanden vóór de vervaldatum. U kunt controleren of een geregistreerde server een oudere versie van de agent wordt gebruikt in het gedeelte van de geregistreerde servers van een Opslagsynchronisatieservice.
- De levensduur van een secundaire agentversie is gebonden aan de bijbehorende primaire versie. Bijvoorbeeld, wanneer de agent-versie 3.0 is vrijgegeven, agentversies 2. \* alle worden ingesteld op verlopen samen.

> [!Note]
> Een versie van de agent installeren met een vervaldatum-waarschuwing wordt een waarschuwing weergegeven maar slagen. Probeert te installeren of te verbinden met een verlopen agentversie wordt niet ondersteund en worden geblokkeerd.
