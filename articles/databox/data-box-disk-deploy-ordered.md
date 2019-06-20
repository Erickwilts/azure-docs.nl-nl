---
title: Zelfstudie om Azure Data Box-schijf | Microsoft Docs
description: Gebruik deze zelfstudie om te leren hoe u zich registreert en een Azure Data Box Disk bestelt waarmee u gegeven in Azure importeert.
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: tutorial
ms.date: 06/17/2019
ms.author: alkohli
Customer intent: As an IT admin, I need to be able to order Data Box Disk to upload on-premises data from my server onto Azure.
ms.openlocfilehash: 5f8fd9b2ecb58b34476bf8ecca7aa08dfc446040
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67273390"
---
# <a name="tutorial-order-an-azure-data-box-disk"></a>Zelfstudie: Een Azure Data Box Disk bestellen

Azure Data Box Disk is een hybride cloudoplossing waarmee u uw on-premises gegevens snel, eenvoudig en betrouwbaar in Azure importeert. U zet uw gegevens op SSD-schijven (solid-state drives) die Microsoft naar u heeft verstuurd en u stuurt de schijven terug. Deze gegevens worden vervolgens geüpload in Azure.

In deze zelfstudie wordt beschreven hoe u een Azure Data Box Disk bestelt. In deze zelfstudie komen deze onderwerpen aan bod:

> [!div class="checklist"]
> * Een Data Box Disk bestellen
> * De bestelling volgen
> * De bestelling annuleren

## <a name="prerequisites"></a>Vereisten

Voltooi voordat u met de implementatie begint de volgende configuratievereisten voor Data Box service en Data Box Disk.

### <a name="for-service"></a>Voor de service

Zorg voordat u begint voor het volgende:
- U hebt een Microsoft Azure Storage-account met toegangsreferenties.
- Zorg ervoor dat het abonnement dat u voor de Data Box-service gebruikt, een van de volgende typen is:
    - Microsoft Enterprise Agreement (EA). Meer informatie over [EA-abonnementen](https://azure.microsoft.com/pricing/enterprise-agreement/).
    - Cloud Solution Provider (CSP). Meer informatie over het [Azure CSP-programma](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-overview).
- U hebt eigenaars- of inzenderstoegang tot het abonnement nodig om een Data Box-order te kunnen maken.

### <a name="for-device"></a>Voor het apparaat

Zorg voordat u begint voor het volgende:
- U hebt een clientcomputer beschikbaar van waaruit u de gegevens kunt kopiëren. De clientcomputer moet voldoen aan deze vereisten:
    - Een [ondersteund besturingssysteem](data-box-disk-system-requirements.md#supported-operating-systems-for-clients) worden uitgevoerd.
    - Andere [vereiste software](data-box-disk-system-requirements.md#other-required-software-for-windows-clients) is geïnstalleerd in het geval van een Windows-client.  

## <a name="order-data-box-disk"></a>Data Box Disk bestellen

Aanmelden bij:

- De Azure-portal op de volgende URL: https://portal.azure.com te bestellen Data Box-schijf.
- Of, in de Azure Government-portal op de volgende URL: https://portal.azure.us. Voor meer informatie gaat u naar [verbinding maken met Azure Government met behulp van de portal](https://docs.microsoft.com/azure/azure-government/documentation-government-get-started-connect-with-portal).

De volgende stappen uitvoeren om Data Box-schijf.

1. Klink linksboven in de hoek van de portal op **+ Een resource maken** en zoek naar *Azure Data Box*. Klik op **Azure Data Box**.
    
   ![Zoek naar Azure Data Box 1](media/data-box-disk-deploy-ordered/search-data-box11.png)

2. Klik op **Create**.

3. Controleer of de Data Box-service beschikbaar is in uw regio. Voer de volgende gegevens in of selecteer deze en klik op **Toepassen**.

    ![De optie Data Box Disk selecteren](media/data-box-disk-deploy-ordered/select-data-box-sku-1.png)

    |Instelling|Value|
    |---|---|
    |Abonnement|Selecteer het abonnement waarvoor Data Box is ingeschakeld.<br> Het abonnement is gekoppeld aan uw factureringsrekening. |
    |Type overdracht| Importeren in Azure|
    |Bronland | Selecteer het land/de regio waarin uw gegevens zich momenteel bevindt.|
    |Doel-Azure-regio|Selecteer de Azure-regio waarnaar u uw gegevens wilt overdragen.|

  
5.  Selecteer **Data Box Disk**. De maximumcapaciteit van de oplossing voor één bestelling van 5 schijven is 35 TB. U kunt meerdere bestellingen doen voor grotere gegevensgrootten.

     ![De optie Data Box Disk selecteren](media/data-box-disk-deploy-ordered/select-data-box-sku-zoom.png)

6.  Voer in **Bestellen** de **Orderdetails** in. Voer de volgende informatie in of selecteer deze.

    |Instelling|Waarde|
    |---|---|
    |Name|Geef een beschrijvende naam op om de bestelling te volgen.<br> De naam kan tussen 3 en 24 tekens bevatten (letters, cijfers en afbreekstreepjes). <br> De naam moet beginnen en eindigen met een letter of cijfer. |
    |Resourcegroep| Gebruik een bestaande of maak een nieuwe. <br> Een resourcegroep is een logische container voor resources die samen kunnen worden beheerd of geïmplementeerd. |
    |Doel-Azure-regio| Selecteer een regio voor uw opslagaccount.<br> Momenteel worden opslagaccounts in alle regio's in de VS, West- en Noord-Europa, Canada en Australië ondersteund. |
    |Geschatte gegevensgrootte in TB| Voer een schatting in TB in. <br>Op basis van de gegevensgrootte stuurt Microsoft u het juiste aantal SSD-schijven van 8 TB (7 TB aan bruikbare capaciteit). <br>De maximale bruikbare capaciteit van 5 schijven is 35 TB. |
    |Wachtwoordsleutel voor schijf| Geef de wachtwoordsleutel voor schijf op als u het selectievakje **Aangepaste sleutel gebruiken in plaats van door Azure gegenereerde sleutel** inschakelt. <br> Geef een alfanumerieke sleutel tussen 12 tot 32 tekens lang op met ten minste één numeriek en één speciaal teken. Alleen de speciale tekens `@?_+` zijn toegestaan. <br> U kunt deze optie overslaan en de door Azure gegenereerde wachtwoordsleutel gebruiken om uw schijven te ontgrendelen.|
    |Opslaglocatie     | Kies in de storage-account of beheerde schijven of beide. <br> Op basis van de opgegeven Azure-regio, selecteer een opslagaccount in de gefilterde lijst van een bestaand opslagaccount. Data Box kan worden gekoppeld aan maximaal 10 opslagaccounts. <br> U kunt ook maken een nieuwe **voor algemeen gebruik v1**, **voor algemeen gebruik v2**, of **Blob storage-account**. <br>U kunt geen opslagaccounts gebruiken waarvoor regels zijn geconfigureerd. De opslagaccounts moeten **toegang vanaf alle netwerken toestaan**. Dit is te configureren in het gedeelte voor firewalls en virtuele netwerken.|

    Als u de storage-account als de opslaglocatie, ziet u de volgende schermafbeelding:

    ![Data Box-schijforder voor storage-account](media/data-box-disk-deploy-ordered/order-storage-account.png)

    Als met behulp van Data Box-schijf voor het maken van beheerde schijven van de on-premises VHD's, moet u ook de volgende informatie:

    |Instelling  |Waarde  |
    |---------|---------|
    |Resourcegroep     | Maak een nieuwe resourcegroep als u van plan bent voor het maken van beheerde schijven vanaf on-premises VHD's. Gebruik een bestaande resourcegroep alleen als deze is gemaakt voor de Data Box-schijforder voor beheerde schijf met de Data Box-service. <br> Slechts één resourcegroep wordt ondersteund.|

    ![Data Box-schijforder voor beheerde schijf](media/data-box-disk-deploy-ordered/order-managed-disks.png)

    Het opslagaccount dat is opgegeven voor beheerde schijven wordt als een tijdelijke opslagaccount dat gebruikt. De Data Box-service wordt de VHD's naar het tijdelijke opslagaccount dat wordt geüpload en vervolgens worden geconverteerd naar beheerde schijven en wordt verplaatst naar de resourcegroepen. Zie voor meer informatie, [controleren of gegevens uploaden naar Azure](data-box-disk-deploy-picked-up.md#verify-data-upload-to-azure).

13. Klik op **volgende**.

    ![Ordergegevens invoeren](media/data-box-disk-deploy-ordered/data-box-order-details.png)

14. In het tabblad **Verzendadres** geeft u uw voor- en achternaam, de naam en het postadres van het bedrijf en een geldig telefoonnummer op. Klik op **Adres valideren**. De service controleert of de service beschikbaar is voor de regio van het verzendadres. Als de service beschikbaar is voor het opgegeven verzendadres, ontvangt u daarover een melding. 

    ![Verzendadres opgeven](media/data-box-disk-deploy-ordered/data-box-shipping-address.png)
15. In de **Meldingsdetails** geeft u een e-mailadres op. De service stuurt e-mailmeldingen naar het opgegeven e-mailadres over updates van de bestelstatus. 

    We raden u aan een e-mailadres van een groep te gebruiken, zodat u meldingen blijft ontvangen als een beheerder de groep verlaat.

16. Bekijk de gegevens met betrekking tot de bestelling, het contact, de meldingen en de privacyvoorwaarden in **Samenvatting**. Vink het selectievakje aan waarmee u akkoord gaat met de privacyvoorwaarden.

17. Klik op **Bestellen**. Het duurt een paar minuten voordat de bestelling is gemaakt.

 
## <a name="track-the-order"></a>De bestelling volgen

Nadat u uw bestelling hebt geplaatst, kunt u de status van de bestelling volgen via de Azure-portal. Ga naar uw bestelling en vervolgens naar **Overzicht** om de status te bekijken. In de portal ziet u dat de taak de status **Besteld** heeft.

![Status bestel van Data Box Disk](media/data-box-disk-deploy-ordered/data-box-portal-ordered.png) 

Als de schijven niet beschikbaar zijn, ontvangt u een melding. Als de schijven beschikbaar zijn, identificeert Microsoft de schijven die moeten worden verzonden en bereidt Microsoft een schijfpakket voor. Tijdens de voorbereiding van de schijf vinden de volgende acties plaats:

- De schijven worden versleuteld met AES-128 BitLocker-versleuteling.  
- De schijven worden vergrendeld om onbevoegde toegang tot de schijven te voorkomen.
- De sleutel die de schijven ontgrendelt, wordt tijdens dit proces gegenereerd.

Wanneer de schijfvoorbereiding is voltooid, wordt de bestelling in de portal weergegeven met de status **Verwerkt**.

Microsoft bereidt uw schijven vervolgens voor en verzendt deze met een regionale vervoerder. U ontvangt uw volgnummer wanneer de schijven zijn verzonden. In de portal wordt bestelling weergegeven met de status **Verzonden**.

## <a name="cancel-the-order"></a>De bestelling annuleren

Als u deze bestelling wilt annuleren, gaat u in de Azure-portal naar **Overzicht** en klikt u in de opdrachtbalk op **Annuleren**.

U kunt alleen annuleren wanneer de schijven zijn besteld en de bestelling nog wordt verwerkt voor verzending. Nadat de bestelling is verwerkt, kunt u de bestelling niet meer annuleren.

![Bestelling annuleren](media/data-box-disk-deploy-ordered/cancel-order1.png)

Als u een geannuleerde bestelling wilt verwijderen, gaat u naar **Overzicht** en klikt u in de opdrachtbalk op **Verwijderen**.


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie bent u meer te weten gekomen over verschillende onderwerpen met betrekking tot Azure Data Box, zoals:

> [!div class="checklist"]
> * Data Box Disk bestellen
> * De bestelling volgen
> * De bestelling annuleren

Ga naar de volgende zelfstudie om te lezen hoe u uw Data Box Disk instelt.

> [!div class="nextstepaction"]
> [Uw Azure Data Box Disk instellen](./data-box-disk-deploy-set-up.md)
