---
title: bestand opnemen
description: bestand opnemen
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: include
ms.date: 10/18/2018
ms.author: raynew
ms.custom: include file
ms.openlocfilehash: 74fe531c07aa871b06b5d0773f7e8fb0ade80be4
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67176390"
---
## <a name="create-a-recovery-services-vault"></a>Een Recovery Services-kluis maken
Een Recovery Services-kluis is een entiteit die de back-ups en herstelpunten die zijn gemaakt na verloop van tijd worden opgeslagen. De Recovery Services-kluis bevat ook het back-upbeleid die gekoppeld aan de beveiligde virtuele machines zijn.

Een Recovery Services-kluis maken:

1. Meld u aan bij uw abonnement in [Azure Portal](https://portal.azure.com/).

2. Selecteer in het menu links **Alle services**.

    ![Alle services selecteren](./media/backup-create-rs-vault/click-all-services.png)

3. In het dialoogvenster **Alle services** voert u **Recovery Services** in. De lijst met resources gefilterd op basis van uw invoer. Selecteer in de lijst met resources **Recovery Services-kluizen**.

    ![Recovery Services-kluizen invoeren en kiezen](./media/backup-create-rs-vault/all-services.png)

    De lijst met Recovery Services-kluizen in het abonnement wordt weergeven.
    
4. Op de **Recovery Services-kluizen** dashboard, selecteer **toevoegen**.

    ![Toevoegen van een Recovery Services-kluis](./media/backup-create-rs-vault/add-button-create-vault.png)

    De **Recovery Services-kluis** in het dialoogvenster wordt geopend. Waarden opgeven voor de **naam**, **abonnement**, **resourcegroep**, en **locatie**.

    ![De Recovery Services-kluis configureren](./media/backup-create-rs-vault/create-new-vault-dialog.png)

   - **Naam**: Voer een beschrijvende naam voor het identificeren van de kluis. De naam moet uniek zijn voor het Azure-abonnement. Geef een naam met ten minste twee, maar niet meer dan 50 tekens. De naam moet beginnen met een letter en bestaan alleen uit letters, cijfers en afbreekstreepjes bevatten.
   - **Abonnement**: Kies het abonnement te gebruiken. Als u een lid van slechts één abonnement bent, ziet u die naam. Als u niet zeker weet welk abonnement u moet gebruiken, gebruikt u het standaardabonnement (aanbevolen). Er zijn meerdere opties alleen als uw werk of school-account is gekoppeld aan meer dan één Azure-abonnement.
   - **Resourcegroep**: Gebruik een bestaande resourcegroep of maak een nieuwe. De lijst met beschikbare resourcegroepen in uw abonnement wilt bekijken, selecteert u **gebruik bestaande**, en selecteert u een resource in de vervolgkeuzelijst. Voor het maken van een nieuwe resourcegroep selecteert **nieuw** en voer de naam. Zie voor meer informatie over resourcegroepen [overzicht van Azure Resource Manager](../articles/azure-resource-manager/resource-group-overview.md).
   - **Locatie**: Selecteer de geografische regio voor de kluis. Een kluis ter bescherming van virtuele machines, de kluis maken **moet** zich in dezelfde regio bevinden als de virtuele machines.

      > [!IMPORTANT]
      > Als u niet zeker van de locatie van uw virtuele machine bent, sluit u het dialoogvenster. Ga naar de lijst met virtuele machines in de portal. Als u virtuele machines in meerdere regio's hebt, maakt u een Recovery Services-kluis in elke regio. Maak de kluis in de eerste locatie voordat u de kluis voor een andere locatie maken. Er is niet nodig om op te geven van de storage-accounts voor het opslaan van de back-upgegevens. De Recovery Services-kluis en de Azure Backup-service worden verwerkt die automatisch.
      >
      >

5. Wanneer u klaar bent om de Recovery Services-kluis maken, selecteert u **maken**.

    ![De Recovery Services-kluis maken](./media/backup-create-rs-vault/click-create-button.png)

    Het kan even om te maken van de Recovery Services-kluis. Houd de statusmeldingen in de **meldingen** gebied aan de rechterbovenhoek van de portal. Nadat de kluis is gemaakt, wordt het weergegeven in de lijst met Recovery Services-kluizen. Als u uw kluis niet ziet, selecteert u **vernieuwen**.

     ![Vernieuw de lijst met back-upkluizen](./media/backup-create-rs-vault/refresh-button.png)
