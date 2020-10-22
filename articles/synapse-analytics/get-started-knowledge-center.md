---
title: 'Zelfstudie: Aan de slag met het Synapse Knowledge Center'
description: In deze zelfstudie leert u hoe u het Synapse Knowledge Center gebruikt.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.subservice: workspace
ms.topic: tutorial
ms.date: 09/15/2020
ms.openlocfilehash: dd055eecaef00a2f3b555034f44f3c04684a1722
ms.sourcegitcommit: 2989396c328c70832dcadc8f435270522c113229
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2020
ms.locfileid: "92171862"
---
# <a name="explore-the-synapse-knowledge-center"></a>Het Synapse Knowledge Center verkennen

In deze zelfstudie leert u hoe u het Synapse Studio Knowledge Center gebruikt.

## <a name="getting-to-the-knowledge-center"></a>Het Knowledge Center verkennen

Er zijn twee manieren om het Knowledge Center te vinden in Synapse Studio:

  1. Klik in de Home-hub onder Handige links op de eerste link met de naam **Knowledge Center**.
  2. Klik in de menubalk bovenaan op **?** en vervolgens op **Knowledge Center**.

Kies een van beide methoden en open het **Knowledge Center**.

## <a name="overview"></a>Overzicht

In het **Knowledge Center** kunt u drie dingen doen:
* **Voorbeelden direct gebruiken**. Deze optie is geoptimaliseerd zodat u zo snel mogelijk analyses in de praktijk kunt bekijken. Als u een snel voorbeeld wilt van hoe Synapse werkt, kiest u deze optie.
* **Beschikbare voorbeeld voor de browser**. Met deze optie kunt u voorbeeldgegevenssets koppelen en voorbeeldcode toevoegen in de vorm van SQL-scripts, -notebooks en -pijplijnen.
* **Rondleiding door Synapse Studio**. Met deze optie krijgt u een korte rondleiding door de basisonderdelen van Synapse Studio. Dit is handig als u Synapse Studio nog nooit eerder hebt gebruikt.

## <a name="exploring-blob-storage-with-sql-on-demand"></a>Blob-opslag verkennen met SQL on demand

1. Klik in het **Knowledge Center** op **Voorbeelden direct gebruiken**
1. Selecteer **Gegevens opvragen met SQL** 
1. Klik op **Voorbeelden direct gebruiken**
1. Er wordt een nieuw SQL-script gemaakt.
1. Ga naar de eerste query (regels 28 tot en met 32) en selecteer de querytekst
1. Klik op Run. De tekst die u hebt geselecteerd, wordt uitgevoerd.

## <a name="loading-more-nyc-taxi-data"></a>Meer NYC Taxi-gegevens laden
1. In het **Knowledge Center** klikt u op **Door beschikbare voorbeelden bladeren** 
1. Selecteer het tabblad **SQL-scripts** bovenaan
1. Selecteer **De dataset van New York Taxicab laden**
1. Kies onder **Invoer**de optie **Een bestaande pool selecteren** en selecteer **SQLDB1**
1. Klik op **Script openen**
1. Er wordt een nieuw SQL-script weergegeven.
1. Klik op **Uitvoeren**
1. Hiermee maakt u verschillende tabellen voor alle NYC Taxi-gegevens en laadt u deze met behulp van de T-SQL COPY-opdracht.

## <a name="next-steps"></a>Volgende stappen

* [Aan de slag met Azure Synapse Analytics](get-started.md)
* [Een werkruimte maken](quickstart-create-workspace.md)
* [SQL on-demand gebruiken](quickstart-sql-on-demand.md)
