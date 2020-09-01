---
title: Gegevens delen en ontvangen van Azure Blob Storage en Azure Data Lake Storage
description: Meer informatie over het delen en ontvangen van gegevens van Azure Blob Storage en Azure Data Lake Storage
author: jifems
ms.author: jife
ms.service: data-share
ms.topic: how-to
ms.date: 08/28/2020
ms.openlocfilehash: a54a9d4c50852fe78fd245723dd29f487d58f4b1
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/01/2020
ms.locfileid: "89270120"
---
# <a name="share-and-receive-data-from-azure-blob-storage-and-azure-data-lake-storage"></a>Gegevens delen en ontvangen van Azure Blob Storage en Azure Data Lake Storage

[!INCLUDE[appliesto-storage](includes/appliesto-storage.md)]

Azure data share ondersteunt het delen op basis van moment opnamen vanuit het opslag account. In dit artikel wordt uitgelegd hoe u gegevens kunt delen en ontvangen uit de volgende bronnen: Azure Blob Storage, Azure Data Lake Storage Gen1 Azure Data Lake Storage Gen2.

De Azure-gegevens share ondersteunt het delen van bestanden, mappen en bestands systemen van Azure Data Lake gen1 en Azure Data Lake Gen2. Het biedt ook ondersteuning voor het delen van blobs, mappen en containers vanuit Azure Blob Storage. Momenteel wordt alleen blok-BLOB ondersteund. Gegevens die worden gedeeld vanuit deze bronnen kunnen worden ontvangen in Azure Data Lake Gen2 of Azure Blob Storage.

Wanneer bestands systemen, containers of mappen worden gedeeld in delen op basis van moment opnamen, kan de gegevens consument ervoor kiezen om een volledige kopie van de share gegevens te maken of om alleen nieuwe of bijgewerkte bestanden te kopiëren. Incrementele moment opname is gebaseerd op de tijd van de laatste wijziging van de bestanden. Bestaande bestanden met dezelfde naam worden overschreven.

## <a name="share-data"></a>Gegevens delen

### <a name="prerequisites-to-share-data"></a>Vereisten voor het delen van gegevens

* Azure-abonnement: Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/) aan voordat u begint.
* Het e-mailadres voor Azure van de ontvanger (de e-mailalias werkt niet).
* Als de bron-Azure-gegevensopslag zich in een ander Azure-abonnement bevindt dan de opslag die u gebruikt om een Data Share-resource te maken, registreert u de [resourceprovider Microsoft.DataShare](concepts-roles-permissions.md#resource-provider-registration) in het abonnement waarin de Azure-gegevensopslag zich bevindt. 

### <a name="prerequisites-for-source-storage-account"></a>Vereisten voor het bron-opslag account

* Een Azure Storage-account: U kunt een gratis [Azure Storage-account](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account) aanmaken als u nog niet een hebt
* Machtiging om naar het opslagaccount te schrijven, aanwezig in *Microsoft.Storage/storageAccounts/write*. Deze machtiging maakt onderdeel uit van de rol Inzender.
* Machtiging om roltoewijzing toe te voegen aan het opslagaccount, aanwezig in *Microsoft.Authorization/role assignments/write*. Deze machtiging maakt onderdeel uit van de rol Eigenaar. 

### <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij de [Azure-portal](https://portal.azure.com/).

### <a name="create-a-data-share-account"></a>Een Data Share-account maken

Maak een Azure Data Share-resource in een Azure-resourcegroep.

1. Selecteer de menu knop in de linkerbovenhoek van de portal en selecteer vervolgens **een resource maken** (+).

1. Zoek naar *Data Share*.

1. Selecteer Data Share en selecteer **Maken**.

1. Vul de basisgegevens van uw Azure Data Share-resource in met de volgende informatie. 

     **Instelling** | **Voorgestelde waarde** | **Beschrijving van veld**
    |---|---|---|
    | Abonnement | Uw abonnement | Selecteer het Azure-abonnement dat u wilt gebruiken voor uw gegevensshare-account.|
    | Resourcegroep | *test-resource-group* | Gebruik een bestaande resourcegroep of maak een nieuwe. |
    | Locatie | *US - oost 2* | Geef een regio op voor uw gegevensshare-account.
    | Name | *datashareaccount* | Geef een naam op voor uw gegevensshare-account. |
    | | |

1. Selecteer **controleren + maken**en vervolgens **maken** om uw gegevens share-account in te richten. Het inrichten van een nieuw gegevensshare-account duurt doorgaans 2 minuten of minder. 

1. Nadat de implementatie is voltooid, selecteert u **Ga naar resource**.

### <a name="create-a-share"></a>Een share maken

1. Ga naar de overzichtspagina van uw gegevensshare.

    ![Uw gegevens delen](./media/share-receive-data.png "Uw gegevens delen") 

1. Selecteer **Beginnen met het delen van uw gegevens**.

1. Selecteer **Maken**.   

1. Vul de details in voor uw share. Geef een naam, type share, beschrijving van de share-inhoud en gebruiksvoorwaarden (optioneel) op. 

    ![EnterShareDetails](./media/enter-share-details.png "Gegevens van share invoeren") 

1. Selecteer **Doorgaan**.

1. Als u gegevens sets wilt toevoegen aan uw share, selecteert u **gegevens sets toevoegen**. 

    ![Gegevens sets toevoegen aan uw share](./media/datasets.png "Gegevenssets")

1. Selecteer het type gegevensset dat u wilt toevoegen. Welke lijst met typen gegevensset wordt weergegeven, is afhankelijk van het type share (momentopname of in-place) dat u in de vorige stap hebt geselecteerd. 

    ![AddDatasets](./media/add-datasets.png "Gegevenssets toevoegen")    

1. Navigeer naar het object dat u wilt delen en selecteer Gegevenssets toevoegen. 

    ![SelectDatasets](./media/select-datasets.png "Gegevenssets selecteren")    

1. Voer op het tabblad Ontvangers de e-mailadressen in van uw gegevensgebruiker door + Ontvanger toevoegen te selecteren. 

    ![AddRecipients](./media/add-recipient.png "Ontvangers toevoegen") 

1. Selecteer **Doorgaan**.

1. Als u het type share momentopname hebt geselecteerd, kunt u het momentopnameschema configureren om updates van uw gegevens naar uw gegevensgebruiker te sturen. 

    ![EnableSnapshots](./media/enable-snapshots.png "Momentopnamen inschakelen") 

1. Selecteer een begintijd en een herhalingsinterval. 

1. Selecteer **Doorgaan**.

1. Controleer op het tabblad Beoordelen en maken de inhoud, instellingen, ontvangers en synchronisatie-instellingen van uw pakket. Selecteer **Maken**.

Uw Azure-gegevensshare is nu gemaakt en de ontvanger van uw gegevensshare kan nu uw uitnodiging accepteren. 

## <a name="receive-data"></a>Gegevens ontvangen

### <a name="prerequisites-to-receive-data"></a>Vereisten voor het ontvangen van gegevens
Voordat u een uitnodiging voor gegevensshares kunt accepteren, moet u een aantal Azure-resources inrichten. Deze worden hieronder weergegeven. 

Zorg ervoor dat alle vereisten zijn voltooid voordat u een uitnodiging voor gegevensshares accepteert. 

* Azure-abonnement: Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/) aan voordat u begint.
* Een Data Share-uitnodiging: Een uitnodiging van Microsoft Azure met een onderwerp met de titel: Azure Data Share-uitnodiging van **<yourdataprovider@domain.com>** .
* Registreer de [Microsoft.DataShare-resourceprovider](concepts-roles-permissions.md#resource-provider-registration) in het Azure-abonnement waarin u een Data Share-resource gaat maken, en in het Azure-abonnement waarin uw Azure-doelgegevensarchieven zich bevinden.

### <a name="prerequisites-for-target-storage-account"></a>Vereisten voor het doel-opslag account

* Een Azure Storage-account: Hier kunt u een [Azure Storage-account](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account) maken als u nog geen account hebt. 
* Machtiging om naar het opslagaccount te schrijven, aanwezig in *Microsoft.Storage/storageAccounts/write*. Deze machtiging maakt onderdeel uit van de rol Inzender. 
* Machtiging om roltoewijzing toe te voegen aan het opslagaccount, aanwezig in *Microsoft.Authorization/role assignments/write*. Deze machtiging maakt onderdeel uit van de rol Eigenaar.  

### <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij de [Azure-portal](https://portal.azure.com/).

### <a name="open-invitation"></a>Uitnodiging openen

1. U kunt de uitnodiging openen vanuit uw e-mail of rechtstreeks vanuit de Azure-portal. 

   Als u de uitnodiging wilt openen vanuit uw e-mail, gaat u in uw Postvak IN naar de uitnodiging van de gegevensprovider. De uitnodiging is van Microsoft Azure en heeft de titel **Azure Data Share-uitnodiging van <yourdataprovider@domain.com>** . Klik op **Uitnodiging weergeven** om de uitnodiging te bekijken in Azure. 

   Als u de uitnodiging rechtstreeks vanuit de Azure-portal wilt openen, gaat u in de Azure-portal naar **Data Share-uitnodigingen**. Hiermee kunt u de lijst met Data Share-uitnodigingen openen.

   ![Lijst met uitnodigingen](./media/invitations.png "Lijst met uitnodigingen") 

1. Selecteer de share die u wilt bekijken. 

### <a name="accept-invitation"></a>Uitnodiging accepteren
1. Zorg ervoor dat u alle velden controleert, met inbegrip van de **Gebruiksvoorwaarden**. Als u akkoord gaat met de gebruiksvoorwaarden, moet u het vakje inschakelen om aan te geven dat u akkoord gaat. 

   ![Gebruiksvoorwaarden](./media/terms-of-use.png "Gebruiksvoorwaarden") 

1. Selecteer onder *Data Share-doelaccount* het abonnement en de resourcegroep waarin u de Data Share-gegevens wilt implementeren. 

   Selecteer voor het veld **Data Share-account** de optie **Nieuwe maken** als u geen bestaand Data Share-account hebt. Anders selecteert u een bestaand Data Share-account waarin u de informatie uit uw gegevensshare wilt accepteren. 

   In het veld **Ontvangen sharenaam** kunt u de standaardwaarde laten staan die is opgegeven door de gegevensprovider. U kunt er ook voor kiezen een nieuwe naam op te geven voor de ontvangen share. 

   Wanneer u akkoord gaat met de gebruiks voorwaarden en een gegevens share-account hebt opgegeven voor het beheren van uw ontvangen share, selecteert u **accepteren en configureren**. Er wordt een shareabonnement gemaakt. 

   ![Opties voor accepteren](./media/accept-options.png "Opties voor accepteren") 

   Hiermee gaat u naar de ontvangen share in uw gegevens share-account. 

   Als u de uitnodiging niet wilt accepteren, selecteert u *Weigeren*. 

### <a name="configure-received-share"></a>Ontvangen share configureren
Volg de onderstaande stappen om te configureren waar u gegevens wilt ontvangen.

1. Selecteer tabblad **gegevens sets** . Schakel het selectie vakje in naast de gegevensset waaraan u een bestemming wilt toewijzen. Selecteer **+ toewijzen aan doel** om een doel gegevens archief te kiezen. 

   ![Toewijzen aan doel](./media/dataset-map-target.png "Toewijzen aan doel") 

1. Selecteer een doel gegevens archief waarvan u wilt dat de gegevens binnenkomen. Alle gegevens bestanden in het doel gegevens archief met hetzelfde pad en dezelfde naam worden overschreven. 

   ![Doel opslag account](./media/map-target.png "Doelopslag") 

1. Als de gegevens provider een momentopname schema voor het delen van gegevens op basis van moment opnamen heeft gemaakt om regel matig een update voor de data te bieden, kunt u ook het schema voor moment opnamen inschakelen door het tabblad **momentopname schema** te selecteren. Schakel het selectie vakje naast het schema voor moment opnamen in en selecteer **+ inschakelen**.

   ![Momentopname schema inschakelen](./media/enable-snapshot-schedule.png "Momentopname schema inschakelen")

### <a name="trigger-a-snapshot"></a>Een momentopname activeren
Deze stappen zijn alleen van toepassing bij delen op basis van momentopnamen.

1. U kunt een moment opname activeren door het tabblad **Details** te selecteren, gevolgd door een **moment opname**van de trigger. Hier kunt u een volledige of incrementele momentopname van uw gegevens activeren. Als dit de eerste keer is dat u gegevens van uw gegevensprovider ontvangt, selecteert u volledig kopiëren. 

   ![Momentopname activeren](./media/trigger-snapshot.png "Momentopname activeren") 

1. Wanneer de status van de laatste uitvoering *Geslaagd* is, gaat u naar het doelgegevensarchief om de ontvangen gegevens te bekijken. Selecteer **Gegevenssets** en klik op de koppeling in het doelpad. 

   ![Gegevenssets van consumenten](./media/consumer-datasets.png "Toewijzing voor gegevenssets van consumenten") 

### <a name="view-history"></a>Geschiedenis weergeven
Deze stap is alleen van toepassing op het delen op basis van moment opnamen. Selecteer het tabblad **geschiedenis** om de geschiedenis van uw moment opnamen weer te geven. Hier vindt u een overzicht van alle moment opnamen die zijn gegenereerd voor de afgelopen 30 dagen. 

## <a name="next-steps"></a>Volgende stappen
U hebt geleerd hoe u gegevens kunt delen en ontvangen van een opslag account met behulp van de Azure data share-service. Voor meer informatie over het delen van andere gegevens bronnen gaat u verder met [ondersteunde gegevens archieven](supported-data-stores.md).

