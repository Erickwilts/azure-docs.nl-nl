---
title: Lijst weigeren toewijzingen voor Azure-resources met behulp van de Azure portal | Microsoft Docs
description: Leer hoe u de gebruikers, groepen, service-principals en beheerde identiteiten die toegang tot specifieke Azure-resource die aan bepaalde bereiken met behulp van de Azure-portal is geweigerd.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 8078f366-a2c4-4fbb-a44b-fc39fd89df81
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/10/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 21ffb1a2539a2e724a91dd3b2818270a5e573ef8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67127492"
---
# <a name="list-deny-assignments-for-azure-resources-using-the-azure-portal"></a>Lijst weigeren toewijzingen voor Azure-resources met behulp van de Azure portal

[Toewijzingen weigeren](deny-assignments.md) blokkeren dat gebruikers kunnen uitvoeren van acties die specifieke Azure-resource, zelfs als een roltoewijzing deze toegang wordt verleend. In dit artikel wordt beschreven hoe u aan de lijst weigeren toewijzingen met behulp van de Azure portal.

> [!NOTE]
> U kunt geen rechtstreeks maken uw eigen toewijzingen weigeren. Zie voor informatie over hoe u weigeren toewijzingen worden gemaakt, [weigeren toewijzingen](deny-assignments.md).

## <a name="prerequisites"></a>Vereisten

Voor informatie over de toewijzing van een weigeren, moet u het volgende hebben:

- `Microsoft.Authorization/denyAssignments/read` machtiging die is opgenomen in de meeste [ingebouwde rollen voor Azure-resources](built-in-roles.md).

## <a name="list-deny-assignments"></a>Lijst met toewijzingen weigeren

Volg deze stappen om een lijst te weigeren toewijzingen aan het abonnement of beheergroep groepsbereik.

1. Klik in de Azure-portal op **alle services** en vervolgens **beheergroepen** of **abonnementen**.

1. Klik op de beheergroep of het abonnement dat u wilt weergeven.

1. Klik op **toegangsbeheer (IAM)** .

1. Klik op de **weigeren toewijzingen** tabblad (of klik op de **weergave** knop op de weergave weigeren toewijzingen tegel).

    Als er een in dit bereik toewijzingen zijn geweigerd of zijn overgenomen, worden deze weergegeven.

    ![Toegangsbeheer - tabblad toewijzingen weigeren](./media/deny-assignments-portal/access-control-deny-assignments.png)

1. Als extra kolommen weergeven, klikt u op **Edit Columns**.

    ![Kolommen voor toewijzingen - weigeren](./media/deny-assignments-portal/deny-assignments-columns.png)

    |  |  |
    | --- | --- |
    | **Naam** | Naam van de toewijzing weigeren. |
    | **Type principal** | Gebruiker, groep, het systeem gedefinieerde groep of service-principal. |
    | **Geweigerd**  | De naam van de beveiligings-principal die is opgenomen in de toewijzing weigeren. |
    | **Id** | De unieke id voor de toewijzing weigeren. |
    | **Uitgesloten beveiligings-principals** | Of er nog beveiligings-principals die zijn uitgesloten van de toewijzing weigeren. |
    | **Niet van toepassing op onderliggende items** | De toewijzing weigeren is of subscopes overgenomen. |
    | **Systeembeveiliging** | Of de toewijzing weigeren wordt beheerd door Azure. Op dit moment altijd Ja. |
    | **Bereik** | Beheergroep, abonnement, resourcegroep of resource. |

1. Een selectievakje toevoegen aan een van de ingeschakelde items en klik vervolgens op **OK** om weer te geven van de geselecteerde kolommen.

## <a name="list-details-about-a-deny-assignment"></a>Details weergeven over de toewijzing van een weigeren

Volg deze stappen aan de lijst met aanvullende informatie over de toewijzing van een weigeren.

1. Open de **weigeren toewijzingen** deelvenster zoals beschreven in de vorige sectie.

1. Klik op de naam van de toewijzing weigeren openen de **gebruikers** blade.

    ![Weigeren toewijzing - gebruikers](./media/deny-assignments-portal/deny-assignment-users.png)

    De **gebruikers** blade bevat de volgende twee secties.

    |  |  |
    | --- | --- |
    | **Toewijzing is van toepassing op weigeren**  | Beveiligings-principals die de weigeren-toewijzing is van toepassing op. |
    | **Niet van toepassing op toewijzing weigeren** | Beveiligings-principals die zijn uitgesloten van de toewijzing weigeren. |

    **Het systeem gedefinieerde Principal** vertegenwoordigt alle gebruikers, groepen, service-principals en beheerde identiteiten in Azure AD-adreslijst.

1. Voor een lijst met de machtigingen die zijn geweigerd, klikt u op **geweigerd machtigingen**.

    ![Toewijzing - machtigingen geweigerd weigeren](./media/deny-assignments-portal/deny-assignment-denied-permissions.png)

    | Actietype | Description |
    | --- | --- |
    | **Acties**  | Beheerbewerkingen geweigerd. |
    | **NotActions** | Beheerbewerkingen die zijn uitgesloten van de bewerking voor het beheer geweigerd. |
    | **DataActions**  | Gegevensbewerkingen geweigerd. |
    | **NotDataActions** | Gegevensbewerkingen uitgesloten van gegevensbewerking geweigerd. |

    Voor het voorbeeld in de vorige schermafbeelding, zijn de volgende de effectieve machtigingen:

    - Alle opslag-bewerkingen op het vlak van gegevens zijn geweigerd, behalve voor compute-bewerkingen.

1. Voor de eigenschappen voor de toewijzing van een weigeren, klikt u op **eigenschappen**.

    ![Eigenschappen van toewijzing - weigeren](./media/deny-assignments-portal/deny-assignment-properties.png)

    Op de **eigenschappen** blade ziet u de naam van de roltoewijzing weigeren, -ID, beschrijving en bereik. De **is niet van toepassing op onderliggende** switch geeft aan of de toewijzing weigeren aan subscopes wordt overgenomen. De **systeembeveiliging** switch geeft aan of dit weigeren toewijzing wordt beheerd door Azure. Dit is momenteel **Ja** in alle gevallen.

## <a name="next-steps"></a>Volgende stappen

* [Informatie over toewijzingen voor Azure-resources te weigeren](deny-assignments.md)
* [Lijst met toewijzingen voor Azure-resources met behulp van Azure PowerShell weigeren](deny-assignments-powershell.md)
