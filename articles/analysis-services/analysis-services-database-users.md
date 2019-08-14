---
title: Database rollen en-gebruikers beheren in Azure Analysis Services | Microsoft Docs
description: Meer informatie over het beheren van database rollen en-gebruikers op een Analysis Services-server in Azure.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 2a6c63c4ae58079c79a9d344f1e2550e4768088f
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2019
ms.locfileid: "68932245"
---
# <a name="manage-database-roles-and-users"></a>Database rollen en-gebruikers beheren

Op het niveau van de model database moeten alle gebruikers deel uitmaken van een rol. Rollen definiëren gebruikers met bepaalde machtigingen voor de model database. Alle gebruikers of beveiligings groepen die aan een rol worden toegevoegd, moeten een account in een Azure AD-Tenant hebben in hetzelfde abonnement als de-server. 

De manier waarop u rollen definieert, is afhankelijk van het hulp programma dat u gebruikt, maar het effect is hetzelfde.

Rolmachtigingen zijn onder andere:
*  **Beheerder** : gebruikers hebben volledige machtigingen voor de data base. Database rollen met beheerders machtigingen verschillen van Server beheerders.
*  **Proces** : gebruikers kunnen verbinding maken met en proces bewerkingen uitvoeren op de data base en model database gegevens analyseren.
*  **Lezen** -gebruikers kunnen een client toepassing gebruiken om verbinding te maken met de model database gegevens en deze te analyseren.

Bij het maken van een tabellaire model project maakt u rollen en voegt u gebruikers of groepen toe aan die rollen met behulp van Role Manager in SQL Server Data Tools (SSDT). Wanneer u hebt geïmplementeerd op een server, gebruikt u SQL Server Management Studio (SSMS), [Analysis Services Power shell](https://docs.microsoft.com/analysis-services/powershell/analysis-services-powershell-reference)-cmdlets of [Tabellaire model scripting language](https://docs.microsoft.com/bi-reference/tmsl/tabular-model-scripting-language-tmsl-reference) (TMSL) om rollen en gebruikers leden toe te voegen of te verwijderen.

Voor **beveiligings groepen** moet een [e-mail bericht worden ingeschakeld](https://docs.microsoft.com/exchange/recipients-in-exchange-online/manage-mail-enabled-security-groups) waarvoor de `MailEnabled` eigenschap is ingesteld op. `True` Wanneer u `obj:groupid@tenantid`een groep op basis van een e-mail adres opgeeft.


## <a name="to-add-or-manage-roles-and-users-in-ssdt"></a>Rollen en gebruikers in SSDT toevoegen of beheren  
  
1.  Klik in SSDT > **Tabellaire model Verkenner**met de rechter muisknop op **rollen**.  
  
2.  Klik in **Role Manager** op **New**.  
  
3.  Typ een naam voor de rol.  
  
     Standaard is de naam van de standaardrol incrementeel genummerd voor elke nieuwe rol. U kunt het beste een naam invoeren waarmee het lidtype duidelijk wordt geïdentificeerd, bijvoorbeeld financiële managers of human resources-specialisten.  
  
4.  Selecteer een van de volgende machtigingen:  
  
    |Machtiging|Description|  
    |----------------|-----------------|  
    |**Geen**|Leden kunnen het model schema niet wijzigen en kunnen geen gegevens opvragen.|  
    |**Lezen**|Leden kunnen gegevens opvragen (op basis van Row filters), maar kunnen het model schema niet wijzigen.|  
    |**Lezen en verwerken**|Leden kunnen gegevens opvragen (op basis van filters op rijniveau) en proces uitvoeren en alle bewerkingen verwerken, maar kunnen het model schema niet wijzigen.|  
    |**Host**|Leden kunnen proces uitvoeren en alle bewerkingen verwerken. Kan het model schema niet wijzigen en kan geen gegevens opvragen.|  
    |**Beheerder**|Leden kunnen het model schema wijzigen en alle gegevens opvragen.|   
  
5.  Als de rol die u maakt lees-of lees-en proces machtiging heeft, kunt u rijdefinities toevoegen met behulp van een DAX-formule. Klik op het tabblad **rij filters** , selecteer een tabel en klik vervolgens op het **Dax-filter** veld en typ een Dax-formule.
  
6.  Klik op **leden** > **toevoegen extern**.  
  
8.  In **extern lid toevoegen**voert u gebruikers of groepen in uw TENANT Azure AD op e-mail adres in. Nadat u op OK hebt geklikt en rollen beheerder hebt gesloten, worden rollen en Rolgroepen weer gegeven in Tabellaire model Verkenner. 
 
     ![Rollen en gebruikers in Tabellaire model Verkenner](./media/analysis-services-database-users/aas-roles-tmexplorer.png)

9. Implementeren op uw Azure Analysis Services-server.


## <a name="to-add-or-manage-roles-and-users-in-ssms"></a>Rollen en gebruikers in SSMS toevoegen of beheren

Als u functies en gebruikers wilt toevoegen aan een geïmplementeerde model database, moet u met de server zijn verbonden als server beheerder of al een databaserol met beheerders machtigingen.

1. Klik in object Exporer met de rechter muisknop op **rollen** > **nieuwe rol**.

2. Voer bij **rol maken**de naam en beschrijving van een rol in.

3. Selecteer een machtiging.

   |Machtiging|Description|  
   |----------------|-----------------|  
   |**Volledig beheer (beheerder)**|Leden kunnen het model schema en proces wijzigen en kunnen query's uitvoeren op alle gegevens.| 
   |**Data base verwerken**|Leden kunnen proces uitvoeren en alle bewerkingen verwerken. Kan het model schema niet wijzigen en kan geen gegevens opvragen.|  
   |**Lezen**|Leden kunnen gegevens opvragen (op basis van Row filters), maar kunnen het model schema niet wijzigen.|  
  
4. Klik op **lidmaatschap**en voer een gebruiker of groep in uw TENANT Azure AD op e-mail adres in.

     ![Gebruiker toevoegen](./media/analysis-services-database-users/aas-roles-adduser-ssms.png)

5. Als de rol die u maakt lees machtiging heeft, kunt u rijdefinities toevoegen met behulp van een DAX-formule. Klik op **rij filters**, selecteer een tabel en typ vervolgens een Dax-formule in het veld **Dax-filter** . 

## <a name="to-add-roles-and-users-by-using-a-tmsl-script"></a>Rollen en gebruikers toevoegen met behulp van een TMSL-script

U kunt een TMSL-script uitvoeren in het XMLA-venster in SSMS of met behulp van Power shell. Gebruik de opdracht [CreateOrReplace](https://docs.microsoft.com/bi-reference/tmsl/createorreplace-command-tmsl) en het object [roles](https://docs.microsoft.com/bi-reference/tmsl/roles-object-tmsl) .

**Voor beeld van TMSL-script**

In dit voor beeld worden een B2B externe gebruiker en een groep toegevoegd aan de rol analist met lees machtigingen voor de SalesBI-data base. Zowel de externe gebruiker als de groep moeten dezelfde Tenant Azure AD hebben.

```
{
  "createOrReplace": {
    "object": {
      "database": "SalesBI",
      "role": "Analyst"
    },
    "role": {
      "name": "Users",
      "description": "All allowed users to query the model",
      "modelPermission": "read",
      "members": [
        {
          "memberName": "user1@contoso.com",
          "identityProvider": "AzureAD"
        },
        {
          "memberName": "group1@adventureworks.com",
          "identityProvider": "AzureAD"
        }
      ]
    }
  }
}
```

## <a name="to-add-roles-and-users-by-using-powershell"></a>Rollen en gebruikers toevoegen met behulp van Power shell

De [sqlserver](https://docs.microsoft.com/analysis-services/powershell/analysis-services-powershell-reference) -module biedt taak-specifieke data base management-cmdlets en de cmdlet invoke-ASCmd voor algemeen gebruik die een TMSL-query (Tabellair model scripting language) of script accepteert. De volgende cmdlets worden gebruikt voor het beheren van database rollen en-gebruikers.
  
|Cmdlet|Description|
|------------|-----------------| 
|[Add-RoleMember](https://docs.microsoft.com/powershell/module/sqlserver/Add-RoleMember)|Een lid toevoegen aan een databaserol.| 
|[Remove-RoleMember](https://docs.microsoft.com/powershell/module/sqlserver/remove-rolemember)|Een lid uit een databaserol verwijderen.|   
|[Invoke-ASCmd](https://docs.microsoft.com/powershell/module/sqlserver/invoke-ascmd)|Voer een TMSL-script uit.|

## <a name="row-filters"></a>Rijgegevens  

Met rijdefinities definieert u welke rijen in een tabel door leden van een bepaalde rol kunnen worden opgevraagd. Rij filters worden gedefinieerd voor elke tabel in een model met behulp van DAX-formules.  
  
Rijdefinities kunnen alleen worden gedefinieerd voor rollen met de machtigingen lezen en lezen en verwerken. Als er geen filter voor een bepaalde tabel is gedefinieerd, kunnen leden standaard alle rijen in de tabel doorzoeken, tenzij kruislings filteren van toepassing is op een andere tabel.
  
 Voor rijgegevens is een DAX-formule vereist, die moet resulteren in een waar/onwaar-waarde om de rijen te definiëren die door leden van de desbetreffende rol kunnen worden opgevraagd. De rijen die niet zijn opgenomen in de DAX-formule, kunnen niet worden opgevraagd. Bijvoorbeeld, de tabel Klanten met de volgende rij filters expressie, *= klanten [land] = "USA"* , leden van de rol verkoop kunnen alleen klanten in de Verenigde Staten bekijken.  
  
Rijgegevens zijn van toepassing op de opgegeven rijen en gerelateerde rijen. Wanneer een tabel meerdere relaties heeft, past filters beveiliging toe voor de relatie die actief is. Rij filters worden gekruist met andere rijtypen die zijn gedefinieerd voor gerelateerde tabellen, bijvoorbeeld:  
  
|Tabel|DAX-expressie|  
|-----------|--------------------|  
|Regio|= Regio [land] = "USA"|  
|ProductCategory|= ProductCategory [name] = "fietsen"|  
|Transacties|=Transactions[Year]=2016|  
  
 Het net-effect kan leden rijen van gegevens opvragen waarbij de klant zich in de Verenigde Staten bevindt, de product categorie fietsen is en het jaar 2016 is. Gebruikers kunnen geen trans acties buiten de USA aanvragen, trans acties die geen fietsen of trans acties zijn die niet in 2016 zijn, tenzij ze lid zijn van een andere rol die deze machtigingen verleent.
  
 U kunt het filter, *= False ()* , gebruiken om de toegang tot alle rijen voor een hele tabel te weigeren.

## <a name="next-steps"></a>Volgende stappen

  [Server beheerders beheren](analysis-services-server-admins.md)   
  [Azure Analysis Services beheren met Power shell](analysis-services-powershell.md)  
  [Naslag informatie over tabellaire model scripting language (TMSL)](https://docs.microsoft.com/bi-reference/tmsl/tabular-model-scripting-language-tmsl-reference)

