---
title: 'Quickstart: Een geheim van Azure Key Vault instellen en ophalen met behulp van een .NET-Web-app-Azure Key Vault | Microsoft Docs'
description: In deze quickstart gaat u met behulp van een .NET-web-app een geheim instellen in Azure Key Vault en dit geheim vervolgens ophalen
services: key-vault
author: msmbaldwin
manager: sumedhb
ms.service: key-vault
ms.topic: quickstart
ms.date: 01/02/2019
ms.author: barclayn
ms.custom: mvc
ms.openlocfilehash: 9ddb1db9b39ac942a3476f50aad39c98198b2a18
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2019
ms.locfileid: "68958598"
---
# <a name="quickstart-set-and-retrieve-a-secret-from-azure-key-vault-by-using-a-net-web-app"></a>Quickstart: een geheim uit Azure Key Vault instellen en ophalen met behulp van een .NET-web-app

In deze quickstart voert u de stappen uit die nodig zijn om een Azure-webtoepassing gegevens te laten lezen uit Azure Key Vault met behulp van beheerde identiteiten voor Azure-resources. Met Key Vault kunt u de informatie beveiligen. In deze zelfstudie leert u procedures om het volgende te doen:

* Een sleutelkluis maken.
* Een geheim opslaan in de sleutelkluis.
* Een geheim ophalen uit de sleutelkluis.
* Een Azure-webtoepassing maken.
* Schakel een [beheerde service-identiteit](../active-directory/managed-identities-azure-resources/overview.md) in voor de web-app.
* De vereiste machtigingen verlenen aan de webtoepassing om gegevens te lezen uit de sleutelkluis.

Neem eerst de [basisconcepten voor Key Vault](key-vault-whatis.md#basic-concepts) door.

>[!NOTE]
>Key Vault is een centrale opslagplaats voor het opslaan van geheimen via een programma. Maar hiervoor moeten toepassingen en gebruikers eerst worden geverifieerd bij Key Vault, wat betekent dat ze een geheim moeten presenteren. Als u de aanbevolen procedures voor beveiliging wilt volgen, moet dit eerste geheim periodiek worden gerouleerd. 
>
>Met [beheerde service-identiteiten voor Azure-resources](../active-directory/managed-identities-azure-resources/overview.md) krijgen toepassingen die in Azure worden uitgevoerd, een identiteit die automatisch door Azure wordt beheerd. Dit helpt bij het oplossen van het *probleem van het introduceren van geheimen* zodat gebruikers en toepassingen aanbevolen procedures kunnen volgen en zich geen zorgen hoeven maken over het rouleren van het eerste geheim.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Vereisten

* In Windows:
  * [Visual Studio 2019](https://www.microsoft.com/net/download/windows) met de volgende werk belastingen:
    * ASP.NET-ontwikkeling en webontwikkeling
    * Platformoverschrijdende ontwikkelmogelijkheden van .NET Core
  * [.NET Core 2.1 SDK of hoger](https://www.microsoft.com/net/download/windows)

* Op de Mac:
  * Zie [Nieuwe functies in Visual Studio voor Mac](https://visualstudio.microsoft.com/vs/mac/).

* Alle platformen:
  * Git ([downloaden](https://git-scm.com/downloads)).
  * Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.
  * [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) versie 2.0.4 of hoger. Deze is beschikbaar voor Windows, Mac en Linux.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Als u zich bij Azure wilt aanmelden met de Azure CLI, voert u het volgende in:

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een resourcegroep met de opdracht [az group create](/cli/azure/group#az-group-create). Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd.

Selecteer de naam van een resourcegroep en vul de tijdelijke aanduiding in.
In het volgende voorbeeld wordt een resourcegroep gemaakt in de regio US - oost:

```azurecli
# To list locations: az account list-locations --output table
az group create --name "<YourResourceGroupName>" --location "East US"
```

De resourcegroep die u net hebt gemaakt, wordt overal in dit artikel gebruikt.

## <a name="create-a-key-vault"></a>Een sleutelkluis maken

Vervolgens maakt u een sleutelkluis in de resourcegroep die u in de vorige stap hebt gemaakt. Geef de volgende informatie op:

* Naam van de sleutelkluis: De naam moet een tekenreeks zijn met 3-24 tekens en mag alleen 0-9, a-z, A-Z en een streepje (-) bevatten.
* Naam van de resourcegroep.
* Locatie: **US - oost**.

```azurecli
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "East US"
```

Vanaf dit punt is uw Azure-account nu als enige gemachtigd om bewerkingen op deze nieuwe kluis uit te voeren.

## <a name="add-a-secret-to-the-key-vault"></a>Een geheim toevoegen aan de sleutelkluis

We gaan een geheim toevoegen om te laten zien hoe dit werkt. U wilt bijvoorbeeld een SQL-verbindingsreeks opslaan of andere gegevens die u veilig wilt bewaren, maar wel beschikbaar wilt stellen aan uw toepassing.

Typ de volgende opdrachten om een geheim te maken in de sleutelkluis met de naam **AppGeheim**. Met dit geheim wordt de waarde **MijnGeheim** opgeslagen.

```azurecli
az keyvault secret set --vault-name "<YourKeyVaultName>" --name "AppSecret" --value "MySecret"
```

Als u de waarde in het geheim als tekst zonder opmaak wilt weergeven:

```azurecli
az keyvault secret show --name "AppSecret" --vault-name "<YourKeyVaultName>"
```

Met deze opdracht vraagt u de geheime gegevens op, inclusief de URI. Wanneer u deze stappen hebt uitgevoerd, beschikt u over een URI voor een geheim in een sleutelkluis. Noteer deze informatie. U hebt deze in een latere stap nodig.

## <a name="clone-the-repo"></a>De opslagplaats klonen

Kloon de opslagplaats om een lokale kopie te maken waarin u de bron kunt bewerken. Voer de volgende opdracht uit:

```
git clone https://github.com/Azure-Samples/key-vault-dotnet-core-quickstart.git
```

## <a name="open-and-edit-the-solution"></a>De oplossing openen en bewerken

Bewerk het bestand program.cs om het voorbeeld met de naam van uw specifieke sleutelkluis uit te voeren:

1. Ga naar de map key-vault-dotnet-core-quickstart.
2. Open het bestand Key-Vault-DotNet-core-QuickStart. SLN in Visual Studio 2019.
3. Open het bestand Program.cs en vervang de tijdelijke aanduiding *YourKeyVaultName* door de naam van de sleutelkluis die u eerder hebt gemaakt.

Deze oplossing maakt gebruik van NuGet-bibliotheken van [AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) en [KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault).

## <a name="run-the-app"></a>De app uitvoeren

Selecteer in het hoofd menu van Visual Studio 2019 **fout opsporing** > **starten zonder fout opsporing**. Wanneer de browser wordt weergegeven, gaat u naar de pagina **Over**. De waarde voor **AppGeheim** wordt weergegeven.

## <a name="publish-the-web-application-to-azure"></a>De webtoepassing publiceren in Azure

Publiceer deze app naar Azure om deze live als web-app in actie te zien en om te controleren of u de geheime waarde kunt ophalen:

1. In Visual Studio selecteert u het project **key-vault-dotnet-core-quickstart**.
2. Selecteer **Publiceren** > **Start**.
3. Maak een nieuwe **App Service** en selecteer **Publiceren**.
4. Wijzig de naam van de app in **keyvaultdotnetcorequickstart**.
5. Selecteer **Maken**.

>[!VIDEO https://sec.ch9.ms/ch9/e93d/a6ac417f-2e63-4125-a37a-8f34bf0fe93d/KeyVault_high.mp4]

## <a name="enable-a-managed-identity-for-the-web-app"></a>Een beheerde identiteit voor de web-app inschakelen

Azure Key Vault biedt een manier voor het veilig opslaan van referenties en andere sleutels en geheimen, maar uw code moet worden geverifieerd voor Key Vault om ze op te halen. In [Overzicht van beheerde identiteiten voor Azure-resources](../active-directory/managed-identities-azure-resources/overview.md) wordt het oplossen van dit probleem eenvoudiger gemaakt door Azure-services een automatisch beheerde identiteit in Azure Active Directory (Azure AD) te geven. U kunt deze identiteit gebruiken voor verificatie bij alle services die ondersteuning bieden voor Azure AD-verificatie, inclusief Key Vault, zonder dat u referenties in uw code hoeft te hebben.

Voer in Azure CLI de opdracht assign-identity uit om de identiteit voor deze toepassing te maken:

   ```azurecli
   az webapp identity assign --name "keyvaultdotnetcorequickstart" --resource-group "<YourResourceGroupName>"
   ```

>[!NOTE]
>U kunt dit ook doen door naar de portal te gaan en de instelling **Identeit/Systeem toegewezen** **Aan** te zetten in de eigenschappen van de webtoepassing.

## <a name="assign-permissions-to-your-application-to-read-secrets-from-key-vault"></a>Machtigingen toewijzen aan uw toepassing voor het lezen van geheimen uit Key Vault

Maak een notitie van de uitvoer wanneer u de toepassing naar Azure publiceert. Gebruik hierbij de notatie:
        
        {
          "principalId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
          "tenantId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
          "type": "SystemAssigned"
        }
        
Voer deze opdracht uit met de naam van uw sleutelkluis en de waarde van **PrincipalId**:

```azurecli

az keyvault set-policy --name '<YourKeyVaultName>' --object-id <PrincipalId> --secret-permissions get list

```

Tijdens het uitvoeren van de toepassing ziet u nu de geheime waarde die is opgehaald. In de voor gaande opdracht geeft u de identiteit van de app service-machtigingen voor het **ophalen** en **weer geven** van bewerkingen op uw sleutel kluis.

## <a name="clean-up-resources"></a>Resources opschonen
Verwijder de resourcegroep, de virtuele machine en alle gerelateerde resources wanneer u ze niet meer nodig hebt. Als u dit wilt doen, selecteert u de resource groep voor de sleutel kluis en selecteert u **verwijderen**.

Verwijder de sleutelkluis met behulp van de opdracht [az keyvault delete](https://docs.microsoft.com/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-delete):

```azurecli
az keyvault delete --name
                   [--resource-group]
                   [--subscription]
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over Key Vault](key-vault-whatis.md)
