---
title: De Azure Backup-agent bijwerken
description: Deze informatie wordt uitgelegd waarom de Azure backup-agent upgraden en het downloaden van de upgrade.
services: backup
cloud: ''
suite: ''
author: markgalioto
manager: carmonm
ms.service: backup
ms.tgt_pltfrm: <optional>
ms.devlang: <optional>
ms.topic: article
ms.date: 8/29/2018
ms.author: markgal
ms.openlocfilehash: 56310b7356dd9e263238234cf3e28bd498fa70fc
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66127823"
---
## <a name="upgrade-the-mars-agent"></a>De MARS-agent bijwerken

Versies van de agent van Microsoft Azure Recovery Service (MARS) hieronder 2.0.9083.0 hebben een afhankelijkheid op de Azure Access Control service (ACS). De MARS-agent wordt ook de Azure Backup agent genoemd. In 2018, Azure [afgeschaft van de Azure Access Control service (ACS)](../articles/active-directory/develop/active-directory-acs-migration.md). Vanaf 19 maart 2018, krijgen alle versies van de MARS-agent hieronder 2.0.9083.0 mislukte back-ups. Om te voorkomen of oplossen van back-upfouten [uw MARS-agent bijwerken naar de meest recente versie](https://go.microsoft.com/fwlink/?linkid=229525). Voor het identificeren van servers waarvoor een upgrade van de MARS-agent, volg de stappen in de [back-up-blog voor het upgraden van agents MARS](https://blogs.technet.microsoft.com/srinathv/2018/01/17/updating-azure-backup-agents/). De MARS-agent wordt gebruikt om back-up van bestanden en mappen en gegevens over de systeemstatus naar Azure. System Center DPM en Azure Backup Server gebruikt de MARS-agent op de back-ups naar Azure.
