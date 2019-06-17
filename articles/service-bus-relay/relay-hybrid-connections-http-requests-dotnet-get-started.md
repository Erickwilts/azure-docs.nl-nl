---
title: Aan de slag met HTTP-aanvragen voor hybride verbindingen van Azure Relay in .NET | Microsoft Docs
description: Hier leert u hoe u een C#-consoletoepassing schrijft voor HTTP-aanvragen voor hybride verbindingen van Azure Relay in .NET.
services: service-bus-relay
documentationcenter: .net
author: spelluru
manager: timlt
editor: ''
ms.assetid: d1386900-b942-4abf-acfc-38d2ef826253
ms.service: service-bus-relay
ms.devlang: tbd
ms.topic: conceptual
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 11/01/2018
ms.author: spelluru
ms.openlocfilehash: 37227b7d0ea1b3630a3c2ce991a61543e6a1503d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66428242"
---
# <a name="get-started-with-relay-hybrid-connections-http-requests-in-net"></a>Aan de slag met HTTP-aanvragen voor hybride verbindingen van Relay in .NET
[!INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

In deze snelstart maakt u .NET maakt u toepassingen voor afzenders en ontvangers waarmee berichten worden verzonden en ontvangen met behulp van het HTTP-protocol. De toepassingen maken gebruik van de functie Hybride verbindingen van Azure Relay. Zie [Azure Relay](relay-what-is-it.md) voor meer informatie over Azure Relay in het algemeen. 

In deze snelstart voert u de volgende stappen uit:

1. Een Relay-naamruimte maken met behulp van Azure Portal.
2. Een hybride verbinding in die naamruimte maken met behulp van Azure Portal.
3. Een serverconsoletoepassing (listener) schrijven om berichten te ontvangen.
4. Een clientconsoletoepassing (afzender) schrijven om berichten te verzenden.
5. Toepassingen uitvoeren. 

## <a name="prerequisites"></a>Vereisten

Voor het voltooien van deze zelfstudie moet aan de volgende vereisten worden voldaan:

* [Visual Studio 2015 of hoger](https://www.visualstudio.com). In de voorbeelden in deze zelfstudie wordt Visual Studio 2017 gebruikt.
* Een Azure-abonnement. Als u nog geen abonnement hebt, [maakt u een gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="create-a-namespace"></a>Een naamruimte maken
[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="create-a-hybrid-connection"></a>Een hybride verbinding maken
[!INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="create-a-server-application-listener"></a>Een servertoepassing (listener) maken
Maak in Visual Studio een C#-consoletoepassing om berichten van de Relay te beluisteren en te ontvangen.

[!INCLUDE [relay-hybrid-connections-http-requests-dotnet-get-started-server](../../includes/relay-hybrid-connections-http-requests-dotnet-get-started-server.md)]

## <a name="create-a-client-application-sender"></a>Een clienttoepassing maken (afzender)
Maak in Visual Studio een C#-consoletoepassing om berichten naar de Relay te sturen.

[!INCLUDE [relay-hybrid-connections-http-requests-dotnet-get-started-client](../../includes/relay-hybrid-connections-http-requests-dotnet-get-started-client.md)]

## <a name="run-the-applications"></a>De toepassingen uitvoeren
1. Voer de servertoepassing uit. U ziet de volgende tekst in het consolevenster:

    ```
    Online
    Server listening
    ```
1. Voer de clienttoepassing uit. U ziet `hello!` in het clientvenster. De client heeft een HTTP-aanvraag verzonden naar de server en de server heeft gereageerd met een `hello!`. 
3. Nu kunt u de consolevensters sluiten door in beide consolevenster op **Enter** te drukken. 

Gefeliciteerd, u hebt een complete toepassing met hybride verbindingen gemaakt.

## <a name="next-steps"></a>Volgende stappen

In deze snelstart hebt u een .NET-client- en servertoepassing gemaakt waarmee berichten worden verzonden en ontvangen met behulp van het HTTP-protocol. De functie Hybride verbindingen van Azure Relay ondersteunt tevens WebSockets voor het verzenden en ontvangen van berichten. Zie de [snelstart over WebSockets](relay-hybrid-connections-dotnet-get-started.md) voor informatie over het gebruik van WebSockets met hybride verbindingen van Azure Relay.

In deze snelstart hebt u .NET Framework gebruikt om client- en servertoepassingen te maken. Zie de [snelstart over WebSockets in Node.js](relay-hybrid-connections-node-get-started.md) of de [snelstart over HTTP in Node.js](relay-hybrid-connections-http-requests-dotnet-get-started.md) voor informatie over het schrijven van client- en servertoepassingen in Node.js.
