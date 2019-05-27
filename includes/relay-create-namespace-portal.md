---
author: clemensv
ms.service: service-bus-relay
ms.topic: include
ms.date: 11/09/2018
ms.author: clemensv
ms.openlocfilehash: bbcf38ab54632144920b729f433c222f426d96dc
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66111547"
---
1. Meld u aan bij [Azure Portal][Azure portal].
2. Selecteer **+ Een resource maken** in het menu aan de linkerkant. Selecteer vervolgens **Integratie** > **Relay**. Als u **Relay** niet in de lijst ziet staan, selecteert u rechtsboven **Alles weergeven**. 
3. Voer bij **Naamruimte maken** een naam in voor de naamruimte. In het systeem wordt onmiddellijk gecontroleerd of de naam beschikbaar is.
4. Kies in het veld **Abonnement** een Azure-abonnement waarin u de naamruimte maakt.
5. Kies in het veld [Resourcegroep](../articles/azure-resource-manager/manage-resource-groups-portal.md) een bestaande resourcegroep waarin de naamruimte moet worden opgenomen of maak een nieuwe resourcegroep.  
6. Kies bij **Locatie** het land of de regio waarin uw naamruimte moet worden gehost.
   
    ![Naamruimte maken][create-namespace]
7. Selecteer **Maken**. Uw naamruimte wordt gemaakt en ingeschakeld. Na enkele minuten stelt het systeem resources ter beschikking voor uw account.

### <a name="get-management-credentials"></a>Beheerreferenties ophalen

1. Selecteer **Alle resources** en selecteer vervolgens de zojuist gemaakte naam voor de naamruimte.
2. Selecteer bij de Relay-naamruimte **Beleid voor gedeelde toegang**.  
3. Selecteer onder **Beleid voor gedeelde toegang** de optie **RootManageSharedAccessKey**.
   
    ![verbinding-gegevens][connection-info]
4. Selecteer onder **Beleid: RootManageSharedAccessKey** de knop **Kopiëren** naast **Verbindingsreeks - primaire sleutel**. Hiermee kopieert u de verbindingsreeks naar het Klembord voor later gebruik. Plak deze waarde in Kladblok of een andere tijdelijke locatie.
   
    ![connection-string][connection-string]

5. Herhaal de vorige stap om de waarde voor de **Primaire sleutel** te kopiëren en plakken naar een tijdelijke locatie zodat u deze later kunt gebruiken.  

<!--Image references-->

[create-namespace]: ./media/relay-create-namespace-portal/create-namespace.png
[connection-info]: ./media/relay-create-namespace-portal/connection-info.png
[connection-string]: ./media/relay-create-namespace-portal/connection-string.png
[Azure portal]: https://portal.azure.com
