---
title: Attestation-enclaves in azure
description: Meer informatie over hoe u Attestation kunt gebruiken om te controleren of uw vertrouwelijk computing-vertrouwde omgeving veilig is
services: virtual-machines
author: JBCook
ms.service: virtual-machines
ms.subservice: workloads
ms.workload: infrastructure
ms.topic: conceptual
ms.date: 9/22/2020
ms.author: JenCook
ms.openlocfilehash: 86856d1f66b5b7d723c907c17d7179ffcd2b82ab
ms.sourcegitcommit: 04fb3a2b272d4bbc43de5b4dbceda9d4c9701310
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/12/2020
ms.locfileid: "94565667"
---
# <a name="attesting-sgx-enclaves"></a>Attestatie van SGX enclaves

Vertrouwelijk Computing op Azure biedt virtuele machines van Intel SGX die een deel van uw code of gegevens kunnen isoleren. Wanneer u met deze [enclaves](confidential-computing-enclaves.md)werkt, moet u verificatie en validatie ontvangen die uw vertrouwde omgeving veilig is. Deze verificatie is het proces van Attestation. 

## <a name="overview"></a>Overzicht 

Met Attestation kan een Relying Party meer vertrouwen krijgen dat hun software (1) wordt uitgevoerd in een enclave en (2) dat de enclave up-to-date en beveiligd is. Een enclave vraagt bijvoorbeeld de onderliggende hardware om een referentie te genereren die het bewijs levert dat de enclave op het platform bestaat. Het rapport kan vervolgens worden doorgegeven aan een tweede enclave die controleert of het rapport op hetzelfde platform is gegenereerd.

![attest code in de enclave](media/attestation/attestation.png)



Attestation moet worden geïmplementeerd met behulp van een beveiligde attestation-service die compatibel is met de systeemsoftware en silicon. Enkele voor beelden van services die u kunt gebruiken zijn

- [Microsoft Azure Attestation (preview)](../attestation/overview.md) of
- [Intel Attestation-en inrichtings Services](https://software.intel.com/sgx/attestation-services)


Dit zijn allemaal compatibel met de Intel SGX-infra structuur van Azure vertrouwelijk computing. 

## <a name="next-steps"></a>Volgende stappen
Probeer de [Microsoft Azure Attestation-voor beelden voor enclave-compatibele apps](/samples/azure-samples/microsoft-azure-attestation/sample-code-for-intel-sgx-attestation-using-microsoft-azure-attestation/).