---
title: Versies beheren
titleSuffix: Language Understanding - Azure Cognitive Services
description: Versies kunnen u bouwt en publiceert verschillende modellen. Er is een goede gewoonte om te klonen van de huidige actieve model naar een andere versie van de app voordat u wijzigingen in het model.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 04/16/2019
ms.author: diberry
ms.openlocfilehash: f919651cf39d1f2c48fca87da935e49e3affa79f
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60198867"
---
# <a name="use-versions-to-edit-and-test-without-impacting-staging-or-production-apps"></a>Versies gebruiken om te bewerken en te testen zonder dat dit fasering of productie-apps

Versies kunnen u bouwt en publiceert verschillende modellen. Er is een goede gewoonte om te klonen van de huidige actieve model met een andere [versie](luis-concept-version.md) van de app voordat u wijzigingen in het model. 

Als u wilt werken met versies, opent u uw app door het selecteren van de naam op **mijn Apps** pagina en selecteer vervolgens **beheren** in de bovenste balk, schakelt u vervolgens **versies** in het linkernavigatievenster. 

De lijst met versies ziet u welke versies worden gepubliceerd, waarin ze worden gepubliceerd, en welke versie momenteel actief is. 

[![Beheersectie, pagina-versies](./media/luis-how-to-manage-versions/versions-import.png "gedeelte beheren, pagina-versies")](./media/luis-how-to-manage-versions/versions-import.png#lightbox)

## <a name="clone-a-version"></a>Een versie klonen

1. Selecteer de versie die u wilt klonen Selecteer **kloon** via de werkbalk. 

2. In de **kloon versie** in het dialoogvenster, typ een naam voor de nieuwe versie, zoals '0.2'.

   ![Dialoogvenster van de kloon-versie](./media/luis-how-to-manage-versions/version-clone-version-dialog.png)
 
     > [!NOTE]
     > Versie-ID kan bestaan alleen uit tekens, cijfers of '.' en mag niet langer zijn dan 10 tekens.
 
   Een nieuwe versie met de opgegeven naam gemaakt en ingesteld als de actieve versie.

## <a name="set-active-version"></a>Actieve versie instellen

Selecteer een versie in de lijst en selecteer vervolgens **Maak actief** via de werkbalk. 

[![Beheersectie, pagina-versies, maken een actie versie](./media/luis-how-to-manage-versions/versions-other.png "beheersectie, pagina-versies, een versie-actie maken")](./media/luis-how-to-manage-versions/versions-other.png#lightbox)

## <a name="import-version"></a>Importversie

1. Selecteer **importeren versie** via de werkbalk. 

2. In de **de nieuwe versie importeren** pop-upvenster, geef de nieuwe naam voor de versie van tien tekens. U hoeft in te stellen van een versie-ID als de versie in het JSON-bestand al in de app bestaat.

    ![Sectie, pagina-versies, nieuwe versie importeren beheren](./media/luis-how-to-manage-versions/versions-import-pop-up.png)

    Nadat u een versie importeert, wordt de nieuwe versie de actieve versie.

### <a name="import-errors"></a>Fouten bij het importeren

* Tokenizer fouten: Als er een **tokenizer fout** bij het importeren, u probeert te importeren een versie die gebruikmaakt van een andere [tokenizer](luis-language-support.md#custom-tokenizer-versions) dan de app wordt momenteel gebruikt. U kunt dit verhelpen Zie [migreren tussen versies tokenizer](luis-language-support.md#migrating-between-tokenizer-versions).

<a name = "export-version"></a>

## <a name="other-actions"></a>Andere acties

* Naar **verwijderen** een versie gebruikt, selecteert u een versie in de lijst en vervolgens **verwijderen** via de werkbalk. Selecteer **OK**. 
* Naar **naam** een versie gebruikt, selecteert u een versie in de lijst en vervolgens **Wijzig de naam van** via de werkbalk. Voer nieuwe naam en selecteer **gedaan**. 
* Naar **exporteren** een versie gebruikt, selecteert u een versie in de lijst en vervolgens **Export app** via de werkbalk. Kies JSON naar de export voor back-up, kiest u **exporteren voor container** naar [gebruik van deze app in een container LUIS](luis-container-howto.md).  

