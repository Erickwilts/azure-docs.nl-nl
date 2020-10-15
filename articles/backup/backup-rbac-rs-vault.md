---
title: Back-ups beheren met op rollen gebaseerd toegangs beheer van Azure
description: Gebruik Azure op rollen gebaseerd toegangs beheer voor het beheren van de toegang tot back-upbeheer bewerkingen in Recovery Services kluis.
ms.reviewer: utraghuv
ms.topic: conceptual
ms.date: 06/24/2019
ms.openlocfilehash: 0dd8d08c4ee79082f47929cf7d453f3f4bbd60ee
ms.sourcegitcommit: 30505c01d43ef71dac08138a960903c2b53f2499
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/15/2020
ms.locfileid: "92090876"
---
# <a name="use-azure-role-based-access-control-to-manage-azure-backup-recovery-points"></a>Op rollen gebaseerd toegangs beheer van Azure gebruiken voor het beheren van Azure Backup herstel punten

Met op rollen gebaseerd toegangs beheer op basis van Azure (Azure RBAC) hebt u verfijnd toegang tot Azure. Met op rollen gebaseerd toegangsbeheer van Azure kunt u taken scheiden binnen uw team en alleen de mate van toegang verlenen aan gebruikers die nodig is om de taken uit te voeren.

> [!IMPORTANT]
> Rollen die door Azure Backup worden verschaft, zijn beperkt tot acties die kunnen worden uitgevoerd in Azure Portal of via REST API of Recovery Services kluis Power shell of CLI-cmdlets. Acties die worden uitgevoerd in de gebruikers interface van de Azure Backup Agent of System Center Data Protection Manager gebruikers interface Azure Backup Server of de gebruikers interface zijn niet van invloed op deze rollen.

Azure Backup biedt drie ingebouwde rollen voor het beheren van de bewerkingen voor back-upbeheer. Meer informatie over [ingebouwde rollen van Azure](../role-based-access-control/built-in-roles.md)

* [Back-upinzender](../role-based-access-control/built-in-roles.md#backup-contributor) : deze rol heeft alle machtigingen voor het maken en beheren van back-ups, behalve het verwijderen van Recovery Services kluis en het verlenen van toegang aan anderen. Stel deze rol in als beheerder van het back-upbeheer, die elke bewerking van het back-upbeheer kan uitvoeren.
* [Back-upoperator](../role-based-access-control/built-in-roles.md#backup-operator) -deze rol heeft machtigingen voor alles wat een bijdrager heeft, behalve het verwijderen van back-ups en het beheer van back-upbeleid. Deze rol is gelijk aan Inzender, behalve dat er geen destructieve bewerkingen kunnen worden uitgevoerd, zoals het stoppen van back-ups met gegevens verwijderen of het verwijderen van de registratie van on-premises resources.
* [Back-uplezer](../role-based-access-control/built-in-roles.md#backup-reader) : deze rol heeft machtigingen voor het weer geven van alle bewerkingen voor back-upbeheer. Stel dat deze rol een bewakings persoon is.

Als u uw eigen rollen wilt definiëren voor nog meer controle, raadpleegt u [aangepaste rollen bouwen](../role-based-access-control/custom-roles.md) in azure RBAC.

## <a name="mapping-backup-built-in-roles-to-backup-management-actions"></a>Ingebouwde functies voor back-ups koppelen aan back-upbeheer acties

In de volgende tabel worden de acties voor back-upbeheer en de bijbehorende minimale Azure-rol vastgelegd die is vereist om deze bewerking uit te voeren.

| Beheer bewerking | Mini maal vereiste Azure-rol | Bereik vereist |
| --- | --- | --- |
| Een Recovery Services-kluis maken | Back-upinzender | Resource groep met de kluis |
| Back-ups van virtuele Azure-machines inschakelen | Back-upoperator | Resource groep met de kluis |
| | Inzender voor virtuele machines | VM-resource |
| Back-up op aanvraag van de virtuele machine | Back-upoperator | Recovery Services-kluis |
| VM herstellen | Back-upoperator | Recovery Services-kluis |
| | Inzender | De resource groep waarin de VM wordt geïmplementeerd |
| | Inzender voor virtuele machines | Bron-VM waarvan een back-up is gemaakt |
| Back-up van niet-beheerde schijven herstellen | Back-upoperator | Recovery Services-kluis |
| | Inzender voor virtuele machines | Bron-VM waarvan een back-up is gemaakt |
| | Inzender voor opslagaccounts | Bron van het opslag account waar de schijven zullen worden teruggezet |
| Beheerde schijven terugzetten vanuit een back-up van de VM | Back-upoperator | Recovery Services-kluis |
| | Inzender voor virtuele machines | Bron-VM waarvan een back-up is gemaakt |
| | Inzender voor opslagaccounts | Het tijdelijke opslag account dat is geselecteerd als onderdeel van de herstel bewerking om gegevens van de kluis te bewaren voordat deze naar beheerde schijven worden geconverteerd |
| | Inzender | De resource groep waarvoor beheerde schijven worden hersteld |
| Afzonderlijke bestanden herstellen vanuit een back-up van de VM | Back-upoperator | Recovery Services-kluis |
| | Inzender voor virtuele machines | Bron-VM waarvan een back-up is gemaakt |
| Back-upbeleid voor Azure VM-back-up maken | Back-upinzender | Recovery Services-kluis |
| Back-upbeleid van Azure VM-back-up wijzigen | Back-upinzender | Recovery Services-kluis |
| Back-upbeleid van Azure VM-back-up verwijderen | Back-upinzender | Recovery Services-kluis |
| Back-up stoppen (met gegevens behouden of gegevens verwijderen) bij een back-up van de VM | Back-upinzender | Recovery Services-kluis |
| On-premises Windows Server/client/SCDPM of Azure Backup Server registreren | Back-upoperator | Recovery Services-kluis |
| Geregistreerde on-premises Windows Server/client-SCDPM of Azure Backup Server verwijderen | Back-upinzender | Recovery Services-kluis |

> [!IMPORTANT]
> Als u de rol van de VM in een VM-bron bereik opgeeft en **back-up** selecteert als onderdeel van de VM-instellingen, wordt het scherm **back-up inschakelen** geopend, zelfs als er al een back-up van de virtuele machine wordt gemaakt. Dit komt doordat de aanroep om de status van de back-up te controleren alleen op abonnements niveau werkt. Om dit te voor komen, gaat u naar de kluis en opent u de weer gave back-upitem van de virtuele machine of geeft u de rol van de VM-bijdrage op abonnements niveau op.

## <a name="minimum-role-requirements-for-the-azure-file-share-backup"></a>Minimale functie vereisten voor de back-up van de Azure-bestands share

In de volgende tabel worden de acties voor back-upbeheer en de bijbehorende rol vastgelegd die vereist zijn voor het uitvoeren van een Azure file share-bewerking.

| Beheer bewerking | Rol vereist | Resources |
| --- | --- | --- |
| Back-ups van Azure-bestands shares inschakelen | Back-upinzender |Recovery Services-kluis |
| |Opslagaccount | Resource voor het opslag account Inzender |
| Back-up op aanvraag van de virtuele machine | Back-upoperator | Recovery Services-kluis |
| Bestands share herstellen | Back-upoperator | Recovery Services-kluis |
| | Inzender voor opslagaccounts | Opslag account resources waar bron-en doel bestands shares voor herstellen aanwezig zijn |
| Afzonderlijke bestanden herstellen | Back-upoperator | Recovery Services-kluis |
| |Inzender voor opslagaccounts|Opslag account resources waar bron-en doel bestands shares voor herstellen aanwezig zijn |
| Beveiliging stoppen |Back-upinzender | Recovery Services-kluis |
| De registratie van het opslag account bij de kluis ongedaan maken |Back-upinzender | Recovery Services-kluis |
| |Inzender voor opslagaccounts | Bron van opslag account|

## <a name="next-steps"></a>Volgende stappen

* [Azure op rollen gebaseerd toegangs beheer (Azure RBAC)](../role-based-access-control/role-assignments-portal.md): aan de slag met Azure RBAC in de Azure Portal.
* Meer informatie over het beheren van toegang met:
  * [PowerShell](../role-based-access-control/role-assignments-powershell.md)
  * [Azure-CLI](../role-based-access-control/role-assignments-cli.md)
  * [REST API](../role-based-access-control/role-assignments-rest.md)
* [Probleem oplossing voor toegangs beheer op basis van rollen in azure](../role-based-access-control/troubleshooting.md): suggesties ophalen voor het oplossen van veelvoorkomende problemen.
