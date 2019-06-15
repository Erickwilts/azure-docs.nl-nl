---
title: Upgrade van het Collector-apparaat in Azure Migrate | Microsoft Docs
description: Bevat informatie over upgrades voor het Azure Migrate Collector-apparaat.
author: musa-57
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 03/29/2019
ms.author: hamusa
services: azure-migrate
ms.openlocfilehash: 7cd44318716200d665ece9ffecc45225bdfb85eb
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60685916"
---
# <a name="collector-appliance-updates"></a>Updates van collector-apparaat

In dit artikel bevat een overzicht van de upgrade-informatie voor de Collector-apparaat in [Azure Migrate](migrate-overview.md).

De Azure Migrate Collector is een lichtgewicht apparaat dat wordt gebruikt voor het detecteren van een on-premises vCenter-omgeving voor evaluatiedoeleinden vóór de migratie naar Azure. [Meer informatie](concepts-collector.md).

## <a name="how-to-upgrade-the-appliance"></a>Upgrade uitvoeren van het apparaat

U kunt de Collector upgraden naar de meest recente versie zonder het ova-bestand opnieuw te downloaden.

1. Sluit alle browservensters en eventuele open bestanden/mappen in het apparaat.
2. Download de meest recente pakket uit de lijst met updates die hieronder worden vermeld in dit artikel.
3. Om ervoor te zorgen dat het gedownloade pakket beveiligd is, open opdrachtvenster voor beheerders en voer de volgende opdracht voor het genereren van de hash voor het ZIP-bestand. De gegenereerde hash moet overeenkomen met de hash op basis van de specifieke versie vermeld:

    ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```

    Voorbeeld: **C:\>CertUtil -HashFile C:\AzureMigrate\CollectorUpdate_release_1.0.9.14.zip SHA256)**
4. Kopieer het zip-bestand naar de virtuele machine van de Collector-apparaat.
5. Met de rechtermuisknop op het zip-bestand > **Alles uitpakken**.
6. Met de rechtermuisknop op **Setup.ps1** > **uitvoeren met PowerShell**, en volgt u de installatie-instructies.

## <a name="collector-update-release-history"></a>Releasegeschiedenis van collector bijwerken

### <a name="continuous-discovery-upgrade-versions"></a>Continue detectie: Upgrade-versies

#### <a name="version-101014-released-on-03292019"></a>Versie 1.0.10.14 (uitgebracht op 29-03/2019)

Er zijn enkele verbeteringen van de gebruikersinterface bevat.

Hash-waarden voor de upgrade [1.0.10.14 pakket](https://aka.ms/migrate/col/upgrade_10_14)

**Algoritme** | **Hash-waarde**
--- | ---
MD5 | 846b1eb29ef2806bcf388d10519d78e6
SHA1 | 6243239fa49c6b3f5305f77e9fd4426a392d33a0
SHA256 | fb058205c945a83cc4a31842b9377428ff79b08247f3fb8bb4ff30c125aa47ad

#### <a name="version-101012-released-on-03132019"></a>Versie 1.0.10.12 (uitgebracht op 03/13/2019)

Bevat oplossingen voor problemen bij het selecteren van Azure in de cloud in het apparaat.

Hash-waarden voor de upgrade [1.0.10.12 pakket](https://aka.ms/migrate/col/upgrade_10_12)

**Algoritme** | **Hash-waarde**
--- | ---
MD5 | 27704154082344c058238000dff9ae44
SHA1 | 41e9e2fb71a8dac14d64f91f0fd780e0d606785e
SHA256 | c6e7504fcda46908b636bfe25b8c73f067e3465b748f77e50027e66f2727c2a9

### <a name="one-time-discovery-deprecated-now-previous-upgrade-versions"></a>Eenmalige discovery (nu afgeschaft): Vorige versies van de upgrade

> [!NOTE]
> Ondersteuning van het apparaat voor eenmalige detectie is nu beëindigd omdat deze methode gebaseerd was op statistiekinstellingen van vCenter Server voor de beschikbaarheid van prestatiegegevenspunten en gemiddelde prestatiemeteritems verzamelde, wat leidde tot een te voorzichtige schaling van virtuele machines voor migratie naar Azure.

#### <a name="version-10916-released-on-10292018"></a>Versie 1.0.9.16 (uitgebracht op 29-10-2018)

Bevat oplossingen voor problemen bij het instellen van het toestel PowerCLI.

Hash-waarden voor de upgrade [1.0.9.16 pakket](https://aka.ms/migrate/col/upgrade_9_16)

**Algoritme** | **Hash-waarde**
--- | ---
MD5 | d2c53f683b0ec7aaf5ba3d532a7382e1
SHA1 | e5f922a725d81026fa113b0c27da185911942a01
SHA256 | a159063ff508e86b4b3b7b9a42d724262ec0f2315bdba8418bce95d973f80cfc

#### <a name="version-10914"></a>Versie 1.0.9.14

Hash-waarden voor de upgrade [1.0.9.14 pakket](https://aka.ms/migrate/col/upgrade_9_14)

**Algoritme** | **Hash-waarde**
--- | ---
MD5 | c5bf029e9fac682c6b85078a61c5c79c
SHA1 | af66656951105e42680dfcc3ec3abd3f4da8fdec
SHA256 | 58b685b2707f273aa76f2e1d45f97b0543a8c4d017cd27f0bdb220e6984cc90e

#### <a name="version-10913"></a>Versie 1.0.9.13

Hash-waarden voor de upgrade [1.0.9.13 pakket](https://aka.ms/migrate/col/upgrade_9_13)

**Algoritme** | **Hash-waarde**
--- | ---
MD5 | 739f588fe7fb95ce2a9b6b4d0bf9917e
SHA1 | 9b3365acad038eb1c62ca2b2de1467cb8eed37f6
SHA256 | 7a49fb8286595f39a29085534f29a623ec2edb12a3d76f90c9654b2f69eef87e


## <a name="next-steps"></a>Volgende stappen

- [Meer informatie](concepts-collector.md) over het Collector-apparaat.
- [Voer een evaluatie uit](tutorial-assessment-vmware.md) voor VMware-VM's.
