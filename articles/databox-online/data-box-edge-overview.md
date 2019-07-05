---
title: Overzicht van Microsoft Azure Data Box Edge | Microsoft Docs
description: Hierin wordt Azure Data Box Edge beschreven, een opslagoplossing die gebruikmaakt van een fysiek apparaat voor netwerkgebaseerde overdracht naar Azure.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: overview
ms.date: 06/28/2019
ms.author: alkohli
ms.openlocfilehash: 3972f9f93cc6323601102f1a54bb067a8995d9e4
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67484761"
---
# <a name="what-is-azure-data-box-edge"></a>Wat is Azure Data Box Edge? 

Azure Data Box Edge is een opslagoplossing waarmee u gegevens kunt verwerken en via een netwerk kunt verzenden naar Azure. In dit artikel staat een overzicht van de Data Box Edge-oplossing, voordelen, belangrijkste mogelijkheden en de scenario’s waarin u dit apparaat kunt implementeren. 

Data Box Edge maakt gebruik van een fysiek apparaat dat door Microsoft wordt geleverd, om de veilige gegevensoverdracht te versnellen. Het fysieke apparaat bevindt zich op uw locatie en u schrijft gegevens ernaar met behulp van het NFS- en SMB-protocol. 

Data Box Edge heeft alle gatewaymogelijkheden van Data Box Gateway. Daarnaast is Data Box voorzien van Edge-rekenmogelijkheden met AI die u helpen gegevens te analyseren, verwerken of filteren terwijl ze naar Azure-blok-blobopslag, Azure-pagina-blobopslag of Azure Files worden verzonden.  

## <a name="use-cases"></a>Gebruiksvoorbeelden

Azure Data Box Edge is een Edge-rekenapparaat met AI dat mogelijkheden voor netwerkgebaseerde gegevensoverdracht biedt. Hier volgen de verschillende scenario’s waarin Data Box Edge kan worden gebruikt voor gegevensoverdracht.

- **Gegevens voorverwerken**: Analyseer gegevens uit on-premises of IoT-apparaten om snel resultaten te verkrijgen terwijl u dicht bij de plek blijft waar gegevens worden gegenereerd. Data Box Edge draagt de volledige gegevensset over naar de cloud om deze geavanceerder te verwerken of grondiger te analyseren.  Voorverwerking kan worden gebruikt om: 

    - Gegevens samen te voegen.
    - Gegevens te wijzigen, bijvoorbeeld om persoonlijk identificeerbare informatie (PII) te verwijderen.
    - Een subset van de benodigde gegevens te maken en deze over te dragen voor grondigere analyse in de cloud.
    - IoT-gebeurtenissen te analyseren en erop te reageren. 

- **Azure Machine Learning afleiden**: Met Data Box Edge kunt u ML-modellen (Machine Learning) uitvoeren om snel resultaten te verkrijgen die kunnen worden gebruikt voordat de gegevens naar de cloud worden verzonden. De volledige gegevensset kan worden overgedragen om door te gaan voor het opnieuw trainen en het verbeteren van uw ML-modellen. Zie voor meer informatie over het gebruik van de Azure ML-hardware versneld modellen op het apparaat Databox Edge [implementeren Azure ML hardwareversnelling modellen op Databox Edge](https://docs.microsoft.com/azure/machine-learning/service/how-to-deploy-fpga-web-service#deploy-to-a-local-edge-server).

- **Gegevens via een netwerk naar Azure overdragen**: Gebruik Data Box Edge om gegevens gemakkelijk en snel naar Azure over te dragen voor verdere berekeningen en analyses of voor archivering. 

## <a name="benefits"></a>Voordelen

Data Box Edge biedt de volgende voordelen:

- **Gemakkelijke gegevensoverdracht**: Het verplaatsen van gegevens in en uit Azure-opslag is net zo gemakkelijk als het werken met een lokale netwerkshare.  
- **Hoge prestaties**: Hoogpresterende overdrachten van en naar Azure. 
- **Snelle toegang**: De recentste bestanden worden in de cache opgeslagen voor snelle toegang tot on-premises bestanden.  
- **Beperkt bandbreedtegebruik**: Gegevens kunnen zelfs naar Azure worden geschreven wanneer het netwerk wordt beperkt voor minder gebruik tijdens piekuren.  
- **Gegevenstransformatie**: Gegevens kunnen worden geanalyseerd, verwerkt of gefilterd terwijl ze naar Azure worden verplaatst.

## <a name="key-capabilities"></a>Belangrijkste mogelijkheden

Data Box Edge biedt de volgende mogelijkheden:

|Mogelijkheid |Description  |
|---------|---------|
|Hoge prestaties     | Volledig geautomatiseerde en zeer geoptimaliseerde gegevensoverdracht en bandbreedte.|
|Ondersteunde protocollen     | Ondersteuning voor het standaard SMB- en NFS-protocol voor gegevensopname. <br> Ga naar [Systeemvereisten voor Data Box Edge](data-box-edge-system-requirements.md) voor meer informatie over ondersteunde versies.|
|Berekenen       |Gegevens kunnen worden geanalyseerd, verwerkt of gefilterd.|
|Toegang tot gegevens     | Rechtstreekse gegevenstoegang vanuit Azure Storage Blobs en Azure Files met behulp van cloud-API’s voor aanvullende gegevensverwerking in de cloud.|
|Snelle toegang     | Lokale cache op het apparaat voor snelle toegang tot laatst gebruikte bestanden.|
|Offline upload     | Modus zonder verbinding ondersteunt scenario’s voor offline uploaden.|
|Gegevensvernieuwing     | Mogelijkheid om lokale bestanden te vernieuwen met de meest recente uit de cloud.|
|Versleuteling    | BitLocker-ondersteuning om gegevens lokaal te versleutelen en de gegevensoverdracht naar de cloud via *HTTPS* te beveiligen.       |
|Flexibiliteit     | Ingebouwde netwerkflexibiliteit.        |


## <a name="components"></a>Onderdelen

De Data Box Edge-oplossing bestaat uit een Data Box Edge-resource, een fysiek Data Box Edge-apparaat en een lokale webinterface.

* **Fysiek Data Box Edge-apparaat**: Een 1U rackserver die door Microsoft wordt geleverd en die kan worden geconfigureerd om gegevens naar Azure te verzenden. 
    
* **Data Box Edge-resource**: Een resource in de Azure-portal waarmee u een Data Box Edge-apparaat kunt beheren via een webinterface waartoe u toegang hebt vanaf verschillende geografische locaties. Gebruik de Data Box Edge-resource om resources te maken en beheren, apparaten en waarschuwingen te bekijken en beheren, en shares te beheren.  

    <!--![The Data Box Edge service in Azure portal](media/data-box-overview/data-box-Edge-service1.png)-->

    Ga voor meer informatie naar [een order maken voor uw gegevens in het Edge-apparaat](data-box-edge-deploy-prep.md#create-a-new-resource).

* **Lokale Data Box-webinterface**: Gebruik de lokale webinterface om diagnoses uit te voeren, het Data Box Edge-apparaat uit te schakelen of opnieuw op te starten, logboeken met kopieerbewerkingen te bekijken en contact op te nemen met Microsoft Ondersteuning om een serviceaanvraag in te dienen.

    <!--![The Data Box Edge local web UI](media/data-box-Edge-overview/data-box-Edge-local-web-ui.png)-->

    Ga naar [De webgebaseerde gebruikersinterface gebruiken om uw Data Box te beheren](data-box-edge-manage-access-power-connectivity-mode.md) voor informatie over het gebruik van de webgebaseerde gebruikersinterface.


## <a name="region-availability"></a>Beschikbaarheid in regio’s

Het fysieke Data Box Edge-apparaat, de Azure-resource en het doelopslagaccount waarnaar u gegevens overdraagt hoeven zich niet allemaal in dezelfde regio te bevinden.

- **Beschikbaarheid van resources**: Voor deze release is de Data Box Edge-resource beschikbaar in de volgende regio’s:
    - **Verenigde Staten** -VS-Oost
    - **Europese Unie** - West-Europa
    - **Azië en Stille Oceaan** - Zuidoost-Azië
    
    Data Box Edge kunnen ook worden geïmplementeerd in de Azure Government-Cloud. Zie voor meer informatie, [wat is Azure Government?](https://docs.microsoft.com/azure/azure-government/documentation-government-welcome).
    
- **Doelopslagaccounts**: De opslagaccounts waarin de gegevens worden opgeslagen, zijn beschikbaar in alle Azure-regio’s. 

    De regio’s waar de opslagaccounts Data Box-gegevens opslaan, moeten zich voor optimale prestaties dicht bij het apparaat bevinden. Een opslagaccount dat zich ver van het apparaat vandaan bevindt, resulteert in lange latenties en tragere prestaties. 


## <a name="next-steps"></a>Volgende stappen

- De [Systeemvereisten voor Data Box Edge](data-box-edge-system-requirements.md) lezen.
- De [limieten voor Data Box Edge](data-box-edge-limits.md) begrijpen.
- [Azure Data Box Edge](data-box-edge-deploy-prep.md) in de Azure-portal implementeren.




