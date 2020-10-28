---
title: Overzicht van Azure Key Vault herstel | Microsoft Docs
description: Key Vault herstel functies zijn ontworpen om te voor komen dat uw sleutel kluis en geheimen, sleutels en certificaten die zijn opgeslagen in de sleutel kluis per ongeluk of opzettelijk worden verwijderd.
ms.service: key-vault
ms.subservice: general
ms.topic: conceptual
author: ShaneBala-keyvault
ms.author: sudbalas
manager: ravijan
ms.date: 09/30/2020
ms.openlocfilehash: fbeb6f5f223642c09183c149188c6717c1f33a8e
ms.sourcegitcommit: 8c7f47cc301ca07e7901d95b5fb81f08e6577550
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/27/2020
ms.locfileid: "92748496"
---
# <a name="how-to-enable-soft-delete-and-purge-protection"></a>Voorlopig verwijderen en leegmaken van beveiliging inschakelen

Dit artikel heeft betrekking op twee herstel functies van Azure Key Vault, zacht verwijderen en beveiliging opschonen. Dit document bevat een overzicht van deze functies en laat zien hoe u deze kunt beheren via de Azure Portal, Azure CLI en Azure PowerShell.

## <a name="what-are-soft-delete-and-purge-protection"></a>Wat is zacht verwijderen en de beveiliging opschonen

Zacht verwijderen en de beveiliging opschonen zijn twee verschillende herstel functies voor de sleutel kluis.
> [!IMPORTANT]
> Tijdelijke verwijderings beveiliging moet zijn ingeschakeld op alle sleutel kluizen. De mogelijkheid om de beveiliging tegen zacht verwijderen uit te scha kelen, wordt vóór december 2020 afgeschaft. Lees hier de volledige informatie [ **here** .](soft-delete-change.md)

**Zacht verwijderen** is ontworpen om onbedoelde verwijdering van uw sleutel kluis en sleutels, geheimen en certificaten die zijn opgeslagen in de sleutel kluis te voor komen. Denk na over zacht verwijderen, zoals een prullenbak. Wanneer u een sleutel kluis of een sleutel kluis-object verwijdert, kan het worden hersteld voor een door de gebruiker Configureer bare Bewaar periode of een standaard waarde van 90 dagen. Sleutel kluizen in de voorlopig verwijderde status kunnen ook worden **opgeschoond** , wat betekent dat ze permanent worden verwijderd. Zo kunt u de sleutel kluizen en sleutel kluis objecten opnieuw maken met dezelfde naam. Voor het herstellen en verwijderen van sleutel kluizen en objecten zijn machtigingen voor het verhoogde toegangs beleid vereist. **Zodra de functie voor voorlopig verwijderen is ingeschakeld, kan deze niet worden uitgeschakeld.**

Het is belang rijk te weten dat de **namen van sleutel kluizen wereld wijd uniek zijn** . u kunt dus geen sleutel kluis maken met dezelfde naam als een sleutel kluis in de voorlopig verwijderde status. De namen van sleutels, geheimen en certificaten zijn ook uniek binnen een sleutel kluis. Het is niet mogelijk om een geheim, sleutel of certificaat met dezelfde naam als een andere te maken in de voorlopig verwijderde status.

Het **opschonen** van de beveiliging is zodanig ontworpen dat het verwijderen van uw sleutel kluis, sleutels, geheimen en certificaten door een kwaadwillende Insider wordt voor komen. U beschouwt dit als een prullenbak met een op tijd gebaseerde vergren deling. U kunt items op elk gewenst moment herstellen tijdens de Configureer bare Bewaar periode. **U kunt een sleutel kluis pas definitief verwijderen of wissen nadat de Bewaar periode is verstreken.** Zodra de Bewaar periode is verstreken, wordt de sleutel kluis of het sleutel kluis object automatisch verwijderd.

> [!NOTE]
> Het opschonen van de beveiliging is zo ontworpen dat er geen beheerdersrol of machtiging is die het opschonen van de beveiliging kan overschrijven, uitschakelen of omzeilen. **Wanneer het leegmaken van de beveiliging is ingeschakeld, kan het niet worden uitgeschakeld of overschreven door iedereen, waaronder micro soft.** Dit betekent dat u een verwijderde sleutel kluis moet herstellen of wachten totdat de Bewaar periode is verstreken voordat u de naam van de sleutel kluis opnieuw gebruikt.

# <a name="azure-portal"></a>[Azure Portal](#tab/azure-portal)

## <a name="verify-if-soft-delete-is-enabled-on-a-key-vault-and-enable-soft-delete"></a>Controleren of de functie voor voorlopig verwijderen is ingeschakeld voor een sleutel kluis en het voorlopig verwijderen inschakelen

1. Meld u aan bij Azure Portal.
1. Selecteer uw sleutelkluis.
1. Klik op de Blade eigenschappen.
1. Controleer of het keuze rondje naast voorlopig verwijderen is ingesteld op ' herstel inschakelen '.
1. Als voorlopig verwijderen niet is ingeschakeld op de sleutel kluis, klikt u op het keuze rondje om zacht verwijderen in te scha kelen en klikt u op opslaan.

:::image type="content" source="../media/key-vault-recovery-1.png" alt-text=" Een scherm afbeelding van de Azure Portal<>":::

## <a name="grant-access-to-a-service-principal-to-purge-and-recover-deleted-secrets"></a>Toegang verlenen aan een Service-Principal om verwijderde geheimen te verwijderen en te herstellen

1. Meld u aan bij Azure Portal.
1. Selecteer uw sleutelkluis.
1. Klik op de Blade toegangs beleid.
1. Zoek in de tabel de rij van de beveiligingsprincipal waaraan u toegang wilt verlenen (of Voeg een nieuwe beveiligingsprincipal toe).
1. Klik op de vervolg keuzelijst voor sleutels, certificaten en geheimen.
1. Ga naar de onderkant van de vervolg keuzelijst en klik op herstellen en leegmaken
1. Beveiligings-principals hebben ook de functies Get en List nodig om de meeste bewerkingen uit te voeren.

:::image type="content" source="../media/key-vault-recovery-2.png" alt-text=" Een scherm afbeelding van de Azure Portal<>":::

## <a name="list-recover-or-purge-a-soft-deleted-key-vault"></a>Een tijdelijke, verwijderde sleutel kluis weer geven, herstellen of verwijderen

1. Meld u aan bij Azure Portal.
1. Klik boven aan de pagina op de zoek balk.
1. Klik onder recente Services op Key Vault. Klik niet op een individuele sleutel kluis.
1. Klik boven aan het scherm op de optie voor het beheren van verwijderde kluizen
1. Er wordt een context venster geopend aan de rechter kant van het scherm.
1. Selecteer uw abonnement.
1. Als uw sleutel kluis zacht is verwijderd, wordt deze weer gegeven in het context venster aan de rechter kant.
1. Als er te veel kluizen zijn, kunt u klikken op ' meer laden ' onder aan het context venster of het gebruik van CLI of Power shell om de resultaten op te halen.
1. Wanneer u de kluis hebt gevonden die u wilt herstellen of verwijderen, schakelt u het selectie vakje ernaast in.
1. Selecteer de optie herstellen onder aan het deel venster context als u de sleutel kluis wilt herstellen.
1. Selecteer de optie opschonen als u de sleutel kluis permanent wilt verwijderen.

:::image type="content" source="../media/key-vault-recovery-3.png" alt-text=" Een scherm afbeelding van de Azure Portal<>":::

:::image type="content" source="../media/key-vault-recovery-4.png" alt-text=" Een scherm afbeelding van de Azure Portal<>":::

## <a name="list-recover-or-purge-soft-deleted-secrets-keys-and-certificates"></a>Tijdelijke verwijderde geheimen, sleutels en certificaten weer geven, herstellen of verwijderen

1. Meld u aan bij Azure Portal.
1. Selecteer uw sleutelkluis.
1. Selecteer de Blade die overeenkomt met het geheime type dat u wilt beheren (sleutels, geheimen of certificaten).
1. Klik aan de bovenkant van het scherm op "verwijderde (sleutels, geheimen of certificaten beheren)
1. Er wordt een context venster weer gegeven aan de rechter kant van het scherm.
1. Als uw geheim, sleutel of certificaat niet wordt weer gegeven in de lijst, heeft het niet de status zacht verwijderd.
1. Selecteer het geheim, de sleutel of het certificaat dat u wilt beheren.
1. Selecteer de optie om onder in het context venster te herstellen of te wissen.

:::image type="content" source="../media/key-vault-recovery-5.png" alt-text=" Een scherm afbeelding van de Azure Portal<>":::

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

## <a name="key-vault-cli"></a>Key Vault (CLI)

* Controleren of de functie voor het voorlopig verwijderen van een sleutel kluis is ingeschakeld

    ```azurecli
    az keyvault show --subscription {SUBSCRIPTION ID} -g {RESOURCE GROUP} -n {VAULT NAME}
    ```

* Voorlopig verwijderen inschakelen op de sleutel kluis

    Voor alle nieuwe sleutel kluizen is de functie voor voorlopig verwijderen standaard ingeschakeld. Als u momenteel een sleutel kluis hebt waarvoor geen tijdelijke verwijdering is ingeschakeld, gebruikt u de volgende opdracht om de optie voorlopig verwijderen in te scha kelen.

    ```azurecli
    az keyvault update --subscription {SUBSCRIPTION ID} -g {RESOURCE GROUP} -n {VAULT NAME} --enable-soft-delete true
    ```

* Sleutel kluis verwijderen (herstelbaar als voorlopig verwijderen is ingeschakeld)

    ```azurecli
    az keyvault delete --subscription {SUBSCRIPTION ID} -g {RESOURCE GROUP} -n {VAULT NAME}
    ```

* Alle voorlopig verwijderde sleutel kluizen weer geven

    ```azurecli
    az keyvault list-deleted --subscription {SUBSCRIPTION ID} --resource-type vault
    ```

* Tijdelijke, verwijderde sleutel herstellen-kluis

    ```azurecli
    az keyvault recover --subscription {SUBSCRIPTION ID} -n {VAULT NAME}
    ```

* Tijdelijke verwijderde sleutel kluis leegmaken **(waarschuwing! met deze bewerking wordt uw sleutel kluis permanent verwijderd.)**

    ```azurecli
    az keyvault purge --subscription {SUBSCRIPTION ID} -n {VAULT NAME}
    ```

* Leegmaken van beveiliging op sleutel-kluis inschakelen

    ```azurecli
    az keyvault update --subscription {SUBSCRIPTION ID} -g {RESOURCE GROUP} -n {VAULT NAME} --enable-purge-protection true
    ```

## <a name="certificates-cli"></a>Certificaten (CLI)

* Toegang verlenen om certificaten te verwijderen en te herstellen

    ```azurecli
    az keyvault set-policy --upn user@contoso.com --subscription {SUBSCRIPTION ID} -g {RESOURCE GROUP} -n {VAULT NAME}  --certificate-permissions recover purge
    ```

* Certificaat verwijderen

    ```azurecli
    az keyvault certificate delete --subscription {SUBSCRIPTION ID} --vault-name {VAULT NAME} --name {CERTIFICATE NAME}
    ```

* Verwijderde certificaten weer geven

    ```azurecli
    az keyvault certificate list-deleted --subscription {SUBSCRIPTION ID} --vault-name {VAULT NAME}
    ```

* Verwijderd certificaat herstellen

    ```azurecli
    az keyvault certificate recover --subscription {SUBSCRIPTION ID} --vault-name {VAULT NAME} --name {CERTIFICATE NAME}
    ```

* Voorlopig verwijderd certificaat leegmaken **(waarschuwing! met deze bewerking wordt uw certificaat permanent verwijderd.)**

    ```azurecli
    az keyvault certificate purge --subscription {SUBSCRIPTION ID} --vault-name {VAULT NAME} --name {CERTIFICATE NAME}
    ```

## <a name="keys-cli"></a>Sleutels (CLI)

* Toegang verlenen om sleutels te verwijderen en te herstellen

    ```azurecli
    az keyvault set-policy --upn user@contoso.com --subscription {SUBSCRIPTION ID} -g {RESOURCE GROUP} -n {VAULT NAME}  --key-permissions recover purge
    ```

* Sleutel verwijderen

    ```azurecli
    az keyvault key delete --subscription {SUBSCRIPTION ID} --vault-name {VAULT NAME} --name {KEY NAME}
    ```

* Verwijderde sleutels vermelden

    ```azurecli
    az keyvault key list-deleted --subscription {SUBSCRIPTION ID} --vault-name {VAULT NAME}
    ```

* Verwijderde sleutel herstellen

    ```azurecli
    az keyvault key recover --subscription {SUBSCRIPTION ID} --vault-name {VAULT NAME} --name {KEY NAME}
    ```

* Zacht verwijderde sleutel leegmaken **(waarschuwing! met deze bewerking wordt uw sleutel permanent verwijderd.)**

    ```azurecli
    az keyvault key purge --subscription {SUBSCRIPTION ID} --vault-name {VAULT NAME} --name {KEY NAME}
    ```

## <a name="secrets-cli"></a>Geheimen (CLI)

* Toegang verlenen om geheimen te verwijderen en te herstellen

    ```azurecli
    az keyvault set-policy --upn user@contoso.com --subscription {SUBSCRIPTION ID} -g {RESOURCE GROUP} -n {VAULT NAME}  --secret-permissions recover purge
    ```

* Geheim verwijderen

    ```azurecli
    az keyvault secret delete --subscription {SUBSCRIPTION ID} --vault-name {VAULT NAME} --name {SECRET NAME}
    ```

* Verwijderde geheimen weer geven

    ```azurecli
    az keyvault secret list-deleted --subscription {SUBSCRIPTION ID} --vault-name {VAULT NAME}
    ```

* Verwijderd geheim herstellen

    ```azurecli
    az keyvault secret recover --subscription {SUBSCRIPTION ID} --vault-name {VAULT NAME} --name {SECRET NAME}
    ```

* Zacht verwijderd geheim opschonen **(waarschuwing! met deze bewerking wordt uw geheim permanent verwijderd.)**

    ```azurecli
    az keyvault secret purge --subscription {SUBSCRIPTION ID} --vault-name {VAULT NAME} --name {SECRET NAME}
    ```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

## <a name="key-vault-powershell"></a>Key Vault (Power shell)

* Controleren of de functie voor het voorlopig verwijderen van een sleutel kluis is ingeschakeld

    ```powershell
    Get-AzKeyVault -VaultName "ContosoVault"
    ```

* Sleutel kluis verwijderen

    ```powershell
    Remove-AzKeyVault -VaultName 'ContosoVault'
    ```

* Alle voorlopig verwijderde sleutel kluizen weer geven

    ```powershell
    Get-AzKeyVault -InRemovedState
    ```

* Tijdelijke, verwijderde sleutel herstellen-kluis

    ```powershell
    Undo-AzKeyVaultRemoval -VaultName ContosoVault -ResourceGroupName ContosoRG -Location westus
    ```

* Verwijder de voorlopig verwijderde sleutel-kluis **(waarschuwing! met deze bewerking wordt uw sleutel kluis permanent verwijderd.)**

    ```powershell
    Remove-AzKeyVault -VaultName ContosoVault -InRemovedState -Location westus
    ```

* Leegmaken van beveiliging op sleutel-kluis inschakelen

    ```powershell
    ($resource = Get-AzResource -ResourceId (Get-AzKeyVault -VaultName "ContosoVault").ResourceId).Properties | Add-Member -MemberType "NoteProperty" -Name "enablePurgeProtection" -Value "true"

    Set-AzResource -resourceid $resource.ResourceId -Properties $resource.Properties
    ```

## <a name="certificates-powershell"></a>Certificaten (Power shell)

* Machtigingen verlenen om certificaten te herstellen en op te schonen

    ```powershell
    Set-AzKeyVaultAccessPolicy -VaultName ContosoVault -UserPrincipalName user@contoso.com -PermissionsToCertificates recover,purge
    ```

* Een certificaat verwijderen

  ```powershell
  Remove-AzKeyVaultCertificate -VaultName ContosoVault -Name 'MyCert'
  ```

* Alle verwijderde certificaten in een sleutel kluis weer geven

  ```powershell
  Get-AzKeyVaultCertificate -VaultName ContosoVault -InRemovedState
  ```

* Een certificaat herstellen met de status verwijderd

  ```powershell
  Undo-AzKeyVaultCertificateRemoval -VaultName ContosoVault -Name 'MyCert'
  ```

* Een voorlopig verwijderd certificaat leegmaken **(waarschuwing! met deze bewerking wordt uw certificaat permanent verwijderd.)**

  ```powershell
  Remove-AzKeyVaultcertificate -VaultName ContosoVault -Name 'MyCert' -InRemovedState
  ```

## <a name="keys-powershell"></a>Sleutels (Power shell)

* Machtigingen verlenen voor het herstellen en leegmaken van sleutels

    ```powershell
    Set-AzKeyVaultAccessPolicy -VaultName ContosoVault -UserPrincipalName user@contoso.com -PermissionsToKeys recover,purge
    ```

* Een sleutel verwijderen

  ```powershell
  Remove-AzKeyVaultKey -VaultName ContosoVault -Name 'MyKey'
  ```

* Alle verwijderde certificaten in een sleutel kluis weer geven

  ```powershell
  Get-AzKeyVaultKey -VaultName ContosoVault -InRemovedState
  ```

* Een voorlopig verwijderde sleutel herstellen

    ```powershell
    Undo-AzKeyVaultKeyRemoval -VaultName ContosoVault -Name ContosoFirstKey
    ```

* Een voorlopig verwijderde sleutel leegmaken **(waarschuwing! met deze bewerking wordt uw sleutel permanent verwijderd.)**

    ```powershell
    Remove-AzKeyVaultKey -VaultName ContosoVault -Name ContosoFirstKey -InRemovedState
    ```

## <a name="secrets-powershell"></a>Geheimen (Power shell)

* Machtigingen verlenen om geheimen te herstellen en op te schonen

    ```powershell
    Set-AzKeyVaultAccessPolicy -VaultName ContosoVault -UserPrincipalName user@contoso.com -PermissionsToSecrets recover,purge
    ```

* Een geheim met de naam SQLPassword verwijderen

  ```powershell
  Remove-AzKeyVaultSecret -VaultName ContosoVault -name SQLPassword
  ```

* Alle verwijderde geheimen in een sleutel kluis weer geven

  ```powershell
  Get-AzKeyVaultSecret -VaultName ContosoVault -InRemovedState
  ```

* Een geheim herstellen met de status verwijderd

  ```powershell
  Undo-AzKeyVaultSecretRemoval -VaultName ContosoVault -Name SQLPAssword
  ```

* Een geheim in een verwijderde staat verwijderen **(waarschuwing! met deze bewerking wordt uw sleutel permanent verwijderd.)**

  ```powershell
  Remove-AzKeyVaultSecret -VaultName ContosoVault -InRemovedState -name SQLPassword
  ```
