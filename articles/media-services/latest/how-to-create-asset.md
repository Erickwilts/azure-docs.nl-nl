---
title: Inhoud uploaden naar een Azure Media Services-asset met Azure CLI
description: In het Azure CLI-script in dit onderwerp ziet u hoe u een Media Services-asset kunt maken om inhoud naar te uploaden.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.assetid: ''
ms.service: media-services
ms.devlang: azurecli
ms.topic: how-to
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/31/2020
ms.author: inhenkel
ms.custom: devx-track-azurecli
ms.openlocfilehash: aa5c9178bc84983b7e577cd63cf5b8d9fb9a8436
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "89254658"
---
# <a name="create-an-asset"></a>Een Asset maken

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

In dit artikel wordt beschreven hoe u een Media Services-asset maakt.  U gebruikt een asset om media-inhoud te bewaren voor codering en streaming.  Lees [Assets in Azure Media Services v3](assets-concept.md) voor meer informatie over Media Services-assets

## <a name="prerequisites"></a>Vereisten

Volg de stappen in [Een Media Services-account maken](./create-account-howto.md) om het vereiste Media Services-account en de resourcegroep te maken om een asset te maken.

## <a name="methods"></a>Methoden

## <a name="cli"></a>[CLI](#tab/cli/)

[!INCLUDE [Create an asset with CLI](./includes/task-create-asset-cli.md)]

## <a name="example-script"></a>Voorbeeldscript

[!code-azurecli-interactive[main](../../../cli_scripts/media-services/create-asset/Create-Asset.sh "Create an asset")]

## <a name="rest"></a>[REST](#tab/rest/)

### <a name="using-rest"></a>REST gebruiken

[!INCLUDE [media-services-cli-instructions.md](./includes/task-create-asset-rest.md)]

### <a name="using-curl"></a>Met cURL

[!INCLUDE [media-services-cli-instructions.md](./includes/task-create-asset-curl.md)]

## <a name="using-postman"></a>Met Postman

[!INCLUDE[Create an asset with Postman](./includes/task-create-asset-postman.md)]

## <a name="net"></a>[.NET](#tab/net/)

[!INCLUDE [media-services-cli-instructions.md](./includes/task-create-asset-dotnet.md)]

---

## <a name="next-steps"></a>Volgende stappen

[Overzicht van Media Services](media-services-overview.md)
