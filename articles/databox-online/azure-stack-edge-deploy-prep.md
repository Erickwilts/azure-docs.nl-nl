---
title: Zelfstudie voor het voorbereiden van Azure-portal, datacenteromgeving om Azure Stack Edge te implementeren | Microsoft Docs
description: De eerste zelfstudie over het implementeren van Azure Stack Edge omvat het voorbereiden van de Azure-portal.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 06/03/2019
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to prepare the portal to deploy Azure Stack Edge so I can use it to transfer data to Azure.
ms.openlocfilehash: e6b752eab3f6a8f40fad8b2f947a82f86a8ccfe5
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/19/2020
ms.locfileid: "83652059"
---
# <a name="tutorial-prepare-to-deploy-azure-stack-edge"></a>Zelfstudie: Voorbereidingen voor de implementatie van Azure Stack Edge  

Dit is de eerste zelfstudie in de reeks zelfstudies voor implementatie die noodzakelijk zijn voor het voltooien van de implementatie van Azure Stack Edge. In deze zelfstudie wordt beschreven hoe u de Azure-portal voorbereidt voor de implementatie van een Azure Stack Edge-resource.

U hebt beheerdersbevoegdheden nodig om het installatie- en configuratieproces uit te voeren. Het voorbereiden van de portal duurt minder dan 10 minuten.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
>
> * Een nieuwe resource maken
> * De activeringssleutel ophalen

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="get-started"></a>Aan de slag

Raadpleeg de volgende zelfstudies in de voorgeschreven volgorde voor het implementeren van Azure Stack Edge.

| **#** | **In deze stap** | **Gebruikt u deze documenten** |
| --- | --- | --- | 
| 1. |**[De Azure-portal voorbereiden voor Azure Stack Edge](azure-stack-edge-deploy-prep.md)** |Maak en configureer uw Azure Stack Edge-resource voordat u een fysiek Azure Stack Edge-apparaat installeert. |
| 2. |**[Azure Stack Edge installeren](azure-stack-edge-deploy-install.md)**|Het fysieke Azure Stack Edge-apparaat uitpakken, plaatsen en aansluiten.  |
| 3. |**[Azure Stack Edge verbinden, instellen en activeren](azure-stack-edge-deploy-connect-setup-activate.md)** |Maak verbinding met de lokale webinterface, voltooi het instellen van het apparaat en activeer het. Het apparaat is klaar om er SMB- of NFS-shares op in te stellen.  |
| 4. |**[Gegevens overdragen met Azure Stack Edge](azure-stack-edge-deploy-add-shares.md)** |Voeg shares toe en maak verbinding met shares via SMB of NFS. |
| 5. |**[Gegevens transformeren met Azure Stack Edge](azure-stack-edge-deploy-configure-compute.md)** |Configureer de rekenmodules op het apparaat om gegevens te transformeren wanneer ze naar Azure worden overgezet. |

U kunt nu Azure Portal gaan instellen.

## <a name="prerequisites"></a>Vereisten

Hier volgen de configuratievereisten voor uw Azure Stack Edge-resource, uw Azure Stack Edge-apparaat en het datacenternetwerk.

### <a name="for-the-azure-stack-edge-resource"></a>Voor de Azure Stack Edge-resource

Zorg voordat u begint voor het volgende:

* Uw Microsoft Azure-abonnement is ingeschakeld voor een Azure Stack Edge-resource. Zorg dat een ondersteund abonnement gebruikt zoals [Microsoft Enterprise Agreement (EA)](https://azure.microsoft.com/overview/sales-number/), [Cloud Solution Provider (CSP)](https://docs.microsoft.com/partner-center/azure-plan-lp) of [Microsoft Azure Sponsorship](https://azure.microsoft.com/offers/ms-azr-0036p/). Abonnementen waar u betaalt per gebruik worden niet ondersteund.

* U hebt toegang als eigenaar of inzender op het niveau van de resourcegroep voor de Azure Stack Edge/Data Box Gateway, IoT Hub en Azure Storage-resources.

  * Als u een Azure Stack Eedge / Data Box Gateway resource wilt maken, moet u machtigingen hebben als inzender (of hoger) op het niveau van de resourcegroep. U moet er ook voor zorgen dat de `Microsoft.DataBoxEdge` provider is geregistreerd. Ga voor meer informatie over het registreren naar [Resourceprovider registreren](azure-stack-edge-manage-access-power-connectivity-mode.md#register-resource-providers).
  * Als u een IoT Hub-resource wilt maken, moet u ervoor zorgen dat de provider Microsoft.Devices is geregistreerd. Ga voor meer informatie over het registreren naar [Resourceprovider registreren](azure-stack-edge-manage-access-power-connectivity-mode.md#register-resource-providers).
  * Als u een resource voor een Storage-account wilt maken, moet u een toegangsbereik van een inzender of hoger hebben op het niveau van de resourcegroep. Azure Storage is standaard een geregistreerde resourceprovider.
* U hebt beheerder-of gebruikerstoegang tot Azure Active Directory Graph API. Zie [Azure Active Directory Graph API](https://docs.microsoft.com/previous-versions/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes#default-access-for-administrators-users-and-guest-users-) voor meer informatie.
* U hebt een Microsoft Azure Storage-account met toegangsreferenties.

### <a name="for-the-azure-stack-edge-device"></a>Voor het Azure Stack Edge-apparaat

Voordat u een fysiek apparaat implementeert, controleert u of:

- U hebt de beveiligingsgegevens die zijn opgenomen in het verzendpakket gecontroleerd.
- Er een 1U-sleuf beschikbaar is in een standaard 19-inch rek in uw datacenter om het apparaat te plaatsen.
- U beschikt over een vlak, stabiel en horizontaal werkoppervlak waar u het apparaat veilig kunt neerleggen.
- De locatie waar u het apparaat wilt neerzetten, beschikt over reguliere wisselstroom van een onafhankelijke bron of over een vermogenseenheid (PDU) met noodvoeding (UPS).
- U hebt toegang tot een fysiek apparaat.

### <a name="for-the-datacenter-network"></a>Voor datacenternetwerk

Zorg voordat u begint voor het volgende:

- Het netwerk in uw datacenter is geconfigureerd volgens de netwerkvereisten voor uw Azure Stack Edge-apparaat. Zie [Systeemvereisten voor Azure Stack Edge](azure-stack-edge-system-requirements.md) voor meer informatie.

- Voor normale bedrijfsomstandigheden van uw Azure Stack Edge hebt u het volgende nodig:

    - Minimaal 10 Mbps downloadbandbreedte om ervoor te zorgen dat het apparaat bijgewerkt blijft.
    - Minimaal 20 Mbps toegewezen upload- en downloadbandbreedte voor het overdragen van bestanden.

## <a name="create-a-new-resource"></a>Een nieuwe resource maken

Als u al een Azure Stack Edge-resource hebt voor het beheer van uw fysieke apparaat, kunt u deze stap overslaan en verdergaan met [Activeringscode ophalen](#get-the-activation-key).

Voer de volgende stappen uit in de Azure-portal om een Azure Stack Edge-resource te maken.

1. Gebruik uw Microsoft Azure-referenties om u aan te melden op 

    - Ga naar de Azure-portal op deze URL: [https://portal.azure.com](https://portal.azure.com).
    - Of de Azure Government Portal op deze URL: [https://portal.azure.us](https://portal.azure.us). Ga naar [Verbinding maken met Azure Government met behulp van de portal](https://docs.microsoft.com/azure/azure-government/documentation-government-get-started-connect-with-portal) voor meer informatie.

2. Selecteer **En een resource maken** in het linkerdeelvenster. Zoek en selecteer **Azure Stack Edge / Data Box Gateway**. Selecteer **Maken**.
3. Kies het abonnement dat u wilt gebruiken voor het Azure Stack Edge-apparaat. Selecteer de regio waar u de Azure Stack Edge-resource wilt implementeren. Zie [Azure-producten die beschikbaar zijn per regio](https://azure.microsoft.com/global-infrastructure/services/?products=databox&regions=all) voor een lijst met alle regio's waar de Azure Stack Edge-resource beschikbaar is.

    Kies een locatie die het dichtst bij de geografische regio ligt waar u uw apparaat wilt implementeren. In de regio worden alleen de metagegevens voor apparaatbeheer opgeslagen. De werkelijke gegevens kunnen worden opgeslagen in elk opslagaccount.
    
    Selecteer bij **Maken** bij de optie **Azure Stack Edge**.

    ![Azure Stack Edge-service zoeken](media/azure-stack-edge-deploy-prep/data-box-edge-sku.png)

3. Voer op het tabblad **Basisinstellingen** de volgende **Projectdetails** in of selecteer deze.
    
    |Instelling  |Waarde  |
    |---------|---------|
    |Abonnement    |Dit wordt automatisch ingevuld op basis van de eerdere selectie. Abonnement is gekoppeld aan uw factureringsrekening. |
    |Resourcegroep  |Maak een nieuwe groep of selecteer een bestaande groep.<br>Meer informatie over [Azure-resourcegroepen](../azure-resource-manager/resource-group-overview.md).     |

4. Voer de volgende **exemplaardetails** in of selecteer deze.

    |Instelling  |Waarde  |
    |---------|---------|
    |Naam   | Een beschrijvende naam om de resource aan te duiden.<br>De naam is tussen 2 en 50 tekens lang en kan letters, cijfers en afbreekstreepjes bevatten.<br> De naam begint en eindigt met een letter of cijfer.        |
    |Regio     |Zie [Azure-producten die beschikbaar zijn per regio](https://azure.microsoft.com/global-infrastructure/services/?products=databox&regions=all) voor een lijst met alle regio's waar de Azure Stack Edge-resource beschikbaar is. Als Azure Government wordt gebruikt, zijn alle overheidsregio's beschikbaar, zoals wordt weergegeven in de [Azure-regio's](https://azure.microsoft.com/global-infrastructure/regions/).<br> Kies een locatie die het dichtst bij de geografische regio ligt waar u uw apparaat wilt implementeren.|

    ![Details van het project en exemplaar](media/azure-stack-edge-deploy-prep/data-box-edge-resource.png)

5. Selecteer **Volgende: Verzendadres**.

    - Als u al een apparaat hebt, selecteert u de keuzelijst met invoervak voor **Ik heb een Azure Stack Edge-apparaat**.
    - Als dit het nieuwe apparaat is dat u bestelt, voert u de naam van de contactpersoon, het bedrijf, het adres voor het verzenden van het apparaat en contactgegevens in.

    ![Verzendadres voor nieuw apparaat](media/azure-stack-edge-deploy-prep/data-box-edge-resource1.png)

6. Selecteer **Volgende: Beoordelen en maken**.

7. Bekijk op het tabblad **Controleren en maken** de **Prijsdetails**, **Gebruiksvoorwaarden** en de details van uw resource. Selecteer de keuzelijst met invoervak voor **Ik heb de privacyvoorwaarden gecontroleerd**.

    ![Details van de resource en de privacyverklaring van Azure Stack Edge controleren](media/azure-stack-edge-deploy-prep/data-box-edge-resource2.png)

8. Selecteer **Maken**.

Het maken van de resource duurt enkele minuten. Nadat de resource succesvol is gemaakt en geïmplementeerd, wordt u daarvan op de hoogte gebracht. Selecteer **Ga naar resource**.

![Ga naar de Azure Stack Edge-resource](media/azure-stack-edge-deploy-prep/data-box-edge-resource3.png)

Nadat de bestelling is geplaatst, controleert Microsoft de bestelling en neemt contact met u op (via e-mail) met verzendgegevens.

![Melding voor beoordeling van de volgorde van de Azure Stack Edge](media/azure-stack-edge-deploy-prep/data-box-edge-resource4.png)

## <a name="get-the-activation-key"></a>De activeringssleutel ophalen

Nadat de Azure Stack Edge-resource is geactiveerd, hebt u de activeringssleutel nodig. Deze sleutel wordt gebruikt om uw Azure Stack Edge-apparaat te activeren en te verbinden met de resource. U kunt deze sleutel nu ophalen, terwijl u Azure Portal geopend hebt.

1. Selecteer de resource die u hebt gemaakt. Selecteer **Overzicht** en selecteer vervolgens **Apparaatinstallatie**.

    ![Apparaatinstallatie selecteren](media/azure-stack-edge-deploy-prep/data-box-edge-select-devicesetup.png)

2. Selecteer **Sleutel genereren** op de tegel **Activeren** om een activeringssleutel te maken. Selecteer het kopieerpictogram om de sleutel te kopiëren en op te slaan voor later gebruik.

    ![Activeringssleutel ophalen](media/azure-stack-edge-deploy-prep/get-activation-key.png)

> [!IMPORTANT]
>
> * De activeringssleutel verloopt drie dagen nadat deze is gegenereerd.
> * Als de sleutel is verlopen, genereert u een nieuwe sleutel. De oudere toegangssleutel is niet langer geldig.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie bent u meer te weten gekomen over verschillende onderwerpen met betrekking tot Azure Stack Edge, zoals:

> [!div class="checklist"]
>
> * Een nieuwe resource maken
> * De activeringssleutel ophalen

Ga naar de volgende zelfstudie om te lezen hoe u Azure Stack Edge installeert.

> [!div class="nextstepaction"]
> [Azure Stack Edge installeren](./azure-stack-edge-deploy-install.md)



