---
title: Beheren en aanvragen resourcequota
titleSuffix: Azure Machine Learning service
description: Deze gebruiksaanwijzing wordt uitgelegd van de verschillende quota voor resources voor Azure Machine Learning en het weergeven en meer quotum aanvragen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: nishankgu
ms.author: nigup
ms.date: 05/10/2019
ms.custom: seodec18
ms.openlocfilehash: f9734a5d8f34536558fbf0c861889f3c7d6719da
ms.sourcegitcommit: bb85a238f7dbe1ef2b1acf1b6d368d2abdc89f10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2019
ms.locfileid: "65523990"
---
# <a name="manage-and-request-quotas-for-azure-resources"></a>Beheren en quota's voor Azure-resources aanvragen

Als met andere Azure-services, er gelden beperkingen voor bepaalde resources die zijn gekoppeld aan de Azure Machine Learning-service. Deze limieten variëren van een bovengrens voor het aantal werkruimten kunt u limieten op de werkelijke onderliggende rekenresources die wordt gebruikt voor trainingen of inferentietaken uw modellen. In dit artikel geeft meer informatie over de vooraf geconfigureerde limieten voor verschillende Azure-resources voor uw abonnement en bevat ook handige koppelingen om aanvraag quotum verbeteringen voor elk type resource. Deze limieten zijn geplaatst om te voorkomen dat te veel uitgevoerd vanwege fraude budget en capaciteitsbeperkingen voor Azure in acht neemt.

Houd er rekening mee als u deze quota ontwerp en de schaal van uw resources van de service Azure Machine Learning voor productieworkloads. Bijvoorbeeld, als uw cluster niet wordt bereikt het doelaantal knooppunten die u hebt opgegeven, klikt u vervolgens u mogelijk hebt een Azure Machine Learning-Computing kernen limiet bereikt voor uw abonnement. Als u de is besteed of wanneer het quotum boven de limiet standaard verhogen wilt, opent u een online ondersteuningsverzoek gratis. De limieten kunnen niet worden verhoogd boven de maximumlimiet-waarde die wordt weergegeven in de volgende tabellen vanwege beperkingen van Azure-capaciteit. Als er geen kolom maximumlimiet is, klikt u vervolgens de resource geen instelbare limieten.

## <a name="special-considerations"></a>Speciale overwegingen

+ Een quotum is een tegoed is besteed, niet een capaciteit gegarandeerd. Als u grootschalige capaciteit nodig hebt, neem dan contact op met ondersteuning van Azure.

+ Uw quotum wordt gedeeld tussen alle services in uw abonnementen met inbegrip van Azure Machine Learning-service. De enige uitzondering hierop is de Azure Machine Learning-Computing waarvoor een aparte quotum van de compute-quotum voor kernen. Zorg ervoor dat het quotagebruik berekenen van alle services bij het evalueren van uw behoeften aan capaciteit.

+ Standaardlimieten variëren per Type aanbieding, zoals gratis proefversie, betalen per gebruik en -serie, zoals Dv2, F, G, enzovoort.

## <a name="default-resource-quotas"></a>Standaard resourcequota

Hier volgt een overzicht van de quotumlimieten door verschillende resourcetypen in uw Azure-abonnement.

> [!Important]
> Limieten kunnen worden gewijzigd. De meest recente kan altijd worden gevonden op het serviceniveau-quotum [document](https://docs.microsoft.com/azure/azure-subscription-service-limits/) voor alles van Azure.

### <a name="virtual-machines"></a>Virtuele machines
Er geldt een limiet voor het aantal virtuele machines die u in uw services of in een zelfstandige wijze op een Azure-abonnement inrichten kunt. Deze limiet is op het regioniveau van de op het totaal aantal kerngeheugens, evenals op basis van de per-familie.

Het is belangrijk om te benadrukken dat cores van virtuele machine beschikt over regionale en regionale beperkingen per grootte reeks (Dv2, F, enz.) die afzonderlijk worden afgedwongen. Neem bijvoorbeeld een abonnement met een limiet van 30 VM-cores voor US - oost, een limiet van 30 cores voor de A-serie en een limiet van 30 cores voor de D-serie. Met dit abonnement zouden 30 A1-VM’s of 30 D1-VM’s kunnen worden geïmplementeerd, of een combinatie van deze twee typen die het totaal van 30 cores niet overschrijdt (bijvoorbeeld 10 A1-VM’s en 20 D1-VM’s).

[!INCLUDE [azure-subscription-limits-azure-resource-manager](../../../includes/azure-subscription-limits-azure-resource-manager.md)]

Voor een meer gedetailleerde en actuele lijst van de quotalimieten, controleert u het artikel brede, door het Azure-quotum [hier](https://docs.microsoft.com/azure/azure-subscription-service-limits).

### <a name="azure-machine-learning-compute"></a>Azure Machine Learning-Computing
Voor Azure Machine Learning-Computing is er een standaard-quotumlimiet voor zowel de aantal kernen en het aantal unieke rekenresources toegestaan per regio in een abonnement. Dit quotum is gescheiden van de bovenstaande kerngeheugenquotum VM en de limieten core niet op dit moment worden gedeeld tussen de twee resourcetypen.

Beschikbare resources:
+ Toegewezen kerngeheugens per regio hebben een limiet van de standaardwaarde van 24 uur per dag en 300 afhankelijk van uw type abonnementsaanbod.  Het aantal toegewezen kerngeheugens per abonnement kan worden verhoogd. Neem contact op met ondersteuning van Azure wilt discussiëren over meer opties.

+ Kernen per regio met lage prioriteit hebben een limiet van de standaardwaarde van 24 uur per dag en 300, afhankelijk van uw type abonnementsaanbod.  Het aantal kernen per abonnement met lage prioriteit kan worden verhoogd. Neem contact op met ondersteuning van Azure wilt discussiëren over meer opties.

+ Clusters per regio hebben een standaardlimiet van 100 en een limiet van 200. Neem contact op met ondersteuning van Azure als u wilt een verhoging boven deze limiet.

+ Er zijn **andere strikte limieten** die kan niet worden overschreden eenmaal is bereikt.

| **Resource** | **Maximumlimiet** |
| --- | --- |
| Maximum aantal werkruimten per resourcegroep | 800 |
| Maximum aantal knooppunten in een enkele bron voor Azure Machine Learning-Computing (AmlCompute) | 100 knooppunten |
| Maximumaantal GPU MPI verwerkt per knooppunt | 1-4 |
| Maximumaantal GPU werknemers per knooppunt | 1-4 |
| Maximale levensduur | 90 dagen<sup>1</sup> |
| Maximale levensduur van een knooppunt van lage prioriteit | 1 dag<sup>2</sup> |
| Parameter Maximum servers per knooppunt | 1 |

<sup>1</sup> de maximale levensduur verwijst naar de tijd die een uitvoering is gestart en wanneer deze is voltooid. Voltooide uitvoeringen behouden voor onbepaalde tijd; gegevens voor uitvoeringen is niet voltooid binnen de maximale levensduur zijn niet toegankelijk.
<sup>2</sup> taken op een knooppunt met lage prioriteit voorrang telkens wanneer er een beperking capaciteit is kunnen worden genomen. Het verdient aanbeveling voor het implementeren van het plaatsen van controlepunten in uw taak.

### <a name="container-instances"></a>Containerinstanties

Er is ook een limiet voor het aantal containerinstanties die u instellen kunt in een bepaalde periode (binnen het bereik van per uur) of in op uw volledige abonnement.

[!INCLUDE [container-instances-limits](../../../includes/container-instances-limits.md)]

Voor een meer gedetailleerde en actuele lijst van de quotalimieten, controleert u het artikel brede, door het Azure-quotum [hier](https://docs.microsoft.com/azure/azure-subscription-service-limits#container-instances-limits).

### <a name="storage"></a>Storage
Er geldt een limiet voor het aantal storage-accounts per regio ook in een bepaald abonnement. De standaardlimiet is 200 en omvat zowel Standard en Premium Storage-accounts. Als u meer dan 200 opslagaccounts in een bepaalde regio nodig hebt, moet u een aanvraag via [ondersteuning voor Azure](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest/). Het Azure Storage-team controleert uw bedrijfsscenario en kan goedkeuren tot 250 opslagaccounts voor een bepaalde regio.


## <a name="find-your-quotas"></a>Uw quota's zoeken

Weergeven van uw quotum voor verschillende resources, zoals virtuele Machines, opslag, netwerk, is het eenvoudig via de Azure-portal.

1. Selecteer in het linkerdeelvenster **alle services** en selecteer vervolgens **abonnementen** onder de categorie Algemeen.

1. Selecteer het abonnement waarvan quota die u zoekt in de lijst met abonnementen.

   **Er is een voorbehoud**, specifiek voor het weergeven van het quotum voor Compute van Azure Machine Learning. Zoals eerder vermeld, staat quotum los van het quotum voor compute voor uw abonnement.

1. Selecteer in het linkerdeelvenster **Machine Learning-service** en selecteer vervolgens elke werkruimte in de onderstaande lijst

1. Op de volgende blade onder de **ondersteuning en probleemoplossing sectie** Selecteer **gebruik + quota** om uw huidige quotalimieten en het gebruik weer te geven.

1. Selecteer een abonnement om de quotumlimiet weer te geven. Vergeet niet om te filteren naar de regio waarin u geïnteresseerd bent.


## <a name="request-quota-increases"></a>Quotaverhogingen aanvraag

Als u wilt verhogen de is besteed of wanneer het quotum boven de standaardlimiet [opent u een ondersteuningsaanvraag online klant](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest/) gratis.

De limieten kunnen niet worden verhoogd boven de maximumlimiet-waarde die wordt weergegeven in de tabellen. Als er geen maximale limiet, hebben niet de bron instelbare limieten. [Dit](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quota-errors) artikel bevat informatie over het proces voor het vergroten van quota in meer detail.

Bij het aanvragen van een quotaverhoging, moet u de service die u aanvraagt te verhogen van het quotum tegen te selecteren die services zoals Machine Learning-servicequotum, Container instances of quotum voor opslag kan worden. Daarnaast voor Azure Machine Learning-Computing, klikt u op de **quotum aanvragen** knop tijdens het bekijken van het quotum voor de bovenstaande stappen te volgen.

> [!NOTE]
> [Gratis proefabonnementen](https://azure.microsoft.com/offers/ms-azr-0044p) komen niet in aanmerking voor is besteed of wanneer het quotum toeneemt. Als u hebt een [gratis proefabonnement](https://azure.microsoft.com/offers/ms-azr-0044p), kunt u upgraden naar een [betalen per gebruik](https://azure.microsoft.com/offers/ms-azr-0003p/) abonnement. Zie voor meer informatie, [Azure-proefabonnement upgraden naar betalen per gebruik](../../billing/billing-upgrade-azure-subscription.md) en [gratis proefabonnement Veelgestelde vragen over](https://azure.microsoft.com/free/free-account-faq).
