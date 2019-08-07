---
title: De Azure VMware-oplossing verkleinen door de CloudSimple-Privécloud
description: Hierin wordt beschreven hoe u een CloudSimple-Privécloud verkleint.
author: sharaths-cs
ms.author: b-shsury
ms.date: 07/01/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 67bb7e0cd8a800c2c2ba87c768739fd573ef6888
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68812203"
---
# <a name="shrink-a-cloudsimple-private-cloud"></a>Een CloudSimple-Privécloud verkleinen

CloudSimple biedt de flexibiliteit voor het dynamisch verkleinen van een Privécloud.  Een Privécloud bestaat uit een of meer vSphere-clusters. Elk cluster kan 3 tot 16 knoop punten bevatten. Bij het verkleinen van een Privécloud verwijdert u een knoop punt uit het bestaande cluster of verwijdert u een volledig cluster. 

## <a name="before-you-begin"></a>Voordat u begint

Voor het verkleinen van een Privécloud moet aan de volgende voor waarden worden voldaan.  Het beheer cluster (eerste cluster) dat is gemaakt tijdens het maken van een Privécloud, kan niet worden verwijderd.

* Een vSphere-cluster moet drie knoop punten hebben.  Een cluster met slechts drie knoop punten kan niet worden verkleind.
* De totale capaciteit die wordt verbruikt, mag niet groter zijn dan het aantal te verkleinen van het cluster. 

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij de Azure Portal op [https://portal.azure.com](https://portal.azure.com).

## <a name="shrink-a-private-cloud"></a>Een Privécloud verkleinen 

1. [Toegang tot de CloudSimple-Portal](access-cloudsimple-portal.md).

2. Open de pagina **resources** .

3. Klik op de Privécloud die u wilt verkleinen

4. Klik op de pagina samen vatting op **verkleinen**.

    ![Persoonlijke Cloud verkleinen](media/shrink-private-cloud.png)

5. Selecteer het cluster dat u wilt verkleinen of verwijderen. 

    ![Private Cloud verkleinen-cluster selecteren](media/shrink-private-cloud-select-cluster.png)

6. Selecteer **een knoop punt verwijderen** of **Verwijder het hele cluster**. 

7. De cluster capaciteit controleren

8. Klik op **verzenden** om de privécloud te verkleinen.

Het verkleinen van de Privécloud wordt gestart.  U kunt de voortgang in taken bewaken.  Het verkleinings proces kan enkele uren duren, afhankelijk van de gegevens, die opnieuw moeten worden gesynchroniseerd op vSAN.

## <a name="next-steps"></a>Volgende stappen

* [VMware-Vm's in azure gebruiken](quickstart-create-vmware-virtual-machine.md)
* Meer informatie over [persoonlijke Clouds](cloudsimple-private-cloud.md)