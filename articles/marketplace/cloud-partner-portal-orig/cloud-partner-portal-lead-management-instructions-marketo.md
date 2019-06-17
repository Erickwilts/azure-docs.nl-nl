---
title: Leadbeheer van de in Marketo configureren | Azure Marketplace
description: Leadbeheer configureren voor Marketo voor klanten van Azure marketplace.
services: Azure, Marketplace, Cloud Partner Portal,
author: dan-wesley
ms.service: marketplace
ms.topic: conceptual
ms.date: 09/14/2018
ms.author: pabutler
ms.openlocfilehash: d749a92a1837bad053dc586477bfc27fc65299fa
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64935213"
---
# <a name="configure-lead-management-in-marketo"></a>Leadbeheer van de in Marketo configureren

In dit artikel wordt beschreven hoe u Marketo instellen voor het afhandelen van verkoopleads van Microsoft.

1. Meld u aan met Marketo.
2. Selecteer **Studio ontwerpen**.
    ![Marketo Design Studio](./media/cloud-partner-portal-lead-management-instructions-marketo/marketo1.png)

3.  Selecteer **nieuw formulier**.
    ![Nieuw formulier Marketo](./media/cloud-partner-portal-lead-management-instructions-marketo/marketo2.png)

4.  Vul de vereiste velden in het nieuwe formulier en selecteer vervolgens **maken**.
    ![Marketo nieuw formulier maken](./media/cloud-partner-portal-lead-management-instructions-marketo/marketo3.png)

4.  Selecteer in het veld Details **voltooien**.
    ![Marketo voltooien formulier](./media/cloud-partner-portal-lead-management-instructions-marketo/marketo4.png)

5.  Goedkeuren en sluiten.

6.  Selecteer op het tabblad MarketplaceLeadBacked **Code insluiten**.
    ![Marketo optie code insluiten](./media/cloud-partner-portal-lead-management-instructions-marketo/marketo5.png)

7.  Marketo Embed Code bevat de code is vergelijkbaar met het volgende voorbeeld.

`<script src="//app-ys12.marketo.com/js/forms2/js/forms2.min.js"></script>`

    <form id="mktoForm_1179"></form>
    <script>MktoForms2.loadForm("("//app-ys12.marketo.com", "123-PQR-789", 1179);</script>

1. Kopieer de waarden die wordt weergegeven in Code insluiten, zodat u configureren dat kunt de **Server Id**, **Munchkin Id**, en **formulier-Id** in de Marketo-velden in de Cloud Partner-Portal.

Gebruik het volgende voorbeeld als richtlijn voor het ophalen van de id's u moet in het codevoorbeeld van Marketo insluiten.

- Server-Id = **ys12**
- Munchkin Id = **123-PQR-789**
- Formulier-Id = **1179**\
