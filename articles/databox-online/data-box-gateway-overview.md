---
title: Overzicht van Microsoft Azure Data Box Gateway | Microsoft Docs
description: Beschrijft Azure Data Box Gateway, een opslagoplossing voor virtuele apparaten waarmee u gegevens naar Azure kunt overdragen
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: overview
ms.date: 08/21/2019
ms.author: alkohli
ms.openlocfilehash: b8c6b4085f56bc12b67bf87177ba33b82c6a6db9
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2020
ms.locfileid: "69900593"
---
# <a name="what-is-azure-data-box-gateway"></a>Wat is Azure Data Box Gateway?

Azure Data Box Gateway is een opslagoplossing waarmee u naadloos gegevens naar Azure kunt verzenden. Dit artikel biedt een overzicht van de Azure Data Box Gateway-oplossing, voordelen, belangrijkste mogelijkheden en de scenario’s waarin u dit apparaat kunt implementeren.

Data Box Gateway is een virtueel apparaat op basis van een virtuele machine die in uw gevirtualiseerde omgeving of hypervisor is ingericht. Het virtuele apparaat bevindt zich op uw locatie en u schrijft gegevens ernaar met behulp van het NFS- en SMB-protocol. Het apparaat draagt uw gegevens vervolgens over naar Azure blok-blob, pagina-blob of Azure Files.

## <a name="use-cases"></a>Gebruiksvoorbeelden

Data Box Gateway kan worden gebruikt voor het overdragen van gegevens naar de cloud, zoals voor archivering in de cloud, herstel na noodgevallen of als uw gegevens in de cloud moeten worden verwerkt. Hier volgen de verschillende scenario’s waarin Data Box Gateway kan worden gebruikt voor gegevensoverdracht.

- **Archivering in de cloud** - Kopieer honderden TB’s aan gegevens veilig en efficiënt naar Azure-opslag met behulp van Data Box Gateway. De gegevens kunnen in één keer of voortdurend worden opgenomen voor archiveringsdoeleinden.

- **Continue gegevensopname** - Continu gegevens innemen in het apparaat om naar de cloud te kopiëren, ongeacht de gegevensgrootte. Als de gegevens naar het gateway-apparaat worden geschreven, worden de gegevens door het apparaat geüpload naar Azure Storage.  

- **Initiële bulkoverdracht gevolgd door incrementele overdracht** - Gebruik Gegevensvak voor de bulkoverdracht in een offlinemodus (eerste seed) en Data Box Gateway voor incrementele overdrachten (doorlopende feed) via het netwerk.

Ga voor meer informatie naar [use cases van Azure Data Box Gateway.](data-box-gateway-use-cases.md)

## <a name="benefits"></a>Voordelen

Data Box Gateway biedt de volgende voordelen:

- **Gemakkelijke gegevensoverdracht** - Het verplaatsen van gegevens in en uit Azure-opslag is net zo gemakkelijk als het werken met een lokale netwerkshare.  
- **Hoge prestaties** - Maakt overdracht van netwerkgegevens minder complex met snelle gegevensoverdrachten naar en vanuit Azure.
- **Snelle toegang en gegevensopname met hoge snelheid tijdens kantooruren** - Data Box-Gateway heeft een lokale cache die u als de grootte van de lokale capaciteit definieert wanneer het virtuele apparaat is ingericht. De grootte van de gegevensschijf moet worden opgegeven volgens de [minimale vereisten voor virtuele apparaten](data-box-gateway-system-requirements.md#specifications-for-the-virtual-device). De lokale cache biedt de volgende voordelen:
    - De lokale cache biedt de mogelijkheid tot gegevensopname met een hoge snelheid. Wanneer een grote hoeveelheid gegevens wordt opgenomen tijdens piekuren, kan de cache de gegevens bevatten en naar de cloud uploaden.
    - De lokale cache biedt snelle leestoegang tot een bepaalde drempelwaarde. Totdat het apparaat voor 50 tot 60 % vol is, zijn alle leesbewerkingen van het apparaat toegankelijk vanuit de cache, zodat deze sneller kunnen plaatsvinden. Zodra de gebruikte ruimte op het apparaat boven deze drempel gaat, begint het apparaat lokale bestanden te verwijderen.
 
- **Beperkt bandbreedtegebruik**: Gegevens kunnen zelfs naar Azure worden geschreven wanneer het netwerk wordt beperkt voor minder gebruik tijdens piekuren.  

## <a name="key-capabilities"></a>Belangrijkste mogelijkheden

Data Box Gateway biedt de volgende mogelijkheden:

|Mogelijkheid |Beschrijving  |
|---------|---------|
|Snelheid     | Volledig geautomatiseerde en zeer geoptimaliseerde gegevensoverdracht en bandbreedte.|
|Ondersteunde protocollen     | Ondersteuning voor het standaard SMB- en NFS-protocol voor gegevensopname. <br> Ga naar [Systeemvereisten voor Data Box Gateway](data-box-gateway-system-requirements.md) voor meer informatie over ondersteunde versies.|
|Toegang tot gegevens     | Zodra de gegevens die per apparaat worden verzonden zich in de cloud bevinden, kan deze verder worden gewijzigd door rechtstreeks toegang te krijgen tot de cloud-API's.|
|Snelle toegang     | Lokale cache op het apparaat voor snelle toegang tot laatst gebruikte bestanden.|
|Offline upload     | Modus zonder verbinding ondersteunt scenario’s voor offline uploaden.|
|Gegevensvernieuwing     | Mogelijkheid om lokale bestanden te vernieuwen met de meest recente uit de cloud.|
|Flexibiliteit     | Ingebouwde netwerkflexibiliteit        |


## <a name="specifications"></a>Specificaties

Het virtuele Data Box Gateway-apparaat heeft de volgende specificaties:

| Specificaties                                          | Beschrijving              |
|---------------------------------------------------------|--------------------------|
| Virtuele processors (kernen)   | Minimaal 4 |
| Geheugen  |Minimaal 8 GB|
| Beschikbaarheid|Eén knooppunt|
| Disks|Besturingssysteemschijf: 250 GB <br> Gegevensschijf: minimaal 2 TB, thin-provisioned en moet worden ondersteund door SSD's|
| Netwerkinterfaces |1 of meer virtuele netwerkinterfaces|
| Systeemeigen protocollen voor het delen van bestanden|SMB en NFS  |
| Beveiliging|Verificatie voor het ontgrendelen van toegang tot apparaten en gegevens <br> Gegevens worden tijdens overdracht versleuteld met AES-256-bits-versleuteling|
| Beheer|Lokale webgebruikersinterface - Eerste installatie, diagnostische gegevens en energiebeheer van apparaat <br> Azure Portal - Dagelijks beheer van Data Box Gateway-apparaten       |

## <a name="components"></a>Onderdelen

De Data Box Gateway-oplossing bestaat uit een Data Box Gateway-resource, een virtueel Data Box Gateway-apparaat en een lokale webinterface.

- **Virtueel Data Box Gateway-apparaat** - Een apparaat op basis van een virtuele machine die in uw gevirtualiseerde omgeving of hypervisor is ingericht en waarmee u gegevens naar Azure kunt verzenden.
    
- **Data Box Gateway-resource** - Een resource in de Azure Portal waarmee u een Data Box Gateway-apparaat kunt beheren via een webinterface waartoe u toegang hebt vanaf verschillende geografische locaties. Gebruik de Data Box Gateway-bron om apparaat, shares, gebruikers en waarschuwingen weer te geven en te beheren. Zie voor meer informatie hoe [u Azure-portal beheren.](data-box-gateway-manage-shares.md)

- **Lokale webgebruikersinterface** van Data Box - Gebruik de lokale webgebruikersinterface om diagnoses uit te voeren, het apparaat af te sluiten en opnieuw op te starten, een ondersteuningspakket te genereren of contact op te nemen met Microsoft Support om een serviceaanvraag in te dienen. Zie voor meer informatie hoe u [kunt beheren met behulp van lokale webgebruikersinterface](data-box-gateway-manage-access-power-connectivity-mode.md).

## <a name="region-availability"></a>Beschikbaarheid in regio’s

Data Box Gateway fysiek apparaat, Azure resource, en target storage account waarop u gegevens overte dragen hoeft niet allemaal in dezelfde regio.

- **Beschikbaarheid van resources** - Ga voor een lijst met alle regio's waar de Data Box Edge-bron beschikbaar is, naar [Azure-producten die beschikbaar](https://azure.microsoft.com/global-infrastructure/services/?regions=all&products=databox)zijn per regio. Data Box Gateway kan ook worden geïmplementeerd in de Azure Government Cloud. Zie [Wat is Azure-overheid voor](https://docs.microsoft.com/azure/azure-government/documentation-government-welcome)meer informatie?

- **Doelopslagaccounts**: De opslagaccounts waarin de gegevens worden opgeslagen, zijn beschikbaar in alle Azure-regio’s.

    De regio’s waar de opslagaccounts Data Box-gegevens opslaan, moeten zich voor optimale prestaties dicht bij het apparaat bevinden. Een opslagaccount dat zich ver van het apparaat vandaan bevindt, resulteert in lange latenties en tragere prestaties.


## <a name="next-steps"></a>Volgende stappen

- Lees de [Systeemvereisten voor Data Box Gateway](data-box-gateway-system-requirements.md).
- Begrijp de [limieten voor Data Box Gateway](data-box-gateway-limits.md).
- Implementeer [Azure Data Box Gateway](data-box-gateway-deploy-prep.md) in de Azure Portal.

