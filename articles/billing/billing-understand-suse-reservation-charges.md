---
title: Korting op software-plan - Azure | Microsoft Docs
description: Meer informatie over hoe software plan kortingen zijn toegepast op de software op virtuele machines.
documentationcenter: ''
author: yashesvi
manager: yashar
editor: ''
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/12/2019
ms.author: banders
ms.openlocfilehash: bcbf5ab48f3476a911fc4ade1eb0c395fb335d43
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60370221"
---
# <a name="azure-software-plan-discount"></a>Azure-software plan korting

Abonnementen voor Azure-software voor SUSE- en Red Hat zijn reserveringen die betrekking hebben op de geïmplementeerde VM's. De korting voor software-plan wordt toegepast op het softwaregebruik van geïmplementeerde VM's die overeenkomen met de reservering.

Wanneer u een virtuele machine afsluit, wordt automatisch de korting toegepast op een andere overeenkomende virtuele machine, indien beschikbaar. Een softwareabonnement dekt de kosten van het uitvoeren van de software op een virtuele machine. Andere kosten in rekening gebracht, zoals rekentijd, opslag en netwerken worden afzonderlijk in rekening gebracht.

Als u wilt het juiste abonnement kopen, moet u inzicht in uw VM-gebruik en het aantal vcpu's op deze virtuele machines. Gebruik de volgende secties voor het identificeren van wat wilt kopen, op basis van de gebruiksgegevens.

## <a name="how-reservation-discount-is-applied"></a>Hoe reserveringskorting wordt toegepast

Een reserveringskorting is '*gebruik-it-of-verliezen-it*'. Dus als u geen overeenkomende resources voor elk uur, verliest klikt u een reserveringshoeveelheid voor dat uur. U niet uitvoeren voor het doorsturen van niet-gebruikte gereserveerde uur.

Als u een resource wordt afgesloten, past de reserveringskorting automatisch naar een andere overeenkomende resource in het opgegeven bereik. Als er geen overeenkomende bronnen u in het opgegeven bereik vindt, wordt de gereserveerde uren *verloren*.

## <a name="review-redhat-vm-usage-before-you-buy"></a>Bekijk het RedHat VM voor gebruik voordat u aanschaft

Naam van het product van uw gegevens over gebruik verkrijgen en de Red Hat-abonnement kopen met hetzelfde type en grootte.

Bijvoorbeeld, als uw gebruik product heeft **Red Hat Enterprise Linux - 1-4 vCPU, VM-licentie**, u moet aanschaffen **Red Hat Enterprise Linux** voor **1-4 vCPU VM**.

<!--ADD RHEL SCREENSHOT -->

## <a name="review-suse-vm-usage-before-you-buy"></a>Bekijk het SUSE VM voor gebruik voordat u aanschaft

Naam van het product van uw gegevens over gebruik verkrijgen en de SUSE-abonnement kopen met hetzelfde type en grootte.

Bijvoorbeeld, als uw verbruik is voor het product **SUSE Linux Enterprise Server prioriteit - 2-4 vCPU ondersteuning voor virtuele machine**, u moet aanschaffen **SUSE Linux Enterprise Server prioriteit** voor **2-4 vCPU**.

![Voorbeeld van het selecteren van het product te kopen](./media/billing-understand-suse-reservation-charges/select-suse-linux-enterprise-server-priority-2-4-vcpu.png)

## <a name="discount-applies-to-different-vm-sizes-for-suse-plans"></a>Korting is van toepassing op andere VM-grootten voor SUSE-abonnementen

Net als de gereserveerde VM-instanties, SUSE plannen bieden aankopen instantieflexibiliteit grootte. Dit betekent dat uw korting geldt zelfs wanneer u een virtuele machine met een aantal verschillende vCPU's implementeren. De korting is van toepassing op andere VM-grootten in de software-plan.

De korting is afhankelijk van de hoogte-breedteverhouding die worden vermeld in de volgende tabellen. De verhouding tussen vergelijkt de relatieve grootte van elke meter in die groep. De verhouding is afhankelijk van de VM-vcpu's. Gebruik de waarde van de verhouding voor het berekenen van het aantal VM-exemplaren de korting voor SUSE Linux-abonnement ophalen.

Bijvoorbeeld, als u een plan voor SUSE Linux Enterprise Server voor HPC Priority voor een virtuele machine met 3 of 4 vcpu's kopen, is de verhouding voor deze reservering 2. De korting dekt de kosten van de SUSE-software voor:

- 2 virtuele machines geïmplementeerd met 1 of 2 vcpu's,
- 1 virtuele machine geïmplementeerd met 3 of 4 Vcpu,
- of 0.77 of over 77% van een virtuele machine met 5 of meer vcpu's.

De verhouding voor 5 of meer vcpu's is 2.6. Dus een reservering voor SUSE met een virtuele machine met 5 of meer vcpu's bevat informatie over een alleen-gedeelte van de softwarekosten voor, die ongeveer 77% is.

De volgende tabellen worden de software-abonnement die kunt u een reservering voor kopen, hun bijbehorende gebruik meters en de verhouding voor elk weergegeven.

### <a name="suse-linux-enterprise-server-for-hpc-priority"></a>SUSE Linux Enterprise Server voor HPC Priority

Naam van de Azure portal marketplace:

- SLES 12 SP3 voor HPC (prioriteit)

|SUSE VIRTUELE MACHINE | Meter-id| Verhouding| Voorbeeld van de VM-grootte|
| -------| ------------------------| --- |--- |
|SLES voor HPC-1-2-vcpu 's|e275a668-ce79-44e2-a659-f43443265e98|1|D2s_v3|
|SLES voor HPC 3-4 vcpu 's|e531e1c0-09c9-4d83-b7d0-a2c6741faa22|2|D4s_v3|
|SLES voor HPC 5 + vcpu's|4edcd5a5-8510-49a8-a9fc-c9721f501913|2.6|D8s_v3|

### <a name="suse-linux-enterprise-server-for-hpc-standard"></a>SUSE Linux Enterprise Server voor HPC Standard

Naam van de Azure portal marketplace:

- SLES 12 SP3 voor HPC

|SUSE VIRTUELE MACHINE | Meter-id | Verhouding|Voorbeeld van de VM-grootte|
| ------- | --- | ------------------------| --- |
|SLES voor HPC-1-2-vcpu 's |8c94ad45-b93b-4772-aab1-ff92fcec6610|1|D2s_v3|
|SLES voor HPC 3-4 vcpu 's|4ed70d2d-e2bb-4dcd-b6fa-42da71861a1c|1.92308|D4s_v3|
|SLES voor HPC 5 + vcpu's |907a85de-024f-4dd6-969c-347d47a1bdff|2.92308|D8s_v3|

### <a name="suse-linux-enterprise-server-for-sap-priority"></a>SUSE Linux Enterprise Server for SAP Priority

Namen van de Azure portal marketplace:

- SLES for SAP 15 (prioriteit)
- SLES voor SAP-12 SP3 (prioriteit)
- SLES voor SAP-12 SP2 (prioriteit)

|SUSE VIRTUELE MACHINE | Meter-id | Verhouding|Voorbeeld van de VM-grootte|
| ------- |------------------------| --- | --- |
|SLES voor SAP Priority 1-2-vcpu 's|497fe0b6-fa3c-4e3d-a66b-836097244142|1|D2s_v3|
|SLES voor SAP Priority 3-4 vcpu 's |847887de-68ce-4adc-8a33-7a3f4133312f|2|D4s_v3|
|SLES voor SAP Priority 5 + vcpu 's |18ae79cd-dfce-48c9-897b-ebd3053c6058|2.41176|D8s_v3|

### <a name="suse-linux-enterprise-server-priority"></a>SUSE Linux Enterprise Server prioriteit

Namen van de Azure portal marketplace:

- SLES 15 (PRIORITEIT)
- SLES 12 SP3 (prioriteit)
- SLES 11 SP4 (prioriteit)

|SUSE VIRTUELE MACHINE | Meter-id | Verhouding|Voorbeeld van de VM-grootte|
| ------- |------------------------| --- |--- |
|SLES 1 vCPU|462cd632-ec6b-4663-b79f-39715f4e8b38|1|B1ms|
|SLES 2-4 vcpu 's |924bee71-5eb8-424f-83ed-a58823c33908|2|D4s_v3|
|SLES 2-4 vcpu 's |60b3ae9d-e77a-46b2-9cdf-92fa87407969|2|D4s_v3|
|SLES 6 vcpu 's |e8862232-6131-4dbe-bde4-e2ae383afc6f|3||
|SLES 8 vcpu 's |e11331a8-fd32-4e71-b60e-4de2a818c67a|3.2|D8s_v3|
|SLES 12 core vcpu 's |a5afd00d-d3ef-4bcd-8b42-f158b2799782|3.2||
|SLES 16 vcpu 's |bb21066f-fe46-46d3-8006-b326b1663e52|3.2| D16s_v3|
|SLES 20 vcpu 's |c5228804-1de6-4bd4-a61c-501d9003acc8|3.2| |
|SLES 24 kernen vcpu 's |-005d-4075-ac11-822ccde9e8f6|3.2| ND24s|
|SLES 32 vcpu 's |180c1a0a-b0a5-4de3-a032-f92925a4bf90|3.2| D32s_v3|
|SLES 40 kernen vcpu 's |a161d3d3-0592-4956-9b64-6829678b6506|3.2||
|SLES 64 vcpu 's |7f5a36ed-d5b5-4732-b6bb-837dbf0fb9d8|3.2| D64s_v3|
|SLES 72 kernen vcpu 's |93329a72-24d7-4faa-93d9-203f367ed334|3.2|F72s_v2|
|SLES 96 kernen vcpu 's |2018c3a8-ff13-41f8-b64d-9558c5206547|3.2||
|SLES 128 kernen vCPUss |ac27e4d7-44b5-4fee-bc1a-78ac5b4abaf7|3.2| M128ms|

### <a name="suse-linux-enterprise-server-standard"></a>SUSE Linux Enterprise Server Standard

Namen van de Azure portal marketplace:

- SLES 15
- SLES 15 (standaard)
- SLES 12 SP3 (standaard)

|SUSE VIRTUELE MACHINE | Meter-id | Verhouding|Voorbeeld van de VM-grootte|
| ------- |------------------------| --- |--- |
|SLES 1-2 kernen vcpu 's |4b2fecfc-b110-4312-8f9d-807db1cb79ae|1|D2s_v3|
|SLES 3-4 kernen vcpu 's |0c3ebb4c-db7d-4125-b45a-0534764d4bda|1.92308|D4s_v3|
|SLES 5 + vcpu's |7b349b65-d906-42e5-833f-b2af38513468|2.30769| D8s_v3|

## <a name="need-help-contact-us"></a>Hulp nodig? Contact opnemen

Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over reserveringen, de volgende artikelen:

- [Wat zijn Azure-reserveringen?](billing-save-compute-costs-reservations.md)
- [Betaal vooruit voor SUSE-software plannen met Azure-reserveringen](../virtual-machines/linux/prepay-suse-software-charges.md)
- [Vooruitbetalen voor Virtual Machines met Azure Reserved VM Instances](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Azure-reserveringen beheren](billing-manage-reserved-vm-instance.md)
- [Gebruik van de reservering voor uw abonnement op gebruiksbasis begrijpen](billing-understand-reserved-instance-usage.md)
- [Inzicht in gebruik van de reservering voor uw Enterprise-inschrijving](billing-understand-reserved-instance-usage-ea.md)
