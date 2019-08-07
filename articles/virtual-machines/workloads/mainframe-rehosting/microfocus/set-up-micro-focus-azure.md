---
title: Micro Focus Enter prise Server 4,0 en Enter prise Developer 4,0 op Azure installeren | Microsoft Docs
description: Host uw IBM z/OS mainframe-workloads met behulp van de micro focus ontwikkelings-en test omgeving op virtuele machines van Azure (Vm's).
services: virtual-machines-linux
documentationcenter: ''
author: njray
ms.author: edprice
manager: edprice
editor: edprice
ms.topic: conceptual
ms.date: 05/29/2019
tags: ''
keywords: ''
ms.service: multiple
ms.openlocfilehash: a5426c3cd7552b24739f9a20e01d5a4b42bd383c
ms.sourcegitcommit: bc3a153d79b7e398581d3bcfadbb7403551aa536
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68834569"
---
# <a name="install-micro-focus-enterprise-server-40-and-enterprise-developer-40-on-azure"></a>Micro Focus Enter prise Server 4,0 en Enter prise Developer 4,0 op Azure installeren

In dit artikel wordt uitgelegd hoe u [micro focus Enter prise Server 4,0](https://www.microfocus.com/documentation/enterprise-developer/es30/) en [micro focus enter prise Developer 4,0](https://www.microfocus.com/documentation/enterprise-developer/ed_30/) kunt instellen op Azure.

Een veelvoorkomende werk belasting op Azure is een ontwikkel-en test omgeving. Dit scenario is gebruikelijk omdat het voordelig en eenvoudig te implementeren is. Met Enter prise server heeft micro focus een van de grootste platformen voor het hosten van mainframes die beschikbaar zijn. U kunt met behulp van virtuele Windows-of Linux-machines (Vm's) de werk belasting van een z/O'S uitvoeren op een goedkopere x86-platform op Azure.

Deze instelling maakt gebruik van Azure-Vm's waarop de installatie kopie van Windows Server 2016 wordt uitgevoerd vanuit Azure Marketplace met Microsoft SQL Server 2017 al geïnstalleerd. Deze instelling is ook van toepassing op Azure Stack.

De bijbehorende ontwikkel omgeving voor Enter prise server is Enter prise Developer, dat wordt uitgevoerd op micro soft Visual Studio 2017 of hoger, Visual Studio Community (gratis te downloaden) of eclips. In dit artikel wordt beschreven hoe u dit implementeert met een virtuele machine met Windows Server 2016, die wordt geleverd bij Visual Studio 2017 of hoger is geïnstalleerd.

## <a name="prerequisites"></a>Vereisten

Bekijk de volgende vereisten voordat u aan de slag gaat:

- Een Azure-abonnement. Als u nog geen abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) voordat u begint.

- De micro focus-software en een geldige licentie (of proef licentie). Als u een bestaande micro focus-klant bent, neemt u contact op met uw micro focus-vertegenwoordiger. Vraag anders [een proef versie](https://www.microfocus.com/products/enterprise-suite/enterprise-server/trial/)aan.

- Down load de documentatie voor [Enter prise server en Enter prise Developer](https://www.microfocus.com/documentation/enterprise-developer/#").

> [!NOTE]
> Een best practice is het instellen van een site-naar-site VPN-tunnel (virtueel particulier netwerk) of een JumpBox zodat u de toegang tot de virtuele Azure-machines kunt beheren.

## <a name="install-enterprise-server"></a>Enterprise-server installeren

1. Voor betere beveiliging en beheer kunt u overwegen om een nieuwe resource groep te maken voor dit project, bijvoorbeeld **RGMicroFocusEntServer**. Gebruik het eerste deel van de naam in azure om het type resource te kiezen, zodat het gemakkelijker is om een lijst te maken.

2. Hiermee maakt u een virtuele machine. Selecteer in azure Marketplace de virtuele machine en het besturings systeem dat u wilt. Hier volgt een aanbevolen installatie:

    - **Enter prise server**: Selecteer ES2 v3 VM (met 2 Vcpu's en 16 GB geheugen) met Windows Server 2016 en SQL Server 2017 geïnstalleerd. Deze installatie kopie is beschikbaar op de Azure Marketplace. Enter prise server kan ook Azure SQL Database gebruiken.

    - **Enter prise Developer**: Selecteer B2ms VM (met 2 Vcpu's en 8 GB geheugen) met Windows 10 en Visual Studio geïnstalleerd. Deze installatie kopie is beschikbaar op de Azure Marketplace.

3. Voer in de sectie **basis beginselen** uw gebruikers naam en wacht woord in. Selecteer het **abonnement** en de **locatie/regio** die u wilt gebruiken voor de vm's. Selecteer **RGMicroFocusEntServer** voor de resource groep.

4. Plaats beide Vm's in hetzelfde virtuele netwerk zodat ze met elkaar kunnen communiceren.

5. Accepteer de standaard waarden voor de overige instellingen. Onthoud de gebruikers naam en het wacht woord die u voor de beheerder van deze Vm's hebt gemaakt.

6. Wanneer de virtuele machines zijn gemaakt, opent u de binnenkomende poorten 9003, 86 en 80 voor HTTP en 3389 voor RDP op de computer van de Enter prise-server en de 3389 op de ontwikkel computer.

7. Als u zich wilt aanmelden bij de virtuele machine van de Enter prise-server, selecteert u in Azure Portal de ES2 v3 VM. Ga naar de sectie **overzicht** en selecteer **verbinding maken** om een RDP-sessie te starten. Meld u aan met de referenties die u hebt gemaakt voor de virtuele machine.

8. Laad vanuit de RDP-sessie de volgende twee bestanden. Omdat u Windows gebruikt, kunt u de bestanden slepen en neerzetten in de RDP-sessie:

    - **es\_40. exe**, het installatie bestand van de Enter prise-server.

    - **mflic**, het bijbehorende licentie bestand — de Enter prise-server wordt niet geladen zonder dat.

9. Dubbel klik op het bestand om de installatie te starten. Selecteer de installatie locatie in het eerste venster en accepteer de gebruiksrecht overeenkomst.

     ![Micro Focus Enter prise server-installatie scherm](media/01-enterprise-server.png)

     Wanneer het installatie programma is voltooid, wordt het volgende bericht weer gegeven:

     ![Micro Focus Enter prise server-installatie scherm](media/02-enterprise-server.png)

### <a name="check-for-updates"></a>Controleren op updates

Controleer na de installatie of er aanvullende updates zijn, omdat een aantal vereisten, zoals micro soft C++ redistributable en .NET Framework, samen met de Enter prise-server zijn geïnstalleerd.

### <a name="upload-the-license"></a>De licentie uploaden

1. Start het micro focus licentie beheer.

2. Klik op **Start** \> **micro focus licentie beheer** \> **licentie beheer**en klik vervolgens op het tabblad **installeren** . Kies het type licentie-indeling dat u wilt uploaden: een licentie bestand of een licentie code van 16 tekens. Voor een bestand, in **licentie bestand**, bladert u bijvoorbeeld naar het **mflic** -bestand dat u eerder naar de virtuele machine hebt geüpload en selecteert u **Licenties installeren**.

     ![Het dialoog venster micro focus licentie beheer](media/03-enterprise-server.png)

3. Controleer of ENTER prise server wordt geladen. Probeer de Enter prise server-beheer site te starten vanuit een browser <http://localhost:86/>met behulp van deze URL . De pagina Enter prise server-beheer wordt weer gegeven zoals weer gegeven.

     ![Beheer pagina voor Enter prise server](media/04-enterprise-admin.png)

## <a name="install-enterprise-developer-on-the-developer-machine"></a>Enter prise Developer installeren op de ontwikkelaars computer

1. Selecteer de resource groep die u eerder hebt gemaakt (bijvoorbeeld **RGMicroFocusEntServer**) en selecteer vervolgens de installatie kopie voor ontwikkel aars.

2. Als u zich wilt aanmelden bij de virtuele machine, gaat u naar de sectie **overzicht** en selecteert u **verbinding maken**. Met deze aanmelding wordt een RDP-sessie gestart. Meld u aan met de referenties die u hebt gemaakt voor de virtuele machine.

3. Laad vanuit de RDP-sessie de volgende twee bestanden (Sleep en zet deze indien gewenst):

    - **edvs2017. exe**, het installatie bestand van de Enter prise-server.

    - **mflic**, het bijbehorende licentie bestand (Enter prise Developer wordt niet zonder IT) geladen.

4. Dubbel klik op het bestand **edvs2017. exe** om de installatie te starten. Selecteer de installatie locatie in het eerste venster en accepteer de gebruiksrecht overeenkomst. Als u wilt, kiest u **Rumba 9,5 installeren** om deze Terminal Emulator te installeren. dit hebt u waarschijnlijk nodig.

     ![Het dialoog venster micro focus Enter prise Developer voor Visual Studio 2017 Setup](media/04-enterprise-server.png)

5. Nadat de installatie is voltooid, wordt het volgende bericht weer gegeven:

     ![Bericht installatie voltooid](media/05-enterprise-server.png)

6. Start de micro focus-licentie beheer net zoals u dat voor de Enter prise-server hebt gedaan. Kies **licentie beheer** **starten** \> met **micro focus licentie beheer** \> en klik op het tabblad **installeren** .

7. Kies het type licentie-indeling dat u wilt uploaden: een licentie bestand of een licentie code van 16 tekens. Voor een bestand, in **licentie bestand**, bladert u bijvoorbeeld naar het **mflic** -bestand dat u eerder naar de virtuele machine hebt geüpload en selecteert u **Licenties installeren**.

     ![Het dialoog venster micro focus licentie beheer](media/07-enterprise-server.png)

Als Enter prise Developer wordt geladen, is uw implementatie van een micro focus-en test omgeving in azure voltooid.

## <a name="next-steps"></a>Volgende stappen

- [De Bank demo-toepassing instellen](./demo.md)
- [Enter prise server uitvoeren in docker-containers](./run-enterprise-server-container.md)
- [Mainframe-toepassings migratie](/azure/architecture/cloud-adoption/infrastructure/mainframe-migration/application-strategies)
