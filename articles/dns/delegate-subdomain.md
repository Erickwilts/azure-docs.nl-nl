---
title: Een Azure DNS-subdomein delegeren
description: Leer hoe u een Azure DNS-subdomein delegeren.
services: dns
author: vhorne
ms.service: dns
ms.topic: article
ms.date: 2/7/2019
ms.author: victorh
ms.openlocfilehash: 31543db8e177701ddfe6beaaa3091d6465b0e9cd
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60790807"
---
# <a name="delegate-an-azure-dns-subdomain"></a>Een Azure DNS-subdomein delegeren

De Azure-portal kunt u een DNS-subdomein delegeren. Als u eigenaar van het domein contoso.com, kunt u bijvoorbeeld een subdomein met de naam delegeren *engineering* aan een andere, afzonderlijke zone die u afzonderlijk vanaf de zone contoso.com beheren kunt.

Als u liever, kunt u een subdomein met delegeren [Azure PowerShell](delegate-subdomain-ps.md).

## <a name="prerequisites"></a>Vereisten

Voor het overdragen van een Azure DNS-subdomein, moet u eerst uw openbare domein naar Azure DNS delegeren. Zie [een domein delegeren naar Azure DNS](./dns-delegate-domain-azure-dns.md) voor instructies over het configureren van de naamservers voor overdracht. Nadat uw domein wordt overgedragen naar de Azure DNS-zone, kunt u een subdomein delegeren.

> [!NOTE]
> Contoso.com wordt gebruikt als voorbeeld in dit artikel. Vervang uw eigen domeinnaam door contoso.com.

## <a name="create-a-zone-for-your-subdomain"></a>Maak een zone voor het subdomein

Maak eerst de zone voor de **engineering** subdomein.

1. Selecteer in de Azure-portal **een resource maken**.
2. Typ in het zoekvak **DNS**, en selecteer **DNS-zone**.
3. Selecteer **Maken**.
4. In de **DNS-zone maken** deelvenster, type **engineering.contoso.com** in de **naam** in het tekstvak.
5. Selecteer de resourcegroep voor uw zone. Mogelijk wilt u dezelfde resourcegroep gebruiken als de bovenliggende zone vergelijkbare resources om samen te bewaren.
6. Klik op **Create**.
7. Nadat de implementatie is voltooid, gaat u naar de nieuwe zone.

## <a name="note-the-name-servers"></a>Houd er rekening mee de naamservers

Noteer vervolgens de vier naamservers voor de engineering-subdomein.

Op de **engineering** zone in het deelvenster Houd er rekening mee de vier naamservers voor de zone. U kunt deze naamservers wordt later gebruiken.

## <a name="create-a-test-record"></a>Een test-record maken

Maak een **A** record die moet worden gebruikt voor het testen. Maak bijvoorbeeld een **www** A vastleggen en deze configureren met een **10.10.10.10** IP-adres.

## <a name="create-an-ns-record"></a>Een NS-record maken

Maak vervolgens een naam (naamserver)-record voor de **engineering** zone.

1. Ga aan de zone voor het bovenliggende domein.
2. Selecteer **Recordset toevoegen**.
3. Op de **recordset toevoegen** deelvenster, type **engineering** in de **naam** in het tekstvak.
4. Voor **Type**, selecteer **NS**.
5. Onder **naamserver**, voer de vier naamservers die u eerder hebt genoteerd bij de **engineering** zone.
6. Klik op **OK**.

## <a name="test-the-delegation"></a>De overdracht testen

Nslookup gebruiken voor het testen van de overdracht.

1. Open een PowerShell-venster.
2. Typ het volgende achter de opdrachtprompt `nslookup www.engineering.contoso.com.`
3. U moet een niet-bindend antwoord met het adres ontvangt **10.10.10.10**.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [reverse-DNS voor services die worden gehost in Azure configureren](dns-reverse-dns-for-azure-services.md).