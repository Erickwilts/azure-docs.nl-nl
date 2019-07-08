---
title: 'Zelfstudie: Toegang tot opslagblobs met een aangepast Azure CDN-domein via HTTPS | Microsoft Docs'
description: ''
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/15/2018
ms.author: magattus
ms.custom: mvc
ms.openlocfilehash: e3b10760b95662570c8a6e802a57e1073e2faa0a
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67593367"
---
# <a name="tutorial-access-storage-blobs-using-an-azure-cdn-custom-domain-over-https"></a>Zelfstudie: Toegang tot opslag-blobs met behulp van een aangepast Azure CDN-domein via HTTPS

Nadat u uw Azure-opslagaccount hebt geïntegreerd met Azure Content Delivery Network (CDN), kunt u een aangepast domein toevoegen en HTTPS in dat domein inschakelen voor uw aangepaste eindpunt voor blob-opslag. 

## <a name="prerequisites"></a>Vereisten

Voordat u de stappen in deze zelfstudie kunt voltooien, moet u eerst uw Azure-opslagaccount integreren met Azure CDN. Zie voor meer informatie [Snelstart: Een Azure storage-account integreren met Azure CDN](cdn-create-a-storage-account-with-cdn.md).

## <a name="add-a-custom-domain"></a>Een aangepast domein toevoegen
Wanneer u een CDN-eindpunt maakt in uw profiel, wordt de naam van het eindpunt, wat een subdomein van azureedge.net is, standaard opgenomen in de URL voor het leveren van CDN-inhoud. U hebt ook de mogelijkheid om een aangepast domein te koppelen aan een CDN-eindpunt. In dat geval wordt de inhoud aangeboden via een aangepast domein in uw URL in plaats van de naam van een eindpunt. Als u wilt een aangepast domein toevoegen aan uw eindpunt, de instructies in deze zelfstudie te volgen: [Een aangepast domein toevoegen aan uw Azure CDN-eindpunt](cdn-map-content-to-custom-domain.md).

## <a name="configure-https"></a>HTTPS configureren
Door het HTTPS-protocol in te schakelen voor uw aangepaste domein weet u zeker dat uw gegevens veilig worden afgeleverd op internet via TLS/SSL-versleuteling. Wanneer uw webbrowser is verbonden met een website via HTTPS, wordt het beveiligingscertificaat van de website gevalideerd en wordt er gecontroleerd of deze is uitgegeven door een legitieme certificeringsinstantie. Voor het configureren van HTTPS voor uw aangepaste domein, de instructies in deze zelfstudie te volgen: [HTTPS configureren op een aangepast Azure CDN-domein](cdn-custom-ssl.md).

## <a name="shared-access-signatures"></a>Shared Access Signatures
Als het eindpunt voor uw blobopslag zo is geconfigureerd dat anonieme leestoegang niet is toegestaan, moet u in elke aanvraag naar het aangepaste domein een [SAS-token (Shared Access Signature)](cdn-sas-storage-support.md) opnemen. De standaardinstelling is dat eindpunten voor blobopslag anonieme leestoegang niet toestaan. Zie [Anonieme leestoegang tot containers en blobs beheren](../storage/blobs/storage-manage-access-to-resources.md) voor meer informatie over SAS.

Azure CDN negeert eventuele beperkingen die zijn toegevoegd aan het SAS-token. Zo hebben alle SAS-tokens een verlooptijd. Maar omdat deze beperking wordt genegeerd, is inhoud nog steeds toegankelijk met een verlopen SAS, althans totdat de inhoud wordt verwijderd van de POP-servers (Point Of Presence) van het CDN. U kunt bepalen hoe lang gegevens worden opgeslagen in de cache van Azure CDN door de Cache-Control-header in te stellen. Zie [Vervaldatum van Azure Storage-blobs in Azure CDN beheren](cdn-manage-expiration-of-blob-content.md) voor meer informatie.

Als u meerdere SAS-URL's voor hetzelfde blob-eindpunt maakt, kunt u overwegen om het cachen van queryreeksen in te schakelen. Hiermee zorgt u ervoor dat elke URL als een unieke entiteit wordt beschouwd. Zie [Cachegedrag in Azure CDN bepalen met queryreeksen](cdn-query-string.md) voor meer informatie.

## <a name="http-to-https-redirection"></a>HTTP-naar-HTTPS-omleiding
U kunt ervoor kiezen om HTTP-verkeer om te leiden naar HTTPS door een [regel voor URL-omleiding](cdn-verizon-premium-rules-engine-reference-features.md#url-redirect) te maken met de [regel-engine van Azure CDN](cdn-verizon-premium-rules-engine.md). Hiervoor hebt u wel een profiel voor **Azure CDN Premium van Verizon** nodig.

![Regel voor URL-omleiding](./media/cdn-storage-custom-domain-https/cdn-url-redirect-rule.png)

In deze regel verwijst *Cdn-endpoint-name* naar de naam die u hebt geconfigureerd voor uw CDN-eindpunt, die u kunt selecteren in de vervolgkeuzelijst. De waarde voor *origin-path* verwijst naar het pad binnen het opslagaccount van herkomst waarin de statische inhoud zich bevindt. Als u alle statische inhoud in één container host, vervangt u *origin-path* door de naam van die container.

## <a name="pricing-and-billing"></a>Prijzen en facturering
Als u via Azure CDN toegang hebt tot blobs, betaalt u [Blob-opslagprijzen](https://azure.microsoft.com/pricing/details/storage/blobs/) voor verkeer tussen de POP-servers en de herkomst (Blob-opslag) en [Azure CDN-prijzen](https://azure.microsoft.com/pricing/details/cdn/) voor gegevens die worden geraadpleegd vanaf de POP-servers.

Als u bijvoorbeeld een opslagaccount in de Verenigde Staten hebt dat is ingesteld voor Azure CDN en iemand in Europa via Azure CDN probeert toegang te krijgen tot een van de blobs in dat opslagaccount, controleert Azure CDN eerst of die blob aanwezig is op de POP-server die zich het dichtst in de buurt van Europa bevindt. Als die wordt gevonden, opent Azure CDN die kopie van de blob en worden de CDN-prijzen gehanteerd, omdat de blob wordt gebruikt via Azure CDN. Als de blob niet wordt gevonden, kopieert Azure CDN de blob naar de POP-server, waardoor er kosten voor uitgaand verkeer en transacties in rekening worden gebracht zoals vastgelegd in de prijzen voor Blob-opslag. Vervolgens wordt het bestand op de POP-server geopend, wat resulteert in facturering voor Azure CDN.

## <a name="next-steps"></a>Volgende stappen
[Zelfstudie: Azure CDN-regels voor opslaan in cache instellen](cdn-caching-rules-tutorial.md)




