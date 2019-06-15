---
title: Groep machines voor evaluatie met Azure Migrate | Microsoft Docs
description: Beschrijft hoe u aan de groep machines voordat u een evaluatie met de service Azure Migrate uitvoert.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 11/28/2018
ms.author: raynew
ms.openlocfilehash: c11d2f22fa08417107b0eecdd902b4521410b358
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60544910"
---
# <a name="group-machines-for-assessment"></a>Groepeer machines voor evaluatie

In dit artikel wordt beschreven hoe u een groep machines voor evaluatie van [Azure Migrate](migrate-overview.md). Azure Migrate beoordeelt de machines in de groep om te controleren of ze geschikt voor migratie naar Azure, en biedt formaat- en kostenramingen voor het uitvoeren van de machine in Azure. Als u de machines die samen moeten worden gemigreerd, kunt u de groep handmatig maken in Azure migreren met behulp van de volgende methode. Als u niet zeker weet over de computers die moeten worden gegroepeerd, kunt u de functie voor visualisatie van afhankelijkheden in Azure Migrate gebruiken om groepen te maken. [Meer informatie.](how-to-create-group-machine-dependencies.md)

> [!NOTE]
> De functie voor visualisatie van afhankelijkheden is niet beschikbaar in Azure Government.

## <a name="create-a-group"></a>Een groep maken

1. In de **overzicht** van de Azure Migrate-project, onder beheren, klikt u op **groepen** > **+ groep**, en geef een groepsnaam op.
2. Een of meer computers toevoegen aan de groep en klikt u op **maken**.
3. U kunt eventueel selecteren om uit te voeren van een nieuwe beoordeling van de groep.

    ![Een groep maken](./media/how-to-create-a-group/create-group.png)

Nadat de groep is gemaakt, kunt u deze wijzigen door het selecteren van de groep op de **groepen** pagina, en vervolgens toevoegen of verwijderen van computers.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het gebruik van [machine afhankelijkheidstoewijzing](how-to-create-group-machine-dependencies.md) te maken van groepen met hoge betrouwbaarheid.
- [Meer informatie](concepts-assessment-calculation.md) over hoe evaluaties worden berekend.
