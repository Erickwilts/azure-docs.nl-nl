---
title: Access-controle logboeken-Azure Portal-Azure Database for MySQL
description: In dit artikel wordt beschreven hoe u de audit Logboeken in Azure Database for MySQL kunt configureren en openen vanuit de Azure Portal.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: how-to
ms.date: 9/29/2020
ms.openlocfilehash: c406fa6b49e800912edb5738b4d60596d828fc94
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "91570501"
---
# <a name="configure-and-access-audit-logs-for-azure-database-for-mysql-in-the-azure-portal"></a>Controle logboeken voor Azure Database for MySQL in het Azure Portal configureren en openen

U kunt de [Azure database for MySQL controle logboeken](concepts-audit-logs.md) en diagnostische instellingen van de Azure Portal configureren.

## <a name="prerequisites"></a>Vereisten

Als u deze hand leiding wilt door lopen, hebt u het volgende nodig:

- [Azure Database for MySQL server](quickstart-create-mysql-server-database-using-azure-portal.md)

## <a name="configure-audit-logging"></a>Controle logboek registratie configureren

>[!IMPORTANT]
> Het is raadzaam om alleen de gebeurtenis typen en gebruikers die vereist zijn voor controle doeleinden te registreren om ervoor te zorgen dat de prestaties van uw server niet sterk worden beïnvloed.

Controle logboek registratie inschakelen en configureren.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

1. Selecteer uw Azure Database for MySQL-server.

1. Selecteer in de sectie **instellingen** van de zijbalk **server parameters**.
    :::image type="content" source="./media/howto-configure-audit-logs-portal/server-parameters.png" alt-text="Serverparameters":::

1. Werk de para meter **audit_log_enabled** bij naar aan.
    :::image type="content" source="./media/howto-configure-audit-logs-portal/audit-log-enabled.png" alt-text="Serverparameters":::

1. Selecteer de [gebeurtenis typen](concepts-audit-logs.md#configure-audit-logging) die moeten worden geregistreerd door de **audit_log_events** -para meter bij te werken.
    :::image type="content" source="./media/howto-configure-audit-logs-portal/audit-log-events.png" alt-text="Serverparameters":::

1. Voeg MySQL-gebruikers toe die moeten worden opgenomen in of uitgesloten van logboek registratie door de **audit_log_exclude_users** -en **audit_log_include_users** -para meters bij te werken. Geef gebruikers op door hun MySQL-gebruikers naam op te geven.
    :::image type="content" source="./media/howto-configure-audit-logs-portal/audit-log-exclude-users.png" alt-text="Serverparameters":::

1. Wanneer u de para meters hebt gewijzigd, kunt u op **Opslaan**klikken. Of u kunt uw wijzigingen **negeren** .
    :::image type="content" source="./media/howto-configure-audit-logs-portal/save-parameters.png" alt-text="Serverparameters":::

## <a name="set-up-diagnostic-logs"></a>Diagnostische logboeken instellen

1. Selecteer in de sectie **bewaking** van de zijbalk **Diagnostische instellingen**.

1. Klik op ' + Diagnostische instelling toevoegen ' :::image type="content" source="./media/howto-configure-audit-logs-portal/add-diagnostic-setting.png" alt-text="Serverparameters":::

1. Geef een naam op voor de diagnostische instelling.

1. Geef op welke gegevens moeten worden gesinkt om de controle Logboeken (opslag account, Event Hub en/of Log Analytics-werk ruimte) te verzenden.

1. Selecteer ' MySqlAuditLogs ' als het logboek type.
:::image type="content" source="./media/howto-configure-audit-logs-portal/configure-diagnostic-setting.png" alt-text="Serverparameters":::

1. Zodra u de gegevens sinks hebt geconfigureerd om de controle logboeken te pipeen naar, kunt u op **Opslaan**klikken.
:::image type="content" source="./media/howto-configure-audit-logs-portal/save-diagnostic-setting.png" alt-text="Serverparameters":::

1. Open de audit logboeken door ze te verkennen in de gegevens-sinks die u hebt geconfigureerd. Het kan tot tien minuten duren voordat de logboeken worden weer gegeven.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [audit logboeken](concepts-audit-logs.md) in azure database for MySQL
- Meer informatie over het configureren van audit Logboeken in de [Azure cli](howto-configure-audit-logs-cli.md)