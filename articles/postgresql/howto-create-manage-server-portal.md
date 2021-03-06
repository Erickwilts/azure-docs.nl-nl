---
title: Azure Database for PostgreSQL beheren-Azure Portal
description: Meer informatie over het beheren van een Azure Database for PostgreSQL-server vanuit de Azure Portal.
author: lfittl-msft
ms.author: lufittl
ms.service: postgresql
ms.topic: how-to
ms.date: 11/20/2019
ms.openlocfilehash: 393e67d1b690f7231b5bf298b44f4db7c489d184
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "91704511"
---
# <a name="manage-an-azure-database-for-postgresql-server-using-the-azure-portal"></a>Een Azure Database for PostgreSQL-server beheren met behulp van de Azure Portal

Dit artikel laat u zien hoe u uw Azure Database for PostgreSQL-servers kunt beheren. Beheer taken zijn onder andere berekening en opslag schalen, beheerders wachtwoord opnieuw instellen en server details weer geven.

## <a name="sign-in"></a>Aanmelden

Meld u aan bij de [Azure-portal](https://portal.azure.com).

## <a name="create-a-server"></a>Een server maken

Ga naar de [Quick](quickstart-create-server-database-portal.md) start voor informatie over het maken en aan de slag met een Azure database for postgresql-server.

## <a name="scale-compute-and-storage"></a>Schaal berekening en opslag

Nadat de server is gemaakt, kunt u de schaal aanpassen tussen de lagen Algemeen en geoptimaliseerd voor geheugen als uw behoeften veranderen. U kunt reken kracht en geheugen ook schalen door de vCores te verg Roten of te verkleinen. Opslag kan omhoog worden geschaald (u kunt de opslag echter niet omlaag schalen).

### <a name="scale-between-general-purpose-and-memory-optimized-tiers"></a>Schaal tussen Algemeen en geoptimaliseerde geheugen lagen

U kunt schalen van Algemeen naar geoptimaliseerd voor geheugen en vice versa. Wijzigen van en van de Basic-laag nadat het maken van de server niet wordt ondersteund.

1. Selecteer uw server in de Azure Portal. Selecteer de **prijs categorie**die u in de sectie **instellingen** bevindt.

2. Selecteer **Algemeen** of **geoptimaliseerd geheugen**, afhankelijk van wat u wilt schalen.

   :::image type="content" source="./media/howto-create-manage-server-portal/change-pricing-tier.png" alt-text="Scherm opname van Azure Portal om de laag basis, Algemeen of geoptimaliseerd voor geheugen te kiezen in Azure Database for PostgreSQL":::

   > [!NOTE]
   > Wanneer u de lagen wijzigt, wordt de server opnieuw opgestart.

3. Selecteer **OK** om de wijzigingen op te slaan.

### <a name="scale-vcores-up-or-down"></a>VCores omhoog of omlaag schalen

1. Selecteer uw server in de Azure Portal. Selecteer de **prijs categorie**die u in de sectie **instellingen** bevindt.

2. Wijzig de instelling **vCore** door de schuif regelaar naar de gewenste waarde te verplaatsen.

   :::image type="content" source="./media/howto-create-manage-server-portal/scaling-compute.png" alt-text="Scherm opname van Azure Portal om de laag basis, Algemeen of geoptimaliseerd voor geheugen te kiezen in Azure Database for PostgreSQL":::

   > [!NOTE]
   > Het schalen van vCores zorgt ervoor dat de server opnieuw wordt opgestart.

3. Selecteer **OK** om de wijzigingen op te slaan.

### <a name="scale-storage-up"></a>Opslag omhoog schalen

1. Selecteer uw server in de Azure Portal. Selecteer de **prijs categorie**die u in de sectie **instellingen** bevindt.

2. Wijzig de **opslag** instelling door de schuif regelaar omhoog te verplaatsen naar de gewenste waarde.

   :::image type="content" source="./media/howto-create-manage-server-portal/scaling-storage.png" alt-text="Scherm opname van Azure Portal om de laag basis, Algemeen of geoptimaliseerd voor geheugen te kiezen in Azure Database for PostgreSQL":::

   > [!NOTE]
   > De opslag kan niet omlaag worden geschaald.

3. Selecteer **OK** om de wijzigingen op te slaan.

## <a name="update-admin-password"></a>Beheerders wachtwoord bijwerken

U kunt het wacht woord van de beheerdersrol wijzigen met behulp van de Azure Portal.

1. Selecteer uw server in de Azure Portal. Selecteer **wacht woord opnieuw instellen**in het venster **overzicht** .

   :::image type="content" source="./media/howto-create-manage-server-portal/overview-reset-password.png" alt-text="Scherm opname van Azure Portal om de laag basis, Algemeen of geoptimaliseerd voor geheugen te kiezen in Azure Database for PostgreSQL":::

2. Voer een nieuw wachtwoord in en bevestig het wachtwoord. In het tekstvak wordt u gevraagd om de vereisten voor wachtwoord complexiteit.

   :::image type="content" source="./media/howto-create-manage-server-portal/reset-password.png" alt-text="Scherm opname van Azure Portal om de laag basis, Algemeen of geoptimaliseerd voor geheugen te kiezen in Azure Database for PostgreSQL":::

3. Selecteer **OK** om het nieuwe wacht woord op te slaan.

## <a name="delete-a-server"></a>Een server verwijderen

U kunt uw server verwijderen als u deze niet meer nodig hebt. 

1. Selecteer uw server in de Azure Portal. Selecteer **verwijderen**in het venster **overzicht** .

   :::image type="content" source="./media/howto-create-manage-server-portal/overview-delete.png" alt-text="Scherm opname van Azure Portal om de laag basis, Algemeen of geoptimaliseerd voor geheugen te kiezen in Azure Database for PostgreSQL":::

2. Typ de naam van de server in het invoervak om te bevestigen dat dit de server is die u wilt verwijderen.

   :::image type="content" source="./media/howto-create-manage-server-portal/confirm-delete.png" alt-text="Scherm opname van Azure Portal om de laag basis, Algemeen of geoptimaliseerd voor geheugen te kiezen in Azure Database for PostgreSQL":::

   > [!NOTE]
   > Het verwijderen van een server is onomkeerbaar.

3. Selecteer **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [back-ups en server herstel](howto-restore-server-portal.md)
- Meer informatie over [Opties voor afstemmen en controleren in azure database for PostgreSQL](concepts-monitoring.md)
