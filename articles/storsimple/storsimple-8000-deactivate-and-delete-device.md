---
title: Deactiveren en verwijderen van een StorSimple 8000-apparaat | Microsoft Docs
description: Beschrijft hoe u StorSimple-apparaat verwijdert uit de service door het eerst deactiveren en vervolgens te verwijderen.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/23/2018
ms.author: alkohli
ms.openlocfilehash: 116ac5c4efda87b5d16336dd326d516299f6955d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "61481942"
---
# <a name="deactivate-and-delete-a-storsimple-device"></a>Een StorSimple-apparaat deactiveren en verwijderen

## <a name="overview"></a>Overzicht

In dit artikel wordt beschreven hoe u deactiveren en verwijderen van een StorSimple-apparaat dat is verbonden met een StorSimple Device Manager-service. De instructies in dit artikel geldt alleen voor apparaten uit StorSimple 8000-serie met inbegrip van de StorSimple Cloud Appliances. Als u van een StorSimple Virtual Array gebruikmaakt, ga dan naar [deactiveren en verwijderen van een StorSimple Virtual Array](storsimple-virtual-array-deactivate-and-delete-device.md).

Deactivering verbreekt de verbinding tussen het apparaat en de bijbehorende StorSimple Device Manager-service. U kunt desgewenst een StorSimple-apparaat buiten gebruik gesteld nemen (bijvoorbeeld, als u wilt vervangen of bijwerken van uw apparaat of als u niet langer StorSimple gebruikt). Als dit het geval is, moet u het apparaat deactiveren voordat u het kunt verwijderen.

Wanneer u een apparaat deactiveert, is alle gegevens die lokaal is opgeslagen op het apparaat niet meer toegankelijk. Alleen de gegevens die zijn gekoppeld aan het apparaat dat is opgeslagen in de cloud kunnen worden hersteld.

> [!WARNING]
> Deactivering is een permanente bewerking en kan niet ongedaan worden gemaakt. Een gedeactiveerd apparaat kan niet worden geregistreerd bij de StorSimple Device Manager-service, tenzij deze is opnieuw ingesteld naar de fabrieksinstellingen.
>
> De fabrieksinstellingen terugzetten proces verwijdert alle gegevens die lokaal zijn opgeslagen op uw apparaat. Daarom moet u een cloud-momentopname van al uw gegevens uitvoeren voordat u een apparaat deactiveren. Deze cloudmomentopname kunt u alle gegevens op een later tijdstip te herstellen.

Na het lezen van deze zelfstudie, kunt u zich aan:

* Een apparaat deactiveren en verwijderen van de gegevens.
* Een apparaat deactiveren en bewaar de gegevens.

> [!NOTE]
> Voordat u een fysieke StorSimple-apparaat of een cloudapparaat deactiveert, beëindigt of verwijdert u clients en hosts die afhankelijk van dat apparaat zijn.


## <a name="deactivate-and-delete-data"></a>Deactiveren en verwijderen van gegevens

Als u geïnteresseerd bent in het apparaat volledig verwijderen en niet wilt behouden van de gegevens op het apparaat, klikt u vervolgens de volgende stappen te voltooien.

#### <a name="to-deactivate-the-device-and-delete-the-data"></a>Het apparaat deactiveren en verwijderen van de gegevens

1. Voordat u een apparaat deactiveert, moet u alle volume verwijderen containers (en de volumes) die zijn gekoppeld aan het apparaat. Nadat u de gekoppelde back-ups hebt verwijderd, kunt u volumecontainers verwijderen.

    > [!NOTE]
    > Voordat u een fysieke StorSimple-apparaat of een cloudapparaat deactiveert, zorg ervoor dat de gegevens van de verwijderde volumecontainer daadwerkelijk is verwijderd van het apparaat. U kunt de grafieken van de verbruik cloud bewaken en wanneer u ziet dat het gebruik van de cloud verwijderen vanwege de back-ups die u hebt verwijderd, klikt u vervolgens kunt u doorgaan met het apparaat deactiveren. Als u het apparaat deactiveren voordat deze vervolgkeuzelijst optreedt, worden de gegevens in het opslagaccount is overkomt en kosten toenemen.

2. Als volgt te werk om het apparaat te deactiveren:
   
   1. Ga naar de StorSimple-apparaatbeheerservice en klik op **Apparaten**. In de **apparaten** blade, selecteer het apparaat dat u uitschakelen wilt, klik met de rechtermuisknop en klik vervolgens op **deactiveren**.

        ![StorSimple-apparaat deactiveren](./media/storsimple-8000-deactivate-and-delete-device/deactivate1.png)
   2. In de **deactiveren** blade, type de naam van het apparaat om te bevestigen en klik vervolgens op **deactiveren**. Het deactiveren-proces wordt gestart en duurt een paar minuten om te voltooien.

        ![StorSimple-apparaat deactiveren](./media/storsimple-8000-deactivate-and-delete-device/deactivate2.png)

3. Na de deactivering, kunt u het apparaat volledig verwijderen. Als u een apparaat verwijdert, wordt deze verwijderd uit de lijst met apparaten die zijn verbonden met de service. De service kan niet meer vervolgens het verwijderde apparaat beheren. Gebruik de volgende stappen uit om te verwijderen van het apparaat:
   
   1. Ga naar de StorSimple-apparaatbeheerservice en klik op **Apparaten**. In de **apparaten** blade, selecteer het gedeactiveerde apparaat dat u verwijderen wilt, klik met de rechtermuisknop en klik vervolgens op **verwijderen**.

        ![StorSimple-apparaat deactiveren](./media/storsimple-8000-deactivate-and-delete-device/deactivate5.png)
   2. In de **verwijderen** blade, type de naam van het apparaat om te bevestigen en klik vervolgens op **verwijderen**. De verwijdering duurt een paar minuten om te voltooien.

        ![StorSimple-apparaat deactiveren](./media/storsimple-8000-deactivate-and-delete-device/deactivate6.png)
   3. Nadat de verwijdering is voltooid is, wordt u gewaarschuwd. De lijst met apparaten wordt ook bijgewerkt om de verwijdering weer te geven.

## <a name="deactivate-and-retain-data"></a>Deactiveren en behoud van gegevens

Als u geïnteresseerd bent in bij het verwijderen van het apparaat, maar de gegevens wilt bewaren, voert u de volgende stappen uit:

#### <a name="to-deactivate-a-device-and-retain-the-data"></a>Op een apparaat deactiveren en de gegevens bewaren
1. Het apparaat deactiveren. Alle volumecontainers en de momentopnamen van het apparaat blijven.
   
   1. Ga naar de StorSimple-apparaatbeheerservice en klik op **Apparaten**. In de **apparaten** blade, selecteer het apparaat dat u uitschakelen wilt, klik met de rechtermuisknop en klik vervolgens op **deactiveren**.

         ![StorSimple-apparaat deactiveren](./media/storsimple-8000-deactivate-and-delete-device/deactivate1.png)
   2. In de **deactiveren** blade, type de naam van het apparaat om te bevestigen en klik vervolgens op **deactiveren**. Het deactiveren-proces wordt gestart en duurt een paar minuten om te voltooien.

         ![StorSimple-apparaat deactiveren](./media/storsimple-8000-deactivate-and-delete-device/deactivate2.png)
2. U kunt nu de volumecontainers en de bijbehorende momentopnamen failover. Voor procedures gaat u naar [Failover en herstel na noodgevallen voor uw StorSimple-apparaat](storsimple-8000-device-failover-disaster-recovery.md).
3. Na het deactiveren en failover, kunt u het apparaat volledig verwijderen. Als u een apparaat verwijdert, wordt deze verwijderd uit de lijst met apparaten die zijn verbonden met de service. De service kan niet meer vervolgens het verwijderde apparaat beheren. Als het apparaat verwijderen, voert u de volgende stappen uit:
   
   1. Ga naar de StorSimple-apparaatbeheerservice en klik op **Apparaten**. In de **apparaten** blade, selecteer het gedeactiveerde apparaat dat u verwijderen wilt, klik met de rechtermuisknop en klik vervolgens op **verwijderen**.

       ![StorSimple-apparaat deactiveren](./media/storsimple-8000-deactivate-and-delete-device/deactivate5.png)
   2. In de **verwijderen** blade, type de naam van het apparaat om te bevestigen en klik vervolgens op **verwijderen**. De verwijdering duurt een paar minuten om te voltooien.

       ![StorSimple-apparaat deactiveren](./media/storsimple-8000-deactivate-and-delete-device/deactivate6.png)
   3. Nadat de verwijdering is voltooid is, wordt u gewaarschuwd. De lijst met apparaten wordt ook bijgewerkt om de verwijdering weer te geven.

     
## <a name="deactivate-and-delete-a-cloud-appliance"></a>Deactiveren en verwijderen van een cloudapparaat

Voor een StorSimple-Cloudapparaat deactiveren vanuit de portal wordt de toewijzing ingetrokken en Hiermee verwijdert u de virtuele machine en de resources die zijn gemaakt tijdens het inrichten. Wanneer het cloudapparaat is gedeactiveerd, kan het niet meer worden hersteld naar de oorspronkelijke staat.

![StorSimple-Cloudapparaat deactiveren](./media/storsimple-8000-deactivate-and-delete-device/deactivate7.png)

Deactivering resultaten in de volgende acties:

* De StorSimple-Cloudapparaat wordt verwijderd uit de service.
* De virtuele machine voor de StorSimple-Cloudapparaat wordt verwijderd.
* De besturingssysteemschijf en gegevensschijven die zijn gemaakt voor het StorSimple-Cloudapparaat worden bewaard. Als u deze entiteiten niet gebruikt, moet u ze handmatig verwijderen.
* De gehoste service en virtuele netwerken die zijn gemaakt tijdens de inrichting worden bewaard. Als u deze entiteiten niet gebruikt, moet u ze handmatig verwijderen.
* Cloudmomentopnamen die zijn gemaakt door de StorSimple-Cloudapparaat worden bewaard.

Nadat het cloudapparaat is gedeactiveerd, kunt u deze verwijderen uit de lijst met apparaten. Selecteer het gedeactiveerde apparaat, met de rechtermuisknop op en klik vervolgens op **verwijderen**. StorSimple waarschuwt u wanneer het apparaat is verwijderd en de lijst met apparaten updates.

## <a name="next-steps"></a>Volgende stappen

* Als u het gedeactiveerde apparaat naar de fabrieksinstellingen herstellen, gaat u naar [het apparaat terugzetten op fabrieksinstellingen](storsimple-8000-manage-device-controller.md#reset-the-device-to-factory-default-settings).
* Voor technische ondersteuning, [Neem contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md).
* Voor meer informatie over het gebruik van de StorSimple Device Manager-service, gaat u naar [de StorSimple Device Manager-service gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-8000-manager-service-administration.md).

