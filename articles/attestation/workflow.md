---
title: Azure Attestation-werkstroom
description: De werkstroom van Azure Attestation.
services: attestation
author: msmbaldwin
ms.service: attestation
ms.topic: overview
ms.date: 08/31/2020
ms.author: mbaldwin
ms.openlocfilehash: 240b27f897d8e7a34026701cf7fdc844eb9d4086
ms.sourcegitcommit: d68c72e120bdd610bb6304dad503d3ea89a1f0f7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/01/2020
ms.locfileid: "89236963"
---
# <a name="workflow"></a>Werkstroom

Microsoft Azure Attestation ontvangt bewijs van enclavess en evalueert de gegevens op basis van de basislijn van de Azure-beveiliging en configureerbaar beleid. Bij een geslaagde verificatie genereert Azure Attestation een Attestation-token om de betrouwbaarheid van de enclave te bevestigen.

De volgende actoren zijn betrokken bij een Azure Attestation-werkstroom:

- **Relying party**: Het onderdeel dat afhankelijk is van Azure Attestation om de geldigheid van de enclave te controleren. 
- **Client**: Het onderdeel dat gegevens van een enclave verzamelt en aanvragen naar Azure Attestation verzendt. 
- **Azure Attestation**: Het onderdeel dat enclavebewijs van de client accepteert, valideert en het Attestation-token naar de client retourneert


## <a name="enclave-validation-work-flow"></a>Werkstroom voor enclave-validatie

Dit zijn de algemene stappen in een typische attestation-werkstroom voor een SGX-enclave (met Azure Attestation):

1. Client verzamelt bewijs van een enclave. Bewijs is informatie over de enclave-omgeving en de clientbibliotheek die in de enclave wordt uitgevoerd.
1. De client heeft een URL die verwijst naar een exemplaar van Azure Attestation. De client wordt geverifieerd bij Azure AD en krijgt een toegangstoken.
1. De client verzendt bewijs naar Azure Attestation samen met het toegangstoken. De exacte gegevens die naar de provider worden verzonden, zijn afhankelijk van het type enclave.
1. Azure Attestation valideert de verzonden informatie en evalueert deze op basis van het geconfigureerde beleid. Als de verificatie slaagt, wordt door Azure Attestation een Attestation-token uitgegeven en geretourneerd naar de client. Als deze stap mislukt, meldt Azure Attestation een fout aan de client. 
1. De client verzendt het Attestation-token naar Relying Party. Relying Party roept het eindpunt van de metagegevens van de openbare sleutel aan om handtekeningcertificaten op te halen. Relying Party verifieert vervolgens de handtekening van het Attestation-token en controleert de betrouwbaarheid van de enclave. 

![Validatiestroom voor enclave](./media/validation-flow.png)


## <a name="next-steps"></a>Volgende stappen
- [Een Attestation-beleid ontwerpen en ondertekenen](author-sign-policy.md)
- [Azure Attestation instellen met behulp van PowerShell](quickstart-powershell.md)
